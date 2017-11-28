---
title: "aaaHow toodeploy webové služby toomultiple oblasti | Microsoft Docs"
description: "Kroky toodeploy (kopie) oblasti tooother novou webovou službu."
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
ms.openlocfilehash: 21fcdb96f118c60ed98b60b1b2df833766c7c8bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodeploy-a-web-service-toomultiple-regions"></a><span data-ttu-id="207dc-103">Jak toodeploy webové služby toomultiple oblastí</span><span class="sxs-lookup"><span data-stu-id="207dc-103">How toodeploy a Web Service toomultiple regions</span></span>
<span data-ttu-id="207dc-104">Hello nové webové služby Azure umožňují tooeasily nasazení webové služby toomultiple oblastí bez nutnosti více předplatných nebo pracovních prostorů.</span><span class="sxs-lookup"><span data-stu-id="207dc-104">hello New Azure Web Services allow you tooeasily deploy a web service toomultiple regions without needing multiple subscriptions or workspaces.</span></span> 

<span data-ttu-id="207dc-105">Ceny je oblast konkrétní, že proto je nutné definovat plán fakturace pro každou oblast, ve které budete nasazovat hello webové služby.</span><span class="sxs-lookup"><span data-stu-id="207dc-105">Pricing is region specific, therefore you must define a billing plan for each region in which you will deploy hello web service.</span></span>

