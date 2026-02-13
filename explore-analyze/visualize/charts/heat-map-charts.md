---
navigation_title: Heat map charts
applies_to:
  stack: ga
  serverless: ga
description: Instructions and best practices for building heat map charts with Kibana Lens in Elastic.
---

# Build heat map charts with {{kib}}

Heat map charts display data as a grid of colored cells, where each cell's color represents the magnitude of a value. They are ideal for visualizing patterns across two categorical or temporal dimensions, identifying correlations, and spotting anomalies in large datasets.

You can create heat map charts in {{kib}} using [**Lens**](../lens.md).

<!-- TODO: Add screenshot
![Example Lens heat map chart showing request counts by hour and day](/explore-analyze/images/heat-map-chart-example.png)
-->

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
3. Configure the [**Cell value**](#cell-value-settings) dimension to define the metric that determines cell colors.

Optionally:
   - Configure the [**Vertical axis**](#vertical-axis-settings) dimension to define the rows of the heat map. Without a vertical axis, the heat map displays a single row of colored cells.

The chart preview updates to show a grid of colored cells. Cell colors represent the magnitude of the metric value. If the grid appears empty, verify that the axes have data for the current time range.
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
5. Select the **Cell value** dimension and choose a sequential color palette.

The resulting heat map shows traffic intensity across hours and days, making it easy to identify peak periods.

<!-- TODO: Add screenshot
![Heat map showing website traffic intensity by hour and day of week](/explore-analyze/images/heat-map-scenario-time-patterns.png "=70%")
-->

### Compare categories with a heat map [category-comparison]

Use heat maps to compare metrics across two categorical dimensions.

#### Example: Response codes by geographic region

1. Create a **Heat map** chart using your web logs {{data-source}}.
2. For the **Horizontal axis**, select `geo.src` with **Top values** (top 10 countries).
3. For the **Vertical axis**, select `response.keyword` with **Top values**.
4. For the **Cell value**, select **Count** to show the number of requests.

This heat map reveals which regions experience more errors or specific response patterns.

<!-- TODO: Add screenshot
![Heat map showing response codes by geographic region](/explore-analyze/images/heat-map-scenario-category-comparison.png "=70%")
-->

### Highlight anomalies with color ranges [anomaly-colors]

Configure color ranges to emphasize unusual values.

1. Create a **Heat map** chart with your dimensions configured.
2. Select the **Cell value** dimension to open its settings.
3. In the **Color palette** configuration, define custom color ranges that highlight normal versus anomalous values:
   - Normal range: Neutral colors (blues, grays)
   - Anomalous range: Attention-grabbing colors (red, orange)

This approach makes outliers immediately visible.

<!-- TODO: Add screenshot
![Heat map with custom color ranges highlighting anomalous values](/explore-analyze/images/heat-map-scenario-anomaly-colors.png "=70%")
-->

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
:   The value that determines cell color intensity. When you drag a field onto the chart, {{kib}} suggests a function based on the field type. You can use aggregation functions like `Sum`, `Average`, `Count`, `Median`, and more, or create custom calculations with [formulas](/explore-analyze/visualize/lens.md#lens-formulas).

    :::{include} ../../_snippets/lens-value-advanced-settings.md
    :::

**Appearance**
:   - **Name**: Customize the metric label displayed in tooltips.
    - **Value format**: Control how numeric values are displayed (number, percent, bytes, and more).
    - **Color palette**: Configure the color palette that maps cell values to colors. The default palette is **Temperature**. You can select a different palette, reverse the color direction, and define custom color ranges with specific value-to-color mappings.

### General layout [appearance-options]

When creating or editing a visualization, you can customize several appearance options from the {icon}`brush` **Style** or ![Legend icon](/explore-analyze/images/kibana-legend-icon.svg "") **Legend** menus.

#### Style settings

**Titles and text**

**Value labels**
:   Control whether cell values are displayed as text inside each cell:
    - **Hide**: Do not display values in cells (default).
    - **Show**: Display the metric value inside each cell.

**Vertical axis**

**Axis title**
:   Show or hide the vertical axis title. When visible, you can enter a custom title or use the default field name.

**Axis labels**
:   Show or hide the labels on the vertical axis.

**Horizontal axis**

**Axis title**
:   Show or hide the horizontal axis title. When visible, you can enter a custom title or use the default field name.

**Axis labels**
:   Show or hide the labels on the horizontal axis.

**Orientation**
:   Control the orientation of horizontal axis labels:
    - **Horizontal**: Labels are displayed horizontally (default).
    - **Vertical**: Labels are rotated 90 degrees.
    - **Angled**: Labels are displayed at a 45-degree angle.

#### Legend settings

**Visibility**
:   Show or hide the legend:
    - **Show**: Display the legend (default).
    - **Hide**: Do not display the legend.

**Position**
:   Set the legend position: **Right** (default), **Left**, **Top**, or **Bottom**.

**Max lines**
:   Set the maximum number of lines for legend labels (1-5). Defaults to 1.

**Truncate**
:   Toggle whether to truncate long legend labels.

**Legend size**
:   Control the size of the legend panel: **Auto** (default), **Small**, **Medium**, **Large**, or **Extra large**.

## Heat map chart examples

The following examples show various configuration options for building impactful heat map charts.

**Request volume by hour and day**
:   Visualize when your website receives the most traffic:

    * Example based on: {{kib}} Sample Data Logs
    * **Horizontal axis**: `@timestamp` (Date histogram, hourly)
    * **Vertical axis**: `@timestamp` (Date histogram, daily)
    * **Cell value**: Count
    * **Color palette**: Blues (sequential)

<!-- TODO: Add screenshot
![Heat map showing request volume by hour and day](/explore-analyze/images/heat-map-example-request-volume.png "=70%")
-->

**Error rates by endpoint and status code**
:   Identify which endpoints have the most errors:

    * Example based on: {{kib}} Sample Data Logs
    * **Horizontal axis**: `request.keyword` (Top 10 values)
    * **Vertical axis**: `response.keyword` (Top values)
    * **Cell value**: Count
    * **Color palette**: Reds (sequential, reversed for higher = darker)

<!-- TODO: Add screenshot
![Heat map showing error rates by endpoint and status code](/explore-analyze/images/heat-map-example-error-rates.png "=70%")
-->

**Sales performance by product and region**
:   Compare product sales across geographic regions:

    * Example based on: {{kib}} Sample Data eCommerce
    * **Horizontal axis**: `geoip.city_name` (Top 10 values)
    * **Vertical axis**: `category.keyword` (Top values)
    * **Cell value**: `Sum(taxful_total_price)`
    * **Color palette**: Greens (sequential)

<!-- TODO: Add screenshot
![Heat map showing sales performance by product and region](/explore-analyze/images/heat-map-example-sales-performance.png "=70%")
-->
