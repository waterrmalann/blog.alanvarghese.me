---
title: "Your users aren't IP addresses."
datePublished: Tue Mar 11 2025 18:30:00 GMT+0000 (Coordinated Universal Time)
cuid: cmam1mksi002l09la3s8gddyn
slug: your-users-arent-ip-addresses
tags: engineering, backend, apis, ratelimit

---

**Let’s get this out of the way first:**

I’m *not* saying IP-based rate limiting is bad. Sometimes it’s the only thing you can. But if you’re still defaulting to IPs every time without thinking twice, you’re doing your users and yourself a disservice.

There are better ways. Smarter ways. And in most cases, more *accurate* ways.

**IP-based limits kinda suck:** Sure, rate limiting by IP looks simple enough. It’s easy to implement, gets the job done, and your reverse proxy probably already has a config for it. But IPs are flaky, shared, and frankly, just not a reliable way to identify a person anymore.

Just to name a few areas where traditional IP-based abuse prevention fails:

* **NATs and Mobile Networks:** Thousands of users behind a cellular carrier or corporate proxy may appear under a single IP.
    
* **Shared Wi-Fi:** Coffee shops, libraries, or events with a public Wi-Fi can skew IP uniqueness.
    
* **Dynamic IPs and VPNs:** Many users rotate through IPs or tunnel through shared VPNs, making IPs transient.
    

And here’s another common case I’ve seen in production environments: Even your own infra might be screwing you. Using a chain of reverse proxies or doing Backend-For-Frontend (BFF) (eg. with Next.js → API Server) or even running a custom gateway layer?

Then you’re not even getting the *real* client IP unless you explicitly forward it via sub-standard headers like `X-Forwarded-For` or `X-Real-IP` and if you don’t know what you’re doing and did not [configure it to spec](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/X-Forwarded-For#security_and_privacy_concerns), any client could easily [spoof the headers](https://www.stackhawk.com/blog/do-you-trust-your-x-forwarded-for-header) and now you’re in deeper shit.

In any of these cases, you risk false positives blocking innocent users and delivering a poor UX + you have weak rate limiting that can easily be bypassed with VPNs or IP rotation.

### Identify what you actually want to limit:

Your intent behind rate limiting should guide implementation. If you want to prevent abuse from bad actors, protect your resources or enforce fair use policies per account, token or session. These are user-centric goals. So, your limiter must be identity aware.

> Is this specific user doing too much, too fast?

And the best way to answer that isn’t “What IPs are they on?” but:

* Who are they? (user identifier)
    
* What token are they using? (API Key, JWTs)
    
* How are they interacting? (Device IDs, Session, Cookies, etc.)
    

If you’ve got a logged-in user, great - limit based on their ID.  
If it’s an API? Use the token.  
If they’re anonymous? Fine, *then* maybe IP is your fallback - *but* that’s fallback, not default.

If you get this right, you tie abuse to the actual bad actor, not collateral victims.

### Hybrid is smarter

You don’t need to pick one hammer for every nail. Use layers:

* Logged-in user: limit per user ID.
    
* API requests: limit per token
    
* Anonymous traffic: maybe IP + some sort of fingerprint
    
* Set rules to detect sudden spikes in usage, suspicious access patterns.
    
* Listen for signals from abuse-prevention layers.
    

Good systems adapt based on context and behavior. Bad ones just frustrate your users because they happen to be using a shared IP.

### Still, IP limiting isn’t always dumb.

Let’s be fair: There *are* legit cases where IP limiting is your best or only shot,

* You’re under a DDoS
    
* You don’t have auth yet (think landing pages or pre-login APIs)
    
* You’re rate limiting at the CDN or edge layer
    

Totally fine. Just don’t build your whole defense strategy around it.

### TL;DR

**IP addresses ≠ users**

They’re guesswork at best and actively misleading at worst.

If you actually care about fairness, abuse prevention, and UX - rate limit the *user*, not the wire they happen to be using that day. Use IPs only when you *have* to and always try to do one step better.