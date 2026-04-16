SET(gtest-1.7.0_SRCS
	src/gtest-all.cc
)

#ADD_DEFINITIONS(-DGTEST_HAS_PTHREAD=1)
add_compile_definitions(_VARIADIC_MAX=10)

FILE(GLOB_RECURSE gtest-1.7.0_HDRS "*.h")

ADD_LIBRARY(gtest ${gtest-1.7.0_SRCS} ${gtest-1.7.0_HDRS})
target_include_directories(gtest
	PUBLIC
		${CMAKE_CURRENT_SOURCE_DIR}
		${CMAKE_CURRENT_SOURCE_DIR}/include
)
