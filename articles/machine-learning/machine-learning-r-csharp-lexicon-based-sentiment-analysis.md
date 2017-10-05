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
# <a name="deprecated-lexicon-based-sentiment-analysis"></a>(nepoužívané) Slovníkový na základě analýzy postojích

> [!NOTE]
> Microsoft DataMarket se postupně vyřazuje z provozu a toto rozhraní API je zastaralá. 
> 
> Můžete najít mnoho užitečné příklad experimentů a rozhraní API v [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com). Další informace o galerii najdete v tématu [sdílené složky a zjišťovat prostředky ve Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).

Jak můžete budete vyhodnocovat názory a postojích směrem k značky nebo témata v online sociálních sítí, uživatelů, jako je Facebook odešle tweetů, recenzemi atd.? Analýza postojích poskytuje metodu pro analýzu takové dotazy.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Obecně existují dvě metody pro analýzu postojích. Jeden používá algoritmus učení pod dohledem a dalších lze považovat za learning bez dohledu. Model klasifikace algoritmu učení pod dohledem obecně staví na velké poznámkou souhrnu. Jeho přesnost je především založený na kvalitu anotace a bude školení proces obvykle trvat dlouhou dobu. Kromě toho, když jsme použít algoritmus do jiné domény, výsledek obvykle není funkční. Ve srovnání s pod dohledem učení, na základě slovníkový používá bez dohledu learning postojích slovník, který nevyžaduje ukládání velkého množství dat souhrnu a cvičení – což umožňuje mnohem rychlejší celého procesu. 

Naše [služby](https://datamarket.azure.com/dataset/aml_labs/lexicon_based_sentiment_analysis) je založený na míru slovníkový MPQA (http://mpqa.cs.pitt.edu/lexicons/subj_lexicon/), který je jednou z nejčastěji používaných míru lexikon. V MPQA nejsou 5097 záporné a 2533 kladné slova. A všechny tyto slova jsou označená poznámkou jako polarita velký nebo malý. Celý souhrnu se v rámci licence GNU General Public License. Webová služba je použít pro všechny krátké věty, jako je například tweetů a hraniční sítě Facebook. 

> Této webové služby, může být například spotřebován uživateli – potenciálně prostřednictvím mobilní aplikace prostřednictvím webu, nebo i v místním počítači. Ale účelem webové služby je také slouží jako příklad použití Azure Machine Learning k vytvoření webové služby v kódu jazyka R. Zadání několika řádků kódu jazyka R a klikne na tlačítko v rámci Azure Machine Learning Studio můžete experimentu vytvořené pomocí kódu jazyka R a publikovat jako webovou službu. Webové služby můžete pak publikování na webu Azure Marketplace a spotřebovávají uživatelů a zařízení po celém světě s žádné nastavení infrastruktury autorem webové služby.
> 
> 

## <a name="consumption-of-web-service"></a>Spotřeba webové služby
Vstupních dat může být jakýkoli text, ale webová služba funguje lépe s krátkou věty. Výstup je číselná hodnota od -1 a 1. Všechny hodnotu nižší než 0 označuje, že postojích textu je záporný; Pokud je kladné vyšší než 0. Absolutní hodnota výsledku označuje sílu přidružené postojích. 

> Tato služba jako hostované v Azure Marketplace je službou OData. To může být volána prostřednictvím metody POST nebo GET. 
> 
> 

Existuje více způsobů využívání služby automatizovaně (je třeba aplikace [sem](http://microsoftazuremachinelearning.azurewebsites.net/)).

### <a name="starting-c-code-for-web-service-consumption"></a>Spouštění kódu C# pro používání webové služby:
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



Vstup je "Dnes je dobrý den." Výstup je "1", což označuje kladné postojích přidružené vstupní věty. 

## <a name="creation-of-web-service"></a>Vytvoření webové služby
> Této webové služby byl vytvořen pomocí Azure Machine Learning. Pro bezplatnou zkušební verzi, jakož i úvodní videa na vytváření experimentů a [publikování webových služeb](machine-learning-publish-a-machine-learning-web-service.md), najdete v tématu [azure.com/ml](http://azure.com/ml). Níže je snímek obrazovky experimentu, který vytvořené pro každou z modulů v rámci experimentu webové služby a příklad kódu.
> 
> 

Z v rámci Azure Machine Learning, která vytvořila nový prázdný experiment. Následující obrázek znázorňuje experimentu toku na základě slovníkový postojích analýzy. Soubor "sent_dict.csv" slovníkový míru MPQA a je nastavená jako jeden ze vstupních údajů z [spustit skript jazyka R][execute-r-script]. Jiné vstup je kontrolu jen Vzorkovaná z datové sady zkontrolujte Amazon pro test, kde můžeme provést výběr, změna název sloupce a rozdělení operace. Používáme balíček hash pro ukládání slovníkový míru v paměti a urychlit proces výpočtu skóre. Celý text bude tokenizovaného "tm" balíčkem a porovná se slovem ve slovníku postojích. Nakonec výpočtu skóre přidáním váhu jednotlivých subjektivní slov v textu. 

### <a name="experiment-flow"></a>Experiment toku:
![Tok experimentu][2]

#### <a name="module-1"></a>Modul 1:
    # Map 1-based optional input ports to variables
    sent_dict_data<- maml.mapInputPort(1) # class: data.frame
    dataset2 <- maml.mapInputPort(2) # class: data.frame

# <a name="install-hash-package"></a>Instalovat balíček hash
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



## <a name="limitations"></a>Omezení
Z hlediska algoritmus je na základě slovníkový postojích analysis nástroj analysis obecné postojích, což lépe než metodu klasifikace pro konkrétních polí, nemusí fungovat. Není dobře negace problém řešit. Budeme používat pevné kódování několik negace slova v naší aplikaci, ale ještě lepší způsob je pomocí slovník negace a vytvořit některá pravidla. Webová služba provádí lépe krátký a jednoduché věty, jako je například tweetů a hraniční sítě Facebook, než na věty dlouhá a složitá, například Amazon recenze. 

## <a name="faq"></a>Nejčastější dotazy
Nejčastější dotazy o spotřebě webové služby nebo publikování na webu Azure Marketplace naleznete v části [zde](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-lexicon-based-sentiment-analysis/sentiment_analysis_1.png
[2]: ./media/machine-learning-r-csharp-lexicon-based-sentiment-analysis/sentiment_analysis_2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/


