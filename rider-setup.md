
## Bonus: IDE (especially JetBrains Rider) support

This guide has been tested successfully:
- Arch Linux as of Feb 3, 2018
- Rider 2017.3

Current Status (Tested):
- Fully functional project build/run
- Remote debugging and breakpoints in Rider

This will enable building Xamarin.Android projects directly from `msbuild`.

**DISCLAIMER: This is not officially supported. Rider only supports Xamarin on Windows/macOS, as Xamarin does not officially support or provide packages for Linux.**

This process may thus break at any time, so proceed at your own risk.

**WARNING: These steps require root access and modifying your Mono installation.**

First, change to your unpacked build of Xamarin.Android:

```sh
cd /path/to/xamarin.android-oss*/
```

Now, you will need to copy build configuration and tools to `/usr/lib/mono`.

```sh
sudo cp -r bin/Debug/lib/xamarin.android/* /usr/lib/mono
```

This command will add the `MonoAndroid` target framework files to `/usr/lib/mono/xbuild-frameworks`, where they will be recognized as target frameworks by `xbuild`.

Let's also copy the `Xamarin.Android` tools in `bin`:

```sh
sudo mkdir -p /usr/lib/mono/xamarin-android/bin/
sudo cp -r bin/Debug/bin/* /usr/lib/mono/xamarin-android/bin/
```

This will put the MSBuild targets where `msbuild` expects them.

Now, you will have to add additional props to your `.csproj` file to help `msbuild` find
the frameworks and tools it needs.

Add a conditional `PropertyGroup` for Linux like this:

```xml
<PropertyGroup Condition=" '$(OS)' == 'Unix' ">
    <MonoAndroidBinDirectory>/usr/lib/mono/xamarin-android/bin</MonoAndroidBinDirectory>
    <AndroidSdkDirectory>$(ANDROID_SDK_PATH)</AndroidSdkDirectory>
    <AndroidNdkDirectory>$(ANDROID_NDK_PATH)</AndroidNdkDirectory>

    <TargetFrameworkRootPath>/usr/lib/mono/xbuild-frameworks/</TargetFrameworkRootPath>
</PropertyGroup>
```

This will tell `msbuild` where to find build tools and scripts for **Xamarin.Android**.

The last line tells `msbuild` to use the `xbuild-frameworks` as the root path for target frameworks, which is where we copied the `MonoAndroid` reference assemblies.
Without this, the build process will complain about being unable to resolve the target framework.

Make sure your `ANDROID_SDK_PATH` and `ANDROID_NDK_PATH` environment variables are set, or change the paths accordingly.

**You should now be able to build your Xamarin.Android project from the command line!**

Finally, there's one last trick Rider has: it will provide its own Android SDK path
by default. To fix that, go into Rider's settings and change the path to the Android SDK
in its options.

You should now be able to open and successfully build **Xamarin.Android** projects in Rider! We did it! :)

I have successfully the remote debugger, and it worked as far as I tested it!
