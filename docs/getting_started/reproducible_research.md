# Reproducible Research

The idea behind reproducible research is to publish a peer-reviewed article together with field/lab observations, instrument data, code, and code documentation in digital format. Here is where Jupyter notebooks become convenient since they can aggregate these components within a single platform.

Perhaps, the best advantage of using a high-level programming language is the ability to load data in a standard and cross-platform format (e.g. .txt, .csv), then conduct numerical, statistical, and geographic analyses, and generate high-quality figures, all within the same environment. In other words, we remove the need to manually transfer data for different analyses between different softwares, reducing the chance of human errors and drastically simplifying the complexity of the analysis. 

>Think about how many mouse and keyboard clicks you have to repeat to re-run the entire data analysis for a manuscript when using the traditional tools.

By writing code within a single platform we create step-by-step and logically arranged machine-executable instructions that enable us to reproduce our data analysis from top to bottom at anytime. This detailed sequence of steps also allows other people to follow our reasoning and check our work (e.g. manuscript reviewers, graduate advisors).

Here are my top five reproducible research practices for research:

1. Avoid manipulation of raw data. This includes files collected by dataloggers, data retrieved from online sources, or field/lab spredsheets. If you are copy-pasting data, you are probably doing it wrong.

2. Document and structure your code so that is readable. This step involves adding comments lines, making use of white space (or cells) to breakdown the code into smaller sections, add references and equations with equation numbers and source. An effective approach is to write code as if you are writing a tutorial to yourself. Documentation needs be brief and precise. Tools that integrate code with a markup language (e.g. Jupyter notebooks) are well-suited for this.

3. Provide public access of observations, data, and code through:

    - dedicated version-control platforms for creating and sharing code like Github and Gitlab. 
    - use a general-purpose open-access repository like Zenodo, which also generate a digital object identifier (DOI)
    - even tools like Dropbox and OneDrive are a step forward for making your research findings reproducible. 
    - host the entire content in your own research website.


## Jupyter Notebooks

- Code lives in cells, which help organizing your code. The main advantage of cells is that you can run (i.e. execute code) cells individually to ensure that the code is working properly before you move forward.

- To run code in a cell press: `ctrl + enter` keys. The result will appear right below the code.

- To run code in a cell and move onto the next cell when done press: `ctrl + shift + enter` keys

<br/>

![alt_text](../_media/jupyter_lab_gui.png "Jupyter Lab GUI")
*Mac users can use double tap with two fingers for zooming the image*



## References and recommended reading

[Guo, P., 2013. Helping scientists, engineers to work up to 100 times faster.](https://dl.acm.org/doi/10.1145/2507771.2507775)

[Shen, H., 2014. Interactive notebooks: Sharing the code. Nature, 515(7525), pp.151-152.](https://www.nature.com/news/interactive-notebooks-sharing-the-code-1.16261)

Sandve, G.K., Nekrutenko, A., Taylor, J. and Hovig, E., 2013. Ten simple rules for reproducible computational research. PLoS computational biology, 9(10).

Skaggs, T.H., Young, M.H. and Vrugt, J.A., 2015. Reproducible research in vadose zone sciences. Vadose Zone Journal, 14(10).


