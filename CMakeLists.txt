PROJECT( Proland40 )

CMAKE_MINIMUM_REQUIRED( VERSION 2.6 )

SET( MY_COMPILE_OPTIONS "" )

# 设置外部依赖库
SET( PTHREAD_ROOT "${PROJECT_BINARY_DIR}/../ork3rd/pthreads-w32-2-8-0-release/Pre-built.2" )
IF( EXISTS ${PTHREAD_ROOT} )
	SET( PThreadInclude ${PTHREAD_ROOT}/include )
	SET( PThreadLib ${PTHREAD_ROOT}/lib/pthreadVCE2.lib )
	MESSAGE( "Pthread Finded!" )
ELSE()
	FIND_PATH( PThreadInclude pthread.h)
	FIND_LIBRARY( PThreadLib pthreadVCE2.lib)
	MESSAGE( "Pthread need configure!" )
ENDIF()

SET( PThreadDll ${PTHREAD_ROOT}/lib/pthreadVCE2.dll )

MESSAGE( ${PThreadInclude} )
MESSAGE( ${PThreadLib} )

SET( GLFW_ROOT "${PROJECT_BINARY_DIR}/../ork3rd/glfw-3.2.1.bin.WIN32" )
IF( EXISTS ${GLFW_ROOT} )
	SET( GLFWInclude ${GLFW_ROOT}/include )
	SET( GLFWLib ${GLFW_ROOT}/lib-vc2015/glfw3dll.lib )
	MESSAGE( "GLFW Finded!" )
ELSE()
	FIND_PATH( GLFWInclude GLFW/glfw3.h)
	FIND_LIBRARY( GLFWLib glfw3dll.lib)
	MESSAGE( "GLFW need configure!" )
ENDIF()

SET( GLFWDll ${GLFW_ROOT}/lib-vc2015/glfw3.dll )

MESSAGE( ${GLFWInclude} )
MESSAGE( ${GLFWLib} )

SET( LIBTIFF_ROOT "${PROJECT_BINARY_DIR}/../proland3rd/libtiff" )
IF( EXISTS ${LIBTIFF_ROOT} )
	SET( LibTiffInclude ${LIBTIFF_ROOT}/include )
	SET( LibTiffLib ${LIBTIFF_ROOT}/lib/libtiff.lib )
	MESSAGE( "LibTiff Finded!" )
ELSE()
	FIND_PATH( LibTiffInclude tiff.h)
	FIND_LIBRARY( LibTiffLib libtiff.lib)
	MESSAGE( "LibTiff need configure!" )
ENDIF()

SET( LibTiffDll 
	${LIBTIFF_ROOT}/bin/libtiff3.dll 
	${LIBTIFF_ROOT}/bin/zlib1.dll
	${LIBTIFF_ROOT}/bin/jpeg62.dll
	${LIBTIFF_ROOT}/bin/libimage.dll
	${LIBTIFF_ROOT}/bin/libpng13.dll
	)

MESSAGE( ${LibTiffInclude} )
MESSAGE( ${LibTiffLib} )

SET( ANTTWEAKBAR_ROOT "${PROJECT_BINARY_DIR}/../proland3rd/AntTweakBar" )
IF( EXISTS ${ANTTWEAKBAR_ROOT} )
	SET( AntTweakBarInclude ${ANTTWEAKBAR_ROOT}/include )
	SET( AntTweakBarLib ${ANTTWEAKBAR_ROOT}/lib/AntTweakBar.lib )
	MESSAGE( "AntTreakBar Finded!" )
ELSE()
	FIND_PATH( AntTweakBarInclude AntTweakBar.h)
	FIND_LIBRARY( AntTweakBarLib AntTweakBar.lib)
	MESSAGE( "AntTreakBar need configure!" )
ENDIF()

SET( AntTweakBarDll ${ANTTWEAKBAR_ROOT}/lib/AntTweakBar.dll )

MESSAGE( ${AntTweakBarInclude} )
MESSAGE( ${AntTweakBarLib} )

FIND_PATH( Ork3Include ork/core/Object.h)
FIND_LIBRARY( Ork3Lib Ork31.lib)
FIND_FILE( Ork3Dll Ork31.dll)

MESSAGE( ${Ork3Include} )
MESSAGE( ${Ork3Lib} )

SET( GLEW_ROOT "${PROJECT_BINARY_DIR}/../ork3rd/glew-1.5.6" )
IF( EXISTS ${GLEW_ROOT} )
	SET( GLEWInclude ${GLEW_ROOT}/include )
	SET( GLEWLib ${GLEW_ROOT}/lib/glew32.lib )
	MESSAGE( "GLEW Finded!" )
