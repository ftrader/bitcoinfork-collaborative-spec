# User requirements for Minimum Viable Hard Fork - Bitcoin XT edition

    Req. ID: MVHF-XT-USER-REQ-1
    Req. Origin: BTCfork
    Req. Type: Functional
    Req. Title: NON-ELECTIVE HARD FORK TO SEPARATE CHAIN
    Req. Text: The fork shall enforce the creation of its own chain by
               separating from the existing chain upon the triggering of a
               non-elective condition.
    Req. Rationale: A non-elective hard fork is needed as the majority of the
                    mining power is now under the control of a few individuals
                    who have refused to upgrade the block size limit.
    Req. Notes: "its own chain" means that post-trigger, the existing chain
                will no longer accept blocks of the forked chain and vice
                versa. The trigger condition is described by a separate
                requirement.
---
    Req. ID: MVHF-XT-USER-REQ-2
    Req. Origin: BTCfork
    Req. Type: Functional
    Req. Title: TRIGGER CONDITION
    Req. Text: The fork shall trigger at a specified block height or upon
               SegWit activation (75%), whichever is reached first.
    Req. Rationale: SegWit activation allows for blocks with SegWit
                    transactions on the network, and would require clients
                    to be able to process those blocks according to SegWit
                    rules. This would require the SegWit functionality to
                    be retained going forward in order to be able to validate
                    the ledger. SegWit functionality is not seen as part of
                    the requirements for an MVF.
    Req. Notes: Satoshi described a hard fork at a predetermined block height
                as a possible mechanism to allow bigger blocks again in future.
                A XT-derived MVHF implementation including the additional
                SegWit trigger would not need the main SegWit functionality,
                only the BIP9 activation logic and parameters compatible with
                BIP141 deployment.
---
    Req. ID: MVHF-XT-USER-REQ-3
    Req. Origin: BTCfork
    Req. Type: Functional
    Req. Title: PERMIT BLOCK SIZE TO EXCEED ONE MEGABYTE
    Req. Text: Upon triggering, the fork shall enable blocks greater than
               1,000,000 bytes.
    Req. Rationale: The historical limit of 1,000,000 was put in as an
                    anti-spam mechanism by Satoshi, not with the intention of
                    limiting growth of Bitcoin. The refusal by the existing
                    developers (XT) and a majority of miners to raise this
                    limit (even to 2MB) is what is motivating a hard fork
                    (and this requirement in particular) to overcome this
                    limitation and restore growth.
    Req. Notes: The actual regulation of block size in the forked solution is
                described by further requirements.
---
    Req. ID: MVHF-XT-USER-REQ-4
    Req. Origin: BTCfork
    Req. Type: Functional
    Req. Title: PERMIT BLOCK SIZE UP TO TWO MEGABYTES
    Req. Text: Upon triggering, the fork shall statically raise the limit of
               block size to 2,000,000 bytes.
    Req. Rationale: A block size of 2MB is considered safe by all parties
                    (even the planned SegWit solution could effectively result
                    in up to 4MB aggregate size of witness + non-witness data).
                    The Cornell study [1] has indicated that even sizes of up
                    to 4MB would be safe (at the time of the study - it is
                    likely that even greater sizes would be acceptable now).
                    Raising the limit to a static 2MB is the simplest and most
                    unproblematic immediate change, would relieve the urgent
                    block size pressure, and could be followed up by a hardfork
                    with a more sophisticated dynamic algorithm allowing
                    greater (or even limited-only-by-infrastructure) block
                    sizes.
                    A conservative 2MB cap as a first step would also be
                    suitable for clients which have not yet implemented
                    improved block propagation protocols such as Bitcoin
                    Unlimited's Xtreme Thinblocks or XT's Compact Blocks.
                    As such it would make the MVF more accesible to wider
                    implementation in alternate clients (btcd + others).
    Req. Notes: It is not established whether block sizes greater than 4MB
                would require protection in the form of a dynamic cap against
                attempts to eliminate full nodes from the network through
                persistent spam attacks (high volume of own transactions).
                Mitigations against such selfish spamming are possible, but
                for simplicity the MVF should stick to proven technology and
                use a safe enough static limit.
                [1] http://fc16.ifca.ai/bitcoin/papers/CDE+16.pdf
---
    Req. ID: MVHF-XT-USER-REQ-5
    Req. Origin: BTCfork
    Req. Type: Functional
    Req. Title: CLEAN NETWORK SEPARATION
    Req. Text: Upon triggering, the fork shall separate from the current
               Bitcoin network, to prevent as much unnecessary processing as
               possible on both sides.
    Req. Rationale: Clean network separation is necessary to prevent processing
                    of old-chain protocol data (transactions and blocks) once
                    the fork has triggered. This would prevents unnecessary
                    computations and second-order consequences (like node
                    banning).
    Req. Notes: This is not currently done by any of the current Bitcoin hard
                fork clients (BIP109-compatible implementations).
                If peer connections are closed and re-opened during the fork,
                there may be some risk of interference during the separation
                manoeuvre.
