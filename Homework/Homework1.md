# [Homework 1 Postman](https://github.com/MariaDash/Postman/blob/main/Homework/Homework1.md#homework-1-postman)
+ [1. GET request endpoint /get_method](https://github.com/MariaDash/Postman/blob/main/Homework/Homework1.md#1-get-request-endpoint-get_method)
+ [2. POST request endpoint /user_info_3](https://github.com/MariaDash/Postman/blob/main/Homework/Homework1.md#2-post-request-endpoint-user_info_3)
+ [3. GET request endpoint/object_info_1](https://github.com/MariaDash/Postman/blob/main/Homework/Homework1.md#3-get-request-endpointobject_info_1)
+ [4. GET method endpoint /object_info_2](https://github.com/MariaDash/Postman/blob/main/Homework/Homework1.md#4-get-method-endpoint-object_info_2)
+ [5. GET method endpoint /object_info_3](https://github.com/MariaDash/Postman/blob/main/Homework/Homework1.md#5-get-method-endpoint-object_info_3)
+ [6. GET method endpoint /object_info_4](https://github.com/MariaDash/Postman/blob/main/Homework/Homework1.md#6-get-method-endpoint-object_info_4)
+ [7. POST request endpoint /user_info_2](https://github.com/MariaDash/Postman/blob/main/Homework/Homework1.md#7--post-request-endpoint-user_info_2)  

## 1. GET request endpoint /get_method
Creating new collection in Postman  clicking on `+` near `Collection`

Naming the collection

In the current collection `add new request`
```
Protocol: http
IP: 162.55.220.72
Port: 5005

EP_1
Method: GET
EndPoint: /get_method
request url params: 
 name: str
 age: int

response: 
[
    “Str”,
    “Str”
]

```
Choose method `GET`

In the field `Enter URL or paste the text` add ***Protocol, IP, Port and Endpoint***

In the *Params* we add parameters of the request as it is written in the documentation

Click `Send` to send a request and click `Save` to save changes (or Ctrl+S)

![GET/get_method](https://github.com/MariaDash/Postman/blob/main/Postman1_pics/Capture1.PNG)

## 2. POST request endpoint /user_info_3

```
EP_2
Method: POST
EndPoint: /user_info_3
request form data: 
 name: str
 age: int
 salary: int

response: 
{'name': name,
          'age': age,
          'salary': salary,
          'family': {'children': [['Alex', 24], ['Kate', 12]],
                     'u_salary_1_5_year': salary * 4}}

```
With `...` adding new request, choose method `POST`

In the field `Enter URL or paste the text` add ***Protocol, IP, Port and Endpoint***

Parameters in **POST** method we add in the tab `Body`

Push the radio button `form-data` and add keys as written in the documentation

Click `Send` to send a request and click `Save` to save changes

![POST/user_info_3](https://github.com/MariaDash/Postman/blob/main/Postman1_pics/Capture2.PNG)

## 3. GET request endpoint/object_info_1
```
EP_3
Method: GET
EndPoint: /object_info_1
request url params: 
 name: str
 age: int
 weight: int

response: 
{'name': name,
          'age': age,
          'daily_food': weight * 0.012,
          'daily_sleep': weight * 2.5}

```
Note: `Ctrl+D` copy the request.

Choose method `GET`

In the field `Enter URL or paste the text` change ***Endpoint***

In the *Params* we add parameters of the request as it is written in the documentation

Click `Send` to send a request and click `Save` to save changes

![/GET/object_info_1](https://github.com/MariaDash/Postman/blob/main/Postman1_pics/Capture3.PNG)

## 4. GET method endpoint /object_info_2
```
EP_4
Method: GET
EndPoint: /object_info_2
request url params: 
 name: str
 age: int
 salary: int

response: 
{'start_qa_salary': salary,
          'qa_salary_after_6_months': salary * 2,
          'qa_salary_after_12_months': salary * 2.7,
          'qa_salary_after_1.5_year': salary * 3.3,
          'qa_salary_after_3.5_years': salary * 3.8,
          'person': {'u_name': [user_name, salary, age],
                     'u_age': age,
                     'u_salary_5_years': salary * 4.2}
          }

```
Method `GET`

In the field `Enter URL or paste the text` change ***Endpoint***

In the *Params* we add parameters of the request as it is written in the documentation

Click `Send` to send a request and click `Save` to save changes

![GET/object_info_2](https://github.com/MariaDash/Postman/blob/main/Postman1_pics/Capture4.PNG)

## 5. GET method endpoint /object_info_3
```
EP_5
Method: GET
EndPoint: /object_info_3
request url params: 
 name: str
 age: int
 salary: int

response: 
{'name': name,
          'age': age,
          'salary': salary,
          'family': {'children': [['Alex', 24], ['Kate', 12]],
                     'pets': {'cat':{'name':'Sunny',
                                     'age': 3},
                              'dog':{'name':'Luky',
                                     'age': 4}},
                     'u_salary_1_5_year': salary * 4}
          }

```
Method `GET`

In the field `Enter URL or paste the text` change ***Endpoint***

In the *Params* we add parameters of the request as it is written in the documentation

Click `Send` to send a request and click `Save` to save changes

![GET/object_info_3_request](https://github.com/MariaDash/Postman/blob/main/Postman1_pics/Capture5.1.PNG)

![GET/object_info_3_response](https://github.com/MariaDash/Postman/blob/main/Postman1_pics/Capture5.2.PNG)

## 6. GET method endpoint /object_info_4
```
EP_6
Method: GET
EndPoint: /object_info_4
request url params: 
 name: str
 age: int
 salary: int

response: 
{'name': name,
          'age': int(age),
          'salary': [salary, str(salary * 2), str(salary * 3)]}
```
Method `GET`

In the field `Enter URL or paste the text` change ***Endpoint***

In the *Params* we add parameters of the request as it is written in the documentation

Click `Send` to send a request and click `Save` to save changes

![GET/object_info_4](https://github.com/MariaDash/Postman/blob/main/Postman1_pics/Capture6.PNG)

## 7.  POST request endpoint /user_info_2
```
EP_7
Method: POST
EndPoint: /user_info_2
request form data: 
 name: str
 age: int
 salary: int

response: 
{'start_qa_salary': salary,
          'qa_salary_after_6_months': salary * 2,
          'qa_salary_after_12_months': salary * 2.7,
          'qa_salary_after_1.5_year': salary * 3.3,
          'qa_salary_after_3.5_years': salary * 3.8,
          'person': {'u_name': [user_name, salary, age],
                     'u_age': age,
                     'u_salary_5_years': salary * 4.2}
          }

```
With `...` adding new request, choose method `POST`

In the field `Enter URL or paste the text` add ***Protocol, IP, Port and Endpoint***

Parameters in **POST** method we add in the tab `Body`

Push the radio button `form-data` and add keys as written in the documentation

Click `Send` to send a request and click `Save` to save changes

![POST/user_info_2](https://github.com/MariaDash/Postman/blob/main/Postman1_pics/Capture7.PNG)

**I created collection test on `Status code 200`. All the requests from the collection passed it**

![200](https://github.com/MariaDash/Postman/blob/main/Postman1_pics/Capture8.PNG)

![test](https://github.com/MariaDash/Postman/blob/main/Postman1_pics/Capture9.PNG)
