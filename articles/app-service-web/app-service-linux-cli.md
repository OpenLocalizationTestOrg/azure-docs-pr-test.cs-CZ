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
# <a name="manage-web-app-on-linux-using-azure-cli"></a><span data-ttu-id="2ae8a-104">Správa webové aplikace na Linuxu pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="2ae8a-104">Manage Web App on Linux using Azure CLI</span></span>

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

<span data-ttu-id="2ae8a-105">Pomocí příkazů hello v tomto článku jsou možné toocreate a Správa webové aplikace v systému Linux pomocí Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="2ae8a-105">Using hello commands in this article you are able toocreate and manage a Web App on Linux using Azure CLI 2.0.</span></span>
<span data-ttu-id="2ae8a-106">Můžete začít používat hello novou verzi hello rozhraní příkazového řádku dvěma způsoby:</span><span class="sxs-lookup"><span data-stu-id="2ae8a-106">You can start using hello new version of hello CLI in two ways:</span></span>

* <span data-ttu-id="2ae8a-107">[Instalace Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) na váš počítač.</span><span class="sxs-lookup"><span data-stu-id="2ae8a-107">[Installing Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) on your machine.</span></span>
* <span data-ttu-id="2ae8a-108">Pomocí [prostředí cloudu Azure (Preview)](../cloud-shell/overview.md)</span><span class="sxs-lookup"><span data-stu-id="2ae8a-108">Using [Azure Cloud Shell (Preview)](../cloud-shell/overview.md)</span></span>


## <a name="create-a-linux-app-service-plan"></a><span data-ttu-id="2ae8a-109">Vytvoření plánu služby App Service pro Linux</span><span class="sxs-lookup"><span data-stu-id="2ae8a-109">Create a Linux App Service Plan</span></span>

<span data-ttu-id="2ae8a-110">toocreate Linux plán služby App Service, můžete použít hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="2ae8a-110">toocreate a Linux App Service Plan, you can use hello following command:</span></span>

```azurecli-interactive
az appservice plan create -n appname -g rgname --islinux -l "South Central US" --sku S1 --number-of-workers 1
``` 

## <a name="create-a-custom-docker-container-web-app"></a><span data-ttu-id="2ae8a-111">Vytvořit vlastní kontejner Docker webové aplikace</span><span class="sxs-lookup"><span data-stu-id="2ae8a-111">Create a custom Docker container Web App</span></span>

<span data-ttu-id="2ae8a-112">toocreate webové aplikace a její konfiguraci toorun vlastní kontejner Docker, můžete použít hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="2ae8a-112">toocreate a web app and configuring it toorun a custom Docker container, you can use hello following command:</span></span>

```azurecli-interactive
az webapp create -n sname -g rgname -p pname -i elnably/dockerimagetest
```
 
## <a name="activate-hello-docker-container-logging"></a><span data-ttu-id="2ae8a-113">Aktivovat protokolování kontejner Docker hello</span><span class="sxs-lookup"><span data-stu-id="2ae8a-113">Activate hello Docker container logging</span></span>

<span data-ttu-id="2ae8a-114">tooactivate hello protokolování kontejner Docker, můžete použít hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="2ae8a-114">tooactivate hello Docker container logging, you can use hello following command:</span></span>

```azurecli-interactive
az webapp log config -n sname -g rgname --web-server-logging filesystem
```
 
## <a name="change-hello-custom-docker-container-for-an-existing-web-app-on-linux-app"></a><span data-ttu-id="2ae8a-115">Změnit hello vlastní kontejner Docker pro stávající webovou aplikaci v aplikaci Linux</span><span class="sxs-lookup"><span data-stu-id="2ae8a-115">Change hello custom Docker container for an existing Web App on Linux App</span></span>

<span data-ttu-id="2ae8a-116">toochange dříve vytvořenou aplikaci, z hello aktuální Docker image tooa novou bitovou kopii, můžete použít hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="2ae8a-116">toochange a previously created app, from hello current Docker image tooa new image, you can use hello following command:</span></span>

```azurecli-interactive
az webapp config container set -n sname -g rgname -c apurvajo/mariohtml5
``` 

## <a name="using-docker-images-from-a-private-registry"></a><span data-ttu-id="2ae8a-117">Pomocí Docker obrázky z privátní registru</span><span class="sxs-lookup"><span data-stu-id="2ae8a-117">Using Docker images from a private registry</span></span>

<span data-ttu-id="2ae8a-118">Můžete nakonfigurovat vaše aplikace toouse Image z privátní registru.</span><span class="sxs-lookup"><span data-stu-id="2ae8a-118">You can configure your app toouse images from a private registry.</span></span> <span data-ttu-id="2ae8a-119">Adresa url hello tooprovide nutnost vaší registru, uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="2ae8a-119">You need tooprovide hello url for your registry, user name, and password.</span></span> <span data-ttu-id="2ae8a-120">Toho lze dosáhnout pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="2ae8a-120">This can be achieved using hello following command:</span></span>

