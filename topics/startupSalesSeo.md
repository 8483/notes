[Source](https://youtu.be/-B58GgsehKQ)

Useful links:

-   https://old.reddit.com/r/seogrowth/comments/qwsdin/seo_is_easy_the_exact_process_we_use_to_scale_our/
-   https://old.reddit.com/r/Entrepreneur/comments/sesdxr/recently_hit_6600000_monthly_organic_traffic_for/
-   https://old.reddit.com/r/Entrepreneur/comments/syt6r2/10_step_local_seo_checklist_i_use_to_rank_local/
-   https://old.reddit.com/r/smallbusiness/comments/sryd1p/seo_consultant_charging_family_business_300month/

# Rules

Rule #1: Create really good content.  
Rule #2: **Create really good content!**  
Rule #3: Bot-friendly HTML.  
Rule #4: Load content fast.

# History

Stuffing a bunch of keywords in a page does not work.

When Google first started, it used **page rank**, and algorithm which weighted relevance and search ranking based on the number of inbound links the website had.

This was promptly exploited by buying backlinks.

Currently, it is impossible to manipulate Google's ranking, which uses 200+ factors, mostly based on how a user behaves. Ex. Did the user immediately bounce, or did it spend hours clicking on links?

# Content

Simply put, a user should stay on the website. The main factors are:

-   **Click-through rate (CTR)** - How likely is a user to click on your link in the search engine ranking page (SERP). Higher is better.
-   **Bounce** - How fast the user clicked back. Lower is better.
-   **Dwell time** - Long long the user stays before bouncing. Longer is better, and the best result is the user never leaving.
    -   **Session duration** - Metric for dwell time.
    -   **Pages per session**- Metric for dwell time.

# Keyword research

There are a ton of expensive tools, but you can do this for free.

Let's say you want to sell beard trimmers. If you try to rank for "best beard trimmer", you will fail. Instead, you want to target niche searches.

You can find these by going to Google search and typing "beard trimmer" + a `letter`, and look at the suggestions. Ex:

-   "beard trimmer a" suggests "beard trimmer apron"
-   "beard trimmer b" suggests "beard trimmer bib"
-   "beard trimmer c" suggests "beard trimmer charger"
-   "beard trimmer d" suggests "beard trimmer detailer"

Keywords belong in these 4 groups.

| Funnel    | Group       | Intent                            | Example          |
| --------- | ----------- | --------------------------------- | ---------------- |
| Attention | Information | Gain general knowledge on a topic | what is DDR4 ram |
| Interest  | Navigation  | Has end destionation in mind      | newegg DDR4 ram  |
| Desire    | Comparison  | Information on future purchase    | best DDR4 ram    |
| Action    | Transaction | Intent to buy                     | DR4 ram price    |

# Body - semantic HTML

When Google crawls the site, it uses semantic HTML elements to understand the content on the page. Accessibility plays a big role as well, like `alt` on images.

```html
<body>
    <article>
        <h1>Title with keywords</h1>

        <p>Useful content...</p>

        <img src="cat.png" alt="image of a cat" />
    </article>
</body>
```

## Schema

schema.org is used for content metadata. Ex. A recipe using a schema will be formatted nicely by Google.

Totally optional, and debatable if it improves ranking.

```html
<article itemscope itemtype="http://schema.org/Article">
    <meta itemprop="datePublished" content="2021-18-11" />
    <meta itemprop="image" content="cat.png" />
    <meta itemprop="publisher" content="author.com" />
    <h1 itemprop="name headline">Article title</h1>
    <a itemprop="author" name="John Smith" href="/authors/john-smith"> John Smith </a>
    <!-- This link leads to an author page using an Author schema -->
</article>
```

## Outbound links

Signal further what the page is about. After crawling your page, Google crawls all the links on the page to understand better what the content is about.

## img alt

An important and easy way to add more metadata.

# aria

Accessible Rich Internet Applications provides meaning to highly interactive widgets on a page.

```html
<div role="progressbar" aria-valuenow="75" aria-valuemin="0" aria-valuemax="100"></div>
```

```html
<img src="cat.png" alt="image of a cat" />
```

# Head - Metadata

Bots use this data to further understand the page. It also formats the appearnace in the search engine listing.

The most important one is the title.

Vendors like Facebook and Twitter have their own tags, used to format the appearance of the link in the feed.

```html
<head>
    <title>Page title that shows up in SERP</title>

    <!-- HTML Meta Tags -->
    <meta http-equiv="Content-Type" content="text/html" charset=utf-8" />
    <meta name="description" content="Detailed description about the content" />
    <meta name="robots" content="index, follow" />

    <!-- Facebook Meta Tags -->
    <meta property="og:url" content="http://domain.com" />
    <meta property="og:type" content="website" />
    <meta property="og:title" content="Page title that shows up in SERP" />
    <meta property="og:description" content="Detailed description about the content" />
    <meta property="og:image" content="http://domain.com/pics/img.jpg" />
    <meta property="og:site_name" content="domain.com" />
</head>
```

# Rendering

## 1. Client-side rendering (Interactivity)

Single-page apps built with frameworks like React, Vue, Angluar... Where the HTML is rendered with javascript in the browser.

-   Great for UX.
-   Slow to meaningful content.
-   Might confuse bots.

**Note:** Google can index client rendered apps, but the reliability is questionable.

## 2. Static rendering (Performance)

Pre-rendered HTML cached on a CDN, where the Javascript loads after the rendering.

-   Fast bot-fiendly content
-   Data can get stale.
-   Does not scale well.

## 3. Server-side rendering (Freshness)

The HTML is rendered on the server, and the client gets the final product.

-   Bot-fiendly content.
-   Fresh content.
-   Slower due to re-rendering.
-   Data fetching redundancy.

Caching can be implemented, but it's less efficient than using static rendering with a CDN.

## 4. Incremental Static Regeneration

Available in next.js where pages are statically generated and re-deployed to the CDN on the fly.

-   Fast, fresh and bot-friendly.
-   Deployment complexity.
-   Vendor lock-in.

## 5. Hybrid Rendering

Frameworks like Angular and next.js, where all three apporaches are possible.

-   Some pages can be static.
-   Other pages can be rendered server-side.
-   And the rest can be client-side.
