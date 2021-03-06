
SET( TARGET_NAME core-hello )
SET( TARGET_SRCS HelloWorld.cpp )
SET( TARGET_RES helloworld.xml quad.mesh terrainShader.glsl	)
SET( TARGET_DIR core )

ADD_EXECUTABLE( ${TARGET_NAME} ${TARGET_SRCS})

TARGET_LINK_LIBRARIES( ${TARGET_NAME}
	proland-core
	proland-graph
	proland-terrain
	proland-atmo
	proland-edit
	proland-forest
	proland-ocean
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
	TARGET_COMPILE_OPTIONS( ${TARGET_NAME} PRIVATE "-DPROLAND_API=__declspec(dllimport)" )
	TARGET_COMPILE_OPTIONS( ${TARGET_NAME} PRIVATE "-DORK_API=__declspec(dllimport)" "-DORK_TEMPLATE_API=" )
	TARGET_COMPILE_OPTIONS( ${TARGET_NAME} PRIVATE /wd"4244" /wd"4002" /wd"4305" /wd"4251" )
ENDIF( MSVC )

INSTALL( TARGETS ${TARGET_NAME} RUNTIME DESTINATION ${PROJECT_BINARY_DIR}/bin/${TARGET_DIR} )

INSTALL( FILES ${TARGET_RES} DESTINATION ${PROJECT_BINARY_DIR}/bin/${TARGET_DIR} )

FILE( WRITE 
	${PROJECT_BINARY_DIR}/bin/${TARGET_DIR}/${TARGET_NAME}-run.bat
	"echo off\n"
	"set PATH=${PROJECT_BINARY_DIR}/bin;%PATH%\n"
	"${TARGET_NAME}.exe"
	)
