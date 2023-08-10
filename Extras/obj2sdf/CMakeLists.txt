
SET(includes
  .
	${PROJECT_ROOT_DIR}/examples/ThirdPartyLibs
	${PROJECT_ROOT_DIR}/src
)

LINK_LIBRARIES(
	 Bullet3Common
)

INCLUDE_DIRECTORIES(${includes})

ADD_EXECUTABLE(App_obj2sdf 
		obj2sdf.cpp
		../../examples/Utils/b3ResourcePath.cpp
		../../examples/Utils/b3ResourcePath.h
		../../examples/ThirdPartyLibs/Wavefront/tiny_obj_loader.cpp
)

