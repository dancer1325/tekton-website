# Pre requisites
* Run some docker daemon
  * [Docker desktop](https://www.docker.com/products/docker-desktop/)

# Local Kubernetes Cluster + Local registry
* [Via Docker Desktop](https://github.com/tektoncd/pipeline/blob/main/docs/developers/local-setup.md#using-docker-desktop)
* [Via Minikube](https://github.com/tektoncd/pipeline/blob/main/docs/developers/local-setup.md#using-minikube)
* [Via Kind](https://github.com/tektoncd/pipeline/blob/main/docs/developers/local-setup.md#using-minikube)
  * Notes
    * Note1: If you use Rancher or Colina -> Not install via brew, since you need <=v0.19.0 [Reason](https://github.com/kubernetes-sigs/kind/issues/3277). 
      * Install [Go](https://go.dev/doc/install) & 
      * `go install sigs.k8s.io/kind@v0.19.0`

# Kind + Scripts to run Tekton components
* 'tekton_in_kind.sh'
  * Mirrored from [here](https://github.com/tektoncd/plumbing/blob/main/hack/tekton_in_kind.sh)
* `sh tekton_in_kind.sh [-c cluster-name -p pipeline-version -t triggers-version -d dashboard-version -k container-runtime]`
  * `[...]` means that it's optional
  * 'pipeline-version', 'triggers-version' and 'dashboard-version' available ones, can be found https://storage.googleapis.com/tekton-releases/
  * Problems:
    * Problem1: "kubelet isn't running or healthy"
      * Attempt1: Remove 'containerdConfigPatches' for the kind cluster
      * Solution: TODO:
* 'tekton_in_kind_mine.sh'
  * My version of the previous script
  * `sh tekton_in_kind_mine.sh`
  * Problems:
    * Problem1: "error validating data: failed to download openapi"
      * 