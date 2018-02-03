
# Setting up Xamarin.Android on Linux

A guide to getting **Xamarin.Android** ready to use on your distro of choice!

This guide has been tested successfully:
- Arch Linux as of Feb 3, 2018

Note that this is officially unsupported, but it can be used as a loose
guide to help in setting this up on your system.

[![works badge](https://cdn.rawgit.com/nikku/works-on-my-machine/v0.2.0/badge.svg)](https://github.com/nikku/works-on-my-machine)

## Why?

Xamarin has stated that there are no plans to officially support Linux. However,
since they open sourced Xamarin, recently they have added support for building on Linux!

## Let's start

## Prerequisites

- Latest `mono` packages for your platform

## Obtain `Xamarin.Android` binaries

### Prebuilt Releases (Recommended)

The latest successful artifacts from automated builds for Linux are available here: <https://jenkins.mono-project.com/view/Xamarin.Android/job/xamarin-android-linux/lastSuccessfulBuild/Azure/>

Download the `xamarin.android-oss_*.orig.tar.bz2` package to your system and extract it.

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
/path/to/oss-xamarin.android*/bin/Debug/bin/xabuild /path/to/samples/HelloWorld/HelloWorld.csproj
```

The build should complete successfully.
If you have problems, try adding `/verbosity:d` to get detailed output from the build process.
You may have a minor error about `libzip.so.4` not being found, just create a symlink to your libzip:

```sh
sudo ln -s /usr/lib/libzip.so /usr/lib/libzip.so.4
```

Next, try building a signed package:

```sh
/path/to/oss-xamarin.android*/bin/Debug/bin/xabuild /path/to/samples/HelloWorld/HelloWorld.csproj /t:SignAndroidPackage
```

This will invoke `keytool` and generate a keystore and use it to sign an `.apk` file, which
will go to your output directory with a name like `YourTestProject-Signed.apk`.

## Bonus: IDE (especially JetBrains Rider) support

This will enable building Xamarin.Android projects directly from `msbuild`.

Please refer to [rider-setup.md](rider-setup.md).
