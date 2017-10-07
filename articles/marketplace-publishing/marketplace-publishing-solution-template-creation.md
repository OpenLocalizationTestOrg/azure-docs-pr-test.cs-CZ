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
# <a name="guide-toocreate-a-solution-template-for-azure-marketplace"></a>Průvodce toocreate šablonu řešení pro Azure Marketplace
Po dokončení kroku 1, [vytváření účtů a registrace][link-acct-creation], jsme vám na základě na hello vytváření řešení šablony kompatibilní s Azure v [technické požadavky pro vytváření Šablona řešení](marketplace-publishing-solution-template-creation-prerequisites.md). Nyní jsme vás provede procesem hello kroky pro vytvoření šablony řešení pro víc virtuálních počítačů na hello [publikování portál] [ link-pubportal] pro hello Azure Marketplace.

## <a name="create-your-solution-template-offer-in-hello-publishing-portal"></a>Vytvořit nabídku šablony řešení v hello publikování portálu
Přejděte příliš [https://publish.windowsazure.com](http://publish.windowsazure.com). Při přihlášení pro první čas toohello hello [publikování portál](https://publish.windowsazure.com/), hello použít stejný účet, který byl zaregistrován profil prodejce vaší společnosti. Každý zaměstnanec vaší společnosti můžete později přidat jako spolusprávce v hello publikování portálu.

### <a name="1-select-solution-templates"></a>1. Vyberte možnost "šablony řešení"
  ![Kreslení][img-pubportal-menu-sol-templ]

### <a name="2-create-a-new-solution-template"></a>2. Vytvořte novou šablonu řešení
  ![Kreslení][img-pubportal-sol-templ-new]

### <a name="3-start-with-topologies"></a>3. Začněte s topologie
Šablona řešení je "nadřazená" tooall topologiemi. V jedné šabloně nabídky/řešení můžete definovat více topologií. Pokud je nabídka vložena toostaging, je vložena se všemi topologiemi. Postupujte podle kroků hello toodefine vaši nabídku:     

* Vytvořit topologii: "Topologie identifikátor" je obvykle název hello hello topologie hello řešení šablony. identifikátor topologie Hello se používá v adrese URL hello, jak je uvedeno níže:

  Azure Marketplace: http://azure.microsoft.com/marketplace/partners/ {PublisherNamespace} / {OfferIdentifier} {TopologyIdentifier}

  Portál Azure: https://portal.azure.com/#gallery/ {PublisherNamespace}. {OfferIdentifier} {TopologyIdentifier}
* Přidejte novou verzi.

### <a name="4-get-your-topology-versions-certified"></a>4. Získání verze vaší topologie certifikaci
Nahrajte soubor zip, který obsahuje všechny požadované soubory tooprovision této konkrétní verzi hello topologie. Tento soubor zip musí obsahovat hello následující:

* *mainTemplate.json* a *createUiDefinition.json* souboru v jeho kořenový adresář.
* Propojených šablon a všechny požadované skripty.

  > [!TIP]
  > Zatímco vaše vývojářům pracovat na vytváření řešení hello šablony topologie a získávání je certifikaci, hello firmy, marketing nebo právní oddělení vaší společnosti může fungovat na hello marketing a právní obsahu.
  >
  >

## <a name="next-steps"></a>Další kroky
Teď, když jste vytvořili šablony řešení a nahrát soubor zip hello, postupujte podle pokynů hello v hello [Marketplace marketing obsahu Průvodce](marketplace-publishing-push-to-staging.md) před odesláním toostaging nabídka hello. toosee hello úplnou sadu marketplace publikování článků, navštivte [Začínáme: jak toopublish toohello nabídka Azure Marketplace](marketplace-publishing-getting-started.md).

Může být také zájem o tyto související články:

* Image virtuálních počítačů: [o Image virtuálních počítačů v Azure](https://msdn.microsoft.com/library/azure/dn790290.aspx)
* Rozšíření virtuálního počítače: [agenta virtuálního počítače a přehled rozšíření virtuálního počítače](https://msdn.microsoft.com/library/azure/dn832621.aspx) a [rozšíření virtuálního počítače Azure a funkce](https://msdn.microsoft.com/library/azure/dn606311.aspx)
* Azure Resource Manager: [vytváření šablon Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md) a [příklady jednoduchou šablonu](https://github.com/rjmax/ArmExamples)
* Omezení účtu úložiště: [jak tooMonitor omezení účtu úložiště](http://blogs.msdn.com/b/mast/archive/2014/08/02/how-to-monitor-for-storage-account-throttling.aspx) a [storage úrovně Premium](../storage/common/storage-premium-storage.md#scalability-and-performance-targets)

[img-pubportal-menu-sol-templ]:media/marketplace-publishing-solution-template-creation/pubportal-menu-solution-templates.png
[img-pubportal-sol-templ-new]:media/marketplace-publishing-solution-template-creation/pubportal-solution-template-new.png
[link-acct-creation]:marketplace-publishing-accounts-creation-registration.md
[link-pubportal]:https://publish.windowsazure.com
