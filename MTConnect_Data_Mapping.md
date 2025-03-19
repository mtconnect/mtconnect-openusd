---
orphan: true
---
# MTConnect and OpenUSD Conceptual Data Mapping Document 

```{important}
**{octicon}`tag;1em` Template Version:** {bdg-secondary}`1.0.0` 
<br>**{octicon}`calendar;1em` Last Update:** {bdg-secondary}`2025-03-12`
```

## Introduction

MTConnect is an open source standard that defines a domain specific semantic vocabulary for manufacturing equipment. The MTConnect standard provides a normative information model and protocol for retrieving information from manufacturing equipment.

MTConnect data sources include things like production equipment, sensor packages, and other hardware. Applications using MTConnect data provide more efficient operations, improved production optimization, and increased productivity.

OpenUSD (Universal Scene Description) is an open-source, scalable 3D framework for interchange of 3D computer graphics data. It provides a robust way to describe, compose, and simulate complex 3D scenes.

In the context of manufacturing, OpenUSD is valuable for creating digital twins, which are virtual representations of physical assets or processes. This enables simulations, visualizations, and analyses that can improve efficiency and reduce downtime.

The mapping of MTConnect data into OpenUSD allows for the creation of dynamic, near real-time digital twins of manufacturing environments.

### Overview

This conceptual data mapping defines how the MTConnect semantics, is translated, and represented within the OpenUSD framework. This allows for accurate and effective use of manufacturing data within a 3D enviroment.

This integration enables:

* Real-time visualization: Live data from MTConnect can drive the animation and behavior of 3D models in OpenUSD, providing a visual representation of the current state of production.

* Simulation and analysis: By combining real-time data with 3D models, manufacturers can simulate different scenarios and analyze potential issues before they occur.

* Enhanced collaboration: OpenUSD facilitates collaboration by allowing different stakeholders to access and interact with the same 3D data.

### References
This document has been prepared in reference to the software or specification versions listed below. Adjustments or considerations may need to be made for previous or future versions than those referenced in this document.

#### MTConnect Reference

