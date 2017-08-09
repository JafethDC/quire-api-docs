---
title: Quire API Reference

language_tabs:
  - shell


search: true
---

# Introduction

## About the api versioning

Currently, there is only one version for the api. Thus v1 is the one by default. However, in the future, you should specify the api version through an Accept header:

`Accept: application/vnd.quire-api.<version>`

`<version>` being v1, v2, etc.

## About the url format

There is no need to append a '.json' to the urls as the json format is set as default.

## About the authentication

> To authorize, use this code:

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: meowmeowmeow"
```

> Make sure to replace `meowmeowmeow` with the corresponding access token.

Quire uses access token to authenticate against some specific routes such as creating or destroying products. In order to obtain that access token, you should request it through the sessions endpoint.

Quire expects to receive a header with the following form:

`Authorization: meowmeowmeow`

<aside class="notice">
You must replace <code>meowmeowmeow</code> with the corresponding access token.
</aside>

# Sessions

## Create a session

```shell
curl --request POST  
  --url https://quire-api.herokuapp.com/sessions
  --header "Authorization: meowmeowmeow"
  --data '
    {
        "user": {
            "last_location": "POINT(-78.1323 -12.4141)"
        },
        "fb_access_token": "RxdEdkB7NqnU5hikPG7zkuXT"
    }
  '
```

> The above command returns JSON structured like this:

```json
{
  "id": 287,
  "email": "narenkukreja@gmail.com",
  "fb_user_id": "7582198748976",
  "name":"Naren Kukreja",
  "last_loc": {
    "lng": "-79.1323",
    "lat": "-12.4141"
  },
  "access_token": "r4Vb5oH7gDDKA47ZBnYCheVx",
  "preference_radius": 15000
}
```

This endpoint will expect to receive an access token obtained after a successful facebook log in and will generate an api access token to perform subsequent requests.

In case the specified user is not present in the database, a new one will be created with the fields obtained from the facebook account along with the ones passed inside the  `user` key.

### HTTP Request

`POST https://quire-api.herokuapp.com/sessions`

### Headers

Header | Value
------ | -----
Content-Type | application/json

### Body

Parameter | Description | Required
--------- | ----------- | --------
fb_access_token | Access token obtained after a successful facebook login | true
user.last_location | User's last location. Must follow the EWKT format : "POINT (longitude latitude)" | true

## Destroy a session

```shell
curl --request DELETE  
  --url https://quire-api.herokuapp.com/sessions
  --header "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this

```json
{
  "success": true
}
```

Destroys the corresponding session by invalidating the access token.

### HTTP Request

`DELETE https://quire-api.herokuapp.com/sessions`

### Headers

Header | Value
------ | -----
Authorization | Your access token

# Products

## Get all the products

```shell
curl --request GET
  --url https://quire-api.herokuapp.com/users/2/products?page=1&per_page=2
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 2,
    "name": "Digital Compressor",
    "description": "Non labore eum et voluptas veniam corrupti deserunt totam. Deleniti aut natus ut adipisci. Aut sit sunt dolorem ut similique possimus quis. Voluptas minus necessitatibus molestias sed. At et maiores consectetur dolorum tempore assumenda.",
    "price": "448.0",
    "created_at": "2016-12-27T05:07:16.444Z",
    "chat_url": "http://sendbird.com/chats/12314",
    "seller": {
      "id": 4,
      "username": "april",
      "fb_user_id": "7582198748976",
      "email": "tiesha@williamson.us",
      "name": "Kathy Pollich",
      "profile_picture": "http://quireapp.s3.amazonaws.com/product_images/imgs/000/000/003/original/april.png?1482815236"
    },
    "images": [
      {
        "id": 3,
        "url": "http://quireapp.s3.amazonaws.com/product_images/imgs/000/000/003/original/kesha_huels.png?1482815236",
        "img_content_type": "image/png"
      },
      {
        "id": 4,
        "url": "http://quireapp.s3.amazonaws.com/product_images/imgs/000/000/004/original/jackelyn.rice.png?1482815238",
        "img_content_type": "image/png"
      }
    ]
  },
  {
    "id": 1,
    "name": "Side Portable Controller 2000",
    "description": "Eum quisquam nisi omnis aliquid praesentium vel eligendi. Dolores quisquam deleniti distinctio ducimus est eos. Quaerat enim dolores laboriosam culpa temporibus.",
    "price": "376.0",
    "created_at": "2016-12-27T05:07:11.996Z",
    "chat_url": "http://sendbird.com/chats/12314",
    "seller": {
      "id": 4,
      "username": "april",
      "fb_user_id": "7582198748976",
      "email": "tiesha@williamson.us",
      "name": "Kathy Pollich",
      "profile_picture": "http://quireapp.s3.amazonaws.com/product_images/imgs/000/000/003/original/april.png?1482815236"
    },
    "images": [
      {
        "id": 1,
        "url": "http://quireapp.s3.amazonaws.com/product_images/imgs/000/000/001/original/ivan.png?1482815232",
        "img_content_type": "image/png"
      },
      {
        "id": 2,
        "url": "http://quireapp.s3.amazonaws.com/product_images/imgs/000/000/002/original/margy.png?1482815234",
        "img_content_type": "image/png"
      }
    ]
  }
]
```

