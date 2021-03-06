# fastapi-tryton-async

SUPPORTS ASYNC FUNCTIONS!
*NOTE*: However, the only one transaction in a time supported!

Install:
```
pip3 install git+https://github.com/TinKurbatoff/fastapi-tryton-async.git
```

Usage:
```
from fastapi import FastAPI
from fastapi import Request
from fastapi_tryton_async import Tryton
from fastapi_tryton_async import options

options.config['TRYTON_DATABASE'] = "my_database"  # What exact database name
options.config['TRYTON_CONFIG'] = "/etc/tryton.conf"
options.config['TRYTON_CONNECTION'] = "postgresql://user:my_secret_password@localhost:5432"

app = FastAPI()

try:
    tryton = Tryton(options, configure_jinja=True)
except Exception as e:
    logger.error(f"Cannot initialize Tryton ERP: {e}")
    exit()
User = tryton.pool.get('res.user')  # Important class type - User

# ——— API endpoints
@app.post(f"/hello/")  
@tryton.transaction(readonly=False)
async def hello(request: Request):  # (request: Request) — required!
    user, = User.search([('login', '=', 'admin')])
    return '%s, Hello World!' % user.name

...

```
*NOTE*: request (fastapi Request class) always required for the decorated function parameters.


There are three configuration options available:

TRYTON_DATABASE: the Tryton’s database to connect.

TRYTON_USER: the Tryton user id to use, by default 0 (aka root).

TRYTON_CONFIG: the optional path to the Tryton’s configuration.

TRYTON_CONNECTION: full path (uri) to the database 

