## Install TAP

(...Playing around with Packaging k8s APIs + TAP beta2...)

### Step 0

Install kapp-controller + secretgen-controller

```
$ time kapp deploy -a prereq -f 0-config-prereq/
```

### Step 1

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

### Step 2

Deploy workload:

```
$ kapp deploy -a app -f 2-config-app/
```

---
### Step 1 Alternative

Create `values.yml` as described above.

1st option:

```
$ time kapp deploy -a tap -f <(ytt -f 1.1-config-tap-alt/a-repo/ --data-values-file values.yml)

$ time kapp deploy -a tap -f <(ytt -f 1.1-config-tap-alt/b-pkg/ --data-values-file values.yml)
```

2nd option w/ tanzu CLI way:

```
$ time kapp deploy -a tap -f <(ytt -f 1.1-config-tap-alt/a-repo/ --data-values-file values.yml)

$ tanzu package install tap -n tap-install -p tap-full.tanzu.vmware.com -v 0.0.1 -f values.yml
```
