# Physics 427 Homework 11

__Due 11:59pm Wednesday 12/4/2024__

## 1. Simple MCMC Sampling (15 points)

In this problem we use MCMC to sample from a quite nontrivial 1D distribution:

$$
P(x) \propto \left[e^{-(x - a)^2/2\sigma_1^2} + e^{-(x - b)^2/2\sigma_2^2}\right]\sin^2(x)
$$

where $a = 4.0$, $b = -4.0$, $\sigma_1 = 3.0$, and $\sigma_2 = 2.0$.

Write a program, `problem1.cpp`, to sample 1,000,000 random numbers from this distribution using the Metropolis-Hastings algorithm. For reference, the algorithm is detailed in [Lecture 25](https://www.pulsarmagnetosphere.com/PHYS-427-Fall2024/Lecture%2025/) and [Lecture 26](https://www.pulsarmagnetosphere.com/PHYS-427-Fall2024/Lecture%2026/). Remember that there is a burn-in period, so choose your total number of MCMC steps accordingly. You can either use the random number generator given in Homework 10, or use the C++ standard random number generator in the `<random>` header. Your program should print the results into a `csv` file, and you should make a plot of the result in the same fashion as [slide 3](https://www.pulsarmagnetosphere.com/PHYS-427-Fall2024/Lecture%2026/#/3) of Lecture 26, comparing the histogram with the analytic expression. To normalize the histogram, you can set `density=True` when calling `plt.hist()`. To normalize the analytic expression $P(x)$ you may need to carry out a numerical integration using whatever method you prefer. Commit the resulting plot as `problem1.png` to the repository. Do not commit your `csv` files to the repository!

## 2. Traveling Salesman Problem (35 points)

In this problem, we will implement a code that solves the traveling salesman problem for a given input. You will need to use the MCMC technique to carry out simulated annealing and find the optimal solution that minimizes the total distance traveled by the salesman.

In the repository, a `coordinates.txt` file is provided which contains the $x$ and $y$ coordinates of 25 cities. Every row is a . A solution is defined to be a path that connects the 25 cities such that:

1. Every city is visited exactly once.
2. The first city is both the initial and final destination.

The goal of the problem is to find a solution that minimizes the total path length:

$$
L = \sum_{i=1}^{25} \sqrt{(x_i - x_{i-1})^2 + (y_i - y_{i-1})^2}
$$

where $x_i$ and $y_i$ are the coordinates of the $i$-th city in the solution. Note that the first city is also the last city in the solution, so $x_0 = x_{25}$ and $y_0 = y_{25}$.

Write a C++ program `problem2.cpp` that reads the `coordinates.txt` file and then perform the simulated annealing with MCMC. Start with an initial solution where the cities are simply connected in the order they appear in the text file. Construct a Markov chain by randomly swapping the order of two cities in the solution. The probability of accepting the new solution is given by the Metropolis algorithm (that is inspired by statistical physics):

$$
P_a = \min\left(1, \exp\left(-\Delta L/T\right)\right)
$$

where $\Delta L$ is the change in total path length. If the new solution is accepted, then it becomes the current solution. If the new solution is rejected, then the current solution remains unchanged. The temperature $T$ is a parameter that controls the probability of accepting a new solution. Initially, the temperature is set to a high value $T_0$ (e.g. $T_0 = 10$), so that the probability of accepting a new solution is close to 1. As the Markov chain progresses, the temperature is gradually lowered, so that the probability of accepting a new solution is close to 0. The temperature is lowered according to the following schedule:

$$
T = T_0 \exp\left(-n/\tau\right)
$$

where $n$ is the step number in the MCMC chain, and $\tau$ is a parameter that controls the rate of cooling. Stop the MCMC chain when the temperature reaches a small value $T_f$ (e.g. $T_f = 10^{-3}$). The final solution is (hopefully) the optimal solution that minimizes the total path length. Remember to choose $\tau$ so that you don't cool the system too quickly that it settles to a local minimum.

Your program should print out the following information:

1. The final solution (the order of the cities).
2. The total path length of the final solution.

Use the following format (numbers are just examples):

```sh
1
24
12
19
...
Total L: 8.37443
```

Include your output as `problem2.txt` and commit it to the repository.