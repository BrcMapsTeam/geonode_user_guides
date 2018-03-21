
wfp have a good user guide (both basic and advanced)
https://geonode.wfp.org/

https://cdnmedia.readthedocs.org/pdf/geonode/stable/geonode.pdf

Any other references?
...

---

# Geonode User Guide

## Introduction

What is geonode?

Purpose of the document?

Who is the document for?

Key terms (attribute table etc.) - or should there be a more comprehensive glossary as an annex?

## 2. ???

_What is the minimum people need to know?_

Navigate the homepage

Choose a layer(s)

Create a basic map using existing layers etc.

Choose a style.

Download a layer.

The Map composer screen, identify, query, measure and navigate

Creating a map using the map composer screen.

Saving/publishing a map (this needs a login)

## 3. ???

_everything else_

Register, Sign in and edit profile

KEY CONCEPT - Naming convention - doc still to be created.

Adding a new layer

Permissions

Layer info, metadata and styling a vector layer (choosing a style, managing styles, creating a simple style, creating a rule based
style).

Raster layer styling

__if you create a style email us! Will need to edit in geoserver; name of sld for example__



Refer back to point 2 about how to create a map



Saving a map



Additional layer options

Edit metadata, remove layers, remove styles.


This is in the map composer view...

Creating and editing a layer from scratch (this must be last!) Referred to a "modifying a layer" in the wfp guide


Uploading documents, static maps and images

Using geonode in QGIS (2.x and 3.x), or ArcGIS (wfs vs wms) - basic rules here

### Styling

_Note: These sections require a user profile; either create a profile (see section: XXXXXXXXX or email: XXXXXXX@XXXXXXXX to request assistance)_

Geonode will automatically style the layer that has been uploaded with a default style. It is likely that this will not be what you as a user requires. Therefore, this section explores how to

- Create and apply simple styles
- Create and apply more advanced styles based on the attributes of the uploaded layer.
- Manage styles and apply a default style.

#### Basic Styling

Simple styles not based on attributes

#### Advanced Styling

Rule based styling

#### Managing Styles

Once a style has been created, you will then need to ensure that it is either the default style or can be selected by a user.

At the top right of the page of the layer page, click on the 'Edit Layer' button. A pop-up should appear

IMAGE_HERE (x2)

Under 'Styles' select 'Manage'. This should open the 'Manage Styles' page for the layer.

_Note: Changes to 'Manage Styles' will only affect the layer that is being worked on_

There should be 3 boxes visible.

- 'Layer default Style': Select the default style here.
- 'Available Styles':
  - Box 1: All available existing styles are listed here.
  - Box 2: This lists all the styles that can be selected for the given layer.

In order for a 'Layer Default Style' to be selected, first you will need to select some 'Available Styles'. Click on one, or more, of the styles in box 1 (left) to bring them across to box 2 (right). Once in box 2, they can be selected to style the layer and be chosen to be the default style

IMAGE_HERE
