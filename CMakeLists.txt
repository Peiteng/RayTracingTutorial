#---------------------------------------------------------------------------------------------------
# CMake Build Configuration for the Ray Tracing Weekend Series
#
# See README.md for guidance.
#---------------------------------------------------------------------------------------------------

cmake_minimum_required ( VERSION 3.1.0 )
set(libs_path C:/MyGLProject/codingLibs/)
set(DLLS_PATH ${CMAKE_CURRENT_SOURCE_DIR})


project ( RTWeekend
  VERSION 3.0.0
  LANGUAGES CXX
)

# Set to c++11
set ( CMAKE_CXX_STANDARD 11 )

# Source
set ( COMMON_ALL
  src/common/rtweekend.h
  src/common/camera.h
  src/common/ray.h
  src/common/vec3.h
)

set ( SOURCE_ONE_WEEKEND
  ${COMMON_ALL}
  src/InOneWeekend/hittable.h
  src/InOneWeekend/hittable_list.h
  src/InOneWeekend/material.h
  src/InOneWeekend/sphere.h
  src/InOneWeekend/main.cc
)

set ( SOURCE_NEXT_WEEK
  ${COMMON_ALL}
  src/common/aabb.h
  src/common/external/stb_image.h
  src/common/perlin.h
  src/common/rtw_stb_image.h
  src/common/texture.h
  src/TheNextWeek/aarect.h
  src/TheNextWeek/box.h
  src/TheNextWeek/bvh.h
  src/TheNextWeek/constant_medium.h
  src/TheNextWeek/hittable.h
  src/TheNextWeek/hittable_list.h
  src/TheNextWeek/material.h
  src/TheNextWeek/moving_sphere.h
  src/TheNextWeek/sphere.h
  src/TheNextWeek/main.cc
)

set ( SOURCE_REST_OF_YOUR_LIFE
  ${COMMON_ALL}
  src/common/aabb.h
  src/common/external/stb_image.h
  src/common/perlin.h
  src/common/rtw_stb_image.h
  src/common/texture.h
  src/TheRestOfYourLife/aarect.h
  src/TheRestOfYourLife/box.h
  src/TheRestOfYourLife/bvh.h
  src/TheRestOfYourLife/hittable.h
  src/TheRestOfYourLife/hittable_list.h
  src/TheRestOfYourLife/material.h
  src/TheRestOfYourLife/onb.h
  src/TheRestOfYourLife/pdf.h
  src/TheRestOfYourLife/sphere.h
  src/TheRestOfYourLife/main.cc
)

# Executables
set ( ALL_PROJECTS
inOneWeekend
theNextWeek
theRestOfYourLife
)


add_executable(inOneWeekend      ${SOURCE_ONE_WEEKEND})
set_target_properties(inOneWeekend PROPERTIES
RUNTIME_OUTPUT_DIRECTORY_DEBUG ${CMAKE_SOURCE_DIR}
RUNTIME_OUTPUT_DIRECTORY_RELEASE ${CMAKE_SOURCE_DIR})


add_executable(theNextWeek       ${SOURCE_NEXT_WEEK})
add_executable(theRestOfYourLife ${SOURCE_REST_OF_YOUR_LIFE})
add_executable(cos_cubed         src/TheRestOfYourLife/cos_cubed.cc         ${COMMON_ALL})
add_executable(cos_density       src/TheRestOfYourLife/cos_density.cc       ${COMMON_ALL})
add_executable(integrate_x_sq    src/TheRestOfYourLife/integrate_x_sq.cc    ${COMMON_ALL})
add_executable(pi                src/TheRestOfYourLife/pi.cc                ${COMMON_ALL})
add_executable(sphere_importance src/TheRestOfYourLife/sphere_importance.cc ${COMMON_ALL})
add_executable(sphere_plot       src/TheRestOfYourLife/sphere_plot.cc       ${COMMON_ALL})

include_directories(src/common)




foreach(X_NAME IN LISTS ALL_PROJECTS)
    message(STATUS "process project: ${X_NAME}")

  #----------------
  #add OPENCV
  set(OpenCV_CMake_PATH ${libs_path}/opencv440/build/ 
  CACHE PATH "Path to OpenCV directory containing 'OpenCVConfig.cmake' file")
  message("check opencv path:" ${OpenCV_CMake_PATH})
  if(EXISTS ${OpenCV_CMake_PATH})
  find_package(OpenCV REQUIRED HINTS ${OpenCV_CMake_PATH})
      set(OpenCV_INCLUDES ${OpenCV_CMake_PATH}/include/)
      set(OpenCV_LIBS debug ${OpenCV_CMake_PATH}/x64/vc15/lib/opencv_world440d.lib optimized ${OpenCV_CMake_PATH}/x64/vc15/lib/opencv_world440.lib) 
      #copy dll file
      set(opencv_release_dlls
              ${OpenCV_CMake_PATH}/x64/vc15/bin/opencv_world440.dll
              ${OpenCV_CMake_PATH}/x64/vc15/bin/opencv_world440d.dll)
      file(COPY ${opencv_release_dlls} DESTINATION ${DLLS_PATH})
  else()
      message("OpenCV 4.4.0 not found!")
  endif()
  target_include_directories(${X_NAME} PUBLIC   
  "${OpenCV_INCLUDES}")

  target_link_libraries(${X_NAME} ${OpenCV_LIBS})
endforeach() 


