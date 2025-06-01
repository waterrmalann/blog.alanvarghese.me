---
title: "Your website has a carbon footprint too."
datePublished: Sun Jun 01 2025 18:30:34 GMT+0000 (Coordinated Universal Time)
cuid: cmbdzxp1i000e09jv0dfu2qme
slug: your-website-has-a-carbon-footprint-too
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/pONBhDyOFoM/upload/536e78c6d91f4d0bb9b1396f755e0342.jpeg
tags: web-development, engineering, full-stack, reflection, climate-change

---

Most developers don’t think twice about what happens after they hit deploy. The code works, the site loads, everyone’s happy. But behind the nice UI and slick animations, your website is chewing through electricity - across servers, networks, and user devices. And yeah, that electricity usually comes from power plants that belch out CO2. So yes, your website is contributing to global warming. Not in some dramatic, polar-bear-drowning way but in a slow, silent, cumulative way. Like a leaky faucet in a city with a million leaky faucets.

Even a modest website visit produces a tangible puff of CO2. In fact, [an average web page today generates roughly 0.5](https://dodonut.com/blog/what-is-the-website-carbon-footprint/#:~:text=The%20average%20web%20page%20tested%20produces%20approximately%200.5%20grams%20CO2%20per%20page%20view.) grams of CO2 per view (about 60kg CO2 per year for 10k monthly views). Now multiply that by *trillions* of pageviews, and it’s quite clear that our slick web apps are part of the climate problem.

Most of the carbon comes from three places:

* Data centers, servers doing the backend work
    
* networks transferring bloated bundles
    
* user devices running your scripts
    

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text"><strong>Fun fact:</strong> <a target="_self" rel="noopener noreferrer nofollow" href="https://sustainablewebdesign.org/estimating-digital-emissions/#:~:text=Based%20on%20combined%20information%20from,the%20total%20system%20energy%20used" style="pointer-events: none">your user’s device often uses more energy rendering your site</a> than your server does serving it. Especially if you’re sending them half a megabyte of JavaScript to animate a button.</div>
</div>

Let’s talk JavaScript. React, Vue, Angular, - whatever - it’s all more bytes than you probably need.React alone adds ~30kb gzipped to your bundle. Angular easily goes over 100kb. You could probably write the same UI in vanilla JS with a tenth of the size. You could actually read this study on the carbon footprint of JS which shows that building a simple app in vanilla JS emitted 5.5mg of CO2 per operation while React emitted 7.97mg. That’s a 45% increase. Just from using React. Sure, it’s more maintainable, testable, whatever - but don’t pretend it’s free. Abstractions cost kilobytes. Sometimes, they cost the atmosphere.

I know this probably sounds like I’m trying to make up an imaginary problem and guilt trip you but here’s a dramatic real-world illustration: Developer Danny van Kooten famously shaved 20 kB off a WordPress plugin (removing an unnecessary JS dependency) which was used on ~2 million sites. That tiny 20 kB reduction - multiplied across all those sites’ monthly pageviews - [reduced global emissions by an estimated 59,000kg of CO2 per month](https://www.dannyvankooten.com/blog/2020/website-carbon-emissions/) equivalent to taking 86 flights from Amsterdam to New York each month (sources in the linked article). One guy. Twenty kilobytes. Sixty tons of carbon. Because it was used on millions of sites. Your project may not have that react but it’s still something.

You can use tools like [Website Carbon Calculator](https://www.websitecarbon.com/) or [Ecograder](https://ecograder.com) which lets you plug in your URL to get an estimate of the site’s emissions. Calculating this might be a little more complex when it comes to web applications than websites, but you get the idea.

Now I’m not saying drop your framework and go live in the forest and write HTML by hand, ditching a framework that makes your work faster and more maintainable isn’t practical. But it’s worth knowing what your choices mean. Frameworks like Svelte or Qwik actually try to minimize runtime JS, which means fewer bytes, faster loads, and yes - less carbon.

Use fewer fonts. Compress your images. Avoid autoplay videos. Minify your JS. Lazy-load content. And consider this: serverless and green hosting actually make a dent. A small dent, sure. But enough small dents and the whole thing starts to bend. You’ll notice that your applications load and run faster, which is a win-win for your UX as well.

You won’t save the planet with a code refactor. But that’s not the point. We made this mess one line of code at a time. Maybe we can start cleaning it up the same way. A few kilobytes saved might seem like nothing. But on the internet, “nothing” scales fast.

### Sources if you like reading:

* [Software Carbon Intensity (SCI) Specification](https://sci.greensoftware.foundation/)
    
* [Wholegrain Digital - Website Carbon Calculator](https://www.websitecarbon.com/)
    
* [Danny van Kooten — CO2 emissions on the web](https://www.dannyvankooten.com/blog/2020/website-carbon-emissions/)
    
* [Dodonut Blog — *What is website carbon footprint?*](https://dodonut.com/blog/what-is-the-website-carbon-footprint/#:~:text=,Website%20Carbon%20Calculator)
    
* [Malin Wadholm, Green and Sustainable JavaScript (MSc Thesis, 2023)](https://www.diva-portal.org/smash/get/diva2:1768632/FULLTEXT01.pdf)
    
* [Sustainable Web Design — What is the Sustainable Web Design Model?](https://sustainablewebdesign.org/estimating-digital-emissions/#:~:text=Energy%20intensity%20,GBhttps://sustainablewebdesign.org/estimating-digital-emissions/)
    
* [The cost of JS frameworks — Gzip bundle sizes for React, Vue, Angular, etc.](https://gist.github.com/Restuta/cda69e50a853aa64912d)
    
* [Energy usage by programming languages — Portugal university study (2017)](https://www.cpsmi.com/blog/energy-efficiency-in-programming-languages/#:~:text=The%20study%20also%20found%20significant,interpreted%20languages%20used%202%2C365%20joules)
    
* [Mightybytes Ecograder — Tools for calculating your website’s CO2 emissions](https://rootwebdesign.studio/articles/tools-for-calculating-your-websites-co2-emissions)
    
* [Katarzyna Wojdalska (ec0lint) — How Can You Mitigate the Carbon Footprint of Websites?](https://gitnation.com/contents/digital-ecology-how-can-you-mitigate-the-carbon-footprint-of-websites)