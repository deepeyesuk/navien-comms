# Overview

Environment of API
- Architecture style: REST
- All API data transmission is encrypted over HTTPS,
- Payload type: JSON
- Host: https://egb17p3twf.execute-api.eu-west-1.amazonaws.com/dev
- Authentication
  : We use API key for authenticate the call. For the details, please contact [DEEPEYES App Support](app@deepeyes.co.uk).
  : Authentication header
  ```javascript
  "x-api-key": "xxxxxxxxxxxxxxxxx"
  ```
  
> If you have any problems or requests, please contact [DEEPEYES App Support](app@deepeyes.co.uk).

## API index (link to each API)
| API Name | direction | called at |
| -------- | ------ | ------ |
| [Create user](#create-user) | CIC -> Service Agent UK | when a new account is created at CIC |
| [Delete user CIC](#delete-user-cic) | CIC -> Service Agent UK | when a use leaves and the head office removes the user's access to the app |
| [Delete user App](#delete-user-app) | App -> Service Agent UK | when a user wants to delete the account permanently |
| [Create job](#create-job) | CIC -> Service Agent UK | when a job is created at CIC |
| [Update job](#update-job) | Service Agent UK -> CIC | when a job is updated or completed |
| [Cancel job](#cancel-job) | CIC -> Service Agent UK | when a job is cancelled |

### Create User
(CIC -> Service Agent UK)
Create a new account of a service agent

#### Method / Endpoint
```javascript
POST /users
```

#### Payload
```javascript
{
  "companyId", "12020394",
  "companyName", "Guildford Gas Engineers",
  "firstname": "firstname",
  "lastname": "lastname",
  "email": "example@email.com"
}
```
> DB Keys: 
>  - email (string)

#### Parameters
NONE

#### Response
* 400 Bad Request: if the request is malformed
* 403 Forbidden: if the correct api key is not in the request header
* 500 Internal Server Error (please contact [DEEPEYES App Support](app@deepeyes.co.uk))
* 201 Created

#### Example
<img width="723" alt="image" src="https://user-images.githubusercontent.com/59367560/185818969-dbdbf158-7d5a-4f19-962e-e31500380b23.png">


### Delete User (CIC)
(CIC -> Service Agent UK)

To effectively prevent a user from signing into the app when the user leaves, you need to delete their existing user record from both the database and the login system. This action will ensure that the user's account is removed entirely and they will no longer have access to sign in.

#### Method / Endpoint
```javascript
POST /users/:user-email/delete
```

#### Parameters
* :user-email: the email address of the user

#### Response
* 400 Bad Request: if the request is malformed
* 403 Forbidden: if the correct api key is not in the request header
* 500 Internal Server Error (please contact [DEEPEYES App Support](app@deepeyes.co.uk))
* 200 OK


### Delete User (App)
(App -> Service Agent UK)

To effectively prevent a user from signing into the app, you need to delete their existing user record from both the database and the login system. This action will ensure that the user's account is removed entirely and they will no longer have access to sign in.

#### Method / Endpoint
```javascript
DELETE /users/:user-email?username=:username
```

#### Parameters
* :user-email: the email address of the user
* :username: the username. It is in GUID format like `08e37d2d-5285-4e57-a309-f74e50b156af`

#### Response
* 400 Bad Request: if the request is malformed
* 403 Forbidden: if the correct api key is not in the request header
* 500 Internal Server Error (please contact [DEEPEYES App Support](app@deepeyes.co.uk))
* 200 OK

#### Example

![image](https://github.com/deepeyesuk/navien-comms/assets/840427/1905b956-5a97-4508-8b16-027688e9bde7)


### Create Job 
(CIC -> Service Agent UK)
Create a new job to take an action

#### Method / Endpoint
```bash
POST /jobs
```

#### Payload
```json
{
  "jobNo": "2022008290002",
  "companyId": "20102918293",
  "customer": {
    "name": "Michael Adam",
    "phone": "07599 123456",
    "address": {
      "line1": "41 Victoria Road",
      "line2": "Kingston Upon Thames",
      "city": "",
      "county": "Surrey",
      "postcode": "KT1 2AB",
      "country": "United Kingdom"
    }
  },
  "product": {
    "id": "product-id",
    "name": "product name"
  },
  "serviceRequestDate": "2022-09-15",
  "jobNotes": "Lorem Ipsum ...",
  "symptom": "A0100004"
}
```
> DB Keys: 
>  - jobId (string),
>  - email (string)

#### Parameters
NONE

#### Response
* 400 Bad Request: if the request is malformed
* 403 Forbidden: if the correct api key is not in the request header
* 500 Internal Server Error (please contact [DEEPEYES App Support](app@deepeyes.co.uk))
* 201 created

#### Example
<img width="728" alt="image" src="https://user-images.githubusercontent.com/59367560/185975558-0956cb91-2d89-4f14-bf13-8f7580e9b039.png">

### Update Job 
(Service Agent UK -> CIC)
Update job when the job is completed or in progress

#### Method / Endpoint
```bash
POST /API/szJobs.json
```

#### Payload
```json
{
  "job_no": "2022008290001",
  "serialNumber": "serial-number",
  "fuel": "LNG",
  "installedPlace": "Kitchen",
  "jobNotes": "Lorem Ipsum ...",
  "symptoms": "A0100004",
  "partChanges": [{
      "repairs": "B0100002",
      "partName": "name 1",
      "partNumber": "11111111",
      "partQty": "1"
    },
    {
      "repairs": "B0100003",
      "partName": "name 2",
      "partNumber": "22222222",
      "partQty": "2"
    }],
  "attachments": [
    "https://sample.com/attach1.jpg",
    "https://sample.com/attach2.jpg",
    "https://sample.com/attach3.jpg"
  ],
  "companyId": "10203810",     
  "engineerName": "Nicolas Tesla",
  "reportDate": "2022-07-27",
  "jobStatus": "completed"
}
```


### Cancel Job 
(CIC -> Service Agent UK)
Specification : 
- Navien Cervice Manager WANT TO cancel a job when the job needs to be cancelled with any reason. 
- It will be handled only by CIC system via DOING click 'Send to APP' button on a job-cancel page of CIC
  * Please see the below REST API section with regards to How to call the API
- Once the job is cancelled, the users aligned with the company will be notified via email
- APP users who have ever been assigned with the related jobNo can see this cancelled job under 'completed' tab on the app.

#### Method / Endpoint
```bash
POST https://{{service-agent-base-url}}/jobs/:jobNo/cancel
```
> Please see the service-agent-base-url via 'overview' section of this document.

#### Payload
```json
{
  "companyId": "<company id (e.g. CT01)>"
}
```

#### example
![image](https://github.com/deepeyesuk/navien-comms/assets/59367560/86fa9e5f-41b2-4a1c-935d-7362d121a76e)
