# Design for Minimum Viable Hard Fork - Bitcoin Unlimited edition


##1. Contents <a id="1-contents"></a>

1. [Contents](#1-contents)
2. [Introduction](#2-introduction)
3. [Overview of changes](#3-overview)
4. [Architecture](#4-architecture)
5. [Detailed design](#5-detailed-design)
6. [Requirements traceability](#6-req-traceability)

NOTE: This document is in a DRAFT state, meaning that certain sections are
incomplete. Nevertheless, those parts that have been described
adequately can serve developers who wish to implement changes needed
to be compatible.


##2. Introduction <a id="2-introduction"></a>

This software design document describes the high-level and detailed
design of a "Minimum Viable Hard Fork" (sometimes shortened to
"Minimum Viable Fork" and abbreviated as MVHF or MVF) based on
Bitcoin Unlimited (BU).

Bitcoin Unlimited is a client for the Bitcoin peer-to-peer electronic
cash and settlement system. Bitcoin Unlimited supports block sizes larger
than 1MB (the block size limit at the time of writing).

The system described in this document is denoted by the compound
abbreviation `MVF-BU`.


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


##2.4 Rationale <a id="2-4-rationale"></a>

MVF-BU is a 'spin-off' of the Bitcoin Unlimited software which triggers
a divergence (hard fork) from the current Bitcoin network once a certain
length of the blockchain is reached, ensuring that all blockchain data
recorded up to that point is preserved, but spinning off to create its
own blockchain by building further blocks on top of its own ledger copy
from that point onwards.

The reason for spinning off is that the current system limits the transaction
rate of the current system is limited by the consensus of a small minority
of Bitcoin users (the current miners) by virtue of their enforcement of
a restrictive 1MB blocksize limit. The developers of the most widely
used client (Bitcoin Core) have also refused for a number of years to
increase the limit and allow the Bitcoin transaction rate to grow
within the constraints of technological possibilities.
Eventually this has resulted in the creation of the BTCfork project
(https://bitcoinforks.org) which aims to show that a 'hard fork'
can be used to safely upgrade the block size limit and restore Bitcoin
on a growth path.

Consensus was reached within the project to implement a MVF with a
minimalistic set of changes which are nevertheless safe in terms of
protecting user funds against loss through preventable attacks.

Such an implementation would then be able to serve as a basis for others
who wished to implement other more feature-rich spin-offs (e.g. ones
which include a change to the POW) to the market.

The community strongly preferred a first MVF to be derived from the BU
client, as BU's 'emergent consensus' approach has the potential to
solve the block size issue once and for all. The MVF-BU seeks to address
this desire.


##2.5 Referenced Material <a id="2-5-referenced-material"></a>

1. MVF-BU Requirements Specification (`requirements.md`)

Other references to be added.


##2.6 Definitions and Acronyms <a id="2-6-definitions-and-acronyms"></a>

Item | Meaning
--- | ---
POW | proof of work
MVF | Minimum Viable Fork
HF | Hard Fork
SegWit | Segregated Witness
SW | Software
SF | Soft fork
BTC | Bitcoin
BU | Bitcoin Unlimited (a Bitcoin full node client software)



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

This section contains detailed design information on the MVF-BU changes.

To be completed: Elements of the design need to be given unique
identifiers, so that they can be traced back to requirements and
identified in the code.


###5.1 Fork Triggering (TRIG)

To be completed: fork triggering (changing between old/new consensus rules)


###5.2 Network Separation (NSEP)

To be completed: network separation actions


###5.3 Difficulty Adjustment (DIAD)

To be completed: difficulty adjustment (reset and algorithm to revert back to old behaviour)


###5.4 Change of Transaction Signatures (CSIG)

To be completed: signature change (for replay protection)


###5.5 Wallet Backup (WABU)

To be completed: automated backup of pre-fork wallet state


###5.5.1 Configuration for wallet backup (MVHF-BU-DES-WABU-1)

A new optional parameter (config file + command line argument) will be added
which will allow the user to specify a location (file path and name)
where WABU will create the wallet backup copy.

The name of the parameter shall be: WABU_PARAMETER_NAME_PLACEHOLDER

If WABU_PARAMETER_NAME_PLACEHOLDER is not configured, the default shall
be to store the wallet backup in the same location as the wallet.dat file
is stored on the user's system by default:

- UNIX-like systems: in the $HOME/.bitcoin/
- Windows: TODO
- Mac OSX: TODO
- others?: TODO

If no backup wallet filename is specified, then a default backup filename
will be used:

    wallet.dat.mvf-bu_<blocknumber>.bak

where <blocknumber> is the height of the last block processed before
the backup was made.

If WABU_PARAMETER_NAME_PLACEHOLDER is specified but does not include
path separators (OS-specific, e.g. forward slashes on UNIX, backward
slashes on Windows), then it is treated as a simple filename and
overrides the default backup filename schema above.

If the user does not specify a path in WABU_PARAMETER_NAME_PLACEHOLDER,
then the default wallet path is used, but if the client is running with
a non-standard `datadir`, then that `datadir` shall be used when creating
the backup.

If the specified configuration value includes path separators but the
last element does not correspond to an existing folder, then the last
element is interpreted as a backup filename and the backup will be
created at that specified location (path + filename).

For safety reasons no switch is provided to disable the wallet backup.
However, if the client is running without a wallet, no backup will be
attempted, but a warning log message will be entered to indicate that
wallet backup has been skipped.


###5.5.2 Triggering of the wallet file backup

The wallet backup will be initiated at the same time that the hard fork's
new consensus rules are activated.

TODO: This may be at the end of the processing of the block preceding the
MVF's fork height, or at the end of the block with said fork height. This
must be made more precise.

TODO: complete design info on the triggering implementation


###5.5.3 Procedure to create the wallet file backup

The procedure is roughly envisaged as follows:

Steps in wallet file backup procedure (tentative):
  1. close the existing wallet disk file
  2. attempt to open the backup wallet disk file for writing
  3. copy the existing wallet file into the backup file
  4. close the backup file
  5. re-open the existing wallet file (to resume further operation)

Note:

###5.6 Client identification (IDME)

To be completed: clear identification of the client to the user/network


##6. Requirements traceability <a id="6-req-traceability"></a>

To be completed: traceability matrix between SW requirements and design

Requires the design elements (in Detailed Design) to be identifiable so
that we can match them to requirements.
