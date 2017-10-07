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
# <a name="use-draft-with-azure-container-service-and-azure-container-registry-toobuild-and-deploy-an-application-tookubernetes"></a>Použít koncept s Azure Container Service a Azure kontejneru registru toobuild a nasazení tooKubernetes aplikace

[Koncept](https://aka.ms/draft) je nový nástroj open source, který umožňuje snadno toodevelop aplikace založené na kontejner a nasadit je tooKubernetes clusterů bez znalost mnohem o Docker a Kubernetes--nebo i jejich instalaci. Pomocí nástroje, například koncept umožňují vám a vaší týmy zaměřit vytváření aplikace hello s Kubernetes, není platícího tolik tooinfrastructure pozornost.

Draft můžete používat s libovolným registrem imagí Dockeru a jakýmkoli clusterem Kubernetes, a to i místně. Tento kurz ukazuje, jak toouse ACS s Kubernetes, ACR a Azure DNS toocreate vývojář za provozu CI/CD kanálu pomocí konceptu.


## <a name="create-an-azure-container-registry"></a>Vytvoření služby Azure Container Registry
Můžete snadno [vytvořit nové registru kontejner Azure](../../container-registry/container-registry-get-started-azure-cli.md), ale hello kroky jsou následující:

1. Vytvoření clusteru ACR registru a hello Kubernetes toomanage skupiny prostředků Azure v rámci služby ACS.
      ```azurecli
      az group create --name draft --location eastus
      ```

2. Vytvořte registr imagí Azure Container Registry pomocí příkazu [az acr create](/cli/azure/acr#create).
      ```azurecli
      az acr create -g draft -n draftacs --sku Basic --admin-enabled true -l eastus
      ```


## <a name="create-an-azure-container-service-with-kubernetes"></a>Vytvoření služby Azure Container Service pomocí Kubernetes

Nyní jste připravené toouse [vytvořit acs az](/cli/azure/acs#create) toocreate ACS clusteru pomocí Kubernetes jako hello `--orchestrator-type` hodnotu.
```azurecli
az acs create --resource-group draft --name draft-kube-acs --dns-prefix draft-cluster --orchestrator-type kubernetes
```

> [!NOTE]
> Protože Kubernetes není hello výchozí orchestrator typ, ujistěte se, použijte hello `--orchestrator-type kubernetes` přepínače.

výstup Hello při úspěšné vypadá podobně jako následující toohello.

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

Teď, když máte cluster, můžete importovat hello pověření pomocí hello [az acs kubernetes get pověření](/cli/azure/acs/kubernetes#get-credentials) příkaz. Nyní máte místní konfigurační soubor pro váš cluster, který je co Helm a koncept potřebovat tooget pracovní úkoly.

## <a name="install-and-configure-draft"></a>Instalace a konfigurace nástroje Draft
Pokyny k instalaci Hello koncept jsou v hello [koncept úložiště](https://github.com/Azure/draft/blob/master/docs/install.md). Jsou poměrně jednoduché, ale vyžadují určitou konfiguraci, protože závisí na [Helm](https://aka.ms/helm) toocreate a nasazení Helm graf do clusteru Kubernetes hello.

1. [Stáhněte a nainstalujte Helm](https://aka.ms/helm#install).
2. Použít Helm toosearch pro a nainstalovat `stable/traefik`a příjem příchozích dat řadiče tooenable příchozí požadavky pro vaše buildy.
    ```bash
    $ helm search traefik
    NAME            VERSION DESCRIPTION
    stable/traefik  1.3.0   A Traefik based Kubernetes ingress controller w...

    $ helm install stable/traefik --name ingress
    ```
    Nyní nastavovat sledování na hello `ingress` řadič toocapture hello externí IP hodnotu, pokud je nasazená. Tuto IP adresu budou hello jeden [namapované domény nasazení tooyour](#wire-up-deployment-domain) v další části hello.

    ```bash
    kubectl get svc -w
    NAME                          CLUSTER-IP     EXTERNAL-IP     PORT(S)                      AGE
    ingress-traefik               10.0.248.104   13.64.108.240   80:31046/TCP,443:32556/TCP   1h
    kubernetes                    10.0.0.1       <none>          443/TCP                      7h
    ```

    V takovém případě hello externí IP pro domény nasazení hello je `13.64.108.240`. Nyní můžete namapovat IP toothat domény.

## <a name="wire-up-deployment-domain"></a>Napojení domény nasazení

Draft vytvoří vydanou verzi pro každý diagram Helmu, který vytvoří – pro každou aplikaci, na které pracujete. Každé z nich získá vygenerovaný název, který je používán jako koncept _subdomény_ nad hello kořenové _nasazení domény_ , kterou řídíte. (V tomto příkladu používáme `squillace.io` jako hello nasazení domény.) tooenable toto chování subdomény, musíte vytvořit záznam pro `'*'` v záznamy DNS pro vaši doménu nasazení, tak, aby každý generované subdomény je směrované toohello Kubernetes příjem příchozích dat řadič clusteru.

Vlastní zprostředkovatel domény má své vlastní servery DNS tooassign způsobem; příliš[delegovat tooAzure nameservers vaší domény DNS](../../dns/dns-delegate-domain-azure-dns.md), proveďte následující kroky hello:

1. Vytvořte skupinu prostředků pro vaši zónu DNS.
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

2. Vytvořte zónu DNS pro vaši doménu.
Použití hello [vytvoření zóny dns sítě az](/cli/azure/network/dns/zone#create) příkaz tooobtain hello nameservers toodelegate DNS řízení tooAzure DNS pro vaši doménu.
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
3. Přidejte hello servery, které jsou uvedeny toohello domény zprostředkovatele pro vaši doménu nasazení, která umožňuje vám toouse Azure DNS toorepoint doménu tak, jak chcete.
4. Vytvořit položku A sadu záznamů pro vaše nasazení domény mapování toohello `ingress` IP z kroku 2 hello předchozí části.
    ```azurecli
    az network dns record-set a add-record --ipv4-address 13.64.108.240 --record-set-name '*' -g squillace.io -z squillace.io
    ```
výstup Hello vypadá podobně jako:
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

5. Nakonfigurujte koncept toouse registr a vytvořte subdomény pro každý Helm graf, který vytváří. tooconfigure koncept, budete potřebovat:
  - Název služby Azure Container Registry (v tomto příkladu `draft`).
  - Klíč registru nebo heslo získané příkazem `az acr credential show -n <registry name> --output tsv --query "passwords[0].value"`.
  - Hello kořenové nasazení domény, které jste nakonfigurovali toomap toohello Kubernetes příchozí externí IP adresu (zde `squillace.io`)

  Volání `draft init` a proces konfigurace hello vás vyzve k zadání hodnoty hello výše. proces Hello něco vypadá hello následující hello poprvé, můžete ji spustit.
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

Nyní jste připravené toodeploy aplikace.


## <a name="build-and-deploy-an-application"></a>Sestavení a nasazení aplikace

V úložišti koncept hello jsou [šesti jednoduchý příklad aplikace](https://github.com/Azure/draft/tree/master/examples). Klonování úložiště hello a použijeme hello [Python příklad](https://github.com/Azure/draft/tree/master/examples/python). Přejděte do adresáře hello příklady nebo Python a typ `draft create` toobuild hello aplikace. By měl vypadat jako následující příklad hello.
```bash
$ draft create
--> Python app detected
--> Ready toosail
```

výstup Hello zahrnuje soubor Docker a Helm grafu. toobuild a nasadit, stačí zadat `draft up`. výstup Hello je rozsáhlé, ale zahájí jako hello následující ukázka.
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

a kdy úspěšné elementů end s něco podobné toohello následující ukázka.
```bash
ab68189731eb: Pushed
53c0ab0341bee12d01be3d3c192fbd63562af7f1: digest: sha256:bb0450ec37acf67ed461c1512ef21f58a500ff9326ce3ec623ce1e4427df9765 size: 2841
--> Deploying tooKubernetes
--> Status: DEPLOYED
--> Notes:

  http://gangly-bronco.squillace.io tooaccess your application

Watching local files for changes...
```

Ať je název grafu, můžete nyní `curl http://gangly-bronco.squillace.io` tooreceive hello odpovědět, `Hello World!`.

## <a name="next-steps"></a>Další kroky

Teď, když máte cluster služby ACS Kubernetes, můžete prozkoumat pomocí [registru kontejner Azure](../../container-registry/container-registry-intro.md) toocreate další a jiné nasazení tohoto scénáře. Můžete například vytvořit sadu záznamů DNS domény draft._základní_doména.doména_nejvyšší_úrovně_, která bude pro specifická nasazení ACS řídit vše pro subdoménu na nižší úrovni.






