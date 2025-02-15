---
title: Fixing Common Errors
---

{/**
This page holds descriptions and solutions to common errors users may face
when deploying to Railway.

When adding a new error/section, please keep in mind that content on this
page should be as detailed as possible, and link out to relevant docs when
necessary. We want to be consistent and complete. When in doubt, try to
stick to this formula:

    - Error as section title (`## Some Error`)
        - A description of the error. Explain to users what error this is (add
          a screenshot if applicable), why this happens, and what causes it.
        - How to solve the error (`## Solution`). Solutions may have sections
          underneath it for language/framework/stack-specific solutions, e.g.:
            - Python
            - Go
            - ...
*/}

When deploying to Railway, you may encounter some errors that prevent your
application from working. These are descriptions and solutions to errors that
users commonly encounter.

## Application Error: This application failed to respond

After deploying your application, you encounter this screen when accessing
your application's domain:

<Image src="https://res.cloudinary.com/railway/image/upload/v1681392822/docs/application-error_wgrwro.png"
alt="Screenshot of application failed to respond error"
width={729} height={675}
quality={80} />

This error occurs when Railway is unable to connect to your application.

Railway needs to know how to communicate with your application. When you
deploy and expose a web application on Railway, we expect your web server
to be available at host `0.0.0.0` and a port that we provide in the form
of a `PORT` variable. The `PORT` variable is automatically injected by
Railway into your application's environment.

Thus, your web server must listen on host `0.0.0.0` and the port that
Railway provides in the `PORT` environment variable.

### Solution

To fix this, start your application's server using:

* Host = `0.0.0.0`,
* Port = Value of the `PORT` environment variable provided by Railway.

<Banner variant="info">
Alternatively, you can set a custom `PORT` service variable in your
Railway environment to tell Railway that your application is available
at the port you specified. For more information, check out
[Defining Variables](/develop/variables#defining-variables).
**This approach is not recommended**.
</Banner>

#### Node/Express

Make your application listen on `0.0.0.0` and `PORT`:

```javascript
// Use PORT provided in environment or default to 3000
var port = process.env.PORT || 3000;

// Listen on `port` and 0.0.0.0
app.listen(port, '0.0.0.0', function () {
  // ...
});
```

#### Python/Gunicorn

`gunicorn` listens on `0.0.0.0` and the `PORT` environment variable by default.
There is no additional configuration necessary.
