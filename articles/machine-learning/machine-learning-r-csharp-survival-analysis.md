---
title: "(nepoužívané) Analýza základními pomocí Azure Machine Learning | Microsoft Docs"
description: "(nepoužívané) Základními analýzy událostí výskyt pravděpodobnosti"
services: machine-learning
documentationcenter: 
author: zhangya
manager: jhubbard
editor: cgronlun
ms.assetid: a142fc45-cdfb-4971-910e-05dab8bc699e
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: zhangya
ROBOTS: NOINDEX
redirect_url: https://gallery.cortanaintelligence.com/
redirect_document_id: TRUE
ms.openlocfilehash: 7d4066d5f15a39c428d8035257c4841f9b3cc775
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deprecated-survival-analysis"></a>(nepoužívané) Základními analýzy

> [!NOTE]
> Microsoft DataMarket se postupně vyřazuje z provozu a toto rozhraní API je zastaralá. 
> 
> Můžete najít mnoho užitečné příklad experimentů a rozhraní API v [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com). Další informace o galerii najdete v tématu [sdílené složky a zjišťovat prostředky ve Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).

V části mnoho scénářů je hlavní výsledek pod assessment čas k události, které vás zajímají. Jinými slovy otázka "při této události dojde?" se zobrazí výzva. Jako příklady, zvažte situace, kdy data popisuje uplynulý čas (dny, let, vzdálenost atd.) až události týkající se (relapse nákazy, aplikaci PhotoDraw stupeň přijata, brzdy pad selhání) dojde k. Každá instance v datech představuje určitý objekt (pacienta student, car, atd.).

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

