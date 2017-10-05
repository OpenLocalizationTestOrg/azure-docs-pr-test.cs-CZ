---
title: "(nepoužívané) Vytváření prognóz - exponenciální vyrovnání - Azure | Microsoft Docs"
description: "(nepoužívané) Webové služby: exponenciální prognózy vyhlazení"
services: machine-learning
documentationcenter: 
author: yijichen
manager: jhubbard
editor: cgronlun
ms.assetid: a4150681-6eac-4145-9eca-0cdf60781dde
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: yijichen
ROBOTS: NOINDEX
redirect_url: https://gallery.cortanaintelligence.com/
redirect_document_id: TRUE
ms.openlocfilehash: 736d5d4adb8ecfd1e3372d273b64917f4a2e76ce
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deprecated-forecasting---exponential-smoothing"></a>(nepoužívané) Vytváření prognóz - exponenciální vyrovnání

> [!NOTE]
> Microsoft DataMarket se postupně vyřazuje z provozu a toto rozhraní API je zastaralá. 
> 
> Můžete najít mnoho užitečné příklad experimentů a rozhraní API v [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com). Další informace o galerii najdete v tématu [sdílené složky a zjišťovat prostředky ve Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).

To [webovou službu](https://datamarket.azure.com/dataset/aml_labs/ets) implementuje model exponenciální vyhlazování (ETS) k vytvoření předpovědi na základě historických dat zadané uživatelem. Zvýší zatížení pro určitý produkt tohoto roku Může I předpovědi Moje produktu prodeje za Štědrý sezónu, tak, aby plánuji můžete efektivně Moje inventáře? Prognózy lze modely jsou výstižný adres takové dotazy. Zadaný posledních dat, zkontrolujte tyto modely skrytá trendy a sezónnosti předpovídat budoucí trendy.  

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

> Tato webová služba může spotřebovat uživateli – potenciálně prostřednictvím mobilní aplikace prostřednictvím webu, nebo i v místním počítači, třeba. Ale účelem webové služby je také slouží jako příklad použití Azure Machine Learning k vytvoření webové služby v kódu jazyka R. Zadání několika řádků kódu jazyka R a klikne na tlačítko v rámci Azure Machine Learning Studio můžete experimentu vytvořené pomocí kódu jazyka R a publikovat jako webovou službu. Webové služby můžete pak publikování na webu Azure Marketplace a spotřebovávají uživatelů a zařízení po celém světě s žádné nastavení infrastruktury autorem webové služby.
> 
> 

## <a name="consumption-of-web-service"></a>Spotřeba webové služby
Tato služba přijímá argumenty 4 a vypočítá ETS prognózy.
Vstupní argumenty jsou:

* Frekvence - určuje četnost (denně nebo týdně nebo měsíční nebo čtvrtletně nebo ročně) nezpracovaná data.
* Horizon - budoucí prognózy časového rámce.
* Date – přidání v nová data řady čas dobu.
* Hodnota – přidejte do nové časových řad datové hodnoty.

Výstup služby je počítané hodnoty prognózy.

Ukázka vstup může být: 

* Frekvence - 12
* Horizon - 12
* Datum - 1, 15 nebo 2012, 2, 15 nebo 2012, 3, 15 nebo 2012; 4, 15 nebo 2012; 5 nebo 15/2012; 6/15/2012; 15/7/2012, 8 / 15/2012, 9, 15 nebo 2012, 10, 15 nebo 2012; 11/15/2012, 12/15/2012; 1, 15 nebo 2013, 2, 15 nebo 2013, 3, 15 nebo 2013; 4, 15 nebo 2013; 5 nebo 15/2013; 6, 15 nebo 2013; 7, 15 nebo 2013, 8 / 15/2013, 9, 15 nebo 2013, 10, 15 nebo 2013; 11, 15 nebo 2013, 12/15/2013; 1, 15 nebo 2014, 2, 15 nebo 2014, 3, 15 nebo 2014; 4/15/2014; 5 nebo 15/2014; 6/15/2014; 7, 15 nebo 2014, 8 / 15/2014; 9, 15 nebo 2014
* Hodnota – 3.479; 3.68; 3.832; 3.941; 3.797; 3.586; 3.508; 3.731; 3.915; 3.844; 3.634; 3.549; 3.557; 3.785; 3.782; 3.601; 3.544; 3.556; 3.65; 3.709; 3.682; 3.511; 3.429; 3.51; 3.523; 3.525; 3.626; 3.695; 3.711; 3.711; 3.693; 3.571; 3.509

> Tato služba jako hostované v Azure Marketplace je službou OData. To může být volána prostřednictvím metody POST nebo GET. 
> 
> 

Existuje více způsobů využívání služby automatizovaně (je třeba aplikace [sem](http://microsoftazuremachinelearning.azurewebsites.net/etsForecasting.aspx)).

### <a name="starting-c-code-for-web-service-consumption"></a>Spouštění kódu C# pro používání webové služby:
    public class Input
    {
            public string frequency;
            public string horizon;
            public string date;
            public string value;
    }

    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }

    void Main()
    {
            var input = new Input() { frequency = TextBox1.Text, horizon = TextBox2.Text, date = TextBox3.Text, value = TextBox4.Text };
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

Z v rámci Azure Machine Learning, která vytvořila nový prázdný experiment. Ukázková vstupní data byla nahrána s předem definovaných datových schématu. Propojen s daty schéma je [spustit skript jazyka R] [ execute-r-script] modul, který generuje ETS prognózy modelu s použitím funkce 'ets' a 'prognózy, z R. 

![Tok experimentu][2]

#### <a name="module-1"></a>Modul 1:
    # Add in the CSV file with the data in the format shown below 
![Vzorek dat][3]    

#### <a name="module-2"></a>Modul 2:
    # Data input
    data <- maml.mapInputPort(1) # class: data.frame
    library(forecast)

    # Preprocessing
    colnames(data) <- c("frequency", "horizon", "dates", "values")
    dates <- strsplit(data$dates, ";")[[1]]
    values <- strsplit(data$values, ";")[[1]]

    dates <- as.Date(dates, format = '%m/%d/%Y')
    values <- as.numeric(values)

    # Fit a time-series model
    train_ts<- ts(values, frequency=data$frequency)
    fit1 <- ets(train_ts)
    train_model <- forecast(fit1, h = data$horizon)
    plot(train_model)

    # Produce forcasting
    train_pred <- round(train_model$mean,2)
    data.forecast <- as.data.frame(t(train_pred))
    colnames(data.forecast) <- paste("Forecast", 1:data$horizon, sep="")

    # Data output
    maml.mapOutputPort("data.forecast");


## <a name="limitations"></a>Omezení
Toto je velmi jednoduchý příklad pro ETS prognózy. Jak je vidět z výše uvedený ukázkový kód, žádné chyby zachytávání je implementována a službu předpokládá, že jsou všechny proměnné průběžné nebo kladnou hodnoty a frekvenci musí být celé číslo větší než 1. Délka hodnotu data a vektory musí být stejná. Proměnnou datum by měl splňovat ve formátu ' mm/dd/rrrr'.

## <a name="faq"></a>Nejčastější dotazy
Nejčastější dotazy o spotřebě webové služby nebo publikování na webu Azure Marketplace naleznete v části [zde](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-forecasting-exponential-smoothing/ets-img1.png
[2]: ./media/machine-learning-r-csharp-forecasting-exponential-smoothing/ets-img2.png
[3]: ./media/machine-learning-r-csharp-forecasting-exponential-smoothing/ets-img3.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/

