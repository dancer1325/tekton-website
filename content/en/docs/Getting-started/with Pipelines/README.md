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
* Complete '../with Tasks'
* Install [Tekton CLI](https://tekton.dev/docs/cli/)

# Goal
* Create two `Tasks`
* Create a `Pipeline` -- containing -- those `Tasks`
* `PipelineRun` to instantiate & run the previous `Pipeline`

# Steps
* Create the second task
  * `kubectl apply -f task2v1.yaml` or `kubectl apply -f task2v1Beta1.yaml` 
    * `kubectl get task` to check that the tasks have been created
    * `kubectl get pods` to check the Tasks create an associated Pod
* Create the pipelines
  * `kubectl apply -f pipelinev1.yaml` or `kubectl apply -f pipelinev1Beta1.yaml`
    * `kubectl get pipelines` to check the existance of the pipeline
    * `kubectl get taskrun` to check that it does NOT create yet 'TaskRun' / each Task in the pipeline!!!
* Create the pipelineRun
  * `kubectl apply -f pipelineRunv1.yaml` or `kubectl apply -f pipelineRunv1Beta1.yaml`
    * `kubectl get taskrun` to check that it creates NOW a 'TaskRun' / each Task in the pipeline!!!  --> 
      * `kubectl get pods` to check that pods NOW have been created!!
      * `kubectl logs taskName-pod` or `tkn pipelinerun logs pipelineRunName` to check the logs thrown by the Pipeline