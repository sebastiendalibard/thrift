#
# Licensed to the Apache Software Foundation (ASF) under one
# or more contributor license agreements. See the NOTICE file
# distributed with this work for additional information
# regarding copyright ownership. The ASF licenses this file
# to you under the Apache License, Version 2.0 (the
# "License"); you may not use this file except in compliance
# with the License. You may obtain a copy of the License at
#
#   http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing,
# software distributed under the License is distributed on an
# "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
# KIND, either express or implied. See the License for the
# specific language governing permissions and limitations
# under the License.
#

moc_%.cpp: %.h
	$(QT_MOC) $(QT_CFLAGS) $< -o $@

SUBDIRS = .

if WITH_TESTS
SUBDIRS += test
endif

pkgconfigdir = $(libdir)/pkgconfig

lib_LTLIBRARIES = libthrift.la
pkgconfig_DATA = thrift.pc
libthrift_la_LDFLAGS = -release $(VERSION) $(BOOST_LDFLAGS)

## We only build the extra libraries if we have the dependencies,
## but we install all of the headers unconditionally.
if AMX_HAVE_LIBEVENT
lib_LTLIBRARIES += libthriftnb.la
pkgconfig_DATA += thrift-nb.pc
endif
if AMX_HAVE_ZLIB
lib_LTLIBRARIES += libthriftz.la
pkgconfig_DATA += thrift-z.pc
endif
if AMX_HAVE_QT
lib_LTLIBRARIES += libthriftqt.la
pkgconfig_DATA += thrift-qt.pc
endif

AM_CXXFLAGS = -Wall
AM_CPPFLAGS = $(BOOST_CPPFLAGS) -I$(srcdir)/src -I$(srcdir)/src/thrift
AM_LDFLAGS = $(BOOST_LDFLAGS)

# Define the source files for the module

libthrift_la_SOURCES = src/thrift/Thrift.cpp \
                       src/thrift/TApplicationException.cpp \
                       src/thrift/VirtualProfiling.cpp \
                       src/thrift/concurrency/ThreadManager.cpp \
                       src/thrift/concurrency/TimerManager.cpp \
                       src/thrift/concurrency/Util.cpp \
                       src/thrift/protocol/TDebugProtocol.cpp \
                       src/thrift/protocol/TDenseProtocol.cpp \
                       src/thrift/protocol/TJSONProtocol.cpp \
                       src/thrift/protocol/TBase64Utils.cpp \
                       src/thrift/transport/TTransportException.cpp \
                       src/thrift/transport/TFDTransport.cpp \
                       src/thrift/transport/TFileTransport.cpp \
                       src/thrift/transport/TSimpleFileTransport.cpp \
                       src/thrift/transport/THttpTransport.cpp \
                       src/thrift/transport/THttpClient.cpp \
                       src/thrift/transport/THttpServer.cpp \
                       src/thrift/transport/TSocket.cpp \
                       src/thrift/transport/TSSLSocket.cpp \
                       src/thrift/transport/TSocketPool.cpp \
                       src/thrift/transport/TServerSocket.cpp \
                       src/thrift/transport/TSSLServerSocket.cpp \
                       src/thrift/transport/TTransportUtils.cpp \
                       src/thrift/transport/TBufferTransports.cpp \
                       src/thrift/server/TServer.cpp \
                       src/thrift/server/TSimpleServer.cpp \
                       src/thrift/server/TThreadPoolServer.cpp \
                       src/thrift/server/TThreadedServer.cpp \
                       src/thrift/async/TAsyncChannel.cpp \
                       src/thrift/processor/PeekProcessor.cpp

if WITH_BOOSTTHREADS
libthrift_la_SOURCES += src/thrift/concurrency/BoostThreadFactory.cpp \
                        src/thrift/concurrency/BoostMonitor.cpp \
                        src/thrift/concurrency/BoostMutex.cpp
else
libthrift_la_SOURCES += src/thrift/concurrency/Mutex.cpp \
                        src/thrift/concurrency/Monitor.cpp \
                        src/thrift/concurrency/PosixThreadFactory.cpp
endif

libthriftnb_la_SOURCES = src/thrift/server/TNonblockingServer.cpp \
                         src/thrift/async/TAsyncProtocolProcessor.cpp \
                         src/thrift/async/TEvhttpServer.cpp \
                         src/thrift/async/TEvhttpClientChannel.cpp 

libthriftz_la_SOURCES = src/thrift/transport/TZlibTransport.cpp

libthriftqt_la_MOC = src/thrift/qt/moc_TQTcpServer.cpp
libthriftqt_la_SOURCES = $(libthriftqt_la_MOC) \
                         src/thrift/qt/TQIODeviceTransport.cpp \
                         src/thrift/qt/TQTcpServer.cpp
CLEANFILES = $(libthriftqt_la_MOC)

# Flags for the various libraries
libthriftnb_la_CPPFLAGS = $(AM_CPPFLAGS) $(LIBEVENT_CPPFLAGS)
libthriftz_la_CPPFLAGS  = $(AM_CPPFLAGS) $(ZLIB_CPPFLAGS)
libthriftqt_la_CPPFLAGS = $(AM_CPPFLAGS) $(QT_CFLAGS)
libthriftnb_la_CXXFLAGS = $(AM_CXXFLAGS)
libthriftz_la_CXXFLAGS  = $(AM_CXXFLAGS)
libthriftqt_la_CXXFLAGS  = $(AM_CXXFLAGS)
libthriftnb_la_LIBADD = $(LIBEVENT_LIBS)
libthriftnb_la_LDFLAGS  = -release $(VERSION) $(BOOST_LDFLAGS)
libthriftz_la_LDFLAGS   = -release $(VERSION) $(BOOST_LDFLAGS)
libthriftqt_la_LDFLAGS   = -release $(VERSION) $(BOOST_LDFLAGS) $(QT_LIBS)

