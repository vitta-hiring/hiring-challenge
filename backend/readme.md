|[back](https://github.com/vitta-health/hiring-challenge)|
|:---:|

# Back-End Challenge

*Go as far as you can following step-by-step.*

We have a really [special map](https://en.wikipedia.org/wiki/Squaring_the_square) that is a square shape of squares.

<img src="map.png" width="416"/>

Each one of those squares we will consider a territory. This pic above shows a special setup of the minimum quantity of non-repeatable-squared squares.

Each territory is divided on 1x1 squares, and they are all paintable.

We are going to build the needed infrastructure and services, come with us!

Do not miss any point, read all content of this file!



---

| Infrastructure Update: *Docker, for real* |
| :---: |
| This challenge was created on 2017, if you still dont use docker for prototyping, some habbits needs to change! |
| Read the docs about `docker` and `docker-compose`. |
| Make a `docker-compose.yml` and add the services you need. Probably you will need at least 2 of them: a server and a database. |

---



## 1. Build the territories API

To start our project we need an API!

### To add a territory

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

And we will get the response

```json
{
  "data": {
    "id": 1,
    "name": "A",
    "start": { "x": 0, "y": 0 },
    "end": { "x": 50, "y": 50 },
    "area": 2500,
    "painted_area": 0
  },
  "error": false
}
```

But if it occours to already exists another territory overlapping this new territory area, we cannot allow that to happen!

We can get the errors:

- `territories/territory-overlay`: this new territory overlays another territory.
- `territories/incomplete-data`: if it misses the `start`, `end` or `name` fields!

### To return the list of all territories

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

### To remove a territory

```
DELETE /territories/:id
```

And we will get the response

```json
{
  "error": false
}
```

We can get the error:

- `territories/not-found`: this territory was not found.

### Get data from a single territory

```
GET /territories/:id
```

And we will get the response

```json
{
  "data": {
    "id": 1,
    "name": "A",
    "start": { "x": 0, "y": 0 },
    "end": { "x": 50, "y": 50 },
    "area": 2500,
    "painted_area": 0
  },
  "error": false
}
```

We can get the error:

- `territories/not-found`: this territory was not found.

---

| Infrastructure Update: *Lets secure a bit* |
| :---: |
| Create a reverse-proxy service to separate de requests from the server. Make it listen to the port `80` and your application at `8888`. 
| Take a chance to read and implement stuff like those written here: https://poweruphosting.com/blog/secure-nginx-server/ |
| We recomend `nginx` or `haproxy` for this step. Do not forget to keep their config file on your repository somehow. |

---



## 2. Make it possible to paint a territory

Here comes the best part!

#### Get status of a square

```
GET /squares/:x/:y
```

And we will get the response

```json
{
  "data": {
    "x": 1,
    "y": 2,
    "painted": false  
  },
  "error": false
}
```

We can get the errors:

- `squares/not-found`: this square does not belongs to any territory.

### Throw paint on it!

```
PATCH /squares/:x/:y/paint
```

And we will get the response

```json
{
  "data": {
    "x": 1,
    "y": 2,
    "painted": true
  },
  "error": false
}
```

We can get the errors:

- `squares/not-found`: this square does not belongs to any territory.

### List all painted squares of a territory 

```
GET /territory/:id ? withpainted={false|true}
```

And we will get the response

```json
{
  "data": {
    "id": 1,
    "name": "A",
    "start": { "x": 0, "y": 0 },
    "end": { "x": 50, "y": 50 },
    "area": 2500,
    "painted_area": 2,
    "painted_squares": [
      { "x": 1, "y": 2 },
      { "x": 2, "y": 3 }
    ]
  },
  "error": false
}
```

We can get the error:

- `territories/not-found`: this territory was not found.



---

| Infrastructure Update: *Load balance that server sir!* |
| :---: |
| Comment the proxy-reverse solution and lets build an even more interesting infrastructure.  |
| Add a service to be our load balancer, we normally use `nginx` or `haproxy`! Duplicate the server service in our `docker-compose.yml`, assign them different ips and list them on your load balancer's config file! |
| Take a chance to read http://nginx.org/en/docs/http/load_balancing.html |

---



## 3. We need a dashboard

---

**Important:** Regardless the infrastructure you create, your solution must answer all of the questions below by command line, a special route or using the following proposal.

---

Now are are going to make ow solution interact with us!

Make your application responde to the route `/dashboard` an HTML that contains:

- List of territories ordered by **most painted area**;
- List of last 5 painted squares;
- List of last 5 errors;
- % of whole map painted;
