# Monster Job Scraper

Extract structured job listings from [monster.com](https://monster.com) — one of the largest U.S. job boards with over 1 million active listings across 14 countries including the US, UK, Germany, and Canada.

**[Monster Job Scraper on Apify →](https://apify.com/blackfalcondata/monster-scraper)**

---

## Key features

**Search with filters** — Search by keyword and location. Filter by country, employment type (full-time, part-time, contract, internship, temp), and date posted (today, last week, last month).

**Structured salary data** — Extracts salary ranges (min/max/currency/period) directly from Monster's structured data — no regex parsing.

**Remote work detection** — Each listing includes `isRemote` field derived from Monster's job location type (REMOTE / ONSITE / HYBRID).

**Incremental mode** — Only get new or changed listings since your last run. Content hash per listing — no duplicates, no re-processing.

**Multi-country support** — Scrape US, UK, DE, CA, FR, AT, NL, BE, IE, SE, ES, IT, CH, and LU job markets from a single actor.

---

## Use cases

**Data pipeline automation**
Collect structured job listings from monster.com on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size for AI pipelines.

**Market research**
Monitor hiring trends, track salary ranges by role and location, and analyze demand patterns across countries with deduplicated data from Monster's full index.

**Job change monitoring**
Use incremental mode with a stable `stateKey` to track when roles are posted, updated, or expire — ideal for recruitment intelligence and competitive analysis.

---

## Quick start

```json
{
  "query": "software engineer",
  "location": "New York",
  "country": "US",
  "maxResults": 50
}
```

---

## Input parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `query` | string | — | Job search keywords. |
| `country` | enum | `"US"` | Which market to search. |
| `location` | string | — | City, state, or region. |
| `radius` | integer | `30` | Search radius around location in miles. |
| `employmentType` | enum | — | Filter by employment type. |
| `datePosted` | enum | — | Filter by how recently jobs were posted. |
| `maxResults` | integer | `25` | Maximum total results. Up to 1,500 per run. |
| `includeDetails` | boolean | `true` | Fetch full job details. |
| `descriptionMaxLength` | integer | `0` | Truncate description to N chars. 0 = no truncation. |
| `compact` | boolean | `false` | Core fields only (for AI-agent/MCP workflows). |
| `incrementalMode` | boolean | `false` | Compare against previous run state. |
| `stateKey` | string | — | Stable identifier for tracked universe. |

---

## FAQ

**Is it legal to scrape monster.com?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible job listings. Always check the target site's terms of service for your specific use case.

**How does incremental mode work?**
Each listing gets a content hash. On subsequent runs, only new or changed listings are emitted — saving time, compute, and storage. Requires a stable `stateKey` per monitored query.

**Which countries are supported?**
US, UK (monster.co.uk), Germany (monster.de), Canada, France, Austria, Netherlands, Belgium, Ireland, Sweden, Spain, Italy, Switzerland, and Luxembourg.

**What salary data is available?**
Structured salary ranges when Monster provides them — min value, max value, currency, and pay period (hourly, annual, etc.). The `isSalaryExtracted` flag in Monster's API indicates whether salary was extracted from the description or provided directly.

**Does it support remote job filtering?**
Yes. Set `employmentType: "REMOTE"` to filter for remote listings. Additionally, the `isRemote` output field reflects Monster's own classification (REMOTE/ONSITE/HYBRID).

**Can I monitor a specific employer?**
Use keyword search with the employer name (e.g. `query: "Google"`) combined with incremental mode to track new job postings from a specific company over time.

---

## Known limitations

- Maximum 1,500 results per run. Monster.com's API limits pagination depth beyond this point.
- Monster.com's estimated total result count (`estimatedTotalSize`) is approximate — actual results may differ slightly from the reported total.
- Salary data is only available when Monster provides it; many listings omit salary information entirely.
- The `skills` field is not available as structured data in Monster's API — occupational categories are broad SOC codes, not skill tags.
- Employment type filtering may return fewer results than expected if Monster's index for that type is sparse for the given query.

---

## Related products by Black Falcon Data

- [Indeed Job Scraper](https://github.com/BlackFalconData-org/indeed-job-scraper) — Indeed job listings with salary data
- [Glassdoor Job Scraper](https://github.com/BlackFalconData-org/glassdoor-job-scraper) — Glassdoor listings with company ratings
- [Reed Scraper](https://github.com/BlackFalconData-org/reed-scraper) — UK job listings from reed.co.uk
- [Dice.com Job Scraper](https://github.com/BlackFalconData-org/dice-com-job-scraper) — US tech job listings

---

*Last updated: 2026-04*
