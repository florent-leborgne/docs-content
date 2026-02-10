---
navigation_title: Treemap charts
applies_to:
  stack: ga
  serverless: ga
description: Instructions and best practices for building treemap charts with Kibana Lens in Elastic.
---

# Build treemap charts with {{kib}}

Treemap charts display hierarchical data as nested rectangles, where each rectangle's size represents a quantitative value. They are ideal for showing part-to-whole relationships within hierarchies, comparing relative sizes of categories, and visualizing how data is distributed across multiple levels.

You can create treemap charts in {{kib}} using [**Lens**](../lens.md).

<!-- TODO: Add screenshot
![Example Lens treemap chart showing product sales by category](/explore-analyze/images/treemap-chart-example.png)
-->

## When to use treemap charts

Treemap charts work best when:

* You have **hierarchical data** with multiple levels of categories
* You want to show **part-to-whole relationships** within a hierarchy
* You need to compare **relative sizes** of many categories at once
* You want to visualize **nested categorization** (for example, region > country > city)

Consider using [pie charts](pie-charts.md) instead when:

* You have a **single level** of categories (no hierarchy)
* You have **fewer than 6 categories**
* Precise percentage comparison is important

Consider using [bar charts](bar-charts.md) instead when:

* You need **precise value comparisons**
* Your categories don't have a natural hierarchy
* You want to show **trends over time**

## Build a treemap chart

To build a treemap chart:

::::::{stepper}

:::::{step} Access Lens
**Lens** is {{kib}}'s main visualization editor. You can access it:
- From a dashboard: On the **Dashboards** page, open or create the dashboard where you want to add a treemap chart, then add a new visualization.
- From the **Visualize library** page by creating a new visualization.
:::::

:::::{step} Set the visualization to Treemap
New visualizations often start as **Bar** charts.

Using the **Visualization type** dropdown, select **Treemap**.
:::::

