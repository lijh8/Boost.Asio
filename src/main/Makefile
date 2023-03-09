# Makefile for subdir

# build shared library with -fPIC, -shared
CXXFLAGS  = # -g -O3 -fPIC # CXXFLAGS for .cpp
CPPFLAGS  = -MMD -MP -I/home/ljh/Downloads/boost_1_81_0/
LDFLAGS   = -L/home/ljh/Downloads/boost_1_81_0/stage/lib # -L$(OBJDIR)/foo # -shared
LDLIBS    = -lpthread
# CC      = $(CXX) # link with CXX for .cpp

LDFLAGS  += -Wl,-rpath,$(OBJDIR)/foo
LDFLAGS  += -Wl,-rpath,'$$ORIGIN/../lib'
#LDFLAGS += -Wl,-soname,$(soname)

# make # NDEBUG=1
ifdef NDEBUG
CPPFLAGS += -DNDEBUG
CXXFLAGS   += -O3 # .cpp
else
CXXFLAGS   += -g # .cpp
LDFLAGS  += -fsanitize=address
endif

SUBDIR    = $(OBJDIR)/main

all : $(SUBDIR)/server $(SUBDIR)/client

# https://make.mad-scientist.net/papers/how-not-to-use-vpath/
# $(SUBDIR)/main : $(addprefix $(SUBDIR)/,$(patsubst %.c,%.o,$(wildcard *.c))) # .cpp
# 	$(CC) $(LDFLAGS) $^ $(LDLIBS) -o $@

$(SUBDIR)/server : $(addprefix $(SUBDIR)/,$(patsubst %.cpp,%.o,server.cpp)) # .cpp
	$(CXX) $(LDFLAGS) $^ $(LDLIBS) -o $@

$(SUBDIR)/client : $(addprefix $(SUBDIR)/,$(patsubst %.cpp,%.o,client.cpp)) # .cpp
	$(CXX) $(LDFLAGS) $^ $(LDLIBS) -o $@

$(SUBDIR)/%.o : %.cpp | $(SUBDIR) # .cpp
	$(CXX) $(CXXFLAGS) $(CPPFLAGS) -c -o $@ $<

$(SUBDIR) : ; @ mkdir $@

-include $(SUBDIR)/*.d
clean : ; -rm -fr $(SUBDIR)
.PHONY : all clean