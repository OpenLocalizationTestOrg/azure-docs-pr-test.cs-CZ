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
# <a name="deprecated-survival-analysis"></a><span data-ttu-id="4f5e6-103">(nepoužívané) Základními analýzy</span><span class="sxs-lookup"><span data-stu-id="4f5e6-103">(deprecated) Survival Analysis</span></span>

> [!NOTE]
> <span data-ttu-id="4f5e6-104">Microsoft DataMarket se postupně vyřazuje z provozu a toto rozhraní API je zastaralá.</span><span class="sxs-lookup"><span data-stu-id="4f5e6-104">The Microsoft DataMarket is being retired and this API has been deprecated.</span></span> 
> 
> <span data-ttu-id="4f5e6-105">Můžete najít mnoho užitečné příklad experimentů a rozhraní API v [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="4f5e6-105">You can find many useful example experiments and APIs in the [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="4f5e6-106">Další informace o galerii najdete v tématu [sdílené složky a zjišťovat prostředky ve Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span><span class="sxs-lookup"><span data-stu-id="4f5e6-106">For more information about the Gallery, see [Share and discover resources in the Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span></span>

<span data-ttu-id="4f5e6-107">V části mnoho scénářů je hlavní výsledek pod assessment čas k události, které vás zajímají.</span><span class="sxs-lookup"><span data-stu-id="4f5e6-107">Under many scenarios, the main outcome under assessment is the time to an event of interest.</span></span> <span data-ttu-id="4f5e6-108">Jinými slovy otázka "při této události dojde?"</span><span class="sxs-lookup"><span data-stu-id="4f5e6-108">In other words, the question “when this event will occur?”</span></span> <span data-ttu-id="4f5e6-109">se zobrazí výzva.</span><span class="sxs-lookup"><span data-stu-id="4f5e6-109">is asked.</span></span> <span data-ttu-id="4f5e6-110">Jako příklady, zvažte situace, kdy data popisuje uplynulý čas (dny, let, vzdálenost atd.) až události týkající se (relapse nákazy, aplikaci PhotoDraw stupeň přijata, brzdy pad selhání) dojde k.</span><span class="sxs-lookup"><span data-stu-id="4f5e6-110">As examples, consider situations where the data describes the elapsed time (days, years, mileage, etc.) until the event of interest (disease relapse, PhD degree received, brake pad failure) occurs.</span></span> <span data-ttu-id="4f5e6-111">Každá instance v datech představuje určitý objekt (pacienta student, car, atd.).</span><span class="sxs-lookup"><span data-stu-id="4f5e6-111">Each instance in the data represents a specific object (a patient, a student, a car, etc.).</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

<span data-ttu-id="4f5e6-112">To [webovou službu](https://datamarket.azure.com/dataset/aml_labs/survivalanalysis) odpovědi na otázku "co je pravděpodobnost, že n čas pro objekt x dojde k události, které vás zajímají?"</span><span class="sxs-lookup"><span data-stu-id="4f5e6-112">This [web service](https://datamarket.azure.com/dataset/aml_labs/survivalanalysis) answers the question “what is the probability that the event of interest will occur by time n for object x?”</span></span> <span data-ttu-id="4f5e6-113">Poskytnutím model analysis základními této webové služby umožňuje uživatelům zadat data pro trénování modelu a otestovat ji.</span><span class="sxs-lookup"><span data-stu-id="4f5e6-113">By providing a survival analysis model, this web service enables users to supply data to train the model and test it.</span></span> <span data-ttu-id="4f5e6-114">Hlavní téma experimentu je model délka uplynulý čas, dokud nedojde k události, které vás zajímají.</span><span class="sxs-lookup"><span data-stu-id="4f5e6-114">The main theme of the experiment is to model the length of the elapsed time until the event of interest occurs.</span></span> 

> <span data-ttu-id="4f5e6-115">Tato webová služba může spotřebovat uživateli – potenciálně prostřednictvím mobilní aplikace prostřednictvím webu, nebo i v místním počítači, třeba.</span><span class="sxs-lookup"><span data-stu-id="4f5e6-115">This web service could be consumed by users – potentially through a mobile app, through a website, or even on a local computer, for example.</span></span> <span data-ttu-id="4f5e6-116">Ale účelem webové služby je také slouží jako příklad použití Azure Machine Learning k vytvoření webové služby v kódu jazyka R.</span><span class="sxs-lookup"><span data-stu-id="4f5e6-116">But the purpose of the web service is also to serve as an example of how Azure Machine Learning can be used to create web services on top of R code.</span></span> <span data-ttu-id="4f5e6-117">Zadání několika řádků kódu jazyka R a klikne na tlačítko v rámci Azure Machine Learning Studio můžete experimentu vytvořené pomocí kódu jazyka R a publikovat jako webovou službu.</span><span class="sxs-lookup"><span data-stu-id="4f5e6-117">With just a few lines of R code and clicks of a button within Azure Machine Learning Studio, an experiment can be created with R code and published as a web service.</span></span> <span data-ttu-id="4f5e6-118">Webové služby můžete pak publikování na webu Azure Marketplace a spotřebovávají uživatelů a zařízení po celém světě s žádné nastavení infrastruktury autorem webové služby.</span><span class="sxs-lookup"><span data-stu-id="4f5e6-118">The web service can then be published to the Azure Marketplace and consumed by users and devices across the world with no infrastructure setup by the author of the web service.</span></span>  
> 
> 

## <a name="consumption-of-web-service"></a><span data-ttu-id="4f5e6-119">Spotřeba webové služby</span><span class="sxs-lookup"><span data-stu-id="4f5e6-119">Consumption of web service</span></span>
<span data-ttu-id="4f5e6-120">Vstupní data schématu webové služby je uvedené v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="4f5e6-120">The input data schema of the web service is shown in the following table.</span></span> <span data-ttu-id="4f5e6-121">Šest údaje jsou potřeba jako vstup: data cvičení, testování data, času zájmu, index "čas" dimenze, index "událost" dimenze a typy proměnných (nepřetržité nebo multi-Factor).</span><span class="sxs-lookup"><span data-stu-id="4f5e6-121">Six pieces of information are needed as the input: training data, testing data, time of interest, the index of "time" dimension, the index of "event" dimension, and the variable types (continuous or factor).</span></span> <span data-ttu-id="4f5e6-122">Jsou Cvičná data je reprezentován řetězcem, kde čárkami oddělených řádky a sloupce, které jsou odděleny středníkem.</span><span class="sxs-lookup"><span data-stu-id="4f5e6-122">The training data is represented with a string, where the rows are separated by comma, and the columns are separated by semicolon.</span></span> <span data-ttu-id="4f5e6-123">Počet funkcí dat je flexibilní.</span><span class="sxs-lookup"><span data-stu-id="4f5e6-123">The number of features of the data is flexible.</span></span> <span data-ttu-id="4f5e6-124">Všechny elementy ve vstupním řetězci musí být číselná.</span><span class="sxs-lookup"><span data-stu-id="4f5e6-124">All the elements in the input string must be numeric.</span></span> <span data-ttu-id="4f5e6-125">V Cvičná data "časové" dimenze označuje, že počet jednotek dobu (dny, let, vzdálenost atd.) uplynulé od počáteční bod studie (pacienta přijetí nedovolenému zacházení programy, aplikaci PhotoDraw studie počáteční studenty, automobilu zahajuje se bude týkat, apod) až události týkající se (pacienta vrácením nedovolenému využití, student získat aplikaci PhotoDraw stupně, car s brzdy pad selhání atd.) nastane.</span><span class="sxs-lookup"><span data-stu-id="4f5e6-125">In the training data, the “time” dimension indicates the number of time units (days, years, mileage, etc.) elapsed since the starting point of the study (a patient receiving drug treatment programs, a student starting PhD study, a car starting to be driven, etc.) until the event of interest (the patient returning to drug usage, the student obtaining the PhD degree, the car having brake pad failure, etc.) occurs.</span></span> <span data-ttu-id="4f5e6-126">Dimenze "událost" značí, zda na konci studie událostí, které vás zajímají.</span><span class="sxs-lookup"><span data-stu-id="4f5e6-126">The “event” dimension indicates whether the event of interest occurs at the end of the study.</span></span> <span data-ttu-id="4f5e6-127">Hodnota "události = 1" znamená, že dojde k události týkající se v době uvedené v dimenzi "čas"; "události = 0" znamená, že v době uvedené v dimenzi "čas" nedošlo k události, které vás zajímají.</span><span class="sxs-lookup"><span data-stu-id="4f5e6-127">A value of “event=1” means that the event of interest occurs at the time indicated by the “time” dimension; “event=0” means that the event of interest has not occurred by the time indicated by the “time” dimension.</span></span>

* <span data-ttu-id="4f5e6-128">trainingdata – řetězec znaků.</span><span class="sxs-lookup"><span data-stu-id="4f5e6-128">trainingdata - A character string.</span></span> <span data-ttu-id="4f5e6-129">Řádky jsou oddělené čárkou a sloupce jsou odděleny středníkem.</span><span class="sxs-lookup"><span data-stu-id="4f5e6-129">Rows are separated by comma, and columns are separated by semicolon.</span></span> <span data-ttu-id="4f5e6-130">Každý řádek obsahuje dimenze "čas", "událost" dimenze a předpověď proměnné.</span><span class="sxs-lookup"><span data-stu-id="4f5e6-130">Each row includes “time” dimension, “event” dimension, and predictor variables.</span></span>
* <span data-ttu-id="4f5e6-131">testingdata – jeden řádek dat, která obsahuje předpověď proměnných pro určitý objekt.</span><span class="sxs-lookup"><span data-stu-id="4f5e6-131">testingdata - One row of data that contains predictor variables for a particular object.</span></span>
* <span data-ttu-id="4f5e6-132">time_of_interest - uplynulá doba n vás zajímají.</span><span class="sxs-lookup"><span data-stu-id="4f5e6-132">time_of_interest - The elapsed time of interest n.</span></span>
* <span data-ttu-id="4f5e6-133">index_time – index sloupce dimenze "čas" (počínaje 1).</span><span class="sxs-lookup"><span data-stu-id="4f5e6-133">index_time - The column index of the “time” dimension (starting from 1).</span></span>
* <span data-ttu-id="4f5e6-134">index_event – index sloupce dimenze "událost" (počínaje 1).</span><span class="sxs-lookup"><span data-stu-id="4f5e6-134">index_event - The column index of the “event” dimension (starting from 1).</span></span>
* <span data-ttu-id="4f5e6-135">variable_types – řetězec znaků se středníky jako oddělovače v ní.</span><span class="sxs-lookup"><span data-stu-id="4f5e6-135">variable_types - A character string with semicolons as separators in it.</span></span> <span data-ttu-id="4f5e6-136">Hodnota 0 představuje průběžné proměnné a 1 představuje Multi-Factor proměnné.</span><span class="sxs-lookup"><span data-stu-id="4f5e6-136">0 represents continuous variables and 1 represents factor variables.</span></span>

<span data-ttu-id="4f5e6-137">Výstup je pravděpodobnost událost, ke kterým dochází ve určitý čas.</span><span class="sxs-lookup"><span data-stu-id="4f5e6-137">The output is the probability of an event occurring by a specific time.</span></span> 

> <span data-ttu-id="4f5e6-138">Tato služba jako hostované v Azure Marketplace je službou OData. To může být volána prostřednictvím metody POST nebo GET.</span><span class="sxs-lookup"><span data-stu-id="4f5e6-138">This service, as hosted on the Azure Marketplace, is an OData service; these may be called through POST or GET methods.</span></span> 
> 
> 

<span data-ttu-id="4f5e6-139">Existuje více způsobů využívání služby automatizovaně (je třeba aplikace [sem](http://microsoftazuremachinelearning.azurewebsites.net/SurvivalAnalysis.aspx)).</span><span class="sxs-lookup"><span data-stu-id="4f5e6-139">There are multiple ways of consuming the service in an automated fashion (an example app is [here](http://microsoftazuremachinelearning.azurewebsites.net/SurvivalAnalysis.aspx)).</span></span> 

### <a name="starting-c-code-for-web-service-consumption"></a><span data-ttu-id="4f5e6-140">Spouštění kódu C# pro používání webové služby:</span><span class="sxs-lookup"><span data-stu-id="4f5e6-140">Starting C# code for web service consumption:</span></span>
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




<span data-ttu-id="4f5e6-141">Výklad tento test je následující.</span><span class="sxs-lookup"><span data-stu-id="4f5e6-141">The interpretation of this test is as follows.</span></span> <span data-ttu-id="4f5e6-142">Za předpokladu, že cílem dat je modelu zabralo až do návratu do obchodu využití pro pacientů, kteří obdrželi některý z programů dvě zpracování.</span><span class="sxs-lookup"><span data-stu-id="4f5e6-142">Assuming the goal of the data is to model the elapsed time until the return to drug usage for the patients who received one of the two treatment programs.</span></span> <span data-ttu-id="4f5e6-143">Výstup čtení webové služby: pacientů se 35 let, s předchozí obchodu zacházení 2 dobu, trvá dlouho domácí zacházení programu, a s možností využití heroinu i kokainu pravděpodobnost vrácením využití nedovolenému 95.64 % od den 500.</span><span class="sxs-lookup"><span data-stu-id="4f5e6-143">The output of the web service reads: for patients being 35 years old, having previous drug treatment 2 times, taking the long residential treatment program, and with both heroin and cocaine usage, the probability of returning to the drug usage is 95.64% by day 500.</span></span>

## <a name="creation-of-web-service"></a><span data-ttu-id="4f5e6-144">Vytvoření webové služby</span><span class="sxs-lookup"><span data-stu-id="4f5e6-144">Creation of web service</span></span>
> <span data-ttu-id="4f5e6-145">Této webové služby byl vytvořen pomocí Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="4f5e6-145">This web service was created using Azure Machine Learning.</span></span> <span data-ttu-id="4f5e6-146">Pro bezplatnou zkušební verzi, jakož i úvodní videa na vytváření experimentů a [publikování webových služeb](machine-learning-publish-a-machine-learning-web-service.md), najdete v tématu [azure.com/ml](http://azure.com/ml).</span><span class="sxs-lookup"><span data-stu-id="4f5e6-146">For a free trial, as well as introductory videos on creating experiments and [publishing web services](machine-learning-publish-a-machine-learning-web-service.md), please see [azure.com/ml](http://azure.com/ml).</span></span> <span data-ttu-id="4f5e6-147">Níže je snímek obrazovky experimentu, který vytvořené pro každou z modulů v rámci experimentu webové služby a příklad kódu.</span><span class="sxs-lookup"><span data-stu-id="4f5e6-147">Below is a screenshot of the experiment that created the web service and example code for each of the modules within the experiment.</span></span>
> 
> 

<span data-ttu-id="4f5e6-148">Ze v rámci Azure Machine Learning, nový prázdný experiment byl vytvořen a dvě [spustit skript jazyka R] [ execute-r-script] moduly byly vyžádány na pracovním prostoru.</span><span class="sxs-lookup"><span data-stu-id="4f5e6-148">From within Azure Machine Learning, a new blank experiment was created and two [Execute R Script][execute-r-script] modules were pulled onto the workspace.</span></span> <span data-ttu-id="4f5e6-149">Schéma dat byl vytvořen s jednoduchou [spustit skript jazyka R][execute-r-script], který definuje schéma vstupní data pro webovou službu.</span><span class="sxs-lookup"><span data-stu-id="4f5e6-149">The data schema was created with a simple [Execute R Script][execute-r-script], which defines the input data schema for the web service.</span></span> <span data-ttu-id="4f5e6-150">Tento modul se pak propojí druhý [spustit skript jazyka R] [ execute-r-script] modul, kterým hlavní pracovní.</span><span class="sxs-lookup"><span data-stu-id="4f5e6-150">This module is then linked to the second [Execute R Script][execute-r-script] module, which does major work.</span></span> <span data-ttu-id="4f5e6-151">Tento modul nemá předzpracování dat, vytváření modelů a předpovědi.</span><span class="sxs-lookup"><span data-stu-id="4f5e6-151">This module does data preprocessing, model building, and predictions.</span></span> <span data-ttu-id="4f5e6-152">V kroku předběžného zpracování dat vstupní data reprezentována dlouhý řetězec transformovat a převést do rámečku data.</span><span class="sxs-lookup"><span data-stu-id="4f5e6-152">In the data preprocessing step, the input data represented by a long string is transformed and converted into a data frame.</span></span> <span data-ttu-id="4f5e6-153">V kroku sestavení modelu externí balíček R "survival_2.37-7.zip" první instalaci pro provádění základními analýzy.</span><span class="sxs-lookup"><span data-stu-id="4f5e6-153">In the model building step, an external R package “survival_2.37-7.zip” is first installed for conducting survival analysis.</span></span> <span data-ttu-id="4f5e6-154">Pak funkce "coxph" se spustí až po úloh zpracování dat řady.</span><span class="sxs-lookup"><span data-stu-id="4f5e6-154">Then the “coxph” function is executed after a series data processing tasks.</span></span> <span data-ttu-id="4f5e6-155">Podrobnosti o "coxph" funkce pro analýzu základními můžete přečíst v dokumentaci k R.</span><span class="sxs-lookup"><span data-stu-id="4f5e6-155">The details of the “coxph” function for survival analysis can be read from the R documentation.</span></span> <span data-ttu-id="4f5e6-156">V kroku předpovědi do pro cvičný model pomocí funkce "surfit" je zadána testování instance a základními křivky pro tuto instanci testování vytváří jako proměnnou "křivky".</span><span class="sxs-lookup"><span data-stu-id="4f5e6-156">In the prediction step, a testing instance is supplied into the trained model with the “surfit” function, and the survival curve for this testing instance is produced as “curve” variable.</span></span> <span data-ttu-id="4f5e6-157">Nakonec se získávají pravděpodobnost času, které vás zajímají.</span><span class="sxs-lookup"><span data-stu-id="4f5e6-157">Finally, the probability of the time of interest is obtained.</span></span> 

### <a name="experiment-flow"></a><span data-ttu-id="4f5e6-158">Experiment toku:</span><span class="sxs-lookup"><span data-stu-id="4f5e6-158">Experiment flow:</span></span>
![Tok experimentu][1]

#### <a name="module-1"></a><span data-ttu-id="4f5e6-160">Modul 1:</span><span class="sxs-lookup"><span data-stu-id="4f5e6-160">Module 1:</span></span>
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

#### <a name="module-2"></a><span data-ttu-id="4f5e6-161">Modul 2:</span><span class="sxs-lookup"><span data-stu-id="4f5e6-161">Module 2:</span></span>
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




## <a name="limitations"></a><span data-ttu-id="4f5e6-162">Omezení</span><span class="sxs-lookup"><span data-stu-id="4f5e6-162">Limitations</span></span>
<span data-ttu-id="4f5e6-163">Této webové služby může trvat pouze číselné hodnoty jako funkce proměnné (sloupce).</span><span class="sxs-lookup"><span data-stu-id="4f5e6-163">This web service can take only numerical values as feature variables (columns).</span></span> <span data-ttu-id="4f5e6-164">Sloupec "události" může trvat pouze hodnota 0 nebo 1.</span><span class="sxs-lookup"><span data-stu-id="4f5e6-164">The “event” column can take only value 0 or 1.</span></span> <span data-ttu-id="4f5e6-165">Sloupec "čas" musí být kladné celé číslo.</span><span class="sxs-lookup"><span data-stu-id="4f5e6-165">The “time” column needs to be a positive integer.</span></span>

## <a name="faq"></a><span data-ttu-id="4f5e6-166">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="4f5e6-166">FAQ</span></span>
<span data-ttu-id="4f5e6-167">Nejčastější dotazy o spotřebě webové služby nebo publikování na webu Azure Marketplace naleznete v části [zde](machine-learning-marketplace-faq.md).</span><span class="sxs-lookup"><span data-stu-id="4f5e6-167">For frequently asked questions on consumption of the web service or publishing to the Azure Marketplace, see [here](machine-learning-marketplace-faq.md).</span></span>

[1]: ./media/machine-learning-r-csharp-survival-analysis/survive_img2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/

