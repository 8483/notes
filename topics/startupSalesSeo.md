# Indexing

1. Check if site is indexed by Google.

```
site:domain.com
```

2. Register website with Google Search Console

-   Add domain as property to console.
-   Add DNS TXT record to server: `google-site-verification=JKDfkaFliasyfIR&l4VT&v4nLNCtlxs`
-   Index/crawl `domain.com` with URL inspection.
-   Add sitemap.

# Sitemap

> Add a sitemap **even if** you only have one page.

**NOTE:** Place the `sitemap.xml` file in the site's root directory.

Search engines like Google use this file to more intelligently crawl the website. It's a way for webmasters to inform search engines about pages on their sites that are available for crawling.

If you manage a website, it's a good idea to maintain an XML sitemap and submit it to search engines. This can help ensure that they're aware of all the pages on your site, especially if your site is large or has pages that aren't easily discovered by the standard crawling process.

While a sitemap might not be strictly necessary for every website, it's a best practice that can help ensure full indexing and provide additional benefits. It's especially useful for larger, dynamic, or new sites.

-   **XML Sitemaps:** These are primarily intended for search engines. They provide a list of URLs on a site, and can also offer additional metadata about each URL (when it was last updated, how often it usually changes, and how important it is, relative to other URLs in the site). Typically, **it's not common (or recommended) to include anchor links in XML sitemaps**. Search engines are interested in crawling distinct pages, and an anchor link usually just takes you to a different section of an already indexed page. If you list an anchor link as a separate entry in an XML sitemap, it doesn't provide meaningful or additional information to search engines.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
   <url>
      <loc>http://www.domain.com/</loc>
      <lastmod>2022-09-01</lastmod>
      <changefreq>monthly</changefreq>
      <priority>0.8</priority>
   </url>
   <url>
      <loc>http://www.domain.com/about.html</loc>
      <lastmod>2022-09-10</lastmod>
      <changefreq>weekly</changefreq>
      <priority>0.5</priority>
   </url>
   <!-- Additional URLs can be added similarly -->
</urlset>
```

-   **HTML Sitemaps:** These are intended primarily for human users. They usually list all of the pages on a website in a clear, hierarchical format, and often link to each of those pages. It helps users navigate and find content on the site. Since these are designed primarily for human users, **you can certainly include anchor links if you believe it will aid navigation or clarity for your site visitors**. For instance, if you have a long FAQ page, and you want to provide users with direct links to specific questions, you might list these anchored sections in an HTML sitemap.

**NOTE: These are different from `index.html`. They are just for indexing and are submitted as `sitemap.html`.**

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>Our Website's Sitemap</title>
    </head>
    <body>
        <h1>Website Sitemap</h1>

        <h2>Home</h2>
        <ul>
            <li><a href="/">Homepage</a></li>
        </ul>

        <h2>About Us</h2>
        <ul>
            <li><a href="/about/history/">Our History</a></li>
            <li><a href="/about/team/">Our Team</a></li>
            <li><a href="/about/mission/">Our Mission</a></li>
        </ul>

        <h2>Products</h2>
        <ul>
            <li><a href="/products/category1/">Category 1</a></li>
            <li><a href="/products/category2/">Category 2</a></li>
            <li><a href="/products/category3/">Category 3</a></li>
        </ul>
    </body>
</html>
```

# SEO Rules

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

SERP = search engine results page

```html
<!-- In order of importance -->

<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />

    <title>domain</title>

    <link rel="shortcut icon" href="/domain_favicon.png" type="image/x-icon" />
    <link rel="icon" href="/domain_favicon.png" type="image/x-icon" />

    <meta name="description" content="Detailed description in 50-150 characters." />
    <meta name="robots" content="index, follow, notranslate" />

    <meta http-equiv="Content-Type" content="text/html" />
    <link rel="canonical" href="https://domain.com/" />

    <meta name="keywords" content="some, key, words" />

    <meta property="og:type" content="website" />
    <meta property="og:url" content="https://domain.com" />
    <meta property="og:site_name" content="domain" />
    <meta property="og:title" content="domain" />
    <meta property="og:description" content="Detailed description in 50-150 characters." />
    <meta property="og:image" content="https://domain.com/domain_banner.jpg" />
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

# Google page ranking

-   **Relevance**

    -   Identify pages relevant to the search query.

-   **Content Quality**

    -   **Comprehensiveness**: Comprehensive and complete content.
    -   **Freshness**: Updated content.
    -   **Content Structure**: Proper use of headings and readability.

-   **Backlinks**

    -   **Number of Linking Domains**: Unique domains linking to the page.
    -   **Authority of Linking Domains**: Weight of links from authoritative sites.
    -   **Anchor Text of Backlinks**: Context from clickable text of a link.
    -   **Nature of the Links**: Value of editorial vs. non-editorial links.

-   **User Experience**

    -   **Mobile Usability**: User-friendliness on mobile.
    -   **Page Speed**: Load times.
    -   **Bounce Rate & Dwell Time**: User interaction metrics.

-   **Technical SEO**

    -   **Secure and Accessible Website**: HTTPS and structured XML sitemap.
    -   **Page Structure**: Proper use of HTML tags.
    -   **Internal Linking**: Links between pages on the same domain.

-   **Search Intent**

    -   Matching user's intended purpose behind queries.

-   **User Engagement**

    -   Metrics like click-through rate (CTR).

-   **Domain Factors**

    -   **Domain Age**: Potential advantage of older domains.
    -   **Domain History**: Trustworthiness based on history.
    -   **TLD (Top-Level Domain)**: Treatment of specific TLDs.

-   **Localization**

    -   Results relevant to searcher's region.

-   **Personalization**

    -   Based on search history and previous site visits.

-   **Social Signals**

    -   Correlation with content engagement on social media.

-   **Brand Signals**
    -   Recognition and trustworthiness of brand searches.

# Links

[Source](https://youtu.be/-B58GgsehKQ)

Useful links:

-   https://old.reddit.com/r/seogrowth/comments/qwsdin/seo_is_easy_the_exact_process_we_use_to_scale_our/
-   https://old.reddit.com/r/Entrepreneur/comments/sesdxr/recently_hit_6600000_monthly_organic_traffic_for/
-   https://old.reddit.com/r/Entrepreneur/comments/syt6r2/10_step_local_seo_checklist_i_use_to_rank_local/
-   https://old.reddit.com/r/smallbusiness/comments/sryd1p/seo_consultant_charging_family_business_300month/
