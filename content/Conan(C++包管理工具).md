```Python
常用命令：
conan version:查看python版本，system版本，cpu架构
conan search xxx从conancenter中搜索包
conan profile path default：查看跑配置文件路径

conan和CMakeLists配合使用的文章
https://www.cnblogs.com/microDeLe/p/15664173.html
```
```Plain
网址 https://docs.conan.io/2/tutorial/consuming_packages/different_configurations.html
操作步骤
1.pip install conan
conan查看
2.sorce ~/.profile
Conan配置文件来构建我们的项目。柯南 配置文件允许用户定义编译器、构建等内容的配置集 配置、架构、共享或静态库等
3.conan profile detect --force
下载项目所需要的库
4.conan install . --output-folder=build --build=missing
5.cd build
6.根据识别到的默认配置，去配置cmake
cmake .. -DCMAKE_TOOLCHAIN_FILE=conan_toolchain.cmake -DCMAKE_BUILD_TYPE=Release
cmake --build .
./compressor 

-----------------------
$ conan install . --build=missing
$ conan install . --build=missing --profile=default
如果需要修改：
将default，新建/profiles/debug->添加如下命令
build_type=Debug
-------------------
用conanfile.py替换conanfile.txt
conanfile.py:它将定义我们的需求（库和构建工具）和逻辑，以修改选项并设置我们希望如何使用这些包。
在使用此文件创建包的情况下，它可以定义（除其他事项外）如何下载包的源代码、如何从这些源代码构建二进制文件、如何打包二进制文件以及为未来消费者提供的有关如何消费的信息包。
和CMakeLists关系
Conan 负责管理依赖项，而 CMake 则负责配置和构建整个项目。这种方式可以更好地组织和管理 C++ 项目，并简化依赖项管理和构建过程。
```
```Python
from conans import ConanFile, CMake, tools
import os
import sys

class PackageConan(ConanFile):
    name = "AA_MapFactory_M"
    version = "0.0.1"
    settings = 'os', 'compiler', 'build_type', 'arch'
    description = 'none'
    url = 'https://git.lotuscars.com.cn/ad4_ad_dev/hmi/map_factory.git'
    license = 'Copyright (c) 2022, Lotus, All Rights Reserved.'
    topics = ()
    generators = 'cmake'

    requires = (
        # remove by hyz 20231130 'protoc/3.12.3@momenta/stable',
        'lutils/1.0.7@ad4-lotus/stable',
        'ltaskflow/1.0.8@ad4-lotus/stable',
        'nlohmann_json/3.9.1rc2@momenta/stable',
        'mf_mlog_publisher/v2.0.8@ad4-lotus/stable',
        # 'maf_interface/dev-maf3.1_20230731.15_31f4aac@ad4-lotus/stable',
        # 'maf_interface/dev-maf3.1_20231031.20_60b5404@ad4-lotus/stable',
        # 'maf_interface/dev-maf3.1_20231107.15_c68134e@ad4-lotus/stable',
        'maf_interface/loop3_v5.4.2_20240329.13_c075cb0@ad4-lotus/stable',
        # ara_struct_convert/loop3_v5.2.2-20231031@ad4-lotus/stable
        # remove by hyz 20231130 'opencv/3.4.17-contrib@ad4-lotus/stable',
        'argparse/V3.1.5_31232a73@momenta/stable',
        'cppzmq/v4.10.0@ad4-lotus/stable',
        #'ara_interface/loop3_v5.2.1@ad4-lotus/stable',
        'adg/2.7.0_IPQP@ad4-lotus/stable'
    )

    build_requires = (
        'googletest/v1.11.0-no-gmock@ad4-lotus/stable',
    )

    def requirements(self):
        if str(self.settings.os.platform).lower() == "orin":
            self.requires('breakpad/v2022.07.12@ad4-lotus/stable')

    def build(self):
        cmake = CMake(self)
        # cmake.verbose = True
        cmake.definitions['CMAKE_BUILD_TYPE'] = os.environ['CMAKE_BUILD_TYPE']
        cmake.configure()
        cmake.build(target=os.environ['LMAKE_BUILD_TARGET'])
        cmake.install()

    def package(self):
        def copy_deps_libs():
            for lib_dir in self.deps_cpp_info.libdirs:
                if not ("data/adg" in lib_dir or "rosruntime" in lib_dir):
                    self.copy('*.so*', dst='lib', src=lib_dir, symlinks=True)
        copy_deps_libs()

    def package_info(self):
        self.cpp_info.libs = tools.collect_libs(self)
```
```C++
cjl@cjl-ThinkPad-T14-Gen-2i:~/Desktop/code/0.cjl/testcanon$ cmake -S . -B build -DCMAKE_TOOLCHAIN_FILE=conan_toolchain.cmake -DCMAKE_BUILD_TYPE=Release
-S .：指定了源代码目录的路径，.表示当前目录，即项目根目录。
-B build：指定了构建目录的路径，这里设置为build目录，用于存放编译生成的中间文件和构建系统所需的文件。
-DCMAKE_TOOLCHAIN_FILE=conan_toolchain.cmake：通过该参数设置了CMAKE_TOOLCHAIN_FILE变量的值为conan_toolchain.cmake，这样CMake会使用指定的工具链文件来配置项目的构建。
-DCMAKE_BUILD_TYPE=Release：通过该参数设置了CMAKE_BUILD_TYPE变量的值为Release，表示以Release模式进行构建。

cjl@cjl-ThinkPad-T14-Gen-2i:~/Desktop/code/0.cjl/testcanon$ cmake --build build



cmake_minimum_required(VERSION 3.15)
project(Server)

find_package(cppzmq REQUIRED)

# include(${CMAKE_BINARY_DIR}/build/conan_toolchain.cmake)
# conan_basic_setup()
add_executable(${PROJECT_NAME} main.cpp)
target_link_libraries(${PROJECT_NAME} ${cppzmq_LIBRARIES})


from conan import ConanFile
from conan.tools.files import copy
import os
import sys
class Server(ConanFile):
    name = "server"
    version = "1.0"
    settings = "os", "compiler", "build_type", "arch"
    description = "none"
    license = "Copyright (c) 2024,CJL,All Rights Reserved" 
    topics = ()
    generators = 'CMakeToolchain',"CMakeDeps"

    def requirements(self):#这些以来可以放置在自己的制品网站
        self.requires("libwebp/1.3.2")#解决版本问题
        self.requires("cppzmq/4.5.0")
        self.requires("nlohmann_json/3.10.0")
        # self.requires("argparse/2.5")
        # self.requires("opencv/4.1.2")

        # self.requires("protobuf/3.13.0")
        

    def build(self):
        cmake=cmake(self)#创建cmake实例
        cmake.configure()#用cmake配置
        cmake.build()#通过cmake构建
        # 编译代码的逻辑，例如运行 cmake、make、msbuild 等构建命令def package(self):
    
    # def source(self):
    # # Please be aware that using the head of the branch instead of an immutable tag
    # # or commit is not a good practice in general.
    #     get(self, "https://github.com/conan-io/libhello/archive/refs/heads/main.zip",
    #         strip_root=True)

    def layout(self):
        cmake_layout(self, src_folder="src")
    
    # def package(self):
    #     self.copy("*.h", dst="include", src="TESTCANON/include")
    #     self.copy("*.lib", dst="lib", keep_path=False)
    def package_info(self):
        self.cpp_info.libs = ["zmq", "nlohmann_json"]
        
        
        https://blog.51cto.com/cerana/6382624
```