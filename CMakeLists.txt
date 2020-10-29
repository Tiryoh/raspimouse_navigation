cmake_minimum_required(VERSION 2.8.3)
project(raspimouse_navigation)

find_package(catkin REQUIRED)

###################################
## catkin specific configuration ##
###################################

catkin_package()

#############
## Install ##
#############

foreach(dir launch param)
  install(DIRECTORY ${dir}/
    DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/${dir}
  )
endforeach(dir)
