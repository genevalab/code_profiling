# Code Profiling
C4L class activity for profiling code in Python and R

Today, we'll walk through exercises to perform code profiling in Python and R using a set of examples I've modified from a [workshop presented by Princeton Research Computing](https://github.com/PrincetonUniversity/intro_debugging).

Before class, you should have installed the following programs on your computer:

1. R and RStudio (instructions for both on [this page](https://posit.co/download/rstudio-desktop/))
2. [Anaconda](https://www.anaconda.com/docs/getting-started/anaconda/install)
3. [Pycharm](https://www.jetbrains.com/pycharm/data-science/?var=anaconda)


With all of these tools installed, you'll be ready for the workshop.

# Getting started
First, you'll need the files for today's activity, which are contained in this repo. In Terminal, navigate to a location where you want to store these files (say ```~/Desktop```) and run ```git clone https://github.com/genevalab/code_profiling/``` to clone this repository.

Now, ```cd``` into the newly created ```code_profiling``` directory ```cd code_profiling```. 

## Setting up today's conda environment
I've created what should be a pretty minimal conda environment for today. Create the environment by running:

```conda env create -f environment.yml```

and confirm its working by running:

``` conda activate python_profile```

if all looks good, you can deactivate for now:

```conda deactivate```

## Set up pycharm project
1. Open pycharm, and go to File>New Project....
2. Select the ```python_profile``` directory for the Location
3. For Environment, click "Select Existing"
4. For Type, select "Conda"
5. Then in the bottom Envirnment dropdown, select "python_profile"
6. Click Create
7. Finally, go to PyCharm>Settings>Project Structure and click the "+" to Add Content Root, and select the ```src" directory.

You should be all set to start profiling!

# Profiling some code
In the source directory you will see a search.py that contains three different search functions ```sort_search```, ```better_search```, and ```simple_search```. Test them out by running them in the PyCharm terminal. For example:

```python search.py simple_search```


## Profiling runtime by operation

```python -m cProfile -s tottime search.py sort_search```



## Visualizing

```
python -m cProfile -o search_stats.pstats search.py sort_search
snakeviz search_stats.pstats
```

## Profiling runtime by line


kernprof -l search.py sort_search

python -m line_profiler -rmt search.py.lprof


## Profiling memory by funtion

To profile the memory use of a function decorate it by adding ```@profile``` to the line immeadiately above the function. For example, I've added ```@profile``` to the line above better search in the screenshot below.
<img width="856" height="237" alt="image" src="https://github.com/user-attachments/assets/06c4f3df-530a-4d26-8d62-950e74edbcd1" />

To profile memory use of that function, run:

```
python -m memory_profiler  search.py better_search
```


 