This endpoint will return the products that belong to the specified user. It supports pagination by using the `page` and `per_page` params.

### HTTP Request

`GET https://quire-api.herokuapp.com/users/<user_id>/products`

### URL parameters

Parameter | Description
--------- | -----------
user_id | user identifier
page | page number
per_page | number of elements per pag

## Get all nearby products

```shell
curl --request GET
  --url https://quire-api.herokuapp.com/users/2/products/nearby?page=1&per_page=2
  --header "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 2,
    "name": "Digital Compressor",
    "description": "Non labore eum et voluptas veniam corrupti deserunt totam. Deleniti aut natus ut adipisci. Aut sit sunt dolorem ut similique possimus quis. Voluptas minus necessitatibus molestias sed. At et maiores consectetur dolorum tempore assumenda.",
    "price": "448.0",
    "created_at": "2016-12-27T05:07:16.444Z",
    "chat_url": "http://sendbird.com/chats/12314",
    "seller": {
      "id": 4,
      "username": "april",
      "fb_user_id": "7582198748976",
      "email": "tiesha@williamson.us",
      "name": "Kathy Pollich",
      "profile_picture": "http://quireapp.s3.amazonaws.com/product_images/imgs/000/000/003/original/april.png?1482815236"
    },
    "images": [
      {
        "id": 3,
        "url": "http://quireapp.s3.amazonaws.com/product_images/imgs/000/000/003/original/kesha_huels.png?1482815236",
        "img_content_type": "image/png"
      },
      {
        "id": 4,
        "url": "http://quireapp.s3.amazonaws.com/product_images/imgs/000/000/004/original/jackelyn.rice.png?1482815238",
        "img_content_type": "image/png"
      }
    ]
  },
  {
    "id": 1,
    "name": "Side Portable Controller 2000",
    "description": "Eum quisquam nisi omnis aliquid praesentium vel eligendi. Dolores quisquam deleniti distinctio ducimus est eos. Quaerat enim dolores laboriosam culpa temporibus.",
    "price": "376.0",
    "created_at": "2016-12-27T05:07:11.996Z",
    "chat_url": "http://sendbird.com/chats/12314",
    "seller": {
      "id": 4,
      "username": "april",
      "fb_user_id": "7582198748976",
      "email": "tiesha@williamson.us",
      "name": "Kathy Pollich",
      "profile_picture": "http://quireapp.s3.amazonaws.com/product_images/imgs/000/000/003/original/april.png?1482815236"
    },
    "images": [
      {
        "id": 1,
        "url": "http://quireapp.s3.amazonaws.com/product_images/imgs/000/000/001/original/ivan.png?1482815232",
        "img_content_type": "image/png"
      },
      {
        "id": 2,
        "url": "http://quireapp.s3.amazonaws.com/product_images/imgs/000/000/002/original/margy.png?1482815234",
        "img_content_type": "image/png"
      }
    ]
  }
]
```

This endpoint will return the products belonging to users lastly located within a given radius. It supports pagination by using the `page` and `per_page` params.

