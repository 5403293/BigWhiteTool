cmake_minimum_required(VERSION 3.21)
# 设置NDK路径
set(NDK_PATH D:/Path/android-ndk-r26b)

##################### Android设置 #####################
set(CMAKE_SYSTEM_NAME ANDROID) # 设置目标编译平台参数 Android
set(CMAKE_BUILD_TYPE Release)
set(CMAKE_SYSTEM_VERSION 21) # 系统版本
set(ANDROID_PLATFORM 21) # 平台版本
set(ANDROID_SDK_TOOLS_VERSION 20) # 平台版本
set(ANDROID_ABI arm64-v8a) # 设置目标构架 armeabi-v7a arm64-v8a x86 x86_64
set(ANDROID_NDK ${NDK_PATH}) # 设置ndk路径
set(CMAKE_TOOLCHAIN_FILE ${NDK_PATH}/build/cmake/android.toolchain.cmake)
set(ANDROID_SDK_ROOT ${NDK_PATH})


project(BigWhiteTool)


set(CMAKE_CXX_STANDARD 17)

##################### 输出文件重定向 #####################
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY
        ${CMAKE_SOURCE_DIR}/outputs/${CMAKE_ANDROID_ARCH_ABI}/
        ) # 重定向输出产物(可执行文件)
##################### 输出文件重定向 #####################

##################### CMake头文件设置 #####################
# 遍历子目录下所有符合情况的头文件
FILE(GLOB_RECURSE FILE_INCLUDES include/*.h*)
include_directories( # 设置全局头文件目录 使其他源码文件可在任意目录<头文件.h>
        include/
        include/ImGui
        include/Android_touch
        include/Android_shm
        include/native_surface
        include/Android_draw
        include/Android_dump
        include/request
        include/tools
        )
##################### CMake头文件设置 #####################

##################### CMake源文件设置 #####################
FILE(GLOB_RECURSE FILE_SOURCES # 遍历子目录下所有符合情况的源文件
        src/*.c*
        )
##################### CMake源文件设置 #####################

##################### 设置三方库文件目录 #####################
link_directories(
        ffmpeg/lib/${ANDROID_ABI}
)
#[[
add_library(biu STATIC IMPORTED)
set_target_properties(biu PROPERTIES IMPORTED_LOCATION ${CMAKE_SOURCE_DIR}/libs/libbiu.a)
]]


##################### 设置三方库文件目录 #####################
#add_subdirectory(DumperSDK)

##################### 添加产物 #####################
add_executable(BigWhiteTool # 生成可执行文件
        ${FILE_INCLUDES} # 头文件
        ${FILE_SOURCES} # 源文件
        src/BigWhiteTool.cpp)



##################### 连接库文件 #####################
# 可以整合第三方库 需要打开注释即可
target_link_libraries(BigWhiteTool PRIVATE EGL GLESv3 log
        android
        #biu
#        UE5Dumper
        )
