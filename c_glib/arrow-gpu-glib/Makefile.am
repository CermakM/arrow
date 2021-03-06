# Licensed to the Apache Software Foundation (ASF) under one
# or more contributor license agreements.  See the NOTICE file
# distributed with this work for additional information
# regarding copyright ownership.  The ASF licenses this file
# to you under the Apache License, Version 2.0 (the
# "License"); you may not use this file except in compliance
# with the License.  You may obtain a copy of the License at
#
#   http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing,
# software distributed under the License is distributed on an
# "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
# KIND, either express or implied.  See the License for the
# specific language governing permissions and limitations
# under the License.

CLEANFILES =

EXTRA_DIST =					\
	meson.build

AM_CPPFLAGS =					\
	-I$(top_builddir)			\
	-I$(top_srcdir)

if HAVE_ARROW_GPU
lib_LTLIBRARIES =				\
	libarrow-gpu-glib.la

libarrow_gpu_glib_la_CXXFLAGS =			\
	$(GLIB_CFLAGS)				\
	$(ARROW_CFLAGS)				\
	$(ARROW_GPU_CFLAGS)			\
	$(GARROW_CXXFLAGS)

libarrow_gpu_glib_la_LIBADD =			\
	$(GLIB_LIBS)				\
	$(ARROW_LIBS)				\
	$(ARROW_GPU_LIBS)			\
	../arrow-glib/libarrow-glib.la

libarrow_gpu_glib_la_headers =			\
	arrow-gpu-glib.h			\
	cuda.h

libarrow_gpu_glib_la_sources =			\
	cuda.cpp				\
	$(libarrow_gpu_glib_la_headers)

libarrow_gpu_glib_la_cpp_headers =		\
	arrow-gpu-glib.hpp			\
	cuda.hpp

libarrow_gpu_glib_la_SOURCES =			\
	$(libarrow_gpu_glib_la_sources)		\
	$(libarrow_gpu_glib_la_cpp_headers)

arrow_gpu_glib_includedir =			\
	$(includedir)/arrow-gpu-glib
arrow_gpu_glib_include_HEADERS =		\
	$(libarrow_gpu_glib_la_headers)		\
	$(libarrow_gpu_glib_la_cpp_headers)

pkgconfigdir = $(libdir)/pkgconfig
pkgconfig_DATA =				\
	arrow-gpu-glib.pc

if HAVE_INTROSPECTION
-include $(INTROSPECTION_MAKEFILE)
INTROSPECTION_GIRS =
INTROSPECTION_SCANNER_ARGS =
INTROSPECTION_SCANNER_ENV =
if USE_ARROW_BUILD_DIR
INTROSPECTION_SCANNER_ENV +=			\
	PKG_CONFIG_PATH=${abs_builddir}/../arrow-glib:$(ARROW_BUILD_DIR)/src/arrow:$${PKG_CONFIG_PATH}
else
INTROSPECTION_SCANNER_ENV +=			\
	PKG_CONFIG_PATH=${abs_builddir}/../arrow-glib:$${PKG_CONFIG_PATH}
endif
INTROSPECTION_COMPILER_ARGS =			\
	--includedir=$(abs_builddir)/../arrow-glib

ArrowGPU-1.0.gir: libarrow-gpu-glib.la
ArrowGPU_1_0_gir_PACKAGES =			\
	arrow-glib
ArrowGPU_1_0_gir_EXPORT_PACKAGES =		\
	arrow-gpu-glib
ArrowGPU_1_0_gir_INCLUDES =			\
	Arrow-1.0
ArrowGPU_1_0_gir_CFLAGS =			\
	$(AM_CPPFLAGS)
ArrowGPU_1_0_gir_LIBS =
ArrowGPU_1_0_gir_FILES =			\
	$(libarrow_gpu_glib_la_sources)
ArrowGPU_1_0_gir_SCANNERFLAGS =					\
	--library-path=$(ARROW_LIB_DIR)				\
	--warn-all						\
	--add-include-path=$(abs_builddir)/../arrow-glib	\
	--identifier-prefix=GArrowGPU				\
	--symbol-prefix=garrow_gpu
if OS_MACOS
ArrowGPU_1_0_gir_LIBS +=			\
	 arrow-glib				\
	 arrow-gpu-glib
ArrowGPU_1_0_gir_SCANNERFLAGS +=				\
	--no-libtool						\
	--library-path=$(abs_builddir)/../arrow-glib/.libs	\
	--library-path=$(abs_builddir)/.libs
else
ArrowGPU_1_0_gir_LIBS +=				\
	$(abs_builddir)/../arrow-glib/libarrow-glib.la	\
	libarrow-gpu-glib.la
endif

INTROSPECTION_GIRS += ArrowGPU-1.0.gir

girdir = $(datadir)/gir-1.0
gir_DATA = $(INTROSPECTION_GIRS)

typelibdir = $(libdir)/girepository-1.0
typelib_DATA = $(INTROSPECTION_GIRS:.gir=.typelib)

CLEANFILES +=					\
	$(gir_DATA)				\
	$(typelib_DATA)
endif
endif
