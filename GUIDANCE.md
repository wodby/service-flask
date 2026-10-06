# Flask on Wodby

What this service adds to the Python service it is based on.

## Variables the service sets

| Variable | Meaning |
| --- | --- |
| `GUNICORN_APP` | `app:app`: the WSGI application is the object `app` in the module or package `app` at the root of the repository. Change the variable on the service when it lives elsewhere, for example to call an application factory. |
| `FLASK_DEBUG` | Set to `true` in environments of type `dev` only. Absent in every other type. |

The application is served by Gunicorn on port 8080, as described for the Python service, not by `flask run`. Do not add a `flask run` start command or an `app.run()` call for deployment. Flask and Gunicorn must both be among the application's dependencies: the image includes neither.

The service sets no other `FLASK_*` variable and no secret key. An application that uses sessions provides its own `SECRET_KEY`, as a secret variable on the service.

## Linked services

The service adds no link variables. The database, mail and Redis or Valkey variables of the Python service apply unchanged, and the application reads them itself: Flask does not read `DB_*`, `SMTP_*` or `REDIS_*`.

## In a development workspace

Gunicorn runs with its polling reloader against `GUNICORN_APP`, so a saved Python file is picked up without a restart. A change to dependencies needs workspace preparation again.
