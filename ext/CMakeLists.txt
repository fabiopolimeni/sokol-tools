fips_begin_lib(getopt)
    fips_dir(getopt/src GROUP .)
    fips_files(getopt.c)
    fips_dir(getopt/include/getopt GROUP .)
    fips_files(getopt.h)
fips_end_lib()
target_include_directories(getopt PUBLIC "getopt/include")

fips_begin_lib(pystring)
    fips_dir(pystring GROUP .)
    fips_files(pystring.cpp pystring.h)
fips_end_lib()
target_include_directories(pystring PUBLIC "pystring")

fips_begin_lib(SPIRV-Tools-opt)
    fips_src(SPIRV-Tools/source/opt GROUP .)
fips_end_lib()
target_include_directories(SPIRV-Tools-opt PRIVATE "generated" "SPIRV-Tools" "SPIRV-Headers/include")
target_include_directories(SPIRV-Tools-opt PUBLIC "SPIRV-Tools/include")

fips_begin_lib(glslang)
    fips_src(glslang/glslang EXCEPT OSDependent/* GROUP "glslang")
    fips_src(glslang/SPIRV GROUP "SPIRV")
    fips_deps(SPIRV-Tools-opt)
fips_end_lib()
target_include_directories(glslang PRIVATE "glslang/glslang")
target_include_directories(glslang PRIVATE "glslang/SPIRV")
target_compile_definitions(glslang PRIVATE ENABLE_OPT=1)
if (FIPS_WINDOWS)
    target_compile_definitions(glslang PRIVATE GLSLANG_OSINCLUDE_WIN32)
else()
    target_compile_definitions(glslang PRIVATE GLSLANG_OSINCLUDE_UNIX)
endif()

fips_begin_lib(SPIRV-Cross)
    fips_dir(SPIRV-Cross GROUP .)
    fips_files(
        GLSL.std.450.h
        spirv_common.hpp
        spirv_cross_containers.hpp
        spirv_cross_error_handling.hpp
        spirv.hpp
        spirv_cross.hpp spirv_cross.cpp
        spirv_parser.hpp spirv_parser.cpp
        spirv_cross_parsed_ir.hpp spirv_cross_parsed_ir.cpp
        spirv_cfg.hpp spirv_cfg.cpp
        spirv_glsl.hpp spirv_glsl.cpp
        spirv_msl.hpp spirv_msl.cpp
        spirv_hlsl.hpp spirv_hlsl.cpp
        spirv_reflect.hpp spirv_reflect.cpp
        spirv_cross_util.hpp spirv_cross_util.cpp)
fips_end_lib()
target_compile_definitions(SPIRV-Cross PRIVATE SPIRV_CROSS_EXCEPTIONS_TO_ASSERTIONS)
