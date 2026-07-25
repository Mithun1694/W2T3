## Firewalls: Types and Uses

## What a firewall actually does

A firewall's job is simple in concept: look at traffic trying to cross a boundary (like between your home network and the internet, or between two parts of a company network) and decide, based on a set of rules, whether to let it through or block it. It doesn't try to understand if traffic is "malicious" in a deep sense — it just enforces "this kind of traffic is/isn't allowed here."

Everything below is really a different answer to the question: *how much does the firewall actually look at before deciding?*

## Packet-Filtering Firewalls

The simplest kind. It checks each packet's basic info — source/destination IP, port number, protocol — against a rule list, with no memory of past packets.

**Use case:** basic border protection, routers/small office setups. Fast and lightweight, but easy to fool because it doesn't track connection state (an attacker can sometimes craft packets that look "allowed" in isolation).

## Stateful Inspection Firewalls

This is the modern default. Instead of judging each packet alone, it tracks the state of active connections — it knows "this packet is part of a connection I already approved" versus "this is a new, unsolicited connection." So if a reply packet doesn't match an existing legitimate connection, it gets blocked automatically.

**Use case:** the standard for most business and consumer firewalls today (built into most routers, OS firewalls, and dedicated firewall appliances).

## Proxy Firewalls (Application-Level Gateways)

Instead of just passing traffic through, a proxy firewall actually terminates the connection itself and creates a new one on the other side — so it can inspect the full content of the traffic (like the actual HTTP request), not just headers. The two sides never talk directly to each other, only to the proxy.

**Use case:** environments that need deep content inspection (e.g., blocking specific file types, filtering web content), at the cost of more processing overhead and potential latency.

## Next-Generation Firewalls (NGFW)

Combines traditional stateful filtering with extra awareness — it can identify traffic by *application* (not just port number, since apps increasingly all use port 443), do deep packet inspection, and often bundle in intrusion prevention functionality directly. Basically a firewall plus several other tools rolled into one appliance.

**Use case:** modern enterprise network perimeters where just knowing "this is port 443 traffic" isn't specific enough — an NGFW might tell you it's specifically Netflix, or Slack, or an unapproved file-sharing app, and enforce policy accordingly.

## Web Application Firewalls (WAF)

A specialized firewall that sits in front of a web application specifically, looking for attack patterns aimed at that layer — SQL injection attempts, XSS payloads, and other web-specific attacks (see the OWASP Top 10 write-up in Week 2 Task 1 for what these look like). It doesn't replace a network firewall; it protects a different layer entirely.

**Use case:** protecting public-facing web apps and APIs specifically, often deployed as a cloud service (e.g., in front of an e-commerce site).

## How to think about it

Most real networks don't pick just one — a typical setup layers several of these: a stateful firewall (or NGFW) at the network edge, plus a WAF specifically in front of any web application, plus host-based firewalls on individual machines. Each one is watching a different layer, so a single misconfiguration in one doesn't leave everything exposed.
