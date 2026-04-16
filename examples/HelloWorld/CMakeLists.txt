# HelloWorld is a minimal sample creating, stepping and deleting a Bullet dynamics world

IF (WIN32)
	ADD_EXECUTABLE(App_HelloWorld
		HelloWorld.cpp
		${BULLET_PHYSICS_SOURCE_DIR}/build3/bullet.rc
	)
ELSE()
	ADD_EXECUTABLE(App_HelloWorld
		HelloWorld.cpp
	)
ENDIF()

target_link_libraries(App_HelloWorld
	PRIVATE
		BulletDynamics
)



IF (INTERNAL_ADD_POSTFIX_EXECUTABLE_NAMES)
			SET_TARGET_PROPERTIES(App_HelloWorld PROPERTIES  DEBUG_POSTFIX "_Debug")
			SET_TARGET_PROPERTIES(App_HelloWorld PROPERTIES  MINSIZEREL_POSTFIX "_MinsizeRel")
			SET_TARGET_PROPERTIES(App_HelloWorld PROPERTIES  RELWITHDEBINFO_POSTFIX "_RelWithDebugInfo")
ENDIF(INTERNAL_ADD_POSTFIX_EXECUTABLE_NAMES)
