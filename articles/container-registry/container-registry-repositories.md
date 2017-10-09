---
title: "aaaAzure kontejneru registru úložiště | Microsoft Docs"
description: "Jak toouse registru Azure kontejner úložiště pro Docker bitové kopie"
services: container-registry
documentationcenter: 
author: cristy
manager: balans
editor: dlepow
ms.service: container-registry
ms.devlang: na
ms.topic: how-to-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/24/2017
ms.author: cristyg
ms.openlocfilehash: 108622c565e41777fbb1fc9da9a01168abc7a7fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-container-registry-repositories"></a><span data-ttu-id="b6089-103">Kontejner Azure registru úložiště</span><span class="sxs-lookup"><span data-stu-id="b6089-103">Azure container registry repositories</span></span>

<span data-ttu-id="b6089-104">Kontejner Azure registru umožňuje toostore obrázků kontejneru v úložiště.</span><span class="sxs-lookup"><span data-stu-id="b6089-104">Azure container registry allows you toostore container images in repositories.</span></span> <span data-ttu-id="b6089-105">A to uložením do úložiště bitové kopie, může mít skupin bitové kopie (nebo verzi bitové kopie) v izolovaném prostředí.</span><span class="sxs-lookup"><span data-stu-id="b6089-105">By storing images in repositories, you can have groups of images (or version of images) in isolated environments.</span></span> <span data-ttu-id="b6089-106">Tyto úložiště můžete určit, když push registru tooyour bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="b6089-106">You can specify these repositories when you push images tooyour registry.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="b6089-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b6089-107">Prerequisites</span></span>
* <span data-ttu-id="b6089-108">**Registr kontejnerů Azure** – Vytvořte registr kontejnerů ve svém předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="b6089-108">**Azure container registry** - Create a container registry in your Azure subscription.</span></span> <span data-ttu-id="b6089-109">Například použít hello [portál Azure](container-registry-get-started-portal.md) nebo hello [Azure CLI 2.0](container-registry-get-started-azure-cli.md).</span><span class="sxs-lookup"><span data-stu-id="b6089-109">For example, use hello [Azure portal](container-registry-get-started-portal.md) or hello [Azure CLI 2.0](container-registry-get-started-azure-cli.md).</span></span>
* <span data-ttu-id="b6089-110">**Rozhraní příkazového řádku dockeru** -tooset do místního počítače jako Docker hostitele a přístup hello příkazového řádku Dockeru příkazy, nainstalujte [modulu Docker](https://docs.docker.com/engine/installation/).</span><span class="sxs-lookup"><span data-stu-id="b6089-110">**Docker CLI** - tooset up your local computer as a Docker host and access hello Docker CLI commands, install [Docker Engine](https://docs.docker.com/engine/installation/).</span></span>
* <span data-ttu-id="b6089-111">**Obrázek pro vyžádání obsahu** – bitovou kopii pro vyžádání obsahu z hello veřejného úložiště Docker Hub registru, se značkami a poslat ho tooyour registru.</span><span class="sxs-lookup"><span data-stu-id="b6089-111">**Pull an image** - Pull an image from hello public Docker Hub registry, tag it, and push it tooyour registry.</span></span> <span data-ttu-id="b6089-112">Návod push a bitové kopie pro vyžádání obsahu, najdete v části [Docker nabízené image tooAzure privátní registru](container-registry-get-started-docker-cli.md).</span><span class="sxs-lookup"><span data-stu-id="b6089-112">For guidance on how push and pull images, see [Push Docker image tooAzure private registry](container-registry-get-started-docker-cli.md).</span></span>


## <a name="viewing-repositories-in-hello-portal"></a><span data-ttu-id="b6089-113">Zobrazení úložiště v hello portálu</span><span class="sxs-lookup"><span data-stu-id="b6089-113">Viewing repositories in hello Portal</span></span>

<span data-ttu-id="b6089-114">Jakmile máte nabídnutých registru kontejneru tooyour bitové kopie, uvidíte seznam hello úložiště hostování hello obrázků v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b6089-114">Once you have pushed images tooyour container registry, you can see a list of hello repositories hosting hello images in hello Azure portal.</span></span>

<span data-ttu-id="b6089-115">Pokud jste postupovali podle kroků hello v hello [Push Docker image tooAzure privátní registru](container-registry-get-started-docker-cli.md) článku teď byste měli mít bitovou kopii Nginx v registru kontejneru.</span><span class="sxs-lookup"><span data-stu-id="b6089-115">If you followed hello steps in hello [Push Docker image tooAzure private registry](container-registry-get-started-docker-cli.md) article, you should now have a Nginx image in your container registry.</span></span> <span data-ttu-id="b6089-116">Jako součást hello pokyny by měl mít zadat obor názvů pro bitové kopie hello.</span><span class="sxs-lookup"><span data-stu-id="b6089-116">As part of hello instructions, you should have specified a namespace for hello image.</span></span> <span data-ttu-id="b6089-117">V příkladu hello níže hello příkaz nabízených oznámení hello NGinx image toohello "samples" úložiště:</span><span class="sxs-lookup"><span data-stu-id="b6089-117">In hello example below, hello command pushes hello NGinx image toohello "samples" repository:</span></span>

```
docker push myregistry.azurecr.io/samples/nginx
```
 <span data-ttu-id="b6089-118">Azure Container Registry podporuje víceúrovňové obory názvů úložiště.</span><span class="sxs-lookup"><span data-stu-id="b6089-118">Azure Container Registry supports multilevel repository namespaces.</span></span> <span data-ttu-id="b6089-119">Tato funkce umožňuje vám toogroup kolekce obrázků související tooa konkrétní aplikaci nebo kolekci toospecific vývoj aplikací nebo provozní týmy.</span><span class="sxs-lookup"><span data-stu-id="b6089-119">This feature enables you toogroup collections of images related tooa specific app, or a collection of apps toospecific development or operational teams.</span></span> <span data-ttu-id="b6089-120">tooread Další informace o úložiště v registrech kontejneru, najdete v části [registrech kontejner Docker privátní v Azure](container-registry-intro.md).</span><span class="sxs-lookup"><span data-stu-id="b6089-120">tooread more about repositories in container registries, see [Private Docker container registries in Azure](container-registry-intro.md).</span></span>

<span data-ttu-id="b6089-121">tooview hello kontejneru registru úložiště:</span><span class="sxs-lookup"><span data-stu-id="b6089-121">tooview hello container registry repositories:</span></span>

1. <span data-ttu-id="b6089-122">Přihlaste se toohello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="b6089-122">Log in toohello Azure portal</span></span>
2. <span data-ttu-id="b6089-123">Na hello **registru kontejner Azure** okně, vyberte hello registru chcete tooinspect</span><span class="sxs-lookup"><span data-stu-id="b6089-123">On hello **Azure Container Registry** blade, select hello registry you wish tooinspect</span></span>
3. <span data-ttu-id="b6089-124">V okně hello registru, klikněte na tlačítko **úložiště** toosee seznam všech hello úložiště a jejich obrázků</span><span class="sxs-lookup"><span data-stu-id="b6089-124">In hello registry blade, click **Repositories** toosee a list of all hello repositories and their images</span></span>
4. <span data-ttu-id="b6089-125">(Volitelné) Vyberte konkrétní image toosee značky</span><span class="sxs-lookup"><span data-stu-id="b6089-125">(Optional) Select a specific image toosee tags</span></span>

![Úložiště hello portálu](./media/container-registry-repositories/container-registry-repositories.png)


## <a name="next-steps"></a><span data-ttu-id="b6089-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b6089-127">Next steps</span></span>
<span data-ttu-id="b6089-128">Nyní když znáte hello základy, jste připravené toostart pomocí vašeho klíče registru!</span><span class="sxs-lookup"><span data-stu-id="b6089-128">Now that you know hello basics, you are ready toostart using your registry!</span></span> <span data-ttu-id="b6089-129">Například začít s nasazením Image tooan kontejneru [Azure Container Service](https://azure.microsoft.com/documentation/services/container-service/) clusteru.</span><span class="sxs-lookup"><span data-stu-id="b6089-129">For example, start deploying container images tooan [Azure Container Service](https://azure.microsoft.com/documentation/services/container-service/) cluster.</span></span>
