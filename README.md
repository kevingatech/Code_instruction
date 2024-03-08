## Introduction:

This documentation serves as a user guide for utilizing our codebase, specifically focusing on the functionality provided by the debug_workflow.py file. The debug_workflow.py file facilitates the matching and evaluation of job titles, roles, and functions against a historical leads dataset, employing sophisticated backend logic to preprocess input strings and conduct accurate assessments.

## Folder Structural:

Our folder structure provided comprises several directories and files organized within a project. Each directory serves a specific purpose in organizing and managing the project's resources. Below is an introduction to the primary directories and their definitions based on the provided subfolders:

### configs:
This directory contains configuration files used to define various settings, mappings, and parameters for the project. Configuration files often store data in YAML to specify how the application should behave or interact with data. Main files in this folder:

    abbreviations.yml
        serves as a mapping between abbreviations and their corresponding expanded forms or full meanings.
    
    acronyms.yml
        serves as a reference for mapping acronyms to their corresponding expanded forms or meanings.
    
    attributes.yml
        defines attributes such as "level" and "function" associated with job roles.
  
    british_to_american.yml
        contains mappings between British English spellings and their American English counterparts.
  
    chars_to_strip.yml
        specify a list of characters that should be stripped or removed from a given text or string. Similar to the previous 
        example, it categorizes characters into three sets: left, right, and both. These sets represent characters that 
        should be stripped from the left side of a string, from the right side, or from both sides, respectively.
  
    invalid.yml
        contain a list of invalid or prohibited values or patterns.
  
    keywords_function.yml
        map job function keywords to specific job function categories.
  
    keywords_level.yml
        map job level keywords to specific job level categories.
  
    keywords_role.yml
        map job role keywords to specific job role categories.
  
    repl_strings.yml
        define replacement strings for specific substrings or patterns found within text.
  
    repl_token_if_prec_by.yml
        specifies a rule for replacing a specific token ('of') with another token (',') under certain conditions.
  
    repl_word_bound.yml
        specify replacements for specific words or phrases.

### data: 
The "data" directory typically stores datasets, input files, or any other data required for the project. It may also include scripts or modules related to data handling, processing, or manipulation. Main files in this folder:

    create_source_of_truth.py
        It transforms the original hist_lead_record DataFrame to a source of truth DataFrame, renaming columns and dropping 
        duplicates. Then, it applies the to_dict function to convert each group to a dictionary and finally converts the 
        entire DataFrame to a dictionary.
        
    Historical Lead Records.csv
        source of truth DataFrame

### debug: 
The "debug" directory is used for debugging purposes and may contain scripts, modules, or files specifically designed for debugging or troubleshooting issues within the project. Debugging files may include scripts to test specific functionalities or to analyze the behavior of the code during execution. Main files in this folder:

    debug_apply_workflow_to_hist.py
        apply function apply_workflow_to_hist workflow to historical data.
        
    debug_filter_keywords.py
        processing a YAML file containing keywords related to different functions, standardizing them, and then filtering 
        them based on certain criteria before saving them back to a YAML file.
        
    debug_workflow.py
        process a job title provided as input.
        
    time_cases.py
        iterating over the first 10,000 rows of a DataFrame (hist_lead_record) and applying a workflow to each row's 'title' 
        column using the workflow_ function. It utilizes the tqdm library to display a progress bar while iterating.

### src: 
The "src" directory, short for "source," typically contains the source code files and modules that constitute the main functionality of the project. It may include subdirectories representing different components or modules of the application, along with their respective Python files. The "init.py" file indicates that the "src" directory is a Python package, allowing its modules to be imported and used in other parts of the project. Main files in this folder:

    __init__.py
    
    __pycache__
    
    capstone.egg-info
    cli.py
        sets up a command-line interface (cli) for a capstone project using the Click library in Python.
    
    commands.py
        defines two Click commands workflow and apply_workflow_to_hist for a cli.
   
    common.py
        defines several functions and initializes the NLTK library for tokenization.
    
    configs.py
        defines a class Configs and some utility functions related to configuration data.
   
    data.py
        serve several purposes related to data processing and retrieval.
    
    extractor.py
        contain methods for extracting job attributes such as level, function, and role from standardized job titles.
    
    paths.py
        contain path to the “Historical Lead Records.csv” file.
    
    processors.py
        define a class Processors with methods for building processors used for matching and replacing keywords.
   
    resolver.py
        defines two functions for resolving the level and function from extracted keywords.
    
    standardizer.py
        defines a collection of standardization methods used to clean and format job titles.
    
    workflow.py
        orchestrates the standardization and extraction process for a given job title input.

## Usage Instructions:

