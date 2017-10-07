---
title: "AAA(Deprecated) Model clusteru – Azure | Microsoft Docs"
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
redirect_document_id: True
ms.openlocfilehash: 7b2dffb855a8d91114752b579115e97d07210e45
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deprecated-cluster-model"></a>(nepoužívané) Model clusteru

> [!NOTE]
> Hello Microsoft DataMarket se postupně vyřazuje z provozu a toto rozhraní API je zastaralá. 
> 
> Mnoho užitečné příklad experimentů a rozhraní API můžete najít v hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com). Další informace o hello Galerie najdete v tématu [sdílené složky a zjišťovat prostředky ve hello Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).

Jak jsme předpovědi skupiny platební služba chování v pořadí tooreduce hello poplatků vypnout riziko vystavitelů platební karty? Jak jsme v můžete definovat skupiny charakteru vlastnosti zaměstnanců pořadí tooimprove výkonu v práci? Jak můžete lékařů klasifikovat pacientů do skupin na základě charakteristik hello jejich nákaz? V zásadě všechny tyto otázky může odpovědět na tyto otázky prostřednictvím analýzy clusteru.   

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Analýza clusteru klasifikuje sadu připomínky do dvou nebo více vzájemně se vylučuje neznámé skupin na základě kombinace proměnné. Hello účelem analýzy clusteru je toodiscover systém uspořádání připomínky, obvykle osoby nebo vlastností jsou do skupiny, kde členů skupin hello sdílí vlastnosti společné. To [služby](https://datamarket.azure.com/dataset/aml_labs/k_cluster_model) používá hello K-Means metodika, běžně používané clustering techniku, toocluster libovolná data do skupin. Této webové služby trvá hello dat a hello počet tisíc clusterů jako vstup a vytváří předpovědi, které hello tisíc skupiny toowhich patří každý připomínky. 

> Tato webová služba může spotřebovat uživateli – potenciálně prostřednictvím mobilní aplikace prostřednictvím webu, nebo i v místním počítači, třeba. Ale hello účelem hello webové služby je také tooserve jako příklad, jak Azure Machine Learning lze použít toocreate webové služby v kódu jazyka R. Zadání několika řádků kódu jazyka R a klikne na tlačítko v rámci Azure Machine Learning Studio můžete experimentu vytvořené pomocí kódu jazyka R a publikovat jako webovou službu. Hello webové služby může být pak publikované toohello Azure Marketplace a využívat tak, že uživatelé a zařízení napříč hello, world s žádné nastavení infrastruktury autorem hello hello webové služby.  
> 
> 

## <a name="consumption-of-web-service"></a>Spotřeba webové služby
Tato webová služba skupiny hello dat do několika tisíc skupin a výstupy hello přiřazení skupiny pro každý řádek. Webová služba Hello očekává hello koncový uživatel tooinput data jako řetězec, kde jsou řádky oddělených čárkami (,) a sloupce jsou odděleny středníkem (;). Webová služba Hello očekává 1 řádek najednou. Datové sadě služby příkladu může vypadat například takto:

![Ukázková data][1]

Předpokládejme, že uživatel chtěli tooseparate hello tato data do 3 vzájemně se vylučujících skupin. Hello vstup hello výše datová sada by hello následující data: hodnota = "10, 5, 2,18; 1; 6,7; 5; 5,22; 3; 4,12; 2; 1,10; 3, 4"; k = "3". výstup Hello je hello členství ve skupině předpokládaných pro každý hello řádků.

> Tato služba jako hostované na hello Azure Marketplace, je službou OData. To může být volána prostřednictvím metody POST nebo GET. 
> 
> 

Existuje více způsobů využívání služby hello automatizovaně (je třeba aplikace [sem](http://microsoftazuremachinelearning.azurewebsites.net/ClusterModel.aspx)).

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
> Této webové služby byl vytvořen pomocí Azure Machine Learning. Pro bezplatnou zkušební verzi, jakož i úvodní videa na vytváření experimentů a [publikování webových služeb](machine-learning-publish-a-machine-learning-web-service.md), najdete v tématu [azure.com/ml](http://azure.com/ml). Níže je snímek obrazovky hello experimentu, který vytvořili hello webové služby a příklad kódu pro každou hello modulů v rámci experimentu hello.
> 
> 

Ze v rámci Azure Machine Learning, nový prázdný experiment byl vytvořen a dvě [spustit skript jazyka R] [ execute-r-script] moduly vyžádat do pracovního prostoru hello. schéma dat Hello byl vytvořen s jednoduchou [spustit skript jazyka R][execute-r-script]. Potom schéma dat hello byl odkazovaný toohello clusteru modelu části znovu vytvořit s [spustit skript jazyka R][execute-r-script]. V hello [spustit skript jazyka R] [ execute-r-script] používat pro model hello clusteru, hello webové služby pak využívá funkce hello "k-means", který je předem do hello [spustit skript jazyka R] [ execute-r-script] z Azure Machine Learning.    

![Tok experimentu][3]

#### <a name="module-1"></a>Modul 1:
    #Enter hello input data as a string 
    mydata <- data.frame(value = "1; 3; 5; 6; 7; 7, 5; 5; 6; 7; 2; 1, 3; 7; 2; 9; 56; 6, 1; 4; 5; 26; 4; 23, 15; 35; 6; 7; 12; 1, 32; 51; 62; 7; 21; 1", k=5, stringsAsFactors=FALSE)

    maml.mapOutputPort("mydata");     


#### <a name="module-2"></a>Modul 2:
    # Map 1-based optional input ports toovariables
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

    # Select data.frame toobe sent toohello output Dataset port
    maml.mapOutputPort("mydatafinal");


## <a name="limitations"></a>Omezení
Toto je velmi jednoduchý příklad clustering webové služby. Jak je vidět z hello ukázkový kód výše, je implementována zachycení žádné chyby a hello služby předpokládá, že všechno, co je nepřetržitá proměnné (bez kategorií funkcí povoleny), jako hello služby pouze vstupy číselných hodnot v době hello hello vytvoření tomuto webovému Služba. Navíc služba hello aktuálně zpracovává velikost omezenou dat, z důvodu toohello požadavků a odpovědí povaha hello webové služby je se volání a hello skutečnost, že hello modelu odpovídat pokaždé, když je volána hello webové služby. 

## <a name="faq"></a>Nejčastější dotazy
Nejčastější dotazy o spotřebě hello webové služby nebo publikování toohello Azure Marketplace naleznete v části [zde](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-cluster-model/cluster-img1.png
[2]: ./media/machine-learning-r-csharp-cluster-model/cluster-img2.png
[3]: ./media/machine-learning-r-csharp-cluster-model/cluster-img3.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/

