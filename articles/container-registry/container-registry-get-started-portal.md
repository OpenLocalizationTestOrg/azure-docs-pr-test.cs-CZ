---
title: "aaaCreate privátní registru Docker - portálu Azure | Microsoft Docs"
description: "Začínáme vytváření a správě privátního registrech kontejner Docker s hello portálu Azure"
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
ms.openlocfilehash: 40f3ce44fea26e5fbeca865c9da6df55c2df9511
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-private-docker-container-registry-using-hello-azure-portal"></a><span data-ttu-id="f6647-103">Vytvořit privátní registru kontejner Docker pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="f6647-103">Create a private Docker container registry using hello Azure portal</span></span>
<span data-ttu-id="f6647-104">Použijte hello Azure portálu toocreate kontejneru registru a jeho nastavení.</span><span class="sxs-lookup"><span data-stu-id="f6647-104">Use hello Azure portal toocreate a container registry and manage its settings.</span></span> <span data-ttu-id="f6647-105">Můžete také vytvořit a spravovat pomocí hello registrech kontejneru [příkazy Azure CLI 2.0](container-registry-get-started-azure-cli.md), [prostředí Azure PowerShell](container-registry-get-started-powershell.md) nebo prostřednictvím kódu programu hello kontejneru registru [REST API](https://go.microsoft.com/fwlink/p/?linkid=834376).</span><span class="sxs-lookup"><span data-stu-id="f6647-105">You can also create and manage container registries using hello [Azure CLI 2.0 commands](container-registry-get-started-azure-cli.md), [Azure PowerShell](container-registry-get-started-powershell.md) or programmatically with hello Container Registry [REST API](https://go.microsoft.com/fwlink/p/?linkid=834376).</span></span>

<span data-ttu-id="f6647-106">Pozadí a koncepty najdete v tématu [hello přehled](container-registry-intro.md).</span><span class="sxs-lookup"><span data-stu-id="f6647-106">For background and concepts, see [hello overview](container-registry-intro.md).</span></span>

## <a name="create-a-container-registry"></a><span data-ttu-id="f6647-107">Vytvoření registru kontejnerů</span><span class="sxs-lookup"><span data-stu-id="f6647-107">Create a container registry</span></span>
1. <span data-ttu-id="f6647-108">V hello [portál Azure](https://portal.azure.com), klikněte na tlačítko **+ nový**.</span><span class="sxs-lookup"><span data-stu-id="f6647-108">In hello [Azure portal](https://portal.azure.com), click **+ New**.</span></span>
2. <span data-ttu-id="f6647-109">Hledání hello marketplace pro **kontejner Azure registru**.</span><span class="sxs-lookup"><span data-stu-id="f6647-109">Search hello marketplace for **Azure container registry**.</span></span>
3. <span data-ttu-id="f6647-110">Vyberte **Azure Container Registry** s vydavatelem **Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="f6647-110">Select **Azure Container Registry**, with publisher **Microsoft**.</span></span>
    <span data-ttu-id="f6647-111">![Služba Container Registry v Azure Marketplace](./media/container-registry-get-started-portal/container-registry-marketplace.png)</span><span class="sxs-lookup"><span data-stu-id="f6647-111">![Container Registry service in Azure Marketplace](./media/container-registry-get-started-portal/container-registry-marketplace.png)</span></span>
4. <span data-ttu-id="f6647-112">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="f6647-112">Click **Create**.</span></span> <span data-ttu-id="f6647-113">Hello **registru kontejner Azure** otevře se okno.</span><span class="sxs-lookup"><span data-stu-id="f6647-113">hello **Azure Container Registry** blade appears.</span></span>

    ![Nastavení registru kontejnerů](./media/container-registry-get-started-portal/container-registry-settings.png)
5. <span data-ttu-id="f6647-115">V hello **registru kontejner Azure** okno, zadejte následující informace hello.</span><span class="sxs-lookup"><span data-stu-id="f6647-115">In hello **Azure Container Registry** blade, enter hello following information.</span></span> <span data-ttu-id="f6647-116">Až budete hotovi, klikněte na **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="f6647-116">Click **Create** when you are done.</span></span>

    <span data-ttu-id="f6647-117">a.</span><span class="sxs-lookup"><span data-stu-id="f6647-117">a.</span></span> <span data-ttu-id="f6647-118">**Název registru:** Globálně jedinečný název domény nejvyšší úrovně konkrétního registru.</span><span class="sxs-lookup"><span data-stu-id="f6647-118">**Registry name**: A globally unique top-level domain name for your specific registry.</span></span> <span data-ttu-id="f6647-119">V tomto příkladu je název registru hello *myRegistry01*, ale nahraďte vlastní jedinečný název.</span><span class="sxs-lookup"><span data-stu-id="f6647-119">In this example, hello registry name is *myRegistry01*, but substitute a unique name of your own.</span></span> <span data-ttu-id="f6647-120">Hello název může obsahovat jenom písmena a číslice.</span><span class="sxs-lookup"><span data-stu-id="f6647-120">hello name can contain only letters and numbers.</span></span>

    <span data-ttu-id="f6647-121">b.</span><span class="sxs-lookup"><span data-stu-id="f6647-121">b.</span></span> <span data-ttu-id="f6647-122">**Skupina prostředků**: Vyberte existující [skupiny prostředků](../azure-resource-manager/resource-group-overview.md#resource-groups) nebo název typu hello nové.</span><span class="sxs-lookup"><span data-stu-id="f6647-122">**Resource group**: Select an existing [resource group](../azure-resource-manager/resource-group-overview.md#resource-groups) or type hello name for a new one.</span></span>

    <span data-ttu-id="f6647-123">c.</span><span class="sxs-lookup"><span data-stu-id="f6647-123">c.</span></span> <span data-ttu-id="f6647-124">**Umístění**: Vyberte datové centrum Azure umístění, kde je služba hello [k dispozici](https://azure.microsoft.com/regions/services/), jako například **jihu USA**.</span><span class="sxs-lookup"><span data-stu-id="f6647-124">**Location**: Select an Azure datacenter location where hello service is [available](https://azure.microsoft.com/regions/services/), such as **South Central US**.</span></span>

    <span data-ttu-id="f6647-125">d.</span><span class="sxs-lookup"><span data-stu-id="f6647-125">d.</span></span> <span data-ttu-id="f6647-126">**Uživatel s oprávněními správce**: Pokud chcete, povolení registru hello tooaccess uživatele správce.</span><span class="sxs-lookup"><span data-stu-id="f6647-126">**Admin user**: If you want, enable an admin user tooaccess hello registry.</span></span> <span data-ttu-id="f6647-127">Toto nastavení po vytvoření hello registru můžete změnit.</span><span class="sxs-lookup"><span data-stu-id="f6647-127">You can change this setting after creating hello registry.</span></span>

      > [!IMPORTANT]
      > <span data-ttu-id="f6647-128">Kromě toho tooproviding přístup prostřednictvím uživatelský účet správce, kontejner registrech podporu ověřování založenou na objekty služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="f6647-128">In addition tooproviding access through an admin user account, container registries support authentication backed by Azure Active Directory service principals.</span></span> <span data-ttu-id="f6647-129">Další informace a aspekty najdete v tématu [Ověřování pomocí registru kontejnerů](container-registry-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="f6647-129">For more information and considerations, see [Authenticate with a container registry](container-registry-authentication.md).</span></span>
      >

    <span data-ttu-id="f6647-130">e.</span><span class="sxs-lookup"><span data-stu-id="f6647-130">e.</span></span> <span data-ttu-id="f6647-131">**Účet úložiště**: použijte hello výchozí nastavení toocreate [účet úložiště](../storage/common/storage-introduction.md), nebo vybrat existující účet úložiště v hello stejné umístění.</span><span class="sxs-lookup"><span data-stu-id="f6647-131">**Storage account**: Use hello default setting toocreate a [storage account](../storage/common/storage-introduction.md), or select an existing storage account in hello same location.</span></span> <span data-ttu-id="f6647-132">Storage úrovně Premium se v tuto chvíli nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="f6647-132">Currently Premium Storage is not supported.</span></span>

## <a name="manage-registry-settings"></a><span data-ttu-id="f6647-133">Správa nastavení registru</span><span class="sxs-lookup"><span data-stu-id="f6647-133">Manage registry settings</span></span>
<span data-ttu-id="f6647-134">Po vytvoření hello registru, najde hello nastavení registru spuštěním v hello **kontejneru registrech** okna portálu hello.</span><span class="sxs-lookup"><span data-stu-id="f6647-134">After creating hello registry, find hello registry settings by starting at hello **Container Registries** blade in hello portal.</span></span> <span data-ttu-id="f6647-135">Například může být nutné hello toolog nastavení v registru tooyour, nebo může být vhodné tooenable nebo zakázat hello uživatel s oprávněními správce.</span><span class="sxs-lookup"><span data-stu-id="f6647-135">For example, you might need hello settings toolog in tooyour registry, or you might want tooenable or disable hello admin user.</span></span>

1. <span data-ttu-id="f6647-136">Na hello **kontejneru registrech** okně klikněte na název hello registru systému Windows.</span><span class="sxs-lookup"><span data-stu-id="f6647-136">On hello **Container Registries** blade, click hello name of your registry.</span></span>

    ![Okno registru kontejneru](./media/container-registry-get-started-portal/container-registry-blade.png)
2. <span data-ttu-id="f6647-138">Klikněte na tlačítko Nastavení přístupu k toomanage, **přístupový klíč**.</span><span class="sxs-lookup"><span data-stu-id="f6647-138">toomanage access settings, click **Access key**.</span></span>

    ![Přístup k registru kontejneru](./media/container-registry-get-started-portal/container-registry-access.png)
3. <span data-ttu-id="f6647-140">Všimněte si hello následující nastavení:</span><span class="sxs-lookup"><span data-stu-id="f6647-140">Note hello following settings:</span></span>

   * <span data-ttu-id="f6647-141">**Přihlášení na server** -hello použijete toolog v registru toohello plně kvalifikovaný název.</span><span class="sxs-lookup"><span data-stu-id="f6647-141">**Login server** - hello fully qualified name you use toolog in toohello registry.</span></span> <span data-ttu-id="f6647-142">V tomto příkladu je to `myregistry01.azurecr.io`.</span><span class="sxs-lookup"><span data-stu-id="f6647-142">In this example, it is `myregistry01.azurecr.io`.</span></span>
   * <span data-ttu-id="f6647-143">**Uživatel s oprávněními správce** – přepnutí tooenable nebo zakázat hello registru Správce uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="f6647-143">**Admin user** - Toggle tooenable or disable hello registry's admin user account.</span></span>
   * <span data-ttu-id="f6647-144">**Uživatelské jméno** a **heslo** -hello přihlašovací údaje účtu uživatele správce hello (Pokud je povoleno) můžete použít toolog v toohello registru.</span><span class="sxs-lookup"><span data-stu-id="f6647-144">**Username** and **Password** - hello credentials of hello admin user account (if enabled) you can use toolog in toohello registry.</span></span> <span data-ttu-id="f6647-145">Volitelně můžete obnovit hesla hello.</span><span class="sxs-lookup"><span data-stu-id="f6647-145">You can optionally regenerate hello passwords.</span></span> <span data-ttu-id="f6647-146">Dvě hesla jsou vytvořeny tak, aby můžete udržovat připojení toohello registru pomocí jedno heslo, zatímco si znovu vygenerujete hello jiné heslo.</span><span class="sxs-lookup"><span data-stu-id="f6647-146">Two passwords are created so that you can maintain connections toohello registry by using one password while you regenerate hello other password.</span></span> <span data-ttu-id="f6647-147">Místo toho najdete v části tooauthenticate pomocí objektu služby [ověřit s privátní registru kontejner Docker](container-registry-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="f6647-147">tooauthenticate with a service principal instead, see [Authenticate with a private Docker container registry](container-registry-authentication.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="f6647-148">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f6647-148">Next steps</span></span>
* [<span data-ttu-id="f6647-149">Push vaší první image pomocí příkazového řádku Dockeru hello</span><span class="sxs-lookup"><span data-stu-id="f6647-149">Push your first image using hello Docker CLI</span></span>](container-registry-get-started-docker-cli.md)
