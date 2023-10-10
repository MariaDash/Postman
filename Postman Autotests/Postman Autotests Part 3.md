# Postman Autotests Part 3
    
## 1. Post request endpoint /login
Need to log in.
+ http://162.55.220.72:5005/login
+ login : str (except /)
+ password : str
+ The incoming token must be passed to all other requests.
+ Further all requests require the presence of a token.

**How to do this:**

1. Create new post request http://162.55.220.72:5005/login
2. input login `Mariia` and password `abracadabra` to form-data
3. Send the request
4. Receive `token`
5. In the environment `Env` add new variable `token`
6. In the request in `Tests` do following:
```
 var RespData = pm.response.json();
    var resp_token = RespData.token;
    console.log(resp_token);

pm.environment.set("token", resp_token);

```
Note: by this we parce json data with token, check that we receive correct token and add a value to our environment variable `token`.
Check in `Environment quick look` new variable current value.
## 2. Post request endpoint /user_info
1. Send a request to
http://162.55.220.72:5005/user_info
2. Add parameters of the request (Raw json)
3. Create json format with this:
```
age: int
salary: int
name: str
auth_token
```
receive the resp:
```
{'start_qa_salary':salary,
 'qa_salary_after_6_months': salary * 2,
 'qa_salary_after_12_months': salary * 2.9,
 'person': {'u_name':[user_name, salary, age],
                                'u_age':age,
                                'u_salary_1.5_year': salary * 4}
                                }
```
How to do this:
1. Send a request to
http://162.55.220.72:5005/user_info
2. Add parameters of the request in the body in raw in JSON format
Create json format like this:
```
{
    "age" :35,
    "salary" : 1500,
    "name" : "Mariia",
    "auth_token" : "/s34lfgbj/str/jjd909/13816kjkWpqc2781str148800evny"
}
```
or
```
{
    "age" :35,
    "salary" : 1500,
    "name" : "Mariia",
    "auth_token" : "{{token}}"
}
```
Tests:
### 1. *Receive Status code 200 response*

In the *Snippets* block choose `Status code: Code is 200`
```
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);                     # in the Tests tab we can see the code
});
```
See the result:

