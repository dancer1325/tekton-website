# Prerequisites
* Run some docker daemon
  * [Docker desktop](https://www.docker.com/products/docker-desktop/)
  * [Rancher dektop](https://rancherdesktop.io/)
* Install some local cluster
  * [minikube](https://minikube.sigs.k8s.io/docs/start/)
  * [kind](https://kind.sigs.k8s.io/)
    * Notes
      * Note1: If you use Rancher or Colina -> Not install via brew, since you need <=v0.19.0 [Reason](https://github.com/kubernetes-sigs/kind/issues/3277)
* Run a local cluster
  * [minikube]  `minikube start`
  * [kind] `kind create cluster`
    * Problems:
      * Problem1: " error during container init: error mounting "cgroup" to rootfs at "/sys/fs/cgroup": mount cgroup:/sys/fs/cgroup/openrc (via /proc/self/fd/7)"
        * Attempt1: `kind create cluster --image kindest/node:v1.28.0`
        * Solution: Install [Go](https://go.dev/doc/install) & `go install sigs.k8s.io/kind@v0.19.0`

# Goal
* Install Tekton Pipelines
* Create a Task
* `TaskRun` to instantiate & run the Task

# Steps
* Install Tekton Pipelines
  * `kubectl apply --filename https://storage.googleapis.com/tekton-releases/pipeline/latest/release.yaml`
  * Notes
    * Note1: For more details -> Check 'tekton-pipelines' repo under '/install'
    * Note2: `kubectl get pods -n tekton-piplines` to check all pods are READY 1/1
* Create a task
  * `kubectl apply -f taskv1.yaml` or `kubectl apply -f taskv1Beta1.yaml` 
    * `kubectl get task` to check that the tasks have been created
    * `kubectl get pods` to check the Tasks create an associated Pod
* Create a taskRun
  * `kubectl apply -f taskRunv1.yaml` or `kubectl apply -f taskRunv1Beta1.yaml`
    * `kubectl get taskrun` or `kubectl describe taskrun taskRunName` to check that the taskRuns have been created
    * `kubectl logs taskName-pod` to check the logs thrown by the Task