
FILE(GLOB_RECURSE lua-5.2.3_SRCS "src/*.c")

FILE(GLOB_RECURSE lua-5.2.3_HDRS "src/*.h")

ADD_LIBRARY(lua-5.2.3 ${lua-5.2.3_SRCS} ${lua-5.2.3_HDRS})
target_include_directories(lua-5.2.3 PRIVATE . src)
target_compile_definitions(lua-5.2.3 PRIVATE LUA_COMPAT_ALL)