1.	Inputting Title:
- Begin by specifying the title you wish to analyze. For example, you can input the title "director, information technology" within the debug_workflow.py file under the debug folder.
- User may need to change the path to the “Historical Lead Records.csv” base on where they save the csv file within paths.py under src folder.

2.	Running debug_workflow.py:
- Execute the debug_workflow.py script to initiate the matching process.
- Upon execution, the script will swiftly generate match results in a clear and informative manner. our code does about 900-1000 titles per second.

3.	Sample Output:
- The output will include detailed information regarding the matching process, including timestamps, log levels, matched keywords, resolution results, and correctness assessments.
- Here's an example of the sample output structure:
```
2024-03-05 19:16:07,195 : INFO : root : director , information technology
2024-03-05 19:16:07,199 : DEBUG : extractor : Matched level 'director' with keyword(s): director
2024-03-05 19:16:07,199 : DEBUG : extractor : Matched level 'contributor' with keyword(s): information technology
2024-03-05 19:16:07,199 : DEBUG : extractor : Matched function 'it' with keyword(s): information technology
2024-03-05 19:16:07,199 : DEBUG : extractor : Matched role 'it general' with keyword(s): technology
2024-03-05 19:16:07,199 : DEBUG : resolver : Level resolved: director
2024-03-05 19:16:07,199 : DEBUG : resolver : Function resolved: it
2024-03-05 19:16:07,199 : DEBUG : resolver : Role resolved: it general
2024-03-05 19:16:07,202 : INFO : workflow : Title: 'director , information technology' not found in historical leads dataset. Cannot assess correctness.
2024-03-05 19:16:07,202 : INFO : workflow : level: director ❌, function: it ❌, role: it general ❌
initial: director , information technology
standardized: director , information technology
role_matched_keywords: ['technology']
role_extracted: ['it general']
role_resolved: it general
is_role_correct: False
function_matched_keywords: ['information technology']
function_extracted: ['it']
function_resolved: it
is_function_correct: False
level_matched_keywords: ['director', 'information technology']
level_extracted: ['director', 'contributor']
level_resolved: director
is_level_correct: False
has_buying_power: True
is_level_buying_power_correct: False
is_invalid: False
is_completely_correct: False
```

4.	Matching Job Function, Level, and Role:
- The script matches job function, level, and role individually based on the preprocessed input string.

5.	Evaluation Process:
- The historical lead records dataset acts as the benchmark for assessing the model's effectiveness.
- Each entry in the input is cross-referenced with the job title, role, and function entries in the historical lead records dataset.
- Extracts from the input that align with historical records are categorized as accurate, while disparities signify incorrect extractions.
This documentation provides comprehensive instructions for utilizing our codebase, emphasizing the functionality and backend logic of the debug_workflow.py file. By following these guidelines, users can effectively match and evaluate job titles, roles, and functions against historical lead records, leveraging our robust methodology for accurate assessments.

## Logic Behaviors in Job Title Standardization
#### 1.	Handling of Invalid Inputs:
The code appears to handle invalid input titles by checking if the input is empty or if it matches any invalid patterns or exact strings. This behavior could be explicitly documented, including how invalid inputs are flagged and excluded from further processing.
  	
#### 2.	Hierarchical Resolution:
The code implements hierarchical resolution for attributes such as Function, Role, and Level. Documenting the specific hierarchy rules used to resolve these attributes based on the extracted keywords would provide clarity on how the system determines attribute values.
   
#### 3.	Assessment of Correctness:
The code assesses the correctness of extracted attributes by comparing them against values from a source of truth (SoT). This process involves checking if the extracted values match the corresponding SoT values and determining whether the extracted attributes are correct or incorrect. Documenting the criteria used for assessing correctness and how it influences the final output would be beneficial.

#### 4.	Handling of Buying Power:
The code includes logic for handling attributes with buying power, such as Levels. This behavior involves determining if a Level with buying power is correctly identified based on the extracted keywords. Detailing how buying power correctness is evaluated and incorporated into the final output could enhance the documentation.

#### 5.	Short Circuiting for Exact Matches:
While the code mentions short circuiting for exact keyword matches, it would be helpful to provide more details on how this optimization works and under what conditions it is applied. Clarifying when and how the system decides to short circuit the keyword matching logic would improve understanding.


## Testing
#### Test Suite
The test suite for the job title standardization system is located in the tests/ directory. It comprises a comprehensive set of test cases designed to validate the functionality and accuracy of the Python functions.

#### Executing Tests
To execute the test suite, users can utilize the pytest framework. By running pytest from the project root directory, users can ensure that their environment is correctly configured and that all Python functions are functioning as expected. The test suite provides valuable feedback on the correctness and reliability of the standardization system.



