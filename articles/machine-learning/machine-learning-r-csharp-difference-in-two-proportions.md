---
title: "AAA(Deprecated) rozdíl v testu proporce - Azure | Microsoft Docs"
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
redirect_document_id: True
ms.openlocfilehash: 820aad377f9dec12b0ef455974aaa95f6e8d723a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deprecated-difference-in-proportions-test"></a>(nepoužívané) Rozdíl v proporce testu

> [!NOTE]
> Hello Microsoft DataMarket se postupně vyřazuje z provozu a toto rozhraní API je zastaralá. 
> 
> Mnoho užitečné příklad experimentů a rozhraní API můžete najít v hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com). Další informace o hello Galerie najdete v tématu [sdílené složky a zjišťovat prostředky ve hello Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).

Jsou dvě proporce statisticky různých? Předpokládejme, že uživatel chce toodetermine toocompare dva filmy, pokud jeden film má podstatně vyšší poměr 'líbí, při porovnání toohello jiné. S ukázkovým velké může být statisticky velký rozdíl mezi proporce hello 0,50 a 0,51. Pomocí malé ukázkové pravděpodobně není dostatek dat toodetermine Pokud tyto proporce ve skutečnosti liší. 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

To [webovou službu](https://datamarket.azure.com/dataset/aml_labs/prop_test) provádí předpoklad testu hello rozdíl dvou hodnot založena na vstup uživatele hello počet úspěchů a celkový počet pokusů pro skupiny porovnání hello 2 hello. V jeden z možných scénářů této webové služby může být volána v rámci filmová porovnání aplikace hello uživatele informuje, zda jeden filmy hello je skutečně 'líbilo' častěji než hello jiných, na základě film hodnocení.

> Tato webová služba může spotřebovat uživateli – potenciálně prostřednictvím mobilní aplikace prostřednictvím webu, nebo i v místním počítači, třeba. Ale hello účelem hello webové služby je také tooserve jako příklad, jak Azure Machine Learning lze použít toocreate webové služby v kódu jazyka R. Zadání několika řádků kódu jazyka R a klikne na tlačítko v rámci Azure Machine Learning Studio můžete experimentu vytvořené pomocí kódu jazyka R a publikovat jako webovou službu. Hello webové služby může být pak publikované toohello Azure Marketplace a využívat tak, že uživatelé a zařízení napříč hello, world s žádné nastavení infrastruktury autorem hello hello webové služby.
> 
> 

## <a name="consumption-of-web-service"></a>Spotřeba webové služby
Tato služba přijímá argumenty 4 a nepodporuje testování předpoklad z poměr stran.

Hello vstupní argumenty jsou:

* Successes1 - počet událostí úspěšného v ukázce 1.
* Successes2 - počet událostí úspěšného v ukázce 2.
* Total1 – velikost vzorku 1.
* Total2 – velikost vzorku 2.

výstup Hello hello služby je hello výsledek hello předpoklad testování společně s hello chi-square statistiky, df, p hodnota a deklarovaná čistá nebo hrubá v ukázkové 1/2 a interval spolehlivosti hranice.

> Tato služba jako hostované na hello Azure Marketplace, je službou OData. To může být volána prostřednictvím metody POST nebo GET. 
> 
> 

Existuje více způsobů využívání služby hello automatizovaně (je třeba aplikace [sem](http://microsoftazuremachinelearning.azurewebsites.net/DifferenceInProportionsTest.aspx)).

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
> Této webové služby byl vytvořen pomocí Azure Machine Learning. Pro bezplatnou zkušební verzi, jakož i úvodní videa na vytváření experimentů a [publikování webových služeb](machine-learning-publish-a-machine-learning-web-service.md), najdete v tématu [azure.com/ml](http://azure.com/ml). Níže je snímek obrazovky hello experimentu, který vytvořili hello webové služby a příklad kódu pro každou hello modulů v rámci experimentu hello.
> 
> 

Ze v rámci Azure Machine Learning, nový prázdný experiment byl vytvořen se dvěma [spustit skript jazyka R] [ execute-r-script] moduly. V modulu první hello je definována schéma dat hello, druhý modul hello používá příkaz prop.test hello v rámci R tooperform hello předpoklad testovací k 2 poměr stran. 

### <a name="experiment-flow"></a>Experiment toku:
![Tok experimentu][2]

#### <a name="module-1"></a>Modul 1:
    ####Schema definition  
    data.set=data.frame(successes1=50,successes2=60,total1=100,total2=100);
    maml.mapOutputPort("data.set"); #send data toooutput port
    dataset1 = maml.mapInputPort(1) #read data from input port


#### <a name="module-2"></a>Modul 2:
    test=prop.test(c(dataset1$successes1[1],dataset1$successes2[1]),c(dataset1$total1[1],dataset1$total2[1])) #conduct hypothesis test

    result=data.frame(
    result=ifelse(test$p.value<0.05,"hello proportions are different!",
    "hello proportions aren't different statistically."),
    ChiSquarestatistic=round(test$statistic,2),df=test$parameter,
    pvalue=round(test$p.value,4),
    proportion1=round(test$estimate[1],4),
    proportion2=round(test$estimate[2],4),
    confintlow=round(test$conf.int[1],4),
    confinthigh=round(test$conf.int[2],4)) 

    maml.mapOutputPort("result"); #output port


## <a name="limitations"></a>Omezení
Toto je velmi jednoduchý příklad pro test rozdíl v poměru 2. Jak je vidět z výše uvedený kód příklad hello, je implementovaný zachycení žádné chyby a hello služby předpokládá, že se všechny proměnné hello průběžné.

## <a name="faq"></a>Nejčastější dotazy
Nejčastější dotazy o spotřebě hello webové služby nebo publikování toohello Azure Marketplace naleznete v části [zde](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-difference-in-two-proportions/hyptest-img1.png
[2]: ./media/machine-learning-r-csharp-difference-in-two-proportions/hyptest-img2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/

