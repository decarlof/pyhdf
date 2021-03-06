# $Id: Makefile,v 1.4 2008-06-30 02:41:44 gosselin_a Exp $
# $Log: not supported by cvs2svn $
# Revision 1.3  2005/07/14 01:36:41  gosselin_a
# pyhdf-0.7-3
# Ported to HDF4.2r1.
# Support for SZIP compression on SDS datasets.
# All classes are now 'new-style' classes, deriving from 'object'.
# Update documentation.
#
# Revision 1.2  2004/11/03 16:02:06  gosselin_a
# Change include and lib directories to the Python2.4 distribution
# (previously Python2.3).
#
# Revision 1.1  2004/08/02 15:00:34  gosselin
# Initial revision
#
#
# ------------------------------------------------------------
# SWIG Examples Makefile
#
# This file is used by the examples to build modules.  Assuming
# you ran configure, this file will probably work.  However,
# it's not perfect so you might need to do some hand tweaking.
#
# Other notes:
#
# 1.   Take a look at the prefixes below.   Since SWIG works with
#      multiple target languages, you may need to find out where
#      certain packages have been installed.   Set the prefixes
#      accordingly.
#
# 2.   To use this makefile, simply set SRCS, INTERFACE, INCLUDES, LIBS,
#      TARGET, and do a
#           $(MAKE) -f Makefile.template.in SRCS='$(SRCS)' \
#           INCLUDES='$(INCLUDES) LIBS='$(LIBS)' INTERFACE='$(INTERFACE)' \
#           TARGET='$(TARGET)' method
#
#      'method' describes what is being built.
#---------------------------------------------------------------

# Adjust for the local installation
# See also PYTHON section below
# 
INCLUDES = -I/usr/include/hdf
LIBS       = -L/usr/lib/hdf -lmfhdf -ldf -lz -ljpeg 
#LIBS       = -L/usr/lib/hdf -lmfhdf -ldf -lz -ljpeg -lsz

TARGET     = hdfext
INTERFACE  = hdfext.i

CC         = gcc
CXX        = g++
CFLAGS     =
prefix     = /usr
exec_prefix= ${prefix}
SRCS       = 
SWIGOPT    = 
SWIG       = swig
RUNTIMEDIR = $(exec_prefix)/lib

LIBM       = -lieee -lm
LIBC       = 
LIBCRYPT   = -lcrypt
SYSLIBS    = $(LIBM) $(LIBC) $(LIBCRYPT)

libtool_comp = $(TOP)/../Tools/libtool --mode compile
libtool_link = $(TOP)/../Tools/libtool --mode link

# X11 options

XLIB       = -L/usr/X11R6/lib -lX11
XINCLUDE   = -I/usr/X11R6/include

IWRAP      = $(INTERFACE:.i=_wrap.i)
ISRCS      = $(IWRAP:.i=.c)
ICXXSRCS   = $(IWRAP:.i=.cxx)
IOBJS      = $(IWRAP:.i=.o)

##################################################################
# Dynamic loading for C++
# If you are going to be building dynamic loadable modules in C++,
# you may need to edit this line appropriately.
#
# This line works for g++, but I'm not sure what it might be
# for other C++ compilers
##################################################################

CPP_DLLIBS = #-L/usr/local/lib/gcc-lib/sparc-sun-solaris2.5.1/2.7.2 \
             -L/usr/local/lib -lg++ -lstdc++ -lgcc

# Solaris workshop 5.0 
# CPP_DLLIBS = -L/opt/SUNWspro/lib -lCrun

# Symbols used for using shared libraries
SO=		.so
LDSHARED=	gcc -shared
CCSHARED=	-fpic
CXXSHARED=      gcc -shared

# This is used for building shared libraries with a number of C++
# compilers.   If it doesn't work,  comment it out.
CXXSHARED= g++ -shared 

OBJS      = $(SRCS:.c=.o) $(CXXSRCS:.cxx=.o)

##################################################################
#####                       Tcl/Tk                          ######
##################################################################

# Set these to your local copy of Tcl/Tk.

TCL_INCLUDE = -I/usr/include
TCL_LIB     = -L/usr/lib -ltcl8.3${TCL_DBGX}
TCL_OPTS    = -ldl 
TK_OPTS     = -ltk -ltcl -ldl 

# Extra Tcl specific dynamic linking options
TCL_DLNK   = 

# -----------------------------------------------------------
# Build a new version of the tclsh shell
# -----------------------------------------------------------


tclsh: $(SRCS)
	$(SWIG) -tcl8 $(SWIGOPT) $(TCL_SWIGOPTS) -ltclsh.i $(INTERFACE)
	$(CC) $(CFLAGS) $(SRCS) $(ISRCS) $(INCLUDES) $(TCL_INCLUDE) \
	$(TCL_LIB)  $(TCL_OPTS) $(LIBS) $(SYSLIBS) -o $(TARGET)

tclsh_cpp: $(SRCS)
	$(SWIG) -tcl8 -c++ $(SWIGOPT) $(TCL_SWIGOPTS) -ltclsh.i $(INTERFACE)
	$(CXX) $(CFLAGS) $(SRCS) $(CXXSRCS) $(ICXXSRCS) $(INCLUDES) $(TCL_INCLUDE) \
	$(TCL_LIB) $(TCL_OPTS) $(LIBS) $(SYSLIBS) -o $(TARGET)

# -----------------------------------------------------------
# Build a new copy of wish
# -----------------------------------------------------------

wish: $(SRCS)
	$(SWIG) -tcl8 $(SWIGOPT) $(TCL_SWIGOPTS) -lwish.i $(INTERFACE)
	$(CC) $(CFLAGS) $(SRCS) $(ISRCS) $(INCLUDES) $(TCL_INCLUDE) \
	$(XINCLUDE) $(TCL_LIB) $(TK_OPTS) $(XLIB) $(LIBS) $(SYSLIBS) -o $(TARGET)


wish_cpp: $(SRCS)
	$(SWIG) -tcl8 -c++ $(SWIGOPT) $(TCL_SWIGOPTS) -lwish.i $(INTERFACE)
	$(CXX) $(CFLAGS) $(SRCS) $(CXXSRCS) $(ICXXSRCS) $(INCLUDES) $(TCL_INCLUDE) \
	$(XINCLUDE) $(TCL_LIB) $(TK_OPTS) $(XLIB) $(LIBS) $(SYSLIBS) -o $(TARGET)

# -----------------------------------------------------------
# Build a Tcl dynamic loadable module (you might need to tweak this)
# -----------------------------------------------------------

tcl:  $(SRCS)
	$(SWIG) -tcl8 $(SWIGOPT) $(TCL_SWIGOPTS) $(INTERFACE)
	$(CC) -c $(CCSHARED) $(CFLAGS) $(SRCS) $(ISRCS) $(INCLUDES) $(TCL_INCLUDE)
	$(LDSHARED) $(OBJS) $(IOBJS) $(TCL_DLNK) $(LIBS) -o $(TARGET)$(SO)