```azurecli-interactive
az webapp config container set -n sname1 -g rgname -c <container name> -r <server url> -u <username> -p <password>
``` 

## <a name="enable-continuous-deployments-for-custom-docker-images"></a><span data-ttu-id="2ae8a-121">Povolit průběžnou nasazení pro vlastní Image Docker</span><span class="sxs-lookup"><span data-stu-id="2ae8a-121">Enable continuous deployments for custom Docker images</span></span>

<span data-ttu-id="2ae8a-122">Hello následující příkaz můžete povolit funkce hello CD a získat adresu url webhooku hello.</span><span class="sxs-lookup"><span data-stu-id="2ae8a-122">With hello following command you can enable hello CD functionality, and get hello webhook url.</span></span> <span data-ttu-id="2ae8a-123">Tato adresa url může být použité tooconfigure jste DockerHub nebo registru Azure kontejner úložiště.</span><span class="sxs-lookup"><span data-stu-id="2ae8a-123">This url can be used tooconfigure you DockerHub or Azure Container Registry repos.</span></span>

```azurecli-interactive
az webapp deployment container config -n sname -g rgname -e true
``` 

## <a name="create-a-web-app-on-linux-app-using-one-of-our-built-in-runtime-frameworks"></a><span data-ttu-id="2ae8a-124">Vytvořit webovou aplikaci na Linux aplikace pomocí jedné z našich předdefinované runtime rozhraní</span><span class="sxs-lookup"><span data-stu-id="2ae8a-124">Create a Web App on Linux App using one of our built-in runtime frameworks</span></span>

<span data-ttu-id="2ae8a-125">toocreate 5.6 webové aplikace PHP na Linux aplikace, které můžete použít následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="2ae8a-125">toocreate a PHP 5.6 Web App on Linux App that, you can use hello following command.</span></span>

```azurecli-interactive
az webapp create -n sname -g rgname -p pname -r "php|5.6"
``` 

## <a name="change-framework-version-for-an-existing-web-app-on-linux-app"></a><span data-ttu-id="2ae8a-126">Změnit verzi framework pro existující webovou aplikaci v aplikaci Linux</span><span class="sxs-lookup"><span data-stu-id="2ae8a-126">Change framework version for an existing Web App on Linux App</span></span>

<span data-ttu-id="2ae8a-127">toochange dříve vytvořenou aplikaci, z hello aktuální framework verze tooNode.js 6.11, můžete použít hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="2ae8a-127">toochange a previously created app, from hello current framework version tooNode.js 6.11, you can use hello following command:</span></span>

```azurecli-interactive
az webapp config set -n sname -g rgname --linux-fx-version "node|6.11"
``` 

## <a name="set-up-git-deployments-for-your-web-app"></a><span data-ttu-id="2ae8a-128">Nastavení nasazení Git pro vaši webovou aplikaci</span><span class="sxs-lookup"><span data-stu-id="2ae8a-128">Set up Git deployments for your Web App</span></span>

<span data-ttu-id="2ae8a-129">tooset až nasazení Git pro vaši aplikaci, můžete použít hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="2ae8a-129">tooset up Git deployments for your app, you can use hello following command:</span></span>

```azurecli-interactive
az webapp deployment source config -n sname -g rgname --repo-url <gitrepo url> --branch <branch>
``` 


## <a name="next-steps"></a><span data-ttu-id="2ae8a-130">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2ae8a-130">Next steps</span></span>
* [<span data-ttu-id="2ae8a-131">Co je Azure webové aplikace v systému Linux?</span><span class="sxs-lookup"><span data-stu-id="2ae8a-131">What is Azure Web App on Linux?</span></span>](app-service-linux-intro.md)
* [<span data-ttu-id="2ae8a-132">Instalace rozhraní příkazového řádku Azure 2.0</span><span class="sxs-lookup"><span data-stu-id="2ae8a-132">Install Azure CLI 2.0</span></span>](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli)
* [<span data-ttu-id="2ae8a-133">Prostředí cloudu Azure (Preview)</span><span class="sxs-lookup"><span data-stu-id="2ae8a-133">Azure Cloud Shell (Preview)</span></span>](../cloud-shell/overview.md)
* [<span data-ttu-id="2ae8a-134">Nastavení přípravných prostředí v Azure App Service</span><span class="sxs-lookup"><span data-stu-id="2ae8a-134">Set up staging environments in Azure App Service</span></span>](./web-sites-staged-publishing.md)
* [<span data-ttu-id="2ae8a-135">Průběžné nasazování pomocí Azure webové aplikace v systému Linux</span><span class="sxs-lookup"><span data-stu-id="2ae8a-135">Continuous Deployment with Azure Web App on Linux</span></span>](./app-service-linux-ci-cd.md)
