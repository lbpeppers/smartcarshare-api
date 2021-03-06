# Smartcar Share API

###### CORS

This API has supports Cross-Origin Resource Sharing in every endpoint. 

###### Content-Type

Every request or response is sent using application/json from the server, clients
must send requests to the API using the same format whenever the request contents
data in the body.

###### Auth

To authenticate, the client must request a JWT to access the API resources, the
authentication request to get the token uses basic authentication headers. Just the 
endpoints for registering a new user and authentication are public, the other 
resources are protected and you will need a JWT to access any data contained there. 
The JWT must be sent through the Authorization header as Bearer token, do not send 
the token in the body of the request.

###### Response Format

Every response from the API will contain a message and a payload. The message will
provide feedback about the request has been made. The payload in some cases can be
null, usually when there is an error the response will contain just the error message
but, there are some cases where the payload can be null even if the request was
successfully, i.e. resource updated successfully.

```javascript
        {
            "message": "String message feedback",
            "payload": "value [or null]"
        }
```

###### Optional Body Parameters

Some parameters can be null in the database, here you can identify these parameters
when they're surrounded by square brackets `[, (String) phone]`. You must send every
parameter in the body even if they're marked as optional but, you should send them
with a value of `null`.

###### Date Format and Booleans

The `(date)` types must be passed as Strings following this format `Y-m-d`. In 
addition, the `(dateTime)` types must be pass as Strings following this format
`Y-m-d H:i:s`. Boolean values are numerical, `0` for `false` and `1` for `true`.

## Users

### Register new user

* **URL:** https://smartcarshare-api.herokuapp.com/users
* **Method:** POST
* **Headers:** Content-Type
* **URL Parameters:** N/A
* **Data Parameters:** (String) email, (String) password
* **Success Response Code:** 201
* **Error Response Code:** 400
* **Sample:**
```javascript
        {
            "message": "The user has been created successfully",
            "payload": {
                "user": {
                    "email": "bryan.hoyer@mail.com",
                    "role": "member",
                    "created_at": "2017-11-09 16:57:20",
                    "updated_at": "2017-11-09 16:57:20",
                    "id": 12
                }
            }
        }
```

### Change password

* **URL:** https://smartcarshare-api.herokuapp.com/users/{id}/changePassword
* **Method:** PUT
* **Headers:** Content-Type, Authorization
* **URL Parameters:** (int) user_id
* **Data Parameters:** (String) old_password, (String) new_password
* **Success Response Code:** 200
* **Error Response Code:** 400, 403
* **Sample:**
```javascript
        {
            "message": "The password does not match",
            "payload": null
        }
```

## Auth

### Get JWT

* **URL:** https://smartcarshare-api.herokuapp.com/auth/token
* **Method:** GET
* **Headers:** Authorization
* **URL Parameters:** N/A
* **Data Parameters:** (String) email, (String) password
* **Success Response Code:** 200
* **Error Response Code:** 400
* **Sample:**
```javascript
        {
            "message": "The user has been authenticated successfully",
            "payload": {
                "jwt": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpYXQiOjE1MTAyMjYwOTcsImp0aSI6Ilg1OGxlcWs0b3IydjBZM1Z5c25TYnZlRjZ1ek55WTFpRm1Ldjg5NUxSbDA9IiwiaXNzIjoiQW5kcmVzcy1NYWMtbWluaS5sb2NhbCIsIm5iZiI6MTUxMDIyNjEwNywiZXhwIjoxNTEwMjI2MjI3LCJkYXRhIjp7InVzZXJfaWQiOiI1In19.jKkRmjqmy4YOIv6qiGcl1zJ1uxsv-s8DsykE5R6nS7c"
            }
        }
```

## Members

### Insert member details

* **URL:** https://smartcarshare-api.herokuapp.com/members
* **Method:** POST
* **Headers:** Content-Type, Authorization
* **URL Parameters:** N/A
* **Data Parameters:** (String) last_name, (String) first_name, (String) licence_no, (date) licence_exp, (String) address, (String) suburb, (int) postcode [, (String) phone]
* **Success Response Code:** 201
* **Error Response Code:** 400
* **Sample:**
```javascript
        {
            "message": "Member details inserted successfully",
            "payload": {
                "id": "12"
                "lastName": "Hoyer",
                "firstName": "Bryan",
                "licenceNo": "ABC0002",
                "licenceExp": "2018-11-25",
                "address": "16 Patriots Street",
                "suburb": "New England",
                "postcode": 2125,
                "phone": "0123456789"    
            }        
        }
```

### Get member details

