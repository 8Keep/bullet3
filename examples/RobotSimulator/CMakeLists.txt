set(_robot_simulator_common_include_dirs
	${BULLET_PHYSICS_SOURCE_DIR}/src
	${BULLET_PHYSICS_SOURCE_DIR}/examples
	${BULLET_PHYSICS_SOURCE_DIR}/examples/SharedMemory
	${BULLET_PHYSICS_SOURCE_DIR}/examples/ThirdPartyLibs
	${BULLET_PHYSICS_SOURCE_DIR}/examples/ThirdPartyLibs/enet/include
	${BULLET_PHYSICS_SOURCE_DIR}/examples/ThirdPartyLibs/clsocket/src
)

set(_robot_simulator_common_compile_defs)
set(_robot_simulator_gui_compile_defs)
set(_robot_simulator_gui_link_libs)
set(_robot_simulator_nogui_link_libs)
set(_robot_simulator_hello_link_libs)

SET(RobotSimulator_SRCS
	RobotSimulatorMain.cpp
	b3RobotSimulatorClientAPI.cpp
	b3RobotSimulatorClientAPI.h
	MinitaurSetup.cpp
	MinitaurSetup.h
	../../examples/ExampleBrowser/InProcessExampleBrowser.cpp
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
)

IF(BUILD_CLSOCKET)
	list(APPEND _robot_simulator_common_compile_defs BT_ENABLE_CLSOCKET ${OSDEF})
ENDIF(BUILD_CLSOCKET)

# some code to support OpenGL and Glew cross platform
IF (WIN32)
	list(APPEND _robot_simulator_common_include_dirs
		${BULLET_PHYSICS_SOURCE_DIR}/btgui/OpenGLWindow/GlewWindows
	)
	list(APPEND _robot_simulator_gui_compile_defs GLEW_STATIC)
	list(APPEND _robot_simulator_gui_link_libs ${OPENGL_gl_LIBRARY} ${OPENGL_glu_LIBRARY})
ELSE(WIN32)
	IF(APPLE)
		find_library(COCOA NAMES Cocoa)
		MESSAGE(${COCOA})
		list(APPEND _robot_simulator_gui_link_libs ${COCOA} ${OPENGL_gl_LIBRARY} ${OPENGL_glu_LIBRARY})
	ELSE(APPLE)
		list(APPEND _robot_simulator_common_include_dirs
			${BULLET_PHYSICS_SOURCE_DIR}/btgui/OpenGLWindow/GlewWindows
		)
		list(APPEND _robot_simulator_gui_compile_defs
			GLEW_INIT_OPENGL11_FUNCTIONS=1
			GLEW_STATIC
			GLEW_DYNAMIC_LOAD_ALL_GLX_FUNCTIONS=1
		)
		FIND_PACKAGE(Threads)
		list(APPEND _robot_simulator_gui_link_libs ${CMAKE_THREAD_LIBS_INIT} ${DL})
		list(APPEND _robot_simulator_nogui_link_libs ${CMAKE_THREAD_LIBS_INIT} ${DL})
		list(APPEND _robot_simulator_hello_link_libs ${CMAKE_THREAD_LIBS_INIT} ${DL})
	ENDIF(APPLE)
ENDIF(WIN32)

ADD_EXECUTABLE(App_RobotSimulator ${RobotSimulator_SRCS})
SET_TARGET_PROPERTIES(App_RobotSimulator PROPERTIES VERSION ${BULLET_VERSION})
SET_TARGET_PROPERTIES(App_RobotSimulator PROPERTIES DEBUG_POSTFIX "_d")
target_include_directories(App_RobotSimulator PRIVATE ${_robot_simulator_common_include_dirs})
target_compile_definitions(App_RobotSimulator
	PRIVATE
		B3_USE_ROBOTSIM_GUI
		${_robot_simulator_common_compile_defs}
		${_robot_simulator_gui_compile_defs}
)
TARGET_LINK_LIBRARIES(App_RobotSimulator PRIVATE
	BulletRobotics
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
	Bullet3Common
	${_robot_simulator_gui_link_libs}
)

IF(WIN32)
	IF(BUILD_ENET OR BUILD_CLSOCKET)
		TARGET_LINK_LIBRARIES(App_RobotSimulator PRIVATE ws2_32 Winmm)
	ENDIF(BUILD_ENET OR BUILD_CLSOCKET)
ENDIF(WIN32)

SET(RobotSimulator_NoGUI_SRCS
	RobotSimulatorMain.cpp
	MinitaurSetup.cpp
	MinitaurSetup.h
)

ADD_EXECUTABLE(App_RobotSimulator_NoGUI ${RobotSimulator_NoGUI_SRCS})
SET_TARGET_PROPERTIES(App_RobotSimulator_NoGUI PROPERTIES VERSION ${BULLET_VERSION})
SET_TARGET_PROPERTIES(App_RobotSimulator_NoGUI PROPERTIES DEBUG_POSTFIX "_d")
target_include_directories(App_RobotSimulator_NoGUI PRIVATE ${_robot_simulator_common_include_dirs})
target_compile_definitions(App_RobotSimulator_NoGUI PRIVATE ${_robot_simulator_common_compile_defs})
TARGET_LINK_LIBRARIES(App_RobotSimulator_NoGUI PRIVATE
	BulletRobotics
	BulletFileLoader
	BulletWorldImporter
	BulletSoftBody
	BulletDynamics
	BulletCollision
	BulletInverseDynamicsUtils
	BulletInverseDynamics
	LinearMath
	Bullet3Common
	${_robot_simulator_nogui_link_libs}
)

IF(WIN32)
	IF(BUILD_ENET OR BUILD_CLSOCKET)
		TARGET_LINK_LIBRARIES(App_RobotSimulator_NoGUI PRIVATE ws2_32 Winmm)
	ENDIF(BUILD_ENET OR BUILD_CLSOCKET)
ENDIF(WIN32)

SET(App_HelloBulletRobotics_SRCS
	HelloBulletRobotics.cpp
)

ADD_EXECUTABLE(App_HelloBulletRobotics ${App_HelloBulletRobotics_SRCS})
SET_TARGET_PROPERTIES(App_HelloBulletRobotics PROPERTIES VERSION ${BULLET_VERSION})
SET_TARGET_PROPERTIES(App_HelloBulletRobotics PROPERTIES DEBUG_POSTFIX "_d")
target_include_directories(App_HelloBulletRobotics PRIVATE ${_robot_simulator_common_include_dirs})
target_compile_definitions(App_HelloBulletRobotics PRIVATE ${_robot_simulator_common_compile_defs})
TARGET_LINK_LIBRARIES(App_HelloBulletRobotics PRIVATE
	BulletRobotics
	BulletFileLoader
	BulletWorldImporter
	BulletSoftBody
	BulletDynamics
	BulletCollision
	BulletInverseDynamicsUtils
	BulletInverseDynamics
	LinearMath
	Bullet3Common
	${_robot_simulator_hello_link_libs}
)

IF(WIN32)
	IF(BUILD_ENET OR BUILD_CLSOCKET)
		TARGET_LINK_LIBRARIES(App_HelloBulletRobotics PRIVATE ws2_32 Winmm)
	ENDIF(BUILD_ENET OR BUILD_CLSOCKET)
ENDIF(WIN32)
