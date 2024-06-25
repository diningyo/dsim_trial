default: sim_1path

WAVE_DUMP   ?= 0
WAVE_FORMAT ?= mxd
# verilog or mixed.
SRC_TYPE    ?= verilog

DSIM_VERSION := 20240422.4.0

WAVE_FILE := wave.$(WAVE_FORMAT)

SIM_IMAGE  := sim_image
SIM_TOP    := work.carry_lookahead_adder_tb

COMPILE_OPTION :=
SIM_OPTION     :=

ifeq "$(WAVE_DUMP)" "1"
COMPILE_OPTION += +acc
SIM_OPTION     += +acc -waves $(WAVE_FILE)
endif

# choose project direcotry
PRJ_DIR := SystemVerilog

ifeq "$(SRC_TYPE)" "mixed"
PRJ_DIR := VHDL_SystemVerilog
endif

SIM_WORK_DIR := carry_lookahead_adder-main/$(PRJ_DIR)/sim
MANIFEST_FILE = $(SIM_WORK_DIR)/dsim_work/sfe/work/$(DSIM_VERSION)/manifest
SIM_IMG_FILE := $(SIM_WORK_DIR)/dsim_work/$(SIM_IMAGE).so

# choose source code list files
VERILOG_RTL_SRCS_LIST := filelist_SV.txt
VHDL_RTL_SRCS_LIST    := filelist_VHDL.txt
TB_SRCS  := tb/tb.sv

ifeq "$(SRC_TYPE)" "verilog"
VERILOG_RTL_SRCS_LIST := filelist.txt
endif

# download tutorial data
.PHONY: get_tutorial
URL_TUTORIAL := https://github.com/metrics-ca/carry_lookahead_adder/archive/refs/heads/main.zip
carry_lookahead_adder-main/.done:
	wget $(URL_TUTORIAL)
	unzip main.zip
	touch $@

get_tutorial: carry_lookahead_adder-main/.done

#
# DSim : simulation with 1-path flow
#  1-path flow can't include VHDL source code.
#  So SRC_TYPE must be verilog.
#
.PHONY: sim_1path
sim_1path: carry_lookahead_adder-main/.done
	cd $(SIM_WORK_DIR) && \
	dsim $(SIM_OPTION) -F $(VERILOG_RTL_SRCS_LIST)

#
# DSim : simulation with 2-path flow
#
.PHONY: compile sim_2path
$(MANIFEST_FILE):
ifeq "$(SRC_TYPE)" "mixed"
	cd $(SIM_WORK_DIR) && \
	dvhcom $(COMPILE_OPTION) -F $(VHDL_RTL_SRCS_LIST)
endif
	cd $(SIM_WORK_DIR) && \
	dvlcom $(COMPILE_OPTION) -F $(VERILOG_RTL_SRCS_LIST)

compile: $(MANIFEST_FILE)

sim_2path: compile
	cd $(SIM_WORK_DIR) && \
	dsim $(SIM_OPTION) -top $(SIM_TOP)

#
# DSim : simulation with 3-path flow
#
.PHONY: elaborate sim_3path
$(SIM_IMG_FILE): $(MANIFEST_FILE)
	cd $(SIM_WORK_DIR) && \
	dsim $(COMPILE_OPTION) -genimage $(SIM_IMAGE) -top $(SIM_TOP)

elaborate: $(SIM_IMG_FILE)

sim_3path: elaborate
	cd $(SIM_WORK_DIR) && \
	dsim $(SIM_OPTION) -image $(SIM_IMAGE)

.PHONY: clean
clean:
	cd $(SIM_WORK_DIR) && \
	$(RM) -rf *.env *.log dsim_work metrics.db wave.*
