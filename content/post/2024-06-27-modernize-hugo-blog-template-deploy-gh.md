+++
title = "Modernize Hugo blog theme + Github Actions"
date = "2024-06-27"
draft = false
tags = ['hugo', 'github']
+++

Time has come to move out of Hugo 0.34 to something never (0.128.0). Hugo is expanding quickly and unfortunately in backward incompatible way. Plus this site is already since some time on Github Actions not Wercker..

<!--more-->

Main differences/troubles I had when modernizing:
* Hugo looks for *baseof.html* file as main index in layouts/default/ dir
* It is not *{{ with .Site.Params.googleAnalytics }}* but *{{ with site.Params.googleAnalytics }}*
* Paginator looks differently, and needs to be initialized before using:
```
// Old way
{{ range ( .Paginate (where .Data.Pages "Type" "post")).Pages }}
    {{ .Render "summary"}}
{{ end }}

// New way
{{ $paginator := .Paginate (where .Site.RegularPages "Section" "post") 10 }}
{{ range $paginator.Pages }}
    {{ .Render "summary"}}
{{ end }}
```

* [Link to git commit diff for modernizing][git-diff]

[git-diff]: https://github.com/pbedn/pbedn.github.io/commit/9e5121d7d170aae235a2ed8bfa55b1283d6c4eed

-- 

Below is link to CI actions template for this blog. It works nice and is pretty fast (seconds).

* [Link to Github Actions workflow][gh-ac-wf]

[gh-ac-wf]: https://github.com/pbedn/pbedn.github.io/blob/master/.github/workflows/actions.yml
