
## Bonus: IDE (especially JetBrains Rider) support

This will enable building Xamarin.Android projects directly from `msbuild`.

**DISCLAIMER: This is not officially supported. Rider only supports Xamarin on Windows/macOS, as Xamarin does not officially support or provide packages for Linux.**

**WARNING: These steps require root access and modifying your Mono installation.**

First, change to your unpacked build of Xamarin.Android:

```sh
cd /path/to/xamarin.android-oss*/
```

Now, you will need to copy build configuration and tools to `/usr/lib/mono`.

```sh
sudo cp -r bin/Debug/lib/* /usr/lib/mono
```

Let's also copy the `Xamarin.Android` tools in `bin`:

```sh
sudo mkdir -p /usr/lib/mono/xamarin-android/bin/
sudo cp -r bin/Debug/bin/* /usr/lib/mono/xamarin-android/bin/
```

This will put the MSBuild targets where `msbuild` expects them.

At this point, if you try to build, `msbuild` will complain about being unable to locate `mandroid`.

Now, you will have to add additional props to your `.csproj` file to help `msbuild` find
the tools it needs.

Add a conditional `PropertyGroup` for Linux like this:

```xml
<PropertyGroup Condition=" '$(OS)' == 'Unix' ">
    <MonoAndroidToolsDirectory>/usr/lib/mono/mandroid</MonoAndroidToolsDirectory>
    <MonoAndroidBinDirectory>/usr/lib/mono/xamarin-android/bin</MonoAndroidBinDirectory>
    <AndroidSdkDirectory>$(ANDROID_SDK_PATH)</AndroidSdkDirectory>
    <AndroidNdkDirectory>$(ANDROID_NDK_PATH)</AndroidNdkDirectory>
</PropertyGroup>
```

This will tell `msbuild` where to find build tools and scripts for **Xamarin.Android**!
Make sure your `ANDROID_SDK_PATH` and `ANDROID_NDK_PATH` environment variables are set,
or change the paths accordingly.

**You should now be able to build your Xamarin.Android project from the command line!**

Finally, there's one last trick Rider has: it will provide its own Android SDK path
by default. To fix that, go into Rider's settings and change the path to the Android SDK
in its options.

You should now be able to open and successfully build **Xamarin.Android** projects in Rider! We did it! :)
