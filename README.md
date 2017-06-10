# About

## Purpose

Sometimes Xamarin Build times can take a long time, adding delays to our development timeline. Below are tips to reduce the compilation time for Debug Configuration builds.

The screenshots below were created on 10 June 2017 using [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/) v7.0.1 and [UITestSampleApp](https://github.com/brminnick/UITestSampleApp).

## Disclaimer

These recommendations are only meant for Debug Configuration builds. They will reduce the time for each compilation, but may increase the size of the app and decrease the performance and load-time of the app.

I highly recommend utilizing the Linker for Release Configuration builds; `Link SDKs and Frameworks` is my recommended minimum setting. I also highly recommend heavily testing the Release Configuration build of the app because the app may behave differently when the Linker is utilized.

# iOS Build Settings

![iOS Build Settings](./Images/iOS_Build_Settings.png)

- Linker behavior: Don’t Link
  - Linking will cause compilation to take longer
- Supported architecture: x86_64
  - Selecting multiple architectures will cause compilation to take longer
  - X86_64 works for iPhone Simulators 5s - 7
  - i386 works for iPhones earlier than the iPhone 5s
- Enable incremental builds
  - This will allow the compiler to only compile code that has changed
- Enable device-specific builds
  - This encourages the compiler to only compile for the targeted device

# Android Build Settings

## General

![iOS Build Settings](./Images/Android_Build_Settings_General.png)

- Use Shared Mono Runtime
  - This will install mono on the targeted device and allows the compiler to only compile the managed code
- Fast Assembly Deployment
  - Doesn’t compile assemblies and deploys assemblies directly to the device

## Linker

![iOS Build Settings](./Images/Android_Build_Settings_Linker.png)

- Linker Behavior: Don’t Link
  - Linking will cause compilation to take longer

## Advanced

![Android Advanced Build Settings](./Images/Android_Build_Settings_Advanced.png)

- Supported ABIs
  - Select the ABI for the targeted device
  - E.g. if you’re deploying to an x86 emulator, only select the x86 ABI

# Project References

![Edit References](./Images/ProjectReferences_EditReferences.png)

- Limit the number of referenced projects in the solution
- Compilation time increases for every referenced PCL
  - Referenced PCLs will compile first, then the Startup Project will compile
  - Using Shared Projects allows the project to compile faster than PCLs