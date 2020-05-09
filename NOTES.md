# Implementation Notes and Guidelines

* Runtime environments `[production, development, test]`
* Configs via YAML!
  * Databases can load environment config from `database.yaml` file (like in Ruby on Rails)
  * We don't want redundant code, and we _definitely_ don't want loading from environment files
* [Starlette](https://www.starlette.io/) as an ASGI server. ASGI (Asynchronous Server Gateway Interface) is the successor to WSGI (Web Server Gateway Interface) and standardizes how asynchronous communication works within and between Python applications.
* Uvicorn as a built-in production-ready server (shold there be a separate development server?)
* [peewee](https://docs.peewee-orm.com/en/latest/index.html) as an ORM. It's minimalist yet powerful, versatile and easy to implement and work with.
* Abstract classes! ABCs are useful for establishing what is intended to be used for what.
* 100% type hinting.
* Request contexts (dubbed "Action Contexts"):
  * These should be a thing, and are going to be called Action Contexts
  * Actions contexts should have easy access to the current database connection within these contexts (no global DB object).
  * Actions contexts should manage their own database connection, so users don't have to remember to open and then close the connection they're dealing with.
    * peewee context manager decorator (<https://docs.peewee-orm.com/en/latest/peewee/database.html#context-managers>) wraps around abstract method in application base model.
  * [gevent](https://www.gevent.org/) integration for seamless async database I/O.
