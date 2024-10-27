# Playgrounds App in Xcode

## Objective

In an attempt to build a [Swift Playground App using Xcode
Cloud](https://developer.apple.com/documentation/xcode/building-swift-packages-or-swift-playground-app-projects-with-xcode-cloud),
I realized that the generated Xcode Archive (`*.xcarchive`) did not contain the app bundle, causing the code signing
operations to fail with exit code `70`.

This repository is an attempt at creating an open source reference for this issue and (after resolving the problem) a
potential template for people to re-create the same, simplified setup.

## Verify Direct Build

✅  The direct build will not work in Xcode Cloud, however this attempt is a demonstration of success.

```
> xcodebuild \
    -workspace playgrounds-app.swiftpm/.swiftpm/xcode/package.xcworkspace \
    -scheme 'Playgrounds App' \
    -destination generic/platform=iOS \
    -archivePath result.xcarchive \
    archive
> ls -l result.xcarchive/Products/Applications
total 0
drwxr-xr-x  10 herzi  staff  320 27 Oct 13:48 Playgrounds App.app
```

## Use Xcode Project Wrapper

⛔️ The following invocation works, but does copy the app into the archive.

```
> xcodebuild \
    -project playgrounds-app.xcodeproj \
    -scheme 'Playgrounds App' \
    -destination generic/platform=iOS \
    -archivePath result.xcarchive \
    archive
> ls -l result.xcarchive/Products/Applications
ls: result.xcarchive/Products/Applications: No such file or directory
```

## Introduce `SKIP_INSTALL=no`

✅ The following invocation works on the toplevel project and creates an archive that can be validated and exported.

```
> xcodebuild \
    -project playgrounds-app.xcodeproj \
    -scheme 'Playgrounds App' \
    -destination generic/platform=iOS \
    -archivePath result.xcarchive\
    archive \
    SKIP_INSTALL=no
> ls -l result.xcarchive/Products/Applications
total 0
drwxr-xr-x  10 herzi  staff  320 27 Oct 13:54 Playgrounds App.app
```
