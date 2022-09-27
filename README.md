# API Detektia

An API to serve Detektia data, available at https://34.65.185.118

Go to https://34.65.185.118/docs in your browser to see the API documentation. <span style="color:salmon">**(currently not working)**</span>


# Using the API

In this document it is shown how to use the API through the terminal, by using the *curl* command.

Please, contact us at info@detektia.com in order to get the `login credentials` of a fully working example.


## Login

The first thing to do is to log in. The API uses OAuth2 with bearer token, so you have to retrieve a token and use it when calling the API methods.

To get the token, use the https://34.65.185.118/auth/token endpoint. Both `username` and `password` have to be provided.

```python
curl -k -X POST https://34.65.185.118/auth/token -H 'accept: application/json' -H 'Content-Type: application/x-www-form-urlencoded' -d 'grant_type=&username=username&password=password&scope=&client_id=&client_secret='
```

The response will contain the token, e.g.:

```python
{
"access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJhZG1pbiIsImV4cCI6MTYzMDA2MDgzOH0.F-Cme8voQHu_UkHsZjD_H7g_asa5ewilv69o5H0Yc-w",
  "token_type": "Bearer"
}
```

In what follows, the token have to be replaced with your retrived `access_token` value.


## Get metadata

The following endpoints retrive the metadata of the datasets.


### Point datasets

Endpoint: https://34.65.185.118/users/username/datasets

```python
curl -k -X GET https://34.65.185.118/users/demo_genoa/datasets -H 'accept: application/json' -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJhZG1pbiIsImV4cCI6MTYzMDA2MDgzOH0.F-Cme8voQHu_UkHsZjD_H7g_asa5ewilv69o5H0Yc-w"
```


This endpoint retrive a list of jsons containing the metadata of all the point datasets available in the database, e.g.:

```json
[
  {
    "scenario":"Genoa",
    "datasetname":"genoa_20170124",
    "username":"demo_genoa",
    "satellite":"sen",
    "orbit":"26",
    "swath":"IW2",
    "geometry":"ASCENDING",
    "inc_angle":41.76348969125569,
    "heading_angle":-167.9403911657992,
    "user_boundary":{
      "coordinates":[
        [[8.950002,44.39],
        [8.950002,44.42],
        [8.99,44.42],
        [8.99,44.39],
        [8.950002,44.39]]
      ],
      "type":"Polygon"
    },
    "count":20263,
    "dates":[
      736177,
      736285,
      ...,
      736717
    ],
    "d_min": 61,
    "d_max": -24,
    "info": {
      "optional_test_filed": "test_body",
      ...,
    },
    "dataset_type":"points"
  },
  {
    "project":"Genoa",
    "datasetname":"genoa_20181209",
    ...
  }
]
```

In this example, there are two point datasets `genoa_20170124` and `genoa_20181209`, both belonging to the project `Genoa`.
The *dates* are expressed as the number of days from the 1st of January, 1 B.C.


In order to get the metadata of a particular point dataset, in a json format, the endpoint is: https://34.65.185.118/users/username/datasets/datasetname/metadata

```python
curl -k -X GET https://34.65.185.118/users/demo_genoa/datasets/genoa_20170124/metadata -H 'accept: application/json' -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJhZG1pbiIsImV4cCI6MTYzMDA2MDgzOH0.F-Cme8voQHu_UkHsZjD_H7g_asa5ewilv69o5H0Yc-w"
```



### Polygon datasets
Endpoint: https://34.65.185.118/users/username/polygons

```python
curl -k -X GET https://34.65.185.118/users/demo_genoa/polygons -H 'accept: application/json' -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJhZG1pbiIsImV4cCI6MTYzMDA2MDgzOH0.F-Cme8voQHu_UkHsZjD_H7g_asa5ewilv69o5H0Yc-w"
```

The response is a list of jsons containing the polygons metadata, e.g.:

```json
[
  {
    "scenario":"Genoa",
    "datasetname":"bhi_genova",
    "username":"demo_genoa",
    "bbox":{
      "coordinates":
        [[[8.953373,44.3996843],
          [8.953373,44.4177737],
          [8.9875057,44.4177737],
          [8.9875057,44.3996843],
          [8.953373,44.3996843]]],
      "type":"Polygon"},
    "count":1511,
    "properties": [
      "velmax_pte",
      "velsd_pte",
      "velmax_normal",
      "velsd_normal"
    ],
    "info": {
      "properties_info": {
        "velmax_pte": "test",
        "velsd_pte": "test",
        "velmax_normal": "test",
        "velsd_normal": "test"
      },
      "optional_test_filed": "test_filed",
      ...,
    },
    "dataset_type": "polygons"
  },
  ...
]
```

