{{- $page := .Page -}}
{{- $paths := $page.Params.related_sourcepaths -}}
{{- if $paths -}}
  {{- $targets := slice -}}
  {{- range $paths -}}
    {{- $matches := where (where (where site.Pages "Params.product" $page.Params.product) "Params.version" $page.Params.version) "Params.source_path" . -}}
    {{- if gt (len $matches) 0 -}}{{- $targets = $targets | append (index $matches 0) -}}{{- end -}}
  {{- end -}}
  {{- if $targets -}}
Related documentation:

{{ range $targets -}}
- [{{ .Title }}]({{ partial "llms-page-url.html" . }})
{{ end -}}
  {{- end -}}
{{- end -}}
