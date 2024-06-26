cmake_minimum_required(VERSION 3.28)
project(pybind_test)

set(CMAKE_EXE_LINKER_FLAGS "-static")  # 解决exe文件无法打开的错误

#************************************pybind基础配置***********************************************************
# 添加包含目录
include_directories(
        "E:\\DataBuffer\\hurleykane\\miniconda3\\include"
        "pybind11\\include"
)

# 添加库目录
set(PYTHON_LIB_DIR "E:\\DataBuffer\\hurleykane\\miniconda3\\libs")
find_library(PYTHON3_LIB python3 PATHS ${PYTHON_LIB_DIR})
find_library(PYTHON311_LIB python311 PATHS ${PYTHON_LIB_DIR})
link_directories(${PTTHON_LIB_DIR})
# 添加依赖项函数
function(link_to_python target_module)
    target_link_libraries(${target_module} PUBLIC
            ${PYTHON3_LIB}
            ${PYTHON311_LIB}
    )
endfunction()

# 添加子目录 pybind11
add_subdirectory(extern/pybind11)
add_subdirectory(source)

# 添加字符设置
add_compile_options("$<$<C_COMPILER_ID:MSVC>:/utf-8>")
add_compile_options("$<$<CXX_COMPILER_ID:MSVC>:/utf-8>")

# main
add_executable(main main.cpp "source/class_cat.cpp")
target_include_directories(main PRIVATE ${PROJECT_SOURCE_DIR}/include)

