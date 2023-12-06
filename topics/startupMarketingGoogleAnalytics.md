# Overview

> If you don't track conversions, you just have a really fancy hit counter.

Ex. Add items to shopping cart, fill out a lead-gen form, download coupons, find a phone number, completing a sale...

> It is **HIGHLY** recommended to use Google Tag Manager with Google Analytics.

| Google Analytics 4 (GA4)         | Universal Analytics (UA)  |
| -------------------------------- | ------------------------- |
| Everything is an event           | Page views and sessions   |
| Account > Property / Data Stream | Account > Property > View |
| Engagement                       | Bouces, Session duration  |
| Exploration (data drilling)      | 30 fixed reports          |
| Conversions (events)             | Goals                     |

# Account

```
Google Account > GA Account > Property > Data Stream (generates new tracking ID)
```

![](../pics/startup/marketing/analytics-account.jpg)

**Create separate properties for each product. A web app and a website for the same product are separate properties.**

If you are an agency, then each client is a separate account. Do not store all client properties under one agency account.

Also, each business is a separate account, not a property.

```
- Google Account
    -   Company Name (GA account)
        -   Product 1 - Web app (property)
            -   Web app (data stream)
        -   Product 1 - Website (property)
            -   Website (data stream)
        -   Product 2 - Mobile app (property)
            -   Mobile app (data stream)
        -   Product 3 - Website (property)
            -   Website (data stream)
```

# Settings

Set data retention from 2 months to 14 months.

```
property > data settings > data retention.
```

Ignore internal traffic.

```
stream > configure tag settings > define internal traffic
```

If you have payment gateways, ignore their referral traffic.

```
stream > configure tag settings > list unwanted referral
```

# Dimensions

**Source**

Traffic origin ex. search engine (Google), a social media platform (Facebook), or a specific website (nytimes.com).

**Medium**

General source category. If the source is Google, the medium might be `organic` (unpaid search traffic) or `cpc` (paid search traffic). Other common medium types are `email`, `social` and `referral`.

**Campaign**

Traffic from a specific marketing campaign or promotion. Ex. During a holiday sale, tag the URLs in your marketing materials with a campaign name like `holiday_sale_2023`.

# Reports

-   **Acquisition**
    -   User acquisition - **ONLY** the first session i.e. first visit source.
    -   Traffic acquisition - the other sessions.
-   **Engagement**
    -   Events - All events.
    -   Conversions - Only events marked as conversions.
    -   Pages - Which pages were visited. Use path (not title) as dimension.
    -   Landing pages - Pages where sessions started.
-   **Monetization**
    -   Ecommerce purchases - Track product purchases with revenue.
-   **Demographics** - Country, language, age, gender...
-   **Tech** - Browser, device, OS...

You can use comparisons to base the whole data on a specific dimension.

# Explorations

-   Free form (pivot table)
-   Funnel
-   Path (Sankey)

# Events

1. Automatically tracked (Default) - Session starts...
2. Enhanced measurements (Extra automatically tracked events) - Scroll, download... (configured in data stream)
3. Recommended events - Predefined/standardized like add_to_cart, signup, checkout...
4. Custom events - Whatever you want.

# Custom events

These require to be linked with custom dimensions.

```
admin > property settings > data display > custom definitions
```

# Admin DebugView

Track events in real time. Use it with Google Tag Manager to verify events are fired. These events take 24-48 hours to show up in reports.

```
admin > property settings > data display > DebugView
```

# UTM Tags

These are dimensions in Google Analytics.

```
https://www.example.com?utm_source=newsletter&utm_medium=email&utm_campaign=summer_sale
```

-   **utm_source** - Which site sent the traffic (ex. Google, newsletter).
-   **utm_medium** - The type of link used, such as cost per click or email.
-   **utm_campaign** - A specific product promotion or strategic campaign (ex. spring_sale).
-   **utm_term** - Identifies search terms.
-   **utm_content** - What specifically was clicked to bring the user to the site, like a banner ad or a text link. It's often used for A/B testing and content-targeted ads.

**Example**

**1. Scenario**

-   You are promoting a "Winter Book Sale".
-   You're using an email newsletter to reach your audience.
-   The campaign includes a specific link to a landing page for the sale.
-   You want to track which book genre advertisement in the email is more effective.

**2. Base URL**

-   This is the URL of your landing page for the Winter Book Sale. Example: `https://www.bookstore.com/winter-sale`

**3. UTM Parameters**

-   **utm_source** - This identifies where the traffic is coming from. Since it's an email newsletter, you might use `newsletter`.
-   **utm_medium** - This is the marketing medium. In this case, it's `email`.
-   **utm_campaign** - This identifies the specific campaign. You could name it `winter_book_sale`.
-   **utm_term** - This is used for paid search to identify keywords. In a non-paid context like an email, it's less common, but you could use it to specify the target audience, like `young_adults`.
-   **utm_content** - This helps differentiate similar content within the same ad or link. Suppose you have two genre advertisements in the same email, like 'Science Fiction' and 'Mystery'. You could create two URLs, one with `utm_content=sci-fi` and another with `utm_content=mystery`.

**4. Complete UTM-Tagged URL**

```
https://www.bookstore.com/winter-sale?utm_source=newsletter&utm_medium=email&utm_campaign=winter_book_sale&utm_term=young_adults&utm_content=sci-fi
```
