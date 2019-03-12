# twitter_ux_bookmarklet
A small bookmarklet for slightly modifying Twitter's UX to focus on the timeline only. This bookmarklet will remove the sidebar ("Who to follow", "Trending", and meta links), and increase the width of the timeline. This allows you to see an uncluttered, focused timeline of tweets with more area for the content.

These changes will persist as long as you are on the page. If you tweet, or change the page in any way, you will have to re-run the bookmarklet.

## How to use it
Create a bookmarklet by creating a new bookmark, but instead of a normal URL, use the javascript text (all of it!) found in the file included here. Now, when you click that bookmark, it will execute the javascript on the page you're looking at.

Alternatively, you can just copypasta the javascript portion (without the `javascript:` in the beginning) into the console.

## What it does
This bookmarklet searches through the elements on the page until it finds the "primary column", which right now is most easily found by `div[data-testid=primaryColumn]`. There is also an alternative version that takes the long way around: `document.getElementsByTagName('main')[0].firstChild.children[1].firstChild.firstChild`.

Once it finds the primary column, where all the tweets are listed, it does the following:
* Removes the sidebar column
* Sets the primary column max-width to 100%, essentially removing the limit
* Sets the margin on the primary column to `10px auto`, essentially removing forced margins
* Sets the max-width of the actual tweet container (several elements down) to 100%, essentially removing the limit

I could further change max-widths and margins for parent elements going up a few levels to make the available width 100% of the browser window width, but there is UX value in having some limit. Having a scrap of text take up 100% of a window's width makes it harder to read, and also would stretch out the images/videos unnecessarily.

## Technical Information
Twitter, and lots of big app companies in general, have made it a pain in the ass to crawl their DOM to find stuff. You can't hook onto IDs or classes because they're randomly generated and frequently change. In fact, nearly anything could be changed and completely screw up similar functions. 

So, instead of that, I am focusing on semantic and mandatory attributes of the elements in question, which could theoretically be changed or omitted, but then the site would be non-compliant or non-responsive and therefore buggy. Those elements are:
 
* `main`. They've chosen to use the `main` element in constructing their site, and have used it to encapsulate the div soup that encapsulates the two columns we care about.
* `[data-testid]`. They're using a React testing library, which grabs onto `[data-testid]` attributes, and they have left those attributes in in the compiled code. 

This bookmarklet is pretty fragile, not gonna lie. They could: 
* change their `[data-testid]` attribute name, 
* use a React library (which exists, I looked it up) to specifically remove that attribute at compile time, 
* add or remove more div soup,
* change out `main` for just another non-semantic element like `div`

## Why?
I don't look at the side bar. The "Who to follow" panel has never been relevant, and has never shown me anyone I would want to follow. The "Trending" panel... I just don't give a shit. I'm not the audience for that, as I'm not some kind of power user that might care what's trending. What I want is to see a timeline of my tweets, and for that to be the focus of the page. Ideally, the sidebar would be collapsable, but that's just not in the cards.
