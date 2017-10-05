---
title: "(nepoužívané) Multivariační lineární regrese - Azure | Microsoft Docs"
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
redirect_document_id: TRUE
ms.openlocfilehash: 65a8005139e920cd19593e954fc1bf836354bdf3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deprecated-multivariate-linear-regression"></a>(nepoužívané) Multivariační lineární regrese

> [!NOTE]
> Microsoft DataMarket se postupně vyřazuje z provozu a toto rozhraní API je zastaralá. 
> 
> Můžete najít mnoho užitečné příklad experimentů a rozhraní API v [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com). Další informace o galerii najdete v tématu [sdílené složky a zjišťovat prostředky ve Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).

Předpokládejme, že máte datovou sadu a chcete rychle předpovědi závislé proměnné (y) pro každého uživatele (i), na základě nezávislých proměnných. Lineární regrese je Oblíbené statistické technika pro takové předpovědi. Zde se předpokládá y závislé proměnné, jako hodnotu nepřetržitá.  

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Může být jednoduchého scénáře, kde zkušeností pokouší předvídat váhu konkrétního (y) podle jejich výška (x). Může být pokročilejších scénářích, kde zkušeností Další informace o jednotlivých (například váhy, pohlaví, soupeření) a pokusí se předpovědi váhu jednotlivých. To [webovou službu](https://datamarket.azure.com/dataset/aml_labs/multivariate_regression) vyhovuje model lineární regrese na data a výstupy předpovězené hodnoty (y) pro každý pozorování v datech.

> Tato webová služba může spotřebovat uživateli – potenciálně prostřednictvím mobilní aplikace prostřednictvím webu, nebo i v místním počítači, třeba. Ale účelem webové služby je také slouží jako příklad použití Azure Machine Learning k vytvoření webové služby v kódu jazyka R. Zadání několika řádků kódu jazyka R a klikne na tlačítko v rámci Azure Machine Learning Studio můžete experimentu vytvořené pomocí kódu jazyka R a publikovat jako webovou službu. Webové služby můžete pak publikování na webu Azure Marketplace a spotřebovávají uživatelů a zařízení po celém světě s žádné nastavení infrastruktury autorem webové služby.  
> 
> 

## <a name="consumption-of-web-service"></a>Spotřeba webové služby
Tato webová služba poskytuje předpovězené hodnoty proměnné závislé na základě nezávislých proměnných pro všechny připomínky. Webová služba očekává, že koncovému uživateli umožňují vstupní data jako řetězec, kde jsou řádky oddělených čárkami (,) a sloupce jsou odděleny středníkem (;). Webová služba očekává 1 řádek v čase a očekává první sloupec jako závislé proměnné. Datové sadě služby příkladu může vypadat například takto:

![Ukázková data][1]

Připomínky bez závislé proměnné by měl zadejte jako hodnotu "NA" pro y. Data vstup pro výše uvedené datová sada by být následující řetězec: "10, 5, 2,18; 1; 6,6; 5.3; 2.1,7; 5; 5,22; 3; 4,12; 2, 1, NA, 3, 4". Výstup je předpovězené hodnoty pro každý z jeho řádků na základě nezávislých proměnných. 

> Tato služba jako hostované v Azure Marketplace je službou OData. To může být volána prostřednictvím metody POST nebo GET. 
> 
> 

Existuje více způsobů využívání služby automatizovaně (je třeba aplikace [sem](http://microsoftazuremachinelearning.azurewebsites.net/MultipleLinearRegressionService.aspx)).

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
> Této webové služby byl vytvořen pomocí Azure Machine Learning. Pro bezplatnou zkušební verzi, jakož i úvodní videa na vytváření experimentů a [publikování webových služeb](machine-learning-publish-a-machine-learning-web-service.md), najdete v tématu [azure.com/ml](http://azure.com/ml). Níže je snímek obrazovky experimentu, který vytvořené pro každou z modulů v rámci experimentu webové služby a příklad kódu.
> 
> 

Ze v rámci Azure Machine Learning, nový prázdný experiment byl vytvořen a dvě [spustit skript jazyka R] [ execute-r-script] moduly byly vyžádány na pracovním prostoru. Tato webová služba běží experimentu Azure Machine Learning s základní skript jazyka R. Existují 2 částí do tohoto experimentu: definice schématu a školení modelu + vyhodnocování. První modul definuje vstupní datové sady, kde je první proměnná závislé proměnné a zbývající proměnné jsou nezávislé na očekávané struktuře. Druhý modul vyhovuje model lineární regrese obecný pro vstupní data.  

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
Toto je velmi jednoduchý příklad více lineární regrese webové služby. Jak je vidět z výše uvedeném příkladu kódu, je implementovaný zachycení žádné chyby a službu předpokládá, že všechno, co je nepřetržitá proměnné (bez kategorií funkcí povoleny), jako služba číselné hodnoty vstupy pouze v době vytvoření této webové služby. Navíc služba aktuálně zpracovává velikost omezenou dat, vyřízení požadavků a odpovědí povaze volání webové služby a fakt, že je probíhá podle modelu pokaždé, když je volat webovou službu. 

## <a name="faq"></a>Nejčastější dotazy
Nejčastější dotazy o spotřebě webové služby nebo publikování na webu Azure Marketplace naleznete v části [zde](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-multivariate-linear-regression/multireg-img1.png
[2]: ./media/machine-learning-r-csharp-multivariate-linear-regression/multireg-img2.png
[3]: ./media/machine-learning-r-csharp-multivariate-linear-regression/multireg-img3.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/

