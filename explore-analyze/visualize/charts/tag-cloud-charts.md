---
navigation_title: Tag cloud charts
applies_to:
  stack: ga
  serverless: ga
description: Instructions and best practices for building tag cloud charts with Kibana Lens in Elastic.
---

# Build tag cloud charts with {{kib}}

Tag cloud charts display text labels (tags) where each tag's size represents its frequency or importance. They are ideal for visualizing word frequency, showing popular categories, and providing an at-a-glance summary of text-based data. They work best when the relative prominence of terms matters more than exact values, and are most effective with up to about 50 items.

You can create tag cloud charts in {{kib}} using [**Lens**](../lens.md).

<!-- TODO: Add screenshot
![Example Lens tag cloud chart showing popular search terms](/explore-analyze/images/tag-cloud-chart-example.png)
-->

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
:   The value that determines tag size. When you drag a field onto the chart, {{kib}} suggests a function based on the field type. You can use aggregation functions like `Sum`, `Average`, `Count`, `Median`, and more, or create custom calculations with [formulas](/explore-analyze/visualize/lens.md#lens-formulas).

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

<!-- TODO: Add screenshot
![Tag cloud showing popular request URLs](/explore-analyze/images/tag-cloud-example-urls.png "=70%")
-->

**Top product categories**
:   Show which product categories are most popular:

    * Example based on: {{kib}} Sample Data eCommerce
    * **Tags**: `category.keyword` (Top 20 values)
    * **Metric**: Count
    * **Color mapping**: Distinct colors per category

<!-- TODO: Add screenshot
![Tag cloud showing top product categories](/explore-analyze/images/tag-cloud-example-categories.png "=70%")
-->

**Frequent log sources**
:   Identify which systems generate the most log entries:

    * Example based on: System logs
    * **Tags**: `host.name` or `service.name` (Top 25 values)
    * **Metric**: Count
    * **Orientation**: Right angled for visual variety

<!-- TODO: Add screenshot
![Tag cloud showing frequent log sources](/explore-analyze/images/tag-cloud-example-log-sources.png "=70%")
-->
