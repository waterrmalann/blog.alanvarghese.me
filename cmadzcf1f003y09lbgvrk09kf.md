---
title: "Reducing blast radius when designing APIs"
datePublished: Tue Apr 29 2025 18:30:00 GMT+0000 (Coordinated Universal Time)
cuid: cmadzcf1f003y09lbgvrk09kf
slug: reducing-blast-radius-when-designing-apis
tags: apis, best-practices, server-driven-ui

---

I’ve been designing APIs for almost a decade now and one of the things that I frequently see developers get wrong is with designing APIs that are stiff and tightly coupled to their clients.

Modern software demands velocity: new features, experiments, and ever-evolving user expectations. This post distills a hard-earned lesson in building APIs that welcome change while minimizing cross-stack regressions.

The *blast radius* of an API is the scope of impact it has across the stack. In systems where the clients are tightly coupled to the API, a poorly scoped backend change that mutates the response format, removes a field or introduces other side effects can unintentionally cripple consuming clients.

Sure, API versioning helps avoid regressions, but it sidesteps the root problem: **a large blast radius**. Modern APIs assume the “frontend” will handle it but if you really want to build resilient, [RESTful APIs](https://htmx.org/essays/how-did-rest-come-to-mean-the-opposite-of-rest/), the server should at least partially drive the state.

I’ll illustrate this with a very trivial example; suppose you want to aggregate some data for analytics and send it to an admin dashboard that renders some overview cards. Here’s the usual suspect:

```json
{
    "usersCount": 1300,
    "totalSales": 4299,
    "activeSessions": 84
}
```

Any consuming frontend client would typically pass this data into a shared card component and move on, assuming the labels for each aggregate.

But what happens if we change `usersCount` to `totalUsersCount` and add an additional `totalOrders` field to the API’s response payload? Any consuming client will break - some won’t recognize the renamed field, others won’t display the new data at all.

Sure, clients could mitigate a part of this through well-implemented error handling practices or abstracting away the API, but the core issue remains. Ideally the API should’ve returned an array of generic card descriptors instead:

```json
{    
    "overview": [
        { "label": "Users", "value": "1,340" },
        { "label": "Total Sales", "value": "$4,299" },
        { "label": "Active Sessions", "value": "84" }
    ]
}
```

The consuming client can simply loop through the array and render the label and values into a card component. Notice how we solved multiple issues here:

* The backend now decides what label to call the card
    
* The backend also decides how the numbers should be formatted.
    
* The frontend automatically adjusts to any additions, removals, or edits in the overview aggregates.
    
* The frontend logic remains minimal and stable. No redeployment is needed for any change here.
    

Of course this is not the ultimate server driven response payload, and you could do even better than this, but we’ve certainly and greatly reduced the impact surface of changes. The cost and time required to change and redeploy really adds up when you have multiple clients, which is often the case with cross-platform applications which work across Web, Android, and iOS, especially mobile where release cycles typically take days.

As with any design decision, going server-driven has its fair share of tradeoffs. Even in the example that I showed you, there are notable concerns:

* The client loses control over the presentation. In real-world cases, the frontend may need to determine formatting based on locale or user preferences which is not going to work if the backend already sends it as a pre-formatted string.
    
* Particular to numbers, reversible UI becomes a challenge - what if you have a slider that changes the value of the numbers you received, you can’t reliably operate on a pre-formatted string and also keep a consistent format.
    
* Internationalization (i18n) and user customization also becomes more difficult unless you’re very deliberate.
    

But these are not dead ends. In most real-world systems, it’s standard for the backend to be aware of user preferences like locale, time zone, language, among other properties. In such a case, you could return an appropriately tailored response.

But even if that context is not available, there are alternatives. For instance, with the number formatting issue: The backend can establish an interface with the clients for how numbers will be exchanged.

```typescript
type UIBaseNumber = {
   value: number;
   locale: string;  // IETF BCP 47 language tag, eg. en-US
   rounding?: number; // number of decimal places to round to
}

type UIPlainNumber = UIBaseNumber & { format: "plain" } // eg. 36,200
type UIPercentage = UIBaseNumber & { format: "percentage" }; // eg. 28%
type UIOrdinal = UIBaseNumber & { format: "ordinal" }; // eg. 3rd, 4th
type UIAmount = UIBaseNumber & { 
    format: "currency"; 
    currency: string;  // ISO 4217 currency string (eg. USD) or the symbol (eg. $)
} // eg. $2,000

type UINumber = UIPlainNumber | UIPercentage | UIOrdinal | UIAmount;

// const formatNumber = (number: UINumber) => string;
```

Once standardized, this interface rarely needs to change. Frontend clients can implement well-tested formatting functions that can turn any data conforming to this interface into a display ready string. This way, clients can reap the rewards of server driven UI while retaining flexibility for any client-side logic.

You can get creative with it and apply the same underlying principle to most of these edge cases encountered when going server driven. Ultimately, presentation logic should remain the client’s domain wherever user experience matters deeply.

A variant of this idea but on steroids is often called [Server-Driven UI](https://medium.com/androidiots/mastering-sdui-a-deep-dive-into-server-driven-ui-8329ad90ab44), which however introduces a lot of complexities of its own, but we can take the “Server-Driven” part of it and run with that, which is good-enough for most cases.

The goal isn’t to build the perfect API, it’s to build one that can survive change without wrecking everything downstream. Tight coupling kills velocity but a small shift in control by letting the server drive structure can pay massive dividends over time.