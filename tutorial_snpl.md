# Tutorial for data analysis & visualization using `snpl`

## Requirements
- Python >= 3.10
- ``numpy``
- ``matplotlib``
- ``hicsv-python``
- ``snpl`` (optional)

## 0. Organize the data

Here's one way to organize the data. 
The project folder (`Project_Example`) has sub-folders for 
different measurement techniques (`GPC`, `NMR`, etc.).
These sub-folders contains a folder to store "raw" data (`raw data`). 

```
Project_Example/
     |-- GPC/
     |    |-- raw data/
     |            |-- 2022-07-14 ... SN628A_MeasInfo.txt
     |            |-- 2022-07-14 ... SN628A_PeakResults_ ... .txt
     |            |-- 2022-07-14 ... SN628A.txt
     |              .
     |              .
     |              . 
     |
     |-- NMR/
     |    |-- raw data/
     |            | -- 
     |
     |-- Rheology/
     |       |-- raw data/
     |              |-- ~~~
     |              |-- ~~~
     |              |-- ~~~
     |
     |-- SAXS/
     |     |-- raw data/
     |            |-- ~~~
     |            |-- ~~~
     |            |-- ~~~
     |
     |-- Tensile/
     |      |-- raw data/
     |             |-- ~~~
     |             |-- ~~~
     |             |-- ~~~
     .      .
     .      .
     .      .
```

I made an example of the project folder (here)[./examples/Project_Example/]. 

Suppose that I have a new GPC data that I want to analyze and plot. 
I would then create a new folder dedicated to that purpose. 

```
Project_A/
     |-- GPC/
     |    |-- raw data/
     |    |       |-- 2022-07-14 ... SN628A_MeasInfo.txt
     |    |       |-- 2022-07-14 ... SN628A_PeakResults_ ... .txt
     |    |       |-- 2022-07-14 ... SN628A.txt
     |    |
     .    |-- 20220819_analysis_SN628series/
     .
     .
```


## 1. Convert raw data into a handy hicsv file
The raw data files in `raw data` folder are not always handy to treat with Python. 
In the present example, the curve data, peak result data, and measurement information
are separated into three files, namely: 

```
2022-07-14-15-30-59-20220714_SN628A.txt
2022-07-14-15-30-59-20220714_SN628A_PeakResults_20220714-abs-2.txt
2022-07-14-15-30-59-20220714_SN628A_MeasInfo.txt
```

This is not really convenient. 
Also, the file name is too long and redundant! 
So let's cook them into a much handier format first. 
Luckily, the utility module `snpl` has a convenient function 
that does the work for you. 

In the folder `20220819_analysis_SN628series`, create a Python script `1_convert.py`. 
(the script name can be anything though)

`1_convert.py`:
```python
# coding=utf-8

import snpl

if __name__ == `__main__`:
    out = snpl.gpc.load_ExportOmniSECReveal("../raw data/2022-07-14-15-30-59-20220714_SN628A.txt")
    out.save("20220714_SN628A.txt")
```

Run the script. It'll create a new text file `20220714_SN628A.txt` in the folder `20220819_analysis_SN628series/`. This new text file is nicely formatted 
following the `hicsv` format which I invented. The file would look like:

```
#{
#    "MeasInfo": {
#        "Guid": "73A474C2-A2D8-4B88-A8D0-CE4FF9635291",
...
#        "Settings_FlowRate": 1.0
#    },
#    "PeakResults 20220714-abs-2": {
#        "InjectionGuid": "73A474C2-A2D8-4B88-A8D0-CE4FF9635291",
#        "AnalysisGuid": "25FA76BD-87D3-40BB-9652-41641D5BD161",
#        "AnalysisDate": "2022-07-14 22:01:22.4062940",
#        "0": {
#            "Mn (g/mol)": 272653.5178312261,
#            "Mw (g/mol)": 335597.25724914204,
...
#}
#"Ret. time (min)","RI"      ,"LALS"    ,"RALS"    ,"DP"      ,"IP"     
0.0               ,-1.37911  ,464.851685,248.536697,-67.353302,31.996769
0.003333          ,-1.379492 ,464.850586,248.536789,-67.405411,31.996288
0.006667          ,-1.386673 ,464.849579,248.536896,-67.45507 ,31.994629
0.01              ,-1.394955 ,464.848785,248.536987,-67.463905,31.992579
0.013333          ,-1.399736 ,464.848175,248.537094,-67.438797,31.99119 
0.016667          ,-1.399217 ,464.847778,248.536987,-67.414658,31.990919
...
```
(it has been shortened for clarity. There were actually many lines at `...`)

See how the file combines the *metadata* (measurement info, analysis results 
such as Mn, Mw, etc) along with the raw data table. 
More importantly, the origin of this hicsv file is recorded in the Python script 
`1_convert.py`. This helps you to keep track of the data during processing. 

## 2. Visualize the file
Now that you have a handy hicsv file, let's plot the chromatogram. Create a Python script `2_plot.py` in the same folder `20220819_analysis_SN628series/`: 

```python
# coding=utf-8

import hicsv
import snpl

if __name__ == `__main__`:
    
    d = hicsv.hicsv("20220714_SN628A.txt")
    t, i = d.ga("Ret. time (min)", "RI")
    snpl.plot(t, i, marker="", ls="-")

    snpl.show()
```

Run the script in the same folder. You will have a graph window pop up. 

## 3. Plot the processed data
