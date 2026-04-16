# App_BasicExample is a minimal sample creating, stepping and deleting a Bullet dynamics world

IF (WIN32)
	ADD_EXECUTABLE(App_BasicExample
		BasicExample.cpp
		main.cpp
		${BULLET_PHYSICS_SOURCE_DIR}/build3/bullet.rc
	)
ELSE()
	ADD_EXECUTABLE(App_BasicExample
		BasicExample.cpp
		main.cpp
	)
ENDIF()

target_link_libraries(App_BasicExample
	PRIVATE
		BulletDynamics
)

IF (INTERNAL_ADD_POSTFIX_EXECUTABLE_NAMES)
	SET_TARGET_PROPERTIES(App_BasicExample PROPERTIES DEBUG_POSTFIX "_Debug")
	SET_TARGET_PROPERTIES(App_BasicExample PROPERTIES MINSIZEREL_POSTFIX "_MinsizeRel")
	SET_TARGET_PROPERTIES(App_BasicExample PROPERTIES RELWITHDEBINFO_POSTFIX "_RelWithDebugInfo")
ENDIF(INTERNAL_ADD_POSTFIX_EXECUTABLE_NAMES)

#################
# Standalone BasicExampleGui using OpenGL (but not the example browser)

SET(AppBasicExampleGui_SRCS
	BasicExample.cpp
	${BULLET_PHYSICS_SOURCE_DIR}/build3/bullet.rc
	../StandaloneMain/main_opengl_single_example.cpp
	../ExampleBrowser/OpenGLGuiHelper.cpp
	../ExampleBrowser/GL_ShapeDrawer.cpp
	../ExampleBrowser/CollisionShape2TriangleMesh.cpp
	../Utils/b3Clock.cpp
)

set(_app_basic_example_gui_include_dirs
	${BULLET_PHYSICS_SOURCE_DIR}/src
	${BULLET_PHYSICS_SOURCE_DIR}/btgui
	${BULLET_PHYSICS_SOURCE_DIR}/examples
	${BULLET_PHYSICS_SOURCE_DIR}/examples/ThirdPartyLibs/glad
)

set(_app_basic_example_gui_compile_defs
	B3_USE_STANDALONE_EXAMPLE
)

set(_app_basic_example_gui_link_libs
	BulletDynamics
	OpenGLWindow
	Bullet3Common
)

# some code to support OpenGL and Glew cross platform
IF (WIN32)
	list(APPEND _app_basic_example_gui_include_dirs
		${BULLET_PHYSICS_SOURCE_DIR}/btgui/OpenGLWindow/GlewWindows
	)
	list(APPEND _app_basic_example_gui_compile_defs GLEW_STATIC)
	list(APPEND _app_basic_example_gui_link_libs
		${OPENGL_gl_LIBRARY}
		${OPENGL_glu_LIBRARY}
	)
ELSE(WIN32)
	IF(APPLE)
		find_library(COCOA NAMES Cocoa)
		MESSAGE(${COCOA})
		list(APPEND _app_basic_example_gui_link_libs
			${COCOA}
			${OPENGL_gl_LIBRARY}
			${OPENGL_glu_LIBRARY}
		)

	ELSE(APPLE)
		list(APPEND _app_basic_example_gui_include_dirs
			${BULLET_PHYSICS_SOURCE_DIR}/btgui/OpenGLWindow/GlewWindows
		)
		FIND_PACKAGE(Threads)
		list(APPEND _app_basic_example_gui_compile_defs
			GLEW_INIT_OPENGL11_FUNCTIONS=1
			GLEW_STATIC
			GLEW_DYNAMIC_LOAD_ALL_GLX_FUNCTIONS=1
		)
		list(APPEND _app_basic_example_gui_link_libs
			${CMAKE_THREAD_LIBS_INIT}
			${DL}
		)
	ENDIF(APPLE)
ENDIF(WIN32)

ADD_EXECUTABLE(AppBasicExampleGui
	${AppBasicExampleGui_SRCS}
)

target_compile_definitions(AppBasicExampleGui PRIVATE ${_app_basic_example_gui_compile_defs})
target_include_directories(AppBasicExampleGui PRIVATE ${_app_basic_example_gui_include_dirs})
target_link_libraries(AppBasicExampleGui PRIVATE ${_app_basic_example_gui_link_libs})

IF (INTERNAL_ADD_POSTFIX_EXECUTABLE_NAMES)
	SET_TARGET_PROPERTIES(AppBasicExampleGui PROPERTIES DEBUG_POSTFIX "_Debug")
	SET_TARGET_PROPERTIES(AppBasicExampleGui PROPERTIES MINSIZEREL_POSTFIX "_MinsizeRel")
	SET_TARGET_PROPERTIES(AppBasicExampleGui PROPERTIES RELWITHDEBINFO_POSTFIX "_RelWithDebugInfo")
ENDIF(INTERNAL_ADD_POSTFIX_EXECUTABLE_NAMES)
