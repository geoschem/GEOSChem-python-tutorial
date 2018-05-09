# Python/xarray tutorial for GEOS-Chem users

[![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/JiaweiZhuang/GEOSChem-python-tutorial/master)

To be used for the workshop at the [GEOS-Chem Asia Meeting](http://acmg.seas.harvard.edu/geos/meetings/2018_GCA/index.html), May 21-23, 2018.

* [Installation](#installation)
  * [Try it right now on the cloud for free](#try-it-right-now-on-the-cloud-for-free)
  * [Install on your own computer](#install-on-your-own-computer)
  * [Install on remote server & shared cluster](#install-on-remote-server--shared-cluster)
* [Why Python](#why-python)
  * [What's wrong with IDL & MATLAB?](#whats-wrong-with-idl--matlab)
  * ["I already have lots of IDL scripts!"](#i-already-have-lots-of-IDL-scripts)
  * [How about NCL? R? Julia?](#how-about-ncl-r-julia)
* [How to learn Python](#how-to-learn-python)
  * [Recommended free materials](#recommended-free-materials)
  * [How to filter irrelevant books](#how-to-filter-irrelevant-books)
  * [From Python to general research computing skills](#from-python-to-general-research-computing-skills)

# Installation

## Try it right now on the cloud for free

[Click here](https://mybinder.org/v2/gh/JiaweiZhuang/GEOSChem-python-tutorial/master) to launch a pre-configured notebook environment on the cloud platform provided freely by the [binder project](https://mybinder.org). Use the Chrome browser if you have trouble loading that page.

If the page is loaded successfully, you should see a [Jupyter notebook](https://jupyter-notebook.readthedocs.io/en/stable/examples/Notebook/What%20is%20the%20Jupyter%20Notebook.html) interface. Then, click on the first notebook to get started. Jupyter combines Python code, execution results, plots, custom texts, and even Latex formulas in a single page. Besides using the Jupyter program, you can also view the static notebook on GitHub (e.g [the first notebook](./Chapter01_explore_NetCDF_data.ipynb)).

The [official doc](https://jupyter-notebook.readthedocs.io/en/stable/examples/Notebook/Notebook%20Basics.html) contains very clear instructions on using Jupyter for interactive computing. The most important command is `Shift+Enter` to execute the current code block.

## Install on your own computer

Python is free & open-source so can be easily installed on any machines. To best way to get the **scientific** Python environment is using the [Conda management system](https://conda.io/docs/). Please follow the [official installation guide](https://conda.io/docs/user-guide/install/index.html) for installing on Linux/Mac/Windows. Linux/Mac also comes with a system Python (`/usr/bin/python`). Don't touch that. Windows users might find the [full Anaconda](https://docs.anaconda.com/anaconda/install/windows) (Conda plus tons of packages) with graphical interface easier to use than the command line.

After installation, check the paths:

```
$ which conda python
.../miniconda3/bin/conda
.../miniconda3/bin/python
```

Clone this repository so you have all the notebooks and data files to work with:

```
$ git clone https://github.com/JiaweiZhuang/GEOSChem-python-tutorial.git
```

Install required packages to an isolated environment (which allows you to manage different versions of packages without affecting each other):

```
$ conda env create -vv -n geo -f geoschem-python-tutorial/environment.yml
$ source activate geo
```

You can use a different name other than `geo` for the environment. `-vv` prints more message so the installation won't look like getting stuck.

Finally, start the Jupyter program
```
$ jupyter notebook
```

You should be taken to the web browser, just like when using the Binder cloud. Or visit the address `localhost:8888` in your browser.

## Install on remote server & shared cluster

The true power of Jupyter notebook is that it can be easily used on the remote servers. All plots can be quickly shown in the browser and don't need to go through the terribly slow X11 Channel. **This drastically improves working efficiency.** When using IDL, I often wait for a long time for the X-Window to pop up.

The installation steps are exactly the same as on your own computer. Even on shared HPC clusters, you can still set up your personal Python environment and don't need to ask the system administrator for permissions. This is possible because Conda does not require root access. For servers without Internet access (commonly seen in China), [here's a workaround](http://www.meteoboy.com/conda-without-internet.html) (in Chinese).

There is a minor change in how you connect to the Jupyter interface. Recall that on your local computer, Jupyter can be accessed by the address `localhost:8888`. 8888 is the default [port number](https://en.wikipedia.org/wiki/Port_(computer_networking)#Common_port_numbers) for local Jupyter. To connect to a remote Jupyter program, use whatever different port number such as 8999. When `ssh` to the remote server, add the below port forwarding option so the signal can be sent to your web browser:

```
$ ssh ... -L 8999:localhost:8999
```

On the remote server, start Jupyter with port number specified, and without password ("token"):

```
$ jupyter notebook --NotebookApp.token='' --no-browser --port=8999
```

The password is not too useful in this case because the signal only goes through your SSH connection, and is not visible to the external world.

A working example can be found [in the GEOSChem-on-cloud project](http://cloud-gc.readthedocs.io/en/stable/chapter02_beginner-tutorial/quick-start.html#step-4-analyze-output-data-with-python-optional), where you manually connect to the Jupyter notebook on Amazon cloud.

# Why Python

Python is getting incredibly popular in the Earth science community. See [Why Python and Jupyter Notebooks](https://unidata.github.io/online-python-training/introduction.html) from UCAR/Unidata's [online Python training](http://unidata.github.io/online-python-training/). AMS has been hosting a [Symposium dedicated to Python](https://ams.confex.com/ams/98Annual/webprogram/8PYTHON.html) for many years; no other programming languages have such a privilege!

Python's major advantages are:

- Free and open-source, unlike IDL & MATLAB that cost a lot of money.
- Very easy to learn. Researchers with any programming experience can pick it up in several minutes. This [Python-MATLAB cheatsheet](https://cheatsheets.quantecon.org) can help your migration.
- Has an incredibly powerful ecosystem. The [Jupyter project](http://jupyter.org) has revolutionized research computing (checkout [this article on Nature](https://www.nature.com/news/interactive-notebooks-sharing-the-code-1.16261), and a [more recent paper](http://ebooks.iospress.nl/publication/42900)). For Earth science in particular, the [xarray package](https://xarray.pydata.org) is revolutionizing the way to process NetCDF data, as well as [many other data formats](https://xarray.pydata.org/en/stable/io.html) including the legacy BPCH format for GEOS-Chem (through [xbpch](http://xbpch.readthedocs.io)).

For occasional programmers, Python can quickly fulfill your basic data analysis needs. For more advanced tasks, there will always be a solution in Python (often when there's no solution in other languages!). Here is a tiny subset of powerful Python packages:

- Day-to-day data analysis and computing: [NumPy](http://www.numpy.org), [Matplotlib](https://matplotlib.org), [Pandas](https://pandas.pydata.org), [xarray](https://xarray.pydata.org)
- Mathematical & Numerical analysis: [SciPy](https://docs.scipy.org/doc/scipy/reference/), [SymPy](https://www.sympy.org)
- Fancy interactive visualization: [Bokeh](https://bokeh.pydata.org), [Plotly](https://plot.ly/python/), [Altair](https://altair-viz.github.io)
- High-performance parallel computing: [Numba](https://numba.pydata.org), [Cython](http://cython.org), [Dask](https://dask.pydata.org/en/latest/)
- Statistics and machine learning: [Statsmodels](https://www.statsmodels.org), [Scikit-learn](http://scikit-learn.org)
- Deep learning: [TensorFLow](https://www.tensorflow.org), [Keras](https://keras.io), [PyTorch](https://pytorch.org)

For Earth science, there are a lot more packages [listed here](http://pyaos.johnny-lin.com/?page_id=20), such as the [xESMF](http://xesmf.readthedocs.io) package I wrote for regridding. Finally, Python/xarray integrates extremely well with cloud computing platforms, through the pioneering [Pangeo project](https://pangeo-data.github.io).

**All these come freely and easily**, as long as you know some basic Python...

## What's wrong with IDL & MATLAB?

The most important reason is that they contradict open science and reproducible research, since other people are not able to run your code without the expensive licenses. [Reproducing a research paper is hard enough](http://www.bbc.com/news/science-environment-39054778). Let us not make it even harder.

Even if money is not a not problem, IDL & MATLAB still lead to a much lower research efficiency (worse user interface, slower code, incomplete functionality...), since they lack the modern features of Python as reviewed in the previous Section.

The astronomy community have completely switched from IDL to Python and developed the outstanding [Astropy package](http://www.astropy.org). The full story is available in [this paper](https://arxiv.org/abs/1610.03159):

> "One major (but not exclusive) driver of the need for change, and probably the most significant, was **widespread dissatisfaction with IDL**. Those who strongly feel that scientific software and analysis should be open are opposed to the high license fees (or really, any cost) required to run the code."

Thus, the choice of programming language is not just a matter of taste. **It has real impact on the research efficiency and the openness of science**.

To be clear, I am not blaming IDL or MATLAB -- they were the pioneers in interactive computing and were the only possible tools in the early time. But it is 2018, and there are much better open-source alternatives.

## "I already have lots of IDL scripts!"

A 100-line IDL script can be often replaced by 10 lines of Python code, so the cost of switching to Python is much lower than you might expect. I learned IDL 4 years ago and had tons of IDL scripts too; but once I entered the Python world I decided to never use IDL (and even MATLAB) again.

With Python/xarray, here's how you open a NetCDF file, extract a variable "O3", and save it to a new file.

```python
import xarray as xr # use "xr" as a shortcut for the xarray module
ds = xr.open_dataset('data.nc') # "ds" contains all information in that NC file
dr = ds['O3'] # Extract a variable from the entire file. Code is self-explanatory
dr.to_netcdf('O3_only.nc') # write this variable to a new file
```

This is even simpler & more intuitive than [NCO](http://nco.sourceforge.net), which is designed specifically for this kind of task (while xarray is general-purpose and can do a lot more things).

With IDL, you would have to write (adapted from [IDL docs](http://www.harrisgeospatial.com/docs/NCDF_Overview.html)):
```IDL
; --- read data ---
fid_in = NCDF_OPEN('data.nc') ; Open the NetCDF file:
var_id = NCDF_VARID(fid, 'O3') ; Get the variable ID
NCDF_VARGET, fid_in, var_id, data  ; Get the variable data
NCDF_CLOSE, fid_in ; close the NetCDF file

; --- write data ---
fid_out = NCDF_CREATE('O3_only.nc', /CLOBBER) ; create a new file

; the first thing is to define coordinate dimension
; assume we know that "O3" is a 4D field with shape (72, 46, 47, 1)
lon_id= NCDF_DIMDEF(fid_out, 'lon', 72)
lat_id= NCDF_DIMDEF(fid_out, 'lat', 46)
lev_id= NCDF_DIMDEF(fid_out, 'lev', 47)
time_id= NCDF_DIMDEF(fid_out, 'time', 1)

; then we need to define the actual coordinate variables
lonarr_id = NCDF_VARDEF(fid_out, 'lon', [lon_id], /FLOAT)
latarr_id = NCDF_VARDEF(fid_out, 'lat', [lat_id], /FLOAT)
levarr_id = NCDF_VARDEF(fid_out, 'lev', [lev_id], /FLOAT)
timearr_id = NCDF_VARDEF(fid_out, 'time', [time_id], /LONG)

; then, define the data variable for our O3 field, with minimal metadata
var_id = NCDF_VARDEF(fid_out, 'O3', [lon_id, lat_id, lev_id, time_id], /FLOAT)
NCDF_ATTPUT, fid_out, T_id, 'long_name', 'Ozone mixing ratio'
NCDF_ATTPUT, fid_out, T_id, 'units','mol/mol'

; Phew... Hope you haven't fallen asleep!
; Let's switch to "data mode" to write actual numerical values
; Assume we already have float arrays lon_arr, lat_arr defined somewhere
NCDF_CONTROL, fid_out, /ENDEF
NCDF_VARPUT, fid_out, lonarr_id, lon_arr
NCDF_VARPUT, fid_out, latarr_id, lat_arr
NCDF_VARPUT, fid_out, levarr_id, lev_arr
NCDF_VARPUT, fid_out, timearr_id, time_arr

NCDF_VARPUT, fid_out, var_id, data ; the actual data for the O3 variable

NCDF_CLOSE, fid_out ; finally done!
```

The key difference is that IDL requires you to write low level code to construct a NetCDF file from scratch (exactly like a Fortran program does), while Python/xarray allows you to write **human-readable ideas**. The efficiency-gain by switching to Python is enormous. You can spend much less time writing boilerplate code and **have much more time thinking about real science**. Your code is also a lot more understandable to others, facilitating collaboration.

It took me roughly 2 days to finish the data analysis part for [my recent paper](https://github.com/JiaweiZhuang/FV3_util), which involves processing Terabytes of data on the cubed-sphere grid. The same task would have taken me many weeks with IDL, if possible at all!

## How about NCL? R? Julia?

They are all great open-source languages. But I recommend Python as the major tool, or the only tool if you only have time to learn one thing.

I love [NCL](https://www.ncl.ucar.edu) but it lacks many modern features, notably Jupyter integration. There are [interests in integrating NCL and Jupyter](https://www2.cisl.ucar.edu/siparcs-2018-projects#Accelerating), but this will take some effort. My favorite thing in NCL is [its colormaps](https://www.ncl.ucar.edu/Document/Graphics/color_table_gallery.shtml). Fortunately, you have [full access to NCL colormaps in Python](https://github.com/hhuangwx/cmaps).

[R](https://www.r-project.org) is great for statistics, but dealing with Earth science data is much more than stats. The ecosystem in Python is much more complete.

[Julia](https://julialang.org) is perhaps the best language for writing atmospheric models (if there is a chance to rewrite existing models, which is not likely to happen within 10 years). But its data analysis ecosystem is still not comparable with Python.

Finally, any R or Julia users should know some basic Python, because Python is the core of Jupyter notebooks where R or Julia code runs in. (**Ju**-**py**-te-**r** actually means Julia+Python+R!)

# How to learn Python

I sometimes hear from friends (especially old-school atmospheric scientists) that "Python is hard" -- it always turns out that they are learning Python in a totally wrong way! Python is a general purpose language used in many domains -- a big use case is web development. If you search for "python tutorial" on the Internet, you are very very likely find tutorials on web development or computer algorithms. Don't waste time on them if you just want to get science done as quickly as possible. Stick to **data science** tutorials -- they teach you how to analyze data, which is exactly what an atmospheric modeller needs. **The proper tutorial should feel very accessible and highly relevant to your daily research work.**

Besides the tutorials in this repository, I recommend those websites below:

## Recommended free materials

Name & Link | Level
--- | ---
[Research Computing in Earth Sciences](https://rabernat.github.io/research_computing/pages/schedule.html) | Beginner
[Python Data Science Handbook](https://jakevdp.github.io/PythonDataScienceHandbook/) | Beginner
[Scipy lecture notes](http://www.scipy-lectures.org) | Beginner + Advanced
[IPython Cookbook](https://ipython-books.github.io) | Advanced

## How to filter irrelevant books

**Avoid** books & tutorials containing

```python
  from numpy import * # will mess-up the variable environment ("namespace")
  from pylab import * # mixing matplotlib and numpy together
  print "hello" # python2 syntax. You should use Python3 only!
```

They are commonly seen in old "Python for scientific computing" books. Don't waste your time on learning old syntaxes.

Also, **skip** books that do not jump into NumPy at the very beginning but instead talk about web frameworks like Flask and Django. They are irrelevant to what you need. **A lot of wonderful Python books actually belong to this category**, such as [Fluent Python](http://shop.oreilly.com/product/0636920032519.do). As an atmospheric scientist (especially an atmospheric chemist), you definitely do not need this level of Python skill.

## From Python to general research computing skills

There is a very similar philosophy for learning most programming tools. If you find Git, vim, or even Linux command line difficult to learn, then the reason is that you are not reading the proper material written for academic researchers. In general, 90% of programming tutorials are targeted at web development, which is a **far bigger** market than scientific computing. Domain scientists who are unaware of this fact can get highly confused when learning programming, especially when trying to find materials on their own.

So, make sure you are reading something related to **Research computing** (not just "coding" in general!)

A great resource is [software carpentry](https://software-carpentry.org). I particularly recommend lessons on [Linux command line](http://swcarpentry.github.io/shell-novice/), [Git](http://swcarpentry.github.io/git-novice/), and [Python](http://swcarpentry.github.io/python-novice-gapminder/). [Effective Computation in Physics](http://shop.oreilly.com/product/0636920033424.do) is also a great book that can bring a researcher's coding skill to the next level.

Finally, this situation (hard to identify proper tutorials) is also true for cloud computing. The cloud should be relatively easy to learn if you follow the correct path (i.e. read the [documentation](http://cloud-gc.readthedocs.io/) I wrote!), instead of learning it from a web programmer's perspective.
