
INCLUDE_DIRECTORIES(
	${PROJECT_ROOT_DIR}/src
	${PROJECT_ROOT_DIR}/examples/ThirdPartyLibs
	${PROJECT_ROOT_DIR}/examples/ThirdPartyLibs/glad
)

IF(NOT WIN32 AND NOT APPLE)
        INCLUDE_DIRECTORIES(
                                        ${PROJECT_ROOT_DIR}/examples/ThirdPartyLibs/optionalX11
        )
        ADD_DEFINITIONS(-DGLEW_STATIC)
        ADD_DEFINITIONS("-DGLEW_INIT_OPENGL11_FUNCTIONS=1")
        ADD_DEFINITIONS("-DGLEW_DYNAMIC_LOAD_ALL_GLX_FUNCTIONS=1")
        ADD_DEFINITIONS("-DDYNAMIC_LOAD_X11_FUNCTIONS=1")
ENDIF()

ADD_DEFINITIONS( -DGLEW_STATIC -DGWEN_COMPILE_STATIC -D_HAS_EXCEPTIONS=0 )

FILE(GLOB gwen_SRCS "*.cpp" "Controls/*.cpp" "Controls/Dialog/*.cpp" "Controls/Dialogs/*.cpp" "Controls/Layout/*.cpp" "Controls/Property/*.cpp" "Input/*.cpp" "Platforms/*.cpp" "Renderers/*.cpp" "Skins/*.cpp")
FILE(GLOB gwen_HDRS "*.h" "Controls/*.h" "Controls/Dialog/*.h" "Controls/Dialogs/*.h" "Controls/Layout/*.h" "Controls/Property/*.h" "Input/*.h" "Platforms/*.h" "Renderers/*.h" "Skins/*.h")

ADD_LIBRARY(gwen ${gwen_SRCS} ${gwen_HDRS})
IF (BUILD_SHARED_LIBS)
IF(WIN32 OR APPLE)
  target_link_libraries(gwen ${OPENGL_gl_LIBRARY})
ENDIF()
ENDIF()


INSTALL(TARGETS gwen
        RUNTIME DESTINATION bin
        LIBRARY DESTINATION lib${LIB_SUFFIX}
        ARCHIVE DESTINATION lib${LIB_SUFFIX})
