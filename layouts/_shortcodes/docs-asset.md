{{- $id := .Get "product" -}}
{{- $source := .Get "source" -}}
{{- $product := index hugo.Data.docs_build_versions.products $id -}}
{{- $version := .Get "version" | default $product.default -}}
{{- $release := index (where $product.versions "slug" $version) 0 -}}
{{- if not $release }}{{ errorf "docs-asset: unknown %s version %s" $id $version }}{{ end -}}
{{- $path := printf "%s/%s/_assets/%s" $id $version $source -}}
![{{ .Get "alt" }}]({{ $path | relURL }})

{{ with .Get "caption" }}{{ . }} {{ end }}Source: [{{ $product.name }} {{ $release.label }}]({{ $product.repo }}/blob/{{ $release.sha }}/{{ $source }})
