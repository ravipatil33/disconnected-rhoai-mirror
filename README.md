# Disconnected-rhoai-mirror
Covers Image Set Configurations required to mirror OpenShift AI images into disconnected environment. 


### Phase 1 — Full RHOAI 3.4 operator mirror (OCP 4.19)

 Scope: All OpenShift AI and dependency operators (no additionalImages, no ModelCars).
 Mirror everything to private Quay registry or to disk ; install OpenShift AI and operators on the cluster.

#### Catalogs used:
   - redhat-operator-index:v4.19
   - certified-operator-index:v4.19
   - community-operator-index:v4.19

#### Package names required and mirrored and catalogs:
- Most of these are from redhat-operator-index catalog.
  ```
   connectivity link operator  -> rhcl-operator
   leader worker set operator  -> leader-worker-set
   kueue                       -> kueue-operator
   jobset operator             -> job-set
   cert-manager-operator       -> openshift-cert-manager-operator
   servicemeshoperator         -> servicemeshoperator3
  ```
  
This can be validated using oc mirror by specifying catalog and package options. 
$ oc mirror list operators --catalog=... --package=<name> --v2
add DEFAULT CHANNEL to ImageSetConfiguration

```
$ oc-mirror list operators   --catalog=registry.redhat.io/redhat/redhat-operator-index:v4.19   --package=rhods-operator 
NAME            DISPLAY NAME  DEFAULT CHANNEL
rhods-operator                stable-3.x

PACKAGE         CHANNEL                   HEAD
rhods-operator  3.2-exception-support     rhods-operator.3.2.1
rhods-operator  alpha                     rhods-operator.2.25.7
rhods-operator  beta                      rhods-operator.3.5.0-ea.1
rhods-operator  embedded                  rhods-operator.2.21.1
rhods-operator  eus-2.16                  rhods-operator.2.16.5
rhods-operator  eus-2.25                  rhods-operator.2.25.7
rhods-operator  eus-2.8                   rhods-operator.2.8.5
rhods-operator  fast                      rhods-operator.2.25.7
rhods-operator  fast-3.x                  rhods-operator.3.3.1
rhods-operator  stable                    rhods-operator.2.25.7
rhods-operator  stable-2.10               rhods-operator.2.10.2
rhods-operator  stable-2.13               rhods-operator.2.13.1
rhods-operator  stable-2.16               rhods-operator.2.16.5
rhods-operator  stable-2.19               rhods-operator.2.19.3
rhods-operator  stable-2.22               rhods-operator.2.22.3
rhods-operator  stable-2.25               rhods-operator.2.25.7
rhods-operator  stable-2.8                rhods-operator.2.8.4
rhods-operator  stable-3.3                rhods-operator.3.3.3
rhods-operator  stable-3.4                rhods-operator.3.4.0
rhods-operator  stable-3.x                rhods-operator.3.4.0
rhods-operator  support-required-upgrade  rhods-operator.3.3.3

$ oc-mirror list operators \
  --catalog=registry.redhat.io/redhat/redhat-operator-index:v4.19 \
  --package=servicemeshoperator3

NAME                  DISPLAY NAME  DEFAULT CHANNEL
servicemeshoperator3                stable

PACKAGE               CHANNEL     HEAD
servicemeshoperator3  candidates  servicemeshoperator3.v3.0.0-tp.2
servicemeshoperator3  stable      servicemeshoperator3.v3.3.4
servicemeshoperator3  stable-3.0  servicemeshoperator3.v3.0.12
servicemeshoperator3  stable-3.1  servicemeshoperator3.v3.1.9
servicemeshoperator3  stable-3.2  servicemeshoperator3.v3.2.6
servicemeshoperator3  stable-3.3  servicemeshoperator3.v3.3.4


$ oc-mirror list operators   --catalog=registry.redhat.io/redhat/redhat-operator-index:v4.19   --package=openshift-cert-manager-operator   

NAME                             DISPLAY NAME  DEFAULT CHANNEL
openshift-cert-manager-operator                stable-v1

PACKAGE                          CHANNEL       HEAD
openshift-cert-manager-operator  stable-v1     cert-manager-operator.v1.19.0
openshift-cert-manager-operator  stable-v1.15  cert-manager-operator.v1.15.2
openshift-cert-manager-operator  stable-v1.16  cert-manager-operator.v1.16.2
openshift-cert-manager-operator  stable-v1.17  cert-manager-operator.v1.17.1
openshift-cert-manager-operator  stable-v1.18  cert-manager-operator.v1.18.1
openshift-cert-manager-operator  stable-v1.19  cert-manager-operator.v1.19.0

NAME           DISPLAY NAME  DEFAULT CHANNEL
rhcl-operator                stable
PACKAGE        CHANNEL  HEAD
rhcl-operator  stable   rhcl-operator.v1.4.0



NAME            DISPLAY NAME  DEFAULT CHANNEL
kueue-operator                stable-v1.3
PACKAGE         CHANNEL      HEAD
kueue-operator  stable-v0.1  kueue-operator.v0.1.0
kueue-operator  stable-v0.2  kueue-operator.v0.2.1
kueue-operator  stable-v1.0  kueue-operator.v1.0.1
kueue-operator  stable-v1.1  kueue-operator.v1.1.0
kueue-operator  stable-v1.2  kueue-operator.v1.2.0
kueue-operator  stable-v1.3  kueue-operator.v1.3.1


NAME               DISPLAY NAME  DEFAULT CHANNEL
leader-worker-set                stable-v1.0
PACKAGE            CHANNEL      HEAD
leader-worker-set  stable-v1.0  leader-worker-set.v1.0.0


NAME     DISPLAY NAME  DEFAULT CHANNEL
job-set                stable-v1.0
PACKAGE  CHANNEL            HEAD
job-set  stable-v1.0        jobset-operator.v1.0.0
job-set  tech-preview-v0.1  jobset-operator.v0.1.0
[ec2-user@ip-10-0-30-199 data]$ 

NAME                                          DISPLAY NAME  DEFAULT CHANNEL
openshift-custom-metrics-autoscaler-operator                stable
PACKAGE                                       CHANNEL  HEAD
openshift-custom-metrics-autoscaler-operator  stable   custom-metrics-autoscaler.v2.19.0-1


NAME           DISPLAY NAME  DEFAULT CHANNEL
tempo-product                stable
PACKAGE        CHANNEL  HEAD
tempo-product  stable   tempo-operator.v0.21.0-1


NAME                            DISPLAY NAME  DEFAULT CHANNEL
cluster-observability-operator                stable
PACKAGE                         CHANNEL  HEAD
cluster-observability-operator  fast     cluster-observability-operator.v1.5.0
cluster-observability-operator  stable   cluster-observability-operator.v1.5.0


NAME                            DISPLAY NAME  DEFAULT CHANNEL
cluster-observability-operator                stable
PACKAGE                         CHANNEL  HEAD
cluster-observability-operator  fast     cluster-observability-operator.v1.5.0
cluster-observability-operator  stable   cluster-observability-operator.v1.5.0


NAME                   DISPLAY NAME  DEFAULT CHANNEL
opentelemetry-product                stable
PACKAGE                CHANNEL  HEAD
opentelemetry-product  stable   opentelemetry-operator.v0.152.0-1



```

