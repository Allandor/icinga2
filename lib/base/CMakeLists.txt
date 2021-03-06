# Icinga 2
# Copyright (C) 2012-2018 Icinga Development Team (https://www.icinga.com/)
#
# This program is free software; you can redistribute it and/or
# modify it under the terms of the GNU General Public License
# as published by the Free Software Foundation; either version 2
# of the License, or (at your option) any later version.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program; if not, write to the Free Software Foundation
# Inc., 51 Franklin St, Fifth Floor, Boston, MA 02110-1301, USA.

mkclass_target(application.ti application-ti.cpp application-ti.hpp)
mkclass_target(configobject.ti configobject-ti.cpp configobject-ti.hpp)
mkclass_target(datetime.ti datetime-ti.cpp datetime-ti.hpp)
mkclass_target(filelogger.ti filelogger-ti.cpp filelogger-ti.hpp)
mkclass_target(function.ti function-ti.cpp function-ti.hpp)
mkclass_target(logger.ti logger-ti.cpp logger-ti.hpp)
mkclass_target(perfdatavalue.ti perfdatavalue-ti.cpp perfdatavalue-ti.hpp)
mkclass_target(streamlogger.ti streamlogger-ti.cpp streamlogger-ti.hpp)
mkclass_target(sysloglogger.ti sysloglogger-ti.cpp sysloglogger-ti.hpp)

set(base_SOURCES
  i2-base.hpp
  application.cpp application.hpp application-ti.hpp application-version.cpp
  array.cpp array.hpp array-script.cpp
  base64.cpp base64.hpp
  boolean.cpp boolean.hpp boolean-script.cpp
  configobject.cpp configobject.hpp configobject-ti.hpp configobject-script.cpp
  configtype.cpp configtype.hpp
  configwriter.cpp configwriter.hpp
  console.cpp console.hpp
  context.cpp context.hpp
  convert.cpp convert.hpp
  datetime.cpp datetime.hpp datetime-ti.hpp datetime-script.cpp
  debug.hpp
  debuginfo.cpp debuginfo.hpp
  dependencygraph.cpp dependencygraph.hpp
  dictionary.cpp dictionary.hpp dictionary-script.cpp
  exception.cpp exception.hpp
  fifo.cpp fifo.hpp
  filelogger.cpp filelogger.hpp filelogger-ti.hpp
  function.cpp function.hpp function-ti.hpp function-script.cpp functionwrapper.hpp
  initialize.cpp initialize.hpp
  json.cpp json.hpp json-script.cpp
  library.cpp library.hpp
  loader.cpp loader.hpp
  logger.cpp logger.hpp logger-ti.hpp
  math-script.cpp
  netstring.cpp netstring.hpp
  networkstream.cpp networkstream.hpp
  number.cpp number.hpp number-script.cpp
  object.cpp object.hpp object-script.cpp
  objectlock.cpp objectlock.hpp
  objecttype.cpp objecttype.hpp
  perfdatavalue.cpp perfdatavalue.hpp perfdatavalue-ti.hpp
  primitivetype.cpp primitivetype.hpp
  process.cpp process.hpp
  registry.hpp
  ringbuffer.cpp ringbuffer.hpp
  scriptframe.cpp scriptframe.hpp
  scriptglobal.cpp scriptglobal.hpp
  scriptutils.cpp scriptutils.hpp
  serializer.cpp serializer.hpp
  singleton.hpp
  socket.cpp socket.hpp
  socketevents.cpp socketevents-epoll.cpp socketevents-poll.cpp socketevents.hpp
  stacktrace.cpp stacktrace.hpp
  statsfunction.hpp
  stdiostream.cpp stdiostream.hpp
  stream.cpp stream.hpp
  streamlogger.cpp streamlogger.hpp streamlogger-ti.hpp
  string.cpp string.hpp string-script.cpp
  sysloglogger.cpp sysloglogger.hpp sysloglogger-ti.hpp
  tcpsocket.cpp tcpsocket.hpp
  threadpool.cpp threadpool.hpp
  timer.cpp timer.hpp
  tlsstream.cpp tlsstream.hpp
  tlsutility.cpp tlsutility.hpp
  type.cpp type.hpp typetype-script.cpp
  unix.hpp
  unixsocket.cpp unixsocket.hpp
  utility.cpp utility.hpp
  value.cpp value.hpp value-operators.cpp
  win32.hpp
  workqueue.cpp workqueue.hpp
)

set_property(SOURCE ${CMAKE_CURRENT_SOURCE_DIR}/application-version.cpp PROPERTY EXCLUDE_UNITY_BUILD TRUE)

if(ICINGA2_UNITY_BUILD)
  mkunity_target(base base base_SOURCES)
endif()

if(HAVE_SYSTEMD)
  find_path(SYSTEMD_INCLUDE_DIR
    NAMES systemd/sd-daemon.h
    HINTS ${SYSTEMD_ROOT_DIR})
  include_directories(${SYSTEMD_INCLUDE_DIR})
endif()

add_library(base OBJECT ${base_SOURCES})

include_directories(${icinga2_SOURCE_DIR}/third-party/execvpe)
link_directories(${icinga2_BINARY_DIR}/third-party/execvpe)

include_directories(${icinga2_SOURCE_DIR}/third-party/mmatch)
link_directories(${icinga2_BINARY_DIR}/third-party/mmatch)

include_directories(${icinga2_SOURCE_DIR}/third-party/socketpair)
link_directories(${icinga2_BINARY_DIR}/third-party/socketpair)

set_target_properties (
  base PROPERTIES
  FOLDER Lib
)

install(CODE "file(MAKE_DIRECTORY \"\$ENV{DESTDIR}${CMAKE_INSTALL_FULL_LOCALSTATEDIR}/cache/icinga2\")")
install(CODE "file(MAKE_DIRECTORY \"\$ENV{DESTDIR}${CMAKE_INSTALL_FULL_LOCALSTATEDIR}/log/icinga2/crash\")")

set(CPACK_NSIS_EXTRA_INSTALL_COMMANDS "${CPACK_NSIS_EXTRA_INSTALL_COMMANDS}" PARENT_SCOPE)
