# indx-library examples

Prefer the CLI for local work. Use `--format compact` for chained agent steps.

## Bootstrap

```bash
indx stats --format compact
indx discovery searchable-fields --format compact
indx discovery canonical-values --format compact
```

## Search files

```bash
indx files search --body '{"search":"sunset","limit":20}' --format compact
indx files search --body '{"tags":["favorite"],"tag_mode":"and","rating":["5"],"limit":20}' --format compact
indx files searchable-fields query --type canonical --key model --query "flux" --format compact
```

## Read file metadata

```bash
indx files metadata get --path "/library/example.png" --view basic --format compact
indx files metadata get --path "/library/example.png" --view full
```

## Update file metadata

```bash
indx files metadata update --body '{"paths":["/library/example.png"],"rating":4}'
indx files metadata update --body '{"paths":["/library/example.png"],"tags_add":["favorite","cover"]}'
indx files metadata update --body '{"paths":["/library/example.png"],"tags_remove":["draft"]}'
indx files metadata update --body '{"paths":["/library/example.png"],"tags_set":["final","portfolio"]}'
indx files metadata update --body '{"paths":["/library/example.png"],"custom":{"Workflow":{"value":"txt2img","type":"text"}}}'
```

## Lists

```bash
indx lists list --format compact
indx lists create --name "Portfolio"
indx lists add-files --name "Portfolio" --paths '["/library/example.png"]'
indx lists files --name "Portfolio" --format compact
indx lists reorder --name "Portfolio" --path "/library/example.png" --position 0
```

## Image sequences

```bash
indx image-sequences list --format compact
indx image-sequences create --name "Turntable" --paths '["/library/a.png","/library/b.png"]'
indx image-sequences metadata get --name "Turntable" --format compact
indx image-sequences metadata update --name "Turntable" --metadata '{"rating":5,"tags":["approved"]}'
indx image-sequences add-to-list --sequence-name "Turntable" --list-name "Portfolio"
```

## HTTP API equivalents

- The CLI mirrors the HTTP API route families under `/v1`.
- Use the CLI locally unless you specifically need remote transport or an embedded integration.
