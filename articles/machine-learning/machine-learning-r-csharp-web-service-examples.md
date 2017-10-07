---
title: "AAA(Deprecated) Machine learning webové služby vytvořené s R - Azure příklady | Microsoft Docs"
description: "(nepoužívané) Najdete užitečné sada webových služeb příklady jsou vytvořené pomocí kódu jazyka R a Machine Learning a publikovaná toohello Azure Marketplace."
keywords: "CSharp, kódu jazyka r příklady webové služby"
services: machine-learning
documentationcenter: 
author: jaymathe
manager: jhubbard
editor: cgronlun
ms.assetid: 97d66cb7-6a84-4ae9-8095-0b5f5ba82d7f
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: jaymathe
ROBOTS: NOINDEX
redirect_url: https://gallery.cortanaintelligence.com/
redirect_document_id: True
ms.openlocfilehash: 20b074d38e65aed907d40549bb61f124cb5dfe1d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deprecated-web-services-examples-using-r-code-on-azure-machine-learning-and-published-toomicrosoft-azure-marketplace"></a>(nepoužívané) Webové služby příklady použití kódu jazyka R na Azure Machine Learning a publikovaná tooMicrosoft Azure Marketplace

> [!NOTE]
> Hello Microsoft DataMarket se postupně vyřazuje z provozu a tato rozhraní API jsou zastaralé. 
> 
> Mnoho užitečné příklad experimentů a rozhraní API můžete najít v hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com). Další informace o hello Galerie najdete v tématu [sdílené složky a zjišťovat prostředky ve hello Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).

V tomto článku jsou třeba webové služby byly vytvořené pomocí Azure Machine Learning a publikovaná toohello Azure Marketplace. Každý webové službu příkladu má komplexní dokumentu připojené, vkládání ukázkových datových sad pro testování hello služby a vysvětlení, jak hello uživatel může vytvářet podobnou službu sami. 

V nástroji Azure Machine Learning Studio uživatelé napsat kód R a několika kliknutími publikujete ji jako webovou službu pro aplikace a zařízení tooconsume kolem hello, world. 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Neměly jednoduché kalkulačky, které poskytují toocreating statistické funkce vlastní prediktivní analýzy postojích text dolování nové i zkušeného R uživatelé mohou mít prospěch ze hello usnadnění, pomocí kterého můžete uživatelům Azure Machine Learning zprovoznit R kód. Když tyto webové služby, které by mohly spotřebovat uživateli, potenciálně prostřednictvím mobilní aplikace nebo webu, hello účel těchto webových služeb, příklady je tooshow, jak můžete zprovoznit Machine Learning R skriptů pro analytické účely a být použité toocreate webové služby na začátek kódu jazyka R.

Každý příklad obsahuje příklad jazyka C# pro používání webové služby.

![Diagram R kódu v Azure Machine Learning: R řešení pro vlastní použití nebo publikované toohello Azure Marketplace.][1]

Zvažte následující scénáře hello.

## <a name="scenario-1-generic-model"></a>Scénář 1: Obecné modelu
Uživatel pracuje s obecné model, který může být dat použité tooa nového uživatele, například základní předpovědi časových řad dat nebo uživatelské metodu R s pokročilou analýzu. Tento uživatel publikuje hello model jako webovou službu pro ostatní tooconsume s jejich daty.

* [Binární klasifikátor](machine-learning-r-csharp-binary-classifier.md)
* [Model clusteru](machine-learning-r-csharp-cluster-model.md)
* [Lineární regrese s množstvím proměnných](machine-learning-r-csharp-multivariate-linear-regression.md)
* [Vytváření prognóz – Exponenciální vyhlazování](machine-learning-r-csharp-forecasting-exponential-smoothing.md)
* [ETS + prognózy STL](machine-learning-r-csharp-retail-demand-forecasting.md)
* [Vytváření prognóz - Autoregressive integrované klouzavý průměr (ARIMA)](machine-learning-r-csharp-arima.md)
* [Analýza přežití](machine-learning-r-csharp-survival-analysis.md)

## <a name="scenario-2-trained-model--specific-data"></a>Scénář 2: Trained model – konkrétní data
Uživatel, který má data, která poskytuje užitečné předpovědi prostřednictvím kódu jazyka R, například velké vzorku charakteru dotazníků clusterovaný prostřednictvím uživatele k-means algoritmus toopredict hello charakteru typu nebo stavu prozkoumáte data, která lze použít toopredict jednotlivého uživatele riziko pro plicní rakoviny s balíčkem analysis R základními. uživatel Hello publikuje data hello prostřednictvím webové služby, který bude předpovídat výsledek nového uživatele.

## <a name="scenario-3-trained-model--generic-data"></a>Scénář 3: Trained model – generických dat.
Uživatel má obecné data (například svátek text), který umožňuje toobe webové služby vytvořené a u obecně do různých typů případy použití a scénáře.

* [Analýza mínění na základě slovníku](machine-learning-r-csharp-lexicon-based-sentiment-analysis.md)

## <a name="scenario-4-advanced-calculator"></a>Scénář 4: Pokročilé kalkulačky
Uživatel poskytuje pokročilé výpočty nebo simulace, které nevyžadují žádné trained model, nebo přizpůsobování modelu toohello uživatelská data.

* [Testování rozdílu v proporcích](machine-learning-r-csharp-difference-in-two-proportions.md)
* [Sada normální distribuce](machine-learning-r-csharp-normal-distribution.md)
* [Sada binomické distribuce](machine-learning-r-csharp-binomial-distribution.md)

## <a name="faq"></a>Nejčastější dotazy
Nejčastější dotazy o spotřebě hello webové služby nebo publikování toohello Marketplace, najdete v části [zde](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-web-service-examples/machine-learning-r-code-options-for-using-and-sharing-cloud.png



