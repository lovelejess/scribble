---
title: libxml2 header not found using pods
date: 2015-11-4 20:52:23
---

Today I learned something new! Of course, learned it after a frustrating pounding my head day at work, but still learned something new, and I sort of understand how CocoaPods work!

I'm working in an iOS mobile application where *Project A* has a dependency on *Project B*, or in more English terms, *Project A* uses *Project B*. Dependencies in iOS applications are known as **CocoaPods**. *Project B* uses a library called [libxml2](http://www.xmlsoft.org/), which is an XML C parser. 

In order to apply this dependency, one must **pod install** using the *Podfile*. After the pod install, *Project B* now includes Project A (with the libxml2 library). 

However! I receive this awful build error:
 `"Lexical or Preprocessor Issue 'libxml2/tree.h' file not found"`

Upon inspection, the *tree.h* does exist and it's in the right location. So what could be wrong? If you [google](http://stackoverflow.com/questions/1428847/libxml-tree-h-no-such-file-or-directory) this error, you will receive the same instructions on how everyone (except me) has resolved this issue. 

Set `HEADER SEARCH PATHS` to `$(SDKROOT)/usr/include/libxml2` and other linker flag stuff, but we did do that in *Project A* & THAT DOESN'T WORK! *Project B* is having the not found issue! 

**The solution is:**

Make sure you set `spec.libraries` to the xml2 library and `spec.xconfig` to the `Header Search Paths`like in the Pod's (in this case, Project A) `.podspec` file so:

```
Pod::Spec.new do |spec|
  spec.libraries = ['xml2.2']
  spec.xconfig = { 'HEADER_SEARCH_PATHS' => '$(SDKROOT)/usr/include/libxml2' }
```

A [Podspec](https://guides.cocoapods.org/making/specs-and-specs-repo.html) file gives detailed information about the Pods being used in the Podfile. Setting the `spec.libraries` and `spec.xconfig` to the correct library and Header Search Paths allows the *Project B* to be able to find the correct header that is used in it's pod, *Project A*. Only setting *Project B'* project's `HEADER SEARCH PATHS` only includes the headers used in its project, and not the pods.

Hope if you bump into this problem, you'll know exactly what to do now :)