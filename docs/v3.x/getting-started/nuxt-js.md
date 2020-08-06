# Getting Started with Nuxt.js

This integration guide is following the [Getting started guide](../getting-started/quick-start.html). We assume that you have completed [Step 8](../getting-started/quick-start.html#_8-consume-the-content-type-s-api) and therefore can consume the API by browsing this [url](http://localhost:1337/restaurants).

If you haven't gone through the getting started guide, the way you request a Strapi API with [Nuxt.js](https://nuxtjs.org/) remains the same except that you will not fetch the same content.

### Create a Nuxt.js app

Create a basic Nuxt.js application with [create-nuxt-app](https://github.com/nuxt/create-nuxt-app).

:::: tabs

::: tab yarn

```bash
yarn create nuxt-app nuxtjs-app
```

:::

::: tab npx

```bash
npx create-nuxt-app nuxtjs-app
```

:::

::::

### Use an HTTP client

Many HTTP clients are available but in this documentation we'll use [Axios](https://github.com/axios/axios) and [Fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API).


:::: tabs

::: tab @nuxtjs/strapi

For this example we are using the awesome [@nuxt/strapi](https://strapi.nuxtjs.org/) module.


```bash
yarn add @nuxtjs/strapi
```

  - Add `@nuxtjs/strapi` to the module section of `nuxt.config.js` with the following settings

```js
modules: ['@nuxtjs/strapi'],
strapi: {
  entities: ['restaurants', 'categories'],
  url: 'http://localhost:1337'
},
```


:::

::: tab axios

```bash
yarn add axios
```

:::

::: tab fetch

No installation needed

:::

::::

### GET Request your collection type

Execute a GET request on the `restaurant` Collection Type in order to fetch all your restaurants.

Be sure that you activated the `find` permission for the `restaurant` Collection Type.


:::: tabs

::: tab @nuxtjs/strapi

*Request*

```js
try {
  await this.$strapi.find('restaurants')
} catch (error) {
  console.log(error);
}
```

:::

::: tab axios

*Request*

```js
axios.get('http://localhost:1337/restaurants')
  .then(response => {
    console.log(response);
  })
  .catch(error => {
    console.log(error);
  })
```
:::

::: tab fetch

*Request*

```js
fetch("http://localhost:1337/restaurants", {
  headers: {
     'Content-Type': 'application/json'
  },
}).then(response => response.json())
  .then(data => console.log(data));
  .catch(error => {
    console.error(error);
  });
```
:::

*Response*

```json
[{
  "id": 1,
  "name": "Biscotte Restaurant",
  "description": "Welcome to Biscotte restaurant! Restaurant Biscotte offers a cuisine based on fresh, quality products, often local, organic when possible, and always produced by passionate producers.",
  "created_by": {
    "id": 1,
    "firstname": "Paul",
    "lastname": "Bocuse",
    "username": null
  },
  "updated_by": {
    "id": 1,
    "firstname": "Paul",
    "lastname": "Bocuse",
    "username": null
  },
  "created_at": "2020-07-31T11:37:16.964Z",
  "updated_at": "2020-07-31T11:37:16.975Z",
  "categories": [{
    "id": 2,
    "name": "French Food",
    "created_by": 1,
    "updated_by": 1,
    "created_at": "2020-07-31T11:36:23.164Z",
    "updated_at": "2020-07-31T11:36:23.172Z"
  }]
}]
```

::::

### Example

:::: tabs

::: tab @nuxt/strapi

`./pages/index.vue`

```js
<template>
  <div class="container">
    <div v-if="error">
      {{ error }}
    </div>
    <ul v-else>
      <li v-for="restaurant in restaurants" :key="restaurant.id">
        {{ restaurant.name }}
      </li>
    </ul>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'App',
  data () {
    return {
      restaurants: [],
      error: null
    }
  },
  async mounted () {
    try {
      this.restaurants = await this.$strapi.find('restaurants')
    } catch (error) {
      this.error = error
    }
  }
}
</script>

```

:::

::: tab axios

`./pages/index.vue`

```js
<template>
  <div class="container">
    <div v-if="error">
      {{ error }}
    </div>
    <ul v-else>
      <li v-for="restaurant in restaurants" :key="restaurant.id">
        {{ restaurant.name }}
      </li>
    </ul>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'App',
  data () {
    return {
      restaurants: [],
      error: null
    }
  },
  mounted () {
    axios.get('http://localhost:1337/restaurants')
      .then(response => {
        this.restaurants = response.data;
      })
      .catch(error => {
        this.error = error;
      })
  }
}
</script>
```

:::

::: tab fetch

`./pages/index.vue`

```js
<template>
  <div class="container">
    <div v-if="error">
      {{ error }}
    </div>
    <ul v-else>
      <li v-for="restaurant in restaurants" :key="restaurant.id">
        {{ restaurant.name }}
      </li>
    </ul>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'App',
  data () {
    return {
      restaurants: [],
      error: null
    }
  },
  mounted () {
    fetch("http://localhost:1337/restaurants", {
      headers: {
         'Content-Type': 'application/json'
      },
    }).then(response => response.json())
      .then(data => {
        this.restaurants = data
      })
      .catch((error) => {
        this.error = error
      });
  }
}
</script>
```

:::

::::

### POST Request your collection type

Execute a POST request on the `restaurant` Collection Type in order to create a restaurant.

Be sure that you activated the `create` permission for the `restaurant` Collection Type and the `find` permission for the `category` Collection type.


:::: tabs

::: tab @nuxtjs/strapi

```js
try {
  await this.$strapi.create('restaurants', {
    name: 'Dolemon Sushi',
    description: 'Unmissable Japanese Sushi restaurant. The cheese and salmon makis are delicious'
  })
} catch (error) {
  this.error = error
}
```
:::

::: tab axios

*Request*

```js
axios.post('http://localhost:1337/restaurants', {
    name: 'Dolemon Sushi',
    description: 'Unmissable Japanese Sushi restaurant. The cheese and salmon makis are delicious'
  })
  .then(response => {
    console.log(response);
  })
  .catch(error => {
    console.log(error);
  });
```
:::

::: tab fetch

*Request*

```js
fetch('http://localhost:1337/restaurants', {
   method: 'POST',
   headers: {
      'Content-Type': 'application/json'
   },
   body: JSON.stringify({
     name: 'Dolemon Sushi',
     description: 'Unmissable Japanese Sushi restaurant. The cheese and salmon makis are delicious'
   })
 }).then(response => {
   return response.json();
 }).then(data => {
   console.log(data);
 }).catch(error => {
   console.error(error);
 });
```
:::

*Response*

```json
{
    "id": 2,
    "name": "Dolemon Sushi",
    "description": "Unmissable Japanese Sushi restaurant. The cheese and salmon makis are delicious",
    "created_by": null,
    "updated_by": null,
    "created_at": "2020-08-04T09:57:11.669Z",
    "updated_at": "2020-08-04T09:57:11.669Z",
    "categories": [
    ]
}
```

::::

### Example

:::: tabs

::: tab @nuxtjs/strapi

`./pages/index.vue`

```js
<template>
<div class="container">
  <div v-if="error">
    {{ error }}
  </div>

  <form id="form" v-on:submit="handleSubmit" v-else>
    <label for="name">Name</label>
    <input id="name" v-model="name" type="text" name="name">

    <label for="description">Description</label>
    <input id="description" v-model="description" type="text" name="description">

    <select v-if="this.allCategories.length > 0" id="categories" v-model="categories" name="categories">
      <option v-for="category in allCategories" :key="category.id" :value="category.id">
        {{ category.name }}
      </option>
    </select>

    <input type="submit" value="Submit">
  </form>

</div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      allCategories: [],
      name: '',
      description: '',
      categories: '',
      error: null
    }
  },
  async mounted() {
    try {
      this.allCategories = await this.$strapi.find('categories')
    } catch (error) {
      this.error = error
    }
  },
  methods: {
    handleSubmit: async function(e) {
      try {
        await this.$strapi.create('restaurants', {
          name: this.name,
          description: this.description,
          categories: this.categories
        })
      } catch (error) {
        this.error = error
      }

      e.preventDefault();
    }
  }
}
</script>
```

:::

::: tab axios

`./pages/index.vue`

```js
<template>
<div class="container">
  <div v-if="error">
    {{ error }}
  </div>

  <form id="form" v-on:submit="handleSubmit" v-else>
    <label for="name">Name</label>
    <input id="name" v-model="name" type="text" name="name">

    <label for="description">Description</label>
    <input id="description" v-model="description" type="text" name="description">

    <select v-if="this.allCategories.length > 0" id="categories" v-model="categories" name="categories">
      <option v-for="category in allCategories" :key="category.id" :value="category.id">
        {{ category.name }}
      </option>
    </select>

    <input type="submit" value="Submit">
  </form>

</div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'App',
  data() {
    return {
      allCategories: [],
      name: '',
      description: '',
      categories: '',
      error: null
    }
  },
  mounted() {
    axios.get('http://localhost:1337/categories')
      .then(response => {
        this.allCategories = response.data;
      })
      .catch(error => {
        this.error = error;
      })
  },
  methods: {
    handleSubmit: function(e) {
      axios.post('http://localhost:1337/restaurants', {
          name: this.name,
          description: this.description,
          categories: this.categories
        })
        .then(response => {
          console.log(response);
        })
        .catch(error => {
          this.error = error;
        });
      e.preventDefault();

    }
  }
}
</script>
```

:::

::: tab fetch

`./pages/index.vue`

```js
<template>
<div class="container">
  <div v-if="error">
    {{ error }}
  </div>

  <form id="form" v-on:submit="handleSubmit" v-else>
    <label for="name">Name</label>
    <input id="name" v-model="name" type="text" name="name">

    <label for="description">Description</label>
    <input id="description" v-model="description" type="text" name="description">

    <select v-if="this.allCategories.length > 0" id="categories" v-model="categories" name="categories">
      <option v-for="category in allCategories" :key="category.id" :value="category.id">
        {{ category.name }}
      </option>
    </select>

    <input type="submit" value="Submit">
  </form>

</div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      allCategories: [],
      name: '',
      description: '',
      categories: '',
      error: null
    }
  },
  mounted() {
    fetch("http://localhost:1337/categories", {
        headers: {
          'Content-Type': 'application/json'
        },
      }).then(response => response.json())
      .then(data => {
        this.allCategories = data
      })
      .catch(error => {
        this.error = error;
      });
  },
  methods: {
    handleSubmit: function(e) {
      fetch('http://localhost:1337/restaurants', {
          method: "POST",
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify({
            name: this.name,
            description: this.description,
            categories: this.categories
          })
        })
        .then(response => response.json())
        .then(data => {
          console.log(data);
        })
        .catch(error => {
          this.error = error;
        });

      e.preventDefault();
    }
  }
}
</script>
```

:::

::::

### PUT Request your collection type

Execute a PUT request on the `restaurant` Collection Type in order to update the category of a restaurant.

Be sure that you activated the `put` permission for the `restaurant` Collection Type.

:::: tabs

We consider that the id of your restaurant is `2`
and the id of your category is `3`

::: tab @nuxtjs/strapi

```js
try {
  await this.$strapi.update('restaurants', 2, {
    categories: [{
      id: 3
    }]
  })
} catch (error) {
  this.error = error
}
```

:::


::: tab axios

*Request*

```js
axios.put('http://localhost:1337/restaurants/2', {
  categories: [{
    id: 3
  }]
})
.then(response => {
  console.log(response);
})
.catch(error => {
  console.log(error);
});
```

:::

::: tab fetch

*Request*

```js
fetch('http://localhost:1337/restaurants/2', {
  method: "PUT",
  headers: {
     'Content-Type': 'application/json'
  },
  body: JSON.stringify({
    categories: [{
      id: 3
    }]
  })
})
.then(response => response.json())
.then(data => {
  console.log(data);
})
.catch((error) => {
  console.error(error);
});
```

:::

*Response*

```json
{
  "id": 2,
  "name": "Dolemon Sushi",
  "description": "Unmissable Japanese Sushi restaurant. The cheese and salmon makis are delicious",
  "created_by": null,
  "updated_by": null,
  "created_at": "2020-08-04T10:21:30.219Z",
  "updated_at": "2020-08-04T10:21:30.219Z",
  "categories": [{
    "id": 3,
    "name": "Japanese",
    "created_by": 1,
    "updated_by": 1,
    "created_at": "2020-08-04T10:24:26.901Z",
    "updated_at": "2020-08-04T10:24:26.911Z"
  }]
}
```

::::

## Conclusion

Here is how to request your Collection Types in Strapi using Nuxt.js. When you create a Collection Type or a Single Type you will have a certain number of REST API endpoints available to interact with.

We just used the GET, POST and PUT methods here but you can [get one entry](../content-api/api-endpoints.html#get-an-entry), [get how much entry you have](../content-api/api-endpoints.html#count-entries) and [delete](../content-api/api-endpoints.html#delete-an-entry) an entry too. Learn more about [API Endpoints](../content-api/api-endpoints.html#api-endpoints)