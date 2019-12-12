Cluster-based Penalty Scaling for Robust Pose Graph Optimization
=======

This work consists of two modules. Use **git submodule update --init --recursive** to download submodules. 

### To Build:
```
git clone https://github.com/amberwood31/robust_backend.git robust_backend
cd robust_backend
mkdir build
cd build
cmake ..
make -j4
cd ..
git clone https://github.com/amberwood31/clustering.git clustering
cd clustering
mkdir build
cd build
cmake ..
make -j4
```

### To Use:
Put the **robust_backend** and **clustering** repository under the same directory so that you can use the scripts to streamline the pose graph optimization process.
Please note the clustering_input_pose_graph_file need to be sorted so that vertices are accessed in incremental manner. The sorting script **sort.py** can be found in the **/robust_backend/scripts** folder.

After building both the **robust_backend** and **clustering** code, assuming you have a pose graph file named **input.g2o** and the sorted pose graph file is named **sorted.g2o**, you can use the scripts as below.
```
cd robust_backend
mkdir ../test_backend/analysis_chamber
cp scripts/* ../test_backend/analysis_chamber
cd bin
./run_example.sh sorted.g2o input.g2o
cd ../../test_backend/analysis_chamber
python plot_g2o_with_link.py output.g2o
```

The python script **plot_g2o_with_link.py** will plot your optimized graph. 

### To Note:
The executable **backend** loads the rtabmap parameters from the file **rtabmap.ini**. By default, the relavant parameters are set as below and this enables the executable to use cluster-based penalty scaling method for robust pose graph optimization.
>Optimizer\Robust=true \
>Optimizer\Strategy=2 \
>... \
>CPS\Status=true \
>CPS\ClusterFactor=true \
>CPS\DenseFactor=true \
>CPS\PriorPenalty=0.1 \
>CPS\ClusterPenalty=1.0 \
>CPS\ClusteringResults=clustering_results.txt \
>CPS\Sigmoid=false \
>... \
>g2o\RobustKernel=true \
>g2o\RobustKernelType=1 \
>g2o\RobustKernelDelta=1 \
>... \
>Reg\Force3DoF=true 

If you want to use switchable constraint (vertigo) method for robust pose graph optimization, change the parameters as below.
>Optimizer\Robust=true \
>Optimizer\Strategy=2 \
>... \
>CPS\Status=false \
>CPS\ClusterFactor=true \
>CPS\DenseFactor=true \
>CPS\PriorPenalty=0.1 \
>CPS\ClusterPenalty=1.0 \
>CPS\ClusteringResults=clustering_results.txt \
>CPS\Sigmoid=false \
>... \
>g2o\RobustKernel=true \
>g2o\RobustKernelType=1 \
>g2o\RobustKernelDelta=1 \
>... \
>Reg\Force3DoF=true 

If you want to use dynamic covariance scaling (DCS) method for robust pose graph optimization, change the parameters as below.
>Optimizer\Robust=false \
>Optimizer\Strategy=1 \
>... \
>CPS\Status=false \
>CPS\ClusterFactor=true \
>CPS\DenseFactor=true \
>CPS\PriorPenalty=0.1 \
>CPS\ClusterPenalty=1.0 \
>CPS\ClusteringResults=clustering_results.txt \
>CPS\Sigmoid=false \
>... \
>g2o\RobustKernel=true \
>g2o\RobustKernelType=1 \
>g2o\RobustKernelDelta=1 # change this to try different kernel size\
>... \
>Reg\Force3DoF=true 

If you want to process 3D datasets, change the parameter **Reg\Force3DOF** to be false. Pose graph files need to be in g2o format.

