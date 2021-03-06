

# Remove the first character in the following line if compile with BOOST Graph library
BOOST = -DLINUX -DLONGLONG_COST -DWIDE_STRING -DLONGDOUBLE_PROB

# Same thing for XML
#XML = XMLFLAG
#XMLI = -I/usr/include/libxml2 -DXMLFLAG
# -DXMLFLAG -DMAXCSP

ifeq ($(BOOST),-DBOOST)
	SRCS = $(SRCSFULL)
else
	SRCS = $(SRCSFULL:tb2boostgraph.cpp=)
endif

ifeq ($(XML),XMLFLAG)
	OBJS = $(SRCS:.cpp=.o) -lxml2 -lgmp
#	-lxml2 -lz -lm -lpthread
else
	OBJS = $(SRCS:.cpp=.o) -lgmp
endif


CC = g++
ICC = icc


ifeq ($(findstring aiopt, $(HOSTNAME)), aiopt)
	ILOGBASE  = /usr/ILOG/CPLEX_Studio123
#	ILOGBASE  = /usr/ILOG/CPLEX_Studio124
else
	#ILOGBASE  = /opt/ibm/ILOG/CPLEX_Studio_Academic123
	ILOGBASE  = /opt/ohpc/pub/ibm/ILOG/CPLEX_Studio128/
endif
#ILOGLIBFORMAT = x86-64_sles10_4.1/static_pic
ILOGLIBFORMAT = x86-64_linux/static_pic
ILOGFLAGS = -DILOGLUE -DIL_STD -I$(ILOGBASE)/cpoptimizer/include/ -I$(ILOGBASE)/concert/include/ -I$(ILOGBASE)/cplex/include/
ILOGLIBS  = -L$(ILOGBASE)/cpoptimizer/lib/$(ILOGLIBFORMAT) -L$(ILOGBASE)/concert/lib/$(ILOGLIBFORMAT) -L$(ILOGBASE)/cplex/lib/$(ILOGLIBFORMAT) -lcp -lilocplex -lcplex -lconcert -lm -lpthread

CFLAGS = -g -Wall $(ILOGFLAGS) $(BOOST) $(XMLI) -DNARYCHAR -DWCSPFORMATONLY


ICCFLAGS = 
#-O3 -ipo -static -DNDEBUG

PFLAGS = 

# -pg -g3 # use these options to catch numerical overflows

BUILD = $(CC) -shared

default: clean AdaWish

%.o : %.cpp %.hpp
	$(CC) $(OFLAGS) $(CFLAGS) $(LIBFLAGS) -c -o $@  $< $(PFLAGS) -ldl

%.o : %.cpp %.h
	$(CC) $(OFLAGS) $(CFLAGS) $(LIBFLAGS) -c -o $@  $< $(PFLAGS) -ldl

%.o : %.cpp
	$(CC) $(OFLAGS) $(CFLAGS) $(LIBFLAGS) -c -o $@  $< $(PFLAGS) -ldl


AdaWish: AdaWISH_cplex.cpp 
	mkdir -p ../bin/
	mkdir -p ../log/
	mkdir -p ../data/
	$(CC) $(OFLAGS) $(CFLAGS) -o ../bin/$@ $< $(ILOGLIBS) -L. -lgmp -ldl

.PHONY: clean
clean:
	rm -f ../bin/AdaWish
