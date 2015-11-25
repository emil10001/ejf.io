+++
date = "2015-11-24T11:00:53-08:00"
draft = true
title = "Android Build System"

+++

<iframe width="560" height="315" src="https://www.youtube.com/embed/OOEDKf06WqA" frameborder="0" allowfullscreen></iframe>

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

Try `dexInProgress = true`

`minSdkVersion 21` is going to be faster.

## Testing

Unit testing config:

    unitTests.all {
        jvmArgs "-XX:MaxPermSize=256m"

        beforeTest -> {...}
    }

Use a separate project just for the test APK.

Version conflicts:

Main APK and Test APK need to have the same Build Tools version.

    dependencies {
        compile ".."
        // ?
    }

## Native Support

new plugins for native

* com.android.model.application - *\*.apk*
* com.android.model.library - *\*.aar*
* com.android.model.native - *\*.so*

    ndk.with {
        moduleName = "native"
    }

New thing?:

    apply plugin: 'com.android.model.application'
    main {
        jni {
            dependencies {
                project ":lib"
            }
        }
    }

## Extending the Android plugin

* no guarantee that task names will be stable
* no guarantee that things won't be merged or split

`afterEvaluate`

* If you want to work with tasks, you have to use `afterEvaluate`
* If you want to work with build types/flavors, etc., you can't use `afterEvaluate`
* There is no way to prepend to the `afterEvaluate` queue

### Variant API

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

* `getName()`
* `getDirName()`
* `getApplicationId()`

Read-only:

* `getBuildType()`
* `getProductFlavors()`
* `getMergedFlavor()`
* `getTaskName()`
* `registerJavaGeneratingTask()`
* `registerResGeneratingTask()`

Public and private task APIs may be broken. They're trying to get better about it.

### Transform API

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

Limitations:

* Global transforms only
* No per-variant transform
* No order control through API

## Gradle tasks

`$./gradlew :app:dependencies [--configuration <name>]`

`$./gradlew :app:sourceSets`

`$./gradlew :app:signingReport`

## Instant Run

Only compiles, dex's, and delivers `.class` that has changed.

* `$override` classes will appear in stack traces, and in the debugger
* New method calls use new code
  * recursive calls can be a mix of old and new method code

* Methods are replaced
  * ...
* Existing instances are untouched
  * instances fields of existing instances are not reinitialized
  * also true for singletons

## API specific Multi-APKs

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

using gradle extensions to create properties

<code snip>

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
            free {dimension price???}
            premium
            blue
        }
    }

## Vector Drawables

Vector in `res/drawable/vector.svg` (or xml), it will automatically generate all flavors. Generated flavors and densities can be configured in the `build.gradle` file.
