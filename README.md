# Functions Compared: App functions, PostgreSQL functions, and Edge functions
This article discusses the differences between app functions, edge functions, and PostgreSQL functions and when to use each type.

## Different types of functions
An app that uses Supabase has plenty of choices for how and where to implement different functions:

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

