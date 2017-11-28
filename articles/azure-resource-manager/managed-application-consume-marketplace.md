---
title: "aaaConsume spravované aplikace Azure Marketplace | Microsoft Docs"
description: "Describeshow toocreate spravované aplikace Azure, která je k dispozici prostřednictvím hello Marketplace."
services: azure-resource-manager
author: ravbhatnagar
manager: rjmax
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 07/11/2017
ms.author: gauravbh; tomfitz
ms.openlocfilehash: 9ae6e11a3f63eb58a9f3199364b5606a7afe5618
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="consume-azure-managed-applications-in-hello-marketplace"></a><span data-ttu-id="6a13b-103">Využívat Azure spravované aplikace v hello Marketplace.</span><span class="sxs-lookup"><span data-stu-id="6a13b-103">Consume Azure managed applications in hello Marketplace</span></span>

<span data-ttu-id="6a13b-104">Jak je popsáno v hello [spravovaných aplikací – přehled](managed-application-overview.md) článek, existují dva scénáře přihlašování tooend end hello.</span><span class="sxs-lookup"><span data-stu-id="6a13b-104">As discussed in hello [Managed Application overview](managed-application-overview.md) article, there are two scenarios in hello end tooend experience.</span></span> <span data-ttu-id="6a13b-105">Jeden je hello vydavatele nebo dodavatele, který chce toocreate spravované aplikace pro použití zákazníků.</span><span class="sxs-lookup"><span data-stu-id="6a13b-105">One is hello publisher or vendor who wants toocreate a managed application for use by customers.</span></span> <span data-ttu-id="6a13b-106">Hello druhou je zákazník end hello nebo hello příjemce hello spravované aplikace.</span><span class="sxs-lookup"><span data-stu-id="6a13b-106">hello second is hello end customer or hello consumer of hello managed application.</span></span> <span data-ttu-id="6a13b-107">Tento článek se zabývá hello druhý scénář.</span><span class="sxs-lookup"><span data-stu-id="6a13b-107">This article covers hello second scenario.</span></span> <span data-ttu-id="6a13b-108">Popisuje, jak můžete nasadit spravované aplikace z hello Microsoft Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="6a13b-108">It describes how you can deploy a managed application from hello Microsoft Azure Marketplace.</span></span>

## <a name="create-from-hello-marketplace"></a><span data-ttu-id="6a13b-109">Vytvoření z hello Marketplace.</span><span class="sxs-lookup"><span data-stu-id="6a13b-109">Create from hello Marketplace</span></span>

<span data-ttu-id="6a13b-110">Nasazení spravované aplikace z hello Marketplace je podobné toodeploying jakýkoli typ prostředky z hello Marketplace.</span><span class="sxs-lookup"><span data-stu-id="6a13b-110">Deploying a managed application from hello Marketplace is similar toodeploying any type of resources from hello Marketplace.</span></span> 

<span data-ttu-id="6a13b-111">Hello portálu, vyberte **+ nový** a vyhledejte hello typ toodeploy řešení.</span><span class="sxs-lookup"><span data-stu-id="6a13b-111">In hello portal, select **+ New** and search for hello type of solution toodeploy.</span></span> <span data-ttu-id="6a13b-112">Dostupné možnosti hello vyberte hello jeden, které potřebujete.</span><span class="sxs-lookup"><span data-stu-id="6a13b-112">From hello available options, select hello one you need.</span></span>

![hledání řešení](./media/managed-application-consume-marketplace/search-apps.png)

<span data-ttu-id="6a13b-114">Zkontrolujte souhrn hello hello aplikace a vyberte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="6a13b-114">Review hello summary of hello application, and select **Create**.</span></span>

![Vytvoření spravované aplikace](./media/managed-application-consume-marketplace/create-marketplace-managed-app.png)

<span data-ttu-id="6a13b-116">Jako žádného jiného řešení se zobrazí s tooprovide hodnoty polí.</span><span class="sxs-lookup"><span data-stu-id="6a13b-116">Like any other solution, you are presented with fields tooprovide values for.</span></span> <span data-ttu-id="6a13b-117">Tato pole se liší podle typu hello spravované aplikace, které vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="6a13b-117">These fields vary by hello type of managed application you create.</span></span> 

## <a name="view-support-information"></a><span data-ttu-id="6a13b-118">Zobrazení informací o podporu</span><span class="sxs-lookup"><span data-stu-id="6a13b-118">View support information</span></span>

<span data-ttu-id="6a13b-119">Po nasazení ve spravované aplikaci má zobrazte hello informací o podpoře pro aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="6a13b-119">After your managed application has deployed, view hello support information for hello application.</span></span> <span data-ttu-id="6a13b-120">V okně hello spravované aplikace je uvedena informace o podpoře hello.</span><span class="sxs-lookup"><span data-stu-id="6a13b-120">In hello managed application blade, hello support information is listed.</span></span>

![Podpora](./media/managed-application-consume-marketplace/support.png)

## <a name="view-publisher-permissions"></a><span data-ttu-id="6a13b-122">Zobrazení vydavatele oprávnění</span><span class="sxs-lookup"><span data-stu-id="6a13b-122">View publisher permissions</span></span>

<span data-ttu-id="6a13b-123">Hello dodavatele, který spravuje vaše aplikace se udělí přístup k prostředkům tooyour.</span><span class="sxs-lookup"><span data-stu-id="6a13b-123">hello vendor that manages your application is granted access tooyour resources.</span></span> <span data-ttu-id="6a13b-124">Vyberte tato oprávnění toosee **autorizací**.</span><span class="sxs-lookup"><span data-stu-id="6a13b-124">toosee those permissions, select **Authorizations**.</span></span>

![Povolení](./media/managed-application-consume-marketplace/authorizations.png)

## <a name="next-steps"></a><span data-ttu-id="6a13b-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6a13b-126">Next steps</span></span>

* <span data-ttu-id="6a13b-127">Informace o publikování spravované aplikaci v hello Marketplace najdete v tématu [spravovaných aplikací Azure Marketplace](managed-application-author-marketplace.md).</span><span class="sxs-lookup"><span data-stu-id="6a13b-127">For information about publishing a managed application in hello Marketplace, see [Azure Managed Applications in Marketplace](managed-application-author-marketplace.md).</span></span>
* <span data-ttu-id="6a13b-128">toopublish spravované aplikace, které jsou pouze k dispozici toousers ve vaší organizaci, najdete v části [vytvoření a publikování aplikace spravované katalogu služby](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="6a13b-128">toopublish managed applications that are only available toousers in your organization, see [Create and publish Service Catalog Managed Application](managed-application-publishing.md).</span></span>
* <span data-ttu-id="6a13b-129">tooconsume spravované aplikace, které jsou pouze k dispozici toousers ve vaší organizaci, najdete v části [využívat aplikace spravované katalogu služby](managed-application-consumption.md).</span><span class="sxs-lookup"><span data-stu-id="6a13b-129">tooconsume managed applications that are only available toousers in your organization, see [Consume a Service Catalog Managed Application](managed-application-consumption.md).</span></span>
