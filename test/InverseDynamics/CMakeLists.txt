IF (BULLET_BUILD_RBDL_COMPARE_TEST)
    set(_test_bullet_rbdl_compare_include_dirs
        .
        ../../src
        ../gtest-1.7.0/include
        ../../examples/ThirdPartyLibs
        ${BULLET_RBDL_SOURCE_DIR}/include/
        ${BULLET_RBDL_SOURCE_DIR}/addons/urdfreader/
        ${BULLET_RBDL_BINARY_DIR}/include
    )
    set(_test_bullet_rbdl_compare_compile_defs
        _VARIADIC_MAX=10
        RBDL_USE_SIMPLE_MATH
    )
    if (MSVC)
        set(_test_bullet_rbdl_compare_link_libs
            BulletDynamics
            BulletCollision
            Bullet3Common
            LinearMath
            gtest
            rbdl_urdfreader-static
            rbdl-static
        )
    else()
        set(_test_bullet_rbdl_compare_link_libs
            BulletDynamics
            BulletCollision
            Bullet3Common
            LinearMath
            gtest
            rbdl_urdfreader
            rbdl
        )
    endif()
    if (NOT WIN32)
        FIND_PACKAGE(Threads)
        list(APPEND _test_bullet_rbdl_compare_link_libs ${CMAKE_THREAD_LIBS_INIT})
    endif()
    
    
    	ADD_EXECUTABLE(Test_DynamicsABA_bulletVSrbdl
    		test_dynABA_bulletVSrbdl.cpp
    		../../examples/Utils/b3ResourcePath.cpp
                    ../../examples/Importers/ImportURDFDemo/ConvertRigidBodies2MultiBody.h
            ../../examples/Importers/ImportURDFDemo/MyMultiBodyCreator.cpp
            ../../examples/Importers/ImportURDFDemo/MyMultiBodyCreator.h
            ../../examples/Importers/ImportURDFDemo/BulletUrdfImporter.cpp
            ../../examples/Importers/ImportURDFDemo/BulletUrdfImporter.h
            ../../examples/Importers/ImportURDFDemo/urdfStringSplit.cpp
            ../../examples/Importers/ImportURDFDemo/UrdfParser.cpp
            ../../examples/Importers/ImportURDFDemo/UrdfParser.h
            ../../examples/Importers/ImportURDFDemo/URDF2Bullet.cpp
            ../../examples/Importers/ImportURDFDemo/URDF2Bullet.h
            ../../examples/Importers/ImportMeshUtility/b3ImportMeshUtility.cpp
            ../../examples/ThirdPartyLibs/stb_image/stb_image.cpp
            ../../examples/Utils/b3Clock.cpp
            ../../Extras/Serialize/BulletWorldImporter/btBulletWorldImporter.cpp
    	../../Extras/Serialize/BulletWorldImporter/btWorldImporter.cpp
    	../../Extras/Serialize/BulletFileLoader/bChunk.cpp
    	../../Extras/Serialize/BulletFileLoader/bFile.cpp
    	../../Extras/Serialize/BulletFileLoader/bDNA.cpp
    	../../Extras/Serialize/BulletFileLoader/btBulletFile.cpp
            ../../examples/Importers/ImportURDFDemo/URDFImporterInterface.h
            ../../examples/Importers/ImportURDFDemo/URDFJointTypes.h
            ../../examples/Importers/ImportObjDemo/Wavefront2GLInstanceGraphicsShape.cpp
            ../../examples/Importers/ImportObjDemo/LoadMeshFromObj.cpp
            ../../examples/Importers/ImportSTLDemo/ImportSTLSetup.h
            ../../examples/Importers/ImportSTLDemo/LoadMeshFromSTL.h
            	../../examples/Importers/ImportColladaDemo/LoadMeshFromCollada.cpp
            	../../examples/Importers/ImportColladaDemo/ColladaGraphicsInstance.h
            	../../examples/ThirdPartyLibs/Wavefront/tiny_obj_loader.cpp
    		../../examples/ThirdPartyLibs/tinyxml2/tinyxml2.cpp
    		
    	)
    target_include_directories(Test_DynamicsABA_bulletVSrbdl PRIVATE ${_test_bullet_rbdl_compare_include_dirs})
    target_compile_definitions(Test_DynamicsABA_bulletVSrbdl PRIVATE ${_test_bullet_rbdl_compare_compile_defs})
    target_link_libraries(Test_DynamicsABA_bulletVSrbdl PRIVATE ${_test_bullet_rbdl_compare_link_libs})
    
    ADD_TEST(Test_DynamicsABA_bulletVSrbdl_PASS Test_DynamicsABA_bulletVSrbdl)
    
    IF (INTERNAL_ADD_POSTFIX_EXECUTABLE_NAMES)
    			SET_TARGET_PROPERTIES(Test_DynamicsABA_bulletVSrbdl PROPERTIES  DEBUG_POSTFIX "_Debug")
    			SET_TARGET_PROPERTIES(Test_DynamicsABA_bulletVSrbdl PROPERTIES  MINSIZEREL_POSTFIX "_MinsizeRel")
    			SET_TARGET_PROPERTIES(Test_DynamicsABA_bulletVSrbdl PROPERTIES  RELWITHDEBINFO_POSTFIX "_RelWithDebugInfo")
    ENDIF(INTERNAL_ADD_POSTFIX_EXECUTABLE_NAMES)

