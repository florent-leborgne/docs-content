---
navigation_title: Mosaic charts
applies_to:
  stack: ga
  serverless: ga
description: Instructions and best practices for building mosaic charts with Kibana Lens in Elastic.
---

# Build mosaic charts with {{kib}}

Mosaic charts display the relationship between two categorical variables as a grid of rectangles, where both the width and height of each rectangle represent proportions of the data. They are ideal for visualizing how categories combine, showing conditional distributions, and exploring relationships between two dimensions. They work best with a moderate number of categories in each dimension (2-8 each).

You can create mosaic charts in {{kib}} using [**Lens**](../lens.md).

<!-- TODO: Add screenshot
![Example Lens mosaic chart showing order status by product category](/explore-analyze/images/mosaic-chart-example.png)
-->

## Build a mosaic chart

:::{include} ../../_snippets/lens-prerequisites.md
:::

To build a mosaic chart:

::::::{stepper}

:::::{step} Access Lens
**Lens** is {{kib}}'s main visualization editor. You can access it:
- From a dashboard: On the **Dashboards** page, open or create the dashboard where you want to add a mosaic chart, then add a new visualization.
- From the **Visualize library** page by creating a new visualization.
:::::

:::::{step} Set the visualization to Mosaic
New visualizations often start as **Bar** charts.

Using the **Visualization type** dropdown, select **Mosaic**.
:::::

