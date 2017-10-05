---
title: "(nepoužívané) Slovníkový na základě analýzy postojích - Azure | Microsoft Docs"
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
redirect_document_id: TRUE
ms.openlocfilehash: 7bc80a1e1067296528eca1a843ea30b0c27af616
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deprecated-lexicon-based-sentiment-analysis"></a><span data-ttu-id="ed1a4-103">(nepoužívané) Slovníkový na základě analýzy postojích</span><span class="sxs-lookup"><span data-stu-id="ed1a4-103">(deprecated) Lexicon Based Sentiment Analysis</span></span>

> [!NOTE]
> <span data-ttu-id="ed1a4-104">Microsoft DataMarket se postupně vyřazuje z provozu a toto rozhraní API je zastaralá.</span><span class="sxs-lookup"><span data-stu-id="ed1a4-104">The Microsoft DataMarket is being retired and this API has been deprecated.</span></span> 
> 
> <span data-ttu-id="ed1a4-105">Můžete najít mnoho užitečné příklad experimentů a rozhraní API v [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="ed1a4-105">You can find many useful example experiments and APIs in the [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="ed1a4-106">Další informace o galerii najdete v tématu [sdílené složky a zjišťovat prostředky ve Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span><span class="sxs-lookup"><span data-stu-id="ed1a4-106">For more information about the Gallery, see [Share and discover resources in the Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span></span>

<span data-ttu-id="ed1a4-107">Jak můžete budete vyhodnocovat názory a postojích směrem k značky nebo témata v online sociálních sítí, uživatelů, jako je Facebook odešle tweetů, recenzemi atd.?</span><span class="sxs-lookup"><span data-stu-id="ed1a4-107">How can you measure users’ opinions and attitudes toward brands or topics in online social networks, such as Facebook posts, tweets, reviews, etc.?</span></span> <span data-ttu-id="ed1a4-108">Analýza postojích poskytuje metodu pro analýzu takové dotazy.</span><span class="sxs-lookup"><span data-stu-id="ed1a4-108">Sentiment analysis provides a method for analyzing such questions.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

<span data-ttu-id="ed1a4-109">Obecně existují dvě metody pro analýzu postojích.</span><span class="sxs-lookup"><span data-stu-id="ed1a4-109">There are generally two methods for sentiment analysis.</span></span> <span data-ttu-id="ed1a4-110">Jeden používá algoritmus učení pod dohledem a dalších lze považovat za learning bez dohledu.</span><span class="sxs-lookup"><span data-stu-id="ed1a4-110">One is using a supervised learning algorithm, and the other can be treated as unsupervised learning.</span></span> <span data-ttu-id="ed1a4-111">Model klasifikace algoritmu učení pod dohledem obecně staví na velké poznámkou souhrnu.</span><span class="sxs-lookup"><span data-stu-id="ed1a4-111">A supervised learning algorithm generally builds a classification model on a large annotated corpus.</span></span> <span data-ttu-id="ed1a4-112">Jeho přesnost je především založený na kvalitu anotace a bude školení proces obvykle trvat dlouhou dobu.</span><span class="sxs-lookup"><span data-stu-id="ed1a4-112">Its accuracy is mainly based on the quality of the annotation, and usually the training process will take a long time.</span></span> <span data-ttu-id="ed1a4-113">Kromě toho, když jsme použít algoritmus do jiné domény, výsledek obvykle není funkční.</span><span class="sxs-lookup"><span data-stu-id="ed1a4-113">Besides that, when we apply the algorithm to another domain, the result is usually not good.</span></span> <span data-ttu-id="ed1a4-114">Ve srovnání s pod dohledem učení, na základě slovníkový používá bez dohledu learning postojích slovník, který nevyžaduje ukládání velkého množství dat souhrnu a cvičení – což umožňuje mnohem rychlejší celého procesu.</span><span class="sxs-lookup"><span data-stu-id="ed1a4-114">Compared to supervised learning, lexicon-based unsupervised learning uses a sentiment dictionary, which doesn’t require storing a large data corpus and training - which makes the whole process much faster.</span></span> 

<span data-ttu-id="ed1a4-115">Naše [služby](https://datamarket.azure.com/dataset/aml_labs/lexicon_based_sentiment_analysis) je založený na míru slovníkový MPQA (http://mpqa.cs.pitt.edu/lexicons/subj_lexicon/), který je jednou z nejčastěji používaných míru lexikon.</span><span class="sxs-lookup"><span data-stu-id="ed1a4-115">Our [service](https://datamarket.azure.com/dataset/aml_labs/lexicon_based_sentiment_analysis) is built on the MPQA Subjectivity Lexicon (http://mpqa.cs.pitt.edu/lexicons/subj_lexicon/), which is one of the most commonly used subjectivity lexicons.</span></span> <span data-ttu-id="ed1a4-116">V MPQA nejsou 5097 záporné a 2533 kladné slova.</span><span class="sxs-lookup"><span data-stu-id="ed1a4-116">There are 5097 negative and 2533 positive words in MPQA.</span></span> <span data-ttu-id="ed1a4-117">A všechny tyto slova jsou označená poznámkou jako polarita velký nebo malý.</span><span class="sxs-lookup"><span data-stu-id="ed1a4-117">And all of these words are annotated as strong or weak polarity.</span></span> <span data-ttu-id="ed1a4-118">Celý souhrnu se v rámci licence GNU General Public License.</span><span class="sxs-lookup"><span data-stu-id="ed1a4-118">The whole corpus is under GNU General Public License.</span></span> <span data-ttu-id="ed1a4-119">Webová služba je použít pro všechny krátké věty, jako je například tweetů a hraniční sítě Facebook.</span><span class="sxs-lookup"><span data-stu-id="ed1a4-119">The web service can be applied to any short sentences, such as tweets and Facebook posts.</span></span> 

> <span data-ttu-id="ed1a4-120">Této webové služby, může být například spotřebován uživateli – potenciálně prostřednictvím mobilní aplikace prostřednictvím webu, nebo i v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="ed1a4-120">This web service could be consumed by users – potentially through a mobile app, through a website, or even on a local computer for example.</span></span> <span data-ttu-id="ed1a4-121">Ale účelem webové služby je také slouží jako příklad použití Azure Machine Learning k vytvoření webové služby v kódu jazyka R.</span><span class="sxs-lookup"><span data-stu-id="ed1a4-121">But the purpose of the web service is also to serve as an example of how Azure Machine Learning can be used to create web services on top of R code.</span></span> <span data-ttu-id="ed1a4-122">Zadání několika řádků kódu jazyka R a klikne na tlačítko v rámci Azure Machine Learning Studio můžete experimentu vytvořené pomocí kódu jazyka R a publikovat jako webovou službu.</span><span class="sxs-lookup"><span data-stu-id="ed1a4-122">With just a few lines of R code and clicks of a button within Azure Machine Learning Studio, an experiment can be created with R code and published as a web service.</span></span> <span data-ttu-id="ed1a4-123">Webové služby můžete pak publikování na webu Azure Marketplace a spotřebovávají uživatelů a zařízení po celém světě s žádné nastavení infrastruktury autorem webové služby.</span><span class="sxs-lookup"><span data-stu-id="ed1a4-123">The web service can then be published to the Azure Marketplace and consumed by users and devices across the world with no infrastructure setup by the author of the web service.</span></span>
> 
> 

## <a name="consumption-of-web-service"></a><span data-ttu-id="ed1a4-124">Spotřeba webové služby</span><span class="sxs-lookup"><span data-stu-id="ed1a4-124">Consumption of web service</span></span>
<span data-ttu-id="ed1a4-125">Vstupních dat může být jakýkoli text, ale webová služba funguje lépe s krátkou věty.</span><span class="sxs-lookup"><span data-stu-id="ed1a4-125">The input data can be any text, but the web service works better with short sentences.</span></span> <span data-ttu-id="ed1a4-126">Výstup je číselná hodnota od -1 a 1.</span><span class="sxs-lookup"><span data-stu-id="ed1a4-126">The output is a numeric value between -1 and 1.</span></span> <span data-ttu-id="ed1a4-127">Všechny hodnotu nižší než 0 označuje, že postojích textu je záporný; Pokud je kladné vyšší než 0.</span><span class="sxs-lookup"><span data-stu-id="ed1a4-127">Any value below 0 denotes that the sentiment of the text is negative; positive if above 0.</span></span> <span data-ttu-id="ed1a4-128">Absolutní hodnota výsledku označuje sílu přidružené postojích.</span><span class="sxs-lookup"><span data-stu-id="ed1a4-128">The absolute value of the result denotes the strength of the associated sentiment.</span></span> 

> <span data-ttu-id="ed1a4-129">Tato služba jako hostované v Azure Marketplace je službou OData. To může být volána prostřednictvím metody POST nebo GET.</span><span class="sxs-lookup"><span data-stu-id="ed1a4-129">This service, as hosted on the Azure Marketplace, is an OData service; these may be called through POST or GET methods.</span></span> 
> 
> 

<span data-ttu-id="ed1a4-130">Existuje více způsobů využívání služby automatizovaně (je třeba aplikace [sem](http://microsoftazuremachinelearning.azurewebsites.net/)).</span><span class="sxs-lookup"><span data-stu-id="ed1a4-130">There are multiple ways of consuming the service in an automated fashion (an example app is [here](http://microsoftazuremachinelearning.azurewebsites.net/)).</span></span>

### <a name="starting-c-code-for-web-service-consumption"></a><span data-ttu-id="ed1a4-131">Spouštění kódu C# pro používání webové služby:</span><span class="sxs-lookup"><span data-stu-id="ed1a4-131">Starting C# code for web service consumption:</span></span>
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



<span data-ttu-id="ed1a4-132">Vstup je "Dnes je dobrý den."</span><span class="sxs-lookup"><span data-stu-id="ed1a4-132">The input is “Today is a good day.”</span></span> <span data-ttu-id="ed1a4-133">Výstup je "1", což označuje kladné postojích přidružené vstupní věty.</span><span class="sxs-lookup"><span data-stu-id="ed1a4-133">The output is “1”, which indicates a positive sentiment associated with the input sentence.</span></span> 

## <a name="creation-of-web-service"></a><span data-ttu-id="ed1a4-134">Vytvoření webové služby</span><span class="sxs-lookup"><span data-stu-id="ed1a4-134">Creation of web service</span></span>
> <span data-ttu-id="ed1a4-135">Této webové služby byl vytvořen pomocí Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="ed1a4-135">This web service was created using Azure Machine Learning.</span></span> <span data-ttu-id="ed1a4-136">Pro bezplatnou zkušební verzi, jakož i úvodní videa na vytváření experimentů a [publikování webových služeb](machine-learning-publish-a-machine-learning-web-service.md), najdete v tématu [azure.com/ml](http://azure.com/ml).</span><span class="sxs-lookup"><span data-stu-id="ed1a4-136">For a free trial, as well as introductory videos on creating experiments and [publishing web services](machine-learning-publish-a-machine-learning-web-service.md), please see [azure.com/ml](http://azure.com/ml).</span></span> <span data-ttu-id="ed1a4-137">Níže je snímek obrazovky experimentu, který vytvořené pro každou z modulů v rámci experimentu webové služby a příklad kódu.</span><span class="sxs-lookup"><span data-stu-id="ed1a4-137">Below is a screenshot of the experiment that created the web service and example code for each of the modules within the experiment.</span></span>
> 
> 

<span data-ttu-id="ed1a4-138">Z v rámci Azure Machine Learning, která vytvořila nový prázdný experiment.</span><span class="sxs-lookup"><span data-stu-id="ed1a4-138">From within Azure Machine Learning, a new blank experiment was created.</span></span> <span data-ttu-id="ed1a4-139">Následující obrázek znázorňuje experimentu toku na základě slovníkový postojích analýzy.</span><span class="sxs-lookup"><span data-stu-id="ed1a4-139">The figure below shows the experiment flow of lexicon-based sentiment analysis.</span></span> <span data-ttu-id="ed1a4-140">Soubor "sent_dict.csv" slovníkový míru MPQA a je nastavená jako jeden ze vstupních údajů z [spustit skript jazyka R][execute-r-script].</span><span class="sxs-lookup"><span data-stu-id="ed1a4-140">The “sent_dict.csv” file is the MPQA subjectivity lexicon, and is set as one of the inputs of [Execute R Script][execute-r-script].</span></span> <span data-ttu-id="ed1a4-141">Jiné vstup je kontrolu jen Vzorkovaná z datové sady zkontrolujte Amazon pro test, kde můžeme provést výběr, změna název sloupce a rozdělení operace.</span><span class="sxs-lookup"><span data-stu-id="ed1a4-141">Another input is a sampled review from the Amazon review dataset for test, where we performed selection, column name modification, and split operations.</span></span> <span data-ttu-id="ed1a4-142">Používáme balíček hash pro ukládání slovníkový míru v paměti a urychlit proces výpočtu skóre.</span><span class="sxs-lookup"><span data-stu-id="ed1a4-142">We use a hash package to store the subjectivity lexicon in the memory and accelerate the score computation process.</span></span> <span data-ttu-id="ed1a4-143">Celý text bude tokenizovaného "tm" balíčkem a porovná se slovem ve slovníku postojích.</span><span class="sxs-lookup"><span data-stu-id="ed1a4-143">The whole text will be tokenized by “tm” package and compared with the word in the sentiment dictionary.</span></span> <span data-ttu-id="ed1a4-144">Nakonec výpočtu skóre přidáním váhu jednotlivých subjektivní slov v textu.</span><span class="sxs-lookup"><span data-stu-id="ed1a4-144">Finally, a score will be calculated by adding the weight of each subjective word in the text.</span></span> 

### <a name="experiment-flow"></a><span data-ttu-id="ed1a4-145">Experiment toku:</span><span class="sxs-lookup"><span data-stu-id="ed1a4-145">Experiment flow:</span></span>
![Tok experimentu][2]

#### <a name="module-1"></a><span data-ttu-id="ed1a4-147">Modul 1:</span><span class="sxs-lookup"><span data-stu-id="ed1a4-147">Module 1:</span></span>
    # Map 1-based optional input ports to variables
    sent_dict_data<- maml.mapInputPort(1) # class: data.frame
    dataset2 <- maml.mapInputPort(2) # class: data.frame

# <a name="install-hash-package"></a><span data-ttu-id="ed1a4-148">Instalovat balíček hash</span><span class="sxs-lookup"><span data-stu-id="ed1a4-148">Install hash package</span></span>
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

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("data.set")



## <a name="limitations"></a><span data-ttu-id="ed1a4-149">Omezení</span><span class="sxs-lookup"><span data-stu-id="ed1a4-149">Limitations</span></span>
<span data-ttu-id="ed1a4-150">Z hlediska algoritmus je na základě slovníkový postojích analysis nástroj analysis obecné postojích, což lépe než metodu klasifikace pro konkrétních polí, nemusí fungovat.</span><span class="sxs-lookup"><span data-stu-id="ed1a4-150">From an algorithm perspective, lexicon-based sentiment analysis is a general sentiment analysis tool, which may not perform better than the classification method for specific fields.</span></span> <span data-ttu-id="ed1a4-151">Není dobře negace problém řešit.</span><span class="sxs-lookup"><span data-stu-id="ed1a4-151">The negation problem is not well dealt with.</span></span> <span data-ttu-id="ed1a4-152">Budeme používat pevné kódování několik negace slova v naší aplikaci, ale ještě lepší způsob je pomocí slovník negace a vytvořit některá pravidla.</span><span class="sxs-lookup"><span data-stu-id="ed1a4-152">We hardcode several negation words in our program, but a better way is using a negation dictionary and build some rules.</span></span> <span data-ttu-id="ed1a4-153">Webová služba provádí lépe krátký a jednoduché věty, jako je například tweetů a hraniční sítě Facebook, než na věty dlouhá a složitá, například Amazon recenze.</span><span class="sxs-lookup"><span data-stu-id="ed1a4-153">The web service performs better on short and simple sentences, such as tweets and Facebook posts, than on long and complex sentences such as Amazon reviews.</span></span> 

## <a name="faq"></a><span data-ttu-id="ed1a4-154">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="ed1a4-154">FAQ</span></span>
<span data-ttu-id="ed1a4-155">Nejčastější dotazy o spotřebě webové služby nebo publikování na webu Azure Marketplace naleznete v části [zde](machine-learning-marketplace-faq.md).</span><span class="sxs-lookup"><span data-stu-id="ed1a4-155">For frequently asked questions on consumption of the web service or publishing to the Azure Marketplace, see [here](machine-learning-marketplace-faq.md).</span></span>

[1]: ./media/machine-learning-r-csharp-lexicon-based-sentiment-analysis/sentiment_analysis_1.png
[2]: ./media/machine-learning-r-csharp-lexicon-based-sentiment-analysis/sentiment_analysis_2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/


