# README

This document provides instructions on how to build and use each artifact used
by our original paper submission (paper title and information provided after acceptance).

**NOTE**: All the instructions in this document are tested with Ubuntu 20.04.
**NOTE**: Every command sequence provided should be run starting from the root
of this repository (SC-AD).
Directory changes are included within the command code blocks in this document.

Below is a table of contents included in this document.

* [Downloading the Artifacts](#downloading-the-artifacts)
* [Retrieving Input Sets](#retrieving-input-sets)
* [Building Our Input Processing Utilities](#building-our-input-processing-utilities)
  * [Building GraphSeparator](#building-graphseparator)
  * [Building GraphReader](#building-graphreader)
  * [Building SlicedGraphReader](#building-slicedgraphreader)
  * [Creating Sorted Edgelists (SEL)](#creating-sorted-edgelists-sel)
  * [Creating Partitioned Edgelists (PEL)](#creating-partitioned-edgelists-pel)
* [Building our models](#building-our-models)
  * [Installing Dependencies Required by gem5](#installing-dependencies-required-by-gem5)
  * [Compiling gem5](#compiling-gem5)
*

## Downloading the Artifacts

To download the artifacts created by the original paper, please run the following
commands in a command line environment.
Each artifact is added as a submodule to this repository.
The following commands will clone this repository along with its submodules.

```sh
git clone https://www.github.com/mahyarsamani/sc-ad.git
cd sc-ad
git submodule init
git submodule update --remote
```

## Retrieving Input Sets

Running the following commands will download the real graphs used by the original
paper as well as generate the synthetic graphs.
The following commands will retrieve the graphs in their original order, may or
may not be sorted by source id.
Therefore, for correct simulation, you should not skip any of the next steps
mentioned in this document.

**NOTE**: As part of the process it will download a copy of the GAPBS repository
to use its synthetic graph generator.
**NOTE**: These commands might require extend time to download/create the graphs.
The required time varies based on your internet/processing speed.

```sh
cd artifact_1_input_processing
./retrieve_graphs.sh
```

The following commands should create a directory under `artifact_1_input_processing/graphs`.
The graphs will be stored under subdirectories in the mentioned directory.
The layout of the directories follow a pattern like `artifact_1_input_processing/graphs/{graph name}/el.txt`

## Building Our Input Processing Utilities

Please follow the instructions in this section to build our graph input processing
utilities.

As mentioned in the artifact description (AD) submission, GraphSorter is a
python script and does not require building.

### Building GraphSeparator

Please run the following command to build our tool GraphSeparator.

```sh
cd artifact_1_input_processing
cd GraphSeparator
make
```

The following command will create an executable under `artifacts_1_input_processing/GraphSeparator/separator`.

### Building GraphReader

Please run the following command to build our tool GraphReader.

```sh
cd artifact_1_input_processing
cd GraphReader
make
```

The following command will create an executable under `artifacts_1_input_processing/GraphReader/loader`.

### Building SlicedGraphReader

Please run the following command to build our tool GraphReader.

```sh
cd artifact_1_input_processing
cd SlicedGraphReader
make
```

The following command will create an executable under `artifacts_1_input_processing/SlicedGraphReader/loader`.

### Creating Sorted Edgelists (SEL)

To create the sorted edgelist for all the graphs, please run the following commands.

```sh
cd artifact_1_input_processing
./create_sel.sh
```

The following command will create the sorted edgelist file for all the graphs under
`artifact_1_input_processing/graphs/{graph name}/sel.txt`.
As mentioned by the AD submission the process of sorting edgelists could be quite
time consuming, there for to limit the sorting process a certain graph(s), you can
specify the name of the graphs using `--graphs {comma separated list of graph names}`
option.

### Creating Partitioned Edgelists (PEL)

To create the partitioned edgelist for all the graphs, please run the following commands.

```sh
cd artifact_1_input_processing
./create_pel.sh
```

The following command will create the partitioned edgelist files (a main file, and a mirrors file) for all the graphs under
`artifact_1_input_processing/graphs/{graph name}/pel_main.txt` and `artifact_1_input_processing/graphs/{graph name}/pel_mirrors.txt`.

**NOTE**: The script under `artifact_1_input_processing/create_pel.sh` uses the
sorted edgelist for each graph as input.
Therefore, if you have limited the graphs in [SEL creation step](#creating-sorted-edgelists-sel), make sure to limit the graph for `create_pel`
by using `--graphs {comma separated list of graph names}` option.

### Creating MIM for CFGraph

To create the memory images for all the graphs, please run the following commands.

```sh
cd artifact_1_input_processing
./create_mim.sh
```


## Building Our Models

To build our models you need to follow that same instructions as provided by
the gem5 website.
Below is a quick primer on installing the dependencies required by gem5 and
building gem5.

### Installing Dependencies Required by gem5

The following command will install all the dependencies required for building
the gem5 binary, including the optional ones.

```sh
sudo apt install build-essential git m4 scons zlib1g zlib1g-dev \
    libprotobuf-dev protobuf-compiler libprotoc-dev libgoogle-perftools-dev \
    python3-dev python-is-python3 libboost-all-dev pkg-config gcc-10 g++-10 \
    python3-tk
```

### Compiling gem5

The following command will compile the adequate gem5 binary for our simulations.
**NOTE**: The following command uses `64` threads to build the gem5 binary,
you can change the number of used threads by changing number following `-j` option.

```sh
cd artifact_2_model
scons build/NULL/gem5.opt -j 64 // replace 64 with desired number of threads
```

The following
