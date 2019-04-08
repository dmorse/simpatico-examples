
This repository contains examples of input files for simpatico simulations 
of a variety of simple systems. Many of the examples involve polymer liquids.

Each top level subdirectory contains one or more examples of simulations of a
single type of physical system. The relevant type of system is indicated by 
the sudirectory name and described in more detail in the README file in each 
such subdirectory. Lower level subdirectories often contain simulations 
performed using different simulation programs, which are indicated by 
directory names such as "mc/" for an mcSim Monte-Carlo (MC) simulation "md/" 
for an mdSim molecular dynamics (MD) simulation and "dd/" for a ddSim parallel
domain-decomposition MD simulation.

-------------------------------------------------------------------------------
Example Directory Structure:

A directory containing an example for a simulation of a single system 
typically looks like this:

   param
   command
   commrst
   run
   clean
   in/
     config
   out/
   rst/

The file "param" is the input parameter file, which the main executable reads 
from standard input. The file "command" is the command script. The file 
"in/config" is the input configuration file. The files "run" and "clean" are
bash scripts that can be used to run a simulation and to remove all files
generated by a simulation, respectively. The directories out/ and rst/ are
locations in which output files are created by the simulation.

In some examples in which an input configuration is shared by several examples
that use different simulation algorithms, the in/ directory is further up the 
directory hierarchy.

---------------------------------------------------------------------------
Example Directory Structure:

The file named "run" in each example directory is a bash script that a user
may invoke to run the example. In the simplest cases, this is done by invoking

   > ./run 

in order to run the simulation and echo log file output to the screen, or 

   > ./run log

in order to run the simulation and write log output to a file named "log". 

In almost all cases, the "run" script runs a simulation that periodically
outputs a binary restart file and then restarts the simulation and runs it
for some additional number of MD steps or MC moves. Files that are produced
by the original simulation are installed in the out/ directory. Files that
are created by the restarted simulation are installed in the rst/ directory.

In some examples the names of the param and (in a few cases) the command files 
may sometimes contain suffixes after "param" and "command".  For example, in
MD simulations, there may exist files param.nvt and param.nve intended for 
use in NVT and NVE simulations of the same system.  Some MC examples instead
have different parameter and command files that illustrate the use of 
different MC moves, which are distinguished by names such as "atomic" (for
atom displacement moves) or "hybrid" (for hybrid MC/MD moves).  In directories
in which the parameter file names have such suffixes, one may choose among
different parameter files by using the suffix as a second argument to the
run command, after an argument that specifies the name of a log file. For
example, in an MD simulation example with parameter files named param.nvt 
and param.nve one can invoke 

  > ./run log nve

to run the nve example. Invoking the run script without such a second 
command lie parameter (or with no parameters) runs a default choice, which
for MD simulations is usually an NVT simulation.

---------------------------------------------------------------------------
Invoking a simulation program:

Users who wish to run simulations directly from the command line, by invoking
the appropriate simulation command, should look in the "run" script to see
the appropriate syntax. 

The syntax for a serial program is usually something like:

   mcSim -q -e -p param -c command > log

for a serial MC simulation, or

   mcSim -q -e -p param -c command > log

for a serial MD simulation. The parameter of the "-p" option is the name of
the parameter file and the parameter of the "-c" option is the name of the
command file. The parameter file name "param" may be replaced by a name with 
a suffix, such as param.nve, as needed. 

To run a parallel program, the executable name must be preceded by a command
to run an MPI job, which is usually named mpirun. For example, the command

   mpirun -np 8 ddSim -q -e -p param -c command

would run a parallel ddSim MD simulation on 8 processors. In a ddSim simulation, 
the number of processors in the mpi command must be consistent with the number 
of processors in the grid defined in the param file.

