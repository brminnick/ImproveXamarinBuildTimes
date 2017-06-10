# Improve Xamarin Build Times

Tips and tricks on how to speed up the time it takes to compile a Xamarin app

## iOS Build Settings

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

## Android Build Settings

### General

![iOS Build Settings](./Images/Android_Build_Settings_General.png)

- Use Shared Mono Runtime 
  - This will install mono on the targeted device and allows the compiler to only compile the managed code
- Fast Assembly Deployment
   - Doesn’t compile assemblies and deploys assemblies directly to the device


### Linker

![iOS Build Settings](./Images/Android_Build_Settings_Linker.png)

- Linker Behavior: Don’t Link
  - Linking will cause compilation to take longer


### Advanced
![Android Advanced Build Settings](./Images/Android_Build_Settings_Advanced.png)

- Supported ABIs
  - Select the ABI for the targeted device
  - E.g. if you’re deploying to an x86 emulator, only select the x86 ABI
