# xamarin-android-linux

A complete guide to getting **Xamarin.Android** ready to use on Linux!

## Why?

Xamarin has stated that there are no plans to officially support Linux. However,
since they open sourced Xamarin, recently they have added support for building on Linux!

## Let's start

## Prerequisites

- Latest `mono` packages for your platform

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

Build a test project (there's one in the source repository) for Xamarin.Android.

```sh
/path/to/oss-xamarin.android*/bin/Debug/bin/xabuild /path/to/YourTestProject.csproj
```

The build should complete successfully. Next, try building a signed package:

```sh
/path/to/oss-xamarin.android*/bin/Debug/bin/xabuild /path/to/YourTestProject.csproj /t:SignAndroidPackage
```

This will invoke `keytool` and generate a keystore and use it to sign an `.apk` file, which
will go to your output directory with a name like `YourTestProject-Signed.apk`.

## Bonus: IDE (especially JetBrains Rider) support

This will enable building Xamarin.Android projects directly from `msbuild`.

**DISCLAIMER: This is not officially supported. AFAIK Rider only supports Xamarin on Windows/macOS, as
Xamarin does not officially support or provide packages for Linux.**

**WARNING: These steps require root access and modifying your Mono installation.**

First, change to your unpacked build of Xamarin.Android:

```sh
cd /path/to/oss-xamarin.android*/
```

Now, you will need to copy build configuration and tools to `/usr/lib/mono`.

```sh
sudo cp -r bin/Debug/lib/* /usr/lib/mono
```

This will put the MSBuild targets where `msbuild` expects them.