:::::{step} Define the data to show
1. Select the {{data-source}} that contains your data.
2. Configure the [**Group by**](#group-by-settings) dimension to define how the rectangles are grouped. Add multiple **Group by** dimensions to create a hierarchy.
3. Configure the [**Metric**](#metric-settings) dimension to define the size of each rectangle.

Optionally:
   - Add additional **Group by** dimensions to create nested hierarchical levels.
:::::

:::::{step} Customize the chart to follow best practices
Tweak the appearance of the chart to your needs. Consider the following best practices:

**Limit hierarchy depth**
:   Keep your treemap to 2-3 levels of hierarchy. Deeper nesting becomes difficult to read and interpret.

**Order categories by size**
:   Arrange rectangles by size (largest first) to make comparisons easier. This is the default behavior in Lens.

**Use color meaningfully**
:   Apply colors to distinguish top-level categories or to represent an additional metric (such as growth rate or status).

**Provide clear labels**
:   Ensure labels are visible and readable. For small rectangles, consider showing only top-level labels or using tooltips.

Refer to [Treemap chart settings](#treemap-chart-settings) to find all configuration options for your treemap chart.
:::::

:::::{step} Save the chart
- If you accessed Lens from a dashboard, select **Save and return** to save the visualization and add it to that dashboard, or select **Save to library** to add the visualization to the Visualize library and reuse it later.
- If you accessed Lens from the Visualize library, select **Save**. A menu opens and offers you to add the visualization to a dashboard and to the Visualize library.
:::::

::::::

## Advanced treemap chart scenarios

### Create a multi-level hierarchy [multi-level]

Treemaps excel at showing nested categorization across multiple levels.

#### Example: Sales by category and product

This example uses the [sample ecommerce data](/manage-data/ingest/sample-data.md) to visualize sales hierarchically.

1. Create a **Treemap** chart using the **{{kib}} Sample Data eCommerce** {{data-source}}.
2. Add a **Group by** dimension for `category.keyword` with **Top values** (top 6).
3. Add a second **Group by** dimension for `products.product_name.keyword` with **Top values** (top 5).
4. Set the **Metric** to `Sum(taxful_total_price)`.

The resulting treemap shows product categories as large rectangles, with individual products nested within each category.

### Group small values into "Other" [other-category]

When you have many small categories, group them to keep the visualization readable.

1. In the **Group by** configuration, select the field.
2. Use **Top values** to limit the number of rectangles displayed.
3. Expand **Advanced**.
4. Enable **Group other values as "Other"** to combine remaining values into a single rectangle.

:::{tip}
Be careful when using "Other" as it could end up being the largest category. If "Other" dominates, consider increasing the number of top values or using a different visualization.
:::

### Compare proportions across time [time-comparison]

Create multiple treemaps to compare how proportions change over different time periods.

1. Create a **Treemap** chart with your hierarchy configured.
2. Add a **Breakdown** dimension using a date field with **Date histogram**.
3. Set the interval to match your comparison needs (daily, weekly, monthly).

This creates a series of treemaps, one for each time period, allowing you to see how the distribution changes over time.

## Treemap chart settings [treemap-chart-settings]

Customize your treemap chart to display exactly the information you need, formatted the way you want.

### Group by settings [group-by-settings]

The **Group by** dimension defines how rectangles are grouped. You can add up to 3 levels of grouping to create hierarchical visualizations.

**Data**
:   The **Group by** dimension supports the following functions:

    - **Top values**: Create rectangles for the most common values in a field.
      - **Number of values**: How many top values to display.
      :::{include} ../../_snippets/lens-rank-by-options.md
      :::
      :::{include} ../../_snippets/lens-breakdown-advanced-settings.md
      :::
    - **Date histogram**: Group data into time-based buckets.
      :::{include} ../../_snippets/lens-histogram-settings.md
      :::
    - **Intervals**: Create numeric ranges for continuous data.
      - **Include empty rows**: Include intervals with no matching documents.
    - **Filters**: Define custom KQL filters to create specific groups.

**Appearance**
:   - **Name**: Customize the legend label.
    - **Color mapping**: Select a color palette or assign specific colors to categories. Refer to [Assign colors to terms](../lens.md#assign-colors-to-terms) for details.

### Metric settings [metric-settings]

The **Metric** dimension defines the size of each rectangle.

**Data**
:   The value that determines rectangle size. When you drag a field onto the chart, {{kib}} suggests a function based on the field type. You can use aggregation functions like `Sum`, `Average`, `Count`, `Median`, and more, or create custom calculations with formulas. Refer to [](/explore-analyze/visualize/lens.md#lens-formulas) for examples.

    :::{include} ../../_snippets/lens-value-advanced-settings.md
    :::

**Appearance**
:   - **Name**: Customize the metric label displayed in tooltips and legends.
    - **Value format**: Control how numeric values are displayed (number, percent, bytes, and more).

### General layout [appearance-options]

When creating or editing a visualization, you can customize several appearance options from the {icon}`brush` **Style** or ![Legend icon](/explore-analyze/images/kibana-legend-icon.svg "") **Legend** menus.

#### Style settings

**Titles and text**

**Labels**
:   Control how labels appear on rectangles:
    - **Show**: Display labels on all rectangles where space permits.
    - **Hide**: Do not display labels on rectangles.

**Values**
:   Control what values appear on rectangles:
    - **Hide**: Do not display values.
    - **Show**: Display the metric value on each rectangle.

#### Legend settings

**Visibility**
:   Specify whether to automatically show the legend or hide it:
    - **Auto**: Show the legend when there are multiple groups (default).
    - **Show**: Always show the legend.
    - **Hide**: Never show the legend.

**Position**
:   Set the legend position: **Top**, **Left**, **Right**, or **Bottom**.

**Nested**
:   When using multiple **Group by** dimensions, enable this option to show the legend in a hierarchical format.

**Label truncation**
:   Choose whether to truncate long legend labels, and set a limit for how many lines to display.

**Width**
:   Set the width of the legend.

## Treemap chart examples

The following examples show various configuration options for building impactful treemap charts.

**Product sales by category**
:   Visualize how sales are distributed across product categories:

    * Example based on: {{kib}} Sample Data eCommerce
    * **Group by**: `category.keyword` (Top 6 values)
    * **Metric**: `Sum(taxful_total_price)`

**Website traffic by country and city**
:   Show geographic distribution of website visitors:

    * Example based on: {{kib}} Sample Data Logs
    * **Group by** (Level 1): `geo.src` (Top 5 values)
    * **Group by** (Level 2): `geo.dest` (Top 3 values)
    * **Metric**: Count

**Disk usage by host and mount point**
:   Visualize storage consumption across your infrastructure:

    * Example based on: System metrics data
    * **Group by** (Level 1): `host.name` (Top values)
    * **Group by** (Level 2): `system.filesystem.mount_point` (Top values)
    * **Metric**: `Max(system.filesystem.used.bytes)`
