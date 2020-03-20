
# Using sources instead of framework

## Vocab for this page

- Dependency: a thrid party project used by sdk
- Framework file: a package/folder `.framework` which contains binary (compiled code) for a dependency
- project: xcodeproj file (package/folder)
- workspace: xcodeworkspace file (package/folder) which contains a list of project

## State after building

When building there is a xcodeworkspace with only the main project

In project we have a group Frameworks/ThirdPart with only Frameworks

## What we want?

Add all the project(xcodeproj) of thrid parties in workspace.

All this project with carthage are under Carthage/Checkout

You can drag one by one into workspace into group ThirdParty

or you can use [punic](https://github.com/phimage/punic)
