# Serviced-apartment access & pricing platform

**Door codes that only exist during the stay. Five apartments run end to end for a paying client.**

![Status](https://img.shields.io/badge/status-live%20with%20real%20guests-2ea043)
![Client](https://img.shields.io/badge/client-paying-635bff)
![Encryption](https://img.shields.io/badge/door%20codes-AES--256--GCM-f78166)

- Encrypted door codes, shown only during the stay
- Cleaner alerts and late checkout with no phone calls
- Pricing driven by local event demand

> Source is private (client system). **[Full write-up →](https://sajjad.omni-genai.com/projects/serviced-apartments)**

`Node.js` · `Express` · `SQLite` · `AES-256-GCM` · `HMAC` · `scrypt` · `Railway` · `Resend`

<details>
<summary><b>How a door code works</b></summary>

```mermaid
flowchart LR
    A[Booking confirmed] --> B[Code issued<br/>days ahead]
    B --> C[(Stored encrypted<br/>AES-256-GCM)]
    B --> D[Written to the lock]
    C --> E{Guest opens<br/>signed link}
    E -- before / after stay --> F[Nothing shown]
    E -- during stay --> G[Code shown]
```

</details>

<details>
<summary><b>Security</b></summary>

- **Encrypted codes:** every door code is encrypted before it reaches the database, so a stolen copy holds nothing usable.
- **Signed links:** guest links are HMAC-signed with an expiry, so they can't be guessed or edited to point at another booking.
- **Passwords and webhooks:** staff passwords are scrypt-hashed, and webhooks are rejected unless the signature matches.
- **Kill switch:** rotating the signing secret cancels every outstanding link at once.

</details>

<details>
<summary><b>Built around operations</b></summary>

- **Codes go to the lock days ahead,** so a broadband fault on arrival day can't lock a guest out at night.
- **New code before old,** so a failed rotation never leaves a cleaner locked out.
- **Staff codes rotate weekly,** so a leaked code stops working on its own.
- **Built before the locks arrived:** a mock lock runs every path for real, and one setting swaps in the live provider.

</details>

<details>
<summary><b>For guests and cleaners</b></summary>

- Personal stay page: wifi, directions, check-in steps, house manual and local events during the stay.
- Codes appear at 15:00 on arrival and vanish at 10:00 on departure, correct across the clock change.
- Late checkout from the guest's own page moves the checkout time and the door code automatically.
- Checkout alerts the cleaner and puts the flat back on sale.

</details>

<details>
<summary><b>Pricing from local demand</b></summary>

- Three event sources merged daily: a venue listings page, a ticketing API and a fixtures feed.
- The scraper is polite: one run a day, conditional requests, and it stops on a refusal instead of retrying.
- Demand weightings are documented as estimates, to be replaced by the client's own occupancy data.

</details>

---

Built by **[Sajjad Mahessar](https://sajjad.omni-genai.com)** · [LinkedIn](https://www.linkedin.com/in/sajjadmahessar/)
