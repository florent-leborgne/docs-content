---
navigation_title: Heat map charts
applies_to:
  stack: ga
  serverless: ga
description: Instructions and best practices for building heat map charts with Kibana Lens in Elastic.
---

# Build heat map charts with {{kib}}

Heat map charts display data as a grid of colored cells, where each cell's color represents the magnitude of a value. They are ideal for visualizing patterns across two dimensions, identifying correlations, and spotting anomalies in large datasets.

You can create heat map charts in {{kib}} using [**Lens**](../lens.md).

<!-- TODO: Add screenshot
![Example Lens heat map chart showing request counts by hour and day](/explore-analyze/images/heat-map-chart-example.png)
-->

## When to use heat map charts

Heat map charts work best when:

* You need to visualize data across **two categorical or temporal dimensions**
* You want to identify **patterns, trends, or anomalies** in your data
* You have a **large dataset** with many data points
* You need to compare **intensity or frequency** across categories

Consider using [bar charts](bar-charts.md) instead when:

* You only have **one dimension** to visualize
* You need precise **value comparisons**
* You have a **small number of data points**

Consider using [line charts](line-charts.md) instead when:

* You want to show **trends over time** for a single metric
* You need to compare **multiple series** directly

## Build a heat map chart

:::{include} ../../_snippets/lens-prerequisites.md
:::

To build a heat map chart:

::::::{stepper}

:::::{step} Access Lens
**Lens** is {{kib}}'s main visualization editor. You can access it:
- From a dashboard: On the **Dashboards** page, open or create the dashboard where you want to add a heat map chart, then add a new visualization.
- From the **Visualize library** page by creating a new visualization.
:::::

:::::{step} Set the visualization to Heat map
New visualizations often start as **Bar** charts.

Using the **Visualization type** dropdown, select **Heat map**.
:::::

