---
navigation_title: Region map charts
applies_to:
  stack: ga
  serverless: ga
description: Instructions and best practices for building region map charts with Kibana Lens in Elastic.
---

# Build region map charts with {{kib}}

Region map charts display data on a geographic map, using colors to represent values for different regions such as countries, states, or provinces. They are ideal for showing geographic distributions, comparing metrics across locations, and identifying regional patterns in your data.

You can create region map charts in {{kib}} using [**Lens**](../lens.md).

<!-- TODO: Add screenshot
![Example Lens region map chart showing sales by country](/explore-analyze/images/region-map-chart-example.png)
-->

## When to use region map charts

Region map charts work best when:

* Your data has a **geographic dimension** (country, state, region)
* You want to show **geographic patterns** or distributions
* You need to compare **values across regions** at a glance
* The geographic context adds meaning to your analysis

Consider using [bar charts](bar-charts.md) instead when:

* You need **precise value comparisons** between regions
* Geographic context is less important than the data values
* You have **many regions** that would clutter a map

Consider using [heat maps](heat-map-charts.md) instead when:

* You're comparing **two non-geographic dimensions**
* You want to show **correlations** between variables

## Build a region map chart

:::{include} ../../_snippets/lens-prerequisites.md
:::

To build a region map chart:

::::::{stepper}

:::::{step} Access Lens
**Lens** is {{kib}}'s main visualization editor. You can access it:
- From a dashboard: On the **Dashboards** page, open or create the dashboard where you want to add a region map chart, then add a new visualization.
- From the **Visualize library** page by creating a new visualization.
:::::

:::::{step} Set the visualization to Region map
New visualizations often start as **Bar** charts.

Using the **Visualization type** dropdown, select **Region map**.
:::::

