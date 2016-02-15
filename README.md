# Lonline for Ruby **(Building process)**
Lonline allows you to log your program's execution into the cloud to avoid server access and disk space usage.

Lonline provides 6 levels of logging and 2 methods to execute reports.  Lonline is a gem to log your program through a storing service called Dynamicloud.  With Dynamicloud we can store data dynamically and very easy, Lonline allows you to set more features and this way log more than only text, level and program trace.

**This documentation has the following content:**

1. [Dependencies](#dependencies)
  1. [Dynamicloud](#dynamicloud) 
2. [Levels](#levels)
3. [Settings](#settings)
  1. [Dynamicloud account](#dynamicloud-account)
4. [How to use](#how-to-use)
 
#Dependencies
**Lonline has two main depedendencies:** Json gem and Dynamicloud gem, so when you're installing lonline gem those dependencies will be installed in your system.

#Dynamicloud
Dynamicloud is a service to store data into the cloud, it allows you to create structures dynamically without intervention from IT teams.  This service allows lonline to store log information very easy and fast.  Additionally, lonline gets the power of Dynamicloud to execute queries and provide reports for your analisis about created log in a specific time, date, year, etc.

For further information about Dynamicloud visit its site [Dynamicloud](http://www.dynamicloud.org/ "Dynamicloud").

#Levels
Lonline provides 6 levels of logging, check out the below table to understand how these levels are activated according the level of logging in your program:

| Level | Activated levels | Description (From the great Log4j library) |
| --- | --- | --- |
| `fatal` | Fatal | Designates very severe error events that will presumably lead the application to abort.|
| `error` | Fatal, Error | Designates error events that might still allow the application to continue running.|
| `warn` | Fatal, Error, Warn | Designates potentially harmful situations.|
| `info` | Fatal, Error, Warn, Info | Designates informational messages that highlight the progress of the application at coarse-grained level.|
| `debug` | Fatal, Error, Info, Warn, Debug | Designates fine-grained informational events that are most useful to debug an application.|
| `trace` | All levels | Traces the code execution between methods, lines, etc.|
| `off` | None | The highest possible rank and is intended to turn off logging.|

#Settings
Lonline needs a basic settings to be configured, Lonline gem comes with a generator to create two main files: lonline.yml and an initializer called lonline.rb (The content of this initializar could be within another initializer in your Rails app).

**For Rails applications execute the generator in your command line as follow:**

`rails g lonline`

**That command will create two files:** `config/lonline.yml` and `initializer/lonline.rb`

**config/lonline.yml**

```yalm
test: &default
  # Credentials for REST APIs
  # Go to https://www.dynamicloud.org/manage and get the API keys available in your profile
  # If you don't have an account in Dynamicloud, visit https://www.dynamicloud.org/signupform
  # You can easily use a social network to sign up
  csk: Enter your Client_Secret_Key
  aci: Enter your API_Client_Id
  # This is the model identifier for test and development environment
  # The model contains the structure to store logs into the cloud
  # For more information about models in Dynamicloud visit https://www.dynamicloud.org/documents/mfdoc
  model_identifier: 0
  # async = true is the best choice to avoid impact in your app's execution.
  # If you want to wait for every request, set async: false
  async: false
  # Shows every warning like rejected request from Dynamicloud and other warnings in lonline
  warning: true
  # Send the backtrace (the ordered method calls) of the log.  If you want to avoid this set to false
  backtrace: true
  # This tag indicates the level of lonline
  # The following table is an explanation of levels in Lonline (Descriptions from the great Log4j library):
  # ------------------------------------------------------------------------------------------------------------
  # | Level      | Activated levels           | Description                                                    |
  # ------------------------------------------------------------------------------------------------------------
  # | fatal      | Fatal                      | Designates very severe error events that will presumably lead  |
  # |            |                            | the application to abort.                                      |
  # ------------------------------------------------------------------------------------------------------------
  # | error      | Fatal, Error               | Designates error events that might still allow the application |
  # |            |                            | to continue running.                                           |
  # ------------------------------------------------------------------------------------------------------------
  # | warn       | Fatal, Error, Warn         | Designates potentially harmful situations.                     |
  # ------------------------------------------------------------------------------------------------------------
  # | info       | Fatal, Error, Warn, Info   | Designates informational messages that highlight the progress  |
  # |            |                            | of the application at coarse-grained level.                    |
  # ------------------------------------------------------------------------------------------------------------
  # | debug      | Fatal, Error, Info, Warn   | Designates fine-grained informational events that are most     |
  # |            | Debug                      | useful to debug an application.                                |
  # ------------------------------------------------------------------------------------------------------------
  # | trace      | All levels                 | Traces the code execution between methods, lines, etc.         |
  # ------------------------------------------------------------------------------------------------------------
  # | off        | None                       | The highest possible rank and is intended to turn off logging. |
  # ------------------------------------------------------------------------------------------------------------
  level: trace
  # report_limit indicates how many records lonline will execute to fetch data from Dynamicloud.
  # If you need to increase this value, please think about the available requests per month in your account.
  # Dynamicloud uses a limit of records per request, at this time the max records per request is 20.  So,
  # report_limit=100 are 5 request.
  report_limit: 100
development:
  <<: *default
production:
  <<: *default
  # Credentials for REST APIs in production environment
  # Go to https://www.dynamicloud.org/manage and get the API keys available in your profile
  # If you don't have an account in Dynamicloud, visit https://www.dynamicloud.org/signupform
  # You can easily use a social network to sign up
  csk: Enter your Client_Secret_Key
  aci: Enter your API_Client_Id
  # This is the model id for production environment
  model_identifier: 0
  # async = true is the best choice to avoid impact in your app's execution.
  # If you want to wait for every request, set async: false
  async: true
  # Shows every warning like rejected request from Dynamicloud
  warning: false
  # Send the backtrace of the log.  If you want to avoid this set to false
  backtrace: true
  # This tag indicates the level of lonline
  # The following table is an explanation of levels in Lonline (Descriptions from the great Log4j library):
  # ------------------------------------------------------------------------------------------------------------
  # | Level      | Activated levels           | Description                                                    |
  # ------------------------------------------------------------------------------------------------------------
  # | fatal      | Fatal                      | Designates very severe error events that will presumably lead  |
  # |            |                            | the application to abort.                                      |
  # ------------------------------------------------------------------------------------------------------------
  # | error      | Fatal, Error               | Designates error events that might still allow the application |
  # |            |                            | to continue running.                                           |
  # ------------------------------------------------------------------------------------------------------------
  # | warn       | Fatal, Error, Warn         | Designates potentially harmful situations.                     |
  # ------------------------------------------------------------------------------------------------------------
  # | info       | Fatal, Error, Warn, Info   | Designates informational messages that highlight the progress  |
  # |            |                            | of the application at coarse-grained level.                    |
  # ------------------------------------------------------------------------------------------------------------
  # | debug      | Fatal, Error, Info, Warn   | Designates fine-grained informational events that are most     |
  # |            | Debug                      | useful to debug an application.                                |
  # ------------------------------------------------------------------------------------------------------------
  # | trace      | All levels                 | Traces the code execution between methods, lines, etc.         |
  # ------------------------------------------------------------------------------------------------------------
  # | off        | None                       | The highest possible rank and is intended to turn off logging. |
  # ------------------------------------------------------------------------------------------------------------
  level: error
  # report_limit indicates how many records lonline will execute to fetch data from Dynamicloud.
  # If you need to increase this value, please think about the available requests per month in your account.
  # Dynamicloud uses a limit of records per request, at this time the max records per request is 20.  So,
  # report_limit=100 are 5 request (if your report execution has at least 100 logs).
  report_limit: 100
```

**initializer/lonline.rb.yml**
```ruby
Lonline::SETUP.load('config/lonline.yml', Rails.env)
# The line below sets the logger you're using in your program, every call of lonline will execute the same method in your logger
# For example:
# Lonline.log.fatal('The app has crashed and its state is unavailable') # This call will execute the following method in your logger
# logger.fatal('The app has crashed and its state is unavailable') # If this method is unavailable, lonline
# will catch the error without any feedback to you.
# If you don't need this behaviour, remove this line.
Lonline::SETUP.logger = Rails.logger
```

**For Ruby applications (No Rails) add this two lines of code to setup lonline:**

```ruby
Lonline::SETUP.load('test/lonline.yml', 'test')
Lonline::SETUP.logger = logger #If your Ruby app doesn't have logger remove this line.
```

#Dynamicloud account

Lonline needs API credentials from a Dynamicloud account, these credentials allow Lonline to access your account's structure (Model).  The mandatory model in your account should be composed for a model with at least three fields.  For further information about models and fields in Dynamicloud visit its documentation at [Models & Fields](https://www.dynamicloud.org/documents/mfdoc "Dynamicloud documentation")

**We are going to explain step by step how to setup your account in Dynamicloud, trust us it's very easy:**

1. Sign up in Dynamicloud (You can use either Google, Linkedin or Github account to speed up the registration)
2. Press the Add Field link in your Real time Dashboard.  Here you need to add thread fields:
  1. **Fields:**
  
| Field identifier | Field label | Field comments | Field type | Is a required field in form? |
| --- | --- | --- | --- | --- |
| `lonlinetext` | Log text | Contains the trace of this log | Textarea | **Yes** |
| `lonlinelevel` | Log level | Contains the log level | Combobox | **Yes** |
| `lonlinelevel` | Complete Trace | Contains the complete trace of the log | Textarea | **No** |  
  
  2. lonlinelevel is a combobox.  You need to add the following options:
  
| Value | Text |
| --- | --- |
| `Fatal` | Fatal |
| `error` | Error |
| `warn` | Warn |
| `Info` | Info |
| `debug` | Debug |
| `trace` | Trace |

#How to use