* **URL:** https://smartcarshare-api.herokuapp.com/members/{id}
* **Method:** GET
* **Headers:** Authorization
* **URL Parameters:** (int) user_id
* **Data Parameters:** N/A
* **Success Response Code:** 200
* **Error Response Code:** 400, 403, 404
* **Sample:**
```javascript
        {
            "message": "OK",
            "payload": {
                "member": {
                    "id": "91",
                    "user_id": "491",
                    "last_name": "Hoyer",
                    "first_name": "Bryan",
                    "licence_no": "ABC0003",
                    "licence_exp": "2018-11-26",
                    "address": "16 Patriots Street",
                    "suburb": "New England",
                    "postcode": "2126",
                    "phone": "0123456782",
                    "created_at": "2017-11-07 15:52:06",
                    "updated_at": "2017-11-08 18:57:52"
                }
            }
        }
```

### Update member details

* **URL:** https://smartcarshare-api.herokuapp.com/members/{id}
* **Method:** PUT
* **Headers:** Content-Type, Authorization
* **URL Parameters:** (int) user_id
* **Data Parameters:** (String) last_name, (String) first_name, (String) licence_no, (date) licence_exp, (String) address, (String) suburb, (int) postcode [, (String) phone]
* **Success Response Code:** 200
* **Error Response Code:** 400, 403
* **Sample:**
```javascript
        {
            "message": "The resource was updated successfully",
            "payload": null
        }
```

## Memberships

### Register new membership

* **URL:** https://smartcarshare-api.herokuapp.com/memberships
* **Method:** POST
* **Headers:** Content-Type, Authorization
* **URL Parameters:** N/A
* **Data Parameters:** (int) membership_type_id, (date) exp_date, (boolean) terms_accepted
* **Success Response Code:** 201
* **Error Response Code:** 400, 404
* **Sample:**
```javascript
        {
            "message": "The membership has been created successfully",
            "payload": {
                "membership": {
                    "id": "31",
                    "member_id": "71",
                    "membership_type_id": "1",
                    "exp_date": "2017-12-12",
                    "status": "0",
                    "terms_accepted": "1",
                    "terms_file": "terms_71.pdf",
                    "approval_date": null,
                    "smartcard_issued": "0",
                    "smartcard_no": null,
                    "created_at": "2017-11-09 00:34:27",
                    "updated_at": "2017-11-09 00:34:27"
                }
            }
        }
```

### Get membership types

* **URL:** https://smartcarshare-api.herokuapp.com/memberships/types
* **Method:** GET
* **Headers:** Authorization
* **URL Parameters:** N/A
* **Data Parameters:** N/A
* **Success Response Code:** 200
* **Error Response Code:** 400, 404
* **Sample:**
```javascript
        {
            "message": "The membership has been created successfully",
            "payload": {
                "membership_types": {
                        "id": "1",
                        "name": "Casual",
                        "annual_fee": "75.00",
                        "description": "This type of membership is best suited for people who want to use a vehicle up to 10 or 12 times a year.",
                        "auth_amount": "500.00",
                        "included_kms": "100.00",
                        "add_km_charge": "0.43",
                        "add_driver_fee": "25.00",
                        "vehicle_small_hour_rate": "10.50",
                        "vehicle_small_day_rate": "90.00",
                        "vehicle_large_hour_rate": "14.50",
                        "vehicle_large_day_rate": "112.00",
                        "valid_from": "2017-01-01 00:00:00",
                        "valid_to": "2017-12-31 23:59:59",
                        "created_at": "2017-11-09 00:34:27",
                        "updated_at": "2017-11-09 00:34:27"
                }
            }
        }
```

## Vehicles

### Get vehicles

* **URL:** https://smartcarshare-api.herokuapp.com/vehicles
* **Method:** GET
* **Headers:** Authorization
* **URL Parameters:** N/A
* **Data Parameters:** N/A
* **Success Response Code:** 200
* **Error Response Code:** 400, 404
* **Sample:**
```javascript
        {
            "message": "OK",
            "payload": {
                "vehicles": [
                    {
                        "id": "4",
                        "rego_no": "BBD342",
                        "odometer": "23738",
                        "make": "Chevrolet",
                        "model": "Captiva",
                        "colour": "Grey",
                        "address": "35 Down Hill",
                        "suburb": "White Star",
                        "postcode": "2210"
                    },
                    {
                        "id": "2",
                        "rego_no": "VAD342",
                        "odometer": "1305",
                        "make": "Ford",
                        "model": "Mustang",
                        "colour": "Black",
                        "address": "24 Roses Street",
                        "suburb": "Chadstone",
                        "postcode": "3148"
                    }
                ]
            }
        }
```

### Insert a review for a vehicle

* **URL:** https://smartcarshare-api.herokuapp.com/vehicles/{id}/reviews
* **Method:** POST
* **Headers:** Content-Type, Authorization
* **URL Parameters:** (int) vehicle_id
* **Data Parameters:** (String) description
* **Success Response Code:** 201
* **Error Response Code:** 400, 404
* **Sample:**
```javascript
        {
            "message": "The review has been created successfully",
            "payload": {
                "review": {
                    "id": 3,
                    "vehicle_id": "1",
                    "author": "Andres Martinez",
                    "description": "Small but cool"
                }
            }
        }
```

