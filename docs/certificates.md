<!--
NOTE FROM THE AUTHOR (Atlas): this page is written at design level on purpose. Details that would identify our setup were left out; the reasons, the shape, the limits and what went wrong are kept.
-->

# Client certificates: a door that checks the device, not the person

## What it is for

Some of our pages should never show a login form. A wall display that is always on cannot ask anyone for a password, and a login screen on a page that must work unattended is a hole with a form in front of it. So the internal dashboards sit behind a door that asks one question before any page is served: *does this device hold a certificate we issued?* If not, the connection is refused before it reaches a page. A browser shows an ugly generic error, not a hint of what is behind.

This replaced an earlier login layer. Two locks side by side were never the goal; one lock that can be measured is simpler than two that disagree.

## How a person or device gets in

Every device gets its own certificate, issued for that device alone, with a name a human can read in the log. It is installed once: on a phone or laptop through the platform's own certificate store, on a server as a file readable only by the account that needs it. After that nothing changes for the user. The device simply opens the page.

The agents use the same mechanism. Each agent's seat holds a certificate for itself and uses it to open the dashboards it has to read. An agent is a client like any other, and when it is wrongly let in or locked out, the door's records show which one.

## Why a private authority

A public authority issues certificates for *servers*, so that strangers can trust a site. We need the opposite: certificates for *our devices*, trusted by our own door and by nobody else. A small private authority does that, and it keeps the question "who may come in?" inside the house. The cost is that we carry the whole responsibility, issuing, renewing, revoking and cleaning up, with no one else to blame.

## How certificates are issued and renewed

Certificates are short-lived on purpose: weeks, not years. A short life turns rotation from a project into a habit, and it turns a stolen device from a problem of years into a problem of weeks. A device renews itself automatically well before expiry. If the renewal fails, the device says so, and a separate watcher reads the door's records and warns before a certificate lapses. Nobody should find out by being locked out.

Names matter more than they look. A certificate named after nothing is a certificate you cannot revoke without guessing. Ours are names a human can read in the log at three in the morning.

## When one is lost, or a device is gone

The authority publishes a revocation list and the door reads it. Two things we learned the hard way here:

1. **Revoking in the authority does nothing by itself.** The first time we measured it, the door ignored the list entirely. A certificate could be marked revoked and still open the door until it expired. Revocation only means something once the door is built to honour it, and it needs a test that fails when it is not.
2. **A list can go stale.** If the door reads an old list, a fresh revocation is not effective. So there is a watcher on the freshness of the list itself, and a check after every rebuild of the door that a revoked test certificate really is refused.

Expired and revoked certificates are cleaned out of the live views within a day and moved to an append-only archive with the date and the reason. Gone from the running system, not gone from the history. The cleanup is a standing watcher with its own heartbeat, because a cleanup task that has quietly died and a cleanup task that finds nothing look identical.

If the strongest key is lost there is a break-glass route. What is sealed away is the *procedure* for minting a short emergency certificate on the day, not a certificate that waits in a drawer: a certificate that lives long enough to wait is a second key under the doormat, and one that is too short is useless when you need it.

## The limits

- **A stolen device that is unlocked gets in.** The door proves the device, not the human. That is the price of having no login screen, and we pay it knowingly. The monitoring of who uses which certificate is the compensation, not a prevention.
- **The error is unfriendly.** A browser without a certificate cannot tell you it is missing one.
- **The revocation path is third-party software on the access route.** If it breaks it can fail closed (loud, and fixable) or open (silent, and bad). That is why the revoked-certificate test runs after every change to the door, not only once.
- **The authority is a single point.** If it is down, nothing new is issued and renewals fail. It was rebuilt once after its hardware was retired, and we wrote the trade-off down: running it beside other services is convenient and it ties its fate to theirs.

## What we learned

- Test with the strictest trust setting available, never with "ignore certificate errors". A check that cannot fail teaches nothing.
- A directory of trust anchors can break the door in two directions: a wrong permission makes the door refuse everyone after the next reload. Write the final permission state down and check it before any reload.
- A certificate that nobody watches expires on a Sunday. Every certificate that matters has a watcher, and every watcher has a heartbeat.
- The silent failure is the dangerous one. Most of the design is about turning silence into a sound.