## <a name="toocreate-a-plan-in-another-region"></a><span data-ttu-id="207dc-106">toocreate plán v jiné oblasti</span><span class="sxs-lookup"><span data-stu-id="207dc-106">toocreate a plan in another region</span></span>
1. <span data-ttu-id="207dc-107">Přihlaste se k [Microsoft Azure Machine Learning webové služby](https://services.azureml.net/).</span><span class="sxs-lookup"><span data-stu-id="207dc-107">Sign into [Microsoft Azure Machine Learning Web Services](https://services.azureml.net/).</span></span>
2. <span data-ttu-id="207dc-108">Klikněte na tlačítko hello **plány** možnost nabídky.</span><span class="sxs-lookup"><span data-stu-id="207dc-108">Click hello **Plans** menu option.</span></span>
3. <span data-ttu-id="207dc-109">V plánech hello přes stránka zobrazení, klikněte na tlačítko **nový**.</span><span class="sxs-lookup"><span data-stu-id="207dc-109">On hello Plans over view page, click **New**.</span></span>
4. <span data-ttu-id="207dc-110">Z hello **předplatné** rozevíracího seznamu, vyberte hello předplatné, ve které hello se bude nacházet nový plán.</span><span class="sxs-lookup"><span data-stu-id="207dc-110">From hello **Subscription** dropdown, select hello subscription in which hello new plan will reside.</span></span>
5. <span data-ttu-id="207dc-111">Z hello **oblast** rozevíracího seznamu, vyberte oblast pro hello nový plán.</span><span class="sxs-lookup"><span data-stu-id="207dc-111">From hello **Region** dropdown, select a region for hello new plan.</span></span> <span data-ttu-id="207dc-112">Hello možnosti plánování pro vybrané oblasti hello se zobrazí v hello **možnosti plánování** oddílu hello stránky.</span><span class="sxs-lookup"><span data-stu-id="207dc-112">hello Plan Options for hello selected region will display in hello **Plan Options** section of hello page.</span></span>
6. <span data-ttu-id="207dc-113">Z hello **skupiny prostředků** rozevíracího seznamu, vyberte prostředek skupiny pro plán hello.</span><span class="sxs-lookup"><span data-stu-id="207dc-113">From hello **Resource Group** dropdown, select a resource group for hello plan.</span></span> <span data-ttu-id="207dc-114">Další informace o skupinách prostředků najdete v části [přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="207dc-114">From more information on resource groups, see [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md).</span></span>
7. <span data-ttu-id="207dc-115">V **název plánu** název typu hello hello plánu.</span><span class="sxs-lookup"><span data-stu-id="207dc-115">In **Plan Name** type hello name of hello plan.</span></span>
8. <span data-ttu-id="207dc-116">V části **plán možnosti**, klikněte na tlačítko hello fakturace úroveň hello nový plán.</span><span class="sxs-lookup"><span data-stu-id="207dc-116">Under **Plan Options**, click hello billing level for hello new plan.</span></span>
9. <span data-ttu-id="207dc-117">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="207dc-117">Click **Create**.</span></span>

## <a name="deploying-hello-web-service-tooanother-region"></a><span data-ttu-id="207dc-118">Nasazení hello webové služby tooanother oblast</span><span class="sxs-lookup"><span data-stu-id="207dc-118">Deploying hello web service tooanother region</span></span>
1. <span data-ttu-id="207dc-119">Klikněte na tlačítko hello **webové služby** možnost nabídky.</span><span class="sxs-lookup"><span data-stu-id="207dc-119">Click hello **Web Services** menu option.</span></span>
2. <span data-ttu-id="207dc-120">Vyberte hello webové služby, které nasazujete tooa novou oblast.</span><span class="sxs-lookup"><span data-stu-id="207dc-120">Select hello Web Service you are deploying tooa new region.</span></span>
3. <span data-ttu-id="207dc-121">Klikněte na tlačítko **kopie**.</span><span class="sxs-lookup"><span data-stu-id="207dc-121">Click **Copy**.</span></span>
4. <span data-ttu-id="207dc-122">V **název webové služby**, zadejte nový název pro hello webovou službu.</span><span class="sxs-lookup"><span data-stu-id="207dc-122">In **Web Service Name**, type a new name for hello web service.</span></span>
5. <span data-ttu-id="207dc-123">V **webové služby popis**, zadejte popis pro hello webovou službu.</span><span class="sxs-lookup"><span data-stu-id="207dc-123">In **Web service description**, type a description for hello web service.</span></span>
6. <span data-ttu-id="207dc-124">Z hello **předplatné** rozevíracího seznamu, vyberte hello předplatné, ve které hello bude umístěn novou webovou službu.</span><span class="sxs-lookup"><span data-stu-id="207dc-124">From hello **Subscription** dropdown, select hello subscription in which hello new web service will reside.</span></span>
7. <span data-ttu-id="207dc-125">Z hello **skupiny prostředků** rozevíracího seznamu, vyberte prostředek skupiny pro hello webovou službu.</span><span class="sxs-lookup"><span data-stu-id="207dc-125">From hello **Resource Group** dropdown, select a resource group for hello web service.</span></span> <span data-ttu-id="207dc-126">Další informace o skupinách prostředků najdete v části [přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="207dc-126">From more information on resource groups, see [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md).</span></span>
8. <span data-ttu-id="207dc-127">Z hello **oblast** rozevíracího seznamu, vyberte hello oblast, ve které toodeploy hello webové služby.</span><span class="sxs-lookup"><span data-stu-id="207dc-127">From hello **Region** dropdown, select hello region in which toodeploy hello web service.</span></span>
9. <span data-ttu-id="207dc-128">Z hello **účet úložiště** rozevíracího seznamu, vyberte účet úložiště, ve které toostore hello webové služby.</span><span class="sxs-lookup"><span data-stu-id="207dc-128">From hello **Storage account** dropdown, select a storage account in which toostore hello web service.</span></span>
10. <span data-ttu-id="207dc-129">Z hello **cena plán** rozevíracího seznamu, vyberte plán v oblasti hello jste vybrali v kroku 8.</span><span class="sxs-lookup"><span data-stu-id="207dc-129">From hello **Price Plan** dropdown, select a plan in hello region you selected in step 8.</span></span>
11. <span data-ttu-id="207dc-130">Klikněte na tlačítko **kopie**.</span><span class="sxs-lookup"><span data-stu-id="207dc-130">Click **Copy**.</span></span>