### Get vehicle reviews

* **URL:** https://smartcarshare-api.herokuapp.com/vehicles/{id}/reviews
* **Method:** GET
* **Headers:** Authorization
* **URL Parameters:** (int) vehicle_id
* **Data Parameters:** N/A
* **Success Response Code:** 200
* **Error Response Code:** 400, 404
* **Sample:**
```javascript
        {
            "message": "OK",
            "payload": {
                "reviews": [
                    {
                        "author": "Andres Martinez",
                        "description": "Small but cool"
                    }
                ]
            }
        }
```

## Locations

### Get locations

* **URL:** https://smartcarshare-api.herokuapp.com/locations
* **Method:** GET
* **Headers:** Authorization
* **URL Parameters:** N/A
* **Data Parameters:** N/A
* **Success Response Code:** 200
* **Error Response Code:** 400, 404
* **Sample:**
```javascript
        {
            "message": "OK",
            "payload": {
                "locations": [
                    {
                        "address": "24 Roses Street",
                        "suburb": "Chadstone",
                        "postcode": "3148"
                    },
                    {
                        "address": "35 Down Hill",
                        "suburb": "White Star",
                        "postcode": "2210"
                    },
                    {
                        "address": "101 Heinz Street",
                        "suburb": "Pittsburgh",
                        "postcode": "3244"
                    },
                    {
                        "address": "21 East Corner",
                        "suburb": "Sea Boaters",
                        "postcode": "2015"
                    },
                    {
                        "address": "133 Down Street",
                        "suburb": "Chadstone",
                        "postcode": "3148"
                    }
                ]
            }
        }
```

## Bookings

### Create a booking

* **URL:** https://smartcarshare-api.herokuapp.com/bookings
* **Method:** POST
* **Headers:** Content-Type, Authorization
* **URL Parameters:** N/A
* **Data Parameters:** (int) vehicle_id, (dateTime) start_date, (dateTime) end_date
* **Success Response Code:** 201
* **Error Response Code:** 400, 404
* **Sample:**
```javascript
        {
            "message": "The booking has been created successfully",
            "payload": {
                "booking": {
                    "id": 8,
                    "membership_id": "1",
                    "vehicle_id": 2,
                    "start_date": "2017-11-10 06:00:00",
                    "end_date": "2017-11-11 06:00:00",
                    "start_kms": "1305"
                }
            }
        }
```

### Get bookings

* **URL:** https://smartcarshare-api.herokuapp.com/bookings
* **Method:** GET
* **Headers:** Authorization
* **URL Parameters:** N/A
* **Data Parameters:** N/A
* **Success Response Code:** 200
* **Error Response Code:** 400, 404
* **Sample:**
```javascript
        {
            "message": "OK",
            "payload": {
                "bookings": [
                    {
                        "id": "5",
                        "membership_id": "1",
                        "vehicle_id": "2",
                        "start_date": "2017-11-10 06:00:00",
                        "end_date": "2017-11-11 06:00:00",
                        "start_kms": "1",
                        "return_date": null,
                        "return_kms": null,
                        "fuel": null,
                        "insurance": null,
                        "total_gst_exc": null,
                        "gst": null,
                        "notes": null,
                        "created_at": null,
                        "updated_at": null
                    },
                    {
                        "id": "6",
                        "membership_id": "1",
                        "vehicle_id": "2",
                        "start_date": "2017-11-10 06:00:00",
                        "end_date": "2017-11-11 06:00:00",
                        "start_kms": "1",
                        "return_date": null,
                        "return_kms": null,
                        "fuel": null,
                        "insurance": null,
                        "total_gst_exc": null,
                        "gst": null,
                        "notes": null,
                        "created_at": null,
                        "updated_at": null
                    }
                ]
            }
        }
```

### Update a booking

* **URL:** https://smartcarshare-api.herokuapp.com/bookings/{id}
* **Method:** PUT
* **Headers:** Content-Type, Authorization
* **URL Parameters:** (int) booking_id
* **Data Parameters:** (int) vehicle_id, (dateTime) start_date, (dateTime) end_date
* **Success Response Code:** 200
* **Error Response Code:** 400, 403, 404
* **Sample:**
```javascript
        {
            "message": "The resource was updated successfully",
            "payload": null
        }
```
### Insert a damage report

* **URL:** https://smartcarshare-api.herokuapp.com/bookings/{id}/reports
* **Method:** POST
* **Headers:** Content-Type, Authorization
* **URL Parameters:** (int) booking_id
* **Data Parameters:** (String) description
* **Success Response Code:** 201
* **Error Response Code:** 400, 404
* **Sample:**
```javascript
        {
            "message": "The report has been created successfully",
            "payload": {
                "report": {
                    "id": 2,
                    "booking_id": "9",
                    "description": "The mirrors were broken"
                }
            }
        }
```
