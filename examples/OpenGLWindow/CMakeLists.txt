
INCLUDE_DIRECTORIES(
	..
	../ThirdPartyLibs
	../../src
	../ThirdPartyLibs/glad
)

FILE(GLOB OpenGLWindow_HDRS "*.h" )

FILE(GLOB OpenGLWindowMac_CPP "Mac*.cpp")
FILE(GLOB OpenGLWindowMacObjC_CPP "Mac*.m")
FILE(GLOB OpenGLWindowWin32_CPP "Win32*.cpp")
FILE(GLOB OpenGLWindowLinux_CPP "X11*.cpp")

FILE(GLOB OpenGLWindowCommon_CPP "*.cpp" )

LIST(REMOVE_ITEM OpenGLWindowCommon_CPP ${OpenGLWindowMac_CPP} )
LIST(REMOVE_ITEM OpenGLWindowCommon_CPP ${OpenGLWindowWin32_CPP} )
LIST(REMOVE_ITEM OpenGLWindowCommon_CPP ${OpenGLWindowLinux_CPP} )
LIST(REMOVE_ITEM OpenGLWindowCommon_CPP X11OpenGLWindow.cpp )
LIST(REMOVE_ITEM OpenGLWindowCommon_CPP MacOpenGLWindow.cpp )
#MESSAGE (${OpenGLWindowCommon_CPP})

IF (WIN32)
        SET(OpenGLWindow_SRCS ${PROJECT_ROOT_DIR}/examples/ThirdPartyLibs/glad/gl.c ${OpenGLWindowWin32_CPP} ${OpenGLWindowCommon_CPP})
	INCLUDE_DIRECTORIES(
				${PROJECT_ROOT_DIR}/examples/ThirdPartyLibs/glad
	)
	ADD_DEFINITIONS(-DGLEW_STATIC)
ENDIF(WIN32)

IF (APPLE)
        SET(OpenGLWindow_SRCS ${OpenGLWindowMac_CPP} ${PROJECT_ROOT_DIR}/examples/ThirdPartyLibs/glad/gl.c ${OpenGLWindowMacObjC_CPP}  ${OpenGLWindowCommon_CPP} )
ENDIF(APPLE)

#no Linux detection?
IF(NOT WIN32 AND NOT APPLE)
	INCLUDE_DIRECTORIES(
		${PROJECT_ROOT_DIR}/examples/ThirdPartyLibs/glad
		${PROJECT_ROOT_DIR}/examples/ThirdPartyLibs/optionalX11
        )
	ADD_DEFINITIONS(-DGLEW_STATIC)
	ADD_DEFINITIONS("-DGLEW_INIT_OPENGL11_FUNCTIONS=1")
       	ADD_DEFINITIONS("-DGLEW_DYNAMIC_LOAD_ALL_GLX_FUNCTIONS=1")
	ADD_DEFINITIONS("-DDYNAMIC_LOAD_X11_FUNCTIONS=1")
        SET(OpenGLWindow_SRCS ${OpenGLWindowLinux_CPP}  ${PROJECT_ROOT_DIR}/examples/ThirdPartyLibs/glad/glx.c ${PROJECT_ROOT_DIR}/examples/ThirdPartyLibs/glad/gl.c ${OpenGLWindowCommon_CPP} )
ENDIF()

IF(BUILD_EGL)
    SET(OpenGLWindow_SRCS ${OpenGLWindow_SRCS}  ${PROJECT_ROOT_DIR}/examples/ThirdPartyLibs/glad/egl.c)
ENDIF(BUILD_EGL)

SET(OpenGLWindow_SRCS ${OpenGLWindow_SRCS}  ${PROJECT_ROOT_DIR}/examples/ThirdPartyLibs/stb_image/stb_image_write.cpp)


ADD_LIBRARY(OpenGLWindow ${OpenGLWindow_SRCS} ${OpenGLWindow_HDRS})
if (UNIX AND NOT APPLE)
  target_link_libraries(OpenGLWindow ${DL})
elseif (APPLE)
  target_link_libraries(OpenGLWindow ${COCOA_LIBRARY})
endif ()

if (BUILD_SHARED_LIBS)
  target_link_libraries(OpenGLWindow Bullet3Common)
  if (WIN32 OR APPLE)
   target_link_libraries(OpenGLWindow ${OPENGL_gl_LIBRARY})
  else()
	set (CMAKE_THREAD_PREFER_PTHREAD TRUE)
	FIND_PACKAGE(Threads)
	target_link_libraries(OpenGLWindow ${CMAKE_THREAD_LIBS_INIT})
  endif()
endif()

#target_link_libraries(OpenGLWindow ${OPENGL_gl_LIBRARY})

INSTALL(TARGETS OpenGLWindow
        RUNTIME DESTINATION bin
        LIBRARY DESTINATION lib${LIB_SUFFIX}
        ARCHIVE DESTINATION lib${LIB_SUFFIX})
