In the following project, we have chosen to examine GDP per capita in selected countries from the European Union, in the period 2010-2019. Annual growth rates for the whole period are calculated to acess which countries that rise and decline the most in GDP. Givem this accessment, we draw parralels to the population growth in the chosen countries and thereby explain the difference in GDP growth-rates. 


```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import matplotlib.pyplot as plt; plt.rcdefaults()
import ipywidgets as widgets
from matplotlib_venn import venn2 # install with pip install matplotlib-venn
from pandas_datareader import wb

# autoreload modules when code is run
%load_ext autoreload
%autoreload 2
```


```python
filename = 'data/EUGDP.xlsx'
```

In the following we have a sample of 38 countries, which are located in Europe. 


```python
pd.read_excel(filename)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Country</th>
      <th>2010</th>
      <th>2011</th>
      <th>2012</th>
      <th>2013</th>
      <th>2014</th>
      <th>2015</th>
      <th>2016</th>
      <th>2017</th>
      <th>2018</th>
      <th>2019</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>Belgium</td>
      <td>363140</td>
      <td>375968</td>
      <td>386175</td>
      <td>392880.0</td>
      <td>403003.3</td>
      <td>416701.4</td>
      <td>430372.1</td>
      <td>446364.9</td>
      <td>459819.8</td>
      <td>473639</td>
    </tr>
    <tr>
      <td>1</td>
      <td>Bulgaria</td>
      <td>38044.1</td>
      <td>41252.6</td>
      <td>42033.5</td>
      <td>41885.4</td>
      <td>42876.1</td>
      <td>45675.8</td>
      <td>48620.5</td>
      <td>52310.0</td>
      <td>56086.9</td>
      <td>60675.3</td>
    </tr>
    <tr>
      <td>2</td>
      <td>Czechia</td>
      <td>156718</td>
      <td>164040</td>
      <td>161434</td>
      <td>157741.6</td>
      <td>156660.0</td>
      <td>168473.3</td>
      <td>176370.1</td>
      <td>191721.8</td>
      <td>207570.3</td>
      <td>219896</td>
    </tr>
    <tr>
      <td>3</td>
      <td>Denmark</td>
      <td>243165</td>
      <td>247880</td>
      <td>254578</td>
      <td>258742.7</td>
      <td>265757.0</td>
      <td>273017.6</td>
      <td>283109.7</td>
      <td>292408.0</td>
      <td>301340.9</td>
      <td>310576</td>
    </tr>
    <tr>
      <td>4</td>
      <td>Germany</td>
      <td>2564400</td>
      <td>2693560</td>
      <td>2745310</td>
      <td>2811350.0</td>
      <td>2927430.0</td>
      <td>3030070.0</td>
      <td>3134100.0</td>
      <td>3244990.0</td>
      <td>3344370.0</td>
      <td>3435760</td>
    </tr>
    <tr>
      <td>5</td>
      <td>Estonia</td>
      <td>14860.7</td>
      <td>16826.8</td>
      <td>18050.7</td>
      <td>19033.4</td>
      <td>20180.0</td>
      <td>20782.2</td>
      <td>21693.6</td>
      <td>23775.8</td>
      <td>26035.9</td>
      <td>28037.2</td>
    </tr>
    <tr>
      <td>6</td>
      <td>Ireland</td>
      <td>167732</td>
      <td>170827</td>
      <td>175116</td>
      <td>179661.3</td>
      <td>194818.2</td>
      <td>262833.4</td>
      <td>271683.6</td>
      <td>297130.8</td>
      <td>324038.2</td>
      <td>347215</td>
    </tr>
    <tr>
      <td>7</td>
      <td>Greece</td>
      <td>226031</td>
      <td>207029</td>
      <td>191204</td>
      <td>180654.3</td>
      <td>178656.5</td>
      <td>177258.4</td>
      <td>176487.9</td>
      <td>180217.6</td>
      <td>184713.6</td>
      <td>187456</td>
    </tr>
    <tr>
      <td>8</td>
      <td>Spain</td>
      <td>1072709</td>
      <td>1063763</td>
      <td>1031099</td>
      <td>1020348.0</td>
      <td>1032158.0</td>
      <td>1077590.0</td>
      <td>1113840.0</td>
      <td>1161878.0</td>
      <td>1202193.0</td>
      <td>1244757</td>
    </tr>
    <tr>
      <td>9</td>
      <td>France</td>
      <td>1995289</td>
      <td>2058369</td>
      <td>2088804</td>
      <td>2117189.0</td>
      <td>2149765.0</td>
      <td>2198432.0</td>
      <td>2234129.0</td>
      <td>2295063.0</td>
      <td>2353090.0</td>
      <td>2418997</td>
    </tr>
    <tr>
      <td>10</td>
      <td>Croatia</td>
      <td>45111.8</td>
      <td>44793</td>
      <td>43940.8</td>
      <td>43703.2</td>
      <td>43401.3</td>
      <td>44616.4</td>
      <td>46615.5</td>
      <td>49094.4</td>
      <td>51625.1</td>
      <td>53936.7</td>
    </tr>
    <tr>
      <td>11</td>
      <td>Italy</td>
      <td>1.61128e+06</td>
      <td>1.64876e+06</td>
      <td>1.62436e+06</td>
      <td>1612751.3</td>
      <td>1627405.6</td>
      <td>1655355.0</td>
      <td>1695786.8</td>
      <td>1736592.8</td>
      <td>1766168.2</td>
      <td>1.78766e+06</td>
    </tr>
    <tr>
      <td>12</td>
      <td>Cyprus</td>
      <td>19410</td>
      <td>19803</td>
      <td>19440.8</td>
      <td>17995.0</td>
      <td>17408.5</td>
      <td>17826.9</td>
      <td>18872.9</td>
      <td>20039.7</td>
      <td>21137.8</td>
      <td>21943.6</td>
    </tr>
    <tr>
      <td>13</td>
      <td>Latvia</td>
      <td>17817.7</td>
      <td>20218.7</td>
      <td>22098.2</td>
      <td>22845.4</td>
      <td>23654.2</td>
      <td>24426.0</td>
      <td>25072.6</td>
      <td>26797.8</td>
      <td>29056.1</td>
      <td>30476.1</td>
    </tr>
    <tr>
      <td>14</td>
      <td>Lithuania</td>
      <td>27955.3</td>
      <td>31233.7</td>
      <td>33331.7</td>
      <td>34985.0</td>
      <td>36544.8</td>
      <td>37321.8</td>
      <td>38893.4</td>
      <td>42269.4</td>
      <td>45264.4</td>
      <td>48339.2</td>
    </tr>
    <tr>
      <td>15</td>
      <td>Luxembourg</td>
      <td>40177.8</td>
      <td>43164.6</td>
      <td>44112.1</td>
      <td>46499.6</td>
      <td>49824.5</td>
      <td>52065.8</td>
      <td>54867.2</td>
      <td>56814.2</td>
      <td>60053.1</td>
      <td>63516.3</td>
    </tr>
    <tr>
      <td>16</td>
      <td>Hungary</td>
      <td>98986.8</td>
      <td>101553</td>
      <td>99733.6</td>
      <td>102032.3</td>
      <td>105905.9</td>
      <td>112210.3</td>
      <td>115259.2</td>
      <td>125603.1</td>
      <td>133782.2</td>
      <td>143826</td>
    </tr>
    <tr>
      <td>17</td>
      <td>Malta</td>
      <td>6599.5</td>
      <td>6835.8</td>
      <td>7164.6</td>
      <td>7644.9</td>
      <td>8507.3</td>
      <td>9628.0</td>
      <td>10338.9</td>
      <td>11284.4</td>
      <td>12366.3</td>
      <td>13208.5</td>
    </tr>
    <tr>
      <td>18</td>
      <td>Netherlands</td>
      <td>639187</td>
      <td>650359</td>
      <td>652966</td>
      <td>660463.0</td>
      <td>671560.0</td>
      <td>690008.0</td>
      <td>708337.0</td>
      <td>738146.0</td>
      <td>774039.0</td>
      <td>812051</td>
    </tr>
    <tr>
      <td>19</td>
      <td>Austria</td>
      <td>295897</td>
      <td>310129</td>
      <td>318653</td>
      <td>323910.2</td>
      <td>333146.1</td>
      <td>344269.2</td>
      <td>357299.7</td>
      <td>370295.8</td>
      <td>385711.9</td>
      <td>398522</td>
    </tr>
    <tr>
      <td>20</td>
      <td>Poland</td>
      <td>361804</td>
      <td>380242</td>
      <td>389377</td>
      <td>394733.8</td>
      <td>411163.2</td>
      <td>430258.1</td>
      <td>426555.7</td>
      <td>467312.9</td>
      <td>496360.9</td>
      <td>527033</td>
    </tr>
    <tr>
      <td>21</td>
      <td>Portugal</td>
      <td>179611</td>
      <td>176096</td>
      <td>168296</td>
      <td>170492.3</td>
      <td>173053.7</td>
      <td>179713.2</td>
      <td>186489.8</td>
      <td>195947.2</td>
      <td>204304.8</td>
      <td>212303</td>
    </tr>
    <tr>
      <td>22</td>
      <td>Romania</td>
      <td>125409</td>
      <td>131925</td>
      <td>133147</td>
      <td>143801.6</td>
      <td>150458.0</td>
      <td>160297.8</td>
      <td>170393.6</td>
      <td>187772.7</td>
      <td>204640.5</td>
      <td>222090</td>
    </tr>
    <tr>
      <td>23</td>
      <td>Slovenia</td>
      <td>36363.9</td>
      <td>37058.6</td>
      <td>36253.3</td>
      <td>36454.3</td>
      <td>37634.3</td>
      <td>38852.6</td>
      <td>40366.6</td>
      <td>42987.1</td>
      <td>45754.8</td>
      <td>48006.6</td>
    </tr>
    <tr>
      <td>24</td>
      <td>Slovakia</td>
      <td>68093</td>
      <td>71214.4</td>
      <td>73483.8</td>
      <td>74354.8</td>
      <td>76255.9</td>
      <td>79758.2</td>
      <td>81038.4</td>
      <td>84517.0</td>
      <td>89721.0</td>
      <td>94177</td>
    </tr>
    <tr>
      <td>25</td>
      <td>Finland</td>
      <td>188143</td>
      <td>197998</td>
      <td>201037</td>
      <td>204321.0</td>
      <td>206897.0</td>
      <td>211385.0</td>
      <td>217518.0</td>
      <td>225835.9</td>
      <td>233619.2</td>
      <td>240078</td>
    </tr>
    <tr>
      <td>26</td>
      <td>Sweden</td>
      <td>374330</td>
      <td>411874</td>
      <td>428825</td>
      <td>440191.2</td>
      <td>437540.9</td>
      <td>454184.3</td>
      <td>466347.6</td>
      <td>479605.4</td>
      <td>471207.2</td>
      <td>474683</td>
    </tr>
    <tr>
      <td>27</td>
      <td>United Kingdom</td>
      <td>1867396</td>
      <td>1.91246e+06</td>
      <td>2.11171e+06</td>
      <td>2098425.7</td>
      <td>2309785.1</td>
      <td>2640934.6</td>
      <td>2435055.2</td>
      <td>2363109.3</td>
      <td>2423736.6</td>
      <td>2.52331e+06</td>
    </tr>
    <tr>
      <td>28</td>
      <td>Iceland</td>
      <td>10332.4</td>
      <td>10889</td>
      <td>11458.5</td>
      <td>12064.1</td>
      <td>13389.9</td>
      <td>15679.8</td>
      <td>18646.1</td>
      <td>21704.9</td>
      <td>21795.2</td>
      <td>21602.7</td>
    </tr>
    <tr>
      <td>29</td>
      <td>Liechtenstein</td>
      <td>:</td>
      <td>:</td>
      <td>:</td>
      <td>4812.4</td>
      <td>5021.7</td>
      <td>5649.1</td>
      <td>5637.7</td>
      <td>5804.3</td>
      <td>5822.5</td>
      <td>:</td>
    </tr>
    <tr>
      <td>30</td>
      <td>Norway</td>
      <td>323761</td>
      <td>358340</td>
      <td>396524</td>
      <td>393408.7</td>
      <td>375947.3</td>
      <td>347632.1</td>
      <td>333471.3</td>
      <td>353316.4</td>
      <td>367893.7</td>
      <td>359109</td>
    </tr>
    <tr>
      <td>31</td>
      <td>Switzerland</td>
      <td>441086</td>
      <td>504021</td>
      <td>519716</td>
      <td>518379.5</td>
      <td>534923.7</td>
      <td>612658.4</td>
      <td>606773.4</td>
      <td>602268.2</td>
      <td>597008.9</td>
      <td>628107</td>
    </tr>
    <tr>
      <td>32</td>
      <td>Montenegro</td>
      <td>3125.1</td>
      <td>3264.8</td>
      <td>3181.5</td>
      <td>3362.5</td>
      <td>3457.9</td>
      <td>3654.5</td>
      <td>3954.2</td>
      <td>4299.1</td>
      <td>4663.1</td>
      <td>:</td>
    </tr>
    <tr>
      <td>33</td>
      <td>North Macedonia</td>
      <td>7108.3</td>
      <td>7544.2</td>
      <td>7584.8</td>
      <td>8149.6</td>
      <td>8562.0</td>
      <td>9072.3</td>
      <td>9656.5</td>
      <td>10038.3</td>
      <td>10698.1</td>
      <td>:</td>
    </tr>
    <tr>
      <td>34</td>
      <td>Albania</td>
      <td>8996.6</td>
      <td>9268.3</td>
      <td>9585.8</td>
      <td>9625.4</td>
      <td>9968.6</td>
      <td>10264.1</td>
      <td>10719.9</td>
      <td>11563.8</td>
      <td>12782.4</td>
      <td>:</td>
    </tr>
    <tr>
      <td>35</td>
      <td>Serbia</td>
      <td>31545.8</td>
      <td>35431.7</td>
      <td>33679.3</td>
      <td>36426.7</td>
      <td>35467.5</td>
      <td>35715.5</td>
      <td>36723.0</td>
      <td>39183.3</td>
      <td>42855.5</td>
      <td>45911.6</td>
    </tr>
    <tr>
      <td>36</td>
      <td>Turkey</td>
      <td>581024</td>
      <td>596491</td>
      <td>678484</td>
      <td>714313.4</td>
      <td>703411.6</td>
      <td>772978.8</td>
      <td>780224.9</td>
      <td>754902.2</td>
      <td>652519.9</td>
      <td>:</td>
    </tr>
    <tr>
      <td>37</td>
      <td>Bosnia and Herzegovina</td>
      <td>12968.9</td>
      <td>13411.8</td>
      <td>13407.5</td>
      <td>13691.8</td>
      <td>13988.3</td>
      <td>14617.4</td>
      <td>15289.9</td>
      <td>16042.4</td>
      <td>16759.3</td>
      <td>:</td>
    </tr>
    <tr>
      <td>38</td>
      <td>Kosovo (under United Nations Security Council ...</td>
      <td>4402</td>
      <td>4814.5</td>
      <td>5058.8</td>
      <td>5326.6</td>
      <td>5567.5</td>
      <td>5807.0</td>
      <td>6070.1</td>
      <td>6413.8</td>
      <td>6725.9</td>
      <td>:</td>
    </tr>
  </tbody>
</table>
</div>



We will look at the first 10 countries on that list, where we will inspect there 


```python
EU_GDP=pd.read_excel(filename)
EUgdp=EU_GDP.head(10)
EUgdp
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Country</th>
      <th>2010</th>
      <th>2011</th>
      <th>2012</th>
      <th>2013</th>
      <th>2014</th>
      <th>2015</th>
      <th>2016</th>
      <th>2017</th>
      <th>2018</th>
      <th>2019</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>Belgium</td>
      <td>363140</td>
      <td>375968</td>
      <td>386175</td>
      <td>392880.0</td>
      <td>403003.3</td>
      <td>416701.4</td>
      <td>430372.1</td>
      <td>446364.9</td>
      <td>459819.8</td>
      <td>473639</td>
    </tr>
    <tr>
      <td>1</td>
      <td>Bulgaria</td>
      <td>38044.1</td>
      <td>41252.6</td>
      <td>42033.5</td>
      <td>41885.4</td>
      <td>42876.1</td>
      <td>45675.8</td>
      <td>48620.5</td>
      <td>52310.0</td>
      <td>56086.9</td>
      <td>60675.3</td>
    </tr>
    <tr>
      <td>2</td>
      <td>Czechia</td>
      <td>156718</td>
      <td>164040</td>
      <td>161434</td>
      <td>157741.6</td>
      <td>156660.0</td>
      <td>168473.3</td>
      <td>176370.1</td>
      <td>191721.8</td>
      <td>207570.3</td>
      <td>219896</td>
    </tr>
    <tr>
      <td>3</td>
      <td>Denmark</td>
      <td>243165</td>
      <td>247880</td>
      <td>254578</td>
      <td>258742.7</td>
      <td>265757.0</td>
      <td>273017.6</td>
      <td>283109.7</td>
      <td>292408.0</td>
      <td>301340.9</td>
      <td>310576</td>
    </tr>
    <tr>
      <td>4</td>
      <td>Germany</td>
      <td>2564400</td>
      <td>2693560</td>
      <td>2745310</td>
      <td>2811350.0</td>
      <td>2927430.0</td>
      <td>3030070.0</td>
      <td>3134100.0</td>
      <td>3244990.0</td>
      <td>3344370.0</td>
      <td>3435760</td>
    </tr>
    <tr>
      <td>5</td>
      <td>Estonia</td>
      <td>14860.7</td>
      <td>16826.8</td>
      <td>18050.7</td>
      <td>19033.4</td>
      <td>20180.0</td>
      <td>20782.2</td>
      <td>21693.6</td>
      <td>23775.8</td>
      <td>26035.9</td>
      <td>28037.2</td>
    </tr>
    <tr>
      <td>6</td>
      <td>Ireland</td>
      <td>167732</td>
      <td>170827</td>
      <td>175116</td>
      <td>179661.3</td>
      <td>194818.2</td>
      <td>262833.4</td>
      <td>271683.6</td>
      <td>297130.8</td>
      <td>324038.2</td>
      <td>347215</td>
    </tr>
    <tr>
      <td>7</td>
      <td>Greece</td>
      <td>226031</td>
      <td>207029</td>
      <td>191204</td>
      <td>180654.3</td>
      <td>178656.5</td>
      <td>177258.4</td>
      <td>176487.9</td>
      <td>180217.6</td>
      <td>184713.6</td>
      <td>187456</td>
    </tr>
    <tr>
      <td>8</td>
      <td>Spain</td>
      <td>1072709</td>
      <td>1063763</td>
      <td>1031099</td>
      <td>1020348.0</td>
      <td>1032158.0</td>
      <td>1077590.0</td>
      <td>1113840.0</td>
      <td>1161878.0</td>
      <td>1202193.0</td>
      <td>1244757</td>
    </tr>
    <tr>
      <td>9</td>
      <td>France</td>
      <td>1995289</td>
      <td>2058369</td>
      <td>2088804</td>
      <td>2117189.0</td>
      <td>2149765.0</td>
      <td>2198432.0</td>
      <td>2234129.0</td>
      <td>2295063.0</td>
      <td>2353090.0</td>
      <td>2418997</td>
    </tr>
  </tbody>
