# Copyright (c) 2011 CNRS
# Author: Florent Lamiraux
#
# This file is part of hpp-wholebody-step-corba.
# hpp-wholebody-step-corba is free software: you can redistribute it
# and/or modify it under the terms of the GNU Lesser General Public
# License as published by the Free Software Foundation, either version
# 3 of the License, or (at your option) any later version.
#
# hpp-wholebody-step-corba is distributed in the hope that it will be
# useful, but WITHOUT ANY WARRANTY; without even the implied warranty
# of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU
# General Lesser Public License for more details.  You should have
# received a copy of the GNU Lesser General Public License along with
# hpp-wholebody-step-corba.  If not, see
# <http://www.gnu.org/licenses/>.

# Requires at least CMake 2.6 to configure the package.
CMAKE_MINIMUM_REQUIRED(VERSION 2.6)

SET(CXX_DISABLE_WERROR true)

INCLUDE(cmake/base.cmake)
INCLUDE(cmake/hpp.cmake)
INCLUDE(cmake/cpack.cmake)
INCLUDE(cmake/idl.cmake)
INCLUDE(cmake/python.cmake)

SET(PROJECT_NAME hpp-wholebody-step-corba)
SET(PROJECT_DESCRIPTION "Corba server and python client for hpp-wholebody-step")
SET(PROJECT_URL "http://github.com/laas/hpp-wholebody-step-corba")

SET(CLIENT_ONLY FALSE CACHE BOOL "Set to true to install the client only")

SET(${PROJECT_NAME}_HEADERS
  include/hpp/corbaserver/wholebody-step/server.hh
  include/hpp/corbaserver/wholebody-step/fwd.hh
)
SET(CUSTOM_HEADER_DIR hpp/corbaserver/wholebody-step)

SETUP_HPP_PROJECT()

# Search for dependencies.
ADD_REQUIRED_DEPENDENCY("omniORB4 >= 4.1.4")
ADD_REQUIRED_DEPENDENCY("hpp-corbaserver >= 4")
IF (NOT CLIENT_ONLY)
  ADD_DOC_DEPENDENCY ("hpp-core >= 3")
  ADD_REQUIRED_DEPENDENCY("hpp-wholebody-step >= 4")
  ADD_REQUIRED_DEPENDENCY("hpp-template-corba >= 1.1")
  ADD_REQUIRED_DEPENDENCY("hpp-util >= 3.2")
ENDIF (NOT CLIENT_ONLY)

# Make sure that python submodule is installed in the same prefix as module.
IF (NOT ${HPP_CORBASERVER_PREFIX} STREQUAL ${CMAKE_INSTALL_PREFIX})
  MESSAGE(WARNING
    "  THIS PACKAGE SHOULD BE INSTALLED IN THE SAME PREFIX AS HPP-CORBASERVER,\n"
    "  SINCE IT INSTALLS A PYTHON SUBMODULE OF HPP.CORBASERVER.")
  MESSAGE("CMAKE_INSTALL_PREFIX=${CMAKE_INSTALL_PREFIX}")
  MESSAGE("HPP_CORBASERVER_PREFIX=${HPP_CORBASERVER_PREFIX}")
ENDIF()

ADD_SUBDIRECTORY(src)

CONFIG_FILES (include/hpp/corbaserver/wholebody-step/doc.hh)

PKG_CONFIG_APPEND_LIBS(${PROJECT_NAME})

SETUP_HPP_PROJECT_FINALIZE()
SETUP_PROJECT_CPACK()