ELSE()
	FIND_PATH( GLEWInclude GL/glew.h)
	FIND_LIBRARY( GLEWLib glew32.lib)
	MESSAGE( "GLEW need configure!" )
ENDIF()

SET( GLEWDll ${GLEW_ROOT}/bin/glew32.dll )

MESSAGE( ${GLEWInclude} )
MESSAGE( ${GLEWLib} )

OPTION( B_USE_FREEGLUT "weather use freeglut or not?" ON )
IF( B_USE_FREEGLUT )
	SET( GLUT_ROOT "${PROJECT_BINARY_DIR}/../ork3rd/freeglut" )
	IF( EXISTS ${GLUT_ROOT} )
		SET( GLUTInclude ${GLUT_ROOT}/include )
		SET( GLUTLib ${GLUT_ROOT}/lib/freeglut.lib )
		MESSAGE( "GLUT Finded!" )
	ELSE()
		FIND_PATH( GLUTInclude GL/glut.h)
		FIND_LIBRARY( GLUTLib freeglut.lib)
		MESSAGE( "GLUT need configure!" )
	ENDIF()
	SET( GLUTDll ${GLUT_ROOT}/bin/freeglut.dll )
	SET( MY_COMPILE_OPTIONS "-DUSEFREEGLUT" )
ELSE()
	SET( GLUT_ROOT "${PROJECT_BINARY_DIR}/../ork3rd/glut-3.7.6-bin" )
	IF( EXISTS ${GLUT_ROOT} )
		SET( GLUTInclude ${GLUT_ROOT} )
		SET( GLUTLib ${GLUT_ROOT}/glut32.lib )
		MESSAGE( "GLUT Finded!" )
	ELSE()
		FIND_PATH( GLUTInclude GL/glut.h)
		FIND_LIBRARY( GLUTLib glut32.lib)
		MESSAGE( "GLUT need configure!" )
	ENDIF()
	SET( GLUTDll ${GLUT_ROOT}/glut32.dll )
ENDIF()

MESSAGE( ${GLUTInclude} )
MESSAGE( ${GLUTLib} )

# 设置编译器参数

ADD_DEFINITIONS(
	-DTIXML_USE_STL 
	-DSTBI_NO_STDIO 
	-DSTBI_NO_WRITE
#	-DHAVE_STRUCT_TIMESPEC
	-D_CRT_SECURE_NO_WARNINGS
#	-DORK_API=
	${MY_COMPILE_OPTIONS}
	)

OPTION( HAVE_STRUCT_TIMESPEC "weather define HAVE_STRUCT_TIMESPEC macro or not?(Don't define this in VS2012)" ON )
IF( HAVE_STRUCT_TIMESPEC )
	ADD_DEFINITIONS( -DHAVE_STRUCT_TIMESPEC )
ENDIF()

INCLUDE_DIRECTORIES(
	${PROJECT_SOURCE_DIR}/core/sources
	${PROJECT_SOURCE_DIR}/atmo/sources
	${PROJECT_SOURCE_DIR}/edit/sources
	${PROJECT_SOURCE_DIR}/forest/sources
	${PROJECT_SOURCE_DIR}/graph/sources
	${PROJECT_SOURCE_DIR}/ocean/sources
	${PROJECT_SOURCE_DIR}/river/sources
	${PROJECT_SOURCE_DIR}/terrain/sources
	${Ork3Include}
	${Ork3Include}/libraries
	${PThreadInclude}
	${AntTweakBarInclude}
	${GLEWInclude}
	)

OPTION( DEF_C99_MATH "define c99 math round and roundf library functions.(need under vs2012)" OFF )
IF( DEF_C99_MATH )
FILE( WRITE 
	${PROJECT_BINARY_DIR}/def_c99_math.h
	"#include <cmath>\n"
	"\n"
	"inline float roundf(float num)\n"
	"{\n"
	"    return num > 0 ? std::floor(num + 0.5f) : std::ceil(num - 0.5f);\n"
	"}\n"
	)
ADD_DEFINITIONS( -DDEF_C99_MATH )
INCLUDE_DIRECTORIES( ${PROJECT_BINARY_DIR} )
ENDIF()

#SET( EXECUTABLE_OUTPUT_PATH ${PROJECT_BINARY_DIR}/bin )