</table>
</div>




```python
myDict = {}
for i in range(2010, 2020): # range goes from 2010 to but not including 2017
    myDict[str(i)] = f'e{i}' 
myDict
```




    {'2010': 'e2010',
     '2011': 'e2011',
     '2012': 'e2012',
     '2013': 'e2013',
     '2014': 'e2014',
     '2015': 'e2015',
     '2016': 'e2016',
     '2017': 'e2017',
     '2018': 'e2018',
     '2019': 'e2019'}




```python
EUgdp.rename(columns = myDict, inplace=True)
EUgdp
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Country</th>
      <th>e2010</th>
      <th>e2011</th>
      <th>e2012</th>
      <th>e2013</th>
      <th>e2014</th>
      <th>e2015</th>
      <th>e2016</th>
      <th>e2017</th>
      <th>e2018</th>
      <th>e2019</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>Belgium</td>
      <td>363140</td>
      <td>375968</td>
      <td>386175</td>
      <td>392880.0</td>
      <td>403003.3</td>
      <td>416701.4</td>
      <td>430372.1</td>
      <td>446364.9</td>
      <td>459819.8</td>
      <td>473639</td>
    </tr>
    <tr>
      <td>1</td>
      <td>Bulgaria</td>
      <td>38044.1</td>
      <td>41252.6</td>
      <td>42033.5</td>
      <td>41885.4</td>
      <td>42876.1</td>
      <td>45675.8</td>
      <td>48620.5</td>
      <td>52310.0</td>
      <td>56086.9</td>
      <td>60675.3</td>
    </tr>
    <tr>
      <td>2</td>
      <td>Czechia</td>
      <td>156718</td>
      <td>164040</td>
      <td>161434</td>
      <td>157741.6</td>
      <td>156660.0</td>
      <td>168473.3</td>
      <td>176370.1</td>
      <td>191721.8</td>
      <td>207570.3</td>
      <td>219896</td>
    </tr>
    <tr>
      <td>3</td>
      <td>Denmark</td>
      <td>243165</td>
      <td>247880</td>
      <td>254578</td>
      <td>258742.7</td>
      <td>265757.0</td>
      <td>273017.6</td>
      <td>283109.7</td>
      <td>292408.0</td>
      <td>301340.9</td>
      <td>310576</td>
    </tr>
    <tr>
      <td>4</td>
      <td>Germany</td>
      <td>2564400</td>
      <td>2693560</td>
      <td>2745310</td>
      <td>2811350.0</td>
      <td>2927430.0</td>
      <td>3030070.0</td>
      <td>3134100.0</td>
      <td>3244990.0</td>
      <td>3344370.0</td>
      <td>3435760</td>
    </tr>
    <tr>
      <td>5</td>
      <td>Estonia</td>
      <td>14860.7</td>
      <td>16826.8</td>
      <td>18050.7</td>
      <td>19033.4</td>
      <td>20180.0</td>
      <td>20782.2</td>
      <td>21693.6</td>
      <td>23775.8</td>
      <td>26035.9</td>
      <td>28037.2</td>
    </tr>
    <tr>
      <td>6</td>
      <td>Ireland</td>
      <td>167732</td>
      <td>170827</td>
      <td>175116</td>
      <td>179661.3</td>
      <td>194818.2</td>
      <td>262833.4</td>
      <td>271683.6</td>
      <td>297130.8</td>
      <td>324038.2</td>
      <td>347215</td>
    </tr>
    <tr>
      <td>7</td>
      <td>Greece</td>
      <td>226031</td>
      <td>207029</td>
      <td>191204</td>
      <td>180654.3</td>
      <td>178656.5</td>
      <td>177258.4</td>
      <td>176487.9</td>
      <td>180217.6</td>
      <td>184713.6</td>
      <td>187456</td>
    </tr>
    <tr>
      <td>8</td>
      <td>Spain</td>
      <td>1072709</td>
      <td>1063763</td>
      <td>1031099</td>
      <td>1020348.0</td>
      <td>1032158.0</td>
      <td>1077590.0</td>
      <td>1113840.0</td>
      <td>1161878.0</td>
      <td>1202193.0</td>
      <td>1244757</td>
    </tr>
    <tr>
      <td>9</td>
      <td>France</td>
      <td>1995289</td>
      <td>2058369</td>
      <td>2088804</td>
      <td>2117189.0</td>
      <td>2149765.0</td>
      <td>2198432.0</td>
      <td>2234129.0</td>
      <td>2295063.0</td>
      <td>2353090.0</td>
      <td>2418997</td>
    </tr>
  </tbody>
</table>
</div>




```python
EUgdp_tall = pd.wide_to_long(EUgdp, stubnames='e', i='Country', j='year')
EUgdp_tall.head(10);
```


```python
EUgdp_tall = EUgdp_tall.reset_index()
```


```python
EUgdp_tall.loc[EUgdp_tall['Country'] == 'Denmark', :].plot(x='year',y='e')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x101b8afbd0>




