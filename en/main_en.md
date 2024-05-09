# PP Multi-Window

    PP Multi-Window plugin is currently a self use module function required in the author's project. Personalized style and expanded common features. Can adapt to the use of multiple windows and support cross level UI rendering. Can easily create, use, and close functions from the blueprint.

## Project Feature Overview

+ A easy-to-use blueprint function library that supports creating windows, finding windows, and closing windows. Can create a control manager based on the UserWidget type and add it to the already created window.

+ Determine the method of creating the global window based on the creation parameters.Encapsulates several simple window creation methods based on specific methods, without the need for positional calculations. Encapsulates the creation method of sorting using the coordinate arrangement order of the monitor in the system, and quickly locates and creates windows based on the current order.
    > The order here is left ->top ->right ->bottom

+ The blueprint function library encapsulates the relevant functions of the monitor, which can obtain and calculate the relevant information of the monitor. This includes obtaining a list of monitors and obtaining information about the main monitor. And it encapsulates the formatted printing function, which can directly output basic information into FString.

+ Simply inherit and encapsulate the SceneCapture2DComponent, add it to the scene, and set the corresponding parameters to automatically display the rendered image in the created window.

+ The created sub window supports the binding of keyboard input events and mouse input events, simulating the use of ViewportClient. It is possible to pass most input events to PlayerController by binding the PlayerController in the level to the already created sub window.

+ The window supports binding callback events On Window Closed.

## User Guide

[Use In Blueprint](blueprint/guide.md)

[Use In CPP](cpp/guide.md)

## Version Modification

+ 2024/4/23
    > Modified the window creation method for PMWDisplayComponent, selecting to create a new window based on the Display number or finding an already created window for rendering based on the variable "bNewDisplay".

+ 2024/5/9
    > Added a feature that allows for keyboard or mouse input of events in the window and forwarding them to the PlayerController in the bound level.

## End

[Select Language](../README.md)

[选择语言](../README.md)
