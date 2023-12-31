# App_BasicExample is a minimal sample creating, stepping and deleting a Bullet dynamics world

INCLUDE_DIRECTORIES(
${PROJECT_ROOT_DIR}/src
)

LINK_LIBRARIES(
 BulletDynamics BulletCollision LinearMath
)

IF (WIN32)
	ADD_EXECUTABLE(App_BasicExample
		BasicExample.cpp
		main.cpp
		${PROJECT_ROOT_DIR}/build3/bullet.rc
	)
ELSE()
	ADD_EXECUTABLE(App_BasicExample
		BasicExample.cpp
		main.cpp
	)
ENDIF()




IF (INTERNAL_ADD_POSTFIX_EXECUTABLE_NAMES)
			SET_TARGET_PROPERTIES(App_BasicExample PROPERTIES  DEBUG_POSTFIX "_Debug")
			SET_TARGET_PROPERTIES(App_BasicExample PROPERTIES  MINSIZEREL_POSTFIX "_MinsizeRel")
			SET_TARGET_PROPERTIES(App_BasicExample PROPERTIES  RELWITHDEBINFO_POSTFIX "_RelWithDebugInfo")
ENDIF(INTERNAL_ADD_POSTFIX_EXECUTABLE_NAMES)





#################
# Standalone BasicExampleGui using OpenGL (but not the example browser)


INCLUDE_DIRECTORIES(
        ${PROJECT_ROOT_DIR}/src
        ${PROJECT_ROOT_DIR}/btgui
        ${PROJECT_ROOT_DIR}/examples
        ${PROJECT_ROOT_DIR}/examples/ThirdPartyLibs/glad
)


SET(AppBasicExampleGui_SRCS
	BasicExample.cpp
        ${PROJECT_ROOT_DIR}/build3/bullet.rc
	../StandaloneMain/main_opengl_single_example.cpp
        ../ExampleBrowser/OpenGLGuiHelper.cpp
        ../ExampleBrowser/GL_ShapeDrawer.cpp
        ../ExampleBrowser/CollisionShape2TriangleMesh.cpp
		../Utils/b3Clock.cpp
)

#this define maps StandaloneExampleCreateFunc to the right 'CreateFunc'
ADD_DEFINITIONS(-DB3_USE_STANDALONE_EXAMPLE)

LINK_LIBRARIES(
        BulletDynamics BulletCollision LinearMath OpenGLWindow Bullet3Common 
)

#some code to support OpenGL and Glew cross platform
IF (WIN32)
        INCLUDE_DIRECTORIES(
             ${PROJECT_ROOT_DIR}/btgui/OpenGLWindow/GlewWindows
        )
        ADD_DEFINITIONS(-DGLEW_STATIC)
	LINK_LIBRARIES( ${OPENGL_gl_LIBRARY} ${OPENGL_glu_LIBRARY} )
ELSE(WIN32)
        IF(APPLE)
                find_library(COCOA NAMES Cocoa)
                MESSAGE(${COCOA})
                link_libraries(${COCOA} ${OPENGL_gl_LIBRARY} ${OPENGL_glu_LIBRARY})

        ELSE(APPLE)
                INCLUDE_DIRECTORIES(
                        ${PROJECT_ROOT_DIR}/btgui/OpenGLWindow/GlewWindows
                )
                ADD_DEFINITIONS("-DGLEW_INIT_OPENGL11_FUNCTIONS=1")
                ADD_DEFINITIONS("-DGLEW_STATIC")
                ADD_DEFINITIONS("-DGLEW_DYNAMIC_LOAD_ALL_GLX_FUNCTIONS=1")

                FIND_PACKAGE(Threads)
                LINK_LIBRARIES( ${CMAKE_THREAD_LIBS_INIT} ${DL} )
        ENDIF(APPLE)
ENDIF(WIN32)


ADD_EXECUTABLE(AppBasicExampleGui
                ${AppBasicExampleGui_SRCS}
)


IF (INTERNAL_ADD_POSTFIX_EXECUTABLE_NAMES)
                        SET_TARGET_PROPERTIES(AppBasicExampleGui PROPERTIES  DEBUG_POSTFIX "_Debug")
                        SET_TARGET_PROPERTIES(AppBasicExampleGui PROPERTIES  MINSIZEREL_POSTFIX "_MinsizeRel")
                        SET_TARGET_PROPERTIES(AppBasicExampleGui PROPERTIES  RELWITHDEBINFO_POSTFIX "_RelWithDebugInfo")
ENDIF(INTERNAL_ADD_POSTFIX_EXECUTABLE_NAMES)


