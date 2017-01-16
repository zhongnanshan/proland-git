
SET( TARGET_NAME terrain-exercise1 )
SET( TARGET_SRCS HelloWorld.cpp MyStaticRef.cpp )
SET( TARGET_RES 
	helloworld.xml 
	quad.mesh 
	square.mesh
	atmosphereShader.glhl
	atmosphereShader.glsl
	globalsShader.glsl
	normalShader.glsl
	oceanShader.glsl
	skyShader.glsl
	terrainShader.glsl
	textShader.glsl
	upsampleOrthoShader.glsl
	upsampleShader.glsl
	text.png
	sunglare.png
	)
SET( TARGET_DIR terrain-exercise1 )

ADD_EXECUTABLE( ${TARGET_NAME} ${TARGET_SRCS})

TARGET_LINK_LIBRARIES( ${TARGET_NAME}
	proland-core-s
	proland-graph-s
	proland-terrain-s
	proland-atmo-s
	proland-edit-s
	proland-forest-s
	proland-ocean-s
	${LibTiffLib}
	${Ork3Lib}
	${GLUTLib}
	${AntTweakBarLib} 
	${GLEWLib}
	${PThreadLib}
	opengl32.lib
	glu32.lib
	)

IF( MSVC )
	TARGET_COMPILE_OPTIONS( ${TARGET_NAME} PRIVATE 
	"-DPROLAND_API=" 
	"-DUSE_MY_ORK_STATIC_REF" 
	"-DUSE_MY_PROLAND_CORE_STATIC_REF" 
	"-DUSE_MY_PROLAND_GRAPH_STATIC_REF"
	"-DUSE_MY_PROLAND_TERRAIN_STATIC_REF"
	"-DUSE_MY_PROLAND_OCEAN_STATIC_REF"
	)
	TARGET_COMPILE_OPTIONS( ${TARGET_NAME} PRIVATE /wd"4244" /wd"4002" /wd"4305" )
ENDIF( MSVC )

INSTALL( TARGETS ${TARGET_NAME} RUNTIME DESTINATION ${PROJECT_BINARY_DIR}/bin/${TARGET_DIR} )

INSTALL( FILES ${TARGET_RES} DESTINATION ${PROJECT_BINARY_DIR}/bin/${TARGET_DIR} )

FILE( WRITE 
	${PROJECT_BINARY_DIR}/bin/${TARGET_DIR}/${TARGET_NAME}-run.bat
	"echo off\n"
	"set PATH=${PROJECT_BINARY_DIR}/bin;%PATH%\n"
	"${TARGET_NAME}.exe"
	)
