---
title: "Ověřování pomocí služby Azure kontejneru registru | Microsoft Docs"
description: "Jak se přihlásit Azure kontejneru registru, pomocí účtu správce nebo objektu služby Azure Active Directory"
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
ms.openlocfilehash: aa2a6bf3d7d9ec22020036851fc0f2bca37e31bf
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="authenticate-with-a-private-docker-container-registry"></a><span data-ttu-id="9213a-103">Ověření pomocí privátní registru kontejner Docker</span><span class="sxs-lookup"><span data-stu-id="9213a-103">Authenticate with a private Docker container registry</span></span>
<span data-ttu-id="9213a-104">Pro práci s obrázky kontejneru v registru kontejner Azure, je přihlásit pomocí `docker login` příkaz.</span><span class="sxs-lookup"><span data-stu-id="9213a-104">To work with container images in an Azure container registry, you log in using the `docker login` command.</span></span> <span data-ttu-id="9213a-105">Můžete se přihlásit pomocí  **[objektu služby Azure Active Directory](../active-directory/active-directory-application-objects.md)**  nebo konkrétního registru **účet správce**.</span><span class="sxs-lookup"><span data-stu-id="9213a-105">You can log in using either an **[Azure Active Directory service principal](../active-directory/active-directory-application-objects.md)** or a registry-specific **admin account**.</span></span> <span data-ttu-id="9213a-106">Tento článek obsahuje více podrobností o těchto identit.</span><span class="sxs-lookup"><span data-stu-id="9213a-106">This article provides more detail about these identities.</span></span>



## <a name="service-principal"></a><span data-ttu-id="9213a-107">Instanční objekt</span><span class="sxs-lookup"><span data-stu-id="9213a-107">Service principal</span></span>

