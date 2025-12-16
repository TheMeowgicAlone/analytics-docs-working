# Platform Limitations

Here we document large-scale and pervasive complicating factors, and other limitations which affect the entire system. Each one is given its own section for clarity.

---

## Attribution

We will always struggle to connect identity to accounts.

### Problem

- Many humans have alternate Discord accounts.
- We will never be able to tie every action to a human.
- We can guess.
- Many humans will have alternate Steam accounts.

### Mitigation

- The platform must assume each account is a new person by default, then enable pairing retroactively.
- Connecting accounts to identities will be an ongoing task for the organization.
- Dashboards should exist to help prioritize these efforts.

### Takeaways

- A dashboard should be created to help focus these efforts.
- Expect to work with low level account IDs more often than member IDs, because member IDs are unreliable.

---

## No SLAs

We are working with consumer grade APIs.

### Problem

- Discord, Stratz, Steam, etc - none of them provide us ANY guarantee of uptime, accuracy, nor stability.

### Mitigation

- The platform must be resilient to malformed, missing, malicious, duplicated or otherwise invalid data.
- This is handled on the developer end well.

### Takeaways

- All processing scripts should plan for missing data heavily.
- Heavy constraints should be placed at the database level.
