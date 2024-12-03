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

 <div
  class="jupyter-notebook"
  style="position: relative; width: 100%; margin: 0 auto;">
  <div class="jupyter-notebook-iframe-container">
    <iframe
      src="/assets/jupyter/ad_multiclassifier.html"
      style="position: absolute; top: 0; left: 0; border-style: none;"
      width="100%"
      height="100%"
      onload="this.parentElement.style.paddingBottom = (this.contentWindow.document.documentElement.scrollHeight + 10) + 'px'"></iframe>
  </div>
</div>
