---
layout: post
title: Sample Notebook
subtitle: Static image
tags: [test]
comments: true
---

This will test markdown

# Title 1
## Subtitle 1


```python
import xarray as xr
import string,os, calendar
from calendar import isleap
import datetime
import pandas as pd
import numpy as np
import math, copy
import sys
from os import path
import time
import dask
import matplotlib as mtlp
mtlp.use('Agg')
import matplotlib.pyplot as plt
import seaborn as sns
import matplotlib.dates as mdates
from os import path
from scipy.optimize import curve_fit
from scipy.interpolate import interp1d
from matplotlib import ticker
```


```python
%matplotlib inline
```


```python
def draw_vertical_seasons(INPUTDIR,OUTPUTDIR,list_stations,name_model_2,name_model_3=None):
  flag_tri = True
  if name_model_3 == None:
    flag_tri = False
    
  plt.style.use('seaborn-paper')
  # Create a color palette
  fig, axs = plt.subplots(nrows=1, ncols=4,figsize=(6.4,3),sharey=True)
  i = 0
  alt_max = 0
  delta_min = 0
  delta_max = 0
  for season in ['DJF','JJA','SON','Annual']:
      df = pd.read_csv(INPUTDIR+season+'/'+str(len(list_stations))+'_stations_'+season+'_vertical_average.csv')
      df = df.drop('Unnamed: 0', axis=1)
      df['diff_'+name_model_2]=df['diff_'+name_model_2]-df['diff_'+name_model_2].mean()
      ax = axs[i]
      df.plot(ax=ax,x = 'diff_'+name_model_2,y='Alt' ,linestyle='solid',marker='.',label=name_model_2,markersize=4,color='blue') 
      if flag_tri:
        df['diff_'+name_model_3]=df['diff_'+name_model_3]-df['diff_'+name_model_3].mean()
        df.plot(ax=ax,x = 'diff_'+name_model_3,y='Alt' ,linestyle='solid',marker='.',markersize=4,label=name_model_3,color='red')
      alt_max = np.maximum(alt_max,int(np.ceil(df['Alt'].max()/1000) * 1000))
      delta_min = np.minimum(delta_min,np.minimum(df['diff_'+name_model_2].min(),df['diff_'+name_model_3].min()))
      delta_max = np.maximum(delta_max,np.maximum(df['diff_'+name_model_2].max(),df['diff_'+name_model_3].max()))
      ax_title = ax.set_title(season)
      ax_title.set_fontweight('bold')  
      ax.get_legend().remove()
      i=i+1
  for i in range(len(axs)):
      ax = axs[i]
      label_x = ax.set_xlabel('$\Delta CO_2$ (ppm)')
      label_x.set_color("black")
      #label_x.set_fontstyle("italic")
      label_x.set_fontweight("bold")
      label_x.set_fontsize(10)
      plt.xticks(fontsize=10)
      ax.set_xlim(left=-2.2,right=+2.2)

  label_y = axs[0].set_ylabel('Altitude (km)')
  label_y.set_color("black")
  #label_y.set_fontstyle("italic")
  label_y.set_fontweight("bold")
  label_y.set_fontsize(12)
  ax.set_ylim(bottom=0.0,top=alt_max)
  y_ticks_loc= axs[0].get_yticks()
  y_ticks_label = np.copy(y_ticks_loc)
  for i in range(1,len(y_ticks_loc)):
    y_ticks_label[i] = y_ticks_loc[i]/1000 
  plt.yticks(y_ticks_loc,y_ticks_label.astype('int').astype('str'))
  plt.tight_layout()
  plt.subplots_adjust(wspace=0.1)
  if flag_tri:
    print('Graph drawn :'+'Vertical_profile_'+name_model_2+'_'+name_model_3+'_seasonal_average.png')
    fig.savefig(OUTPUTDIR+'Vertical_profile_'+name_model_2+'_'+name_model_3+'_seasonal_average.png', dpi=100) 
  else:
    print('Graph drawn :'+'Vertical_profile_'+name_model_2+'_seasonal_average.png')
    fig.savefig(OUTPUTDIR+'Vertical_profile_'+name_model_2+'_seasonal_average.png', dpi=100) 
  plt.show()  

```


```python
INPUTDIR = '/home/users/zlloret/Notebooks/First paper/Stats/Vertical/'
OUTPUTDIR = '/home/users/zlloret/Comparaison_Mesures/Graphs/LMDZ_ICO_ozone/Vertical_profiles/'
list_aircrafts=['aircraft_AAO','aircraft_HAA','aircraft_SAN']
```


```python
draw_vertical_seasons(INPUTDIR,OUTPUTDIR,list_aircrafts,'LMDZ','ICO')
```

    Graph drawn :Vertical_profile_LMDZ_ICO_seasonal_average.png



    
![png](/assets/img/output_5_1.png)
    



```python

```
