SET(RobotSimulator_SRCS
		TwoJointMain.cpp
		../../examples/SharedMemory/plugins/collisionFilterPlugin/collisionFilterPlugin.cpp
		../../examples/SharedMemory/plugins/pdControlPlugin/pdControlPlugin.cpp
		../../examples/SharedMemory/plugins/pdControlPlugin/pdControlPlugin.h
		../../examples/SharedMemory/b3RobotSimulatorClientAPI_NoDirect.cpp
		../../examples/SharedMemory/b3RobotSimulatorClientAPI_NoDirect.h
		../../examples/SharedMemory/IKTrajectoryHelper.cpp
		../../examples/SharedMemory/IKTrajectoryHelper.h
		../../examples/ExampleBrowser/InProcessExampleBrowser.cpp
		../../examples/SharedMemory/plugins/tinyRendererPlugin/tinyRendererPlugin.cpp
		../../examples/SharedMemory/plugins/tinyRendererPlugin/TinyRendererVisualShapeConverter.cpp
		../../examples/TinyRenderer/geometry.cpp
		../../examples/TinyRenderer/model.cpp
		../../examples/TinyRenderer/tgaimage.cpp
		../../examples/TinyRenderer/our_gl.cpp
		../../examples/TinyRenderer/TinyRenderer.cpp
		../../examples/SharedMemory/InProcessMemory.cpp
		../../examples/SharedMemory/PhysicsClient.cpp
		../../examples/SharedMemory/PhysicsClient.h
		../../examples/SharedMemory/PhysicsServer.cpp
		../../examples/SharedMemory/PhysicsServer.h
		../SharedMemory/GraphicsServerExample.cpp
		../SharedMemory/GraphicsClientExample.cpp
		../SharedMemory/RemoteGUIHelper.cpp
		../SharedMemory/RemoteGUIHelperTCP.cpp
		../SharedMemory/GraphicsServerExample.h
		../SharedMemory/GraphicsClientExample.h
		../SharedMemory/RemoteGUIHelper.h
		../SharedMemory/GraphicsSharedMemoryCommands.h
		../SharedMemory/GraphicsSharedMemoryPublic.h
		../../examples/SharedMemory/PhysicsServerExample.cpp
		../../examples/SharedMemory/PhysicsServerExampleBullet2.cpp
		../../examples/SharedMemory/SharedMemoryInProcessPhysicsC_API.cpp
		../../examples/SharedMemory/PhysicsServerSharedMemory.cpp
		../../examples/SharedMemory/PhysicsServerSharedMemory.h
		../../examples/SharedMemory/PhysicsDirect.cpp
		../../examples/SharedMemory/PhysicsDirect.h
		../../examples/SharedMemory/PhysicsDirectC_API.cpp
		../../examples/SharedMemory/PhysicsDirectC_API.h
		../../examples/SharedMemory/PhysicsServerCommandProcessor.cpp
		../../examples/SharedMemory/PhysicsServerCommandProcessor.h
		../../examples/SharedMemory/b3PluginManager.cpp
		
		../../examples/SharedMemory/PhysicsClientSharedMemory.cpp
		../../examples/SharedMemory/PhysicsClientSharedMemory.h
		../../examples/SharedMemory/PhysicsClientSharedMemory_C_API.cpp
		../../examples/SharedMemory/PhysicsClientSharedMemory_C_API.h
		../../examples/SharedMemory/PhysicsClientC_API.cpp
		../../examples/SharedMemory/PhysicsClientC_API.h
		../../examples/SharedMemory/Win32SharedMemory.cpp
		../../examples/SharedMemory/Win32SharedMemory.h
		../../examples/SharedMemory/PosixSharedMemory.cpp
		../../examples/SharedMemory/PosixSharedMemory.h
		../../examples/Utils/b3ResourcePath.cpp
		../../examples/Utils/b3ResourcePath.h
		../../examples/Utils/RobotLoggingUtil.cpp
		../../examples/Utils/RobotLoggingUtil.h
		../../examples/ThirdPartyLibs/tinyxml2/tinyxml2.cpp
		../../examples/ThirdPartyLibs/Wavefront/tiny_obj_loader.cpp
		../../examples/ThirdPartyLibs/Wavefront/tiny_obj_loader.h
		../../examples/ThirdPartyLibs/stb_image/stb_image.cpp
		../../examples/Importers/ImportColladaDemo/LoadMeshFromCollada.cpp
		../../examples/Importers/ImportObjDemo/LoadMeshFromObj.cpp
		../../examples/Importers/ImportObjDemo/Wavefront2GLInstanceGraphicsShape.cpp
		../../examples/Importers/ImportMJCFDemo/BulletMJCFImporter.cpp
		../../examples/Importers/ImportURDFDemo/BulletUrdfImporter.cpp
		../../examples/Importers/ImportURDFDemo/MyMultiBodyCreator.cpp
		../../examples/Importers/ImportURDFDemo/URDF2Bullet.cpp
		../../examples/Importers/ImportURDFDemo/UrdfParser.cpp
		../../examples/Importers/ImportURDFDemo/urdfStringSplit.cpp
		../../examples/Importers/ImportMeshUtility/b3ImportMeshUtility.cpp 
		../../examples/MultiThreading/b3PosixThreadSupport.cpp
		../../examples/MultiThreading/b3Win32ThreadSupport.cpp
		../../examples/MultiThreading/b3ThreadSupportInterface.cpp
		
)

