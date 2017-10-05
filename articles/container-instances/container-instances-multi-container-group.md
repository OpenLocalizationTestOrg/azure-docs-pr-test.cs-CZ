---
title: "Azure kontejner instancí - skupina více kontejneru | Dokumentace Azure"
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
ms.openlocfilehash: 140f58582645ea32f77e901eb13364ed145bbecf
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="deploy-a-container-group"></a><span data-ttu-id="0c32e-103">Nasazení kontejneru skupiny</span><span class="sxs-lookup"><span data-stu-id="0c32e-103">Deploy a container group</span></span>

<span data-ttu-id="0c32e-104">Azure instancí kontejnerů podporu nasazení několika kontejnerů do jednoho hostitele pomocí *skupina kontejneru*.</span><span class="sxs-lookup"><span data-stu-id="0c32e-104">Azure Container Instances support the deployment of multiple containers onto a single host using a *container group*.</span></span> <span data-ttu-id="0c32e-105">To je užitečné při sestavování něho aplikace pro protokolování, sledování nebo jakoukoli jinou konfiguraci, které služba potřebuje druhý připojené procesu.</span><span class="sxs-lookup"><span data-stu-id="0c32e-105">This is useful when building an application sidecar for logging, monitoring, or any other configuration where a service needs a second attached process.</span></span> 

<span data-ttu-id="0c32e-106">Tento dokument vás provede spuštění jednoduché něho více kontejneru konfigurace pomocí šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="0c32e-106">This document walks through running a simple multi-container sidecar configuration using an Azure Resource Manager template.</span></span>

## <a name="configure-the-template"></a><span data-ttu-id="0c32e-107">Konfigurace šablony</span><span class="sxs-lookup"><span data-stu-id="0c32e-107">Configure the template</span></span>

<span data-ttu-id="0c32e-108">Vytvořte soubor s názvem `azuredeploy.json` a zkopírujte do ní následující kód json.</span><span class="sxs-lookup"><span data-stu-id="0c32e-108">Create a file named `azuredeploy.json` and copy the following json into it.</span></span> 

<span data-ttu-id="0c32e-109">V této ukázce je definována kontejner skupiny s dvěma kontejnery a veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="0c32e-109">In this sample, a container group with two containers and a public IP address is defined.</span></span> <span data-ttu-id="0c32e-110">První kontejner skupiny spustí internetovou přístupných aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0c32e-110">The first container of the group runs an internet facing application.</span></span> <span data-ttu-id="0c32e-111">Druhý kontejneru něho, zašle požadavek HTTP do hlavní webové aplikace prostřednictvím místní sítě skupiny.</span><span class="sxs-lookup"><span data-stu-id="0c32e-111">The second container, the sidecar, makes an HTTP request to the main web application via the group's local network.</span></span> 

<span data-ttu-id="0c32e-112">Tento příklad něho může rozšířit, aby spuštění výstrahy v případě obdržel kódu odpovědi HTTP než 200 OK.</span><span class="sxs-lookup"><span data-stu-id="0c32e-112">This sidecar example could be expanded to trigger an alert if it received an HTTP response code other than 200 OK.</span></span> 

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

<span data-ttu-id="0c32e-113">Použít privátní kontejneru registru bitovou kopii, přidáte objekt v dokumentu json v následujícím formátu.</span><span class="sxs-lookup"><span data-stu-id="0c32e-113">To use a private container image registry, add an object to the json document with the following format.</span></span>

```json
"imageRegistryCredentials": [
    {
    "server": "[parameters('imageRegistryLoginServer')]",
    "username": "[parameters('imageRegistryUsername')]",
    "password": "[parameters('imageRegistryPassword')]"
    }
]
```

## <a name="deploy-the-template"></a><span data-ttu-id="0c32e-114">Nasazení šablony</span><span class="sxs-lookup"><span data-stu-id="0c32e-114">Deploy the template</span></span>