ENDIF (BULLET_BUILD_RBDL_COMPARE_TEST)

set(_test_bullet_inverse_dynamics_include_dirs
	.
	../../src
	../gtest-1.7.0/include
	../../Extras/InverseDynamics
)
set(_test_bullet_inverse_dynamics_compile_defs
	_VARIADIC_MAX=10
)
set(_test_bullet_inverse_dynamics_link_libs
	BulletInverseDynamicsUtils
	BulletInverseDynamics
	Bullet3Common
	LinearMath
	gtest
)
IF (NOT WIN32)
	FIND_PACKAGE(Threads)
	list(APPEND _test_bullet_inverse_dynamics_link_libs ${CMAKE_THREAD_LIBS_INIT})
ENDIF()


	ADD_EXECUTABLE(Test_BulletInverseDynamicsJacobian
		test_invdyn_jacobian.cpp
	)
target_include_directories(Test_BulletInverseDynamicsJacobian PRIVATE ${_test_bullet_inverse_dynamics_include_dirs})
target_compile_definitions(Test_BulletInverseDynamicsJacobian PRIVATE ${_test_bullet_inverse_dynamics_compile_defs})
target_link_libraries(Test_BulletInverseDynamicsJacobian PRIVATE ${_test_bullet_inverse_dynamics_link_libs})

ADD_TEST(Test_BulletInverseDynamicsJacobian_PASS Test_BulletInverseDynamicsJacobian)

IF (INTERNAL_ADD_POSTFIX_EXECUTABLE_NAMES)
			SET_TARGET_PROPERTIES(Test_BulletInverseDynamicsJacobian PROPERTIES  DEBUG_POSTFIX "_Debug")
			SET_TARGET_PROPERTIES(Test_BulletInverseDynamicsJacobian PROPERTIES  MINSIZEREL_POSTFIX "_MinsizeRel")
			SET_TARGET_PROPERTIES(Test_BulletInverseDynamicsJacobian PROPERTIES  RELWITHDEBINFO_POSTFIX "_RelWithDebugInfo")
ENDIF(INTERNAL_ADD_POSTFIX_EXECUTABLE_NAMES)

set(_test_bullet_inverse_forward_dynamics_include_dirs
        .
        ../../src
        ../gtest-1.7.0/include
        ../../Extras/InverseDynamics
        ../../examples/ThirdPartyLibs
)
set(_test_bullet_inverse_forward_dynamics_compile_defs
        _VARIADIC_MAX=10
)
set(_test_bullet_inverse_forward_dynamics_link_libs
        BulletDynamics
        BulletCollision
        BulletInverseDynamicsUtils
        BulletInverseDynamics
        Bullet3Common
        LinearMath
        gtest
)
IF (NOT WIN32)
	FIND_PACKAGE(Threads)
	list(APPEND _test_bullet_inverse_forward_dynamics_link_libs ${CMAKE_THREAD_LIBS_INIT})
