{
    "slug": "devtools",
    "date": "2015-05-28T00:13:00.000Z",
    "tags": ["io15","googleio"],
    "title": "What's new in Android Development Tools",
    "publishdate": "2015-05-28T00:13:00.000Z"
}

What's new in Android Development Tools
=====

1. Design Tools
1. Faster Emulator
1. NDK

## Design

Android Design Support Library, backwards compatible.

### Vector drawables

Auto-generate the correct asset sizes at build time, may decrease apk size. Allows adding tint programtically (check on that one).

### Visual Design Editors

Don't need to rely solely on xml editors.

## Develop

Gradle Plugin post 1.0

* Correctness
* Performance
* NDK
* Testing

### Build Steps Performance

* Jack
  * compiles straight from Java to Dex file
* PNG Cruncher
  * much faster
* Coming: aapt

### Gradle Overhead

* Building the Model
* Flavor Build Type Variants
* ...?

### Next Gen Gradle Plugin

* Do less
* Cache what doesn't change
* More optimization/parallelism

However, if dependencies change, wondering if those projects need to be explicitly rebuilt, or if Gradle will pick up that there's a change.

* Similar DSL
* Preview in a few weeks
* Final later this year

### Data Binding

* Build time support
* Experimental plugin for now
* Will move to 1.3 plugin before release

### NDK C/C++ Support - Gradle

* Based on Native Support in Gradle
* Next Gen Plugin only for now

### NDK C/C++ Support - Studio

* Based on CLion from JetBrains
* Debugging & Refactoring
* Free for Android Development

Support for C/C++ in Studio looks very similar to Java support. Many of IDE's utilities can be used in the same way for NDK stuff. JNI is also supported.

### Unit Test Support

* Runs on Desktop JVM
* All Android Code *must* be mocked
* src/test/java

### Test story

* Android Studio
* Android Testing Library
* Cloud Test Lab
* Google Play Testing

### Emulator

* Working on Performance and Stability
  * Auto-installs HAXM
  * Fixed thousands of issues in GL renderer
* Test M Developer Preview Features

### Android Auto Emulator

New version of the Auto Emulator coming.

### Annotations in Studio

Annotations can help to enforce certain assumptions about threading. It can help with array sizes, and float ranges. This means that you can specify if a primate array is expected to be of length 2, that if an array is passed in that has a different size, it can warn you or fail on that. This can also be used for M's new permissions requirements.

### Debugging

There are new debugging tools that allow you to get more useful information about variables listed in the debugger, and dig through the code to find what they relate to. May require Annotations.

### Data Binding

Maps 'find view by id' to something more sane.