<span data-ttu-id="0c32e-115">Vytvořte skupinu prostředků pomocí příkazu [az group create](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="0c32e-115">Create a resource group with the [az group create](/cli/azure/group#create) command.</span></span>

```azurecli-interactive
az group create --name myResourceGroup --location westus
```

<span data-ttu-id="0c32e-116">Nasazení šablony s [vytvořit nasazení skupiny az](/cli/azure/group/deployment#create) příkaz.</span><span class="sxs-lookup"><span data-stu-id="0c32e-116">Deploy the template with the [az group deployment create](/cli/azure/group/deployment#create) command.</span></span>

```azurecli-interactive
az group deployment create --name myContainerGroup --resource-group myResourceGroup --template-file azuredeploy.json
```

<span data-ttu-id="0c32e-117">Během pár sekund zobrazí se první odpověď z Azure.</span><span class="sxs-lookup"><span data-stu-id="0c32e-117">Within a few seconds, you will receive an initial response from Azure.</span></span> 

## <a name="view-deployment-state"></a><span data-ttu-id="0c32e-118">Zobrazení stavu nasazení</span><span class="sxs-lookup"><span data-stu-id="0c32e-118">View deployment state</span></span>

<span data-ttu-id="0c32e-119">Chcete-li zobrazit stav nasazení, použijte `az container show` příkaz.</span><span class="sxs-lookup"><span data-stu-id="0c32e-119">To view the state of the deployment, use the `az container show` command.</span></span> <span data-ttu-id="0c32e-120">Tento příkaz vrátí zřízené veřejnou IP adresu přes která je přístupná aplikace.</span><span class="sxs-lookup"><span data-stu-id="0c32e-120">This returns the provisioned public IP address over which the application can be accessed.</span></span>

```azurecli-interactive
az container show --name myContainerGroup --resource-group myResourceGroup -o table
```

<span data-ttu-id="0c32e-121">Výstup:</span><span class="sxs-lookup"><span data-stu-id="0c32e-121">Output:</span></span>

```azurecli
Name              ResourceGroup    ProvisioningState    Image                                                             IP:ports           CPU/Memory    OsType    Location
----------------  ---------------  -------------------  ----------------------------------------------------------------  -----------------  ------------  --------  ----------
myContainerGroup  myResourceGrou2  Succeeded            microsoft/aci-tutorial-sidecar,microsoft/aci-tutorial-app:v1      40.118.253.154:80  1.0 core/1.5 gb   Linux     westus
```

## <a name="view-logs"></a><span data-ttu-id="0c32e-122">Zobrazit protokoly</span><span class="sxs-lookup"><span data-stu-id="0c32e-122">View logs</span></span>   

<span data-ttu-id="0c32e-123">Zobrazit výstup protokolu kontejneru pomocí `az container logs` příkaz.</span><span class="sxs-lookup"><span data-stu-id="0c32e-123">View the log output of a container using the `az container logs` command.</span></span> <span data-ttu-id="0c32e-124">`--container-name` Argument určuje kontejner, ze kterého protokoly pro vyžádání obsahu.</span><span class="sxs-lookup"><span data-stu-id="0c32e-124">The `--container-name` argument specifies the container from which to pull logs.</span></span> <span data-ttu-id="0c32e-125">V tomto příkladu je zadána první kontejneru.</span><span class="sxs-lookup"><span data-stu-id="0c32e-125">In this example, the first container is specified.</span></span> 

```azurecli-interactive
az container logs --name myContainerGroup --container-name aci-tutorial-app --resource-group myResourceGroup
```

<span data-ttu-id="0c32e-126">Výstup:</span><span class="sxs-lookup"><span data-stu-id="0c32e-126">Output:</span></span>

```bash
istening on port 80
::1 - - [27/Jul/2017:17:35:29 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [27/Jul/2017:17:35:32 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [27/Jul/2017:17:35:35 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [27/Jul/2017:17:35:38 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
```

<span data-ttu-id="0c32e-127">Pokud chcete zobrazit protokoly pro kontejner straně car, spusťte stejný příkaz zadání druhý název kontejneru.</span><span class="sxs-lookup"><span data-stu-id="0c32e-127">To see the logs for the side-car container, run the same command specifying the second container name.</span></span>

```azurecli-interactive
az container logs --name myContainerGroup --container-name aci-tutorial-sidecar --resource-group myResourceGroup
```

<span data-ttu-id="0c32e-128">Výstup:</span><span class="sxs-lookup"><span data-stu-id="0c32e-128">Output:</span></span>

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

<span data-ttu-id="0c32e-129">Jak můžete vidět, je něho pravidelně zajistit požadavek HTTP je hlavní webové aplikace prostřednictvím místní sítě skupiny zkontrolujte, zda je spuštěn.</span><span class="sxs-lookup"><span data-stu-id="0c32e-129">As you can see, the sidecar is periodically making an HTTP request to the main web application via the group's local network to ensure that it is running.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0c32e-130">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0c32e-130">Next steps</span></span>

<span data-ttu-id="0c32e-131">Tento dokument popsané kroky potřebné pro nasazení s více kontejnerů Azure container instance.</span><span class="sxs-lookup"><span data-stu-id="0c32e-131">This document covered the steps needed for deploying a multi-container Azure container instance.</span></span> <span data-ttu-id="0c32e-132">Koncová instancí kontejnerů Azure prostředí najdete v kurzu instancí kontejnerů Azure.</span><span class="sxs-lookup"><span data-stu-id="0c32e-132">For an end to end Azure Container Instances experience, see the Azure Container Instances tutorial.</span></span>

> [!div class="nextstepaction"]
> <span data-ttu-id="0c32e-133">[Azure instancí kontejnerů kurzu]:./container-instances-tutorial-prepare-app.md</span><span class="sxs-lookup"><span data-stu-id="0c32e-133">[Azure Container Instances tutorial]: ./container-instances-tutorial-prepare-app.md</span></span>