# -----------------------------------------------------------
# Build a Tcl7.5 dynamic loadable module for C++
# -----------------------------------------------------------

tcl_cpp: $(SRCS)
	$(SWIG) -tcl8 -c++ $(SWIGOPT) $(TCL_SWIGOPTS) $(INTERFACE)
	$(CXX) -c $(CCSHARED) $(CFLAGS) $(SRCS) $(CXXSRCS) $(ICXXSRCS) $(INCLUDES) $(TCL_INCLUDE)
	$(CXXSHARED) $(OBJS) $(IOBJS) $(TCL_DLNK) $(LIBS) $(CPP_DLLIBS) -o $(TARGET)$(SO)

# -----------------------------------------------------------
# Build a Tcl7.5 dynamic loadable module, linked against SWIG runtime lib
# -----------------------------------------------------------

TCL_RUNTIME=-L$(RUNTIMEDIR) -lswigtcl8

tcl_multi:  $(SRCS)
	$(SWIG) -c -tcl8 $(SWIGOPT) $(TCL_SWIGOPTS) $(INTERFACE)
	$(CC) -c $(CCSHARED) $(CFLAGS) $(SRCS) $(ISRCS) $(INCLUDES) $(TCL_INCLUDE)
	$(LDSHARED) $(OBJS) $(IOBJS) $(TCL_RUNTIME) $(TCL_DLNK) $(LIBS) -o $(TARGET)$(SO)

tcl_multi_cpp: $(SRCS)
	$(SWIG) -c -tcl8 -c++ $(SWIGOPT) $(TCL_SWIGOPTS) $(INTERFACE)
	$(CXX) -c $(CCSHARED) $(CFLAGS) $(SRCS) $(CXXSRCS) $(ICXXSRCS) $(INCLUDES) $(TCL_INCLUDE)
	$(CXXSHARED) $(OBJS) $(IOBJS) $(TCL_RUNTIME) $(TCL_DLNK) $(LIBS) $(CPP_DLLIBS) -o $(TARGET)$(SO)

# -----------------------------------------------------------------
# Cleaning the Tcl examples
# -----------------------------------------------------------------

tcl_clean:
	rm -f *.o *.so *_wrap* *~ .~* core  mytclsh 

##################################################################
#####                       PERL 5                          ######
##################################################################

# You need to set this variable to the Perl5 directory containing the
# files "perl.h", "EXTERN.h" and "XSUB.h".   With Perl5.003, it's
# usually something like /usr/local/lib/perl5/arch-osname/5.003/CORE.

PERL5_INCLUDE= /usr/lib/perl5/5.8.0/i386-linux-thread-multi/CORE

# Extra Perl specific dynamic linking options
PERL5_DLNK   = 

PERL5_CCFLAGS = -D_REENTRANT -D_GNU_SOURCE -fno-strict-aliasing -D_LARGEFILE_SOURCE -D_FILE_OFFSET_BITS=64 -I/usr/include/gdbm

# ----------------------------------------------------------------
# Build a Perl5 dynamically loadable module (C)
# ----------------------------------------------------------------

perl5: $(SRCS)
	$(SWIG) -perl5 $(SWIGOPT) $(INTERFACE)
	$(CC) -c -Dbool=char $(CCSHARED) $(CFLAGS) $(SRCS) $(ISRCS) $(INCLUDES) $(PERL5_CCFLAGS) -I$(PERL5_INCLUDE)
	$(LDSHARED) $(OBJS) $(IOBJS) $(PERL5_DLNK) $(LIBS) -o $(TARGET)$(SO)

# ----------------------------------------------------------------
# Build a Perl5 dynamically loadable module (C++)
# ----------------------------------------------------------------

perl5_cpp: $(SRCS)
	$(SWIG) -perl5 -c++ $(SWIGOPT) $(INTERFACE)
	$(CXX) -c $(CCSHARED) $(CFLAGS) -Dexplicit= $(SRCS) $(CXXSRCS) $(ICXXSRCS) $(INCLUDES) $(PERL5_CCFLAGS) -I$(PERL5_INCLUDE)
	$(CXXSHARED) $(OBJS) $(IOBJS) $(PERL5_DLNK) $(LIBS) $(CPP_DLLIBS) -o $(TARGET)$(SO)

# ----------------------------------------------------------------
# Build a Perl5 dynamically loadable module, linked against SWIG runtime lib
# ----------------------------------------------------------------

PERL5_RUNTIME=-L$(RUNTIMEDIR) -lswigpl

perl5_multi: $(SRCS)
	$(SWIG) -c -perl5 $(SWIGOPT) $(INTERFACE)
	$(CC) -c -Dbool=char $(CCSHARED) $(CFLAGS) $(SRCS) $(ISRCS) $(INCLUDES) $(PERL5_CCFLAGS) -I$(PERL5_INCLUDE)
	$(LDSHARED) $(OBJS) $(IOBJS) $(PERL5_RUNTIME) $(PERL5_DLNK) $(LIBS) -o $(TARGET)$(SO)

perl5_multi_cpp: $(SRCS)
	$(SWIG) -c -perl5 -c++ $(SWIGOPT) $(INTERFACE)
	$(CXX) -c $(CCSHARED) $(CFLAGS) -Dexplicit= $(SRCS) $(CXXSRCS) $(ICXXSRCS) $(INCLUDES) $(PERL5_CCFLAGS) -I$(PERL5_INCLUDE)
	$(CXXSHARED) $(OBJS) $(IOBJS) $(PERL5_RUNTIME) $(PERL5_DLNK) $(LIBS) $(CPP_DLLIBS) -o $(TARGET)$(SO)

# ----------------------------------------------------------------
# Build a module from existing XS C source code.   (ie. from xsubpp).
# ----------------------------------------------------------------
perl5_xs: $(SRCS)
	$(CC) -c $(CCSHARED) $(CFLAGS) $(SRCS) $(INCLUDES) -I$(PERL5_INCLUDE)
	$(LDSHARED) $(OBJS) $(LIBS) -o $(TARGET)$(SO)

# ----------------------------------------------------------------
# Build a statically linked Perl5 executable
# ----------------------------------------------------------------

PERL5_LIB    = -L$(PERL5_INCLUDE) -lperl.so -ldl  $(SYSLIBS)

perl5_static: $(SRCS)
	$(SWIG) -perl5 -static -lperlmain.i $(SWIGOPT) $(INTERFACE)
	$(CC) $(CFLAGS) -Dbool=char $(SRCS) $(ISRCS) $(INCLUDES) -I$(PERL5_INCLUDE) $(PERL5_LIB) $(LIBS) -o $(TARGET)

perl5_static_cpp: $(SRCS)
	$(SWIG) -perl5 -c++ -static -lperlmain.i $(SWIGOPT) $(INTERFACE)
	$(CXX) $(CFLAGS) -Dexplicit= $(SRCS) $(CXXSRCS) $(ICXXSRCS) $(INCLUDES) -I$(PERL5_INCLUDE) $(PERL5_LIB) $(LIBS) -o $(TARGET)

# -----------------------------------------------------------------
# Cleaning the Perl5 examples
# -----------------------------------------------------------------