set(_two_joint_include_dirs
	${BULLET_PHYSICS_SOURCE_DIR}/src
	${BULLET_PHYSICS_SOURCE_DIR}/examples
	${BULLET_PHYSICS_SOURCE_DIR}/examples/ThirdPartyLibs
	${BULLET_PHYSICS_SOURCE_DIR}/examples/ThirdPartyLibs/enet/include
	${BULLET_PHYSICS_SOURCE_DIR}/examples/ThirdPartyLibs/clsocket/src
)

set(_two_joint_compile_defs)
set(_two_joint_link_libs
	BulletExampleBrowserLib
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

IF(BUILD_CLSOCKET)
 list(APPEND _two_joint_compile_defs BT_ENABLE_CLSOCKET)
ENDIF(BUILD_CLSOCKET)

IF(WIN32)
	list(APPEND _two_joint_link_libs
		${OPENGL_gl_LIBRARY}
		${OPENGL_glu_LIBRARY}
	)
	IF(BUILD_ENET)
		list(APPEND _two_joint_compile_defs WIN32 BT_ENABLE_ENET)
	ENDIF(BUILD_ENET)
	IF(BUILD_CLSOCKET)
		list(APPEND _two_joint_compile_defs WIN32)
	ENDIF(BUILD_CLSOCKET)

ELSE(WIN32)
	IF(BUILD_ENET)
		list(APPEND _two_joint_compile_defs HAS_SOCKLEN_T BT_ENABLE_ENET)
	ENDIF(BUILD_ENET)

	IF(BUILD_CLSOCKET)	
		list(APPEND _two_joint_compile_defs ${OSDEF})
	ENDIF(BUILD_CLSOCKET)
ENDIF(WIN32)


IF(BUILD_ENET)
	set(RobotSimulator_SRCS ${RobotSimulator_SRCS} 
		../../examples/SharedMemory/PhysicsClientUDP.cpp
		../../examples/SharedMemory/PhysicsClientUDP_C_API.cpp
		../../examples/SharedMemory/PhysicsClientUDP.h		
		../../examples/SharedMemory/PhysicsClientUDP_C_API.h
		../../examples/ThirdPartyLibs/enet/win32.c
		../../examples/ThirdPartyLibs/enet/unix.c
		../../examples/ThirdPartyLibs/enet/callbacks.c
		../../examples/ThirdPartyLibs/enet/compress.c
		../../examples/ThirdPartyLibs/enet/host.c
		../../examples/ThirdPartyLibs/enet/list.c
		../../examples/ThirdPartyLibs/enet/packet.c
		../../examples/ThirdPartyLibs/enet/peer.c
		../../examples/ThirdPartyLibs/enet/protocol.c
	)
ENDIF(BUILD_ENET)

IF(BUILD_CLSOCKET)
	set(RobotSimulator_SRCS ${RobotSimulator_SRCS}
		 ../../examples/SharedMemory/PhysicsClientTCP.cpp
                 ../../examples/SharedMemory/PhysicsClientTCP.h
                 ../../examples/SharedMemory/PhysicsClientTCP_C_API.cpp
                 ../../examples/SharedMemory/PhysicsClientTCP_C_API.h
		 ../../examples/ThirdPartyLibs/clsocket/src/SimpleSocket.cpp
		 ../../examples/ThirdPartyLibs/clsocket/src/ActiveSocket.cpp
		 ../../examples/ThirdPartyLibs/clsocket/src/PassiveSocket.cpp
	)
ENDIF()


#some code to support OpenGL and Glew cross platform
IF (WIN32)
	list(APPEND _two_joint_include_dirs
		${BULLET_PHYSICS_SOURCE_DIR}/btgui/OpenGLWindow/GlewWindows
	)
	list(APPEND _two_joint_compile_defs GLEW_STATIC)
ELSE(WIN32)
	IF(APPLE)
		find_library(COCOA NAMES Cocoa)
		MESSAGE(${COCOA})
		list(APPEND _two_joint_link_libs ${COCOA} ${OPENGL_gl_LIBRARY} ${OPENGL_glu_LIBRARY})

	ELSE(APPLE)
		list(APPEND _two_joint_include_dirs
			${BULLET_PHYSICS_SOURCE_DIR}/btgui/OpenGLWindow/GlewWindows
		)
		list(APPEND _two_joint_compile_defs
			GLEW_INIT_OPENGL11_FUNCTIONS=1
			GLEW_STATIC
			GLEW_DYNAMIC_LOAD_ALL_GLX_FUNCTIONS=1
		)
		list(APPEND _two_joint_link_libs pthread ${DL})
	ENDIF(APPLE)
ENDIF(WIN32)


ADD_EXECUTABLE(App_TwoJoint  ${RobotSimulator_SRCS})

SET_TARGET_PROPERTIES(App_TwoJoint PROPERTIES VERSION ${BULLET_VERSION})
SET_TARGET_PROPERTIES(App_TwoJoint PROPERTIES DEBUG_POSTFIX "_d")
target_include_directories(App_TwoJoint PRIVATE ${_two_joint_include_dirs})
target_compile_definitions(App_TwoJoint PRIVATE ${_two_joint_compile_defs})
target_link_libraries(App_TwoJoint PRIVATE ${_two_joint_link_libs})


IF(WIN32)
	IF(BUILD_ENET OR BUILD_CLSOCKET)
		TARGET_LINK_LIBRARIES(App_TwoJoint PRIVATE ws2_32 )
	ENDIF(BUILD_ENET OR BUILD_CLSOCKET)
ENDIF(WIN32)

