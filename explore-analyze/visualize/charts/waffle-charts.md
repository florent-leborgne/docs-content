---
navigation_title: Waffle charts
applies_to:
  stack: ga
  serverless: ga
description: Instructions and best practices for building waffle charts with Kibana Lens in Elastic.
---

# Build waffle charts with {{kib}}

Waffle charts display data as a grid of small squares, where each square represents a portion of the whole. They are ideal for showing percentages, visualizing survey results, and making proportions intuitive by representing data as discrete units. They work best with fewer than 10 categories.

You can create waffle charts in {{kib}} using [**Lens**](../lens.md).

<!-- TODO: Add screenshot
![Example Lens waffle chart showing browser market share](/explore-analyze/images/waffle-chart-example.png)
-->

## Build a waffle chart

:::{include} ../../_snippets/lens-prerequisites.md
:::

To build a waffle chart:

::::::{stepper}

:::::{step} Access Lens
**Lens** is {{kib}}'s main visualization editor. You can access it:
- From a dashboard: On the **Dashboards** page, open or create the dashboard where you want to add a waffle chart, then add a new visualization.
- From the **Visualize library** page by creating a new visualization.
:::::

:::::{step} Set the visualization to Waffle
New visualizations often start as **Bar** charts.

Using the **Visualization type** dropdown, select **Waffle**.
:::::

