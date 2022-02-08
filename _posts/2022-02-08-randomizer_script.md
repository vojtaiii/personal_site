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

The following MATLAB script takes all files in `cmd` specified by the given extension and randomly renames them, including an appendix given in `lastName`.

Author: Michal Šimek

```matlab
% rename.m
% Author: Michal Simek
% CTU FEE, February 2022
clc,clear

files=dir('*.wav');
n=length(files);
m=n;
p = randperm(n,m);
namevector = {files.name};
ll = ['a' 'b' 'c' 'd' 'e'];
y = cell(5,1);
lastName = '_phon';

i = 1;
fileID = fopen('new_names.txt','w');

while ~isempty(p) 
    name=strrep(files(p(1)).name,'.WAV','');
    name=strrep(name,'.wav','');
    ind = find(contains(namevector,name(1:5)));
    c = ismember(p, ind);
   
    name = namevector(ind);
    
    for k = 1:length(name)
        [y{k}, Fs]=audioread(name{k});
    end
      
    
    if(i<10)
        for k = 1:length(name)
            file{k} = ['00' num2str(i) ll(k) lastName '.wav'];
            audiowrite(file{k},y{k},Fs)
        end
    else
        for k = 1:length(name)
            file{k} = ['0' num2str(i) ll(k) lastName '.wav'];
            audiowrite(file{k},y{k},Fs)
        end
    end
    
    for s = 1:length(name)
        fprintf(fileID,[file{s} ' ' name{s} '\n']);
    end
    delete(files(p(c)).name)
    p(c) = [];
    i = i+1;
end
fclose(fileID);
```
