{{- $productID := .Get "product" -}}
{{- $sourcePath := .Get "source" -}}
{{- $label := .Get "label" -}}
{{- $product := index hugo.Data.docs_build_versions.products $productID -}}
{{- $version := $product.default -}}
{{- $default := index (where $product.versions "slug" $version) 0 -}}
{{- $pages := where (where (where site.Pages "Params.product" $productID) "Params.version" $version) "Params.source_path" $sourcePath -}}
{{- if $pages -}}
{{- with index $pages 0 -}}
[{{ $label | default .Title }}]({{ partial "llms-page-url.html" . }}) ({{ $product.name }} {{ $default.label }})
{{- end -}}
{{- else -}}
{{- errorf "docs-link: missing %s page for %s default release %s" $sourcePath $productID $version -}}
{{- end -}}
