# Compile OpenFOAM v2112 on Lehigh Sol
1. Download OpenFOAM v2112
```bash
cd $HOME
mkdir OpenFOAM
cd OpenFOAM
git clone https://develop.openfoam.com/Development/openfoam.git
mv openfoam OpenFOAM-v2112
cd OpenFOAM-v2112
git checkout OpenFOAM-v2112
wget https://github.com/BrushXue/OztekinGroup/blob/main/OpenFOAM-Sol.patch
git apply OpenFOAM-Sol.patch
git submodule init
git submodule update
```

2. Environment Setup
```bash
echo "module load gcc" >> $HOME/.bashrc
echo "module load mpich" >> $HOME/.bashrc
echo "source $HOME/OpenFOAM/OpenFOAM-v2112/etc/bashrc" >> $HOME/.bashrc
source $HOME/.bashrc
# These modules are used during compilation
# Load them in SLURM scripts as needed
module load cmake boost adios2 fftw hypre kahip metis petsc scotch
```

3. Compile OpenFOAM
```bash
foam
export MPI_ARCH_PATH="/share/Apps/lusoft/opt/spack/linux-centos8-haswell/gcc-9.3.0/mpich/3.3.2-rc3lxvt"
# Max 2 cores on the head node
./Allwmake -j 2 -s -l
# Run twice then check the log file
./Allwmake -j 2 -s -l
wmRefresh
```

4. swak4Foam (Optional)

# Sample SLURM Script
```bash
#!/bin/bash
#SBATCH --partition=enge
#SBATCH -t 48:00:00
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=36
#SBATCH --job-name YOUR_JOBNAME
#SBATCH --mail-type=ALL
#SBATCH --mail-user=YOUR_EMAIL

source /etc/profile.d/zlmod.sh
module load gcc
module load mpich
source /home/YOUR_USERNAME/OpenFOAM/OpenFOAM-v2112/etc/bashrc

cd ${SLURM_SUBMIT_DIR}
decomposePar
srun renumberMesh -overwrite -parallel > renumberMesh.log
srun viscoelasticFluidFoam -parallel > viscoelasticFluidFoam.log
```
