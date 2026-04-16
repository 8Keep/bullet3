
set(_pybullet_include_dirs
	${BULLET_PHYSICS_SOURCE_DIR}/src
	${BULLET_PHYSICS_SOURCE_DIR}/examples
	${BULLET_PHYSICS_SOURCE_DIR}/examples/ThirdPartyLibs
	${BULLET_PHYSICS_SOURCE_DIR}/examples/ThirdPartyLibs/enet/include
	${BULLET_PHYSICS_SOURCE_DIR}/examples/ThirdPartyLibs/clsocket/src
	${PYTHON_INCLUDE_DIRS}
)
set(_pybullet_compile_defs
	STATIC_LINK_SPD_PLUGIN
)
set(_pybullet_link_libs
	BulletRoboticsGUI
	BulletExampleBrowserLib
	BulletRobotics
	BulletFileLoader
	BulletWorldImporter
	BulletSoftBody
	BulletDynamics
	BulletCollision
	BulletInverseDynamicsUtils
	BulletInverseDynamics
	LinearMath
	OpenGLWindow
	gwen
	BussIK
	Bullet3Common
)
IF(BUILD_PYBULLET_NUMPY)
	list(APPEND _pybullet_include_dirs ${PYTHON_NUMPY_INCLUDE_DIR})
ENDIF()

IF(ENABLE_VHACD)
	list(APPEND _pybullet_compile_defs BT_ENABLE_VHACD)
	list(APPEND _pybullet_include_dirs
		../../Extras/VHACD/inc
		../../Extras/VHACD/public
	)
ENDIF(ENABLE_VHACD)
	
SET(pybullet_SRCS
	pybullet.c
)

IF(BUILD_CLSOCKET)
 list(APPEND _pybullet_compile_defs BT_ENABLE_CLSOCKET)
ENDIF()

IF(WIN32)
	list(APPEND _pybullet_link_libs ${OPENGL_gl_LIBRARY} ${OPENGL_glu_LIBRARY})
	IF(BUILD_ENET)
		list(APPEND _pybullet_compile_defs WIN32 BT_ENABLE_ENET)
	ENDIF()
	IF(BUILD_CLSOCKET)
		list(APPEND _pybullet_compile_defs WIN32)
	ENDIF()

ELSE(WIN32)
	IF(BUILD_ENET)
		list(APPEND _pybullet_compile_defs HAS_SOCKLEN_T BT_ENABLE_ENET)
	ENDIF()

	IF(BUILD_CLSOCKET)	
		list(APPEND _pybullet_compile_defs ${OSDEF})
	ENDIF()
ENDIF(WIN32)



ADD_LIBRARY(pybullet SHARED ${pybullet_SRCS})

SET_TARGET_PROPERTIES(pybullet PROPERTIES PREFIX "")
SET_TARGET_PROPERTIES(pybullet PROPERTIES POSTFIX "")

SET_TARGET_PROPERTIES(pybullet PROPERTIES VERSION ${BULLET_VERSION})
SET_TARGET_PROPERTIES(pybullet PROPERTIES SOVERSION ${BULLET_VERSION})
SET_TARGET_PROPERTIES(pybullet PROPERTIES DEBUG_POSTFIX "_d")
target_include_directories(pybullet PRIVATE ${_pybullet_include_dirs})
target_compile_definitions(pybullet PRIVATE ${_pybullet_compile_defs})
target_link_libraries(pybullet PRIVATE ${_pybullet_link_libs})


IF(WIN32)
	IF(BUILD_ENET OR BUILD_CLSOCKET)
		TARGET_LINK_LIBRARIES(pybullet PRIVATE ws2_32 )
	ENDIF()

	SET_TARGET_PROPERTIES(pybullet PROPERTIES SUFFIX ".pyd" )
ENDIF(WIN32)

IF (APPLE)
  SET_TARGET_PROPERTIES(pybullet PROPERTIES SUFFIX ".so" )
ENDIF()


IF (WIN32)
  TARGET_LINK_LIBRARIES(pybullet PRIVATE ${PYTHON_LIBRARIES})
ELSEIF (APPLE)
  SET_TARGET_PROPERTIES(pybullet PROPERTIES LINK_FLAGS "-undefined dynamic_lookup")
ENDIF ()
# else Linux: dont link

IF(WIN32)
  SET(PYTHON_SITE_PACKAGES Lib/site-packages CACHE PATH "Python install path")
ELSE()
  SET(PYTHON_SITE_PACKAGES lib/python${PYTHON_VERSION_MAJOR}.${PYTHON_VERSION_MINOR}/site-packages CACHE PATH "Python install path")
ENDIF()

INSTALL(TARGETS pybullet DESTINATION ${PYTHON_SITE_PACKAGES})
