
SCRIPTS_DIR:=$(abspath $(dir $(lastword $(MAKEFILE_LIST))))
EDAPACK_SYMBIYOSYS_DIR:=$(abspath $(SCRIPTS_DIR)/..)
PACKAGES_DIR=$(EDAPACK_SYMBIYOSYS_DIR)/packages
BUILD_DIR=$(EDAPACK_SYMBIYOSYS_DIR)/build
EDAPACK_BUILD_URL=https://github.com/EDAPack/edapack-build

include $(EDAPACK_SYMBIYOSYS_DIR)/etc/package.info

GNU_MIRROR_URL ?= http://mirrors.kernel.org/gnu

YOSYS_VERSION=0.8
YOSYS_DIR=yosys-master
YOSYS_ZIP=$(YOSYS_DIR).zip
YOSYS_URL=https://github.com/YosysHQ/yosys/archive/master.zip

YICES2_VERSION=2.6.1
YICES2_DIR=yices2-Yices-$(YICES2_VERSION)
YICES2_ZIP=$(YICES2_DIR).zip
YICES2_URL=https://github.com/SRI-CSL/yices2/archive/Yices-$(YICES2_VERSION).zip

GMP_VERSION=6.1.0
GMP_DIR=gmp-$(GMP_VERSION)
GMP_TBZ=$(GMP_DIR).tar.bz2
GMP_URL=$(GNU_MIRROR_URL)/gmp/$(GMP_TBZ)

READLINE_VERSION=7.0
READLINE_DIR=readline-$(READLINE_VERSION)
READLINE_TGZ=$(READLINE_DIR).tar.gz
READLINE_URL=https://ftp.gnu.org/gnu/readline/$(READLINE_TGZ)

FFI_VERSION=3.2.1
FFI_DIR=libffi-$(FFI_VERSION)
FFI_TGZ=$(FFI_DIR).tar.gz
FFI_URL=ftp://sourceware.org/pub/libffi/$(FFI_TGZ)

Z3_VERSION=4.8.4
Z3_DIR=z3-z3-$(Z3_VERSION)
Z3_ZIP=$(Z3_DIR).zip
Z3_URL=https://github.com/Z3Prover/z3/archive/z3-$(Z3_VERSION).zip

BOOLECTOR_VERSION=3.0.0
BOOLECTOR_DIR=boolector-$(BOOLECTOR_VERSION)
BOOLECTOR_ZIP=$(BOOLECTOR_DIR).zip
BOOLECTOR_URL=https://github.com/Boolector/boolector/archive/$(BOOLECTOR_VERSION).zip

LINGELING_DIR=lingeling-master
LINGELING_ZIP=$(LINGELING_DIR).zip
LINGELING_URL=https://github.com/arminbiere/lingeling/archive/master.zip

BTOR2TOOLS_DIR=btor2tools-master
BTOR2TOOLS_ZIP=$(BTOR2TOOLS_DIR).zip
BTOR2TOOLS_URL=https://github.com/Boolector/btor2tools/archive/master.zip

ABC_DIR=abc-master
ABC_ZIP=$(ABC_DIR).zip
ABC_URL=https://github.com/berkeley-abc/abc/archive/master.zip

SBY_DIR=SymbiYosys-master
SBY_ZIP=$(SYMBIYOSYS_DIR).zip
SBY_URL=https://github.com/YosysHQ/SymbiYosys/archive/master.zip

SUPER_PROVE_TGZ=super_prove.tar.gz
#SUPER_PROVE_URL=https://downloads.bvsrc.org/super_prove/super_prove-hwmcc17_final-2-d7b71160dddb-CentOS_6.7-Release.tar.gz
SUPER_PROVE_URL=https://downloads.bvsrc.org/super_prove/super_prove-hwmcc17_final-2-d7b71160dddb-CentOS_7-Release.tar.gz

EXTAVY_URL=https://bitbucket.org/arieg/extavy.git

SYMBIYOSYS_PREFIX=$(BUILD_DIR)/symbiyosys/symbiyosys

ifeq (true,$(VERBOSE))
Q=
WGET=wget
UNZIP=unzip -o
UNTARGZ=tar xvzf
UNTARBZ=tar xvjf
else
Q=@
WGET=wget -q
UNZIP=unzip -o -q
UNTARGZ=tar xzf
UNTARBZ=tar xjf
endif