ENDIF()


        ADD_EXECUTABLE(Test_BulletInverseForwardDynamics
                test_invdyn_bullet.cpp
		../../examples/Utils/b3ResourcePath.cpp
                ../../examples/Importers/ImportURDFDemo/ConvertRigidBodies2MultiBody.h
        	../../examples/Importers/ImportURDFDemo/MultiBodyCreationInterface.h
        ../../examples/Importers/ImportURDFDemo/MyMultiBodyCreator.cpp
        ../../examples/Importers/ImportURDFDemo/MyMultiBodyCreator.h
        ../../examples/Importers/ImportURDFDemo/BulletUrdfImporter.cpp
        ../../examples/Importers/ImportURDFDemo/BulletUrdfImporter.h
        ../../examples/Importers/ImportURDFDemo/urdfStringSplit.cpp
        ../../examples/Importers/ImportURDFDemo/UrdfParser.cpp
        ../../examples/Importers/ImportURDFDemo/UrdfParser.h
        ../../examples/Importers/ImportURDFDemo/URDF2Bullet.cpp
        ../../examples/Importers/ImportURDFDemo/URDF2Bullet.h
        ../../examples/Importers/ImportMeshUtility/b3ImportMeshUtility.cpp
        ../../examples/ThirdPartyLibs/stb_image/stb_image.cpp
        ../../examples/Utils/b3Clock.cpp
        ../../Extras/Serialize/BulletWorldImporter/btBulletWorldImporter.cpp
	../../Extras/Serialize/BulletWorldImporter/btWorldImporter.cpp
	../../Extras/Serialize/BulletFileLoader/bChunk.cpp
	../../Extras/Serialize/BulletFileLoader/bFile.cpp
	../../Extras/Serialize/BulletFileLoader/bDNA.cpp
	../../Extras/Serialize/BulletFileLoader/btBulletFile.cpp
        ../../examples/Importers/ImportURDFDemo/URDFImporterInterface.h
        ../../examples/Importers/ImportURDFDemo/URDFJointTypes.h
        ../../examples/Importers/ImportObjDemo/Wavefront2GLInstanceGraphicsShape.cpp
        ../../examples/Importers/ImportObjDemo/LoadMeshFromObj.cpp
        ../../examples/Importers/ImportSTLDemo/ImportSTLSetup.h
        ../../examples/Importers/ImportSTLDemo/LoadMeshFromSTL.h
        	../../examples/Importers/ImportColladaDemo/LoadMeshFromCollada.cpp
        	../../examples/Importers/ImportColladaDemo/ColladaGraphicsInstance.h
        	../../examples/ThirdPartyLibs/Wavefront/tiny_obj_loader.cpp
		../../examples/ThirdPartyLibs/tinyxml2/tinyxml2.cpp
		
        )
target_include_directories(Test_BulletInverseForwardDynamics PRIVATE ${_test_bullet_inverse_forward_dynamics_include_dirs})
target_compile_definitions(Test_BulletInverseForwardDynamics PRIVATE ${_test_bullet_inverse_forward_dynamics_compile_defs})
target_link_libraries(Test_BulletInverseForwardDynamics PRIVATE ${_test_bullet_inverse_forward_dynamics_link_libs})

ADD_TEST(Test_BulletInverseForwardDynamics_PASS Test_BulletInverseForwardDynamics)

IF (INTERNAL_ADD_POSTFIX_EXECUTABLE_NAMES)
                        SET_TARGET_PROPERTIES(Test_BulletInverseForwardDynamics PROPERTIES  DEBUG_POSTFIX "_Debug")
                        SET_TARGET_PROPERTIES(Test_BulletInverseForwardDynamics PROPERTIES  MINSIZEREL_POSTFIX "_MinsizeRel")
                        SET_TARGET_PROPERTIES(Test_BulletInverseForwardDynamics PROPERTIES  RELWITHDEBINFO_POSTFIX "_RelWithDebugInfo")
ENDIF(INTERNAL_ADD_POSTFIX_EXECUTABLE_NAMES)

IF (NOT WIN32)
	FIND_PACKAGE(Threads)
	list(APPEND _test_bullet_inverse_dynamics_link_libs ${CMAKE_THREAD_LIBS_INIT})
ENDIF()


        ADD_EXECUTABLE(Test_BulletInverseDynamics
                test_invdyn_kinematics.cpp
        )
target_include_directories(Test_BulletInverseDynamics PRIVATE ${_test_bullet_inverse_dynamics_include_dirs})
target_compile_definitions(Test_BulletInverseDynamics PRIVATE ${_test_bullet_inverse_dynamics_compile_defs})
target_link_libraries(Test_BulletInverseDynamics PRIVATE ${_test_bullet_inverse_dynamics_link_libs})

ADD_TEST(Test_BulletInverseDynamics_PASS Test_BulletInverseDynamics)

IF (INTERNAL_ADD_POSTFIX_EXECUTABLE_NAMES)
                        SET_TARGET_PROPERTIES(Test_BulletInverseDynamics PROPERTIES  DEBUG_POSTFIX "_Debug")
                        SET_TARGET_PROPERTIES(Test_BulletInverseDynamics PROPERTIES  MINSIZEREL_POSTFIX "_MinsizeRel")
                        SET_TARGET_PROPERTIES(Test_BulletInverseDynamics PROPERTIES  RELWITHDEBINFO_POSTFIX "_RelWithDebugInfo")
ENDIF(INTERNAL_ADD_POSTFIX_EXECUTABLE_NAMES)
