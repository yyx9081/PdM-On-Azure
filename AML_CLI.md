#### CLI Script
- login
az login

- subscription
az account list
az account set -s <subscription id>

- resource group
az group list
az group create -l (region) -n (resourceGroup)

- AML service
az ml account experimentation create -n <experimentation name> -g <resource group name>
az ml env setup -n <new deployment environment name> -l <e.g. eastus2> -g <resource group name> -c (for cluster mode)
az ml env show -n <deployment environment name> -g <existing resource group name>
az ml account modelmanagement create --location <e.g. eastus2> -n <new model management account name> -g <existing resource group name> --sku-name S1
az ml account modelmanagement set -n <youracctname> -g <yourresourcegroupname>
az ml env set -n <deployment environment name> -g <existing resource group name>
az ml env show

- Create real-time web service
az ml model register --model model.pkl --name model.pkl
az ml manifest create --manifest-name <new manifest name> -f score_iris.py -r python -i <model ID> -s service_schema.json -c aml_config\conda_dependencies.yml
az ml image create -n irisimage --manifest-id <manifest ID>
az ml service create realtime --image-id <image ID> -n irisapp --collect-model-data true
az ml image usage -i <image ID>
az ml env get-credentials -g <resource group name> -n <deployment account>

- Create real time web service in one command
az ml service create realtime -f score_iris.py --model-file model.pkl -s service_schema.json -n irisapp -r python --collect-model-data true -c aml_config\conda_dependencies.yml

- Run real-time web service
az ml service usage realtime -i <web service ID>
az ml service run realtime -i <web service ID> -d "{\"input_df\": [{\"petal width\": 0.25, \"sepal length\": 3.0, \"sepal width\": 3.6, \"petal length\": 1.3}]}"

- namespace register
az provider register --namespace <namespace ex. Microsoft.Storage>
az provider show -n <namespace>
az provider list --query "[].{Provider:namespace, Status:registrationState}" --out table 


#### IoT
- IoT Hub
az iot hub create --resource-group <resource group name> --name <iot hub name> --sku F1

#### Spark on HDInsight
az ml computetarget attach cluster --name <myhdi> --address <myhdi-ssh.azurehdinsight.net> --username <sshusername> --password <sshpwd>
az ml experiment prepare -c <myhdi>

#### Spark Deployment
az ml service create realtime -f score_iris.py -m iris.mml -s service_schema_spark.json -r spark-py -n irismml -c aml_config/conda_dependencies.yml