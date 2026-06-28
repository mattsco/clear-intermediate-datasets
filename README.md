# Clear intermediate datasets

A Dataiku DSS plugin that frees up disk space by clearing the data of intermediate datasets in a Flow, while keeping the datasets you actually need.

In a typical Flow, only the source datasets (Flow inputs) and the final results (Flow outputs) usually need to be persisted. Everything in between is rebuildable from the recipes and often eats up significant storage. This plugin identifies those intermediate datasets and clears their data in one click.

## What it does

The plugin adds a project macro that:

- Walks the Flow to detect **Flow inputs** (datasets that are never produced by a recipe) and **Flow outputs** (datasets that are never consumed by a recipe), and keeps them.
- Clears every other dataset, i.e. the intermediate ones produced and consumed within the Flow.
- Optionally preserves partitioned datasets and datasets shared with other projects.
- Returns a table listing each kept dataset and the reason it was kept.

Clearing a dataset only drops its data; the dataset definition and its place in the Flow are untouched, so it can be rebuilt at any time.

## Installation

Install it as a regular DSS plugin: from the **Plugins** store, or by uploading / pointing to this repository in **Plugins > Add plugin > Fetch from Git repository**. No code environment is required.

## Usage

1. Open the project whose Flow you want to clean.
2. Go to the **... menu (top bar) > Macros**, then select **Clear intermediate datasets**.
3. Set the parameters and run.

### Parameters

| Parameter | Default | Description |
|---|---|---|
| **Dry run** | `true` | Only lists the datasets that would be cleared, without deleting any data. Run it once in dry-run mode to review the plan before committing. |
| **Keep partitioned datasets** | `false` | Also keep datasets that use partitioning. |
| **Keep shared datasets** | `false` | Also keep datasets exposed to other projects. |

The result table reports the datasets that are kept, each with a status: `KEEP(INPUT)`, `KEEP(OUTPUT)`, `KEEP(PARTITIONED)` or `KEEP(SHARED)`.

> Start with **Dry run** enabled. Disable it only once the list of kept datasets matches what you expect — clearing data cannot be undone (datasets must be rebuilt).

## How datasets are classified

For each recipe in the project, the macro collects its input and output datasets. From the deduplicated sets:

- A dataset that appears only as an input (never an output) is a **Flow input** → kept.
- A dataset that appears only as an output (never an input) is a **Flow output** → kept.
- Any remaining dataset is intermediate → cleared (unless covered by *Keep partitioned* / *Keep shared*).

## License

Apache Software License.

## Authors

Pierre Pfennig & Harizo Rajaona (Dataiku).
