include $(MAKE_INCLUDE_GEN)

.PHONY: all clean

#*****************************************************************************#
#                               VARIABLES	                              #
#*****************************************************************************#
SHELL := /bin/bash

OUTPUT_DIR = $(CHIPDIR)/examples/refrigerator-app/ameba/build/chip

#*****************************************************************************#
#                               CFLAGS                                        #
#*****************************************************************************#

GLOBAL_CFLAGS += -DCHIP_PROJECT=1

CHIP_CFLAGS = $(GLOBAL_CFLAGS)
CHIP_CXXFLAGS += $(GLOBAL_CFLAGS)

#*****************************************************************************#
#                               Include FILE LIST                             #
#*****************************************************************************#

GLOBAL_CFLAGS += -I$(CHIPDIR)/config/ameba
GLOBAL_CFLAGS += -I$(CHIPDIR)/src/include/platform/Ameba
GLOBAL_CFLAGS += -I$(CHIPDIR)/src/include
GLOBAL_CFLAGS += -I$(CHIPDIR)/src/lib
GLOBAL_CFLAGS += -I$(CHIPDIR)/src
GLOBAL_CFLAGS += -I$(CHIPDIR)/src/system
GLOBAL_CFLAGS += -I$(CHIPDIR)/src/app
GLOBAL_CFLAGS += -I$(CHIPDIR)/third_party/nlassert/repo/include
GLOBAL_CFLAGS += -I$(CHIPDIR)/third_party/nlio/repo/include
GLOBAL_CFLAGS += -I$(CHIPDIR)/third_party/nlunit-test/repo/src

GLOBAL_CFLAGS += -I$(BASEDIR)/component/common/bluetooth/realtek/sdk/example/bt_matter_adapter

#*****************************************************************************#
#                        RULES TO GENERATE TARGETS                            #
#*****************************************************************************#

# Define the Rules to build the core targets
all: GENERATE_NINJA

GENERATE_NINJA:
	echo "INSTALL CHIP..." && \
	mkdir -p $(OUTPUT_DIR) && \
	echo                                   > $(OUTPUT_DIR)/args.gn && \
	echo "import(\"//args.gni\")"          >> $(OUTPUT_DIR)/args.gn && \
	echo target_cflags_c  = [$(foreach word,$(CHIP_CFLAGS),\"$(word)\",)] | sed -e 's/=\"/=\\"/g;s/\"\"/\\"\"/g;'  >> $(OUTPUT_DIR)/args.gn && \
	echo target_cflags_cc = [$(foreach word,$(CHIP_CXXFLAGS),\"$(word)\",)] | sed -e 's/=\"/=\\"/g;s/\"\"/\\"\"/g;'   >> $(OUTPUT_DIR)/args.gn && \
	echo ameba_ar = \"arm-none-eabi-ar\"    >> $(OUTPUT_DIR)/args.gn && \
	echo ameba_cc = \"arm-none-eabi-gcc\"   >> $(OUTPUT_DIR)/args.gn && \
	echo ameba_cxx = \"arm-none-eabi-c++\"  >> $(OUTPUT_DIR)/args.gn && \
	echo ameba_cpu = \"ameba\"               >> $(OUTPUT_DIR)/args.gn && \
	echo chip_inet_config_enable_ipv4 = "false" >> $(OUTPUT_DIR)/args.gn && \
	echo chip_enable_ota_requestor = "true" >> $(OUTPUT_DIR)/args.gn && \
	echo chip_support_enable_storage_api_audit = "false" >> $(OUTPUT_DIR)/args.gn && \
	echo chip_use_transitional_commissionable_data_provider = "true" >> $(OUTPUT_DIR)/args.gn && \
	echo chip_logging = "true" >> $(OUTPUT_DIR)/args.gn && \
	echo chip_error_logging  = "true" >> $(OUTPUT_DIR)/args.gn && \
	echo chip_progress_logging  = "true" >> $(OUTPUT_DIR)/args.gn && \
	echo chip_detail_logging = "false" >> $(OUTPUT_DIR)/args.gn && \
	cd $(CHIPDIR) && PW_ENVSETUP_QUIET=1 . scripts/activate.sh && \
	mkdir -p $(CHIPDIR)/config/ameba/components/chip && \
	cd $(CHIPDIR)/config/ameba/components/chip && gn gen --check --fail-on-unused-args $(CHIPDIR)/examples/refrigerator-app/ameba/build/chip && \
	cd $(CHIPDIR)/config/ameba/components/chip ; ninja -C $(CHIPDIR)/examples/refrigerator-app/ameba/build/chip :ameba && \
	cp -f $(OUTPUT_DIR)/lib/* $(ROOTDIR)/lib/application/

#*****************************************************************************#
#              CLEAN GENERATED FILES                                          #
#*****************************************************************************#
clean:
	echo "RM $(OUTPUT_DIR)"
	rm -rf $(OUTPUT_DIR)

-include $(DEPS)
