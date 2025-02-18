---
title: "DNSSEC Cryptographic Algorithm Recommendation Update Process"
abbrev: "DNSSEC Algorithms Update Process"
docname: draft-ietf-dnsop-rfc8624-bis-06
category: info
ipr: trust200902
stream: IETF
updates: 8624

stand_alone: yes
pi: [toc, sortrefs, symrefs, docmapping]

author:
  -
    ins: W. Hardaker
    name: Wes Hardaker
    org: USC/ISI
    email: ietf@hardakers.net
  -
    ins: W. Kumari
    name: Warren Kumari
    org: Google
    email: warren@kumari.net

normative:
  RFC2119:
  RFC8126:
  RFC8174:
  RFC8624:
  DNSKEY-IANA:
    author:
      name: IANA
    target: "https://www.iana.org/assignments/dns-sec-alg-numbers/dns-sec-alg-numbers.xml#dns-sec-alg-numbers-1"
    title: DNS Security Algorithm Numbers
  DS-IANA:
    author:
      name: IANA
    target: "http://www.iana.org/assignments/ds-rr-types"
    title: Delegation Signer (DS) Resource Record (RR) Type Digest Algorithms

informative:
  RFC4034:
  RFC4509:
  RFC5155:
  RFC5702:
  RFC5933:
  RFC6605:
  RFC6781:
  RFC7583:
  RFC8080:
  RFC9364:
  TLS-ciphersuites:
    author:
      name: IANA
    target: "https://www.iana.org/assignments/tls-parameters/tls-parameters.xhtml#tls-parameters-4"
    title: Transport Layer Security (TLS) Parameters

--- abstract

   The DNSSEC protocol makes use of various cryptographic algorithms to provide
   authentication of DNS data and proof of non-existence.  To ensure
   interoperability between DNS resolvers and DNS authoritative servers, it is
   necessary to specify both a set of algorithm implementation requirements and
   usage guidelines to ensure that there is at least one algorithm that all
   implementations support.  This document updates RFC8624 by moving the
   canonical source of algorithm implementation requirements and usage guidance
   for DNSSEC from RFC8624 to an IANA registry. This is done both to allow
   the list to be more easily updated, and to allow the list to be more easily
   referenced. Future extensions to this registry can be made under new,
   incremental update RFCs.

   The document does not change the status (MUST, MAY, RECOMMENDED, etc) of any
   of the algorithms listed in RFC8624; that is the work of future documents.

--- middle

# Introduction

   DNS Security Extensions (DNSSEC) [RFC9364] is used to provide
   authentication of DNS data. The DNSSEC signing algorithms are
   defined by various RFCs, including [RFC4034], [RFC4509], [RFC5155],
   [RFC5702], [RFC5933], [RFC6605], [RFC8080].

   To ensure interoperability, a set of "mandatory-to-implement"
   DNSKEY algorithms are defined in [RFC8624].  To make the current
   status of the algorithms more easily accessible and understandable,
   and to make future changes to these recommendations easier to
   publish, this document moves the canonical status of the algorithms
   from [RFC8624] to the IANA DNSSEC algorithm registries.
   Additionally, as advice to operators, it adds recommendations for
   deploying and the usage of these algorithms.

  This is similar to the process used for the [TLS-ciphersuites] registry,
  where the canonical list of ciphersuites is in the IANA registry, and the
  RFCs reference the IANA registry.