| Version | Reference Documents  |
|---|---|
|v2.5|[MTConnect Information Model](https://model.mtconnect.org/), [MTConnect GitHub](https://github.com/mtconnect), [MTConnect C++ Agent](https://github.com/mtconnect/cppagent)|


#### OpenUSD Reference

| Version | Reference Documents  |
|---|---|    
| 24.08  | [OpenUSD C++ and Schema Documentation](https://openusd.org/release/api/index.html), [OpenUSD Github Repository](https://github.com/PixarAnimationStudios/OpenUSD), [USD Terms and Concepts](https://openusd.org/release/glossary.html)  |     


### General Assumptions and Constraints

The Template Version `v1.0.0` focuses only one On-Way mapping; MTConnect -> OpenUSD.

### Definitions, Acronyms, Abbreviations

Following are the MTConnect terms used in this document. Kindly refer to the [MTConnect Information Model](https://model.mtconnect.org/) for more information.

| MTConnect Term | Definition |
|---|---|    
| `Component`  |  logical or physical entity that provides a capability. |     
| `Device`  | `Component` composed of a piece of equipment that produces observations about itself. |
| `DataItem` | information reported about a piece of equipment. |
| `Observation` | telemetry data for a `DataItem` at a point in time. |

| Component Type | Definition |
|---|---|   
| `Axis` | `Component` composed of a motion system that provides linear or rotational motion for a piece of equipment. |
| `Rotary` | `Axis` that provides rotation about a fixed axis. |

| DataItem Type | Definition |
|---|---| 
| `ANGLE` | angular position. |
| `ANGULAR_VELOCITY` | rate of change of angular position. |

| DataItem subType | Definition |
|---|---| 
| `ACTUAL` | reported value of an observation. |
| `MEASURED` | `ACTUAL` that has uncertainty. |
| `COMMANDED` | directive value including adjustments such as an offset or overrides. |

## Concepts

| MTConnect | OpenUSD | Description |
|---|---|---|
|[Component](#Component)| Prim | |
|[Device](#Device)| Prim |Device is a type of Component and is composed of one or more Components.|
|[DataItem](#DataItem) || DataItem does not directly map to any openUSD concept but DataItem properties can utilize `customData` to store all DataItem properties (metadata).|
|[Observation](#Observation) |Attribute| Observation made for a DataItem maps directly onto the corressponding Attribute of a Prim.|

### Component

An MTConnect Component maps to a Prim in openUSD.

#### Properties
| MTConnect | OpenUSD | Description |
|---|---|---|
|[id](#component-id)|prim.name|Component id maps onto the Name of the corressponding Prim.|
|[component_path](#component_path)|customData.mtconnect:component_path|XML Path to the location of Component in the MTConnectDevices Response Document.|

##### Component: id

|  | Name | Data Type |
|---|---|---|
|MTConnect|id| ID |
|OpenUSD|prim.name| string |

##### component_path

NOT an MTConnect property. To be used in OpenUSD to provide XML path to the location of the Component in the MTConnectDevices Response Document.

|  | Name | Data Type |
|---|---|---|
|OpenUSD|customData.mtconnect:component_path| string |

### Device

An MTConnect Device maps to a Prim in openUSD. It may have multiple Prims defined within.

#### Properties
| MTConnect | OpenUSD | Description |
|---|---|---|
|[id](#device-id)|prim.name|Component id maps onto the Name of the corressponding Prim.|
|[mtconnectdevices_uri](#mtconnectdevices_uri)|customData.mtconnect:mtconnectdevices_uri|URI of the MTConnectDevices Response Document.|
|[device_path](#device_path)|customData.mtconnect:device_path|XML Path to the location of Device in the MTConnectDevices Response Document.|

##### Device: id

|  | Name | Data Type |
|---|---|---|
|MTConnect|id| ID |
|OpenUSD|prim.name| string |

##### mtconnectdevices_uri

NOT an MTConnect property. To be used in OpenUSD to provide URI of the MTConnectDevices Response Document.

|  | Name | Data Type |
|---|---|---|
|OpenUSD|customData.mtconnect:mtconnectdevices_uri| asset |

##### device_path

NOT an MTConnect property. To be used in OpenUSD to provide XML path to the location of the Device in the MTConnectDevices Response Document.

|  | Name | Data Type |
|---|---|---|
|OpenUSD|customData.mtconnect:device_path| string |

### DataItem

An MTConnect DataItem does not directly map to an openUSD concept. `customData` can be used to store the DataItem metadata. See [MTConnect Namespace](#mtconnect-namespace) for details on how to use MTConnect semantics within `customData`.

#### Properties
| MTConnect | OpenUSD | Description |
|---|---|---|
|[id](#dataitem-id)|customData.mtconnect:dataitem:id|MANDATORY|
|[name](#dataitem-name)|customData.mtconnect:dataitem:name|OPTIONAL but recommended.|
|[units](#dataitem-units)|customData.mtconnect:dataitem:|MANDATORY for `category` = `SAMPLE`. `units` **MUST** be one of [MTConnect Units](https://model.mtconnect.org/#Enumeration__EAID_8FEC81E4_8E1F_4f45_820B_F9F25DD83F9A)|
|[type](#dataitem-type)|customData.mtconnect:dataitem:type|MADATORY. `type` **MUST** be one of [MTConnect Event Types](https://model.mtconnect.org/#Enumeration___19_0_3_45f01b9_1580398379726_606068_12802), [MTConnect Sample Types](https://model.mtconnect.org/#Enumeration___19_0_3_45f01b9_1580398370126_672808_12777), [MTConnect Condition Types](https://model.mtconnect.org/#Enumeration___19_0_3_45f01b9_1580398386435_855466_12827)|
|[subType](#dataitem-subType)|customData.mtconnect:dataitem:subType|OPTIONAL. If given, **MUST** be one of [MTConnect DataItem SubTypes](https://model.mtconnect.org/#Enumeration___19_0_3_45f01b9_1579563592155_977172_22064)|
|[category](#dataitem-category)|customData.mtconnect:dataitem:category|MANDATORY. `category` **MUST** be one of [MTConnect DataItem Category](https://model.mtconnect.org/#Enumeration___19_0_3_91b028d_1579277872728_249968_3735)|

##### DataItem: id

|  | Name | Data Type |
|---|---|---|
|MTConnect|id| string |
|OpenUSD|customData.mtconnect:dataitem:id| string |

##### DataItem: name

|  | Name | Data Type |
|---|---|---|
|MTConnect|name| string |
|OpenUSD|customData.mtconnect:dataitem:name| string |

##### DataItem: units

|  | Name | Data Type |
|---|---|---|
|MTConnect|units| string |
|OpenUSD|customData.mtconnect:dataitem:units| string |

##### DataItem: type

|  | Name | Data Type |
|---|---|---|
|MTConnect|type| string |
|OpenUSD|customData.mtconnect:dataitem:type| string |


##### DataItem: subType

|  | Name | Data Type |
|---|---|---|
|MTConnect|subType| string |
|OpenUSD|customData.mtconnect:dataitem:subType| string |


##### DataItem: category

|  | Name | Data Type |
|---|---|---|
|MTConnect|category| string |
|OpenUSD|customData.mtconnect:dataitem:category| string |

### Observation

An MTConnect Observation directly maps onto an Attribute defined for a Prim. 

The Observation mapping is not explicitly mentioned in the OpenUSD document but can be inferred by the customData for that Attribute with the DataItem properties.

Following is an example of an Observation mapped to an Attribute.

```
float drive:angular:physics:targetPosition = 0 (
    customData {
        string mtconnect:dataitem:name="j1.state.angular.physics.targetPosition"
        string mtconnect:dataitem:id = "j1_targetPosition"
        string mtconnect:dataitem:type = "ANGLE"
        string mtconnect:dataitem:subType="COMMANDED"
        string mtconnect:dataitem:units= "DEGREE"
        string mtconnect:dataitem:category="SAMPLE"
    }
)
```

Here the value of `drive:angular:physics:targetPosition` is mapped to `Observation` for `DataItem:type` `ANGLE`.

When ingesting MTConnect telemetry data into Omniverse, the only dynamic data is Observation and is mapped onto the value of the corressponding Attribute.

## Appendices

### MTConnect Namespace

The `mtconnect` namespace is used to store data adhering to the MTConnect standard, providing metadata and references to MTConnect device information.

Attributes within the `mtconnect` namespace should conform to the following:

`asset mtconnect:mtconnectdevices_uri (asset):`

- Purpose: Stores the URI or file path to an MTConnectDevices XML or data file.
- Expected Value: A valid file path or URI (e.g., "file:///path/to/devices.xml", "http://example.com/devices.xml").
- Example: asset mtconnect:mtconnectdevices_uri = @./mtconnect_devices.xml@

`string mtconnect:device_path (string):`
- Purpose: Stores the MTConnect Device path.
- Expected Value: A string representing the device path within the MTConnect data structure.
- Example: string mtconnect:device_path = "/MTConnectDevices/Devices/Device[\<uuid\>]"

`string mtconnect:component_path (string):`
- Purpose: Stores the MTConnect Component path.
- Expected Value: A string representing the component path within the MTConnect data structure.
- Example: string mtconnect:component_path = "//Devices/Device[\<uuid\>]/Components/Axes[\<id\>]"

string mtconnect:dataitem:name (string):
- Purpose: Stores the MTConnect DataItem name.
- Expected Value: A string representing the name of the data item.
- Example: string mtconnect:dataitem:name = "X_Axis_Position"

`string mtconnect:dataitem:id (string):`
- Purpose: Stores the MTConnect DataItem ID.
- Expected Value: A string representing the unique identifier of the data item.
- Example: string mtconnect:dataitem:id = "d1234"

`string mtconnect:dataitem:type (string):`
- Purpose: Stores the MTConnect DataItem type.
- Expected Value: A string representing the data type of the data item (e.g., "POSITION", "TEMPERATURE", "LOAD").
- Example: string mtconnect:dataitem:type = "ANGLE"

`string mtconnect:dataitem:subType (string):`
- Purpose: Stores the MTConnect DataItem subType.
- Expected Value: A string representing a more specific sub-type of the data item (e.g., "ACTUAL", "COMMANDED", "TARGET").
- Example: string mtconnect:dataitem:subType = "ACTUAL"

`string mtconnect:dataitem:units (string):`
- Purpose: Stores the units of measurement for the Observations made for MTConnect DataItem.
- Expected Value: A string representing the units (e.g., "MILLIMETER", "CELSIUS", "DEGREE").
- Example: string mtconnect:dataitem:units = "DEGREE"

`string mtconnect:dataitem:category (string):`
- Purpose: Stores the MTConnect DataItem category.
- Expected Value: A string representing the category of the data item (e.g., "SAMPLE", "EVENT", "CONDITION").
- Example: string mtconnect:dataitem:category = "SAMPLE"
