## Benchmark Glaciers and Machine Learning

I applied machine learning techniques to investigate the mass balance trends of  different "benchmark" glaciers
over the last half-century.

***

## Introduction 

I’m working on building a methodology to investigate glaciers and deglaciation using remote sensing data. I plan to use a combination of both MODIS & Landsat/Sentinel data to have decent spatial and temporal resolutions in my study. I’m using the [USGS benchmark glacier project](https://www.usgs.gov/programs/climate-research-and-development-program/science/usgs-benchmark-glacier-project) as a starting point. These five glaciers in Alaska, Washington, and Montana have been monitored in-situ for the past half-century. The USGS releases a wide variety of data on these benchmark glaciers including mass balance measures, preprocessed aerial photographs, and glacier extent shapefiles. My goal is to help formulate a remote sensing methodology to inventory glaciers and deglaciation on a larger scale. In-situ monitoring is simply not feasable on a global scale, so remote sensing methods are needed to study the vast majority of Earth's glaciers. I am particularly interested in answering questions about the downstream effects of melting glaciers and plan to investigate this niche for my master’s project this coming year. This machine learning project serves as a starting point for incorperating machine learning methods into my project. While this project uses benchmark glacier mass balance data, I do hope to apply some of my recently learned machine learning skills into my remote sensing methods in the future.

There is some dataset that we can use to help solve this problem. This allows a machine learning approach. This is how I will solve the problem using supervised/unsupervised/reinforcement/etc. machine learning.

We did this to solve the problem. We concluded that...

## Data

Here is an overview of the dataset, how it was obtained and the preprocessing steps taken, with some plots!

![](assets/IMG/datapenguin.png){: width="500" }

*Figure 1: Here is a caption for my diagram. This one shows a pengiun [1].*

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


## Code

My project code is available [here](https://colab.research.google.com/drive/1dhLpk-ZUXa-RG0jSgcaCqQ6c3OYZn3J8?usp=sharing)

Here is how this work could be developed further in a future project.

## References


[back](./)

