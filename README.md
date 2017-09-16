
# Pymaceuticals

trend 1: Compared to the placebo, most of the drugs were similarly ineffective in curtailing tumor growth other than Capomullin and Ramicane.

trend 2: The error intervals are so high for the metastatic site growth that the values are not even reliable.

trend 3: Again, except for Capomullin and Ramicane, the mice all had similarly poor survival rates when compared with the placebo.


```python
import numpy as np
import matplotlib.pyplot as plt
import pandas as pd
```


```python
drug = pd.read_csv("mouse_drug_data.csv")
trial = pd.read_csv("clinicaltrial_data.csv")
trial.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Mouse ID</th>
      <th>Timepoint</th>
      <th>Tumor Volume (mm3)</th>
      <th>Metastatic Sites</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>b128</td>
      <td>0</td>
      <td>45.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>f932</td>
      <td>0</td>
      <td>45.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>g107</td>
      <td>0</td>
      <td>45.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>a457</td>
      <td>0</td>
      <td>45.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>c819</td>
      <td>0</td>
      <td>45.0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```python
drug.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Mouse ID</th>
      <th>Drug</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>f234</td>
      <td>Stelasyn</td>
    </tr>
    <tr>
      <th>1</th>
      <td>x402</td>
      <td>Stelasyn</td>
    </tr>
    <tr>
      <th>2</th>
      <td>a492</td>
      <td>Stelasyn</td>
    </tr>
    <tr>
      <th>3</th>
      <td>w540</td>
      <td>Stelasyn</td>
    </tr>
    <tr>
      <th>4</th>
      <td>v764</td>
      <td>Stelasyn</td>
    </tr>
  </tbody>
</table>
</div>




```python
alldrugs = []
for d in drug['Drug']:
    if d not in alldrugs:
        alldrugs.append(d)

trialID = trial.set_index("Mouse ID")
drugID = drug.set_index("Mouse ID")
total = drug.merge(trial)
survive = total.groupby("Timepoint")["Mouse ID"].count()
test1 = total.loc[total["Drug"] == "Stelasyn"].groupby("Timepoint")["Tumor Volume (mm3)"].mean()
test2 = total.loc[total["Drug"] == "Capomulin"].groupby("Timepoint")["Tumor Volume (mm3)"].mean()
total.loc[total["Drug"] == "Stelasyn"].groupby("Timepoint")["Tumor Volume (mm3)"].std()
test1

```




    Timepoint
    0     45.000000
    5     47.527452
    10    49.463844
    15    51.529409
    20    54.067395
    25    56.166123
    30    59.826738
    35    62.440699
    40    65.356386
    45    68.438310
    Name: Tumor Volume (mm3), dtype: float64




```python
need_list = []
x_limit = 50
x_axis = np.arange(0, x_limit, 5)
colors = ["red", "blue", "green", "yellow", "black", "orange", "purple", "gray", "gold", "cyan"]
for d in alldrugs:
    for c in colors:
        each = total.loc[total["Drug"] == d].groupby("Timepoint")["Tumor Volume (mm3)"]
        n, = plt.plot(x_axis, each.mean(), marker = "o", color = colors[-1], label = str(d), linestyle = "--")
        need_list.append(n)
        e = each.std()
        #print(e)
        plt.errorbar(x_axis, each.mean(), yerr=e, linestyle = "None")
        colors.pop()
        break
plt.title("Tumor Response to Treatment")
plt.ylabel("Tumor Volume (mm^3)")
plt.xlabel("Time (days)")
plt.legend(need_list, alldrugs, loc="upper left", prop={'size': 7})
plt.ylim(20, 80)
plt.xlim(0, x_limit)
plt.show()

```


![png](output_5_0.png)



```python
need_list2 = []
x_limit = 50
x_axis = np.arange(0, x_limit, 5)
colors = ["red", "blue", "green", "yellow", "black", "orange", "purple", "gray", "gold", "cyan"]
for d in alldrugs:
    for c in colors:
        each = total.loc[total["Drug"] == d].groupby("Timepoint")["Metastatic Sites"]
        n, = plt.plot(x_axis, each.mean(), marker = "o", color = colors[-1], label = str(d), linestyle="--")
        need_list2.append(n)
        e = each.std()
        #print(e)
        plt.errorbar(x_axis, each.mean(), yerr=e, linestyle = "None")
        colors.pop()
        break
plt.title("Metatstatic Spread Through Treatment")
plt.ylabel("Met. Sites")
plt.xlabel("Time (days)")
plt.legend(need_list2, alldrugs, loc="upper left", prop={'size': 7})
plt.ylim(-1, 4.5)
plt.xlim(0, x_limit)
plt.show()
```


![png](output_6_0.png)



```python
j
need_list3 = []
x_limit = 50
x_axis = np.arange(0, x_limit, 5)
colors = ["red", "blue", "green", "yellow", "black", "orange", "purple", "gray", "gold", "cyan"]
for d in alldrugs:
    for c in colors:
        each = total.loc[total["Drug"] == d].groupby("Timepoint")["Mouse ID"].count()
        survival = [x/each[0]*100 for x in each]
        n, = plt.plot(x_axis, survival, marker = "o", color = colors[-1], label = str(d), linestyle="--")
        need_list3.append(n)
        colors.pop()
        break
plt.title("Survival During Treatment")
plt.ylabel("Survival Rate (%)")
plt.xlabel("Time (days)")
plt.legend(need_list3, alldrugs, loc="lower left", prop={'size': 7})
plt.ylim(20, 100)
plt.xlim(0, x_limit)
plt.show()

```


![png](output_7_0.png)



```python
allpercents = []
positive = []
for d in alldrugs:
    j = total.loc[total["Drug"]==d].groupby("Timepoint")["Tumor Volume (mm3)"].mean()
    perc = (j[45]-j[0])/j[0]*100
    allpercents.append(perc)
    if perc > 0:
        positive.append(True)
    else:
        positive.append(False)
fin = pd.DataFrame({"percents": allpercents, "Boolean": positive})
x_labels = alldrugs
x_axis = np.arange(10)
j = [e for e in x_axis]
got_plot = plt.bar(x_axis, fin["percents"], width=.98,color=[x for x in fin["Boolean"].map({True: 'red', False: 'green'})])#fin["Boolean"].map({True: 'red', False: 'green'}))
tick_locations = [value for value in x_axis]
plt.xticks(tick_locations, x_labels, rotation = 45)
plt.title("Tumor Change Over 45 Day Treatment")
plt.xlabel("Drugs")
plt.ylabel("% Tumor Volume Change")
plt.ylim(-30, 70)
a = 4
for i in fin["percents"].round(0):
    b = j[0]-.4
    if i<0:
        a = -7
        b=b-.05
    plt.text(b, a, str(int(i))+"%")
    a = 4
    b = j[0] -.4
    j.pop(0)

plt.show()


```


![png](output_8_0.png)



```python

```


```python

```


```python

```


```python

```
