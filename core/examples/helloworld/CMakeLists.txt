
ADD_EXECUTABLE( core-hello HelloWorld.cpp MyStaticRef.cpp )

TARGET_LINK_LIBRARIES( core-hello 
	proland-core-s
	proland-graph-s
	proland-terrain-s
	proland-atmo-s
	proland-edit-s
	proland-forest-s
	proland-ocean-s
#	${LibTiffLib}
	${Ork3Lib}
	${GLUTLib}
#	${AntTweakBarLib} 
#	${GLEWLib}
	${PThreadLib}
	opengl32.lib
	glu32.lib
	)

IF( MSVC )
	TARGET_COMPILE_OPTIONS( core-hello PRIVATE "-DPROLAND_API=" "-DUSE_MY_ORK_STATIC_REF" "-DUSE_MY_PROLAND_CORE_STATIC_REF" )
	TARGET_COMPILE_OPTIONS( core-hello PRIVATE /wd"4244" /wd"4002" /wd"4305" )
ENDIF( MSVC )
