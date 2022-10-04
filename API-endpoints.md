
# Project Title

API Endpoints 

## 1) Create User

### Request

`POST` /api/user/create_user

`Form Body` 

    name = Name of the user
    email = Email ID of user
    mobile = Mobile Number
    pwd = Password
    user_image = User image file

### Response

a) Creation Successful

`Status Code` - 202

`Response` 

    {
        response : true,
        data : {
            name: "Soham",
            email: "abc@gmail.com",
            mobile: "1234567890"
            user_image: "User image File"
        }
        code : 202
    }

b) User with same Mobile Exist

`Status Code` - 409

`Response` 

    {
        response : false,
        data : "User with same mobile number exist",
        code : 4091
    }

c) User with same Email Exist

`Status Code` - 409

`Response` 

    {
        response : false,
        data : "User with same email ID exist",
        code : 4092
    }

d) Internal Server error

`Status Code` - 500

`Response` 

    {
        response : false,
        data : "Internal Server error"
        code : 500
    }

## 2) Login User

### a) Send OTP

#### Request

`POST` /api/user/send_otp

`Form Body` 

    mobile = Mobile Number

####  Response

a) OTP Sent Successful

`Status Code` - 202

`Response` 

    {
        response : true,
        data : "OTP Sent Successfully"
        code : 202
    }

b) Internal Server error

`Status Code` - 500

`Response` 

    {
        response : false,
        data : "Internal Server error"
        code : 500
    }

### b) Verify OTP

`POST` /api/user/verify_otp

`Form Body` 

    otp = OTP Code

####  Response

a) OTP Varification Successful

`Status Code` - 202

`Response` 

    {
        response : true,
        data : {
            name: "Soham",
            email: "abc@gmail.com",
            mobile: "1234567890"
            user_image: "User image File"
        }
        code : 200
    }

b) OTP Varification Unsuccessful

`Status Code` - 403

`Response` 

    {
        response : false,
        data : "OTP dont match"
        code : 403
    }

c) Internal Server error

`Status Code` - 500

`Response` 

    {
        response : false,
        data : "Internal Server error"
        code : 500
    }


## 3) Update User

### Request

`POST` /api/user/update_user/:id

`URL params` id - Id of the user

`Form Body` 

    name = Name of the user
    email = Email ID of user
    mobile = Mobile Number
    pwd = Password

### Response

a) Creation Successful

`Status Code` - 202

`Response` 

    {
        response : true,
        data : {
            name: "Soham",
            email: "abc@gmail.com",
            mobile: "1234567890"
        }
        code : 202
    }

b) User with same Mobile Exist

`Status Code` - 409

`Response` 

    {
        response : false,
        data : "User with same mobile number exist"
        code : 4091
    }

c) User with same Email Exist

`Status Code` - 409

`Response` 

    {
        response : false,
        data : "User with same email ID exist"
        code : 4092
    }

d) Internal Server error

`Status Code` - 500

`Response` 

    {
        response : false,
        data : "Internal Server error"
        code : 500
    }


## 4) Get Notifications

### Request

`GET` /api/user/get_notifications/:id

`URL Params` 

    id - ID of the User

### Response

a) Creation Successful

`Status Code` - 202

`Response` 

    {
        response : true,
        data : [{
            noti_id:"456bbgvjnfviwdn",
            title : "Good Morning",
            type : "",
            type_id : "",
            image : noti.jpg,
            date : 1664858770
        },..]
        code : 202
    }

b) Internal Server error

`Status Code` - 500

`Response` 

    {
        response : false,
        data : "Internal Server error"
        code : 500
    }

------------


## 5) Create Instructor

### Request

`POST` /api/instructor/create_instructor

`Form Body` 

    name = Name of the instructor
    email = Email ID of instructor
    mobile = Mobile Number
    pwd = Password
    inst_image = Instructorimage file

### Response

a) Creation Successful

`Status Code` - 202

`Response` 

    {
        response : true,
        data : {
            name: "Soham",
            email: "abc@gmail.com",
            mobile: "1234567890"
            inst_image: "Instructor image File"
        }
        code : 202
    }

b) Instructor with same Mobile Exist

`Status Code` - 409

`Response` 

    {
        response : false,
        data : "Instructor with same mobile number exist"
        code : 4091
    }

c) Instructor with same Email Exist

`Status Code` - 409

`Response` 

    {
        response : false,
        data : "Instructor with same email ID exist"
        code : 4092
    }

d) Internal Server error

`Status Code` - 500

`Response` 

    {
        response : false,
        data : "Internal Server error"
        code : 500
    }

## 6) Login Instructor

### a) Send OTP

#### Request

`POST` /api/instructor/send_otp

`Form Body` 

    mobile = Mobile Number

####  Response

a) OTP Sent Successful

`Status Code` - 202

`Response` 

    {
        response : true,
        data : "OTP Sent Successfully"
        code : 202
    }

b) Internal Server error

`Status Code` - 500

`Response` 

    {
        response : false,
        data : "Internal Server error"
        code : 500
    }

### b) Verify OTP

`POST` /api/instructor/verify_otp

`Form Body` 

    otp = OTP Code

####  Response

a) OTP Varification Successful

`Status Code` - 202

`Response` 

    {
        response : true,
        data : {
            name: "Soham",
            email: "abc@gmail.com",
            mobile: "1234567890"
            inst_image: "Instructor image File"
        }
        code : 200
    }

b) OTP Varification Unsuccessful

`Status Code` - 403

`Response` 

    {
        response : false,
        data : "OTP dont match"
        code : 403
    }

c) Internal Server error

`Status Code` - 500

`Response` 

    {
        response : false,
        data : "Internal Server error"
        code : 500
    }


## 7) Update Instructor

### Request

`POST` /api/instructor/update_instructor/:id

`URL params` id - Id of the instructor

`Form Body` 

    name = Name of the instructor
    email = Email ID of instructor
    mobile = Mobile Number
    pwd = Password

### Response

a) Creation Successful

`Status Code` - 202

`Response` 

    {
        response : true,
        data : {
            name: "Soham",
            email: "abc@gmail.com",
            mobile: "1234567890"
        }
        code : 202
    }

b) Instructor with same Mobile Exist

`Status Code` - 409

`Response` 

    {
        response : false,
        data : "Instructor with same mobile number exist"
        code : 4091
    }

c)  Instructor with same Email Exist

`Status Code` - 409

`Response` 

    {
        response : false,
        data : "Instructor with same email ID exist"
        code : 4092
    }

d) Internal Server error

`Status Code` - 500

`Response` 

    {
        response : false,
        data : "Internal Server error"
        code : 500
    }


