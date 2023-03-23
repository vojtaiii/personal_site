---
title: "Randomize files in a folder"
date: 2022-02-05T21:00:00-04:00
categories:
  - tools
tags:
  - wav
  - matlab
  - audio processing
---

The following MATLAB script takes all files in `cmd` specified by the given extension and renames them in a random order, given the root string `root`.

```matlab
% Rename files in the folder in random order
% Vojta Illner
% March 2023

extension = '*.wav';
root = 'AUQ';
names_table = "names_table.csv";

files=dir(extension);
n=length(files);
p = randperm(n);
namevector = {files.name};

f = waitbar(0, 'Renaming files...');
oldnames = strings(n,1);
newnames = strings(n,1);
for i = 1 : n
    waitbar(1/i, f, 'Renaming files...');
    oldnames(i) = namevector{p(i)};
    newnames(i) = strcat(root, '_', string(i), strrep(extension, '*', ''));

    % rename the file
    movefile(oldnames(i), newnames(i))
end

% Create a link file
writematrix([oldnames, newnames], names_table);

close(f)
```
