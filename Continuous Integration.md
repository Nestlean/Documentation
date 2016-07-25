# Continuous Integration
Yours CI system could deploy builds to Nestlean automatically right after all tests will be passed.

## API token
Each application has it’s unique API token which is used to communicate with Nestlean.
To get a token follow next steps:

1. Login to Nestlean
2. Click over your application in the dashboard
3. Click ‘Project Settings’ button next to your account name in navigation bar
4. Copy token from API Token field (e.g. api_0123456789abcdef0123456789abcdef0)

## Submit build to Nestlean

To submit a build you need to send POST message to next API-call:

`https://<your_company>.nestlean.com/api/ci/build.upload`

It’s content type should be `multipart/form-data` & next parameters are required:

* `token`: string - api-token
* `notes`: string - notes about what’s new in this build
* `build`: binary - build package: .ipa for iOS or .apk for Android

There are two additional parameters which could be set optional:

* `symb`: binary:
`dSym` file for iOS - zipped Package `*.app.dSYM` OR mapping file itself from the package, located at path `*.app.dSYM/Contents/Resources/DWARF/<app_name>;` 
 , `ProGuard` file for Android
* `author`: string - build author; if it wasn’t set then author name will be `build machine`

## Error handling
In case if something went wrong Nestlean will return status code 400 with error details as JSON.

Next errors are possible:

1. Wrong content type:

  ```
 {
     "errors": [
         {
             "title": "failed",
             "description": "bad content type"
         }
     ]
 }
  ```
  
2. Missing required parameter:

  ```
 {
     "errors": [
         {
             "title": "not valid",
             "description": "API token is not valid"
         },
         {
             "title": "not valid",
             "description": "notes can't be empty"
         },
         {
             "title": "not found",
             "description": "build not found"
         }
     ]
 }
  ``` 
  
3. Wrong api token:

  ```
 {
     "errors": [
         {
             "title": "not found",
             "description": "application was not found for API token"
         }
     ]
 }
  ``` 
  
4. Uploaded file is not a build::

  ```
 {
     "errors": [
         {
             "title": "not valid",
             "description": "build is invalid"
         }
     ]
 }
  ``` 
  
5. Wrong build platform:

  ```
 {
     "errors": [
         {
             "title": "not valid",
             "description": "build must be created for iOS"
         }
     ]
 }
  ``` 
  
6. Uploading the same build again:

  ```
 {
     "errors": [
         {
             "title": "failed",
             "description": "this build has been already uploaded"
         }
     ]
 }
  ``` 