perl5_clean:
	rm -f *.o *.so *_wrap* *~ .~* core  myperl *.pm

##################################################################
#####                       PYTHON                          ######
##################################################################

# Make sure these locate your Python installation
PYTHON_VER = python2.4
PYTHON_INCLUDE= -DHAVE_CONFIG_H \
                -I/usr/include/${PYTHON_VER} \
                -I/usr/lib/${PYTHON_VER}/config \
                -I/usr/lib/${PYTHON_VER}/site-packages/numpy/core/include
PYTHON_LIB    = /usr/lib/${PYTHON_VER}/config

# Extra Python specific dynamic linking options
PYTHON_DLNK   = 

# ----------------------------------------------------------------
# Build a C dynamically loadable module
# ----------------------------------------------------------------

python: $(SRCS)
	$(SWIG) -python $(SWIGOPT) $(INTERFACE)
	$(CC) -c $(CCSHARED) $(CFLAGS) $(ISRCS) $(SRCS) $(INCLUDES) $(PYTHON_INCLUDE)
	$(LDSHARED) $(OBJS) $(IOBJS) $(PYTHON_DLNK) $(LIBS) -o _$(TARGET)$(SO)

# -----------------------------------------------------------------
# Build a C++ dynamically loadable module
# -----------------------------------------------------------------

python_cpp: $(SRCS)
	$(SWIG) -c++ -python $(SWIGOPT) $(INTERFACE)
	$(CXX) -c $(CCSHARED) $(CFLAGS) $(ICXXSRCS) $(SRCS) $(CXXSRCS) $(INCLUDES) $(PYTHON_INCLUDE)
	$(CXXSHARED) $(OBJS) $(IOBJS) $(PYTHON_DLNK) $(LIBS) $(CPP_DLLIBS) -o _$(TARGET)$(SO)

# -----------------------------------------------------------------
# Build a dynamically loadable module, linked against SWIG Runtime lib
# -----------------------------------------------------------------

PYTHON_RUNTIME=-L$(RUNTIMEDIR) -lswigpy

python_multi: $(SRCS)
	$(SWIG) -c -python $(SWIGOPT) $(INTERFACE)
	$(CC) -c $(CCSHARED) $(CFLAGS) $(ISRCS) $(SRCS) $(INCLUDES) $(PYTHON_INCLUDE)
	$(LDSHARED) $(OBJS) $(IOBJS) $(PYTHON_RUNTIME) $(PYTHON_DLNK) $(LIBS) -o _$(TARGET)$(SO)

python_multi_cpp: $(SRCS)
	$(SWIG) -c -c++ -python $(SWIGOPT) $(INTERFACE)
	$(CXX) -c $(CCSHARED) $(CFLAGS) $(ICXXSRCS) $(SRCS) $(CXXSRCS) $(INCLUDES) $(PYTHON_INCLUDE)
	$(CXXSHARED) $(OBJS) $(IOBJS) $(PYTHON_RUNTIME) $(PYTHON_DLNK) $(LIBS) $(CPP_DLLIBS) -o _$(TARGET)$(SO)


# -----------------------------------------------------------------
# Build statically linked Python interpreter
#
# These should only be used in conjunction with the %include embed.i
# library file
# -----------------------------------------------------------------

#TKINTER = -L/usr/X11R6.3/lib -L/usr/local/compat/lib -ltk4.0 -ltcl7.4 -lX11
TKINTER =
PYTHON_LIBOPTS = -lpython2.2 -ldl  $(TKINTER) $(SYSLIBS)

python_static: $(SRCS)
	$(SWIG) -python -lembed.i $(SWIGOPT) $(INTERFACE)
	$(CC) $(CFLAGS) -Xlinker -export-dynamic $(ISRCS) $(SRCS) $(INCLUDES) \
	$(PYTHON_INCLUDE) $(LIBS) -L$(PYTHON_LIB) $(PYTHON_LIBOPTS) -o $(TARGET)

python_static_cpp: $(SRCS)
	$(SWIG) -c++ -python -lembed.i $(SWIGOPT) $(INTERFACE)
	$(CXX) $(CFLAGS) $(ICXXSRCS) $(SRCS) $(CXXSRCS) $(INCLUDES) \
	$(PYTHON_INCLUDE) $(LIBS)  -L$(PYTHON_LIB) $(PYTHON_LIBOPTS) -o $(TARGET)

# -----------------------------------------------------------------
# Cleaning the python examples
# -----------------------------------------------------------------

python_clean:
	rm -f *.o *.so *_wrap* *~ .~* core  mypython *.pyc

##################################################################
#####                       GUILE                           ######
##################################################################

# Make sure these locate your Guile installation
GUILE_INCLUDE = 
GUILE_LIB     = 

# ----------------------------------------------------------------
# Build a C dynamically loadable module
# ----------------------------------------------------------------

guile: $(SRCS)
	$(SWIG) -guile -Linkage ltdlmod $(SWIGOPT) $(INTERFACE)
	$(CC) -c $(CCSHARED) $(CFLAGS) $(INCLUDES) $(GUILE_INCLUDE) $(ISRCS) $(SRCS)
	$(LDSHARED) $(OBJS) $(IOBJS) $(LIBS) -o lib$(TARGET)$(SO)

# -----------------------------------------------------------------
# Build a C++ dynamically loadable module
# -----------------------------------------------------------------

guile_cpp: $(SRCS)
	$(SWIG) -c++ -guile -Linkage ltdlmod $(SWIGOPT) $(INTERFACE)
	$(CXX) -c $(CCSHARED) $(CFLAGS) $(INCLUDES) $(GUILE_INCLUDE) $(ICXXSRCS) $(SRCS) $(CXXSRCS)
	$(CXXSHARED) $(OBJS) $(IOBJS) $(LIBS) $(CPP_DLLIBS) -o lib$(TARGET)$(SO)

# -----------------------------------------------------------------
# Build a dynamically loadable module with passive linkage
# -----------------------------------------------------------------

guile_passive: $(SRCS)
	$(SWIG) -guile -Linkage passive $(SWIGOPT) $(INTERFACE)
	$(CC) -c $(CCSHARED) $(CFLAGS) $(INCLUDES) $(GUILE_INCLUDE) $(ISRCS) $(SRCS)
	$(LDSHARED) $(OBJS) $(IOBJS) $(LIBS) -o lib$(TARGET)$(SO)

guile_passive_cpp: $(SRCS)
	$(SWIG) -c++ -guile -Linkage passive $(SWIGOPT) $(INTERFACE)
	$(CXX) -c $(CCSHARED) $(CFLAGS) $(INCLUDES) $(GUILE_INCLUDE) $(ICXXSRCS) $(SRCS) $(CXXSRCS)
	$(CXXSHARED) $(OBJS) $(IOBJS) $(LIBS) $(CPP_DLLIBS) -o lib$(TARGET)$(SO)

# -----------------------------------------------------------------
# Build a dynamically loadable module with passive linkage, 
# linked against SWIG runtime lib
# -----------------------------------------------------------------

