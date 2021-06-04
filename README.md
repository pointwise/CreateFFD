# CreateFFD
Copyright 2021 Cadence Design Systems, Inc. All rights reserved worldwide.

This script will generate FFD boxes for use with Stanford's SU2 solver. The
boxes are constructed using the extents of grouped connectors (2D) or domains
(3D). The group names are parsed for the parameters necessary to generate the
box. Each box is assigned a layer to help the user declutter the display and
make it easier for the data to be written to the .su2 grid file.

![ScriptImage](https://raw.github.com/pointwise/CreateFFD/master/Grid.png)

To create a group, select the entities and go to *Create, Group*. The group
name should be of the following format: **ffd-name-scale-dimension**.

|Parameter   |Description                                                 |
|:-----------|:-----------------------------------------------------------|
|*ffd*       |Tag to let script know the group will be used for an FFD box|
|*name*      |The name of the FFD box                                     |
|*scale*     |The x,y,z scale vector for the box scaling (default=1.2)    |
|*dimension* |Polynomial degree in the i,j,k directions (>=1, default=1)  |

    Example group name (please note the format): ffd-wing-1.2,1.2,1.2-4,5,2
    Short group name format (will use defaults): ffd-wing

![ScriptImage](https://raw.github.com/pointwise/CreateFFD/master/Groups.png)

Grids created in 2D for SU2 must be in the z-plane. Therefore, when running
in 2D, only x,y compenents of the scale vector and dimension are used. Also,
to invoke 2D mode, ensure that the CAE solver dimension has been set to 2D.

    Example 2D group name (only x,y needed): ffd-wing-1.2,1.2-4,2

The boxes are aligned with the global x,y,z axes. However, database points
are generated at nodes and all connector, domain, and block centroids so
the user can transform the box using the Edit, Transform menu. These points
can be used as anchor points during various transformations.

When the script is run it will check to see if there are any *FFD Layers*
where an FFD layer is a layer with a description of the form: *FFD - XXX*.
The script will then ensure that all *FFD groups* have been used to create
their respective FFD boxes and layers. Lastly, the script will export all
FFD boxes to a file containing all necessary FFD information for SU2. The
file name will be ffd.su2, The script will only export the FFD boxes when
there is a one-to-one match between the groups and the layers. Therefore,
the script must be run at least twice to create all the FFD boxes and then
export them for SU2.

Because the script looks to export all FFD layers whenever it is executed,
FFD layers you would not like to export should be tagged *FFDx* in the
layer description field. For example,

    Script will export the following layer:     FFD - wing
    Script will not export the following layer: FFDx - wing

*Hint: You can organize the layers by name by clicking the Description
column in the Layer panel.*

![ScriptImage](https://raw.github.com/pointwise/CreateFFD/master/Layers.png)

Typically, you'll run this script after you've completed the grid, setup
all the CAE boundary conditions, and exported the .su2 grid file. Once
you create the FFD boxes and export the data, simply concatenate the two
files together like the following example,

    cat grid.su2 ffd.su2 > aircraft.su2

## Disclaimer
This file is licensed under the Cadence Public License Version 1.0 (the "License"), a copy of which is found in the LICENSE file, and is distributed "AS IS." 
TO THE MAXIMUM EXTENT PERMITTED BY APPLICABLE LAW, CADENCE DISCLAIMS ALL WARRANTIES AND IN NO EVENT SHALL BE LIABLE TO ANY PARTY FOR ANY DAMAGES ARISING OUT OF OR RELATING TO USE OF THIS FILE. 
Please see the License for the full text of applicable terms.
