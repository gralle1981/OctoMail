# OctoMail

A mailbox enhancement for WoW 1.12, tailored for [OctoWoW](https://octowow.st).
Maintained by **Roby_Brok**. Part of my [OctoWoW addon setup](https://github.com/roby-brok/octowow-addons).

This is a fork of **[TurtleMail](https://github.com/sica42/TurtleMail)** by **shirsig** and
**sica42** — all of the actual work is theirs. I renamed it and fixed two bugs.

---

## What it does

An extension to the Blizzard mail interface: open all mail with one button, autocomplete recipient
names, a searchable log of everything sent and received, and auction mail marked up so you can see
at a glance what sold, expired or was outbid.

## What this fork changes

**Fixed: the autocomplete list never stopped growing.** It kept a "last seen" stamp per recipient
and dropped anyone unseen for 30 days — except the stamp was written with `GetTime()`, which counts
seconds since the *client launched* and resets to roughly zero at every login. A stamp from an
earlier session is therefore usually *larger* than the current reading, so the subtraction came out
negative and the check could only ever pass during one unbroken 30-day session. In practice nothing
was ever removed. Now uses `time()`, an absolute timestamp that survives logging out.

**Fixed: the mail log was never trimmed.** Every mail sent or received was appended to a saved
variable with no limit, so it grew for the life of the character and was parsed back in full at
every login. Now capped at the most recent 500 entries per log, oldest dropped first.

**Renamed to OctoMail**, with the saved variables moved to match. Slash commands are `/octomail`
and `/om`.

## Coming from TurtleMail

Your data carries over, but the timing matters:

1. Install OctoMail **alongside** TurtleMail
2. Log in once — your mail log, autocomplete names, saved recipient and window position are
   imported, and you will see a message confirming it
3. Remove TurtleMail

The import can only read TurtleMail's saved variables while TurtleMail is still installed, because
that addon is what loads them. Delete it first and there is nothing left to read — the data is not
destroyed, but OctoMail cannot reach it. Importing runs once and never overwrites anything you have
already accumulated.

`/tm` is deliberately **not** registered, so it cannot fight with TurtleMail during that one
overlapping login. Use `/om`.

## Install

1. **[Download](https://github.com/roby-brok/OctoMail/archive/refs/heads/master.zip)**
2. Unpack the zip
3. **Rename the folder `OctoMail-master` to `OctoMail`** — this step is not optional
4. Move `OctoMail` into `Wow-Directory\Interface\AddOns`
5. Restart WoW

Step 3 matters because WoW only loads an addon when the folder name matches the `.toc` inside it. A
folder called `OctoMail-master` containing `OctoMail.toc` is skipped in silence — no error, no entry
in the addon list, it simply never runs.

## Credits

* **[shirsig](https://github.com/shirsig)** and **[sica42](https://github.com/sica42/TurtleMail)** —
  wrote TurtleMail, which is essentially all of this addon.
* Localisations (de, es, fr, ru) came with the original.

Neither fix is OctoWoW-specific and both apply equally upstream. If sica42 wants either one, it's
theirs to take — I forked rather than opened a pull request only because the rename made it
unsuitable as a patch.
