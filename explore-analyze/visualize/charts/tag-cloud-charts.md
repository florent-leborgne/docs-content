---
navigation_title: Tag cloud charts
applies_to:
  stack: ga
  serverless: ga
description: Instructions and best practices for building tag cloud charts with Kibana Lens in Elastic.
---

# Build tag cloud charts with {{kib}}

Tag cloud charts display text labels (tags) where each tag's size represents its frequency or importance. They are ideal for visualizing word frequency, showing popular categories, and providing an at-a-glance summary of text-based data.

You can create tag cloud charts in {{kib}} using [**Lens**](../lens.md).

<!-- TODO: Add screenshot
![Example Lens tag cloud chart showing popular search terms](/explore-analyze/images/tag-cloud-chart-example.png)
-->

## When to use tag cloud charts

Tag cloud charts work best when:

* You want to show **relative frequency** of terms or categories
* You need a quick **visual summary** of text-based data
* You're displaying **popular items**, such as tags, keywords, or categories
* The exact values are less important than the **relative prominence**

Consider using [bar charts](bar-charts.md) instead when:

* You need **precise value comparisons**
* You have **more than 50 items** to display
* You need to show **trends over time**
* The **order** of items matters

Consider using [tables](tables.md) instead when:

* You need to show **exact counts** alongside terms
* You want to **sort or filter** the data interactively
* You have **many columns** of information to display

## Build a tag cloud chart

:::{include} ../../_snippets/lens-prerequisites.md
:::

To build a tag cloud chart:

::::::{stepper}

:::::{step} Access Lens
**Lens** is {{kib}}'s main visualization editor. You can access it:
- From a dashboard: On the **Dashboards** page, open or create the dashboard where you want to add a tag cloud chart, then add a new visualization.
- From the **Visualize library** page by creating a new visualization.
:::::

:::::{step} Set the visualization to Tag cloud
New visualizations often start as **Bar** charts.

Using the **Visualization type** dropdown, select **Tag cloud**.
:::::