##  Document Audience

   The columns added to the IANA ["DNS Security Algorithm Numbers"][DNSKEY-IANA]
   and ["DNSSEC Delegation Signer (DS) Resource Record (RR) Type Digest
   Algorithms"][DS-IANA] registries target DNSSEC operators and implementers.

   Implementations need to meet both high security expectations as
   well as provide interoperability between various vendors and with
   different versions.

   The field of cryptography evolves continuously.  New, stronger algorithms
   appear, and existing algorithms may be found to be less secure than
   originally thought.  Therefore, algorithm implementation requirements and
   usage guidance need to be updated from time to time in order to reflect the
   new reality, and to allow for a smooth transition to more secure algorithms,
   as well as deprecation of algorithms deemed to no longer be secure.

   Implementations need to be conservative in the selection of
   algorithms they implement in order to minimize both code complexity
   and the  attack surface.

   The perspective of implementers may differ from that of an operator
   who wishes to deploy and configure DNSSEC with only the safest
   algorithm.  As such this document also adds new recommendations
   about which algorithms should be deployed regardless of
   implementation status. In general it is expected that deployment
   of aging algorithms should generally be reduced before
   implementations stop supporting them.

##  Updating Algorithm Requirement Levels

   By the time a DNSSEC cryptographic algorithm is made
   mandatory-to-implement, it should already be available in most
   implementations.  This document defines an IANA registration
   modification to allow future documents to specify the
   implementation recommendations for each algorithm, as the
   recommendation status of each DNSSEC cryptographic algorithm is
   expected to change over time.  For example, there is no guarantee
   that newly introduced algorithms will become mandatory-to-implement
   in the future.  Likewise, published algorithms are continuously
   subjected to cryptographic attack and may become too weak, or even
   be completely broken, and will require deprecation in the future.

   It is expected that the deprecation of an algorithm will be performed
   gradually.  This provides time for implementations to update
   their implemented algorithms while remaining interoperable.  Unless
   there are strong security reasons, an algorithm is expected to be
   downgraded from MUST to NOT RECOMMENDED or MAY, instead of directly
   from MUST to MUST NOT.  Similarly, an algorithm that has not been
   mentioned as mandatory-to-implement is expected to be first introduced
   as RECOMMENDED instead of a MUST.

   Since the effect of using an unknown DNSKEY algorithm is that the zone is
   treated as insecure, it is recommended that algorithms which have been
   downgraded to NOT RECOMMENDED or lower not be used by authoritative
   nameservers and DNSSEC signers to create new DNSKEY's.  This will allow for
   deprecated algorithms to become used less and less over time.  Once an
   algorithm has reached a sufficiently low level of deployment, it can be
   marked as MUST NOT, so that recursive resolvers can remove support for
   validating it.

   Validating recursive resolvers are encouraged to retain support for all
   algorithms not marked as MUST NOT.

## Requirements notation

   The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT",
   "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY",
   and "OPTIONAL" in this document are to be interpreted as described
   in BCP 14 {{RFC2119}} {{RFC8174}} when, and only when, they appear
   in all capitals, as shown here.

   [RFC2119] considers the term SHOULD equivalent to RECOMMENDED, and
   SHOULD NOT equivalent to NOT RECOMMENDED.  The authors of this
   document have chosen to use the terms RECOMMENDED and NOT
   RECOMMENDED, as this more clearly expresses the recommendations to
   implementers.

# Adding usage and implementation recommendations to the IANA DNSSEC registries

   Per this document, the following columns are being added to the
   following DNSSEC algorithm registries registered with IANA:

   |-----------------------------------|----------------------------------|
   | Registry                          | Column added                     |
   |-----------------------------------|----------------------------------|
   | Domain Security Algorithm Numbers | Use for DNSSSEC Signing          |
   |-----------------------------------|----------------------------------|
   | Domain Security Algorithm Numbers | Use for DNSSSEC Validation       |
   |-----------------------------------|----------------------------------|
   | Domain Security Algorithm Numbers | Implement for DNSSSEC Signing    |
   |-----------------------------------|----------------------------------|
   | Domain Security Algorithm Numbers | Implement for DNSSSEC Validation |
   |-----------------------------------|----------------------------------|
   | Digest Algorithm                  | Use for DNSSSEC Delegation       |
   |-----------------------------------|----------------------------------|
   | Digest Algorithm                  | Use for DNSSSEC Validation       |
   |-----------------------------------|----------------------------------|
   | Digest Algorithm                  | Implement for DNSSSEC Delegation |
   |-----------------------------------|----------------------------------|
   | Digest Algorithm                  | Implement for DNSSSEC Validation |
   |-----------------------------------|----------------------------------|

   Adding a new entry to the "DNS System Algorithm Numbers" registry
   with a recommended value of "MAY" in the "Use for DNSSSEC Signing",
   "Use for DNSSSEC Validation", "Implement for DNSSSEC Signing", or
   "Implement for DNSSSEC Validation" columns is via the
   "Specification Required" policy as defined in [RFC8126] in order to
   promote continued evolution of DNSSEC algorithms and DNSSEC
   agility.  New entries add through the "Specification Required"
   process will have the value of "MAY" for all columns. (Ed note (RFC
   Editor - please delete this before publication): As a reminder: the
   "Specification Required" policy includes a requirement for a
   designated expert to review the request.)

   Adding a new entry to, or changing existing values in,
   the "DNS System Algorithm Numbers" registry for the "Use for
   DNSSSEC Signing", "Use for DNSSSEC Validation", "Implement for
   DNSSSEC Signing", or "Implement for DNSSSEC Validation" columns to
   any other value than "MAY" requires a Standards Action.

   Adding a new entry to the "Digest Algorithms" registry with a recommended
   value of "MAY" in the "Use for DNSSSEC Delegation", "Use for DNSSSEC
   Validation", "Implement for DNSSSEC Delegation", or "Implement for DNSSSEC
   Validation" columns is via the "Specification Required" policy as defined in
   [RFC8126].

   Adding a new entry to, or changing existing values in,
   the "DNS System Algorithm Numbers" registry for the "Use for
   DNSSSEC Delegation", "Use for DNSSSEC Validation", "Implement for
   DNSSSEC Delegation", or "Implement for DNSSSEC Validation" columns
   to any other value than "MAY" requires a Standards Action.

   If an item is not marked as "RECOMMENDED", it does not necessarily
   mean that it is flawed; rather, it indicates that the item either
   has not been through the IETF consensus process, has limited
   applicability, or is intended only for specific use cases.

   The following sections state the initial values to be populated
   into these rows, with Implementation values transcribed from
   [RFC8624].  Use for columns was also set to the same values from
   [RFC8624], as there is no existing documented values and general
   interpretation of the registries to date indicate they should be the
   same, although may differ in the future.

#  DNS System Algorithm Numbers Column Values

   Initial recommendation columns of use and implementation
   recommendations for the "Domain Name System Security (DNSSEC)
   Algorithm Numbers" are shown in Table 2.

   When there are multiple
   RECOMMENDED algorithms in the "use" column, operators should choose
   the best algorithm according to local policy.

   |----|---------------------|------------------------|---------------------------|------------------------------|---------------------------------|
   | N  | Mnemonics           | Use for DNSSEC Signing | Use for DNSSEC Validation | Implement for DNSSEC Signing | Implement for DNSSEC Validation |
   |----|---------------------|------------------------|---------------------------|
------------------------------|---------------------------------|
   | 1  | RSAMD5              | MUST NOT               | MUST NOT                  | MUST NOT                     | MUST NOT                        |
   |----|---------------------|------------------------|---------------------------|------------------------------|---------------------------------|
   | 3  | DSA                 | MUST NOT               | MUST NOT                  | MUST NOT                     | MUST NOT                        |
   |----|---------------------|------------------------|---------------------------|------------------------------|---------------------------------|
   | 5  | RSASHA1             | NOT RECOMMENDED        | RECOMMENDED               | NOT RECOMMENDED              | MUST                            |
   |----|---------------------|------------------------|---------------------------|------------------------------|---------------------------------|
   | 6  | DSA-NSEC3-SHA1      | MUST NOT               | MUST NOT                  | MUST NOT                     | MUST NOT                        |
   |----|---------------------|------------------------|---------------------------|------------------------------|---------------------------------|
   | 7  | RSASHA1-NSEC3- SHA1 | NOT RECOMMENDED        | RECOMMENDED               | NOT RECOMMENDED              | MUST                            |
   |----|---------------------|------------------------|---------------------------|------------------------------|---------------------------------|
   | 8  | RSASHA256           | RECOMMENDED            | RECOMMENDED               | MUST                         | MUST                            |
   |----|---------------------|------------------------|---------------------------|------------------------------|---------------------------------|
   | 10 | RSASHA512           | NOT RECOMMENDED        | RECOMMENDED               | NOT RECOMMENDED              | MUST                            |
   |----|---------------------|------------------------|---------------------------|------------------------------|---------------------------------|
   | 12 | ECC-GOST            | MUST NOT               | MAY                       | MUST NOT                     | MAY                             |
   |----|---------------------|------------------------|---------------------------|------------------------------|---------------------------------|
   | 13 | ECDSAP256SHA256     | RECOMMENDED            | RECOMMENDED               | MUST                         | MUST                            |
   |----|---------------------|------------------------|---------------------------|------------------------------|---------------------------------|
   | 14 | ECDSAP384SHA384     | MAY                    | RECOMMENDED               | MAY                          | RECOMMENDED                     |
   |----|---------------------|------------------------|---------------------------|------------------------------|---------------------------------|
   | 15 | ED25519             | RECOMMENDED            | RECOMMENDED               | RECOMMENDED                  | RECOMMENDED                     |
   |----|---------------------|------------------------|---------------------------|------------------------------|---------------------------------|
   | 16 | ED448               | MAY                    | RECOMMENDED               | MAY                          | RECOMMENDED                     |
   |----|---------------------|------------------------|---------------------------|------------------------------|---------------------------------|


#  DNSSEC Delegation Signer (DS) Resource Record (RR) Type Digest Algorithms Column Values

   Initial recommendation columns of use and implementation
   recommendations for the "DNSSEC Delegation Signer (DS) Resource
   Record (RR) Type Digest Algorithms" registry are shown in Table 3.

   When there are multiple RECOMMENDED algorithms in the "use" column,
   operators should choose the best algorithm according to local
   policy.

   |--------|-------------------|---------------------------|---------------------------|---------------------------------|---------------------------------|
   | Number | Mnemonics         | Use for DNSSEC Delegation | Use for DNSSEC Validation | Implement for DNSSEC Delegation | Implement for DNSSEC Validation |
   |--------|-------------------|---------------------------|---------------------------|---------------------------------|---------------------------------|
   | 0      | NULL (CDS only)   | MUST NOT \[*\]            | MUST NOT \[*\]            | MUST NOT \[*\]                  | MUST NOT \[*\]                  |
   |--------|-------------------|---------------------------|---------------------------|---------------------------------|---------------------------------|
   | 1      | SHA-1             | MUST NOT                  | RECOMMENDED               | MUST NOT                        | MUST                            |
   |--------|-------------------|---------------------------|---------------------------|---------------------------------|---------------------------------|
   | 2      | SHA-256           | RECOMMENDED               | RECOMMENDED               | MUST                            | MUST                            |
   |--------|-------------------|---------------------------|---------------------------|---------------------------------|---------------------------------|
   | 3      | GOST R 34.11-94   | MUST NOT                  | MAY                       | MUST NOT                        | MAY                             |
   |--------|-------------------|---------------------------|---------------------------|---------------------------------|---------------------------------|
   | 4      | SHA-384           | MAY                       | RECOMMENDED               | MAY                             | RECOMMENDED                     |
   |--------|-------------------|---------------------------|---------------------------|---------------------------------|---------------------------------|
   | 5      | GOST R 34.11-2012 | MAY                       | MAY                       | MAY                             | MAY                             |
   |--------|-------------------|---------------------------|---------------------------|---------------------------------|---------------------------------|
   | 6      | SM3               | MAY                       | MAY                       | MAY                             | MAY                             |
   |--------|-------------------|---------------------------|---------------------------|---------------------------------|---------------------------------|


#  Security Considerations

   This document makes no modifications to the security of the
   existing protocol or recommendations described in [RFC8624].  Thus,
   the security considerations remain the same, which we quote below.

   The security of cryptographic systems depends on both the strength of
   the cryptographic algorithms chosen and the strength of the keys used
   with those algorithms.  The security also depends on the engineering
   of the protocol used by the system to ensure that there are no non-
   cryptographic ways to bypass the security of the overall system.

   This document concerns itself with the selection of cryptographic algorithms
   for the use of DNSSEC, specifically with the selection of
   "mandatory-to-implement" algorithms.  The algorithms identified in this
   document as "MUST" or "RECOMMENDED" to implement are not known to be broken at
   the current time, and cryptographic research so far leads us to believe that
   they are likely to remain adequately secure unless significant and
   unexpected discovery is made. However, this isn't necessarily forever, and
   it is expected that future documents will be issued from time to time to
   reflect the current best practices in this area.

   Retiring an algorithm too soon would result in a zone signed with the
   retired algorithm being downgraded to the equivalent of an unsigned
   zone.  Therefore, algorithm deprecation must be done very slowly and
   only after careful consideration and measurement of its use.

#  Operational Considerations

   DNSKEY algorithm rollover in a live zone is a complex process.  See
   [RFC6781] and [RFC7583] for guidelines on how to perform algorithm
   rollovers.

   DS algorithm rollover in a live zone is also a complex process.
   Upgrading algorithm at the same time as rolling the new KSK key will
   lead to DNSSEC validation failures, and users MUST upgrade the DS
   algorithm first before rolling the Key Signing Key.

#  IANA Considerations

  The IANA is requested to update the [DNSKEY-IANA] and [DS-IANA] registries
  according to the following sections.

## Update to the "DNS Security Algorithm Numbers" registry

  This document requests IANA update the "DNS Security Algorithm
  Numbers" registry ([DNSKEY-IANA]) registry with the following
  additional columns:

  * "Use for DNSSEC Signing"
  * "Use for DNSSEC Validation"
  * "Implement for DNSSEC Signing"
  * "Implement for DNSSEC Validation"

  These values must be populated using values from Table 2 of this
  document.

  Additionally, the registration policy for the [DNSKEY-IANA] registry
  should match the text describing the requirements in this document.

## Update to the "Digest Algorithms" registry

  This document requests IANA update the "Digest Algorithms" registry
  ([DS-IANA]) registry with the following additional columns:

  * "Use for DNSSEC Delegation"
  * "Use for DNSSEC Validation"
  * "Implement for DNSSEC Delegation"
  * "Implement for DNSSEC Validation"

  These values must be populated using values from Table 3 of this
  document.

  * Update the registration policy for the [DS-IANA] registry to
    match the text describing update requirements above.
  * Mark values 128 - 252 as "Reserved"
  * Mark values 253 and 254 as "Reserved for Private Use"
  * Delete the (now superfluous) column "Status" from the registry

  Additionally, the registration policy for the [DS-IANA] registry
  should match the text describing the requirements in this document.

#  Acknowledgments

  This document is based on, and extends, RFC 8624, which was authored by Paul
  Wouters, and Ondrej Sury.

  The content of this document was heavily discussed by participants of the
  DNSOP working group.  The authors appreciate the thoughtfulness of the many
  opinions expressed by working group participants that all helped shaped this
  document. We thank Paul Hoffman and Paul Wouters for their contributed text,
  and also Shumon Huque, S Moonesamy, Peter Thomassen, Paul Wouters, Stefan
  Ubbink, and Loganaden Velvindron for their reviews and comments.



--- back

# ChangeLog

## Changes from ietf-05 to ietf-06

    * Address Eric Vyncke (RAD!) AD review comments.

## Changes from ietf-03 to ietf-05

    * Updated "entry requirements" to be "Specification Required".
    * Marked values 128 - 252 as "Reserved" in "Digest Algorithms" as
    break-glass mechanism in case we get a flood of these. To align with the
    "DNS Security Algorithm Numbers" registry (that reserves 123 - ...)
    * Marked values 253 and 254 as "Reserved for Private Use" in "Digest
    Algorithms"
    * Deleted the (now superfluous) column "Status" from the "Digest

## Changes from ietf-02 to ietf-03

   * Fixed the reference in the Abstract (no links in Abstracts)
   * Added Updates: to the header.

## Changes from ietf-01 to ietf-02

   * Changed the MUST values in the tables for the Use columns to
     RECOMMENDED based on discussions no the dnsop mailing list.

   * Other minor wording and formatting changes

## Changes from ietf-00 to ietf-01

   * Only NIT fixing

## Changes from hardaker-04 to ietf-00

   * Just a draft name and number change.

## Changes from -03 to -04

   * Changed the columns being added from 2 per table to 4, based on
     discussion within the dnsop working group mailing list.  This was
     a fairly major set of changes.

## Changes since RFC8624

   * The primary purpose of this revision is to introduce the new
     columns to existing registries.  It makes no changes to the
     previously defined values.

   * Merged in RFC9157 updates.

   * Set authors as Wes Hardaker, Warren Kumari.