-  From catalogs : certified and community 

gpu-operator-certified      -> certified-operator-index (not redhat)
```
   $ oc-mirror list operators   --catalog=registry.redhat.io/redhat/certified-operator-index:v4.19   --package=gpu-operator-certified 
NAME                    DISPLAY NAME  DEFAULT CHANNEL
gpu-operator-certified                v26.3

PACKAGE                 CHANNEL  HEAD
gpu-operator-certified  stable   gpu-operator-certified.v26.3.2
gpu-operator-certified  v1.10    gpu-operator-certified.v1.10.1
gpu-operator-certified  v1.11    gpu-operator-certified.v1.11.1
gpu-operator-certified  v22.9    gpu-operator-certified.v22.9.2
gpu-operator-certified  v23.3    gpu-operator-certified.v23.3.2
gpu-operator-certified  v23.6    gpu-operator-certified.v23.6.1
gpu-operator-certified  v23.9    gpu-operator-certified.v23.9.2
gpu-operator-certified  v24.3    gpu-operator-certified.v24.3.0
gpu-operator-certified  v24.6    gpu-operator-certified.v24.6.2
gpu-operator-certified  v24.9    gpu-operator-certified.v24.9.2
gpu-operator-certified  v25.10   gpu-operator-certified.v25.10.1
gpu-operator-certified  v25.3    gpu-operator-certified.v25.3.4
gpu-operator-certified  v26.3    gpu-operator-certified.v26.3.2
```

grafana operator            -> community-operator-index (not redhat)

```
   $ oc-mirror list operators --catalog=registry.redhat.io/redhat/community-operator-index:v4.19 --package=grafana-operator

NAME              DISPLAY NAME  DEFAULT CHANNEL
grafana-operator                v5
PACKAGE           CHANNEL  HEAD
grafana-operator  v5       grafana-operator.v5.24.0
```
### Prerequisites on connected bastion (RHEL 9) - Use this disk to push to private registry:

1. Login to registry and to private registry
   ```
   $  podman login registry.redhat.io
   $  podman login <YOUR_QUAY_HOST>:443
   ```
3. Mirror to disk (~120–180 GB for this operator set; use 1 TB disk):
   ```
   $ oc mirror -c imageset-p1-rhoai.yaml file:///data/mymirror --v2
   ```

5. Push to private Quay:
   ```
   $ oc mirror -c imageset-p1-rhoai.yaml \
   --from file:///data/mymirror docker://<YOUR_QUAY_HOST>:443 --v2
   ```

7. Apply generated catalog/IDMS (merge with existing foundation — do not replace):
   ```
   $  oc apply -f /data/mymirror/working-dir/cluster-resources/
   ```

#### Additional Notes 
 Validate exact package name in catalog : once per catalog (do NOT loop oc mirror list per package):
    ```
    $   oc mirror list operators --catalog=registry.redhat.io/redhat/redhat-operator-index:v4.19 --v2 | tee /tmp/rh.txt
    $   oc mirror list operators --catalog=registry.redhat.io/redhat/certified-operator-index:v4.19 --v2 | tee /tmp/cert.txt
    $   oc mirror list operators --catalog=registry.redhat.io/redhat/community-operator-index:v4.19 --v2 | tee /tmp/community.txt
   ```

- Phase 2: additionalImages from rhoai-disconnected-install-helper (notebooks, etc.)
 Use - imageset-p2-rhoai.yaml
- Phase 3: ModelCars (mirror separately — very large)
 Use - imageset-p3-rhoai.yaml 
