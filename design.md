# Design for Minimum Viable Hard Fork - Bitcoin Unlimited edition

##1. Contents <a id="1-contents"></a>

1. [Contents](#1-contents)
2. [Introduction](#2-introduction)
3. [Overview of changes](#3-overview)
4. [Architecture](#4-architecture)
5. [Detailed design](#5-detailed-design)
6. [Requirements traceability](#6-req-traceability)

##2. Introduction <a id="2-introduction"></a>

To be completed.

Should give describe the purpose and structure of this document,
its scope and intended audience, and give references to other applicable
documents and reference material.

##3. Overview of changes <a id="3-overview"></a>

To be completed.

##4. Architecture <a id="4-architecture"></a>

To be completed.

Should indicate parts of the system are being changed and how.

##5. Detailed design <a id="5-detailed-design"></a>

To be completed.

Detailed design information on all the changes.
Elements of the design need to be given unique identifiers, so that they
can be traced back to requirements, and identified in the code.

###5.x Wallet backup procedure

(this is a temporary section to hold some preliminary ideas on this)

Steps in wallet file backup procedure (tentative):
  1. close the existing wallet disk file
  2. attempt to open the backup wallet disk file for writing
  3. copy the existing wallet file into the backup file
  4. close the backup file
  5. re-open the existing wallet file (to resume further operation)

##6. Requirements traceability <a id="6-req-traceability"></a>

To be completed.

Requires the design elements (in Detailed Design) to be identifiable so
that we can match them to requirements.