![status200](https://github.com/MariaDash/Postman/blob/main/Postman3_pics/Pict1.PNG)
### 2. Check the json structure in the response.
+ Firstly I created a json schema from json response:
```
{
  "type": "object",
  "properties": {
    "person": {
      "type": "object",
      "properties": {
        "u_age": {
          "type": "integer"
        },
        "u_name": {
          "type": "array",
          "items": [
            {
              "type": "string"
            },
            {
              "type": "integer"
            },
            {
              "type": "integer"
            }
          ]
        },
        "u_salary_1_5_year": {
          "type": "integer"
        }
      },
      "required": [
        "u_age",
        "u_name",
        "u_salary_1_5_year"
      ]
    },
    "qa_salary_after_12_months": {
      "type": "number"
    },
    "qa_salary_after_6_months": {
      "type": "integer"
    },
    "start_qa_salary": {
      "type": "integer"
    }
  },
  "required": [
    "person",
    "qa_salary_after_12_months",
    "qa_salary_after_6_months",
    "start_qa_salary"
  ],
  "additionalProperties": false
}
```
Then I used tiny validator snippet:
```
var schema = {
  "type": "object",
  "properties": {
    "person": {
      "type": "object",
      "properties": {
        "u_age": {
          "type": "integer"
        },
        "u_name": {
          "type": "array",
          "items": [
            {
              "type": "string"
            },
            {
              "type": "integer"
            },
            {
              "type": "integer"
            }
          ]
        },
        "u_salary_1_5_year": {
          "type": "integer"
        }
      },
      "required": [
        "u_age",
        "u_name",
        "u_salary_1_5_year"
      ]
    },
    "qa_salary_after_12_months": {
      "type": "number"
    },
    "qa_salary_after_6_months": {
      "type": "integer"
    },
    "start_qa_salary": {
      "type": "integer"
    }
  },
  "required": [
    "person",
    "qa_salary_after_12_months",
    "qa_salary_after_6_months",
    "start_qa_salary"
  ],
  "additionalProperties": false
}



var RespData = pm.response.json()
pm.test('Validate the schema json', function () {
    pm.expect(tv4.validate(RespData, schema)).to.be.true;
   
});
console.log(tv4.error)
```
// Option2
```
var RespData = pm.response.json()
pm.test('Validate the schema json', function () {
    pm.expect(RespData).to.have.jsonSchema(schema);
   
});
```
See the result:

```
Status 200OK
240 ms
314 B
null
```
### 3. The response contains the coefficients of salary multiplication, write tests to check the correctness of the result of multiplication by the coefficient.
```
var u_salary_x4 = RespData.person.u_salary_1_5_year
pm.test("Salaryx4 is equal to u_salary_1_5_year", function () {
    pm.expect(u_salary_x4).to.eql(RespData.person.u_name[1]*4)
})
var qa_salary_x2 = RespData.qa_salary_after_6_months
pm.test("Salaryx2 is equal to qa_salary_after_6_months", function () {
    pm.expect(qa_salary_x2).to.eql(RespData.person.u_name[1]*2)
});

var qa_salary_x2_9 = RespData.qa_salary_after_12_months
pm.test("Salaryx2.9 is equal to qa_salary_after_12_months", function () {
    pm.expect(qa_salary_x2_9).to.eql(RespData.person.u_name[1]*2.9)
});
```
See the result:

![salary_coef](https://github.com/MariaDash/Postman/blob/main/Postman3_pics/Pict2.PNG)

### 4. Get the value from response field "u_salary_1_5_year" and send it to the field salary of the request
Choose environment `Env3`, then go to `Tests`:
```
var RespData = pm.response.json()
var u_salary = RespData.person.u_salary_1_5_year
pm.environment.set("salary", u_salary)
console.log(u_salary)
```
Check that in `Environment quick look` appear the current value of environment variable `salary`.

To send this value to another url http://162.55.220.72:5005/get_test_user request we need to add this value to parameter salary as current value `{{salary}}` in the request /get_test_user

## 3. POST request endpoint /new_data
Send the request
http://162.55.220.72:5005/new_data
adding the parameters in form-data:
```
age: int
salary: int
name: str
auth_token
```
Resp.
```
{'name':name,
  'age': int(age),
  'salary': [salary, str(salary*2), str(salary*3)]}
```
Tests:
### 1. *Receive Status code 200 response*

In the *Snippets* block choose `Status code: Code is 200`
```
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);                     # in the Tests tab we can see the code
});
```
See the result:

![status200_1](https://github.com/MariaDash/Postman/blob/main/Postman3_pics/Pict3.PNG)

### 2. Check the json structure in the response.
+ Firstly I created a json schema from json response:
```
{
  "type": "object",
  "properties": {
    "age": {
      "type": "integer"
    },
    "name": {
      "type": "string"
    },
    "salary": {
      "type": "array",
      "items": [
        {
          "type": "integer"
        },
        {
          "type": "string"
        },
        {
          "type": "string"
        }
      ]
    }
  },
  "required": [
    "age",
    "name",
    "salary"
  ],
  "additionalProperties": false
}
```
then I used tiny validator snippet:
```
var schema = {
  "type": "object",
  "properties": {
    "age": {
      "type": "integer"
    },
    "name": {
      "type": "string"
    },
    "salary": {
      "type": "array",
      "items": [
        {
          "type": "integer"
        },
        {
          "type": "string"
        },
        {
          "type": "string"
        }
      ]
    }
  },
  "required": [
    "age",
    "name",
    "salary"
  ],
  "additionalProperties": false
}
var RespData = pm.response.json()
pm.test('Validate the schema json', function () {
    pm.expect(tv4.validate(RespData, schema)).to.be.true;
   
});
console.log(tv4.error)
```
// Option2
```
var RespData = pm.response.json()
pm.test('Validate the schema json', function () {
    pm.expect(RespData).to.have.jsonSchema(schema);
   
});
```
See the result:

```
Status 200OK
240 ms
314 B
null
```
### 3. The response contains the coefficients of salary multiplication, write tests to check the correctness of the result of multiplication by the coefficient.
```   
   var ReqData = request.data

pm.test("Salaryx2 is equal to salary", function () {
    pm.expect(+RespData.salary[1]).to.eql(ReqData.salary*2)
});
pm.test("Salaryx3 is equal to salary", function () {
    pm.expect(+RespData.salary[2]).to.eql(ReqData.salary*3)
});
```
See the result:

![salary_coef2](https://github.com/MariaDash/Postman/blob/main/Postman3_pics/Pict4.PNG)

or
```
pm.test("Salaryx2 is equal to salary", function () {
    pm.expect(+RespData.salary[1]).to.eql(RespData.salary[0]*2)
});

pm.test("Salaryx3 is equal to salary", function () {
    pm.expect(+RespData.salary[2]).to.eql(RespData.salary[0]*3)
});
```
See the result:

![salary_coef3](https://github.com/MariaDash/Postman/blob/main/Postman3_pics/Pict4.1.PNG)

### 4. Check if 2nd element of salary array is greater than 1st and 0th
```
pm.test("2 perameter is more than 1", function () {
    pm.expect(+RespData.salary[2]).to.be.above(+RespData.salary[1])
});

pm.test("2 perameter is more than 0", function () {
    pm.expect(+RespData.salary[2]).to.be.above(RespData.salary[0])
});
```
See the result:

![salary_above](https://github.com/MariaDash/Postman/blob/main/Postman3_pics/Pict5.1.PNG)

## 4. POST request endpoint /test_pet_info
Send the request
```
http://162.55.220.72:5005/test_pet_info
adding the parameters in form-data:
age: int
salary: int
name: str
auth_token
```
Resp.
```
{'name': name,
 'age': age,
 'daily_food':weight * 0.012,
 'daily_sleep': weight * 2.5}
```


Tests:
### 1. *Receive Status code 200 response*

In the *Snippets* block choose `Status code: Code is 200`
```
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);                     # in the Tests tab we can see the code
});
```
See the result:

![status200_2](https://github.com/MariaDash/Postman/blob/main/Postman3_pics/Pict6.PNG)

### 2. Check the json structure in the response.
+ Firstly I created a json schema from json response:
```
{
  "type": "object",
  "properties": {
    "age": {
      "type": "integer"
    },
    "daily_food": {
      "type": "number"
    },
    "daily_sleep": {
      "type": "number"
    },
    "name": {
      "type": "string"
    }
  },
  "required": [
    "age",
    "daily_food",
    "daily_sleep",
    "name"
  ],
  "additionalProperties": false
};

```
then I used tiny validator snippet:
```
var schema = {
  "type": "object",
  "properties": {
    "age": {
      "type": "integer"
    },
    "daily_food": {
      "type": "number"
    },
    "daily_sleep": {
      "type": "number"
    },
    "name": {
      "type": "string"
    }
  },
  "required": [
    "age",
    "daily_food",
    "daily_sleep",
    "name"
  ],
  "additionalProperties": false
};



var RespData = pm.response.json()


pm.test('Schema is valid', function () {
    pm.expect(tv4.validate(RespData, schema)).to.be.true;
   
});
console.log(tv4.error)
```
// Option2
```
var RespData = pm.response.json()
pm.test('Validate the schema json', function () {
    pm.expect(RespData).to.have.jsonSchema(schema);
   
});
```
See the result:

```
Status 200OK
240 ms
314 B
null
```
### 3. The answer contains the coefficients of multiplication of weight, write tests to check the correctness of the result of multiplication by the coefficient.
```
var RespData = pm.response.json()
var ReqData = request.data
var RespDf = RespData.daily_food
pm.test("weight*0.012 is equal to weight", function () {
    pm.expect(RespDf).to.eql(ReqData.weight*0.012)
});
var RespDs = RespData.daily_sleep
pm.test("weight*2.5 is equal to weight", function () {
    pm.expect(RespDs).to.eql(ReqData.weight*2.5)
});
```
See the result:

![weight](https://github.com/MariaDash/Postman/blob/main/Postman3_pics/Pict7.PNG)

## 5. Post request endpoint /get_test_user
Send the request
```
http://162.55.220.72:5005/get_test_user
adding the parameters in form-data:
POST
age: int
salary: int
name: str
auth_token
```
Resp.
```
{'name': name,
 'age':age,
 'salary': salary,
 'family':{'children':[['Alex', 24],['Kate', 12]],
 'u_salary_1.5_year': salary * 4}
  }
```
Here we can add {{salary}} to request from Env3 as it was written in 4th task of endpoint /user_info.

Tests:
### 1. *Receive Status code 200 response*

In the *Snippets* block choose `Status code: Code is 200`
```
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);                     # in the Tests tab we can see the code
});
```
See the result:

![status200](https://github.com/MariaDash/Postman/blob/main/Postman3_pics/Pict8.PNG)
### 2. Check the json structure in the response.
+ Firstly I created a json schema from json response:
```
{
  "type": "object",
  "properties": {
    "age": {
      "type": "string"
    },
    "family": {
      "type": "object",
      "properties": {
        "children": {
          "type": "array",
          "items": [
            {
              "type": "array",
              "items": [
                {
                  "type": "string"
                },
                {
                  "type": "integer"
                }
              ]
            },
            {
              "type": "array",
              "items": [
                {
                  "type": "string"
                },
                {
                  "type": "integer"
                }
              ]
            }
          ]
        },
        "u_salary_1_5_year": {
          "type": "integer"
        }
      },
      "required": [
        "children",
        "u_salary_1_5_year"
      ]
    },
    "name": {
      "type": "string"
    },
    "salary": {
      "type": "integer"
    }
  },
  "required": [
    "age",
    "family",
    "name",
    "salary"
  ],
  "additionalProperties": false
}
```
Then I used tiny validator snippet:
```
var schema = {
  "type": "object",
  "properties": {
    "age": {
      "type": "string"
    },
    "family": {
      "type": "object",
      "properties": {
        "children": {
          "type": "array",
          "items": [
            {
              "type": "array",
              "items": [
                {
                  "type": "string"
                },
                {
                  "type": "integer"
                }
              ]
            },
            {
              "type": "array",
              "items": [
                {
                  "type": "string"
                },
                {
                  "type": "integer"
                }
              ]
            }
          ]
        },
        "u_salary_1_5_year": {
          "type": "integer"
        }
      },
      "required": [
        "children",
        "u_salary_1_5_year"
      ]
    },
    "name": {
      "type": "string"
    },
    "salary": {
      "type": "integer"
    }
  },
  "required": [
    "age",
    "family",
    "name",
    "salary"
  ],
  "additionalProperties": false
};
var RespData = pm.response.json()


pm.test('Schema is valid', function () {
    pm.expect(tv4.validate(RespData, schema)).to.be.true;
   
});
console.log(tv4.error)
```
// Option2
```
var RespData = pm.response.json()
pm.test('Validate the schema json', function () {
    pm.expect(RespData).to.have.jsonSchema(schema);
   
});
```
See the result:

```
Status 200OK
240 ms
314 B
null
```
### 3. Check that the value of the field name = the value of the variable name from the environment
```
var Env_Name = pm.environment.get("name")
pm.test("Value of the field name is equal to variable name", function () {
    pm.expect(RespData.name).to.eql(Env_Name)

});
```
See the result:

![name](https://github.com/MariaDash/Postman/blob/main/Postman3_pics/Pict9.PNG)

or
```
pm.test("Value of the field name is equal to variable name", function () {
    pm.expect(RespData.name).to.eql(pm.environment.toObject().name)

});
```
or
```
pm.test("Value of the field name is equal to variable name", function () {
    pm.expect(RespData.name).to.eql(pm.environment.get("name"))

});
```
See the result:

![name2](https://github.com/MariaDash/Postman/blob/main/Postman3_pics/Pict9.2.PNG)

### 4. Check that the value of the age field in the response matches the value of the age field sent in the request
```
var ReqData = request.data;
pm.test("Response age is equal to age request", function () {
    pm.expect(RespData.age).to.eql(ReqData.age)

});
```
See the result:

![name](https://github.com/MariaDash/Postman/blob/main/Postman3_pics/Pict10.PNG)

## 6. POST request endpoint /currency
You can take any object from the sent list, use js random. Take the Cur_ID in the object and pass it through the environment to the next request.
Send the request
```
http://54.157.99.22:80/currency
adding the parameters in form-data:
auth_token
```
Resp. Sending the array of objects
```
[
{"Cur_Abbreviation": str,
 "Cur_ID": int,
 "Cur_Name": str
}
…
{"Cur_Abbreviation": str,
 "Cur_ID": int,
 "Cur_Name": str
}
]
```
Tests:
### 1. You can take any object from the sent list, use js random. Take the Cur_ID in the object and pass it through the environment to the next request.
Create new env.variable `Cur_ID` in the `Env3`
Set the randome value:
Without `js random`:
```
var RespData = pm.response.json();
var Resp1 = RespData[1];
pm.environment.set("Cur_ID", Resp1.Cur_ID);
console.log(Resp1.Cur_ID)
```
With `js random`:
```
var RespData = pm.response.json();
function getRndInteger() {
  return Math.floor(Math.random() * RespData.length);
}
var rand = Math.floor(Math.random() * RespData.length);
console.log(rand)
pm.environment.set("Cur_ID", rand);
```
then we check in `Environment quick look` our variable value.

### 7. Post request endpoint /curr_byn
Send the request
```
http://162.55.220.72:5005/curr_byn
adding the parameters in form-data:
auth_token
curr_code: int
```
Resp.
```
{
    "Cur_Abbreviation": str
    "Cur_ID": int,
    "Cur_Name": str,
    "Cur_OfficialRate": float,
    "Cur_Scale": int,
    "Date": str
}
```
We insert the value of `curr_code` as `{{Cur_ID}}`.
Tests:
### 1. *Receive Status code 200 response*

In the *Snippets* block choose `Status code: Code is 200`
```
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);                     # in the Tests tab we can see the code
});
```
See the result:

![status200](https://github.com/MariaDash/Postman/blob/main/Postman3_pics/Pict11.PNG)

### 2. Check the json structure in the response.
+ Firstly I created a json schema from json response:
```
{
    "type": "object",
  "properties": {
    "Cur_Abbreviation": {
      "type": "string"
    },
    "Cur_ID": {
      "type": "integer"
    },
    "Cur_Name": {
      "type": "string"
    },
    "Cur_OfficialRate": {
      "type": "number"
    },
    "Cur_Scale": {
      "type": "integer"
    },
    "Date": {
      "type": "string"
    }
  },
  "required": [
    "Cur_Abbreviation",
    "Cur_ID",
    "Cur_Name",
    "Cur_OfficialRate",
    "Cur_Scale",
    "Date"
  ],
  "additionalProperties": false
}
```
Then I used tiny validator:
```
var schema = {
   "type": "object",
  "properties": {
    "Cur_Abbreviation": {
      "type": "string"
    },
    "Cur_ID": {
      "type": "integer"
    },
    "Cur_Name": {
      "type": "string"
    },
    "Cur_OfficialRate": {
      "type": "number"
    },
    "Cur_Scale": {
      "type": "integer"
    },
    "Date": {
      "type": "string"
    }
  },
  "required": [
    "Cur_Abbreviation",
    "Cur_ID",
    "Cur_Name",
    "Cur_OfficialRate",
    "Cur_Scale",
    "Date"
  ],
  "additionalProperties": false
}

var RespData = pm.response.json();

pm.test('Schema is valid', function () {
    pm.expect(tv4.validate(RespData, schema)).to.be.true
    });
console.log(tv4.error)
```
// Option2
```
var RespData = pm.response.json()
pm.test('Validate the schema json', function () {
    pm.expect(RespData).to.have.jsonSchema(schema);
   
});    
```
See the result:
```
200OK
147 ms
309 B
null
```
### *** Cycle
1) get a list of currencies
2) iterate the list of currencies
3) in each iteration, send a request to the server to get the rate of each currency
4) if 500 code is returned, go to the next iteration
5) if we get 200 code, check the response json for the presence of the "Cur_OfficialRate" field
6) if there is a field, we write information about the currency in the console in the form of response
```
{
     "Cur_Abbreviation": str
     Cur_ID: int,
     "Cur_Name": str,
     Cur_OfficialRate: float
     Cur_Scale: int,
     "Date": str
}
```
7) move on to the next iteration
```
let RespData = pm.response.json();
let token = pm.environment.get('token');
for (let i = 0; i < RespData.length; i ++)  {
cur_id = RespData[i].Cur_ID;
    
    const postRequest = {
        url: 'http://54.157.99.22:80/curr_byn',
        method: 'POST',
        header: {
            'Content-Type': 'application/json',
     },
        body: {
            mode: 'formdata',
            formdata: [
                { key: 'auth_token', value: token},
                { key: 'curr_code', value: `${cur_id}` }
        ]
    }
};
pm.sendRequest(postRequest, (err, response) => {

 if  (pm.response.code === 200) {  
    let resp_Data = response.json()
     if (resp_Data.hasOwnProperty("Cur_OfficialRate"))
   { console.log(resp_Data)}
            
        } 
if (pm.response.code === 500) {
return
} 
else {console.log('error')}  
}   
)     
}
```
![cycle1](https://github.com/MariaDash/Postman/blob/main/Postman3_pics/Pict12.1.PNG)

Note: here I used `${cur_id}` in `reversed quotes` to change a number to a string. Also I could write `(String(cur_id))`

Result:

![cycle2](https://github.com/MariaDash/Postman/blob/main/Postman3_pics/Pict13.PNG)