<span data-ttu-id="9213a-108">Můžete [přiřadit objekt služby](container-registry-get-started-azure-cli.md#assign-a-service-principal) do registru a použít jej pro základní ověřování Docker.</span><span class="sxs-lookup"><span data-stu-id="9213a-108">You can [assign a service principal](container-registry-get-started-azure-cli.md#assign-a-service-principal) to your registry and use it for basic Docker authentication.</span></span> <span data-ttu-id="9213a-109">Při používání objektu služby se doporučuje pro většinu scénářů.</span><span class="sxs-lookup"><span data-stu-id="9213a-109">Using a service principal is recommended for most scenarios.</span></span> <span data-ttu-id="9213a-110">Zadejte ID aplikace a heslo k objektu zabezpečení služby `docker login` příkaz, jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="9213a-110">Provide the app ID and password of the service principal to the `docker login` command, as shown in the following example:</span></span>

```
docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
```

<span data-ttu-id="9213a-111">Po přihlášení, Docker ukládá do mezipaměti přihlašovací údaje, takže nemusíte pamatovat ID aplikace.</span><span class="sxs-lookup"><span data-stu-id="9213a-111">Once logged in, Docker caches the credentials, so you don't need to remember the app ID.</span></span>

> [!TIP]
> <span data-ttu-id="9213a-112">Pokud chcete, můžete obnovit heslo objekt služby spuštěním `az ad sp reset-credentials` příkaz.</span><span class="sxs-lookup"><span data-stu-id="9213a-112">If you want, you can regenerate the password of a service principal by running the `az ad sp reset-credentials` command.</span></span>
>


<span data-ttu-id="9213a-113">Objekty služby povolit [přístupu podle rolí](../active-directory/role-based-access-control-configure.md) do registru.</span><span class="sxs-lookup"><span data-stu-id="9213a-113">Service principals allow [role-based access](../active-directory/role-based-access-control-configure.md) to a registry.</span></span> <span data-ttu-id="9213a-114">Dostupné role jsou:</span><span class="sxs-lookup"><span data-stu-id="9213a-114">Available roles are:</span></span>
  * <span data-ttu-id="9213a-115">Čtečka (pouze přístup k vyžádání obsahu).</span><span class="sxs-lookup"><span data-stu-id="9213a-115">Reader (pull only access).</span></span>
  * <span data-ttu-id="9213a-116">Přispěvatel (pull a push).</span><span class="sxs-lookup"><span data-stu-id="9213a-116">Contributor (pull and push).</span></span>
  * <span data-ttu-id="9213a-117">Vlastník (pull, nabízená oznámení a přiřadit role jiným uživatelům).</span><span class="sxs-lookup"><span data-stu-id="9213a-117">Owner (pull, push, and assign roles to other users).</span></span>

<span data-ttu-id="9213a-118">Anonymní přístup není k dispozici na registrech kontejner Azure.</span><span class="sxs-lookup"><span data-stu-id="9213a-118">Anonymous access is not available on Azure Container Registries.</span></span> <span data-ttu-id="9213a-119">Pro veřejné bitové kopie můžete použít [úložiště Docker Hub](https://docs.docker.com/docker-hub/).</span><span class="sxs-lookup"><span data-stu-id="9213a-119">For public images you can use [Docker Hub](https://docs.docker.com/docker-hub/).</span></span>

<span data-ttu-id="9213a-120">Několik hlavních objektů služby lze přiřadit registru, který umožňuje definovat přístup pro různé uživatele nebo aplikace.</span><span class="sxs-lookup"><span data-stu-id="9213a-120">You can assign multiple service principals to a registry, which allows you to define access for different users or applications.</span></span> <span data-ttu-id="9213a-121">Objekty služby také povolit "bezobslužných" připojení k registru v vývojáře nebo DevOps scénářů, jako jsou následující příklady:</span><span class="sxs-lookup"><span data-stu-id="9213a-121">Service principals also enable "headless" connectivity to a registry in developer or DevOps scenarios such as the following examples:</span></span>

  * <span data-ttu-id="9213a-122">Nasazení kontejnerů z registru pro systémy orchestration včetně DC/OS, Docker Swarm a Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="9213a-122">Container deployments from a registry to orchestration systems including DC/OS, Docker Swarm and Kubernetes.</span></span> <span data-ttu-id="9213a-123">Můžete také pull kontejneru registrů související služby Azure, jako [Container Service](../container-service/index.yml), [služby App Service](../app-service/index.md), [Batch](../batch/index.md), [Service Fabric](/azure/service-fabric/)a další.</span><span class="sxs-lookup"><span data-stu-id="9213a-123">You can also pull container registries to related Azure services such as [Container Service](../container-service/index.yml), [App Service](../app-service/index.md), [Batch](../batch/index.md), [Service Fabric](/azure/service-fabric/), and others.</span></span>

  * <span data-ttu-id="9213a-124">Průběžnou integraci a nasazení řešení (například Visual Studio Team Services nebo volaných) které vytvářet bitové kopie kontejneru a vložit je registru.</span><span class="sxs-lookup"><span data-stu-id="9213a-124">Continuous integration and deployment solutions (such as Visual Studio Team Services or Jenkins) that build container images and push them to a registry.</span></span>





## <a name="admin-account"></a><span data-ttu-id="9213a-125">Účet správce</span><span class="sxs-lookup"><span data-stu-id="9213a-125">Admin account</span></span>
<span data-ttu-id="9213a-126">S každou registru, které vytvoříte se automaticky vytvoří účet správce.</span><span class="sxs-lookup"><span data-stu-id="9213a-126">With each registry you create, an admin account gets created automatically.</span></span> <span data-ttu-id="9213a-127">Ve výchozím nastavení je účet zakázán, ale můžete ji povolit a spravovat přihlašovací údaje, například prostřednictvím [portál](container-registry-get-started-portal.md#manage-registry-settings) nebo pomocí [příkazy Azure CLI 2.0](container-registry-get-started-azure-cli.md#manage-admin-credentials).</span><span class="sxs-lookup"><span data-stu-id="9213a-127">By default the account is disabled, but you can enable it and manage the credentials, for example through the [portal](container-registry-get-started-portal.md#manage-registry-settings) or using the [Azure CLI 2.0 commands](container-registry-get-started-azure-cli.md#manage-admin-credentials).</span></span> <span data-ttu-id="9213a-128">Každý účet správce se s dvě hesla, které mohou vytvořit znovu.</span><span class="sxs-lookup"><span data-stu-id="9213a-128">Each admin account is provided with two passwords, both of which can be regenerated.</span></span> <span data-ttu-id="9213a-129">Zadaná dvě hesla umožňují udržování připojení k registru pomocí jedno heslo, zatímco si znovu vygenerujete jiné heslo.</span><span class="sxs-lookup"><span data-stu-id="9213a-129">The two passwords allow you to maintain connections to the registry by using one password while you regenerate the other password.</span></span> <span data-ttu-id="9213a-130">Pokud je účet povolený, můžete předat uživatelské jméno a buď heslo k `docker login` příkazu pro základní ověřování do registru.</span><span class="sxs-lookup"><span data-stu-id="9213a-130">If the account is enabled, you can pass the user name and either password to the `docker login` command for basic authentication to the registry.</span></span> <span data-ttu-id="9213a-131">Například:</span><span class="sxs-lookup"><span data-stu-id="9213a-131">For example:</span></span>

```
docker login myregistry.azurecr.io -u myAdminName -p myPassword1
```

> [!IMPORTANT]
> <span data-ttu-id="9213a-132">Účet správce je určená pro jednoho uživatele pro přístup k registru, především pro účely testování.</span><span class="sxs-lookup"><span data-stu-id="9213a-132">The admin account is designed for a single user to access the registry, mainly for test purposes.</span></span> <span data-ttu-id="9213a-133">Není doporučeno sdílet mezi ostatní uživatelé přihlašovací údaje účtu správce.</span><span class="sxs-lookup"><span data-stu-id="9213a-133">It is not recommended to share the admin account credentials among other users.</span></span> <span data-ttu-id="9213a-134">Všichni uživatelé se zobrazí jako jednoho uživatele do registru.</span><span class="sxs-lookup"><span data-stu-id="9213a-134">All users appear as a single user to the registry.</span></span> <span data-ttu-id="9213a-135">Změna nebo zakázání účtu zakáže přístup k registru pro všechny uživatele, kteří používají přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="9213a-135">Changing or disabling this account disables registry access for all users who use the credentials.</span></span>
>


### <a name="next-steps"></a><span data-ttu-id="9213a-136">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9213a-136">Next steps</span></span>
* <span data-ttu-id="9213a-137">[Push vaší první image pomocí rozhraní příkazového řádku Dockeru](container-registry-get-started-docker-cli.md).</span><span class="sxs-lookup"><span data-stu-id="9213a-137">[Push your first image using the Docker CLI](container-registry-get-started-docker-cli.md).</span></span>
* <span data-ttu-id="9213a-138">Další informace o ověřování v kontejneru registru ve verzi preview, najdete v článku [příspěvku na blogu](https://blogs.msdn.microsoft.com/stevelasker/2016/11/17/azure-container-registry-user-accounts/).</span><span class="sxs-lookup"><span data-stu-id="9213a-138">For more information about authentication in the Container Registry preview, see the [blog post](https://blogs.msdn.microsoft.com/stevelasker/2016/11/17/azure-container-registry-user-accounts/).</span></span>
