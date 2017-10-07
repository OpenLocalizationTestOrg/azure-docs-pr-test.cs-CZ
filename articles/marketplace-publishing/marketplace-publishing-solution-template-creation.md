---
title: "aaaGuide toocreating šablonu řešení pro hello Marketplace | Microsoft Docs"
description: "Podrobné pokyny, jak toocreate, certifikovat a nasazení šablony řešení Image více virtuálních počítačů pro nákup na hello Azure Marketplace."
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
ms.openlocfilehash: b0e7067176337dd0d3f6f6ec04c963f80f706ab0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="guide-toocreate-a-solution-template-for-azure-marketplace"></a><span data-ttu-id="cebfd-103">Průvodce toocreate šablonu řešení pro Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="cebfd-103">Guide toocreate a solution template for Azure Marketplace</span></span>
<span data-ttu-id="cebfd-104">Po dokončení kroku 1, [vytváření účtů a registrace][link-acct-creation], jsme vám na základě na hello vytváření řešení šablony kompatibilní s Azure v [technické požadavky pro vytváření Šablona řešení](marketplace-publishing-solution-template-creation-prerequisites.md).</span><span class="sxs-lookup"><span data-stu-id="cebfd-104">After completing step 1, [Account creation and registration][link-acct-creation], we guided you on hello creation of an Azure-compatible solution template at [Technical prerequisites for creating a solution template](marketplace-publishing-solution-template-creation-prerequisites.md).</span></span> <span data-ttu-id="cebfd-105">Nyní jsme vás provede procesem hello kroky pro vytvoření šablony řešení pro víc virtuálních počítačů na hello [publikování portál] [ link-pubportal] pro hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="cebfd-105">Now we will walk you through hello steps for creating a solution template for multiple VMs on hello [Publishing Portal][link-pubportal] for hello Azure Marketplace.</span></span>