### HTTP Request

`GET https://quire-api.herokuapp.com/users/<user_id>/products/nearby`

### Headers

Header | Value
------ | -----
Authorization | Your access token

### URL parameters

Parameter | Description
--------- | -----------
user_id | user identifier
page | page number
per_page | number of elements per pag

## Get a product

```shell
curl --request GET
  --url https://quire-api.herokuapp.com/users/2/products/1
```

> The above command returns JSON structured like this:

```json
  {
    "id": 1,
    "name": "Side Portable Controller 2000",
    "description": "Eum quisquam nisi omnis aliquid praesentium vel eligendi. Dolores quisquam deleniti distinctio ducimus est eos. Quaerat enim dolores laboriosam culpa temporibus.",
    "price": "376.0",
    "created_at": "2016-12-27T05:07:11.996Z",
    "chat_url": "http://sendbird.com/chats/12314",
    "seller": {
      "id": 4,
      "username": "april",
      "fb_user_id": "7582198748976",
      "email": "tiesha@williamson.us",
      "name": "Kathy Pollich",
      "profile_picture": "http://quireapp.s3.amazonaws.com/product_images/imgs/000/000/003/original/april.png?1482815236"
    },
    "images": [
      {
        "id": 1,
        "url": "http://quireapp.s3.amazonaws.com/product_images/imgs/000/000/001/original/ivan.png?1482815232",
        "img_content_type": "image/png"
      },
      {
        "id": 2,
        "url": "http://quireapp.s3.amazonaws.com/product_images/imgs/000/000/002/original/margy.png?1482815234",
        "img_content_type": "image/png"
      }
    ]
  }
```

This endpoint will return the specified product that belong to the specified user.

### HTTP Request

`GET https://quire-api.herokuapp.com/users/<user_id>/products/<product_id>`

### URL parameters

Parameter | Description
--------- | -----------
user_id | user identifier
product_id | product identifier


## Create a product

```shell
curl --request POST
  --header "Authorization: meowmeowmeow"
  --url https://quire-api.herokuapp.com/users/2/products
  --data '
  {
  	"product": {
  		"name": "iPod Nano 6G",
  		"description": "Music and video player",
  		"price": 600,
  		"images_attributes": [
  			{
  				"img_base":"data:image/jpeg;base64,/9j/4AAQSkZJRgABAQAAAQABAAD/2wBDAA..."
  			}
  		]
  	}
  }'
```

> The above command returns JSON structured like this

```json
{
  "success": true,
  "product": {
    "id": 239,
    "name": "iPod Nano 6g",
    "description": "Music and video player",
    "price": "600",
    "created_at": "2017-01-08T21:26:23.935Z",
    "chat_url": null,
    "seller":
      {
        "id": 2,
        "username": "vickie.legros",
        "fb_user_id": "7582198748976",
        "email": "hwa.mcglynn@sawaynwolff.us",
        "name": "Glory Auer",
        "profile_picture": "http://quireapp.s3.amazonaws.com/product_images/imgs/000/000/003/original/april.png?1482815236"
    },
    "images":
      [
        {
          "id": 135,
          "url": "/system/product_images/imgs/000/000/135/original/fred.png?1483910783",
          "img_content_type": "image/png"
        }
      ]
  }
}
```

This endpoint will create a new product with the provided fields and return a json representation of the just created product.

### HTTP Request

`POST https://quire-api.herokuapp.com/users/<user_id>/products`

### Headers

Header | Value
------ | -----
Authorization | Your access token
Content-Type | application/json

### URL parameters

Parameter | Description
--------- | -----------
user_id | user identifier

### Body

Parameter | Description | Required
--------- | ----------- | --------
product.name | Product name | true
product.description | Product description | true
product.price | Product price | true
product.images_attributes | Array of product images | true, at least 1 image, at most 5, otherwise an error will be returned
product.images_attributes[].img_base | Product image encoded with base 64 | true (for its scope)

## Update a product

```shell
curl --request PATCH
  --header "Authorization: meowmeowmeow"
  --url https://quire-api.herokuapp.com/users/2/products/239
  --data '
  {
  	"product": {
  		"name": "iPod Nano 6.1G",
  		"price": 800,
  	}
  }'
```

