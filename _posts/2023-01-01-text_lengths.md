---
title: "Calculate the average word count in text files"
date: 2023-01-01T21:00:00-04:00
categories:
  - tools
tags:
  - text
  - linguistics
---

A python script to gather all `.txt` files in the specified folder and calculates average word count and standard deviation. Specify the folder with the `-f` flag. Default is current working directory. Only the essential packages are needed (including `statistics`).

### Usage

```shell
python average_text_length.py -f "C:\path\to\folder"
```

### The code

```python
"""
average_text_length.py
Compute the text length statistics for all .txt files in the current folder.

Vojtech Illner
January 2023
Prague, Czech Republic
"""

import os
import sys
import getopt
import glob
import statistics


# ============================================
def main():
    print("Calculating...")
    # Retrieve the target folder
    folder = retrieve_input()

    # List all the text files
    os.chdir(folder)
    files = glob.glob("*" + ".txt")

    # Allocate the lengths array
    lengths = [0] * len(files)

     # Cycle through the files
    for i, file in enumerate(files):
        # Read the file content
        with open(os.path.join(folder, file), 'r', encoding='utf-8') as f:
            texts = f.readlines()
            f.close()
        
        # Flatten the texts variable
        text = " ".join(texts)

        # Count the number of words
        lengths[i] = len(text.split())

    # Calculate the statistics
    mean = statistics.mean(lengths)
    std = statistics.stdev(lengths)

    # Print the results
    print("Text length stats:")
    print("-----------------------------")
    print("Number of files: " + str(len(files)))
    print("Mean number of words: " + str(mean))
    print("Standard deviation: " + str(std))
    print("-----------------------------")



 # ============================================
def retrieve_input():
    """
    Retrieve and return the arguments delivered to the script.

        * -f = specify the folder of the text files (default is cwd)
    """
    folder = ""

    # Remove the 1st argument from the list of command line arguments
    argument_list = sys.argv[1:]

    # Specify options and long options
    options = "f::"

    # Parse the input arguments
    try:
        arguments, values = getopt.getopt(argument_list, options)
    except getopt.error as err:
        raise Exception("Error when reading input parameters: {}".format(str(err)))

    # Decide the action based on the argument flag
    for arg, val in arguments:
        if arg == '-f':
            folder = val

    if folder == "":
        folder = os.getcwd()

    # Return the arguments
    return folder


    
# Run the script.
# ============================================
if __name__ == '__main__':
    main()
```
