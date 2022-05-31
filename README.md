[ WORK IN PROGRESS ]

# Functions Compared: App functions, PostgreSQL functions, and Edge functions
This article discusses the differences between app functions, edge functions, and PostgreSQL functions and when to use each type.

## Different types of functions
An app that uses Supabase has plenty of choices for how and where to implement different functions:
- **App Functions** are part of your client application and run in the context of the user's device
- **PostgreSQL Functions** run in the context of the PostgreSQL database server
- **Edge Functions** run on Deno Deploy, a hosted cloud service with nodes all over the world, ensuring the functions run as close to your users as possible

Your application can use any or all of these types of functions, and each of these types has specific advantages and disadvanges, based on the your use case(s).

### App Functions
App functions run in the context of the application and are typically run on the user's device (a laptop or desktop computer, a phone, a tablet, or even a tv or IOT device).  These functions are written in the app's language such as React, Angular, Swift, Flutter, etc. and run inside the context of a web browser (for web and PWA apps) or in the context of a native, compiled app.

#### App Function Strengths
App functions, while limited in what data they have access to (for example, they can't access data in your database directly since they're running on the client) have some huge advantages.

##### Scalability
Since they're running on the user's machine, App Functions are nearly infinitely scalable.  Millions of users can run the same function without affecting anyone else.  It's also the cheapest way to scale, since there's no cost to you, the developer to execute the function.

##### Language Familiarity
You're familiar with your app language, of course, and are almost certainly comfortable writing in that language.  PostgreSQL and Edge functions may use a different language and/or use different syntax you're not used to.

#### App Function Weaknesses

##### Isolation
These functions exist and run on the client, so by default they don't have access to shared (server) data or other shared resources.  If data is required for a function, it needs to either be fetched from the server or be cached locally.

### PostgreSQL Functions
PostgreSQL functions are stored in your database and run in the context of the database server.  They're a **part** of PostgreSQL and have all the advantages and disadvantages associated with that fact.

#### PostgreSQL Function Strengths

##### Tight Database Integration
###### Full, immediate access to all the data
###### Can run as a trigger
###### Security is built in
###### Can be scheduled to run automatically with the `pg_cron` extension

##### Can be faster since no data needs to be transferred across the network
##### Multiple languages are supported

#### PostgreSQL Function Weaknesses

##### They're stored inside the database
Since PostgreSQL functions are stored inside the database, that can make them awkward to work with for the average developer.  Functions are created or edited using SQL commands, which inserts them into an internal table in your database.  There's no easy way to "pull up the code" to make a quick edit.  For this reason it's best to store your function source somewhere in your source code tree (in a protected branch that users can't see) so you can modify the code and re-run it in a SQL Editor to update the function in the database.

##### Scalability
Functions that are CPU or I/O intensive can take up valuable resources on your database server, so it's important to have efficiency in mind when writing these functions.  You need to keep in mind things like:

- How many users will be executing this function?
- How often will this function be running?
- How many times might this function be run?

A large, complex function might not be a scalability concern if it's an administrative function run once a month by a single user.  But that same large function used in a loop by a million users might bring a server to its knees.

##### Location and Speed
Depending on where your database server is located in relationship to your end users, database functions may not perform nearly as well since there may be considerable network lag involved.  So these functions may not be suitable where you need very quick results (where milliseconds count).

### Edge Functions
Edge functions operate somewhere in the middle, between your application and your database server, and are designed to be small and extremely fast.

#### Edge Function Strengths
##### Secure
Edge functions run in an environment that can't be accessed by users of your application (as app functions can be) so the're much more secure.
##### Powerful (since they can use elevated security privileges)
Since they're running in a secure environment, edge functions can use the `service role` key, which gives your functions more complete access to your PostgreSQL database.  This allows you to perform functions to do things like add, modify, or delete users.  Edge functions using the `service role` key also can bypass any RLS (Row Level Security) Policies as well.  

#### Edge Function Weaknesses
##### Short-lived
Edge functions have an execution time limit, so they're designed for short, fast operations.  They'll usually time out if you try to use them to perform long-running processes.
##### Limits and Cost
Based on your project's subscription level, there are execution limits for edge functions based on monthly runtime hours, number of invocations, script size, and number of functions.

