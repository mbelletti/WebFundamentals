project_path: /web/tools/_project.yaml
description: Official documentation for the "Performance Budgets" and "Keep Request Counts Low And File Sizes Small" Lighthouse audits.
book_path: /web/tools/_book.yaml

{# wf_updated_on: 2019-05-06 #}
{# wf_published_on: 2019-05-06 #}
{# wf_blink_components: Platform>DevTools #}

# Performance Budgets (Keep Request Counts Low And File Sizes Small) {: .page-title }

## Overview {: #overview }

<aside class="note">
  The <b>Performance Budgets</b> and <b>Keep Request Counts And File Sizes Small</b> audits both
  link to this page.
</aside>

After putting in the hard work of improving your site performance, use a *performance budget* to prevent
your site performance from regressing over time.

The **Keep Request Counts Low And File Sizes Small** audit lists the total number of requests
and transfer size of scripts, images, and so on. This
audit is available in all Lighthouse runtime environments.

<figure>
  <img src="images/requestcounts.png"
       alt="The Keep Request Counts And File Sizes Small audit."/>
  <figcaption>
    <b>Figure 1</b>. The <b>Keep Request Counts And File Sizes Small</b> audit.
  </figcaption>
</figure>

You can define a budget file to impose explicit limits on some or all of these resource types. After defining
your budget file the new **Over Budget** column tells you whether you're exceeding any of your self-defined limits.

<figure>
  <img src="images/budgetscustom.png"
       alt="A custom budget report."/>
  <figcaption>
    <b>Figure 2</b>. A custom budget report.
  </figcaption>
</figure>

When you define a budget file, the **Performance Budgets** audit only reports on the resource types that you've specified.
But you can still access the information for the rest of the types from the **Keep Request Counts Low And File Sizes Small**
audit.

Example JSON output:

```json
"performance-budget": {
  "id": "performance-budget",
  "title": "Performance budget",
  "description": "Keep the quantity and size of network requests under the targets set by the provided performance budget. [Learn more](https://developers.google.com/web/tools/lighthouse/audits/budgets).",
  "score": null,
  "scoreDisplayMode": "informative",
  "details": {
    "type": "table",
    "headings": [
      {
        "key": "label",
        "itemType": "text",
        "text": "Resource Type"
      },
      {
        "key": "requestCount",
        "itemType": "numeric",
        "text": "Requests"
      },
      {
        "key": "size",
        "itemType": "bytes",
        "text": "Transfer Size"
      },
      {
        "key": "countOverBudget",
        "itemType": "text",
        "text": ""
      },
      {
        "key": "sizeOverBudget",
        "itemType": "bytes",
        "text": "Over Budget"
      }
    ],
    "items": [
      {
        "resourceType": "image",
        "label": "Image",
        "requestCount": 6,
        "size": 172522
      },
      {
        "resourceType": "script",
        "label": "Script",
        "requestCount": 2,
        "size": 446616
      },
      {
        "resourceType": "total",
        "label": "Total",
        "requestCount": 22,
        "size": 755199
      },
      {
        "resourceType": "third-party",
        "label": "Third-party",
        "requestCount": 13,
        "size": 207011,
        "countOverBudget": "3 requests"
      }
    ]
  }
},
```

See the following pages to learn more about performance budgets:

* [Start Performance Budgeting](https://addyosmani.com/blog/performance-budgets/){: .external }
* [Performance budgets 101](https://web.dev/performance-budgets-101/){: .external }
* [Your first performance budget](https://web.dev/your-first-performance-budget/){: .external }
* [Incorporate performance budgets into your build process](https://web.dev/incorporate-performance-budgets-into-your-build-tools/){: .external }

## Setup {: #setup }

Follow the instructions below to configure Lighthouse to report when you're over budget. As mentioned
before, this is only possible when running Lighthouse from the command line. The **Keep Request Counts Low
And File Sizes Small** audit is always available in all Lighthouse runtime environments and requires no configuration.

### Define a budget file {: #define }

By convention the budget file is usually called `budget.json` but you can call it whatever you like.
The example below sets the following budgets:

* Overall transfer size of JavaScript resources on the page: 300 kilobytes
* Overall transfer size of third-party resources on the page: 200 kilobytes
* Total number of requests made: 50 requests
* Time to Interactive: 5000ms
* First Meaningful Paint: 2000ms

```json
[
  {
    "resourceSizes": [
      {
        "resourceType": "script",
        "budget": 300
      },
      {
        "resourceType": "third-party",
        "budget": 200
      }
    ],
      {
        "resourceType": "total",
        "budget": 50
      }
    ],
    "timings": [
      {
        "metric": "interactive",
        "budget": 5000
      },
      {
        "metric": "first-meaningful-paint",
        "budget": 2000
      }
    ]
  }
]
```
The table below describes each of the `budget.json` properties.

For a complete list of supported performance metrics and resource types, refer to the [Performance Budgets](https://github.com/GoogleChrome/lighthouse/blob/master/docs/performance-budgets.md) section of the Lighthouse docs.

### Pass the budget file as a flag {: #flags }

When running Lighthouse from the command line, pass the `--budget-path` (or `--budgetPath`) flag followed
by the path to your budget file in order to calculate whenever a category is over budget.

    lighthouse https://youtube.com --budget-path=budget.json

Pass the `--output=json` flag followed by `--output-path=report.json` to save your report results as JSON
in the current working directory.

    lighthouse https://youtube.com --budget-path=budget.json --output=json --output-path=report.json

If you were to assign your JSON report results to a variable named `report` you could access your
**Keep Request Counts And File Sizes Small** and **Performance Budgets** data from 
`report.audits['resource-summary']` and `report.audits['performance-budget']`, respectively.

## Recommendations {: #recommendations }

Explore the related audits and guides below to learn how to stay within your budget for each `resourceType`
category.

* `document`
    * [Uses An Excessive DOM Size](/web/tools/lighthouse/audits/dom-size)
    * [Enable Text Compression](/web/tools/lighthouse/audits/text-compression)
* `script`
    * [Reduce JavaScript payloads with code-splitting](https://web.dev/reduce-javascript-payloads-with-code-splitting/)
    * [Remove unused code](https://web.dev/remove-unused-code/)
    * [Minify and compress network payloads](https://web.dev/reduce-network-payloads-using-text-compression/)
    * [Enable Text Compression](/web/tools/lighthouse/audits/text-compression)
    * [JavaScript Bootup Time Is Too High](/web/tools/lighthouse/audits/bootup)
* `stylesheet`
    * [Defer non-critical CSS](https://web.dev/defer-non-critical-css/)
    * [Enable Text Compression](/web/tools/lighthouse/audits/text-compression)
    * [Minify CSS](/web/tools/lighthouse/audits/minify-css)
* `image`
    * [Use Imagemin to compress images](https://web.dev/use-imagemin-to-compress-images/)
    * [Replace animated GIFs with video](https://web.dev/replace-gifs-with-videos/)
    * [Use lazysizes to lazyload images](https://web.dev/use-lazysizes-to-lazyload-images/)
    * [Serve responsive images](https://web.dev/serve-responsive-images/)
    * [Serve images with correct dimensions](https://web.dev/serve-images-with-correct-dimensions/)
    * [Use WebP images](https://web.dev/serve-images-webp/)
    * [Optimize Images](/web/tools/lighthouse/audits/optimize-images)
* `media`
    * [Replace animated GIFs with video](https://web.dev/replace-gifs-with-videos/)

## More information {: #extra }

[src]: https://github.com/GoogleChrome/lighthouse/blob/master/lighthouse-core/audits/performance-budget.js

Sources:

* [Audit source][src]{: .external }

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}
