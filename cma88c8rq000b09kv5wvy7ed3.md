---
title: "The identity crisis with open-source"
datePublished: Sat May 03 2025 12:59:31 GMT+0000 (Coordinated Universal Time)
cuid: cma88c8rq000b09kv5wvy7ed3
slug: the-identity-crisis-with-open-source
tags: opensource, oss, cncf

---

For a long time, open source was a simple idea: share your code, let others use it, improve it, and build on it freely. The movement was mostly powered by a passion for the craft and community. In recent years however, open source has been going through something of an identity crisis. Companies that once championed open source are now pulling back, changing licenses, or pulling their projects out of open foundations. The latest example is [Synadia’s attempt to reclaim control of NATS from the Cloud Native Computing Foundation (CNCF)](https://thenewstack.io/synadia-attempts-to-reclaim-nats-back-from-cncf/) sparks concerns about whether even “neutral” foundations can hold open-source communities together. But in a recent twist, [Synadia and CNCF found common ground](https://www.synadia.com/blog/synadia-response-to-cncf). While NATS will stay under CNCF, the drama reveals deeper cracks in how OSS is governed, funded, and protected from corporate tug-of-war.

## NATS vs CNCF

NATS is a lightweight messaging system designed for high performance, scale, and simplicity. It’s used in everything from IoT to microservices. Back in 2018, Synadia donated NATS to CNCF, trusting the foundation to be a neutral home for its growth. But then, Synadia surprised the ecosystem by formally requesting to pull NATS out of CNCF as they no longer felt ‘aligned’ with the foundation’s direction and wanted tighter control over the project’s future. This was unprecedented. No one had tried to pull a project out of CNCF before.

But after a tense week of backchannel negotiations and public reaction, Synadia and CNCF struck a deal. Synadia handed over the NATS trademark registrations to the Linux Foundation. CNCF retained control of the domain, Github repos, and project assets. Synadia, while still a key contributor, committed to respecting neutral governance. If they ever fork NATS into a proprietary product, they’ll do so under a different name.

The project didn’t fracture - but the possibility was real. And it highlights this dilemma: OSS creators want community support, but they also don’t want their work being commercialized out from under them.

## But this wasn’t the case with MongoDB, Redis, and HashiCorp.

Unlike NATS, these projects didn’t bother with diplomacy. They just changed the license.

MongoDB switched to the Server Side Public License (SSPL), a license rejected by the OSI as incompatible with the Open Source Definition. This was a direct shot at AWS and other cloud providers reselling hosted MongoDB

Redis recently did the same. After years of fighting off Redis-as-a-service clones, they dropped the permissive BSD license in favor of a dual license model: one open, one commercial. That rubbed a lot of people the wrong way. In fact, it led to the birth of a new fork called [Valkey](https://valkey.io/), backed by people who wanted to keep Redis truly open.

HashiCorp, known for tools like Terraform and Vault, made its own pivot in 2023. It moved many of its core projects to the Business Source License (BSL), a not so open license model that shuts down all competition. A movement which gave birth to [OpenTofu](https://opentofu.org/).

**Notice the pattern?** A company builds a great open-source tool, it becomes popular, and then cloud providers start offering it as a service. The original creators see little to no benefit from this. Worse, they sometimes get outcompeted by their own code. It’s no surprise that they’ll get defensive and start drawing lines around what’s truly “open”.

Foundations like CNCF were supposed to help with this, offering neutral governance and a home for these projects. But governance isn’t the same as protection. And once a project is in a foundation, pulling it out isn’t easy or even allowed.

And what happens when companies reclaim their code or change the rules? Forks. Fragmentation. Confusion. Communities split. Trust erodes. Developers don’t know which version to use. Users worry about long-term support. It creates noise, and noise isn’t good for ecosystems.

With the unchecked leverage of hyperscale cloud providers, it’s easy for AWS, Google Cloud, and Azure to exploit the openness of licenses like Apache 2.0 or MIT and routinely package open-source software into managed services, contribute little upstream, and churn out billions. I’m not saying they don’t contribute back or fund projects, they only do so when it aligns with their business interests, the problem here is asymmetric value extraction.

OSS isn’t dying, it’s mutating. And the next version won’t be free as in beer. If we don’t redefine what ‘open’ means in the age of hyperscalers, we’ll need new licenses, sustainable models where creators benefit proportionally from downstream commercialization, or at the very least, more honest conversations about what “open” really means. Because if we don’t, we’ll keep seeing these license pivots and more communities left picking up the pieces.