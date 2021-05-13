### API endpoints
There are four endpoints provided by the [json-server](https://github.com/typicode/json-server). As per [json-server](https://github.com/typicode/json-server) documentation:
all of endpoints support `GET`, `POST`, `PUT` and `DELETE` so be careful!
The API generates an in memory JSON-database on runtime that contains 1000 products, 100 users and their carts by default. That
means restarting will recreate the database. There is no authentication, so you can use any user id, or ignore the endpoint
altogether.

Endpoints:
- `/recommendeds` is a utility endpoint to get the first 10 products
- `/products`
- `/users`
- `/carts`

[json-server](https://github.com/typicode/json-server) cheatsheet:
- Search with `GET /products?q={keyword}`
- Paginate with `GET /products?_page={page_number}&_limit={number_of_entries}`
- Get specific product `GET /products/{product_id}`
- Get specific user `GET /users/{user_id}`
- Get specific cart `GET /carts/{user_id}`
- Whole database is viewable with `GET /db`
- More info if needed on [json-server github page](https://github.com/typicode/json-server)

Expect objects to look something like this:

```typescript
type Product = {
    id: number,
    name: string,
    description: string,
    defaultImage: string,
    images: string[],
    price: number,
    discount: number,
};

type User = {
    id: number,
    name: {
        firstName: string,
        lastName: string,
    }
    phone: string,
    avatar: string,
    email: string,
    address: {
        country: string,
        city: string,
        zip: string,
        street: string,
    },
    orders: {
        id: number,
        products: {
            id: number,
            quantity: number,
        }[],
    },
    role: 'ADMIN' | 'CUSTOMER' // Role is based on i % 2
};

type Cart = {
    id: number, // User id
    products: {
          id: number,
          quantity: number,
    }[],
}
```

### Examples:

#### Products
- `GET` `http://localhost:8080/products`
- `GET` `http://localhost:8080/products?q={keyword}`
  ```javascript
      [
          {
              "id": 1,
              "name": "Incredible Metal Sausages",
              "description": "The slim & simple Maple Gaming Keyboard from Dev Byte comes with a sleek body and 7- Color RGB LED Back-lighting for smart functionality",
              "defaultImage": "http://placeimg.com/640/480/cats", // Unfortunately faker.js doesn't support dogs...
              "images": [
                  "http://placeimg.com/640/480/cats",
                  "http://placeimg.com/640/480/cats",
                  "http://placeimg.com/640/480/cats",
                  "http://placeimg.com/640/480/cats"
              ],
              "price": 64946.54, // IKR, it's an expensive metal sausage!
              "discount": 8
          },
          ...
      ]
  ```
- `POST` `http://localhost:8080/products`
```json
    {
        "name": "Incredible Metal Sausages",
        "description": "The slim & simple Maple Gaming Keyboard from Dev Byte comes with a sleek body and 7- Color RGB LED Back-lighting for smart functionality",
        "defaultImage": "http://placeimg.com/640/480/cats",
        "images": [
            "http://placeimg.com/640/480/cats",
            "http://placeimg.com/640/480/cats",
            "http://placeimg.com/640/480/cats",
            "http://placeimg.com/640/480/cats"
        ],
        "price": 64946.54,
        "discount": 8
    }
```
- `PUT` `http://localhost:8080/products/{product_id}`
```json
    {
        "name": "Changed this",
        "description": "And this",
        "defaultImage": "http://placeimg.com/640/480/cats",
        "images": [
            "http://placeimg.com/640/480/cats",
            "http://placeimg.com/640/480/cats",
            "http://placeimg.com/640/480/cats",
            "http://placeimg.com/640/480/cats"
        ],
        "price": 1,
        "discount": 0
    }
```
#### Users
- `GET` `http://localhost:8080/users/{user_id}`
    ```javascript
    {
        "id": 1,
        "name": {
            "firstName": "Cesar",
            "lastName": "Reichel"
        },
        "phone": "1-869-324-5801 x510",
        "avatar": "https://s3.amazonaws.com/uifaces/faces/twitter/saarabpreet/128.jpg",
        "email": "Charlie.Ernser@gmail.com",
        "address": {
            "country": "Martinique",
            "city": "Stromanfurt",
            "zip": "30627",
            "street": "3607 Olson Motorway"
        },
        "role": "ADMIN",
        "orders": [
                {
                    "id": 1,
                    "products": [
                        {
                            "id": 388,
                            "quantity": 5
                        },
                        ...
                    ]
                },
            ],
    }

    ```
#### Carts
- `GET` `http://localhost:8080/carts/{user_id}`
    ```javascript
      {
          "id": 1,
          "products": [
              {
                  "id": 468,
                  "quantity": 7
              },
              ...
          ]
      }
    ```
Deployed on Heroku at https://smart-hardware-shop.herokuapp.com
