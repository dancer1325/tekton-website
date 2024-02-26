# Prerequisites
* Follow the previous '/with Tasks' & '/with Pipelines'
* Install curl
  * [MacOs] `brew install curl`

# Goal
* Install Tekton Triggers
* Create a Tekton Trigger to run the 'hello-goodbye' Pipeline created in '../with Pipelines'
* Set up an
  * `EventListener`
  * `TriggerTemplate` which 
    * once the `EventListener` detects an event → specifying a blueprint for `PipelineRun`
  * `TriggerBinding` which
    * passes the data to -> `PipelineRun`

# Steps
* Install
  * Tekton Triggers
    * `kubectl apply --filename https://storage.googleapis.com/tekton-releases/triggers/latest/release.yaml`
  * Tekton Triggers Interceptors
    * `kubectl apply --filename https://storage.googleapis.com/tekton-releases/triggers/latest/interceptors.yaml`
    * Note: Super important to install it for make it run!!
  * Notes
    * Note1: For more details -> Check 'tekton-triggers' repo under '/install'
    * Note2: `kubectl get pods -n tekton-piplines` to check all pods are READY 1/1
* Create a triggerTemplate
  * `kubectl apply -f triggerTemplatev1Alpha1.yaml` or `kubectl apply -f triggerTemplatev1Beta1.yaml`
    * `kubectl get triggertemplate` to check the existence
    * `kubectl get pipelinerun` to check that NOT new PipelineRun have been created!!
* Create a triggerBinding
  * `kubectl apply -f triggerBindingv1Alpha1.yaml` or `kubectl apply -f triggerBindingv1Beta1.yaml`
    * `kubectl get triggerbinding` to check the existence
    * `kubectl get pipelinerun` to check that NOT new PipelineRun have been created!!
* Create a ServiceAccount
  * to make work the event listener
  * `kubectl apply -f rbacForv1Alpha1.yaml` or `kubectl apply -f rbacForv1Beta1.yaml
    * `kubectl get service` to check that services of type 'ClusterIP' have been created!! 
  * `kubectl port-forward service/el-hello-listener-v1alpha1 8080` or `kubectl port-forward service/el-hello-listener-v1beta1 8081:8080`
    * forward the services outside the cluster
* Create an EventListener
  * `kubectl apply -f eventListenerv1Alpha1.yaml` or `kubectl apply -f eventListenerv1Beta1.yaml`
    * `kubectl get eventlistener` to check the existence
    * `kubectl get pods` to check the existence of pods related to the event listener
* Trigger events
  * `curl -v -H 'content-Type: application/json' -d '{"username": "Alfred-Alpha1"}' http://localhost:8080` or `curl -v -H 'content-Type: application/json' -d '{"username": "Alfred-Beta1"}' http://localhost:8081`
    * Problems:
      * Problem1: "failed to connect to localhost:8080 inside namespace"
        * Attempt1: Delete all pods created during  '../withTasks' and '../withPipelines'
        * Attempt2: Start from the scratch the cluster, making run just 1! version
        * Solution: Start from the scratch, using '/allWithoutSuffixes'
          * `kubectl apply -f helloTask.yaml`, 
          * `kubectl apply -f goodbyeTask.yaml`,
          * `kubectl apply -f pipeline.yaml`,
          * `kubectl apply -f triggerTemplate.yaml`,
          * `kubectl apply -f triggerBinding.yaml`,
          * `kubectl apply -f rbac.yaml`,
          * `kubectl apply -f eventListener.yaml`
          * `kubectl port-forward service/el-hello-listener 8080`
          * Note: It must exist some problem in files v1Alpha1 and v1Beta1 -- TODO: Find what
  * `tkn pipelinerun logs pipelineRunName` to check the logs thrown