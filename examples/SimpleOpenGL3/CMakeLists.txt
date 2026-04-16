
SET(AppSimpleOpenGL3_SRCS
	main.cpp
	${BULLET_PHYSICS_SOURCE_DIR}/build3/bullet.rc
)

set(_simple_opengl3_include_dirs
	${BULLET_PHYSICS_SOURCE_DIR}/src
	${BULLET_PHYSICS_SOURCE_DIR}/btgui
	${BULLET_PHYSICS_SOURCE_DIR}/examples
	${BULLET_PHYSICS_SOURCE_DIR}/examples/ThirdPartyLibs/Glew
)
set(_simple_opengl3_compile_defs)
set(_simple_opengl3_link_libs
	gwen
	OpenGLWindow
	Bullet3Common
	${OPENGL_gl_LIBRARY}
	${OPENGL_glu_LIBRARY}
)

IF (WIN32)
	SET(AppSimpleOpenGL3_SRCS ${AppSimpleOpenGL3_SRCS}  ${AppSimpleOpenGL3_Common_SRCS})
	list(APPEND _simple_opengl3_include_dirs
		${BULLET_PHYSICS_SOURCE_DIR}/btgui/OpenGLWindow/GlewWindows
	)
	list(APPEND _simple_opengl3_compile_defs GLEW_STATIC)
ELSE(WIN32)
	IF(APPLE)
		find_library(COCOA NAMES Cocoa)
		MESSAGE(${COCOA})
		list(APPEND _simple_opengl3_link_libs ${COCOA})

	ELSE(APPLE)
		list(APPEND _simple_opengl3_include_dirs
			${BULLET_PHYSICS_SOURCE_DIR}/btgui/OpenGLWindow/GlewWindows
		)
		list(APPEND _simple_opengl3_compile_defs
			GLEW_INIT_OPENGL11_FUNCTIONS=1
			GLEW_STATIC
			GLEW_DYNAMIC_LOAD_ALL_GLX_FUNCTIONS=1
		)

		FIND_PACKAGE(Threads)
		list(APPEND _simple_opengl3_link_libs
			X11
			${CMAKE_THREAD_LIBS_INIT}
			${DL}
			Xext
		)
	ENDIF(APPLE)
ENDIF(WIN32)


ADD_EXECUTABLE(AppSimpleOpenGL3
		${AppSimpleOpenGL3_SRCS}
)
target_include_directories(AppSimpleOpenGL3 PRIVATE ${_simple_opengl3_include_dirs})
target_compile_definitions(AppSimpleOpenGL3 PRIVATE ${_simple_opengl3_compile_defs})
target_link_libraries(AppSimpleOpenGL3 PRIVATE ${_simple_opengl3_link_libs})


IF (INTERNAL_ADD_POSTFIX_EXECUTABLE_NAMES)
			SET_TARGET_PROPERTIES(AppSimpleOpenGL3 PROPERTIES  DEBUG_POSTFIX "_Debug")
			SET_TARGET_PROPERTIES(AppSimpleOpenGL3 PROPERTIES  MINSIZEREL_POSTFIX "_MinsizeRel")
			SET_TARGET_PROPERTIES(AppSimpleOpenGL3 PROPERTIES  RELWITHDEBINFO_POSTFIX "_RelWithDebugInfo")
ENDIF(INTERNAL_ADD_POSTFIX_EXECUTABLE_NAMES)