SET( CORE_SRCS 
		core/sources/proland/math/color.cpp
		core/sources/proland/math/color.h
		core/sources/proland/math/geometry.h
		core/sources/proland/math/noise.cpp
		core/sources/proland/math/noise.h
		core/sources/proland/math/seg2.h
		core/sources/proland/particles/LifeCycleParticleLayer.cpp
		core/sources/proland/particles/LifeCycleParticleLayer.h
		core/sources/proland/particles/ParticleLayer.cpp
		core/sources/proland/particles/ParticleLayer.h
		core/sources/proland/particles/ParticleProducer.cpp
		core/sources/proland/particles/ParticleProducer.h
		core/sources/proland/particles/ParticleStorage.cpp
		core/sources/proland/particles/ParticleStorage.h
		core/sources/proland/particles/RandomParticleLayer.cpp
		core/sources/proland/particles/RandomParticleLayer.h
		core/sources/proland/particles/WorldParticleLayer.cpp
		core/sources/proland/particles/WorldParticleLayer.h
		core/sources/proland/particles/screen/ParticleGrid.cpp
		core/sources/proland/particles/screen/ParticleGrid.h
		core/sources/proland/particles/screen/ScreenParticleLayer.cpp
		core/sources/proland/particles/screen/ScreenParticleLayer.h
		core/sources/proland/particles/terrain/FlowTile.cpp
		core/sources/proland/particles/terrain/FlowTile.h
		core/sources/proland/particles/terrain/TerrainParticleLayer.cpp
		core/sources/proland/particles/terrain/TerrainParticleLayer.h
		core/sources/proland/producer/CPUTileStorage.cpp
		core/sources/proland/producer/CPUTileStorage.h
		core/sources/proland/producer/GPUTileStorage.cpp
		core/sources/proland/producer/GPUTileStorage.h
		core/sources/proland/producer/ObjectTileStorage.cpp
		core/sources/proland/producer/ObjectTileStorage.h
		core/sources/proland/producer/TileCache.cpp
		core/sources/proland/producer/TileCache.h
		core/sources/proland/producer/TileLayer.cpp
		core/sources/proland/producer/TileLayer.h
		core/sources/proland/producer/TileProducer.cpp
		core/sources/proland/producer/TileProducer.h
		core/sources/proland/producer/TileStorage.cpp
		core/sources/proland/producer/TileStorage.h
		core/sources/proland/terrain/CylindricalDeformation.cpp
		core/sources/proland/terrain/CylindricalDeformation.h
		core/sources/proland/terrain/Deformation.cpp
		core/sources/proland/terrain/Deformation.h
		core/sources/proland/terrain/DrawTerrainTask.cpp
		core/sources/proland/terrain/DrawTerrainTask.h
		core/sources/proland/terrain/ReadbackManager.cpp
		core/sources/proland/terrain/ReadbackManager.h
		core/sources/proland/terrain/SphericalDeformation.cpp
		core/sources/proland/terrain/SphericalDeformation.h
		core/sources/proland/terrain/TerrainNode.cpp
		core/sources/proland/terrain/TerrainNode.h
		core/sources/proland/terrain/TerrainQuad.cpp
		core/sources/proland/terrain/TerrainQuad.h
		core/sources/proland/terrain/TileSampler.cpp
		core/sources/proland/terrain/TileSampler.h
		core/sources/proland/terrain/TileSamplerZ.cpp
		core/sources/proland/terrain/TileSamplerZ.h
		core/sources/proland/terrain/UpdateTerrainTask.cpp
		core/sources/proland/terrain/UpdateTerrainTask.h
		core/sources/proland/terrain/UpdateTileSamplersTask.cpp
		core/sources/proland/terrain/UpdateTileSamplersTask.h
		core/sources/proland/ui/BasicViewHandler.cpp
		core/sources/proland/ui/BasicViewHandler.h
		core/sources/proland/ui/EventRecorder.cpp
		core/sources/proland/ui/EventRecorder.h
		core/sources/proland/ui/MousePositionHandler.cpp
		core/sources/proland/ui/MousePositionHandler.h
		core/sources/proland/ui/SceneVisitor.cpp
		core/sources/proland/ui/SceneVisitor.h
		core/sources/proland/ui/twbar/DrawTweakBarTask.cpp
		core/sources/proland/ui/twbar/DrawTweakBarTask.h
		core/sources/proland/ui/twbar/TweakBarHandler.cpp
		core/sources/proland/ui/twbar/TweakBarHandler.h
		core/sources/proland/ui/twbar/TweakBarManager.cpp
		core/sources/proland/ui/twbar/TweakBarManager.h
		core/sources/proland/ui/twbar/TweakResource.cpp
		core/sources/proland/ui/twbar/TweakResource.h
		core/sources/proland/ui/twbar/TweakSceneGraph.cpp
		core/sources/proland/ui/twbar/TweakSceneGraph.h
		core/sources/proland/ui/twbar/TweakViewHandler.cpp
		core/sources/proland/ui/twbar/TweakViewHandler.h
		core/sources/proland/util/CylinderViewController.cpp
		core/sources/proland/util/CylinderViewController.h
		core/sources/proland/util/PlanetViewController.cpp
		core/sources/proland/util/PlanetViewController.h
		core/sources/proland/util/TerrainViewController.cpp
		core/sources/proland/util/TerrainViewController.h
		core/sources/proland/util/mfs.cpp
		core/sources/proland/util/mfs.h
	)

