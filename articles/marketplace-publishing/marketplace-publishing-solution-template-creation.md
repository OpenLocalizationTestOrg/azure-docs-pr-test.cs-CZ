---
title: "Průvodce vytvořením šablony řešení pro Marketplace | Microsoft Docs"
description: "Podrobné pokyny o tom, jak vytvářet, certifikovat a nasadit šablonu více virtuálních počítačů bitové kopie řešení pro zakoupit na webu Azure Marketplace."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: e14e05f2-2385-4ce0-b351-0747cb74ba19
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/27/2016
ms.author: hascipio; v-divte
ms.openlocfilehash: b753bfb4bd69bd9aacf4eebd8999397394c28bc4
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="guide-to-create-a-solution-template-for-azure-marketplace"></a><span data-ttu-id="0f3e1-103">Průvodce Vytvořit šablonu řešení pro Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="0f3e1-103">Guide to create a solution template for Azure Marketplace</span></span>
<span data-ttu-id="0f3e1-104">Po dokončení kroku 1, [vytváření účtů a registrace][link-acct-creation], jsme vám na základě na vytvoření šablony řešení kompatibilní s Azure v [technické požadavky pro vytvoření šablony řešení](marketplace-publishing-solution-template-creation-prerequisites.md).</span><span class="sxs-lookup"><span data-stu-id="0f3e1-104">After completing step 1, [Account creation and registration][link-acct-creation], we guided you on the creation of an Azure-compatible solution template at [Technical prerequisites for creating a solution template](marketplace-publishing-solution-template-creation-prerequisites.md).</span></span> <span data-ttu-id="0f3e1-105">Nyní jsme vás provede kroky pro vytvoření šablony řešení pro víc virtuálních počítačů na [publikování portál] [ link-pubportal] pro Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="0f3e1-105">Now we will walk you through the steps for creating a solution template for multiple VMs on the [Publishing Portal][link-pubportal] for the Azure Marketplace.</span></span>

