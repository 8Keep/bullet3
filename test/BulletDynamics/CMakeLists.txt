add_subdirectory(pendulum)

set(_test_bullet_dynamics_libs
	BulletDynamics
	gtest
)
if (NOT WIN32)
	FIND_PACKAGE(Threads)
	list(APPEND _test_bullet_dynamics_libs ${CMAKE_THREAD_LIBS_INIT})
endif()

ADD_EXECUTABLE(Test_btKinematicCharacterController test_btKinematicCharacterController.cpp)
target_compile_definitions(Test_btKinematicCharacterController
	PRIVATE
		USE_GTEST
		_VARIADIC_MAX=10
)
target_include_directories(Test_btKinematicCharacterController
	PRIVATE
		${PROJECT_SOURCE_DIR}/src
		${PROJECT_SOURCE_DIR}/test/gtest-1.7.0/include
)
target_link_libraries(Test_btKinematicCharacterController
	PRIVATE
		${_test_bullet_dynamics_libs}
)

ADD_TEST(Test_btKinematicCharacterController_PASS Test_btKinematicCharacterController)

IF (INTERNAL_ADD_POSTFIX_EXECUTABLE_NAMES)
			SET_TARGET_PROPERTIES(Test_btKinematicCharacterController PROPERTIES  DEBUG_POSTFIX "_Debug")
			SET_TARGET_PROPERTIES(Test_btKinematicCharacterController PROPERTIES  MINSIZEREL_POSTFIX "_MinsizeRel")
			SET_TARGET_PROPERTIES(Test_btKinematicCharacterController PROPERTIES  RELWITHDEBINFO_POSTFIX "_RelWithDebugInfo")
ENDIF(INTERNAL_ADD_POSTFIX_EXECUTABLE_NAMES)