:::::{step} Define the data to show
1. Select the {{data-source}} that contains your data.
2. Configure the [**Group by**](#group-by-settings) dimension to define the categories. Each category is displayed as a colored section of the waffle.
3. Configure the [**Metric**](#metric-settings) dimension to define the value for each category. This determines how many squares each category occupies.

Optionally:
   - Enable [**Multiple metrics**](#multiple-metrics) in the layer settings to define each category as a separate metric.

The chart preview updates to show a grid of colored squares. Each color represents a category, and the number of squares reflects its proportion of the total.
:::::

:::::{step} Customize the chart to follow best practices
Tweak the appearance of the chart to your needs. Consider the following best practices:

**Limit categories**
:   Keep your waffle chart to a maximum of 6-8 categories. More categories make the chart difficult to read.

**Use intuitive colors**
:   Assign colors that have semantic meaning when possible (for example, green for success, red for errors). Use the [color mapping feature](../lens.md#assign-colors-to-terms) for consistent coloring.

**Consider the grid size**
:   A 10x10 grid (100 squares) works well for percentages. Each square naturally represents 1%.

**Order categories meaningfully**
:   Arrange categories from largest to smallest or in a natural order (such as satisfaction ratings from low to high).

Refer to [Waffle chart settings](#waffle-chart-settings) to find all configuration options for your waffle chart.
:::::

:::::{step} Save the chart
- If you accessed Lens from a dashboard, select **Save and return** to save the visualization and add it to that dashboard, or select **Save to library** to add the visualization to the Visualize library and reuse it later.
- If you accessed Lens from the Visualize library, select **Save**. A menu opens and offers you to add the visualization to a dashboard and to the Visualize library.
:::::

::::::

## Advanced waffle chart scenarios

### Show percentage completion [percentage-completion]

Use a waffle chart to visualize progress toward a goal as a percentage.

1. Create a **Waffle** chart and remove any existing **Group by** dimension.
2. Open **Layer settings**:
   * {applies_to}`serverless: ga` {applies_to}`stack: ga 9.3` Select {icon}`app_management` **Layer settings**.
   * {applies_to}`stack: ga 9.0-9.2` Select {icon}`boxes_horizontal`, then select **Layer settings**.
3. Select **Multiple metrics**, then close the settings.
4. Add two metrics:
   - **Completed**: A formula or aggregation representing completed items
   - **Remaining**: A formula representing remaining items (for example, `goal - completed`)
5. Assign distinct colors (for example, green for completed, gray for remaining).

The waffle shows filled squares for completed work and empty (gray) squares for remaining work.

<!-- TODO: Add screenshot
![Waffle chart showing percentage completion with two metrics](/explore-analyze/images/waffle-scenario-completion.png "=70%")
-->

### Compare survey responses [survey-responses]

Waffle charts are excellent for visualizing Likert scale responses or categorical survey data.

#### Example: Customer satisfaction breakdown

1. Create a **Waffle** chart using your survey data.
2. For the **Group by** dimension, select your satisfaction rating field with **Top values**.
3. Set the **Metric** to **Count**.
4. Assign colors that match the sentiment:
   - Very satisfied: Dark green
   - Satisfied: Light green
   - Neutral: Gray
   - Dissatisfied: Light red
   - Very dissatisfied: Dark red

The resulting waffle shows each response category with intuitive coloring.

<!-- TODO: Add screenshot
![Waffle chart showing customer satisfaction breakdown](/explore-analyze/images/waffle-scenario-survey.png "=70%")
-->

### Group smaller values as "Other" [other-category]

When you have many small categories, group them to keep the visualization readable.

1. In the **Group by** configuration, select the field.
2. Use **Top values** to limit the number of categories displayed.
3. Expand **Advanced**.
4. Enable **Group other values as "Other"** to combine remaining values.

## Waffle chart settings [waffle-chart-settings]

Customize your waffle chart to display exactly the information you need, formatted the way you want.

### Group by settings [group-by-settings]

The **Group by** dimension defines how the waffle is divided into colored sections. Waffle charts support a single **Group by** dimension.

**Data**
:   The **Group by** dimension supports the following functions:

    - **Top values**: Create sections for the most common values in a field.
      - **Field**: Select the field to group by. You can add up to 4 fields to create multi-term sections. When multiple fields are selected, each section represents a unique combination of values across those fields. You can reorder the fields by dragging them to change their priority.
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
    - **Filters**: Define custom KQL filters to create specific sections.

**Appearance**
:   - **Name**: Customize the legend label.
    - **Color mapping**: Select a color palette or assign specific colors to categories. Refer to [Assign colors to terms](../lens.md#assign-colors-to-terms) for details.

### Metric settings [metric-settings]

The **Metric** dimension defines the value for each category, determining how many squares each section occupies.

**Data**
:   The value that determines how many squares each category fills. When you drag a field onto the chart, {{kib}} suggests a function based on the field type. You can use aggregation functions like `Sum`, `Average`, `Count`, `Median`, and more, or create custom calculations with [formulas](/explore-analyze/visualize/lens.md#lens-formulas).

    :::{include} ../../_snippets/lens-value-advanced-settings.md
    :::

**Appearance**
:   - **Name**: Customize the metric label displayed in tooltips and legends.
    - **Value format**: Control how numeric values are displayed (number, percent, bytes, and more).
    - **Series color**: When using multiple metrics without a **Group by** dimension, assign a specific color to each metric.

### Multiple metrics [multiple-metrics]

Enable **Multiple metrics** in the layer settings to define each waffle section as a separate metric rather than using a categorical field.

1. Open **Layer settings**:
   * {applies_to}`serverless: ga` {applies_to}`stack: ga 9.3` Select {icon}`app_management` **Layer settings**.
   * {applies_to}`stack: ga 9.0-9.2` Select {icon}`boxes_horizontal`, then select **Layer settings**.
2. Select **Multiple metrics**, then close the settings.
3. Add multiple **Metric** dimensions, each representing a section of the waffle.

### General layout [appearance-options]

When creating or editing a visualization, you can customize the legend from the ![Legend icon](/explore-analyze/images/kibana-legend-icon.svg "") **Legend** menu.

:::{note}
Waffle charts do not have configurable style settings. The chart automatically displays labels and percentages on each section.
:::

#### Legend settings

**Visibility**
:   Specify whether to automatically show the legend or hide it:
    - **Auto**: Show the legend when there are multiple categories.
    - **Show**: Always show the legend (default).
    - **Hide**: Never show the legend.

**Position**
:   Set the legend position: **Right** (default), **Left**, **Top**, or **Bottom**.

**Statistics**
:   Show the **Value** statistic in the legend to display the numeric value alongside each legend entry. This is enabled by default.

**Truncate**
:   Toggle whether to truncate long legend labels, and set a maximum number of lines (default: 1).

**Legend size**
:   Control the size of the legend panel: **Auto** (default), **Small**, **Medium**, **Large**, or **Extra large**.

## Waffle chart examples

The following examples show various configuration options for building impactful waffle charts.

**Browser market share**
:   Visualize the distribution of browsers used by your website visitors:

    * Example based on: {{kib}} Sample Data Logs
    * **Group by**: `machine.os.keyword` (Top 5 values)
    * **Metric**: Count
    * **Color mapping**: Distinct colors for each browser

<!-- TODO: Add screenshot
![Waffle chart showing browser market share](/explore-analyze/images/waffle-example-browser.png "=70%")
-->

**Order status distribution**
:   Show how orders are distributed across status categories:

    * Example based on: {{kib}} Sample Data eCommerce
    * **Group by**: `customer_gender` (Top values)
    * **Metric**: Count
    * **Legend**: Show with values

<!-- TODO: Add screenshot
![Waffle chart showing order status distribution](/explore-analyze/images/waffle-example-orders.png "=70%")
-->

**Project completion progress**
:   Display progress toward a project milestone:

    * Configuration: [**Multiple metrics**](#multiple-metrics)
    * **Metrics**:
      - Tasks completed: `Count(kql='status: completed')`
      - Tasks remaining: `Count(kql='status: pending OR status: in_progress')`
    * **Colors**: Green for completed, gray for remaining

<!-- TODO: Add screenshot
![Waffle chart showing project completion progress](/explore-analyze/images/waffle-example-progress.png "=70%")
-->