uname_o:=$(shell uname -o)

ifeq (Linux,$(uname_o))
platform=linux_x86_64
else
ifeq (GNU/Linux,$(uname_o))
platform=linux_x86_64
else
platform=unknown
endif
endif

PACKAGE=$(BUILD_DIR)/symbiyosys-$(platform)-$(version).tar.gz

all : $(PACKAGE)

$(PACKAGE) : \
		$(BUILD_DIR)/z3.d \
		$(BUILD_DIR)/yices2.d \
		$(BUILD_DIR)/yosys.d \
		$(BUILD_DIR)/boolector.d \
		$(BUILD_DIR)/sby.d \
		$(BUILD_DIR)/super_prove.d \
		$(BUILD_DIR)/stdcpp.d \
		$(BUILD_DIR)/symbiyosys/bin/sby \
		$(PACKAGES_DIR)/install.py \
		$(EDAPACK_SYMBIYOSYS_DIR)/etc/package.info \
		$(EDAPACK_SYMBIYOSYS_DIR)/etc/modulefile
	$(Q)mkdir -p $(BUILD_DIR)/symbiyosys/etc
	$(Q)cp $(EDAPACK_SYMBIYOSYS_DIR)/etc/package.info $(BUILD_DIR)/symbiyosys/etc
	$(Q)cp $(PACKAGES_DIR)/install.py $(BUILD_DIR)/symbiyosys/etc
	$(Q)sed -e 's/{{version}}/$(version)/g' \
		$(EDAPACK_SYMBIYOSYS_DIR)/etc/modulefile > \
		$(BUILD_DIR)/symbiyosys/etc/modulefile
	$(Q)cd $(BUILD_DIR)/symbiyosys ; tar czf $(PACKAGE) *

#YOSYS_EXTRA_CXXFLAGS += -I$(BUILD_DIR)/yosys/deps/include
YOSYS_EXTRA_CXXFLAGS += -I$(SYMBIYOSYS_PREFIX)/lib/$(FFI_DIR)/include
#YOSYS_EXTRA_CXXFLAGS += -DUINTPTR_MAX=0xFFFFFFFFFFFFFFFF -DUINT_MAX=0xFFFFFFFFFFFFFFFF
YOSYS_EXTRA_LDFLAGS += -L$(SYMBIYOSYS_PREFIX)/lib
YOSYS_EXTRA_LDFLAGS += -L$(SYMBIYOSYS_PREFIX)/lib64

$(BUILD_DIR)/yosys.d : \
		$(PACKAGES_DIR)/$(YOSYS_ZIP) \
		$(PACKAGES_DIR)/$(ABC_ZIP) \
		$(PACKAGES_DIR)/$(READLINE_TGZ) \
		$(PACKAGES_DIR)/$(FFI_TGZ)
	$(Q)rm -rf $(BUILD_DIR)/yosys
	$(Q)mkdir -p $(BUILD_DIR)/yosys
#	$(Q)cd $(BUILD_DIR)/yosys ; $(UNTARGZ) $(PACKAGES_DIR)/$(READLINE_TGZ)
	$(Q)cd $(BUILD_DIR)/yosys ; $(UNTARGZ) $(PACKAGES_DIR)/$(FFI_TGZ)
