
# Glob module


```python
import glob

# Print all files in current working directory
print(glob.os.listdir())

# The glob module also has several functions that allows to navigate the directory. 
# print(glob.os.chdir('..'))
# glob.os.chdir('Datasets/')


```


```python
import glob
txtfiles = []
for file in glob.glob("*.ipynb"):
    txtfiles.append(file)

    
for filename in txtfiles:
    pd.read_csv(filename)


```

    anonymous_functions.ipynb
    assignment_ideas.ipynb
    booleans.ipynb
    calculator_examples.ipynb
    challenge_birthday_paradox.ipynb
    challenge_blackjack.ipynb
    challenge_carroll_dayofweek.ipynb
    challenge_dna_matching.ipynb
    challenge_doy.ipynb
    challenge_hangman.ipynb
    challenge_monty_hall.ipynb
    challenge_photoperiod.ipynb
    challenge_random_groups.ipynb
    challenge_random_walk.ipynb
    challenge_tank.ipynb
    challenge_vpd.ipynb
    curve_fitting.ipynb
    data_structures.ipynb
    data_types.ipynb
    datetime_module.ipynb
    ebook.ipynb
    error handling.ipynb
    for_loops.ipynb
    functions.ipynb
    glob_module.ipynb
    hello_world.ipynb
    if_statement.ipynb
    images.ipynb
    import_tabular_data.ipynb
    importing_modules.ipynb
    indexing_and_slicing_lists.ipynb
    inputs.ipynb
    installing_modules.ipynb
    lecture_notes.ipynb
    lotka_volterra_model.ipynb
    math_module.ipynb
    matplotlib_module.ipynb
    matplotlib_plot.ipynb
    matplotlib_soil_temperature.ipynb
    matplotlib_subplots.ipynb
    matplotlib_yyplots.ipynb
    morse_code.ipynb
    morse_code_game.ipynb
    newton_law_cooling.ipynb
    numpy_module.ipynb
    os_module.ipynb
    pandas2_module.ipynb
    pandas_module.ipynb
    plotting_bokeh.ipynb
    precipitation_summary_stats.ipynb
    problems.ipynb
    puzzles.ipynb
    pygame_module.ipynb
    random_module.ipynb
    random_student_picker.ipynb
    soil_temperature_bokeh.ipynb
    strings.ipynb
    thermal_time.ipynb
    time_module.ipynb
    url_iss.ipynb
    url_ks_mesonet.ipynb
    url_ok_mesonet.ipynb
    url_request_uscrn.ipynb
    venn_diagrams.ipynb
    while_loops.ipynb

