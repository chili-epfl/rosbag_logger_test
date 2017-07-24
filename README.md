Attempt to cross-compile a basic `rosbag_storage` command-line test program for Android for the purposes of building a
QML plugin later on.

First, get `crystax-ndk-10.3.2`. Extract it to `/opt/`. Then from within `/opt/crystax-ndk-10.3.2/`, run

```
sudo ./build/tools/make-standalone-toolchain.sh --arch=arm --toolchain=arm-linux-androideabi-4.9 --platform=android-14 --install-dir=/opt/crystax-standalone-toolchain
```

Then, get `android-cmake`:

```
cd ~/src/ && git clone git@github.com:taka-no-me/android-cmake.git
```

Then, build and install the following one by one, using their `README-ANDROID.md`s:

https://github.com/chili-epfl/console_bridge/tree/android-cross-compile-test

https://github.com/chili-epfl/lz4/tree/android-cross-compile-test

https://github.com/chili-epfl/ros_comm/tree/android-cross-compile-test/utilities/roslz4

https://github.com/chili-epfl/roscpp_core/tree/android-cross-compile-test/cpp_common

https://github.com/chili-epfl/roscpp_core/tree/android-cross-compile-test/rostime

https://github.com/chili-epfl/roscpp_core/tree/android-cross-compile-test/roscpp_traits

https://github.com/chili-epfl/roscpp_core/tree/android-cross-compile-test/roscpp_serialization

https://github.com/chili-epfl/ros_comm/tree/android-cross-compile-test/tools/rosbag_storage

https://github.com/chili-epfl/std_msgs/tree/android-cross-compile-test

Finally, attempt to build this test by:

```
mkdir build-android && cd build-android
cmake -DCMAKE_TOOLCHAIN_FILE=~/src/android-cmake/android.toolchain.cmake -DANDROID_STANDALONE_TOOLCHAIN=/opt/crystax-standalone-toolchain -DCMAKE_INSTALL_PREFIX=/opt/crystax-standalone-toolchain/sysroot/usr/ -DCMAKE_BUILD_TYPE=Release ..
make -j 5
```

You should get `error: undefined reference to 'BZ2_*'` type of linker errors. This is normal, these symbols are missing
for some reason from the Crystax libraries. The solution at this point is to cross compile bzip2 manually.

What is weird is that you'll also get `ros::Header::*` type of errors. These symbols are in `cpp_common` libraries, they
are being linked but for some reason the symbols themselves are not found. To be investigated.