---
    Req. ID: MVHF-XT-USER-REQ-6
    Req. Origin: BTCfork
    Req. Type: Functional
    Req. Title: NEW SEEDS
    Req. Text: Upon triggering, the fork shall switch to using a new set of
               DNS seeds.
    Req. Rationale: The current DNS seed nodes, if used by the fork after
                    it has triggered, would lead to undue interference with
                    full nodes of the current chain (since it would result in
                    superfluous connection attempts) and load on the seeds of
                    the current chain.
                    Re-use of existing seed nodes is also a security risk as
                    these seeds are partly under adversarial control and could
                    be used to provide spurious information to fork nodes or
                    provide attack information to other entities.
    Req. Notes: The list of seeds is unfortunately currently hardcoded.
                Ideally, seed data would be moved to a more easily manageable
                configuration file which could be adjusted without
                needing to rebuild executables.
---
    Req. ID: MVHF-XT-USER-REQ-7
    Req. Origin: BTCfork
    Req. Type: Functional
    Req. Title: DIFFICULTY RESET TO LOW VALUE
    Req. Text: Upon triggering, the fork shall reset the difficulty to a low
               enough value to permit mining by even a tiny fraction of the
               current hashpower.
    Req. Rationale: The set of miners mining the new fork is unknown, and may
                    be a small minority initially. They would be unable to
                    mine effectively at the current difficulty of the Bitcoin
                    network, therefore a difficulty reset is required to allow
                    the new fork chain to be mined.
    Req. Notes: The requirement for a difficulty reset is essentially
                independent of whether the Proof-of-Work (POW) function is
                changed. This MVF does not mandate change of POW.
---
    Req. ID: MVHF-XT-USER-REQ-8
    Req. Origin: BTCfork
    Req. Type: Functional
    Req. Title: ADJUSTMENT OF DIFFICULTY RETARGETING PERIOD
    Req. Text: Upon triggering, the fork shall start with a reduced difficulty
               retargeting period and gradually recover to the current 2016
               block (~2 week) adjustment period.
    Req. Rationale: The hashing power allocated to the fork might vary
                    substantially. To prevent certain attacks [2], it is
                    important that the retargeting period be low initially,
                    so that difficulty can adjust faster to the faster-changing
                    circumstances on a fork.
                    However, there is strong support for reverting to the
                    current 2-week period over time, as this apparently helps
                    miners conduct planning more effectively than would be
                    possible given a much faster retargeting cycle.
    Req. Notes: [2] primarily attacks which add a lot of hashpower to raise the
                    difficulty, then withhold that hashpower to deny service
                    on the new chain (there may be other similar attacks
                    enabled by too-slow difficulty adjustment)
---
    Req. ID: MVHF-XT-USER-REQ-9
    Req. Origin: BTCfork
    Req. Type: Functional
    Req. Title: REPLAY ATTACK PREVENTION
    Req. Text: Upon triggering, the fork shall modify signatures for
               transactions conducted on its chain such that these transactions
               will be invalid on the existing chain, in order to prevent
               replay attacks.
    Req. Rationale: Replay attacks (where a malicious bridge can forward
                    transactions made on one chain to another, causing a loss
                    of transactional independence between the chains) have
                    been shown to be disruptive to the ecosystem e.g. in the
                    ETH/ETC fork. Such attacks can open the door to legal
                    repercussions due to unwanted spends.
    Req. Notes: A simple, but as yet untested proposal is the SIGHASH change
                proposal made by Iguana developer jl777 in [3].
                [3] https://steemit.com/bitcoin/@jl777/bitcoin-spinoff-fork-how-to-make-a-clean-fork-without-any-replay-attack-and-no-blockchain-visible-changes
---
    Req. ID: MVHF-XT-USER-REQ-10
    Req. Origin: BTCfork
    Req. Type: Functional
    Req. Title: BACKUP COPY OF PRE-FORK WALLET
    Req. Text: Upon triggering, the fork shall attempt to create a backup of
               the user's wallet in a known place, failing which it shall
               exit gracefully without corrupting the existing (pre-fork)
               state of the wallet.
    Req. Rationale: Users will need a reliable way to obtain a version of
                    their wallet containing no post-fork transactions,
                    so that they can safely use a purely pre-fork wallet for
                    operating on the existing chain (i.e. with the old client).
                    Relying on users to cease transacting prior to the fork
                    trigger and to manually prepare wallet backups is
                    error-prone and will lead to bad user experiences.
                    Therefore the fork should create such a backup by itself,
                    in a well-documented location (e.g. modified filename).
    Req. Notes: Currently no known fork implementations do this, this would
                have to be developed from scratch.



# System requirements for Minimum Viable Hard Fork - Bitcoin XT edition

TBD - These will be derived from the above user requirements.
