###############################################################################
# Makefile Template
# Andrew Tu
#
###############################################################################


###############################################################################
# Important Variables
###############################################################################
SRC_DIR := src
RELEASE_DIR := release
TIMING_DIR := timing
DEBUG_DIR := debug

# Compilers
CXX := g++
CXX_FLAGS := -std=gnu++11 #-fopenmp

RELEASE_FLAGS := -o3
DEBUG_FLAGS := -g3

INCLUDES := -I include
LIBS := -lpthread
#LIBS :=

###############################################################################
# Sources List + Object List Creation
###############################################################################

# A list of source files for the executable we want to build
EXEC_SRCS := \
  $(SRC_DIR)/wordlist.cpp\
  $(SRC_DIR)/grid.cpp \
  $(SRC_DIR)/wordSearch.cpp\
  $(SRC_DIR)/main.cpp

TIMING_SRCS := \
  $(SRC_DIR)/wordlist.cpp\
  $(SRC_DIR)/grid.cpp \
  $(SRC_DIR)/wordSearch.cpp\
  $(SRC_DIR)/timingMain.cpp

# An self generating list of objects for each of the objects needed in the 
# executable
RELEASE_EXEC_OBJS += $(patsubst $(SRC_DIR)/%.cpp, $(RELEASE_DIR)/%.o, \
	     $(filter $(SRC_DIR)/%.cpp, $(EXEC_SRCS)))

DEBUG_EXEC_OBJS += $(patsubst $(SRC_DIR)/%.cpp, $(DEBUG_DIR)/%.o, \
	     $(filter $(SRC_DIR)/%.cpp, $(EXEC_SRCS)))

TIMING_EXEC_OBJS += $(patsubst $(SRC_DIR)/%.cpp, $(TIMING_DIR)/%.o, \
	     $(filter $(SRC_DIR)/%.cpp, $(TIMING_SRCS)))

.PHONY: \
  all \
  release \
  debug \
  timing \
  clean \
  cleanall \
  tests

###############################################################################
# Build Commands 
###############################################################################
all : \
  debug\
  timing\
  release\
  tests

release :\
 	$(RELEASE_DIR)/Driver

debug :\
  	$(DEBUG_DIR)/Driver

timing :\
	$(TIMING_DIR)/Driver

tests:
	(cd test && make)

clean :
	@rm -rf $(DEBUG_DIR) $(RELEASE_DIR) $(TIMING_DIR) tmp 
	(cd test && make clean)

cleanall : clean
	@rm -rf .googletest
	
	

# The build sequence for the particular executable we want to build. 
$(RELEASE_DIR)/Driver: $(RELEASE_EXEC_OBJS)
	@echo $(RELEASE_EXEC_OBJS)
	$(CXX) $(CXX_FLAGS) $(RELEASE_FLAGS) $(INCLUDES) $(LIBS) -o $@ $^

$(DEBUG_DIR)/Driver: $(DEBUG_EXEC_OBJS)
	@echo $(DEBUG_EXEC_OBJS)
	$(CXX) $(CXX_FLAGS) $(DEBUG_FLAGS) $(INCLUDES) $(LIBS) -o $@ $^

$(TIMING_DIR)/Driver: $(TIMING_EXEC_OBJS)
	@echo $(TIMING_EXEC_OBJS)
	$(CXX) $(CXX_FLAGS) $(DEBUG_FLAGS) $(INCLUDES) $(LIBS) -o $@ $^


###############################################################################
# Dependencies 
###############################################################################

# Build the .o file for each src file
$(RELEASE_DIR)/%.o : $(SRC_DIR)/%.cpp | release_directory
	@echo $(RELEASE_EXEC_OBJS)
	$(CXX) $(CXX_FLAGS) $(RELEASE_FLAGS) $(INCLUDES) -c $< -o $@

$(DEBUG_DIR)/%.o : $(SRC_DIR)/%.cpp | debug_directory
	$(CXX) $(CXX_FLAGS) $(DEBUG_FLAGS) $(INCLUDES) -c $< -o $@

$(TIMING_DIR)/%.o : $(SRC_DIR)/%.cpp | timing_directory 
	$(CXX) $(CXX_FLAGS) $(DEBUG_FLAGS) $(INCLUDES) -c $< -o $@

# Autogen dependencies
$(RELEASE_DIR)/%.d : $(SRC_DIR)/%.cpp
	@echo "Running Autogen..."
	@set -e;\
	rm -f $@;\
	$(CXX) -MM $(CXX_FLAGS) $(INCLUDES) $< > $@.$$$$;\
	sed 's,\($*\)\.o[ :]*,$(RELEASE_DIR)/\1.o $@ : ,g' < $@.$$$$ > $@; \
	rm -f $@.$$$$

$(DEBUG_DIR)/%.d : $(SRC_DIR)/%.cpp
	@echo "Running Autogen..."
	@set -e;\
	rm -f $@;\
	$(CXX) -MM $(CXX_FLAGS) $(INCLUDES) $< > $@.$$$$;\
	sed 's,\($*\)\.o[ :]*,$(DEBUG_DIR)/\1.o $@ : ,g' < $@.$$$$ > $@; \
	rm -f $@.$$$$

$(TIMING_DIR)/%.d : $(SRC_DIR)/%.cpp
	@echo "Running Autogen..."
	@set -e;\
	rm -f $@;\
	$(CXX) -MM $(CXX_FLAGS) $(INCLUDES) $< > $@.$$$$;\
	sed 's,\($*\)\.o[ :]*,$(TIMING_DIR)/\1.o $@ : ,g' < $@.$$$$ > $@; \
	rm -f $@.$$$$

# Create the build directories
release_directory :
	@echo "Make Release Dir"
	@mkdir -p $(RELEASE_DIR)

debug_directory :
	@mkdir -p $(DEBUG_DIR)

timing_directory :
	@mkdir -p $(TIMING_DIR)

-include $(OBJS:.o=.d)
