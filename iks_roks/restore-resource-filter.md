---

copyright:
  years: 2026, 2026
lastupdated: "2026-11-03"

keywords: restore filter, resource exclusion, LoadBalancer, Kubernetes restore, namespace recovery, iks, roks

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Excluding resources during namespace restore (Restore Resource Filter)
{: #restore-resource-filter}

The IBM Restore Resource Filter plug-in allows the user to skip specific Kubernetes resource types during a namespace restore, based on field values in the backed-up resource. The most common use case is excluding `LoadBalancer` Services when you restore a namespace into a new cluster, so you can re-create them manually with the correct cloud load-balancer settings.

You control filtering entirely through a ConfigMap that you create or update before triggering a restore. No changes to the {{site.data.keyword.baas_full}} UI or API are required.

The plug-in is automatically enabled on {{site.data.keyword.baas_full_notm}} deployments where the management namespace name starts with `brs-backup-agent`.
{: note}

## Before you begin
{: #restore-resource-filter-prereqs}

Identify your {{site.data.keyword.baas_full_notm}} management namespace. On IBM IKS/ROKS, it typically starts with `brs-backup-agent`:

```sh
kubectl get ns | grep brs-backup-agent
```
{: codeblock}

Note the full name (for example, `brs-backup-agent-abc123`). You need it in the following steps.

## Quick start: exclude LoadBalancer Services
{: #restore-resource-filter-quickstart}

### Create the filter ConfigMap
{: #restore-filter-step1}
{: step}

Replace `BRS_NAMESPACE` with your management namespace name and `BACKUP_NAMESPACE` with the namespace name as it appeared in the backup (not the restore target name if you are restoring under a different name):

```sh
export BRS_NAMESPACE=brs-backup-agent-<your-cluster-id>
export BACKUP_NAMESPACE=<your-application-namespace>

kubectl apply -f - <<EOF
apiVersion: v1
kind: ConfigMap
metadata:
  name: brs-restore-resource-filters
  namespace: ${BRS_NAMESPACE}
  labels:
    velero.io/plugin-config: ""
data:
  config: |
    version: v1
    rules:
      - name: drop-loadbalancer-services
        resourceTypes: ["Service"]
        namespaces: ["${BACKUP_NAMESPACE}"]
        match:
          all:
            - path: /spec/type
              operator: Equals
              value: LoadBalancer
EOF
```
{: codeblock}

### Run the restore from {{site.data.keyword.baas_full_notm}}
{: #restore-filter-step2}
{: step}

Start your namespace restore through {{site.data.keyword.baas_full_notm}} as you normally do. No additional configuration is required on the {{site.data.keyword.baas_full_notm}} side.

### Verify the result
{: #restore-filter-step3}
{: step}

After the restore completes, confirm that LoadBalancer Services are absent and ClusterIP or NodePort Services are present:

```sh
kubectl get svc -n <target-namespace> -o wide
```
{: codeblock}

## ConfigMap reference
{: #restore-filter-configmap-reference}

### Required structure
{: #restore-filter-structure}

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: brs-restore-resource-filters          # name is fixed
  namespace: brs-backup-agent-<cluster-id>    # your BRS management namespace
  labels:
    velero.io/plugin-config: ""               # required — plugin cannot see the ConfigMap without this label
data:
  config: |
    version: v1
    rules:
      - name: <rule-name>
        resourceTypes: [<Kind>, ...]
        namespaces: [<backup-namespace>, ...]  # optional
        match:
          all: [...]   # optional, but at least one of all/any is required
          any: [...]
```

### Rule schema
{: #restore-filter-rule-schema}

| Field | Required | Description |
|---|---|---|
| `version` | Yes | Must be `v1`. Any other value disables filtering. |
| `rules` | Yes | List of exclusion rules. A resource is skipped if **any** rule matches. |
| `rules[].name` | Yes | Nonempty identifier; appears in plug-in logs. |
| `rules[].resourceTypes` | Yes | Kubernetes Kinds in CamelCase (for example, `Service`, `PersistentVolumeClaim`). Group-qualified forms like `VirtualMachine.kubevirt.io` are supported. Use `*` to match all types. |
| `rules[].namespaces` | No | Backup namespace names to scope the rule. Omit or leave empty to match all namespaces. |
| `rules[].match` | Yes | Predicate group with at least one of `all` or `any`. |
{: caption="Rule schema fields" caption-side="bottom"}

### Predicate fields
{: #restore-filter-predicates}

| Field | Description |
|---|---|
| `path` | RFC 6901 JSON Pointer to a field in the backed-up resource (for example, `/spec/type`). |
| `operator` | One of: `Equals`, `NotEquals`, `In`, `NotIn`, `Exists`, `DoesNotExist`. |
| `value` | Single scalar for `Equals` / `NotEquals`. |
| `values` | Nonempty list of scalars for `In` / `NotIn`. |
{: caption="Predicate fields" caption-side="bottom"}

Predicates are evaluated against the resource **as it was at backup time**, including fields Velero normally strips during restore (such as `uid`, `creationTimestamp`, `status`). This means that you can safely match on `/status` or server-managed metadata.

#### JSON Pointer escaping
{: #restore-filter-json-pointer}

Paths follow RFC 6901. If a key contains `/` or `~` (common in annotation and label keys), escape as follows:

| Character | Escape | Example key | Pointer segment |
|---|---|---|---|
| `/` | `~1` | `restore.ibm.com/skip` | `restore.ibm.com~1skip` |
| `~` | `~0` | `flag~v2` | `flag~0v2` |
{: caption="JSON Pointer escaping" caption-side="bottom"}

For example, to match annotation `restore.ibm.com/skip: "true"`:

```yaml
- path: /metadata/annotations/restore.ibm.com~1skip
  operator: Exists
```

## Examples
{: #restore-filter-examples}

### Exclude LoadBalancer Services in multiple namespaces
{: #restore-filter-ex-multi-ns}

```yaml
version: v1
rules:
  - name: drop-loadbalancer-services
    resourceTypes: ["Service"]
    namespaces: ["app-frontend", "app-backend", "payments-prod"]
    match:
      all:
        - path: /spec/type
          operator: Equals
          value: LoadBalancer
```

### Exclude resources marked with an annotation
{: #restore-filter-ex-annotation}

Useful when you want application teams to opt specific objects out of a restore:

```yaml
version: v1
rules:
  - name: drop-annotated-resources
    resourceTypes: ["*"]
    match:
      all:
        - path: /metadata/annotations/restore.ibm.com~1skip
          operator: Exists
```

Set the following annotation on the workload object before backup:

```yaml
metadata:
  annotations:
    restore.ibm.com/skip: "true"
```

## Important behavior notes
{: #restore-filter-behavior-notes}

- **Rules match the backed-up resource, not the live cluster.** Field values reflect the state at backup time.
- **`namespaces` refers to the backup namespace**, not the restore target. If you restore `payments-prod` into `payments-prod-dr`, write `namespaces: ["payments-prod"]`.
- **Invalid configuration never fails a restore.** If the ConfigMap is absent, malformed, uses the wrong version, or has empty rules, filtering is disabled and the restore proceeds normally. Always check Velero logs after a restore if resources you expected to skip were still created.
- **Use CamelCase Kinds.** Use `Service`, not `services`. Use `PersistentVolumeClaim`, not `persistentvolumeclaims`.
- **There is no per-restore toggle in the UI.** The ConfigMap is the only control plane. To disable filtering for one restore, delete or rename the ConfigMap before starting the restore.

## Managing the filter ConfigMap
{: #restore-filter-manage}

| Goal | Action |
|---|---|
| Change rules | `kubectl apply -f` with an updated ConfigMap (same name and namespace). The plug-in picks up changes on the next restore. |
| Disable filtering temporarily | Delete the ConfigMap: `kubectl delete configmap brs-restore-resource-filters -n <BRS_NAMESPACE>` |
| Disable for one restore | Help ensure the ConfigMap does not exist before starting that restore. |
{: caption="Managing the filter ConfigMap" caption-side="bottom"}

## Troubleshooting
{: #restore-filter-troubleshoot}

**Filtered resources still appear after restore**

1. Confirm the ConfigMap exists in the {{site.data.keyword.baas_full_notm}} namespace (not the application namespace):

   ```sh
   kubectl get configmap brs-restore-resource-filters -n "${BRS_NAMESPACE}" -o yaml
   ```
   {: codeblock}

2. Confirm the `velero.io/plugin-config: ""` label is present.
3. Confirm the `namespaces` value matches the **backup** namespace name, not the restore target name.
4. Check Velero logs for lines like:
   `Skipping restore of Service <namespace>/<name> excluded by resource filter rule "drop-loadbalancer-services"`

**Filtering has no effect at all**

- Verify your management namespace name starts with `brs-backup-agent`. The plug-in is only registered in that environment.
- Help ensure `version: v1` is set exactly.
- Validate YAML indentation inside the `config: |` block.
