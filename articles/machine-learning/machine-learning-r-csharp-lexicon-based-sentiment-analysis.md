---
title: "AAA(Deprecated) slovníkový na základě postojích analýza - Azure | Microsoft Docs"
description: "(nepoužívané) Slovníkový na základě analýzy postojích"
services: machine-learning
documentationcenter: 
author: pengxia
manager: jhubbard
editor: cgronlun
ms.assetid: 912f41af-966c-4d79-a413-6f9fc02823df
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: pengxia
ROBOTS: NOINDEX
redirect_url: https://gallery.cortanaintelligence.com/
redirect_document_id: True
ms.openlocfilehash: 1ed7e19441c6a8ad270a0c0f567b4aea588a583e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deprecated-lexicon-based-sentiment-analysis"></a><span data-ttu-id="940f5-103">(nepoužívané) Slovníkový na základě analýzy postojích</span><span class="sxs-lookup"><span data-stu-id="940f5-103">(deprecated) Lexicon Based Sentiment Analysis</span></span>

> [!NOTE]
> <span data-ttu-id="940f5-104">Hello Microsoft DataMarket se postupně vyřazuje z provozu a toto rozhraní API je zastaralá.</span><span class="sxs-lookup"><span data-stu-id="940f5-104">hello Microsoft DataMarket is being retired and this API has been deprecated.</span></span> 
> 
> <span data-ttu-id="940f5-105">Mnoho užitečné příklad experimentů a rozhraní API můžete najít v hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="940f5-105">You can find many useful example experiments and APIs in hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="940f5-106">Další informace o hello Galerie najdete v tématu [sdílené složky a zjišťovat prostředky ve hello Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span><span class="sxs-lookup"><span data-stu-id="940f5-106">For more information about hello Gallery, see [Share and discover resources in hello Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span></span>

<span data-ttu-id="940f5-107">Jak můžete budete vyhodnocovat názory a postojích směrem k značky nebo témata v online sociálních sítí, uživatelů, jako je Facebook odešle tweetů, recenzemi atd.?</span><span class="sxs-lookup"><span data-stu-id="940f5-107">How can you measure users’ opinions and attitudes toward brands or topics in online social networks, such as Facebook posts, tweets, reviews, etc.?</span></span> <span data-ttu-id="940f5-108">Analýza postojích poskytuje metodu pro analýzu takové dotazy.</span><span class="sxs-lookup"><span data-stu-id="940f5-108">Sentiment analysis provides a method for analyzing such questions.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

<span data-ttu-id="940f5-109">Obecně existují dvě metody pro analýzu postojích.</span><span class="sxs-lookup"><span data-stu-id="940f5-109">There are generally two methods for sentiment analysis.</span></span> <span data-ttu-id="940f5-110">Jeden používá algoritmus učení pod dohledem a hello jiných lze považovat za learning bez dohledu.</span><span class="sxs-lookup"><span data-stu-id="940f5-110">One is using a supervised learning algorithm, and hello other can be treated as unsupervised learning.</span></span> <span data-ttu-id="940f5-111">Model klasifikace algoritmu učení pod dohledem obecně staví na velké poznámkou souhrnu.</span><span class="sxs-lookup"><span data-stu-id="940f5-111">A supervised learning algorithm generally builds a classification model on a large annotated corpus.</span></span> <span data-ttu-id="940f5-112">Jeho přesnost je především založený na kvalitu hello hello poznámky a obvykle hello školení procesu bude trvat dlouhou dobu.</span><span class="sxs-lookup"><span data-stu-id="940f5-112">Its accuracy is mainly based on hello quality of hello annotation, and usually hello training process will take a long time.</span></span> <span data-ttu-id="940f5-113">Kromě toho, když jsme použít hello algoritmus tooanother domény, hello výsledek není obvykle dobrou.</span><span class="sxs-lookup"><span data-stu-id="940f5-113">Besides that, when we apply hello algorithm tooanother domain, hello result is usually not good.</span></span> <span data-ttu-id="940f5-114">Porovná toosupervised učení se slovníkový na základě postojích slovník, který nevyžaduje ukládání velkého množství dat souhrnu při strojovém učení bez dohledu a příprava, díky čemuž hello celého procesu mnohem rychlejší.</span><span class="sxs-lookup"><span data-stu-id="940f5-114">Compared toosupervised learning, lexicon-based unsupervised learning uses a sentiment dictionary, which doesn’t require storing a large data corpus and training - which makes hello whole process much faster.</span></span> 

<span data-ttu-id="940f5-115">Naše [služby](https://datamarket.azure.com/dataset/aml_labs/lexicon_based_sentiment_analysis) je založený na hello MPQA míru slovníkový (http://mpqa.cs.pitt.edu/lexicons/subj_lexicon/), což je jedna ze lexikon hello nejčastěji používaná míru.</span><span class="sxs-lookup"><span data-stu-id="940f5-115">Our [service](https://datamarket.azure.com/dataset/aml_labs/lexicon_based_sentiment_analysis) is built on hello MPQA Subjectivity Lexicon (http://mpqa.cs.pitt.edu/lexicons/subj_lexicon/), which is one of hello most commonly used subjectivity lexicons.</span></span> <span data-ttu-id="940f5-116">V MPQA nejsou 5097 záporné a 2533 kladné slova.</span><span class="sxs-lookup"><span data-stu-id="940f5-116">There are 5097 negative and 2533 positive words in MPQA.</span></span> <span data-ttu-id="940f5-117">A všechny tyto slova jsou označená poznámkou jako polarita velký nebo malý.</span><span class="sxs-lookup"><span data-stu-id="940f5-117">And all of these words are annotated as strong or weak polarity.</span></span> <span data-ttu-id="940f5-118">celý svátek Hello je v rámci licence GNU General Public License.</span><span class="sxs-lookup"><span data-stu-id="940f5-118">hello whole corpus is under GNU General Public License.</span></span> <span data-ttu-id="940f5-119">Hello webová služba může být krátký věty použité tooany, například tweetů a hraniční sítě Facebook.</span><span class="sxs-lookup"><span data-stu-id="940f5-119">hello web service can be applied tooany short sentences, such as tweets and Facebook posts.</span></span> 

> <span data-ttu-id="940f5-120">Této webové služby, může být například spotřebován uživateli – potenciálně prostřednictvím mobilní aplikace prostřednictvím webu, nebo i v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="940f5-120">This web service could be consumed by users – potentially through a mobile app, through a website, or even on a local computer for example.</span></span> <span data-ttu-id="940f5-121">Ale hello účelem hello webové služby je také tooserve jako příklad, jak Azure Machine Learning lze použít toocreate webové služby v kódu jazyka R.</span><span class="sxs-lookup"><span data-stu-id="940f5-121">But hello purpose of hello web service is also tooserve as an example of how Azure Machine Learning can be used toocreate web services on top of R code.</span></span> <span data-ttu-id="940f5-122">Zadání několika řádků kódu jazyka R a klikne na tlačítko v rámci Azure Machine Learning Studio můžete experimentu vytvořené pomocí kódu jazyka R a publikovat jako webovou službu.</span><span class="sxs-lookup"><span data-stu-id="940f5-122">With just a few lines of R code and clicks of a button within Azure Machine Learning Studio, an experiment can be created with R code and published as a web service.</span></span> <span data-ttu-id="940f5-123">Hello webové služby může být pak publikované toohello Azure Marketplace a využívat tak, že uživatelé a zařízení napříč hello, world s žádné nastavení infrastruktury autorem hello hello webové služby.</span><span class="sxs-lookup"><span data-stu-id="940f5-123">hello web service can then be published toohello Azure Marketplace and consumed by users and devices across hello world with no infrastructure setup by hello author of hello web service.</span></span>
> 
> 

## <a name="consumption-of-web-service"></a><span data-ttu-id="940f5-124">Spotřeba webové služby</span><span class="sxs-lookup"><span data-stu-id="940f5-124">Consumption of web service</span></span>
<span data-ttu-id="940f5-125">Hello vstupních dat může být jakýkoli text, ale hello webová služba funguje lépe s krátkou věty.</span><span class="sxs-lookup"><span data-stu-id="940f5-125">hello input data can be any text, but hello web service works better with short sentences.</span></span> <span data-ttu-id="940f5-126">výstup Hello je číselná hodnota od -1 a 1.</span><span class="sxs-lookup"><span data-stu-id="940f5-126">hello output is a numeric value between -1 and 1.</span></span> <span data-ttu-id="940f5-127">Všechny hodnotu nižší než 0 označuje, že je hello postojích textu hello záporné; Pokud je kladné vyšší než 0.</span><span class="sxs-lookup"><span data-stu-id="940f5-127">Any value below 0 denotes that hello sentiment of hello text is negative; positive if above 0.</span></span> <span data-ttu-id="940f5-128">absolutní hodnota Hello hello výsledku označuje hello sílu postojích hello přidružené.</span><span class="sxs-lookup"><span data-stu-id="940f5-128">hello absolute value of hello result denotes hello strength of hello associated sentiment.</span></span> 

> <span data-ttu-id="940f5-129">Tato služba jako hostované na hello Azure Marketplace, je službou OData. To může být volána prostřednictvím metody POST nebo GET.</span><span class="sxs-lookup"><span data-stu-id="940f5-129">This service, as hosted on hello Azure Marketplace, is an OData service; these may be called through POST or GET methods.</span></span> 
> 
> 

<span data-ttu-id="940f5-130">Existuje více způsobů využívání služby hello automatizovaně (je třeba aplikace [sem](http://microsoftazuremachinelearning.azurewebsites.net/)).</span><span class="sxs-lookup"><span data-stu-id="940f5-130">There are multiple ways of consuming hello service in an automated fashion (an example app is [here](http://microsoftazuremachinelearning.azurewebsites.net/)).</span></span>

### <a name="starting-c-code-for-web-service-consumption"></a><span data-ttu-id="940f5-131">Spouštění kódu C# pro používání webové služby:</span><span class="sxs-lookup"><span data-stu-id="940f5-131">Starting C# code for web service consumption:</span></span>
    public class ScoreResult
    {
            [DataMember]
            public double result
            {
                get;
                set;
            }
    }

    void main()
    {
            using (var wb = new WebClient())
            {
                var acitionUri = new Uri("PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score");
                DataServiceContext ctx = new DataServiceContext(acitionUri);
                var cred = new NetworkCredential("PutEmailAddressHere", "ChangeToAPIKey");
                var cache = new CredentialCache();

                cache.Add(acitionUri, "Basic", cred);
                ctx.Credentials = cache;
                var query = ctx.Execute<ScoreResult>(acitionUri, "POST", true, new BodyOperationParameter("Text", TextBox1.Text));
                ScoreResult scoreResult = query.ElementAt(0);
                double result = scoreResult.result;
            }
    }



<span data-ttu-id="940f5-132">vstup Hello je "Dnes je dobrý den."</span><span class="sxs-lookup"><span data-stu-id="940f5-132">hello input is “Today is a good day.”</span></span> <span data-ttu-id="940f5-133">výstup Hello je "1", což označuje kladné postojích přidružené hello vstupní věty.</span><span class="sxs-lookup"><span data-stu-id="940f5-133">hello output is “1”, which indicates a positive sentiment associated with hello input sentence.</span></span> 

## <a name="creation-of-web-service"></a><span data-ttu-id="940f5-134">Vytvoření webové služby</span><span class="sxs-lookup"><span data-stu-id="940f5-134">Creation of web service</span></span>
> <span data-ttu-id="940f5-135">Této webové služby byl vytvořen pomocí Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="940f5-135">This web service was created using Azure Machine Learning.</span></span> <span data-ttu-id="940f5-136">Pro bezplatnou zkušební verzi, jakož i úvodní videa na vytváření experimentů a [publikování webových služeb](machine-learning-publish-a-machine-learning-web-service.md), najdete v tématu [azure.com/ml](http://azure.com/ml).</span><span class="sxs-lookup"><span data-stu-id="940f5-136">For a free trial, as well as introductory videos on creating experiments and [publishing web services](machine-learning-publish-a-machine-learning-web-service.md), please see [azure.com/ml](http://azure.com/ml).</span></span> <span data-ttu-id="940f5-137">Níže je snímek obrazovky hello experimentu, který vytvořili hello webové služby a příklad kódu pro každou hello modulů v rámci experimentu hello.</span><span class="sxs-lookup"><span data-stu-id="940f5-137">Below is a screenshot of hello experiment that created hello web service and example code for each of hello modules within hello experiment.</span></span>
> 
> 

<span data-ttu-id="940f5-138">Z v rámci Azure Machine Learning, která vytvořila nový prázdný experiment.</span><span class="sxs-lookup"><span data-stu-id="940f5-138">From within Azure Machine Learning, a new blank experiment was created.</span></span> <span data-ttu-id="940f5-139">Hello obrázek níže ukazuje hello experimentu toku na základě slovníkový postojích analýzy.</span><span class="sxs-lookup"><span data-stu-id="940f5-139">hello figure below shows hello experiment flow of lexicon-based sentiment analysis.</span></span> <span data-ttu-id="940f5-140">soubor "sent_dict.csv" Hello hello MPQA míru slovníkový a je nastavená jako jeden z hello vstupů z [spustit skript jazyka R][execute-r-script].</span><span class="sxs-lookup"><span data-stu-id="940f5-140">hello “sent_dict.csv” file is hello MPQA subjectivity lexicon, and is set as one of hello inputs of [Execute R Script][execute-r-script].</span></span> <span data-ttu-id="940f5-141">Jiné vstup je kontrolu jen Vzorkovaná z hello Amazon kontrolní datové sady pro test, kde můžeme provést výběr, změna název sloupce a rozdělení operace.</span><span class="sxs-lookup"><span data-stu-id="940f5-141">Another input is a sampled review from hello Amazon review dataset for test, where we performed selection, column name modification, and split operations.</span></span> <span data-ttu-id="940f5-142">Můžeme použít hodnotu hash balíčku toostore hello míru slovníkový v hello paměti a urychlit proces výpočtu skóre hello.</span><span class="sxs-lookup"><span data-stu-id="940f5-142">We use a hash package toostore hello subjectivity lexicon in hello memory and accelerate hello score computation process.</span></span> <span data-ttu-id="940f5-143">celý text Hello se tokenizovaného balíček "tm" a ve srovnání s hello slova ve slovníku postojích hello.</span><span class="sxs-lookup"><span data-stu-id="940f5-143">hello whole text will be tokenized by “tm” package and compared with hello word in hello sentiment dictionary.</span></span> <span data-ttu-id="940f5-144">Nakonec bude přidáním hello váhu jednotlivých subjektivní slov v textu hello vypočítat skóre.</span><span class="sxs-lookup"><span data-stu-id="940f5-144">Finally, a score will be calculated by adding hello weight of each subjective word in hello text.</span></span> 

### <a name="experiment-flow"></a><span data-ttu-id="940f5-145">Experiment toku:</span><span class="sxs-lookup"><span data-stu-id="940f5-145">Experiment flow:</span></span>
![Tok experimentu][2]

#### <a name="module-1"></a><span data-ttu-id="940f5-147">Modul 1:</span><span class="sxs-lookup"><span data-stu-id="940f5-147">Module 1:</span></span>
    # Map 1-based optional input ports toovariables
    sent_dict_data<- maml.mapInputPort(1) # class: data.frame
    dataset2 <- maml.mapInputPort(2) # class: data.frame

# <a name="install-hash-package"></a><span data-ttu-id="940f5-148">Instalovat balíček hash</span><span class="sxs-lookup"><span data-stu-id="940f5-148">Install hash package</span></span>
    install.packages("src/hash_2.2.6.zip", lib = ".", repos = NULL, verbose = TRUE)
    success <- library("hash", lib.loc = ".", logical.return = TRUE, verbose = TRUE)
    library(tm)
    library(stringr)

    #create sentiment dictionary
    negation_word <- c("not","nor", "no")
    result <- c()
    sent_dict <- hash()
    sent_dict <- hash(sent_dict_data$word, sent_dict_data$polarity)

    #  Compute sentiment score for each document
    for (m in 1:nrow(dataset2)){
    polarity_ratio <- 0
    polarity_total <- 0
    not <- 0
    sentence <- tolower(dataset2[m,1])
    if (nchar(sentence) > 0){
        token_array <- scan_tokenizer(sentence)
        for (j in 1:length(token_array)){
            word = str_replace_all(token_array[j], "[^[:alnum:]]", "")
            for (k in 1:length(negation_word)){
              if (word == negation_word[k]){
                not <- (not+1) %% 2

              }
            }
            if (word != ""){
                if (!is.null(sent_dict[[word]])){
                  polarity_ratio <- polarity_ratio + (-2*not+1)*sent_dict[[word]]
                  polarity_total <- polarity_total + abs(sent_dict[[word]])
                }
            }

        }
    }
    if (polarity_total > 0){
        result <- c(result, polarity_ratio/polarity_total)
    }else{
        result<- c(result,0)
    }
    }

    # Sample operation
    data.set <- data.frame(result)

    # Select data.frame toobe sent toohello output Dataset port
    maml.mapOutputPort("data.set")



## <a name="limitations"></a><span data-ttu-id="940f5-149">Omezení</span><span class="sxs-lookup"><span data-stu-id="940f5-149">Limitations</span></span>
<span data-ttu-id="940f5-150">Z hlediska algoritmus je na základě slovníkový postojích analysis nástroj analysis obecné postojích, což lépe než hello metodu klasifikace pro konkrétních polí, nemusí fungovat.</span><span class="sxs-lookup"><span data-stu-id="940f5-150">From an algorithm perspective, lexicon-based sentiment analysis is a general sentiment analysis tool, which may not perform better than hello classification method for specific fields.</span></span> <span data-ttu-id="940f5-151">není dobře Hello negace problém řešit.</span><span class="sxs-lookup"><span data-stu-id="940f5-151">hello negation problem is not well dealt with.</span></span> <span data-ttu-id="940f5-152">Budeme používat pevné kódování několik negace slova v naší aplikaci, ale ještě lepší způsob je pomocí slovník negace a vytvořit některá pravidla.</span><span class="sxs-lookup"><span data-stu-id="940f5-152">We hardcode several negation words in our program, but a better way is using a negation dictionary and build some rules.</span></span> <span data-ttu-id="940f5-153">Webová služba Hello provádí lépe krátký a jednoduché věty, jako je například tweetů a hraniční sítě Facebook, než na věty dlouhá a složitá, například Amazon recenze.</span><span class="sxs-lookup"><span data-stu-id="940f5-153">hello web service performs better on short and simple sentences, such as tweets and Facebook posts, than on long and complex sentences such as Amazon reviews.</span></span> 

## <a name="faq"></a><span data-ttu-id="940f5-154">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="940f5-154">FAQ</span></span>
<span data-ttu-id="940f5-155">Nejčastější dotazy o spotřebě hello webové služby nebo publikování toohello Azure Marketplace naleznete v části [zde](machine-learning-marketplace-faq.md).</span><span class="sxs-lookup"><span data-stu-id="940f5-155">For frequently asked questions on consumption of hello web service or publishing toohello Azure Marketplace, see [here](machine-learning-marketplace-faq.md).</span></span>

[1]: ./media/machine-learning-r-csharp-lexicon-based-sentiment-analysis/sentiment_analysis_1.png
[2]: ./media/machine-learning-r-csharp-lexicon-based-sentiment-analysis/sentiment_analysis_2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/