> The above command returns JSON structured like this

```json
{
  "id": 239,
  "name": "iPod Nano 6.1G",
  "description": "Est nesciunt rerum alias occaecati. Ut perferendis eveniet eligendi necessitatibus laborum doloribus in. Sequi et omnis voluptas recusandae.",
  "price": "800",
  "created_at": "2017-01-08T21:26:23.935Z",
  "updated_at": "2017-01-09T23:21:25.935Z",
  "chat_url": "http://sendbird.com/chats/12314",
  "seller":
    {
      "id": 316,
      "username": "vickie.legros",
      "fb_user_id": "7582198748976",
      "email": "hwa.mcglynn@sawaynwolff.us",
      "name": "Glory Auer",
      "profile_picture": "http://quireapp.s3.amazonaws.com/product_images/imgs/000/000/003/original/april.png?1482815236"
  },
  "images":
    [
      {
        "id": 133,
        "url": "/system/product_images/imgs/000/000/133/original/golden.mcglynn.png?1483910782",
        "img_content_type": "image/png"
      },
      {
        "id": 135,
        "url": "/system/product_images/imgs/000/000/135/original/fred.png?1483910783",
        "img_content_type": "image/png"
      }
    ]
}
```

This endpoint will update the specified product with the provided fields and return a json representation of the just modified product.

### HTTP Request

`PATCH https://quire-api.herokuapp.com/users/<user_id>/products/<product_id>`

### Headers

Header | Value
------ | -----
Authorization | Your access token
Content-Type | application/json

### URL parameters

Parameter | Description
--------- | -----------
user_id | user identifier
product_id | product identifier

### Body

Parameter | Description | Required
--------- | ----------- | --------
product.name | Product name | false
product.description | Product description | false
product.price | Product price | false

## Destroy a product

```shell
curl --request DELETE
  --header "Authorization: meowmeowmeow"
  --url https://quire-api.herokuapp.com/users/2/products/239
```

> The above command returns JSON structured like this

```json
{
  "success": true
}
```

This endpoint destroys the specified product associated with the specified user.

### HTTP Request

`DELETE https://quire-api.herokuapp.com/users/<user_id>/products/<product_id>`

### Headers

Header | Value
------ | -----
Authorization | Your access token

### URL parameters

Parameter | Description
--------- | -----------
user_id | user identifier
product_id | product identifier

# Product images

## Get all the product images

```shell
curl --request GET
  --url https://quire-api.herokuapp.com/users/2/products/3/images
```

> The above command returns JSON structured like this

```json
[
  {
    "id": 27,
    "url": "http://quireapp.s3.amazonaws.com/product_images/imgs/000/000/027/original/oneida.png?1483916366",
    "img_content_type": "image/png"
  },
  {
    "id": 28,
    "url": "http://quireapp.s3.amazonaws.com/product_images/imgs/000/000/028/original/will_beier.png?1483916367",
    "img_content_type": "image/png"
  }
]
```

This endpoints returns all the images of the specified product.

### HTTP Request

`GET https://quire-api.herokuapp.com/users/<user_id>/products/<product_id>/images`

### URL parameters

Parameter | Description
--------- | -----------
user_id | user identifier
product_id | product identifier

## Create product image

```shell
curl --request POST
  --header "Authorization: meowmeowmeow"
  --url https://quire-api.herokuapp.com/users/2/products/4/images
  --data '
  {
  	"product_image":{
  		"img_base":"data:image/jpeg;base64,/9j/4AAQSkZJRgABAQAAAQABAAD/..."
  	}
  }
  '
```

> The above command returns JSON structured like this

```json
{
    "id": 28,
    "url": "http://quireapp.s3.amazonaws.com/product_images/imgs/000/000/028/original/will_beier.png?1483916367",
    "img_content_type": "image/png"
}
```

This endpoint will create a new image for the specified product with the provided fields and return a json representation of the just created image. The specified product will have at most 4 images to be able to admit this one.

### HTTP Request

`POST https://quire-api.herokuapp.com/users/<user_id>/products/<product_id>/images`

### Headers

