cmake_minimum_required(VERSION 3.0)

project(pc_server)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
set(CMAKE_INSTALL_RPATH_USE_LINK_PATH TRUE)

add_definitions("-DWEBRTC_LINUX -DWEBRTC_POSIX -DWEBRTC_NS_FLOAT")

set(WEBRTC_ROOT "/home/ubuntu/webrtc-checkout/src")

include_directories(
  ${WEBRTC_ROOT}
)

set(WEBRTC_SRC_
  base/stringutils.cc
  base/stringencode.cc
  base/checks.cc
  tools/simple_command_line_parser.cc
  )

function(prepend_path var prefix)
   set(listVar "")
   foreach(f ${ARGN})
      list(APPEND listVar "${prefix}/${f}")
   endforeach(f)
   set(${var} "${listVar}" PARENT_SCOPE)
endfunction(prepend_path)

prepend_path(WEBRTC_SRC ${WEBRTC_ROOT}/webrtc ${WEBRTC_SRC_})

set(SRC_LIST
  data_socket.cc
  peer_channel.cc
  utils.cc
  main.cc
)

add_executable(pc_server ${SRC_LIST} ${WEBRTC_SRC})
