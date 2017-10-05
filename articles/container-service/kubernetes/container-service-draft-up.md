---
title: "Použití nástroje Draft se službami Azure Container Service a Azure Container Registry | Dokumentace Microsoftu"
description: "Vytvořte cluster ACS Kubernetes a službu Azure Container Registry, abyste mohli vytvořit první aplikaci v Azure pomocí nástroje Draft."
services: container-service
documentationcenter: 
author: squillace
manager: gamonroy
editor: 
tags: draft, helm, acs, azure-container-service
keywords: Docker, Containers, microservices, Kubernetes, Draft, Azure
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/31/2017
ms.author: rasquill
ms.custom: mvc
ms.openlocfilehash: e7e3ea461145571753a1a6d768b52118dcbfb507
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="use-draft-with-azure-container-service-and-azure-container-registry-to-build-and-deploy-an-application-to-kubernetes"></a><span data-ttu-id="86e0d-104">Použití nástroje Draft se službami Azure Container Service a Azure Container Registry k sestavení a nasazení aplikace do Kubernetes</span><span class="sxs-lookup"><span data-stu-id="86e0d-104">Use Draft with Azure Container Service and Azure Container Registry to build and deploy an application to Kubernetes</span></span>

<span data-ttu-id="86e0d-105">[Draft](https://aka.ms/draft) je nový opensourcový nástroj usnadňující vývoj kontejnerových aplikací a jejich nasazení do clusterů Kubernetes, který nevyžaduje téměř žádné znalosti Dockeru ani Kubernetes – dokonce je nemusíte ani instalovat.</span><span class="sxs-lookup"><span data-stu-id="86e0d-105">[Draft](https://aka.ms/draft) is a new open-source tool that makes it easy to develop container-based applications and deploy them to Kubernetes clusters without knowing much about Docker and Kubernetes -- or even installing them.</span></span> <span data-ttu-id="86e0d-106">Používání nástrojů, jako je Draft, umožňuje vám a vašim týmům zaměřit se na vytváření aplikací pomocí Kubernetes, aniž byste se museli tolik zabývat infrastrukturou.</span><span class="sxs-lookup"><span data-stu-id="86e0d-106">Using tools like Draft let you and your teams focus on building the application with Kubernetes, not paying as much attention to infrastructure.</span></span>

<span data-ttu-id="86e0d-107">Draft můžete používat s libovolným registrem imagí Dockeru a jakýmkoli clusterem Kubernetes, a to i místně.</span><span class="sxs-lookup"><span data-stu-id="86e0d-107">You can use Draft with any Docker image registry and any Kubernetes cluster, including locally.</span></span> <span data-ttu-id="86e0d-108">Tento kurz ukazuje použití služby ACS s Kubernetes, službou Azure Container Registry a Azure DNS k vytvoření živého vývojářského kanálu pro průběžnou integraci a doručování pomocí nástroje Draft.</span><span class="sxs-lookup"><span data-stu-id="86e0d-108">This tutorial shows how to use ACS with Kubernetes, ACR, and Azure DNS to create a live CI/CD developer pipeline using Draft.</span></span>


## <a name="create-an-azure-container-registry"></a><span data-ttu-id="86e0d-109">Vytvoření služby Azure Container Registry</span><span class="sxs-lookup"><span data-stu-id="86e0d-109">Create an Azure Container Registry</span></span>
<span data-ttu-id="86e0d-110">Můžete snadno [vytvořit novou službu Azure Container Registry](../../container-registry/container-registry-get-started-azure-cli.md), ale postup je následující:</span><span class="sxs-lookup"><span data-stu-id="86e0d-110">You can easily [create a new Azure Container Registry](../../container-registry/container-registry-get-started-azure-cli.md), but the steps are as follows:</span></span>

1. <span data-ttu-id="86e0d-111">Vytvořte skupinu prostředků Azure pro správu registru Azure Container Registry a clusteru Kubernetes ve službě ACS.</span><span class="sxs-lookup"><span data-stu-id="86e0d-111">Create a Azure resource group to manage your ACR registry and the Kubernetes cluster in ACS.</span></span>
      ```azurecli
      az group create --name draft --location eastus
      ```

2. <span data-ttu-id="86e0d-112">Vytvořte registr imagí Azure Container Registry pomocí příkazu [az acr create](/cli/azure/acr#create).</span><span class="sxs-lookup"><span data-stu-id="86e0d-112">Create an ACR image registry using [az acr create](/cli/azure/acr#create)</span></span>
      ```azurecli
      az acr create -g draft -n draftacs --sku Basic --admin-enabled true -l eastus
      ```


## <a name="create-an-azure-container-service-with-kubernetes"></a><span data-ttu-id="86e0d-113">Vytvoření služby Azure Container Service pomocí Kubernetes</span><span class="sxs-lookup"><span data-stu-id="86e0d-113">Create an Azure Container Service with Kubernetes</span></span>

<span data-ttu-id="86e0d-114">Nyní jste připraveni použít příkaz [az acs create](/cli/azure/acs#create) k vytvoření clusteru ACS s použitím Kubernetes jako hodnoty `--orchestrator-type`.</span><span class="sxs-lookup"><span data-stu-id="86e0d-114">Now you're ready to use [az acs create](/cli/azure/acs#create) to create an ACS cluster using Kubernetes as the `--orchestrator-type` value.</span></span>
```azurecli
az acs create --resource-group draft --name draft-kube-acs --dns-prefix draft-cluster --orchestrator-type kubernetes
```

> [!NOTE]
> <span data-ttu-id="86e0d-115">Vzhledem k tomu, že Kubernetes není výchozím typem orchestrátoru, nezapomeňte použít přepínač `--orchestrator-type kubernetes`.</span><span class="sxs-lookup"><span data-stu-id="86e0d-115">Because Kubernetes is not the default orchestrator type, be sure you use the `--orchestrator-type kubernetes` switch.</span></span>

<span data-ttu-id="86e0d-116">Výstup po úspěšném provedení je podobný následujícímu.</span><span class="sxs-lookup"><span data-stu-id="86e0d-116">The output when successful looks similar to the following.</span></span>

```json
waiting for AAD role to propagate.done
{
  "id": "/subscriptions/<guid>/resourceGroups/draft/providers/Microsoft.Resources/deployments/azurecli14904.93snip09",
  "name": "azurecli1496227204.9323909",
  "properties": {
    "correlationId": "<guid>",
    "debugSetting": null,
    "dependencies": [],
    "mode": "Incremental",
    "outputs": null,
    "parameters": {
      "clientSecret": {
        "type": "SecureString"
      }
    },
    "parametersLink": null,
    "providers": [
      {
        "id": null,
        "namespace": "Microsoft.ContainerService",
        "registrationState": null,
        "resourceTypes": [
          {
            "aliases": null,
            "apiVersions": null,
            "locations": [
              "westus"
            ],
            "properties": null,
            "resourceType": "containerServices"
          }
        ]
      }
    ],
    "provisioningState": "Succeeded",
    "template": null,
    "templateLink": null,
    "timestamp": "2017-05-31T10:46:29.434095+00:00"
  },
  "resourceGroup": "draft"
}
```

<span data-ttu-id="86e0d-117">Když teď máte cluster, můžete importovat přihlašovací údaje pomocí příkazu [az acs kubernetes get-credentials](/cli/azure/acs/kubernetes#get-credentials).</span><span class="sxs-lookup"><span data-stu-id="86e0d-117">Now that you have a cluster, you can import the credentials by using the [az acs kubernetes get-credentials](/cli/azure/acs/kubernetes#get-credentials) command.</span></span> <span data-ttu-id="86e0d-118">Nyní máte místní konfigurační soubor pro váš cluster, který pro svou práci potřebuje Helm a Draft.</span><span class="sxs-lookup"><span data-stu-id="86e0d-118">Now you have a local configuration file for your cluster, which is what Helm and Draft need to get their work done.</span></span>

## <a name="install-and-configure-draft"></a><span data-ttu-id="86e0d-119">Instalace a konfigurace nástroje Draft</span><span class="sxs-lookup"><span data-stu-id="86e0d-119">Install and configure draft</span></span>
<span data-ttu-id="86e0d-120">Pokyny k instalaci nástroje Draft najdete v [úložišti Draft](https://github.com/Azure/draft/blob/master/docs/install.md).</span><span class="sxs-lookup"><span data-stu-id="86e0d-120">The installation instructions for Draft are in the [Draft repository](https://github.com/Azure/draft/blob/master/docs/install.md).</span></span> <span data-ttu-id="86e0d-121">I když jsou poměrně jednoduché, vyžadují určitou konfiguraci, protože instalace je závislá na [Helmu](https://aka.ms/helm), který musí vytvořit diagram Helmu a nasadit ho do clusteru Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="86e0d-121">They are relatively simple, but do require some configuration, as it depends on [Helm](https://aka.ms/helm) to create and deploy a Helm chart into the Kubernetes cluster.</span></span>

1. <span data-ttu-id="86e0d-122">[Stáhněte a nainstalujte Helm](https://aka.ms/helm#install).</span><span class="sxs-lookup"><span data-stu-id="86e0d-122">[Download and install Helm](https://aka.ms/helm#install).</span></span>
2. <span data-ttu-id="86e0d-123">Pomocí Helmu vyhledejte a nainstalujte `stable/traefik`, kontroler příchozího přenosu dat pro umožnění příchozích požadavků ve vašich buildech.</span><span class="sxs-lookup"><span data-stu-id="86e0d-123">Use Helm to search for and install `stable/traefik`, and ingress controller to enable inbound requests for your builds.</span></span>
    ```bash
    $ helm search traefik
    NAME            VERSION DESCRIPTION
    stable/traefik  1.3.0   A Traefik based Kubernetes ingress controller w...

    $ helm install stable/traefik --name ingress
    ```
    <span data-ttu-id="86e0d-124">Nyní nastavte na kontroleru `ingress` sledování, aby při nasazení zachytil hodnotu externí IP adresy.</span><span class="sxs-lookup"><span data-stu-id="86e0d-124">Now set a watch on the `ingress` controller to capture the external IP value when it is deployed.</span></span> <span data-ttu-id="86e0d-125">Tato IP adresa bude v další části [namapovaná na vaši doménu nasazení](#wire-up-deployment-domain).</span><span class="sxs-lookup"><span data-stu-id="86e0d-125">This IP address will be the one [mapped to your deployment domain](#wire-up-deployment-domain) in the next section.</span></span>

    ```bash
    kubectl get svc -w
    NAME                          CLUSTER-IP     EXTERNAL-IP     PORT(S)                      AGE
    ingress-traefik               10.0.248.104   13.64.108.240   80:31046/TCP,443:32556/TCP   1h
    kubernetes                    10.0.0.1       <none>          443/TCP                      7h
    ```

    <span data-ttu-id="86e0d-126">V tomto případě je externí IP adresa pro doménu nasazení `13.64.108.240`.</span><span class="sxs-lookup"><span data-stu-id="86e0d-126">In this case, the external IP for the deployment domain is `13.64.108.240`.</span></span> <span data-ttu-id="86e0d-127">Nyní můžete namapovat doménu na tuto IP adresu.</span><span class="sxs-lookup"><span data-stu-id="86e0d-127">Now you can map your domain to that IP.</span></span>

## <a name="wire-up-deployment-domain"></a><span data-ttu-id="86e0d-128">Napojení domény nasazení</span><span class="sxs-lookup"><span data-stu-id="86e0d-128">Wire up deployment domain</span></span>

<span data-ttu-id="86e0d-129">Draft vytvoří vydanou verzi pro každý diagram Helmu, který vytvoří – pro každou aplikaci, na které pracujete.</span><span class="sxs-lookup"><span data-stu-id="86e0d-129">Draft creates a release for each Helm chart it creates -- each application you are working on.</span></span> <span data-ttu-id="86e0d-130">Každá z nich dostane vygenerovaný název, který Draft používá jako _subdoménu_ kořenové _domény nasazení_, nad kterou máte kontrolu.</span><span class="sxs-lookup"><span data-stu-id="86e0d-130">Each one gets a generated name that is used by draft as a _subdomain_ on top of the root _deployment domain_ that you control.</span></span> <span data-ttu-id="86e0d-131">(V tomto příkladu používáme jako doménu nasazení `squillace.io`.) K povolení tohoto chování subdomén musíte v záznamech DNS pro vaši doménu nasazení vytvořit záznam A pro `'*'`, aby se každá vygenerovaná subdoména přesměrovala do kontroleru příchozího přenosu clusteru Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="86e0d-131">(In this example, we use `squillace.io` as the deployment domain.) To enable this subdomain behavior, you must create an A record for `'*'` in your DNS entries for your deployment domain, so that each generated subdomain is routed to the Kubernetes cluster's ingress controller.</span></span>

<span data-ttu-id="86e0d-132">Váš poskytovatel domény má vlastní způsob přiřazování serverů DNS. Pokud chcete [delegovat názvové servery vaší domény do Azure DNS](../../dns/dns-delegate-domain-azure-dns.md), postupujte následovně:</span><span class="sxs-lookup"><span data-stu-id="86e0d-132">Your own domain provider has their own way to assign DNS servers; to [delegate your domain nameservers to Azure DNS](../../dns/dns-delegate-domain-azure-dns.md), you take the following steps:</span></span>

1. <span data-ttu-id="86e0d-133">Vytvořte skupinu prostředků pro vaši zónu DNS.</span><span class="sxs-lookup"><span data-stu-id="86e0d-133">Create a resource group for your zone.</span></span>
    ```azurecli
    az group create --name squillace.io --location eastus
    {
      "id": "/subscriptions/<guid>/resourceGroups/squillace.io",
      "location": "eastus",
      "managedBy": null,
      "name": "zones",
      "properties": {
        "provisioningState": "Succeeded"
      },
      "tags": null
    }
    ```

2. <span data-ttu-id="86e0d-134">Vytvořte zónu DNS pro vaši doménu.</span><span class="sxs-lookup"><span data-stu-id="86e0d-134">Create a DNS zone for your domain.</span></span>
<span data-ttu-id="86e0d-135">Pomocí příkazu [az network dns zone create](/cli/azure/network/dns/zone#create) získejte názvové servery pro delegování řízení DNS do Azure DNS pro vaši doménu.</span><span class="sxs-lookup"><span data-stu-id="86e0d-135">Use the [az network dns zone create](/cli/azure/network/dns/zone#create) command to obtain the nameservers to delegate DNS control to Azure DNS for your domain.</span></span>
    ```azurecli
    az network dns zone create --resource-group squillace.io --name squillace.io
    {
      "etag": "<guid>",
      "id": "/subscriptions/<guid>/resourceGroups/zones/providers/Microsoft.Network/dnszones/squillace.io",
      "location": "global",
      "maxNumberOfRecordSets": 5000,
      "name": "squillace.io",
      "nameServers": [
        "ns1-09.azure-dns.com.",
        "ns2-09.azure-dns.net.",
        "ns3-09.azure-dns.org.",
        "ns4-09.azure-dns.info."
      ],
      "numberOfRecordSets": 2,
      "resourceGroup": "squillace.io",
      "tags": {},
      "type": "Microsoft.Network/dnszones"
    }
    ```
3. <span data-ttu-id="86e0d-136">Přidejte získané servery DNS k poskytovateli vaší domény nasazení – to vám umožní použít Azure DNS ke směrování vaší domény, jak budete chtít.</span><span class="sxs-lookup"><span data-stu-id="86e0d-136">Add the DNS servers you are given to the domain provider for your deployment domain, which enables you to use Azure DNS to repoint your domain as you want.</span></span>
4. <span data-ttu-id="86e0d-137">Vytvořte položku sady záznamů A pro mapování domény nasazení na ID adresu `ingress` z kroku 2 v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="86e0d-137">Create an A record-set entry for your deployment domain mapping to the `ingress` IP from step 2 of the previous section.</span></span>
    ```azurecli
    az network dns record-set a add-record --ipv4-address 13.64.108.240 --record-set-name '*' -g squillace.io -z squillace.io
    ```
<span data-ttu-id="86e0d-138">Výstup by měl vypadat asi takto:</span><span class="sxs-lookup"><span data-stu-id="86e0d-138">The output looks something like:</span></span>
    ```json
    {
      "arecords": [
        {
          "ipv4Address": "13.64.108.240"
        }
      ],
      "etag": "<guid>",
      "id": "/subscriptions/<guid>/resourceGroups/squillace.io/providers/Microsoft.Network/dnszones/squillace.io/A/*",
      "metadata": null,
      "name": "*",
      "resourceGroup": "squillace.io",
      "ttl": 3600,
      "type": "Microsoft.Network/dnszones/A"
    }
    ```

5. <span data-ttu-id="86e0d-139">Nakonfigurujte Draft pro použití vašeho registru a vytvoření subdomény pro každý diagram Helmu, který vytvoří.</span><span class="sxs-lookup"><span data-stu-id="86e0d-139">Configure Draft to use your registry and create subdomains for each Helm chart it creates.</span></span> <span data-ttu-id="86e0d-140">Ke konfiguraci nástroje Draft potřebujete:</span><span class="sxs-lookup"><span data-stu-id="86e0d-140">To configure Draft, you need:</span></span>
  - <span data-ttu-id="86e0d-141">Název služby Azure Container Registry (v tomto příkladu `draft`).</span><span class="sxs-lookup"><span data-stu-id="86e0d-141">your Azure Container Registry name (in this example, `draft`)</span></span>
  - <span data-ttu-id="86e0d-142">Klíč registru nebo heslo získané příkazem `az acr credential show -n <registry name> --output tsv --query "passwords[0].value"`.</span><span class="sxs-lookup"><span data-stu-id="86e0d-142">your registry key, or password, from `az acr credential show -n <registry name> --output tsv --query "passwords[0].value"`.</span></span>
  - <span data-ttu-id="86e0d-143">Kořenovou doménu nasazení, u které jste nakonfigurovali mapování na externí IP adresu příchozího přenosu dat Kubernetes (v tomto příkladu `squillace.io`).</span><span class="sxs-lookup"><span data-stu-id="86e0d-143">the root deployment domain that you have configured to map to the Kubernetes ingress external IP address (here, `squillace.io`)</span></span>

  <span data-ttu-id="86e0d-144">Zavolejte `draft init` a proces konfigurace vás vyzve k zadání výše uvedených hodnot.</span><span class="sxs-lookup"><span data-stu-id="86e0d-144">Call `draft init` and the configuration process prompts you for the values above.</span></span> <span data-ttu-id="86e0d-145">Proces při prvním spuštění vypadá asi takto:</span><span class="sxs-lookup"><span data-stu-id="86e0d-145">The process looks something like the following the first time you run it.</span></span>
 ```bash
    $ draft init
    Creating pack ruby...
    Creating pack node...
    Creating pack gradle...
    Creating pack maven...
    Creating pack php...
    Creating pack python...
    Creating pack dotnetcore...
    Creating pack golang...
    $DRAFT_HOME has been configured at /Users/ralphsquillace/.draft.

    In order to install Draft, we need a bit more information...

    1. Enter your Docker registry URL (e.g. docker.io, quay.io, myregistry.azurecr.io): draft.azurecr.io
    2. Enter your username: draft
    3. Enter your password:
    4. Enter your org where Draft will push images [draft]: draft
    5. Enter your top-level domain for ingress (e.g. draft.example.com): squillace.io
    Draft has been installed into your Kubernetes Cluster.
    Happy Sailing!
    ```

<span data-ttu-id="86e0d-146">Nyní jste připraveni nasadit aplikaci.</span><span class="sxs-lookup"><span data-stu-id="86e0d-146">Now you're ready to deploy an application.</span></span>


## <a name="build-and-deploy-an-application"></a><span data-ttu-id="86e0d-147">Sestavení a nasazení aplikace</span><span class="sxs-lookup"><span data-stu-id="86e0d-147">Build and deploy an application</span></span>

<span data-ttu-id="86e0d-148">V úložišti Draft najdete [šest jednoduchých ukázkových aplikací](https://github.com/Azure/draft/tree/master/examples).</span><span class="sxs-lookup"><span data-stu-id="86e0d-148">In the Draft repo are [six simple example applications](https://github.com/Azure/draft/tree/master/examples).</span></span> <span data-ttu-id="86e0d-149">Naklonujte úložiště a použijte [příklad v Pythonu](https://github.com/Azure/draft/tree/master/examples/python).</span><span class="sxs-lookup"><span data-stu-id="86e0d-149">Clone the repo and let's use the [Python example](https://github.com/Azure/draft/tree/master/examples/python).</span></span> <span data-ttu-id="86e0d-150">Přejděte do adresáře examples/Python a zadáním příkazu `draft create` sestavte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="86e0d-150">Change into the examples/Python directory, and type `draft create` to build the application.</span></span> <span data-ttu-id="86e0d-151">Mělo by to vypadat asi jako v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="86e0d-151">It should look like the following example.</span></span>
```bash
$ draft create
--> Python app detected
--> Ready to sail
```

<span data-ttu-id="86e0d-152">Výstup zahrnujte soubor Dockerfile a digram Helmu.</span><span class="sxs-lookup"><span data-stu-id="86e0d-152">The output includes a Dockerfile and a Helm chart.</span></span> <span data-ttu-id="86e0d-153">K sestavení a nasazení stačí zadat `draft up`.</span><span class="sxs-lookup"><span data-stu-id="86e0d-153">To build and deploy, you just type `draft up`.</span></span> <span data-ttu-id="86e0d-154">Výstup je rozsáhlý, ale začíná podobně jako následující příklad.</span><span class="sxs-lookup"><span data-stu-id="86e0d-154">The output is extensive, but begins like the following example.</span></span>
```bash
$ draft up
--> Building Dockerfile
Step 1 : FROM python:onbuild
onbuild: Pulling from library/python
10a267c67f42: Pulling fs layer
fb5937da9414: Pulling fs layer
9021b2326a1e: Pulling fs layer
dbed9b09434e: Pulling fs layer
ea8a37f15161: Pulling fs layer
<snip>
```

<span data-ttu-id="86e0d-155">A v případě úspěšného provedení končí podobně jako následující příklad.</span><span class="sxs-lookup"><span data-stu-id="86e0d-155">and when successful ends with something similar to the following example.</span></span>
```bash
ab68189731eb: Pushed
53c0ab0341bee12d01be3d3c192fbd63562af7f1: digest: sha256:bb0450ec37acf67ed461c1512ef21f58a500ff9326ce3ec623ce1e4427df9765 size: 2841
--> Deploying to Kubernetes
--> Status: DEPLOYED
--> Notes:

  http://gangly-bronco.squillace.io to access your application

Watching local files for changes...
```

<span data-ttu-id="86e0d-156">Bez ohledu na název vašeho diagramu teď můžete spustit příkaz `curl http://gangly-bronco.squillace.io` a obdržet odpověď `Hello World!`.</span><span class="sxs-lookup"><span data-stu-id="86e0d-156">Whatever your chart's name is, you can now `curl http://gangly-bronco.squillace.io` to receive the reply, `Hello World!`.</span></span>

## <a name="next-steps"></a><span data-ttu-id="86e0d-157">Další kroky</span><span class="sxs-lookup"><span data-stu-id="86e0d-157">Next steps</span></span>

<span data-ttu-id="86e0d-158">Když teď máte cluster ACS Kubernetes, můžete prozkoumat používání služby [Azure Container Registry](../../container-registry/container-registry-intro.md), abyste mohli vytvářet další a jiná nasazení tohoto scénáře.</span><span class="sxs-lookup"><span data-stu-id="86e0d-158">Now that you have an ACS Kubernetes cluster, you can investigate using [Azure Container Registry](../../container-registry/container-registry-intro.md) to create more and different deployments of this scenario.</span></span> <span data-ttu-id="86e0d-159">Můžete například vytvořit sadu záznamů DNS domény draft._základní_doména.doména_nejvyšší_úrovně_, která bude pro specifická nasazení ACS řídit vše pro subdoménu na nižší úrovni.</span><span class="sxs-lookup"><span data-stu-id="86e0d-159">For example, you can create a draft._basedomain.toplevel_ domain DNS record-set that controls things off of a deeper subdomain for specific ACS deployments.</span></span>






