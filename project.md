## Benchmark Glaciers and Machine Learning

I applied machine learning techniques to investigate the mass balance trends of  different "benchmark" glaciers
over the last half-century.

***

## Introduction 

I’m working on building a methodology to investigate glaciers and deglaciation using remote sensing data. I plan to use a combination of both MODIS & Landsat/Sentinel data to have decent spatial and temporal resolutions in my study. I’m using the [USGS benchmark glacier project](https://www.usgs.gov/programs/climate-research-and-development-program/science/usgs-benchmark-glacier-project) as a starting point. These five glaciers in Alaska, Washington, and Montana have been monitored in-situ for the past half-century. The USGS releases a wide variety of data on these benchmark glaciers including mass balance measures, preprocessed aerial photographs, and glacier extent shapefiles. My goal is to help formulate a remote sensing methodology to inventory glaciers and deglaciation on a larger scale. In-situ monitoring is simply not feasable on a global scale, so remote sensing methods are needed to study the vast majority of Earth's glaciers. I am particularly interested in answering questions about the downstream effects of melting glaciers and plan to investigate this niche for my master’s project this coming year. This machine learning project serves as a starting point for incorperating machine learning methods into my project. While this project uses benchmark glacier mass balance data, I do hope to apply some of my recently learned machine learning skills into my remote sensing methods in the future.

Machine learning could allow time series prediction with autoregression. Autoregression would potentially allow the prediction of future glacier mass balance based on past mass balance time series data. I wanted to try out using an ARIMA model (or autoregressive integrated moving average), as it is a powerful supervised regression tool which allows the prediction of future values based on past values in a time series. 

In the end, applying the ARIMA model to this benchmark glacier data did not prove to be useful. Error values ended up being very large. In comparison to a simple linear regression model, the ARIMA model performed significantly worse. For the Gulkana glacier, the average error (calculated by dividing root mean squared error by mean test data values) for the linear regression of 14.6% was much lower than the average error for ARIMA at 70.6%. Neither error is ideal, but this comparison demonstrates that the ARIMA model is not useful in this situation.

## Data
Glacialogical data is released by the USGS for its 5 different benchmark glaciers. This project focuses on the four benchmark glaciers with the greatest number of data entries. Most of the ARIMA analysis is done on only the Wolverine and Gulkana glaciers as only these datasets have more than the reccomended minium of 50 entries for to run the ARIMA model.

Before visualizing or modeling the glacialogical data, visualization was necessary. Each of the glaceir datasets was loaded as a pandas dataframe object to make the data easier to work with moving forward. 

```python
import pandas as pd
SC_data = pd.read_csv(SC_filepath)
W_data = pd.read_csv(W_filepath)
G_data = pd.read_csv(G_filepath)
LC_data = pd.read_csv(LC_filepath)

SC_df = pd.DataFrame(SC_data)
W_df = pd.DataFrame(W_data)
G_df = pd.DataFrame(G_data)
LC_df = pd.DataFrame(LC_data)
```

After the data for each glacier was loaded in as dataframes, the data needed to be grouped by "site_name." In the glacialogical data files, there are data from numerous collection sites. There is a column which indicates from which site the data in a given row come from. While there are potentially better methods for aggregating the data from these different sites, each site on each glacier was kept separate during this project for the sake of simplicity. The data were grouped by "site_name" as shown below:

```python
SC_groupedDF = SC_df.groupby('site_name')
SC_grouped_dataframes  = [group for _, group in SC_groupedDF]

W_groupedDF = W_df.groupby('site_name')
W_grouped_dataframes  = [group for _, group in W_groupedDF]

G_groupedDF = G_df.groupby('site_name')
G_grouped_dataframes  = [group for _, group in G_groupedDF]

LC_groupedDF = LC_df.groupby('site_name')
LC_grouped_dataframes  = [group for _, group in LC_groupedDF]
```

To briefly visualize what the data looks like in each of the four glacier data files, I plotted the recorded elevation (a proxy for mass balance) at each in-situ measuring site on the South Cascade Glacier over time. Figures for the other glaciers can be found in the "Additional Figures" section below.

![](assets/IMG/SC_plot.png){: width="500" }

Figure 1 (above) shows the measured elevation at each of the in-situ sites on South Cascade Glacier

When observing the figures, it becomes clear that the sites are not consistent with each other in terms of years used and number of samples collected. To once again simplify this project, only a single site would be used from each glacier. The main factor to decided which sites would be used was a breadth in the number of years a site was active. Each of the site Bs had a long range of years sampled compared to other sites on their respective glaciers, so all site Bs were chosen for consistency, even though this is most likely a coincidence. In a lengthier study moving beyond just machine learning methods, a different apporach for how to aggregate the values from the different sites should be used. It would be best to look to past literature on the Benchmark Glacier Project to get a sense of how this issue is usally resolved. The code to create a "Site B" subset for each benchmark glacier is as follows:

```python
# Dictionary to store dataframes for each site
SC_site_dataframes = {}
W_site_dataframes = {}
G_site_dataframes = {}
LC_site_dataframes = {}

# Iterate through groups and store dataframes
for site, group_df in SC_groupedDF:
    SC_site_dataframes[site] = group_df
for site, group_df in W_groupedDF:
    W_site_dataframes[site] = group_df
for site, group_df in G_groupedDF:
    G_site_dataframes[site] = group_df
for site, group_df in LC_groupedDF:
    LC_site_dataframes[site] = group_df

# Accessing individual dataframes for each selected site
SC_site_B_dataframe = SC_site_dataframes['B']
W_site_B_dataframe = W_site_dataframes['B']
G_site_B_dataframe = G_site_dataframes['B']
LC_site_B_dataframe = LC_site_dataframes['B']
```
Once each glacier Site B was isolated, they could all be plotted for comparison. In the following plot, it becomes apparent that the duration of each glacier's monitoring does not line up well with the others. Nor do the elevations, tho this is expected as the elevtions are only a proxy for mass balance. Later, these elevations were normalized for consistency before models were run.

![](assets/IMG/SiteB_All_plot.png){: width="500" }

Figure 2 (above) outines the years in which each glacier had monitoring on its "Site B" and at which elevation the "Site B" point was at any given year

#### Datasets used in the project are linked here:
[Gulkana Glacialogical Data](https://drive.google.com/file/d/1KciRCT_4cVXChv1nSc8eOxBHqwMQuBno/view?usp=sharing)

[Wolverine Glacialogical Data](https://drive.google.com/file/d/1xfuH47yD8KlLSmyM8nRQJvXeM015vO0Y/view?usp=sharing)

[Lemon Creek Glacialogical Data](https://drive.google.com/file/d/1u-vBvsn2Pz2mM3EJPyptl-6nDYzsBNDV/view?usp=sharing)

[South Cascade Glacialogical Data](https://drive.google.com/file/d/1bJiP9jKUJTpS_rrZukeZ84h8QLUTsnO8/view?usp=sharing)



## Modelling

Here are some more details about the machine learning approach, and why this was deemed appropriate for the dataset. 

The model might involve optimizing some quantity. You can include snippets of code if it is helpful to explain things.

```python
from sklearn.ensemble import ExtraTreesClassifier
from sklearn.datasets import make_classification
X, y = make_classification(n_features=4, random_state=0)
clf = ExtraTreesClassifier(n_estimators=100, random_state=0)
clf.fit(X, y)
clf.predict([[0, 0, 0, 0]])
```

This is how the method was developed.

## Results

Figure X shows... [description of Figure X].

## Discussion

From Figure X, one can see that... [interpretation of Figure X].

## Conclusion

Here is a brief summary. From this work, the following conclusions can be made:
* first conclusion
* second conclusion

Here is how this work could be developed further in a future project.


## Additional Figures
![](assets/IMG/G_plot.png){: width="500" }

Figure A (above) shows the measured elevation at each of the in-situ sites on Gulkana Glacier

![](assets/IMG/W_plot.png){: width="500" }

Figure B (above) shows the measured elevation at each of the in-situ sites on Wolverine Glacier

![](assets/IMG/LC_plot.png){: width="500" }

Figure C (above) shows the measured elevation at each of the in-situ sites on Lemon Creek Glacier

## Code

My project code is available [here](https://colab.research.google.com/drive/1dhLpk-ZUXa-RG0jSgcaCqQ6c3OYZn3J8?usp=sharing)


## References

[Autoregressive Integrated Moving Average (ARIMA) Prediction Model](https://www.investopedia.com/terms/a/autoregressive-integrated-moving-average-arima.asp#:~:text=An%20autoregressive%20integrated%20moving%20average%2C%20or%20ARIMA%2C%20is%20a%20statistical,values%20based%20on%20past%20values.)

[Guidance on Splitting Training and Testing Data](https://stackoverflow.com/questions/72544161/how-many-training-and-testing-data-should-i-use)

[Time Series Forecasting With ARIMA Model in Python for Temperature Prediction](https://medium.com/swlh/temperature-forecasting-with-arima-model-in-python-427b2d3bcb53)

[USGS Benchmark Glacier Project](https://www.usgs.gov/programs/climate-research-and-development-program/science/usgs-benchmark-glacier-project)

[back](./)

