---
title: "aaaAzure kontejner instancí - skupina více kontejneru | Dokumentace Azure"
description: "Azure kontejner instancí - skupina více kontejneru"
services: container-instances
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/26/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 976f578cd2a9bf7f05ab97f24662139bb72062ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-container-group"></a><span data-ttu-id="90737-103">Nasazení kontejneru skupiny</span><span class="sxs-lookup"><span data-stu-id="90737-103">Deploy a container group</span></span>

<span data-ttu-id="90737-104">Azure instancí kontejnerů podporu hello nasazení několika kontejnerů do jednoho hostitele pomocí *skupina kontejneru*.</span><span class="sxs-lookup"><span data-stu-id="90737-104">Azure Container Instances support hello deployment of multiple containers onto a single host using a *container group*.</span></span> <span data-ttu-id="90737-105">To je užitečné při sestavování něho aplikace pro protokolování, sledování nebo jakoukoli jinou konfiguraci, které služba potřebuje druhý připojené procesu.</span><span class="sxs-lookup"><span data-stu-id="90737-105">This is useful when building an application sidecar for logging, monitoring, or any other configuration where a service needs a second attached process.</span></span> 

<span data-ttu-id="90737-106">Tento dokument vás provede spuštění jednoduché něho více kontejneru konfigurace pomocí šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="90737-106">This document walks through running a simple multi-container sidecar configuration using an Azure Resource Manager template.</span></span>

## <a name="configure-hello-template"></a><span data-ttu-id="90737-107">Konfigurace šablony hello</span><span class="sxs-lookup"><span data-stu-id="90737-107">Configure hello template</span></span>

<span data-ttu-id="90737-108">Vytvořte soubor s názvem `azuredeploy.json` a kopírování hello následující json do ní.</span><span class="sxs-lookup"><span data-stu-id="90737-108">Create a file named `azuredeploy.json` and copy hello following json into it.</span></span> 

<span data-ttu-id="90737-109">V této ukázce je definována kontejner skupiny s dvěma kontejnery a veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="90737-109">In this sample, a container group with two containers and a public IP address is defined.</span></span> <span data-ttu-id="90737-110">první kontejneru Hello hello skupiny spustí internetovou přístupných aplikaci.</span><span class="sxs-lookup"><span data-stu-id="90737-110">hello first container of hello group runs an internet facing application.</span></span> <span data-ttu-id="90737-111">druhý kontejneru Hello hello něho díky hlavní webové aplikace HTTP žádost toohello prostřednictvím místní sítě hello skupiny.</span><span class="sxs-lookup"><span data-stu-id="90737-111">hello second container, hello sidecar, makes an HTTP request toohello main web application via hello group's local network.</span></span> 

<span data-ttu-id="90737-112">Tento příklad něho může být rozšířené tootrigger výstrahu, pokud dostali kódu odpovědi HTTP než 200 OK.</span><span class="sxs-lookup"><span data-stu-id="90737-112">This sidecar example could be expanded tootrigger an alert if it received an HTTP response code other than 200 OK.</span></span> 

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
  },
  "variables": {
    "container1name": "aci-tutorial-app",
    "container1image": "microsoft/aci-helloworld:latest",
    "container2name": "aci-tutorial-sidecar",    
    "container2image": "microsoft/aci-tutorial-sidecar"
  },
    "resources": [
      {
        "name": "myContainerGroup",
        "type": "Microsoft.ContainerInstance/containerGroups",
        "apiVersion": "2017-08-01-preview",
        "location": "[resourceGroup().location]",
        "properties": {
          "containers": [
            {
              "name": "[variables('container1name')]",
              "properties": {
                "image": "[variables('container1image')]",
                "resources": {
                  "requests": {
                    "cpu": 1,
                    "memoryInGb": 1.5
                    }
                },
                "ports": [
                  {
                    "port": 80
                  }
                ]
              }
            },
            {
              "name": "[variables('container2name')]",
              "properties": {
                "image": "[variables('container2image')]",
                "resources": {
                  "requests": {
                    "cpu": 1,
                    "memoryInGb": 1.5
                    }
                }
              }
            }
          ],
          "osType": "Linux",
          "ipAddress": {
            "type": "Public",
            "ports": [
              {
                "protocol": "tcp",
                "port": "80"
              }
            ]
          }
        }
      }
    ],
    "outputs": {
      "containerIPv4Address": {
        "type": "string",
        "value": "[reference(resourceId('Microsoft.ContainerInstance/containerGroups/', 'myContainerGroup')).ipAddress.ip]"
      }
    }
  }
