# How IDS/IPS Works

## The core difference from a firewall

A firewall decides what traffic is *allowed to enter* in the first place, based on rules like IP/port/protocol. An IDS/IPS operates differently — it assumes some traffic will get through (because it's technically "allowed" — right ports, right protocol) and instead watches for *behavior* that looks malicious, even from traffic a firewall would happily pass.

**IDS (Intrusion Detection System):** watches traffic, flags anything suspicious, and alerts someone — but doesn't block anything itself. It's a smoke detector: it tells you something's wrong, but doesn't put out the fire.

**IPS (Intrusion Prevention System):** does the same detection, but sits inline (directly in the traffic path) and can actually block or drop the malicious traffic automatically, in real time. It's the smoke detector wired directly to the sprinkler system.

## How it actually detects something is wrong

There are two main approaches, often used together:

**Signature-based detection**
Compares traffic against a database of known attack patterns — similar to how antivirus uses malware signatures. If a packet matches a known SQL injection pattern or a known exploit's traffic fingerprint, it gets flagged.

- Strength: very accurate for known, previously-seen attacks, low false-positive rate.
- Weakness: useless against a brand-new attack it doesn't have a signature for yet (zero-days).

**Anomaly-based detection**
Instead of matching known patterns, it builds a baseline of "normal" traffic/behavior for a network, then flags anything that deviates significantly from that baseline — a sudden spike in traffic to an unusual destination, a device suddenly making connections it's never made before, unusual login times, etc.

- Strength: can catch novel/unknown attacks that don't match any existing signature.
- Weakness: prone to false positives, since "unusual" isn't always "malicious" — a legitimate but unusual event can trigger an alert.

Most real IDS/IPS deployments (like Snort or Suricata) use a mix of both: signatures for known threats, plus some anomaly/heuristic detection layered on top.

## Where it sits on the network

- **Network-based (NIDS/NIPS):** monitors traffic across a whole network segment, usually placed at a choke point like right behind the firewall, so it sees everything crossing that boundary.
- **Host-based (HIDS/HIPS):** runs on an individual machine, watching that specific system's logs, file changes, and process activity rather than network traffic broadly.

## Why the IDS vs IPS choice matters

Putting a system inline (IPS) to actively block traffic sounds strictly better, but it comes with real risk: if it has a false positive, it can actively block legitimate traffic (potentially taking down something important). An IDS is "safer" in that sense — it never breaks anything, it just alerts — but it also means a human needs to actually notice and react to the alert before any damage is stopped. Plenty of real deployments run IDS mode for anything experimental or high-risk-of-false-positive, and only flip specific well-tested rules over to active IPS blocking.

## How this fits with a firewall

Think of the layers as: firewall decides who's allowed to knock on the door at all → IDS/IPS watches what happens *after* someone's already been let in, in case something that looked fine at the door turns out to be an attack once you look closer at what it's actually doing.