## <a name="create-your-solution-template-offer-in-hello-publishing-portal"></a><span data-ttu-id="cebfd-106">Vytvořit nabídku šablony řešení v hello publikování portálu</span><span class="sxs-lookup"><span data-stu-id="cebfd-106">Create your solution template offer in hello Publishing Portal</span></span>
<span data-ttu-id="cebfd-107">Přejděte příliš [https://publish.windowsazure.com](http://publish.windowsazure.com). Při přihlášení pro první čas toohello hello [publikování portál](https://publish.windowsazure.com/), hello použít stejný účet, který byl zaregistrován profil prodejce vaší společnosti.</span><span class="sxs-lookup"><span data-stu-id="cebfd-107">Go too [https://publish.windowsazure.com](http://publish.windowsazure.com). When signing in for hello first time toohello [Publishing Portal](https://publish.windowsazure.com/), use hello same account with which your company’s seller profile was registered.</span></span> <span data-ttu-id="cebfd-108">Každý zaměstnanec vaší společnosti můžete později přidat jako spolusprávce v hello publikování portálu.</span><span class="sxs-lookup"><span data-stu-id="cebfd-108">Later, you can add any employee of your company as a co-admin in hello Publishing Portal.</span></span>

### <a name="1-select-solution-templates"></a><span data-ttu-id="cebfd-109">1. Vyberte možnost "šablony řešení"</span><span class="sxs-lookup"><span data-stu-id="cebfd-109">1. Select "Solution templates"</span></span>
  ![Kreslení][img-pubportal-menu-sol-templ]

### <a name="2-create-a-new-solution-template"></a><span data-ttu-id="cebfd-111">2. Vytvořte novou šablonu řešení</span><span class="sxs-lookup"><span data-stu-id="cebfd-111">2. Create a new solution template</span></span>
  ![Kreslení][img-pubportal-sol-templ-new]

### <a name="3-start-with-topologies"></a><span data-ttu-id="cebfd-113">3. Začněte s topologie</span><span class="sxs-lookup"><span data-stu-id="cebfd-113">3. Start with topologies</span></span>
<span data-ttu-id="cebfd-114">Šablona řešení je "nadřazená" tooall topologiemi.</span><span class="sxs-lookup"><span data-stu-id="cebfd-114">A solution template is a "parent" tooall of its topologies.</span></span> <span data-ttu-id="cebfd-115">V jedné šabloně nabídky/řešení můžete definovat více topologií.</span><span class="sxs-lookup"><span data-stu-id="cebfd-115">You can define multiple topologies in one offer/solution template.</span></span> <span data-ttu-id="cebfd-116">Pokud je nabídka vložena toostaging, je vložena se všemi topologiemi.</span><span class="sxs-lookup"><span data-stu-id="cebfd-116">When an offer is pushed toostaging, it is pushed with all of its topologies.</span></span> <span data-ttu-id="cebfd-117">Postupujte podle kroků hello toodefine vaši nabídku:</span><span class="sxs-lookup"><span data-stu-id="cebfd-117">Follow hello steps below toodefine your offer:</span></span>     

* <span data-ttu-id="cebfd-118">Vytvořit topologii: "Topologie identifikátor" je obvykle název hello hello topologie hello řešení šablony.</span><span class="sxs-lookup"><span data-stu-id="cebfd-118">Create a Topology: “Topology Identifier” is typically hello name of hello topology for hello solution template.</span></span> <span data-ttu-id="cebfd-119">identifikátor topologie Hello se používá v adrese URL hello, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="cebfd-119">hello topology identifier is used in hello URL as shown below:</span></span>

  <span data-ttu-id="cebfd-120">Azure Marketplace: http://azure.microsoft.com/marketplace/partners/ {PublisherNamespace} / {OfferIdentifier} {TopologyIdentifier}</span><span class="sxs-lookup"><span data-stu-id="cebfd-120">Azure Marketplace: http://azure.microsoft.com/marketplace/partners/{PublisherNamespace}/{OfferIdentifier}{TopologyIdentifier}</span></span>

  <span data-ttu-id="cebfd-121">Portál Azure: https://portal.azure.com/#gallery/ {PublisherNamespace}. {OfferIdentifier} {TopologyIdentifier}</span><span class="sxs-lookup"><span data-stu-id="cebfd-121">Azure Portal: https://portal.azure.com/#gallery/{PublisherNamespace}.{OfferIdentifier}{TopologyIdentifier}</span></span>
* <span data-ttu-id="cebfd-122">Přidejte novou verzi.</span><span class="sxs-lookup"><span data-stu-id="cebfd-122">Add a new version.</span></span>

### <a name="4-get-your-topology-versions-certified"></a><span data-ttu-id="cebfd-123">4. Získání verze vaší topologie certifikaci</span><span class="sxs-lookup"><span data-stu-id="cebfd-123">4. Get your topology versions certified</span></span>
<span data-ttu-id="cebfd-124">Nahrajte soubor zip, který obsahuje všechny požadované soubory tooprovision této konkrétní verzi hello topologie.</span><span class="sxs-lookup"><span data-stu-id="cebfd-124">Upload a zip file that contains all required files tooprovision that particular version of hello topology.</span></span> <span data-ttu-id="cebfd-125">Tento soubor zip musí obsahovat hello následující:</span><span class="sxs-lookup"><span data-stu-id="cebfd-125">This zip file must contain hello following:</span></span>

* <span data-ttu-id="cebfd-126">*mainTemplate.json* a *createUiDefinition.json* souboru v jeho kořenový adresář.</span><span class="sxs-lookup"><span data-stu-id="cebfd-126">*mainTemplate.json* and *createUiDefinition.json* file at its root directory.</span></span>
* <span data-ttu-id="cebfd-127">Propojených šablon a všechny požadované skripty.</span><span class="sxs-lookup"><span data-stu-id="cebfd-127">Any linked templates and all required scripts.</span></span>

  > [!TIP]
  > <span data-ttu-id="cebfd-128">Zatímco vaše vývojářům pracovat na vytváření řešení hello šablony topologie a získávání je certifikaci, hello firmy, marketing nebo právní oddělení vaší společnosti může fungovat na hello marketing a právní obsahu.</span><span class="sxs-lookup"><span data-stu-id="cebfd-128">While your developers work on creating hello solution template topologies and getting them certified, hello business, marketing, and/or legal departments of your company can work on hello marketing and legal content.</span></span>
  >
  >

## <a name="next-steps"></a><span data-ttu-id="cebfd-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cebfd-129">Next steps</span></span>
<span data-ttu-id="cebfd-130">Teď, když jste vytvořili šablony řešení a nahrát soubor zip hello, postupujte podle pokynů hello v hello [Marketplace marketing obsahu Průvodce](marketplace-publishing-push-to-staging.md) před odesláním toostaging nabídka hello.</span><span class="sxs-lookup"><span data-stu-id="cebfd-130">Now that you created your solution template and uploaded hello zip file, please follow hello instructions in hello [Marketplace marketing content guide](marketplace-publishing-push-to-staging.md) before pushing hello offer toostaging.</span></span> <span data-ttu-id="cebfd-131">toosee hello úplnou sadu marketplace publikování článků, navštivte [Začínáme: jak toopublish toohello nabídka Azure Marketplace](marketplace-publishing-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="cebfd-131">toosee hello full set of marketplace publishing articles, visit [Getting started: How toopublish an offer toohello Azure Marketplace](marketplace-publishing-getting-started.md).</span></span>

<span data-ttu-id="cebfd-132">Může být také zájem o tyto související články:</span><span class="sxs-lookup"><span data-stu-id="cebfd-132">You might also be interested in these related articles:</span></span>

* <span data-ttu-id="cebfd-133">Image virtuálních počítačů: [o Image virtuálních počítačů v Azure](https://msdn.microsoft.com/library/azure/dn790290.aspx)</span><span class="sxs-lookup"><span data-stu-id="cebfd-133">VM images: [About Virtual Machine Images in Azure](https://msdn.microsoft.com/library/azure/dn790290.aspx)</span></span>
* <span data-ttu-id="cebfd-134">Rozšíření virtuálního počítače: [agenta virtuálního počítače a přehled rozšíření virtuálního počítače](https://msdn.microsoft.com/library/azure/dn832621.aspx) a [rozšíření virtuálního počítače Azure a funkce](https://msdn.microsoft.com/library/azure/dn606311.aspx)</span><span class="sxs-lookup"><span data-stu-id="cebfd-134">VM extensions: [VM Agent and VM Extensions Overview](https://msdn.microsoft.com/library/azure/dn832621.aspx) and [Azure VM Extensions and Features](https://msdn.microsoft.com/library/azure/dn606311.aspx)</span></span>
* <span data-ttu-id="cebfd-135">Azure Resource Manager: [vytváření šablon Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md) a [příklady jednoduchou šablonu](https://github.com/rjmax/ArmExamples)</span><span class="sxs-lookup"><span data-stu-id="cebfd-135">Azure Resource Manager: [Authoring Azure Resource Manager Templates](../azure-resource-manager/resource-group-authoring-templates.md) and [Simple Template Examples](https://github.com/rjmax/ArmExamples)</span></span>
* <span data-ttu-id="cebfd-136">Omezení účtu úložiště: [jak tooMonitor omezení účtu úložiště](http://blogs.msdn.com/b/mast/archive/2014/08/02/how-to-monitor-for-storage-account-throttling.aspx) a [storage úrovně Premium](../storage/common/storage-premium-storage.md#scalability-and-performance-targets)</span><span class="sxs-lookup"><span data-stu-id="cebfd-136">Storage account throttles: [How tooMonitor for Storage Account Throttling](http://blogs.msdn.com/b/mast/archive/2014/08/02/how-to-monitor-for-storage-account-throttling.aspx) and [Premium storage](../storage/common/storage-premium-storage.md#scalability-and-performance-targets)</span></span>

[img-pubportal-menu-sol-templ]:media/marketplace-publishing-solution-template-creation/pubportal-menu-solution-templates.png
[img-pubportal-sol-templ-new]:media/marketplace-publishing-solution-template-creation/pubportal-solution-template-new.png
[link-acct-creation]:marketplace-publishing-accounts-creation-registration.md
[link-pubportal]:https://publish.windowsazure.com