GUILE_RUNTIME=-L$(RUNTIMEDIR) -lswigguile

guile_passive_multi: $(SRCS)
	$(SWIG) -c -guile -Linkage passive $(SWIGOPT) $(INTERFACE)
	$(CC) -c $(CCSHARED) $(CFLAGS) $(INCLUDES) $(GUILE_INCLUDE) $(ISRCS) $(SRCS)
	$(LDSHARED) $(OBJS) $(IOBJS) $(GUILE_RUNTIME) $(LIBS) -o lib$(TARGET)$(SO)

guile_passive_multi_cpp: $(SRCS)
	$(SWIG) -c -c++ -guile -Linkage passive $(SWIGOPT) $(INTERFACE)
	$(CXX) -c $(CCSHARED) $(CFLAGS) $(INCLUDES) $(GUILE_INCLUDE) $(ICXXSRCS) $(SRCS) $(CXXSRCS)
	$(CXXSHARED) $(OBJS) $(IOBJS) $(GUILE_RUNTIME) $(LIBS) $(CPP_DLLIBS) -o lib$(TARGET)$(SO)

# -----------------------------------------------------------------
# Build statically linked Guile interpreter
# -----------------------------------------------------------------

GUILE_LIBOPTS =  -ldl  $(SYSLIBS)

guile_static: $(SRCS)
	$(SWIG) -guile -lguilemain.i -Linkage ltdlmod $(SWIGOPT) $(INTERFACE)
	$(CC) $(CFLAGS) $(ISRCS) $(SRCS) $(INCLUDES) \
	  -DSWIGINIT="SCM scm_init_$(TARGET)_module(void); scm_init_$(TARGET)_module();" \
	  $(GUILE_INCLUDE) $(LIBS) -L$(GUILE_LIB) $(GUILE_LIBOPTS) -o $(TARGET)-guile

guile_static_cpp: $(SRCS)
	$(SWIG) -c++ -guile -lguilemain.i -Linkage ltdlmod $(SWIGOPT) $(INTERFACE)
	$(CXX) $(CFLAGS) $(ICXXSRCS) $(SRCS) $(CXXSRCS) $(INCLUDES) \
	  -DSWIGINIT="SCM scm_init_$(TARGET)_module(void); scm_init_$(TARGET)_module();" \
	  $(GUILE_INCLUDE) $(LIBS) -L$(GUILE_LIB) $(GUILE_LIBOPTS) -o $(TARGET)-guile

guile_simple: $(SRCS)
	$(SWIG) -guile -lguilemain.i -Linkage simple $(SWIGOPT) $(INTERFACE)
	$(CC) $(CFLAGS) $(ISRCS) $(SRCS) $(INCLUDES) \
	  $(GUILE_INCLUDE) $(LIBS) -L$(GUILE_LIB) $(GUILE_LIBOPTS) -o $(TARGET)-guile

guile_simple_cpp: $(SRCS)
	$(SWIG) -c++ -guile -lguilemain.i -Linkage simple $(SWIGOPT) $(INTERFACE)
	$(CXX) $(CFLAGS) $(ICXXSRCS) $(SRCS) $(CXXSRCS) $(INCLUDES) \
	  $(GUILE_INCLUDE) $(LIBS) -L$(GUILE_LIB) $(GUILE_LIBOPTS) -o $(TARGET)-guile

# -----------------------------------------------------------------
# Cleaning the Guile examples
# -----------------------------------------------------------------

guile_clean:
	rm -f *.o *.so *_wrap* *~ .~* core  my-guile $(TARGET)

##################################################################
#####                       JAVA                            ######
##################################################################

# You need to set this variable to the java directories containing the
# files "jni.h" and "md.h"
# usually something like /usr/java/include and /usr/java/include/<arch-osname>.
JAVA_INCLUDE= 

# Extra Java specific dynamic linking options
JAVA_DLNK  = 
JAVALIBPREFIX = lib

# ----------------------------------------------------------------
# Build a java dynamically loadable module (C)
# ----------------------------------------------------------------

java: $(SRCS)
	$(SWIG) -java $(SWIGOPT) $(INTERFACE)
	$(CC) -c $(CCSHARED) $(CFLAGS) $(SRCS) $(ISRCS) $(INCLUDES) $(JAVA_INCLUDE)
	$(LDSHARED) $(OBJS) $(IOBJS) $(JAVA_DLNK) $(LIBS) -o $(JAVALIBPREFIX)$(TARGET)$(SO)

# ----------------------------------------------------------------
# Build a java dynamically loadable module (C++)
# ----------------------------------------------------------------

java_cpp: $(SRCS)
	$(SWIG) -java -c++ $(SWIGOPT) $(INTERFACE)
	$(CXX) -c $(CCSHARED) $(CFLAGS) $(SRCS) $(CXXSRCS) $(ICXXSRCS) $(INCLUDES) $(JAVA_INCLUDE)
	$(CXXSHARED) $(OBJS) $(IOBJS) $(JAVA_DLNK) $(LIBS) $(CPP_DLLIBS) -o $(JAVALIBPREFIX)$(TARGET)$(SO)

# ----------------------------------------------------------------
# Build a java dynamically loadable module
# ----------------------------------------------------------------

java_multi: $(SRCS)
	$(SWIG) -java $(SWIGOPT) $(INTERFACE)
	$(CC) -c $(CCSHARED) $(CFLAGS) $(SRCS) $(ISRCS) $(INCLUDES) $(JAVA_INCLUDE)
	$(LDSHARED) $(OBJS) $(IOBJS) $(JAVA_DLNK) $(LIBS) -o $(JAVALIBPREFIX)$(TARGET)$(SO)

java_multi_cpp: $(SRCS)
	$(SWIG) -java -c++ $(SWIGOPT) $(INTERFACE)
	$(CXX) -c $(CCSHARED) $(CFLAGS) $(SRCS) $(CXXSRCS) $(ICXXSRCS) $(INCLUDES) $(JAVA_INCLUDE)
	$(CXXSHARED) $(OBJS) $(IOBJS) $(JAVA_DLNK) $(LIBS) $(CPP_DLLIBS) -o $(JAVALIBPREFIX)$(TARGET)$(SO)

# -----------------------------------------------------------------
# Cleaning the java examples
# -----------------------------------------------------------------

java_clean:
	rm -f *.o *.so *_wrap* *~ .~* core  *.class `find . -name \*.java | grep -v main.java`

##################################################################
#####                       MZSCHEME                        ######
##################################################################

MZC = test -n "" && 

# ----------------------------------------------------------------
# Build a C/C++ dynamically loadable module
# ----------------------------------------------------------------

mzscheme: $(SRCS)
	$(SWIG) -mzscheme $(SWIGOPT) $(INTERFACE)
	$(MZC) ++ccf "$(INCLUDES)" --cc $(ISRCS) $(SRCS)
	$(MZC) --ld $(TARGET)$(SO) $(OBJS) $(IOBJS)

