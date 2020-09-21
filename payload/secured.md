---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: page
title: Squire secured inner payload
description: Simple secured payload for contact tracing
menubar: docs_menu
---

# DRAFT Secure Payload design

This example of an [inner payload](/payload/inner) that is designed to work exclusively with
the [Squire envelope payload](/payload/envelope) provides additional security and privacy
guarantees over and above the [Squire simple inner payload](/payload/simple) example.

![Secured payload data](/images/SquirePayloadSecured.png)

This payload was driven by two somewhat competing desires:-

- Provide as much information to a state/national health authority as possible to prevent virus transmission
- Preserve contact-to-contact privacy

The data we support for both is described in the following two sections. Later follows a detailed explanation
of how the payload works.

<b>DRAFT</b> This is a draft payload description currently under review.

## Providing information to a national health authority

We wanted to support these extended contact tracing approaches:-

- Allow the tracking of spread throughout a contact graph network, with persistent graph-node identifiers
- Allow the spotting of asymptomatic individuals, and super spreaders, through network analysis
- Support not just First Order contact notifications, but also Second and Third order, to drastically cut virus transmission in the peak of a pandemic

This places some restrictions on the outer payload:-
- ClientID is knowable by the health authority and can be linked to a persistent identifier over time (although NOT necessarily the registration ID of that phone user)

This also requires the inner payload to provide the following information:-
- Exposure service token (32 bytes) - Token issued by phone A allowing the remote phone B to notify phone A's health authority that the exposure is valid
- Phone exposure confirmation token (16 bytes) - Token issued by phone A, passed by phone A's health authority back to it when phone B is ill, allowing that phone to verify that the exposure is real, and was not the result of a replay/relay or spoofing attack, or the result of health authority for phone A being compromised

## Enhancing privacy

Privacy is an issue when an organisation can request, warrant, or extract the contact information from a health authority and identify the individual phones or users
behind the ClientIDs passed during contact event sharing. I.e. knowing the identity between the 'nodes' on a contact graph.

Ideally we want to share a solid and verifiable contact graph with a national health authority without them knowing both sides of the contact graph, even if both sides declare themselves as ill
and submites their contact information to the same health authority.

The way to do this is to provide edge information to the health authority, and a persistent but unregistered identifier (Node ID) for the contacts. The way to ensure that the exposure
is a valid one and only acted upon is to verify the tokens shared between the two phones locally, and encrypted so only they know the content shared,
and verifiable with secrets only stored in the secre enclave of each phone.

Basically we do this:-
- Provide all graph edge information to the health authority, and risk calculation is done centrally, subtotalling by Persistent UUID
- Post the ClientIDs, exposure confirmation tokens, and summarised risk subtotals to all devices under the health authority's control - regularly, within 30 minutes of receipt (See the Ferretti et al paper, March 2020 for rationale)
- Leave exposure risk verification, and notifying to the health authority that the user is exposed, to the exposed mobile phone (not doing this centrally)

Identity information is managed as follows:-

