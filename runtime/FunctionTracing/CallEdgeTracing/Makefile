##===- runtime/libprofile/Makefile -------------------------*- Makefile -*-===##
#
#                     The LLVM Compiler Infrastructure
#
# This file is distributed under the University of Illinois Open Source
# License. See LICENSE.TXT for details.
#
##===----------------------------------------------------------------------===##

LEVEL = ../../..

LIBRARYNAME = call_edge_tracing_rt
LINK_LIBS_IN_SHARED = 1
SHARED_LIBRARY = 1

# Build and install this archive.                                                                                                                  
BUILD_ARCHIVE = 1
override NO_INSTALL_ARCHIVES =
#CPPFLAGS=-std=c++11 -stdlib=libstdc++
CPPFLAGS=-I $(HOME)/usr/local/include -fexceptions -stdlib=libstdc++

#CFLAGS=-std=c+11 -stdlib=libc++
#ifeq ($(OS),Darwin)
#LDFLAGS=-lc++
#endif

include $(LEVEL)/Makefile.common

