# Renovate shared presets

This folder contains shared presets.  
It also improve separation of concerns between packageRules in a single file quickly become hard to maintain.  

# How to use a preset

1. Add the preset to your renovate.json file:

```json
{
	//json
  "extends": [
    "github>argoproj/argo-cd//renovate-presets:argoproj"
]
}
```

### Note :

It would make sense to move this folder to a new repository in the future.  

Benefits:  
- Improved feedback loop for configuration changes, no need to wait for a PR to be merged into master.
- Avoid polluting git history.  
- Avoids consuming the repository's CI/CD resources.
- The `renovate.json` in each repository can be simplified to only include a single presets :
	```json
	{
	  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
	  "extends": [
	    "github>argoproj/renovate-presets//argoproj/renovate.json5"
	  ],
	  // rules are empty and this file won't need to be modified again.
	  "packageRules": []
	}
	```
Inconvenient:  
- Owners of a repository can impact the configuration of all repositories. Use codeowners to prevent this.

Example of repo structure :  
```shell
.
├── README.md
├── .github/CODEOWNERS
├── common.json5       # common presets for all repositories
├── fix/
│   └── openssf-merge-confidence-columns.json5
├── custom-managers/
│   ├── bash.json5
│   └── yaml.json5
└── argoproj/ # organization
    ├── argo-cd/ # repository
        ├── devtools.json5 # rules specific to the devtool (CI and dev environment...)
        ├── doc.json5 # rules specific to the doc written with mkdoc.
        ├── # etc...
        └── renovate.json5 # A single preset referenced from the repository argopro/argo-cd.
    ├── argo-rollouts/ # repository
    │    ── renovate.json5




```
