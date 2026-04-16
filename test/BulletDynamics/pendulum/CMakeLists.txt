set(_test_pendulum_libs
	BulletDynamics
	gtest
)
if (NOT WIN32)
	FIND_PACKAGE(Threads)
	list(APPEND _test_pendulum_libs ${CMAKE_THREAD_LIBS_INIT})
endif()

	ADD_EXECUTABLE(Test_BulletDynamics
		 ../../../examples/MultiBody/Pendulum.cpp
	)
target_compile_definitions(Test_BulletDynamics
	PRIVATE
		USE_GTEST
		_VARIADIC_MAX=10
)
target_include_directories(Test_BulletDynamics
	PRIVATE
		${CMAKE_CURRENT_SOURCE_DIR}
		${PROJECT_SOURCE_DIR}/src
		${PROJECT_SOURCE_DIR}/test/gtest-1.7.0/include
)
target_link_libraries(Test_BulletDynamics
	PRIVATE
		${_test_pendulum_libs}
)

ADD_TEST(Test_BulletDynamics_PASS Test_BulletDynamics)

IF (INTERNAL_ADD_POSTFIX_EXECUTABLE_NAMES)
			SET_TARGET_PROPERTIES(Test_BulletDynamics PROPERTIES  DEBUG_POSTFIX "_Debug")
			SET_TARGET_PROPERTIES(Test_BulletDynamics PROPERTIES  MINSIZEREL_POSTFIX "_MinsizeRel")
			SET_TARGET_PROPERTIES(Test_BulletDynamics PROPERTIES  RELWITHDEBINFO_POSTFIX "_RelWithDebugInfo")
ENDIF(INTERNAL_ADD_POSTFIX_EXECUTABLE_NAMES)