To [webovou službu](https://datamarket.azure.com/dataset/aml_labs/survivalanalysis) odpovědi na otázku "co je pravděpodobnost, že n čas pro objekt x dojde k události, které vás zajímají?" Poskytnutím model analysis základními této webové služby umožňuje uživatelům zadat data pro trénování modelu a otestovat ji. Hlavní téma experimentu je model délka uplynulý čas, dokud nedojde k události, které vás zajímají. 

> Tato webová služba může spotřebovat uživateli – potenciálně prostřednictvím mobilní aplikace prostřednictvím webu, nebo i v místním počítači, třeba. Ale účelem webové služby je také slouží jako příklad použití Azure Machine Learning k vytvoření webové služby v kódu jazyka R. Zadání několika řádků kódu jazyka R a klikne na tlačítko v rámci Azure Machine Learning Studio můžete experimentu vytvořené pomocí kódu jazyka R a publikovat jako webovou službu. Webové služby můžete pak publikování na webu Azure Marketplace a spotřebovávají uživatelů a zařízení po celém světě s žádné nastavení infrastruktury autorem webové služby.  
> 
> 

## <a name="consumption-of-web-service"></a>Spotřeba webové služby
Vstupní data schématu webové služby je uvedené v následující tabulce. Šest údaje jsou potřeba jako vstup: data cvičení, testování data, času zájmu, index "čas" dimenze, index "událost" dimenze a typy proměnných (nepřetržité nebo multi-Factor). Jsou Cvičná data je reprezentován řetězcem, kde čárkami oddělených řádky a sloupce, které jsou odděleny středníkem. Počet funkcí dat je flexibilní. Všechny elementy ve vstupním řetězci musí být číselná. V Cvičná data "časové" dimenze označuje, že počet jednotek dobu (dny, let, vzdálenost atd.) uplynulé od počáteční bod studie (pacienta přijetí nedovolenému zacházení programy, aplikaci PhotoDraw studie počáteční studenty, automobilu zahajuje se bude týkat, apod) až události týkající se (pacienta vrácením nedovolenému využití, student získat aplikaci PhotoDraw stupně, car s brzdy pad selhání atd.) nastane. Dimenze "událost" značí, zda na konci studie událostí, které vás zajímají. Hodnota "události = 1" znamená, že dojde k události týkající se v době uvedené v dimenzi "čas"; "události = 0" znamená, že v době uvedené v dimenzi "čas" nedošlo k události, které vás zajímají.

* trainingdata – řetězec znaků. Řádky jsou oddělené čárkou a sloupce jsou odděleny středníkem. Každý řádek obsahuje dimenze "čas", "událost" dimenze a předpověď proměnné.
* testingdata – jeden řádek dat, která obsahuje předpověď proměnných pro určitý objekt.
* time_of_interest - uplynulá doba n vás zajímají.
* index_time – index sloupce dimenze "čas" (počínaje 1).
* index_event – index sloupce dimenze "událost" (počínaje 1).
* variable_types – řetězec znaků se středníky jako oddělovače v ní. Hodnota 0 představuje průběžné proměnné a 1 představuje Multi-Factor proměnné.

Výstup je pravděpodobnost událost, ke kterým dochází ve určitý čas. 

> Tato služba jako hostované v Azure Marketplace je službou OData. To může být volána prostřednictvím metody POST nebo GET. 
> 
> 

Existuje více způsobů využívání služby automatizovaně (je třeba aplikace [sem](http://microsoftazuremachinelearning.azurewebsites.net/SurvivalAnalysis.aspx)). 

### <a name="starting-c-code-for-web-service-consumption"></a>Spouštění kódu C# pro používání webové služby:
    public class Input
    {
            public string trainingdata;
            public string testingdata;
            public string timeofinterest;
            public string indextime;
            public string indexevent;
            public string variabletypes;
    }

    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }

    void Main()
    {
            var input = new Input() { trainingdata = TextBox1.Text, testingdata = TextBox2.Text, timeofinterest = TextBox3.Text, indextime = TextBox4.Text, indexevent = TextBox5.Text, variabletypes = TextBox6.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();

            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));

            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }




Výklad tento test je následující. Za předpokladu, že cílem dat je modelu zabralo až do návratu do obchodu využití pro pacientů, kteří obdrželi některý z programů dvě zpracování. Výstup čtení webové služby: pacientů se 35 let, s předchozí obchodu zacházení 2 dobu, trvá dlouho domácí zacházení programu, a s možností využití heroinu i kokainu pravděpodobnost vrácením využití nedovolenému 95.64 % od den 500.

## <a name="creation-of-web-service"></a>Vytvoření webové služby
> Této webové služby byl vytvořen pomocí Azure Machine Learning. Pro bezplatnou zkušební verzi, jakož i úvodní videa na vytváření experimentů a [publikování webových služeb](machine-learning-publish-a-machine-learning-web-service.md), najdete v tématu [azure.com/ml](http://azure.com/ml). Níže je snímek obrazovky experimentu, který vytvořené pro každou z modulů v rámci experimentu webové služby a příklad kódu.
> 
> 

Ze v rámci Azure Machine Learning, nový prázdný experiment byl vytvořen a dvě [spustit skript jazyka R] [ execute-r-script] moduly byly vyžádány na pracovním prostoru. Schéma dat byl vytvořen s jednoduchou [spustit skript jazyka R][execute-r-script], který definuje schéma vstupní data pro webovou službu. Tento modul se pak propojí druhý [spustit skript jazyka R] [ execute-r-script] modul, kterým hlavní pracovní. Tento modul nemá předzpracování dat, vytváření modelů a předpovědi. V kroku předběžného zpracování dat vstupní data reprezentována dlouhý řetězec transformovat a převést do rámečku data. V kroku sestavení modelu externí balíček R "survival_2.37-7.zip" první instalaci pro provádění základními analýzy. Pak funkce "coxph" se spustí až po úloh zpracování dat řady. Podrobnosti o "coxph" funkce pro analýzu základními můžete přečíst v dokumentaci k R. V kroku předpovědi do pro cvičný model pomocí funkce "surfit" je zadána testování instance a základními křivky pro tuto instanci testování vytváří jako proměnnou "křivky". Nakonec se získávají pravděpodobnost času, které vás zajímají. 

### <a name="experiment-flow"></a>Experiment toku:
![Tok experimentu][1]

#### <a name="module-1"></a>Modul 1:
    #Data schema with example data (replaced with data from web service)
    trainingdata="53;1;29;0;0;3,79;1;34;0;1;2,45;1;27;0;1;1,37;1;24;0;1;1,122;1;30;0;1;1,655;0;41;0;0;1,166;1;30;0;0;3,227;1;29;0;0;3,805;0;30;0;0;1,104;1;24;0;0;1,90;1;32;0;0;1,373;1;26;0;0;1,70;1;36;0;0;1”
    testingdata="35;2;1;1"
    time_of_interest="500"
    index_time="1"
    index_event="2"

    # 0 - continuous; 1 -  factor
    variable_types="0;0;1;1"

    sampleInput=data.frame(trainingdata,testingdata,time_of_interest,index_time,index_event,variable_types)

    maml.mapOutputPort("sampleInput"); #send data to output port

#### <a name="module-2"></a>Modul 2:
    #Read data from input port
    data <- maml.mapInputPort(1) 
    colnames(data) <- c("trainingdata","testingdata","time_of_interest","index_time","index_event","variable_types")

    # Preprocessing training data
    traindingdata=data$trainingdata
    y=strsplit(as.character(data$trainingdata),",")
    n_row=length(unlist(y))
    z=sapply(unlist(y), strsplit, ";", simplify = TRUE)
    mydata <- data.frame(matrix(unlist(z), nrow=n_row, byrow=T), stringsAsFactors=FALSE)
    n_col=ncol(mydata)

    # Preprocessing testing data
    testingdata=as.character(data$testingdata)
    testingdata=unlist(strsplit(testingdata,";"))

    # Preprocessing other input parameters
    time_of_interest=data$time_of_interest
    time_of_interest=as.numeric(as.character(time_of_interest))
    index_time = data$index_time
    index_event = data$index_event
    variable_types = data$variable_types

    # Necessary R packages
    install.packages("src/packages_survival/survival_2.37-7.zip",lib=".",repos=NULL,verbose=TRUE)
    library(survival)

    # Prepare to build model
    attach(mydata)

    for (i in 1:n_col){ mydata[,i]=as.numeric(mydata[,i])} 
    d_time=paste("X",index_time,sep = "")
    d_event=paste("X",index_event,sep = "")
    v_time_event <- c(d_time,d_event)
    v_predictors = names(mydata)[!(names(mydata) %in% v_time_event)]

    variable_types = unlist(strsplit(as.character(variable_types),";"))

    len = length(v_predictors)
    c="" # Construct the execution string
    for (i in 1:len){
    if(i==len){
    if(variable_types[i]!=0){ c=paste(c, "factor(",v_predictors[i],")",sep="")}
     else{ c=paste(c, v_predictors[i])}
    }else{
    if(variable_types[i]!=0){c=paste(c, "factor(",v_predictors[i],") + ",sep="")}
    else{c=paste(c, v_predictors[i],"+")}
    }
    }
    f=paste("coxph(Surv(",d_time,",",d_event,") ~")
    f=paste(f,c)
    f=paste(f,", data=mydata )")

    # Fit a Cox proportional hazards model and get the predicted survival curve for a testing instance 
    fit=eval(parse(text=f))

    testingdata = as.data.frame(matrix(testingdata, ncol=len,byrow = TRUE),stringsAsFactors=FALSE)
    names(testingdata)=v_predictors
    for (i in 1:len){ testingdata[,i]=as.numeric(testingdata[,i])}

    curve=survfit(fit,testingdata)

    # Based on user input, find the event occurrence probability
    position_closest=which.min(abs(prob_event$time - time_of_interest))

    if(prob_event[position_closest,"time"]==time_of_interest){# exact match
    output=prob_event[position_closest,"prob"]
    }else{# not exact match
    if(time_of_interest>max(prob_event$time)){
    output=prob_event[position_closest,"prob"]
    }else if(time_of_interest<min(prob_event$time)){
    output=prob_event[position_closest,"prob"]
    }else{output=(prob_event[position_closest,"prob"]+prob_event[position_closest+1,"prob"])/2}
    }

    #Pull out results to send to web service
    output=paste(round(100*output, 2), "%") 
    maml.mapOutputPort("output"); #output port




## <a name="limitations"></a>Omezení
Této webové služby může trvat pouze číselné hodnoty jako funkce proměnné (sloupce). Sloupec "události" může trvat pouze hodnota 0 nebo 1. Sloupec "čas" musí být kladné celé číslo.

## <a name="faq"></a>Nejčastější dotazy
Nejčastější dotazy o spotřebě webové služby nebo publikování na webu Azure Marketplace naleznete v části [zde](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-survival-analysis/survive_img2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/

