---
title: "Media Type Registration for Protocol Buffers"
category: info

docname: draft-murray-dispatch-mime-protobuf-latest
submissiontype: IETF  # also: "independent", "editorial", "IAB", or "IRTF"
number: 00
date:
consensus: true
v: 3
area: ART
workgroup: DISPATCH
keyword:
 - MIME
 - protobuf
 - media type
 - application
venue:
  group: DISPATCH
  type: Working Group
  mail: dispatch@ietf.org
  arch: https://www.ietf.org/mailman/listinfo/dispatch
  github: /wkumari/draft-murray-dispatch-mime-protobuf
  latest: https://example.com/LATEST

author:
 -
  ins: M. Kucherawy
  name: Murray S. Kucherawy
  email: superuser@gmail.com
  role: editor
 -
  ins: W. Kumari
  name: Warren Kumari
  org: Google
  email: warren@kumari.net

normative:
  RFC6838:
  RFC4289:
  RFC4648:
  RFC7159:
  RFC6657:
  Protobuf:
    target: "https://protobuf.dev/"
    title: "Protocol Buffers"

informative:
  RFC2045:
  RFC8446:

--- abstract

This document registers media types for Protocol Buffers, a common extensible mechanism for serializing structured data.

--- middle

# Introduction {#intro}

(For Rob: Feel free to edit for accuracy, but no need to expound too much; we refer to the full specification URI later.)

Protocol Buffers ("protobufs") were introduced in 2008 as a free, open source, platform-independent mechanism for transport and storage of structured data.  Their use has become
increasingly common.

The specification for protobufs generally includes an interface definition language and a code generator so that clients and servers understanding the structured data can be
easily crafted and deployed.

Serialized objects are occasionally transported within media that make use of media types (see {{RFC2045}} et seq) to identify payloads.  Accordingly,
current and historical media types used for this purpose would benefit from registration.  This document requests those registrations of IANA.

# Payload Description {#description}

These media types are used in the transport of serialized objects only.  The IDL and object definitions, if transported, would be used with any appropriate text media type.
In the three figures below, only the third of these would ever be used with these media types (a JSON example is depicted).

An example use of the IDL to specify a "Person" object:

~~~
edition = "2023";

message Person {
  string name = 1;
  int32 id = 2;
  string email = 3;
}
~~~

An example of python code that uses the IDL definition above to create an instance of a "Person" object:

~~~ python
person = Person()
person.id = 1234
person.name = "John Doe"
person.email = "jdoe@example.com"
~~~

An example of the above instance expressed in JSON:

~~~
{
  "name": "John Doe",
  "id": 1234,
  "email": "jdoe@example.com"
}
~~~

(For Rob: If the actual JSON payload looks different than this, please update.)

# Encoding Considerations {#encoding}

(For Rob: Anything else to say here?)

A protobuf payload can be either in JSON form or in binary form.  For binary forms that need to transit non-binary transports, base64 (xref to RFC 4648) is recommended.

# Security Considerations {#security}

(For Rob: Anything else to say here?)

The payload for these media types contain no directly executable code.  However, it is common for a protobuf definition to be used as input to a code generator which then
does produce something executable.

A malformed request to a protobuf server could be crafted to, for example, allocate a very large amount of memory, potentially impacting other operations on that server.

Protobuf provides no security or integrity services.  Clients or servers for which this is a concern should avail themselves of solutions that provide such capabilities
(e.g., {{RFC8446}}).

# IANA Considerations {#iana}

(For Rob: Add any required/optional parameters in both of the next two sections, plus any other relevant discussion.)

## Registration for "application/protobuf" Media Type

Type name: application

Subtype name: protobuf

Required parameters: N/A

Optional parameters: N/A

Encoding considerations: binary

Security considerations: see {{security}}

Interoperability considerations: The protobufs specification includes versioning provisions to ensure backward compatibility when encountering payloads with unknown properties.

Published specification: {{Protobuf}}

Applications that use this media type: Any application with a need to exchange or store structured objects across platforms or implementations.

Fragment identifier considerations: None.

Additional information:

     Deprecated alias names for this type: x-protobuf
     Magic number(s):
     File extension(s):
     Macintosh file type code(s):

Person & email address to contact for further information: protobuf-external@google.com

Intended usage: COMMON

Restrictions on usage: N/A

Author: Rob (details here)

Change controller: protobuf-external@google.com

Provisional registration? (standards tree only): No

## Registration for "application/protobuf+json" Media Type

Type name: application

Subtype name: protobuf

Required parameters: N/A

Optional parameters: N/A

Encoding considerations:  Same as encoding considerations of application/json as specified in {{RFC7159}}, Section 11.

Security considerations: see {{security}}

Interoperability considerations: The protobufs specification includes versioning provisions to ensure backward compatibility when encountering payloads with unknown properties.

Published specification: {{Protobuf}}

Applications that use this media type: Any application with a need to exchange or store structured objects across platforms or implementations.

Fragment identifier considerations: None.

Additional information:

     Deprecated alias names for this type: x-protobuf+json
     Magic number(s):
     File extension(s):
     Macintosh file type code(s):

Person & email address to contact for further information: protobuf-external@google.com

Intended usage: COMMON

Restrictions on usage: N/A

Author: Rob (details here)

Change controller: protobuf-external@google.com

Provisional registration? (standards tree only): No

--- back

# Acknowledgments
{:numbered="false"}

Orie Steele provided valuable feedback to this work.
