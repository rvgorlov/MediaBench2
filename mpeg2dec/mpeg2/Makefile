# Makefile for mpeg2decode / mpeg2encode

# Copyright (C) 1996, MPEG Software Simulation Group. All Rights Reserved.

#
# Disclaimer of Warranty
#
# These software programs are available to the user without any license fee or
# royalty on an "as is" basis.  The MPEG Software Simulation Group disclaims
# any and all warranties, whether express, implied, or statuary, including any
# implied warranties or merchantability or of fitness for a particular
# purpose.  In no event shall the copyright-holder be liable for any
# incidental, punitive, or consequential damages of any kind whatsoever
# arising from the use of these programs.
#
# This disclaimer of warranty extends to the user of these programs and user's
# customers, employees, agents, transferees, successors, and assigns.
#
# The MPEG Software Simulation Group does not represent or warrant that the
# programs furnished hereunder are free of infringement of any third-party
# patents.
#
# Commercial implementations of MPEG-1 and MPEG-2 video, including shareware,
# are subject to royalty fees to patent holders.  Many of these patents are
# general enough such that they are unavoidable regardless of implementation
# design.
#
#

# uncomment the following two lines if you want to include X11 support
# (mpeg2decode)

#USE_DISP = -DDISPLAY
#LIBS = -lX11

# uncomment the following two lines if you want to use shared memory
# (faster display if server and client run on the same machine)
# (mpeg2decode)

#USE_SHMEM = -DSH_MEM
#LIBS = -lXext -lX11

# if your X11 include files / libraries are in a non standard location:
# set INCLUDEDIR to -I followed by the appropriate include file path and
# set LIBRARYDIR to -L followed by the appropriate library path
# (mpeg2decode)

#INCLUDEDIR = -I/usr/openwin/include
#LIBRARYDIR = -L/usr/openwin/lib

# select one of the following CC CFLAGS settings

#REPORTS= 2> ./report-gcc-$<.lst
#REPORTS= 

#PROF= -g -pg -static
PROF=

#
# GNU gcc
#
CC= icc
#CC = gcc
#CC= clang

REPORTS= 2> ./report-$(CC)--$<.lst
#REPORTS=
#CFLAGS = -O3 -w

# GCC 
# vect
#CFLAGS= $(PROF) -O3 -ffast-math -fivopts -march=native -fopt-info-vec -fopt-info-vec-missed
# scalar
#CFLAGS = $(PROF) -O3 -ffast-math -fivopts -march=native -fno-tree-vectorize

#clang
# vect
#CFLAGS = $(PROF) -O3 -ffast-math -fvectorize -Rpass=loop-vectorize -Rpass-missed=loop-vectorize -Rpass-analysis=loop-vectorize
# scalar
#CFLAGS = $(PROF) -O3 -ffast-math -fno-vectorize

#ICC
#vect
CFLAGS = -O3 -xHost -qopt-report3 -qopt-report-phase=vec,loop -qopt-report-embed
#scalar
#CFLAGS = $(PROF) -O3 -xHost -no-vec 

LL= $(CC)

all: mpeg2decode mpeg2encode

mpeg2decode:
	cd src/mpeg2dec; make 'CC=$(CC)' \
	'CFLAGS=$(CFLAGS) $(USE_DISP) $(USE_SHMEM) $(INCLUDEDIR)' \
	'LIBS=$(LIBS)' 'LIBRARYDIR=$(LIBRARYDIR)' \
	'LL=$(LL)' 'REPORTS=$(REPORTS)' 'PROF=$(PROF)'

mpeg2encode:
	cd src/mpeg2enc; make 'CC=$(CC)' 'CFLAGS=$(CFLAGS)' 'LL=$(LL)' 'REPORTS=$(REPORTS)' 'PROF=$(PROF)'

pc:
	cd src/mpeg2dec; make pc 'CC=$(CC)' 'CFLAGS=$(CFLAGS)' 'LL=$(LL)' 'REPORTS=$(REPORTS)' 'PROF=$(PROF)'
	cd src/mpeg2enc; make pc 'CC=$(CC)' 'CFLAGS=$(CFLAGS)' 'LL=$(LL)' 'REPORTS=$(REPORTS)' 'PROF=$(PROF)'

clean:
	cd src/mpeg2dec; make clean
	cd src/mpeg2enc; make clean

test:
	cd verify; ./verify

hpctimer: hpctimer.o
	gcc -O0 -Wall -g -std=c11 -c hpctimer.c -o hpctimer.o 

hpctimer.o: hpctimer.c hpctimer.h

#diplom:
#	./src/mpeg2enc/mpeg2encode ./src/mpeg2enc/input_base_4CIF_96bps_15.par ./src/mpeg2enc/output_base_4CIF_96bps_15.mpg
#	./src/mpeg2dec/mpeg2decode -b ./src/mpeg2dec/input_base_4CIF_96bps.mpg -o3 ./src/mpeg2dec/output_base_4CIF_96bps_%03d

diplom:
	cd src/mpeg2enc; make test 
	cd src/mpeg2dec; make test

diplombig:
	cd src/mpeg2enc; make testbig 
	cd src/mpeg2dec; make testbig

#$(CC) $(CFLAGS) -o $@ -c $< 2> ./reportgcc-$<.lst 
%.o: %.c 
	$(CC) $(CFLAGS) -o $@ -c $< $(REPORTS) 


profiling: profenc profdec 

#profenc:
#	./src/mpeg2enc/mpeg2encode ./src/mpeg2enc/test.par ./src/mpeg2enc/new.m2v
#	gprof ./src/mpeg2enc/mpeg2encode ./gmon.out > ./profiling/enc_profiling.txt

#profdec:
#	./src/mpeg2dec/mpeg2decode -f -b ./src/mpeg2enc/test.m2v
#	gprof ./src/mpeg2dec/mpeg2decode ./gmon.out > ./profiling/dec_profiling.txt

profenc:
	cd src/mpeg2enc; make profiling

profdec:
	cd src/mpeg2dec; make profiling

