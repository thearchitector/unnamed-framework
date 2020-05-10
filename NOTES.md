# Implementation Notes and Guidelines

* Runtime environments `[production, development, test]`
* Configs via environment files, following the 12-factor app guidelines (<https://12factor.net/config>)
  * Loading configs from YAML files is an extra step
  * Code should be agnostic of environment configs!
  * Ability to check current environment via `UFramework.env` (which loads from environment via `starlette.config.environ`)
* [Starlette](https://www.starlette.io/) as an ASGI server. ASGI (Asynchronous Server Gateway Interface) is the successor to WSGI (Web Server Gateway Interface) and standardizes how asynchronous communication works within and between Python applications.
* Uvicorn as a built-in production-ready server (should there be a separate development server?)
* Abstract classes! ABCs are useful for establishing what is intended to be used for what --> library (not service) APIs should use ABCs
* 100% type hinting
* [peewee](https://docs.peewee-orm.com/en/latest/index.html) as an ORM. It's minimalist yet powerful, versatile and easy to implement and work with.
  * [gevent](https://www.gevent.org/) integration for seamless async database I/O (<https://docs.peewee-orm.com/en/latest/peewee/database.html#async-with-gevent>)
* Request contexts (dubbed "Action Contexts"):
  * Each request has an action context
  * Actions contexts should have easy access to the current database connection (no global DB object).
    * Database object is associated with root Starlette application (as an attribute)
  * Actions contexts should manage their own database connection, so users don't have to remember to open and then close the connection they're dealing with.
    * peewee context manager decorator (<https://docs.peewee-orm.com/en/latest/peewee/database.html#context-managers>) wraps around abstract method in application base model.
    * Custom middleware to manage database contexts (<https://www.starlette.io/middleware/>).
* Routing is tricky:
  * Routing via method decorators (like FastAPI?)
