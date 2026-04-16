
SET(includes
  .
	${BULLET_PHYSICS_SOURCE_DIR}/examples/ThirdPartyLibs
	${BULLET_PHYSICS_SOURCE_DIR}/src
)

ADD_EXECUTABLE(App_obj2sdf 
		obj2sdf.cpp
		../../examples/Utils/b3ResourcePath.cpp
		../../examples/Utils/b3ResourcePath.h
		../../examples/ThirdPartyLibs/Wavefront/tiny_obj_loader.cpp
)
target_include_directories(App_obj2sdf PRIVATE ${includes})
target_link_libraries(App_obj2sdf PRIVATE Bullet3Common)
