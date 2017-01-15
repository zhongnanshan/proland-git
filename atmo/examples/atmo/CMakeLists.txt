
ADD_EXECUTABLE( atmo HelloWorld.cpp )

TARGET_LINK_LIBRARIES( atmo 
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
	TARGET_COMPILE_OPTIONS( atmo PRIVATE "-DPROLAND_API=" )
	TARGET_COMPILE_OPTIONS( atmo PRIVATE /wd"4244" /wd"4002" /wd"4305" )
ENDIF( MSVC )
