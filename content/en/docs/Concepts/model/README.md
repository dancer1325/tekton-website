# Prerequisites
* Follow the '../gettingStarted/withTasks' prerequisites

# Step
* := operation in a CI/CD workflow which
  * is performed via an image that you provide -- 'stepConcepts.yaml'
    * `kubectl apply -f stepConcepts.yaml`
    * `kubectl logs pod/prove-step-pod` to confirm that pod has been executed

# Pipelines
* 's state is tracked -- via -- Kubernetes annotations which  -- 'pipelineConcepts.yaml' --
  * `kubectl apply -f pipelineConcepts.yaml`
  * are projected as 👁️files inside step container 👁️— via — Downward API 
    * `kubectl describe pod/pipeline-concept-intermediate-pod` to confirm the existance of volume of type downward
    * `kubectl exec -it pipeline-concept-intermediate-pod sh` & `ls /tekton/downward` & `cat /tekton/downward/ready`
      * to confirm the existance of files with the pipeline's annotations
* injects an entrypoint binary in the step containers  which
  * `kubectl exec -it pipeline-concept-intermediate-pod sh` & `ls /tekton` and check the existence of 'bin/' 
  * watches closely the projected files
    * `kubectl get pod pipeline-concept-intermediate-pod -o jsonpath='{.spec.volumes[?(@.projected)]}'`
    * once an annotation appears as file → start the step’s command
      * Attempt1: `kubectl describe configmap/kube-root-ca.crt` check the configMap related to the projected volume
      * Attempt2: `kubectl describe taskrun/pipeline-concept-intermediate` related to completed pod, and check the annotation 'pipeline.tekton.dev/release: releaseID'
      * Solution: TODO: How to test?
* schedules 👁️containers to run after & before YOUR step containers 👁️
  * 's state is tracked via TaskRun or PipelineRun
    * Problems: 
      * Problem1: How to check the existance of those containers?
        * Attempt1: `kubectl logs taskrun/pipeline-concept-intermediate` or `kubectl logs pipelinerun/pipeline-concept`
        * Solution: TODO