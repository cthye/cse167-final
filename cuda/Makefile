CUDA_PATH     ?= /usr/local/cuda
HOST_COMPILER  = g++
NVCC           = $(CUDA_PATH)/bin/nvcc -ccbin $(HOST_COMPILER)

# select one of these for Debug vs. Release
#NVCC_DBG       = -g -G
NVCC_DBG       =

NVCCFLAGS      = $(NVCC_DBG) -m64
#! replace the gencode
GENCODE_FLAGS  = -gencode arch=compute_50,code=sm_50

#!
SRCS = main.cu
INCS = vector.hpp ray.hpp camera.hpp object.hpp intersection.hpp scene.hpp material.hpp utils.hpp

cudart: cudart.o
	$(NVCC) $(NVCCFLAGS) $(GENCODE_FLAGS) -o cudart cudart.o

cudart.o: $(SRCS) $(INCS)
	$(NVCC) $(NVCCFLAGS) $(GENCODE_FLAGS) -o cudart.o -c main.cu

out.ppm: cudart
	rm -f out.ppm
	./cudart > out.ppm

out.jpg: out.ppm
	rm -f out.jpg
	ppmtojpeg out.ppm > out.jpg

profile_basic: cudart
	nvprof ./cudart > out.ppm

# use nvprof --query-metrics
profile_metrics: cudart
	nvprof --metrics achieved_occupancy,inst_executed,inst_fp_32,inst_fp_64,inst_integer ./cudart > out.ppm

clean:
	rm -f cudart cudart.o out.ppm out.jpg
