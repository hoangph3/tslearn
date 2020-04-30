<!-- Our title -->
<div align="center">
  <h3>tslearn </h3>
</div>

<!-- Short description -->
<p align="center"> 
   The machine learning toolkit for time series analysis in Python
</p>

<!-- The badges -->
<p align="center">
    <a href="https://badge.fury.io/py/tslearn">
        <img alt="PyPI" src="https://badge.fury.io/py/tslearn.svg">
    </a>
    <a href="http://tslearn.readthedocs.io/en/latest/?badge=latest">
        <img alt="Documentation" src="https://readthedocs.org/projects/tslearn/badge/?version=latest">
    </a>
    <a href="https://travis-ci.org/rtavenar/tslearn">
        <img alt="Build" src="https://travis-ci.org/rtavenar/tslearn.svg?branch=master">
    </a>
    <a href="https://codecov.io/gh/rtavenar/tslearn">
        <img alt="Codecov" src="https://codecov.io/gh/rtavenar/tslearn/branch/master/graph/badge.svg">
    </a>
    <a href="https://pepy.tech/project/tslearn">
        <img alt="Downloads" src="https://pepy.tech/badge/tslearn">
    </a>
</p>

<!-- Draw horizontal rule -->
<hr>

<!-- Table of content -->

| Section | Description |
|-|-|
| [Installation](#installation) | Installing the dependencies and tslearn |
| [Getting started](#getting-started) | A quick introduction on how to use tslearn |
| [Available features](#available-features) | An extensive overview of tslearn's functionalities |
| [Documentation](#documentation) | A link to our API reference and a gallery of examples |
| [Contributing](#contributing) | A guide for heroes willing to contribute |
| [Citation](#referencing-tslearn) | A citation for tslearn for scholarly articles |

## Installation
There are different alternatives to install tslearn:
* PyPi: `python -m pip install tslearn`
* Conda: `conda install -c conda-forge tslearn`
* Git: `python -m pip install https://github.com/tslearn-team/tslearn/archive/master.zip`

In order for the installation to be successful, the required dependencies must be installed. For a more detailed guide on how to install tslearn, please see the [Documentation](https://tslearn.readthedocs.io/en/latest/?badge=latest#installation).

## Getting started

### 1. Getting the data in the right format
tslearn expects a time series dataset to be formatted as a 3D `numpy` array. The three dimensions correspond to the number of time series, the number of measurements per time series and the number of dimensions respectively (`n_ts, max_sz, d`). In order to get the data in the right format, different solutions exist:
* [You can use the utility functions such as `to_time_series_dataset`.](https://tslearn.readthedocs.io/en/latest/gen_modules/tslearn.utils.html#module-tslearn.utils)
* [You can convert from other popular time series toolkits in Python.](https://tslearn.readthedocs.io/en/latest/integration_other_software.html)
* [You can load any of the UCR datasets in the required format.](https://tslearn.readthedocs.io/en/latest/gen_modules/tslearn.datasets.html#module-tslearn.datasets)
* [You can generate synthetic data using the `generators` module.](https://tslearn.readthedocs.io/en/latest/gen_modules/tslearn.generators.html#module-tslearn.generators)

It should further be noted that tslearn [supports variable-length timeseries](https://tslearn.readthedocs.io/en/latest/variablelength.html).

```python3
>>> from tslearn.utils import to_time_series_dataset
>>> my_first_time_series = [1, 3, 4, 2]
>>> my_second_time_series = [1, 2, 4, 2]
>>> my_third_time_series = [1, 2, 4, 2, 2]
>>> X = to_time_series_dataset([my_first_time_series,
                                my_second_time_series,
                                my_third_time_series])
>>> y = [0, 1, 1]
```

### 2. Data preprocessing and transformations
Optionally, tslearn has several utilities to preprocess the data. In order to facilitate the convergence of different algorithms, you can [scale time series](https://tslearn.readthedocs.io/en/latest/gen_modules/tslearn.preprocessing.html#module-tslearn.preprocessing). Alternatively, in order to speed up training times, one can [resample](https://tslearn.readthedocs.io/en/latest/gen_modules/preprocessing/tslearn.preprocessing.TimeSeriesResampler.html#tslearn.preprocessing.TimeSeriesResampler) the data or apply a [piece-wise transformation](https://tslearn.readthedocs.io/en/latest/gen_modules/tslearn.piecewise.html#module-tslearn.piecewise).

```python3
>>> from tslearn.preprocessing import TimeSeriesScalerMinMax
>>> X_scaled = TimeSeriesScalerMinMax().fit_transform(X)
>>> print(X_scaled)
[[[0.] [0.667] [1.] [0.333] [nan]]
 [[0.] [0.333] [1.] [0.333] [nan]]
 [[0.] [0.333] [1.] [0.333] [0.333]]]
```

### 3. Training a model

After getting the data in the right format, a model can be trained. Depending on the use case, tslearn supports different tasks: classification, clustering and regression. For an extensive overview of possibilities, check out our [gallery of examples](https://tslearn.readthedocs.io/en/latest/auto_examples/index.html).

```python3
>>> from tslearn.neighbors import KNeighborsTimeSeriesClassifier
>>> knn = KNeighborsTimeSeriesClassifier(n_neighbors=1)
>>> knn.fit(X_scaled, y)
>>> print(knn.predict(X_scaled))
[0 1 1]
```

As can be seen, the models in tslearn follow the same API as those of the well-known scikit-learn. Moreover, they are fully compatible with it, allowing to use different scikit-learn utilities such as [hyper-parameter tuning and pipelines](https://tslearn.readthedocs.io/en/latest/auto_examples/plot_knnts_sklearn.html#sphx-glr-auto-examples-plot-knnts-sklearn-py).

### 4. More analyses

tslearn further allows to perform all different types of analysis. Examples include [calculating barycenters](https://tslearn.readthedocs.io/en/latest/gen_modules/tslearn.barycenters.html#module-tslearn.barycenters) of a group of time series or calculate the distances between time series using a [variety of distance metrics](https://tslearn.readthedocs.io/en/latest/gen_modules/tslearn.metrics.html#module-tslearn.metrics).

## Available features

| data                                                                                                                                                                                         | processing                                                                                                              | clustering                                                                                                                                                                      | classification                                                                                                                                                                          | regression                                                                                                                                                                           | metrics                                                                                                                              |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------|
| [UCR Datasets](https://tslearn.readthedocs.io/en/latest/gen_modules/tslearn.datasets.html#module-tslearn.datasets)                                                                           | [Scaling](https://tslearn.readthedocs.io/en/latest/gen_modules/tslearn.preprocessing.html#module-tslearn.preprocessing) | [TimeSeriesKMeans](https://tslearn.readthedocs.io/en/latest/gen_modules/clustering/tslearn.clustering.TimeSeriesKMeans.html#tslearn.clustering.TimeSeriesKMeans)                | [KNN Classifier](https://tslearn.readthedocs.io/en/latest/gen_modules/neighbors/tslearn.neighbors.KNeighborsTimeSeriesClassifier.html#tslearn.neighbors.KNeighborsTimeSeriesClassifier) | [KNN Regressor](https://tslearn.readthedocs.io/en/latest/gen_modules/neighbors/tslearn.neighbors.KNeighborsTimeSeriesRegressor.html#tslearn.neighbors.KNeighborsTimeSeriesRegressor) | [Dynamic Time Warping](https://tslearn.readthedocs.io/en/latest/gen_modules/metrics/tslearn.metrics.dtw.html#tslearn.metrics.dtw)    |
| [Generators](https://tslearn.readthedocs.io/en/latest/gen_modules/tslearn.generators.html#module-tslearn.generators)                                                                         | [Piecewise](https://tslearn.readthedocs.io/en/latest/gen_modules/tslearn.piecewise.html#module-tslearn.piecewise)       | [KShape](https://tslearn.readthedocs.io/en/latest/gen_modules/clustering/tslearn.clustering.KShape.html#tslearn.clustering.KShape)                                              | [TimeSeriesSVC](https://tslearn.readthedocs.io/en/latest/gen_modules/svm/tslearn.svm.TimeSeriesSVC.html#tslearn.svm.TimeSeriesSVC)                                                      | [TimeSeriesSVR](https://tslearn.readthedocs.io/en/latest/gen_modules/svm/tslearn.svm.TimeSeriesSVR.html#tslearn.svm.TimeSeriesSVR)                                                   | [Global Alignment Kernel](https://tslearn.readthedocs.io/en/latest/gen_modules/metrics/tslearn.metrics.gak.html#tslearn.metrics.gak) |
| Conversion([1](https://tslearn.readthedocs.io/en/latest/gen_modules/tslearn.utils.html#module-tslearn.utils), [2](https://tslearn.readthedocs.io/en/latest/integration_other_software.html)) |                                                                                                                         | [GAKKmeans](https://tslearn.readthedocs.io/en/latest/gen_modules/clustering/tslearn.clustering.GlobalAlignmentKernelKMeans.html#tslearn.clustering.GlobalAlignmentKernelKMeans) | [ShapeletModel](https://tslearn.readthedocs.io/en/latest/gen_modules/shapelets/tslearn.shapelets.ShapeletModel.html#tslearn.shapelets.ShapeletModel)                                    |                                                                                                                                                                                      | [Barycenters](https://tslearn.readthedocs.io/en/latest/gen_modules/tslearn.barycenters.html#module-tslearn.barycenters)              |


## Documentation

The documentation, including a gallery of examples, is hosted at [readthedocs](http://tslearn.readthedocs.io/en/latest/index.html).

## Contributing

If you would like to contribute to `tslearn`, please have a look at [our contribution guidelines](CONTRIBUTING.md). A list of interesting TODO's can be found [here](https://github.com/rtavenar/tslearn/issues?utf8=✓&q=is%3Aissue%20is%3Aopen%20label%3A%22new%20feature%22%20). **If you want other ML methods for time series to be added to this TODO list, do not hesitate to [open an issue](https://github.com/rtavenar/tslearn/issues/new/choose)!**

## Referencing tslearn

If you use `tslearn` in a scientific publication, we would appreciate citations:

```bibtex
@misc{tslearn,
      title={tslearn: A machine learning toolkit dedicated to time-series
             data},
      author={Romain Tavenard and Johann Faouzi and Gilles Vandewiele and
              Felix Divo and Guillaume Androz and Chester Holtz and Marie
              Payne and Roman Yurchak and Marc Ru{\ss}wurm and Kushal
              Kolar and Eli Woods},
      year={2017},
      note={\url{https://github.com/rtavenar/tslearn}}
}
```

#### Acknowledgments
Authors would like to thank Mathieu Blondel for providing code for [Kernel k-means](https://gist.github.com/mblondel/6230787) and [Soft-DTW](https://github.com/mblondel/soft-dtw).
