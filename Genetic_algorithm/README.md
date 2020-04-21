Run a Genetic algorithm to optimise a Rosenbrock function.

Open the terminal from local, and run the command 'dakota -i GA.in -o GA.out'. Then the optimisation will start.

Suggestion for parallel computing:
The genetic algorithm supports parallel computing. For example, if there are 50 candidates in one generation, those 50 candidates can be simulated at the same time in multi processors to save time.

Open the terminal from local, and run the command 'mpirun -np 3 dakota -i GA.in -o GA.out'. Then the optimisation will start.

Here the number '3' means the number of processor used in the parallel simulation, and can be changed for your need.

More options for parallel simulation can be found in chapter 17 in Dakota's user's guider (downloaded from Dakota website).
