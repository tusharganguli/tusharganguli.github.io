---
layout: post
title: Anomaly Detection - Classifier
date: 2023-07-16 11:11:11-1111
description: Techniques for Anomaly Detection using Multi-Classifier
tags: anomaly_detection data_analysis time_series
categories: DataScience
giscus_comments: true
related_posts: false
---

For categorical data an ensemble of multiple classifiers were used:

- Logistic Regression, K-Nearest Neighbor, Support Vector Classifier, Decision Tree Classifier
- Different voting classifiers were analyzed (hard and soft voting classifier).
- Calibration techniques were applied namely: Platt Scaling and Isotonic Regression.

{::nomarkdown}
{% assign jupyter_path = '/assets/jupyter/ad_multiclassifier.ipynb' | relative_url %}
{% capture notebook_exists %}{% file_exists /assets/jupyter/ad_multiclassifier.ipynb %}{% endcapture %}
{% if notebook_exists == 'true' %}
{% jupyter_notebook jupyter_path %}
{% else %}

  <p>Sorry, the notebook you are looking for does not exist.</p>
{% endif %}
{:/nomarkdown}
