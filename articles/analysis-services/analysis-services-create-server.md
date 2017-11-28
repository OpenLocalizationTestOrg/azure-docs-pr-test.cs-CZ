---
title: "aaaCreate serveru služby Analysis Services v Azure | Microsoft Docs"
description: "Zjistěte, jak instance toocreate serveru služby Analysis Services v Azure."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 7f560216-8a9a-4d06-852e-48cf24deab19
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: 3668f659039f79f3dd71498d1066e8682bf33228
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-analysis-services-server-in-azure-portal"></a><span data-ttu-id="6aa69-103">Vytvoření serveru Azure Analysis Services na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="6aa69-103">Create an Azure Analysis Services server in Azure portal</span></span>
<span data-ttu-id="6aa69-104">Tento článek vás provede procesem vytvoření prostředek serveru služby Analysis Services ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="6aa69-104">This article walks you through creating an Analysis Services server resource in your Azure subscription.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="6aa69-105">Než začnete</span><span class="sxs-lookup"><span data-stu-id="6aa69-105">Before you begin</span></span>
<span data-ttu-id="6aa69-106">toocomplete tento rychlý start, budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="6aa69-106">toocomplete this quickstart, you need:</span></span>

* <span data-ttu-id="6aa69-107">**Předplatné Azure**: navštivte [bezplatná zkušební verze Azure](https://azure.microsoft.com/offers/ms-azr-0044p/) toocreate účet.</span><span class="sxs-lookup"><span data-stu-id="6aa69-107">**Azure subscription**: Visit [Azure Free Trial](https://azure.microsoft.com/offers/ms-azr-0044p/) toocreate an account.</span></span>
* <span data-ttu-id="6aa69-108">**Azure Active Directory**: vaše předplatné musí být přidružen klienta služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="6aa69-108">**Azure Active Directory**: Your subscription must be associated with an Azure Active Directory tenant.</span></span> <span data-ttu-id="6aa69-109">A je třeba toobe přihlášení tooAzure s účtem v této službě Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="6aa69-109">And, you need toobe signed in tooAzure with an account in that Azure Active Directory.</span></span> <span data-ttu-id="6aa69-110">Účty Microsoft nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="6aa69-110">Microsoft accounts are not supported.</span></span> <span data-ttu-id="6aa69-111">Další, najdete v části toolearn [ověřování a uživatel oprávnění](analysis-services-manage-users.md).</span><span class="sxs-lookup"><span data-stu-id="6aa69-111">toolearn more, see [Authentication and user permissions](analysis-services-manage-users.md).</span></span>
* <span data-ttu-id="6aa69-112">**Skupina prostředků**: už máte skupinu prostředků nebo [vytvořte novou](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6aa69-112">**Resource group**: Use a resource group you already have or [create a new one](../azure-resource-manager/resource-group-overview.md).</span></span>

> [!NOTE]
> <span data-ttu-id="6aa69-113">Vytvoření serveru může znamenat, že se vám začne fakturovat nová služba.</span><span class="sxs-lookup"><span data-stu-id="6aa69-113">Creating a server might result in a new billable service.</span></span> <span data-ttu-id="6aa69-114">Další, najdete v části toolearn [ceny služby Analysis Services](https://azure.microsoft.com/pricing/details/analysis-services/).</span><span class="sxs-lookup"><span data-stu-id="6aa69-114">toolearn more, see [Analysis Services pricing](https://azure.microsoft.com/pricing/details/analysis-services/).</span></span>
> 
> 

## <a name="toocreate-a-server-in-azure-portal"></a><span data-ttu-id="6aa69-115">toocreate serveru na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="6aa69-115">toocreate a server in Azure portal</span></span>
1. <span data-ttu-id="6aa69-116">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="6aa69-116">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>  
2. <span data-ttu-id="6aa69-117">Klikněte na tlačítko **+ nový** > **Data + analýzy** > **služby Analysis Services**.</span><span class="sxs-lookup"><span data-stu-id="6aa69-117">Click **+ New** > **Data + analytics** > **Analysis Services**.</span></span>
3. <span data-ttu-id="6aa69-118">V hello **služby Analysis Services** okně vyplňte hello požadované pole a stiskněte klávesu **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="6aa69-118">In hello **Analysis Services** blade, fill in hello required fields, and then press **Create**.</span></span>
   
    ![Vytvoření serveru](./media/analysis-services-create-server/aas-create-server-blade.png)
   
   * <span data-ttu-id="6aa69-120">**Název serveru**: Zadejte jedinečný název používá tooreference hello serveru.</span><span class="sxs-lookup"><span data-stu-id="6aa69-120">**Server name**: Type a unique name used tooreference hello server.</span></span>
   * <span data-ttu-id="6aa69-121">**Předplatné**: Vyberte předplatné hello bills tento server k.</span><span class="sxs-lookup"><span data-stu-id="6aa69-121">**Subscription**: Select hello subscription this server bills to.</span></span>
   * <span data-ttu-id="6aa69-122">**Skupina prostředků**: Tyto kontejnery jsou navrženou toohelp správu kolekcí prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="6aa69-122">**Resource group**: These containers are designed toohelp you manage a collection of Azure resources.</span></span> <span data-ttu-id="6aa69-123">Další, najdete v části toolearn [skupiny prostředků](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6aa69-123">toolearn more, see [resource groups](../azure-resource-manager/resource-group-overview.md).</span></span>
   * <span data-ttu-id="6aa69-124">**Umístění**: Tento Azure datacenter umístění hostitele hello serveru.</span><span class="sxs-lookup"><span data-stu-id="6aa69-124">**Location**: This Azure datacenter location hosts hello server.</span></span> <span data-ttu-id="6aa69-125">Vyberte umístění pro nejbližší největší uživatelskou základnu.</span><span class="sxs-lookup"><span data-stu-id="6aa69-125">Choose a location nearest your largest user base.</span></span>
   * <span data-ttu-id="6aa69-126">**Cenová úroveň**: Vyberte cenovou úroveň.</span><span class="sxs-lookup"><span data-stu-id="6aa69-126">**Pricing tier**: Select a pricing tier.</span></span> <span data-ttu-id="6aa69-127">Jsou podporovány tabulkové modely až too400 GB.</span><span class="sxs-lookup"><span data-stu-id="6aa69-127">Tabular models up too400 GB are supported.</span></span> <span data-ttu-id="6aa69-128">Další, najdete v části toolearn [ceny služby Azure Analysis Services](https://azure.microsoft.com/pricing/details/analysis-services/).</span><span class="sxs-lookup"><span data-stu-id="6aa69-128">toolearn more, see [Azure Analysis Services pricing](https://azure.microsoft.com/pricing/details/analysis-services/).</span></span>
4. <span data-ttu-id="6aa69-129">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="6aa69-129">Click **Create**.</span></span>

<span data-ttu-id="6aa69-130">Vytvoření trvá obvykle pod minutu; často jenom pár sekund.</span><span class="sxs-lookup"><span data-stu-id="6aa69-130">Create usually takes under a minute; often just a few seconds.</span></span> <span data-ttu-id="6aa69-131">Pokud jste vybrali **přidat tooPortal**, přejděte portálu toosee tooyour nový server.</span><span class="sxs-lookup"><span data-stu-id="6aa69-131">If you selected **Add tooPortal**, navigate tooyour portal toosee your new server.</span></span> <span data-ttu-id="6aa69-132">Nebo přejděte příliš**další služby** > **služby Analysis Services** toosee Pokud váš server je připraven.</span><span class="sxs-lookup"><span data-stu-id="6aa69-132">Or, navigate too**More services** > **Analysis Services** toosee if your server is ready.</span></span>

 ![Řídicí panel](./media/analysis-services-create-server/aas-create-server-dashboard.png)


## <a name="next-steps"></a><span data-ttu-id="6aa69-134">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6aa69-134">Next steps</span></span>
<span data-ttu-id="6aa69-135">Po vytvoření serveru, můžete [nasadit model](analysis-services-deploy.md) tooit pomocí rozšíření SSDT nebo pomocí SSMS.</span><span class="sxs-lookup"><span data-stu-id="6aa69-135">Once you've created your server, you can [deploy a model](analysis-services-deploy.md) tooit by using SSDT or with SSMS.</span></span>

<span data-ttu-id="6aa69-136">Pokud model nasazení serveru tooyour připojí tooon místní zdroje dat, je třeba tooinstall [místní brána dat](analysis-services-gateway.md) na počítači v síti.</span><span class="sxs-lookup"><span data-stu-id="6aa69-136">If a model you deploy tooyour server connects tooon-premises data sources, you need tooinstall an [On-premises data gateway](analysis-services-gateway.md) on a computer in your network.</span></span>

