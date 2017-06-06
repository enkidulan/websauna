====================
Sentry error logging
====================

Introduction
============

`Sentry <http://sentry.readthedocs.org/>`_ is a popular open source error logging server for Python application.

How to
======

The most efficient and easy way of having sentry integration would be to use  `websauna.sentry addon <https://github.com/websauna/websauna.sentry>`_, check that link for more instructions.

Alternatively, you can setup  `pyramid_raven <https://github.com/thruflo/pyramid_raven>`_ as described below.  

Install pyramid_raven::

    pip install pyramid_raven

Add in your ``production.ini``::

    pyramid.includes =
        pyramid_raven
        ...


    # pyramid_raven.swallow_parse_errors = False
    raven.dsn = https://x:y@sentry.example.com

Add a *subscriber* for event *InternalServerError*, to do so just insert following code into *views.py*::

    from pyramid.events import subscriber
    from websauna.system.core.events import InternalServerError
    from websauna.system.core.loggingcapture import get_logging_user_context

    @subscriber(InternalServerError)
    def notify_raven(event):
        request = event.request
        user_context = get_logging_user_context(request)
        request.raven.user_context(user_context)
        request.raven.captureException()
