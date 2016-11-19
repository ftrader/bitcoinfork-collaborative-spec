# Design for Minimum Viable Hard Fork - Bitcoin Core edition


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
Bitcoin Core.

Bitcoin Core is a client for the Bitcoin peer-to-peer electronic
cash and settlement system. Bitcoin Core supports block sizes up to 1MB
(the block size limit) at the time of writing.

The system described in this document is denoted by the compound
abbreviation `MVF-Core`.


##2.1 Purpose <a id="2-1-purpose"></a>

The purpose of this document is:

1. to give software developers an overview of the architecture and
sufficient detail information to fully understand the MVF-Core design

2. to provide traceability between requirements and design

3. to enable traceability from design to the MVF-Core components in the
source code


##2.2 Scope <a id="2-2-scope"></a>

This document covers the design for the software requirements
specified for MVF-Core (refer to the `MVHF-CORE-SW-*` requirements in the
`requirements.md` document).

Since no standardized formal requirements specifications and design
documents exist at this time for the Bitcoin system and its various
implementations (including Satoshi-derived clients like Bitcoin Core),
the MVF only covers the requirements for the functionality which it
changes (delta to existing implementation).

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

MVF-Core is a 'spin-off' of the Bitcoin Core software which triggers
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

The MVF-Core seeks to address this desire for minimality of changes
with respect to the most widely used client (Core at the time of writing).


##2.5 Referenced Material <a id="2-5-referenced-material"></a>

1. MVF-Core Requirements Specification (`requirements.md`)

Other references to be added.


##2.6 Definitions and Acronyms <a id="2-6-definitions-and-acronyms"></a>

Item | Meaning
--- | ---
BTC | Bitcoin
HF | Hard Fork
HW | Hardware
MVF | Minimum Viable Fork
POW | proof of work
SegWit | Segregated Witness
SF | Soft fork
SW | Software



##3. Overview of changes <a id="3-overview"></a>

The MVF-Core changes can be grouped into functional categories, according
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


##4.2 Structure of the MVF-Core client software

To be completed:

Should indicate the main static and dynamic parts of the system
(software components, processes etc), and which are being affected by
the proposed design changes.


##5. Detailed design <a id="5-detailed-design"></a>

This section contains detailed design information on the MVF-Core changes.

To be completed: Elements of the design need to be given unique
identifiers, so that they can be traced back to requirements and
identified in the code.


###5.1 Fork Triggering (TRIG)

To be completed: fork triggering (changing between old/new consensus rules)


####5.1.1 Extracted common definitions (MVHF-CORE-DES-TRIG-1)

Certain TRIG-related parameters and functions that are needed in
other files will be extracted into the MVF-Core specific files:

- mvf-core.h
- mvf-core.cpp

The TRIG-related parameters to be placed into the common files:

- block triggering heights for the various networks (mainnet, testnet, regtestnet, nolnet)


####5.1.2 Addition of BIP9 SegWit parameters (MVHF-CORE-DES-TRIG-2)

BIP9 deployment parameters for SegWit soft-fork activation will be added in
params.h / chainparams.cpp .

