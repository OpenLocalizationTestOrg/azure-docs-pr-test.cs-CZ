---
title: "Vytvoření serveru služby Analysis Services v Azure | Microsoft Docs"
description: "Naučte se vytvořit instanci služby Analysis Services serveru v Azure."
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
ms.openlocfilehash: 95b367e7cd74405088190c1fe19cf92990759d90
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-an-azure-analysis-services-server-in-azure-portal"></a><span data-ttu-id="7e08a-103">Vytvoření serveru Azure Analysis Services na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="7e08a-103">Create an Azure Analysis Services server in Azure portal</span></span>
<span data-ttu-id="7e08a-104">Tento článek vás provede procesem vytvoření prostředek serveru služby Analysis Services ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="7e08a-104">This article walks you through creating an Analysis Services server resource in your Azure subscription.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="7e08a-105">Než začnete</span><span class="sxs-lookup"><span data-stu-id="7e08a-105">Before you begin</span></span>
<span data-ttu-id="7e08a-106">K dokončení tohoto rychlého startu je potřeba:</span><span class="sxs-lookup"><span data-stu-id="7e08a-106">To complete this quickstart, you need:</span></span>

* <span data-ttu-id="7e08a-107">**Předplatné Azure:** Pokud si chcete vytvořit účet, přejděte na stránku [Bezplatný zkušební verze Azure](https://azure.microsoft.com/offers/ms-azr-0044p/).</span><span class="sxs-lookup"><span data-stu-id="7e08a-107">**Azure subscription**: Visit [Azure Free Trial](https://azure.microsoft.com/offers/ms-azr-0044p/) to create an account.</span></span>
* <span data-ttu-id="7e08a-108">**Azure Active Directory**: vaše předplatné musí být přidružen klienta služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="7e08a-108">**Azure Active Directory**: Your subscription must be associated with an Azure Active Directory tenant.</span></span> <span data-ttu-id="7e08a-109">A, musíte být přihlášeni do Azure pomocí účtu v této službě Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="7e08a-109">And, you need to be signed in to Azure with an account in that Azure Active Directory.</span></span> <span data-ttu-id="7e08a-110">Účty Microsoft nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="7e08a-110">Microsoft accounts are not supported.</span></span> <span data-ttu-id="7e08a-111">Další informace najdete v tématu [Ověřování a uživatelská oprávnění](analysis-services-manage-users.md).</span><span class="sxs-lookup"><span data-stu-id="7e08a-111">To learn more, see [Authentication and user permissions](analysis-services-manage-users.md).</span></span>
* <span data-ttu-id="7e08a-112">**Skupina prostředků**: už máte skupinu prostředků nebo [vytvořte novou](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="7e08a-112">**Resource group**: Use a resource group you already have or [create a new one](../azure-resource-manager/resource-group-overview.md).</span></span>

> [!NOTE]
> <span data-ttu-id="7e08a-113">Vytvoření serveru může znamenat, že se vám začne fakturovat nová služba.</span><span class="sxs-lookup"><span data-stu-id="7e08a-113">Creating a server might result in a new billable service.</span></span> <span data-ttu-id="7e08a-114">Další informace najdete v tématu [Ceny služby Azure Analysis Services](https://azure.microsoft.com/pricing/details/analysis-services/).</span><span class="sxs-lookup"><span data-stu-id="7e08a-114">To learn more, see [Analysis Services pricing](https://azure.microsoft.com/pricing/details/analysis-services/).</span></span>
> 
> 

## <a name="to-create-a-server-in-azure-portal"></a><span data-ttu-id="7e08a-115">Vytvoření serveru na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="7e08a-115">To create a server in Azure portal</span></span>
1. <span data-ttu-id="7e08a-116">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="7e08a-116">Sign in to the [Azure portal](https://portal.azure.com).</span></span>  
2. <span data-ttu-id="7e08a-117">Klikněte na tlačítko **+ nový** > **Data + analýzy** > **služby Analysis Services**.</span><span class="sxs-lookup"><span data-stu-id="7e08a-117">Click **+ New** > **Data + analytics** > **Analysis Services**.</span></span>
3. <span data-ttu-id="7e08a-118">V **služby Analysis Services** okně vyplňte požadovaná pole a stiskněte klávesu **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="7e08a-118">In the **Analysis Services** blade, fill in the required fields, and then press **Create**.</span></span>
   
    ![Vytvoření serveru](./media/analysis-services-create-server/aas-create-server-blade.png)
   
   * <span data-ttu-id="7e08a-120">**Název serveru**: Zadejte jedinečný název slouží k odkazování na server.</span><span class="sxs-lookup"><span data-stu-id="7e08a-120">**Server name**: Type a unique name used to reference the server.</span></span>
   * <span data-ttu-id="7e08a-121">**Předplatné**: Vyberte předplatné, tento server bills k.</span><span class="sxs-lookup"><span data-stu-id="7e08a-121">**Subscription**: Select the subscription this server bills to.</span></span>
   * <span data-ttu-id="7e08a-122">**Skupina prostředků**: Tyto kontejnery slouží ke správě kolekce prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="7e08a-122">**Resource group**: These containers are designed to help you manage a collection of Azure resources.</span></span> <span data-ttu-id="7e08a-123">Další informace najdete v tématu [skupiny prostředků](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="7e08a-123">To learn more, see [resource groups](../azure-resource-manager/resource-group-overview.md).</span></span>
   * <span data-ttu-id="7e08a-124">**Umístění**: Tento Azure datacenter umístění hostitelem serveru.</span><span class="sxs-lookup"><span data-stu-id="7e08a-124">**Location**: This Azure datacenter location hosts the server.</span></span> <span data-ttu-id="7e08a-125">Vyberte umístění pro nejbližší největší uživatelskou základnu.</span><span class="sxs-lookup"><span data-stu-id="7e08a-125">Choose a location nearest your largest user base.</span></span>
   * <span data-ttu-id="7e08a-126">**Cenová úroveň**: Vyberte cenovou úroveň.</span><span class="sxs-lookup"><span data-stu-id="7e08a-126">**Pricing tier**: Select a pricing tier.</span></span> <span data-ttu-id="7e08a-127">Tabulkové modely až 400 GB jsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="7e08a-127">Tabular models up to 400 GB are supported.</span></span> <span data-ttu-id="7e08a-128">Další informace najdete v tématu [ceny služby Azure Analysis Services](https://azure.microsoft.com/pricing/details/analysis-services/).</span><span class="sxs-lookup"><span data-stu-id="7e08a-128">To learn more, see [Azure Analysis Services pricing](https://azure.microsoft.com/pricing/details/analysis-services/).</span></span>
4. <span data-ttu-id="7e08a-129">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="7e08a-129">Click **Create**.</span></span>

<span data-ttu-id="7e08a-130">Vytvoření trvá obvykle pod minutu; často jenom pár sekund.</span><span class="sxs-lookup"><span data-stu-id="7e08a-130">Create usually takes under a minute; often just a few seconds.</span></span> <span data-ttu-id="7e08a-131">Pokud jste vybrali **přidávat na portál**, přejděte na portál zobrazíte nový server.</span><span class="sxs-lookup"><span data-stu-id="7e08a-131">If you selected **Add to Portal**, navigate to your portal to see your new server.</span></span> <span data-ttu-id="7e08a-132">Nebo přejděte na **další služby** > **služby Analysis Services** chcete zobrazit, pokud váš server je připraven.</span><span class="sxs-lookup"><span data-stu-id="7e08a-132">Or, navigate to **More services** > **Analysis Services** to see if your server is ready.</span></span>

 ![Řídicí panel](./media/analysis-services-create-server/aas-create-server-dashboard.png)


## <a name="next-steps"></a><span data-ttu-id="7e08a-134">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7e08a-134">Next steps</span></span>
<span data-ttu-id="7e08a-135">Po vytvoření serveru, můžete [nasadit model](analysis-services-deploy.md) k němu pomocí rozšíření SSDT nebo pomocí SSMS.</span><span class="sxs-lookup"><span data-stu-id="7e08a-135">Once you've created your server, you can [deploy a model](analysis-services-deploy.md) to it by using SSDT or with SSMS.</span></span>

<span data-ttu-id="7e08a-136">Pokud model nasadit na server se připojí k místním zdrojům dat, je potřeba nainstalovat [místní brána dat](analysis-services-gateway.md) na počítači v síti.</span><span class="sxs-lookup"><span data-stu-id="7e08a-136">If a model you deploy to your server connects to on-premises data sources, you need to install an [On-premises data gateway](analysis-services-gateway.md) on a computer in your network.</span></span>