# 生成动态库
	
ADD_LIBRARY( proland-core SHARED ${CORE_SRCS} )

TARGET_LINK_LIBRARIES( proland-core
	${AntTweakBarLib} 
	${Ork3Lib} 
	${PThreadLib} 
	${GLEWLib}
	opengl32.lib
	glu32.lib
	)

IF( MSVC )
	TARGET_COMPILE_OPTIONS( proland-core PRIVATE "-DPROLAND_API=__declspec(dllexport)" )
	TARGET_COMPILE_OPTIONS( proland-core PRIVATE "-DORK_API=__declspec(dllimport)" "-DORK_TEMPLATE_API=" )
	TARGET_COMPILE_OPTIONS( proland-core PRIVATE /wd"4244" /wd"4091" /wd"4002" /wd"4251" )
ENDIF( MSVC )

SET( ATMO_SRCS 
		atmo/sources/proland/preprocess/atmo/PreprocessAtmo.cpp
		atmo/sources/proland/preprocess/atmo/PreprocessAtmo.h
	)

ADD_LIBRARY( proland-atmo SHARED ${ATMO_SRCS} )

TARGET_LINK_LIBRARIES( proland-atmo
	${Ork3Lib}
	${GLEWLib}
	${PThreadLib}
	${GLUTLib}
	opengl32.lib
	glu32.lib
	)

IF( MSVC )
	TARGET_COMPILE_OPTIONS( proland-atmo PRIVATE "-DPROLAND_API=__declspec(dllexport)" )
	TARGET_COMPILE_OPTIONS( proland-atmo PRIVATE "-DORK_API=__declspec(dllimport)" "-DORK_TEMPLATE_API=" )
	TARGET_COMPILE_OPTIONS( proland-atmo PRIVATE /wd"4244" /wd"4305" /wd"4251" /wd"4091" )
ENDIF( MSVC )

SET( EDIT_SRCS
		edit/sources/proland/EditPlugin.cpp
		edit/sources/proland/EditPlugin.h
		edit/sources/proland/edit/EditElevationProducer.cpp
		edit/sources/proland/edit/EditElevationProducer.h
		edit/sources/proland/edit/EditGraphOrthoLayer.cpp
		edit/sources/proland/edit/EditGraphOrthoLayer.h
		edit/sources/proland/edit/EditHydroGraphOrthoLayer.cpp
		edit/sources/proland/edit/EditHydroGraphOrthoLayer.h
		edit/sources/proland/edit/EditOrthoCPUProducer.cpp
		edit/sources/proland/edit/EditOrthoCPUProducer.h
		edit/sources/proland/edit/EditOrthoProducer.cpp
		edit/sources/proland/edit/EditOrthoProducer.h
		edit/sources/proland/edit/EditResidualProducer.cpp
		edit/sources/proland/edit/EditResidualProducer.h
		edit/sources/proland/edit/EditorHandler.cpp
		edit/sources/proland/edit/EditorHandler.h
		edit/sources/proland/ui/twbar/TweakDemEditor.cpp
		edit/sources/proland/ui/twbar/TweakDemEditor.h
		edit/sources/proland/ui/twbar/TweakGraphLayer.cpp
		edit/sources/proland/ui/twbar/TweakGraphLayer.h
		edit/sources/proland/ui/twbar/TweakHydroGraphLayer.cpp
		edit/sources/proland/ui/twbar/TweakHydroGraphLayer.h
		edit/sources/proland/ui/twbar/TweakOrthoEditor.cpp
		edit/sources/proland/ui/twbar/TweakOrthoEditor.h
		edit/sources/proland/ui/twbar/TweakRivers.cpp
		edit/sources/proland/ui/twbar/TweakRivers.h
	)

ADD_LIBRARY( proland-edit SHARED ${EDIT_SRCS} )

TARGET_LINK_LIBRARIES( proland-edit 
	proland-core
	proland-terrain
	proland-graph
	proland-river
	${AntTweakBarLib} 
	${Ork3Lib}
#	${GLEWLib}
#	${PThreadLib}
#	${GLUTLib}
#	opengl32.lib
#	glu32.lib
	)

