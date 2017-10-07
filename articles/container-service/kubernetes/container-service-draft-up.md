---
title: aaaUse koncept s Azure Container Service a Azure kontejneru registru | Microsoft Docs
description: "Vytvoření clusteru služby ACS Kubernetes a registru kontejner Azure toocreate svoji první aplikaci v Azure pomocí konceptu."
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
ms.openlocfilehash: f5e21cda01e5e8452bf86a5c8fa458904d89f451
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-draft-with-azure-container-service-and-azure-container-registry-toobuild-and-deploy-an-application-tookubernetes"></a><span data-ttu-id="2373a-104">Použít koncept s Azure Container Service a Azure kontejneru registru toobuild a nasazení tooKubernetes aplikace</span><span class="sxs-lookup"><span data-stu-id="2373a-104">Use Draft with Azure Container Service and Azure Container Registry toobuild and deploy an application tooKubernetes</span></span>

<span data-ttu-id="2373a-105">[Koncept](https://aka.ms/draft) je nový nástroj open source, který umožňuje snadno toodevelop aplikace založené na kontejner a nasadit je tooKubernetes clusterů bez znalost mnohem o Docker a Kubernetes--nebo i jejich instalaci.</span><span class="sxs-lookup"><span data-stu-id="2373a-105">[Draft](https://aka.ms/draft) is a new open-source tool that makes it easy toodevelop container-based applications and deploy them tooKubernetes clusters without knowing much about Docker and Kubernetes -- or even installing them.</span></span> <span data-ttu-id="2373a-106">Pomocí nástroje, například koncept umožňují vám a vaší týmy zaměřit vytváření aplikace hello s Kubernetes, není platícího tolik tooinfrastructure pozornost.</span><span class="sxs-lookup"><span data-stu-id="2373a-106">Using tools like Draft let you and your teams focus on building hello application with Kubernetes, not paying as much attention tooinfrastructure.</span></span>

<span data-ttu-id="2373a-107">Draft můžete používat s libovolným registrem imagí Dockeru a jakýmkoli clusterem Kubernetes, a to i místně.</span><span class="sxs-lookup"><span data-stu-id="2373a-107">You can use Draft with any Docker image registry and any Kubernetes cluster, including locally.</span></span> <span data-ttu-id="2373a-108">Tento kurz ukazuje, jak toouse ACS s Kubernetes, ACR a Azure DNS toocreate vývojář za provozu CI/CD kanálu pomocí konceptu.</span><span class="sxs-lookup"><span data-stu-id="2373a-108">This tutorial shows how toouse ACS with Kubernetes, ACR, and Azure DNS toocreate a live CI/CD developer pipeline using Draft.</span></span>


## <a name="create-an-azure-container-registry"></a><span data-ttu-id="2373a-109">Vytvoření služby Azure Container Registry</span><span class="sxs-lookup"><span data-stu-id="2373a-109">Create an Azure Container Registry</span></span>
<span data-ttu-id="2373a-110">Můžete snadno [vytvořit nové registru kontejner Azure](../../container-registry/container-registry-get-started-azure-cli.md), ale hello kroky jsou následující:</span><span class="sxs-lookup"><span data-stu-id="2373a-110">You can easily [create a new Azure Container Registry](../../container-registry/container-registry-get-started-azure-cli.md), but hello steps are as follows:</span></span>

1. <span data-ttu-id="2373a-111">Vytvoření clusteru ACR registru a hello Kubernetes toomanage skupiny prostředků Azure v rámci služby ACS.</span><span class="sxs-lookup"><span data-stu-id="2373a-111">Create a Azure resource group toomanage your ACR registry and hello Kubernetes cluster in ACS.</span></span>
      ```azurecli
      az group create --name draft --location eastus
      ```

2. <span data-ttu-id="2373a-112">Vytvořte registr imagí Azure Container Registry pomocí příkazu [az acr create](/cli/azure/acr#create).</span><span class="sxs-lookup"><span data-stu-id="2373a-112">Create an ACR image registry using [az acr create](/cli/azure/acr#create)</span></span>
      ```azurecli
      az acr create -g draft -n draftacs --sku Basic --admin-enabled true -l eastus
      ```


## <a name="create-an-azure-container-service-with-kubernetes"></a><span data-ttu-id="2373a-113">Vytvoření služby Azure Container Service pomocí Kubernetes</span><span class="sxs-lookup"><span data-stu-id="2373a-113">Create an Azure Container Service with Kubernetes</span></span>

<span data-ttu-id="2373a-114">Nyní jste připravené toouse [vytvořit acs az](/cli/azure/acs#create) toocreate ACS clusteru pomocí Kubernetes jako hello `--orchestrator-type` hodnotu.</span><span class="sxs-lookup"><span data-stu-id="2373a-114">Now you're ready toouse [az acs create](/cli/azure/acs#create) toocreate an ACS cluster using Kubernetes as hello `--orchestrator-type` value.</span></span>
```azurecli
az acs create --resource-group draft --name draft-kube-acs --dns-prefix draft-cluster --orchestrator-type kubernetes
```

> [!NOTE]
> <span data-ttu-id="2373a-115">Protože Kubernetes není hello výchozí orchestrator typ, ujistěte se, použijte hello `--orchestrator-type kubernetes` přepínače.</span><span class="sxs-lookup"><span data-stu-id="2373a-115">Because Kubernetes is not hello default orchestrator type, be sure you use hello `--orchestrator-type kubernetes` switch.</span></span>

<span data-ttu-id="2373a-116">výstup Hello při úspěšné vypadá podobně jako následující toohello.</span><span class="sxs-lookup"><span data-stu-id="2373a-116">hello output when successful looks similar toohello following.</span></span>

```json
waiting for AAD role toopropagate.done
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

<span data-ttu-id="2373a-117">Teď, když máte cluster, můžete importovat hello pověření pomocí hello [az acs kubernetes get pověření](/cli/azure/acs/kubernetes#get-credentials) příkaz.</span><span class="sxs-lookup"><span data-stu-id="2373a-117">Now that you have a cluster, you can import hello credentials by using hello [az acs kubernetes get-credentials](/cli/azure/acs/kubernetes#get-credentials) command.</span></span> <span data-ttu-id="2373a-118">Nyní máte místní konfigurační soubor pro váš cluster, který je co Helm a koncept potřebovat tooget pracovní úkoly.</span><span class="sxs-lookup"><span data-stu-id="2373a-118">Now you have a local configuration file for your cluster, which is what Helm and Draft need tooget their work done.</span></span>

## <a name="install-and-configure-draft"></a><span data-ttu-id="2373a-119">Instalace a konfigurace nástroje Draft</span><span class="sxs-lookup"><span data-stu-id="2373a-119">Install and configure draft</span></span>
<span data-ttu-id="2373a-120">Pokyny k instalaci Hello koncept jsou v hello [koncept úložiště](https://github.com/Azure/draft/blob/master/docs/install.md).</span><span class="sxs-lookup"><span data-stu-id="2373a-120">hello installation instructions for Draft are in hello [Draft repository](https://github.com/Azure/draft/blob/master/docs/install.md).</span></span> <span data-ttu-id="2373a-121">Jsou poměrně jednoduché, ale vyžadují určitou konfiguraci, protože závisí na [Helm](https://aka.ms/helm) toocreate a nasazení Helm graf do clusteru Kubernetes hello.</span><span class="sxs-lookup"><span data-stu-id="2373a-121">They are relatively simple, but do require some configuration, as it depends on [Helm](https://aka.ms/helm) toocreate and deploy a Helm chart into hello Kubernetes cluster.</span></span>

1. <span data-ttu-id="2373a-122">[Stáhněte a nainstalujte Helm](https://aka.ms/helm#install).</span><span class="sxs-lookup"><span data-stu-id="2373a-122">[Download and install Helm](https://aka.ms/helm#install).</span></span>
2. <span data-ttu-id="2373a-123">Použít Helm toosearch pro a nainstalovat `stable/traefik`a příjem příchozích dat řadiče tooenable příchozí požadavky pro vaše buildy.</span><span class="sxs-lookup"><span data-stu-id="2373a-123">Use Helm toosearch for and install `stable/traefik`, and ingress controller tooenable inbound requests for your builds.</span></span>
    ```bash
    $ helm search traefik
    NAME            VERSION DESCRIPTION
    stable/traefik  1.3.0   A Traefik based Kubernetes ingress controller w...

    $ helm install stable/traefik --name ingress
    ```
    <span data-ttu-id="2373a-124">Nyní nastavovat sledování na hello `ingress` řadič toocapture hello externí IP hodnotu, pokud je nasazená.</span><span class="sxs-lookup"><span data-stu-id="2373a-124">Now set a watch on hello `ingress` controller toocapture hello external IP value when it is deployed.</span></span> <span data-ttu-id="2373a-125">Tuto IP adresu budou hello jeden [namapované domény nasazení tooyour](#wire-up-deployment-domain) v další části hello.</span><span class="sxs-lookup"><span data-stu-id="2373a-125">This IP address will be hello one [mapped tooyour deployment domain](#wire-up-deployment-domain) in hello next section.</span></span>

    ```bash
    kubectl get svc -w
    NAME                          CLUSTER-IP     EXTERNAL-IP     PORT(S)                      AGE
    ingress-traefik               10.0.248.104   13.64.108.240   80:31046/TCP,443:32556/TCP   1h
    kubernetes                    10.0.0.1       <none>          443/TCP                      7h
    ```

    <span data-ttu-id="2373a-126">V takovém případě hello externí IP pro domény nasazení hello je `13.64.108.240`.</span><span class="sxs-lookup"><span data-stu-id="2373a-126">In this case, hello external IP for hello deployment domain is `13.64.108.240`.</span></span> <span data-ttu-id="2373a-127">Nyní můžete namapovat IP toothat domény.</span><span class="sxs-lookup"><span data-stu-id="2373a-127">Now you can map your domain toothat IP.</span></span>

## <a name="wire-up-deployment-domain"></a><span data-ttu-id="2373a-128">Napojení domény nasazení</span><span class="sxs-lookup"><span data-stu-id="2373a-128">Wire up deployment domain</span></span>

<span data-ttu-id="2373a-129">Draft vytvoří vydanou verzi pro každý diagram Helmu, který vytvoří – pro každou aplikaci, na které pracujete.</span><span class="sxs-lookup"><span data-stu-id="2373a-129">Draft creates a release for each Helm chart it creates -- each application you are working on.</span></span> <span data-ttu-id="2373a-130">Každé z nich získá vygenerovaný název, který je používán jako koncept _subdomény_ nad hello kořenové _nasazení domény_ , kterou řídíte.</span><span class="sxs-lookup"><span data-stu-id="2373a-130">Each one gets a generated name that is used by draft as a _subdomain_ on top of hello root _deployment domain_ that you control.</span></span> <span data-ttu-id="2373a-131">(V tomto příkladu používáme `squillace.io` jako hello nasazení domény.) tooenable toto chování subdomény, musíte vytvořit záznam pro `'*'` v záznamy DNS pro vaši doménu nasazení, tak, aby každý generované subdomény je směrované toohello Kubernetes příjem příchozích dat řadič clusteru.</span><span class="sxs-lookup"><span data-stu-id="2373a-131">(In this example, we use `squillace.io` as hello deployment domain.) tooenable this subdomain behavior, you must create an A record for `'*'` in your DNS entries for your deployment domain, so that each generated subdomain is routed toohello Kubernetes cluster's ingress controller.</span></span>

<span data-ttu-id="2373a-132">Vlastní zprostředkovatel domény má své vlastní servery DNS tooassign způsobem; příliš[delegovat tooAzure nameservers vaší domény DNS](../../dns/dns-delegate-domain-azure-dns.md), proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="2373a-132">Your own domain provider has their own way tooassign DNS servers; too[delegate your domain nameservers tooAzure DNS](../../dns/dns-delegate-domain-azure-dns.md), you take hello following steps:</span></span>

1. <span data-ttu-id="2373a-133">Vytvořte skupinu prostředků pro vaši zónu DNS.</span><span class="sxs-lookup"><span data-stu-id="2373a-133">Create a resource group for your zone.</span></span>
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

2. <span data-ttu-id="2373a-134">Vytvořte zónu DNS pro vaši doménu.</span><span class="sxs-lookup"><span data-stu-id="2373a-134">Create a DNS zone for your domain.</span></span>
<span data-ttu-id="2373a-135">Použití hello [vytvoření zóny dns sítě az](/cli/azure/network/dns/zone#create) příkaz tooobtain hello nameservers toodelegate DNS řízení tooAzure DNS pro vaši doménu.</span><span class="sxs-lookup"><span data-stu-id="2373a-135">Use hello [az network dns zone create](/cli/azure/network/dns/zone#create) command tooobtain hello nameservers toodelegate DNS control tooAzure DNS for your domain.</span></span>
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
3. <span data-ttu-id="2373a-136">Přidejte hello servery, které jsou uvedeny toohello domény zprostředkovatele pro vaši doménu nasazení, která umožňuje vám toouse Azure DNS toorepoint doménu tak, jak chcete.</span><span class="sxs-lookup"><span data-stu-id="2373a-136">Add hello DNS servers you are given toohello domain provider for your deployment domain, which enables you toouse Azure DNS toorepoint your domain as you want.</span></span>
4. <span data-ttu-id="2373a-137">Vytvořit položku A sadu záznamů pro vaše nasazení domény mapování toohello `ingress` IP z kroku 2 hello předchozí části.</span><span class="sxs-lookup"><span data-stu-id="2373a-137">Create an A record-set entry for your deployment domain mapping toohello `ingress` IP from step 2 of hello previous section.</span></span>
    ```azurecli
    az network dns record-set a add-record --ipv4-address 13.64.108.240 --record-set-name '*' -g squillace.io -z squillace.io
    ```
<span data-ttu-id="2373a-138">výstup Hello vypadá podobně jako:</span><span class="sxs-lookup"><span data-stu-id="2373a-138">hello output looks something like:</span></span>
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

5. <span data-ttu-id="2373a-139">Nakonfigurujte koncept toouse registr a vytvořte subdomény pro každý Helm graf, který vytváří.</span><span class="sxs-lookup"><span data-stu-id="2373a-139">Configure Draft toouse your registry and create subdomains for each Helm chart it creates.</span></span> <span data-ttu-id="2373a-140">tooconfigure koncept, budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="2373a-140">tooconfigure Draft, you need:</span></span>
  - <span data-ttu-id="2373a-141">Název služby Azure Container Registry (v tomto příkladu `draft`).</span><span class="sxs-lookup"><span data-stu-id="2373a-141">your Azure Container Registry name (in this example, `draft`)</span></span>
  - <span data-ttu-id="2373a-142">Klíč registru nebo heslo získané příkazem `az acr credential show -n <registry name> --output tsv --query "passwords[0].value"`.</span><span class="sxs-lookup"><span data-stu-id="2373a-142">your registry key, or password, from `az acr credential show -n <registry name> --output tsv --query "passwords[0].value"`.</span></span>
  - <span data-ttu-id="2373a-143">Hello kořenové nasazení domény, které jste nakonfigurovali toomap toohello Kubernetes příchozí externí IP adresu (zde `squillace.io`)</span><span class="sxs-lookup"><span data-stu-id="2373a-143">hello root deployment domain that you have configured toomap toohello Kubernetes ingress external IP address (here, `squillace.io`)</span></span>

  <span data-ttu-id="2373a-144">Volání `draft init` a proces konfigurace hello vás vyzve k zadání hodnoty hello výše.</span><span class="sxs-lookup"><span data-stu-id="2373a-144">Call `draft init` and hello configuration process prompts you for hello values above.</span></span> <span data-ttu-id="2373a-145">proces Hello něco vypadá hello následující hello poprvé, můžete ji spustit.</span><span class="sxs-lookup"><span data-stu-id="2373a-145">hello process looks something like hello following hello first time you run it.</span></span>
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

    In order tooinstall Draft, we need a bit more information...

    1. Enter your Docker registry URL (e.g. docker.io, quay.io, myregistry.azurecr.io): draft.azurecr.io
    2. Enter your username: draft
    3. Enter your password:
    4. Enter your org where Draft will push images [draft]: draft
    5. Enter your top-level domain for ingress (e.g. draft.example.com): squillace.io
    Draft has been installed into your Kubernetes Cluster.
    Happy Sailing!
    ```

<span data-ttu-id="2373a-146">Nyní jste připravené toodeploy aplikace.</span><span class="sxs-lookup"><span data-stu-id="2373a-146">Now you're ready toodeploy an application.</span></span>


## <a name="build-and-deploy-an-application"></a><span data-ttu-id="2373a-147">Sestavení a nasazení aplikace</span><span class="sxs-lookup"><span data-stu-id="2373a-147">Build and deploy an application</span></span>

<span data-ttu-id="2373a-148">V úložišti koncept hello jsou [šesti jednoduchý příklad aplikace](https://github.com/Azure/draft/tree/master/examples).</span><span class="sxs-lookup"><span data-stu-id="2373a-148">In hello Draft repo are [six simple example applications](https://github.com/Azure/draft/tree/master/examples).</span></span> <span data-ttu-id="2373a-149">Klonování úložiště hello a použijeme hello [Python příklad](https://github.com/Azure/draft/tree/master/examples/python).</span><span class="sxs-lookup"><span data-stu-id="2373a-149">Clone hello repo and let's use hello [Python example](https://github.com/Azure/draft/tree/master/examples/python).</span></span> <span data-ttu-id="2373a-150">Přejděte do adresáře hello příklady nebo Python a typ `draft create` toobuild hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="2373a-150">Change into hello examples/Python directory, and type `draft create` toobuild hello application.</span></span> <span data-ttu-id="2373a-151">By měl vypadat jako následující příklad hello.</span><span class="sxs-lookup"><span data-stu-id="2373a-151">It should look like hello following example.</span></span>
```bash
$ draft create
--> Python app detected
--> Ready toosail
```

<span data-ttu-id="2373a-152">výstup Hello zahrnuje soubor Docker a Helm grafu.</span><span class="sxs-lookup"><span data-stu-id="2373a-152">hello output includes a Dockerfile and a Helm chart.</span></span> <span data-ttu-id="2373a-153">toobuild a nasadit, stačí zadat `draft up`.</span><span class="sxs-lookup"><span data-stu-id="2373a-153">toobuild and deploy, you just type `draft up`.</span></span> <span data-ttu-id="2373a-154">výstup Hello je rozsáhlé, ale zahájí jako hello následující ukázka.</span><span class="sxs-lookup"><span data-stu-id="2373a-154">hello output is extensive, but begins like hello following example.</span></span>
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

<span data-ttu-id="2373a-155">a kdy úspěšné elementů end s něco podobné toohello následující ukázka.</span><span class="sxs-lookup"><span data-stu-id="2373a-155">and when successful ends with something similar toohello following example.</span></span>
```bash
ab68189731eb: Pushed
53c0ab0341bee12d01be3d3c192fbd63562af7f1: digest: sha256:bb0450ec37acf67ed461c1512ef21f58a500ff9326ce3ec623ce1e4427df9765 size: 2841
--> Deploying tooKubernetes
--> Status: DEPLOYED
--> Notes:

  http://gangly-bronco.squillace.io tooaccess your application

Watching local files for changes...
```

<span data-ttu-id="2373a-156">Ať je název grafu, můžete nyní `curl http://gangly-bronco.squillace.io` tooreceive hello odpovědět, `Hello World!`.</span><span class="sxs-lookup"><span data-stu-id="2373a-156">Whatever your chart's name is, you can now `curl http://gangly-bronco.squillace.io` tooreceive hello reply, `Hello World!`.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2373a-157">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2373a-157">Next steps</span></span>

<span data-ttu-id="2373a-158">Teď, když máte cluster služby ACS Kubernetes, můžete prozkoumat pomocí [registru kontejner Azure](../../container-registry/container-registry-intro.md) toocreate další a jiné nasazení tohoto scénáře.</span><span class="sxs-lookup"><span data-stu-id="2373a-158">Now that you have an ACS Kubernetes cluster, you can investigate using [Azure Container Registry](../../container-registry/container-registry-intro.md) toocreate more and different deployments of this scenario.</span></span> <span data-ttu-id="2373a-159">Můžete například vytvořit sadu záznamů DNS domény draft._základní_doména.doména_nejvyšší_úrovně_, která bude pro specifická nasazení ACS řídit vše pro subdoménu na nižší úrovni.</span><span class="sxs-lookup"><span data-stu-id="2373a-159">For example, you can create a draft._basedomain.toplevel_ domain DNS record-set that controls things off of a deeper subdomain for specific ACS deployments.</span></span>






