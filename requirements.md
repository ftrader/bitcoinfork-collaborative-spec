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

    Traceability:   To be completed
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

    Traceability:   To be completed

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

    Text:           Upon triggering, the system shall cleanly separate from
                    the current Bitcoin network and prevent as much
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

    Traceability:   MVHF-BU-USER-REQ-10


##4. Software requirements <a id="4-sw-reqs"></a>

To be completed.