Header | Value
------ | -----
Authorization | Your access token
Content-Type | application/json

### URL parameters

Parameter | Description
--------- | -----------
user_id | user identifier
product_id | product identifier, the product will have at most 4 images to admit this one

### Body

Parameter | Description | Required
--------- | ----------- | --------
product_image.img_base | Product image encoded with base 64. It should follow this format: "data:[file_type];base64,[encoded_image]". An example of file_type is image/png | true

## Destroy product image

```shell
curl --request DELETE
  --header "Authorization: meowmeowmeow"
  --url https://quire-api.herokuapp.com/users/2/products/239/images/3
```

> The above command returns JSON structured like this

```json
{
  "success": true
}
```

This endpoint destroys the specified product image

### HTTP Request

`DELETE https://quire-api.herokuapp.com/users/<user_id>/products/<product_id>/images/<product_image_id>`

### Headers

Header | Value
------ | -----
Authorization | Your access token

### URL parameters

Parameter | Description
--------- | -----------
user_id | user identifier
product_id | product identifier
product_image_id | product image identifier

# Users

## Update user

```shell
curl --request PATCH
  --header "Authorization: meowmeowmeow"
  --url https://quire-api.herokuapp.com/users/2
  --data '
  {
  	"user": {
  		"last_location": "POINT(-19.42424 0.42424)"
  	}
  }'
```

> The above command returns JSON structured like this

```json
{
  "id": 287,
  "username": "royal.bosco",
  "email": "eliza_lakin@rauschumm.biz",
  "name":"Daniela Von",
  "last_loc": {
    "lng": "-19.42424",
    "lat": "0.42424"
  },
  "access_token": "r4Vb5oH7gDDKA47ZBnYCheVx",
  "preference_radius": 15000
}
```

This endpoint will update the specified user with the provided fields and return a json representation of the just modified user.

### HTTP Request

`PATCH https://quire-api.herokuapp.com/users/<user_id>`

### Headers

Header | Value
------ | -----
Authorization | Your access token
Content-Type | application/json

### URL parameters

Parameter | Description
--------- | -----------
user_id | user identifier

### Body

Parameter | Description | Required
--------- | ----------- | --------
user.username | Username | false
user.last_location | User's last location. Must follow the EWKT format : "POINT (longitude latitude)" | false
user.preference_radius | User's radius to define nearby users | false

# Wished products

## Index

```shell
curl --request GET
  --header "Authorization: meowmeowmeow"
  --url https://quire-api.herokuapp.com/me/wished_products
```

> The above command returns JSON structured like this

```json
[
  {
    "id": 2,
    "name": "Digital Compressor",
    "description": "Non labore eum et voluptas veniam corrupti deserunt totam. Deleniti aut natus ut adipisci. Aut sit sunt dolorem ut similique possimus quis. Voluptas minus necessitatibus molestias sed. At et maiores consectetur dolorum tempore assumenda.",
    "price": "448.0",
    "created_at": "2016-12-27T05:07:16.444Z",
    "chat_url": "http://sendbird.com/chats/12314",
    "seller": {
      "id": 4,
      "username": "april",
      "fb_user_id": "7582198748976",
      "email": "tiesha@williamson.us",
      "name": "Kathy Pollich"
    },
    "images": [
      {
        "id": 3,
        "url": "http://quireapp.s3.amazonaws.com/product_images/imgs/000/000/003/original/kesha_huels.png?1482815236",
        "img_content_type": "image/png"
      },
      {
        "id": 4,
        "url": "http://quireapp.s3.amazonaws.com/product_images/imgs/000/000/004/original/jackelyn.rice.png?1482815238",
        "img_content_type": "image/png"
      }
    ]
  },
  {
    "id": 1,
    "name": "Side Portable Controller 2000",
    "description": "Eum quisquam nisi omnis aliquid praesentium vel eligendi. Dolores quisquam deleniti distinctio ducimus est eos. Quaerat enim dolores laboriosam culpa temporibus.",
    "price": "376.0",
    "created_at": "2016-12-27T05:07:11.996Z",
    "chat_url": "http://sendbird.com/chats/12314",
    "seller": {
      "id": 4,
      "username": "april",
      "fb_user_id": "7582198748976",
      "email": "tiesha@williamson.us",
      "name": "Kathy Pollich"
    },
    "images": [
      {
        "id": 1,
        "url": "http://quireapp.s3.amazonaws.com/product_images/imgs/000/000/001/original/ivan.png?1482815232",
        "img_content_type": "image/png"
      },
      {
        "id": 2,
        "url": "http://quireapp.s3.amazonaws.com/product_images/imgs/000/000/002/original/margy.png?1482815234",
        "img_content_type": "image/png"
      }
    ]
  }
]
```

