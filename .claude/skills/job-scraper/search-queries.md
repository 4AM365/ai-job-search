# Search Queries for Job Scraper

<!-- SETUP: Customize these queries based on your skills, target roles, and location -->

## Search Sites

Primary (US job market):
- **linkedin.com/jobs** - LinkedIn job listings (filter: your city/metro or Remote)
- **indeed.com** - largest US job aggregator
- **glassdoor.com** - listings plus company reviews and salary data
- **ziprecruiter.com** - broad US aggregator
- **Google Jobs** - aggregates postings across boards (search the role + location directly)

Niche / role-dependent (add as relevant):
- **builtin.com** - tech roles by metro (Built In NYC, Built In Austin, etc.)
- **wellfound.com** (formerly AngelList) - startup roles
- **usajobs.gov** - US federal government jobs
- **dice.com** - tech/IT contracting
- **weworkremotely.com**, **remoteok.com** - remote-first listings

Secondary (company career pages via Google):
- Direct Google searches with `site:` filters for known target companies (e.g. `site:careers.<company>.com`)
- Many companies post via Greenhouse/Lever/Ashby: `site:boards.greenhouse.io [YOUR_ROLE]`, `site:jobs.lever.co [YOUR_ROLE]`

## Query Categories

Queries are grouped by priority. Each query should be combined with your location terms (e.g. "[YOUR_CITY], [YOUR_STATE]", the metro name, or "remote") where the site supports it.

### Priority 1: [YOUR_PRIMARY_ROLE_TYPE]

These match your strongest and most desired career direction.

```
site:linkedin.com/jobs "[YOUR_PRIMARY_JOB_TITLE]" [YOUR_CITY]
site:indeed.com "[YOUR_PRIMARY_JOB_TITLE]" [YOUR_CITY] [YOUR_STATE]
"[YOUR_PRIMARY_JOB_TITLE]" "[YOUR_KEY_SKILL]" jobs [YOUR_CITY]
```

### Priority 2: [YOUR_DOMAIN_EXPERTISE]

These match your domain expertise.

```
site:indeed.com [YOUR_DOMAIN_KEYWORD_1] [YOUR_CITY] OR [YOUR_METRO]
site:linkedin.com/jobs [YOUR_DOMAIN_KEYWORD_2] [YOUR_STATE]
site:glassdoor.com [YOUR_DOMAIN_KEYWORD_1] [YOUR_CITY]
```

### Priority 3: [YOUR_ADJACENT_ROLE_TYPE]

Adjacent roles you could pivot into.

```
site:linkedin.com/jobs "[YOUR_ADJACENT_TITLE_1]" [YOUR_KEY_SKILL] [YOUR_CITY]
site:indeed.com "[YOUR_ADJACENT_TITLE_2]" [YOUR_KEY_SKILL] [YOUR_STATE]
```

### Priority 4: Remote / Hybrid

A US-market staple - many strong roles are location-flexible. Run these without a city filter.

```
site:linkedin.com/jobs "[YOUR_PRIMARY_JOB_TITLE]" remote
"[YOUR_PRIMARY_JOB_TITLE]" "[YOUR_KEY_SKILL]" remote jobs
site:weworkremotely.com [YOUR_KEY_SKILL]
site:wellfound.com "[YOUR_PRIMARY_JOB_TITLE]" remote
```

### Priority 5: Broader Technical / Consulting

Wider net for general technical roles.

```
site:indeed.com [YOUR_KEY_SKILL] developer [YOUR_CITY]
site:linkedin.com/jobs "[YOUR_KEY_SKILL] developer" [YOUR_CITY]
"technical consultant" [YOUR_DOMAIN] [YOUR_CITY] jobs
```

## Location Filter

When evaluating results, verify the job location is within reasonable commute distance from your home, or is remote/hybrid-eligible. Define acceptable areas:
- [YOUR_CITY], [YOUR_STATE] and surrounding metro
- [ACCEPTABLE_AREA_1]
- [ACCEPTABLE_AREA_2]
- Remote (US) - [acceptable / preferred / not applicable]
- [BORDERLINE_AREA] (borderline - ~X min by car/transit)
- [TOO_FAR_AREA] (too far)

Note US-specific signals while filtering: **work authorization / visa sponsorship** requirements, **on-site vs. hybrid vs. remote**, and whether **relocation** is expected.

## Date Filter

Only include jobs posted within the last 14 days, or with an application deadline that has not yet passed. If a posting date cannot be determined, include it but flag as "date unknown".

## Adapting Queries

If the user specifies a focus area, select queries from the matching category and also generate 2-3 custom queries for that focus. For example:
- "/scrape [focus_area]" -> relevant category queries + custom focus-specific queries
