---
title: "(nepoužívané) Rozdíl v testu proporce - Azure | Microsoft Docs"
description: "(nepoužívané) Rozdíl v proporce testu"
services: machine-learning
documentationcenter: 
author: aniedea
manager: jhubbard
editor: cgronlun
ms.assetid: 9356b821-5345-44f6-8e26-719f2dea5e6d
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: aniedea
ROBOTS: NOINDEX
redirect_url: https://gallery.cortanaintelligence.com/
redirect_document_id: TRUE
ms.openlocfilehash: a08f91ca76eef2562caeb9eb64cec5e549ed2f5f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deprecated-difference-in-proportions-test"></a>(nepoužívané) Rozdíl v proporce testu

> [!NOTE]
> Microsoft DataMarket se postupně vyřazuje z provozu a toto rozhraní API je zastaralá. 
> 
> Můžete najít mnoho užitečné příklad experimentů a rozhraní API v [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com). Další informace o galerii najdete v tématu [sdílené složky a zjišťovat prostředky ve Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).

Jsou dvě proporce statisticky různých? Předpokládejme, že uživatel chce k porovnání dvou filmy určit, pokud má jedna film výrazně vyšší poměr 'líbí, při porovnání na druhý. S ukázkovým velké může být statisticky velký rozdíl mezi proporce 0,50 a 0,51. Pomocí malé ukázkové nemusí být dostatek dat. určí, jestli je ve skutečnosti různých tyto poměr stran. 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

To [webovou službu](https://datamarket.azure.com/dataset/aml_labs/prop_test) provádí předpoklad testu rozdíl ve dvou hodnot založena na vstup uživatele počet úspěchů a celkový počet pokusů pro skupiny 2 porovnání. V jeden z možných scénářů této webové služby může být volána v rámci filmová porovnání aplikace, která uživatele vyzve, zda mezi filmy je skutečně 'líbilo' častěji než druhý, na základě film hodnocení.

> Tato webová služba může spotřebovat uživateli – potenciálně prostřednictvím mobilní aplikace prostřednictvím webu, nebo i v místním počítači, třeba. Ale účelem webové služby je také slouží jako příklad použití Azure Machine Learning k vytvoření webové služby v kódu jazyka R. Zadání několika řádků kódu jazyka R a klikne na tlačítko v rámci Azure Machine Learning Studio můžete experimentu vytvořené pomocí kódu jazyka R a publikovat jako webovou službu. Webové služby můžete pak publikování na webu Azure Marketplace a spotřebovávají uživatelů a zařízení po celém světě s žádné nastavení infrastruktury autorem webové služby.
> 
> 

## <a name="consumption-of-web-service"></a>Spotřeba webové služby
Tato služba přijímá argumenty 4 a nepodporuje testování předpoklad z poměr stran.

Vstupní argumenty jsou:

* Successes1 - počet událostí úspěšného v ukázce 1.
* Successes2 - počet událostí úspěšného v ukázce 2.
* Total1 – velikost vzorku 1.
* Total2 – velikost vzorku 2.

Výstup služby je výsledkem předpoklad testování společně s chi-square statistiky, df, p hodnota a deklarovaná čistá nebo hrubá v ukázkové 1/2 a interval spolehlivosti hranice.

> Tato služba jako hostované v Azure Marketplace je službou OData. To může být volána prostřednictvím metody POST nebo GET. 
> 
> 

Existuje více způsobů využívání služby automatizovaně (je třeba aplikace [sem](http://microsoftazuremachinelearning.azurewebsites.net/DifferenceInProportionsTest.aspx)).

### <a name="starting-c-code-for-web-service-consumption"></a>Spouštění kódu C# pro používání webové služby:
    public class Input
    {
            public string successes1;
            public string successes2;
            public string total1;
            public string total2;
    }

    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }

    void Main()
    {
            var input = new Input() { successes1 = TextBox1.Text, successes2 = TextBox2.Text, total1 = TextBox3.Text, total2 = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();

            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));

            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }


## <a name="creation-of-web-service"></a>Vytvoření webové služby
> Této webové služby byl vytvořen pomocí Azure Machine Learning. Pro bezplatnou zkušební verzi, jakož i úvodní videa na vytváření experimentů a [publikování webových služeb](machine-learning-publish-a-machine-learning-web-service.md), najdete v tématu [azure.com/ml](http://azure.com/ml). Níže je snímek obrazovky experimentu, který vytvořené pro každou z modulů v rámci experimentu webové služby a příklad kódu.
> 
> 

Ze v rámci Azure Machine Learning, nový prázdný experiment byl vytvořen se dvěma [spustit skript jazyka R] [ execute-r-script] moduly. V první modul, který je definován schéma dat zatímco druhý modul používá příkaz prop.test v rámci R Pokud chcete provést test předpoklad 2 poměr stran. 

### <a name="experiment-flow"></a>Experiment toku:
![Tok experimentu][2]

#### <a name="module-1"></a>Modul 1:
    ####Schema definition  
    data.set=data.frame(successes1=50,successes2=60,total1=100,total2=100);
    maml.mapOutputPort("data.set"); #send data to output port
    dataset1 = maml.mapInputPort(1) #read data from input port


#### <a name="module-2"></a>Modul 2:
    test=prop.test(c(dataset1$successes1[1],dataset1$successes2[1]),c(dataset1$total1[1],dataset1$total2[1])) #conduct hypothesis test

    result=data.frame(
    result=ifelse(test$p.value<0.05,"The proportions are different!",
    "The proportions aren't different statistically."),
    ChiSquarestatistic=round(test$statistic,2),df=test$parameter,
    pvalue=round(test$p.value,4),
    proportion1=round(test$estimate[1],4),
    proportion2=round(test$estimate[2],4),
    confintlow=round(test$conf.int[1],4),
    confinthigh=round(test$conf.int[2],4)) 

    maml.mapOutputPort("result"); #output port


## <a name="limitations"></a>Omezení
Toto je velmi jednoduchý příklad pro test rozdíl v poměru 2. Jak je vidět z výše uvedený ukázkový kód, je implementovaný zachycení žádné chyby a službu předpokládá, že se průběžné všech proměnných.

## <a name="faq"></a>Nejčastější dotazy
Nejčastější dotazy o spotřebě webové služby nebo publikování na webu Azure Marketplace naleznete v části [zde](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-difference-in-two-proportions/hyptest-img1.png
[2]: ./media/machine-learning-r-csharp-difference-in-two-proportions/hyptest-img2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/