#	$(Q)cd $(BUILD_DIR)/yosys/$(READLINE_DIR) ; \
#		./configure --disable-shared \
#			--prefix=$(BUILD_DIR)/yosys/deps
#	$(Q)cd $(BUILD_DIR)/yosys/$(READLINE_DIR) ; $(MAKE)
#	$(Q)cd $(BUILD_DIR)/yosys/$(READLINE_DIR) ; $(MAKE) install
	$(Q)cd $(BUILD_DIR)/yosys/$(FFI_DIR) ; \
		./configure --disable-shared \
			--prefix=$(SYMBIYOSYS_PREFIX)
	$(Q)cd $(BUILD_DIR)/yosys/$(FFI_DIR) ; $(MAKE)
	$(Q)cd $(BUILD_DIR)/yosys/$(FFI_DIR) ; $(MAKE) install
	$(Q)cd $(BUILD_DIR)/yosys ; $(UNZIP) $(PACKAGES_DIR)/$(YOSYS_ZIP)
	$(Q)cd $(BUILD_DIR)/yosys ; $(UNZIP) $(PACKAGES_DIR)/$(ABC_ZIP)
	$(Q)cd $(BUILD_DIR)/yosys ; mv $(ABC_DIR) $(YOSYS_DIR)/abc
	$(Q)cd $(BUILD_DIR)/yosys/$(YOSYS_DIR) ; \
		patch -p1 < $(SCRIPTS_DIR)/yosys.patch
	$(Q)cd $(BUILD_DIR)/yosys/$(YOSYS_DIR) ; \
		sed -i \
			-e 's/CONFIG := clang/CONFIG := gcc/g' \
			-e 's/ENABLE_READLINE := 1/ENABLE_READLINE := 0/g' \
			-e 's/ENABLE_TCL := 1/ENABLE_TCL := 0/g' \
			-e 's/CXXFLAGS := \$$(CXXFLAGS)/CXXFLAGS := \$$(CXXFLAGS) \$$(EXTRA_CXXFLAGS)/g' Makefile \
			-e 's/LDFLAGS := \$$(LDFLAGS)/LDFLAGS := \$$(LDFLAGS) \$$(EXTRA_LDFLAGS)/g' Makefile
	$(Q)if test "x$(VERBOSE)" = "xtrue"; then \
		cd $(BUILD_DIR)/yosys/$(YOSYS_DIR) ; \
		sed -i -e 's/PRETTY = 1/PRETTY = 0/g' Makefile; \
	fi
	$(Q)cd $(BUILD_DIR)/yosys/$(YOSYS_DIR)/abc ; \
		sed -i -e 's/-Wall/\$$(EXTRA_CXXFLAGS) -Wall/g' \
			-e 's/-lreadline/\$$(EXTRA_LDFLAGS) -lreadline/g' \
		Makefile
	$(Q)cd $(BUILD_DIR)/yosys/$(YOSYS_DIR) ; \
		$(MAKE) PREFIX=$(SYMBIYOSYS_PREFIX) \
			EXTRA_CXXFLAGS="$(YOSYS_EXTRA_CXXFLAGS)" \
			EXTRA_LDFLAGS="$(YOSYS_EXTRA_LDFLAGS)" \
			ABCPULL=0 ABCREV=default VERBOSE=$(Q)
	$(Q)cd $(BUILD_DIR)/yosys/$(YOSYS_DIR) ; \
		$(MAKE) PREFIX=$(SYMBIYOSYS_PREFIX) \
			ABCPULL=0 ABCREV=default VERBOSE=$(Q) install
	$(Q)touch $@

$(BUILD_DIR)/readline.d : $(PACKAGES_DIR)/$(READLINE_TGZ)
	$(Q)rm -rf $(BUILD_DIR)/readline
	$(Q)mkdir -p $(BUILD_DIR)/readline
	$(Q)cd $(BUILD_DIR)/readline ; $(UNTARGZ) $(PACKAGES_DIR)/$(READLINE_TGZ)
	$(Q)cd $(BUILD_DIR)/readline/$(READLINE_DIR) ; \
		./configure --disable-shared --prefix=$(BUILD_DIR)/readline/inst
	$(Q)cd $(BUILD_DIR)/readline/$(READLINE_DIR) ; $(MAKE) 
	$(Q)cd $(BUILD_DIR)/readline/$(READLINE_DIR) ; $(MAKE) install
	$(Q)touch $@

$(PACKAGES_DIR)/$(READLINE_TGZ) :
	$(Q)if test ! -d $(PACKAGES_DIR); then mkdir -p $(PACKAGES_DIR); fi
	$(Q)$(WGET) -O $@ $(READLINE_URL)

$(PACKAGES_DIR)/$(FFI_TGZ) :
	$(Q)if test ! -d $(PACKAGES_DIR); then mkdir -p $(PACKAGES_DIR); fi
	$(Q)$(WGET) -O $@ $(FFI_URL)

