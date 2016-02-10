# Commands

The following is an outline of some of the commands we'll know and love when
working with OS X on the command-line only.

## xcodebuild

Majority of builds can be automated by configuring your `xcodeproj`
appropriately, storing it in your SCM, and running a command similar to the
following...

    xcodebuild -project MyProject.xcodeproj \
               -target MyProject \
               -configuration Debug \
               -sdk iphonesimulator5.0 clean build

Here's a breakdown...

- `xcodebuild` is a tool we can use to, you guessed it, run Xcode builds on the
  command-line.

- `-project MyProject.xcodeproj` defines the `xcodeproj` file holding our
  project configuration and build settings.

- `-target MyProject` the actual result of this entire build process.

- `-configuration Debug` the build level or context we are building our target
  for. This could be `Test` or something custom.

- `-sdk iphonesimulator5.0` matches the iOS SDK we are building our target
  against. Notice the ancient SDK version.

- `clean` wipes away our project build environment before performing our latest
  build.

- `build` is yup, the build.

Let's look at another...

    xcodebuild -workspace Foo.xcworkspace \
               -scheme Foo \
               -sdk iphonesimulator \
               -configuration Debug \
               ONLY_ACTIVE_ARCH=NO
               TEST_AFTER_BUILD=YES
               clean build

## xcrun

The following will package your .ipa file.

    xcrun -sdk iphoneos PackageApplication \
          <PRODUCT.app> \
          -o <PRODUCT.ipa>

...or re-sign the .ipa.

    xcrun -sdk iphoneos PackageApplication \
          <PRODUCT.app> \
          -o <PRODUCT.ipa> \
          --sign <CODESIGN_IDENTITY> \
          --embed <PROVISONING_PROFILE>

