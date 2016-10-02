# Requirements for Minimum Viable Hard Fork - Bitcoin Unlimited edition

Draft of Minimum Viable Hard Fork based on Bitcoin Unlimited

##1. Contents <a id="1-contents"></a>

1. [Contents](#1-contents)
2. [User requirements](#2-user-reqs)
3. [System requirements](#3-sys-reqs)
4. [Software requirements](#4-sw-reqs)

##2. User requirements <a id="2-user-reqs"></a>

    Requirement:    MVHF-BU-USER-REQ-1

    Origin:         BTCfork

    Type:           Functional

    Title:          NON-ELECTIVE HARD FORK TO SEPARATE CHAIN

    Text:           The fork shall enforce the creation of its own chain by
                    separating from the existing chain upon the triggering of a
                    non-elective condition.

    Rationale:      A non-elective hard fork is needed as the majority of the
                    mining power is now under the control of a few individuals
                    who have refused to upgrade the block size limit.

    Notes:          "its own chain" means that post-trigger, the existing chain
                    will no longer accept blocks of the forked chain and vice
                    versa. The trigger condition is described by MVHF-BU-USER-REQ-2.

    Traceability:   MVHF-BU-SYS-REQ-1
---
    Requirement:    MVHF-BU-USER-REQ-2

    Origin:         BTCfork

    Type:           Functional

    Title:          TRIGGER CONDITION

    Text:           The fork shall trigger at a specified block height or upon
                    SegWit activation (95%), whichever is reached first.

    Rationale:      SegWit activation allows for blocks with SegWit
                    transactions on the network, and would require clients
                    to be able to process those blocks according to SegWit
                    rules. This would require the SegWit functionality to
                    be retained going forward in order to be able to validate
                    the ledger. SegWit functionality is not seen as part of
                    the requirements for an MVF.

    Notes:          1. Satoshi described a hard fork at a predetermined block height
                    as a possible mechanism to allow bigger blocks again in future.
                    A Unlimited-derived MVHF implementation including the additional
                    SegWit trigger would not need the main SegWit functionality,
                    only the BIP9 activation logic and parameters compatible with
                    BIP141 deployment.
                    2. Once SegWit is released, the final code needs to be
                    inspected to ensure that Core's implementation conforms
                    to BIP9 in the sense that the 95% threshold is respected
                    and it does not allow SW transactions to be relayed or
                    mined into blocks prior to the SegWit soft-fork reaching
                    ACTIVE state. Should this not be the case, the SegWit
                    trigger condition in this requirement may need to be
                    adjusted.

    Traceability:   MVHF-BU-SYS-REQ-2
---
    Requirement:    MVHF-BU-USER-REQ-3

    Origin:         BTCfork

    Type:           Functional

    Title:          PERMIT BLOCK SIZE TO EXCEED ONE MEGABYTE

    Text:           Upon triggering, the fork shall enable blocks greater than
                    1,000,000 bytes.

    Rationale:      The historical limit of 1,000,000 was put in as an
                    anti-spam mechanism by Satoshi, not with the intention of
                    limiting growth of Bitcoin. The refusal by the existing
                    developers (Core) and a majority of miners to raise this
                    limit (even to 2MB) is what is motivating a hard fork
                    (and this requirement in particular) to overcome this
                    limitation and restore growth.

    Notes:          The regulation of block size in Bitcoin Unlimited is
                    subject to emergent consensus.
                    This requirement can be validated using a majority of
                    miners configured to generate blocks > 1MB, and a
                    majority of the nodes supporting relay of such blocks
                    according to their Excessive Block Size setting.

    Traceability:   MVHF-BU-SYS-REQ-3
---
    Requirement:    MVHF-BU-USER-REQ-4

    Origin:         BTCfork

    Type:           Functional

    Title:          ENABLE BLOCK SIZE OF TWO MEGABYTES

    Text:           Upon triggering, the fork shall enable a block size
                    of 2,000,000 bytes.

    Rationale:      A block size of 2MB is considered safe by all parties
                    (even the planned SegWit solution could effectively result
                    in up to 4MB aggregate size of witness + non-witness data).
                    The Cornell study [1] has indicated that even sizes of up
                    to 4MB would be safe (at the time of the study - it is
                    likely that even greater sizes would be acceptable now).
                    Raising the accepted block size to 2MB is the simplest and most
                    unproblematic immediate change, would relieve the urgent
                    block size pressure.
                    If node operators on the fork network think it is safe to
                    raise the accepted block sizes, they can simply adjust
                    their client configurations after the fork has happened.
                    A conservative 2MB setting as a first step would also be
                    suitable for clients which have not yet implemented
                    improved block propagation protocols such as Bitcoin
                    Unlimited's Xtreme Thinblocks or Core's Compact Blocks.
                    As such it would make the MVF more accessible to wider
                    implementation in alternate clients (btcd + others).

    Notes:          1. Enabling a block size of 2MB implies that blocks
                    up to this size can be mined and relayed without hindrance
                    by MVF nodes. This can be achieved by appropriate setting
                    of the default values for the EBS and MGS parameters,
                    if necessary enforcing 2MB as a minimum value for
                    the EBS and MGS parameters from the fork trigger onwards.
                    It does not mean that 2MB is a hard limit from then on -
                    there is no hard limit in a Bitcoin Unlimited network
                    where accepted block size is regulated by EBS/AD
                    settings on relaying nodes.
                    2. As soon as the fork has happened, users of MVF will be
                    in control of further evolution of the block size, by
                    configuring the appropriate parameters (EBS, AD, MGS etc).
                    3. It is not established whether block sizes greater than 4MB
                    would require protection in the form of a dynamic cap against
                    attempts to eliminate full nodes from the network through
                    persistent spam attacks (high volume of own transactions).
                    Mitigations against such selfish spamming are possible, but
                    for simplicity the MVF should stick to proven technology and
                    use a safe enough initial consensus value for the post-fork
                    block size.
                    [1] http://fc16.ifca.ai/bitcoin/papers/CDE+16.pdf

    Traceability:   MVHF-BU-SYS-REQ-4
---
    Requirement:    MVHF-BU-USER-REQ-5

    Origin:         BTCfork

    Type:           Functional

    Title:          CLEAN NETWORK SEPARATION

    Text:           Upon triggering, the fork shall separate from the current
                    Bitcoin network, to prevent as much unnecessary processing as
                    possible on both sides.

    Rationale:      Clean network separation is necessary to prevent processing
                    of old-chain protocol data (transactions and blocks) once
                    the fork has triggered. This would prevents unnecessary
                    computations and second-order consequences (like node
                    banning).

    Notes:          This is not currently done by any of the current Bitcoin hard
                    fork clients (BIP109-compatible implementations).
                    If peer connections are closed and re-opened during the fork,
                    there may be some risk of interference during the separation
                    manoeuvre.

    Traceability:   MVHF-BU-SYS-REQ-5
---
    Requirement:    MVHF-BU-USER-REQ-6

    Origin:         BTCfork

    Type:           Functional

    Title:          NEW SEEDS

    Text:           Upon triggering, the fork shall switch to using a new set of
                    DNS seeds.

    Rationale:      The current DNS seed nodes, if used by the fork after
                    it has triggered, would lead to undue interference with
                    full nodes of the current chain (since it would result in
                    superfluous connection attempts) and load on the seeds of
                    the current chain.
                    Re-use of existing seed nodes is also a security risk as
                    these seeds are partly under adversarial control and could
                    be used to provide spurious information to fork nodes or
                    provide attack information to other entities.

    Notes:          The list of seeds is unfortunately currently hardcoded.
                    Ideally, seed data would be moved to a more easily manageable
                    configuration file which could be adjusted without
                    needing to rebuild executables.

    Traceability:   MVHF-BU-SYS-REQ-6
---
    Requirement:    MVHF-BU-USER-REQ-7

    Origin:         BTCfork

    Type:           Functional

    Title:          DIFFICULTY RESET TO LOW VALUE

    Text:           Upon triggering, the fork shall reset the difficulty to a low
                    enough value to permit mining by even a tiny fraction of the
                    current hashpower.

    Rationale:      The set of miners mining the new fork is unknown, and may
                    be a small minority initially. They would be unable to
                    mine effectively at the current difficulty of the Bitcoin
                    network, therefore a difficulty reset is required to allow
                    the new fork chain to be mined.

    Notes:          The requirement for a difficulty reset is essentially
                    independent of whether the Proof-of-Work (POW) function is
                    changed. This MVF does not mandate change of POW.

    Traceability:   MVHF-BU-SYS-REQ-7
---
    Requirement:    MVHF-BU-USER-REQ-8

    Origin:         BTCfork

    Type:           Functional

    Title:          ADJUSTMENT OF DIFFICULTY RETARGETING PERIOD

    Text:           Upon triggering, the fork shall start with a reduced difficulty
                    retargeting period and gradually recover to the current 2016
                    block (~2 week) adjustment period.

    Rationale:      The hashing power allocated to the fork might vary
                    substantially. To prevent certain attacks [2], it is
                    important that the retargeting period be low initially,
                    so that difficulty can adjust faster to the faster-changing
                    circumstances on a fork.
                    However, there is strong support for reverting to the
                    current 2-week period over time, as this apparently helps
                    miners conduct planning more effectively than would be
                    possible given a much faster retargeting cycle.

    Notes:          [2] primarily attacks which add a lot of hashpower to raise the
                        difficulty, then withhold that hashpower to deny service
                        on the new chain (there may be other similar attacks
                        enabled by too-slow difficulty adjustment)

    Traceability:   MVHF-BU-SYS-REQ-8
---
    Requirement:    MVHF-BU-USER-REQ-9

    Origin:         BTCfork

    Type:           Functional

    Title:          REPLAY ATTACK PREVENTION

    Text:           Upon triggering, the fork shall modify signatures for
                    transactions conducted on its chain such that these transactions
                    will be invalid on the existing chain, in order to prevent
                    replay attacks.

    Rationale:      Replay attacks (where a malicious bridge can forward
                    transactions made on one chain to another, causing a loss
                    of transactional independence between the chains) have
                    been shown to be disruptive to the ecosystem e.g. in the
                    ETH/ETC fork. Such attacks can open the door to legal
                    repercussions due to unwanted spends.

    Notes:          A simple, but as yet untested proposal is the SIGHASH change
                    proposal made by Iguana developer jl777 in [3].
                    [3] https://steemit.com/bitcoin/@jl777/bitcoin-spinoff-fork-how-to-make-a-clean-fork-without-any-replay-attack-and-no-blockchain-visible-changes

    Traceability:   MVHF-BU-SYS-REQ-9
---
    Requirement:    MVHF-BU-USER-REQ-10

    Origin:         BTCfork

    Type:           Functional

    Title:          BACKUP COPY OF PRE-FORK WALLET

    Text:           Upon triggering, the fork shall attempt to create a backup of
                    the user's wallet in a known place, failing which it shall
                    exit gracefully without corrupting the existing (pre-fork)
                    state of the wallet.

    Rationale:      Users will need a reliable way to obtain a version of
                    their wallet containing no post-fork transactions,
                    so that they can safely use a purely pre-fork wallet for
                    operating on the existing chain (i.e. with the old client).
                    Relying on users to cease transacting prior to the fork
                    trigger and to manually prepare wallet backups is
                    error-prone and will lead to bad user experiences.
                    Therefore the fork should create such a backup by itself,
                    in a well-documented location (e.g. modified filename).

    Notes:          Currently no known fork implementations do this, this would
                    have to be developed from scratch.

    Traceability:   MVHF-BU-SYS-REQ-10
---
    Requirement:    MVHF-BU-USER-REQ-11

    Origin:         BTCfork

    Type:           Functional

    Title:          CLEARLY DISTINGUISHABLE CLIENT IDENTIFICATION

    Text:           Ensure that the fork client is distinguishable from the
                    non-forked client to the user, both visually and how it
                    identifies over the network.

    Rationale:      Care must be taken to ensure that a user can easily
                    distinguish the forked and non-forked software clients,
                    otherwise usage errors may result (e.g. transacting with
                    pre-fork coins on an unintended chain).

    Notes:          The forked software should distinctly identify on startup,
                    in graphical splash and help screens, when starting log
                    files and when called with options to display its version
                    information distinct version information, including when
                    transmitting its user agent string over the network.

    Traceability:   MVHF-BU-SYS-REQ-11

##3. System requirements <a id="3-sys-reqs"></a>

    Requirement:    MVHF-BU-SYS-REQ-1

    Origin:         BTCfork

    Type:           Functional

    Title:          NON-ELECTIVE HARD FORK TO SEPARATE CHAIN

    Text:           The system shall enforce the creation of its own chain by
                    separating from the existing chain upon the triggering of a
                    non-elective condition.

    Rationale:      refer to MVHF-BU-USER-REQ-1

    Notes:          "Its own chain" means that post-trigger, the existing chain
                    will no longer accept blocks of the forked chain and vice
                    versa. The trigger condition is described by MVHF-BU-SYS-REQ-2.

    Traceability:   MVHF-BU-USER-REQ-1
---
    Requirement:    MVHF-BU-SYS-REQ-2

    Origin:         BTCfork

    Type:           Functional

    Title:          TRIGGER CONDITION

    Text:           The system shall trigger the fork at a specified block height or upon
                    SegWit activation (95%), whichever is reached first.

    Rationale:      refer to MVHF-BU-USER-REQ-2

    Notes:          'Trigger the fork' means to initiate the actions that will separate
                    the chains and enforce the updated consensus rules of the fork.

    Traceability:   MVHF-BU-USER-REQ-2
---
    Requirement:    MVHF-BU-SYS-REQ-3

    Origin:         BTCfork

    Type:           Functional

    Title:          PERMIT BLOCK SIZE TO EXCEED ONE MEGABYTE

    Text:           Upon triggering of the fork, the system shall enable
                    blocks greater than 1,000,000 bytes.

    Rationale:      refer to MVHF-BU-USER-REQ-3

    Notes:          1. This requirement is already satisfied by Bitcoin Unlimited
                    without requiring software changes (or indeed a fork).
                    The node operators can simply set the appropriate
                    parameters so that miners would mine blocks > 1MB,
                    and other nodes could relay them immediately as long
                    as they do not exceed their configured Excessive Block Size.
                    This requirement can be tested by verifying the mining
                    and relay of blocks slightly larger than 1MB after the
                    fork has triggered.
                    2. It is recommended to release the MVHF client with
                    appropriate default settings (EBS, MGS) conducive to
                    immediate formation (mining and relay) of a >1MB block in
                    the forked chain.

    Traceability:   MVHF-BU-USER-REQ-3
---
    Requirement:    MVHF-BU-SYS-REQ-4

    Origin:         BTCfork

    Type:           Functional

    Title:          ENABLE BLOCK SIZE OF TWO MEGABYTES

    Text:           Upon triggering, the system shall enable a block size
                    of 2,000,000 bytes.

    Rationale:      refer to MVHF-BU-USER-REQ-4

    Notes:          1. Enabling a block size of 2MB implies that blocks
                    up to this size can be mined and relayed without hindrance
                    by MVF nodes. This can be achieved by appropriate setting
                    of the default values for the EBS and MGS parameters,
                    if necessary enforcing 2MB as a minimum value for
                    the EBS and MGS parameters from the fork trigger onwards.
                    It does not mean that 2MB is a hard limit from then on -
                    there is no hard limit in a Bitcoin Unlimited network
                    where accepted block size is regulated by EBS/AD
                    settings on relaying nodes.
                    2. As soon as the fork has happened, users of MVF will be
                    in control of further evolution of the block size, by
                    configuring the appropriate parameters (EBS, AD, MGS etc).

    Traceability:   MVHF-BU-USER-REQ-4
---
    Requirement:    MVHF-BU-SYS-REQ-5

    Origin:         BTCfork

    Type:           Functional

    Title:          CLEAN NETWORK SEPARATION

    Text:           Upon triggering, the system shall ensure that both
                    chains are clearly distinguishable and their networks
                    separated from each other, to prevent as much
                    unnecessary processing as possible on both sides.

    Rationale:      refer to MVHF-BU-USER-REQ-5

    Notes:          The separation state (forked or not) should be persisted
                    so that only the forked network is used after triggering.
                    If "and never the twain shall meet" cannot be
                    fully realized, then at least nodes of the different
                    networks shall part ways as swiftly as possible.

    Traceability:   MVHF-BU-USER-REQ-5
---
    Requirement:    MVHF-BU-SYS-REQ-6

    Origin:         BTCfork

    Type:           Functional

    Title:          NEW SEEDS

    Text:           The system shall use a distinct set of DNS seeds.

    Rationale:      refer to MVHF-BU-USER-REQ-6

    Notes:          The list of seeds is currently hardcoded.
                    Ideally, seed data would be moved to a more easily manageable
                    configuration file which could be adjusted without
                    needing to rebuild executables.

    Traceability:   MVHF-BU-USER-REQ-6
---
    Requirement:    MVHF-BU-SYS-REQ-7

    Origin:         BTCfork

    Type:           Functional

    Title:          DIFFICULTY RESET TO LOW VALUE

    Text:           Upon triggering of the fork, the system shall reset
                    the difficulty to a low enough value so that the
                    projected initial supporting hashpower would be able to
                    mine a block on average every 10 minutes.

    Rationale:      MVHF-BU-USER-REQ-7

    Notes:          1. Need to obtain more data for projected initial
                    supporting hashpower. A survey conducted earlier
                    indicated a range between ~70-600TH/s, which great
                    uncertainty in the commitments:
                    https://np.reddit.com/r/btcfork/comments/4wwq70/who_has_miners_gathering_dust_and_would_be/
                    in order to calibrate the difficulty reset.
                    2. The reset to low difficulty also requires faster
                    retargeting immediately after the fork, to compensate
                    for rapid changes in hashpower.

    Traceability:   MVHF-BU-USER-REQ-7
---
    Requirement:    MVHF-BU-SYS-REQ-8

    Origin:         BTCfork

    Type:           Functional

    Title:          ADJUSTMENT OF DIFFICULTY RETARGETING PERIOD

    Text:           Upon triggering of the fork, the system shall reduce
                    the difficulty retargeting period and deterministically
                    recover to the current 2016 block (~2 week) adjustment
                    period.

    Rationale:      refer to MVHF-BU-USER-REQ-8

    Notes:          A safe minimum retargeting period still has
                    to be determined. The reduction and recovery requirements
                    can be made precise in software requirements.

    Traceability:   MVHF-BU-USER-REQ-8
---
    Requirement:    MVHF-BU-SYS-REQ-9

    Origin:         BTCfork

    Type:           Functional

    Title:          REPLAY ATTACK PREVENTION

    Text:           Upon triggering of the fork, the system shall emit
                    and accept only modified signatures such that transactions
                    signed by it will be mutually invalid with those signed
                    on the existing chain.

    Rationale:      refer to MVHF-BU-USER-REQ-9

    Notes:          refer to MVHF-BU-USER-REQ-9

    Traceability:   MVHF-BU-USER-REQ-9
---
    Requirement:    MVHF-BU-SYS-REQ-10

    Origin:         BTCfork

    Type:           Functional

    Title:          BACKUP COPY OF PRE-FORK WALLET

    Text:           Upon triggering of the fork, the system shall create a
                    backup of the wallet in use (or the default wallet if
                    none is in use) in a new file. If the backup fails
                    for any reason the software shall exit gracefully,
                    preserving the existing (pre-fork) state of the wallet.

    Rationale:      refer to MVHF-BU-USER-REQ-10

    Notes:          -

    Traceability:   MVHF-BU-USER-REQ-10, MVHF-BU-SW-REQ-10-1, MVHF-BU-SW-REQ-10-2,
                    MVHF-BU-SW-REQ-10-3, MVHF-BU-SW-REQ-10-4, MVHF-BU-SW-REQ-10-5
---
    Requirement:    MVHF-BU-SYS-REQ-11

    Origin:         BTCfork

    Type:           Functional

    Title:          CLEARLY DISTINGUISHABLE SYSTEM IDENTIFICATION

    Text:           The system shall ensure that clearly distinguishable
                    identification is presented to the user in all necessary
                    places, to prevent misidentification of the system by
                    the user.

    Rationale:      refer to MVHF-BU-USER-REQ-11

    Notes:          Necessary places include (possibly non-exhaustive list):
                    1. in the Graphical User Interface (GUI) splash screen
                    and other places where the client version is indicated
                    2. in log file messages during startup
                    3. in the user agent string sent over the network
                    4. in the RPC output calls that return version information
                    5. in version information displayed by command line client
                    programs on request

    Traceability:   MVHF-BU-USER-REQ-11, MVHF-BU-SW-REQ-11-1, MVHF-BU-SW-REQ-11-2

##4. Software requirements <a id="4-sw-reqs"></a>

    Requirement:    MVHF-BU-SW-REQ-10-1

    Origin:         BTCfork

    Type:           Functional

    Title:          CONFIGURATION PARAMETER FOR WALLET BACKUP FILE LOCATION

    Text:           The client shall allow the user to configure a file path
                    where the wallet backup file shall be created when the
                    fork is triggered.

    Rationale:      It should be the user's choice to decide where to create
                    the wallet backup. For safety reasons no switch is
                    provided to disable the wallet backup.

    Notes:          If the specified configuration value does not includes
                    path separators, it is to be treated as a filename and
                    the default path where a user wallet is located shall
                    be used.
                    If the specified configuration value includes path
                    separators, the backup shall be created at that
                    specified location (path + filename).

    Traceability:   MVHF-BU-SYS-REQ-10
---
    Requirement:    MVHF-BU-SW-REQ-10-2

    Origin:         BTCfork

    Type:           Functional

    Title:          INITIATE WALLET BACKUP AFTER COMPLETION OF TRIGGER BLOCK PROCESSING

    Text:           After processing the trigger block, the client shall
                    initiate the wallet backup procedure.

    Rationale:      The backup of the wallet can and should be done *after*
                    the trigger block has been digested.

    Notes:          This requirement needs to be verified by receiving some
                    funds in the trigger block, and checking that they have
                    been accounted for in the backed-up wallet.

    Traceability:   MVHF-BU-SYS-REQ-10
---
    Requirement:    MVHF-BU-SW-REQ-10-3

    Origin:         BTCfork

    Type:           Functional

    Title:          WALLET BACKUP PROCEDURE - IF RUNNING WITH A WALLET IN USE

    Text:           If running with a wallet in use, the client shall back up
                    the wallet, ensuring that the application cannot write
                    to it while performing the backup.

    Rationale:      If a client is running with a wallet then it should be backed up.
                    If wallet use is disabled (e.g. using --disable-wallet),
                    there is no need to backup a wallet - in that case
                    MVHF-BU-SW-REQ-10-4 applies.

    Notes:          -

    Traceability:   MVHF-BU-SYS-REQ-10
---
    Requirement:    MVHF-BU-SW-REQ-10-4

    Origin:         BTCfork

    Type:           Functional

    Title:          WALLET BACKUP PROCEDURE - IF RUNNING WITHOUT A WALLET

    Text:           If running without a wallet, the client shall skip the
                    wallet backup procedure.

    Rationale:      If a client is not running with a wallet (e.g. using
                    --disable-wallet), there is no need nor safe ability
                    to perform the backup.

    Notes:          There is no need to back up a wallet if none is in active
                    use. In this case the user assumes responsibility for
                    backing up any existing wallet files prior to using them
                    with the client.

    Traceability:   MVHF-BU-SYS-REQ-10
---
    Requirement:    MVHF-BU-SW-REQ-10-5

    Origin:         BTCfork

    Type:           Functional

    Title:          SAFE CLIENT SHUTDOWN IN CASE OF WALLET BACKUP FAILURE

    Text:           If the wallet backup procedure (refer to
                    MVHF-BU-SW-REQ-10-3) fails, the client shall perform a
                    safe shutdown which preserves the state of the wallet
                    prior to the fork.

    Rationale:      Do not risk writing post-fork data to the wallet.

    Notes:          -

    Traceability:   MVHF-BU-SYS-REQ-10
---
    Requirement:    MVHF-BU-SW-REQ-11-1

    Origin:         BTCfork

    Type:           Functional

    Title:          CLEARLY DISTINGUISHABLE CLIENT NAME

    Text:           The client shall clearly identify itself as 'MVF-BU'
                    in the following places:
                    1. in the GUI splash screen
                    2. in the GUI title bar
                    3. in the GUI 'About' menu entry under the 'Help' menu
                    4. in the GUI 'About' dialog window (window title and version string)
                    5. in the GUI 'Information' pane of the 'Help->Debug' window (client name and user agent)
                    6. in the GUI 'Command line options' dialog under the 'Help' menu (client name)
                    7. in log file messages during startup
                    8. in the user agent string sent over the network
                    9. in the RPC output calls that return version information
                    10. in help and version information displayed on request
                        by command line client programs such as bitcoind,
                        bitcoin-cli, and bitcoin-tx

    Rationale:      -

    Notes:          The list above may still be missing some items.
                    The graphical aspects of this requirement can be
                    verified by demonstration.
                    Log file / RPC information should be verified programmatically.

    Traceability:   MVHF-BU-SYS-REQ-11
---
    Requirement:    MVHF-BU-SW-REQ-11-2

    Origin:         BTCfork

    Type:           Functional

    Title:          CLEARLY DISTINGUISHABLE DEBUG TRACES

    Text:           The client shall prefix any new permanent debug
                    traces using a recognisable tag such as 'MVF'.

    Rationale:      -

    Notes:          Since the client identifies itself as 'MVF-BU', the
                    'BU' can be omitted from debug traces and 'MVF' should
                    be sufficient.

    Traceability:   MVHF-BU-SYS-REQ-11

