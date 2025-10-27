# Code Profiling
C4L class activity for profiling code in Python and R

Today, we'll walk through exercises to perform code profiling in Python and R using a set of examples I've modified from a [workshop presented by Princeton Research Computing](https://github.com/PrincetonUniversity/intro_debugging).

Before class, you should have installed the following programs on your computer:

1. R and RStudio (instructions for both on [this page](https://posit.co/download/rstudio-desktop/))
2. [Anaconda](https://www.anaconda.com/docs/getting-started/anaconda/install)
3. [PyCharm](https://www.jetbrains.com/pycharm/data-science/?var=anaconda)


With these tools installed, you'll be ready for the workshop.

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

## Set up PyCharm project
1. Open PyCharm, and go to File>New Project....
2. Select the ```python_profile``` directory for the Location
3. For Environment, click "Select Existing"
4. For Type, select "Conda"
5. Then, in the bottom Environment dropdown, select "python_profile"
6. Click Create
7. Finally, go to the menu at the top of the screen and select PyCharm>Settings>Project Structure and click the "+" to Add Content Root, and select the ```src``` directory.
   Note: You may get an error here if it does not allow you to select a nested directory. Click the X next to the existing Content root to remove it, and then you should be able to choose ```src```.

You should be all set to start profiling!

# Profiling Python code
In the source directory you will see a script ```search.py``` that contains three different search functions ```sort_search```, ```better_search```, and ```simple_search```. Test them out by running them in the PyCharm terminal. For example:

```python search.py simple_search```


## Profiling runtime by operation
To investigate the breakdown of runtime for each function, we'll use cProfile. The cProfile module is a built-in deterministic profiler used to measure the execution time of Python programs. It tracks how many times and how long each function is called, providing detailed statistics about the performance of your code. This helps identify bottlenecks and optimize code efficiency.

Try running cProfile with each function using the command below (and substituting other function names at the end in the place of ```sort_search```). This command sorts the output by runtime, so scroll up to see the longest running parts.

```python -m cProfile -s tottime search.py sort_search```

### Questions: Which function takes the longest to execute? What elements of each function are taking the bulk of the time to process?

## Visualizing

While the text output is detailed, it can be quite long and hard to parse. Snakeviz generates HTML-based interactive visualization tools to track profiling times. Run cProfile again, this time creating  ```.pstats``` output files that can then be plotted using snakeviz. Do this for each of the three functions.

```
python -m cProfile -o sort_search_stats.pstats search.py sort_search
snakeviz sort_search_stats.pstats
```
### Question: Did your impression of what took the most time change when examined visually?

## Profiling memory use by function

To profile the memory use of a function, decorate it by adding ```@profile``` to the line immediately above the function. For example, I've added ```@profile``` to the line above, better search in the screenshot below.
<img width="856" height="237" alt="image" src="https://github.com/user-attachments/assets/06c4f3df-530a-4d26-8d62-950e74edbcd1" />

To profile the memory use of that function, run:

```
python -m memory_profiler search.py better_search
```
Which gives the following output:
<img width="856" height="253" alt="image" src="https://github.com/user-attachments/assets/c052db2d-4c5c-4e97-b29b-ff476f55ee06" />

### Question: Which function uses the most memory?

# Profiling R code

We'll do our R code profiling in RStudio. Open up the app, then go to Session>Set Working Directory and select ```/code_profiling/R_profile/``` as your working directory. You should see two scripts ```mbm_examples.R``` and ```profviz_examples.R``` in the Files pane of RStudio. Click on each to open the scripts (usually these will appear in the upper right pane, with a tab for each script.

Run the first two lines of ```mbm_examples.R``` to install any R packages you will need run the scripts. 

Now run the code down to line 37 to run MicroBenchMark, which will generate a text output of run times for each of four different sorting techniques. Microbenchmark runs each function multiple times (in this example 50) to give a mean and distribution of run times.  Run the next example to compare three different methods of querying and summarizing tabular data (base R, SQL, and dplyr). 

While this is useful information, it doesn't help us understand what elements of each option are influencing runtime.

Switch over to ```profviz_examples.R``` for a graphical approach to profiling R scripts in Profvis. Profvis creates an interactive web page that provides a graphical interface to explore where time is spent during code execution. By wrapping your code in a profvis call, you get an HTML widget that opens in a browser (or an RStudio pane), showing detailed timing and profiling information visually. 

Run everything down through line 21. You should see a new interactive plot appear. It goes to the top right pane for me.

### Look over the profvis output. Where is most time spent in this sorting script?

Now run the remaining lines of the script and visualize the profile for a snail sorting algorithm. Where is most of the time in that script spent?

We'll use these tools for our assignment this semester, so use any remaining class time to play around with the tools you will likely use for your code.

 