IF( MSVC )
	TARGET_COMPILE_OPTIONS( proland-edit PRIVATE "-DPROLAND_API=__declspec(dllexport)" )
	TARGET_COMPILE_OPTIONS( proland-edit PRIVATE "-DORK_API=__declspec(dllimport)" "-DORK_TEMPLATE_API=" )
	TARGET_COMPILE_OPTIONS( proland-edit PRIVATE /wd"4091" /wd"4002" /wd"4244" /wd"4251" /wd"4275" )
ENDIF( MSVC )

SET( FOREST_SRCS
		forest/sources/proland/ForestPlugin.cpp
		forest/sources/proland/ForestPlugin.h
		forest/sources/proland/plants/DrawPlantsShadowTask.cpp
		forest/sources/proland/plants/DrawPlantsShadowTask.h
		forest/sources/proland/plants/DrawPlantsTask.cpp
		forest/sources/proland/plants/DrawPlantsTask.h
		forest/sources/proland/plants/LccProducer.cpp
		forest/sources/proland/plants/LccProducer.h
		forest/sources/proland/plants/Plants.cpp
		forest/sources/proland/plants/Plants.h
		forest/sources/proland/plants/PlantsProducer.cpp
		forest/sources/proland/plants/PlantsProducer.h
		forest/sources/proland/preprocess/trees/PreprocessTree.cpp
		forest/sources/proland/preprocess/trees/PreprocessTree.h
		forest/sources/proland/preprocess/trees/PreprocessTreeTables.cpp
	)

ADD_LIBRARY( proland-forest SHARED ${FOREST_SRCS} )

TARGET_INCLUDE_DIRECTORIES( proland-forest PRIVATE ${LibTiffInclude} )

TARGET_LINK_LIBRARIES( proland-forest 
	proland-core
	${LibTiffLib}
	${Ork3Lib}
	${GLUTLib}
#	${AntTweakBarLib} 
#	${GLEWLib}
#	${PThreadLib}
#	opengl32.lib
#	glu32.lib
	)

IF( MSVC )
	TARGET_COMPILE_OPTIONS( proland-forest PRIVATE "-DPROLAND_API=__declspec(dllexport)" )
	TARGET_COMPILE_OPTIONS( proland-forest PRIVATE "-DORK_API=__declspec(dllimport)" "-DORK_TEMPLATE_API=" )
	TARGET_COMPILE_OPTIONS( proland-forest PRIVATE /wd"4091" /wd"4244" /wd"4002" /wd"4305" /wd"4251" )
ENDIF( MSVC )

SET( GRAPH_SRCS
		graph/sources/proland/GraphPlugin.cpp
		graph/sources/proland/GraphPlugin.h
		graph/sources/proland/dem/ElevationCurveData.cpp
		graph/sources/proland/dem/ElevationCurveData.h
		graph/sources/proland/dem/ElevationGraphLayer.cpp
		graph/sources/proland/dem/ElevationGraphLayer.h
		graph/sources/proland/dem/ElevationMargin.cpp
		graph/sources/proland/dem/ElevationMargin.h
		graph/sources/proland/dem/RoadElevationLayer.cpp
		graph/sources/proland/dem/RoadElevationLayer.h
		graph/sources/proland/dem/WaterElevationLayer.cpp
		graph/sources/proland/dem/WaterElevationLayer.h
		graph/sources/proland/graph/Area.cpp
		graph/sources/proland/graph/Area.h
		graph/sources/proland/graph/BasicCurvePart.cpp
		graph/sources/proland/graph/BasicCurvePart.h
		graph/sources/proland/graph/BasicGraph.cpp
		graph/sources/proland/graph/BasicGraph.h
		graph/sources/proland/graph/ComposedMargin.cpp
		graph/sources/proland/graph/ComposedMargin.h
		graph/sources/proland/graph/Curve.cpp
		graph/sources/proland/graph/Curve.h
		graph/sources/proland/graph/CurvePart.cpp
		graph/sources/proland/graph/CurvePart.h
		graph/sources/proland/graph/FileReader.cpp
		graph/sources/proland/graph/FileReader.h
		graph/sources/proland/graph/FileWriter.cpp
		graph/sources/proland/graph/FileWriter.h
		graph/sources/proland/graph/Graph.cpp
		graph/sources/proland/graph/Graph.h
		graph/sources/proland/graph/GraphListener.cpp
		graph/sources/proland/graph/GraphListener.h
		graph/sources/proland/graph/LazyArea.cpp
		graph/sources/proland/graph/LazyArea.h
		graph/sources/proland/graph/LazyCurve.cpp
		graph/sources/proland/graph/LazyCurve.h
		graph/sources/proland/graph/LazyGraph.cpp
		graph/sources/proland/graph/LazyGraph.h
		graph/sources/proland/graph/LazyNode.cpp
		graph/sources/proland/graph/LazyNode.h
		graph/sources/proland/graph/LineCurvePart.cpp
		graph/sources/proland/graph/LineCurvePart.h
		graph/sources/proland/graph/Margin.cpp
		graph/sources/proland/graph/Margin.h
		graph/sources/proland/graph/Node.cpp
		graph/sources/proland/graph/Node.h
		graph/sources/proland/graph/producer/CurveData.cpp
		graph/sources/proland/graph/producer/CurveData.h
		graph/sources/proland/graph/producer/CurveDataFactory.cpp
		graph/sources/proland/graph/producer/CurveDataFactory.h
		graph/sources/proland/graph/producer/GetCurveDatasTask.h
		graph/sources/proland/graph/producer/GraphLayer.cpp
		graph/sources/proland/graph/producer/GraphLayer.h
		graph/sources/proland/graph/producer/GraphProducer.cpp
		graph/sources/proland/graph/producer/GraphProducer.h
		graph/sources/proland/graph/producer/Tesselator.cpp
		graph/sources/proland/graph/producer/Tesselator.h
		graph/sources/proland/ortho/FieldsOrthoLayer.cpp
		graph/sources/proland/ortho/FieldsOrthoLayer.h
		graph/sources/proland/ortho/ForestOrthoLayer.cpp
		graph/sources/proland/ortho/ForestOrthoLayer.h
		graph/sources/proland/ortho/LineOrthoLayer.cpp
		graph/sources/proland/ortho/LineOrthoLayer.h
		graph/sources/proland/ortho/MaskOrthoLayer.cpp
		graph/sources/proland/ortho/MaskOrthoLayer.h
		graph/sources/proland/ortho/OrthoMargin.cpp
		graph/sources/proland/ortho/OrthoMargin.h
		graph/sources/proland/ortho/RoadOrthoLayer.cpp
		graph/sources/proland/ortho/RoadOrthoLayer.h
		graph/sources/proland/ortho/WaterOrthoLayer.cpp
		graph/sources/proland/ortho/WaterOrthoLayer.h
	)

