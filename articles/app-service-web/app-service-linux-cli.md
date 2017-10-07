---
title: "aaaManage webové aplikace v systému Linux pomocí Azure CLI 2.0 | Microsoft Docs"
description: "Správa webové aplikace na Linuxu pomocí rozhraní příkazového řádku Azure."
keywords: "služby Azure app service, webové aplikace, rozhraní příkazového řádku, linux, operačních systémů"
services: app-service
documentationCenter: 
author: ahmedelnably
manager: erikre
editor: 
ms.assetid: 
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: aelnably
ms.openlocfilehash: 5e8e0da8a362450c56d2e87e087f77598ec874ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-web-app-on-linux-using-azure-cli"></a>Správa webové aplikace na Linuxu pomocí rozhraní příkazového řádku Azure

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

Pomocí příkazů hello v tomto článku jsou možné toocreate a Správa webové aplikace v systému Linux pomocí Azure CLI 2.0.
Můžete začít používat hello novou verzi hello rozhraní příkazového řádku dvěma způsoby:

* [Instalace Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) na váš počítač.
* Pomocí [prostředí cloudu Azure (Preview)](../cloud-shell/overview.md)


## <a name="create-a-linux-app-service-plan"></a>Vytvoření plánu služby App Service pro Linux

toocreate Linux plán služby App Service, můžete použít hello následující příkaz:

```azurecli-interactive
az appservice plan create -n appname -g rgname --islinux -l "South Central US" --sku S1 --number-of-workers 1
``` 

## <a name="create-a-custom-docker-container-web-app"></a>Vytvořit vlastní kontejner Docker webové aplikace

toocreate webové aplikace a její konfiguraci toorun vlastní kontejner Docker, můžete použít hello následující příkaz:

```azurecli-interactive
az webapp create -n sname -g rgname -p pname -i elnably/dockerimagetest
```
 
## <a name="activate-hello-docker-container-logging"></a>Aktivovat protokolování kontejner Docker hello

tooactivate hello protokolování kontejner Docker, můžete použít hello následující příkaz:

```azurecli-interactive
az webapp log config -n sname -g rgname --web-server-logging filesystem
```
 
## <a name="change-hello-custom-docker-container-for-an-existing-web-app-on-linux-app"></a>Změnit hello vlastní kontejner Docker pro stávající webovou aplikaci v aplikaci Linux

toochange dříve vytvořenou aplikaci, z hello aktuální Docker image tooa novou bitovou kopii, můžete použít hello následující příkaz:

```azurecli-interactive
az webapp config container set -n sname -g rgname -c apurvajo/mariohtml5
``` 

## <a name="using-docker-images-from-a-private-registry"></a>Pomocí Docker obrázky z privátní registru

Můžete nakonfigurovat vaše aplikace toouse Image z privátní registru. Adresa url hello tooprovide nutnost vaší registru, uživatelské jméno a heslo. Toho lze dosáhnout pomocí hello následující příkaz:

```azurecli-interactive
az webapp config container set -n sname1 -g rgname -c <container name> -r <server url> -u <username> -p <password>
``` 

## <a name="enable-continuous-deployments-for-custom-docker-images"></a>Povolit průběžnou nasazení pro vlastní Image Docker

Hello následující příkaz můžete povolit funkce hello CD a získat adresu url webhooku hello. Tato adresa url může být použité tooconfigure jste DockerHub nebo registru Azure kontejner úložiště.

```azurecli-interactive
az webapp deployment container config -n sname -g rgname -e true
``` 

## <a name="create-a-web-app-on-linux-app-using-one-of-our-built-in-runtime-frameworks"></a>Vytvořit webovou aplikaci na Linux aplikace pomocí jedné z našich předdefinované runtime rozhraní

toocreate 5.6 webové aplikace PHP na Linux aplikace, které můžete použít následující příkaz hello.

```azurecli-interactive
az webapp create -n sname -g rgname -p pname -r "php|5.6"
``` 

## <a name="change-framework-version-for-an-existing-web-app-on-linux-app"></a>Změnit verzi framework pro existující webovou aplikaci v aplikaci Linux

toochange dříve vytvořenou aplikaci, z hello aktuální framework verze tooNode.js 6.11, můžete použít hello následující příkaz:

```azurecli-interactive
az webapp config set -n sname -g rgname --linux-fx-version "node|6.11"
``` 

## <a name="set-up-git-deployments-for-your-web-app"></a>Nastavení nasazení Git pro vaši webovou aplikaci

tooset až nasazení Git pro vaši aplikaci, můžete použít hello následující příkaz:

```azurecli-interactive
az webapp deployment source config -n sname -g rgname --repo-url <gitrepo url> --branch <branch>
``` 


## <a name="next-steps"></a>Další kroky
* [Co je Azure webové aplikace v systému Linux?](app-service-linux-intro.md)
* [Instalace rozhraní příkazového řádku Azure 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli)
* [Prostředí cloudu Azure (Preview)](../cloud-shell/overview.md)
* [Nastavení přípravných prostředí v Azure App Service](./web-sites-staged-publishing.md)
* [Průběžné nasazování pomocí Azure webové aplikace v systému Linux](./app-service-linux-ci-cd.md)