mzscheme_cpp: $(SRCS)
	$(SWIG) -mzscheme -c++ $(SWIGOPT) $(INTERFACE)
	$(MZC) ++ccf "$(INCLUDES)" --cc $(ICXXSRCS) $(SRCS) $(CXXSRCS)
	$(MZC) --ld $(TARGET)$(SO) $(OBJS) $(IOBJS) $(CPP_DLLIBS) 

# ----------------------------------------------------------------
# Build a dynamically loadable module, linked against SWIG runtime
# ----------------------------------------------------------------

MZSCHEME_RUNTIME=-L$(RUNTIMEDIR) -lswigmz

mzscheme_multi: $(SRCS)
	$(SWIG) -c -mzscheme $(SWIGOPT) $(INTERFACE)
	$(MZC) ++ccf "$(INCLUDES)" --cc $(ISRCS) $(SRCS)
	$(MZC) --ld $(TARGET)$(SO) $(OBJS) $(IOBJS) $(MZSCHEME_RUNTIME)

mzscheme_multi_cpp: $(SRCS)
	$(SWIG) -c -mzscheme -c++ $(SWIGOPT) $(INTERFACE)
	$(MZC) ++ccf "$(INCLUDES)" --cc $(ICXXSRCS) $(SRCS) $(CXXSRCS)
	$(MZC) --ld $(TARGET)$(SO) $(OBJS) $(IOBJS) $(MZSCHEME_RUNTIME) $(CPP_DLLIBS) 

# -----------------------------------------------------------------
# Cleaning the mzscheme examples
# -----------------------------------------------------------------

mzscheme_clean:
	rm -f *.o *.so *_wrap* *~ .~* core 

##################################################################
#####                          Ocaml                         #####
##################################################################

OCC=ocamlc
OCAMLDLGEN=ocamldlgen
OCAMLFIND=ocamlfind
OCAMLMKTOP=ocamlmktop
NOLINK ?= false

ocaml_static: $(SRCS)
	$(SWIG) -ocaml $(SWIGOPT) $(INTERFACE)
	$(OCC) -g -c -ccopt -g -ccopt "$(INCLUDES)" $(ISRCS) $(SRCS)
	$(OCC) -g -c $(INTERFACE:%.i=%.mli)
	$(OCC) -g -c $(INTERFACE:%.i=%.ml)
	test -z "$(PROGFILE)" || test -f "$(PROGFILE)" && \
		$(OCC) -c $(PROGFILE)
	$(NOLINK) || $(OCC) -g -ccopt -g -cclib -g -custom -o $(TARGET) \
		$(INTERFACE:%.i=%.cmo) \
		$(PROGFILE:%.ml=%.cmo) \
		$(INTERFACE:%.i=%_wrap.o) $(OBJS) -cclib "$(LIBS)"

ocaml_dynamic: $(SRCS)
	$(SWIG) -ocaml $(SWIGOPT) $(INTERFACE)
	$(OCC) -g -c -ccopt -g -ccopt "$(INCLUDES)" $(ISRCS) $(SRCS)
	$(CXXSHARED) $(CCSHARED) $(CFLAGS) -o $(INTERFACE:%.i=%.so) \
		$(INTERFACE:%.i=%_wrap.o) $(OBJS) $(LIBS)
	$(OCAMLDLGEN) $(INTERFACE:%.i=%.ml) $(INTERFACE:%.i=%.so) > \
		$(INTERFACE:%.i=%_dynamic.ml)
	mv $(INTERFACE:%.i=%_dynamic.ml) $(INTERFACE:%.i=%.ml)
	rm $(INTERFACE:%.i=%.mli)
	$(OCAMLFIND) $(OCC) -g -c -package dl $(INTERFACE:%.i=%.ml)
	test -z "$(PROGFILE)" || test -f "$(PROGFILE)" && \
		$(OCC) -c $(PROGFILE)
	$(NOLINK) || $(OCAMLFIND) \
		$(OCC) -g -ccopt -g -cclib -g -custom -o $(TARGET) \
		-package dl -linkpkg \
		$(INTERFACE:%.i=%.cmo) $(PROGFILE:%.ml=%.cmo)

ocaml_static_toplevel: $(SRCS)
	$(SWIG) -ocaml $(SWIGOPT) $(INTERFACE)
	$(OCC) -g -c -ccopt -g -ccopt "$(INCLUDES)" $(ISRCS) $(SRCS)
	$(OCC) -g -c $(INTERFACE:%.i=%.mli)
	$(OCC) -g -c $(INTERFACE:%.i=%.ml)
	test -z "$(PROGFILE)" || test -f "$(PROGFILE)" && \
		$(OCC) -c $(PROGFILE)
	$(NOLINK) || $(OCAMLMKTOP) \
		-g -ccopt -g -cclib -g -custom -o $(TARGET)_top \
		$(INTERFACE:%.i=%.cmo) \
		$(INTERFACE:%.i=%_wrap.o) $(OBJS) -cclib "$(LIBS)"

ocaml_static_cpp: $(SRCS)
	$(SWIG) -ocaml -c++ $(SWIGOPT) $(INTERFACE)
	cp $(ICXXSRCS) $(ICXXSRCS:%.cxx=%.c)
	$(OCC) -cc '$(CXX)' -g -c -ccopt -g -ccopt "-xc++ $(INCLUDES)" \
		$(ICXXSRCS:%.cxx=%.c) $(SRCS) $(CXXSRCS)
	$(OCC) -g -c $(INTERFACE:%.i=%.mli)
	$(OCC) -g -c $(INTERFACE:%.i=%.ml)
	test -z "$(PROGFILE)" || test -f "$(PROGFILE)" && \
		$(OCC) -c $(PROGFILE)
	$(NOLINK) || $(OCC) -g -ccopt -g -cclib -g -custom -o $(TARGET) \
		$(INTERFACE:%.i=%.cmo) \
		$(PROGFILE:%.ml=%.cmo) \
		$(INTERFACE:%.i=%_wrap.o) $(OBJS) \
		-cclib "$(LIBS)" -cc '$(CXX)' 

ocaml_static_cpp_toplevel: $(SRCS)
	$(SWIG) -ocaml -c++ $(SWIGOPT) $(INTERFACE)
	cp $(ICXXSRCS) $(ICXXSRCS:%.cxx=%.c)
	$(OCC) -cc '$(CXX)' -g -c -ccopt -g -ccopt "-xc++ $(INCLUDES)" \
		$(ICXXSRCS:%.cxx=%.c) $(SRCS) $(CXXSRCS)
	$(OCC) -g -c $(INTERFACE:%.i=%.mli)
	$(OCC) -g -c $(INTERFACE:%.i=%.ml)
	test -z "$(PROGFILE)" || test -f "$(PROGFILE)" && \
		$(OCC) -c $(PROGFILE)
	$(NOLINK) || $(OCAMLMKTOP) \
		-g -ccopt -g -cclib -g -custom -o $(TARGET)_top \
		$(INTERFACE:%.i=%.cmo) \
		$(INTERFACE:%.i=%_wrap.o) $(OBJS) \
		-cclib "$(LIBS)" -cc '$(CXX)' 

