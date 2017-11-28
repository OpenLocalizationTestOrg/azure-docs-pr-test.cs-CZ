---
title: aaaAuthenticate s registru kontejner Azure | Microsoft Docs
description: "Jak služba toolog v registru tooan kontejner Azure pomocí Azure Active Directory objekt zabezpečení nebo účet správce"
services: container-registry
documentationcenter: 
author: stevelas
manager: balans
editor: cristyg
tags: 
keywords: 
ms.assetid: 128a937a-766a-415b-b9fc-35a6c2f27417
ms.service: container-registry
ms.devlang: na
ms.topic: how-to-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/24/2017
ms.author: stevelas
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a0b0462e8432b2567689debca322e2426baa7fa2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="authenticate-with-a-private-docker-container-registry"></a><span data-ttu-id="9591d-103">Ověření pomocí privátní registru kontejner Docker</span><span class="sxs-lookup"><span data-stu-id="9591d-103">Authenticate with a private Docker container registry</span></span>
<span data-ttu-id="9591d-104">toowork s obrázky kontejneru v registru kontejner Azure, můžete přihlásit pomocí hello `docker login` příkaz.</span><span class="sxs-lookup"><span data-stu-id="9591d-104">toowork with container images in an Azure container registry, you log in using hello `docker login` command.</span></span> <span data-ttu-id="9591d-105">Můžete se přihlásit pomocí  **[objektu služby Azure Active Directory](../active-directory/active-directory-application-objects.md)**  nebo konkrétního registru **účet správce**.</span><span class="sxs-lookup"><span data-stu-id="9591d-105">You can log in using either an **[Azure Active Directory service principal](../active-directory/active-directory-application-objects.md)** or a registry-specific **admin account**.</span></span> <span data-ttu-id="9591d-106">Tento článek obsahuje více podrobností o těchto identit.</span><span class="sxs-lookup"><span data-stu-id="9591d-106">This article provides more detail about these identities.</span></span>



## <a name="service-principal"></a><span data-ttu-id="9591d-107">Instanční objekt</span><span class="sxs-lookup"><span data-stu-id="9591d-107">Service principal</span></span>

