## API Doc

This file is a overall index of all API endpoints for
8Weike App. Any change to this file needs at least two
reviewers from both client (iOS or web) and server teams.

### Credential

- `POST /api/login/phone`
    - Description: login endpoint using cellphone number
    - Request params:
        - `phone`:`string` the cellphone number
        - `password`:`string` user password
    - Response:
        - `error`:`object` Error object. `null` if user logged in successfully  
            - `statusCode`:`int` Error status code
                - 401 if user provides invalid credential
                - 400 for all other errors
            - `message`:`string` Error message 
        - `credential`: `object` a minimal repredentation of credential with following properties
            - `id`: `string` the credential id
            - `profileId`: `stering` the profile id that 
- `POST /api/login/weixin`
- `POST /api/login/weibo`
- `GET /api/login`
    - Description: check if the user has logged in and retrieve credential ID if so
    - Request params: no request params needed
    - Response:
        - `statusCode`: 200,
        - `credential`: `object` object containing the credential Id
            - `id`: `integer` the logged in credential ID or null if not
- `GET /api/logout`
    - Description: logout endpoint
    - Request params: No params needed
    - Response: 200 with no other information
- `POST /api/signup/phone/web`
    - Description: web-only signup endpoint with cellphone. It will send a TFA code to phone.
    - Note: This endpoint involves Captcha check so it's not suitable for mobile environment.
    - Request params:
        - `phone`:`string` the cellphone number
        - `password`:`string` user password
        - `captcha`: `string` the user-input of the captcha image
        - `hash`: `string` the original bcrypted hash that corresponds to the actual captcha value
    - Response:
        - `error`:`object` Error object. `null` if user signed up successfully  
            - `statusCode`:`int` Error status code
                - 400 if one of the following scenarios occurs:
                    - Incorrect captcha
                    - Credential already exists in database
                - 500 if one of the following scenatios occurs:
                    - Error occurred in the process of sending TFA code
                    - Error occurred during session seving
            - `message`:`string` Error message
- `POST /api/signup/phone/mobile`
    - Description: mobile-only signup endpoint with cellphone. It will send a TFA code to phone.
    - Note: This endpoint does not go through Captcha check so it's not suitable for web.
    - Request params:
        - `phone`:`string` the cellphone number
        - `password`:`string` user password
    - Response:
        - `error`:`object` Error object. `null` if user signed up successfully  
            - `statusCode`:`int` Error status code
                - 400 if Credential already exists in database                    
                - 500 if one of the following scenatios occurs:
                    - Error occurred in the process of sending TFA code
                    - Error occurred during session seving
            - `message`:`string` Error message
- `POST /api/signup/verify`
    - Description: verify the TFA code and save user credential into DB
    - Request params:
        - `code`: `string` the user-input TFA code
    - Response:
        - `error`:`object` Error object. `null` if user signed up successfully  
            - `statusCode`:`int` Error status code
                - 400 if Credential already exists in database                    
                - 500 if one of the following scenatios occurs:
                    - Error occurred in the process of sending code
                    - Error occurred during session seving
            - `message`:`string` Error message
        - `credential`: `object` an object containing credential ID
            - `id`: `integer` the credential ID for the newly created user
- `GET /api/forget` [Not implemented yet]
    - Description: use the cellphone number to check if the user exists
    - Request params:
        - `phone`:`string` the cellphone number used for getting password
    - Response:
        - `statusCode`:`int` Status code indicating whether the user exists
            - `200`: user exists 
            - `404`: user doesn't exist
        - `message`:`string` Error message
- `POST /api/forget` [Not implemented yet]
    - Description: endpoint for setting up new password
    - Method: `POST`
    - Request params:
        - `phone`:`string` the cellphone number for resetting password
        - `password`:`string` the new password for the user

### Profile

- `POST /api/profile`
    - Description: create a new profile
    - Note: this endpoint requires that the user has logged in
    - Request params:
        - `nickname`:`string` the nickname for the profile
    - Response:
        - `error`:`object` Error object. `null` if profile is created successfully
            - `statusCode`:`int` Status code
                - `400`: error occurred in the middle
            - `message`:`string` Error message
        - `profile`: `object` an object containing the new profileID
            - `profileId`: the profileId of the new profile
- `GET /api/profile/info`
    - Description: retrieve the minimal representation of a profile
    - Note: this endpoint requires that the user has logged in
    - Note: this endpoint is more suitable for web, since it's better to get the whole
      user in one call on mobile.
    - Request params: no param is needed
    - Response:
        - `error`: Error object. `null` if success
            - `statusCode`:`int` Status code
                - `400`: error occurred in the middle
        - `profile`: `object` the partial info of a user
            - `nickname`: `string` the profile's nickname
            - `description`: `string` profile description
            - `avatar`: `bitmap` the avatar of the profile
- `GET /api/profile/full` [Not implemented yet]
    - Description: retrieve the full representation of a profile
    - Note: this endpoint is more suitable for web, since it's better to get the whole
      user in one call on mobile.
    - Request params: no param is needed
    - Response:
        - `error`: Error object. `null` if success
            - `statusCode`:`int` Status code
                - `400`: error occurred in the middle
        - `profile`: `object` the partial info of a user

### Others

- `GET /api/captcha`
    - Description: web-onlt endpoint to get a captcha image data and hash
    - Request params: no param is needed
    - Response:
        - `error`: Error object. `null` if image is created successfully
            - `statusCode`:`int` Status code
                - `400`: error occurred in the middle
        - `captcha`: `object` the captcha info
            - `hash`: `string` the bcrypt hash for the actual result of the captcha
            - `picture`: `array<int>` base64 image data buffer            
- `GET /api/global_info`
    - Description: get company-specific data from the server
    - Request params: no param is needed
    - Response:
        - `globalInfo`: `object` the company information
            - `company`: `string` the company name
            - `version`: `string` version number of the current product
