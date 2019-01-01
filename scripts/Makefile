
SCRIPTS_DIR=$(abspath $(dir $(lastword $(MAKEFILE_LIST))))
EDAPACK_SYMBIYOSYS_DIR=$(abspath $(SCRIPTS_DIR)/..)
PACKAGES_DIR=$(EDAPACK_SYMBIYOSYS_DIR)/packages
BUILD_DIR=$(EDAPACK_SYMBIYOSYS_DIR)/build

YOSYS_VERSION=0.8
YOSYS_DIR=yosys-master
YOSYS_ZIP=$(YOSYS_DIR).zip
YOSYS_URL=https://github.com/YosysHQ/yosys/archive/master.zip

YICES2_VERSION=2.6.1
YICES2_DIR=yices2-Yices-$(YICES2_VERSION)
YICES2_ZIP=$(YICES2_DIR).zip
YICES2_URL=https://github.com/SRI-CSL/yices2/archive/Yices-$(YICES2_VERSION).zip

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
SUPER_PROVE_URL=https://downloads.bvsrc.org/super_prove/super_prove-hwmcc17_final-2-d7b71160dddb-CentOS_6.7-Release.tar.gz

EXTAVY_URL=https://bitbucket.org/arieg/extavy.git

SYMBIYOSYS_PREFIX=$(BUILD_DIR)/symbiyosys/symbiyosys

ifeq (true,$(VERBOSE))
Q=
WGET=wget
UNZIP=unzip -o
UNTARGZ=tar xvzf
else
Q=@
WGET=wget -q
UNZIP=unzip -o -q
UNTARGZ=tar xzf
endif



all : \
	$(BUILD_DIR)/z3.d \
	$(BUILD_DIR)/yices2.d \
	$(BUILD_DIR)/yosys.d \
	$(BUILD_DIR)/boolector.d \
	$(BUILD_DIR)/sby.d \
	$(BUILD_DIR)/super_prove.d \
	$(BUILD_DIR)/symbiyosys/bin/sby


$(BUILD_DIR)/yosys.d : \
		$(PACKAGES_DIR)/$(YOSYS_ZIP) \
		$(PACKAGES_DIR)/$(ABC_ZIP)
	$(Q)rm -rf $(BUILD_DIR)/yosys
	$(Q)mkdir -p $(BUILD_DIR)/yosys
	$(Q)cd $(BUILD_DIR)/yosys ; $(UNZIP) $(PACKAGES_DIR)/$(YOSYS_ZIP)
	$(Q)cd $(BUILD_DIR)/yosys ; $(UNZIP) $(PACKAGES_DIR)/$(ABC_ZIP)
	$(Q)cd $(BUILD_DIR)/yosys ; mv $(ABC_DIR) $(YOSYS_DIR)/abc
	$(Q)cd $(BUILD_DIR)/yosys/$(YOSYS_DIR) ; \
		$(MAKE) PREFIX=$(SYMBIYOSYS_PREFIX) \
			ABCPULL=0 ABCREV=default VERBOSE=$(Q)
	$(Q)cd $(BUILD_DIR)/yosys/$(YOSYS_DIR) ; \
		$(MAKE) PREFIX=$(SYMBIYOSYS_PREFIX) \
			ABCPULL=0 ABCREV=default VERBOSE=$(Q) install
	$(Q)touch $@

$(BUILD_DIR)/yices2.d : $(PACKAGES_DIR)/$(YICES2_ZIP)
	$(Q)rm -rf $(BUILD_DIR)/yices2
	$(Q)mkdir -p $(BUILD_DIR)/yices2
	$(Q)cd $(BUILD_DIR)/yices2 ; $(UNZIP) $^
	$(Q)cd $(BUILD_DIR)/yices2/$(YICES2_DIR) ; autoconf
	$(Q)cd $(BUILD_DIR)/yices2/$(YICES2_DIR) ; \
		./configure --prefix=$(SYMBIYOSYS_PREFIX)
	$(Q)cd $(BUILD_DIR)/yices2/$(YICES2_DIR) ; $(MAKE)
	$(Q)cd $(BUILD_DIR)/yices2/$(YICES2_DIR) ; $(MAKE) install
	$(Q)touch $@

$(PACKAGES_DIR)/$(YICES2_ZIP) :
	$(Q)mkdir -p $(PACKAGES_DIR)
	$(Q)$(WGET) -O $@ $(YICES2_URL)

$(BUILD_DIR)/z3.d : $(PACKAGES_DIR)/$(Z3_ZIP)
	$(Q)rm -rf $(BUILD_DIR)/z3
	$(Q)mkdir -p $(BUILD_DIR)/z3
	$(Q)cd $(BUILD_DIR)/z3 ; $(UNZIP) $(PACKAGES_DIR)/$(Z3_ZIP)
	$(Q)cd $(BUILD_DIR)/z3/$(Z3_DIR) ; $(UNZIP) $(PACKAGES_DIR)/$(Z3_ZIP)
	$(Q)cd $(BUILD_DIR)/z3/$(Z3_DIR) ; \
		./configure --prefix=$(SYMBIYOSYS_PREFIX) 
	$(Q)cd $(BUILD_DIR)/z3/$(Z3_DIR)/build ; $(MAKE)
	$(Q)cd $(BUILD_DIR)/z3/$(Z3_DIR)/build ; $(MAKE) install
	$(Q)touch $@

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

$(BUILD_DIR)/symbiyosys/bin/sby : $(SCRIPTS_DIR)/sby
	$(Q)mkdir -p `dirname $(@)`
	$(Q)cp $^ $@
	$(Q)chmod +x $@

clean :
	$(Q)rm -rf $(BUILD_DIR)