include_thriftdir = $(includedir)/thrift
include_thrift_HEADERS = \
                         $(top_builddir)/config.h \
                         src/thrift/TDispatchProcessor.h \
                         src/thrift/Thrift.h \
                         src/thrift/TReflectionLocal.h \
                         src/thrift/TProcessor.h \
                         src/thrift/TApplicationException.h \
                         src/thrift/TLogging.h

include_concurrencydir = $(include_thriftdir)/concurrency
include_concurrency_HEADERS = \
                         src/thrift/concurrency/BoostThreadFactory.h \
                         src/thrift/concurrency/Exception.h \
                         src/thrift/concurrency/Mutex.h \
                         src/thrift/concurrency/Monitor.h \
                         src/thrift/concurrency/PlatformThreadFactory.h \
                         src/thrift/concurrency/PosixThreadFactory.h \
                         src/thrift/concurrency/Thread.h \
                         src/thrift/concurrency/ThreadManager.h \
                         src/thrift/concurrency/TimerManager.h \
                         src/thrift/concurrency/FunctionRunner.h \
                         src/thrift/concurrency/Util.h

include_protocoldir = $(include_thriftdir)/protocol
include_protocol_HEADERS = \
                         src/thrift/protocol/TBinaryProtocol.h \
                         src/thrift/protocol/TBinaryProtocol.tcc \
                         src/thrift/protocol/TCompactProtocol.h \
                         src/thrift/protocol/TCompactProtocol.tcc \
                         src/thrift/protocol/TDenseProtocol.h \
                         src/thrift/protocol/TDebugProtocol.h \
                         src/thrift/protocol/TBase64Utils.h \
                         src/thrift/protocol/TJSONProtocol.h \
                         src/thrift/protocol/TProtocolTap.h \
                         src/thrift/protocol/TProtocolException.h \
                         src/thrift/protocol/TVirtualProtocol.h \
                         src/thrift/protocol/TProtocol.h

include_transportdir = $(include_thriftdir)/transport
include_transport_HEADERS = \
                         src/thrift/transport/TFDTransport.h \
                         src/thrift/transport/TFileTransport.h \
                         src/thrift/transport/TSimpleFileTransport.h \
                         src/thrift/transport/TServerSocket.h \
                         src/thrift/transport/TSSLServerSocket.h \
                         src/thrift/transport/TServerTransport.h \
                         src/thrift/transport/THttpTransport.h \
                         src/thrift/transport/THttpClient.h \
                         src/thrift/transport/THttpServer.h \
                         src/thrift/transport/TSocket.h \
                         src/thrift/transport/TSSLSocket.h \
                         src/thrift/transport/TSocketPool.h \
                         src/thrift/transport/TVirtualTransport.h \
                         src/thrift/transport/TTransport.h \
                         src/thrift/transport/TTransportException.h \
                         src/thrift/transport/TTransportUtils.h \
                         src/thrift/transport/TBufferTransports.h \
                         src/thrift/transport/TShortReadTransport.h \
                         src/thrift/transport/TZlibTransport.h

include_serverdir = $(include_thriftdir)/server
include_server_HEADERS = \
                         src/thrift/server/TServer.h \
                         src/thrift/server/TSimpleServer.h \
                         src/thrift/server/TThreadPoolServer.h \
                         src/thrift/server/TThreadedServer.h \
                         src/thrift/server/TNonblockingServer.h

include_processordir = $(include_thriftdir)/processor
include_processor_HEADERS = \
                         src/thrift/processor/PeekProcessor.h \
                         src/thrift/processor/StatsProcessor.h

include_asyncdir = $(include_thriftdir)/async
include_async_HEADERS = \
                     src/thrift/async/TAsyncChannel.h \
                     src/thrift/async/TAsyncDispatchProcessor.h \
                     src/thrift/async/TAsyncProcessor.h \
                     src/thrift/async/TAsyncBufferProcessor.h \
                     src/thrift/async/TAsyncProtocolProcessor.h \
                     src/thrift/async/TEvhttpClientChannel.h \
                     src/thrift/async/TEvhttpServer.h

include_qtdir = $(include_thriftdir)/qt
include_qt_HEADERS = \
                  src/thrift/qt/TQIODeviceTransport.h \
                  src/thrift/qt/TQTcpServer.h

THRIFT = $(top_builddir)/compiler/cpp/thrift

WINDOWS_DIST = \
             README_WINDOWS \
             src/thrift/windows \
             thrift.sln \
             libthrift.vcxproj \
             libthrift.vcxproj.filters \
             libthriftnb.vcxproj \
             libthriftnb.vcxproj.filters

EXTRA_DIST = \
             README \
             README.SSL \
             thrift-nb.pc.in \
             thrift.pc.in \
             thrift-z.pc.in \
             thrift-qt.pc.in \
             $(WINDOWS_DIST)
