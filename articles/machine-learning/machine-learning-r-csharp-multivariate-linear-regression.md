---
title: "AAA(Deprecated) Multivariate lineární regrese - Azure | Microsoft Docs"
description: "(nepoužívané) Multivariační lineární regrese"
services: machine-learning
documentationcenter: 
author: jaymathe
manager: jhubbard
editor: cgronlun
ms.assetid: 2fb78220-ced9-4564-a439-9e5df6772994
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
ms.openlocfilehash: 0ff7221cd06c0ef059b0c5bf327016588174dcfe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deprecated-multivariate-linear-regression"></a>(nepoužívané) Multivariační lineární regrese

> [!NOTE]
> Hello Microsoft DataMarket se postupně vyřazuje z provozu a toto rozhraní API je zastaralá. 
> 
> Mnoho užitečné příklad experimentů a rozhraní API můžete najít v hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com). Další informace o hello Galerie najdete v tématu [sdílené složky a zjišťovat prostředky ve hello Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).

Předpokládejme, že máte datovou sadu a by jako tooquickly předpovědi závislé proměnné (y) pro každého uživatele (i), na základě nezávislých proměnných. Lineární regrese je Oblíbené statistické technika pro takové předpovědi. Hello závislé proměnné y tady se předpokládá, že toobe průběžné hodnoty.  

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Může být jednoduchý scénář, kde hello zkušeností se pokouší toopredict hello váhu konkrétního (y) podle jejich výška (x). Pokročilejších scénářích může být, kde hello zkušeností Další informace o jednotlivých (například váhy, pohlaví, soupeření) hello a pokusí toopredict hello váhu individuální hello. To [webovou službu](https://datamarket.azure.com/dataset/aml_labs/multivariate_regression) pro rozlišení hello data toohello model lineární regrese a výstupy hello předpovězené hodnoty (y) pro každý hello pozorování v datech hello.

> Tato webová služba může spotřebovat uživateli – potenciálně prostřednictvím mobilní aplikace prostřednictvím webu, nebo i v místním počítači, třeba. Ale hello účelem hello webové služby je také tooserve jako příklad, jak Azure Machine Learning lze použít toocreate webové služby v kódu jazyka R. Zadání několika řádků kódu jazyka R a klikne na tlačítko v rámci Azure Machine Learning Studio můžete experimentu vytvořené pomocí kódu jazyka R a publikovat jako webovou službu. Hello webové služby může být pak publikované toohello Azure Marketplace a využívat tak, že uživatelé a zařízení napříč hello, world s žádné nastavení infrastruktury autorem hello hello webové služby.  
> 
> 

## <a name="consumption-of-web-service"></a>Spotřeba webové služby
Tento web service poskytuje hello předpovědět hodnoty proměnné hello závislé na základě nezávislých proměnných hello všech hello připomínky. Webová služba Hello očekává hello koncový uživatel tooinput data jako řetězec, kde jsou řádky oddělených čárkami (,) a sloupce jsou odděleny středníkem (;). Webová služba Hello očekává 1 řádek v čase a očekává hello první sloupec toobe hello závislé proměnné. Datové sadě služby příkladu může vypadat například takto:

![Ukázková data][1]

Připomínky bez závislé proměnné by měl zadejte jako hodnotu "NA" pro y. Hello vstupních dat pro hello výše datová sada by být hello následující řetězec: "10, 5, 2,18; 1; 6,6; 5.3; 2.1,7; 5; 5,22; 3; 4,12; 2, 1, NA, 3, 4". Hello výstup hello předpovězené hodnoty pro každý hello řádků podle hello nezávislých proměnných. 

> Tato služba jako hostované na hello Azure Marketplace, je službou OData. To může být volána prostřednictvím metody POST nebo GET. 
> 
> 

Existuje více způsobů využívání služby hello automatizovaně (je třeba aplikace [sem](http://microsoftazuremachinelearning.azurewebsites.net/MultipleLinearRegressionService.aspx)).

### <a name="starting-c-code-for-web-service-consumption"></a>Spouštění kódu C# pro používání webové služby:
    public class Input
    {
            public string value;
    }

    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }

    void Main()
    {
            var input = new Input() { value = TextBox1.Text };
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

Ze v rámci Azure Machine Learning, nový prázdný experiment byl vytvořen a dvě [spustit skript jazyka R] [ execute-r-script] moduly byly vyžádány do pracovního prostoru hello. Tato webová služba běží experimentu Azure Machine Learning s základní skript jazyka R. Existují experimentovat toothis 2 částí: definice schématu a školení modelu + vyhodnocování. první modul Hello definuje hello očekávána struktura hello vstupní datové sady, kde první proměnná hello je hello závislé proměnné a hello zbývající proměnné jsou nezávislé. druhý modul Hello vyhovuje model lineární regrese obecný pro vstupní data hello.  

![Tok experimentu][3]

#### <a name="module-1"></a>Modul 1:
#### <a name="schema-definition"></a>Definice schématu
    data <- data.frame(value = "1;2;3,4;5;6,7;8;9", stringsAsFactors=FALSE) maml.mapOutputPort("data");  

#### <a name="module-2"></a>Modul 2:
#### <a name="lm-modeling"></a>Modelování LM
    data <- maml.mapInputPort(1) # class: data.frame  

    data.split <- strsplit(data[1,1], ",")[[1]]  
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)  
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)  
    data.split <- as.data.frame(t(data.split)) 
    data.split <- data.matrix(data.split) 
    data.split <- data.frame(data.split) 
    model <- lm(data.split)  

    out=data.frame(predict(model,data.split))  
    out <- data.frame(t(out))

    maml.mapOutputPort("out");  

## <a name="limitations"></a>Omezení
Toto je velmi jednoduchý příklad více lineární regrese webové služby. Jak je vidět z hello ukázkový kód výše, je implementována zachycení žádné chyby a hello služby předpokládá, že všechno, co je nepřetržitá proměnné (bez kategorií funkcí povoleny), jako hello služby pouze vstupy číselných hodnot v době hello hello vytvoření tomuto webovému Služba. Navíc služba hello aktuálně zpracovává velikost omezenou dat, z důvodu toohello požadavků a odpovědí povaha hello webové služby je se volání a hello skutečnost, že hello modelu odpovídat pokaždé, když je volána hello webové služby. 

## <a name="faq"></a>Nejčastější dotazy
Nejčastější dotazy o spotřebě hello webové služby nebo publikování toohello Azure Marketplace naleznete v části [zde](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-multivariate-linear-regression/multireg-img1.png
[2]: ./media/machine-learning-r-csharp-multivariate-linear-regression/multireg-img2.png
[3]: ./media/machine-learning-r-csharp-multivariate-linear-regression/multireg-img3.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/

