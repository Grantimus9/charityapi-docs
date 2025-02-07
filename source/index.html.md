---
title: CharityAPI.org API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  # - ruby

toc_footers:
  - <a href='https://www.charityapi.org'>Sign Up for an API Key</a>
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

includes:
  - errors
  - contact

search: true

code_clipboard: true

meta:
  - name: description
    content: Documentation for the CharityAPI API
---

# Introduction

Welcome to the CharityAPI.org API Reference!

Here you'll find the full reference for the CharityAPI.org API, including authentication, error handling, and
links to more information where interesting. If you have any questions or requests for API support, please feel free to [reach out](https://www.charityapi.org/contact).

Base URL: `https://api.charityapi.org`

# Authentication

```shell
# With shell, you can just pass the correct header with each request
curl "https://api.charityapi.org/api/organizations/search/food" \
  -H "apikey: keyhere"
```

> Make sure to replace `keyhere` with your API key.

<aside class="notice">
CharityAPI.org expects all API requests to be authenticated.
</aside>

Every call should include a header that looks like the following:

HTTP Header | Value
---------- | -------
apikey | Your API Key

When you [register](https://api.charityapi.org/signup) you will receive 2 API Keys; one live (prefixed with `live-`) and one test (prefixed with `test-`). Calls with the test key will return validly-shaped but bogus data; you can use it in development but we don't recommend using it for your test suite or CI. For your test suite, please mock or stub out the responses from this API.

Examples:

Type | Example Value
---------- | -------
Live | live-PKDv6IZSgxXEJzGnQLIdDsObIpr1PpA5NQd0VbKh2JZaaTkxsH1X5eRcbsyEiOVhPWOOYVoD3zTrXVyO
Test | test-aaRkxsH1X5eRcbsyEiOVhPWOOYVoD3zTrXVyOPKDv6IZSgxXEJzGnQLIdDsObIpr0PpA5NQz0VbQh2JZ

<aside class="notice">
These are not real keys.
</aside>

# Organizations

## List Nonprofits

```shell
curl "https://api.charityapi.org/api/organizations?since=2025-01-01" \
  -H "apikey: apikeyhere"
```

> Successful response is a list of organizations

```json
{
  "data": [{
     "zip": "20006-1631",
     "tax_period": 201712,
     "subsection": 3,
     "street": "1629 K ST NW STE 300",
     "status": 1,
     "state": "DC",
     "sort_name": null,
     "ruling": 201602,
     "revenue_amt": 81553,
     "pf_filing_req_cd": 0,
     "organization": 1,
     "ntee_cd": "K30",
     "name": "MEANS DATABASE INC",
     "income_cd": 3,
     "income_amt": 81553,
     "ico": "% MARIA ROSE BELDING",
     "group": 0,
     "foundation": 15,
     "filing_req_cd": 1,
     "ein": "474262060",
     "deductibility": 1,
     "classification": 1000,
     "city": "WASHINGTON",
     "asset_cd": 3,
     "asset_amt": 31245,
     "affiliation": 3,
     "activity": 0,
     "acct_pd": 12
  }, 
  {
     "zip": "20006-1631",
     "tax_period": 201712,
     "subsection": 3,
     "street": "123 Main Street",
     "status": 1,
     "state": "DC",
     "sort_name": null,
     "ruling": 201602,
     "revenue_amt": 81553,
     "pf_filing_req_cd": 0,
     "organization": 1,
     "ntee_cd": "K30",
     "name": "Fake Organization",
     "income_cd": 3,
     "income_amt": 81553,
     "ico": "% Executive Director Name",
     "group": 0,
     "foundation": 15,
     "filing_req_cd": 1,
     "ein": "123456789",
     "deductibility": 1,
     "classification": 1000,
     "city": "WASHINGTON",
     "asset_cd": 3,
     "asset_amt": 31245,
     "affiliation": 3,
     "activity": 0,
     "acct_pd": 12
  }
  ]
}  
```

> No results returns an empty list

```json
{
    "data": []
}
```

This endpoint permits listing organizations. The parameter "since" is required. 

### HTTP Request

`GET https://api.charityapi.org/api/organizations?since=YYYY-MM-DD`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
since | N/A | Required. You can query for all organizations that have been added to the IRS Publication 78 Business Master File since a given date by providing the date in the "since" query param. The parameter must be an iso8601 formatted date in the format YYYY-MM-DD. For example, use "2025-01-30" to query all organizations added after January 30th, 2025. Because the IRS updates date only once a month, we recommend only querying for new updates once a week and a lookback period of a month to ensure your database receives all incremental additions. Each new monthly publication includes approximately 7,000 new organizations, but each month varies. 

## Get Nonprofit By EIN

```shell
curl "https://api.charityapi.org/api/organizations/:ein" \
  -H "apikey: apikeyhere"
```

> If the EIN / tax ID number matches a nonprofit that's currently listed in the IRS database, it will be returned:

```json
{
  "data": {
     "zip": "20006-1631",
     "tax_period": 201712,
     "subsection": 3,
     "street": "1629 K ST NW STE 300",
     "status": 1,
     "state": "DC",
     "sort_name": null,
     "ruling": 201602,
     "revenue_amt": 81553,
     "pf_filing_req_cd": 0,
     "organization": 1,
     "ntee_cd": "K30",
     "name": "MEANS DATABASE INC",
     "income_cd": 3,
     "income_amt": 81553,
     "ico": "% MARIA ROSE BELDING",
     "group": 0,
     "foundation": 15,
     "filing_req_cd": 1,
     "ein": "474262060",
     "deductibility": 1,
     "classification": 1000,
     "city": "WASHINGTON",
     "asset_cd": 3,
     "asset_amt": 31245,
     "affiliation": 3,
     "activity": 0,
     "acct_pd": 12
  }
}  
```

> If no current nonprofit is found:

```json
{
    "data": null
}
```

This endpoint retrieves a single nonprofit entry from the currently listed nonprofits in the IRS's Business Master File, indicating they are a nonprofit. This will return both charities and other nonprofits, like political organizations and insurance organizations.

### HTTP Request

`GET https://api.charityapi.org/api/organizations/:ein`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
ein | N/A | The Tax ID number / Employer Identification Number


## Organization Schema

```shell
curl "https://api.charityapi.org/api/organizations/schema" \
  -H "apikey: apikeyhere"
```

> Always returns

```json
{
    "acct_pd": "Accounting Period.\n        This designates the accounting period or fiscal year ending date (Jan - Dec) of the organization (MM).",
    "activity": "Description Not Available",
    "affiliation": "Organization Grouping, based on the following split:\n        1: Central - This code is used if the organization is a central type organization (no group exemption) of a National,\n        Regional or Geographic grouping of organizations.\n        2: Intermediate - This code is used if the organization is an intermediate organization (no group exemption) of a\n        National, Regional or Geographic grouping of organizations (such as a state headquarters of a national\n        organization).\n        3: Independent - This code is used if the organization is an independent organization or an independent auxiliary\n        (i.e., not affiliated with a National, Regional, or Geographic grouping of organizations).\n        6: Central - This code is used if the organization is a parent (group ruling) and is not a church or 501(c)(1)\n        organization.\n        7: Intermediate - This code is used if the organization is a group exemption intermediate organization of a National,\n        Regional or Geographic grouping of organizations.\n        8: Central - This code is used if the organization is a parent (group ruling) and is a church or 501(c)(1) organization.\n        9: Subordinate - This code is used if the organization is a subordinate in a group ruling.",
    "asset_amt": "Asset Amount.\n        Asset Amount is an amount from the most recent Form 990 series return filed by the organization. Asset Amount is the\n        Book Value Total Assets End of Year - PART X Balance Sheet Line 16 Col. (B) shown on the Form 990. This field is also\n        from PART II, Line 25, Col. (B) EOY on Form 990EZ and PART II, Line 16, Col. (b) on Form 990PF. This field is dollars\n        only.",
    "asset_cd": "Asset Codes relate to the book value amount of assets shown on the most recent Form 990 series return filed by the\n        organization.\n        ASSET CODE/INCOME CODE TABLE\n        Code Description($)\n        0 0\n        1 1 to 9,999\n        2 10,000 to 24,999\n        3 25,000 to 99,999\n        4 100,000 to 499,999\n        5 500,000 to 999,999\n        6 1,000,000 to 4,999,999\n        7 5,000,000 to 9,999,999\n        8 10,000,000 to 49,999,999\n        9 50,000,000 to greater",
    "city": "city",
    "classification": "Subsection Codes are the codes shown under section 501(c) of the Internal Revenue Code of 1986, which define the\n        category under which an organization may be exempt. A table of subsection and classification codes (which reflects a\n        further breakdown of the Internal Revenue Code subsections) can be found in the section entitled 'Table of EO Subsection\n        and Classification Codes' (https://www.irs.gov/pub/irs-soi/eo_info.pdf). One to four different classification codes may be present.",
    "deductibility": "Deductibility Code signifies whether contributions made to an organization are deductible.\n        1: Contributions are deductible.\n        2: Contributions are not deductible.\n        4: Contributions are deductible by treaty (foreign organizations).",
    "ein": "Employer Identification Number, or Tax ID Number",
    "filing_req_cd": "This indicates the primary return(s) the organization is required to file.\n        01 990 (all other) or 990EZ return\n        02 990 - Required to file Form 990-N - Income less than $25,000 per year\n        03 990 - Group return\n        04 990 - Required to file Form 990-BL, Black Lung Trusts\n        06 990 - Not required to file (church)\n        07 990 - Government 501(c)(1)\n        13 990 - Not required to file (religious organization)\n        14 990 - Not required to file (instrumentalities of states or political subdivisions)\n        00 990 - Not required to file (all other)",
    "foundation": "Foundation Code.\n        00 All organizations except 501(c)(3)\n        02 Private operating foundation exempt from paying excise taxes on investment income\n        03 Private operating foundation (other)\n        04 Private non-operating foundation\n        09 Suspense\n        10 Church 170(b)(1)(A)(i)\n        11 School 170(b)(1)(A)(ii)\n        12 Hospital or medical research organization 170(b)(1)(A)(iii)\n        13 Organization which operates for benefit of college or university and is owned or operated by a governmental\n        unit 170(b)(1)(A)(iv)\n        14 Governmental unit 170(b)(1)(A)(v)\n        15 Organization which receives a substantial part of its support from a governmental unit or the general public\n        170(b)(1)(A)(vi)\n        16 Organization that normally receives no more than one-third of its support from gross investment income and\n        unrelated business income and at the same time more than one-third of its support from\n        contributions, fees, and gross receipts related to exempt purposes. 509(a)(2)\n        17 Organizations operated solely for the benefit of and in conjunction with organizations described in 10 through\n        16 above. 509(a)(3)\n        18 Organization organized and operated to test for public safety 509(a)(4)\n        21 509(a)(3) Type I\n        22 509(a)(3) Type II\n        23 509(a)(3) Type III functionally integrated\n        24 509(a)(3) Type III not functionally integrated",
    "group": "This is a four-digit IRS-internal number assigned to central/parent organizations holding group exemption letters.",
    "ico": "In Care Of - the person to whom correspondence should be addressed.",
    "income_amt": "Income Amount is a computer generated amount from the most recent Form 990 series return filed by the organization.\n        Income Amount is computer generated using PART I, Total Revenue Line 12 and adding 'back in' the expense items, i.e.\n        Line 6b (Rental Expenses) shown on the Form 990 return. On Form 990EZ it is generated using PART I, Line 9 and\n        adding 'back in' the expense items, i.e. Line 5b (Cost or Other Basis Expenses). Income Amount for Form 990PF is\n        generated using Part I, Line 10b (Cost of Goods) and adding Part I, Line 12, Col. (A) (Total Revenue Col. A) and Part IV,\n        Line 1, Col. (G) (Cost or Other Basis). This field is dollars only.",
    "income_cd": "Income Codes relate to the amount of income shown on the most recent Form 990 series return filed by the organization.\n        ASSET CODE/INCOME CODE TABLE\n        Code Description($)\n        0 0\n        1 1 to 9,999\n        2 10,000 to 24,999\n        3 25,000 to 99,999\n        4 100,000 to 499,999\n        5 500,000 to 999,999\n        6 1,000,000 to 4,999,999\n        7 5,000,000 to 9,999,999\n        8 10,000,000 to 49,999,999\n        9 50,000,000 to greater",
    "name": "The name of the organization",
    "ntee_cd": "National Taxonomy of Exempt Entities Code.",
    "organization": "  This defines the type of organization as follows:\n        Code Description\n        1 Corporation\n        2 Trust\n        3 Co-operative\n        4 Partnership\n        5 Association",
    "pf_filing_req_cd": "1 990-PF return\n        0 No 990-PF return",
    "revenue_amt": "Amount from Form 990, Part 1, Line 12, or Part 1, Line 9, of Form 990EZ.",
    "ruling": "This is the month and year (YYYYMM) on a ruling or determination letter recognizing the organization's exempt status.",
    "sort_name": "Sort Name Line is another name under which the organization does business. Also used for trade names, chapter names,\n        or local numbers for subordinate organizations of group rulings. Check this field in addition to the name field when confirming identity.",
    "state": "state",
    "status": "The EO Status Code defines the type of exemption held by the organization. The following is a list of EO status codes and\n        their definitions included in these files:\n         Code Description\n        01 Unconditional Exemption\n        02 Conditional Exemption\n        12 Trust described in section 4947(a)(2) of the IR Code\n        25 Organization terminating its private foundation status under section 507(b)(1)(B) of the Code",
    "street": "Street Address Line 1",
    "subsection": "Subsection Codes are the codes shown under section 501(c) of the Internal Revenue Code of 1986, which define the category under which an organization may be exempt. A table of subsection and classification codes (which reflects a further breakdown of the Internal Revenue Code subsections) can be found in the section entitled 'Table of EO Subsection and Classification Codes' (https://www.irs.gov/pub/irs-soi/eo_info.pdf). One to four different classification codes may be present.",
    "tax_period": "This is the tax period of the latest return filed (YYYYMM).",
    "zip": "zip code"
}

```

The special `/api/organizations/schema` endpoint returns a minimal data dictionary, or field explanation for the organization attributes. This endpoint is designed to make it a little easier to integrate and understand the data elements as a developer, but is not suitable for displaying information to users in production. This data is straight from the [IRS explanation](https://www.irs.gov/pub/irs-soi/eo_info.pdf).

### HTTP Request

`GET https://api.charityapi.org/api/organizations/schema`



# Public Charity Check

## Charity Check API

```shell
curl "https://api.charityapi.org/api/public_charity_check/:ein" \
  -H "apikey: apikeyhere"
```

> Organization is a charity:

```json
{
    "data": {
        "public_charity": true,
        "ein": "474262060"
    }
}
```

> Organization is not a charity:

```json
{
    "data": {
        "ein": "1",
        "public_charity": false
    }
}
```

This public charity check makes it simple to confirm whether or not a tax ID number is a real charity and tax deductible. Send the API the tax ID number and it will respond with a simple true/false indicating that organization's current status.

[Blog Post Introducing the simple Charity Check API](https://www.charityapi.org/post/charity-data-api)

### HTTP Request

`GET https://api.charityapi.org/api/public_charity_check/:ein`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
ein | N/A | The Tax ID number / Employer Identification Number


# Search Charities

## Search Nonprofits and Charities

```shell
curl "https://api.charityapi.org/api/organizations/search/:term" \
  -H "apikey: apikeyhere"
```

> Search results are a list of organizations:

```json
{
    "data": [
        {
         "zip": "20006-1631",
         "tax_period": 201712,
         "subsection": 3,
         "street": "1629 K ST NW STE 300",
         "status": 1,
         "state": "DC",
         "sort_name": null,
         "ruling": 201602,
         "revenue_amt": 81553,
         "pf_filing_req_cd": 0,
         "organization": 1,
         "ntee_cd": "K30",
         "name": "MEANS DATABASE INC",
         "income_cd": 3,
         "income_amt": 81553,
         "ico": "% MARIA ROSE BELDING",
         "group": 0,
         "foundation": 15,
         "filing_req_cd": 1,
         "ein": "474262060",
         "deductibility": 1,
         "classification": 1000,
         "city": "WASHINGTON",
         "asset_cd": 3,
         "asset_amt": 31245,
         "affiliation": 3,
         "activity": 0,
         "acct_pd": 12
       },
       {},
       {}
    ]
}
```

> No results returns an empty list:

```json
{
    "data": []
}
```

Search the ~ 2 million public charities by search term. This endpoint requires at least 3 characters to return results, and will return the top results ordered by relevance and charity size. 

This searches the nonprofit "name" and "sort_name" fields. Organizations whose charitable status has lapsed will not appear in search results.

You may optionally filter results further by providing "city" and "state" parameters. 

### HTTP Request

`GET https://api.charityapi.org/api/organizations/search/:term`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
term | N/A | Search term. URL-encoded.
city | null | Filter results to only this city. This field is mildly typo tolerant, and requires at least two characters be provided. 
state | null | Filter results to only this state code, e.g. "AZ" or "IA". Use "PR" for Puerto Rico.


## Autocomplete

```shell
curl "https://api.charityapi.org/api/organizations/autocomplete/:term" \
  -H "apikey: apikeyhere"
```

> The response is a list of organization names and EINs.

```json
{
    "data": [
        {
         "name": "MEANS DATABASE INC",
         "ein": "474262060"
       },
       {
        "name": "MEANS OF GRACE",
        "ein": "825057778"
       },
       {}
    ]
}
```

> No results returns an empty list:

```json
{
    "data": []
}
```
<aside class="notice">
The autocomplete endpoint is only available to paying accounts.
</aside>

The autocomplete endpoint is designed to power your autocomplete (typeahead) features. This endpoint searches only the name field of all organizations, and is typo-tolerant, making it perfect for enabling your users to search for a charity by name.

You must provide at least 3 characters before results will be returned.

If you require more information about a charity, use the organizations endpoint: `GET https://api.charityapi.org/api/organizations/:ein`


### HTTP Request

`GET https://api.charityapi.org/api/organizations/autocomplete/:term`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
term | N/A | Search term. URL-encoded.


# Data Imports

## Get Recent Data Imports 

```shell
curl "https://api.charityapi.org/api/dataimports/recent" \
  -H "apikey: apikeyhere"
```
> Returns a list of dataimports. 

```json
{
    "data": {
        "in_progress_dataimport": null,
        "most_recent_complete_dataimport": {
            "completed": true,
            "crawled_urls": [
                "https://www.irs.gov/pub/irs-soi/eo1.csv",
                "https://www.irs.gov/pub/irs-soi/eo2.csv",
                "https://www.irs.gov/pub/irs-soi/eo3.csv",
                "https://www.irs.gov/pub/irs-soi/eo4.csv"
            ],
            "id": 357,
            "inserted_at": "2022-10-17T04:00:00",
            "name": "Data Import 2022-10-17",
            "s3_path": "redacted",
            "s3_urls": "redacted",
            "to_crawl_urls": [
                "https://www.irs.gov/pub/irs-soi/eo1.csv",
                "https://www.irs.gov/pub/irs-soi/eo2.csv",
                "https://www.irs.gov/pub/irs-soi/eo3.csv",
                "https://www.irs.gov/pub/irs-soi/eo4.csv"
            ],
            "updated_at": "2022-10-17T07:51:42"
        },
        "most_recent_dataimport": {
            "completed": true,
            "crawled_urls": [
                "https://www.irs.gov/pub/irs-soi/eo1.csv",
                "https://www.irs.gov/pub/irs-soi/eo2.csv",
                "https://www.irs.gov/pub/irs-soi/eo3.csv",
                "https://www.irs.gov/pub/irs-soi/eo4.csv"
            ],
            "id": 357,
            "inserted_at": "2022-10-17T04:00:00",
            "name": "Data Import 2022-10-17",
            "s3_path": "redacted",
            "s3_urls": "redacted",
            "to_crawl_urls": [
                "https://www.irs.gov/pub/irs-soi/eo1.csv",
                "https://www.irs.gov/pub/irs-soi/eo2.csv",
                "https://www.irs.gov/pub/irs-soi/eo3.csv",
                "https://www.irs.gov/pub/irs-soi/eo4.csv"
            ],
            "updated_at": "2022-10-17T07:51:42"
        }
    }
}
```

You can also retrieve the most recent dataimports by providing the string "recent" instead of a dataimport ID. This will return an object with 3 keys: 

Key | Description | Notes
--------- | ------- | -----------
in_progress_dataimport | The data import currently in progress, if any | null if none in progress
most_recent_complete_dataimport | The most recent data import that's complete | N/A
most_recent_dataimport | The most recent data import regardless of status | N/A

The same data import may appear in multiple keys; for example if it is both the most recent and complete data import.

### HTTP Request

`GET https://api.charityapi.org/api/dataimports/recent`

## Get Data Import By ID 

```shell
curl "https://api.charityapi.org/api/dataimports/:id" \
  -H "apikey: apikeyhere"
```

> Returns a single Data Import. 

```json
{
    "data": {
        "completed": true,
        "crawled_urls": [
            "https://www.irs.gov/pub/irs-soi/eo1.csv",
            "https://www.irs.gov/pub/irs-soi/eo2.csv",
            "https://www.irs.gov/pub/irs-soi/eo3.csv",
            "https://www.irs.gov/pub/irs-soi/eo4.csv"
        ],
        "id": 355,
        "inserted_at": "2022-09-19T00:00:00",
        "name": "Data Import 2022-09-19",
        "s3_path": "redacted",
        "s3_urls": "redacted",
        "to_crawl_urls": [
            "https://www.irs.gov/pub/irs-soi/eo1.csv",
            "https://www.irs.gov/pub/irs-soi/eo2.csv",
            "https://www.irs.gov/pub/irs-soi/eo3.csv",
            "https://www.irs.gov/pub/irs-soi/eo4.csv"
        ],
        "updated_at": "2022-09-19T02:16:04"
    }
}
```

> If a dataimport is currently in progress, it will indicate progress. Active imports do not disrupt the API's availability. 

```json
{
    "data": {
        "completed": false,
        "crawled_urls": [
            "https://www.irs.gov/pub/irs-soi/eo1.csv",
            "https://www.irs.gov/pub/irs-soi/eo2.csv"
        ],
        "id": 600,
        "inserted_at": "2023-09-19T00:00:00",
        "name": "Data Import 2023-09-19",
        "s3_path": "redacted",
        "s3_urls": "redacted",
        "to_crawl_urls": [
            "https://www.irs.gov/pub/irs-soi/eo1.csv",
            "https://www.irs.gov/pub/irs-soi/eo2.csv",
            "https://www.irs.gov/pub/irs-soi/eo3.csv",
            "https://www.irs.gov/pub/irs-soi/eo4.csv"
        ],
        "updated_at": "2023-09-19T02:16:04"
    }
}
```

> No results returns null:

```json
{
    "data": null
}
```

Retrieve a dataimport by its ID. 

### HTTP Request

`GET https://api.charityapi.org/api/dataimports/:id`


# NTEE Codes 

## List NTEE Codes 

```shell
curl "https://api.charityapi.org/api/ntee_codes" \
  -H "apikey: apikeyhere"
```

> Returns all NTEE Codes in a list.

```json 
{
    "data": [
        {
            "code": "X900",
            "definition": null,
            "title": null
        },
        {
            "code": "U99",
            "definition": "Use this code for organizations that clearly provide science and technology research services where the major purpose is unclear enough that a more specific code cannot be accurately assigned.",
            "title": "Science & Technology N.E.C."
        },
        {
            "code": "V01",
            "definition": "Organizations whose activities focus on influencing public policy within the Social Science Research Institutes, Services major group area. Includes a variety of activities from public education and influencing public opinion to lobbying national and state legislatures.",
            "title": "Alliances & Advocacy"
        }
    ]
}

```

Lists all NTEE Codes observed in IRS data. Many codes do not have corresponding metadata. There are many NTEE codes, making this a particularly large json response. 

### HTTP Request

`GET https://api.charityapi.org/api/ntee_codes`


## Get NTEE category By Code 

```shell
curl "https://api.charityapi.org/api/ntee_codes/:code" \
  -H "apikey: apikeyhere"
```

> Sample response for https://api.charityapi.org/api/ntee_codes/U99 

```json 
{
    "data": {
        "code": "U99",
        "definition": "Use this code for organizations that clearly provide science and technology research services where the major purpose is unclear enough that a more specific code cannot be accurately assigned.",
        "title": "Science & Technology N.E.C."
    }
}

```

> Returns null if there is no matching code 

```json 
{
    "data": null
}
```

The National Taxonomy of Exempt Entities (NTEE) is a taxonomy designed to classify all nonprofit entities by activity type. 

Nonprofits choose the most appropriate code for their activities, and the IRS provides this code in the publication 78 data (tax exempt entities). 

For each organization, you will see a `ntee_cd` NTEE code field that you can then use to lookup the NTEE code for the organization. 

### HTTP Request

`GET https://api.charityapi.org/api/ntee_codes/:code`