SegWit (BIP141/143/147) deployment parameters have been set as follows:
(https://github.com/bitcoin/bitcoin/pull/8937)

- versionbit 1
- start time: midnight 15 november 2016 UTC (Epoch timestamp 1479168000)
- timeout: midnight 15 november 2017 UTC (Epoch timestamp 1510704000)


####5.1.3 New consensus parameters for fixed trigger height (MVHF-CORE-DES-TRIG-3)

A new consensus parameter, nMVFActivateForkHeight, will be added in
params.h / chainparams.cpp, and initialized with the default trigger heights
according to each network, as defined in mvf-core.h .

An access function MVFActivateForkHeight() return a constant object
will also be provided. (TODO: not needed?)


####5.1.4 New global variable for command line trigger height (MVHF-CORE-DES-TRIG-4)

A new command line/configuration file parameter, `forkheight`, will be added.
It will be initialized with the default trigger height according to the
active network, as determined at runtime.
The value of forkheight will be subject to minimum fork height checks
(validity checks) upon startup. Specifying an invalid value will lead to
non-startup (i.e. controlled shutdown of the application during startup).
The active fork height value will be stored in a global variable
(FinalActivateForkHeight) accessible from various parts of the code.


####5.1.5 New variables and config file for tracking fork activation states

#####5.1.5.1 New global variables (MVHF-CORE-DES-TRIG-5)

A new boolean global variable, isMVFHardForkActive, will be added to
track the present activation state of the fork's new consensus rules.
It will be initialized to false and set to true when the block height of
the active chain is at least equal to `FinalActivateForkHeight` or when
the SegWit soft-fork activates.
When the variable transitions to true, fork activation actions will be
performed.
If a re-org happens and the height of the active chain drops below the
`FinalActivateForkHeight`, fork deactivation actions will be performed
(i.e. reversion to pre-fork consensus rules).

Another new boolean global variable, wasMVFHardForkPreviouslyActivated,
will be added for tracking the activation state of the fork across
program restarts.
It will be initialized to false and set to true only if a btcfork.conf
configuration file is found at startup (ref TODO).
If this variable is set to true, certain one-off fork activation actions
(e.g. network separation and the automatic wallet backup) will be skipped.


#####5.1.5.2 btcfork.conf configuration file (MVHF-CORE-DES-TRIG-10)

Certain forking actions, such as separating to a different network,
should be performed only once and persisted across restarts of the
client.

The software will not modify the user's existing configuration file
(bitcoin.conf) automatically as that would be too risky (some users
re-use the same configuration file for several clients).

Instead, when the fork activates, the client will write out fork-relevant
configuration items into a separate configuration file (btcfork.conf) in
the datadir.

If this file is present during startup, the client software will know
that the fork has been activated already.

Primarily, this will be used to connect to the new (forked) network instead
of the old network.

The software shall also warn the user at startup that certain fork
configuration parameters can be integrated (manually) into the existing
configuration file (bitcoin.conf), whereupon the btcfork.conf file may
be removed (since the node is past the fork activation point, another fork
activation will not be performed unless non-default fork parameters are
specified by the user via command line or the configuration file.


####5.1.6 Fork activation procedure (MVHF-CORE-DES-TRIG-6)

TODO: elaborate

1. network separation actions (NSEP)
2. difficulty reset and initialization of recovery algorithm (DIAD)
3. activation of modified SIGHASH values (CSIG)
4. set isMVFHardForkActive to true


####5.1.7 Fork deactivation procedure (MVHF-CORE-DES-TRIG-7)

To be completed. It is anticipated that no network separation actions
need to be undone (if anything, the whole fork network performs a re-org
to below the fork height together).
This leaves the consensus rules that may need to be reverted to pre-fork
conditions.

TODO: elaborate

1. difficulty back to pre-fork, reset of recovery algorithm (DIAD)
2. revert to pre-fork SIGHASH values (CSIG)
3. set isMVFHardForkActive to false


####5.1.8 Help information about new configuration parameters (MVHF-CORE-DES-TRIG-8)

Help text listing the MVF-Core specific new parameters and their allowed
values will be added to the output of the `--help` option, as is customary.


####5.1.9 Output fork trigger information on `getblockchaininfo` RPC call (MVHF-CORE-DES-TRIG-9)

The `getblockinfo` RPC call will be extended to provide a `hardforks`
entry with the following info:

- id: name of fork, value 'mvhf' can be kept common across MVF clients
- forkheight: the configured block height (can be set using `-forkheight`)
- forkid: the configured fork id (can be set using `-forkid`)
- blocks_remaining: how many blocks left to the fork height
- segwit_status: the activation status of the SegWit soft-fork

Additionally, the `softforks` section will be extended with the details on
the SegWit soft-fork in the same way that Core provides this information.


###5.2 Network Separation (NSEP)

To be completed: network separation actions


####5.2.1 Extracted common definitions (MVHF-CORE-DES-NSEP-1)

The following NSEP-related parameters and functions that are needed in
other files will be extracted into MVF-Core specific common files:

- default value for post-fork network port

See 5.1.1 (TODO: check reference accuracy) for description of the common files.


###5.3 Difficulty Adjustment (DIAD)

As a non-elective spin-off, the MVF finds itself in the situation of a
possible minority hard fork as far as hashpower is concerned.

The difficulty adjustment will be modified as follows to cope with this
circumstance:

- there will be a reset to a lowered difficulty when the fork activates
- the interval at which difficulty is re-evaluated will be shortened
  when the fork activates
- for a short time after the fork activates, a larger difficulty adjustment
  than usual will be allowed (in fact, an unconstrained adjustment, only
  bounded by the POW limit)
- for a fixed period of time, significantly shortened difficulty retargeting
  intervals (of the order of hours instead of weeks) will be in effect

Together, these changes should allow sufficient responsiveness to large
changes in network hashpower.

NOTE: the algorithm still targets 600 seconds average block confirmation time
(this property of Bitcoin is not changed by the MVF).

The retargeting algorithm will revert to its pre-fork behavior after a
span of 180*144 post-fork blocks. That equals roughly half a year, with
deviations due to probable error in estimating initial hashrate, unpredictable
changes in network hashrate etc.

After that, the usual 2016 block retargeting period will take effect again,
and difficulty adjustments likewise will be constrained as before.


####5.3.1 Extracted common definitions (MVHF-CORE-DES-DIAD-1)

The following DIAD-related parameters and functions that are needed in
other files will be extracted into the MVF-Core specific common files:

- difficulty reset value for various networks (mainnet, testnet, regtestnet, nolnet)

See 5.1.1 (TODO: check reference accuracy) for description of the common files.


####5.3.2 Reset of difficulty at fork activation time (MVHF-CORE-DES-DIAD-2)

The lowered difficulty values have not yet been finally decided.

On regtest network, the POW limit (~ nBits=0x207fffff) will be used.


####5.3.3 Faster retargeting following fork activation (MVHF-CORE-DES-DIAD-3)

For a limited number of blocks following the fork activation,
special retargeting and difficulty adjustment rules will be in effect.

If the fork activates at height A, retargeting will occur according to the
following schedule:
- Blocks A+1..A+10: retarget every block (~10 mins)
- Blocks A+11..A+43: retarget every 3 blocks (~30 mins)
- Blocks A+44..A+101: retarget every 6 blocks (~1hr)
- Blocks A+102..A+2011: retarget every 18 blocks ~(3hrs)
- Blocks A+2012..A+(180*144)-1: retarget every 72 blocks ~(12hrs)
- Blocks A+(180*144)..onward: back to every 2016 blocks (~2 weeks)

A new function, MVFPowTargetTimespan(Height) will return the targeting
timespan (in seconds) for a given block height past the fork activation.
This will be used in DifficultyAdjustmentInterval() to return
a height-dependent number of blocks instead of the fixed 2016.

To facilitate this, DifficultyAdjustmentInterval() will be extended to
take a height parameter and return the MVF-specific interval while
in the fork recovery period.


####5.3.4 Recovery of retargeting to at fork activation time (MVHF-CORE-DES-DIAD-4)

Difficulty retargeting and adjustment will revert to their pre-fork
behavior when a pre-set number of blocks ("end of retargeting recovery")
past the fork activation point is reached.


####5.3.5 Free difficulty adjustment during per-block retargeting (MVHF-CORE-DES-DIAD-5)

Usually, changes in difficulty are constrained by a "factor of 4" check
to prevent very rapid swings.

During the time directly after the fork triggers, the difficulty needs to
adapt very rapidly to the actual hashrate, in order to converge back to
the target timespan of average 10 minutes per block.

Therefore, as long as the retargeting interval is equal to one block,
the difficulty will be allowed to adjust freely.


####5.3.6 --force-retarget parameter for regtest testing (MVHF-CORE-DES-DIAD-6)

To be able to test the difficulty adjustment during MVF retargeting on
regtest network, it is necessary to override the logic that usually
short-circuits the full difficulty calculation on regtest.

A new command line parameter, `--force-retarget`, will be added to enable
the full difficulty calculations so that system tests can use it to test
the retargeting.
This parameter will have no side effect when used on non-regtest networks.


####5.3.7 `difficultyadjinterval` field for `getblockchaininfo` RPC (MVHF-CORE-DES-DIAD-7)

To facilitiate verification of the difficulty adjustment intervals before,
during and after the fork's retargeting recovery period, a new entry
`difficultyadjinterval` entry will be added to the output of the
`getblockchaininfo` RPC call.

TODO: would it be useful to have another entry indicating how many blocks
remain until the next difficulty retarget?


###5.4 Change of Transaction Signatures (CSIG)

To be completed: signature change (for replay protection)


####5.4.1 Extracted common definitions (MVHF-CORE-DES-CSIG-1)

The following CSIG-related parameters and functions that are needed in
other files will be extracted into the MVF-Core specific common files:

- default value for fork ID (mask which is ORed with SIGHASH constants)

See 5.1.1 (TODO: check reference accuracy) for description of the common files.


####5.4.2 Parameter validation (MVHF-CORE-DES-CSIG-2)

At startup, the fork id must be validated to be in the range 0 ... 0xFFFFFF .


###5.5 Wallet Backup (WABU)

To be completed: automated backup of pre-fork wallet state


####5.5.1 Extracted common definitions (MVHF-CORE-DES-WABU-1)

The following WABU-related parameters and functions that are needed in
other files will be extracted into the MVF-Core specific common files:

- autoWalletBackupSuffix: the default wallet backup filename suffix

See 5.1.1 (TODO: check reference accuracy) for description of the common files.


####5.5.2 Configuration for wallet backup (MVHF-CORE-DES-WABU-2)

Two new new optional parameters will be added (config file + command line
arguments) which will allow the user to specify

1. `autobackupwalletpath`: the location (file path and/or name) of wallet backup
2. `autobackupblock`: the block number at which to perform the backup

The `autobackupblock` will default to `(forkheight-1)`, i.e. the block
before the fork activates and the new consensus rules come into force.

If `autobackupwalletpath` is not configured, the default shall
be to store the wallet backup in the same location as the wallet.dat file
is stored on the user's system by default:

- UNIX-like systems: in the $HOME/.bitcoin/
- Windows: TODO
- Mac OSX: TODO
- others?: TODO

If no backup wallet filename is specified, then a default backup filename
will be used:

    wallet.dat.auto.<blocknumber>.bak

where `<blocknumber>` is the height of the last block processed before
the backup was made.

If `autobackupwalletpath` is specified but does not include
path separators (OS-specific, e.g. forward slashes on UNIX, backward
slashes on Windows), then it is treated as a simple filename and
overrides the default backup filename schema above.

If the user does not specify a path in `autobackupwalletpath`,
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


####5.5.3 Triggering of the wallet file backup (MVHF-CORE-DES-WABU-3)

The automatic wallet backup is enabled by default. The wallet backup will
be initiated just before the hard fork's new consensus rules are activated.
This is at the end of the processing of the block preceding the fork
block height (FinalActivateForkHeight). So the automatic backup triggers
after the (fork block - 1) becomes the best block and before the fork block.
This is configured by the default value for `-autobackupblock`.
After the fork this parameter could be used to trigger future backups by
overriding with a custom value.

The default name for the backup file is the same as the current file with
a new suffix: ".auto.@.bak" where @ is the `-autobackupblock` value.
Otherwise the filename maybe customized via a new command line parameter
called `-autobackupwalletpath`.


####5.5.4 Procedure to create the wallet file backup (MVHF-CORE-DES-WABU-4)

TODO: adapt this description to what actually takes place

The procedure is roughly envisaged as follows:

Steps in wallet file backup procedure (tentative):
  1. close the existing wallet disk file
  2. attempt to open the backup wallet disk file for writing
  3. copy the existing wallet file into the backup file
  4. close the backup file
  5. re-open the existing wallet file (to resume further operation)


####5.5.5 Shutdown in case of wallet backup failure (MVHF-CORE-DES-WABU-5)

To be completed.

At the moment, a runtime error is generated to crash the application.
It seems that an orderly shutdown using StartShutdown() might run a
risk of further writes to the wallet. More investigation is needed.

###5.6 Client identification (IDME)

NOTE: Care must be taken to preserve attribution to 'The Bitcoin Core
developers' and any comment strings referring to 'Bitcoin Core' where
it would not be appropriate to replace 'Bitcoin Core' with MVF-Core.


###5.6.1 Command line client identification (MVHF-CORE-DES-IDME-1)

Current command line client identify themselves when called with
`--version` or `--help` (except `bitcoin-tx`, which does not understand the
`--version` option).

The following table shows how the output should be modified:

Program     | Core  | MVF-Core
---         | --- | ---
bitcoind    | `Bitcoin Core Daemon version v0.12.1.0-b0dfbf1` | `Bitcoin MVF-Core Daemon version v0.12.1.0-b0dfbf1`
bitcoin-cli | `Bitcoin Core RPC client version v0.12.1.0-b0dfbf1` | `Bitcoin MVF-Core RPC client version v0.12.1.0-b0dfbf1`
bitcoin-tx  | `Bitcoin Core bitcoin-tx utility version v0.12.1.0-b0dfbf1` | `Bitcoin MVF-Core bitcoin-tx utility version v0.12.1.0-b0dfbf1`


###5.6.2 GUI client identification (MVHF-CORE-DES-IDME-2)

In the following locations in the GUI, the designation 'Bitcoin Core'
will be replaced by 'Bitcoin MVF-Core':

1. splash screen

2. main window title bar

3. 'About' menu entry under the 'Help' menu

4. 'About' dialog window (modify window title and version string)

5. 'Information' pane of the 'Help->Debug' window (modify client name and user agent)

6. 'Command line options' dialog under the 'Help' menu (modify client name)

7. dialog windows

8. in the system tray icon

Where version information follows after the client name, this format shall be
retained.


###5.6.3 Identification on the network (MVHF-CORE-DES-IDME-3)

The `Satoshi` in the BU user agent string shall be replaced with
`MVF-Core`, e.g.

Core: `/Satoshi:0.12.1`
MVF-Core: `/MVF-Core:0.12.1`

The `getnetworkinfo` RPC call output shall be changed in similar manner:

Core: `/Satoshi:x.x.x/`
MVF-Core: `/MVF-Core:x.x.x`

###5.6.4 Identification in log file messages


####5.6.4.1 Startup message (MVHF-CORE-DES-IDME-4)

TODO: verify this is suitable

The client shall identify with the same string that the GUI 'Help->Debug'
window displays for 'client name'.


####5.6.4.2 Debug messages (MVHF-CORE-DES-IDME-5)

New or modified debug traces added in the software will use `MVF` as a
prefix. 'Core' can be omitted because it will be visible from the beginning
of the log file that the client is 'MVF-Core'.


###5.6.5 Identification in package/mountable disk volumes (MVHF-CORE-DES-IDME-6)

The identification in the Mac OSX diskimage volume label will be changed
from `Bitcoin-Core` to `Bitcoin-MVF-Core`.

TODO: any other packages that need renaming?


##6. Requirements traceability <a id="6-req-traceability"></a>

To be completed: traceability matrix between SW requirements and design

Requires the design elements (in Detailed Design) to be identifiable so
that we can match them to requirements.


###6.1 Requirements -> design

TODO: the list of SW requirements is still incomplete as they are
still under elaboration.

Each SW requirement should be covered by at least one design element,
but there can be more than one design elements for a requirement.

Requirement | Design elements
--- | ---
MVHF-CORE-SW-REQ-1-1 | MVHF-CORE-DES-TRIG-1,MVHF-CORE-DES-TRIG-3,MVHF-CORE-DES-CSIG-1,MVHF-CORE-DES-CSIG-2
MVHF-CORE-SW-REQ-1-2 | MVHF-CORE-DES-TRIG-5,MVHF-CORE-DES-TRIG-7
MVHF-CORE-SW-REQ-1-3 | MVHF-CORE-DES-TRIG-8
MVHF-CORE-SW-REQ-2-1 | MVHF-CORE-DES-TRIG-3,MVHF-CORE-DES-TRIG-4,MVHF-CORE-DES-TRIG-6
MVHF-CORE-SW-REQ-2-2 | MVHF-CORE-DES-TRIG-2,MVHF-CORE-DES-TRIG-6
MVHF-CORE-SW-REQ-2-3 | MVHF-CORE-DES-TRIG-5
MVHF-CORE-SW-REQ-2-4 | MVHF-CORE-DES-TRIG-9
MVHF-CORE-SW-REQ-2-5  | MVHF-CORE-DES-TRIG-10
MVHF-CORE-SW-REQ-10-1 | MVHF-CORE-DES-WABU-1,MVHF-CORE-DES-WABU-2
MVHF-CORE-SW-REQ-10-2 | MVHF-CORE-DES-WABU-3
MVHF-CORE-SW-REQ-10-3 | MVHF-CORE-DES-WABU-4
MVHF-CORE-SW-REQ-10-4 | MVHF-CORE-DES-WABU-4
MVHF-CORE-SW-REQ-10-5 | MVHF-CORE-DES-WABU-5
MVHF-CORE-SW-REQ-11-1 | MVHF-CORE-DES-IDME-1,MVHF-CORE-DES-IDME-2,MVHF-CORE-DES-IDME-3,MVHF-CORE-DES-IDME-4,MVHF-CORE-DES-IDME-6
MVHF-CORE-SW-REQ-11-2 | MVHF-CORE-DES-IDME-5
TODO (software reqs) | MVHF-CORE-DES-NSEP-1
MVHF-CORE-SW-REQ-7-1 | MVHF-CORE-DES-DIAD-1,MVHF-CORE-DES-DIAD-2
MVHF-CORE-SW-REQ-8-1 | MVHF-CORE-DES-DIAD-1,MVHF-CORE-DES-DIAD-3,MVHF-CORE-DES-DIAD-4,MVHF-CORE-DES-DIAD-6
MVHF-CORE-SW-REQ-8-2 | MVHF-CORE-DES-DIAD-5
MVHF-CORE-SW-REQ-8-3 | MVHF-CORE-DES-DIAD-7


###6.2 Design -> requirements

No design element should be untraceable - each one should lead back to
at least one SW requirement.

A design element could potentially cover multiple requirements, but
this should be looked at carefully. With MVF-Core it should be highly
unlikely to happen.

Design element(s) | Software requirement
--- | ---
MVHF-CORE-DES-IDME-1,MVHF-CORE-DES-IDME-2,MVHF-CORE-DES-IDME-3,MVHF-CORE-DES-IDME-4,MVHF-CORE-DES-IDME-6 | MVHF-CORE-SW-REQ-11-1
MVHF-CORE-DES-IDME-5 | MVHF-CORE-SW-REQ-11-2
MVHF-CORE-DES-TRIG-1,MVHF-CORE-DES-CSIG-1,MVHF-CORE-DES-CSIG-2 | MVHF-CORE-SW-REQ-1-1
MVHF-CORE-DES-TRIG-2 | MVHF-CORE-SW-REQ-2-2
MVHF-CORE-DES-TRIG-3 | MVHF-CORE-SW-REQ-1-1,MVHF-CORE-SW-REQ-2-1
MVHF-CORE-DES-TRIG-4 | MVHF-CORE-SW-REQ-2-1
MVHF-CORE-DES-TRIG-5 | MVHF-CORE-SW-REQ-1-2,MVHF-CORE-SW-REQ-2-3
MVHF-CORE-DES-TRIG-6 | MVHF-CORE-SW-REQ-2-1,MVHF-CORE-SW-REQ-2-2
MVHF-CORE-DES-TRIG-7 | MVHF-CORE-SW-REQ-1-2
MVHF-CORE-DES-TRIG-8 | MVHF-CORE-SW-REQ-1-3
MVHF-CORE-DES-TRIG-9 | MVHF-CORE-SW-REQ-2-4
MVHF-CORE-DES-TRIG-10| MVHF-CORE-SW-REQ-2-5
MVHF-CORE-DES-WABU-1,MVHF-CORE-DES-WABU-2 | MVHF-CORE-SW-REQ-10-1
MVHF-CORE-DES-WABU-3 | MVHF-CORE-SW-REQ-10-2
MVHF-CORE-DES-WABU-4 | MVHF-CORE-SW-REQ-10-3,MVHF-CORE-SW-REQ-10-4
MVHF-CORE-DES-WABU-5 | MVHF-CORE-SW-REQ-10-5
MVHF-CORE-DES-NSEP-1 | TODO (software reqs)
MVHF-CORE-DES-DIAD-1 | MVHF-CORE-SW-REQ-7-1,MVHF-CORE-SW-REQ-8-1
MVHF-CORE-DES-DIAD-2 | MVHF-CORE-SW-REQ-7-1
MVHF-CORE-DES-DIAD-3,MVHF-CORE-DES-DIAD-4,MVHF-CORE-DES-DIAD-6 | MVHF-CORE-SW-REQ-8-1
MVHF-CORE-DES-DIAD-5 | MVHF-CORE-SW-REQ-8-2
MVHF-CORE-DES-DIAD-7 | MVHF-CORE-SW-REQ-8-3
---