ADD_LIBRARY( proland-graph SHARED ${GRAPH_SRCS} )

TARGET_LINK_LIBRARIES( proland-graph 
	proland-core
	proland-terrain
#	${LibTiffLib}
	${Ork3Lib}
#	${GLUTLib}
	${AntTweakBarLib} 
#	${GLEWLib}
	${PThreadLib}
	opengl32.lib
	glu32.lib
	)

IF( MSVC )
	TARGET_COMPILE_OPTIONS( proland-graph PRIVATE "-DPROLAND_API=__declspec(dllexport)" )
	TARGET_COMPILE_OPTIONS( proland-graph PRIVATE "-DORK_API=__declspec(dllimport)" "-DORK_TEMPLATE_API=" )
	TARGET_COMPILE_OPTIONS( proland-graph PRIVATE /wd"4244" /wd"4002" /wd"4251" /wd"4091" )
ENDIF( MSVC )

SET( OCEAN_SRCS
		ocean/sources/proland/OceanPlugin.cpp
		ocean/sources/proland/OceanPlugin.h
		ocean/sources/proland/ocean/DrawOceanFFTTask.cpp
		ocean/sources/proland/ocean/DrawOceanFFTTask.h
		ocean/sources/proland/ocean/DrawOceanTask.cpp
		ocean/sources/proland/ocean/DrawOceanTask.h
	)

ADD_LIBRARY( proland-ocean SHARED ${OCEAN_SRCS} )

TARGET_LINK_LIBRARIES( proland-ocean 
	proland-core
#	${LibTiffLib}
	${Ork3Lib}
#	${GLUTLib}
#	${AntTweakBarLib} 
#	${GLEWLib}
	${PThreadLib}
	opengl32.lib
	glu32.lib
	)

IF( MSVC )
	TARGET_COMPILE_OPTIONS( proland-ocean PRIVATE "-DPROLAND_API=__declspec(dllexport)" )
	TARGET_COMPILE_OPTIONS( proland-ocean PRIVATE "-DORK_API=__declspec(dllimport)" "-DORK_TEMPLATE_API=" )
	TARGET_COMPILE_OPTIONS( proland-ocean PRIVATE /wd"4244" /wd"4002" /wd"4305" /wd"4251" /wd"4091" )
ENDIF( MSVC )

