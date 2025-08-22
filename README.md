# L4G: Two-hop Label Management for Group Steiner Tree Search on Graphs

The introduction of these files are as follows. 



## HL-GST_data

The data files are zipped together, and should be unzipped first. There are six datasets: Musae, Twitch, Github, Amazon, DBLP, Reddit. There are 7 files for each dataset. For example, the Musae dataset contains the following 7 files. 

1. "exp_musae_info.txt". This readable file contains the basic information of this dataset, including the numbers of mock and non-mock vertices and edges in this dataset. This file also shows every edge and edge weight. For example, "Edge 0 1 1000000" shows that there is a mock edge between vertex 0 and vertex 1, with the constant mock edge weight of 1000000; and "Edge 0 4 51" shows that there is a non-mock edge between vertex 0 and vertex 4, with a non-mock edge weight of 51, which corresponds to a pairwise Jaccard distance of 0.51. Notably, mock vertices are only adjacent to non-mock vertices through mock edges, and each mock edge connects a mock vertex and a non-mock vertex. Thus, the above two edges indicate that vertex 0 and vertex 4 are non-mock vertices, while vertex 1 is a mock vertex.

The following 6 binary files can be read by codes when conducting experiments. The reason for generating binary files is that it is much faster to read binary files than to read raw data files when conducting experiments.

2. "exp_musae_new_is_mock_Jacard.binary". This binary file uses bool values to show the mock or non-mock state of each vertex.

3. "exp_musae_input_graph_Mock_Jacard.binary". This binary file contains an adjacency list of the tranformed graph with both mock and non-mock components.

4. "exp_musae_input_graph_noMock_Jacard.binary". This binary file contains an adjacency list of the original graph with only non-mock components.

5. "exp_musae_select_groups1_Jacard.binary". This binary file contains the randomly selected sets of vertex groups for querying group Steiner trees in experiments. Each set contains 3 vertex groups. 

6. "exp_musae_select_groups2_Jacard.binary". This binary file contains the randomly selected sets of vertex groups for querying group Steiner trees in experiments. Each set contains 5 vertex groups. 

7. "exp_musae_select_groups3_Jacard.binary". This binary file contains the randomly selected sets of vertex groups for querying group Steiner trees in experiments. Each set contains 7 vertex groups. 


## HL-GST_code

There are some h and cpp files for conducting experiments in the paper. The h files are at "HL-GST_code\include", while the cpp files are at "HL-GST_code\src". In particular,

- "exp_HL4GST.cpp" at "HL-GST_code\src\label_generation" contains codes for conducting the label generation experiments in the paper. 

- "exp_HL4GST_nonHOP_maintain.cpp" at "HL-GST_code\src\nonHOP_maintain" contains codes for conducting the label maintenance experiments in the paper. 

More detailed codes in other regions can be traced by the above codes for experiment. Specifically,

- "HL-GST_code\include\label_generation" contains source codes of the implemented algortihms in the label generation experiments.

- "HL-GST_code\include\nonHOP_maintain" contains source codes of the implemented algortihms in the label maintenance experiments.


## Compiling and Running the Code

We provide two methods for compiling and running the experiments: a local build using CMake and a containerized build using Docker for maximum reproducibility. All experiments were originally conducted on a Linux server with the Ubuntu 20.04.1 system.

### Method 1: Local Build with CMake

This method uses the cross-platform build tool CMake to compile the code on your local machine.

**1. Prepare the Environment**

First, ensure the necessary tools and libraries are installed on your system:

  * A **C++17 compliant compiler** (e.g., g++, Clang).
  * **CMake** (version 3.10 or higher).
  * The **Boost C++ library**. CMake will automatically detect it if installed in a standard system path.

Next, prepare the project files. Suppose your top-level project directory is `HL-GST`.

  * **Download and unzip the datasets.** The unzipped data should be at `HL-GST/HL-GST_data`.
  * **Download and unzip the codes.** The unzipped code should be at `HL-GST/HL-GST_code`.

**2. Compile the Code**

All executables are compiled using an out-of-source build to keep the source directory clean.

```bash
cd HL-GST/HL-GST_code
mkdir build
cd build
cmake ..
make
```

After a successful compilation, all executables will be located in the `HL-GST/HL-GST_code/build/` directory.

**3. Run the Experiments**

All experiments are managed by the unified `run.sh` script. This script must be executed from the `HL-GST/HL-GST_code` directory.

  * **To run the experiments of generating 2-hop labels:**

    ```bash
    sh run.sh gen-exp
    ```

  * **To run the experiments of maintaining labels:**

    ```bash
    # First, use the following command to generate the initial labels:
    sh run.sh nonhop-lppr

    # Then, use the following command to run the experiments of maintaining labels:
    sh run.sh nonhop-batch
    ```

The experiment results will be automatically saved in CSV files.

-----

### Method 2: Reproducible Build with Docker (Recommended)

This method uses Docker to create a self-contained environment with all dependencies pre-installed, guaranteeing that the code runs exactly as intended.

**1. Prepare the Environment**

  * Ensure **Docker** is installed and running on your system.
  * Arrange the project files as described in Method 1 (i.e., `HL-GST/HL-GST_code` and `HL-GST/HL-GST_data`).

**2. Build the Docker Image**

This step builds the Docker image from the provided `Dockerfile`. It only needs to be done once.

```bash
cd HL-GST/HL-GST_code
sudo docker build -t hlgst-experiment .
```

**3. Run the Experiments in a Container**

After the image is built, you can start a container to run the experiments.

First, create a local directory to store the results:

```bash
cd /path/to/your/HL-GST
mkdir results
```

Then, run the container, which will place you inside its command-line environment:

```bash
sudo docker run -it --rm \
  -v /path/to/your/HL-GST/HL-GST_data:/app/HL-GST_data \
  -v /path/to/your/HL-GST/results:/app/HL-GST_code/results \
  hlgst-experiment
```

Once inside the container, you can run the experiments using the `run.sh` script.

  * **To run the experiments of generating 2-hop labels:**

    ```bash
    sh run.sh gen-exp
    ```

  * **To run the experiments of maintaining labels:**

    ```bash
    sh run.sh nonhop-lppr
    sh run.sh nonhop-batch
    ```

Any result files will be saved in the `results` directory. To exit the container, simply type `exit`.


All the experiments in the paper are conducted via the above approaches.

