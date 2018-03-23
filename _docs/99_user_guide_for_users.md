
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

_Note: These sections require a user profile; either create a profile (see section: XXXXXXXXX or email: XXXXXXX@XXXXXXXX to request assistance)._

_In addition, please inform the XXXXXXXX team or email XXXX@XXXX when a new style is created, in order to check that the style has been applied correctly_

Geonode will automatically style the layer that has been uploaded with a default style. It is likely that this will not be what you as a user requires. Therefore, this section explores how to

- Create and apply simple styles
- Create and apply more advanced styles based on the attributes of the uploaded layer.
- Manage styles and apply a default style.

#### Basic Styling

Simple styles not based on attributes

##### Example 1: .....
WHAT IS THE EXAMPLE?

Start by opening the Layer Styles pop up box. This can be opened by selecting 'Edit Layer' button in the top right, then 'Edit' in the Styles column. Alternatively select the paintbrush icon at the top of the map viewer

IMAGE_HERE (multiple)

Now set a Name and Description for your new style. This can be completed by selecting 'Edit' in the first section of the pop up.

_Please add a sensible description and title of the style which you create._

IMAGE_HERE

Now, click on the 'Rule'. As a default this should be named 'Untitled 1' and click on 'Edit'. An additional pop up should appear. This pop up displays three tabs: 'Basic', 'Labels' and 'Advanced'.


---

- Basic
  - Name the rule
  - If a point feature, the symbol to represent a feature and its size and rotation
  - Colour and opacity of the feature
  - Style, colour and width of the line feature

IMAGE_HERE (show a before and after?)

- Labels (_Note: these options will only be referred to in the "Advanced Styling" section_)
  - Tick box to select if a label is required
  - Field to use as the content of the label
  - Font colour and opacity
  - Additional advanced options

IMAGE_HERE

---

- Advanced (_Note: these options will only be referred to in the "Advanced Styling" section_)
  - Limit by scale
  - Limit by condition
    - If this condition is successful, the layer will show only the successful result, styled as in the 'Basic' tab

When working with the advanced styling options it is useful for the user to know the attribute table which the layer represents.

---

Using the Basic and Label tabs style the layer.

This may vary between points, lines and polygon shapefiles.


#### Advanced Styling

Rule based styling

Geonode allows styling based on attributes of the uploaded layer. This is one step further than the Basic Styling tutorial above.

##### Example 1: Rule Based Styles
In the basic styling tutorial above we could only style all attributes the same, all regions represented as a single colour. This tutorial will show you how to create 'rule based styles'.

For example, you have uploaded a polygon layer with two columns, 'name' and 'value'. Lets assume the value is related to vulnerability, with more vulnerable as red, represented by a value of 10, and less vulnerable, represented by a value of 0.

Start by opening the Layer Styles pop up box. This can be opened by selecting 'Edit Layer' button in the top right, then 'Edit' in the Styles column.

IMAGE_HERE

Following the same instructions above, name and set a description for your new style.

As above select the 'Rule' which has been provided and select 'Edit'. As above the 'Style Rule' pop up box should appear,  where you find three tabs: 'Basic', 'Labels' and 'Advanced'. To find out about each of these options, reference the previous section: Basic Styling.

In the example, we will classify the vulnerability into several groups progressively getting darker: 0 (blank), 1-2 (light red) ..., 9-10 (dark red)

Select the 'Advanced' tab and select 'limit by condition'. Here you will be able to create a statement where if the layer fulfills this statement it will be styled as per the 'Basic' tab.

We will start with the most vulnerable 9-10. The condition which we will set will be: Match 'any' of the following: 'value' 'between' '9' '10'. As you complete the statement the map should update. Now go to the 'basic' tab and style this rule as dark red. Click 'Save'.

Back on the 'Layer Styles' pop-up duplicate the rule that you have just created and edit this rule, as above, but for e.g. vulnerability level 7-8, and so on.

Once all rules have been created, click save. Your layer should now be styled as a choropleth.

##### Example 2: Styles with Labels



Labels....
Need to make sure not to destroy a label!










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

#### Administrators: Styles in Geoserver

Once a new style has been added to Geonode, an administrator would need to edit the style within GeoServer.



new style names in Geoserver
update these! and ensure that the layer it links to works!
