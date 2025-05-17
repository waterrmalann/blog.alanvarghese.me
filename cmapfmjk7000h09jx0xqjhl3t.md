---
title: "Auth is a product, not a feature."
datePublished: Sun Feb 23 2025 18:30:00 GMT+0000 (Coordinated Universal Time)
cuid: cmapfmjk7000h09jx0xqjhl3t
slug: auth-is-a-product-not-a-feature
tags: authentication, backend, apis, best-practices

---

“Just generate a token and check it later, right?”

You build your app, add a login form, generate a JWT, and call it a day. A couple of days later, a user complains they keep getting logged out. Then you start wondering: should the token be long-lived? Should you store it as a cookie or in `localStorage`? Wait, what about CSRF? What about logging out from other devices?

Welcome to the **hellhole that is authentication**.

It seems like a simple problem until you try to do it properly but that’s not just the bad part, a tiny mistake could open you up to serious security issues. But what exactly are these issues and why is there a collective panic anytime someone says they’re building their own auth?

The devil is really in the details. Token expiry, refresh logic, secure cookie flags, `HttpOnly` settings, `SameSite` policies, invalidation on logout, multi-device sync, replay protection and a whole load of other things. Miss just one, and you’ve got yourself a vulnerability.

## What good auth actually involves

Let’s try to sum up a non-exhaustive list of what a proper authentication system includes:

* You’ve got to securely store the passwords, and that involves salting, peppering, tuning cost factors.
    
* There is the choice between Sessions vs JWTs where the tradeoffs are lack of control and scalability.
    
* And if you go the stateless route, there’s refresh tokens! Yes, we’ll get to those in a bit.
    
* Account recovery - email flows, rate-limiting, token expiration.
    
* 2FA / MFA - TOPT, SMS, hardware keys?
    
* The ability to logout from one device without killing all others, ie. token revocation.
    
* Finding the right place to store tokens: Local Storage, Cookies (and their flags: `HttpOnly`, `Secure`, `SameSite`)
    
* Abuse prevention by rate-limiting, IP-banning, brute-force prevention
    
* Device/session tracking: Users expect to see “You’re logged in one these devices.”
    
* Social login support for UX, and dealing OAuth2 dance, provider-specific quirks.
    
* Proper audit logging where you can track access patterns and help users spot suspicious activity.
    

Like I mentioned, this list isn’t complete. And if you skip any of these, your system either sucks for users, or is a security liability.

## JWTs

Most people start with access tokens: JWTs that grant API access. You issue it on login, the client stores it somewhere, and it’s sent with every request.

But access tokens carry a dilemma: lifespan vs security. If you make them short-lived, users get logged out constantly. If you make them long-lived, a stolen token is good forever.

And this is how the duct-taping usually starts, you either:

* Crank up the expiry to 30 days and hope for the best
    
* Introduce opaque session tokens and store them server-side
    

Or - correctly - you bring in refresh tokens.

### Why you need refresh tokens

For a long time, I thought refresh tokens were pointless. “If the client needs to store both access and refresh tokens anyway, and if the refresh token gets compromised, it’s game over… so why bother?”

It’s not like you can keep Refresh tokens in a magical untouchable box client-side, if that were the case, Access tokens should be stored with the same level security and now having two tokens doesn’t make sense.

That is only partially correct. Refresh tokens do not give you more security by being immune to theft - in fact, I’d argue they’re just as susceptible to compromise. What they *do* offer is a mechanism for enabling **token lifecycle management** in an otherwise stateless system.

Access tokens (like JWTs) are stateless and typically short-lived. Once issued, you can’t revoke them unless you maintain state, which defeats their purpose.

Refresh tokens introduce state - typically stored securely server-side or with a trusted auth provider. This allows:

* Revocation (e.g. on logout or suspicious activity)
    
* Rotation (Detect and mitigate replay attacks)
    
* Longevity (potentially infinite token lifetime, offering superior UX)
    

A reasonable concern is that refresh tokens are inefficient because you need a DB lookup. But that’s misleading:

* Access tokens hit your APIs hundreds of times per session.
    
* Refresh tokens are used once every 5-15 minutes - or less.
    

In a typical setup, an access token might be used in 100+ API requests before a refresh token needs to be checked once. Meanwhile, the added benefits like revocation and session control far outweigh this minor overhead which you can always optimize away with in-memory stores or token hashes.

Of course, like I mentioned: a refresh token isn’t a magic bullet. If it’s stolen, you’re still in trouble. That’s why good security practices are key:

* The tokens must be stored securely (e.g. `HttpOnly`, `Secure` cookies for web apps)
    
* Use rotating refresh tokens (issue a new one every time it’s used and invalidate the old one)
    
* Associate additional metadata like user agent, IP, client info, app version, rough geo-location with refresh tokens and cross-reference when revalidating.
    
* Implement heuristics to detect refresh token reuse, or other suspicious activity if there is mismatch in metadata
    

Modern users don’t want to log in every day. They want to:

* Stay signed in across tabs and sessions.
    
* Re-authenticate only when doing sensitive things (eg. changing passwords)
    
* See what devices are signed in and revoke them individually
    

All good authentication systems have similar implementations in place, but they also have years of iteration, battle-testing, and real-world attack resilience baked into them. This is why you shouldn’t build your own authentication system unless of course you’re fully prepared to replicate the depth of security considerations that platforms like Auth0, Firebase Auth, Cognito, or Supabase handle out of the box, elements that we have covered in this article - and then some.

If you’re rolling your own auth, treat it with the same product rigor you give your main offering. Otherwise, outsource it to someone who does because anything less, is a **liability**.