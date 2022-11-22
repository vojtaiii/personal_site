---
title: "Concatenate all text files in the folder"
date: 2022-06-06T21:00:00-04:00
categories:
  - tools
tags:
  - text
  - linguistics
---

A python script to concatenate all `.txt` files in the specified folder. Specify the folder with the `-f` flag. Default is current working directory. Only the essential packages are needed.

### Usage

```shell
python concatenate_texts.py -f "C:\path\to\folder"
```

### The code

```python
"""
Concatenate all the .txt files in the specified folder.

Vojtech Illner
November 2022
Bern, Switzerland
"""

import os
import sys
import getopt
import glob


# ============================================
def main():
    # Retrieve the target folder
    folder = retrieve_input()

    # List all the text files
    os.chdir(folder)
    files = glob.glob("*" + ".txt")

    # Create a path for the output file
    output_path = os.path.join(folder, os.path.basename(folder) + "_concatenated.txt")

    # Create the output file
    f_concat = open(output_path, 'w', encoding='utf-8')

    # Cycle through the files
    for file in files:
        # Read the file content
        with open(os.path.join(folder, file), 'r', encoding='utf-8') as f:
            texts = f.readlines()
            f.close()
        # Write it to the overall file
        for text in texts:
            f_concat.write(text)

    f_concat.close()
    print("Concatenation finished.")


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
