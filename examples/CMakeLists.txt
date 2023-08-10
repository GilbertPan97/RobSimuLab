
SUBDIRS( ExampleBrowser  RobotSimulator SharedMemory  ThirdPartyLibs/Gwen ThirdPartyLibs/BussIK ThirdPartyLibs/clsocket OpenGLWindow)

IF(BUILD_PYBULLET)
	SUBDIRS(pybullet)
ENDIF(BUILD_PYBULLET)
