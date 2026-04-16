IF(BUILD_BULLET3)
	#SUBDIRS( TestBullet3OpenCL )
	add_subdirectory(InverseDynamics)
	add_subdirectory(SharedMemory)
ENDIF(BUILD_BULLET3)

add_subdirectory(gtest-1.7.0)
add_subdirectory(collision)
add_subdirectory(BulletDynamics)
