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
# <a name="deploy-a-container-group"></a>Nasazení kontejneru skupiny

Azure instancí kontejnerů podporu hello nasazení několika kontejnerů do jednoho hostitele pomocí *skupina kontejneru*. To je užitečné při sestavování něho aplikace pro protokolování, sledování nebo jakoukoli jinou konfiguraci, které služba potřebuje druhý připojené procesu. 

Tento dokument vás provede spuštění jednoduché něho více kontejneru konfigurace pomocí šablony Azure Resource Manager.

## <a name="configure-hello-template"></a>Konfigurace šablony hello

Vytvořte soubor s názvem `azuredeploy.json` a kopírování hello následující json do ní. 

V této ukázce je definována kontejner skupiny s dvěma kontejnery a veřejnou IP adresu. první kontejneru Hello hello skupiny spustí internetovou přístupných aplikaci. druhý kontejneru Hello hello něho díky hlavní webové aplikace HTTP žádost toohello prostřednictvím místní sítě hello skupiny. 

Tento příklad něho může být rozšířené tootrigger výstrahu, pokud dostali kódu odpovědi HTTP než 200 OK. 

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

toouse registru privátní kontejneru bitovou kopii, přidat k dokumentu json objektu toohello s hello formátu.

```json
"imageRegistryCredentials": [
    {
    "server": "[parameters('imageRegistryLoginServer')]",
    "username": "[parameters('imageRegistryUsername')]",
    "password": "[parameters('imageRegistryPassword')]"
    }
]
```

## <a name="deploy-hello-template"></a>Nasazení šablony hello

Vytvořte skupinu prostředků s hello [vytvořit skupinu az](/cli/azure/group#create) příkaz.

```azurecli-interactive
az group create --name myResourceGroup --location westus
```

Nasazení šablony hello s hello [vytvořit nasazení skupiny az](/cli/azure/group/deployment#create) příkaz.

```azurecli-interactive
az group deployment create --name myContainerGroup --resource-group myResourceGroup --template-file azuredeploy.json
```

Během pár sekund zobrazí se první odpověď z Azure. 

## <a name="view-deployment-state"></a>Zobrazení stavu nasazení

Stav hello tooview hello nasazení, použijte hello `az container show` příkaz. Tento příkaz vrátí hello zřízený veřejnou IP adresu přes které hello je přístupná aplikace.

```azurecli-interactive
az container show --name myContainerGroup --resource-group myResourceGroup -o table
```

Výstup:

```azurecli
Name              ResourceGroup    ProvisioningState    Image                                                             IP:ports           CPU/Memory    OsType    Location
----------------  ---------------  -------------------  ----------------------------------------------------------------  -----------------  ------------  --------  ----------
myContainerGroup  myResourceGrou2  Succeeded            microsoft/aci-tutorial-sidecar,microsoft/aci-tutorial-app:v1      40.118.253.154:80  1.0 core/1.5 gb   Linux     westus
```

## <a name="view-logs"></a>Zobrazit protokoly   

Zobrazit výstup hello protokolu kontejneru pomocí hello `az container logs` příkaz. Hello `--container-name` argument určuje hello kontejneru z které toopull protokolů. V tomto příkladu je zadána hello první kontejneru. 

```azurecli-interactive
az container logs --name myContainerGroup --container-name aci-tutorial-app --resource-group myResourceGroup
```

Výstup:

```bash
istening on port 80
::1 - - [27/Jul/2017:17:35:29 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [27/Jul/2017:17:35:32 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [27/Jul/2017:17:35:35 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [27/Jul/2017:17:35:38 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
```

toosee hello protokoly pro kontejner hello straně car, spusťte hello stejný příkaz zadání hello druhý název kontejneru.

```azurecli-interactive
az container logs --name myContainerGroup --container-name aci-tutorial-sidecar --resource-group myResourceGroup
```

Výstup:

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

Jak vidíte, něho hello je pravidelně zajistit hlavní webové aplikace HTTP žádost toohello prostřednictvím místní sítě tooensure hello skupiny, který je spuštěn.

## <a name="next-steps"></a>Další kroky

Tento dokument zahrnutých hello kroky potřebné pro nasazení s více kontejner instanci Azure container. Tooend end, které prostředí Azure kontejner instancí naleznete v kurzu instancí kontejnerů Azure hello.

> [!div class="nextstepaction"]
> [Azure instancí kontejnerů kurzu]:./container-instances-tutorial-prepare-app.md
