# Simple programs to locate the bug in current glfw development branch.

## Checkout and build

```shell
$ git clone --recursive https://github.com/zhanggyb/glfw_bug.git
```

This command also checkout current glfw source code from https://github.com/glfw/glfw.git into glfw_bug/glfw. (as a sub module)

To build and run:

```shell
$ cd glfw_bug
$ mkdir build
$ cd build
$ cmake ../
$ ./bin/glfw_bug
```