```

<span data-ttu-id="90737-113">toouse registru privátní kontejneru bitovou kopii, přidat k dokumentu json objektu toohello s hello formátu.</span><span class="sxs-lookup"><span data-stu-id="90737-113">toouse a private container image registry, add an object toohello json document with hello following format.</span></span>

```json
"imageRegistryCredentials": [
    {
    "server": "[parameters('imageRegistryLoginServer')]",
    "username": "[parameters('imageRegistryUsername')]",
    "password": "[parameters('imageRegistryPassword')]"
    }
]
```

## <a name="deploy-hello-template"></a><span data-ttu-id="90737-114">Nasazení šablony hello</span><span class="sxs-lookup"><span data-stu-id="90737-114">Deploy hello template</span></span>

<span data-ttu-id="90737-115">Vytvořte skupinu prostředků s hello [vytvořit skupinu az](/cli/azure/group#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="90737-115">Create a resource group with hello [az group create](/cli/azure/group#create) command.</span></span>

```azurecli-interactive
az group create --name myResourceGroup --location westus
```

<span data-ttu-id="90737-116">Nasazení šablony hello s hello [vytvořit nasazení skupiny az](/cli/azure/group/deployment#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="90737-116">Deploy hello template with hello [az group deployment create](/cli/azure/group/deployment#create) command.</span></span>

```azurecli-interactive
az group deployment create --name myContainerGroup --resource-group myResourceGroup --template-file azuredeploy.json
```

<span data-ttu-id="90737-117">Během pár sekund zobrazí se první odpověď z Azure.</span><span class="sxs-lookup"><span data-stu-id="90737-117">Within a few seconds, you will receive an initial response from Azure.</span></span> 

## <a name="view-deployment-state"></a><span data-ttu-id="90737-118">Zobrazení stavu nasazení</span><span class="sxs-lookup"><span data-stu-id="90737-118">View deployment state</span></span>

<span data-ttu-id="90737-119">Stav hello tooview hello nasazení, použijte hello `az container show` příkaz.</span><span class="sxs-lookup"><span data-stu-id="90737-119">tooview hello state of hello deployment, use hello `az container show` command.</span></span> <span data-ttu-id="90737-120">Tento příkaz vrátí hello zřízený veřejnou IP adresu přes které hello je přístupná aplikace.</span><span class="sxs-lookup"><span data-stu-id="90737-120">This returns hello provisioned public IP address over which hello application can be accessed.</span></span>

```azurecli-interactive
az container show --name myContainerGroup --resource-group myResourceGroup -o table
```

<span data-ttu-id="90737-121">Výstup:</span><span class="sxs-lookup"><span data-stu-id="90737-121">Output:</span></span>

```azurecli
Name              ResourceGroup    ProvisioningState    Image                                                             IP:ports           CPU/Memory    OsType    Location
----------------  ---------------  -------------------  ----------------------------------------------------------------  -----------------  ------------  --------  ----------
myContainerGroup  myResourceGrou2  Succeeded            microsoft/aci-tutorial-sidecar,microsoft/aci-tutorial-app:v1      40.118.253.154:80  1.0 core/1.5 gb   Linux     westus
```

## <a name="view-logs"></a><span data-ttu-id="90737-122">Zobrazit protokoly</span><span class="sxs-lookup"><span data-stu-id="90737-122">View logs</span></span>   

<span data-ttu-id="90737-123">Zobrazit výstup hello protokolu kontejneru pomocí hello `az container logs` příkaz.</span><span class="sxs-lookup"><span data-stu-id="90737-123">View hello log output of a container using hello `az container logs` command.</span></span> <span data-ttu-id="90737-124">Hello `--container-name` argument určuje hello kontejneru z které toopull protokolů.</span><span class="sxs-lookup"><span data-stu-id="90737-124">hello `--container-name` argument specifies hello container from which toopull logs.</span></span> <span data-ttu-id="90737-125">V tomto příkladu je zadána hello první kontejneru.</span><span class="sxs-lookup"><span data-stu-id="90737-125">In this example, hello first container is specified.</span></span> 

```azurecli-interactive
az container logs --name myContainerGroup --container-name aci-tutorial-app --resource-group myResourceGroup
```

<span data-ttu-id="90737-126">Výstup:</span><span class="sxs-lookup"><span data-stu-id="90737-126">Output:</span></span>

```bash
istening on port 80
::1 - - [27/Jul/2017:17:35:29 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [27/Jul/2017:17:35:32 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [27/Jul/2017:17:35:35 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [27/Jul/2017:17:35:38 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
```

<span data-ttu-id="90737-127">toosee hello protokoly pro kontejner hello straně car, spusťte hello stejný příkaz zadání hello druhý název kontejneru.</span><span class="sxs-lookup"><span data-stu-id="90737-127">toosee hello logs for hello side-car container, run hello same command specifying hello second container name.</span></span>

```azurecli-interactive
az container logs --name myContainerGroup --container-name aci-tutorial-sidecar --resource-group myResourceGroup
```

<span data-ttu-id="90737-128">Výstup:</span><span class="sxs-lookup"><span data-stu-id="90737-128">Output:</span></span>

```bash
Every 3.0s: curl -I http://localhost                                                                                                                       Mon Jul 17 11:27:36 2017

  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0  0  1663    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
HTTP/1.1 200 OK
Accept-Ranges: bytes
Content-Length: 1663
Content-Type: text/html; charset=utf-8
Last-Modified: Sun, 16 Jul 2017 02:08:22 GMT
Date: Mon, 17 Jul 2017 18:27:36 GMT
```

<span data-ttu-id="90737-129">Jak vidíte, něho hello je pravidelně zajistit hlavní webové aplikace HTTP žádost toohello prostřednictvím místní sítě tooensure hello skupiny, který je spuštěn.</span><span class="sxs-lookup"><span data-stu-id="90737-129">As you can see, hello sidecar is periodically making an HTTP request toohello main web application via hello group's local network tooensure that it is running.</span></span>

## <a name="next-steps"></a><span data-ttu-id="90737-130">Další kroky</span><span class="sxs-lookup"><span data-stu-id="90737-130">Next steps</span></span>

<span data-ttu-id="90737-131">Tento dokument zahrnutých hello kroky potřebné pro nasazení s více kontejner instanci Azure container.</span><span class="sxs-lookup"><span data-stu-id="90737-131">This document covered hello steps needed for deploying a multi-container Azure container instance.</span></span> <span data-ttu-id="90737-132">Tooend end, které prostředí Azure kontejner instancí naleznete v kurzu instancí kontejnerů Azure hello.</span><span class="sxs-lookup"><span data-stu-id="90737-132">For an end tooend Azure Container Instances experience, see hello Azure Container Instances tutorial.</span></span>

> [!div class="nextstepaction"]
> <span data-ttu-id="90737-133">[Azure instancí kontejnerů kurzu]:./container-instances-tutorial-prepare-app.md</span><span class="sxs-lookup"><span data-stu-id="90737-133">[Azure Container Instances tutorial]: ./container-instances-tutorial-prepare-app.md</span></span>
