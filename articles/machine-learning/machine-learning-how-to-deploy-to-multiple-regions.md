---
title: "Postup nasazení webové služby do několika oblastí | Microsoft Docs"
description: "Postup nasazení (kopírování) novou webovou službu k jiné oblasti."
services: machine-learning
documentationcenter: 
author: vDonGlover
manager: raymondl
editor: cgronlun
ms.assetid: 36c60411-f2db-4ee2-9b66-b1f1d77a8f44
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: v-donglo
ms.openlocfilehash: 3895537bbca72e687838ff5013c291dfee3be707
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-deploy-a-web-service-to-multiple-regions"></a><span data-ttu-id="f908d-103">Jak nasadit webovou službu do více oblastí</span><span class="sxs-lookup"><span data-stu-id="f908d-103">How to deploy a Web Service to multiple regions</span></span>
<span data-ttu-id="f908d-104">Nové webové služby Azure umožňují snadno nasadit webovou službu do několika oblastí bez nutnosti více předplatných nebo pracovních prostorů.</span><span class="sxs-lookup"><span data-stu-id="f908d-104">The New Azure Web Services allow you to easily deploy a web service to multiple regions without needing multiple subscriptions or workspaces.</span></span> 

<span data-ttu-id="f908d-105">Ceny je oblast konkrétní, že proto je nutné definovat plán fakturace pro každou oblast, ve které budete nasazovat webové služby.</span><span class="sxs-lookup"><span data-stu-id="f908d-105">Pricing is region specific, therefore you must define a billing plan for each region in which you will deploy the web service.</span></span>

