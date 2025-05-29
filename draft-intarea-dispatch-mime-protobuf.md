---
title: "Media Type Registration for Protocol Buffers"
category: info

docname: draft-intarea-dispatch-mime-protobuf-latest
submissiontype: IETF  # also: "independent", "editorial", "IAB", or "IRTF"
#number: 00 -- this is the RFC number, not the draft number....
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
  RFC8446: # The Transport Layer Security (TLS) Protocol Version 1.3
  RFC9205: # Building Protocols with HTTP
  CORB:
    author:
      org: Chromium
    title: "Cross-Origin Read Blocking for Web Developers"
    target: https://www.chromium.org/Home/chromium-security/corb-for-developers

  MIMESNIFF:
    author:
      org: WHATWG
    title: "MIME Sniffing: Living Standard"
    target: https://mimesniff.spec.whatwg.org/#mime-type-groups

  Any:
    author:
      org: Protobuf
    title: "any.proto Schema Definition"
    target: https://github.com/protocolbuffers/protobuf/blob/main/src/google/protobuf/any.proto

  Binary:
    author:
      org: Protobuf
    title: "Protobuf Binary Wire Encoding Spec"
    target: https://protobuf.dev/programming-guides/encoding

  ProtoJSON:
    author:
      org: Protobuf
    title: "Protobuf JSON Wire Encoding Spec"
    target: https://protobuf.dev/programming-guides/json

  Proto2:
    author:
      org: Protobuf
    title: "Proto2 Schema Language Specification"
    target: https://protobuf.dev/reference/protobuf/proto2-spec

  Proto3:
    author:
      org: Protobuf
    title: "Proto3 Schema Language Specification"
    target: https://protobuf.dev/reference/protobuf/proto3-spec

  Edition2023:
    author:
      org: Protobuf
    title: "Proto Edition 2023 Schema Language Specification"
    target: https://protobuf.dev/reference/protobuf/edition-2023-spec

--- abstract

This document registers media types for Protocol Buffers, a common extensible mechanism for serializing structured data.

--- middle

# Introduction {#intro}

