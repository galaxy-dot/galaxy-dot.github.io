---
title: Hexo - Add Google Analytics and View Counts
date: 2024-07-23
categories: hexo
tags:
  - hexo
---

This article outlines how to add Google Analytics to NexT themed Hexo site.  <!-- more -->

## What's Google Analytics (GA)
In a nutshell, GA is a web analytics service offered by Google that tracks and reports website traffic and also mobile app traffic & events, currently as a platform inside the Google Marketing Platform brand. [Wikipedia](https://en.wikipedia.org/wiki/Google_Analytics)   

If you're interested in to learn more about GA, feel free to check out their [free courses](https://analytics.google.com/analytics/academy/).

What we need for adding GA is an Google Analytic account where you can find the tracking ID starting with `G`

## NexT Supports GA
According to NexT offical documentations [Statistics and Analytics](
https://theme-next.js.org/docs/third-party-services/), all we need to do is update `google_analytics` section in `_config.next.yml` with our tracking ID
```
google_analytics:
  tracking_id: <your tracking ID>
  # If you only need the pageview feature, set the following option to true to get a better performance.
  only_pageview: false
```
Once your deployed above changes, you should be able to see statics in your GA dashboard. 

## Firebase
Since we're using GA, it won't take much efforts to add Firebase, which can be used to show view counts. 
Following [instructions](https://theme-next.js.org/docs/third-party-services/statistics-and-analytics#Firebase) to create the apiKey and projectId. It's safe to expose the apiKey to public in this use case so do not worry. 

## Refs

1. [Statistics and Analytics](
https://theme-next.js.org/docs/third-party-services/)
2. [Add Article Views to Your Hexo Blog](https://qiuyiwu.github.io/2019/01/26/Hexo-View/)
3. [Hexo NexT 主題的閱讀次數統計](https://blog.maple3142.net/2017/11/04/hexo-next-readcount/)
