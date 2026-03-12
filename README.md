A GitHub Action workflow for automating a WARN Act notice ETL pipeline.

## How it works

The [extract, transform and load](https://github.com/biglocalnews/warn-github-flow/actions/workflows/etl.yml) Action runs every few hours. It does the following:

- 🔪 Gather raw WARN Act notices from all of our sources with [warn-scraper](https://github.com/biglocalnews/warn-scraper)
- 🪢 Consolidate the raw files into a single, standardized dataset with [warn-transformer](https://github.com/biglocalnews/warn-transformer)
- ⏫ Upload the files to our archive on [biglocalnews.org](https://biglocalnews.org) with [upload-files](https://github.com/biglocalnews/upload-files)
- 📟 Send Slack and Teams alerts

```mermaid
flowchart TB
    subgraph Extract
    A[Scrape sources] --> B[Commit to source-specific branches]
    B --> C[Upload raw files to biglocalnews.org]
    end
    subgraph Transform
    subgraph Consolidate
    D[Download raw files from biglocalnews.org] --> E[Merge into a single file]
    end
    subgraph Integrate
        F[Reconcile latest data with current database]
        F --> G[Identify any additions and amendments]
    end
    end
    subgraph Load
    H[Commit transformed files to `transformer` branch] --> I[Upload transformed files to biglocalnews.org]
    end
    subgraph Alert
    subgraph Members
    L[Forward new notices via Slack and Teams bots]
    end
    subgraph Administrators
    J[Post status report to Big Local News Slack]
    end
    end
    Extract --> Transform
    Consolidate --> Integrate
    Transform --> Load
    Load --> Alert
```

## About

The project is sponsored by [Big Local News](https://biglocalnews.org/#/about), a program at Stanford University that collects data for impactful journalism. The code is maintained by [Ben Welsh](https://palewi.re/who-is-ben-welsh/), a visiting data journalist from the Los Angeles Times.