- Each phone registers with a heath authority on first launch, and creates a base symmetric key using Diffie-Hellman-Merkle approach, and agrees an epoch time
- Each phone uses the symmetric key and agreed epoch to generate a ClientID using the TOTP approach [RFC-6238 (external link)](https://tools.ietf.org/html/rfc6238). This is NOT the outer packet client id, but a validation ID used to secure comms to the health authority (E.g. via a one time use security header)
- Each phone generates two of its own public/private key pair which is stored only locally in the secure enclave. These are:-
  - Ephemeral public/private key pair, for short lived cryptographic comms between devices over Bluetooth. MUST be generated per-device and used only once (I.e. for this single client exchange). Never shared with the health authority, deleted after use.
  - Exposure confirmation public/private key pair, used to generate locally verifiable exposure confirmation tokens. Regularly rotated (E.g. daily), but stored locally for 15 days for verification. Never shared with the health authority
- Each phone generates a 256 bit (32 bytes) symmetric key, stored locally in the secure enclave, along with a random epoch value less than the ClientID epoch value, and generates the ClientID using TOTP. Key never shared directly with the health authority, but the TOTP result is.
- Each phone generates a 16 byte UUID used as a persistent identifier. Never shared by the transmitter directly with a health authority, only indirectly via a nearby phone, and only encrypted using the health authority's submission TOTP data

## Who knows what, and when?

The [Transmitting](/background/glossary) phone know:-

- It's own master identity symmetric key/UUID (Agreed via Diffie-Hellman-Merkle with healthcare central system)
- It's health authorities public key for any given day
- It's own ephemeral (Bluetooth) pubic/private key pair for this single exchange
- It's own exposure confirmation public/private key pair for this day
- It's own persistent UUID
- It's own geo-approximator (E.g. postal district in the UK - 10000 households on average)
- It's own phone make and model identifier string
- The receiver's ephemeral public key for this single exchange

The [Receiving](/background/glossary) phone knows:-

- The transmitter's ephemeral public key
- The transmitter provided exposure service token for the transmitter's health authority, which it cannot decrypt, secured with the daily server public key, which includes:-
  - Transmitter's persistent UUID (16 bytes)
  - Geo-approximator for the transmitter (4 bytes)
  - Transmitter phone make and model identifier (8 bytes)
  - TOTP token for the transmitter's persistent UUID (4 bytes)
- The transmitter provided exposure confirmation service token, which it cannot decrypt, can only be decrypted by the transmitter's confirmation private key, but which contains:-
  - ClientID of the receiver (16 bytes)
- The transmitter's country and state codes (allowing passing of exposure data to this health authority)

The Transmitter's health authority knows, from the transmitter:-

- The transmitter's master identity symmetric key/UUID (agreed via Diffie-Hellman-Merkle)
- It's own daily exposure submission public/private key pair

The Receiver's health authority knows, from the receiver:-

- The transmitter's exposure service token, for this receiver at this time, which it cannot decrypt
- The transmitter's exposure confirmation token, for this receiver at this time, which it cannot decrypt
- The transmitter's country and state codes (allowing passing of exposure data to this health authority)
- The date or datetime of the contact as observed by the receiver (optional, depends on receiver's app)
- Optionally (but NOT recommended - here in case of a bad actor country using the payload) the transmitter's ephemeral public key as observed by the receiver, which is why this should be rotated per contact
- datetime of contact and its distance RSSI info

The transmitter's health authority is told by the receiver's health authority:-

- Receiver phone make and model only
- Transmitter's exposure service token, and decrypts it, revealing:-
  - Transmitter's persistent UUID (which it CANNOT identify the owner of)
  - Geo-approximator for the transmitter
  - Transmitter phone make and model identifier
  - TOTP token for the transmitter's persistent UUID
- Transmitter's exposure confirmation token, which it cannot decrypt

The transmitter's health authority posts the below to all phones it manages (MUST BE WITHIN 30 MINUTES):-

- Transmitter's exposure confirmation token
- Datetime of start of contact event
- Risk accrued score associated with it
- TOTP token for the transmitter's persistent UUID
- Date time this data was posted

The transmitter can now:-

- Recognise and decrypt its own exposure confirmation token, extracting:-
  - Country Code, State Code, ClientID of the receiver, which it verifies the date and time of with its history record
  - Transmission datetime from unix epoch
- Verify that the server decrypted TOTP for it's persistent UUID matches the time of the exposure confirmation token (thus verifying ALL THREE participants, transmitter, receiver, transmitter health authority)
- Use the risk score to calculate the locally acrued risk for a given date/set of days, and see whether it breaches the risk threshold
- Notify it's health authority that they have breached the risk threshold, ideally not immediately after receiving the latest exposures keys (MUST be within 3hrs 30 mins, and should take the posting date time in to account)
  - May also share the person's total risk score for the last 7-14 days

Other receiving phones receive:-

- Transmitter's exposure confirmation token, which it cannot decrypt
- Contact event start date time
- Risk score for this unknown pair of people in the contact event
- TOTP token for the transmitter's persistent UUID, which it cannot use as it doesn't know the associated transmitter persistent UUID

A random bluetooth sniffer sees:-

- Ephemeral public keys for receiver and transmitter, which it cannot use as it doesn't have the private keys
- Encrypted data exchanges between the two phones, including the health exposure service token and exposure confirmation token, which it cannot decrypt
- Country and State code for receiver and transmitter
- Any random Bluetooth data being advertised by that phone (E.g. 15 min rotating BLE mac address, "Adam's iPhone" name, FindMyPhone or other manufacturer specific data)
   - On some phones this includes the TX power of the advertiser (transmitter)
- It can calculate it's own RSSI to each device, but not see the RSSI between the two devices

## Assumptions

Health authority and mobile device have a way to rotate and fetch server public/private keys and symmetric key exchange should the server keys become compromised.
This is down to the national health authority to implement in their own app.

## Security Analysis - CIA

Confidentiality - Yes. Provided by encrypting all data between both phones, and from phones to health service, preventing interception, spoofing, relay and replay attacks. A one time use Diffie-Hellman-Merkle symmetric key agreement is used to encrypt the data. Uses phones' built in secure enclave for storage of cryptographic material where provided.

Integrity - Yes. By using a TOTP code that the health authority can decrypt and an exposure confirmation token the transmitting phone can only decrypt, the transmitting
phone knows that both phones and its health authority have all been involved in the exchange, and the data has been verified. Data cannot leak or be extracted and
use by another device as the transmitting device checks the time and tokens it issued, ensuring integrity. Even if data passes via a health authority of a hostile
state actor it cannot be manipulated to cause the exposure of incorrect or targetted individuals and groups of the originating country.

Availability - Yes. Bluetooth provides built in CRC checks of data, so by the time Squire sees the data it has already been validated. Supporting both read
and write approaches to Bluetooth data exchange ensures the maximum support of mobile phones, as some phones can discover and read and write from others,
but not be discoverable and readable/writable themselves (~35% of Android phones in the UK do not support advertising [[21]](/paper/bibliography#a-21)).

Non-repudiation - Yes. The original transmitter can verify that the exposure token was from the exact phone they communicated with and that their
data was decrypted and interpreted by their own health authority, and that the time of the contact is valid. All three participants (Transmitter, Receiver, Transmitter's health authority)
are authenticated in this approach. Authentication of the receivers health authority if left to the communication channel between those authorities.

## What attacks does this prevent?

- Westminster/hospital attack - Where a state actor can generate a large set of fake device 
registrations and intercept a set of identifiers that it claims it, itself, received. This
prevents locking down of specific, targetted, areas so long as a large 'spike' of
notifications from a single source is trapped.
- Relay attacks - cannot work as the entire contact chain is verified by the transmitter
- Replay attacks - Comms are encrypted, and pub/priv keys are one time use, preventing valid replays
- Compromise of networking between mobile and its health authority. E.g. impersonation of network device in the chain, post registration

## Where did this idea come from?

A mix of :-

- Kerberos ([RFC 4120 (external link)](https://tools.ietf.org/html/rfc4120)); and 
- Time base one time password - TOTP ([RFC-6238 (external link)](https://tools.ietf.org/html/rfc6238)); and
- Diffie-Hellman-Merkle key agreement ([RFC-2631 (external link)](https://tools.ietf.org/html/rfc2631)).

Diffie-Hellman-Merkle should be used over an encrypted channel between each mobile phone app and its health authority to exchange cryptographic material between
mobile and health authority servers that allow a mutually agreed symmetric key to be created without interception. This key is used
to secure all communication between mobile and health server, like a password, but instead of directly being used should be used with TOTP to generate a one time use key per request. 
This prevents well funded actors breaking Diffie-Hellman-Merkle protected exchanges through pre-calculation of factors as they do not know the mutually agreed epoch.
In the same way a client UUID can be generated.

Optionally, in a very centralised system with lower privacy guarantees, the rotation key (TOTP password) derived from this symmetric key is used as the transmitter persistent UUID (NOT the bluetooth ClientID).
This should not be necessary, however, as all epidemiological data is provided without this needing to be done.

Time based one-time password is used in several areas, but with different source material:-

- To secure comms between a mobile device and its health authority server directly
- To generate a one time use exposure confirmation code allowing a mobile device to verify any received exposure keys
- To provide the transmitter's health authority, via the receiver and its health authority, with an encrypted verification code, allowing receiver and server to be verified in the eyes of the transmitter when an exposure notification is received

Kerberos was used as inspiration for the method of token passing and validation without 
communication between the health authority and trasmitter each time (and thus loss of privacy). 
The difference here is that each transmitter, NOT the health authority server, acts as its 
own authentication authority, and treats the health authority it belongs to as a 'Service' 
provider (hence the term exposure service token). 

The transmitter also treats itself as a service 
provider, so it can verify the exposure record later on.
The receiver phone is regarded as a 'client' of both the transmitter and health 
authority exposure notification service.

## Extensions

Given that enough contact graph data is known by the health authority it would 
be possible for it to identify asymptomatic people and super spreaders.
In this scenario the health authority would have to add a different 
notification list where the exposure confirmation token was verifiable
as being from the health authority server. 

This would of course potentially
identify this transmitter as the hitherto anonymous end of the contact graph, 
identifying both ends of the contact event and linking the persistent UUID 
forever to a known real transmitting device, if both transmitter and receiver are 
from the same health authority.

It is important, therefore, that when an app becomes aware of being dangerously exposed,
or notified that its user could be a super spreader, that it doesn't immediately
declare itself as in this state. It should lag this status update by a random time period
of up to 30 minutes.
