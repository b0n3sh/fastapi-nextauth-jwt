# fastapi-nextauth-jwt
[![PyPI version](https://badge.fury.io/py/fastapi-nextauth-jwt.svg)](https://badge.fury.io/py/fastapi-nextauth-jwt)

This project contains a FastAPI dependency that can be used to decrypt and validate JWTs generated by NextAuth.
The purpose of this project is to make it easy to use a FastAPI backend in projects that use Next.js and NextAuth 
in the frontend. 

Besides JWT decryption and validation, NextAuth compatible cross-site request forgery (CSRF) protection is also implemented.

# Installation
```shell
pip install fastapi-nextauth-jwt
```
# Usage

```python
from typing import Annotated
from fastapi import FastAPI, Depends
from fastapi_nextauth_jwt import NextAuthJWT

app = FastAPI()

JWT = NextAuthJWT(
    secret="y0uR_SuP3r_s3cr37_$3cr3t",
)

@app.get("/")
async def return_jwt(jwt: Annotated[dict, Depends(JWT)]):
    return jwt
```

There are a few configuration options available in the NextAuthJWT constructor, but the most important one is `secret`,
which should be equivalent to `NEXTAUTH_SECRET` on the Next.js side. A real application would obviously not hardcode the secret like this,
but rely on the `NEXTAUTH_SECRET` env var.

It is also possible to enable or disable CSRF protection using `csrf_prevention_enabled`. 
If this is not set, this will looks at the ENV environment variable. If this is `dev` then CSRF protection will be disabled.
It is also possible to customize the HTTP verbs to which CSRF protection is applied.

You should also set the `NEXTAUTH_URL` environment variable, as it is used to determine
whether or not secure cookies are being used. Or you can set the cookie names manually.

## Examples
A [simple example](https://github.com/TCatshoek/fastapi-nextauth-jwt/tree/main/examples/simple) is available in the examples folder. It uses Next.js URL rewrites to direct
requests to FastAPI. This is just one way to do it, putting both the backend and frontend
behind something like nginx would also be a good strategy. As long as the cookies can make it to FastAPI
you should be good to go!