Protocol Buffers ("protobufs") were introduced in 2008 as a free, open source, platform-independent mechanism for transport and storage of structured data: their use has become
increasingly common and Protobuf implementations exist in many languages (C++, C#, Dart, Go, Java, Kotlin, Objective-C, Python, JavaScript, Ruby, Swift, and perhaps others). See {{Protobuf}} for more information.

Protobuf consists of an interface definition language ("IDL"), wire encoding formats, and language-specific implementations (typically involving a generated API) so that clients and servers can be easily deployed using a common schema. Protobuf supports multiple wire formats for interchange: {{Binary}}, which is optimized for wire efficiency, and {{ProtoJSON}}, which maps the Protobuf schema onto a JSON structure.

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

An example of python code that uses code generated from the IDL definition above to create an instance of a "Person" object:

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

# Encoding Considerations {#encoding}

Protobuf supports only the {{Binary}} and {{ProtoJSON}} formats for interchange, both of which are platform-independent.
For binary forms that need to transit non-binary transports, a base64 `Content-Transfer-Encoding` (xref to {{RFC4648}}) is recommended.

The media type includes an optional "encoding" parameter indicating which encoding format is to be used with that particular payload.  This is included for future extensibility. Valid values for this parameter are "binary" and "json", and other values MUST be treated as an error.  See {{iana}} for the defaults for each of the two registered media types.  Using "binary" for the JSON type or "json" for the binary type MUST be treated as an error.

# Versions {#versions}

Protobuf (without a number), later also referred to as protobuf1, was the original private version of protobuf.  Protobuf2 and protobuf3 came later, with the latter of these being current at the time of writing.

The number associated with the name refers to evolutions of the schema of the IDL, not the wire format.  Accordingly, a serialized object generated by any of these is compatible with any other.  The media type registrations in {{iana}} include support for versioning of the wire format, should it ever change, but do not refer to the IDL, which can evolve independently.

Clients MUST reject payloads with an unsupported version number.

# Security Considerations {#security}

The payload for these media types contain no directly executable code. While it is common for a protobuf definition to be used as input to a code generator which then produces something executable, but that applies to the schema language, not serializations.

Protobuf provides no security, privacy, integrity, or compression services: clients or servers for which this is a concern should avail themselves of solutions that provide such capabilities (e.g. {{RFC8446}}). Implementations should be careful when processing Protobuf like any binary format: a malformed request to a protobuf server could be crafted to, for example, allocate a very large amount of memory, potentially impacting other operations on that server.

In order to safely use Protobuf serializations on the web, it is important to ensure that they are not interpreted as another document type, such as JavaScript: we recommend base64-encoding binary Protobuf responses whenever possible to prevent parsing as active content. Servers should generally follow the advice of {{RFC9205}} to prevent content sniffing for all binary formats.

Further, when using JSON serializations it is important that it is clear to browsers that the content is pure JSON, so that they can inhibit Cross-Site Script Inclusion or side-channel attacks using techniques such as Cross-Origin Read Blocking ({{CORB}}). Per {{RFC6839}}, pure JSON content is indicated by a `+json` subtype suffix (see also {{MIMESNIFF}}); so when serializing Protobuf content to JSON, users MUST use the `application/protobuf+json` media type. When using JSON, `charset` can prevent certain encoding confusion attacks so users should specify it for all JSON encodings.

In the {{Any}} type there is technically a link, which was intended to be dereferenced to obtain schemas for a given type; however this is not supported by widely used Protobuf implementations.

# IANA Considerations {#iana}

This document requests the registration of `application/protobuf` and `application/protobuf+json` as media types for Protobuf, and the notation of `application/x-protobuf`, `application/x-protobuffer`, and `application/x-protobuf+json` as deprecated aliases:

## Registration for the "application/protobuf" Media Type

Type name: application

Subtype name: protobuf

Required parameters: none

Optional parameters:

* `encoding`, which indicates the type of Protobuf encoding and is "binary" by default for `application/protobuf`, indicating the {{Binary}} format. At the time of writing, no other encoding can be used for `application/protobuf` so this parameter is for extensibility.
* `version`, which indicates the version of the wire encoding specification (not the schema language), with default `1`. At the time of writing, no protobuf wire encodings are versioned so this parameter is for extensibility. Unversioned wire encodings should be treated as having version `1`. Protobuf implementations should accept all versions of wire encodings defined at the time of implementation.

Encoding considerations: binary

Security considerations: see {{security}}

Interoperability considerations: The Protobuf specification includes versioning provisions to ensure backward compatibility when encountering payloads with unknown properties.

Published specification: {{Protobuf}}

Applications that use this media type: Any application with a need to exchange or store structured objects across platforms or implementations.

Fragment identifier considerations: None.

Additional information:

     Deprecated alias names for this type: `application/x-protobuf`, `application/x-protobuffer`
     Magic number(s):
     File extension(s):
     Macintosh file type code(s):

Person & email address to contact for further information: Protobuf \<protobuf-team@google.com\>

Intended usage: COMMON

Restrictions on usage: None

Author: Rob Sloan \<rmsj@google.com\>

Change controller: Protobuf \<protobuf-team@google.com\>

Provisional registration? (standards tree only): No

## Registration for "application/protobuf+json" Media Type

Type name: application

Subtype name: protobuf+json

Required parameters: `charset`, which must be set to `utf-8` (case-insensitive)

Optional parameters:

* `encoding`, which indicates the type of Protobuf encoding and is `json` by default for `application/protobuf+json`, indicating the {{ProtoJSON}} format. At the time of writing, no other encoding can be used for `application/protobuf+json` so this parameter is for extensibility.
* `version`, which indicates the version of the wire encoding specification (not the schema language), with default `1`. At the time of writing, no protobuf wire encodings are versioned so this parameter is for extensibility. Unversioned wire encodings should be treated as having version `1`. Protobuf implementations should accept all versions of wire encodings defined at the time of implementation.

Encoding considerations: Same as encoding considerations of `application/json` as specified in {{RFC7159}}, Section 11.

Security considerations: see {{security}}

Interoperability considerations: The Protobuf specification includes versioning provisions to ensure backward compatibility when encountering payloads with unknown properties.

Published specification: {{Protobuf}}

Applications that use this media type: Any application with a need to exchange or store structured objects across platforms or implementations.

Fragment identifier considerations: None.

Additional information:

     Deprecated alias names for this type: x-protobuf+json
     Magic number(s):
     File extension(s):
     Macintosh file type code(s):

Person & email address to contact for further information: Protobuf \<protobuf-team@google.com\>

Intended usage: COMMON

Restrictions on usage: None

Author: Rob Sloan \<rmsj@google.com\>

Change controller: Protobuf \<protobuf-team@google.com\>

Provisional registration? (standards tree only): No

# Contact

Please contact protobuf-team@google.com for requests to adjust this specification. Issues may be raised at https://github.com/protocolbuffers/protobuf.

--- back

# Acknowledgments
{:numbered="false"}

Orie Steele provided valuable feedback to this work.
