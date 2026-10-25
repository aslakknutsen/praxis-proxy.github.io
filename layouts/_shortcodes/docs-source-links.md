{{- $page := .Page -}}
{{- $repo := $page.Params.source_repo -}}
{{- $sha := $page.Params.source_commit -}}
{{- $path := $page.Params.source_path -}}
{{- $productID := $page.Params.product -}}
{{- $productName := $productID -}}
{{- with index hugo.Data.docs_build_versions.products $productID -}}{{- $productName = .name -}}{{- end -}}
{{- $versionLabel := $page.Params.version_label | default $page.Params.version -}}
{{- $sourceURL := printf "%s/blob/%s/%s" $repo $sha $path -}}
{{- $issueURL := "" -}}
{{- with $page.Params.issue_url -}}
  {{- $base := . | strings.TrimSuffix "/new" | strings.TrimSuffix "/" -}}
  {{- $title := printf "Docs: %s" $page.Title -}}
  {{- $body := printf "Project: %s\nVersion: %s\nSource: %s" $productName $versionLabel $sourceURL -}}
  {{- $issueURL = printf "%s/new?%s" $base (querify "title" $title "body" $body | safeURL) -}}
{{- end -}}
{{- if and $repo $sha $path -}}
Source: [{{ $path }}]({{ $sourceURL }}){{ with $issueURL }} · [Report an issue]({{ . }}){{ end }}{{ with $page.Params.edit_url }} · [Edit this page]({{ . }}){{ end }} · {{ $productName }} {{ $versionLabel }}
{{- end -}}
