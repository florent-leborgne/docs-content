---
navigation_title: Gauge charts
applies_to:
  stack: ga
  serverless: ga
description: Instructions and best practices for building gauge charts with Kibana Lens in Elastic.
---

# Build gauge charts with {{kib}}

Gauge charts display a single value within a defined range, showing how close the value is to a target or threshold. They are ideal for monitoring KPIs, tracking progress toward goals, and highlighting when values fall within acceptable, warning, or critical ranges.

You can create gauge charts in {{kib}} using [**Lens**](../lens.md).

<!-- TODO: Add screenshot
![Example Lens gauge chart showing CPU usage at 73%](/explore-analyze/images/gauge-chart-example.png)
-->

## When to use gauge charts

Gauge charts work best when:

* You need to display a **single metric** against a defined range
* You want to show **progress toward a goal** or target
* You need to highlight **threshold levels** (good, warning, critical)
* The value has a **known minimum and maximum**

Consider using [metric charts](metric-charts.md) instead when:

* You want to display the **raw value** without a range context
* You need to compare **multiple metrics** side by side
* The metric doesn't have meaningful min/max boundaries

Consider using [bar charts](bar-charts.md) instead when:

* You need to compare values across **multiple categories**
* You want to show **trends over time**

## Build a gauge chart

To build a gauge chart:

::::::{stepper}

:::::{step} Access Lens
**Lens** is {{kib}}'s main visualization editor. You can access it:
- From a dashboard: On the **Dashboards** page, open or create the dashboard where you want to add a gauge chart, then add a new visualization.
- From the **Visualize library** page by creating a new visualization.
:::::

:::::{step} Set the visualization to Gauge
New visualizations often start as **Bar** charts.

Using the **Visualization type** dropdown, select **Gauge**.
:::::