SET( TERRAIN_SRCS
		terrain/sources/proland/TerrainPlugin.cpp
		terrain/sources/proland/TerrainPlugin.h
		terrain/sources/proland/dem/CPUElevationProducer.cpp
		terrain/sources/proland/dem/CPUElevationProducer.h
		terrain/sources/proland/dem/ElevationProducer.cpp
		terrain/sources/proland/dem/ElevationProducer.h
		terrain/sources/proland/dem/NormalProducer.cpp
		terrain/sources/proland/dem/NormalProducer.h
		terrain/sources/proland/dem/ResidualProducer.cpp
		terrain/sources/proland/dem/ResidualProducer.h
		terrain/sources/proland/ortho/EmptyOrthoLayer.cpp
		terrain/sources/proland/ortho/EmptyOrthoLayer.h
		terrain/sources/proland/ortho/OrthoCPUProducer.cpp
		terrain/sources/proland/ortho/OrthoCPUProducer.h
		terrain/sources/proland/ortho/OrthoGPUProducer.cpp
		terrain/sources/proland/ortho/OrthoGPUProducer.h
		terrain/sources/proland/ortho/OrthoProducer.cpp
		terrain/sources/proland/ortho/OrthoProducer.h
		terrain/sources/proland/ortho/TextureLayer.cpp
		terrain/sources/proland/ortho/TextureLayer.h
		terrain/sources/proland/preprocess/terrain/AbstractTileCache.cpp
		terrain/sources/proland/preprocess/terrain/AbstractTileCache.h
		terrain/sources/proland/preprocess/terrain/ApertureMipmap.cpp
		terrain/sources/proland/preprocess/terrain/ApertureMipmap.h
		terrain/sources/proland/preprocess/terrain/ColorMipmap.cpp
		terrain/sources/proland/preprocess/terrain/ColorMipmap.h
		terrain/sources/proland/preprocess/terrain/HeightMipmap.cpp
		terrain/sources/proland/preprocess/terrain/HeightMipmap.h
		terrain/sources/proland/preprocess/terrain/Preprocess.cpp
		terrain/sources/proland/preprocess/terrain/Preprocess.h
		terrain/sources/proland/preprocess/terrain/Util.cpp
		terrain/sources/proland/preprocess/terrain/Util.h
	) 

ADD_LIBRARY( proland-terrain SHARED ${TERRAIN_SRCS} )

TARGET_INCLUDE_DIRECTORIES( proland-terrain PRIVATE ${LibTiffInclude} )

TARGET_LINK_LIBRARIES( proland-terrain 
	proland-core
	${LibTiffLib}
	${Ork3Lib}
#	${GLUTLib}
#	${AntTweakBarLib} 
#	${GLEWLib}
	${PThreadLib}
#	opengl32.lib
#	glu32.lib
	)

IF( MSVC )
	TARGET_COMPILE_OPTIONS( proland-terrain PRIVATE "-DPROLAND_API=__declspec(dllexport)" )
	TARGET_COMPILE_OPTIONS( proland-terrain PRIVATE "-DORK_API=__declspec(dllimport)" "-DORK_TEMPLATE_API=" )
	TARGET_COMPILE_OPTIONS( proland-terrain PRIVATE /wd"4244" /wd"4002" /wd"4305" /wd"4251" /wd"4091" )
ENDIF( MSVC )

SET( RIVER_SRCS
		river/sources/proland/RiverPlugin.cpp
		river/sources/proland/RiverPlugin.h
		river/sources/proland/rivers/AnimatedPerlinWaveTile.cpp
		river/sources/proland/rivers/AnimatedPerlinWaveTile.h
		river/sources/proland/rivers/DrawRiversTask.cpp
		river/sources/proland/rivers/DrawRiversTask.h
		river/sources/proland/rivers/HydroFlowProducer.cpp
		river/sources/proland/rivers/HydroFlowProducer.h
		river/sources/proland/rivers/HydroFlowTile.cpp
		river/sources/proland/rivers/HydroFlowTile.h
		river/sources/proland/rivers/PerlinWaveTile.cpp
		river/sources/proland/rivers/PerlinWaveTile.h
		river/sources/proland/rivers/UpdateRiversTask.cpp
		river/sources/proland/rivers/UpdateRiversTask.h
		river/sources/proland/rivers/WaveTile.cpp
		river/sources/proland/rivers/WaveTile.h
		river/sources/proland/rivers/graph/HydroCurve.cpp
		river/sources/proland/rivers/graph/HydroCurve.h
		river/sources/proland/rivers/graph/HydroGraph.cpp
		river/sources/proland/rivers/graph/HydroGraph.h
		river/sources/proland/rivers/graph/HydroGraphFactory.cpp
		river/sources/proland/rivers/graph/HydroGraphFactory.h
		river/sources/proland/rivers/graph/LazyHydroCurve.cpp
		river/sources/proland/rivers/graph/LazyHydroCurve.h
		river/sources/proland/rivers/graph/LazyHydroGraph.cpp
		river/sources/proland/rivers/graph/LazyHydroGraph.h
		river/sources/proland/rivers/graph/LazyHydroGraphFactory.cpp
		river/sources/proland/rivers/graph/LazyHydroGraphFactory.h
		river/sources/proland/ui/twbar/TweakRivers.cpp
		river/sources/proland/ui/twbar/TweakRivers.h
	)

