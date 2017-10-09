---
title: aaaPush Docker image tooprivate Azure registru | Microsoft Docs
description: "Push a načítat Docker registru privátní kontejneru tooa bitové kopie v Azure pomocí příkazového řádku Dockeru hello"
services: container-registry
documentationcenter: 
author: stevelas
manager: balans
editor: cristyg
tags: 
keywords: 
ms.assetid: 64fbe43f-fdde-4c17-a39a-d04f2d6d90a1
ms.service: container-registry
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/24/2017
ms.author: stevelas
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a81a6f4bfcb23642a89ac7631348d40e2f4911a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="push-your-first-image-tooa-private-docker-container-registry-using-hello-docker-cli"></a><span data-ttu-id="c4c95-103">Push vaší první obrázek tooa privátní Docker kontejneru registru pomocí příkazového řádku Dockeru hello</span><span class="sxs-lookup"><span data-stu-id="c4c95-103">Push your first image tooa private Docker container registry using hello Docker CLI</span></span>
<span data-ttu-id="c4c95-104">Azure kontejneru registru uchovává a spravuje privátní [Docker](http://hub.docker.com) kontejner imagí, podobné toohello způsob [úložiště Docker Hub](https://hub.docker.com/) ukládá veřejné imagí Dockeru.</span><span class="sxs-lookup"><span data-stu-id="c4c95-104">An Azure container registry stores and manages private [Docker](http://hub.docker.com) container images, similar toohello way [Docker Hub](https://hub.docker.com/) stores public Docker images.</span></span> <span data-ttu-id="c4c95-105">Použít hello [rozhraní příkazového řádku Dockeru](https://docs.docker.com/engine/reference/commandline/cli/) (příkazového řádku Dockeru) pro [přihlášení](https://docs.docker.com/engine/reference/commandline/login/), [nabízené](https://docs.docker.com/engine/reference/commandline/push/), [vyžádání](https://docs.docker.com/engine/reference/commandline/pull/)a další operace na vaše kontejneru registru.</span><span class="sxs-lookup"><span data-stu-id="c4c95-105">You use hello [Docker Command-Line Interface](https://docs.docker.com/engine/reference/commandline/cli/) (Docker CLI) for [login](https://docs.docker.com/engine/reference/commandline/login/), [push](https://docs.docker.com/engine/reference/commandline/push/), [pull](https://docs.docker.com/engine/reference/commandline/pull/), and other operations on your container registry.</span></span>

<span data-ttu-id="c4c95-106">Pozadí a koncepty najdete v tématu [hello – přehled](container-registry-intro.md)</span><span class="sxs-lookup"><span data-stu-id="c4c95-106">For more background and concepts, see [hello overview](container-registry-intro.md)</span></span>



## <a name="prerequisites"></a><span data-ttu-id="c4c95-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c4c95-107">Prerequisites</span></span>
* <span data-ttu-id="c4c95-108">**Registr kontejnerů Azure** – Vytvořte registr kontejnerů ve svém předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="c4c95-108">**Azure container registry** - Create a container registry in your Azure subscription.</span></span> <span data-ttu-id="c4c95-109">Například použít hello [portál Azure](container-registry-get-started-portal.md) nebo hello [Azure CLI 2.0](container-registry-get-started-azure-cli.md).</span><span class="sxs-lookup"><span data-stu-id="c4c95-109">For example, use hello [Azure portal](container-registry-get-started-portal.md) or hello [Azure CLI 2.0](container-registry-get-started-azure-cli.md).</span></span>
* <span data-ttu-id="c4c95-110">**Rozhraní příkazového řádku dockeru** -tooset do místního počítače jako Docker hostitele a přístup hello příkazového řádku Dockeru příkazy, nainstalujte [modulu Docker](https://docs.docker.com/engine/installation/).</span><span class="sxs-lookup"><span data-stu-id="c4c95-110">**Docker CLI** - tooset up your local computer as a Docker host and access hello Docker CLI commands, install [Docker Engine](https://docs.docker.com/engine/installation/).</span></span>

## <a name="log-in-tooa-registry"></a><span data-ttu-id="c4c95-111">Přihlaste se tooa registru</span><span class="sxs-lookup"><span data-stu-id="c4c95-111">Log in tooa registry</span></span>
<span data-ttu-id="c4c95-112">Spustit `docker login` toolog v registru kontejneru tooyour s vaší [registru pověření](container-registry-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="c4c95-112">Run `docker login` toolog in tooyour container registry with your [registry credentials](container-registry-authentication.md).</span></span>

<span data-ttu-id="c4c95-113">Hello následující příklad úspěšně projde hello ID a hesla služby Azure Active Directory [instanční objekt](../active-directory/active-directory-application-objects.md).</span><span class="sxs-lookup"><span data-stu-id="c4c95-113">hello following example passes hello ID and password of an Azure Active Directory [service principal](../active-directory/active-directory-application-objects.md).</span></span> <span data-ttu-id="c4c95-114">Například může přiřadili jste hlavní tooyour registru služby automation scénář.</span><span class="sxs-lookup"><span data-stu-id="c4c95-114">For example, you might have assigned a service principal tooyour registry for an automation scenario.</span></span>

```
docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
```

> [!TIP]
> <span data-ttu-id="c4c95-115">Ujistěte se, že toospecify hello registru plně kvalifikovaný název (všechny malá písmena).</span><span class="sxs-lookup"><span data-stu-id="c4c95-115">Make sure toospecify hello fully qualified registry name (all lowercase).</span></span> <span data-ttu-id="c4c95-116">V tomto příkladu je to `myregistry.azurecr.io`.</span><span class="sxs-lookup"><span data-stu-id="c4c95-116">In this example, it is `myregistry.azurecr.io`.</span></span>

## <a name="steps-toopull-and-push-an-image"></a><span data-ttu-id="c4c95-117">Kroky toopull a nabízenými bitovou kopii</span><span class="sxs-lookup"><span data-stu-id="c4c95-117">Steps toopull and push an image</span></span>
<span data-ttu-id="c4c95-118">Hello podle příkladu stahování hello Nginx bitovou kopii z hello veřejného úložiště Docker Hub registru značky pro vaši privátní kontejner Azure registru nabízených oznámení tooyour registru a pak ho znovu vrátí.</span><span class="sxs-lookup"><span data-stu-id="c4c95-118">hello follow example downloads hello Nginx image from hello public Docker Hub registry, tags it for your private Azure container registry, pushes it tooyour registry, then pulls it again.</span></span>

<span data-ttu-id="c4c95-119">**1. Hello Docker oficiální image pro vyžádání obsahu pro Nginx**</span><span class="sxs-lookup"><span data-stu-id="c4c95-119">**1. Pull hello Docker official image for Nginx**</span></span>

<span data-ttu-id="c4c95-120">Nejprve pro vyžádání obsahu hello veřejné Nginx image tooyour místního počítače.</span><span class="sxs-lookup"><span data-stu-id="c4c95-120">First pull hello public Nginx image tooyour local computer.</span></span>

```
docker pull nginx
```
<span data-ttu-id="c4c95-121">**2. Spusťte kontejner nginx a sváže hello**</span><span class="sxs-lookup"><span data-stu-id="c4c95-121">**2. Start hello Nginx container**</span></span>

<span data-ttu-id="c4c95-122">Hello následující příkaz spustí místní kontejner nginx a sváže hello interaktivně na portu 8080, což vám toosee výstup z Nginx.</span><span class="sxs-lookup"><span data-stu-id="c4c95-122">hello following command starts hello local Nginx container interactively on port 8080, allowing you toosee output from Nginx.</span></span> <span data-ttu-id="c4c95-123">Odebere hello systémem kontejneru jednou byla zastavena.</span><span class="sxs-lookup"><span data-stu-id="c4c95-123">It removes hello running container once stopped.</span></span>

```
docker run -it --rm -p 8080:80 nginx
```

<span data-ttu-id="c4c95-124">Procházet příliš[adrese http://localhost: 8080](http://localhost:8080) tooview hello systémem kontejneru.</span><span class="sxs-lookup"><span data-stu-id="c4c95-124">Browse too[http://localhost:8080](http://localhost:8080) tooview hello running container.</span></span> <span data-ttu-id="c4c95-125">Zobrazí obrazovky podobné toohello následující jeden.</span><span class="sxs-lookup"><span data-stu-id="c4c95-125">You see a screen similar toohello following one.</span></span>

![Server Nginx na místním počítači](./media/container-registry-get-started-docker-cli/nginx.png)

<span data-ttu-id="c4c95-127">toostop hello spuštěné kontejneru, stiskněte [CTRL] + [C].</span><span class="sxs-lookup"><span data-stu-id="c4c95-127">toostop hello running container, press [CTRL]+[C].</span></span>

<span data-ttu-id="c4c95-128">**3. Vytvoření aliasu hello bitové kopie v registru**</span><span class="sxs-lookup"><span data-stu-id="c4c95-128">**3. Create an alias of hello image in your registry**</span></span>

<span data-ttu-id="c4c95-129">Hello následující příkaz vytvoří alias hello bitové kopie s registru tooyour plně kvalifikovanou cestu.</span><span class="sxs-lookup"><span data-stu-id="c4c95-129">hello following command creates an alias of hello image, with a fully qualified path tooyour registry.</span></span> <span data-ttu-id="c4c95-130">Tento příklad určuje hello `samples` zbytečné soubory tooavoid oboru názvů v kořenovém hello hello registru.</span><span class="sxs-lookup"><span data-stu-id="c4c95-130">This example specifies hello `samples` namespace tooavoid clutter in hello root of hello registry.</span></span>

```
docker tag nginx myregistry.azurecr.io/samples/nginx
```  

<span data-ttu-id="c4c95-131">**4. Push hello image tooyour registru**</span><span class="sxs-lookup"><span data-stu-id="c4c95-131">**4. Push hello image tooyour registry**</span></span>

```
docker push myregistry.azurecr.io/samples/nginx
```

<span data-ttu-id="c4c95-132">**5. Hello image pro vyžádání obsahu z registru**</span><span class="sxs-lookup"><span data-stu-id="c4c95-132">**5. Pull hello image from your registry**</span></span>

```
docker pull myregistry.azurecr.io/samples/nginx
```

<span data-ttu-id="c4c95-133">**6. Spusťte kontejner nginx a sváže hello z registru**</span><span class="sxs-lookup"><span data-stu-id="c4c95-133">**6. Start hello Nginx container from your registry**</span></span>

```
docker run -it --rm -p 8080:80 myregistry.azurecr.io/samples/nginx
```

<span data-ttu-id="c4c95-134">Procházet příliš[adrese http://localhost: 8080](http://localhost:8080) tooview hello systémem kontejneru.</span><span class="sxs-lookup"><span data-stu-id="c4c95-134">Browse too[http://localhost:8080](http://localhost:8080) tooview hello running container.</span></span>

<span data-ttu-id="c4c95-135">toostop hello spuštěné kontejneru, stiskněte [CTRL] + [C].</span><span class="sxs-lookup"><span data-stu-id="c4c95-135">toostop hello running container, press [CTRL]+[C].</span></span>

<span data-ttu-id="c4c95-136">**7. (Volitelné) Odeberte hello bitovou kopii**</span><span class="sxs-lookup"><span data-stu-id="c4c95-136">**7. (Optional) Remove hello image**</span></span>

```
docker rmi myregistry.azurecr.io/samples/nginx
```

##<a name="concurrent-limits"></a><span data-ttu-id="c4c95-137">Omezení souběžnosti</span><span class="sxs-lookup"><span data-stu-id="c4c95-137">Concurrent Limits</span></span>
<span data-ttu-id="c4c95-138">V některých scénářích provádění volání současně může vést k chybám.</span><span class="sxs-lookup"><span data-stu-id="c4c95-138">In some scenarios, executing calls concurrently might result in errors.</span></span> <span data-ttu-id="c4c95-139">Hello následující tabulka obsahuje hello omezení souběžných volání s operace "Push" a "Pro vyžádání obsahu" na kontejner Azure registru:</span><span class="sxs-lookup"><span data-stu-id="c4c95-139">hello following table contains hello limits of concurrent calls with "Push" and "Pull" operations on Azure container registry:</span></span>

| <span data-ttu-id="c4c95-140">Operace</span><span class="sxs-lookup"><span data-stu-id="c4c95-140">Operation</span></span>  | <span data-ttu-id="c4c95-141">Omezení</span><span class="sxs-lookup"><span data-stu-id="c4c95-141">Limit</span></span>                                  |
| ---------- | -------------------------------------- |
| <span data-ttu-id="c4c95-142">PULL</span><span class="sxs-lookup"><span data-stu-id="c4c95-142">PULL</span></span>       | <span data-ttu-id="c4c95-143">Až too10 souběžných vrátí na registru</span><span class="sxs-lookup"><span data-stu-id="c4c95-143">Up too10 concurrent pulls per registry</span></span> |
| <span data-ttu-id="c4c95-144">PUSH</span><span class="sxs-lookup"><span data-stu-id="c4c95-144">PUSH</span></span>       | <span data-ttu-id="c4c95-145">Až too5 souběžných nabízených oznámení na registru</span><span class="sxs-lookup"><span data-stu-id="c4c95-145">Up too5 concurrent pushes per registry</span></span> |

## <a name="next-steps"></a><span data-ttu-id="c4c95-146">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c4c95-146">Next steps</span></span>
<span data-ttu-id="c4c95-147">Nyní když znáte hello základy, jste připravené toostart pomocí vašeho klíče registru!</span><span class="sxs-lookup"><span data-stu-id="c4c95-147">Now that you know hello basics, you are ready toostart using your registry!</span></span> <span data-ttu-id="c4c95-148">Například začít s nasazením Image tooan kontejneru [Azure Container Service](https://azure.microsoft.com/documentation/services/container-service/) clusteru.</span><span class="sxs-lookup"><span data-stu-id="c4c95-148">For example, start deploying container images tooan [Azure Container Service](https://azure.microsoft.com/documentation/services/container-service/) cluster.</span></span>
