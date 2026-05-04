# indx-admin examples

Prefer the CLI for local work. Use `--format compact` for chained agent steps and job polling.

## Watched directories

```bash
indx watched-directories list --format compact
indx watched-directories add --path "/library/output" --options '{"name":"Main Output","includeSubdirs":true}'
indx watched-directories update --id 3 --updates '{"enabled":true,"rescanMetadataOnStartup":false}'
indx watched-directories reorder --orderings '[{"id":3,"sort_order":0}]'
indx watched-directories remove --id 3
```

## Scan flows

```bash
indx scans watched-directory --id 3 --format compact
indx scans watched-directory --id 3 --wait --timeout-ms 120000 --format compact
indx scans all --mode phased --wait --timeout-ms 300000 --format compact
indx scans metadata-rescan --id 3 --wait --timeout-ms 120000 --format compact
```

## Jobs

```bash
indx jobs get job_123 --format compact
indx jobs wait job_123 --timeout-ms 120000 --format compact
```

## Startup scan mode

```bash
indx settings startup-scan-mode get --format compact
indx settings startup-scan-mode set --mode new_missing --format compact
```

## HTTP API equivalents

- The CLI mirrors the HTTP API route families under `/v1`.
- Use the CLI locally unless you specifically need remote transport or an embedded integration.
