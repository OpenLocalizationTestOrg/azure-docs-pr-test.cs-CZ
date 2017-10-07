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
# <a name="deprecated-survival-analysis"></a><span data-ttu-id="951fd-103">(nepoužívané) Základními analýzy</span><span class="sxs-lookup"><span data-stu-id="951fd-103">(deprecated) Survival Analysis</span></span>

> [!NOTE]
> <span data-ttu-id="951fd-104">Hello Microsoft DataMarket se postupně vyřazuje z provozu a toto rozhraní API je zastaralá.</span><span class="sxs-lookup"><span data-stu-id="951fd-104">hello Microsoft DataMarket is being retired and this API has been deprecated.</span></span> 
> 
> <span data-ttu-id="951fd-105">Mnoho užitečné příklad experimentů a rozhraní API můžete najít v hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="951fd-105">You can find many useful example experiments and APIs in hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="951fd-106">Další informace o hello Galerie najdete v tématu [sdílené složky a zjišťovat prostředky ve hello Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span><span class="sxs-lookup"><span data-stu-id="951fd-106">For more information about hello Gallery, see [Share and discover resources in hello Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span></span>

<span data-ttu-id="951fd-107">V části mnoho scénářů je hlavní výsledek hello pod assessment hello čas tooan událost zájmu.</span><span class="sxs-lookup"><span data-stu-id="951fd-107">Under many scenarios, hello main outcome under assessment is hello time tooan event of interest.</span></span> <span data-ttu-id="951fd-108">Jinými slovy hello otázku "při této události dojde?"</span><span class="sxs-lookup"><span data-stu-id="951fd-108">In other words, hello question “when this event will occur?”</span></span> <span data-ttu-id="951fd-109">se zobrazí výzva.</span><span class="sxs-lookup"><span data-stu-id="951fd-109">is asked.</span></span> <span data-ttu-id="951fd-110">Jako příklady, zvažte situacích, kde hello data popisuje hello uplynulý čas (dny, let, vzdálenost atd.), dokud hello dojde k události, které vás zajímají (relapse nákazy, aplikaci PhotoDraw stupeň přijata, brzdy pad selhání).</span><span class="sxs-lookup"><span data-stu-id="951fd-110">As examples, consider situations where hello data describes hello elapsed time (days, years, mileage, etc.) until hello event of interest (disease relapse, PhD degree received, brake pad failure) occurs.</span></span> <span data-ttu-id="951fd-111">Každá instance ve hello dat představuje určitý objekt (pacienta student, car, atd.).</span><span class="sxs-lookup"><span data-stu-id="951fd-111">Each instance in hello data represents a specific object (a patient, a student, a car, etc.).</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

<span data-ttu-id="951fd-112">To [webovou službu](https://datamarket.azure.com/dataset/aml_labs/survivalanalysis) odpovědi hello otázku "co je hello pravděpodobnost, že hello události týkající se provádí n čas pro objekt x?"</span><span class="sxs-lookup"><span data-stu-id="951fd-112">This [web service](https://datamarket.azure.com/dataset/aml_labs/survivalanalysis) answers hello question “what is hello probability that hello event of interest will occur by time n for object x?”</span></span> <span data-ttu-id="951fd-113">Poskytnutím model analysis základními této webové služby umožňuje uživatelům toosupply data tootrain hello modelu a otestovat.</span><span class="sxs-lookup"><span data-stu-id="951fd-113">By providing a survival analysis model, this web service enables users toosupply data tootrain hello model and test it.</span></span> <span data-ttu-id="951fd-114">Hlavní téma Hello hello experimentu je délka hello toomodel hello uplynulého času, dokud nedojde k hello událostí, které vás zajímají.</span><span class="sxs-lookup"><span data-stu-id="951fd-114">hello main theme of hello experiment is toomodel hello length of hello elapsed time until hello event of interest occurs.</span></span> 

> <span data-ttu-id="951fd-115">Tato webová služba může spotřebovat uživateli – potenciálně prostřednictvím mobilní aplikace prostřednictvím webu, nebo i v místním počítači, třeba.</span><span class="sxs-lookup"><span data-stu-id="951fd-115">This web service could be consumed by users – potentially through a mobile app, through a website, or even on a local computer, for example.</span></span> <span data-ttu-id="951fd-116">Ale hello účelem hello webové služby je také tooserve jako příklad, jak Azure Machine Learning lze použít toocreate webové služby v kódu jazyka R.</span><span class="sxs-lookup"><span data-stu-id="951fd-116">But hello purpose of hello web service is also tooserve as an example of how Azure Machine Learning can be used toocreate web services on top of R code.</span></span> <span data-ttu-id="951fd-117">Zadání několika řádků kódu jazyka R a klikne na tlačítko v rámci Azure Machine Learning Studio můžete experimentu vytvořené pomocí kódu jazyka R a publikovat jako webovou službu.</span><span class="sxs-lookup"><span data-stu-id="951fd-117">With just a few lines of R code and clicks of a button within Azure Machine Learning Studio, an experiment can be created with R code and published as a web service.</span></span> <span data-ttu-id="951fd-118">Hello webové služby může být pak publikované toohello Azure Marketplace a využívat tak, že uživatelé a zařízení napříč hello, world s žádné nastavení infrastruktury autorem hello hello webové služby.</span><span class="sxs-lookup"><span data-stu-id="951fd-118">hello web service can then be published toohello Azure Marketplace and consumed by users and devices across hello world with no infrastructure setup by hello author of hello web service.</span></span>  
> 
> 

## <a name="consumption-of-web-service"></a><span data-ttu-id="951fd-119">Spotřeba webové služby</span><span class="sxs-lookup"><span data-stu-id="951fd-119">Consumption of web service</span></span>
<span data-ttu-id="951fd-120">vstupní data schématu Hello hello webové služby se zobrazí v hello následující tabulka.</span><span class="sxs-lookup"><span data-stu-id="951fd-120">hello input data schema of hello web service is shown in hello following table.</span></span> <span data-ttu-id="951fd-121">Šest údaje jsou potřeba jako vstup hello: Cvičná data, testování data, času, které vás zajímají, index hello "časové" dimenze, hello index "událost" dimenze a typy proměnných hello (souvislé nebo multi-Factor).</span><span class="sxs-lookup"><span data-stu-id="951fd-121">Six pieces of information are needed as hello input: training data, testing data, time of interest, hello index of "time" dimension, hello index of "event" dimension, and hello variable types (continuous or factor).</span></span> <span data-ttu-id="951fd-122">Hello Cvičná data je reprezentován řetězcem, kde hello řádky jsou oddělené čárkou a hello sloupce jsou odděleny středníkem.</span><span class="sxs-lookup"><span data-stu-id="951fd-122">hello training data is represented with a string, where hello rows are separated by comma, and hello columns are separated by semicolon.</span></span> <span data-ttu-id="951fd-123">Hello řadu funkcí hello dat je flexibilní.</span><span class="sxs-lookup"><span data-stu-id="951fd-123">hello number of features of hello data is flexible.</span></span> <span data-ttu-id="951fd-124">Všechny elementy hello ve vstupním řetězci hello musí být číselná hodnota.</span><span class="sxs-lookup"><span data-stu-id="951fd-124">All hello elements in hello input string must be numeric.</span></span> <span data-ttu-id="951fd-125">V hello Cvičná data dimenze času"hello" označuje, že hello počet jednotek (dny, let, vzdálenost atd.), které uplynuly od hello výchozí bod hello prostudovali (pacienta přijetí nedovolenému zacházení programy, aplikaci PhotoDraw studie počáteční studenty, automobilu od toobe řízené, atd.) dokud hello zájmu (hello pacienta vrácení toodrug využití, hello student získání hello aplikaci PhotoDraw stupně, car hello s selhání brzdy pad, atd.) dojde k události.</span><span class="sxs-lookup"><span data-stu-id="951fd-125">In hello training data, hello “time” dimension indicates hello number of time units (days, years, mileage, etc.) elapsed since hello starting point of hello study (a patient receiving drug treatment programs, a student starting PhD study, a car starting toobe driven, etc.) until hello event of interest (hello patient returning toodrug usage, hello student obtaining hello PhD degree, hello car having brake pad failure, etc.) occurs.</span></span> <span data-ttu-id="951fd-126">dimenze "událost" Hello značí, zda na konci hello hello studie hello událostí, které vás zajímají.</span><span class="sxs-lookup"><span data-stu-id="951fd-126">hello “event” dimension indicates whether hello event of interest occurs at hello end of hello study.</span></span> <span data-ttu-id="951fd-127">Hodnota "události = 1" znamená, že hello událostí, které vás zajímají nastane v čase hello indikován dimenze "čas" hello; "události = 0" znamená, že hello událostí, které vás zajímají nedošlo k hello doby indikován hello "čas" dimenze.</span><span class="sxs-lookup"><span data-stu-id="951fd-127">A value of “event=1” means that hello event of interest occurs at hello time indicated by hello “time” dimension; “event=0” means that hello event of interest has not occurred by hello time indicated by hello “time” dimension.</span></span>

* <span data-ttu-id="951fd-128">trainingdata – řetězec znaků.</span><span class="sxs-lookup"><span data-stu-id="951fd-128">trainingdata - A character string.</span></span> <span data-ttu-id="951fd-129">Řádky jsou oddělené čárkou a sloupce jsou odděleny středníkem.</span><span class="sxs-lookup"><span data-stu-id="951fd-129">Rows are separated by comma, and columns are separated by semicolon.</span></span> <span data-ttu-id="951fd-130">Každý řádek obsahuje dimenze "čas", "událost" dimenze a předpověď proměnné.</span><span class="sxs-lookup"><span data-stu-id="951fd-130">Each row includes “time” dimension, “event” dimension, and predictor variables.</span></span>
* <span data-ttu-id="951fd-131">testingdata – jeden řádek dat, která obsahuje předpověď proměnných pro určitý objekt.</span><span class="sxs-lookup"><span data-stu-id="951fd-131">testingdata - One row of data that contains predictor variables for a particular object.</span></span>
* <span data-ttu-id="951fd-132">time_of_interest - hello uplynulá doba n vás zajímají.</span><span class="sxs-lookup"><span data-stu-id="951fd-132">time_of_interest - hello elapsed time of interest n.</span></span>
* <span data-ttu-id="951fd-133">index_time – index sloupce hello dimenze "čas" hello (počínaje 1).</span><span class="sxs-lookup"><span data-stu-id="951fd-133">index_time - hello column index of hello “time” dimension (starting from 1).</span></span>
* <span data-ttu-id="951fd-134">index_event – index sloupce hello dimenze "událost" hello (počínaje 1).</span><span class="sxs-lookup"><span data-stu-id="951fd-134">index_event - hello column index of hello “event” dimension (starting from 1).</span></span>
* <span data-ttu-id="951fd-135">variable_types – řetězec znaků se středníky jako oddělovače v ní.</span><span class="sxs-lookup"><span data-stu-id="951fd-135">variable_types - A character string with semicolons as separators in it.</span></span> <span data-ttu-id="951fd-136">Hodnota 0 představuje průběžné proměnné a 1 představuje Multi-Factor proměnné.</span><span class="sxs-lookup"><span data-stu-id="951fd-136">0 represents continuous variables and 1 represents factor variables.</span></span>

<span data-ttu-id="951fd-137">výstup Hello je hello pravděpodobnost událost, ke kterým dochází ve určitý čas.</span><span class="sxs-lookup"><span data-stu-id="951fd-137">hello output is hello probability of an event occurring by a specific time.</span></span> 

> <span data-ttu-id="951fd-138">Tato služba jako hostované na hello Azure Marketplace, je službou OData. To může být volána prostřednictvím metody POST nebo GET.</span><span class="sxs-lookup"><span data-stu-id="951fd-138">This service, as hosted on hello Azure Marketplace, is an OData service; these may be called through POST or GET methods.</span></span> 
> 
> 

<span data-ttu-id="951fd-139">Existuje více způsobů využívání služby hello automatizovaně (je třeba aplikace [sem](http://microsoftazuremachinelearning.azurewebsites.net/SurvivalAnalysis.aspx)).</span><span class="sxs-lookup"><span data-stu-id="951fd-139">There are multiple ways of consuming hello service in an automated fashion (an example app is [here](http://microsoftazuremachinelearning.azurewebsites.net/SurvivalAnalysis.aspx)).</span></span> 

### <a name="starting-c-code-for-web-service-consumption"></a><span data-ttu-id="951fd-140">Spouštění kódu C# pro používání webové služby:</span><span class="sxs-lookup"><span data-stu-id="951fd-140">Starting C# code for web service consumption:</span></span>
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




<span data-ttu-id="951fd-141">Hello výklad tento test je následující.</span><span class="sxs-lookup"><span data-stu-id="951fd-141">hello interpretation of this test is as follows.</span></span> <span data-ttu-id="951fd-142">Za předpokladu, že cílem hello hello dat je toomodel hello uplynulý čas dokud hello vrátí toodrug využití pro hello pacientů, kteří obdrželi jeden hello dva zacházení programů.</span><span class="sxs-lookup"><span data-stu-id="951fd-142">Assuming hello goal of hello data is toomodel hello elapsed time until hello return toodrug usage for hello patients who received one of hello two treatment programs.</span></span> <span data-ttu-id="951fd-143">Hello výstup hello webové služby čtení: pacientů se 35 let, s předchozí obchodu zacházení 2 dobu, trvá dlouho domácí zacházení programu hello, a s možností využití heroinu i kokainu hello pravděpodobnost vrácení toohello nedovolenému využití je 95.64 % od den 500.</span><span class="sxs-lookup"><span data-stu-id="951fd-143">hello output of hello web service reads: for patients being 35 years old, having previous drug treatment 2 times, taking hello long residential treatment program, and with both heroin and cocaine usage, hello probability of returning toohello drug usage is 95.64% by day 500.</span></span>

## <a name="creation-of-web-service"></a><span data-ttu-id="951fd-144">Vytvoření webové služby</span><span class="sxs-lookup"><span data-stu-id="951fd-144">Creation of web service</span></span>
> <span data-ttu-id="951fd-145">Této webové služby byl vytvořen pomocí Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="951fd-145">This web service was created using Azure Machine Learning.</span></span> <span data-ttu-id="951fd-146">Pro bezplatnou zkušební verzi, jakož i úvodní videa na vytváření experimentů a [publikování webových služeb](machine-learning-publish-a-machine-learning-web-service.md), najdete v tématu [azure.com/ml](http://azure.com/ml).</span><span class="sxs-lookup"><span data-stu-id="951fd-146">For a free trial, as well as introductory videos on creating experiments and [publishing web services](machine-learning-publish-a-machine-learning-web-service.md), please see [azure.com/ml](http://azure.com/ml).</span></span> <span data-ttu-id="951fd-147">Níže je snímek obrazovky hello experimentu, který vytvořili hello webové služby a příklad kódu pro každou hello modulů v rámci experimentu hello.</span><span class="sxs-lookup"><span data-stu-id="951fd-147">Below is a screenshot of hello experiment that created hello web service and example code for each of hello modules within hello experiment.</span></span>
> 
> 

<span data-ttu-id="951fd-148">Ze v rámci Azure Machine Learning, nový prázdný experiment byl vytvořen a dvě [spustit skript jazyka R] [ execute-r-script] moduly byly vyžádány do pracovního prostoru hello.</span><span class="sxs-lookup"><span data-stu-id="951fd-148">From within Azure Machine Learning, a new blank experiment was created and two [Execute R Script][execute-r-script] modules were pulled onto hello workspace.</span></span> <span data-ttu-id="951fd-149">Hello schéma dat byl vytvořen s jednoduchou [spustit skript jazyka R][execute-r-script], která definuje schéma hello vstupní data pro hello webové služby.</span><span class="sxs-lookup"><span data-stu-id="951fd-149">hello data schema was created with a simple [Execute R Script][execute-r-script], which defines hello input data schema for hello web service.</span></span> <span data-ttu-id="951fd-150">Tento modul je pak druhý propojené toohello [spustit skript jazyka R] [ execute-r-script] modul, kterým hlavní pracovní.</span><span class="sxs-lookup"><span data-stu-id="951fd-150">This module is then linked toohello second [Execute R Script][execute-r-script] module, which does major work.</span></span> <span data-ttu-id="951fd-151">Tento modul nemá předzpracování dat, vytváření modelů a předpovědi.</span><span class="sxs-lookup"><span data-stu-id="951fd-151">This module does data preprocessing, model building, and predictions.</span></span> <span data-ttu-id="951fd-152">V kroku předběžného zpracování dat hello hello vstupní data reprezentována dlouhý řetězec transformovat a převést do rámečku data.</span><span class="sxs-lookup"><span data-stu-id="951fd-152">In hello data preprocessing step, hello input data represented by a long string is transformed and converted into a data frame.</span></span> <span data-ttu-id="951fd-153">V kroku sestavení modelu hello externí balíček R "survival_2.37-7.zip" první instalaci pro provádění základními analýzy.</span><span class="sxs-lookup"><span data-stu-id="951fd-153">In hello model building step, an external R package “survival_2.37-7.zip” is first installed for conducting survival analysis.</span></span> <span data-ttu-id="951fd-154">Funkce "coxph" hello se pak spustí až po úloh zpracování dat řady.</span><span class="sxs-lookup"><span data-stu-id="951fd-154">Then hello “coxph” function is executed after a series data processing tasks.</span></span> <span data-ttu-id="951fd-155">Podrobnosti o Hello hello "coxph" funkce pro analýzu základními může číst z hello R dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="951fd-155">hello details of hello “coxph” function for survival analysis can be read from hello R documentation.</span></span> <span data-ttu-id="951fd-156">V kroku předpovědi hello do hello trained model, který obsahuje funkci "surfit" hello je zadána testování instance a hello základními křivky pro tuto instanci testování vytváří jako proměnnou "křivky".</span><span class="sxs-lookup"><span data-stu-id="951fd-156">In hello prediction step, a testing instance is supplied into hello trained model with hello “surfit” function, and hello survival curve for this testing instance is produced as “curve” variable.</span></span> <span data-ttu-id="951fd-157">Nakonec se získávají hello pravděpodobnosti hello času, které vás zajímají.</span><span class="sxs-lookup"><span data-stu-id="951fd-157">Finally, hello probability of hello time of interest is obtained.</span></span> 

### <a name="experiment-flow"></a><span data-ttu-id="951fd-158">Experiment toku:</span><span class="sxs-lookup"><span data-stu-id="951fd-158">Experiment flow:</span></span>
![Tok experimentu][1]

#### <a name="module-1"></a><span data-ttu-id="951fd-160">Modul 1:</span><span class="sxs-lookup"><span data-stu-id="951fd-160">Module 1:</span></span>
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

#### <a name="module-2"></a><span data-ttu-id="951fd-161">Modul 2:</span><span class="sxs-lookup"><span data-stu-id="951fd-161">Module 2:</span></span>
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




## <a name="limitations"></a><span data-ttu-id="951fd-162">Omezení</span><span class="sxs-lookup"><span data-stu-id="951fd-162">Limitations</span></span>
<span data-ttu-id="951fd-163">Této webové služby může trvat pouze číselné hodnoty jako funkce proměnné (sloupce).</span><span class="sxs-lookup"><span data-stu-id="951fd-163">This web service can take only numerical values as feature variables (columns).</span></span> <span data-ttu-id="951fd-164">sloupec "události" Hello, může trvat pouze hodnota 0 nebo 1.</span><span class="sxs-lookup"><span data-stu-id="951fd-164">hello “event” column can take only value 0 or 1.</span></span> <span data-ttu-id="951fd-165">sloupec "čas" Hello musí toobe kladné celé číslo.</span><span class="sxs-lookup"><span data-stu-id="951fd-165">hello “time” column needs toobe a positive integer.</span></span>

## <a name="faq"></a><span data-ttu-id="951fd-166">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="951fd-166">FAQ</span></span>
<span data-ttu-id="951fd-167">Nejčastější dotazy o spotřebě hello webové služby nebo publikování toohello Azure Marketplace naleznete v části [zde](machine-learning-marketplace-faq.md).</span><span class="sxs-lookup"><span data-stu-id="951fd-167">For frequently asked questions on consumption of hello web service or publishing toohello Azure Marketplace, see [here](machine-learning-marketplace-faq.md).</span></span>

[1]: ./media/machine-learning-r-csharp-survival-analysis/survive_img2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/