$(BUILD_DIR)/yices2.d : \
		$(PACKAGES_DIR)/$(YICES2_ZIP) \
		$(PACKAGES_DIR)/$(GMP_TBZ)
	$(Q)rm -rf $(BUILD_DIR)/yices2
	$(Q)mkdir -p $(BUILD_DIR)/yices2
	$(Q)cd $(BUILD_DIR)/yices2 ; $(UNZIP) $(PACKAGES_DIR)/$(YICES2_ZIP)
	$(Q)cd $(BUILD_DIR)/yices2 ; $(UNTARBZ) $(PACKAGES_DIR)/$(GMP_TBZ)
	$(Q)cd $(BUILD_DIR)/yices2/$(GMP_DIR) ; \
		./configure --prefix=$(SYMBIYOSYS_PREFIX) \
		CFLAGS="-fPIC"
	$(Q)cd $(BUILD_DIR)/yices2/$(GMP_DIR) ; $(MAKE)
	$(Q)cd $(BUILD_DIR)/yices2/$(GMP_DIR) ; $(MAKE) install
	$(Q)cd $(BUILD_DIR)/yices2/$(YICES2_DIR) ; autoconf
	$(Q)cd $(BUILD_DIR)/yices2/$(YICES2_DIR) ; \
		./configure --prefix=$(SYMBIYOSYS_PREFIX) \
			LDFLAGS="-L$(SYMBIYOSYS_PREFIX)/lib" \
			CFLAGS="-I$(SYMBIYOSYS_PREFIX)/include" \
			--with-pic-gmp=$(SYMBIYOSYS_PREFIX)/lib/libgmp.a \
			--with-pic-include-dir=$(SYMBIYOSYS_PREFIX)/include 
	$(Q)cd $(BUILD_DIR)/yices2/$(YICES2_DIR) ; $(MAKE)
	$(Q)cd $(BUILD_DIR)/yices2/$(YICES2_DIR) ; $(MAKE) install
	$(Q)touch $@

$(PACKAGES_DIR)/$(YICES2_ZIP) :
	$(Q)mkdir -p $(PACKAGES_DIR)
	$(Q)$(WGET) -O $@ $(YICES2_URL)

$(PACKAGES_DIR)/$(GMP_TBZ) :
	$(Q)mkdir -p $(PACKAGES_DIR)
	$(Q)$(WGET) -O $@ $(GMP_URL)

$(BUILD_DIR)/z3.d : $(PACKAGES_DIR)/$(Z3_ZIP)
	$(Q)rm -rf $(BUILD_DIR)/z3
	$(Q)mkdir -p $(BUILD_DIR)/z3
	$(Q)cd $(BUILD_DIR)/z3 ; $(UNZIP) $(PACKAGES_DIR)/$(Z3_ZIP)
	$(Q)cd $(BUILD_DIR)/z3/$(Z3_DIR) ; $(UNZIP) $(PACKAGES_DIR)/$(Z3_ZIP)
	$(Q)cd $(BUILD_DIR)/z3/$(Z3_DIR) ; export PYTHON=python3 ; \
		./configure --prefix=$(SYMBIYOSYS_PREFIX) 
	$(Q)cd $(BUILD_DIR)/z3/$(Z3_DIR)/build ; $(MAKE)
	$(Q)cd $(BUILD_DIR)/z3/$(Z3_DIR)/build ; $(MAKE) install
	$(Q)touch $@

# CMake picks up /usr/bin/c++ unless we force it otherwise
CMAKE_CXX_COMPILER_ID=GNU
export CMAKE_CXX_COMPILER_ID
CXX=g++
export CXX
CC=gcc
export CC

