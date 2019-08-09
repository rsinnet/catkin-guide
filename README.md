# Guide to the Catkin Build System

The catkin build system uses two files: `package.xml` and `CMakeLists.txt`

## `Package.xml`

### `depend`
The `depend` tag is equivalent to adding `build_depend`, `build_export_depend`, and `exec_depend`.

For some packages, an `exec_depend` may not be required.
This means that package doesn't have to be installed on the deployed. An example of this is Eigen 3 which is a headerless library.
Any included code is compiled into your binaries.

### `build_depend`

This is for specifying packages which are required at build time.
Anything listed here will also need to be `find-packaged()`-ed in the `CMakeLists.txt` file.

For packages with C++ headers and/or libraries, a `build_depend` tag is required.

If a package has only Python libraries and/or scripts along with some run-time data like a `launch` file, it does not require a `build_depend`.

For packages that generate messages, a `build_depend` is required because the messages are generated at runtime.
This includes both C++, Python, and Lisp bindings.

If a package has CMake configuration files, then a `build_depend` is required.

### `build_export_depend`
The `build_export_depend` tag allows you to build a _dev_ package which includes header files (installed with a _runtime_ package).
This _dev_ package may be used to build against your package (headers and libraries) without necessarily including all the dependencies required to build your package.
This can happen when you include header files from an external package in your `.cpp` files but not in your header files.
If you're using data structures from an external package, it's often not possible to do this because these data structures may be used in your public API.
However, if you're using algorithms (like a vision algorithm from OpenCV), then you may be able to get away with having a `build_depend` but no `build_export_depend`.
By correctly restricting dependencies, a developer building against package will not have to install as many dependencies.

## `CMakeLists.txt`

### `find_package`
`find_package` is a CMake command that essentially helps find required include and library paths as well as required compiler flags.
Thus, this is only required for build depenencies.
(Note: Another method to achieve this functionality is `pkg-config`.)

Using `find_package` outside of Catkin, a user would typically provide a mypackageConfig.cmake or FindMyPackage.cmake file which gets installed into the `CMAKE_MODULE_PATH`.
With Catkin, this is a bit different as Catkin provides this `find_package` functionality without required a bunch of boilerplate code.
For a Catkin package, one calls `find_package` on Catkin itself and the Catkin package is treated as a component; i.e.:
```
find_package(catkin COMPONENTS mypackage)
```
Some of the implementation details can be found in the links below.

*See also*:
- https://cmake.org/cmake/help/latest/command/find_package.html
- https://github.com/ros/catkin/blob/kinetic-devel/cmake/catkinConfig.cmake.in
- https://github.com/ros/catkin/blob/kinetic-devel/cmake/templates/pkgConfig.cmake.in

If using `find_package` with non-Catkin components, there is a ROS Package that provides some common CMake modules for use with `find_package`:
-https://github.com/ros/cmake_modules/blob/0.4-devel/README.md

### `catkin_package`
The `catkin_package` macro is needed in a package, say `my_package`, to define CMake variable exports.
For example, what include directories are required to build against this package?


#### `CATKIN_DEPENDS`
Any package that appears as `CATKIN_DEPENDS` must be listed in the `find_package(catkin ...)` command.
E.g.:
```
find_package(catkin REQUIRED COMPONENTS some_package)
catkin_package(CATKIN_DEPENDS some_package)
```


### See Also
- http://wiki.ros.org/catkin/package.xml
- http://docs.ros.org/melodic/api/catkin/html/howto/format1/catkin_library_dependencies.html
- https://github.com/ros/catkin/blob/kinetic-devel/cmake/catkin_package.cmake