![png](output_11_1.png)



```python
import ipywidgets as widgets
def plot_e(Dataframe, Country): 
    I = Dataframe['Country'] == Country
    
    ax=Dataframe.loc[I,:].plot(x='year',y='e', style='-o', legend='False')
    ax.set_ylabel('GDP per capita')
    ax.set_title("GDP per capita 2010-2019 for selected countries in Europe")
```


```python
widgets.interact(plot_e, 
    Dataframe = widgets.fixed(EUgdp_tall),
    Country = widgets.Dropdown(description='Country', options=EUgdp_tall.Country.unique(), value='Denmark')
                ); 
```


    interactive(children=(Dropdown(description='Country', index=3, options=('Belgium', 'Bulgaria', 'Czechia', 'Den…


From the plot above it is evident that severeal countries in the EU experience rising GDP over the couse of nine years. Although it is not all EU countries that have a positive growth in GDP. Greece is one of the few countries in the EU, and the only country in this sample, that have experienced ongoing decline in GDP. From the plot it can be seen that from at least the year 2010, the GDP has been consistently falling up until the year 2016. After this year the economic circumstances change, and the GDP is rising slowly. 
To investigate the rate of this decline, it would be interesting to calculate the annual growth rate for GDP for the whole period and thereafter analyze the economic circumstance during the time period and its relation to GDP. 


```python
EUgdp1=EUgdp.copy()
```


```python
def Annual_Growth_rate(EUgdp):
    return ((EUgdp['e2019']/EUgdp['e2010'])**(1/9)-1)*100
EUgdp1['Annual growth rate']=EUgdp1.apply(Annual_Growth_rate,axis=1)

```


```python
objects = ('BE', 'BG', 'CZE', 'DNK', 'DE', 'EST', 'IRE', 'GR', 'ESP', 'FRA')
y_pos = np.arange(len(objects))
performance = EUgdp1['Annual growth rate']

plt.bar(y_pos, performance, align='center', alpha=0.5)
plt.xticks(y_pos, objects)
plt.ylabel('Grwoth rate in pct.')
plt.title('Annual growth rate in GDP for selected EU countries for the period 2010-2019')

plt.show()
```


![png](output_17_0.png)


From the barplot above that the country with the most rapid growth in GDP is Ireland, and the country with the largest decline is Greece, as earlier mentioned. It would now be interesting to investigate which economic circumstances that have had the given effect on the Ireland and Greece.


```python
regions = ['IRL', 'GRC']
```


```python
pop = wb.download(indicator='SP.POP.TOTL', country=regions, start=2010, end=2018)
population=pop.rename(columns = {'SP.POP.TOTL' : 'population'})
population
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>population</th>
    </tr>
    <tr>
      <th>country</th>
      <th>year</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td rowspan="9" valign="top">Greece</td>
      <td>2018</td>
      <td>10727668</td>
    </tr>
    <tr>
      <td>2017</td>
      <td>10754679</td>
    </tr>
    <tr>
      <td>2016</td>
      <td>10775971</td>
    </tr>
    <tr>
      <td>2015</td>
      <td>10820883</td>
    </tr>
    <tr>
      <td>2014</td>
      <td>10892413</td>
    </tr>
    <tr>
      <td>2013</td>
      <td>10965211</td>
    </tr>
    <tr>
      <td>2012</td>
      <td>11045011</td>
    </tr>
    <tr>
      <td>2011</td>
      <td>11104899</td>
    </tr>
    <tr>
      <td>2010</td>
      <td>11121341</td>
    </tr>
    <tr>
      <td rowspan="9" valign="top">Ireland</td>
      <td>2018</td>
      <td>4853506</td>
    </tr>
    <tr>
      <td>2017</td>
      <td>4807388</td>
    </tr>
    <tr>
      <td>2016</td>
      <td>4755335</td>
    </tr>
    <tr>
      <td>2015</td>
      <td>4701957</td>
    </tr>
    <tr>
      <td>2014</td>
      <td>4657740</td>
    </tr>
    <tr>
      <td>2013</td>
      <td>4623816</td>
    </tr>
    <tr>
      <td>2012</td>
      <td>4599533</td>
    </tr>
    <tr>
      <td>2011</td>
      <td>4580084</td>
    </tr>
    <tr>
      <td>2010</td>
      <td>4560155</td>
    </tr>
  </tbody>
</table>
</div>




```python
year = [2010, 2011, 2012, 2013, 2014, 2015, 2016, 2017, 2018]
pop_greece = [11121341, 11104899, 11045011, 10965211, 10892413, 10820883, 10775971, 10754679, 10727668]


plt.plot(year, pop_greece, color='g')
plt.xlabel('Greece')
plt.ylabel('Population in million')
plt.title('Population in Greece 2010-2018')
plt.show()

year = [2010, 2011, 2012, 2013, 2014, 2015, 2016, 2017, 2018]
pop_Ireland =[4560155, 4580084, 4599533, 4623816, 4657740, 4701957, 4755335, 4807388, 4853506]
plt.plot(year, pop_Ireland, color='orange')
plt.xlabel('Ireland')
plt.ylabel('Population in million')
plt.title('Population in Ireland 2010-2018')
plt.show()
plt.tight_layout()
```


![png](output_21_0.png)



![png](output_21_1.png)



    <Figure size 432x288 with 0 Axes>


From the plots above, it is evident that the development in the population is very different for the two countries, hence the difference in annual GDP growth. 

Looking at the first plot, we see that the population for greece is declining. Greece has suffered severly from the economic crisis, which has lead to emigration, declining birth and an aging population. This results in lower GDP over the particular period.

For Ireland the situation is the absolute opposite. A rise in population has given rise to growing GDP over the period. A larger population can contribute to employment and thereby higher Gross Domestic Product. 
