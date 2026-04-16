#ADD_DEFINITIONS(-DGTEST_HAS_PTHREAD=1)
set(_test_collision_libs
	LinearMath
	gtest
)
if (NOT WIN32)
	FIND_PACKAGE(Threads)
	list(APPEND _test_collision_libs ${CMAKE_THREAD_LIBS_INIT})
endif()

	ADD_EXECUTABLE(Test_Collision
		main.cpp
		../../src/BulletCollision/NarrowPhaseCollision/btVoronoiSimplexSolver.cpp
		../../src/BulletCollision/NarrowPhaseCollision/btVoronoiSimplexSolver.h
		../../src/BulletCollision/CollisionShapes/btSphereShape.cpp
		../../src/BulletCollision/CollisionShapes/btMultiSphereShape.cpp
		../../src/BulletCollision/CollisionShapes/btPolyhedralConvexShape.cpp
		../../src/BulletCollision/CollisionShapes/btConcaveShape.cpp
		../../src/BulletCollision/CollisionShapes/btConvexShape.cpp
		../../src/BulletCollision/CollisionShapes/btConvexInternalShape.cpp
		../../src/BulletCollision/CollisionShapes/btCollisionShape.cpp
		../../src/BulletCollision/CollisionShapes/btConvexPolyhedron.cpp
		../../src/BulletCollision/CollisionShapes/btHeightfieldTerrainShape.cpp
		../../src/BulletCollision/CollisionShapes/btTriangleCallback.cpp
	)

target_compile_definitions(Test_Collision PRIVATE _VARIADIC_MAX=10)
target_include_directories(Test_Collision
	PRIVATE
		${CMAKE_CURRENT_SOURCE_DIR}
		${PROJECT_SOURCE_DIR}/src
)
target_link_libraries(Test_Collision
	PRIVATE
		${_test_collision_libs}
)

ADD_TEST(Test_Collision_PASS Test_Collision)

IF (INTERNAL_ADD_POSTFIX_EXECUTABLE_NAMES)
			SET_TARGET_PROPERTIES(Test_Collision PROPERTIES  DEBUG_POSTFIX "_Debug")
			SET_TARGET_PROPERTIES(Test_Collision PROPERTIES  MINSIZEREL_POSTFIX "_MinsizeRel")
			SET_TARGET_PROPERTIES(Test_Collision PROPERTIES  RELWITHDEBINFO_POSTFIX "_RelWithDebugInfo")
ENDIF(INTERNAL_ADD_POSTFIX_EXECUTABLE_NAMES)
