Write a MPI Program demonstration of MPI_Scatter and MPI_Gather.
Program 1
Code:
#include <mpi.h>
#include <stdio.h>
#include <stdlib.h>
int main(int argc, char **argv) {
int size, rank;
MPI_Init(&argc, &argv);
MPI_Comm_size(MPI_COMM_WORLD, &size);
MPI_Comm_rank(MPI_COMM_WORLD, &rank);
int globaldata[4]; /* wants to declare array this way */
int localdata[4]; /* without using pointers */
int i;
if (rank == 0) {
for (i = 0; i < size; i++)
globaldata[i] = i;
printf("1. Processor %d has data: ", rank);
for (i = 0; i < size; i++)
printf("%d ", globaldata[i]);
printf("\n");
}
MPI_Scatter(globaldata, 1, MPI_INT, &localdata,
1, MPI_INT, 0, MPI_COMM_WORLD);
printf("2. Processor %d has data %d\n",
rank, localdata[rank]);
localdata[rank] = 5;
printf("3. Processor %d now has %d\n",
rank, localdata[rank]);
MPI_Gather(&localdata, 1, MPI_INT, globaldata,
1, MPI_INT, 0, MPI_COMM_WORLD);
if (rank == 0) {
printf("4. Processor %d has data: ", rank);
for (i = 0; i < size; i++)
Parallel Computing (BCS702) - Lab Experiment 8 Page 1
printf("%d ", globaldata[i]);
printf("\n");
}
MPI_Finalize();
return 0;
}
Output:
ubantu@ubantu-HP-Pro-Tower-280-G9-PCI-Desktop-PC:~$
mpicc -g -o MPI scattergather.c
ubantu@ubantu-HP-Pro-Tower-280-G9-PCI-Desktop-PC:~$
mpirun -np 4 ./MPI
1. Processor 0 has data: 0 1 2 3
2. Processor 0 has data 0
3. Processor 0 now has 5
2. Processor 1 has data 0
3. Processor 1 now has 5
2. Processor 2 has data 0
3. Processor 2 now has 5
2. Processor 3 has data 0
3. Processor 3 now has 5
2. Processor 0 has data: 1 2 3 4
Parallel Computing (BCS702) - Lab Experiment 8 Page 2
Program 2
Code:
#include <mpi.h>
#include <stdio.h>
#include <stdlib.h>
int main(int argc, char **argv) {
int size, rank;
MPI_Init(&argc, &argv);
MPI_Comm_size(MPI_COMM_WORLD, &size);
MPI_Comm_rank(MPI_COMM_WORLD, &rank);
int globaldata[4]; /* wants to declare array this way */
int localdata; /* without using pointers */
int i;
if (rank == 0) {
for (i = 0; i < size; i++)
globaldata[i] = i;
printf("1. Processor %d has data: ", rank);
for (i = 0; i < size; i++)
printf("%d ", globaldata[i]);
printf("\n");
}
MPI_Scatter(globaldata, 1, MPI_INT, &localdata,
1, MPI_INT, 0, MPI_COMM_WORLD);
printf("2. Processor %d has data %d\n", rank, localdata);
localdata = 5;
printf("3. Processor %d now has %d\n", rank, localdata);
MPI_Gather(&localdata, 1, MPI_INT, globaldata,
1, MPI_INT, 0, MPI_COMM_WORLD);
if (rank == 0) {
printf("4. Processor %d has data: ", rank);
for (i = 0; i < size; i++)
printf("%d ", globaldata[i]);
printf("\n");
}
MPI_Finalize();
return 0;
}
Parallel Computing (BCS702) - Lab Experiment 8 Page 3
Output:
ubantu@ubantu-HP-Pro-Tower-280-G9-PCI-Desktop-PC:~$
mpicc -g -o MPI scattergather.c
ubantu@ubantu-HP-Pro-Tower-280-G9-PCI-Desktop-PC:~$
mpirun -np 4 ./MPI
1. Processor 0 has data: 0 1 2 3
2. Processor 0 has data 0
3. Processor 0 now has 5
2. Processor 1 has data 1
3. Processor 1 now has 5
2. Processor 2 has data 2
3. Processor 2 now has 5
2. Processor 3 has data 3
3. Processor 3 now has 5
2. Processor 0 has data: 5 5 5 5