ocaml_dynamic_cpp: $(SRCS)
	$(SWIG) -ocaml -c++ $(SWIGOPT) $(INTERFACE)
	cp $(ICXXSRCS) $(ICXXSRCS:%.cxx=%.c)
	$(OCC) -cc '$(CXX)' -g -c -ccopt -g -ccopt "-xc++ $(INCLUDES)" \
		$(ICXXSRCS:%.cxx=%.c) $(SRCS) $(CXXSRCS) -ccopt -fPIC
	$(CXXSHARED) $(CXXFLAGS) -o $(INTERFACE:%.i=%.so) \
		$(INTERFACE:%.i=%_wrap.o) $(OBJS) \
		$(CPP_DLLIBS) $(LIBS)
	$(OCAMLDLGEN) $(INTERFACE:%.i=%.ml) $(INTERFACE:%.i=%.so) > \
		$(INTERFACE:%.i=%_dynamic.ml)
	mv $(INTERFACE:%.i=%_dynamic.ml) $(INTERFACE:%.i=%.ml)
	rm $(INTERFACE:%.i=%.mli)
	$(OCAMLFIND) $(OCC) -g -c -package dl $(INTERFACE:%.i=%.ml)
	test -z "$(PROGFILE)" || test -f "$(PROGFILE)" && \
		$(OCC) -c $(PROGFILE)
	$(NOLINK) || $(OCAMLFIND) \
		$(OCC) -cclib -export-dynamic -g -ccopt -g -cclib -g -custom \
		-o $(TARGET) \
		-package dl -linkpkg \
		$(INTERFACE:%.i=%.cmo) $(PROGFILE:%.ml=%.cmo) -cc '$(CXX)'

ocaml_static_multi_cpp: $(SRCS)
	$(SWIG) -c -ocaml -c++ $(SWIGOPT) \
		$(INTERFACE)
	cp $(ICXXSRCS) $(ICXXSRCS:%.cxx=%.c)
	$(OCC) -cc '$(CXX)' -g -c -ccopt -g -ccopt "-xc++ $(INCLUDES)" \
		$(ICXXSRCS:%.cxx=%.c) $(SRCS) $(CXXSRCS)
	$(OCC) -g -c $(INTERFACE:%.i=%.mli)
	$(OCC) -g -c $(INTERFACE:%.i=%.ml)
	test -z "$(PROGFILE)" || test -f "$(PROGFILE)" && \
		$(OCC) -c $(PROGFILE)
	$(NOLINK) || $(OCC) -g -ccopt -g -cclib -g -custom -o $(TARGET) \
		$(INTERFACE:%.i=%.cmo) \
		$(PROGFILE:%.ml=%.cmo) \
		$(INTERFACE:%.i=%_wrap.o) -cclib "$(LIBS)" -cc '$(CXX)' 

ocaml_clean:
	rm -f *.o *.so *_wrap* *~ .~* core  *.cmo *.cmi $(MLFILE) $(MLFILE)i

##################################################################
#####                       RUBY                            ######
##################################################################

# Make sure these locate your Ruby installation
RUBY_CFLAGS=  -DHAVE_CONFIG_H
RUBY_INCLUDE= -I/usr/local/lib/ruby/1.4/arch
RUBY_LIB     = /usr/local/lib/ruby/1.4/arch
RUBY_DLNK = 

# ----------------------------------------------------------------
# Build a C dynamically loadable module
# ----------------------------------------------------------------

ruby: $(SRCS)
	$(SWIG) -ruby $(SWIGOPT) $(INTERFACE)
	$(CC) -c $(CCSHARED) $(CFLAGS) $(RUBY_CFLAGS) $(ISRCS) $(SRCS) $(INCLUDES) $(RUBY_INCLUDE)
	$(LDSHARED) $(OBJS) $(IOBJS) $(RUBY_DLNK) $(LIBS) -o $(TARGET)$(SO)

# -----------------------------------------------------------------
# Build a C++ dynamically loadable module
# -----------------------------------------------------------------

ruby_cpp: $(SRCS)
	$(SWIG) -c++ -ruby $(SWIGOPT) $(INTERFACE)
	$(CXX) -c $(CCSHARED) $(CFLAGS) $(RUBY_CFLAGS) $(ICXXSRCS) $(SRCS) $(CXXSRCS) $(INCLUDES) $(RUBY_INCLUDE)
	$(CXXSHARED) $(OBJS) $(IOBJS) $(RUBY_DLNK) $(LIBS) $(CPP_DLLIBS) -o $(TARGET)$(SO)

# -----------------------------------------------------------------
# Build a dynamically loadable module, linked against SWIG runtime lib
# -----------------------------------------------------------------

RUBY_RUNTIME=-L$(RUNTIMEDIR) -lswigrb

ruby_multi: $(SRCS)
	$(SWIG) -c -ruby $(SWIGOPT) $(INTERFACE)
	$(CC) -c $(CCSHARED) $(CFLAGS) $(RUBY_CFLAGS) $(ISRCS) $(SRCS) $(INCLUDES) $(RUBY_INCLUDE)
	$(LDSHARED) $(OBJS) $(IOBJS) $(RUBY_RUNTIME) $(RUBY_DLNK) $(LIBS) -o $(TARGET)$(SO)

ruby_multi_cpp: $(SRCS)
	$(SWIG) -c -c++ -ruby $(SWIGOPT) $(INTERFACE)
	$(CXX) -c $(CCSHARED) $(CFLAGS) $(RUBY_CFLAGS) $(ICXXSRCS) $(SRCS) $(CXXSRCS) $(INCLUDES) $(RUBY_INCLUDE)
	$(CXXSHARED) $(OBJS) $(IOBJS) $(RUBY_RUNTIME) $(RUBY_DLNK) $(LIBS) $(CPP_DLLIBS) -o $(TARGET)$(SO)

# -----------------------------------------------------------------
# Build statically linked Ruby interpreter
#
# These should only be used in conjunction with the %include embed.i
# library file
# -----------------------------------------------------------------

RUBY_LIBOPTS = -lruby -lm -ldl  $(SYSLIBS)

ruby_static: $(SRCS)
	$(SWIG) -ruby -lembed.i $(SWIGOPT) $(INTERFACE)
	$(CC) $(CFLAGS) $(RUBY_CFLAGS) -Xlinker -export-dynamic $(ISRCS) $(SRCS) $(INCLUDES) \
	$(RUBY_INCLUDE) $(LIBS) -L$(RUBY_LIB) $(RUBY_LIBOPTS) -o $(TARGET)

ruby_cpp_static: $(SRCS)
	$(SWIG) -c++ -ruby -lembed.i $(SWIGOPT) $(INTERFACE)
	$(CXX) $(CFLAGS) $(RUBY_CFLAGS) $(ICXXSRCS) $(SRCS) $(CXXSRCS) $(INCLUDES) \
	$(RUBY_INCLUDE) $(LIBS)  -L$(RUBY_LIB) $(RUBY_LIBOPTS) -o $(TARGET)


# -----------------------------------------------------------------
# Cleaning the Ruby examples
# -----------------------------------------------------------------

