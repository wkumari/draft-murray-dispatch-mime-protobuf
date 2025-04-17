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
  latest: https://github.com/wkumari/draft-murray-dispatch-mime-protobuf

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
 -
  ins: R. Sloan
  name: Rob Sloan
  org: Google
  email: rmsj@google.com

normative:
  RFC1341: # MIME (Multipurpose Internet Mail Extensions)
  RFC2045: # MIME formats and encodings
  RFC2046: # Definition of media types
  RFC2077: # Model top-level media type
  RFC2119: # Key words for use in RFCs to Indicate Requirement Levels
  RFC6657: # Update to MIME regarding "charset" parameter handling in textual media types
  RFC6838: # Media type specifications and registration procedures
  RFC6839: # Additional Media Type Structured Syntax Suffixes
  RFC7303: # XML media types
  RFC8081: # Font top-level media type
  RFC9694: # Guidelines for the definition of new top-level media types
  RFC9695: # Haptics top-level media type
  RFC4289: # Multipurpose Internet Mail Extensions (MIME) Part Four: Registration Procedures
  RFC4648: # The Base16, Base32, and Base64 Data Encodings
  RFC7159: # The JavaScript Object Notation (JSON) Data Interchange Format
  Protobuf:
    target: "https://protobuf.dev/"
    title: "Protocol Buffers"

informative:
  RFC2045: # Multipurpose Internet Mail Extensions (MIME) Part One: Format of Internet Message Bodies
  RFC8446: # The Transport Layer Security (TLS) Protocol Version 1.3
  CROSS-ORIGIN-READ-BLOCKING:
    author:
      org: Chromium
    title: "Cross-Origin Read Blocking for Web Developers"
    target: https://www.chromium.org/Home/chromium-security/corb-for-developers

  MIME-SNIFF
    author:
      org: WHATWG
    title: "MIME Sniffing: Living Standard"
    target: https://mimesniff.spec.whatwg.org/#mime-type-groups

  PROTO-ANY:
    author:
      org: Protobuf
    title: "any.proto Schema Definition"
    target: https://github.com/protocolbuffers/protobuf/blob/main/src/google/protobuf/any.proto

  PROTOBUF-WEBSITE:
    author:
      org: Protobuf
    title: "Protobuf Website"
    target: https://protobuf.dev

  BINARY-FORMAT:
    author:
      org: Protobuf
    title: "Protobuf Binary Wire Encoding Spec"
    target: https://protobuf.dev/programming-guides/encoding

  JSON-FORMAT:
    author:
      org: Protobuf
    title: "Protobuf JSON Wire Encoding Spec"
    target: https://protobuf.dev/programming-guides/json

  PROTOBUF-SCHEMA-LANGUAGE-2:
    author:
      org: Protobuf
    title: "Proto2 Schema Language Specification"
    target: https://protobuf.dev/reference/protobuf/proto2-spec

  PROTOBUF-SCHEMA-LANGUAGE-3:
    author:
      org: Protobuf
    title: "Proto3 Schema Language Specification"
    target: https://protobuf.dev/reference/protobuf/proto3-spec

  PROTOBUF-SCHEMA-LANGUAGE-EDITION-2023:
    author:
      org: Protobuf
    title: "Proto Edition 2023 Schema Language Specification"
    target: https://protobuf.dev/reference/protobuf/edition-2023-spec

  GRPC-WEBSITE:
    author:
      org: Protobuf
    title: "gRPC Website"
    target: https://grpc.io

--- abstract

This document registers media types for Protocol Buffers, a common extensible mechanism for serializing structured data.

--- middle

# Introduction {#intro}

Protocol Buffers ("protobufs") were introduced in 2008 as a free, open source, platform-independent mechanism for transport and storage of structured data: their use has become
increasingly common and Protobuf implementations exist in many languages (C++, C#, Dart, Go, Java, Kotlin, Objective-C, Python, JavaScript, Ruby, Swift, and perhaps others). See {{PROTOBUF-WEBSITE}} for more information.

Protobuf consists of an interface definition language, wire encoding formats, and language-specific implementations (typically involving a generated API) so that clients and servers can be easily deployed using a common schema. Protobuf supports wire formats for interchange: {{BINARY-FORMAT}}, which is optimized for wire efficiency, and {{JSON-FORMAT}}, which maps the Protobuf schema onto a JSON structure.

Serialized objects are occasionally transported within media that make use of media types (see {{RFC2045}} et seq) to identify payloads. Accordingly,
current and historical media types used for this purpose would benefit from registration. This document requests those registrations of IANA.

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
