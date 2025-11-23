+++
title = "{{ replace .Name "-" " " | title }}"
date = {{ .Date }}
lastmod = {{ .Date }}
draft = true
type = "source"
# Status: processing (reading/watching), done (notes extracted)
status = "processing"
# Source Metadata
author = ""
source_url = ""
tags = ["source"]
summary = "Source material"
+++

## Metadata
- **Author**: {{ .Params.author }}
- **URL**: {{ .Params.source_url }}
- **Type**: {{ .Params.type }}

## Summary
> Short summary of what this source is about.

## Notes
- 

