## Install TAP

(...Playing around with Packaging k8s APIs + TAP beta2...)

Install kapp-controller + secretgen-controller

```
$ time kapp deploy -a prereq -f 0-config-prereq/
```

Create `values.yml`:

```yaml
image_registry:
  username: "<tanzunet username>"
  password: "<tanzunet password>"
rw_app_registry:
  # e.g. us-east4-docker.pkg.dev/cf-sandbox-dkalinin/test-areg-private/tbs
  server_repo: "<registry location where things can be written/read>"
  username: "<username>"
  password: "<password>"
```

Install TAP:

```
$ time kapp deploy -a tap -f <(ytt -f 1-config-tap/ --data-values-file values.yml)
```

(Currently not all TAP components are installed because some require manual configuration.)

Deploy workload:

```
$ kapp deploy -a app -f 2-config-app/
```
