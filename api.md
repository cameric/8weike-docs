## API Doc

This file is a overall index of all API endpoints for
8Weike App. Any change to this file needs at least two
reviewers from both client (iOS or web) and server teams.

### User

- `/login/general`
    - Description: general login endpoint using cellphone
    - Method: `POST` 
    - Request params:
        - `phone`:`string` the cellphone number
        - `password`:`string` user password
    - Response params:
        - `error`:`object` Error object. `null` if user logged in successfully  
            - `statusCode`:`int` Error status code
            - `message`:`string` Error message 
        - `user`: [](User object) the complete user object 
    - Response mock: [](/login/general mock)
- `/login/wechat`
- `/login/weibo`
- `/login/alipay`
- `/signup`
    - Description: signup endpoint using cellphone
    - Method: `POST` 
    - Request params:
        - `phone`:`string` the cellphone number
        - `password`:`string` user password
    - Response params:
        - `error`:`object` Error object. `null` if user signed up successfully  
            - `statusCode`:`int` Error status code
            - `message`:`string` Error message
        - `user`:`object` the complete user object 
    - Response mock: /signup mock
- `/forget/check`
    - Description: use the cellphone number to check if the user exists
    - Method: `POST`
    - Request params:
        - `phone`:`string` the cellphone number used for getting password
    - Response params:
        - `statusCode`:`int` Status code indicating whether the user exists
            - `200`: user exists 
            - `404`: user doesn't exist
- `/forget/set`
    - Description: endpoint for password retrieval
    - Method: `POST`
    - Request params:
        - `phone`:`string` the cellphone number for resetting password
        - `password`:`string` the new password for the user    
- `/logout`
    - Description: logout of current user
    - Method: `POST`
    - Request params:
        - `userid`:`string` the userid to sign out
    - Response params:
        - `error`:`object` Error object. `null` if user logges out successfully  
            - `statusCode`:`int` Error status code
            - `message`:`string` Error message
                   