:::::{step} Define the data to show
1. Select the {{data-source}} that contains your data.
2. Configure the [**Tags**](#tags-settings) dimension to define which field provides the text labels.
3. Configure the [**Metric**](#metric-settings) dimension to define the value that determines each tag's size.

The chart preview updates to show text labels sized by metric value, with more prominent tags representing higher values.
:::::

:::::{step} Customize the chart to follow best practices
Tweak the appearance of the chart to your needs. Consider the following best practices:

**Limit the number of tags**
:   Keep your tag cloud to 20-50 tags maximum. Too many tags create visual clutter and make the most important terms hard to identify.

**Use meaningful metrics**
:   Choose a metric that represents importance or frequency. Count is common, but Sum, Average, or custom formulas can provide different insights.

**Consider orientation**
:   Multiple orientations (horizontal and angled) create visual interest but can make reading harder. Use single orientation for clarity.

**Choose appropriate colors**
:   Use colors to add meaning (categories) or keep them neutral to focus attention on size differences.

Refer to [Tag cloud chart settings](#tag-cloud-chart-settings) to find all configuration options for your tag cloud chart.
:::::

:::::{step} Save the chart
- If you accessed Lens from a dashboard, select **Save and return** to save the visualization and add it to that dashboard, or select **Save to library** to add the visualization to the Visualize library and reuse it later.
- If you accessed Lens from the Visualize library, select **Save**. A menu opens and offers you to add the visualization to a dashboard and to the Visualize library.
:::::

::::::

## Advanced tag cloud chart scenarios

### Visualize popular content [popular-content]

Use tag clouds to show the most frequently accessed or requested content.

#### Example: Popular URLs

This example uses the [sample web logs data](/manage-data/ingest/sample-data.md) to visualize the most requested pages.

1. Create a **Tag cloud** chart using the **{{kib}} Sample Data Logs** {{data-source}}.
2. For the **Tags** dimension, select `request.keyword` with **Top values** (top 30).
3. For the **Metric**, select **Count** to show the number of requests for each URL.
4. Select {icon}`brush` **Style** and adjust the font size range for better readability.

The resulting tag cloud shows the most popular pages, with more frequently accessed pages appearing larger.

### Show keyword frequency in logs [log-keywords]

Tag clouds can highlight frequently occurring terms in log messages or error text.

#### Example: Common error messages

1. Create a **Tag cloud** chart using your log {{data-source}}.
2. For the **Tags** dimension, select your error message or keyword field with **Top values**.
3. For the **Metric**, select **Count**.
4. Filter to show only error-level messages using the query bar.

### Use custom colors for categories [category-colors]

Apply colors to make tag clouds more informative.

1. Create a **Tag cloud** chart with your tags configured.
2. In the **Tags** dimension configuration, select **Color mapping**.
3. Assign specific colors to important categories using the [color mapping feature](../lens.md#assign-colors-to-terms).

## Tag cloud chart settings [tag-cloud-chart-settings]

Customize your tag cloud chart to display exactly the information you need, formatted the way you want.

### Tags settings [tags-settings]

The **Tags** dimension defines the text labels that appear in the cloud.

**Data**
:   The **Tags** dimension supports the following functions:

    - **Top values**: Display the most common values in a field.
      - **Field**: Select the field to group by. You can add up to 4 fields to create multi-term tags. When multiple fields are selected, each tag represents a unique combination of values across those fields. You can reorder the fields by dragging them to change their priority.
      - **Number of values**: How many tags to display (recommended: 20-50).
      :::{include} ../../_snippets/lens-rank-by-options.md
      :::
      :::{include} ../../_snippets/lens-breakdown-advanced-settings.md
      :::

**Appearance**
:   - **Name**: Customize the label shown in the visualization title.
    - **Color mapping**: Select a color palette or assign specific colors to tags. Refer to [Assign colors to terms](../lens.md#assign-colors-to-terms) for details.

### Metric settings [metric-settings]

The **Metric** dimension defines the value that determines each tag's size.

**Data**
:   The value that determines tag size. When you drag a field onto the chart, {{kib}} suggests a function based on the field type. You can use aggregation functions like `Sum`, `Average`, `Count`, `Median`, and more, or create custom calculations with formulas. Refer to [](/explore-analyze/visualize/lens.md#lens-formulas) for examples.

    :::{include} ../../_snippets/lens-value-advanced-settings.md
    :::

**Appearance**
:   - **Name**: Customize the metric label.
    - **Value format**: Control how numeric values are displayed in tooltips.

### General layout [appearance-options]

When creating or editing a visualization, you can customize several appearance options from the {icon}`brush` **Style** menu.

#### Style settings

**Font size**
:   Define the range of font sizes used in the tag cloud:
    - **Minimum**: The smallest font size for low-frequency tags.
    - **Maximum**: The largest font size for high-frequency tags.

**Orientation**
:   Define the orientation of the tags:
    - **Single**: All tags are horizontal.
    - **Right angled**: Tags are either horizontal or vertical.
    - **Multiple**: Tags appear at various angles.

**Show label**
:   Display a label for the tag cloud. The label text is defined by the **Name** field in the Tags dimension.

## Tag cloud chart examples

The following examples show various configuration options for building impactful tag cloud charts.

**Popular request URLs**
:   Visualize the most frequently requested pages on your website:

    * Example based on: {{kib}} Sample Data Logs
    * **Tags**: `request.keyword` (Top 30 values)
    * **Metric**: Count
    * **Orientation**: Single (horizontal)

**Top product categories**
:   Show which product categories are most popular:

    * Example based on: {{kib}} Sample Data eCommerce
    * **Tags**: `category.keyword` (Top 20 values)
    * **Metric**: Count
    * **Color mapping**: Distinct colors per category

**Frequent log sources**
:   Identify which systems generate the most log entries:

    * Example based on: System logs
    * **Tags**: `host.name` or `service.name` (Top 25 values)
    * **Metric**: Count
    * **Orientation**: Right angled for visual variety