## <a name="to-create-a-plan-in-another-region"></a><span data-ttu-id="f908d-106">Vytvoření plánu v jiné oblasti</span><span class="sxs-lookup"><span data-stu-id="f908d-106">To create a plan in another region</span></span>
1. <span data-ttu-id="f908d-107">Přihlaste se k [Microsoft Azure Machine Learning webové služby](https://services.azureml.net/).</span><span class="sxs-lookup"><span data-stu-id="f908d-107">Sign into [Microsoft Azure Machine Learning Web Services](https://services.azureml.net/).</span></span>
2. <span data-ttu-id="f908d-108">Klikněte **plány** možnost nabídky.</span><span class="sxs-lookup"><span data-stu-id="f908d-108">Click the **Plans** menu option.</span></span>
3. <span data-ttu-id="f908d-109">V plánech přes stránka zobrazení, klikněte na tlačítko **nový**.</span><span class="sxs-lookup"><span data-stu-id="f908d-109">On the Plans over view page, click **New**.</span></span>
4. <span data-ttu-id="f908d-110">Z **předplatné** rozevíracího seznamu, vyberte předplatné, ve kterém se bude nacházet nový plán.</span><span class="sxs-lookup"><span data-stu-id="f908d-110">From the **Subscription** dropdown, select the subscription in which the new plan will reside.</span></span>
5. <span data-ttu-id="f908d-111">Z **oblast** rozevíracího seznamu, vyberte oblast pro nový plán.</span><span class="sxs-lookup"><span data-stu-id="f908d-111">From the **Region** dropdown, select a region for the new plan.</span></span> <span data-ttu-id="f908d-112">Možnosti plánování pro vybrané oblasti se zobrazí v **možnosti plánování** části stránky.</span><span class="sxs-lookup"><span data-stu-id="f908d-112">The Plan Options for the selected region will display in the **Plan Options** section of the page.</span></span>
6. <span data-ttu-id="f908d-113">Z **skupiny prostředků** rozevíracího seznamu, vyberte prostředek skupiny pro plán.</span><span class="sxs-lookup"><span data-stu-id="f908d-113">From the **Resource Group** dropdown, select a resource group for the plan.</span></span> <span data-ttu-id="f908d-114">Další informace o skupinách prostředků najdete v části [přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f908d-114">From more information on resource groups, see [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md).</span></span>
7. <span data-ttu-id="f908d-115">V **název plánu** zadejte název plánu.</span><span class="sxs-lookup"><span data-stu-id="f908d-115">In **Plan Name** type the name of the plan.</span></span>
8. <span data-ttu-id="f908d-116">V části **plán možnosti**, klikněte na úroveň fakturace pro nový plán.</span><span class="sxs-lookup"><span data-stu-id="f908d-116">Under **Plan Options**, click the billing level for the new plan.</span></span>
9. <span data-ttu-id="f908d-117">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="f908d-117">Click **Create**.</span></span>

## <a name="deploying-the-web-service-to-another-region"></a><span data-ttu-id="f908d-118">Nasazení webové služby pro jiné oblasti</span><span class="sxs-lookup"><span data-stu-id="f908d-118">Deploying the web service to another region</span></span>
1. <span data-ttu-id="f908d-119">Klikněte **webové služby** možnost nabídky.</span><span class="sxs-lookup"><span data-stu-id="f908d-119">Click the **Web Services** menu option.</span></span>
2. <span data-ttu-id="f908d-120">Vyberte webovou službu, kterou nasazujete do nové oblasti.</span><span class="sxs-lookup"><span data-stu-id="f908d-120">Select the Web Service you are deploying to a new region.</span></span>
3. <span data-ttu-id="f908d-121">Klikněte na tlačítko **kopie**.</span><span class="sxs-lookup"><span data-stu-id="f908d-121">Click **Copy**.</span></span>
4. <span data-ttu-id="f908d-122">V **název webové služby**, zadejte nový název pro webovou službu.</span><span class="sxs-lookup"><span data-stu-id="f908d-122">In **Web Service Name**, type a new name for the web service.</span></span>
5. <span data-ttu-id="f908d-123">V **webové služby popis**, zadejte popis pro webovou službu.</span><span class="sxs-lookup"><span data-stu-id="f908d-123">In **Web service description**, type a description for the web service.</span></span>
6. <span data-ttu-id="f908d-124">Z **předplatné** rozevíracího seznamu, vyberte předplatné, ve kterém se bude nacházet novou webovou službu.</span><span class="sxs-lookup"><span data-stu-id="f908d-124">From the **Subscription** dropdown, select the subscription in which the new web service will reside.</span></span>
7. <span data-ttu-id="f908d-125">Z **skupiny prostředků** rozevíracího seznamu, vyberte prostředek skupiny pro webovou službu.</span><span class="sxs-lookup"><span data-stu-id="f908d-125">From the **Resource Group** dropdown, select a resource group for the web service.</span></span> <span data-ttu-id="f908d-126">Další informace o skupinách prostředků najdete v části [přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f908d-126">From more information on resource groups, see [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md).</span></span>
8. <span data-ttu-id="f908d-127">Z **oblast** rozevíracího seznamu, vyberte oblast, ve které chcete nasadit webovou službu.</span><span class="sxs-lookup"><span data-stu-id="f908d-127">From the **Region** dropdown, select the region in which to deploy the web service.</span></span>
9. <span data-ttu-id="f908d-128">Z **účet úložiště** účet rozevíracího seznamu, vyberte úložiště pro uložení webovou službu.</span><span class="sxs-lookup"><span data-stu-id="f908d-128">From the **Storage account** dropdown, select a storage account in which to store the web service.</span></span>
10. <span data-ttu-id="f908d-129">Z **cena plán** rozevíracího seznamu, vyberte plán v oblasti, které jste vybrali v kroku 8.</span><span class="sxs-lookup"><span data-stu-id="f908d-129">From the **Price Plan** dropdown, select a plan in the region you selected in step 8.</span></span>
11. <span data-ttu-id="f908d-130">Klikněte na tlačítko **kopie**.</span><span class="sxs-lookup"><span data-stu-id="f908d-130">Click **Copy**.</span></span>

