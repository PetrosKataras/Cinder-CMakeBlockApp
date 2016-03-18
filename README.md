Cinder-CMakeBlockApp
=====================

Test Cinder app for illustrating linking and including blocks through CMake on the current Cinder android_linux branch.

The app links and uses the standard OSC block and Cinder-Skinning from [this](https://github.com/num3ric/Cinder-Skinning/tree/glnext) branch. It should be located located under the samples directory.

If you want to have your app in a custom directory you should only need to define where it can find Cinder and also update the resource paths in the include dir.

It also assumes that assimp was installed with `sudo make install` on its default installation dir which on Linux is `/usr/local/lib` and `/usr/local/include`.
