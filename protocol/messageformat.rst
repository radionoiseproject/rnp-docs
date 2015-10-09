.. highlight:: json

Message Format
==============

The message format used for most messages, for both client-server and
client-client communication, is inspired by the
`Flux Standard Action <https://github.com/acdlite/flux-standard-action>`_
data structure, and serialized as JSON.

Examples
--------

The basic message looks like this::

    {
        "type": "ACTION_TYPE",
        "payload": {
            "text": "payload can contain an arbitrary JSON data type"
        }
    }

An error would look like this instead::

    {
        "type": "ACTION_TYPE",
        "payload": {
            "type": "ERROR_TYPE",
            "message": "Human readable error message"
        },
        "error": true
    }

Specification
-------------

A message MUST:

* Be encoded as valid JSON that can be decoded by any standard JSON parser.
* Have a ``"type"`` property.

A message MAY:

* Have an ``"error"`` property
* Have a ``"payload"`` property
* Have a ``"meta"`` property

A message MUST NOT include any properties other than ``"type"``, ``"payload"``,
``"error"``, or ``"meta"``.

Type
""""

The ``"type"`` property identifies the action that the message corresponds to.
It MUST be a string.

By convention, the type string is in all-capitals, with words separated by
underscores. The list of types is defined for :ref:`client-server-messages` and
:ref:`client-client-messages`.