ruby_clean:
	rm -f *.o *.so *_wrap* *~ .~* core  myruby *.pm

##################################################################
#####                       PHP                             ######
##################################################################

# -------------------------------------------------------------------
# Build a PHP4 dynamically loadable module (C)
# -------------------------------------------------------------------

PHP4_INCLUDE = 

php4: $(SRCS)
	$(SWIG) -php4 -cppext cxx $(SWIGOPT) $(INTERFACE)
	$(CC) -c $(CCSHARED) $(CFLAGS) $(SRCS) $(ISRCS) $(INCLUDES) $(PHP4_INCLUDE)
	$(LDSHARED) $(OBJS) $(IOBJS) $(LIBS) -o $(TARGET)$(SO)

# --------------------------------------------------------------------
# Build a PHP4 dynamically loadable module (C++)
# --------------------------------------------------------------------

php4_cpp: $(SRCS)
	$(SWIG) -php4 -cppext cxx -c++ $(SWIGOPT) $(INTERFACE)
	$(CXX) -c $(CCSHARED) $(CFLAGS) $(SRCS) $(CXXSRCS) $(ICXXSRCS) $(INCLUDES) $(PHP4_INCLUDE)
	$(CXXSHARED) $(OBJS) $(IOBJS) $(LIBS) $(CPP_DLLIBS) -o $(TARGET)$(SO)

# ---------------------------------------------------------------------
# Build a dynamically loadable module, linked against SWIG Runtime lib
# ---------------------------------------------------------------------

PHP4_RUNTIME=-L$(RUNTIMEDIR) -lswigphp4

php4_multi: $(SRCS)
	$(SWIG) -c -php4 -cppext cxx $(SWIGOPT) $(INTERFACE)
	$(CC) -c $(CCSHARED) $(CFLAGS) $(ISRCS) $(SRCS) $(INCLUDES) $(PHP4_INCLUDE)
	$(LDSHARED) $(OBJS) $(IOBJS) $(PHP4_RUNTIME) $(PHP4_DLNK) $(LIBS) -o $(TARGET)module$(SO)

php4_multi_cpp: $(SRCS)
	$(SWIG) -c -c++ -php4 -cppext cxx $(SWIGOPT) $(INTERFACE)
	$(CXX) -c $(CCSHARED) $(CFLAGS) $(ICXXSRCS) $(SRCS) $(CXXSRCS) $(INCLUDES) $(PHP4_INCLUDE)
	$(CXXSHARED) $(OBJS) $(IOBJS) $(PHP4_RUNTIME) $(PHP4_DLNK) $(LIBS) $(CPP_DLLIBS) -o $(TARGET)module$(SO)

# -----------------------------------------------------------------
# Cleaning the PHP4 examples
# -----------------------------------------------------------------

php4_clean:
	rm -f *.o *$(SO) *_wrap* *~ .~* core  *.php

##################################################################
#####                       Pike                            ######
##################################################################

# Make sure these locate your Pike installation
PIKE_CFLAGS  =  -DHAVE_CONFIG_H
PIKE_INCLUDE = 
PIKE_LIB     = @PIKELIB@
PIKE_DLNK    = 

# ----------------------------------------------------------------
# Build a C dynamically loadable module
# ----------------------------------------------------------------

pike: $(SRCS)
	$(SWIG) -pike $(SWIGOPT) $(INTERFACE)
	$(CC) -c $(CCSHARED) $(CFLAGS) $(PIKE_CFLAGS) $(ISRCS) $(SRCS) $(INCLUDES) $(PIKE_INCLUDE)
	$(LDSHARED) $(OBJS) $(IOBJS) $(PIKE_DLNK) $(LIBS) -o $(TARGET)$(SO)

# -----------------------------------------------------------------
# Build a C++ dynamically loadable module
# -----------------------------------------------------------------

pike_cpp: $(SRCS)
	$(SWIG) -c++ -pike $(SWIGOPT) $(INTERFACE)
	$(CXX) -c $(CCSHARED) $(CFLAGS) $(PIKE_CFLAGS) $(ICXXSRCS) $(SRCS) $(CXXSRCS) $(INCLUDES) $(PIKE_INCLUDE)
	$(CXXSHARED) $(OBJS) $(IOBJS) $(PIKE_DLNK) $(LIBS) $(CPP_DLLIBS) -o $(TARGET)$(SO)

# -----------------------------------------------------------------
# Build a dynamically loadable module, linked against SWIG runtime lib
# -----------------------------------------------------------------

PIKE_RUNTIME=-L$(RUNTIMEDIR) -lswigpike

pike_multi: $(SRCS)
	$(SWIG) -c -pike $(SWIGOPT) $(INTERFACE)
	$(CC) -c $(CCSHARED) $(CFLAGS) $(PIKE_CFLAGS) $(ISRCS) $(SRCS) $(INCLUDES) $(PIKE_INCLUDE)
	$(LDSHARED) $(OBJS) $(IOBJS) $(PIKE_RUNTIME) $(PIKE_DLNK) $(LIBS) -o $(TARGET)$(SO)

pike_multi_cpp: $(SRCS)
	$(SWIG) -c -c++ -pike $(SWIGOPT) $(INTERFACE)
	$(CXX) -c $(CCSHARED) $(CFLAGS) $(PIKE_CFLAGS) $(ICXXSRCS) $(SRCS) $(CXXSRCS) $(INCLUDES) $(PIKE_INCLUDE)
	$(CXXSHARED) $(OBJS) $(IOBJS) $(PIKE_RUNTIME) $(PIKE_DLNK) $(LIBS) $(CPP_DLLIBS) -o $(TARGET)$(SO)

# -----------------------------------------------------------------
# Build statically linked Pike interpreter
#
# These should only be used in conjunction with the %include embed.i
# library file
# -----------------------------------------------------------------

PIKE_LIBOPTS = @PIKELINK@ -ldl  $(SYSLIBS)

pike_static: $(SRCS)
	$(SWIG) -pike -lembed.i $(SWIGOPT) $(INTERFACE)
	$(CC) $(CFLAGS) $(PIKE_CFLAGS) -Xlinker -export-dynamic $(ISRCS) $(SRCS) $(INCLUDES) \
	$(PIKE_INCLUDE) $(LIBS) -L$(PIKE_LIB) $(PIKE_LIBOPTS) -o $(TARGET)

pike_cpp_static: $(SRCS)
	$(SWIG) -c++ -pike -lembed.i $(SWIGOPT) $(INTERFACE)
	$(CXX) $(CFLAGS) $(PIKE_CFLAGS) $(ICXXSRCS) $(SRCS) $(CXXSRCS) $(INCLUDES) \
	$(PIKE_INCLUDE) $(LIBS)  -L$(PIKE_LIB) $(PIKE_LIBOPTS) -o $(TARGET)

# -----------------------------------------------------------------
# Cleaning the Pike examples
# -----------------------------------------------------------------

pike_clean:
	rm -f *.o *$(SO) *_wrap* *~ .~* core  mypike 


