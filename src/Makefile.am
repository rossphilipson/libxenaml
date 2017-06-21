#
#
# Makefile.am:
#
# $Id:$
#
# $Log:$
#
#
#

INCLUDES =

SRCS= libxenaml.c version.c amlcore.c amlgen.c amlres.c
CPROTO=cproto

XENAMLSRCS=${SRCS}

noinst_HEADERS= project.h prototypes.h xenaml-tail.h ext_prototypes.h

libxenaml_la_SOURCES = ${XENAMLSRCS}

BUILT_SOURCES = version.h xenaml.h

lib_LTLIBRARIES=libxenaml.la

include_HEADERS=xenaml.h

AM_CFLAGS=-g

libxenaml_la_LDFLAGS = \
        -version-info $(LT_CURRENT):$(LT_REVISION):$(LT_AGE) \
        -release $(LT_RELEASE) \
        -export-dynamic

VFD=${srcdir}/..
VF=${shell cat ${VFD}/version-files}
VFS=${VF:%=${VFD}/%}
VCHK=${shell cat ${VFS} | @MD5SUM@ | @AWK@ '{print $$1 }' }
VNUM=${shell @GREP@ ${VCHK} ${VFD}/version-md5sums | @AWK@ '{ print $$2 }'  }
VDEF=${shell echo `cat ${VFD}/version-major`.`cat ${VFD}/version-minor`.`cat ${VFD}/version-micro` }


xenaml.h:xenaml-head.h xenaml-defs.h ext_prototypes.h xenaml-tail.h
	cat xenaml-head.h xenaml-defs.h ext_prototypes.h xenaml-tail.h > xenaml.h

protos:
	echo > prototypes.h
	echo > ext_prototypes.h
	cat xenaml-head.h xenaml-defs.h ext_prototypes.h xenaml-tail.h > xenaml.h
	${CPROTO} -v -DINT_PROTOS ${INCLUDES} ${SRCS} > prototypes.tmp
	mv -f prototypes.tmp prototypes.h
	${CPROTO} -v -DEXT_PROTOS ${INCLUDES} ${SRCS} > ext_prototypes.h
	cat xenaml-head.h xenaml-defs.h ext_prototypes.h xenaml-tail.h > xenaml.h

version.h: $(VFD)/version-files $(VFD)/version-major \
	$(VFD)/version-minor $(VFD)/version-micro \
	$(VFD)/version-md5sums ${VFS} Makefile
	if [ .${VNUM} = . ]; then \
		echo "#define FULL_VERSION \"libxenaml Version ${VDEF} + Edits\"" > version.h; \
		echo ${VDEF}-E > version-num; \
	else \
		echo "#define FULL_VERSION \"libxenaml Version ${VNUM}\"" > version.h; \
		echo ${VNUM} > version-num; \
	fi

