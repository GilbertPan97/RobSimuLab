cmake_minimum_required(VERSION 2.4)

IF(COMMAND cmake_policy)
   cmake_policy(SET CMP0003 NEW)
ENDIF(COMMAND cmake_policy)

INCLUDE_DIRECTORIES(${PROJECT_ROOT_DIR}/src )

#FILE(GLOB INC_FILES ../*.h)

SET (INC_FILES
	DNA_rigidbody.h

	${PROJECT_ROOT_DIR}/src/LinearMath/btVector3.h
	${PROJECT_ROOT_DIR}/src/LinearMath/btMatrix3x3.h
	${PROJECT_ROOT_DIR}/src/LinearMath/btTransform.h
	${PROJECT_ROOT_DIR}/src/BulletCollision/CollisionShapes/btCollisionShape.h
	${PROJECT_ROOT_DIR}/src/BulletCollision/CollisionShapes/btConvexInternalShape.h
	${PROJECT_ROOT_DIR}/src/BulletCollision/CollisionDispatch/btCollisionObject.h
)

# Build makesdna executable
SET(SRC makesdna.cpp)
ADD_EXECUTABLE(makesdna ${SRC} ${INC_FILES})

IF (CMAKE_CL_64)
	# Output BulletDNA.c
	ADD_CUSTOM_COMMAND(
		OUTPUT ${PROJECT_ROOT_DIR}/src/LinearMath/btSerializer64.cpp
		COMMAND ${CMAKE_CFG_INTDIR}/makesdna ${PROJECT_ROOT_DIR}/src/LinearMath/btSerializer64.cpp ${PROJECT_ROOT_DIR}/Extras/Serialize/CommonSerialize/
		DEPENDS makesdna
	)
SET(SRC  ${PROJECT_ROOT_DIR}/src/LinearMath/btSerializer64.cpp)

ELSE()
	# Output BulletDNA.c
	ADD_CUSTOM_COMMAND(
		OUTPUT ${PROJECT_ROOT_DIR}/src/LinearMath/btSerializer.cpp
		COMMAND ${CMAKE_CFG_INTDIR}/makesdna ${PROJECT_ROOT_DIR}/src/LinearMath/btSerializer.cpp ${PROJECT_ROOT_DIR}/Extras/Serialize/CommonSerialize/
		DEPENDS makesdna
	)
SET(SRC  ${PROJECT_ROOT_DIR}/src/LinearMath/btSerializer.cpp)

ENDIF()

# Build bf_dna library


ADD_LIBRARY(BulletDNA ${SRC} ${INC_FILES})

MESSAGE(STATUS "Configuring makesdna")
