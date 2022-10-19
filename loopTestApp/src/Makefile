TOP=../..

include $(TOP)/configure/CONFIG
#----------------------------------------
#  ADD MACRO DEFINITIONS AFTER THIS LINE
#=============================

#==================================================
# build a support library

LIBRARY_IOC += loopTest

# xxxRecord.h will be created from xxxRecord.dbd
#DBDINC += xxxRecord
# install loopTest.dbd into <top>/dbd
DBD += loopTest.dbd

# specify all source files to be compiled and added to the library
#loopTest_SRCS += xxx

loopTest_LIBS += $(EPICS_BASE_IOC_LIBS)

#===========================

include $(TOP)/configure/RULES
#----------------------------------------
#  ADD RULES AFTER THIS LINE

