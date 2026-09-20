# AB licence list

The list AB MEP Supports copies read to find out whether they have been withdrawn. One file:

**`mep-supports.json`** - the block list, signed with the publisher's private key.

Copies licensed directly by the publisher (`tools\personal-build.ps1` in the AB.MepSupports.Revit
repository) read it at most once a day at

```
https://raw.githubusercontent.com/Al-Qublawi/ab-licence/main/mep-supports.json
```

A copy or a machine named in the file stops placing, recalculating, removing and syncing supports -
Preview and the settings keep working, and the message says who to contact. Taking an id back out gives
the copy its full use again at its next check.

## Private now, public when it is needed

**While this repository is private the address above returns 404, and a copy reads that as "nothing is
blocked".** That is the normal state, and it means nothing of yours is on the open internet.

To actually stop a copy: **Settings -> General -> Change visibility -> Public**. From then on the copies
read the file and obey it. Making the repository private again is the same as an empty list - everything
is released.

Nothing here gives anything away: the file holds opaque ids and a note you write, no source code, no
customer details, and it cannot be forged - a list signed with any other key is ignored.

## Changing the list

**Never edit `mep-supports.json` by hand.** It is `{payload, signature}`: the payload is the list, and the
signature is made with a private key that exists only on the publisher's PC. Any edit breaks the signature
and every copy ignores the file - which fails safe (nothing is blocked), not open.

Write a new one instead, on the PC that has the key, listing **everything that should be blocked** - the
file replaces the previous one, it does not add to it:

```powershell
cd <the AB.MepSupports.Revit clone>
.\tools\blocklist.ps1 -Block P-202609-LAP -Machine AB-9TUY-GMXT -Note 'Laptop stolen 21 Sep 2026'
```

That writes `dist\blocklist.json`. Copy it over `mep-supports.json` here, commit and push. Check it first
if you like:

```powershell
.\tools\blocklist.ps1 -Verify .\mep-supports.json
```

`store\issued-copies.md` in the add-in's repository is the record of which copy id belongs to whom.

## To release everything again

Sign an empty list (`.\tools\blocklist.ps1` with no `-Block` or `-Machine`) and push it, or make this
repository private again. Either way the copies are free at their next check, within a day of being online.
