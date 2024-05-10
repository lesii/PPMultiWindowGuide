# PP Multi-Window

- [PP Multi-Window](#pp-multi-window)
  - [CreateWindow](#createwindow)
    - [FPMWCreateParams Define](#fpmwcreateparams-define)
    - [1.Global Create](#1global-create)
    - [2.Create By Monitor](#2create-by-monitor)
    - [3.Create By Display](#3create-by-display)
    - [4.Creat Display FullWindow](#4creat-display-fullwindow)
  - [Create a UserWidget control handle](#create-a-userwidget-control-handle)
    - [1.Create Handle By Type](#1create-handle-by-type)
    - [2.Add Widget and return handle](#2add-widget-and-return-handle)
  - [Window access function](#window-access-function)
    - [1.Add Handle](#1add-handle)
    - [2.Remove Handle](#2remove-handle)
    - [3.Add UserWidget](#3add-userwidget)
    - [4.Remove UserWidget](#4remove-userwidget)
    - [5.Get Window Index](#5get-window-index)
    - [6.Check Valid](#6check-valid)
    - [7.Bind PlayerController](#7bind-playercontroller)
    - [8.Clear PlayerController](#8clear-playercontroller)
    - [9.Set Mouse Capture](#9set-mouse-capture)
    - [10.Bind On Window Destroyed](#10bind-on-window-destroyed)
  - [UserWidget Control Handle](#userwidget-control-handle)
    - [1.Remove From Window](#1remove-from-window)
    - [2.Get Widget](#2get-widget)
  - [Blueprint function Libraray](#blueprint-function-libraray)
    - [1.Get Window Handle](#1get-window-handle)
    - [2.Clear All Windows](#2clear-all-windows)
    - [3.Close Window](#3close-window)
  - [Monitor](#monitor)
    - [FPPDisplayInfo Define](#fppdisplayinfo-define)
    - [1.Get Primary Info](#1get-primary-info)
    - [2.Get All Info](#2get-all-info)
    - [3.Get All Info By Display](#3get-all-info-by-display)
    - [4.Get Info By Monitor Index](#4get-info-by-monitor-index)
    - [5.Get Info By Display Index](#5get-info-by-display-index)
    - [6.Get Global Rect](#6get-global-rect)
    - [7.Monitor Global To Relative](#7monitor-global-to-relative)
    - [8.Monitor Relative To Global](#8monitor-relative-to-global)
    - [9.Display Global To Relative](#9display-global-to-relative)
    - [10.Display Relative To Global](#10display-relative-to-global)
    - [11.Format Print DisplayInfo](#11format-print-displayinfo)
  - [End](#end)

## CreateWindow

### FPMWCreateParams Define

![node](../../assets/blueprint/createparams.png)

| Data              |   Type           |   Describe                                                |
|   :-:             |     :-:          |   :-                                                     |
| WinIdx            |   int32          | Create a handle index for the window, which is automatically added to the end by default  |
| Title             |   FText          |  Window title, if empty, defaults to borderless|
| CaptureMode       |EMouseCaptureMode |    Determine the mouse capture method for the window (continuous capture, mouse down capture, right mouse button capture)|
| Draw              |FPMWPainter       |    Window drawing position and size information                                 |
| Rule              |FPMWRuleParams    |    Create alignment and window type for windows                              |
| Border            |FPMWBorderParams  |    Create window border parameters                                       |
| Transparency      |FPMWOpacityParams |    Create transparent parameters for windows                                       |

</br>

### 1.Global Create

![node](../../assets/blueprint/create_global.png)

- Define parameter information to create a sub window and return the created window handle. You can define the index number of the creation window in the parameters. If it is -1, the creation will be automatically added at the end.

| Parameter            |Type              |   Describe                                                   |
|   :-:             |     :-:         |   :-                                                     |
| bCreateToWorld    |   bool          | Whether the window is bound to the world, check to destroy the window after switching scenes (handle not destroyed)  |
| bRecreate         |   bool          |  Is it necessary to recreate the window index based on the parameters passed in? If not checked, parameter modifications will be made on the window found|
| Params            |FPMWCreateParams |    Creating parameter passing determines the way the window is created                       |

</br>

### 2.Create By Monitor

![node](../../assets/blueprint/create_monitor.png)

- Similar to the global creation method, but the position information filled in the drawing parameters will be converted and created based on the local coordinate system of the monitor. Added a parameter for the monitor index, which is obtained based on the index in the hardware.

| Parameter            |Type              |   Describe                                                                       |
|   :-:             |     :-:         |   :-                                                                         |
| bCreateToWorld    |   bool          | Whether the window is bound to the world, check to destroy the window after switching scenes (handle not destroyed)                       |
| bRecreate         |   bool          |  Is it necessary to recreate the window index based on the parameters passed in? If not checked, parameter modifications will be made on the window found   |
| MonitorIdx        |       int32     |    Monitor Index by hardware sort                                   |
| Params            |FPMWCreateParams |    Creating parameter passing determines the way the window is created                                           |

</br>

### 3.Create By Display

![node](../../assets/blueprint/create_display.png)

- Similar to the global creation method, but the position information filled in the drawing parameters will be converted and created based on the local coordinate system displayed by the system. Added a system display index parameter, obtained by sorting the monitor coordinates.

| Parameter            |Type              |   Describe                                                                       |
|   :-:             |     :-:         |   :-                                                                         |
| bCreateToWorld    |   bool          | Whether the window is bound to the world, check to destroy the window after switching scenes (handle not destroyed)                       |
| bRecreate         |   bool          |  Is it necessary to recreate the window index based on the parameters passed in? If not checked, parameter modifications will be made on the window found   |
| DisplayIdx        |       int32     |    Display Index sorted by monitor display rect                                    |
| Params            |FPMWCreateParams |    Creating parameter passing determines the way the window is created                                           |

</br>

### 4.Creat Display FullWindow

![node](../../assets/blueprint/create_relative.png)

- Create a window directly into the set Display without making any other parameter selections.

| Parameter            |Type              |   Describe                                                                       |
|   :-:             |     :-:         |   :-                                                                         |
| bCreateToWorld    |   bool          | Whether the window is bound to the world, check to destroy the window after switching scenes (handle not destroyed)                       |
| DisplayIdx        |       int32     |    Display index, sort the monitor coordinates by left>top>right>bottom to obtain the monitor index, which is more convenient for determining the display position.                                    |

</br>

[Back To Catalog](#pp-multi-window)

</br>

## Create a UserWidget control handle

### 1.Create Handle By Type

![node](../../assets/blueprint/create_wdigethandle.png)

- Generate a control based on UserWidgetType and return the control handle. This control is bound to the world and will be destroyed when switching levels.

| Parameter            |Type              |   Describe                                                                       |
|   :-:             |     :-:         |   :-                                                                         |
| WidgetType        |   TSubclassOf\<UUserWidget>         | Widget Class Type                       |

</br>

### 2.Add Widget and return handle

![node](../../assets/blueprint/add_widget.png)

- Add an instantiated control object to a created window and return the control handle of the control. This control will not be automatically destroyed, but the handle will be automatically destroyed.

| Parameter            |Type                    |   Describe                       |
|   :-:            |     :-:                |   :-                         |
| WindowIdx        |   int32                | Window Idx                      |
| Widget           |   UUserWidget*         | Widget Object pointer           |

</br>

[Back To Catalog](#pp-multi-window)

</br>

## Window access function

### 1.Add Handle

![node](../../assets/blueprint/add_handle.png)

- Add the controls managed by the created control handle to the currently selected window

| Parameter            |Type                    |   Describe                       |
|   :-:            |     :-:                |   :-                         |
| WidgetHandle     |   APMWWidget*          | widget handle                  |
| ZOrder           |   int32                | z order      |

</br>

### 2.Remove Handle

![node](../../assets/blueprint/remove_handle.png)

- Remove the widget managed by the created control handle from the current window

| Parameter            |Type                    |   Describe                       |
|   :-:            |     :-:                |   :-                         |
| WidgetHandle     |   APMWWidget*          | widget handle                  |

</br>

### 3.Add UserWidget

![node](../../assets/blueprint/add_widget_direct.png)

- Add the already created widget object to the current window, which is not managed by a handle

| Parameter            |Type                    |   Describe                       |
|   :-:            |     :-:                |   :-                         |
| InWidget         |   UUserWidget*         | UserWidget Object Pointer                  |
| ZOrder           |   int32                | z order      |

</br>

### 4.Remove UserWidget

![node](../../assets/blueprint/remove_widget_direct.png)

- Remove the already created widget object from the current window

| Parameter            |Type                    |   Describe                       |
|   :-:            |     :-:                |   :-                         |
| InWidget         |   UUserWidget*         | UserWidget Object Pointer                  |

</br>

### 5.Get Window Index

![node](../../assets/blueprint/get_displayindex.png)

</br>

### 6.Check Valid

![node](../../assets/blueprint/is_valid.png)

- Check if the window handle manages a valid window. If the window does not exist, return false
  
</br>

### 7.Bind PlayerController

![node](../../assets/blueprint/bind_worldplayer.png)

- Bind the window to PlayerController. This method allows you to pass an Input Event to the currently bound PlayerController in the window. If you select Create To World during creation, it will automatically bind the PlayerController for the current level.

| Parameter                 |Type                    |   Describe                       |
|   :-:                 |     :-:                |   :-                         |
| InPlayerController    |   APlayerController*   |  Current Level PC    |
  
</br>

### 8.Clear PlayerController

![node](../../assets/blueprint/clear_worldplayer.png)

- Removing the bound PlayerController from the window can prevent input events from being received by the PlayerController.
  
</br>

### 9.Set Mouse Capture

![node](../../assets/blueprint/set_capture_mode.png)

- This method simulates the capture design of GameViewportClient and determines the mouse capture method for the current window based on parameters.

| Parameter                 |Type                    |   Describe        |
|   :-:                 |     :-:                |   :-          |
| InMouseCaptureMode    |   EMouseCaptureMode   |  Mouse Capture Mode   |

</br>

### 10.Bind On Window Destroyed

![node](../../assets/blueprint/bind_window_destroyed.png)

- Assign event On Window Destroyed

| Parameter                 |Type                    |   Describe        |
|   :-:                 |     :-:                |   :-          |
| InHandle    |   UPMWHandle*   |  closed window handle   |

</br>

[Back To Catalog](#pp-multi-window)

</br>

## UserWidget Control Handle

### 1.Remove From Window

![node](../../assets/blueprint/remove_from_window.png)

</br>

### 2.Get Widget

![node](../../assets/blueprint/get_widget.png)

</br>

[Back To Catalog](#pp-multi-window)

</br>

## Blueprint function Libraray

### 1.Get Window Handle

![node](../../assets/blueprint/get_window_handle.png)

- Returns the window handle based on the window index.

| Parameter                 |Type                    |   Describe        |
|   :-:                 |     :-:                |   :-          |
| WindowIdx             |   int32                |  Returns the window handle based on the window index   |

</br>

### 2.Clear All Windows

![node](../../assets/blueprint/clear_windows.png)

</br>

### 3.Close Window

![node](../../assets/blueprint/close_window.png)

- Close Window By passed Window Index。

| Parameter                 |Type                    |   Describe        |
|   :-:                 |     :-:                |   :-          |
| WindowIdx             |   int32                |  Window Index   |

</br>

[Back To Catalog](#pp-multi-window)

</br>

## Monitor

### FPPDisplayInfo Define

![node](../../assets/blueprint/display_info.png)

| 数据              |Type            |   Describe                    |
|   :-:             |     :-:       |   :-                      |
| Name              |   FString     |    Monitor Name                 |
| ID                |   FString     |    MonitorID                  |
| Size              |   FVector2D   |    Monitor Resolution            |
| Rect              |   FMargin     |    MonitorRect Info          |
| bIsPrimary        |   bool        |    Is Primary Monitor          |

</br>

### 1.Get Primary Info

![node](../../assets/blueprint/primary_display.png)

</br>

### 2.Get All Info

![node](../../assets/blueprint/all_display.png)

</br>

### 3.Get All Info By Display

![node](../../assets/blueprint/all_sort_display.png)

</br>

### 4.Get Info By Monitor Index

![node](../../assets/blueprint/get_info_monitor.png)

</br>

### 5.Get Info By Display Index

![node](../../assets/blueprint/get_info_display.png)

</br>

### 6.Get Global Rect

![node](../../assets/blueprint/global_rect.png)

</br>

### 7.Monitor Global To Relative

![node](../../assets/blueprint/m_global_relative.png)

</br>

### 8.Monitor Relative To Global

![node](../../assets/blueprint/m_relative_global.png)

</br>

### 9.Display Global To Relative

![node](../../assets/blueprint/d_global_relative.png)

</br>

### 10.Display Relative To Global

![node](../../assets/blueprint/d_relative_global.png)

</br>

### 11.Format Print DisplayInfo

![node](../../assets/blueprint/print_info.png)

</br>

[Back To Catalog](#pp-multi-window)

</br>

## End

[Back Introduction](../main_en.md)
