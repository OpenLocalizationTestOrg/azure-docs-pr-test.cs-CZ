---
title: "(nepoužívané) Cluster Model – Azure | Microsoft Docs"
description: "(nepoužívané) Model clusteru"
services: machine-learning
documentationcenter: 
author: FrancescaLazzeri
manager: jhubbard
editor: cgronlun
ms.assetid: 51b8d012-ed44-4312-920c-9c808dfd4ff6
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: lazzeri
ROBOTS: NOINDEX
redirect_url: https://gallery.cortanaintelligence.com/
redirect_document_id: TRUE
ms.openlocfilehash: 7cbbbd6d4236dab638eb3051595a584557480841
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deprecated-cluster-model"></a>(nepoužívané) Model clusteru

> [!NOTE]
> Microsoft DataMarket se postupně vyřazuje z provozu a toto rozhraní API je zastaralá. 
> 
> Můžete najít mnoho užitečné příklad experimentů a rozhraní API v [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com). Další informace o galerii najdete v tématu [sdílené složky a zjišťovat prostředky ve Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).

Jak jsme předpovědi skupiny chování držitelům platební karty snižte riziko poplatků vypnout vystavitelů platební karty? Jak jsme definovat skupiny charakteru vlastnosti zaměstnanců za účelem zlepšení výkonu v práci? Jak můžete lékařů klasifikovat pacientů do skupin na základě charakteristik jejich nákaz? V zásadě všechny tyto otázky může odpovědět na tyto otázky prostřednictvím analýzy clusteru.   

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Analýza clusteru klasifikuje sadu připomínky do dvou nebo více vzájemně se vylučuje neznámé skupin na základě kombinace proměnné. Účelem analýzy clusteru je zjistit systém uspořádání připomínky, obvykle osoby nebo vlastností jsou do skupiny, kde se členy skupin sdílí vlastnosti společné. To [služby](https://datamarket.azure.com/dataset/aml_labs/k_cluster_model) používá K-Means metodika, běžně používané clustering techniku, do clusteru libovolná data do skupin. Tato webová služba přijímá data a počet tisíc clusterů jako vstup a vytváří předpovědi, které k skupin, do kterých patří každý připomínky. 

> Tato webová služba může spotřebovat uživateli – potenciálně prostřednictvím mobilní aplikace prostřednictvím webu, nebo i v místním počítači, třeba. Ale účelem webové služby je také slouží jako příklad použití Azure Machine Learning k vytvoření webové služby v kódu jazyka R. Zadání několika řádků kódu jazyka R a klikne na tlačítko v rámci Azure Machine Learning Studio můžete experimentu vytvořené pomocí kódu jazyka R a publikovat jako webovou službu. Webové služby můžete pak publikování na webu Azure Marketplace a spotřebovávají uživatelů a zařízení po celém světě s žádné nastavení infrastruktury autorem webové služby.  
> 
> 

## <a name="consumption-of-web-service"></a>Spotřeba webové služby
Seskupení dat do sady skupin k této webové služby a výstupy přiřazení skupiny pro každý řádek. Webová služba očekává, že koncovému uživateli umožňují vstupní data jako řetězec, kde jsou řádky oddělených čárkami (,) a sloupce jsou odděleny středníkem (;). Webová služba očekává 1 řádek najednou. Datové sadě služby příkladu může vypadat například takto:

![Ukázková data][1]

Předpokládejme, že uživatel chtěli oddělení tato data do 3 vzájemně se vylučujících skupin. Data vstup výše datová sada může být následující: hodnota = "10, 5, 2,18; 1; 6,7; 5; 5,22; 3; 4,12; 2; 1,10; 3, 4"; k = "3". Výstup je členství ve skupině předpokládaných pro jednotlivé řádky.

> Tato služba jako hostované v Azure Marketplace je službou OData. To může být volána prostřednictvím metody POST nebo GET. 
> 
> 

Existuje více způsobů využívání služby automatizovaně (je třeba aplikace [sem](http://microsoftazuremachinelearning.azurewebsites.net/ClusterModel.aspx)).

### <a name="starting-c-code-for-web-service-consumption"></a>Spouštění kódu C# pro používání webové služby:
    public class Input
    {
            public string value;
            public string k;
    }

    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }

    void Main()
    {
            var input = new Input() { value = TextBox1.Text, k = TextBox2.Text };
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

Ze v rámci Azure Machine Learning, nový prázdný experiment byl vytvořen a dvě [spustit skript jazyka R] [ execute-r-script] moduly vyžádat na pracovním prostoru. Schéma dat byl vytvořen s jednoduchou [spustit skript jazyka R][execute-r-script]. Potom se schéma dat propojené s části model clusteru znovu vytvořit s [spustit skript jazyka R][execute-r-script]. V [spustit skript jazyka R] [ execute-r-script] použít pro model clusteru, webovou službu pak využívá funkce "k-means", který je předem do [spustit skript jazyka R] [ execute-r-script] z Azure Machine Learning.    

![Tok experimentu][3]

#### <a name="module-1"></a>Modul 1:
    #Enter the input data as a string 
    mydata <- data.frame(value = "1; 3; 5; 6; 7; 7, 5; 5; 6; 7; 2; 1, 3; 7; 2; 9; 56; 6, 1; 4; 5; 26; 4; 23, 15; 35; 6; 7; 12; 1, 32; 51; 62; 7; 21; 1", k=5, stringsAsFactors=FALSE)

    maml.mapOutputPort("mydata");     


#### <a name="module-2"></a>Modul 2:
    # Map 1-based optional input ports to variables
    mydata <- maml.mapInputPort(1) # class: data.frame

    data.split <- strsplit(mydata[1,1], ",")[[1]]
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)
    data.split <- as.data.frame(t(data.split))

    data.split <- data.matrix(data.split)
    data.split <- data.frame(data.split)

    # K-Means cluster analysis
    fit <- kmeans(data.split, mydata$k) # k-cluster solution

    # Get cluster means 
    aggregate(data.split,by=list(fit$cluster),FUN=mean)
    # Append cluster assignment
    mydatafinal <- data.frame(t(fit$cluster))
    n_col=ncol(mydatafinal)
    colnames(mydatafinal) <- paste("V",1:n_col,sep="")

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("mydatafinal");


## <a name="limitations"></a>Omezení
Toto je velmi jednoduchý příklad clustering webové služby. Jak je vidět z výše uvedeném příkladu kódu, je implementovaný zachycení žádné chyby a službu předpokládá, že všechno, co je nepřetržitá proměnné (bez kategorií funkcí povoleny), jako služba číselné hodnoty vstupy pouze v době vytvoření této webové služby. Navíc služba aktuálně zpracovává velikost omezenou dat, vyřízení požadavků a odpovědí povaze volání webové služby a fakt, že je probíhá podle modelu pokaždé, když je volat webovou službu. 

## <a name="faq"></a>Nejčastější dotazy
Nejčastější dotazy o spotřebě webové služby nebo publikování na webu Azure Marketplace naleznete v části [zde](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-cluster-model/cluster-img1.png
[2]: ./media/machine-learning-r-csharp-cluster-model/cluster-img2.png
[3]: ./media/machine-learning-r-csharp-cluster-model/cluster-img3.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/

