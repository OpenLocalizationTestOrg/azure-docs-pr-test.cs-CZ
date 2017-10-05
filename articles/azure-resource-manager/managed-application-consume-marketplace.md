---
title: "Používat spravované aplikace Azure Marketplace | Microsoft Docs"
description: "Describeshow k vytvoření Azure spravované aplikace, která je k dispozici prostřednictvím Marketplace."
services: azure-resource-manager
author: ravbhatnagar
manager: rjmax
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 07/11/2017
ms.author: gauravbh; tomfitz
ms.openlocfilehash: baf456740bddd562391ed64d707f990c8921d710
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/29/2017
---
# <a name="consume-azure-managed-applications-in-the-marketplace"></a><span data-ttu-id="44da5-103">Využívat Azure spravované aplikace na webu Marketplace</span><span class="sxs-lookup"><span data-stu-id="44da5-103">Consume Azure managed applications in the Marketplace</span></span>

<span data-ttu-id="44da5-104">Jak je popsáno v [spravovaných aplikací – přehled](managed-application-overview.md) článek, přihlašování a provést tak kompletní existují dva scénáře.</span><span class="sxs-lookup"><span data-stu-id="44da5-104">As discussed in the [Managed Application overview](managed-application-overview.md) article, there are two scenarios in the end to end experience.</span></span> <span data-ttu-id="44da5-105">Jeden je vydavatele nebo dodavatele, který chce vytvořit spravované aplikace pro použití zákazníků.</span><span class="sxs-lookup"><span data-stu-id="44da5-105">One is the publisher or vendor who wants to create a managed application for use by customers.</span></span> <span data-ttu-id="44da5-106">Druhá je koncového zákazníka nebo příjemce spravované aplikace.</span><span class="sxs-lookup"><span data-stu-id="44da5-106">The second is the end customer or the consumer of the managed application.</span></span> <span data-ttu-id="44da5-107">Tento článek se zabývá druhý scénář.</span><span class="sxs-lookup"><span data-stu-id="44da5-107">This article covers the second scenario.</span></span> <span data-ttu-id="44da5-108">Popisuje, jak můžete nasadit spravované aplikace z webu Microsoft Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="44da5-108">It describes how you can deploy a managed application from the Microsoft Azure Marketplace.</span></span>

## <a name="create-from-the-marketplace"></a><span data-ttu-id="44da5-109">Vytvoření z Marketplace</span><span class="sxs-lookup"><span data-stu-id="44da5-109">Create from the Marketplace</span></span>

<span data-ttu-id="44da5-110">Nasazení spravované aplikace z Marketplace je podobná nasazení jakýmikoli prostředky z Marketplace.</span><span class="sxs-lookup"><span data-stu-id="44da5-110">Deploying a managed application from the Marketplace is similar to deploying any type of resources from the Marketplace.</span></span> 

<span data-ttu-id="44da5-111">Na portálu, vyberte **+ nový** a vyhledejte typu řešení pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="44da5-111">In the portal, select **+ New** and search for the type of solution to deploy.</span></span> <span data-ttu-id="44da5-112">Z dostupných možností vyberte, které potřebujete.</span><span class="sxs-lookup"><span data-stu-id="44da5-112">From the available options, select the one you need.</span></span>

![hledání řešení](./media/managed-application-consume-marketplace/search-apps.png)

<span data-ttu-id="44da5-114">Zkontrolujte souhrn aplikace a vyberte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="44da5-114">Review the summary of the application, and select **Create**.</span></span>

![Vytvoření spravované aplikace](./media/managed-application-consume-marketplace/create-marketplace-managed-app.png)

<span data-ttu-id="44da5-116">Jako žádného jiného řešení se zobrazí pole k zadání hodnot pro.</span><span class="sxs-lookup"><span data-stu-id="44da5-116">Like any other solution, you are presented with fields to provide values for.</span></span> <span data-ttu-id="44da5-117">Tato pole se liší podle typu spravované aplikace, které vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="44da5-117">These fields vary by the type of managed application you create.</span></span> 

## <a name="view-support-information"></a><span data-ttu-id="44da5-118">Zobrazení informací o podporu</span><span class="sxs-lookup"><span data-stu-id="44da5-118">View support information</span></span>

<span data-ttu-id="44da5-119">Po nasazení ve spravované aplikaci má zobrazte informace o podpoře pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="44da5-119">After your managed application has deployed, view the support information for the application.</span></span> <span data-ttu-id="44da5-120">V okně spravované aplikace je uvedena informace podpory.</span><span class="sxs-lookup"><span data-stu-id="44da5-120">In the managed application blade, the support information is listed.</span></span>

![Podpora](./media/managed-application-consume-marketplace/support.png)

## <a name="view-publisher-permissions"></a><span data-ttu-id="44da5-122">Zobrazení vydavatele oprávnění</span><span class="sxs-lookup"><span data-stu-id="44da5-122">View publisher permissions</span></span>

<span data-ttu-id="44da5-123">Výrobce, který spravuje vaše aplikace je udělen přístup k prostředkům.</span><span class="sxs-lookup"><span data-stu-id="44da5-123">The vendor that manages your application is granted access to your resources.</span></span> <span data-ttu-id="44da5-124">Pokud chcete zobrazit tato oprávnění, vyberte **autorizací**.</span><span class="sxs-lookup"><span data-stu-id="44da5-124">To see those permissions, select **Authorizations**.</span></span>

![Povolení](./media/managed-application-consume-marketplace/authorizations.png)

## <a name="next-steps"></a><span data-ttu-id="44da5-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="44da5-126">Next steps</span></span>

* <span data-ttu-id="44da5-127">Informace o publikování aplikace spravované v Marketplace najdete v tématu [spravovaných aplikací Azure Marketplace](managed-application-author-marketplace.md).</span><span class="sxs-lookup"><span data-stu-id="44da5-127">For information about publishing a managed application in the Marketplace, see [Azure Managed Applications in Marketplace](managed-application-author-marketplace.md).</span></span>
* <span data-ttu-id="44da5-128">Publikování spravované aplikace, které jsou dostupné jenom pro uživatele ve vaší organizaci, najdete v části [vytvoření a publikování aplikace spravované katalogu služby](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="44da5-128">To publish managed applications that are only available to users in your organization, see [Create and publish Service Catalog Managed Application](managed-application-publishing.md).</span></span>
* <span data-ttu-id="44da5-129">Používat spravované aplikace, které jsou dostupné jenom pro uživatele ve vaší organizaci, najdete v části [využívat aplikace spravované katalogu služby](managed-application-consumption.md).</span><span class="sxs-lookup"><span data-stu-id="44da5-129">To consume managed applications that are only available to users in your organization, see [Consume a Service Catalog Managed Application](managed-application-consumption.md).</span></span>