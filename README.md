# xamarin-android-linux

A complete guide to getting **Xamarin.Android** ready to use on Linux!

## Why?

Xamarin has stated that there are no plans to officially support Linux. However,
since they open sourced Xamarin, recently they have added support for building on Linux!

## Let's start

## Obtain `Xamarin.Android` binaries

### Prebuilt Releases (Recommended)

The latest successful artifacts from automated builds for Linux are available here: <https://jenkins.mono-project.com/view/Xamarin.Android/job/xamarin-android-linux/lastSuccessfulBuild/Azure/>

Download the `oss-xamarin.android*` package to your system and extract it.

### From source

See their [instructions](https://github.com/xamarin/xamarin-android/blob/master/README.md) on building from source.

## Environment setup

Install the Android SDK and NDK, then set environment variables:

(You may have to adjust the paths; these are the default for Android Studio)

```sh
export ANDROID_SDK_PATH=$HOME/Android/Sdk
export ANDROID_NDK_PATH=$HOME/Android/Sdk/ndk-bundle
```

## Test build

Build a test project (there's one in the source repository) for Xamarin.Android
