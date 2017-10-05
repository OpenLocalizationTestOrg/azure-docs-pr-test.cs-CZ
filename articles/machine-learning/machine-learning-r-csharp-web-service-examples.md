---
title: "(nepoužívané) Machine learning webových služeb příklady vytvořené s R - Azure | Microsoft Docs"
description: "(nepoužívané) Najít užitečné sadu webové služby příklady jsou vytvořené pomocí kódu jazyka R a Machine Learning a publikované na webu Azure Marketplace."
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
redirect_document_id: TRUE
ms.openlocfilehash: 9514025db6f812f9e7934ea2d1575e948d6585b0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deprecated-web-services-examples-using-r-code-on-azure-machine-learning-and-published-to-microsoft-azure-marketplace"></a>(nepoužívané) Webové služby příklady použití kódu jazyka R v Azure Machine Learning a publikované na webu Microsoft Azure Marketplace

> [!NOTE]
> Microsoft DataMarket se postupně vyřazuje z provozu a tato rozhraní API jsou zastaralé. 
> 
> Můžete najít mnoho užitečné příklad experimentů a rozhraní API v [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com). Další informace o galerii najdete v tématu [sdílené složky a zjišťovat prostředky ve Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).

V tomto článku jsou třeba webové služby byly vytvořené pomocí Azure Machine Learning a publikována ve službě Azure Marketplace. Každý webové službu příkladu má komplexní dokumentu připojené, vkládání ukázkových datových sad pro testování služeb a vysvětlení, jak uživatel může vytvořit podobnou službu sami. 

Uživatelé v Azure Machine Learning Studio, můžete napsat kód R a několika kliknutími publikujete ji jako webové služby pro aplikace a zařízení využívají po celém světě. 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Neměly jednoduché kalkulačky, které poskytují statistické funkce pro vytváření vlastní prediktivní analýzy postojích text dolování nové i zkušeného R uživatelé mohou mít prospěch ze usnadnění, pomocí kterého můžete uživatelům Azure Machine Learning zprovoznit R kód. Tyto webové, které by mohly spotřebovat služby uživateli, potenciálně prostřednictvím mobilní aplikace nebo webu, účel tyto příklady webové služby je zobrazit, jak můžete zprovoznit Machine Learning R skriptů pro analytické účely a použije k vytvoření webové služby v horní části R kódu.

Každý příklad obsahuje příklad jazyka C# pro používání webové služby.

![Diagram R kódu v Azure Machine Learning: R řešení pro chráněné pomocí publikování nebo v Azure Marketplace.][1]

Zvažte následující scénáře.

## <a name="scenario-1-generic-model"></a>Scénář 1: Obecné modelu
Uživatel pracuje s obecné model, který lze použít pro nové uživatele data, například základní předpovědi časových řad dat nebo uživatelské metodu R s pokročilou analýzu. Tento uživatel publikuje model jako webovou službu, aby jej ostatní mohli používat s jejich daty.

* [Binární klasifikátor](machine-learning-r-csharp-binary-classifier.md)
* [Model clusteru](machine-learning-r-csharp-cluster-model.md)
* [Lineární regrese s množstvím proměnných](machine-learning-r-csharp-multivariate-linear-regression.md)
* [Vytváření prognóz – Exponenciální vyhlazování](machine-learning-r-csharp-forecasting-exponential-smoothing.md)
* [ETS + prognózy STL](machine-learning-r-csharp-retail-demand-forecasting.md)
* [Vytváření prognóz - Autoregressive integrované klouzavý průměr (ARIMA)](machine-learning-r-csharp-arima.md)
* [Analýza přežití](machine-learning-r-csharp-survival-analysis.md)

## <a name="scenario-2-trained-model--specific-data"></a>Scénář 2: Trained model – konkrétní data
Uživatel, který má data, která poskytuje užitečné předpovědi prostřednictvím kódu jazyka R, jako je například velké vzorku charakteru dotazníků clusteru prostřednictvím k-means algoritmus k předvídání uživatele charakteru typu nebo stavu průzkum data, která mohou být použity k předpovědi riziko jednotlivého uživatele pro plicní rakoviny s balíčkem analysis R základními. Uživatel publikuje data prostřednictvím webové služby, který bude předpovídat výsledek nového uživatele.

## <a name="scenario-3-trained-model--generic-data"></a>Scénář 3: Trained model – generických dat.
Uživatel má obecný data (například svátek text), která umožňuje webové služby vytvořené a obecně uplatnit na různé typy případy použití a scénáře.

* [Analýza mínění na základě slovníku](machine-learning-r-csharp-lexicon-based-sentiment-analysis.md)

## <a name="scenario-4-advanced-calculator"></a>Scénář 4: Pokročilé kalkulačky
Uživatel poskytuje pokročilé výpočty nebo simulace, které nevyžadují žádné trained model, nebo přizpůsobování modelu k datům uživatele.

* [Testování rozdílu v proporcích](machine-learning-r-csharp-difference-in-two-proportions.md)
* [Sada normální distribuce](machine-learning-r-csharp-normal-distribution.md)
* [Sada binomické distribuce](machine-learning-r-csharp-binomial-distribution.md)

## <a name="faq"></a>Nejčastější dotazy
Nejčastější dotazy o spotřebě webové služby nebo publikování na webu Marketplace, najdete v části [zde](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-web-service-examples/machine-learning-r-code-options-for-using-and-sharing-cloud.png



