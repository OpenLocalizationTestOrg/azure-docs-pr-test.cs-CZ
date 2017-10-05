---
title: "(nepoužívané) Normální rozdělení webové služby Suite - Azure | Microsoft Docs"
description: "(nepoužívané) Normální rozdělení webové služby sady"
services: machine-learning
documentationcenter: 
author: ireiter
manager: jhubbard
editor: cgronlun
ms.assetid: aab7b92e-953b-43d8-b0af-031394406bfe
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: ireiter
ROBOTS: NOINDEX
redirect_url: https://gallery.cortanaintelligence.com/
redirect_document_id: TRUE
ms.openlocfilehash: 79d1621330ad56b0c62ca46cfac424c2306e371f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deprecated-normal-distribution-suite"></a>(nepoužívané) Sada normálního rozdělení

> [!NOTE]
> Microsoft DataMarket se postupně vyřazuje z provozu a toto rozhraní API je zastaralá. 
> 
> Můžete najít mnoho užitečné příklad experimentů a rozhraní API v [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com). Další informace o galerii najdete v tématu [sdílené složky a zjišťovat prostředky ve Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).

Sada normálního rozdělení je sada ukázkové webové služby ([generátor](https://datamarket.azure.com/dataset/aml_labs/ndg7), [Quantile kalkulačky](https://datamarket.azure.com/dataset/aml_labs/ndq5), [pravděpodobnosti kalkulačky](https://datamarket.azure.com/dataset/aml_labs/ndp5)), které pomoci v generování a zpracování Normální distribuce. Služby umožňují generování posloupnost délka, výpočet quantiles z daného pravděpodobnosti a výpočet pravděpodobnosti z daného quantile normálního rozdělení. Jednotlivé služby vysílá různých výstupů založené na vybraná služba (viz popis dole). Sada normálního rozdělení je založena na qnorm funkce R, rnorm a pnorm, které jsou součástí balíčku R statistiky.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

> Tato webová služba může spotřebovat uživateli – potenciálně prostřednictvím mobilní aplikace prostřednictvím webu, nebo i v místním počítači, třeba. Ale účelem webové služby je také slouží jako příklad použití Azure Machine Learning k vytvoření webové služby v kódu jazyka R. Zadání několika řádků kódu jazyka R a klikne na tlačítko v rámci Azure Machine Learning Studio můžete experimentu vytvořené pomocí kódu jazyka R a publikovat jako webovou službu. Webové služby můžete pak publikování na webu Azure Marketplace a spotřebovávají uživatelů a zařízení po celém světě s žádné nastavení infrastruktury autorem webové služby.  
> 
> 

## <a name="consumption-of-web-service"></a>Spotřeba webové služby
Sada normálního rozdělení zahrnuje následující 3 služby.

### <a name="normal-distribution-quantile-calculator"></a>Kalkulačky Quantile normálního rozdělení
Tato služba přijímá 4 argumenty normálního rozdělení a vypočítá přidružené quantile.

Vstupní argumenty jsou:

* p – jeden pravděpodobnost událost normálního rozdělení. 
* Střední – střední normálního rozdělení.
* SD - směrodatnou odchylku normálního rozdělení. 
* Straně - L pro na dolní straně distribuce a U pro rozdělení na horní straně.

Výstup služby je počítaný quantile, který je přidružen daný pravděpodobnosti.

### <a name="normal-distribution-probability-calculator"></a>Normální rozdělení pravděpodobnosti kalkulačky
Tato služba přijímá 4 argumenty normálního rozdělení a vypočítá přidružené pravděpodobnosti.

Vstupní argumenty jsou:

* q - a jeden quantile událost normálního rozdělení. 
* Střední – střední normálního rozdělení.
* SD - směrodatnou odchylku normálního rozdělení. 
* Straně - L pro na dolní straně distribuce a U pro rozdělení na horní straně.

Výstup služby je počítaný pravděpodobnost, že je přidružen daný quantile.

### <a name="normal-distribution-generator"></a>Generátor normálního rozdělení
Tato služba přijímá 3 argumenty normálního rozdělení a generuje náhodné pořadí čísel, který je obvykle distribuován. Následující argumenty by měly být zadané ho v rámci žádosti:

* n-počet pozorování. 
* Střední – střední normálního rozdělení.
* SD - směrodatnou odchylku normálního rozdělení. 

Výstup služby je posloupnost délku n s normální rozdělení, podle střední a sd argumenty.

> Tato služba jako hostované v Azure Marketplace je službou OData. To může být volána prostřednictvím metody POST nebo GET. 
> 
> 

Existuje více způsobů využívání služby automatizovaně (například aplikace jsou tady: [generátor](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionGenerator.aspx), [pravděpodobnosti kalkulačky](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionProbabilityCalculator.aspx), [Quantile kalkulačky](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionQuantileCalculator.aspx)).

### <a name="starting-c-code-for-web-service-consumption"></a>Spouštění kódu C# pro používání webové služby:
### <a name="normal-distribution-quantile-calculator"></a>Kalkulačky Quantile normálního rozdělení
    public class Input
    {
            public string p;
            public string mean;
            public string sd;
            public string side;
    }

    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }

    void Main()
    {
            var input = new Input() { p = TextBox1.Text, mean = TextBox2.Text, sd = TextBox3.Text, side = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();

            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));

            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }


### <a name="normal-distribution-probability-calculator"></a>Normální rozdělení pravděpodobnosti kalkulačky
    public class Input
    {
            public string q;
            public string mean;
            public string sd;
            public string side;
    }

    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }

    void Main()
    {
            var input = new Input() { q = TextBox1.Text, mean = TextBox2.Text, sd = TextBox3.Text, side = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();

            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));

            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }

### <a name="normal-distribution-generator"></a>Generátor normálního rozdělení
    public class Input
    {
            public string n;
            public string mean;
            public string sd;
    }

    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }

    void Main()
    {
            var input = new Input() { n = TextBox1.Text, mean = TextBox2.Text, sd = TextBox3.Text };
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
> Této webové služby byl vytvořen pomocí Azure Machine Learning. Pro bezplatnou zkušební verzi, jakož i úvodní videa na vytváření experimentů a [publikování webových služeb](machine-learning-publish-a-machine-learning-web-service.md), najdete v tématu [azure.com/ml](http://azure.com/ml). 
> 
> 

Níže je snímek obrazovky experimentu, který vytvořené pro každou z modulů v rámci experimentu webové služby a příklad kódu.

### <a name="normal-distribution-quantile-calculator"></a>Kalkulačky Quantile normálního rozdělení
Experiment toku:

![Tok experimentu][2]

    #Data schema with example data (replaced with data from web service)
    data.set=data.frame(p=0.1,mean=0,sd=1,side='L');
    maml.mapOutputPort("data.set"); #send data to output port

    # Map 1-based optional input ports to variables
    dataset1 <- maml.mapInputPort(1) # class: data.frame

    param = dataset1
    if (param$p < 0 ) {
    print('Bad input: p must be between 0 and 1')
    param$p = 0
    } else if (param$p > 1) {
    print('Bad input: p must be between 0 and 1')
    param$p = 1
    }
    q = qnorm(param$p,mean=param$mean,sd=param$sd)

    if (param$side == 'U'){
    q = 2* param$mean - q
    } else if (param$side =='L') {
    q = q
    } else {
    print("Invalid side choice")
    }

    output = as.data.frame(q)

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");

### <a name="normal-distribution-probability-calculator"></a>Normální rozdělení pravděpodobnosti kalkulačky
Experiment toku:

![Tok experimentu][3]

     #Data schema with example data (replaced with data from web service)
    data.set=data.frame(q=-1,mean=0,sd=1,side='L');
    maml.mapOutputPort("data.set"); #send data to output port

    # Map 1-based optional input ports to variables
    dataset1 <- maml.mapInputPort(1) # class: data.frame

    param = dataset1
    prob = pnorm(param$q,mean=param$mean,sd=param$sd)

    if (param$side == 'U'){
    prob = 1 - prob
    } else if (param$side =='B') {
    prob = ifelse(prob<=0.5,prob * 2, 1)
    } else if (param$side =='L') {
    prob = prob
    } else {
    print("Invalid side choice")
    }

    output = as.data.frame(prob)

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");

### <a name="normal-distribution-generator"></a>Generátor normálního rozdělení
Experiment toku:

![Tok experimentu][4]

    #Data schema with example data (replaced with data from web service)
    data.set=data.frame(n=50,mean=0,sd=1);
    maml.mapOutputPort("data.set"); #send data to output port

    # Map 1-based optional input ports to variables
    dataset1 <- maml.mapInputPort(1) # class: data.frame

    param = dataset1
    dist = rnorm(param$n,mean=param$mean,sd=param$sd)

    output = as.data.frame(t(dist))

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");

## <a name="limitations"></a>Omezení
Toto jsou velmi jednoduché příklady, které obaluje normálního rozdělení. Jak je vidět z výše uvedeném příkladu kódu, málo zachytávání chyb je implementováno.

## <a name="faq"></a>Nejčastější dotazy
Nejčastější dotazy o spotřebě webové služby nebo publikování na webu Azure Marketplace naleznete v části [zde](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-normal-distribution/normal-img1.png
[2]: ./media/machine-learning-r-csharp-normal-distribution/normal-img2.png
[3]: ./media/machine-learning-r-csharp-normal-distribution/normal-img3.png
[4]: ./media/machine-learning-r-csharp-normal-distribution/normal-img4.png

