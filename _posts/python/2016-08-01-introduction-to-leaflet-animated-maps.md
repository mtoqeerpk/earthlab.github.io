---
layout: single
title: 'Using Leaflet and Folium to make interactive maps in Python'
date: 2016-08-01
authors: [Matt Oakley]
category: [tutorials]
excerpt: 'This tutorial shows how to make interactive maps in Python with folium.'
sidebar:
  nav:
author_profile: false
comments: true
lang: [python]
lib: [folium, pandas]
---

Interactive maps are useful for data exploration and communicating research. [Leaflet](http://leafletjs.com/), an open-source JavaScript library, facilitates the development of interactive maps, but is designed to be used via JavaScript. This tutorial provides a short demonstration of the [folium](https://python-visualization.github.io/folium/) package, which provides an easy to use interface to Leaflet for Python users. 

*Note: to see these maps execute these commands on your local machine*

## Objectives

- Create a map centered at an inputted location
- Create points on the map
- Try out some of folium's other features

## Dependencies

- Python
- folium
- pandas


```python
!pip install folium
!pip install pandas
```


```python
import folium
from folium.plugins import MarkerCluster
import pandas as pd
```

## Show Map Centered at a Point

The most fundamental part of creating an interactive map is to allow users to input a location and view that location on the map; imagine how difficult it'd be to scroll/zoom to each specific point you want to view! Let's create a map and center it at a specific geographic point such as Boulder, Colorado.


```python
#Define coordinates of where we want to center our map
boulder_coords = [40.015, -105.2705]

#Create the map
my_map = folium.Map(location = boulder_coords, zoom_start = 13)

#Display the map
my_map
```




<div style="width:100%;"><div style="position:relative;width:100%;height:0;padding-bottom:60%;"><iframe src="data:text/html;charset=utf-8;base64,PCFET0NUWVBFIGh0bWw+CjxoZWFkPiAgICAKICAgIDxtZXRhIGh0dHAtZXF1aXY9ImNvbnRlbnQtdHlwZSIgY29udGVudD0idGV4dC9odG1sOyBjaGFyc2V0PVVURi04IiAvPgogICAgPHNjcmlwdD5MX1BSRUZFUl9DQU5WQVM9ZmFsc2U7IExfTk9fVE9VQ0g9ZmFsc2U7IExfRElTQUJMRV8zRD1mYWxzZTs8L3NjcmlwdD4KICAgIDxzY3JpcHQgc3JjPSJodHRwczovL2Nkbi5qc2RlbGl2ci5uZXQvbnBtL2xlYWZsZXRAMS4yLjAvZGlzdC9sZWFmbGV0LmpzIj48L3NjcmlwdD4KICAgIDxzY3JpcHQgc3JjPSJodHRwczovL2FqYXguZ29vZ2xlYXBpcy5jb20vYWpheC9saWJzL2pxdWVyeS8xLjExLjEvanF1ZXJ5Lm1pbi5qcyI+PC9zY3JpcHQ+CiAgICA8c2NyaXB0IHNyYz0iaHR0cHM6Ly9tYXhjZG4uYm9vdHN0cmFwY2RuLmNvbS9ib290c3RyYXAvMy4yLjAvanMvYm9vdHN0cmFwLm1pbi5qcyI+PC9zY3JpcHQ+CiAgICA8c2NyaXB0IHNyYz0iaHR0cHM6Ly9jZG5qcy5jbG91ZGZsYXJlLmNvbS9hamF4L2xpYnMvTGVhZmxldC5hd2Vzb21lLW1hcmtlcnMvMi4wLjIvbGVhZmxldC5hd2Vzb21lLW1hcmtlcnMuanMiPjwvc2NyaXB0PgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL2Nkbi5qc2RlbGl2ci5uZXQvbnBtL2xlYWZsZXRAMS4yLjAvZGlzdC9sZWFmbGV0LmNzcyIvPgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL21heGNkbi5ib290c3RyYXBjZG4uY29tL2Jvb3RzdHJhcC8zLjIuMC9jc3MvYm9vdHN0cmFwLm1pbi5jc3MiLz4KICAgIDxsaW5rIHJlbD0ic3R5bGVzaGVldCIgaHJlZj0iaHR0cHM6Ly9tYXhjZG4uYm9vdHN0cmFwY2RuLmNvbS9ib290c3RyYXAvMy4yLjAvY3NzL2Jvb3RzdHJhcC10aGVtZS5taW4uY3NzIi8+CiAgICA8bGluayByZWw9InN0eWxlc2hlZXQiIGhyZWY9Imh0dHBzOi8vbWF4Y2RuLmJvb3RzdHJhcGNkbi5jb20vZm9udC1hd2Vzb21lLzQuNi4zL2Nzcy9mb250LWF3ZXNvbWUubWluLmNzcyIvPgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL2NkbmpzLmNsb3VkZmxhcmUuY29tL2FqYXgvbGlicy9MZWFmbGV0LmF3ZXNvbWUtbWFya2Vycy8yLjAuMi9sZWFmbGV0LmF3ZXNvbWUtbWFya2Vycy5jc3MiLz4KICAgIDxsaW5rIHJlbD0ic3R5bGVzaGVldCIgaHJlZj0iaHR0cHM6Ly9yYXdnaXQuY29tL3B5dGhvbi12aXN1YWxpemF0aW9uL2ZvbGl1bS9tYXN0ZXIvZm9saXVtL3RlbXBsYXRlcy9sZWFmbGV0LmF3ZXNvbWUucm90YXRlLmNzcyIvPgogICAgPHN0eWxlPmh0bWwsIGJvZHkge3dpZHRoOiAxMDAlO2hlaWdodDogMTAwJTttYXJnaW46IDA7cGFkZGluZzogMDt9PC9zdHlsZT4KICAgIDxzdHlsZT4jbWFwIHtwb3NpdGlvbjphYnNvbHV0ZTt0b3A6MDtib3R0b206MDtyaWdodDowO2xlZnQ6MDt9PC9zdHlsZT4KICAgIAogICAgPHN0eWxlPiNtYXBfMjRjNDI5MTA3YWI0NDlmM2JhZDY1YzMzZjZlY2Q3YjYgewogICAgICAgIHBvc2l0aW9uOiByZWxhdGl2ZTsKICAgICAgICB3aWR0aDogMTAwLjAlOwogICAgICAgIGhlaWdodDogMTAwLjAlOwogICAgICAgIGxlZnQ6IDAuMCU7CiAgICAgICAgdG9wOiAwLjAlOwogICAgICAgIH0KICAgIDwvc3R5bGU+CjwvaGVhZD4KPGJvZHk+ICAgIAogICAgCiAgICA8ZGl2IGNsYXNzPSJmb2xpdW0tbWFwIiBpZD0ibWFwXzI0YzQyOTEwN2FiNDQ5ZjNiYWQ2NWMzM2Y2ZWNkN2I2IiA+PC9kaXY+CjwvYm9keT4KPHNjcmlwdD4gICAgCiAgICAKICAgIAogICAgICAgIHZhciBib3VuZHMgPSBudWxsOwogICAgCgogICAgdmFyIG1hcF8yNGM0MjkxMDdhYjQ0OWYzYmFkNjVjMzNmNmVjZDdiNiA9IEwubWFwKAogICAgICAgICdtYXBfMjRjNDI5MTA3YWI0NDlmM2JhZDY1YzMzZjZlY2Q3YjYnLCB7CiAgICAgICAgY2VudGVyOiBbNDAuMDE1LCAtMTA1LjI3MDVdLAogICAgICAgIHpvb206IDEzLAogICAgICAgIG1heEJvdW5kczogYm91bmRzLAogICAgICAgIGxheWVyczogW10sCiAgICAgICAgd29ybGRDb3B5SnVtcDogZmFsc2UsCiAgICAgICAgY3JzOiBMLkNSUy5FUFNHMzg1NywKICAgICAgICB6b29tQ29udHJvbDogdHJ1ZSwKICAgICAgICB9KTsKCiAgICAKICAgIAogICAgdmFyIHRpbGVfbGF5ZXJfZTUzZDYxZGRkODJmNGUyMTg1Njc1ZjAxZmExY2VhMTUgPSBMLnRpbGVMYXllcigKICAgICAgICAnaHR0cHM6Ly97c30udGlsZS5vcGVuc3RyZWV0bWFwLm9yZy97en0ve3h9L3t5fS5wbmcnLAogICAgICAgIHsKICAgICAgICAiYXR0cmlidXRpb24iOiBudWxsLAogICAgICAgICJkZXRlY3RSZXRpbmEiOiBmYWxzZSwKICAgICAgICAibWF4TmF0aXZlWm9vbSI6IDE4LAogICAgICAgICJtYXhab29tIjogMTgsCiAgICAgICAgIm1pblpvb20iOiAwLAogICAgICAgICJub1dyYXAiOiBmYWxzZSwKICAgICAgICAic3ViZG9tYWlucyI6ICJhYmMiCn0pLmFkZFRvKG1hcF8yNGM0MjkxMDdhYjQ0OWYzYmFkNjVjMzNmNmVjZDdiNik7Cjwvc2NyaXB0Pg==" style="position:absolute;width:100%;height:100%;left:0;top:0;border:none !important;" allowfullscreen webkitallowfullscreen mozallowfullscreen></iframe></div></div>



## Adding Markers to the Map

Now that we're able to display a map centeret at a point of our choosing, we can try adding some more content to the map via markers. Markers can be extremely useful for storing information about locations on the map such as cross streets, building information, etc. Using the same map, let's add markers indicating where CU Boulder is as well as East Campus and the SEEC building.


```python
#Define the coordinates we want our markers to be at
CU_Boulder_coords = [40.007153, -105.266930]
East_Campus_coords = [40.013501, -105.251889]
SEEC_coords = [40.009837, -105.241905]

#Add markers to the map
folium.Marker(CU_Boulder_coords, popup = 'CU Boulder').add_to(my_map)
folium.Marker(East_Campus_coords, popup = 'East Campus').add_to(my_map)
folium.Marker(SEEC_coords, popup = 'SEEC Building').add_to(my_map)

#Display the map
my_map
```




<div style="width:100%;"><div style="position:relative;width:100%;height:0;padding-bottom:60%;"><iframe src="data:text/html;charset=utf-8;base64,PCFET0NUWVBFIGh0bWw+CjxoZWFkPiAgICAKICAgIDxtZXRhIGh0dHAtZXF1aXY9ImNvbnRlbnQtdHlwZSIgY29udGVudD0idGV4dC9odG1sOyBjaGFyc2V0PVVURi04IiAvPgogICAgPHNjcmlwdD5MX1BSRUZFUl9DQU5WQVM9ZmFsc2U7IExfTk9fVE9VQ0g9ZmFsc2U7IExfRElTQUJMRV8zRD1mYWxzZTs8L3NjcmlwdD4KICAgIDxzY3JpcHQgc3JjPSJodHRwczovL2Nkbi5qc2RlbGl2ci5uZXQvbnBtL2xlYWZsZXRAMS4yLjAvZGlzdC9sZWFmbGV0LmpzIj48L3NjcmlwdD4KICAgIDxzY3JpcHQgc3JjPSJodHRwczovL2FqYXguZ29vZ2xlYXBpcy5jb20vYWpheC9saWJzL2pxdWVyeS8xLjExLjEvanF1ZXJ5Lm1pbi5qcyI+PC9zY3JpcHQ+CiAgICA8c2NyaXB0IHNyYz0iaHR0cHM6Ly9tYXhjZG4uYm9vdHN0cmFwY2RuLmNvbS9ib290c3RyYXAvMy4yLjAvanMvYm9vdHN0cmFwLm1pbi5qcyI+PC9zY3JpcHQ+CiAgICA8c2NyaXB0IHNyYz0iaHR0cHM6Ly9jZG5qcy5jbG91ZGZsYXJlLmNvbS9hamF4L2xpYnMvTGVhZmxldC5hd2Vzb21lLW1hcmtlcnMvMi4wLjIvbGVhZmxldC5hd2Vzb21lLW1hcmtlcnMuanMiPjwvc2NyaXB0PgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL2Nkbi5qc2RlbGl2ci5uZXQvbnBtL2xlYWZsZXRAMS4yLjAvZGlzdC9sZWFmbGV0LmNzcyIvPgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL21heGNkbi5ib290c3RyYXBjZG4uY29tL2Jvb3RzdHJhcC8zLjIuMC9jc3MvYm9vdHN0cmFwLm1pbi5jc3MiLz4KICAgIDxsaW5rIHJlbD0ic3R5bGVzaGVldCIgaHJlZj0iaHR0cHM6Ly9tYXhjZG4uYm9vdHN0cmFwY2RuLmNvbS9ib290c3RyYXAvMy4yLjAvY3NzL2Jvb3RzdHJhcC10aGVtZS5taW4uY3NzIi8+CiAgICA8bGluayByZWw9InN0eWxlc2hlZXQiIGhyZWY9Imh0dHBzOi8vbWF4Y2RuLmJvb3RzdHJhcGNkbi5jb20vZm9udC1hd2Vzb21lLzQuNi4zL2Nzcy9mb250LWF3ZXNvbWUubWluLmNzcyIvPgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL2NkbmpzLmNsb3VkZmxhcmUuY29tL2FqYXgvbGlicy9MZWFmbGV0LmF3ZXNvbWUtbWFya2Vycy8yLjAuMi9sZWFmbGV0LmF3ZXNvbWUtbWFya2Vycy5jc3MiLz4KICAgIDxsaW5rIHJlbD0ic3R5bGVzaGVldCIgaHJlZj0iaHR0cHM6Ly9yYXdnaXQuY29tL3B5dGhvbi12aXN1YWxpemF0aW9uL2ZvbGl1bS9tYXN0ZXIvZm9saXVtL3RlbXBsYXRlcy9sZWFmbGV0LmF3ZXNvbWUucm90YXRlLmNzcyIvPgogICAgPHN0eWxlPmh0bWwsIGJvZHkge3dpZHRoOiAxMDAlO2hlaWdodDogMTAwJTttYXJnaW46IDA7cGFkZGluZzogMDt9PC9zdHlsZT4KICAgIDxzdHlsZT4jbWFwIHtwb3NpdGlvbjphYnNvbHV0ZTt0b3A6MDtib3R0b206MDtyaWdodDowO2xlZnQ6MDt9PC9zdHlsZT4KICAgIAogICAgPHN0eWxlPiNtYXBfMjRjNDI5MTA3YWI0NDlmM2JhZDY1YzMzZjZlY2Q3YjYgewogICAgICAgIHBvc2l0aW9uOiByZWxhdGl2ZTsKICAgICAgICB3aWR0aDogMTAwLjAlOwogICAgICAgIGhlaWdodDogMTAwLjAlOwogICAgICAgIGxlZnQ6IDAuMCU7CiAgICAgICAgdG9wOiAwLjAlOwogICAgICAgIH0KICAgIDwvc3R5bGU+CjwvaGVhZD4KPGJvZHk+ICAgIAogICAgCiAgICA8ZGl2IGNsYXNzPSJmb2xpdW0tbWFwIiBpZD0ibWFwXzI0YzQyOTEwN2FiNDQ5ZjNiYWQ2NWMzM2Y2ZWNkN2I2IiA+PC9kaXY+CjwvYm9keT4KPHNjcmlwdD4gICAgCiAgICAKICAgIAogICAgICAgIHZhciBib3VuZHMgPSBudWxsOwogICAgCgogICAgdmFyIG1hcF8yNGM0MjkxMDdhYjQ0OWYzYmFkNjVjMzNmNmVjZDdiNiA9IEwubWFwKAogICAgICAgICdtYXBfMjRjNDI5MTA3YWI0NDlmM2JhZDY1YzMzZjZlY2Q3YjYnLCB7CiAgICAgICAgY2VudGVyOiBbNDAuMDE1LCAtMTA1LjI3MDVdLAogICAgICAgIHpvb206IDEzLAogICAgICAgIG1heEJvdW5kczogYm91bmRzLAogICAgICAgIGxheWVyczogW10sCiAgICAgICAgd29ybGRDb3B5SnVtcDogZmFsc2UsCiAgICAgICAgY3JzOiBMLkNSUy5FUFNHMzg1NywKICAgICAgICB6b29tQ29udHJvbDogdHJ1ZSwKICAgICAgICB9KTsKCiAgICAKICAgIAogICAgdmFyIHRpbGVfbGF5ZXJfZTUzZDYxZGRkODJmNGUyMTg1Njc1ZjAxZmExY2VhMTUgPSBMLnRpbGVMYXllcigKICAgICAgICAnaHR0cHM6Ly97c30udGlsZS5vcGVuc3RyZWV0bWFwLm9yZy97en0ve3h9L3t5fS5wbmcnLAogICAgICAgIHsKICAgICAgICAiYXR0cmlidXRpb24iOiBudWxsLAogICAgICAgICJkZXRlY3RSZXRpbmEiOiBmYWxzZSwKICAgICAgICAibWF4TmF0aXZlWm9vbSI6IDE4LAogICAgICAgICJtYXhab29tIjogMTgsCiAgICAgICAgIm1pblpvb20iOiAwLAogICAgICAgICJub1dyYXAiOiBmYWxzZSwKICAgICAgICAic3ViZG9tYWlucyI6ICJhYmMiCn0pLmFkZFRvKG1hcF8yNGM0MjkxMDdhYjQ0OWYzYmFkNjVjMzNmNmVjZDdiNik7CiAgICAKICAgICAgICB2YXIgbWFya2VyXzFkYjgwYzk5ZDRhNTQxMjg4MGE3MzQ4Mjk1MjU1MjU1ID0gTC5tYXJrZXIoCiAgICAgICAgICAgIFs0MC4wMDcxNTMsIC0xMDUuMjY2OTNdLAogICAgICAgICAgICB7CiAgICAgICAgICAgICAgICBpY29uOiBuZXcgTC5JY29uLkRlZmF1bHQoKQogICAgICAgICAgICAgICAgfQogICAgICAgICAgICApLmFkZFRvKG1hcF8yNGM0MjkxMDdhYjQ0OWYzYmFkNjVjMzNmNmVjZDdiNik7CiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2E5OTZkZGRjNmU2NzRkZmJiMTNmMDI0Yzg0NzBlZjM1ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnCiAgICAgICAgICAgIAogICAgICAgICAgICB9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfMWI3ZjgyNjdkMTE5NGM4MTgxZTljZGUxZWY5MmFkOTkgPSAkKCc8ZGl2IGlkPSJodG1sXzFiN2Y4MjY3ZDExOTRjODE4MWU5Y2RlMWVmOTJhZDk5IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5DVSBCb3VsZGVyPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9hOTk2ZGRkYzZlNjc0ZGZiYjEzZjAyNGM4NDcwZWYzNS5zZXRDb250ZW50KGh0bWxfMWI3ZjgyNjdkMTE5NGM4MTgxZTljZGUxZWY5MmFkOTkpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIG1hcmtlcl8xZGI4MGM5OWQ0YTU0MTI4ODBhNzM0ODI5NTI1NTI1NS5iaW5kUG9wdXAocG9wdXBfYTk5NmRkZGM2ZTY3NGRmYmIxM2YwMjRjODQ3MGVmMzUpCiAgICAgICAgICAgIDsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgdmFyIG1hcmtlcl9iNjZhMTUwM2Q1NGY0ODQ0YjgyMjM1NTJmMzVhNzU0YyA9IEwubWFya2VyKAogICAgICAgICAgICBbNDAuMDEzNTAxLCAtMTA1LjI1MTg4OV0sCiAgICAgICAgICAgIHsKICAgICAgICAgICAgICAgIGljb246IG5ldyBMLkljb24uRGVmYXVsdCgpCiAgICAgICAgICAgICAgICB9CiAgICAgICAgICAgICkuYWRkVG8obWFwXzI0YzQyOTEwN2FiNDQ5ZjNiYWQ2NWMzM2Y2ZWNkN2I2KTsKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfZWQzNTI5NTk2YjRiNDNiYmI0MzA3Mzc3YWQ1NWM2Y2YgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCcKICAgICAgICAgICAgCiAgICAgICAgICAgIH0pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9hMzc5MzhjN2Y3Yzg0Nzg4ODdmZGYxMDc3ZDRhYjQzZCA9ICQoJzxkaXYgaWQ9Imh0bWxfYTM3OTM4YzdmN2M4NDc4ODg3ZmRmMTA3N2Q0YWI0M2QiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkVhc3QgQ2FtcHVzPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9lZDM1Mjk1OTZiNGI0M2JiYjQzMDczNzdhZDU1YzZjZi5zZXRDb250ZW50KGh0bWxfYTM3OTM4YzdmN2M4NDc4ODg3ZmRmMTA3N2Q0YWI0M2QpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIG1hcmtlcl9iNjZhMTUwM2Q1NGY0ODQ0YjgyMjM1NTJmMzVhNzU0Yy5iaW5kUG9wdXAocG9wdXBfZWQzNTI5NTk2YjRiNDNiYmI0MzA3Mzc3YWQ1NWM2Y2YpCiAgICAgICAgICAgIDsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgdmFyIG1hcmtlcl9mNDRiMWVhZjc5OGU0YmJhYjdhZGVjNDZkZWE0YTUxYSA9IEwubWFya2VyKAogICAgICAgICAgICBbNDAuMDA5ODM3LCAtMTA1LjI0MTkwNV0sCiAgICAgICAgICAgIHsKICAgICAgICAgICAgICAgIGljb246IG5ldyBMLkljb24uRGVmYXVsdCgpCiAgICAgICAgICAgICAgICB9CiAgICAgICAgICAgICkuYWRkVG8obWFwXzI0YzQyOTEwN2FiNDQ5ZjNiYWQ2NWMzM2Y2ZWNkN2I2KTsKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfOGU2ODBkNjc0NjE3NDA2MDllMGNjMmM5YzI3NjE5ODggPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCcKICAgICAgICAgICAgCiAgICAgICAgICAgIH0pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF81MTY3ZGMxYmJkOGU0OTQwYWJlY2YwM2JlMzNlMjhiZiA9ICQoJzxkaXYgaWQ9Imh0bWxfNTE2N2RjMWJiZDhlNDk0MGFiZWNmMDNiZTMzZTI4YmYiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPlNFRUMgQnVpbGRpbmc8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzhlNjgwZDY3NDYxNzQwNjA5ZTBjYzJjOWMyNzYxOTg4LnNldENvbnRlbnQoaHRtbF81MTY3ZGMxYmJkOGU0OTQwYWJlY2YwM2JlMzNlMjhiZik7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgbWFya2VyX2Y0NGIxZWFmNzk4ZTRiYmFiN2FkZWM0NmRlYTRhNTFhLmJpbmRQb3B1cChwb3B1cF84ZTY4MGQ2NzQ2MTc0MDYwOWUwY2MyYzljMjc2MTk4OCkKICAgICAgICAgICAgOwoKICAgICAgICAgICAgCiAgICAgICAgCjwvc2NyaXB0Pg==" style="position:absolute;width:100%;height:100%;left:0;top:0;border:none !important;" allowfullscreen webkitallowfullscreen mozallowfullscreen></iframe></div></div>



## Other Features

Leaflet (or folium for our purposes) has some other pretty great features that you may find useful. There are some great tutorials that can be found online with examples that should be easily adaptable to your code. Additionally, folium can work with pandas dataframes in order to overlay data onto the interactive map. 


```python
#Adding a tileset to our map
map_with_tiles = folium.Map(location = boulder_coords, tiles = 'Stamen Toner')
map_with_tiles
```




<div style="width:100%;"><div style="position:relative;width:100%;height:0;padding-bottom:60%;"><iframe src="data:text/html;charset=utf-8;base64,PCFET0NUWVBFIGh0bWw+CjxoZWFkPiAgICAKICAgIDxtZXRhIGh0dHAtZXF1aXY9ImNvbnRlbnQtdHlwZSIgY29udGVudD0idGV4dC9odG1sOyBjaGFyc2V0PVVURi04IiAvPgogICAgPHNjcmlwdD5MX1BSRUZFUl9DQU5WQVM9ZmFsc2U7IExfTk9fVE9VQ0g9ZmFsc2U7IExfRElTQUJMRV8zRD1mYWxzZTs8L3NjcmlwdD4KICAgIDxzY3JpcHQgc3JjPSJodHRwczovL2Nkbi5qc2RlbGl2ci5uZXQvbnBtL2xlYWZsZXRAMS4yLjAvZGlzdC9sZWFmbGV0LmpzIj48L3NjcmlwdD4KICAgIDxzY3JpcHQgc3JjPSJodHRwczovL2FqYXguZ29vZ2xlYXBpcy5jb20vYWpheC9saWJzL2pxdWVyeS8xLjExLjEvanF1ZXJ5Lm1pbi5qcyI+PC9zY3JpcHQ+CiAgICA8c2NyaXB0IHNyYz0iaHR0cHM6Ly9tYXhjZG4uYm9vdHN0cmFwY2RuLmNvbS9ib290c3RyYXAvMy4yLjAvanMvYm9vdHN0cmFwLm1pbi5qcyI+PC9zY3JpcHQ+CiAgICA8c2NyaXB0IHNyYz0iaHR0cHM6Ly9jZG5qcy5jbG91ZGZsYXJlLmNvbS9hamF4L2xpYnMvTGVhZmxldC5hd2Vzb21lLW1hcmtlcnMvMi4wLjIvbGVhZmxldC5hd2Vzb21lLW1hcmtlcnMuanMiPjwvc2NyaXB0PgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL2Nkbi5qc2RlbGl2ci5uZXQvbnBtL2xlYWZsZXRAMS4yLjAvZGlzdC9sZWFmbGV0LmNzcyIvPgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL21heGNkbi5ib290c3RyYXBjZG4uY29tL2Jvb3RzdHJhcC8zLjIuMC9jc3MvYm9vdHN0cmFwLm1pbi5jc3MiLz4KICAgIDxsaW5rIHJlbD0ic3R5bGVzaGVldCIgaHJlZj0iaHR0cHM6Ly9tYXhjZG4uYm9vdHN0cmFwY2RuLmNvbS9ib290c3RyYXAvMy4yLjAvY3NzL2Jvb3RzdHJhcC10aGVtZS5taW4uY3NzIi8+CiAgICA8bGluayByZWw9InN0eWxlc2hlZXQiIGhyZWY9Imh0dHBzOi8vbWF4Y2RuLmJvb3RzdHJhcGNkbi5jb20vZm9udC1hd2Vzb21lLzQuNi4zL2Nzcy9mb250LWF3ZXNvbWUubWluLmNzcyIvPgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL2NkbmpzLmNsb3VkZmxhcmUuY29tL2FqYXgvbGlicy9MZWFmbGV0LmF3ZXNvbWUtbWFya2Vycy8yLjAuMi9sZWFmbGV0LmF3ZXNvbWUtbWFya2Vycy5jc3MiLz4KICAgIDxsaW5rIHJlbD0ic3R5bGVzaGVldCIgaHJlZj0iaHR0cHM6Ly9yYXdnaXQuY29tL3B5dGhvbi12aXN1YWxpemF0aW9uL2ZvbGl1bS9tYXN0ZXIvZm9saXVtL3RlbXBsYXRlcy9sZWFmbGV0LmF3ZXNvbWUucm90YXRlLmNzcyIvPgogICAgPHN0eWxlPmh0bWwsIGJvZHkge3dpZHRoOiAxMDAlO2hlaWdodDogMTAwJTttYXJnaW46IDA7cGFkZGluZzogMDt9PC9zdHlsZT4KICAgIDxzdHlsZT4jbWFwIHtwb3NpdGlvbjphYnNvbHV0ZTt0b3A6MDtib3R0b206MDtyaWdodDowO2xlZnQ6MDt9PC9zdHlsZT4KICAgIAogICAgPHN0eWxlPiNtYXBfMzBlY2MxMjNkZmU0NDVjY2FhNWEyODk0ZmM1ZDk4NWIgewogICAgICAgIHBvc2l0aW9uOiByZWxhdGl2ZTsKICAgICAgICB3aWR0aDogMTAwLjAlOwogICAgICAgIGhlaWdodDogMTAwLjAlOwogICAgICAgIGxlZnQ6IDAuMCU7CiAgICAgICAgdG9wOiAwLjAlOwogICAgICAgIH0KICAgIDwvc3R5bGU+CjwvaGVhZD4KPGJvZHk+ICAgIAogICAgCiAgICA8ZGl2IGNsYXNzPSJmb2xpdW0tbWFwIiBpZD0ibWFwXzMwZWNjMTIzZGZlNDQ1Y2NhYTVhMjg5NGZjNWQ5ODViIiA+PC9kaXY+CjwvYm9keT4KPHNjcmlwdD4gICAgCiAgICAKICAgIAogICAgICAgIHZhciBib3VuZHMgPSBudWxsOwogICAgCgogICAgdmFyIG1hcF8zMGVjYzEyM2RmZTQ0NWNjYWE1YTI4OTRmYzVkOTg1YiA9IEwubWFwKAogICAgICAgICdtYXBfMzBlY2MxMjNkZmU0NDVjY2FhNWEyODk0ZmM1ZDk4NWInLCB7CiAgICAgICAgY2VudGVyOiBbNDAuMDE1LCAtMTA1LjI3MDVdLAogICAgICAgIHpvb206IDEwLAogICAgICAgIG1heEJvdW5kczogYm91bmRzLAogICAgICAgIGxheWVyczogW10sCiAgICAgICAgd29ybGRDb3B5SnVtcDogZmFsc2UsCiAgICAgICAgY3JzOiBMLkNSUy5FUFNHMzg1NywKICAgICAgICB6b29tQ29udHJvbDogdHJ1ZSwKICAgICAgICB9KTsKCiAgICAKICAgIAogICAgdmFyIHRpbGVfbGF5ZXJfMjAyZDgxMzZhYjM1NDAyMmE2NGQ4M2EwYTY0YTc3N2QgPSBMLnRpbGVMYXllcigKICAgICAgICAnaHR0cHM6Ly9zdGFtZW4tdGlsZXMte3N9LmEuc3NsLmZhc3RseS5uZXQvdG9uZXIve3p9L3t4fS97eX0ucG5nJywKICAgICAgICB7CiAgICAgICAgImF0dHJpYnV0aW9uIjogbnVsbCwKICAgICAgICAiZGV0ZWN0UmV0aW5hIjogZmFsc2UsCiAgICAgICAgIm1heE5hdGl2ZVpvb20iOiAxOCwKICAgICAgICAibWF4Wm9vbSI6IDE4LAogICAgICAgICJtaW5ab29tIjogMCwKICAgICAgICAibm9XcmFwIjogZmFsc2UsCiAgICAgICAgInN1YmRvbWFpbnMiOiAiYWJjIgp9KS5hZGRUbyhtYXBfMzBlY2MxMjNkZmU0NDVjY2FhNWEyODk0ZmM1ZDk4NWIpOwo8L3NjcmlwdD4=" style="position:absolute;width:100%;height:100%;left:0;top:0;border:none !important;" allowfullscreen webkitallowfullscreen mozallowfullscreen></iframe></div></div>




```python
#Using polygon markers with colors instead of default markers
polygon_map = folium.Map(location = boulder_coords, zoom_start = 13)

CU_Boulder_coords = [40.007153, -105.266930]
East_Campus_coords = [40.013501, -105.251889]
SEEC_coords = [40.009837, -105.241905]

#Add markers to the map
folium.RegularPolygonMarker(CU_Boulder_coords, popup = 'CU Boulder', fill_color = '#00ff40',
                            number_of_sides = 3, radius = 10).add_to(polygon_map)
folium.RegularPolygonMarker(East_Campus_coords, popup = 'EastCampus', fill_color = '#bf00ff',
                            number_of_sides = 5, radius = 10).add_to(polygon_map)
folium.RegularPolygonMarker(SEEC_coords, popup = 'SEEC Building', fill_color = '#ff0000',
                           number_of_sides = 8, radius = 10).add_to(polygon_map)

#Display the map
polygon_map
```




<div style="width:100%;"><div style="position:relative;width:100%;height:0;padding-bottom:60%;"><iframe src="data:text/html;charset=utf-8;base64,PCFET0NUWVBFIGh0bWw+CjxoZWFkPiAgICAKICAgIDxtZXRhIGh0dHAtZXF1aXY9ImNvbnRlbnQtdHlwZSIgY29udGVudD0idGV4dC9odG1sOyBjaGFyc2V0PVVURi04IiAvPgogICAgPHNjcmlwdD5MX1BSRUZFUl9DQU5WQVM9ZmFsc2U7IExfTk9fVE9VQ0g9ZmFsc2U7IExfRElTQUJMRV8zRD1mYWxzZTs8L3NjcmlwdD4KICAgIDxzY3JpcHQgc3JjPSJodHRwczovL2Nkbi5qc2RlbGl2ci5uZXQvbnBtL2xlYWZsZXRAMS4yLjAvZGlzdC9sZWFmbGV0LmpzIj48L3NjcmlwdD4KICAgIDxzY3JpcHQgc3JjPSJodHRwczovL2FqYXguZ29vZ2xlYXBpcy5jb20vYWpheC9saWJzL2pxdWVyeS8xLjExLjEvanF1ZXJ5Lm1pbi5qcyI+PC9zY3JpcHQ+CiAgICA8c2NyaXB0IHNyYz0iaHR0cHM6Ly9tYXhjZG4uYm9vdHN0cmFwY2RuLmNvbS9ib290c3RyYXAvMy4yLjAvanMvYm9vdHN0cmFwLm1pbi5qcyI+PC9zY3JpcHQ+CiAgICA8c2NyaXB0IHNyYz0iaHR0cHM6Ly9jZG5qcy5jbG91ZGZsYXJlLmNvbS9hamF4L2xpYnMvTGVhZmxldC5hd2Vzb21lLW1hcmtlcnMvMi4wLjIvbGVhZmxldC5hd2Vzb21lLW1hcmtlcnMuanMiPjwvc2NyaXB0PgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL2Nkbi5qc2RlbGl2ci5uZXQvbnBtL2xlYWZsZXRAMS4yLjAvZGlzdC9sZWFmbGV0LmNzcyIvPgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL21heGNkbi5ib290c3RyYXBjZG4uY29tL2Jvb3RzdHJhcC8zLjIuMC9jc3MvYm9vdHN0cmFwLm1pbi5jc3MiLz4KICAgIDxsaW5rIHJlbD0ic3R5bGVzaGVldCIgaHJlZj0iaHR0cHM6Ly9tYXhjZG4uYm9vdHN0cmFwY2RuLmNvbS9ib290c3RyYXAvMy4yLjAvY3NzL2Jvb3RzdHJhcC10aGVtZS5taW4uY3NzIi8+CiAgICA8bGluayByZWw9InN0eWxlc2hlZXQiIGhyZWY9Imh0dHBzOi8vbWF4Y2RuLmJvb3RzdHJhcGNkbi5jb20vZm9udC1hd2Vzb21lLzQuNi4zL2Nzcy9mb250LWF3ZXNvbWUubWluLmNzcyIvPgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL2NkbmpzLmNsb3VkZmxhcmUuY29tL2FqYXgvbGlicy9MZWFmbGV0LmF3ZXNvbWUtbWFya2Vycy8yLjAuMi9sZWFmbGV0LmF3ZXNvbWUtbWFya2Vycy5jc3MiLz4KICAgIDxsaW5rIHJlbD0ic3R5bGVzaGVldCIgaHJlZj0iaHR0cHM6Ly9yYXdnaXQuY29tL3B5dGhvbi12aXN1YWxpemF0aW9uL2ZvbGl1bS9tYXN0ZXIvZm9saXVtL3RlbXBsYXRlcy9sZWFmbGV0LmF3ZXNvbWUucm90YXRlLmNzcyIvPgogICAgPHN0eWxlPmh0bWwsIGJvZHkge3dpZHRoOiAxMDAlO2hlaWdodDogMTAwJTttYXJnaW46IDA7cGFkZGluZzogMDt9PC9zdHlsZT4KICAgIDxzdHlsZT4jbWFwIHtwb3NpdGlvbjphYnNvbHV0ZTt0b3A6MDtib3R0b206MDtyaWdodDowO2xlZnQ6MDt9PC9zdHlsZT4KICAgIAogICAgPHN0eWxlPiNtYXBfOTQ1NDcyYWQxM2M4NGM5NmIzNzkxODYyY2QwNWUwNWIgewogICAgICAgIHBvc2l0aW9uOiByZWxhdGl2ZTsKICAgICAgICB3aWR0aDogMTAwLjAlOwogICAgICAgIGhlaWdodDogMTAwLjAlOwogICAgICAgIGxlZnQ6IDAuMCU7CiAgICAgICAgdG9wOiAwLjAlOwogICAgICAgIH0KICAgIDwvc3R5bGU+CiAgICA8c2NyaXB0IHNyYz0iaHR0cHM6Ly9jZG5qcy5jbG91ZGZsYXJlLmNvbS9hamF4L2xpYnMvbGVhZmxldC1kdmYvMC4zLjAvbGVhZmxldC1kdmYubWFya2Vycy5taW4uanMiPjwvc2NyaXB0Pgo8L2hlYWQ+Cjxib2R5PiAgICAKICAgIAogICAgPGRpdiBjbGFzcz0iZm9saXVtLW1hcCIgaWQ9Im1hcF85NDU0NzJhZDEzYzg0Yzk2YjM3OTE4NjJjZDA1ZTA1YiIgPjwvZGl2Pgo8L2JvZHk+CjxzY3JpcHQ+ICAgIAogICAgCiAgICAKICAgICAgICB2YXIgYm91bmRzID0gbnVsbDsKICAgIAoKICAgIHZhciBtYXBfOTQ1NDcyYWQxM2M4NGM5NmIzNzkxODYyY2QwNWUwNWIgPSBMLm1hcCgKICAgICAgICAnbWFwXzk0NTQ3MmFkMTNjODRjOTZiMzc5MTg2MmNkMDVlMDViJywgewogICAgICAgIGNlbnRlcjogWzQwLjAxNSwgLTEwNS4yNzA1XSwKICAgICAgICB6b29tOiAxMywKICAgICAgICBtYXhCb3VuZHM6IGJvdW5kcywKICAgICAgICBsYXllcnM6IFtdLAogICAgICAgIHdvcmxkQ29weUp1bXA6IGZhbHNlLAogICAgICAgIGNyczogTC5DUlMuRVBTRzM4NTcsCiAgICAgICAgem9vbUNvbnRyb2w6IHRydWUsCiAgICAgICAgfSk7CgogICAgCiAgICAKICAgIHZhciB0aWxlX2xheWVyX2U1Y2ZlYTliYTBjZTQ1MDBiYmViYTRhOGQxMWYyNjY0ID0gTC50aWxlTGF5ZXIoCiAgICAgICAgJ2h0dHBzOi8ve3N9LnRpbGUub3BlbnN0cmVldG1hcC5vcmcve3p9L3t4fS97eX0ucG5nJywKICAgICAgICB7CiAgICAgICAgImF0dHJpYnV0aW9uIjogbnVsbCwKICAgICAgICAiZGV0ZWN0UmV0aW5hIjogZmFsc2UsCiAgICAgICAgIm1heE5hdGl2ZVpvb20iOiAxOCwKICAgICAgICAibWF4Wm9vbSI6IDE4LAogICAgICAgICJtaW5ab29tIjogMCwKICAgICAgICAibm9XcmFwIjogZmFsc2UsCiAgICAgICAgInN1YmRvbWFpbnMiOiAiYWJjIgp9KS5hZGRUbyhtYXBfOTQ1NDcyYWQxM2M4NGM5NmIzNzkxODYyY2QwNWUwNWIpOwogICAgCiAgICAgICAgICAgIHZhciByZWd1bGFyX3BvbHlnb25fbWFya2VyXzM4ZjNmMmYwMmE3NDRmYjI5OGMwN2NjNGE1ZTAwNDIxID0gbmV3IEwuUmVndWxhclBvbHlnb25NYXJrZXIoCiAgICAgICAgICAgICAgICBuZXcgTC5MYXRMbmcoNDAuMDA3MTUzLC0xMDUuMjY2OTMpLAogICAgICAgICAgICAgICAgewogICAgICAgICAgICAgICAgICAgIGljb24gOiBuZXcgTC5JY29uLkRlZmF1bHQoKSwKICAgICAgICAgICAgICAgICAgICBjb2xvcjogJ2JsYWNrJywKICAgICAgICAgICAgICAgICAgICBvcGFjaXR5OiAxLAogICAgICAgICAgICAgICAgICAgIHdlaWdodDogMiwKICAgICAgICAgICAgICAgICAgICBmaWxsQ29sb3I6ICcjMDBmZjQwJywKICAgICAgICAgICAgICAgICAgICBmaWxsT3BhY2l0eTogMSwKICAgICAgICAgICAgICAgICAgICBudW1iZXJPZlNpZGVzOiAzLAogICAgICAgICAgICAgICAgICAgIHJvdGF0aW9uOiAwLAogICAgICAgICAgICAgICAgICAgIHJhZGl1czogMTAKICAgICAgICAgICAgICAgICAgICB9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF85NDU0NzJhZDEzYzg0Yzk2YjM3OTE4NjJjZDA1ZTA1Yik7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF84NDE0ODdiMjMwOGE0MTJlODI2NDMyYTAyYzI0MmU5ZiA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJwogICAgICAgICAgICAKICAgICAgICAgICAgfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzJkZmY3MTY2MzJkODQzOTdhN2VjYjdiNTYxM2YyYTZhID0gJCgnPGRpdiBpZD0iaHRtbF8yZGZmNzE2NjMyZDg0Mzk3YTdlY2I3YjU2MTNmMmE2YSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+Q1UgQm91bGRlcjwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfODQxNDg3YjIzMDhhNDEyZTgyNjQzMmEwMmMyNDJlOWYuc2V0Q29udGVudChodG1sXzJkZmY3MTY2MzJkODQzOTdhN2VjYjdiNTYxM2YyYTZhKTsKICAgICAgICAgICAgCgogICAgICAgICAgICByZWd1bGFyX3BvbHlnb25fbWFya2VyXzM4ZjNmMmYwMmE3NDRmYjI5OGMwN2NjNGE1ZTAwNDIxLmJpbmRQb3B1cChwb3B1cF84NDE0ODdiMjMwOGE0MTJlODI2NDMyYTAyYzI0MmU5ZikKICAgICAgICAgICAgOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHJlZ3VsYXJfcG9seWdvbl9tYXJrZXJfNjk4YTlmYWExZjQxNDZlZWE0Yzk2MjAyY2I4Nzc5ZmMgPSBuZXcgTC5SZWd1bGFyUG9seWdvbk1hcmtlcigKICAgICAgICAgICAgICAgIG5ldyBMLkxhdExuZyg0MC4wMTM1MDEsLTEwNS4yNTE4ODkpLAogICAgICAgICAgICAgICAgewogICAgICAgICAgICAgICAgICAgIGljb24gOiBuZXcgTC5JY29uLkRlZmF1bHQoKSwKICAgICAgICAgICAgICAgICAgICBjb2xvcjogJ2JsYWNrJywKICAgICAgICAgICAgICAgICAgICBvcGFjaXR5OiAxLAogICAgICAgICAgICAgICAgICAgIHdlaWdodDogMiwKICAgICAgICAgICAgICAgICAgICBmaWxsQ29sb3I6ICcjYmYwMGZmJywKICAgICAgICAgICAgICAgICAgICBmaWxsT3BhY2l0eTogMSwKICAgICAgICAgICAgICAgICAgICBudW1iZXJPZlNpZGVzOiA1LAogICAgICAgICAgICAgICAgICAgIHJvdGF0aW9uOiAwLAogICAgICAgICAgICAgICAgICAgIHJhZGl1czogMTAKICAgICAgICAgICAgICAgICAgICB9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF85NDU0NzJhZDEzYzg0Yzk2YjM3OTE4NjJjZDA1ZTA1Yik7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8xNjc4ODJjZWI5Nzk0M2I2OTIzMGJkMmMzM2IxNTIwMSA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJwogICAgICAgICAgICAKICAgICAgICAgICAgfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzIzOTVmMWM2YzQzZTQxZDk5ODk2ZjcyMTA2OWY5OTBmID0gJCgnPGRpdiBpZD0iaHRtbF8yMzk1ZjFjNmM0M2U0MWQ5OTg5NmY3MjEwNjlmOTkwZiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+RWFzdENhbXB1czwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfMTY3ODgyY2ViOTc5NDNiNjkyMzBiZDJjMzNiMTUyMDEuc2V0Q29udGVudChodG1sXzIzOTVmMWM2YzQzZTQxZDk5ODk2ZjcyMTA2OWY5OTBmKTsKICAgICAgICAgICAgCgogICAgICAgICAgICByZWd1bGFyX3BvbHlnb25fbWFya2VyXzY5OGE5ZmFhMWY0MTQ2ZWVhNGM5NjIwMmNiODc3OWZjLmJpbmRQb3B1cChwb3B1cF8xNjc4ODJjZWI5Nzk0M2I2OTIzMGJkMmMzM2IxNTIwMSkKICAgICAgICAgICAgOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHJlZ3VsYXJfcG9seWdvbl9tYXJrZXJfNWZlMjE5ZDU0ZGYzNGEwNjk1MzM5NTFmMTBhNTUyZTUgPSBuZXcgTC5SZWd1bGFyUG9seWdvbk1hcmtlcigKICAgICAgICAgICAgICAgIG5ldyBMLkxhdExuZyg0MC4wMDk4MzcsLTEwNS4yNDE5MDUpLAogICAgICAgICAgICAgICAgewogICAgICAgICAgICAgICAgICAgIGljb24gOiBuZXcgTC5JY29uLkRlZmF1bHQoKSwKICAgICAgICAgICAgICAgICAgICBjb2xvcjogJ2JsYWNrJywKICAgICAgICAgICAgICAgICAgICBvcGFjaXR5OiAxLAogICAgICAgICAgICAgICAgICAgIHdlaWdodDogMiwKICAgICAgICAgICAgICAgICAgICBmaWxsQ29sb3I6ICcjZmYwMDAwJywKICAgICAgICAgICAgICAgICAgICBmaWxsT3BhY2l0eTogMSwKICAgICAgICAgICAgICAgICAgICBudW1iZXJPZlNpZGVzOiA4LAogICAgICAgICAgICAgICAgICAgIHJvdGF0aW9uOiAwLAogICAgICAgICAgICAgICAgICAgIHJhZGl1czogMTAKICAgICAgICAgICAgICAgICAgICB9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF85NDU0NzJhZDEzYzg0Yzk2YjM3OTE4NjJjZDA1ZTA1Yik7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8yZTVmOTdkZTA0MTg0YmFjOGU4OWUyYjhlNThiYjk0MyA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJwogICAgICAgICAgICAKICAgICAgICAgICAgfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2IxMjYwNzJlYmY5MzQzNzliYmVhMmEwYmI2NWIyYmFmID0gJCgnPGRpdiBpZD0iaHRtbF9iMTI2MDcyZWJmOTM0Mzc5YmJlYTJhMGJiNjViMmJhZiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+U0VFQyBCdWlsZGluZzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfMmU1Zjk3ZGUwNDE4NGJhYzhlODllMmI4ZTU4YmI5NDMuc2V0Q29udGVudChodG1sX2IxMjYwNzJlYmY5MzQzNzliYmVhMmEwYmI2NWIyYmFmKTsKICAgICAgICAgICAgCgogICAgICAgICAgICByZWd1bGFyX3BvbHlnb25fbWFya2VyXzVmZTIxOWQ1NGRmMzRhMDY5NTMzOTUxZjEwYTU1MmU1LmJpbmRQb3B1cChwb3B1cF8yZTVmOTdkZTA0MTg0YmFjOGU4OWUyYjhlNThiYjk0MykKICAgICAgICAgICAgOwoKICAgICAgICAgICAgCiAgICAgICAgCjwvc2NyaXB0Pg==" style="position:absolute;width:100%;height:100%;left:0;top:0;border:none !important;" allowfullscreen webkitallowfullscreen mozallowfullscreen></iframe></div></div>


