
IF (BUILD_BLEND_DEMO OR INTERNAL_UPDATE_SERIALIZATION_STRUCTURES)
	#SUBDIRS(BlenderSerialize )
ENDIF()


IF(INTERNAL_UPDATE_SERIALIZATION_STRUCTURES)

# makesdna and HeaderGenerator are for advanced use only
# makesdna can re-generate the binary DNA representing the Bullet serialization structures
# Be very careful modifying any of this, otherwise the .bullet format becomes incompatible

	add_subdirectory(BulletFileLoader)
	add_subdirectory(BulletXmlWorldImporter)
	add_subdirectory(BulletWorldImporter)
	add_subdirectory(HeaderGenerator)
	add_subdirectory(makesdna)

ELSE(INTERNAL_UPDATE_SERIALIZATION_STRUCTURES)

	add_subdirectory(BulletFileLoader)
	add_subdirectory(BulletXmlWorldImporter)
	add_subdirectory(BulletWorldImporter)

ENDIF (INTERNAL_UPDATE_SERIALIZATION_STRUCTURES)