$(BUILD_DIR)/boolector.d : \
		$(PACKAGES_DIR)/$(BOOLECTOR_ZIP) \
		$(PACKAGES_DIR)/$(LINGELING_ZIP) \
		$(PACKAGES_DIR)/$(BTOR2TOOLS_ZIP)
	$(Q)rm -rf $(BUILD_DIR)/boolector
	$(Q)mkdir -p $(BUILD_DIR)/boolector
	$(Q)cd $(BUILD_DIR)/boolector;$(UNZIP) $(PACKAGES_DIR)/$(BOOLECTOR_ZIP)
	$(Q)mkdir -p $(BUILD_DIR)/boolector/$(BOOLECTOR_DIR)/deps
	$(Q)cd $(BUILD_DIR)/boolector;$(UNZIP) $(PACKAGES_DIR)/$(LINGELING_ZIP)
	$(Q)cd $(BUILD_DIR)/boolector ; \
		mv $(LINGELING_DIR) $(BOOLECTOR_DIR)/deps/lingeling
	$(Q)cd $(BUILD_DIR)/boolector/$(BOOLECTOR_DIR)/deps/lingeling ; \
		sed -i -e 's/\bclone\b/prv_clone/g' ilingeling.c
	$(Q)cd $(BUILD_DIR)/boolector;$(UNZIP) $(PACKAGES_DIR)/$(BTOR2TOOLS_ZIP)
	$(Q)cd $(BUILD_DIR)/boolector ; \
		mv $(BTOR2TOOLS_DIR) $(BOOLECTOR_DIR)/deps/btor2tools
	$(Q)cd $(BUILD_DIR)/boolector/$(BOOLECTOR_DIR)/deps/lingeling ; \
		./configure.sh -fPIC
	$(Q)cd $(BUILD_DIR)/boolector/$(BOOLECTOR_DIR)/deps/lingeling ; $(MAKE)
	$(Q)cd $(BUILD_DIR)/boolector/$(BOOLECTOR_DIR)/deps/btor2tools ; \
		./configure.sh -fPIC
	$(Q)cd $(BUILD_DIR)/boolector/$(BOOLECTOR_DIR)/deps/btor2tools ; $(MAKE)
	$(Q)cd $(BUILD_DIR)/boolector/$(BOOLECTOR_DIR) ; ./configure.sh
	$(Q)cd $(BUILD_DIR)/boolector/$(BOOLECTOR_DIR)/build ; $(MAKE)
	$(Q)mkdir -p $(SYMBIYOSYS_PREFIX)/bin
	$(Q)cd $(BUILD_DIR)/boolector/$(BOOLECTOR_DIR)/build/bin ; \
		cp {boolector,btor*} $(SYMBIYOSYS_PREFIX)/bin
	$(Q)cd $(BUILD_DIR)/boolector/$(BOOLECTOR_DIR)/deps/btor2tools/bin ; \
		cp btorsim $(SYMBIYOSYS_PREFIX)/bin
	$(Q)touch $@


$(PACKAGES_DIR)/$(BOOLECTOR_ZIP) :
	$(Q)mkdir -p $(PACKAGES_DIR)
	$(Q)$(WGET) -O $@ $(BOOLECTOR_URL)

$(PACKAGES_DIR)/$(LINGELING_ZIP) :
	$(Q)mkdir -p $(PACKAGES_DIR)
	$(Q)$(WGET) -O $@ $(LINGELING_URL)

$(PACKAGES_DIR)/$(BTOR2TOOLS_ZIP) :
	$(Q)mkdir -p $(PACKAGES_DIR)
	$(Q)$(WGET) -O $@ $(BTOR2TOOLS_URL)

$(BUILD_DIR)/sby.d : \
		$(PACKAGES_DIR)/$(SBY_ZIP)
	$(Q)rm -rf $(BUILD_DIR)/sby
	$(Q)mkdir -p $(BUILD_DIR)/sby
	$(Q)cd $(BUILD_DIR)/sby ; \
		$(UNZIP) $(PACKAGES_DIR)/$(SBY_ZIP)
	$(Q)cd $(BUILD_DIR)/sby/$(SBY_DIR) ; \
		$(MAKE) PREFIX=$(SYMBIYOSYS_PREFIX) install
	$(Q)touch $@

$(PACKAGES_DIR)/$(SBY_ZIP) :
	$(Q)mkdir -p $(PACKAGES_DIR)
	$(Q)$(WGET) -O $@ $(SBY_URL)

$(PACKAGES_DIR)/$(Z3_ZIP) :
	$(Q)mkdir -p $(PACKAGES_DIR)
	$(Q)$(WGET) -O $@ $(Z3_URL)

$(PACKAGES_DIR)/$(YOSYS_ZIP) : 
	$(Q)mkdir -p $(PACKAGES_DIR)
	$(Q)$(WGET) -O $@ $(YOSYS_URL)

