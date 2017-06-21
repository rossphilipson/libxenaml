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
SUBDIRS=src test

EXTRA_DIST= version-major version-minor version-micro version-files version-md5sums

bin_SCRIPTS = libxenaml-config

pkgconfigdir=${libdir}/pkgconfig
pkgconfig_DATA= libxenaml.pc

libxenaml.pc.src:libxenaml.pc.src.in
libxenaml.pc:libxenaml.pc.src
	@SED@ -f version.sed < libxenaml.pc.src > libxenaml.pc || rm -f libxenaml.pc

libxenaml-config.src:libxenaml-config.src.in
libxenaml-config:libxenaml-config.src version.sed
	@SED@ -f version.sed < libxenaml-config.src > libxenaml-config || rm -f libxenaml-config
	chmod +x $@
	

VFD=${srcdir}
VF=${shell cat ${VFD}/version-files}
VFS=${VF:%=${VFD}/%}
VCHK=${shell cat ${VFS} | @MD5SUM@ | @AWK@ '{print $$1 }' }
VNUM=${shell @GREP@ ${VCHK} ${VFD}/version-md5sums | @AWK@ '{ print $$2 }'  }
VDEF=${shell echo `cat ${VFD}/version-major`.`cat ${VFD}/version-minor`.`cat ${VFD}/version-micro` }

version.sed: $(VFD)/version-files $(VFD)/version-major \
	$(VFD)/version-minor $(VFD)/version-micro \
	$(VFD)/version-md5sums ${VFS} Makefile
	if [ .${VNUM} = . ]; then \
		echo "s/%VERSION%/${VDEF}-E/g" > version.sed; \
	else \
		echo "s/%VERSION%/${VNUM}/g" > version.sed; \
	fi

