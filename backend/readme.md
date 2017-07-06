|[back](https://github.com/vitta-health/hiring-challenge)|
|:---:|

## Back-End Challenge

We have a really [special map](https://en.wikipedia.org/wiki/Squaring_the_square) that is a square shape of squares.

<img src="map.png" width="416"/>

Each one of those squares we will consider a territory. This pic above shows a special setup of the minimum quantity of non-repeatable-squared squares.

Each territory is divided on 1x1 squares, and they are all paintable.

### 1. Build the territories API

#### To add a territory

You may send:

```json
{
  "name": "A",
  "start": { "x": 0, "y": 0 },
  "end": { "x": 50, "y": 50 }
}
```

To this route:

```
POST /territories
```

#### To return the list of all territories

```
GET /territories
```

And we will get the response

```json
{
  "count": 1,
  "data": [
    {
      "id": 1,
      "name": "A",
      "start": { "x": 0, "y": 0 },
      "end": { "x": 50, "y": 50 },
      "area": 2500,
      "painted_area": 2
    }
  ]
}
```

#### To remove a territory

```
DELETE /territories/:id
```

#### Get data from a territory

```
GET /territories/:id
```

And we will get the response

```json
{
  "id": 1,
  "name": "A",
  "start": { "x": 0, "y": 0 },
  "end": { "x": 50, "y": 50 },
  "number_painted_squares": 2,
  "painted_squares": [
    { "x": 1, "y": 2 },
    { "x": 2, "y": 2 }
  ]
}
```

### 2. Make it possible to paint a territory

#### Get status of a square

```
GET /square/:x/:y
```

And we will get the response

```json
{
  "x": 1,
  "y": 2,
  "painted": true
}
```

#### Throw paint on it!

```
PATCH /square/:x/:y/paint
```