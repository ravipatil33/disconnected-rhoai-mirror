# Disconnected-rhoai-mirror
Covers Image Set Configurations required to mirror OpenShift AI images into disconnected environment. 


### Phase 1 — Full RHOAI 3.4 operator mirror (OCP 4.19)

 Scope: All OpenShift AI and dependency operators (no additionalImages, no ModelCars).
 Mirror everything to private Quay registry or to disk ; install OpenShift AI and operators on the cluster.

##### Catalogs used:
   - redhat-operator-index:v4.19
   - certified-operator-index:v4.19
   - community-operator-index:v4.19

# Package names required and mirrored and catalogs:
- Most of these are from redhat-operator-index catalog. 
   connectivity link operator  -> rhcl-operator
   leader worker set operator  -> leader-worker-set
   kueue                       -> kueue-operator
   jobset operator             -> job-set
   cert-manager-operator       -> openshift-cert-manager-operator
   servicemeshoperator         -> servicemeshoperator3

-  From catalogs : certified and community 
   gpu-operator-certified      -> certified-operator-index (not redhat)
   grafana operator            -> community-operator-index (not redhat)

##### Prerequisites on connected bastion (RHEL 9) - Use this disk to push to private registry:

1. Login to registry and to private registry 
 $  podman login registry.redhat.io
 $  podman login <YOUR_QUAY_HOST>:443

2. Mirror to disk (~120–180 GB for this operator set; use 1 TB disk):
$   oc mirror -c imageset-p1-rhoai.yaml file:///data/mymirror --v2

3. Push to private Quay:
$  oc mirror -c imageset-p1-rhoai.yaml \
   --from file:///data/mymirror docker://<YOUR_QUAY_HOST>:443 --v2

4. Apply generated catalog/IDMS (merge with existing foundation — do not replace):
$  oc apply -f /data/mymirror/working-dir/cluster-resources/


Additional Notes 
 Validate exact package name in catalog : once per catalog (do NOT loop oc mirror list per package):
$   oc mirror list operators --catalog=registry.redhat.io/redhat/redhat-operator-index:v4.19 --v2 | tee /tmp/rh.txt
$   oc mirror list operators --catalog=registry.redhat.io/redhat/certified-operator-index:v4.19 --v2 | tee /tmp/cert.txt
$   oc mirror list operators --catalog=registry.redhat.io/redhat/community-operator-index:v4.19 --v2 | tee /tmp/community.txt

- Phase 2: additionalImages from rhoai-disconnected-install-helper (notebooks, etc.)
 Use - imageset-p2-rhoai.yaml
- Phase 3: ModelCars (mirror separately — very large)
 Use - imageset-p3-rhoai.yaml 
