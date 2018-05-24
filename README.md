# About

## Purpose

Sometimes Xamarin Build times can take a long time, adding delays to our development timeline. Below are tips to reduce the compilation time for Debug Configuration builds.

The screenshots below were created on 10 June 2017 using [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/) v7.0.1 and [UITestSampleApp](https://github.com/brminnick/UITestSampleApp).

## Disclaimer

These recommendations are only meant for Debug Configuration builds. They will reduce the time for each compilation, but may increase the size of the app and decrease the performance and load-time of the app.

I highly recommend utilizing the Linker for Release Configuration builds; `Link SDKs and Frameworks` is my recommended minimum setting. I also highly recommend heavily testing the Release Configuration build of the app because the app may behave differently when the Linker is utilized.

## Additional Resources

### Xamarin Videos

- [Understanding Implications of Build Options, Xamarin Evolve 2016](https://evolve.xamarin.com/session/56e21fe4bad314273ca4d825)

### Xamarin.iOS Documention

- [iOS Build Mechanics](https://developer.xamarin.com/guides/ios/advanced_topics/ios-build-mechanics/)

### Xamarin.Android Documentation

- [Build Process](https://developer.xamarin.com/guides/android/under_the_hood/build_process/)
- [Build ABI Specific APKs](https://developer.xamarin.com/guides/android/advanced_topics/build-abi-specific-apks/)
- [Linking on Android](https://developer.xamarin.com/guides/android/advanced_topics/linking/)

# iOS Build Settings

![iOS Build Settings](./Images/iOS_Build_Settings.png)

- Linker behavior: Don’t Link for Simulator Builds, Link on Devices Builds
  - The Linker will reduce the amount of managed code which means less native code to AOT compile
  - Simulator builds don't AOT compile, but Device builds do AOT compile
- Enable incremental builds
  - Allows the AOT compiler to compile only the code that changed
- Enable device-specific builds
  - This encourages the compiler to only compile for the targeted device
  - This allows you to select many supported architectures
  - Selecting multiple architectures without enabling device-specific builds will cause compilation to take longer
    - X86_64 Architecture works for iPhone Simulators 5s - 7
    - i386 Architecture works for iPhone Simulators earlier than the iPhone 5s

# Android Build Settings

## General

![Android Build Settings, General](./Images/Android_Build_Settings_General.png)

- Use Shared Mono Runtime
  - This will install Mono on the targeted device
  - Installing Mono on the device means that Mono doesn't have to be bundled in the APK, making the APK quicker to compile and deploy
- Fast Assembly Deployment
  - Deploys assemblies to a directory on the device instead of bundling them in the APK
  - The APK compiles faster, is smaller, and  is faster to deploy
  - Allows Xamarin to deploy individual changed assemblies instead of rebuilding/redeploying the entire APK

## Linker

![Android Build Settings, Linker](./Images/Android_Build_Settings_Linker.png)

- Linker Behavior: Link SDK assemblies only
  - The Linker will reduce the amount of managed code which means less native code to compile
  - Because Android uses emulators which behave similarly to Android devices, there isn't a recommended difference between Emulator & Device builds for the Linker settings like there is for iOS

## Advanced

![Android Build Settings, Advanced](./Images/Android_Build_Settings_Advanced.png)

- Supported ABIs
  - Select the ABI for the targeted device
  - E.g. if you’re deploying to an x86 emulator, only select the x86 ABI

## `CSPROJ`

![Android CSProj File](./Images/AndroidExplicitCrunch.png)

_NOTE: This is an experimental setting!_

- Add [`AndroidExplicitCrunch`](https://developer.xamarin.com/guides/android/under_the_hood/build_process/#Resource_Properties) to the Droid `csproj` file
  - When this property is set, the build process pre-crunches the .png files
  - Requires Xamarin.Android 7.0+
  - If you are building an app with a very large number of local drawables, an initial build (or rebuild) can take minutes to complete. To speed up the build process, set `AndroidExplicitCrunch` property to `true` in the `csproj` file.
  - It is not known if this setting will truly speed up your build. Time your initial build, and then a second build with no changes. Make the call if the setting helps your app or not.


# Project References

![Edit References](./Images/ProjectReferences_EditReferences.png)

- Limit the number of referenced projects in the solution
- Compilation time increases for every referenced PCL
  - Referenced PCLs will compile first, then the Startup Project will compile
  - Using Shared Projects allows the project to compile faster than PCLs

# Thank You

Thank you to all who have helped contribute to this doc!

- [Jon Douglas](https://twitter.com/_JonDouglas)
- [Kevin Ford](https://twitter.com/bowman74)
- [Mikayla Hutchinson](https://twitter.com/mjhutchinson)
- [Patrick Suzuki](https://github.com/patrickluvsoj)
- [Pierce Boggan](https://twitter.com/pierceboggan)
- [Rob DeRosa](https://github.com/rob-derosa)
- [Jonathan Peppers](https://jonathanpeppers.com)
