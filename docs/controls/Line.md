# Line

A native line graph using D3 based on standard design patterns.

-   [Line](#line)
    -   [Usage](#usage)
        -   [Structure](#structure)
            -   [Multi Line](#multi-line)
            -   [Spline Line](#spline-line)
    -   [JSON Properties](#json-properties)
        -   [Root](#root)
        -   [Data](#data)
            -   [Required](#required)
            -   [Optional](#optional)
        -   [Values](#values)
            -   [Required](#required-1)
            -   [Optional](#optional-1)
        -   [Regions](#regions)
            -   [Required](#required-2)
            -   [Optional](#optional-2)
        -   [Constraints](#constraints)

## Usage

### Structure

You will **not** need all the properties in the example below.
Check out _optional_/_required_ properties explained in the [JSON Properties](#json-properties) section.

```javascript
var root = {
    bindTo: "#root",
    axis: {
        x: {
            type: Carbon.helpers.AXIS_TYPE.TIME_SERIES,
            label: "Some X Label",
            lowerLimit: "2016-01-01T12:00:00Z",
            upperLimit: "2017-01-01T12:00:00Z"
        },
        y: {
            label: "Some Y Label",
            lowerLimit: 0,
            upperLimit: 20
        },
        y2: {
            show: false,
            label: "Some Y2 Label",
            lowerLimit: 0,
            upperLimit: 250
        }
    },
    dateline: [
        {
            showDatelineIndicator: true,
            label: {
                display: "Release A"
            },
            color: Carbon.helpers.COLORS.GREEN,
            shape: Carbon.helpers.SHAPES.DARK.TRIANGLE,
            onClick: (onCloseCB, payload) => {
                // onCloseCB needs to called by the consumer after popup is closed;
                // Payload is the dateline input object
            },
            value: new Date(2016, 5, 1).toISOString()
        }
    ],
    clickPassThrough: {
        datelines: false
    },
    showLabel: true,
    showLegend: true,
    showShapes: true,
    showVGrid: true,
    showHGrid: true
};

var data = {
    key: "uid_1",
    label: {
        display: "Data Label 1"
    },
    regions: [
        {
            axis: "y",
            start: 2,
            end: 10,
            color: "#f4f4f4"
        }
    ],
    shape: Carbon.helpers.SHAPES.DARK.CIRCLE,
    color: Carbon.helpers.COLORS.BLUE,
    onClick: (onCloseCB, key, index, value) => {
        //onCloseCB needs to called by the consumer after popup is closed;
        //This is so that graphing api can remove the selected indicator from data point
    },
    values: [
        {
            x: "2016-02-03T12:00:00Z",
            y: 4
        },
        {
            x: "2016-05-01T12:00:00Z",
            y: 15,
            isCritical: true
        },
        {
            x: "2016-10-01T12:00:00Z",
            y: 10
        }
    ]
};
var lineDefault = Carbon.api.graph(root);
lineDefault.loadContent(Carbon.api.line(data));
```

#### Multi Line

For loading multiple data-sets, you can load as additional content:

```javascript
var lineDefault = Carbon.api.graph(/* Input JSON */);
lineDefault.loadContent(Carbon.api.line(/* Data array A */));
lineDefault.loadContent(Carbon.api.line(/* Data array B */));
lineDefault.loadContent(Carbon.api.line(/* Data array C */));
lineDefault.loadContent(Carbon.api.line(/* Data array D */));
```

#### Spline Line

For a Spline Line Graph, load the following content:

```javascript
var splineData = {
    key: "uid_2",
    label: {
        display: "Data Label 2"
    },
    type: Carbon.helpers.LINE_TYPE.SPLINE,
    values: [
        {
            x: "2016-03-01T12:00:00Z",
            y: 14
        },
        {
            x: "2016-04-10T12:00:00Z",
            y: 1
        },
        {
            x: "2016-11-01T12:00:00Z",
            y: 18
        }
    ]
};
lineDefault.loadContent(Carbon.api.line(splineData));
```

## JSON Properties

### Root

Refer [GraphAPI](../core/GraphAPI.md) `Root` for more details.

### Data

#### Required

| Property Name | Expected | Description                             |
| ------------- | -------- | --------------------------------------- |
| key           | string   | Unique id which represents the data-set |
| values        | Array    | [Values](#values)                       |

#### Optional

| Property Name | Expected | Default            | Description                                                           |
| ------------- | -------- | ------------------ | --------------------------------------------------------------------- |
| yAxis         | string   | "y"                | Setting for using different Y based axis. For now: its either Y or Y2 |
| type          | string   | LINE_TYPE.LINEAR   | Nature of line that needs to be drawn (Linear or Cardinal)            |
| regions       | array    | []                 | Refer [Regions](#regions)                                             |
| label         | object   | {}                 | Display value for the data-set which the data points belong to        |
| color         | string   | COLORS.BLACK       | Color for the data point and line                                     |
| shape         | string   | SHAPES.DARK.CIRCLE | Shape for representing the data points                                |
| onClick       | Function | undefined          | Any action that can be performed when clicking on the data point      |

### Values

#### Required

| Property Name | Expected | Description                                |
| ------------- | -------- | ------------------------------------------ |
| x             | string   | Co-ordinate x, for plotting the data point |
| y             | string   | Co-ordinate y, for plotting the data point |

#### Optional

| Property Name | Expected | Default | Description                                                |
| ------------- | -------- | ------- | ---------------------------------------------------------- |
| isCritical    | boolean  | `false` | Shows an indicator surrounding the data point when enabled |

### Regions

Each data-set can have 1 or more regions. `start` and `end` is necessary for rendering a horizontal area.

#### Required

| Property Name | Expected | Description         |
| ------------- | -------- | ------------------- |
| start         | number   | Start of the region |
| end           | number   | End of the region   |

#### Optional

| Property Name | Expected | Default                                                            | Description                           |
| ------------- | -------- | ------------------------------------------------------------------ | ------------------------------------- |
| axis          | string   | "y"                                                                | Defines which axis if represents from |
| color         | string   | ![#f4f4f4](https://placehold.it/15/f4f4f4/000000?text=+) `#f4f4f4` | Default color of the region area      |

### Constraints

-   If data-set `label` display is not provided then the legend item will not be shown as well
