# MooViz

MooViz (**M**ulti-**o**bjective **o**ptimisation **Viz**ualisations) stores the objective values produced by Multi-Objective Evolutionary Algorithms. Visualisation is one of the great challenges for Muti-Objective Problems, specially when working with problems consisting of 4 or more objectives.

![visualising the objective space](gif/3D_DTLZ2_Shahin.gif)

## Organisation of repository

This repository is organised first into the number of dimensions (objectives), then into the problem being solved, then into the number of the sample. I.e., `/objectives/problem/sample/sub-directories`

 The sub-directories are:

`/lambda` folder contains the candidate solutions up for consideration at each generation, i.e. they are competing to survive in the next generation and produce offspring.

`/mu` folder contains the parent solutions, these are solutions which have survived and will producing the lambda solutions.

`/fig` folder contains my own plots of the mu and lambda solutions. This has been omitted due to file size.

`/gif` folder contains my own ffmpeg GIF output of my `/fig` folder `ffmpeg -i %d.png output.gif`. Uploaded to [gfycat](https://gfycat.com/EnragedLikelyBarnacle) too.

Each folder contains output at each iteration of the optimisation process.

## Optimisation algorithm

The algorithm used to generate these results is CMA-PAES-HAGA. This is a many-objective optimisation algorithm.  Open-access paper can be found [here](http://eprints.bournemouth.ac.uk/24371/)

```
@article{rostami2016covariance,
  title={Covariance matrix adaptation pareto archived evolution strategy with hypervolume-sorted adaptive grid algorithm},
  author={Rostami, Shahin and Neri, Ferrante},
  journal={Integrated Computer-Aided Engineering},
  volume={23},
  number={4},
  pages={313--329},
  year={2016},
  publisher={IOS Press}
}
```


The problem being solved is [WFG9](http://www.wfg.csse.uwa.edu.au/toolkit/)

```
@inproceedings{huband2005scalable,
  title={A scalable multi-objective test problem toolkit},
  author={Huband, Simon and Barone, Luigi and While, Lyndon and Hingston, Phil},
  booktitle={International Conference on Evolutionary Multi-Criterion Optimization},
  pages={280--295},
  year={2005},
  organization={Springer}
}
```

# Auxiliary information
The CMA-PAES-HAGA algorithm (used to generate these results) maintains additional parameters which describe a solution. Some of this data has also been stored in this repository to enhance your visualisations. E.g., you could make the markers indicating a solution smaller or larger depending on how much of the objective space they dominate explicitly.

`auxiliary/mu_parent_id`

There is a data file for each generation. This requires some explanation of the ordering of the lambda population at each generation:
- The first 100 solutions are the mu (parent) population selected in the previous generation
- The second 100 solutions are the offspring population created from the parent population.
- Eeach solution in the mu population creates an offspring solution.
- They are in order, such that in the lambda population, the solution at index 0 is the parent for the solution at index 100.

Column 1 of the data indicates which solution was this solutions parent. E.g., if the parent_id is 77, then the solution at index 77 is the parent. This ordering is maintained in all data-sets on this repository.

Column 2 of the data indicates if the solution is a parent. If it is a parent, you should ignore Column 1, as it will state the ID of the solution and not the parent. E.g. if the parent_id is 77, but column 2 is 1, you ignore it. However, if you see two solutions with parent_id, this indicates that both the parent and the offspring made it to the next generation. This can be interesting information. 

`auxiliary/mu_sigma`

There is a data file for each generation. This represents the adapted step-size for every solution in the selected mu population. The higher the sigma, the larger the steps when varying a solution. Higher sigma can indicate confidence in the direction of the evolution of solutions, lower sigma can indicate lower confidence in the direction (or maybe the solutions have converged).

`dominance of solutions`

In the mu population, the order of solutions indicates the explicit hypervolume of the objective space that they dominate. A solution at index 0 covers a greater area compared to the solution at index 1, when you're not considering the area which both of them overlap. This is in respect to a reference point. 

This reference point is dynamically calculated at each generation:

```
ref = (lambda_pop.max(axis=0) * .1) + lambda_pop.max(axis=0)
```

where lambda_pop.max(axis=0) returns the largest (worst) value found in each column (objective).

`auxiliary/pref`

If preference articulation has been used, whether it's *a priori* or *progressive*, the preferences will be stored for each generation.

`auxiliary/mu_var`

The problem variables for each corresponding solution in `mu/` are stored here. The minimum requirement is that the problem variables for the final population is stored.

`auxiliary/max_refs`

This folder stores the current extreme (worst) objective values found at each generation. 
