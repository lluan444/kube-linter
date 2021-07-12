
## Laura's Notes on Add a New Template and Its Checks##

The base approach is to pick an merged and use it as an example to learn how to develop a template and the checks. See the following two PR examples.

https://github.com/stackrox/kube-linter/pull/191

https://github.com/stackrox/kube-linter/pull/188

### Steps ###

0. create a data struct under `objectkinds/` for the resource kind of your template, e.g. clusterrolebinding.go, if not exists already
1. create a new directory under `templates/`, the directory name is the package name for the new template, e.g. `templates/clusterrolebinding/` 
2. implement `template.go` under `templates/clusterrolebinding/`
3. create `internal/params/params.go` for parameters under `templates/clusterrolebinding/`
4. run auto-generate to genetare internal/params/gen-params.go: 
	`% make go-generated-srcs`
5. add the newly creates template in `templates/all/all.go`
6. for development and testing, create a check yaml file under `builtinchecks/yamls`, and add the check to the list in `internal/defaultchecks/default_checks.go`
7. for release, remove the new check from `internal/defaultchecks/default_checks.go`. Add the check to `config.json` file and use —config option to run `kibe-linter lint`

8. to make a build: `% make build`
9. to run kube-linter: `.gobin/kube-linter lint <k8s-yaml-examples>`
	 
10. run with custom configuration (see example: https://github.com/mfosterrox/kube-linter-walkthrough/blob/main/configs/config_customChecks.yaml): 
  `% .gobin/kube-linter lint <k8s-yaml-examples> --config myconfig.yaml`

11. to update the auto-generated files before submitting a PR: `% make go-generated-srcs` and `% make generated-srcs`
  
12. run lint before submitting a PR: `% make lint`

13. write go tests under templates/<your-template/ if your template is complex (see examples of the existing templates)
14. to run tests: `% go test ./pkg/templates/hostmounts/...` (test a specific package), or `% make test` (test all)

15. add your checks to default list /internal/defaultchecks/default_checks.go if suggested by PR reviewr


