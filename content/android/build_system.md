+++
date = "2015-11-30T07:00:53-08:00"
title = "Android Build System"

+++

*On November 23 & 24, I attended the first ever [Android Dev Summit](https://androiddevsummit.withgoogle.com/). The following are notes that I took during talks. I have included the video of the talk as well. Again, these are my notes from the talk, not my original content.*

<iframe width="560" height="315" src="https://www.youtube.com/embed/OOEDKf06WqA" frameborder="0" allowfullscreen></iframe>

Chiu-Ki Chan (Android GDE) took really great notes during the session as well, here's what she had:

<blockquote class="twitter-tweet" lang="en"><p lang="en" dir="ltr">The Android Build System by <a href="https://twitter.com/droidxav">@droidxav</a> <a href="https://twitter.com/benol">@benol</a> <a href="https://twitter.com/dochez">@dochez</a> <a href="https://twitter.com/hashtag/AndroidDevSummit?src=hash">#AndroidDevSummit</a> <a href="https://twitter.com/hashtag/Sketchnotes?src=hash">#Sketchnotes</a> <a href="https://t.co/ppZAfh5J3l">pic.twitter.com/ppZAfh5J3l</a></p>&mdash; Chiu-Ki Chan (@chiuki) <a href="https://twitter.com/chiuki/status/669240227219660800">November 24, 2015</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

## Tips

* Use the daemon
  * `--daemon` or `org.gradle.debug = true`
* Give it enough memeory
* Control the number of threads
* Use the latest build tools
* Get an SSD
* Don't use timestamps or commit numbers in debug build files

Try:

    android {
        dexOptions {
            dexInProgress = true
        }
    }

Consider having a development flavor using min SDK 21, as it's going to be faster.

    android.productFlavors {
        normal
        dev {
            minSdkVersion 21
        }
    }

## Testing

### Test dependencies

    dependencies {
        compile ".."
        testCompile "junit:junit:4.12"
        testCompile "org.mockito:mockito-core:1.9.5"
    }

### Unit testing

config:

    unitTests.all {
        jvmArgs "-XX:MaxPermSize=256m"

        beforeTest { descriptor ->
            logger.lifecycle("Running test: " + descriptor)
        }
    }

### Using a test project

Use a separate project just for the test APK.

In main project:

    apply plugin: 'com.android.application'
    android {
        ...
        publishNonDefault true
    }

In test project:

    apply plugin: 'com.android.test'
    android {
        targetProjectPath ':app'
        targetVariant 'debug'

        defaultConfig {
            minSdkVersion 8
            testInstrumentationRunner 'android.support.test.runner.AndroidJUnitRunner'
        }
    }

Test code then lives in the **main** source set of your **test project**.

Instrumentation tag still needs to be added to the **test project** manifest, however they're hoping to be able to remove this step soon:

    <manifest ...>
        <instrumentation
            android:name="android.support.test.runner.AndroidJUnitRunner"
            android:targetPackage="com.example.app" />
    </manifest>

Running task on **test project**:

    ./gradlew :testProject:connectedAndroidTest

One note on all of this is to make sure that you avoid build tools and dependency version conflicts! Main APK and Test APK need to have the same Build Tools version, and same versions for dependencies. This should be common sense (since you want to test the same versions of everything).

## Native Support

### New plugins

[New plugins for native (content at link may change)](http://tools.android.com/tech-docs/new-build-system/gradle-experimental):

* com.android.model.application - *\*.apk*
* com.android.model.library - *\*.aar*
* com.android.model.native - *\*.so*

In build.gradle:

    apply plugin: 'com.android.model.application'
    model {
        android {
            compileSdkVersion = 22
            buildToolsVersion = "22.0.1"

            ndk.with {
                moduleName = "native"
                // and other configurations like the following
                CFlags.add("-DCUSTOM_DEFINE")
                cppFlags.add("-DCUSTOM_DEFINE")
                ldFlags.add("-L/custom/lib/path")
                ldLibs.add("log")
                stl = "stlport_static"
            }
        }
    }

### Inter-module dependencies

Inter-module dependencies for native code, using a library project for the native code:

    apply plugin: 'com.android.model.application'
    model {
        android.sources {
            main {
                jni {
                    dependencies {
                        project ":lib"
                    }
                }
            }
        }
    }

Perhaps the **lib project** with native code is set up like this:

    apply plugin: "com.android.model.native"
    model {
        android {
            compileSdkVersion = 21
        }
        android.ndk {
            moduleName = "hello"
        }
    }

## Extending the Android plugin

### afterEvaluate

Old way:

    project.afterEvaluate {
        def task = project.tasks.getByName('transformClassesWithInstantRunForDebug')
        // ...
    }

* not part of the official API
* no guarantee that task names will be stable
* no guarantee that things won't be merged or split

Here's what the flow looks like if you want to write Gradle Android plugins:

<img alt="gradle extension flow" width="75%" src="https://storage.googleapis.com/ejf-io/android_dev_summit/gradle_extension_flow.png">

* If you want to work with tasks, you have to use `afterEvaluate`
* If you want to work with build types/flavors, etc., you can't use `afterEvaluate`
* There is no way to prepend to the `afterEvaluate` queue

### Variant API

The correct way to extend the Gradle Android plugin.

    android {
        applicationVariants.all {
            variant -> ...
        }
    }

Or:

    android {
        libraryVariants.all {
            variant -> ...
        }
    }

Note the `.all`, that's important!

* `getName()`
  * Unique Name for variants
* `getDirName()`
  * Unique folder segment(s)
* `getApplicationId()`
  * Computed application id

Read-only:

* `getBuildType()`
* `getProductFlavors()`
* `getMergedFlavor()`
* `getTaskName()`
* `registerJavaGeneratingTask()`
* `registerResGeneratingTask()`

Public and private task APIs may be broken. They're trying to get better about it.

### Transform API

Problem: inject tasks between `javac` and `dx`.

<img alt="javac to dx" width="75%" src="https://storage.googleapis.com/ejf-io/android_dev_summit/javac_dx.png">

> It's a mess!

Solution: Transform API

<img alt="javac to dx" width="75%" src="https://storage.googleapis.com/ejf-io/android_dev_summit/transform_api.png">

> Much cleaner!

* Pipeline of transforms
* API abstracts Gradle tasks, focus on the transform part
* Handles dependencies between transforms
  * Just register a new transform
* Support different types of content: class, resources and dex files
* Support different scopes: Project, sub-projects, etc.

`com.android.build.api.transform.Transform` methods:

* `getName()`
* `getInputTypes()`
* `getScopes()`
* `isIncremental()`
* `transform(...)`

Using it:

    dependencies {
        compile "com.android.tools.build:transform-api:2.0.0"
    }

The Transform API uses a fixed order, to reduce bugs.

Built-in transforms:

* Jacoco
* Proguard/ New shrinker
* InstantRun
* Dex

Limitations:

* Global transforms only
* No per-variant transform
* No order control through API

[Javadoc for Android Gradle DSL](http://google.github.io/android-gradle-dsl/javadoc)

## Inspecting your builds

The following Gradle tasks will help you to inspect your builds:

* `$./gradlew :app:dependencies [--configuration <name>]`
* `$./gradlew :app:sourceSets`
* `$./gradlew :app:signingReport`

## Instant Run

Only compiles, dex's, and delivers `.class` that has changed.

<img alt="instant run loader" width="75%" src="https://storage.googleapis.com/ejf-io/android_dev_summit/instant_run_loader.png">

* `$override` classes will appear in stack traces, and in the debugger
* New method calls use new code
  * recursive calls can be a mix of old and new method code
* Classes are loaded once, not reloaded
  * Class initializer changes are incompatible changes
* Static variables are not reloaded
* Methods are replaced
* Existing instances are untouched
  * instances fields of existing instances are not reinitialized
  * also true for singletons
* Restart app if you get weird behavior

## API specific Multi-APKs

Deprecated way:

    android {
        ...
        productFlavors {
            x86 {
                ndk {
                    abiFilter "x86"
                }
            }
            ...
            full {...}
        }
    }

New way of doing it:

    android {
        splits {
            abi {
                enabled true
                reset()
                include 'x86', 'armeabi-v7a', 'mips'
                universalApk true
            }
        }
    }

## Resource Shrinker

    android {
        buildTypes {
            release {
                minifyEnabled true
                shrinkResources true
                proguardFiles getDefaultProguardFile('proguard-android.txt')
            }
        }
    }

## Sharing constants between sub-projects

You can use Gradle extensions to create shared properties.

In `/build.gradle`:

    buildscript {
        ...
    }
    ext {
        toolsVersion = "23.0.2"
        deps = [
            guava: "com.google.guava:guava:18.0",
            ...
        ]
    }

In `/app/build.gradle`:

    android {
        buildToolsVersion rootProject.toolsVersion
        ...
    }
    dependencies {
        compile rootProject.deps.guava
    }

## Scoping

The `dependencies` block is a **top-level** attribute in the current version. Even if you add dependencies only within specific flavors of your app, they will be added to all flavors. Instead, if you prepend the product flavor to the compile command in the dependencies block, you can restrict it that way.

    android {
        productFlavors {
            free {...}
            paid {...}
        }
    }
    dependencies {
        freeCompile '...'
    }

## Flavor Dimensions

old:

    android {
        productFlavors {
            freeBlue
            freeRed
            premiumBlue
            premiumRed
        }
    }

New:

    android {
        flavorDimensions "price", "color"
        productFlavors {
            free { dimension "price" }
            premium { dimension "price" }
            blue { dimension "color" }
            red  { dimension "color" }
        }
    }

## Variant filters

    android {
        variantFilter { VariantFilter variant ->
            if (variant.flavors[0].name == "dev"
                && variant.buildType.name == "release") {
                // no point in having a devRelease variant
                variant.ignore = true
            }
        }
    }

## Vector Drawables

<img alt="vector drawables" width="75%" src="https://storage.googleapis.com/ejf-io/android_dev_summit/vector_drawables.png">

Vector in `res/drawable/vector.xml` (or xml), it will automatically generate all flavors. Generated flavors and densities can be configured in the `build.gradle` file. Officially supported in API 21, density specific pngs generated for older devices. Per flavor setting.