:::::{step} Define the data to show
1. Select the {{data-source}} that contains your data.
2. Configure the [**Horizontal axis**](#horizontal-axis-settings) dimension to define the columns. The width of each column represents the proportion of data in that category.
3. Configure the [**Vertical axis**](#vertical-axis-settings) dimension to define the rows within each column. The height of each rectangle represents the proportion within that column.
4. The **Metric** is automatically set to **Count**. This determines the size of each rectangle.

The chart preview updates to show a grid of rectangles. Column widths represent the proportion of each horizontal category, and rectangle heights within each column show the distribution of vertical categories.
:::::

:::::{step} Customize the chart to follow best practices
Tweak the appearance of the chart to your needs. Consider the following best practices:

**Limit categories**
:   Keep both dimensions to a maximum of 6-8 categories each. More categories create tiny rectangles that are hard to read.

**Order categories meaningfully**
:   Arrange categories in a logical order (by size, alphabetically, or by a natural ordering) to make patterns easier to identify.

**Use color for the vertical dimension**
:   Colors typically represent the vertical axis categories, making it easier to track how each category appears across columns.

**Consider aspect ratio**
:   Wide mosaics work better for data with many horizontal categories. Square mosaics work better for balanced data.

Refer to [Mosaic chart settings](#mosaic-chart-settings) to find all configuration options for your mosaic chart.
:::::

:::::{step} Save the chart
- If you accessed Lens from a dashboard, select **Save and return** to save the visualization and add it to that dashboard, or select **Save to library** to add the visualization to the Visualize library and reuse it later.
- If you accessed Lens from the Visualize library, select **Save**. A menu opens and offers you to add the visualization to a dashboard and to the Visualize library.
:::::

::::::

## Advanced mosaic chart scenarios

### Explore category associations [category-associations]

Use mosaic charts to discover how categories from two different fields are associated.

#### Example: Browser usage by operating system

This example uses the [sample web logs data](/manage-data/ingest/sample-data.md) to visualize the relationship between browsers and operating systems.

1. Create a **Mosaic** chart using the **{{kib}} Sample Data Logs** {{data-source}}.
2. For the **Horizontal axis**, select `machine.os.keyword` with **Top values** (top 5).
3. For the **Vertical axis**, select `geo.src` with **Top values** (top 5 countries).

The resulting mosaic shows:
- Column widths representing the proportion of each operating system
- Rectangle heights within each column showing the distribution of countries for that OS

<!-- TODO: Add screenshot
![Mosaic chart showing OS usage by country](/explore-analyze/images/mosaic-scenario-category-associations.png "=70%")
-->

### Compare distributions across segments [segment-comparison]

Mosaic charts reveal whether distributions differ across segments.

#### Example: Product categories by customer gender

1. Create a **Mosaic** chart using the **{{kib}} Sample Data eCommerce** {{data-source}}.
2. For the **Horizontal axis**, select `customer_gender` with **Top values**.
3. For the **Vertical axis**, select `category.keyword` with **Top values** (top 6).

If the rectangle heights are similar across columns, the distribution is independent. If heights vary significantly, there's an association between gender and category preference.

<!-- TODO: Add screenshot
![Mosaic chart showing product categories by customer gender](/explore-analyze/images/mosaic-scenario-segment-comparison.png "=70%")
-->

### Group smaller categories [other-category]

When some categories are too small to display meaningfully, group them into an "Other" category.

1. In the dimension configuration, select the field.
2. Use **Top values** to limit the number of categories displayed.
3. Expand **Advanced**.
4. Enable **Group other values as "Other"** to combine remaining values.

## Mosaic chart settings [mosaic-chart-settings]

Customize your mosaic chart to display exactly the information you need, formatted the way you want.

### Horizontal axis settings [horizontal-axis-settings]

The **Horizontal axis** dimension defines the columns of the mosaic. Column widths represent the proportion of each category.

**Data**
:   The **Horizontal axis** dimension supports the following functions:

    - **Top values**: Create columns for the most common values in a field.
      - **Field**: Select the field to group by. You can add up to 4 fields to create multi-term columns. When multiple fields are selected, each column represents a unique combination of values across those fields. You can reorder the fields by dragging them to change their priority.
      - **Number of values**: How many categories to display.
      :::{include} ../../_snippets/lens-rank-by-options.md
      :::
      :::{include} ../../_snippets/lens-breakdown-advanced-settings.md
      :::
    - **Filters**: Define custom KQL filters to create specific columns.

**Appearance**
:   - **Name**: Customize the axis label.

### Vertical axis settings [vertical-axis-settings]

The **Vertical axis** dimension defines the rows within each column. Rectangle heights represent the proportion of each category within the column.

**Data**
:   The **Vertical axis** dimension supports the following functions:

    - **Top values**: Create rows for the most common values in a field.
      - **Field**: Select the field to group by. You can add up to 4 fields to create multi-term rows. When multiple fields are selected, each row represents a unique combination of values across those fields. You can reorder the fields by dragging them to change their priority.
      - **Number of values**: How many categories to display.
      :::{include} ../../_snippets/lens-rank-by-options.md
      :::
      :::{include} ../../_snippets/lens-breakdown-advanced-settings.md
      :::
    - **Filters**: Define custom KQL filters to create specific rows.

**Appearance**
:   - **Name**: Customize the axis label.
    - **Color mapping**: Select a color palette or assign specific colors to categories. Refer to [Assign colors to terms](../lens.md#assign-colors-to-terms) for details.

### Metric settings [metric-settings]

The **Metric** dimension defines the value used to calculate rectangle sizes. In mosaic charts, this is typically **Count**.

**Data**
:   The value that determines rectangle proportions. You can use aggregation functions like `Count`, `Sum`, or create custom calculations with [formulas](/explore-analyze/visualize/lens.md#lens-formulas).

    :::{include} ../../_snippets/lens-value-advanced-settings.md
    :::

:::{note}
Mosaic charts do not support multiple metrics. Each cell represents a single count or aggregated value.
:::

### General layout [appearance-options]

When creating or editing a visualization, you can customize several appearance options from the {icon}`brush` **Style** or ![Legend icon](/explore-analyze/images/kibana-legend-icon.svg "") **Legend** menus.

#### Style settings

**Titles and text**

**Values**
:   Control what values appear on rectangles:
    - **Percentage**: Display the percentage of total (default).
    - **Integer**: Display the raw numeric value.
    - **Hide**: Do not display values.

    When displaying percentages, you can also configure the **Decimal places** (default: 2).

#### Legend settings

**Visibility**
:   Specify whether to automatically show the legend or hide it:
    - **Auto**: Show the legend when there are multiple categories (default).
    - **Show**: Always show the legend.
    - **Hide**: Never show the legend.

**Position**
:   Set the legend position: **Right** (default), **Left**, **Top**, or **Bottom**.

**Nested**
:   When using both axes, enable this option to show the legend in a hierarchical format.

**Statistics**
:   Show the **Value** statistic in the legend to display the numeric value alongside each legend entry.

**Truncate**
:   Toggle whether to truncate long legend labels, and set a maximum number of lines (default: 1).

**Legend size**
:   Control the size of the legend panel: **Auto** (default), **Small**, **Medium**, **Large**, or **Extra large**.

## Mosaic chart examples

The following examples show various configuration options for building impactful mosaic charts.

**Operating system by country**
:   Visualize how operating system usage varies by geographic region:

    * Example based on: {{kib}} Sample Data Logs
    * **Horizontal axis**: `machine.os.keyword` (Top 5 values)
    * **Vertical axis**: `geo.src` (Top 5 values)
    * **Metric**: Count

<!-- TODO: Add screenshot
![Mosaic chart showing OS by country](/explore-analyze/images/mosaic-example-os-by-country.png "=70%")
-->

**Product category by customer segment**
:   Show purchasing patterns across customer segments:

    * Example based on: {{kib}} Sample Data eCommerce
    * **Horizontal axis**: `customer_gender` (Top values)
    * **Vertical axis**: `category.keyword` (Top 5 values)
    * **Metric**: Count

<!-- TODO: Add screenshot
![Mosaic chart showing product categories by gender](/explore-analyze/images/mosaic-example-category-by-gender.png "=70%")
-->

**Response codes by request type**
:   Analyze how different request types result in different response codes:

    * Example based on: {{kib}} Sample Data Logs
    * **Horizontal axis**: `request.keyword` (Top 5 values)
    * **Vertical axis**: `response.keyword` (Top values)
    * **Metric**: Count

<!-- TODO: Add screenshot
![Mosaic chart showing response codes by request type](/explore-analyze/images/mosaic-example-response-by-request.png "=70%")
-->
