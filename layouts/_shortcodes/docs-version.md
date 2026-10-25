{{- $page := .Page -}}
{{- $productID := $page.Params.product -}}
{{- $product := index hugo.Data.docs_build_versions.products $productID -}}
{{- if $product -}}
{{- $label := $page.Params.version_label | default $page.Params.version -}}
Documentation version: {{ $product.name }} {{ $label }}{{ if eq $page.Params.version "dev" }} (unreleased development snapshot){{ else if $page.Params.version_archive }} (archived release; not the current default){{ else }} (current default release){{ end }}.
{{- end -}}
