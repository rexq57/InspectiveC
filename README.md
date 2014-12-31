InspectiveC
======

*MobileSubstrate based objc_msgSend hook for debugging/inspection purposes.*

Based on [itrace by emeau](https://github.com/emeau/itrace), [AspectiveC by saurik](http://svn.saurik.com/repos/menes/trunk/aspectivec/AspectiveC.mm), and [Subjective-C by kennytm](http://networkpx.blogspot.com/2009/09/introducing-subjective-c.html).

Logs output to **/tmp/InspectiveC** or **/var/mobile/Containers/Data/Application/\<App-Hex\>/Documents/InspectiveC** (sandbox). Inside the InspectiveC folder, you'll find **\<exe\>/\<pid\>_\<tid\>.log**.

This is **not compatible with arm64** at the moment, although I do hope to add support in the future.

**Features:**
* Watch specific objects
* Watch instances of a specific class
* Watch specific selectors
* Prints arguments

**Hopeful Features (in no particular order):**
* Print retvals
* Hook obj_msgSend[st|fp]ret
* More advanced filtering
* arm64 support???
* Optimizations
  * Nicer hooking
  * Reduce redundancy
  * Better multithreading performance

**Usage:**

Properly [install theos](http://iphonedevwiki.net/index.php/Theos/Setup) and grab yourself a copy
of the iOS SDK. You may have to modify the Makefile (i.e. ARCHS or TARGET) and/or InspectiveC.mm. I
compile this on my Mac with Clang - if you use anything different you may have some issues with the
assembly code.

Modify InspectiveC.plist to choose where to inject InspectiveC and just run "make package" to get
the dylib. Then, in your tweak, you can reference the following API by including InspectiveC.h.

```c
// Watches/unwatches the specified object. Objects will be automatically unwatched when they
// receive a -|dealloc| message.
void InspectiveC_watchObject(id obj);
void InspectiveC_unwatchObject(id obj);

// Watches/unwatches instances of the specified class ONLY - will not watch subclass instances.
void InspectiveC_watchInstancesOfClass(Class clazz);
void InspectiveC_unwatchInstancesOfClass(Class clazz);

// Watches/unwatches the specified selector.
void InspectiveC_watchSelector(SEL _cmd);
void InspectiveC_unwatchSelector(SEL _cmd);

// Enables/disables logging for the current thread.
void InspectiveC_enableLogging();
void InspectiveC_disableLogging();
```