## <a name="create-your-solution-template-offer-in-the-publishing-portal"></a><span data-ttu-id="0f3e1-106">Vytvořit nabídku šablony řešení na portálu publikování</span><span class="sxs-lookup"><span data-stu-id="0f3e1-106">Create your solution template offer in the Publishing Portal</span></span>
<span data-ttu-id="0f3e1-107">Přejděte na [https://publish.windowsazure.com](http://publish.windowsazure.com). Při prvním přihlášení k [publikování portál](https://publish.windowsazure.com/), použijte stejný účet, který byl zaregistrován profil prodejce vaší společnosti.</span><span class="sxs-lookup"><span data-stu-id="0f3e1-107">Go to  [https://publish.windowsazure.com](http://publish.windowsazure.com). When signing in for the first time to the [Publishing Portal](https://publish.windowsazure.com/), use the same account with which your company’s seller profile was registered.</span></span> <span data-ttu-id="0f3e1-108">Každý zaměstnanec vaší společnosti můžete později přidat jako spolusprávce na portálu pro publikování.</span><span class="sxs-lookup"><span data-stu-id="0f3e1-108">Later, you can add any employee of your company as a co-admin in the Publishing Portal.</span></span>

### <a name="1-select-solution-templates"></a><span data-ttu-id="0f3e1-109">1. Vyberte možnost "šablony řešení"</span><span class="sxs-lookup"><span data-stu-id="0f3e1-109">1. Select "Solution templates"</span></span>
  ![Kreslení][img-pubportal-menu-sol-templ]

### <a name="2-create-a-new-solution-template"></a><span data-ttu-id="0f3e1-111">2. Vytvořte novou šablonu řešení</span><span class="sxs-lookup"><span data-stu-id="0f3e1-111">2. Create a new solution template</span></span>
  ![Kreslení][img-pubportal-sol-templ-new]

### <a name="3-start-with-topologies"></a><span data-ttu-id="0f3e1-113">3. Začněte s topologie</span><span class="sxs-lookup"><span data-stu-id="0f3e1-113">3. Start with topologies</span></span>
<span data-ttu-id="0f3e1-114">Šablona řešení je „nadřazená“ všem svým topologiím.</span><span class="sxs-lookup"><span data-stu-id="0f3e1-114">A solution template is a "parent" to all of its topologies.</span></span> <span data-ttu-id="0f3e1-115">V jedné šabloně nabídky/řešení můžete definovat více topologií.</span><span class="sxs-lookup"><span data-stu-id="0f3e1-115">You can define multiple topologies in one offer/solution template.</span></span> <span data-ttu-id="0f3e1-116">Pokud je nabídka vložena do přípravy, je vložena se všemi topologiemi.</span><span class="sxs-lookup"><span data-stu-id="0f3e1-116">When an offer is pushed to staging, it is pushed with all of its topologies.</span></span> <span data-ttu-id="0f3e1-117">Použijte následující postup k definování vaši nabídku:</span><span class="sxs-lookup"><span data-stu-id="0f3e1-117">Follow the steps below to define your offer:</span></span>     

* <span data-ttu-id="0f3e1-118">Vytvořit topologii: "Identifikátor topologie" je obvykle název topologie pro šablonu řešení.</span><span class="sxs-lookup"><span data-stu-id="0f3e1-118">Create a Topology: “Topology Identifier” is typically the name of the topology for the solution template.</span></span> <span data-ttu-id="0f3e1-119">Identifikátor topologie se používá v adrese URL, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="0f3e1-119">The topology identifier is used in the URL as shown below:</span></span>

  <span data-ttu-id="0f3e1-120">Azure Marketplace: http://azure.microsoft.com/marketplace/partners/ {PublisherNamespace} / {OfferIdentifier} {TopologyIdentifier}</span><span class="sxs-lookup"><span data-stu-id="0f3e1-120">Azure Marketplace: http://azure.microsoft.com/marketplace/partners/{PublisherNamespace}/{OfferIdentifier}{TopologyIdentifier}</span></span>

  <span data-ttu-id="0f3e1-121">Portál Azure: https://portal.azure.com/#gallery/ {PublisherNamespace}. {OfferIdentifier} {TopologyIdentifier}</span><span class="sxs-lookup"><span data-stu-id="0f3e1-121">Azure Portal: https://portal.azure.com/#gallery/{PublisherNamespace}.{OfferIdentifier}{TopologyIdentifier}</span></span>
* <span data-ttu-id="0f3e1-122">Přidejte novou verzi.</span><span class="sxs-lookup"><span data-stu-id="0f3e1-122">Add a new version.</span></span>

### <a name="4-get-your-topology-versions-certified"></a><span data-ttu-id="0f3e1-123">4. Získání verze vaší topologie certifikaci</span><span class="sxs-lookup"><span data-stu-id="0f3e1-123">4. Get your topology versions certified</span></span>
<span data-ttu-id="0f3e1-124">Nahrajte soubor zip, který obsahuje všechny požadované soubory ke zřízení tohoto konkrétní verzi topologii.</span><span class="sxs-lookup"><span data-stu-id="0f3e1-124">Upload a zip file that contains all required files to provision that particular version of the topology.</span></span> <span data-ttu-id="0f3e1-125">Tento soubor zip musí obsahovat následující:</span><span class="sxs-lookup"><span data-stu-id="0f3e1-125">This zip file must contain the following:</span></span>

* <span data-ttu-id="0f3e1-126">*mainTemplate.json* a *createUiDefinition.json* souboru v jeho kořenový adresář.</span><span class="sxs-lookup"><span data-stu-id="0f3e1-126">*mainTemplate.json* and *createUiDefinition.json* file at its root directory.</span></span>
* <span data-ttu-id="0f3e1-127">Propojených šablon a všechny požadované skripty.</span><span class="sxs-lookup"><span data-stu-id="0f3e1-127">Any linked templates and all required scripts.</span></span>

  > [!TIP]
  > <span data-ttu-id="0f3e1-128">Vaše vývojáři práci při vytváření šablony topologie řešení a získávání je certifikaci, business, marketing nebo právní oddělení vaší společnosti můžete pracovat na marketing a právní obsahu.</span><span class="sxs-lookup"><span data-stu-id="0f3e1-128">While your developers work on creating the solution template topologies and getting them certified, the business, marketing, and/or legal departments of your company can work on the marketing and legal content.</span></span>
  >
  >

## <a name="next-steps"></a><span data-ttu-id="0f3e1-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0f3e1-129">Next steps</span></span>
<span data-ttu-id="0f3e1-130">Teď, když jste vytvořili šablony řešení a nahrát soubor zip, postupujte podle pokynů v [Marketplace marketing obsahu Průvodce](marketplace-publishing-push-to-staging.md) před odesláním nabídku na pracovní.</span><span class="sxs-lookup"><span data-stu-id="0f3e1-130">Now that you created your solution template and uploaded the zip file, please follow the instructions in the [Marketplace marketing content guide](marketplace-publishing-push-to-staging.md) before pushing the offer to staging.</span></span> <span data-ttu-id="0f3e1-131">Kompletní marketplace publikování článků naleznete [Začínáme: postup publikování nabídky pro Azure Marketplace](marketplace-publishing-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="0f3e1-131">To see the full set of marketplace publishing articles, visit [Getting started: How to publish an offer to the Azure Marketplace](marketplace-publishing-getting-started.md).</span></span>

<span data-ttu-id="0f3e1-132">Může být také zájem o tyto související články:</span><span class="sxs-lookup"><span data-stu-id="0f3e1-132">You might also be interested in these related articles:</span></span>

* <span data-ttu-id="0f3e1-133">Image virtuálních počítačů: [o Image virtuálních počítačů v Azure](https://msdn.microsoft.com/library/azure/dn790290.aspx)</span><span class="sxs-lookup"><span data-stu-id="0f3e1-133">VM images: [About Virtual Machine Images in Azure](https://msdn.microsoft.com/library/azure/dn790290.aspx)</span></span>
* <span data-ttu-id="0f3e1-134">Rozšíření virtuálního počítače: [agenta virtuálního počítače a přehled rozšíření virtuálního počítače](https://msdn.microsoft.com/library/azure/dn832621.aspx) a [rozšíření virtuálního počítače Azure a funkce](https://msdn.microsoft.com/library/azure/dn606311.aspx)</span><span class="sxs-lookup"><span data-stu-id="0f3e1-134">VM extensions: [VM Agent and VM Extensions Overview](https://msdn.microsoft.com/library/azure/dn832621.aspx) and [Azure VM Extensions and Features](https://msdn.microsoft.com/library/azure/dn606311.aspx)</span></span>
* <span data-ttu-id="0f3e1-135">Azure Resource Manager: [vytváření šablon Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md) a [příklady jednoduchou šablonu](https://github.com/rjmax/ArmExamples)</span><span class="sxs-lookup"><span data-stu-id="0f3e1-135">Azure Resource Manager: [Authoring Azure Resource Manager Templates](../azure-resource-manager/resource-group-authoring-templates.md) and [Simple Template Examples](https://github.com/rjmax/ArmExamples)</span></span>
* <span data-ttu-id="0f3e1-136">Omezení účtu úložiště: [monitorování omezení účtu úložiště](http://blogs.msdn.com/b/mast/archive/2014/08/02/how-to-monitor-for-storage-account-throttling.aspx) a [storage úrovně Premium](../storage/common/storage-premium-storage.md#scalability-and-performance-targets)</span><span class="sxs-lookup"><span data-stu-id="0f3e1-136">Storage account throttles: [How to Monitor for Storage Account Throttling](http://blogs.msdn.com/b/mast/archive/2014/08/02/how-to-monitor-for-storage-account-throttling.aspx) and [Premium storage](../storage/common/storage-premium-storage.md#scalability-and-performance-targets)</span></span>

[img-pubportal-menu-sol-templ]:media/marketplace-publishing-solution-template-creation/pubportal-menu-solution-templates.png
[img-pubportal-sol-templ-new]:media/marketplace-publishing-solution-template-creation/pubportal-solution-template-new.png
[link-acct-creation]:marketplace-publishing-accounts-creation-registration.md
[link-pubportal]:https://publish.windowsazure.com
