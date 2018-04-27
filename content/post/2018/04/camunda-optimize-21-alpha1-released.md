+++
author = "Johannes Heinemann, Sebastian Stamm"
categories = ["Camunda Optimize"]
tags = ["Camunda Optimize", "Release Note"]
date = "2018-04-27T15:00:00+01:00"
title = "Camunda Optimize 2.1.0-alpha1 Released"
+++

We are happy to announce the release of Camunda Optimize version 2.1.0-alpha1.

This release marks the first alpha release of Camunda Optimize 2.1.0. The current alpha version can deal with process instances that were delete from the engine history and with that also the data import performance was improved significantly. In addition to that, the interactions with tables have been have been extended, especially for Raw Data Reports.

<!--more-->

The [complete release notes](https://app.camunda.com/jira/secure/ReleaseNote.jspa?projectId=10730&version=15305) are available in Jira.

You can [try out a free trial of Camunda Optimize](#how-to-get-it).

# Data import improvements

## Optimize data import can handle the deletion of historic data

## Performance enhancements

# Table improvements

{{< figure class="main teaser no-border" src="tableImprovements.gif">}}

This release contains significant improvements to Raw Data Reports. With an increased default column width the contents of the table are easily readable even when the table contains many columns. We also moved the Variable label from the variable columns to a higher-level header, freeing up space in all columns.

## Performance Improvements for large datasets

Tables with a lot of rows and columns are now rendered significantly faster. If the table contains more than 20 rows, you can use the "Previous" and "Next" buttons at the bottom of the table to scroll through the entries.

## Hide and re-arrange columns

Raw Data reports contain a lot of information. With Camunda Optimize 2.1.0-alpha1 you can configure which information is included in the table report. Using a button to the right of the table in the Report edit mode, you can select which columns are included in the report.

You can also re-arrange the order of columns by grabbing any column by the header and moving it to the desired position.

# How to get it

If you want to give the new Camunda Optimize a shot, you can download the alpha release from [here](https://docs.camunda.org/enterprise/download/#camunda-optimize) with your enterprise customer credentials. In contrast to the core engine, Camunda Optimize is not open source, so please sign up [here](https://camunda.com/download/enterprise/) for a free 30 days trail version.