:::::{step} Define the data to show
1. Select the {{data-source}} that contains your data.
2. Configure the [**Metric**](#metric-settings) dimension to define the value displayed on the gauge.
3. Optionally, configure the [**Maximum**](#maximum-settings) dimension to set a dynamic upper bound based on your data.

The gauge automatically displays the metric value within the defined range.
:::::

:::::{step} Customize the chart to follow best practices
Tweak the appearance of the chart to your needs. Consider the following best practices:

**Set meaningful bounds**
:   Define the minimum and maximum values that make sense for your metric. A CPU usage gauge should range from 0 to 100, while a sales target might range from 0 to your quarterly goal.

**Use color bands for thresholds**
:   Configure color ranges to indicate performance levels. Use green for acceptable values, yellow for warning, and red for critical thresholds.

**Choose the right shape**
:   Select a gauge shape that fits your dashboard layout. Use **Arc** shapes for traditional gauge appearance, or **Linear** for a more compact horizontal or vertical display.

**Add context with titles**
:   Provide clear titles that explain what the gauge measures and what the target value represents.

Refer to [Gauge chart settings](#gauge-chart-settings) to find all configuration options for your gauge chart.
:::::

:::::{step} Save the chart
- If you accessed Lens from a dashboard, select **Save and return** to save the visualization and add it to that dashboard, or select **Save to library** to add the visualization to the Visualize library and reuse it later.
- If you accessed Lens from the Visualize library, select **Save**. A menu opens and offers you to add the visualization to a dashboard and to the Visualize library.
:::::

::::::

## Advanced gauge chart scenarios

### Create a goal-tracking gauge [goal-tracking]

Use a gauge to track progress toward a specific target, such as monthly sales goals or project completion percentage.

1. Create a **Gauge** chart and select your {{data-source}}.
2. Configure the **Metric** dimension with your progress value (for example, `Sum(sales_amount)`).
3. Select {icon}`brush` **Style**.
4. In **Appearance**, set:
   - **Minimum**: `0`
   - **Maximum**: Your target value (for example, `100000` for a $100K sales goal)
   - **Goal**: Your target value to display a goal marker
5. Configure color bands to show progress levels:
   - 0-50%: Red (behind schedule)
   - 50-80%: Yellow (on track)
   - 80-100%: Green (ahead of schedule)

### Configure color bands for thresholds [color-bands]

Color bands help users quickly understand whether a value is within acceptable ranges.

1. Create a **Gauge** chart with your metric configured.
2. Select {icon}`brush` **Style**.
3. In **Appearance**, select **Custom color bands**.
4. Add color ranges by specifying:
   - **From**: The starting value for this band
   - **To**: The ending value for this band
   - **Color**: The color to display for values in this range
5. Add multiple bands to create threshold indicators.

#### Example: Server health monitoring

This example creates a gauge showing server response time with color-coded health indicators.

| Band | Range | Color | Meaning |
|------|-------|-------|---------|
| Healthy | 0-200ms | Green | Normal response times |
| Warning | 200-500ms | Yellow | Elevated response times |
| Critical | 500ms+ | Red | Unacceptable performance |

### Use a dynamic maximum [dynamic-maximum]

Instead of setting a fixed maximum value, you can use a field from your data to set the maximum dynamically.

1. Create a **Gauge** chart with your metric configured.
2. Add a **Maximum** dimension.
3. Select a field and aggregation that represents the upper bound (for example, `Max(quota)` for a quota-based gauge).

This approach is useful when targets vary by category, time period, or user.

## Gauge chart settings [gauge-chart-settings]

Customize your gauge chart to display exactly the information you need, formatted the way you want.

### Metric settings [metric-settings]

The **Metric** dimension defines the value displayed on the gauge.

**Data**
:   The value that the gauge displays. When you drag a field onto the chart, {{kib}} suggests a function based on the field type. You can use aggregation functions like `Sum`, `Average`, `Count`, `Median`, `Last value`, and more, or create custom calculations with formulas. Refer to [](/explore-analyze/visualize/lens.md#lens-formulas) for examples.

    :::{include} ../../_snippets/lens-value-advanced-settings.md
    :::

**Appearance**
:   - **Name**: Customize the metric label displayed in the gauge.
    - **Value format**: Control how numeric values are displayed (number, percent, bytes, and more).
    - **Color**: Override the default color for the metric value.

### Maximum settings [maximum-settings]

The **Maximum** dimension optionally defines a dynamic upper bound for the gauge.

**Data**
:   A field and aggregation that sets the maximum value dynamically. Useful when the upper bound varies based on your data (for example, quotas, targets, or capacity limits).

    :::{include} ../../_snippets/lens-value-advanced-settings.md
    :::

**Appearance**
:   - **Name**: Customize the label for the maximum value.
    - **Value format**: Control how the maximum value is displayed.

### General layout [appearance-options]

When creating or editing a visualization, you can customize several appearance options from the {icon}`brush` **Style** menu.

#### Style settings

**Shape**
:   Choose the gauge shape:
    - **Minor arc**: A partial circle arc (default).
    - **Major arc**: A larger circular arc.
    - **Circle**: A full 360-degree gauge.
    - **Linear horizontal**: A horizontal bar gauge.
    - **Linear vertical**: A vertical bar gauge.

**Appearance**

**Minimum**
:   The minimum value for the gauge range. Defaults to `0`.

**Maximum**
:   The maximum value for the gauge range. Set this to your target or upper bound. Overridden if a **Maximum** dimension is configured.

**Goal**
:   An optional target value to display as a marker on the gauge.

**Color**

**Bands**
:   Configure color bands to indicate threshold levels:
    - **Auto**: Automatically assigns colors based on the palette.
    - **Custom**: Define specific color ranges with From, To, and Color values.

**Titles and text**

**Title**
:   Show or hide the metric title on the gauge.

**Subtitle**
:   Add a subtitle for additional context.

## Gauge chart examples

The following examples show various configuration options for building impactful gauge charts.

**CPU usage monitoring**
:   Monitor system CPU usage with threshold-based coloring:

    * Example based on: System metrics data
    * **Metric**: `Average(system.cpu.total.pct)` formatted as percent
    * **Shape**: Minor arc
    * **Minimum**: 0, **Maximum**: 100
    * **Color bands**: 0-70 (green), 70-90 (yellow), 90-100 (red)

**Sales progress toward goal**
:   Track monthly sales against a target:

    * Example based on: {{kib}} Sample Data eCommerce
    * **Metric**: `Sum(taxful_total_price)`
    * **Shape**: Major arc
    * **Minimum**: 0, **Maximum**: 50000
    * **Goal**: 50000
    * **Color bands**: Custom gradient from red to green

**Disk space utilization**
:   Display disk space usage as a percentage of capacity:

    * Example based on: System metrics data
    * **Metric**: Formula `sum(system.filesystem.used.bytes) / sum(system.filesystem.total.bytes) * 100`
    * **Shape**: Circle
    * **Minimum**: 0, **Maximum**: 100
    * **Color bands**: 0-60 (green), 60-80 (yellow), 80-100 (red)
