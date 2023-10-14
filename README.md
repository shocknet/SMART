# SMART

*(Slightly More Advanced Relay Technology (for Nostr))*

`rfc`

**PURPOSE**

A middleware service that provides a performant interface to nostr, by sitting between client applications and relay-networks, to run logic that is beyond the scope or NIP compliance of core relay implementations. These added capabilities or “SMART Routes” are enabled by server-side caching, indexing, and rendering that would otherwise have to otherwise be done client-side and cause poor usability.

SMART Routes should supply only results that are contrived from NIPs, and are therefore verifiable. A client appending `/proof` to their request should expect the raw events needed to reconstruct a result. Routes are to be defined similarly to NIPs, but with the assumptions of being an application back-end rather than a client-side protocol.

Unlike NIPs, an SR is referenced by name instead of arbitrary numbers. For example, a route for GET `/smart/npub/nip05/` would be referenced as `npub-nip05`.

SMART is **not** intended as its own relay, but drop-in middleware for client applications, that will in many instances forward raw events and requests.

**Example usecases:**

- A nostr-based contact list in lightning wallet:
    - User inputs their `npub`,
    - Client GETs `/smart/npub/contacts/`,
    - A list of all accounts the provided npub follows is returned.
    - Each contact returned would contain lnurl or lpub `payee` information from its indexed metadata, and could thus be used by the wallet for the sending payments, as well as other profile display information like avatars, pet names, etc.

- Key Delegation:
    - Responses should include data published via delegated keys to save a client multiple round trip look-ups. Indexes enabling this route could combine multiple delegation NIPs into a single GET response.

**OTHER NON-ROUTE FEATURES**

- Scavenging and Forwarding Policies
    - Write events received from clients may be forwarded to other networks.
    - Other networks may also be queried if local results are unsatisfactory.
    - Service publishes forwarding and scavenging policies
        - Clients may use these to help a user determine if they want to specify additional networks for use.

- Scalability
    - Uses relays as write-leader for eventual consistency.
    - Access tiering
    - Highly Available

- Private API
    - Chaumian tokens, sold to or earned reputationally by clients, allow ephemeral keys to write without leaking who whitelisted them.
    - Authenticated routes can protect metadata Direct Messages and other sensitive kinds.

**DEFINITIONS**

- Unsatisfactory Results
    - No results for specified kind / tag / age
    - Signatory has tagged a relay that is not in scavenging array

**ROUTES**

- Stable
- Dev
- To Do
  - npub-avatar
  - npub-name
  - npub-nip05
  - npub-payee
  - npub-contacts
  - npub-delegates
  - npub-relays
  - raw-write
  - raw-req
  - nevent-
  - nevent-
  - nprofile-
