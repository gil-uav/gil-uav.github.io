---
layout:     	post
title:     		Dataset generation pt. 3
author:     	Vegard Bergsvik Øvstegård
tags:           post machine-learning 
subtitle:    	Using QGIS to get ground truth-images.
toc:            true
category:       semantic_segmentation
---

In order to extract the ground truth (GT) images from the vector maps recieved by
[Kartverket](https://www.kartverket.no/). I used [QGIS](https://qgis.org/en/site/), an open source
geographic information system. It was how-ever quite tedious to to extract the GT images manually
using the GUI. So i wrote some simple [python functions](https://github.com/gil-uav/orthophoto-dataset-generator/blob/master/qgis.py) to help me automate it.
To put it short, the functions import all the orthophoto maps as layers to QGIS, and exports the images
with the same spatial extent said orthophotos, but from the GT vector maps.

### Map issues
I did discover some issues while inspecting the GT vector maps and the orthophotos:

**1.** Rectification of orthophotos was not perfect, some buildings were not completely segmented. ![Segmentation issue due to Rectification](/img/rect_seg_issue.png)
**2.** Not all buildings where segmented.![Missing building segmentation.](/img/missing_building_seg.png)
**3.** Water segmentation of rivers where highly inaccurate, this is due to the riverbed naturally shifting.![Inaccurate river segmentation](/img/bad_river_seg.png)



To solve issue **1**, i used QGIS to adjust the GT vector layer a bit. I simply widened the vector by
one meter at scale. The only way to solve issue number **2** and **3** is either with manual adjustment of
the vector maps, or simply scrubbing the dataset after creation. Both tedious jobs, but they might
be worth while. An idea to automate the scrubbing for issue number **2**. is to train the network on the
dataset, and then use the network identify and remove sets with non-segmented buildings. These fixes will be considered
after the first training runs.


### QGIS helper functions:

```python

import os

from qgis.core import QgsProject
from glob import glob

project = QgsProject.instance()
root = QgsProject.instance().layerTreeRoot()


def import_map(path_to_tif: str, root):
    """
    Imports a .tif file as map-layer.

    Parameters
    ----------
    path_to_tif : str
        Path to .tif file.
    root
        Project instance layer root.

    Returns
    -------

    """
    rlayer = QgsRasterLayer(
        path_to_tif, os.path.basename(path_to_tif).replace(".tif", "")
    )
    if not rlayer.isValid():
        print("Layer failed to load!")
    iface.addRasterLayer(path_to_tif, os.path.basename(path_to_tif).replace(".tif", ""))


def import_all_maps(path):
    """
    Imports all .tif files to project as layers.
    Parameters
    ----------
    path : str
        Path to folder containing .tif files(Ortophotos)

    Returns
    -------

    """
    maps = [y for x in os.walk(path) for y in glob(os.path.join(x[0], "*.tif"))]
    for m in ops:
        import_map(m, root)
        print("{} imported.".format(os.path.basename(m)))


def get_ortophoto_layers():
    """
    Returns all ortophoto layers from project.
    Returns
    -------
    layers : list
        List of ortophoto layers.

    """
    layers = [
        l
        for l in QgsProject.instance().mapLayers().values()
        if l.name().startswith("33")
    ]
    layers = [
        l
        for l in QgsProject.instance().mapLayers().values()
        if l.type() == QgsMapLayer.RasterLayer
    ]
    return layers


def export_basedata_as_img(layer, export_path: str):
    """
    Saves ground-truth for layer as .png file.

    Parameters
    ----------
    export_path : str
        Where to save the image.
    layer
        Orthophoto layer to produce ground-truth map from.

    """
    outfile = os.path.join(export_path, "{}_y.png".format(layer.name()))

    settings = QgsMapSettings()
    settings.setLayers(
        [
            QgsProject.instance().mapLayersByName("fkb_bygning_omrade")[0],
            QgsProject.instance().mapLayersByName("fkb_vann_omrade")[0],
        ]
    )
    settings.setBackgroundColor(QColor(0, 0, 0))
    settings.setOutputSize(QSize(layer.width(), layer.height()))
    settings.setExtent(layer.extent())
    render = QgsMapRendererParallelJob(settings)

    def finished():
        img = render.renderedImage()
        img.save(outfile, "png")

    render.finished.connect(finished)
    render.start()
    print("Ground truth image export of {} started.".format(layer.name()))
    from qgis.PyQt.QtCore import QEventLoop

    loop = QEventLoop()
    render.finished.connect(loop.quit)
    loop.exec_()
    print("Ground truth image of {} exported to: {}".format(layer.name(), outfile))


def export_all_ground_truth_maps(export_path: str):
    """
    Exports ground truth images of all orthophoto layers.

    export_path : str
        Where to save the image.
    """
    for l in get_ortophoto_layers():
        export_basedata_as_img(l, export_path)

```