$(PACKAGES_DIR)/$(ABC_ZIP) : 
	$(Q)if test ! -d $(PACKAGES_DIR); then mkdir -p $(PACKAGES_DIR); fi
	$(Q)$(WGET) -O $@ $(ABC_URL)

$(BUILD_DIR)/super_prove.d : $(PACKAGES_DIR)/$(SUPER_PROVE_TGZ)
	$(Q)rm -rf $(BUILD_DIR)/super_prove
	$(Q)rm -rf $(SYMBIYOSYS_PREFIX)/super_prove
	$(Q)mkdir -p $(BUILD_DIR)
	$(Q)cd $(BUILD_DIR) ; $(UNTARGZ) $(PACKAGES_DIR)/$(SUPER_PROVE_TGZ)
	$(Q)cd $(BUILD_DIR) ; mv super_prove $(SYMBIYOSYS_PREFIX)
	$(Q)mkdir -p $(SYMBIYOSYS_PREFIX)/bin
	$(Q)cp $(SCRIPTS_DIR)/suprove $(SYMBIYOSYS_PREFIX)/bin/suprove
	$(Q)chmod +x $(SYMBIYOSYS_PREFIX)/bin/suprove
	$(Q)touch $@

$(PACKAGES_DIR)/$(SUPER_PROVE_TGZ) : 
	$(Q)mkdir -p $(PACKAGES_DIR)
	$(Q)$(WGET) -O $@ $(SUPER_PROVE_URL)

$(BUILD_DIR)/extavy.d : 
	$(Q)rm -rf $(BUILD_DIR)/extavy
	$(Q)mkdir -p $(BUILD_DIR)/extavy
	$(Q)cd $(BUILD_DIR)/extavy ; git clone $(EXTAVY_URL)
	$(Q)cd $(BUILD_DIR)/extavy/extavy ; git submodule update --init
	$(Q)cd $(BUILD_DIR)/extavy/extavy ; mkdir build
	$(Q)cd $(BUILD_DIR)/extavy/extavy/build ; cmake -DCMAKE_BUILD_TYPE=Release ..
	$(Q)cd $(BUILD_DIR)/extavy/extavy/build ; $(MAKE) 
	$(Q)cd $(BUILD_DIR)/extavy/extavy/build ; \
		cp avy/src/{avy,avybmc} $(SYMBIYOSYS_PREFIX)/bin
	$(Q)touch $@

WHICH_LIBSTDCPP:=$(shell $(CXX) -print-file-name=libstdc++.so)

$(BUILD_DIR)/stdcpp.d :
	$(Q)mkdir -p $(SYMBIYOSYS_PREFIX)/lib
	$(Q)cd `dirname $(WHICH_LIBSTDCPP)`; \
		tar cf - libstdc++.so* | (cd $(SYMBIYOSYS_PREFIX)/lib ; tar xf -)
	$(Q)touch $@

$(BUILD_DIR)/symbiyosys/bin/sby : $(SCRIPTS_DIR)/sby
	$(Q)mkdir -p `dirname $(@)`
	$(Q)cp $^ $@
	$(Q)chmod +x $@

release : $(PACKAGE) $(PACKAGES_DIR)/upload.py
	$(Q)python3 $(PACKAGES_DIR)/upload.py \
		--org EDAPack --repo symbiyosys \
		--key $(GITHUB_API_TOKEN) --version $(version) $(PACKAGE)

$(PACKAGES_DIR)/install.py :
	$(Q)mkdir -p $(PACKAGES_DIR)
	$(Q)$(WGET) -O $@ \
          $(EDAPACK_BUILD_URL)/raw/master/scripts/install.py || \
		(rm -f $@ ; exit 1)

$(PACKAGES_DIR)/upload.py :
	$(Q)mkdir -p $(PACKAGES_DIR)
	$(Q)$(WGET) -O $@ \
          $(EDAPACK_BUILD_URL)/raw/master/scripts/upload.py || \
		(rm -rf $@ ; exit 1)

clean :
	$(Q)rm -rf $(BUILD_DIR)

clean-all :
	$(Q)rm -rf $(BUILD_DIR) $(PACKAGES_DIR)