In this example there is only one polygon dataset in the database, `bhi_geonva`, which belongs to the same project as the point datasets, `Genoa`.

In order to get the metadata of a particular polygon dataset, in a json format, the endpoint is: https://34.65.185.118/users/username/polygons/datasetname/metadata

```python
curl -k -X GET https://34.65.185.118/users/demo_genova/polygons/bhi_genoa/metadata -H 'accept: application/json' -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJhZG1pbiIsImV4cCI6MTYzMDA2MDgzOH0.F-Cme8voQHu_UkHsZjD_H7g_asa5ewilv69o5H0Yc-w"
```


### Extended values 
endpoint: https://34.65.185.118/users/username/extended

```python
curl -k -X GET https://34.65.185.118/users/demo_genoa/extended -H 'accept: application/json' -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJhZG1pbiIsImV4cCI6MTYzMDA2MDgzOH0.F-Cme8voQHu_UkHsZjD_H7g_asa5ewilv69o5H0Yc-w"
```

The response body is a list of jsons containing the extended metadata, e.g.:

```python
[
  {
    "scenario": "Genoa",
    "datasetname": "genoa_20170124"
    "key": "warnings",
    "type": "categoric",
    "units": "None",
    "info": {"test_filed": "test_body"}
  },
  {
    "scenario": "Genoa",
    "datasetname": "genoa_20170124"
    "key": "clusters_2",
    "type": "categoric",
    "units": "None",
    "info": {"test_filed": "test_body"}
  },
  {
    "scenario": "Genoa",
    "datasetname": "genoa_20181209"
    ...
  }
  ...
]
```

The endpoint for getting the metadata of a particular dataset is
https://34.65.185.118/users/username/extended/datasetname/metadata

```python
curl -k -X GET https://34.65.185.118/users/demo_genoa/extended/genoa_20170124/metadata -H 'accept: application/json' -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJhZG1pbiIsImV4cCI6MTYzMDA2MDgzOH0.F-Cme8voQHu_UkHsZjD_H7g_asa5ewilv69o5H0Yc-w"
```


## Get data
The following endpoints retrive the data of the datasets.


### Polygon dataset
Endpoint: https://34.65.185.118/users/username/polygons/datasetname 

```python
curl -k -X GET https://34.65.185.118/users/demo_genoa/polygons/bhi_genova -H 'accept: application/json' -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJhZG1pbiIsImV4cCI6MTYzMDA2MDgzOH0.F-Cme8voQHu_UkHsZjD_H7g_asa5ewilv69o5H0Yc-w"
```

This endpoint retrives a *FeatureCollection* with all the polygons in the dataset, e.g.:

```json
{
  "type": "FeatureCollection",
  "features": [
    {
      "type": "Feature",
      "geometry": {
        "coordinates": 
          [[[[8.9747056, 44.4095969],
            [8.9747134, 44.4096336],
            ...
          ]],
        "type": "Polygon"
      },
      "properties": {
        "id": 1,
        "velmax1": 4.31,
        "velmax2": 7.83,
        "ac": 2.31,
        ...
      }
    },
    {
    ...
    },
    ...
  ]
}
```

The response contains the id, coordinates and all the loaded indices (properties: `velmax1`, `velmax2`, `ac`...) of each polygon.


### Point dataset
Endpoint: https://34.65.185.118/users/username/datasets/datasetname 

This endpoint retrives the velocity and time series of all the dataset points. This endpoint can have an optional `request body`:

- `bbox` *xmin,xmax,ymin,ymax* (floats) or `polygon` *{"coordinates": [[[, ],[, ],[, ],...]], "type": "Polygon"}* (geojson): limit the points to be retrived, to the ones contained inside them. Only onw of the two options *bbox* or *polygon* can be passed. In order to get the whole area, non of these argumnets shall be passed. 
- `date_spam`, start_date and end_date (integers): delimit the time series to be retrieved between the start and end dates. If no *date_span* argument is passed, the response only contains point velocities. Thus, in order to get the complete time series, the start and end dates have to be the first and last dates, which are given in the metadata.

Here, as an example, a query with all the arguments is shown:


