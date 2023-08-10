INCLUDE_DIRECTORIES( ${PROJECT_ROOT_DIR}/src
${PROJECT_ROOT_DIR}/src
${PROJECT_ROOT_DIR}/Extras/Serialize/BulletFileLoader
${PROJECT_ROOT_DIR}/Extras/Serialize/BlenderSerialize
)

ADD_LIBRARY(BlenderSerialize dna249.cpp dna249-64bit.cpp bBlenderFile.cpp  bBlenderFile.h bMain.cpp bMain.h )