ADD_LIBRARY( proland-river SHARED ${RIVER_SRCS} )

TARGET_LINK_LIBRARIES( proland-river 
	proland-core
	proland-graph
	proland-terrain
#	${LibTiffLib}
	${Ork3Lib}
#	${GLUTLib}
#	${AntTweakBarLib} 
#	${GLEWLib}
	${PThreadLib}
	opengl32.lib
	glu32.lib
	)

#TARGET_INCLUDE_DIRECTORIES( proland-river-s PRIVATE ${GLEWInclude} )

IF( MSVC )
	TARGET_COMPILE_OPTIONS( proland-river PRIVATE "-DPROLAND_API=__declspec(dllexport)" )
	TARGET_COMPILE_OPTIONS( proland-river PRIVATE "-DORK_API=__declspec(dllimport)" "-DORK_TEMPLATE_API=" )
	TARGET_COMPILE_OPTIONS( proland-river PRIVATE /wd"4244" /wd"4002" /wd"4305" /wd"4251" /wd"4091" )
ENDIF( MSVC )

#ADD_EXECUTABLE( demo demo/ProlandDemo.cpp )

#TARGET_INCLUDE_DIRECTORIES( demo PRIVATE ${LibTiffInclude} )

#TARGET_LINK_LIBRARIES( demo 
#	proland-core
#	proland-graph
#	proland-terrain
#	proland-atmo
#	proland-edit
#	proland-forest
#	proland-ocean
#	${LibTiffLib}
#	${Ork3Lib}
#	${GLUTLib}
#	${AntTweakBarLib} 
#	${GLEWLib}
#	${PThreadLib}
#	opengl32.lib
#	glu32.lib
#	)

#IF( MSVC )
#	TARGET_COMPILE_OPTIONS( demo PRIVATE "-DPROLAND_API=__declspec(dllimport)" )
#	TARGET_COMPILE_OPTIONS( demo PRIVATE "-DORK_API=__declspec(dllimport)" "-DORK_TEMPLATE_API=" )
#	TARGET_COMPILE_OPTIONS( demo PRIVATE /wd"4244" /wd"4002" /wd"4305" /wd"4251" /wd"4091" )
#ENDIF( MSVC )

ADD_SUBDIRECTORY( demo )
ADD_SUBDIRECTORY( atmo/examples/atmo )
ADD_SUBDIRECTORY( core/examples/helloworld )
ADD_SUBDIRECTORY( edit/examples/edit1 )
ADD_SUBDIRECTORY( edit/examples/edit2 )
ADD_SUBDIRECTORY( edit/examples/edit3 )
ADD_SUBDIRECTORY( edit/examples/edit4 )
ADD_SUBDIRECTORY( forest/examples/trees1 )
ADD_SUBDIRECTORY( graph/examples/graph1 )
ADD_SUBDIRECTORY( ocean/examples/ocean1 )
ADD_SUBDIRECTORY( ocean/examples/ocean2 )
ADD_SUBDIRECTORY( river/examples/river1 )
ADD_SUBDIRECTORY( terrain/examples/exercise1 )
ADD_SUBDIRECTORY( terrain/examples/exercise2 )
ADD_SUBDIRECTORY( terrain/examples/exercise3 )
ADD_SUBDIRECTORY( terrain/examples/exercise4 )
ADD_SUBDIRECTORY( terrain/examples/exercise5 )
ADD_SUBDIRECTORY( terrain/examples/preprocess )
ADD_SUBDIRECTORY( terrain/examples/terrain1 )
ADD_SUBDIRECTORY( terrain/examples/terrain2 )
ADD_SUBDIRECTORY( terrain/examples/terrain3 )
ADD_SUBDIRECTORY( terrain/examples/terrain4 )
ADD_SUBDIRECTORY( terrain/examples/terrain5 )

INSTALL( TARGETS 
	proland-atmo
	proland-core
	proland-edit
	proland-forest
	proland-graph
	proland-ocean
	proland-river
	proland-terrain
	RUNTIME DESTINATION 
	${PROJECT_BINARY_DIR}/bin
	)

INSTALL( FILES 
	${Ork3Dll}
	${PThreadDll} 
	${GLFWDll} 
	${LibTiffDll} 
	${AntTweakBarDll} 
	${GLEWDll}
	${GLUTDll}
	DESTINATION 
	${PROJECT_BINARY_DIR}/bin 
	)
