---
title: "Vytvoření soukromého registru Dockeru – Azure Portal | Dokumentace Microsoftu"
description: "Začínáme vytvářet a spravovat soukromé registry kontejnerů Dockeru pomocí webu Azure Portal"
services: container-registry
documentationcenter: 
author: stevelas
manager: balans
editor: dlepow
tags: 
keywords: 
ms.assetid: 53a3b3cb-ab4b-4560-bc00-366e2759f1a1
ms.service: container-registry
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/24/2017
ms.author: stevelas
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7fbbb56d775ee96c9a44363a4e41d4fc3c630582
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-private-docker-container-registry-using-the-azure-portal"></a><span data-ttu-id="352cd-103">Vytvoření soukromého registru kontejnerů Dockeru pomocí webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="352cd-103">Create a private Docker container registry using the Azure portal</span></span>
<span data-ttu-id="352cd-104">Pomocí webu Azure Portal můžete vytvořit registr kontejnerů a spravovat jeho nastavení.</span><span class="sxs-lookup"><span data-stu-id="352cd-104">Use the Azure portal to create a container registry and manage its settings.</span></span> <span data-ttu-id="352cd-105">Vytvořit a spravovat registry kontejnerů můžete také pomocí [příkazů Azure CLI 2.0](container-registry-get-started-azure-cli.md), [Azure PowerShellu](container-registry-get-started-powershell.md) nebo programově pomocí rozhraní [REST API](https://go.microsoft.com/fwlink/p/?linkid=834376) služby Container Registry.</span><span class="sxs-lookup"><span data-stu-id="352cd-105">You can also create and manage container registries using the [Azure CLI 2.0 commands](container-registry-get-started-azure-cli.md), [Azure PowerShell](container-registry-get-started-powershell.md) or programmatically with the Container Registry [REST API](https://go.microsoft.com/fwlink/p/?linkid=834376).</span></span>

<span data-ttu-id="352cd-106">Související informace a koncepty najdete v tématu [s přehledem](container-registry-intro.md).</span><span class="sxs-lookup"><span data-stu-id="352cd-106">For background and concepts, see [the overview](container-registry-intro.md).</span></span>

## <a name="create-a-container-registry"></a><span data-ttu-id="352cd-107">Vytvoření registru kontejnerů</span><span class="sxs-lookup"><span data-stu-id="352cd-107">Create a container registry</span></span>
1. <span data-ttu-id="352cd-108">Na webu [Azure Portal](https://portal.azure.com) klikněte na **+ Nový**.</span><span class="sxs-lookup"><span data-stu-id="352cd-108">In the [Azure portal](https://portal.azure.com), click **+ New**.</span></span>
2. <span data-ttu-id="352cd-109">V Marketplace vyhledejte **Azure container registry**.</span><span class="sxs-lookup"><span data-stu-id="352cd-109">Search the marketplace for **Azure container registry**.</span></span>
3. <span data-ttu-id="352cd-110">Vyberte **Azure Container Registry** s vydavatelem **Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="352cd-110">Select **Azure Container Registry**, with publisher **Microsoft**.</span></span>
    <span data-ttu-id="352cd-111">![Služba Container Registry v Azure Marketplace](./media/container-registry-get-started-portal/container-registry-marketplace.png)</span><span class="sxs-lookup"><span data-stu-id="352cd-111">![Container Registry service in Azure Marketplace](./media/container-registry-get-started-portal/container-registry-marketplace.png)</span></span>
4. <span data-ttu-id="352cd-112">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="352cd-112">Click **Create**.</span></span> <span data-ttu-id="352cd-113">Otevře se okno **Azure Container Registry**.</span><span class="sxs-lookup"><span data-stu-id="352cd-113">The **Azure Container Registry** blade appears.</span></span>

    ![Nastavení registru kontejnerů](./media/container-registry-get-started-portal/container-registry-settings.png)
5. <span data-ttu-id="352cd-115">V okně **Azure Container Registry** zadejte následující informace.</span><span class="sxs-lookup"><span data-stu-id="352cd-115">In the **Azure Container Registry** blade, enter the following information.</span></span> <span data-ttu-id="352cd-116">Až budete hotovi, klikněte na **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="352cd-116">Click **Create** when you are done.</span></span>

    <span data-ttu-id="352cd-117">a.</span><span class="sxs-lookup"><span data-stu-id="352cd-117">a.</span></span> <span data-ttu-id="352cd-118">**Název registru:** Globálně jedinečný název domény nejvyšší úrovně konkrétního registru.</span><span class="sxs-lookup"><span data-stu-id="352cd-118">**Registry name**: A globally unique top-level domain name for your specific registry.</span></span> <span data-ttu-id="352cd-119">V tomto příkladu je název registru *myRegistry01*, ale nahraďte jej vlastním jedinečným názvem.</span><span class="sxs-lookup"><span data-stu-id="352cd-119">In this example, the registry name is *myRegistry01*, but substitute a unique name of your own.</span></span> <span data-ttu-id="352cd-120">Název může obsahovat pouze písmena a číslice.</span><span class="sxs-lookup"><span data-stu-id="352cd-120">The name can contain only letters and numbers.</span></span>

    <span data-ttu-id="352cd-121">b.</span><span class="sxs-lookup"><span data-stu-id="352cd-121">b.</span></span> <span data-ttu-id="352cd-122">**Skupina prostředků:** Vyberte existující [skupinu prostředků](../azure-resource-manager/resource-group-overview.md#resource-groups) nebo zadejte název nové skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="352cd-122">**Resource group**: Select an existing [resource group](../azure-resource-manager/resource-group-overview.md#resource-groups) or type the name for a new one.</span></span>

    <span data-ttu-id="352cd-123">c.</span><span class="sxs-lookup"><span data-stu-id="352cd-123">c.</span></span> <span data-ttu-id="352cd-124">**Umístění:** Vyberte umístění datového centra Azure, ve kterém je tato služba [dostupná](https://azure.microsoft.com/regions/services/), například **Střed USA – jih**.</span><span class="sxs-lookup"><span data-stu-id="352cd-124">**Location**: Select an Azure datacenter location where the service is [available](https://azure.microsoft.com/regions/services/), such as **South Central US**.</span></span>

    <span data-ttu-id="352cd-125">d.</span><span class="sxs-lookup"><span data-stu-id="352cd-125">d.</span></span> <span data-ttu-id="352cd-126">**Uživatel s právy pro správu:** Pokud chcete, povolte uživateli s právy pro správu přístup k registru.</span><span class="sxs-lookup"><span data-stu-id="352cd-126">**Admin user**: If you want, enable an admin user to access the registry.</span></span> <span data-ttu-id="352cd-127">Toto nastavení můžete po vytvoření registru změnit.</span><span class="sxs-lookup"><span data-stu-id="352cd-127">You can change this setting after creating the registry.</span></span>

      > [!IMPORTANT]
      > <span data-ttu-id="352cd-128">Kromě poskytování přístupu prostřednictvím účtu uživatele s právy pro správu registry kontejnerů podporují ověřování zajišťované instančními objekty Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="352cd-128">In addition to providing access through an admin user account, container registries support authentication backed by Azure Active Directory service principals.</span></span> <span data-ttu-id="352cd-129">Další informace a aspekty najdete v tématu [Ověřování pomocí registru kontejnerů](container-registry-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="352cd-129">For more information and considerations, see [Authenticate with a container registry](container-registry-authentication.md).</span></span>
      >

    <span data-ttu-id="352cd-130">e.</span><span class="sxs-lookup"><span data-stu-id="352cd-130">e.</span></span> <span data-ttu-id="352cd-131">**Účet úložiště:** Použijte výchozí nastavení a vytvořte si [účet úložiště](../storage/common/storage-introduction.md), nebo vyberte existující účet úložiště ve stejném umístění.</span><span class="sxs-lookup"><span data-stu-id="352cd-131">**Storage account**: Use the default setting to create a [storage account](../storage/common/storage-introduction.md), or select an existing storage account in the same location.</span></span> <span data-ttu-id="352cd-132">Storage úrovně Premium se v tuto chvíli nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="352cd-132">Currently Premium Storage is not supported.</span></span>

## <a name="manage-registry-settings"></a><span data-ttu-id="352cd-133">Správa nastavení registru</span><span class="sxs-lookup"><span data-stu-id="352cd-133">Manage registry settings</span></span>
<span data-ttu-id="352cd-134">Po vytvoření registru začněte v okně portálu **Registry kontejnerů** a vyhledejte nastavení registru.</span><span class="sxs-lookup"><span data-stu-id="352cd-134">After creating the registry, find the registry settings by starting at the **Container Registries** blade in the portal.</span></span> <span data-ttu-id="352cd-135">Nastavení můžete potřebovat například k přihlášení ke svému registru, nebo můžete chtít povolit nebo zakázat uživatele s právy pro správu.</span><span class="sxs-lookup"><span data-stu-id="352cd-135">For example, you might need the settings to log in to your registry, or you might want to enable or disable the admin user.</span></span>

1. <span data-ttu-id="352cd-136">V okně **Kontejnery registrů** klikněte na název svého registru.</span><span class="sxs-lookup"><span data-stu-id="352cd-136">On the **Container Registries** blade, click the name of your registry.</span></span>

    ![Okno registru kontejneru](./media/container-registry-get-started-portal/container-registry-blade.png)
2. <span data-ttu-id="352cd-138">Chcete-li spravovat nastavení přístupu, klikněte na **Přístupový klíč**.</span><span class="sxs-lookup"><span data-stu-id="352cd-138">To manage access settings, click **Access key**.</span></span>

    ![Přístup k registru kontejneru](./media/container-registry-get-started-portal/container-registry-access.png)
3. <span data-ttu-id="352cd-140">Všimněte si těchto nastavení:</span><span class="sxs-lookup"><span data-stu-id="352cd-140">Note the following settings:</span></span>

   * <span data-ttu-id="352cd-141">**Přihlašovací server** – Plně kvalifikovaný název, který slouží k přihlášení k registru.</span><span class="sxs-lookup"><span data-stu-id="352cd-141">**Login server** - The fully qualified name you use to log in to the registry.</span></span> <span data-ttu-id="352cd-142">V tomto příkladu je to `myregistry01.azurecr.io`.</span><span class="sxs-lookup"><span data-stu-id="352cd-142">In this example, it is `myregistry01.azurecr.io`.</span></span>
   * <span data-ttu-id="352cd-143">**Uživatel s právy pro správu** – Tímto přepínačem můžete povolit nebo zakázat uživatelský účet s právy pro správu registru.</span><span class="sxs-lookup"><span data-stu-id="352cd-143">**Admin user** - Toggle to enable or disable the registry's admin user account.</span></span>
   * <span data-ttu-id="352cd-144">**Uživatelské jméno** a **Heslo** – Přihlašovací údaje uživatelského účtu s právy pro správu (pokud je povolený), pomocí kterých se můžete přihlásit k registru.</span><span class="sxs-lookup"><span data-stu-id="352cd-144">**Username** and **Password** - The credentials of the admin user account (if enabled) you can use to log in to the registry.</span></span> <span data-ttu-id="352cd-145">Hesla můžete volitelně vygenerovat znovu.</span><span class="sxs-lookup"><span data-stu-id="352cd-145">You can optionally regenerate the passwords.</span></span> <span data-ttu-id="352cd-146">Vytvoří se dvě hesla, abyste mohli spravovat připojení k registru pomocí jednoho hesla, zatímco znovu vygenerujete druhé heslo.</span><span class="sxs-lookup"><span data-stu-id="352cd-146">Two passwords are created so that you can maintain connections to the registry by using one password while you regenerate the other password.</span></span> <span data-ttu-id="352cd-147">Pokud chcete naopak provést ověření pomocí instančního objektu, přečtěte si popis [ověření pomocí privátního registru kontejnerů Dockeru](container-registry-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="352cd-147">To authenticate with a service principal instead, see [Authenticate with a private Docker container registry](container-registry-authentication.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="352cd-148">Další kroky</span><span class="sxs-lookup"><span data-stu-id="352cd-148">Next steps</span></span>
* [<span data-ttu-id="352cd-149">Nahrání první image pomocí rozhraní příkazového řádku Dockeru</span><span class="sxs-lookup"><span data-stu-id="352cd-149">Push your first image using the Docker CLI</span></span>](container-registry-get-started-docker-cli.md)
