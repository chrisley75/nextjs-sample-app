# Deploy app with Tekton

Deploy all tekton yaml file into openshift project

	.
	├── README.md
	├── base
	│   ├── build-bot.SA.yaml
	│   ├── create-configuration.Task.yaml
	│   ├── deploy.Task.yaml
	│   ├── emea-6-group7-git-ops.ConfigMap.yaml
	│   ├── git-credentials.Secret.yaml
	│   ├── nextjs.Pipeline.yaml
	│   ├── quay-io-credentials.Secret.yaml
	│   ├── save-configuration-group7.Task.yaml
	│   ├── vcs-trigger.EventListener.yaml
	│   ├── vcs-trigger.SA.yaml
	│   ├── vcs-trigger.TriggerBinding.yaml
	│   ├── vcs-trigger.TriggerTemplate.yaml
	│   └── vsc-trigger.Route.yaml
	└── nextjs.PipelineRun.yaml


`> oc create -f base/`

> **npm task** should be install using **kubectl**, not using **tkn hub install command**.
> 
> **tkn** checks version controll requirements that can denied the deployment 

[Tekon hub NPM](https://hub.tekton.dev/tekton/task/npm)

`kubectl apply -f https://raw.githubusercontent.com/tektoncd/catalog/main/task/npm/0.1/npm.yaml`

| Yaml configuration file | Description|
|---|---|
|[build-bot.SA.yaml](base/build-bot.SA.yaml)| Create **Service Account (SA)** that will be used by the pipelineRun, TriggerTemplate |
|[create-configuration.Task.yaml](base/create-configuration.Task.yaml) | Create **configuration** Task called by pipeline |
|[deploy.Task.yaml ](base/deploy.Task.yaml )| Create **deploy** Task called by pipeline |
|[emea-6-group7-git-ops.ConfigMap.yaml](base/deploy.Task.yaml ) | **ConfigMap** to define custome variable that will be called by the task (ie save-configuration task here). Declare as key/value|
|[git-credentials.Secret.yaml](base/deploy.Task.yaml )| **Secret** for **git User** and **git Personal Access Token (PAT)** for connecting to GitHub |
|[nextjs.Pipeline.yaml](base/nextjs.Pipeline.yaml)| **Pipeline** that calls **tasks**|
|[quay-io-credentials.Secret.yaml](base/quay-io-credentials.Secret.yaml)| **Secret** for authenticate robot on quay.io ==> [Quay Robot Accounts](https://docs.quay.io/glossary/robot-accounts.html) |
|[save-configuration-group7.Task.yaml](base/save-configuration-group7.Task.yaml)| Create **save-configuration** Task called by pipeline|
|[vcs-trigger.EventListener.yaml](base/vcs-trigger.EventListener.yaml)| Create **Tekton VCS EventListener** - listens for events at a specified port on the Kubernetes cluster. Specifies one or more Triggers.|
|[vcs-trigger.SA.yaml](base/vcs-trigger.SA.yaml)| Create **Service Account (SA)** that will be used by the **VCS EventListener**|
|[vcs-trigger.TriggerBinding.yaml](base/vcs-trigger.TriggerBinding.yaml)| Create **TriggerBinding** - specifies the fields in the event payload from which you want to extract data and the fields in your corresponding TriggerTemplate to populate with the extracted values. You can then use the populated fields in the TriggerTemplate to populate fields in the associated TaskRun or PipelineRun.|
|[vcs-trigger.TriggerTemplate.yaml](base/vcs-trigger.TriggerTemplate.yaml)| Create **TriggerTemplate** - specifies a blueprint for the resource, such as a TaskRun or PipelineRun, to execute when your EventListener receives an event. It exposes parameters that you can use anywhere within your resource's template.|
|[vsc-trigger.Route.yaml](base/vsc-trigger.Route.yaml)| Create **Route** to expose **VCS EventListener**, this route will be declared in the VCS webhook (ie GitHub)|
|[nextjs.PipelineRun.yaml](nextjs.PipelineRun.yaml)| Create **pipelineRun** to execute the pipeline (not used if we deploy using vcs TriggerTemplate)|