<span data-ttu-id="9591d-108">Můžete [přiřadit objekt služby](container-registry-get-started-azure-cli.md#assign-a-service-principal) tooyour registru a použít jej pro základní ověřování Docker.</span><span class="sxs-lookup"><span data-stu-id="9591d-108">You can [assign a service principal](container-registry-get-started-azure-cli.md#assign-a-service-principal) tooyour registry and use it for basic Docker authentication.</span></span> <span data-ttu-id="9591d-109">Při používání objektu služby se doporučuje pro většinu scénářů.</span><span class="sxs-lookup"><span data-stu-id="9591d-109">Using a service principal is recommended for most scenarios.</span></span> <span data-ttu-id="9591d-110">Zadejte ID aplikace hello a heslo hello služby hlavní toohello `docker login` příkaz, jak je znázorněno v hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="9591d-110">Provide hello app ID and password of hello service principal toohello `docker login` command, as shown in hello following example:</span></span>

```
docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
```

<span data-ttu-id="9591d-111">Po přihlášení, Docker ukládá do mezipaměti přihlašovací údaje hello, takže není nutné ID tooremember hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="9591d-111">Once logged in, Docker caches hello credentials, so you don't need tooremember hello app ID.</span></span>

> [!TIP]
> <span data-ttu-id="9591d-112">Pokud chcete, můžete obnovit heslo hello objekt služby spuštěním hello `az ad sp reset-credentials` příkaz.</span><span class="sxs-lookup"><span data-stu-id="9591d-112">If you want, you can regenerate hello password of a service principal by running hello `az ad sp reset-credentials` command.</span></span>
>


<span data-ttu-id="9591d-113">Objekty služby povolit [přístupu podle rolí](../active-directory/role-based-access-control-configure.md) tooa registru.</span><span class="sxs-lookup"><span data-stu-id="9591d-113">Service principals allow [role-based access](../active-directory/role-based-access-control-configure.md) tooa registry.</span></span> <span data-ttu-id="9591d-114">Dostupné role jsou:</span><span class="sxs-lookup"><span data-stu-id="9591d-114">Available roles are:</span></span>
  * <span data-ttu-id="9591d-115">Čtečka (pouze přístup k vyžádání obsahu).</span><span class="sxs-lookup"><span data-stu-id="9591d-115">Reader (pull only access).</span></span>
  * <span data-ttu-id="9591d-116">Přispěvatel (pull a push).</span><span class="sxs-lookup"><span data-stu-id="9591d-116">Contributor (pull and push).</span></span>
  * <span data-ttu-id="9591d-117">Vlastník (pull, nabízená oznámení a přiřadit role tooother uživatele).</span><span class="sxs-lookup"><span data-stu-id="9591d-117">Owner (pull, push, and assign roles tooother users).</span></span>

<span data-ttu-id="9591d-118">Anonymní přístup není k dispozici na registrech kontejner Azure.</span><span class="sxs-lookup"><span data-stu-id="9591d-118">Anonymous access is not available on Azure Container Registries.</span></span> <span data-ttu-id="9591d-119">Pro veřejné bitové kopie můžete použít [úložiště Docker Hub](https://docs.docker.com/docker-hub/).</span><span class="sxs-lookup"><span data-stu-id="9591d-119">For public images you can use [Docker Hub](https://docs.docker.com/docker-hub/).</span></span>

<span data-ttu-id="9591d-120">Můžete přiřadit více objekty služby tooa registru, která vám umožní přístup toodefine pro různé uživatele nebo aplikace.</span><span class="sxs-lookup"><span data-stu-id="9591d-120">You can assign multiple service principals tooa registry, which allows you toodefine access for different users or applications.</span></span> <span data-ttu-id="9591d-121">Objekty služby také povolit "bezobslužných" připojení k registru tooa vývojáře nebo DevOps scénáře, jako je hello následující příklady:</span><span class="sxs-lookup"><span data-stu-id="9591d-121">Service principals also enable "headless" connectivity tooa registry in developer or DevOps scenarios such as hello following examples:</span></span>

  * <span data-ttu-id="9591d-122">Nasazení kontejnerů ze systémů registru tooorchestration včetně DC/OS, Docker Swarm a Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="9591d-122">Container deployments from a registry tooorchestration systems including DC/OS, Docker Swarm and Kubernetes.</span></span> <span data-ttu-id="9591d-123">Můžete také posunout toorelated registrech kontejneru služby Azure, jako [Container Service](../container-service/index.yml), [služby App Service](../app-service/index.md), [Batch](../batch/index.md), [Service Fabric](/azure/service-fabric/)a další.</span><span class="sxs-lookup"><span data-stu-id="9591d-123">You can also pull container registries toorelated Azure services such as [Container Service](../container-service/index.yml), [App Service](../app-service/index.md), [Batch](../batch/index.md), [Service Fabric](/azure/service-fabric/), and others.</span></span>

  * <span data-ttu-id="9591d-124">Průběžnou integraci a nasazení řešení (například Visual Studio Team Services nebo volaných) vytvářet bitové kopie kontejneru a vložit je tooa registru.</span><span class="sxs-lookup"><span data-stu-id="9591d-124">Continuous integration and deployment solutions (such as Visual Studio Team Services or Jenkins) that build container images and push them tooa registry.</span></span>





## <a name="admin-account"></a><span data-ttu-id="9591d-125">Účet správce</span><span class="sxs-lookup"><span data-stu-id="9591d-125">Admin account</span></span>
<span data-ttu-id="9591d-126">S každou registru, které vytvoříte se automaticky vytvoří účet správce.</span><span class="sxs-lookup"><span data-stu-id="9591d-126">With each registry you create, an admin account gets created automatically.</span></span> <span data-ttu-id="9591d-127">Ve výchozím nastavení je účet hello zakázat, ale můžete ji povolit a spravovat hello přihlašovací údaje, například prostřednictvím hello [portál](container-registry-get-started-portal.md#manage-registry-settings) nebo pomocí hello [příkazy Azure CLI 2.0](container-registry-get-started-azure-cli.md#manage-admin-credentials).</span><span class="sxs-lookup"><span data-stu-id="9591d-127">By default hello account is disabled, but you can enable it and manage hello credentials, for example through hello [portal](container-registry-get-started-portal.md#manage-registry-settings) or using hello [Azure CLI 2.0 commands](container-registry-get-started-azure-cli.md#manage-admin-credentials).</span></span> <span data-ttu-id="9591d-128">Každý účet správce se s dvě hesla, které mohou vytvořit znovu.</span><span class="sxs-lookup"><span data-stu-id="9591d-128">Each admin account is provided with two passwords, both of which can be regenerated.</span></span> <span data-ttu-id="9591d-129">dvě hesla Hello povolit toomaintain připojení toohello registru s využitím jedno heslo, zatímco si znovu vygenerujete hello jiné heslo.</span><span class="sxs-lookup"><span data-stu-id="9591d-129">hello two passwords allow you toomaintain connections toohello registry by using one password while you regenerate hello other password.</span></span> <span data-ttu-id="9591d-130">Pokud je povoleno hello účet, můžete předat hello uživatelské jméno a buď heslo toohello `docker login` příkazu pro základní ověřování toohello registru.</span><span class="sxs-lookup"><span data-stu-id="9591d-130">If hello account is enabled, you can pass hello user name and either password toohello `docker login` command for basic authentication toohello registry.</span></span> <span data-ttu-id="9591d-131">Například:</span><span class="sxs-lookup"><span data-stu-id="9591d-131">For example:</span></span>

```
docker login myregistry.azurecr.io -u myAdminName -p myPassword1
```

> [!IMPORTANT]
> <span data-ttu-id="9591d-132">účet správce Hello je určená pro registru hello tooaccess jednoho uživatele, především pro účely testování.</span><span class="sxs-lookup"><span data-stu-id="9591d-132">hello admin account is designed for a single user tooaccess hello registry, mainly for test purposes.</span></span> <span data-ttu-id="9591d-133">Není doporučeno tooshare přihlašovací údaje účtu správce hello mezi jiných uživatelů.</span><span class="sxs-lookup"><span data-stu-id="9591d-133">It is not recommended tooshare hello admin account credentials among other users.</span></span> <span data-ttu-id="9591d-134">Všichni uživatelé se zobrazí jako soubor registru toohello jednoho uživatele.</span><span class="sxs-lookup"><span data-stu-id="9591d-134">All users appear as a single user toohello registry.</span></span> <span data-ttu-id="9591d-135">Změna nebo zakázání účtu zakáže přístup k registru pro všechny uživatele, kteří používají přihlašovací údaje hello.</span><span class="sxs-lookup"><span data-stu-id="9591d-135">Changing or disabling this account disables registry access for all users who use hello credentials.</span></span>
>


### <a name="next-steps"></a><span data-ttu-id="9591d-136">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9591d-136">Next steps</span></span>
* <span data-ttu-id="9591d-137">[Push vaší první image pomocí příkazového řádku Dockeru hello](container-registry-get-started-docker-cli.md).</span><span class="sxs-lookup"><span data-stu-id="9591d-137">[Push your first image using hello Docker CLI](container-registry-get-started-docker-cli.md).</span></span>
* <span data-ttu-id="9591d-138">Další informace o ověřování ve verzi preview hello kontejneru registru najdete v tématu hello [příspěvku na blogu](https://blogs.msdn.microsoft.com/stevelasker/2016/11/17/azure-container-registry-user-accounts/).</span><span class="sxs-lookup"><span data-stu-id="9591d-138">For more information about authentication in hello Container Registry preview, see hello [blog post](https://blogs.msdn.microsoft.com/stevelasker/2016/11/17/azure-container-registry-user-accounts/).</span></span>
