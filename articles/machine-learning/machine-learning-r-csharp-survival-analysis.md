---
title: "AAA(Deprecated) základními Analysis pomocí Azure Machine Learning | Microsoft Docs"
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
redirect_document_id: True
ms.openlocfilehash: af946d8df5ba650a9d74fbabbe3b15d3a07dd508
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deprecated-survival-analysis"></a>(nepoužívané) Základními analýzy

> [!NOTE]
> Hello Microsoft DataMarket se postupně vyřazuje z provozu a toto rozhraní API je zastaralá. 
> 
> Mnoho užitečné příklad experimentů a rozhraní API můžete najít v hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com). Další informace o hello Galerie najdete v tématu [sdílené složky a zjišťovat prostředky ve hello Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).

V části mnoho scénářů je hlavní výsledek hello pod assessment hello čas tooan událost zájmu. Jinými slovy hello otázku "při této události dojde?" se zobrazí výzva. Jako příklady, zvažte situacích, kde hello data popisuje hello uplynulý čas (dny, let, vzdálenost atd.), dokud hello dojde k události, které vás zajímají (relapse nákazy, aplikaci PhotoDraw stupeň přijata, brzdy pad selhání). Každá instance ve hello dat představuje určitý objekt (pacienta student, car, atd.).

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

To [webovou službu](https://datamarket.azure.com/dataset/aml_labs/survivalanalysis) odpovědi hello otázku "co je hello pravděpodobnost, že hello události týkající se provádí n čas pro objekt x?" Poskytnutím model analysis základními této webové služby umožňuje uživatelům toosupply data tootrain hello modelu a otestovat. Hlavní téma Hello hello experimentu je délka hello toomodel hello uplynulého času, dokud nedojde k hello událostí, které vás zajímají. 

> Tato webová služba může spotřebovat uživateli – potenciálně prostřednictvím mobilní aplikace prostřednictvím webu, nebo i v místním počítači, třeba. Ale hello účelem hello webové služby je také tooserve jako příklad, jak Azure Machine Learning lze použít toocreate webové služby v kódu jazyka R. Zadání několika řádků kódu jazyka R a klikne na tlačítko v rámci Azure Machine Learning Studio můžete experimentu vytvořené pomocí kódu jazyka R a publikovat jako webovou službu. Hello webové služby může být pak publikované toohello Azure Marketplace a využívat tak, že uživatelé a zařízení napříč hello, world s žádné nastavení infrastruktury autorem hello hello webové služby.  
> 
> 

## <a name="consumption-of-web-service"></a>Spotřeba webové služby
vstupní data schématu Hello hello webové služby se zobrazí v hello následující tabulka. Šest údaje jsou potřeba jako vstup hello: Cvičná data, testování data, času, které vás zajímají, index hello "časové" dimenze, hello index "událost" dimenze a typy proměnných hello (souvislé nebo multi-Factor). Hello Cvičná data je reprezentován řetězcem, kde hello řádky jsou oddělené čárkou a hello sloupce jsou odděleny středníkem. Hello řadu funkcí hello dat je flexibilní. Všechny elementy hello ve vstupním řetězci hello musí být číselná hodnota. V hello Cvičná data dimenze času"hello" označuje, že hello počet jednotek (dny, let, vzdálenost atd.), které uplynuly od hello výchozí bod hello prostudovali (pacienta přijetí nedovolenému zacházení programy, aplikaci PhotoDraw studie počáteční studenty, automobilu od toobe řízené, atd.) dokud hello zájmu (hello pacienta vrácení toodrug využití, hello student získání hello aplikaci PhotoDraw stupně, car hello s selhání brzdy pad, atd.) dojde k události. dimenze "událost" Hello značí, zda na konci hello hello studie hello událostí, které vás zajímají. Hodnota "události = 1" znamená, že hello událostí, které vás zajímají nastane v čase hello indikován dimenze "čas" hello; "události = 0" znamená, že hello událostí, které vás zajímají nedošlo k hello doby indikován hello "čas" dimenze.

* trainingdata – řetězec znaků. Řádky jsou oddělené čárkou a sloupce jsou odděleny středníkem. Každý řádek obsahuje dimenze "čas", "událost" dimenze a předpověď proměnné.
* testingdata – jeden řádek dat, která obsahuje předpověď proměnných pro určitý objekt.
* time_of_interest - hello uplynulá doba n vás zajímají.
* index_time – index sloupce hello dimenze "čas" hello (počínaje 1).
* index_event – index sloupce hello dimenze "událost" hello (počínaje 1).
* variable_types – řetězec znaků se středníky jako oddělovače v ní. Hodnota 0 představuje průběžné proměnné a 1 představuje Multi-Factor proměnné.

výstup Hello je hello pravděpodobnost událost, ke kterým dochází ve určitý čas. 

> Tato služba jako hostované na hello Azure Marketplace, je službou OData. To může být volána prostřednictvím metody POST nebo GET. 
> 
> 

Existuje více způsobů využívání služby hello automatizovaně (je třeba aplikace [sem](http://microsoftazuremachinelearning.azurewebsites.net/SurvivalAnalysis.aspx)). 

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




Hello výklad tento test je následující. Za předpokladu, že cílem hello hello dat je toomodel hello uplynulý čas dokud hello vrátí toodrug využití pro hello pacientů, kteří obdrželi jeden hello dva zacházení programů. Hello výstup hello webové služby čtení: pacientů se 35 let, s předchozí obchodu zacházení 2 dobu, trvá dlouho domácí zacházení programu hello, a s možností využití heroinu i kokainu hello pravděpodobnost vrácení toohello nedovolenému využití je 95.64 % od den 500.

## <a name="creation-of-web-service"></a>Vytvoření webové služby
> Této webové služby byl vytvořen pomocí Azure Machine Learning. Pro bezplatnou zkušební verzi, jakož i úvodní videa na vytváření experimentů a [publikování webových služeb](machine-learning-publish-a-machine-learning-web-service.md), najdete v tématu [azure.com/ml](http://azure.com/ml). Níže je snímek obrazovky hello experimentu, který vytvořili hello webové služby a příklad kódu pro každou hello modulů v rámci experimentu hello.
> 
> 

Ze v rámci Azure Machine Learning, nový prázdný experiment byl vytvořen a dvě [spustit skript jazyka R] [ execute-r-script] moduly byly vyžádány do pracovního prostoru hello. Hello schéma dat byl vytvořen s jednoduchou [spustit skript jazyka R][execute-r-script], která definuje schéma hello vstupní data pro hello webové služby. Tento modul je pak druhý propojené toohello [spustit skript jazyka R] [ execute-r-script] modul, kterým hlavní pracovní. Tento modul nemá předzpracování dat, vytváření modelů a předpovědi. V kroku předběžného zpracování dat hello hello vstupní data reprezentována dlouhý řetězec transformovat a převést do rámečku data. V kroku sestavení modelu hello externí balíček R "survival_2.37-7.zip" první instalaci pro provádění základními analýzy. Funkce "coxph" hello se pak spustí až po úloh zpracování dat řady. Podrobnosti o Hello hello "coxph" funkce pro analýzu základními může číst z hello R dokumentaci. V kroku předpovědi hello do hello trained model, který obsahuje funkci "surfit" hello je zadána testování instance a hello základními křivky pro tuto instanci testování vytváří jako proměnnou "křivky". Nakonec se získávají hello pravděpodobnosti hello času, které vás zajímají. 

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

    maml.mapOutputPort("sampleInput"); #send data toooutput port

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

    # Prepare toobuild model
    attach(mydata)

    for (i in 1:n_col){ mydata[,i]=as.numeric(mydata[,i])} 
    d_time=paste("X",index_time,sep = "")
    d_event=paste("X",index_event,sep = "")
    v_time_event <- c(d_time,d_event)
    v_predictors = names(mydata)[!(names(mydata) %in% v_time_event)]

    variable_types = unlist(strsplit(as.character(variable_types),";"))

    len = length(v_predictors)
    c="" # Construct hello execution string
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

    # Fit a Cox proportional hazards model and get hello predicted survival curve for a testing instance 
    fit=eval(parse(text=f))

    testingdata = as.data.frame(matrix(testingdata, ncol=len,byrow = TRUE),stringsAsFactors=FALSE)
    names(testingdata)=v_predictors
    for (i in 1:len){ testingdata[,i]=as.numeric(testingdata[,i])}

    curve=survfit(fit,testingdata)

    # Based on user input, find hello event occurrence probability
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

    #Pull out results toosend tooweb service
    output=paste(round(100*output, 2), "%") 
    maml.mapOutputPort("output"); #output port




## <a name="limitations"></a>Omezení
Této webové služby může trvat pouze číselné hodnoty jako funkce proměnné (sloupce). sloupec "události" Hello, může trvat pouze hodnota 0 nebo 1. sloupec "čas" Hello musí toobe kladné celé číslo.

## <a name="faq"></a>Nejčastější dotazy
Nejčastější dotazy o spotřebě hello webové služby nebo publikování toohello Azure Marketplace naleznete v části [zde](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-survival-analysis/survive_img2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/