This endpoint will retrieve the currently logged in user's wish list's elements.

### HTTP Request

`GET https://quire-api.herokuapp.com/me/wished_products`

### Headers

Header | Value
------ | -----
Authorization | Your access token
Content-Type | application/json

### URL parameters

Parameter | Description
--------- | -----------

### Body

Parameter | Description | Required
--------- | ----------- | --------

## Create

```shell
curl --request POST
  --header "Authorization: meowmeowmeow"
  --url https://quire-api.herokuapp.com/me/wished_products
  --data '
  {
    "wished_product": {
      "product_id": 34
    }
  }'
```

> The above command returns JSON structured like this

```json
{
  "success": true
}
```

This endpoint will add the specified product to the currently logged in user's wish list. It will also mark the product to be skipped.

### HTTP Request

`POST https://quire-api.herokuapp.com/me/wished_products`

### Headers

Header | Value
------ | -----
Authorization | Your access token
Content-Type | application/json

### URL parameters

Parameter | Description
--------- | -----------

### Body

Parameter | Description | Required
--------- | ----------- | --------
wished_product.product_id | Product's id | true

## Delete

```shell
curl --request DELETE
  --header "Authorization: meowmeowmeow"
  --url https://quire-api.herokuapp.com/me/wished_products/356
```

> The above command returns JSON structured like this

```json
{
  "success": true
}
```

This endpoint will remove the specified product from the currently logged in user's wish list. It will also unmark the product so that it's shown to the user again.

### HTTP Request

`DELETE https://quire-api.herokuapp.com/me/wished_products/<product_id>`

### Headers

Header | Value
------ | -----
Authorization | Your access token
Content-Type | application/json

### URL parameters

Parameter | Description
--------- | -----------
product_id | The product to delete's id

# Skipped products

## Create

```shell
curl --request POST
  --header "Authorization: meowmeowmeow"
  --url https://quire-api.herokuapp.com/me/skipped_products
  --data '
  {
    "skipped_product": {
      "product_id": 34
    }
  }'
```

> The above command returns JSON structured like this

```json
{
  "success": true
}
```

This endpoint will 'mark' the specified product so that it's not shown to the currently logged in user.

### HTTP Request

`POST https://quire-api.herokuapp.com/me/skipped_products`

### Headers

Header | Value
------ | -----
Authorization | Your access token
Content-Type | application/json

### URL parameters

Parameter | Description
--------- | -----------

### Body

Parameter | Description | Required
--------- | ----------- | --------
skipped_product.product_id | Product's id | true

# Chats

## Create

```shell
curl --request POST
  --header "Authorization: meowmeowmeow"
  --url https://quire-api.herokuapp.com/me/chats
  --data '
  {
    "chat": {
      "creator_id": 34,
      "product_id": 735,
      "url": "http://sendbird.com/chats/3298523"
    }
  }'
```

> The above command returns JSON structured like this

```json
{
  "id": 3,
  "product_id": 346,
  "creator_id": 56,
  "url": "http://sendbird.com/chats/4949"
}
```

This endpoint will create a record of a chat created in SendBird. It will also mark the product to be skipped.

### HTTP Request

`POST https://quire-api.herokuapp.com/me/chats`

### Headers

Header | Value
------ | -----
Authorization | Your access token
Content-Type | application/json

### URL parameters

Parameter | Description
--------- | -----------

### Body

Parameter | Description | Required
--------- | ----------- | --------
chat.creator_id | The buyer's id | true
chat.product_id | The product's id | true
chat.url | The sendbird group channel's url | true
