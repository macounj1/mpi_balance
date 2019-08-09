# mpi_balance
A batch MPI example for a PBS scheduler of balancing the number of R
instances per node and available cores per R instance. The example
combines [pbdMPI](https://github.com/RBigData/pbdMPI) (from pbdr.org)
with `mclapply()` (from the parallel package).

Modify the `-A`, `-q`, and `-W` parameters in the `hello_balance.pbs`
script to match your local requirements. The script expects both files
to be in a directory named `~/mpi_balance`. Typically, this is submitted
at the shell prompt on a cluster login node by

```{sh}
$ qsub hello_balance.pbs 
```
Note that other multithreaded R packages using multiple cores are
managed similarly but each may have its own parameters that specify
the number of cores used. Some may require setting environment
variables. 

Care must be taken to prevent oversubscribing available
cores and slowing things down. In particular, multithreaded BLAS (such
as OpenBLAS) may by default use all available cores on a node,
conflicting with other on-node MPI instances or processes forked by
mclapply. See also wrathematics/openblasctl on GitHub for OpenBLAS
thread control from R.

While this script illustrates how to manage availability of cores to
MPI ranks, the actual placement of threads is done by the OS. The OS
does a pretty good job in our experience, although there can be a
difference between theory and reality as is illustrated in
this [Multithreaded Programming post on reddit](https://www.reddit.com/r/aww/comments/2oagj8/multithreaded_programming_theory_and_practice/).
