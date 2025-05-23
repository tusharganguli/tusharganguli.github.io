---
layout: post
title: Anomaly Detection - Time Series
date: 2023-07-16 11:11:11-1111
description: Techniques for Anomaly Detection using Time Series
tags: anomaly_detection data_analysis time_series
categories: DataScience
giscus_comments: true
related_posts: false
---

For time series data, methods studied and implemented were:

- Isolation forest
- Local Outlier Factor
- Autoencoders
- AutoRegressive Integrated Moving Average(ARIMA)
- Moving Average
- Z-score analysis.

{::nomarkdown}
{% assign jupyter_path = '/assets/jupyter/ad_timeseries.ipynb' | relative_url %}
{% capture notebook_exists %}{% file_exists /assets/jupyter/ad_timeseries.ipynb %}{% endcapture %}
{% if notebook_exists == 'true' %}
{% jupyter_notebook jupyter_path %}
{% else %}

  <p>Sorry, the notebook you are looking for does not exist.</p>
{% endif %}
{:/nomarkdown}