:::::{step} Define the data to show
1. Select the {{data-source}} that contains your data.
2. Configure the [**Region**](#region-settings) dimension to define which geographic field to use. This field should contain region codes (ISO country codes, state abbreviations, and more) that can be matched to map boundaries.
3. Configure the [**Metric**](#metric-settings) dimension to define the value displayed for each region. This determines the color intensity.

The chart preview updates to show a map with regions colored by metric value. If regions appear gray, verify that the field values match the expected geographic codes (such as ISO country codes).
:::::

:::::{step} Customize the chart to follow best practices
Tweak the appearance of the chart to your needs. Consider the following best practices:

**Use appropriate region granularity**
:   Match the map granularity to your data. Use country-level maps for global data, or state/province maps for national data.

**Choose a sequential color palette**
:   For data ranging from low to high, use a sequential palette (light to dark) to show intensity clearly.

**Handle missing regions**
:   Decide how to display regions with no data. Gray or transparent regions indicate missing data without distorting the visualization.

**Consider choropleth best practices**
:   Region maps are choropleth maps, where color represents data values. Be aware that larger regions can visually dominate, even if their values are smaller.

Refer to [Region map chart settings](#region-map-chart-settings) to find all configuration options for your region map chart.
:::::

:::::{step} Save the chart
- If you accessed Lens from a dashboard, select **Save and return** to save the visualization and add it to that dashboard, or select **Save to library** to add the visualization to the Visualize library and reuse it later.
- If you accessed Lens from the Visualize library, select **Save**. A menu opens and offers you to add the visualization to a dashboard and to the Visualize library.
:::::

::::::

## Advanced region map chart scenarios

### Visualize global traffic distribution [global-traffic]

Show how website visitors or requests are distributed across countries.

#### Example: Website visitors by country

This example uses the [sample web logs data](/manage-data/ingest/sample-data.md) to visualize visitor geography.

1. Create a **Region map** chart using the **{{kib}} Sample Data Logs** {{data-source}}.
2. For the **Region** dimension, select `geo.src` (source country).
3. For the **Metric**, select **Count** to show the number of requests from each country.
4. Select {icon}`brush` **Style** and choose a sequential color palette (for example, blues).

The resulting map shows countries colored by traffic volume, with darker colors indicating more visitors.

### Compare sales performance by region [sales-by-region]

Use region maps to visualize sales or revenue distribution across geographic areas.

#### Example: Revenue by country

1. Create a **Region map** chart using your sales {{data-source}}.
2. For the **Region** dimension, select your country field.
3. For the **Metric**, select `Sum(revenue)` or your equivalent sales field.
4. Configure color ranges to highlight top-performing and underperforming regions.

### Use different map layers [map-layers]

Region maps support different geographic boundary sets. Choose the appropriate layer based on your data:

* **World countries**: For global data using ISO country codes
* **US states**: For United States data using state abbreviations
* **Administrative regions**: For sub-country analysis where available

To change the map layer:

1. Select {icon}`brush` **Style**.
2. In **Layer**, select the appropriate boundary set.

## Region map chart settings [region-map-chart-settings]

Customize your region map chart to display exactly the information you need, formatted the way you want.

### Region settings [region-settings]

The **Region** dimension defines which geographic areas to display on the map.

**Data**
:   The **Region** dimension supports the following functions:

    - **Top values**: Display the regions with the highest metric values.
      - **Field**: Select the field to group by. You can add up to 4 fields to create multi-term groupings. When multiple fields are selected, each region represents a unique combination of values across those fields. You can reorder the fields by dragging them to change their priority.
      - **Number of values**: How many regions to display.
      :::{include} ../../_snippets/lens-rank-by-options.md
      :::
      :::{include} ../../_snippets/lens-breakdown-advanced-settings.md
      :::

**Appearance**
:   - **Name**: Customize the region label displayed in tooltips.

:::{note}
The region field must contain values that can be matched to geographic boundaries, such as ISO 3166 country codes (US, GB, DE) or state/province codes. Fields containing full country or region names may not match correctly.
:::

### Metric settings [metric-settings]

The **Metric** dimension defines the value that determines each region's color.

**Data**
:   The value that determines region color intensity. When you drag a field onto the chart, {{kib}} suggests a function based on the field type. You can use aggregation functions like `Sum`, `Average`, `Count`, `Median`, and more, or create custom calculations with formulas. Refer to [](/explore-analyze/visualize/lens.md#lens-formulas) for examples.

    :::{include} ../../_snippets/lens-value-advanced-settings.md
    :::

**Appearance**
:   - **Name**: Customize the metric label displayed in tooltips.
    - **Value format**: Control how numeric values are displayed (number, percent, bytes, and more).

### General layout [appearance-options]

When creating or editing a visualization, you can customize several appearance options from the {icon}`brush` **Style** or ![Legend icon](/explore-analyze/images/kibana-legend-icon.svg "") **Legend** menus.

#### Style settings

**Layer**
:   Select the geographic boundary set to use:
    - **World countries**: ISO country boundaries
    - **US states**: United States state boundaries
    - Other available boundary sets based on your configuration

**Color**

**Palette**
:   Choose a color palette for the map:
    - **Sequential**: Colors range from light to dark, suitable for data ranging from low to high.
    - **Diverging**: Colors diverge from a neutral midpoint.

**Custom ranges**
:   Enable custom color ranges to define specific value-to-color mappings.

**Reverse**
:   Reverse the color palette direction.

#### Legend settings

**Visibility**
:   Specify whether to automatically show the legend or hide it:
    - **Auto**: Show the legend when useful (default).
    - **Show**: Always show the legend.
    - **Hide**: Never show the legend.

**Position**
:   Set the legend position: **Top**, **Left**, **Right**, or **Bottom**.

## Region map chart examples

The following examples show various configuration options for building impactful region map charts.

**Website traffic by country**
:   Visualize the geographic distribution of website visitors:

    * Example based on: {{kib}} Sample Data Logs
    * **Region**: `geo.src` (Top values)
    * **Metric**: Count
    * **Color palette**: Blues (sequential)
    * **Layer**: World countries

**Customer distribution by state**
:   Show where your customers are located within a country:

    * Example based on: {{kib}} Sample Data eCommerce
    * **Region**: `geoip.region_name` (Top values)
    * **Metric**: Unique count of `customer_id`
    * **Color palette**: Greens (sequential)
    * **Layer**: US states (if applicable)

**Revenue per capita by country**
:   Compare normalized revenue across countries:

    * Example based on: Sales data with population information
    * **Region**: Country code field
    * **Metric**: Formula `sum(revenue) / sum(population)`
    * **Color palette**: Oranges (sequential)
    * **Layer**: World countries
