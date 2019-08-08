# Guide to the Catkin Build System

## Package.xml

### depend
The `depend` tag is equivalent to adding `build_depend`, `build_export_depend`, and `exec_depend`.

For some packages, an `exec_depend` may not be required.
This means that package doesn't have to be installed on the deployed. An example of this is Eigen 3 which is a headerless library.
Any included code is compiled into your binaries.

### build_depend

This is for specifying packages which are required at build time.
Anything listed here will also need to be `find-packaged()`-ed in the `CMakeListss.txt` file.

For packages with C++ headers and/or libraries, a `build_depend` tag is required.

If a package has only Python libraries and/or scripts along with some run-time data like a `launch` file, it does not require a `build_depend`.

For packages that generate messages, a `build_depend` is required because the messages are generated at runtime.
This includes both C++, Python, and Lisp bindings.

If a package has CMake configuration files, then a `build_depend` is required.

### build_export_depend
The `build_export_depend` tag allows you to build a _dev_ package which includes header files (installed with a _runtime_ package).
This _dev_ package may be used to build against your package (headers and libraries) without necessarily including all the dependencies required to build your package.
This can happen when you include header files from an external package in your `.cpp` files but not in your header files.
If you're using data structures from an external package, it's often not possible to do this because these data structures may be used in your public API.
However, if you're using algorithms (like a vision algorithm from OpenCV), then you may be able to get away with having a `build_depend` but no `build_export_depend`.
By correctly restricting dependencies, a developer building against package will not have to install as many dependencies.

### See Also
- http://wiki.ros.org/catkin/package.xml
- http://docs.ros.org/melodic/api/catkin/html/howto/format1/catkin_library_dependencies.html
