# PyNotes for Environmental Scientists

A set of hands-on coding exercises to solve common tasks and problems in environmental sciences, soil science, and agronomy.


## Audience 

This material is part of an introductory graduate level course offered to students with little or no programming experience in environmental sciences, soil science, and agronomy; but anyone interested in learning how to code and with sufficient discipline to follow along the notes should find this content useful. The material is aimed at individuals seeking to be more productive, work faster, handle larger datasets, stimulate creative data analysis, develop code for self-teaching environmental processes, and generate reproducible science.

The Python language was selected because it is free, has a relatively straightforard and simple syntax, has been widely adopted by the scientific community, and contains a rich ecosystem of linraries and tools for generating reproducible research (e.g. Jupyter Notebooks). 

These notes are intended to be simple and explicit, and code expressions may not be the most efficient or *pythonic*. As students progress through the examples, they will become more familiar with the syntax, documentation sources, and eventually will be able to write more compact code and add extra layers of complexity to the existing code. 


## Motivation

The Python notes stem from the need to increase code literacy in students pursuing a career in environmental sciences. With the advent of technological advances and automated data collection systems, scientific programming is becoming an essential skill for reproducible data analysis.

The three main aspects that motivated this material are:

1. The lack of online examples including real datasets in environmental sciences. The material presented in this series of Jupyter notebooks relies entirely on data from published studies in peer-reviwed journals or data collected by the author and his students as part of past and present research efforts;

2. The vast amount of existing material about coding is either aimed at the general public with trivial examples or students familiar with advanced concepts in computer science, both of which have little appeal to graduate students and early career scientists in environmental sciences that are learning to write code for the first time.

3. I wanted to create a set of short, interactive, and reproducible scripts in the form of notebooks that students can download and execute anytime. Students can take advantage of tools such as [Github](https://github.com) and [Binder](https://mybinder.org) to gain access to the entire material and start coding in a matter of minutes.


## Goals

Students who successfully complete the material should be able to:

- construct effective, well documented, and error free scripts and functions.
- apply high-level programming to generate publication quality figures and optimize simple models.
- find information independently for self-teaching and problem solving.
- learn good programming habits and basic reproducible research practices by following short exercises using real data.

## Datasets

The datasets used along these notebooks can be found in the `/datasets` directory of the Github repository. Throughout the examples it is assumed that the Python interpreter of the Jupyter notebook is in the `pynotes/notebooks` directory, reason why the directories are relative to this path in the exercises, for example: `pd.read_csv("../datasets/file.csv")`. 

If you want to follow along an exercise without downloading the entire material, you can always read the data directly from the Github repository, just make sure you get the URL link for the "Raw" data. For example, to read the Anscombe's dataset:

`pd.read_csv('https://raw.githubusercontent.com/andres-patrignani/pynotes/master/datasets/anscombe_quartet.csv')`

Follow this video to learn how to obtain the link for the raw data.

<video loop autoplay="autoplay" width="100%" name="Video Name" src="_media/read_dataset_from_github.mp4"></video>


## About the author

Andres Patrignani is an assistant professor of Soil Water Processes in the Department of Agronomy at Kansas State University. He lives in Manhattan, KS and his primary research interests are in the field of soil physics and hydrology.


## Feedback

All the code has been written with teaching in mind. If you spot an important error or a no-no that should not be taught to students, please share your opinion and or suggestion.
- For bug reports, code suggestions, and topic requests please open an issue in the [Github repository](https://github.com/andres-patrignani/pynotes/issues).

- For other related issues feel free to contact me at andrespatrignani@ksu.edu


## Support

The content of this website is used as a foundation for a gradaute level course in Scientific Programming and Reproducible Research offered every Spring semester within the Department of Agronomy at Kansas State University.

This initiative is partially supported by the Kansas State University [Open/Alternative Textbook Initiative](https://www.lib.k-state.edu/open-textbook)


## License

All the code in these Jupyter notebooks has been written entirely by the author unless noted otherwise. The entire material is available for free under the Creative Commons Attribution-NonCommercial-ShareAlike ([CC BY-NC-SA](https://creativecommons.org/licenses/by-nc-sa/4.0/)) license