```python
curl -k -X GET 'https://34.65.185.118/users/demo_genoa/datasets/genoa_20181209?date_span=736177&date_span=737413&polygon=%7B%22coordinates%22%3A+%5B%5B%5B8.973958%2C+44.409764%5D%2C+%5B8.975782%2C+44.409579%5D%2C+%5B8.975424%2C+44.409205%5D%2C+%5B8.974227%2C+44.409236%5D%2C+%5B8.973958%2C+44.409764%5D%5D%5D%2C+%22type%22%3A+%22Polygon%22%7D' -H 'accept: application/json' -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJhZG1pbiIsImV4cCI6MTYzMDA2MDgzOH0.F-Cme8voQHu_UkHsZjD_H7g_asa5ewilv69o5H0Yc-w"
```

**Note**, in this example the *polygon* json has been URL-encoded. Its original form is the following:

```json
polygon={"coordinates": [[[8.973958, 44.409764], [8.975782, 44.409579], [8.975424, 44.409205], [8.974227, 44.409236], [8.973958, 44.409764]]], "type": "Polygon"}
```

The response body is

```json
{
  "type":"FeatureCollection",
  "features":[
    {
      "type":"Feature",
      "geometry":{
        "coordinates":[8.974203,44.40936],
        "type":"Point"
      },
      "properties":{
        "id":105009,
        "vel":-5.4,
        "736177":8.0,
        "736285":7.2,
        "736309":6.6,
        "736357":6.5,
        ...
      },
      "id":"105009",
      "bbox":null
    },
    { 
      "type":"Feature",
      "geometry":{
        "coordinates":[8.975165,44.40924],
        "type":"Point"
      },
      "properties":{
        "id":105421,
        "vel":-4.2,
        "736177":5.0,
        "736285":5.0,
        "736309":4.6,
        "736357":4.7,
      },
      "id":"105419",
      "bbox":null
    },
    ...
  ],
  "bbox":null
}
```

### Point details

Apart form the velocity and the time series data, each point can also have loaded values such as indices or labels. These values are called *extended values*. This in formation can be retreived only for a given set of points, identified by their id, with the endpoint 
https://34.65.185.118/users/username/datasets/datasetname/details

This endpoint needs a request body in which the point ids are specified. In these example let's take the two first point ids of the previous response body: `105009` and `105421`.

```python
curl -k -X GET 'https://34.65.185.118/users/demo_genoa/datasets/genoa_20181209/details?ids=105009&ids=105421' -H 'accetp:application/json' -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJhZG1pbiIsImV4cCI6MTYzMDA2MDgzOH0.F-Cme8voQHu_UkHsZjD_H7g_asa5ewilv69o5H0Yc-w"
```

The response body is a feature collection with a list of jsons containing all the information of the requested points.

```json
{
  "type":"FeatureCollection",
  "features":[
    {
      "type":"Feature",
      "geometry":{
        "coordinates":[8.957126,44.41489],"type":"Point"},
      "properties":{
        "id":91173,
        "vel":-0.8,
        "736177":4.3,
        "736285":2,
        "736309":1.4,
        "736357":0.8,
        ...,
        "clusters_2":2,
        "clusters_4":2,
        "warnings":20171120,
        ...
      },
      "id":"105421",
      "bbox":null
    },
    {
      "type":"Feature",
      "geometry":{
        "coordinates":[8.974203,44.40936],
        "type":"Point"
      },
      "properties":{
        "id":105009,
        "vel":-5.4,
        "736177":8,
        "736285":7.2,
        "736309":6.6,
        "736357":6.5,
        ...,
        "clusters_2":1,
        "clusters_4":4,
        "warnings":20180904,
        ...
      },
      "id":"105009",
      "bbox":null
    }
  ],
  "bbox":null
}

```

As can be seen, not only velocity and all the dates are retrieved, but also the lables of two different clusterizations and the date of a warning.


### Extended values 

The extended values can be access for all the points at once indicating its key. This can be done with the endpoint https://34.65.185.118/users/username/datasets/datasetname/extended and the `request body` *key*=extended_key.

Let's take the *warnings* key for this example.

```python
curl -k -X GET https://34.65.185.118/users/demo_genoa/datasets/genoa_20181209/extended?key=warnings -H 'accetp:application/json' -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJhZG1pbiIsImV4cCI6MTYzMDA2MDgzOH0.F-Cme8voQHu_UkHsZjD_H7g_asa5ewilv69o5H0Yc-w"
```

The response body is a json containing the point ids along with their associated extended value.

```python
{
  "warning": {
    "1": "0",
    "2": "0",
    "3": "0",
    "4": "1",
    "5": "0",
    "6": "0",
    ...,
  }
}
```
