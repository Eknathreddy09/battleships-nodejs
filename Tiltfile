SOURCE_IMAGE = os.getenv("SOURCE_IMAGE", default='dev.local/battleship-inner-source')
LOCAL_PATH = os.getenv("LOCAL_PATH", default='.')
NAMESPACE = os.getenv("NAMESPACE", default='default')

k8s_custom_deploy(
    'battleship-inner',
    apply_cmd="tanzu apps workload apply -f config/workload.yaml --update-strategy replace --debug --live-update" +
               " --local-path " + LOCAL_PATH +
               " --source-image " + SOURCE_IMAGE +
               " --namespace " + NAMESPACE +
               " --yes --output yaml",
    delete_cmd="tanzu apps workload delete -f config/workload.yaml --namespace " + NAMESPACE + " --yes",
    deps=['pom.xml', './target/classes'],
    container_selector='workload',
    live_update=[
      sync('./target/classes', '/workspace/BOOT-INF/classes')
    ]
)
allow_k8s_contexts('tap-iterate-cluster')
k8s_resource('battleship-inner', port_forwards=["8080:8080"],
            extra_pod_selectors=[{'carto.run/workload-name': 'battleship-inner', 'app.kubernetes.io/component': 'run'}])