# -----------------------------------------------------------------
# Compile a CHICKEN Scheme file to C.
#
# -----------------------------------------------------------------

CHICKEN_LIBOPTS =   $(SYSLIBS)
CHICKEN_SHAREDLIBOPTS =   $(SYSLIBS)
CHICKEN_CFLAGS = 
CHICKEN_SHAREDCFLAGS = 
CHICKENOPTS = -optimize-level 3 -quiet

chicken: $(INTERFACE)
	 $(INTERFACE) $(CHICKENOPTS) -explicit-use \
		-output-file $(TARGET)

chicken_c:
	$(SWIG) -chicken $(SWIGOPT) $(INCLUDE) $(INTERFACE)

chicken_cpp:
	$(SWIG) -c++ -chicken $(SWIGOPT) $(INCLUDE) $(INTERFACE)

compile: $(INTERFACE)
	$(CC) $(CFLAGS) $(INCLUDE) -c $(INTERFACE) -o $(TARGET)

cxxcompile: $(INTERFACE)
	$(CXX) $(CFLAGS) $(INCLUDE) -c $(INTERFACE) -o $(TARGET)

chicken_csi:
	X=`pwd` && cd  && \
	 /src/csi.scm $(CHICKENOPTS) \
		-include-path /src \
		-prologue /src/build.scm \
		-prelude "(declare (uses posix $(INTERFACE)))" \
		-output-file $$X/$(TARGET)

chicken_clean:
	rm -f *.o *.so *_wrap* *~ .~* core  STACKTRACE
	rm -f *.exe module$(SO)

# -----------------------------------------------------------------
# Build a CHICKEN dynamically loadable module
# -----------------------------------------------------------------

chicken_module: $(SRCS) $(CXXSRCS) $(CHICKSRCS)
	$(CC) -c $(CFLAGS) $(CHICKEN_SHAREDCFLAGS) $(INCLUDE) \
		$(CHICKEN_INCLUDE) $(CHICKSRCS)
	test x = "x$(SRCS)" || \
		$(CC) -c $(CFLAGS) $(CHICKEN_SHAREDCFLAGS) \
			$(INCLUDE) $(CHICKEN_INCLUDE) \
			$(ISRCS) $(SRCS)
	test x = "x$(CXXSRCS)" || \
		$(CXX) -c $(CFLAGS) $(CHICKEN_SHAREDCFLAGS) \
			$(INCLUDE) $(CHICKEN_INCLUDE) \
			$(ICXXSRCS) $(CXXSRCS)
	$(CXXSHARED) $(SRCS:.c=.o) $(CXXSRCS:.cxx=.o) \
		$(CHICKSRCS:.c=.o) \
		$(LIBS) $(CHICKEN_SHAREDLIBOPTS) \
		-o $(TARGET)$(SO)

# -----------------------------------------------------------------
# Build statically linked CHICKEN interpreter
# -----------------------------------------------------------------

chicken_static: csi.c $(SRCS) $(CXXSRCS) $(CHICKSRCS)
	$(CC) -c $(CFLAGS) $(CHICKEN_CFLAGS) $(INCLUDE) $(CHICKEN_INCLUDE) \
		csi.c
	$(CC) -c $(CFLAGS) $(CHICKEN_CFLAGS) $(INCLUDE) \
		$(CHICKEN_INCLUDE) $(CHICKSRCS)
	test x = "x$(SRCS)" || \
		$(CC) -c $(CFLAGS) $(INCLUDE) $(CHICKEN_INCLUDE) \
			$(CHICKEN_CFLAGS) $(ISRCS) $(SRCS)
	test x = "x$(CXXSRCS)" || \
		$(CXX) -c $(CFLAGS) $(INCLUDE) $(CHICKEN_INCLUDE) \
			$(CHICKEN_CFLAGS) $(ICXXSRCS) $(CXXSRCS)
	$(CXX) csi.o $(SRCS:.c=.o) $(CXXSRCS:.cxx=.o) \
		$(CHICKSRCS:.c=.o) \
		$(LIBS) $(CHICKEN_LIBOPTS) \
		-o $(TARGET)

##################################################################
#####                      CSHARP                           ######
##################################################################

# Extra CSharp specific dynamic linking options
CSHARP_DLNK  = 
CSHARPLIBPREFIX = lib
CSHARPCOMPILER = 
CSHARPCILINTERPRETER = 

# ----------------------------------------------------------------
# Build a CSharp dynamically loadable module (C)
# ----------------------------------------------------------------

csharp: $(SRCS)
	$(SWIG) -csharp $(SWIGOPT) $(INTERFACE)
	$(CC) -c $(CCSHARED) $(CFLAGS) $(SRCS) $(ISRCS) $(INCLUDES)
	$(LDSHARED) $(OBJS) $(IOBJS) $(CSHARP_DLNK) $(LIBS) -o $(CSHARPLIBPREFIX)$(TARGET)$(SO)

# ----------------------------------------------------------------
# Build a CSharp dynamically loadable module (C++)
# ----------------------------------------------------------------

csharp_cpp: $(SRCS)
	$(SWIG) -csharp -c++ $(SWIGOPT) $(INTERFACE)
	$(CXX) -c $(CCSHARED) $(CFLAGS) $(SRCS) $(CXXSRCS) $(ICXXSRCS) $(INCLUDES)
	$(CXXSHARED) $(OBJS) $(IOBJS) $(CSHARP_DLNK) $(LIBS) $(CPP_DLLIBS) -o $(CSHARPLIBPREFIX)$(TARGET)$(SO)

# ----------------------------------------------------------------
# Build a CSharp dynamically loadable module
# ----------------------------------------------------------------

csharp_multi: $(SRCS)
	$(SWIG) -csharp $(SWIGOPT) $(INTERFACE)
	$(CC) -c $(CCSHARED) $(CFLAGS) $(SRCS) $(ISRCS) $(INCLUDES)
	$(LDSHARED) $(OBJS) $(IOBJS) $(CSHARP_DLNK) $(LIBS) -o $(CSHARPLIBPREFIX)$(TARGET)$(SO)

csharp_multi_cpp: $(SRCS)
	$(SWIG) -csharp -c++ $(SWIGOPT) $(INTERFACE)
	$(CXX) -c $(CCSHARED) $(CFLAGS) $(SRCS) $(CXXSRCS) $(ICXXSRCS) $(INCLUDES)
	$(CXXSHARED) $(OBJS) $(IOBJS) $(CSHARP_DLNK) $(LIBS) $(CPP_DLLIBS) -o $(CSHARPLIBPREFIX)$(TARGET)$(SO)

# ----------------------------------------------------------------
# Compile CSharp files
# ----------------------------------------------------------------

csharp_compile: $(SRCS)
	$(CSHARPCOMPILER) $(CSHARPFLAGS) $(CSHARPSRCS)

# -----------------------------------------------------------------
# Cleaning the CSharp examples
# -----------------------------------------------------------------

csharp_clean:
	rm -f *.o *.dll *_wrap* *~ .~* core  runme gc.log `find . -name \*.cs | grep -v runme.cs`

