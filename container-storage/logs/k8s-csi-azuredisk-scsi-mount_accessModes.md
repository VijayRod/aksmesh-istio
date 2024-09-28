- https://kubernetes.io/docs/concepts/storage/persistent-volumes/#access-modes
- https://learn.microsoft.com/en-us/troubleshoot/azure/azure-kubernetes/fail-to-mount-azure-disk-volume#error3: An Azure disk can be mounted only as ReadWriteOnce, which makes it available to one node in AKS.
- https://learn.microsoft.com/en-us/azure/aks/azure-csi-disk-storage-provision: An Azure disk can only be mounted with Access mode type ReadWriteOnce, which makes it available to one node in AKS. This access mode still allows multiple pods to access the volume when the pods run on the same node.