:::::{step} Define the data to show
1. Select the {{data-source}} that contains your data.
2. Configure the [**Horizontal axis**](#horizontal-axis-settings) dimension to define the columns of the heat map.
3. Configure the [**Vertical axis**](#vertical-axis-settings) dimension to define the rows of the heat map.
4. Configure the [**Cell value**](#cell-value-settings) dimension to define the metric that determines cell colors.

Optionally:
   - Add a **Breakdown** dimension to split the heat map into multiple charts.

The chart preview updates to show a grid of colored cells. Cell colors represent the magnitude of the metric value. If the grid appears empty, verify that both axes have data for the current time range.
:::::

:::::{step} Customize the chart to follow best practices
Tweak the appearance of the chart to your needs. Consider the following best practices:

**Choose appropriate dimensions**
:   Select dimensions that have a reasonable number of distinct values. Too many values create unreadable grids with tiny cells.

**Use sequential color palettes**
:   For data that ranges from low to high, use a sequential palette (light to dark). Reserve diverging palettes for data with a meaningful midpoint.

**Consider data density**
:   If cells are too small to read, reduce the number of buckets or use a smaller time interval on your axes.

**Order categories meaningfully**
:   For categorical axes, order values logically (alphabetically, by frequency, or by a natural ordering like days of the week).

Refer to [Heat map chart settings](#heat-map-chart-settings) to find all configuration options for your heat map chart.
:::::

:::::{step} Save the chart
- If you accessed Lens from a dashboard, select **Save and return** to save the visualization and add it to that dashboard, or select **Save to library** to add the visualization to the Visualize library and reuse it later.
- If you accessed Lens from the Visualize library, select **Save**. A menu opens and offers you to add the visualization to a dashboard and to the Visualize library.
:::::

::::::

## Advanced heat map chart scenarios

### Visualize activity patterns over time [time-patterns]

Heat maps are excellent for spotting temporal patterns, such as peak usage hours or seasonal trends.

#### Example: Website traffic by hour and day

This example uses the [sample web logs data](/manage-data/ingest/sample-data.md) to visualize when website traffic is highest.

1. Create a **Heat map** chart using the **{{kib}} Sample Data Logs** {{data-source}}.
2. For the **Horizontal axis**, select `@timestamp` with **Date histogram** and interval set to **Hour of day**.
3. For the **Vertical axis**, select `@timestamp` with **Date histogram** and interval set to **Day of week**.
4. For the **Cell value**, select **Count** to show the number of requests.
5. Select {icon}`brush` **Style** and choose a sequential color palette.

The resulting heat map shows traffic intensity across hours and days, making it easy to identify peak periods.

### Compare categories with a heat map [category-comparison]

Use heat maps to compare metrics across two categorical dimensions.

#### Example: Response codes by geographic region

1. Create a **Heat map** chart using your web logs {{data-source}}.
2. For the **Horizontal axis**, select `geo.src` with **Top values** (top 10 countries).
3. For the **Vertical axis**, select `response.keyword` with **Top values**.
4. For the **Cell value**, select **Count** to show the number of requests.

This heat map reveals which regions experience more errors or specific response patterns.

### Highlight anomalies with color ranges [anomaly-colors]

Configure color ranges to emphasize unusual values.

1. Create a **Heat map** chart with your dimensions configured.
2. Select {icon}`brush` **Style**.
3. In **Color**, enable **Custom ranges**.
4. Define ranges that highlight normal versus anomalous values:
   - Normal range: Neutral colors (blues, grays)
   - Anomalous range: Attention-grabbing colors (red, orange)

This approach makes outliers immediately visible.

## Heat map chart settings [heat-map-chart-settings]

Customize your heat map chart to display exactly the information you need, formatted the way you want.

### Horizontal axis settings [horizontal-axis-settings]

The **Horizontal axis** dimension defines the columns of the heat map.

**Data**
:   The **Horizontal axis** dimension supports the following functions:

    - **Top values**: Create columns for the most common values in a field.
      - **Field**: Select the field to group by. You can add up to 4 fields to create multi-term columns. When multiple fields are selected, each column represents a unique combination of values across those fields. You can reorder the fields by dragging them to change their priority.
      - **Number of values**: How many top values to display.
      :::{include} ../../_snippets/lens-rank-by-options.md
      :::
      :::{include} ../../_snippets/lens-breakdown-advanced-settings.md
      :::
    - **Date histogram**: Group data into time-based buckets.
      - **Field**: Select the date field to use for the time-based grouping.
      :::{include} ../../_snippets/lens-histogram-settings.md
      :::
    - **Intervals**: Create numeric ranges for continuous data.
      - **Field**: Select the numeric field to create intervals from.
      - **Include empty rows**: Include intervals with no matching documents.

**Appearance**
:   - **Name**: Customize the axis label.

### Vertical axis settings [vertical-axis-settings]

The **Vertical axis** dimension defines the rows of the heat map.

**Data**
:   The **Vertical axis** dimension supports the same functions as the horizontal axis:

    - **Top values**: Create rows for the most common values in a field.
      - **Field**: Select the field to group by. You can add up to 4 fields to create multi-term rows. When multiple fields are selected, each row represents a unique combination of values across those fields. You can reorder the fields by dragging them to change their priority.
      - **Number of values**: How many top values to display.
      :::{include} ../../_snippets/lens-rank-by-options.md
      :::
      :::{include} ../../_snippets/lens-breakdown-advanced-settings.md
      :::
    - **Date histogram**: Group data into time-based buckets.
      - **Field**: Select the date field to use for the time-based grouping.
      :::{include} ../../_snippets/lens-histogram-settings.md
      :::
    - **Intervals**: Create numeric ranges for continuous data.
      - **Field**: Select the numeric field to create intervals from.
      - **Include empty rows**: Include intervals with no matching documents.

**Appearance**
:   - **Name**: Customize the axis label.

### Cell value settings [cell-value-settings]

The **Cell value** dimension defines the metric that determines cell colors.

**Data**
:   The value that determines cell color intensity. When you drag a field onto the chart, {{kib}} suggests a function based on the field type. You can use aggregation functions like `Sum`, `Average`, `Count`, `Median`, and more, or create custom calculations with formulas. Refer to [](/explore-analyze/visualize/lens.md#lens-formulas) for examples.

    :::{include} ../../_snippets/lens-value-advanced-settings.md
    :::

**Appearance**
:   - **Name**: Customize the metric label displayed in tooltips.
    - **Value format**: Control how numeric values are displayed (number, percent, bytes, and more).

### General layout [appearance-options]

When creating or editing a visualization, you can customize several appearance options from the {icon}`brush` **Style** or ![Legend icon](/explore-analyze/images/kibana-legend-icon.svg "") **Legend** menus.

#### Style settings

**Color**

**Palette**
:   Choose a color palette for the heat map:
    - **Sequential**: Colors range from light to dark, suitable for data ranging from low to high.
    - **Diverging**: Colors diverge from a neutral midpoint, suitable for data with positive and negative values.

**Custom ranges**
:   Enable custom color ranges to define specific value-to-color mappings.

**Reverse**
:   Reverse the color palette direction.

**Titles and text**

**Show labels**
:   Display the cell value as text inside each cell.

#### Legend settings

**Visibility**
:   Specify whether to automatically show the legend or hide it:
    - **Auto**: Show the legend when useful (default).
    - **Show**: Always show the legend.
    - **Hide**: Never show the legend.

**Position**
:   Set the legend position: **Top**, **Left**, **Right**, or **Bottom**.

## Heat map chart examples

The following examples show various configuration options for building impactful heat map charts.

**Request volume by hour and day**
:   Visualize when your website receives the most traffic:

    * Example based on: {{kib}} Sample Data Logs
    * **Horizontal axis**: `@timestamp` (Date histogram, hourly)
    * **Vertical axis**: `@timestamp` (Date histogram, daily)
    * **Cell value**: Count
    * **Color palette**: Blues (sequential)

**Error rates by endpoint and status code**
:   Identify which endpoints have the most errors:

    * Example based on: {{kib}} Sample Data Logs
    * **Horizontal axis**: `request.keyword` (Top 10 values)
    * **Vertical axis**: `response.keyword` (Top values)
    * **Cell value**: Count
    * **Color palette**: Reds (sequential, reversed for higher = darker)

**Sales performance by product and region**
:   Compare product sales across geographic regions:

    * Example based on: {{kib}} Sample Data eCommerce
    * **Horizontal axis**: `geoip.city_name` (Top 10 values)
    * **Vertical axis**: `category.keyword` (Top values)
    * **Cell value**: `Sum(taxful_total_price)`
    * **Color palette**: Greens (sequential)
