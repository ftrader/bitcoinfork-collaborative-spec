# Design for Minimum Viable Hard Fork - Bitcoin Unlimited edition

##1. Contents <a id="1-contents"></a>

1. [Contents](#1-contents)
2. [Introduction](#2-introduction)
3. [Overview of changes](#3-overview)
4. [Architecture](#4-architecture)
5. [Detailed design](#5-detailed-design)
6. [Requirements traceability](#6-req-traceability)


##2. Introduction <a id="2-introduction"></a>

This software design document describes the high-level and detailed
design of a "Minimum Viable Hard Fork" (sometimes shortened to
"Minimum Viable Fork" and abbreviated as MVHF or MVF) based on
Bitcoin Unlimited (BU).

The system described in this document is denoted by the compound
abbreviation `MVF-BU`.

It is a 'spin-off' of the Bitcoin software which triggers a divergence
from the current Bitcoin system once a certain length of the blockchain
is reached, ensuring that all blockchain data recorded up to that point
are preserved, but spinning off to create its own blockchain by build
its own distinct blocks on top of the ledger from that point onwards.

Bitcoin Unlimited is a client for the Bitcoin peer-to-peer electronic
cash and settlement system. Bitcoin Unlimited supports block sizes larger
than 1MB (the block size limit at the time of writing).

The reason for spinning off is that the current system limits the transaction
rate by virtue of its restrictive 1MB blocksize limit, and its developers
(Bitcoin Core) have refused to increase the limit and allow the system
to grow freely for a number of years. Eventually this has resulted in a
decision by the BTCfork project [TODO: reference] to perform a 'hard fork'
in order to upgrade the block size limit.

A decision was taken to implement an MVF which should represent a
minimalistic set of changes which are nevertheless safe in terms of
protecting user funds against loss through preventable attacks.

The community strongly preferred a first MVF to be derived from the BU
client, as BU's 'emergent consensus' approach has the potential to
solve the block size issue once and for all. The MVF-BU seeks to address
this community desire.


##2.1 Purpose <a id="2-1-purpose"></a>

The purpose of this document is:

1. to give software developers an overview of the architecture and
sufficient detail information to fully understand the MVF-BU design

2. to provide traceability between requirements and design

3. to enable traceability from design to the MVF-BU components in the
source code


##2.2 Scope <a id="2-2-scope"></a>

This document covers the design for the software requirements
specified for MVF-BU (refer to the `MVHF-BU-SW-*` requirements in the
`requirements.md` document).

Since no standardized formal requirements specifications and design
documents exist at this time for the Bitcoin system and its various
implementations (including Satoshi-derived clients like Bitcoin Core
and Bitcoin Unlimited), the MVF only covers the requirements for the
functionality which it changes (delta to existing implementation).

Where feasible and deemed important to the reader's understanding,
contextual design information may be given to situate the MVF changes.

The following aspects of the MVF will be described:

- software architecture changes
- interface changes
- functionality changes
- performance considerations


##2.3 Intended Audience <a id="2-3-intended-audience"></a>

This document is addressed to software developers familiar with Bitcoin
who work on or with the Bitcoin software or connecting systems.

This includes the following groups, among others:

- core Bitcoin software developers
- mining pool developers
- exchange developers
- wallet developers


##2.4 Referenced Material <a id="2-4-referenced-material"></a>

1. MVF-BU Requirements Specification (`requirements.md`)

Other references to be added.


##2.5 Definitions and Acronyms <a id="2-5-definitions-and-acronyms"></a>



##3. Overview of changes <a id="3-overview"></a>

The MVF-BU changes can be grouped into functional categories, according
to the requirements.

These have been listed allow, with assigned acronyms that will allow them
to be recognized easily in code and documentation references.

Category acronym | Description
--- | ---
TRIG | fork triggering (changing between old/new consensus rules)
NSEP | network separation actions
DIAD | difficulty adjustment (reset and algorithm to revert back to old behaviour)
CSIG | signature change (for replay protection)
WABU | wallet backup
IDME | clear identification of system

The acronyms are used in design element identifiers in this document,
and have been picked

##4. Architecture <a id="4-architecture"></a>


##4.1 The Bitcoin system

To be completed:

High-level description of what is a Bitcoin system, relationship of fork
clients and non-fork clients in the network before and after the trigger,
high-level description of changes in network, protocol, blockchain data,
(others?)


##4.2 Structure of the MVF-BU client software

To be completed:

Should indicate the main static and dynamic parts of the system
(software components, processes etc), and which are being affected by
the proposed design changes.


##5. Detailed design <a id="5-detailed-design"></a>

To be completed.

Detailed design information on all the changes.
Elements of the design need to be given unique identifiers, so that they
can be traced back to requirements, and identified in the code.


###5.x Wallet Backup (WABU)

(this is a temporary section to hold some preliminary ideas on this.
please update as needed)


###5.x.y Procedure to create the wallet file backup

The procedure is roughly envisaged as follows:

Steps in wallet file backup procedure (tentative):
  1. close the existing wallet disk file
  2. attempt to open the backup wallet disk file for writing
  3. copy the existing wallet file into the backup file
  4. close the backup file
  5. re-open the existing wallet file (to resume further operation)


##6. Requirements traceability <a id="6-req-traceability"></a>

To be completed: traceability matrix between SW requirements and design

Requires the design elements (in Detailed Design) to be identifiable so
that we can match them to requirements.
