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
underscores. The list of types is defined separately for
:ref:`client-server-messages` and
:ref:`client-client-messages`.

Error
"""""

The ``"error"`` property identifies whether the message represents a success
or error condition.

For the message to be interpreted as an error, the ``"error"`` property must
be present, and must have the boolean literal ``true`` as its value::

    "error": true

If the ``"error"`` property is not present, or its value is the boolean literal
``false``, or the ``null`` literal, then the message is treated as a success
condition::

    "error": false
    "error": null

Setting the error property to any other value is not permitted.

Payload
"""""""

The permitted values of the ``"payload"`` property depend on whether the
message represents a success or error condition; see `Error`_ for details.

For a success condition, the ``"payload"`` property can be any valid
JSON data, including arrays, objects, numbers, strings, etc.
If the message type doesn't require any particular payload, the ``"payload"``
property can also be absent.
The permitted values for the payload property are defined by the particular
message type, see `Type`_ for details on determining the message type and
links to the message type specifications.

On an error condition, the ``"payload"`` property MAY be present. If present,
it can either have the literal value ``null``, or contain a JSON object as
shown::

    "payload": {
        "type": "AUTH_FAILURE",
        "message": "The system couldn't find the user, or the password was incorrect"
    }

The ``"type"`` field MUST be present. Its value MUST be a string, containing
a machine-readable error code as defined by the message type. By convention,
the string is in all-capitals with words separated by underscores.

The ``"message"`` field MAY be present. If present, its value MUST be a string.
This provides a human-readable description of the error that occurred.
Note that this string is not intended to be displayed to an end user, since
it cannot be localized; it is intended to assist in debugging issues.
Applications are expected to generate localized messages based on the error
``"type"`` field and any additional message-specific information included in
the ``"meta"`` field.
