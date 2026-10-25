![{{ .Get "alt" }}]({{ .Get "src" | relURL }})
{{ with .Get "caption" }}{{ . }}
{{ end -}}
