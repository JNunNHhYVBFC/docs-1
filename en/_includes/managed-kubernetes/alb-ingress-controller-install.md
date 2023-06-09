# Installing the {{ alb-name }} Ingress controller

To balance the load and distribute traffic between {{ k8s }} applications, use an {{ alb-full-name }} [Ingress controller](../../application-load-balancer/tools/k8s-ingress-controller/index.md). It runs the load balancer and the required auxiliary resources when the user creates an `Ingress` resource in a {{ managed-k8s-name }} cluster.

## Getting started {#before-you-begin}

1. {% include [cli-install](../cli-install.md) %}

   {% include [default-catalogue](../default-catalogue.md) %}

1. [Create a service account](../../iam/operations/sa/create.md) for the ingress controller to run and [assign the following roles to it](../../iam/operations/sa/assign-role-for-sa.md):
   * `alb.editor`: To create the required resources.
   * `vpc.publicAdmin`: To manage [external connectivity](../../vpc/security/index.md#roles-list).
   * `certificate-manager.certificates.downloader`: To use certificates registered in [{{ certificate-manager-full-name }}](../../certificate-manager/).
   * `compute.viewer`: To use {{ managed-k8s-name }} cluster nodes in balancer [target groups](../../application-load-balancer/concepts/target-group.md).
1. Create a [static access key](../../iam/operations/sa/create-access-key.md) for the service account in JSON format and save it to the `sa-key.json` file:

   ```bash
   yc iam key create \
     --service-account-name <name of service account for Ingress controller> \
     --format=json > sa-key.json
   ```


## Installation using {{ marketplace-full-name }} {#marketplace-install}

1. Go to the folder page and select **{{ managed-k8s-name }}**.
1. Click the name of the desired cluster and select the **{{ marketplace-short-name }}** ![Marketplace](../../_assets/marketplace.svg) tab.
1. Under **Applications available for installation**, select [ALB Ingress Controller](/marketplace/products/yc/alb-ingress-controller) and click **Use**.
1. Configure the application:
   * **Namespace**: Select a [namespace](../../managed-kubernetes/concepts/index.md#namespace) or create a new one.
   * **Application name**: Enter an application name.
   * **Folder ID**: Specify a [folder ID](../../resource-manager/operations/folder/get-id.md).
   * **Cluster ID**: Specify a [cluster ID](../../managed-kubernetes/operations/kubernetes-cluster/kubernetes-cluster-list.md).
   * **Secret Key**: Paste the contents of the `sa-key.json` file.
1. Click **Install**.


## Installation using a Helm chart {#install-alb-helm}

### Getting started {#before-helm}

1. {% include [helm-install](helm-install.md) %}

1. {% include [kubectl-install](kubectl-install.md) %}

1. Install the [`jq` utility](https://stedolan.github.io/jq/) for piped processing of JSON files.

   ```bash
   sudo apt update && sudo apt install jq
   ```

### Installation using a Helm chart {#helm-install}

1. To install a [Helm chart](https://helm.sh/docs/topics/charts/) with the Ingress controller, run these commands:

   
   ```bash
   export HELM_EXPERIMENTAL_OCI=1 && \
   cat sa-key.json | helm registry login {{ registry }} --username 'json_key' --password-stdin && \
   helm pull oci://{{ registry }}/yc-marketplace/yandex-cloud/yc-alb-ingress/yc-alb-ingress-controller-chart \
     --version <Helm chart version> \
     --untar && \
   helm install \
     --namespace <namespace> \
     --create-namespace \
     --set folderId=<folder ID> \
     --set clusterId=<cluster ID> \
     --set-file saKeySecretKey=sa-key.json \
     yc-alb-ingress-controller ./yc-alb-ingress-controller-chart/
   ```

   You can check the current version of the Helm chart on the [application page](/marketplace/products/yc/alb-ingress-controller#docker-images).



## See also {#see-also}

* [Description of Ingress controllers](https://kubernetes.io/docs/concepts/services-networking/ingress-controllers/) in the {{ k8s }} documentation.


* [Practical guideline for configuring the {{ alb-name }} Ingress controller](../../managed-kubernetes/tutorials/alb-ingress-controller.md).


* [Reference for the {{ alb-name }} Ingress controller](../../application-load-balancer/k8s-ref/index.md).