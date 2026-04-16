
IF(NOT WIN32 AND NOT APPLE)
        list(APPEND _gwen_include_dirs
                                        ${BULLET_PHYSICS_SOURCE_DIR}/examples/ThirdPartyLibs/optionalX11
        )
        list(APPEND _gwen_compile_defs
		GLEW_STATIC
		GLEW_INIT_OPENGL11_FUNCTIONS=1
		GLEW_DYNAMIC_LOAD_ALL_GLX_FUNCTIONS=1
		DYNAMIC_LOAD_X11_FUNCTIONS=1
	)
ENDIF()

set(_gwen_include_dirs
	${BULLET_PHYSICS_SOURCE_DIR}/src
	${BULLET_PHYSICS_SOURCE_DIR}/examples/ThirdPartyLibs
	${BULLET_PHYSICS_SOURCE_DIR}/examples/ThirdPartyLibs/glad
	${_gwen_include_dirs}
)
set(_gwen_compile_defs
	GLEW_STATIC
	GWEN_COMPILE_STATIC
	_HAS_EXCEPTIONS=0
	${_gwen_compile_defs}
)

FILE(GLOB gwen_SRCS "*.cpp" "Controls/*.cpp" "Controls/Dialog/*.cpp" "Controls/Dialogs/*.cpp" "Controls/Layout/*.cpp" "Controls/Property/*.cpp" "Input/*.cpp" "Platforms/*.cpp" "Renderers/*.cpp" "Skins/*.cpp")
FILE(GLOB gwen_HDRS "*.h" "Controls/*.h" "Controls/Dialog/*.h" "Controls/Dialogs/*.h" "Controls/Layout/*.h" "Controls/Property/*.h" "Input/*.h" "Platforms/*.h" "Renderers/*.h" "Skins/*.h")

ADD_LIBRARY(gwen ${gwen_SRCS} ${gwen_HDRS})
target_include_directories(gwen PRIVATE ${_gwen_include_dirs})
target_compile_definitions(gwen PRIVATE ${_gwen_compile_defs})
IF(WIN32 OR APPLE)
  target_link_libraries(gwen PRIVATE ${OPENGL_gl_LIBRARY})
ENDIF()


INSTALL(TARGETS gwen
        RUNTIME DESTINATION bin
        LIBRARY DESTINATION lib${LIB_SUFFIX}
        ARCHIVE DESTINATION lib${LIB_SUFFIX})
