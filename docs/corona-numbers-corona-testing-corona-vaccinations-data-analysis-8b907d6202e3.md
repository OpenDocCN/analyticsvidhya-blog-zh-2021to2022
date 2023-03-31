# 电晕数，电晕测试，电晕接种印度:数据分析

> 原文：<https://medium.com/analytics-vidhya/corona-numbers-corona-testing-corona-vaccinations-data-analysis-8b907d6202e3?source=collection_archive---------7----------------------->

![](img/2294593fb55005746be86080d056ddf5.png)

印度是目前受电晕影响最严重的国家，每日死亡人数超过 360 万，死亡人数超过 3000 人。重要的是，我们要密切关注所有的数字，以便理解第二波浪潮的影响和后果。

# 数据分析

我收集了来自[](https://www.kaggle.com/sudalairajkumar/covid19-in-india)**的数据，是从[**covid 19 印度**](https://www.covid19india.org/) **刮来的。**zip 文件包含电晕数、电晕测试、电晕疫苗的数据。**

**打开笔记本，导入所有库。**

```
import pyforest
import plotly.express as px
```

# **疫苗接种数据**

**疫苗接种数据在 state 列中有一些行，其州名为 **India** ，假设州名没有记录，则改为 India。**

```
Vaccination_data= pd.read_csv("covid_vaccine_statewise.csv", index_col="State")
Vaccination_data
```

**![](img/0452987798d041bd6fa87ed3b3a7c258.png)**

**我们将删除 state 列中值为 India 的行。**

```
Vaccination_data=Vaccination_data.drop(["India"])
```

**![](img/0d8ed5ced5b090b323bcd2674962a953.png)**

```
Vaccination_data=Vaccination_data.reset_index()
```

**![](img/bd0446fcd4245a6fd36bda68674d8238.png)**

```
Vaccination_data.drop(['Updated On'],inplace = True, axis = 1)
Vaccination_data.columns
```

**![](img/e9bc7f2b274d95c22083efa41a8735ca.png)**

## **疫苗接种可视化**

```
columns=['Total Individuals Registered', 'Total Sessions Conducted',
       'Total Sites ', 'First Dose Administered', 'Second Dose Administered',
       'Male(Individuals Vaccinated)', 'Female(Individuals Vaccinated)',
       'Transgender(Individuals Vaccinated)', 'Total Covaxin Administered',
       'Total CoviShield Administered', 'Total Individuals Vaccinated',
       'Total Doses Administered']for i in columns:
    fig=px.treemap(Vaccination_data, values=i, path=["State"], template="plotly_dark",title="<b>TreeMap representation of different States w.r.t. their {}</b>".format(i))
    fig.show()
```

**![](img/37727c5cde0f4e09918cbed74d2dacd0.png)****![](img/cd3caec5de1b87cefff103740abc7897.png)****![](img/c010b72e8e3de225d5f4ceb191686d59.png)****![](img/f8b9b516bb222a786111e36c6cebe0d4.png)****![](img/8e5b497921d9fe4b4f9fde9d7ddcd709.png)****![](img/18b0edb9300cf3328fc5912fbbb6787c.png)****![](img/46fd14302e86db7d49472d39d5db9519.png)****![](img/22c0c18fe953709fe12163614e48ad8b.png)**

****观察**:马哈拉施特拉邦和拉贾斯坦邦在给人们接种疫苗方面最为积极。U.P .是人口最多的州，第二大州的人口几乎是 UP 的一半，所以他们的疫苗表现令人沮丧。**

## **每个州每 100 人的疫苗接种率**

```
population=pd.read_csv("population_density.csv")
Vaccination_data= pd.read_csv("covid_vaccine_statewise.csv")
population = population.rename(columns={'Population\n(%)':'population', 'State or union territory':'State'})
```

**我之前已经使用过人口数据集，并且已经下载到我的系统中，你可以从 [**Kaggle**](https://www.kaggle.com/aravindm27/india-medical-resources/) 下载。**

**我们将在“州”列中加入疫苗接种和人口表。**

```
vaccination_population= pd.merge(Vaccination_data, population, on='State', how='left')
```

**要添加一个新列来查找疫苗接种的百分比，我们将创建一个新列，并在 2 个不同的列上使用百分比公式。**

```
vaccination_population["vaccination_per_100"]=(vaccination_population["Total Individuals Vaccinated"]/vaccination_population["population"])*100fig=px.treemap(vaccination_population, values=vaccination_per_100, path=["State"], template="plotly_dark",title="<b>TreeMap representation of different States w.r.t. their {} people</b>")
    fig.show()
```

**![](img/31957c371c1c415fd5b799c4e0aaf5e2.png)**

**观察**:喀拉拉邦和古吉拉特邦做得很好，因为它们是相对较大的邦。****

# **电晕数**

**因为数据中没有提到活动的 covid 号，所以我们将创建一个新列。**

```
india_data= pd.read_csv("covid_india.csv")india_data = india_data.rename(columns={'State/UnionTerritory':'States','Cured':'Recovered'})
india_data.head()
india_data['Active'] = india_data['Confirmed']-india_data['Recovered']-india_data['Deaths']
india_data.head()
```

**![](img/1bd56de0b19d870578962d6625dffba1.png)**

## **电晕箱数量可视化**

```
columns = ['Recovered', 'Deaths', 'Confirmed', 'Active']
for i in columns:
    fig = px.treemap(india_data,values=i,path=['States'],template="plotly_dark",title="<b>TreeMap representation of different States w.r.t. their {}</b>".format(i))
    fig.show()
```

**![](img/fb3dada5ce373db0b286fbe9d18fedbc.png)****![](img/296daf8617f667d09b73d623cc7ceebd.png)****![](img/50dab9a779e73c075493c026a45024bb.png)****![](img/4f3bc693655dd8e27b509c75eb8f5b14.png)**

****观察**:马哈拉施特拉邦在所有的排行榜上都名列前茅，并且在遏制疫情蔓延方面做得非常糟糕。**

# **测试数字**

```
test_data = pd.read_csv("StatewiseTestingDetails.csv")
test_data = state_data.fillna(0)
test_data.dropna(inplace=True)test_data['Negative'] = pd.to_numeric(test_data['Negative'],errors='coerce')
test_data.info()
```

**![](img/88c7a664a6ec2f6a9fcb2848f2ab831e.png)**

## **Covid 测试数字可视化**

```
columns = ['TotalSamples', 'Negative', 'Positive']
for i in columns:
    fig = px.treemap(test_data,values=i,path=['State'],template="plotly_dark",title="<b>TreeMap representation of different States w.r.t. their {}</b>".format(i))
    fig.show()
```

**![](img/d56e15981225b5399961fad12e0a556d.png)****![](img/d4c183b95d049852e6ddc781e41d53c4.png)****![](img/702a3948a8560ed71d5ee70d63f7b1e8.png)**

****观察**:马哈拉施特拉邦并不是进行核试验次数最多的邦，这是一大败笔。**

# **结论**

**马哈拉施特拉邦和拉贾斯坦邦在疫苗接种方面做得很好。而北方邦在测试中表现出色。马哈拉施特拉邦的测试数据糟糕透顶。**