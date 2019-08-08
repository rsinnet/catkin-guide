# Guide to the Catkin Build System

## Package.xml

### build_depend

This is for specifying packages which are required at build time.
Anything listed here will also need to be `find-packaged()`-ed in the `CMakeListss.txt` file.

For packages with C++ headers and/or libraries, a `build_depend` tag is required.

If a package has only Python libraries and/or scripts along with some run-time data like a `launch` file, it does not require a `build_depend`.

For packages that generate messages, a `build_depend` is required because the messages are generated at runtime.
This includes both C++, Python, and Lisp bindings.


### See Also
- http://wiki.ros.org/catkin/package.xml
