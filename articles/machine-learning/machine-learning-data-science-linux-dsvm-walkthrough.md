---
title: "vědecké účely aaaData na hello Linux datové vědy virtuálního počítače | Microsoft Docs"
description: "Jak tooperform několik běžných vědecké zpracování dat úlohy s hello virtuálního počítače s Linuxem dat vědecké účely."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 34ef0b10-9270-474f-8800-eecb183bbce4
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: bradsev;paulsh
ms.openlocfilehash: 78764825f2e834fa4ddb7fdc2f59418dbe736e1d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="data-science-on-hello-linux-data-science-virtual-machine"></a>Vědecké zpracování dat na hello Linux datové vědy virtuálního počítače
Tento návod ukazuje, jak tooperform několik běžných vědecké zpracování dat úlohy s hello virtuálního počítače s Linuxem dat vědecké účely. Hello Linux datové vědy virtuálního počítače (DSVM) je bitová kopie virtuálního počítače, která je k dispozici v Azure, který je předem nainstalovaná s kolekcí nástrojů pro běžně používané k analýze dat a strojové učení. Hello klíčové softwarové součásti je uvedeno v hello [zřídit hello Linux datové vědy virtuálního počítače](machine-learning-data-science-linux-dsvm-intro.md) tématu. Hello image virtuálního počítače umožňuje snadno tooget spustit provádění vědecké zpracování dat v minutách, bez nutnosti tooinstall a každý z nástrojů hello nakonfigurovat jednotlivě. Můžete snadno škálovat hello virtuálních počítačů, v případě potřeby a zastavte ji když není používán. Proto tento prostředek je elastické a nákladově efektivní.

Hello ukázáno v tomto názorném postupu úloh vědecké účely dat postupujte podle hello kroků uvedených v hello [proces vědecké účely dat Team](https://azure.microsoft.com/documentation/learning-paths/data-science-process/). Tento proces zajišťuje toodata systematicky vědecké účely, které týmům datových vědců tooeffectively spolupracovat přes životního cyklu hello vytváření inteligentní aplikace. proces vědecké účely dat Hello také poskytuje iterativní framework vědecké zpracování dat, který může následovat jednotlivce.

Budeme analyzovat hello [spambase](https://archive.ics.uci.edu/ml/datasets/spambase) datovou sadu v tomto návodu. Je to sada e-mailů, které jsou označeny jako nevyžádané pošty nebo šunkou (tj. nejsou nevyžádané pošty), a také obsahuje statistikami o hello obsah e-mailů hello. statistiky Hello zahrnuté jsou popsané v hello vedle ale jeden oddíl.

## <a name="prerequisites"></a>Požadavky
Než budete moct použít virtuální počítač s Linuxem dat vědecké účely, musíte mít následující hello:

* **Předplatné**. Pokud není již nemáte, přečtěte si téma [vytvořit účet Azure zdarma Dnes](https://azure.microsoft.com/free/).
* A [ **Linux vědecké zpracování dat virtuálních počítačů**](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm). Informace o zřizování tohoto virtuálního počítače najdete v tématu [zřídit hello Linux datové vědy virtuální počítač](machine-learning-data-science-linux-dsvm-intro.md).
* [X2Go](http://wiki.x2go.org/doku.php) v počítači nainstalován a otevřít relaci XFCE. Informace o instalaci a konfiguraci **X2Go klienta**, najdete v části [instalace a konfigurace klienta X2Go](machine-learning-data-science-linux-dsvm-intro.md#installing-and-configuring-x2go-client). 
* **AzureML účet**. Pokud jste ještě nemáte, zaregistrujte si novým v hello [domovské stránky AzureML](https://studio.azureml.net/). Není toohelp vrstvy volné využití, které začít pracovat.

## <a name="download-hello-spambase-dataset"></a>Stáhnout hello spambase datové sady
Hello [spambase](https://archive.ics.uci.edu/ml/datasets/spambase) datové sady je poměrně malý sada dat, která obsahuje jenom 4601 příklady. To je vhodné velikost toouse při demonstraci toho, že některé klíčové funkce hello hello vědecké účely Data virtuálního počítače jako jeho uchovává požadavky na prostředky hello mírné.

> [!NOTE]
> Tento návod byl vytvořen na D2 v2 velikost datové vědy virtuální počítač s Linuxem. Tato velikost DSVM je schopná zpracovávat hello postupy v tomto návodu.
>
>

Pokud potřebujete další prostor úložiště, můžete vytvořit další disky a připojte je tooyour virtuálních počítačů. Tyto disky používají trvalé úložiště Azure, takže jejich uchování dat i v případě, že hello server je znovu poskytnuto kvůli tooresizing nebo je vypnutý. tooadd disku a jeho připojení tooyour virtuálních počítačů, postupujte podle pokynů hello v [přidat tooa disku virtuálního počítače s Linuxem](../virtual-machines/linux/add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Tyto kroky použijte rozhraní příkazového řádku Azure (Azure CLI), který je již nainstalován na hello DSVM hello. Proto tyto postupy lze provést zcela z hello virtuální počítač. Jiná možnost tooincrease úložiště je toouse [soubory Azure](../storage/files/storage-how-to-use-files-linux.md).

toodownload hello data, otevřete okno terminálu a spusťte tento příkaz:

    wget http://archive.ics.uci.edu/ml/machine-learning-databases/spambase/spambase.data

stažený soubor Hello nemá řádek záhlaví, takže vytvoříme jiný soubor, který má hlavičku. Spusťte tento příkaz toocreate soubor s příslušnou hlavičky hello:

    echo 'word_freq_make, word_freq_address, word_freq_all, word_freq_3d,word_freq_our, word_freq_over, word_freq_remove, word_freq_internet,word_freq_order, word_freq_mail, word_freq_receive, word_freq_will,word_freq_people, word_freq_report, word_freq_addresses, word_freq_free,word_freq_business, word_freq_email, word_freq_you, word_freq_credit,word_freq_your, word_freq_font, word_freq_000, word_freq_money,word_freq_hp, word_freq_hpl, word_freq_george, word_freq_650, word_freq_lab,word_freq_labs, word_freq_telnet, word_freq_857, word_freq_data,word_freq_415, word_freq_85, word_freq_technology, word_freq_1999,word_freq_parts, word_freq_pm, word_freq_direct, word_freq_cs, word_freq_meeting,word_freq_original, word_freq_project, word_freq_re, word_freq_edu,word_freq_table, word_freq_conference, char_freq_semicolon, char_freq_leftParen,char_freq_leftBracket, char_freq_exclamation, char_freq_dollar, char_freq_pound, capital_run_length_average,capital_run_length_longest, capital_run_length_total, spam' > headers

Potom řetězení hello společně s hello příkaz dva soubory:

    cat spambase.data >> headers
    mv headers spambaseHeaders.data

datovou sadu Hello má několik typů statistik na každou e-mailu:

* Sloupce jako ***word\_frekvence\_WORD*** znamenat hello procento slova v hello e-mailu, které odpovídají *WORD*. Například pokud *word\_frekvence\_zkontrolujte* je 1, 1 % všechna slova v e-mailu hello byly *zkontrolujte*.
* Sloupce jako ***char\_frekvence\_CHAR*** znamenat hello procento všechny znaky v hello e-mailu, které byly *CHAR*.
* ***kapitálové\_spustit\_délka\_nejdelší*** je hello nejdelší délka posloupnosti velkých písmen.
* ***kapitálové\_spustit\_délka\_průměrná*** je hello průměrnou délku všech pořadí velkých písmen.
* ***kapitálové\_spustit\_délka\_celkový*** je celková délka hello všech pořadí velkých písmen.
* ***nevyžádané pošty*** Určuje, zda e-mailu hello byl úvahy nevyžádané pošty nebo ne (1 = nevyžádané pošty, 0 = není nevyžádané poště).

## <a name="explore-hello-dataset-with-microsoft-r-open"></a>Prozkoumejte hello datovou sadu s Microsoft R otevřete
Umožňuje zkontrolovat hello data a provádět některé základní machine learning s R. hello vědecké účely dat virtuálních počítačů se dodává s [Microsoft R otevřete](https://mran.revolutionanalytics.com/open/) předinstalován. Hello matematické vícevláknové knihovny v této verzi R nabízí lepší výkon než různých verzí jednovláknové. Microsoft R otevřete také poskytuje reprodukovatelnosti pomocí snímku úložiště balíčků CRAN hello.

Ukázky v tomto návodu, klonování hello používá kódu tooget kopie hello **Azure-Machine-Learning--vědecké zpracování dat** úložiště pomocí git, který je předem nainstalovaná na hello virtuálních počítačů. Z příkazového řádku hello git spusťte příkaz:

    git clone https://github.com/Azure/Azure-MachineLearning-DataScience.git

Otevřete okno terminálu a spusťte novou relaci R s hello R interaktivní konzoly.

> [!NOTE]
> Můžete taky Rstudia pro hello následující postupy. tooinstall Rstudia, provedení tohoto příkazu v terminálu:`./Desktop/DSVM\ tools/installRStudio.sh`
>
>

tooimport hello data a nastavení prostředí hello, spusťte:

    data <- read.csv("spambaseHeaders.data")
    set.seed(123)

toosee souhrnné statistické údaje o jednotlivých sloupců:

    summary(data)

Pro jiné zobrazení dat hello:

    str(data)

Ukazuje to hello typ pro každou proměnnou a hello nejprve několik hodnot v datové sadě hello.

Hello *nevyžádané pošty* sloupec byl načten jako celé číslo, ale je ve skutečnosti kategorií proměnné (nebo multi-Factor). tooset typ:

    data$spam <- as.factor(data$spam)

toodo některé průzkumné analýzy, použijte hello [ggplot2](http://ggplot2.org/) balíček oblíbených grafovým knihovny pro R, který už je nainstalovaný na hello virtuálních počítačů. Poznámka od hello souhrnná data zobrazit dřív, máme souhrnné statistiky na hello frekvenci hello vykřičník znak. Umožňuje vykreslení těchto frekvencí zde s hello následující příkazy:

    library(ggplot2)
    ggplot(data) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)

Vzhledem k tomu, že hello nula panelu je zkosení hello výkresu, budeme se jich zbavit:

    email_with_exclamation = data[data$char_freq_exclamation > 0, ]
    ggplot(email_with_exclamation) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)

Je větší než 1, která vypadá zajímavé netriviální hustotu. Podívejme se na právě tato data:

    ggplot(data[data$char_freq_exclamation > 1, ]) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)

Potom ji rozdělte podle šunkou vs nevyžádané pošty:

    ggplot(data[data$char_freq_exclamation > 1, ], aes(x=char_freq_exclamation)) +
    geom_density(lty=3) +
    geom_density(aes(fill=spam, colour=spam), alpha=0.55) +
    xlab("spam") +
    ggtitle("Distribution of spam \nby frequency of !") +
    labs(fill="spam", y="Density")

Tyto příklady by měl umožňují toomake podobné pozemků Dobrý den dalších sloupců tooexplore hello data obsažená v nich.

## <a name="train-and-test-an-ml-model"></a>Natrénuje a otestuje ML model
Nyní Pojďme train několika machine learning modelů tooclassify hello e-mailů v datové sadě hello jako obsahující buď span nebo šunkou. Jsme učení rozhodovacího stromu modelu a modelu náhodných doménové struktury v této části a pak testování jejich přesnost své předpovědi.

> [!NOTE]
> balíček rpart (rekurzivní vytváření oddílů a regresní stromy) Hello používá v hello následující kód je již nainstalován na hello vědecké účely dat virtuálních počítačů.
>
>

První můžeme rozdělit hello datové sady na školení a testovací sady:

    rnd <- runif(dim(data)[1])
    trainSet = subset(data, rnd <= 0.7)
    testSet = subset(data, rnd > 0.7)

A pak vytvořte rozhodovací strom tooclassify hello e-mailů.

    require(rpart)
    model.rpart <- rpart(spam ~ ., method = "class", data = trainSet)
    plot(model.rpart)
    text(model.rpart)

Tady je výsledek hello:

![1](./media/machine-learning-data-science-linux-dsvm-walkthrough/decision-tree.png)

toodetermine její výkon na školení hello nastavení, použijte hello následující kód:

    trainSetPred <- predict(model.rpart, newdata = trainSet, type = "class")
    t <- table(`Actual Class` = trainSet$spam, `Predicted Class` = trainSetPred)
    accuracy <- sum(diag(t))/sum(t)
    accuracy

toodetermine jak dobře provádí na testovací sada hello:

    testSetPred <- predict(model.rpart, newdata = testSet, type = "class")
    t <- table(`Actual Class` = testSet$spam, `Predicted Class` = testSetPred)
    accuracy <- sum(diag(t))/sum(t)
    accuracy

Zkuste taky umožňuje model náhodných doménové struktury. Náhodné doménovými strukturami cvičení velkého množství rozhodovací stromy a výstup třídu, která hello režim hello klasifikace ze všech jednotlivých rozhodovací stromy hello. Poskytují účinnější strojového učení a přístup podle jejich odstraňte hello tendence rozhodovacího stromu modelu toooverfit školení datové sady.

    require(randomForest)
    trainVars <- setdiff(colnames(data), 'spam')
    model.rf <- randomForest(x=trainSet[, trainVars], y=trainSet$spam)

    trainSetPred <- predict(model.rf, newdata = trainSet[, trainVars], type = "class")
    table(`Actual Class` = trainSet$spam, `Predicted Class` = trainSetPred)

    testSetPred <- predict(model.rf, newdata = testSet[, trainVars], type = "class")
    t <- table(`Actual Class` = testSet$spam, `Predicted Class` = testSetPred)
    accuracy <- sum(diag(t))/sum(t)
    accuracy


## <a name="deploy-a-model-tooazure-ml"></a>Nasazení tooAzure modelu ML
[Azure Machine Learning Studio](https://studio.azureml.net/) (AzureML) je Cloudová služba, která umožňuje snadno toobuild a nasadit řešení prediktivní analýzy. Jednou z funkcí hello dobrý AzureML je jeho možnost toopublish žádné R fungovat jako webovou službu. Hello balíček AzureML R vytváří snadno toodo nasazení přímo naší relace R na hello DSVM.

toodeploy hello rozhodovací strom kód z předchozí části hello, musíte toosign v tooAzure Machine Learning Studio. Potřebujete ID vašeho pracovního prostoru a tokenu toosign autorizace v. toofind tyto hodnoty a proměnné AzureML hello inicializovat s nimi:

Vyberte **nastavení** v levé nabídce hello. Poznámka: vaše **ID pracovního prostoru**. ![2](./media/machine-learning-data-science-linux-dsvm-walkthrough/workspace-id.png)

Vyberte **autorizace tokeny** z nabídky režijní hello a Poznámka vaše **primární autorizační Token**.![ 3](./media/machine-learning-data-science-linux-dsvm-walkthrough/workspace-token.png)

Zatížení hello **AzureML** balíček a potom nastavit hodnoty proměnných hello se vaše ID token a pracovního prostoru v relaci prostředí R na hello DSVM:

    require(AzureML)
    wsAuth = "<authorization-token>"
    wsID = "<workspace-id>"


Umožňuje zjednodušit hello modelu toomake jednodušší tooimplement této ukázce. Vyberte hello tří proměnných v hello rozhodovací strom nejbližší toohello kořenové a vytvořit novou větev pomocí právě těchto tří proměnných:

    colNames <- c("char_freq_dollar", "word_freq_remove", "word_freq_hp", "spam")
    smallTrainSet <- trainSet[, colNames]
    smallTestSet <- testSet[, colNames]
    model.rpart <- rpart(spam ~ ., method = "class", data = smallTrainSet)

Potřebujeme předpovědi funkce, která přebírá hello funkce jako vstup a vrátí hello předpovězené hodnoty:

    predictSpam <- function(char_freq_dollar, word_freq_remove, word_freq_hp) {
        predictDF <- predict(model.rpart, data.frame("char_freq_dollar" = char_freq_dollar,
        "word_freq_remove" = word_freq_remove, "word_freq_hp" = word_freq_hp))
        return(colnames(predictDF)[apply(predictDF, 1, which.max)])
    }

Publikování hello predictSpam funkce tooAzureML pomocí hello **publishWebService** funkce:

    spamWebService <- publishWebService("predictSpam",
        "spamWebService",
        list("char_freq_dollar"="float", "word_freq_remove"="float","word_freq_hp"="float"),
        list("spam"="int"),
        wsID, wsAuth)

Tato funkce přebírá hello **predictSpam** fungovat, vytvoří webové služby s názvem **spamWebService** s definované vstupy a výstupy a vrátí informace o nový koncový bod hello.

Zobrazení podrobností hello publikované webové služby, včetně jeho klíče rozhraní API koncového bodu a přístup pomocí příkazu hello:

    spamWebService[[2]]

tootry ho na hello prvních 10 řádků hello testovací sada:

    consumeDataframe(spamWebService$endpoints[[1]]$PrimaryKey, spamWebService$endpoints[[1]]$ApiLocation, smallTestSet[1:10, 1:3])


## <a name="use-other-tools-available"></a>Použít jiné nástroje, které jsou k dispozici
Hello zbývající části ukazují, jak hello toouse některé nástroje hello nainstalovat na virtuální počítač s Linuxem dat vědecké účely. Tady je hello seznam nástrojů, které jsou popsané:

* XGBoost
* Python
* Jupyterhub
* Rattle
* PostgreSQL & Squirrel SQL
* SQL Server datového skladu

## <a name="xgboost"></a>XGBoost
[XGBoost](https://xgboost.readthedocs.org/en/latest/) je nástroj, který poskytuje rychlé a přesné boosted stromu implementaci.

    require(xgboost)
    data <- read.csv("spambaseHeaders.data")
    set.seed(123)

    rnd <- runif(dim(data)[1])
    trainSet = subset(data, rnd <= 0.7)
    testSet = subset(data, rnd > 0.7)

    bst <- xgboost(data = data.matrix(trainSet[,0:57]), label = trainSet$spam, nthread = 2, nrounds = 2, objective = "binary:logistic")

    pred <- predict(bst, data.matrix(testSet[, 0:57]))
    accuracy <- 1.0 - mean(as.numeric(pred > 0.5) != testSet$spam)
    print(paste("test accuracy = ", accuracy))

XGBoost můžete také volat z pythonu nebo příkazového řádku.

## <a name="python"></a>Python
Pro vývoj pomocí Python se nainstalovaly v hello DSVM hello Anaconda Python distribuce 2.7 a 3.5.

> [!NOTE]
> zahrnuje Hello distribuční Anaconda [Condas](http://conda.pydata.org/docs/index.html), což může být použité toocreate vlastní prostředí pro jazyk Python, které mají různé verze nebo balíčky nainstalované v nich.
>
>

Umožňuje číst v některých hello spambase datovou sadu a klasifikovat hello e-mailů s support vector počítačů v scikit-Další informace:

    import pandas
    from sklearn import svm    
    data = pandas.read_csv("spambaseHeaders.data", sep = ',\s*')
    X = data.ix[:, 0:57]
    y = data.ix[:, 57]
    clf = svm.SVC()
    clf.fit(X, y)

toomake předpovědi:

    clf.predict(X.ix[0:20, :])

tooshow jak toopublish koncového bodu AzureML umožňuje zpřístupnění jednodušší model hello tří proměnných, jako jsme to udělali při jsme dříve publikované hello R modelu.

    X = data.ix[["char_freq_dollar", "word_freq_remove", "word_freq_hp"]]
    y = data.ix[:, 57]
    clf = svm.SVC()
    clf.fit(X, y)

toopublish hello modelu tooAzureML:

    # Publish hello model.
    workspace_id = "<workspace-id>"
    workspace_token = "<workspace-token>"
    from azureml import services
    @services.publish(workspace_id, workspace_token)
    @services.types(char_freq_dollar = float, word_freq_remove = float, word_freq_hp = float)
    @services.returns(int) # 0 or 1
    def predictSpam(char_freq_dollar, word_freq_remove, word_freq_hp):
        inputArray = [char_freq_dollar, word_freq_remove, word_freq_hp]
        return clf.predict(inputArray)

    # Get some info about hello resulting model.
    predictSpam.service.url
    predictSpam.service.api_key

    # Call hello model
    predictSpam.service(1, 1, 1)

> [!NOTE]
> To je dostupná jenom pro python 2.7 a není dosud nepodporováno u 3.5. Spustit s **/anaconda/bin/python2.7**.
>
>

## <a name="jupyterhub"></a>Jupyterhub
distribuce Anaconda Hello v hello DSVM se dodává s poznámkového bloku Jupyter, kód pro Python, R nebo Dita tooshare prostředí napříč platformami a analýzy. Poznámkový blok Jupyter Hello je přístupné přes JupyterHub. Přihlášení pomocí místních Linux uživatelské jméno a heslo v ***https://\<název DNS virtuálního počítače nebo IP adresu\>: 8000 /***. Všechny konfigurační soubory pro JupyterHub se nacházejí v adresáři **/etc/jupyterhub**.

Několik ukázkových poznámkových bloků jsou již nainstalovány na hello virtuálních počítačů:

* V tématu hello [IntroToJupyterPython.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IntroToJupyterPython.ipynb) pro ukázkové Python Poznámkový blok.
* V tématu [IntroTutorialinR](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IntroTutorialinR.ipynb) pro ukázku **R** poznámkového bloku.
* V tématu hello [IrisClassifierPyMLWebService](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IrisClassifierPyMLWebService.ipynb) pro jiné ukázku **Python** poznámkového bloku.

> [!NOTE]
> Dále je dostupný z příkazového řádku hello na hello virtuálního počítače s Linuxem datové vědy Hello Dita jazyk.
>
>

## <a name="rattle"></a>Rattle
[Rattle](https://cran.r-project.org/web/packages/rattle/index.html) (hello Analytical nástroj R tooLearn snadno) je grafický nástroj pro dolování dat R. Obsahuje intuitivní rozhraní, které umožňuje snadno tooload, prozkoumat a transformovat data a vytvářet a vyhodnocení modelů.  článek Hello [Rattle: A Data Mining grafického uživatelského rozhraní pro R](https://journal.r-project.org/archive/2009-2/RJournal_2009-2_Williams.pdf) poskytuje návod, který ukazuje její funkce.

Instalace a spuštění Rattle s hello následující příkazy:

    if(!require("rattle")) install.packages("rattle")
    require(rattle)
    rattle()

> [!NOTE]
> Není nutné instalovat na hello DSVM. Ale Rattle můžete být vyzváni další balíčky tooinstall až ho načte.
>
>

Rattle používá rozhraní založené na kartě. Většina hello karty odpovídají toosteps v hello [proces vědecké účely dat](https://azure.microsoft.com/documentation/learning-paths/data-science-process/), jako jsou načítání dat nebo zkoumat ho. proces vědecké účely dat Hello toky z levé tooright prostřednictvím karty hello. Ale hello poslední karta obsahuje protokolu hello R příkazy spustit Rattle.

tooload a nakonfigurujte datovou sadu hello:

* tooload hello soubor, vyberte hello **Data** kartě, pak
* Zvolte hello selektor další příliš**Filename** a zvolte **spambaseHeaders.data**.
* soubor tooload hello. Vyberte **Execute** v horní řádek hello tlačítek. Měli byste vidět souhrn jednotlivých sloupců, včetně jeho identifikovaných datového typu, ať už se jedná o vstup, cíl nebo jiný typ proměnné a hello počet jedinečných hodnot.
* Rattle má identifikovány správně hello **nevyžádané pošty** sloupec jako cíl hello. Vyberte hello nevyžádané pošty sloupec a potom sadu hello **cílový datový typ** příliš**Categoric**.

tooexplore hello data:

* Vyberte hello **prozkoumat** kartě.
* Klikněte na tlačítko **souhrnné**, pak **Execute**, toosee některé informace o hello typy proměnných a některé souhrnné statistiky.
* tooview jiné typy Statistika každou proměnnou, vyberte jiné možnosti jako **popisujících** nebo **Základy**.

Hello **prozkoumat** karta vám umožní toogenerate mnoho pronikavého ukazuje zeměpisný. tooplot histogram hello dat:

* Vyberte **distribuce**.
* Zkontrolujte **Histogram** pro **word_freq_remove** a **word_freq_you**.
* Vyberte **provést**. Měli byste vidět, že oba hustotu ukazuje zeměpisný v okně jednoho grafu, pokud je zřejmé, že hello slovo "vy" se zobrazí mnohem častěji v e-mailů než "Odebrat".

Hello korelace pozemků jsou také zajímavé. toocreate jeden:

* Zvolte **korelace** jako hello **typu**, pak
* Vyberte **provést**.
* Rattle vás varuje, že doporučí maximálně 40 proměnné. Vyberte **Ano** tooview hello vykreslení.

Existují některé zajímavé korelací, které se spustit: "technologie" důrazně korelační příliš "HP" a "labs", např. Je také důrazně korelační příliš "650", protože je 650 hello směrové číslo oblasti dárců hello datovou sadu.

v okně Explore hello jsou k dispozici Hello číselné hodnoty hello korelací mezi slovy. Je zajímavé toonote, například, že je "technologie" negativní korelační s "vaše" a "peníze".

Rattle můžete převést hello datovou sadu toohandle některé běžné problémy. Například, ale umožňuje vám toorescale funkce, dává chybějící hodnoty, zpracování extrémních a odebrat proměnné nebo připomínky s chybějící data. Rattle můžete také určit pravidla přidružení mezi připomínky nebo proměnné. Tyto karty jsou nad rámec této úvodní prohlídka.

Rattle můžete také provést analýzu clusteru. Umožňuje vyloučit některé funkce toomake hello výstup jednodušší tooread. Na hello **Data** , zvolte **Ignorovat** další tooeach hello proměnných s výjimkou těchto deset položek:

* word_freq_hp
* word_freq_technology
* word_freq_george
* word_freq_remove
* word_freq_your
* word_freq_dollar
* word_freq_money
* capital_run_length_longest
* word_freq_business
* nevyžádané pošty

Potom přejděte zpět toohello **clusteru** , zvolte **KMeans**a sadu hello *počtu clusterů, které* too4. Potom **provést**. v okně výstupu hello se nezobrazí výsledky Hello. Jeden cluster má vysoká frekvence "Jirka" a "hp" a je pravděpodobně legitimní firemní e-mail.

toobuild jednoduché rozhodovací strom strojové učení modelu:

* Vyberte hello **modelu** kartě
* Zvolte **stromu** jako hello **typu**.
* Vyberte **Execute** toodisplay hello stromu v textové podobě v hello výstup okna.
* Vyberte hello **kreslení** tlačítko tooview grafické verze. To vypadá velmi podobné stromu toohello jsme získali dříve pomocí *rpart*.

Jednou z funkcí hello dobrý Rattle je jeho možnost toorun metody několik strojové učení a rychle vyhodnotit jejich. Tady je postup hello:

* Zvolte **všechny** pro hello **typu**.
* Vyberte **provést**.
* Po jejím dokončení můžete kliknout na všechny jedním **typ**, například **SVM**a zobrazit výsledky hello.
* Také můžete porovnat výkon hello hello modely na ověření hello nastavit pomocí hello **Evaluate** kartě. Například hello **chyba matice** výběr uvádí hello nedorozuměním matice, celkový chyby a chyba zprůměrovanou třídy pro každý model hello ověření sadu.
* Můžete také vykreslení křivek ROC, provádět analýzy velkých a malých písmen a dělat jiné typy modelu hodnocení.

Jakmile budete hotovi, vytváření modelů, vyberte hello **protokolu** kartě kód hello R tooview spustit Rattle během relace. Můžete vybrat hello **exportovat** toosave tlačítko ji.

> [!NOTE]
> V aktuální verzi Rattle je chyba. toomodify hello skriptu nebo ho používat toorepeat vaše kroky později, je třeba vložit znak # před * exportovat tento protokol... * v textu hello hello protokolu.
>
>

## <a name="postgresql--squirrel-sql"></a>PostgreSQL & Squirrel SQL
Hello DSVM se dodává s PostgreSQL nainstalována. PostgreSQL je relační databáze sofistikované, open source. Tato část uvádí, jak tooload naše spamu datové sady do PostgreSQL a pak zadat dotaz.

Před načtením dat hello, je nutné tooallow ověřování hesla z hello localhost. Na příkazovém řádku:

    sudo gedit /var/lib/pgsql/data/pg_hba.conf

Téměř hello dolní části hello konfiguračním souboru se několik řádků, které podrobností hello povoleno připojení:

    # "local" is for Unix domain socket connections only
    local   all             all                                     trust
    # IPv4 local connections:
    host    all             all             127.0.0.1/32            ident
    # IPv6 local connections:
    host    all             all             ::1/128                 ident

Změna hello "IPv4 místní připojení" řádku toouse md5 místo ident, takže jsme umožní přihlásit se pomocí uživatelského jména a hesla:

    # IPv4 local connections:
    host    all             all             127.0.0.1/32            md5

A restartujte službu postgres hello:

    sudo systemctl restart postgresql

toolaunch psql, interaktivní terminálu PostgreSQL jako uživatel předdefinované postgres hello, spusťte následující příkaz z řádku hello:

    sudo -u postgres psql

Vytvořit nový uživatelský účet, pomocí hello stejné uživatelské jméno jako hello Linux účet, který jste aktuálně přihlášeni jako a zadejte pro něj heslo:

    CREATE USER <username> WITH CREATEDB;
    CREATE DATABASE <username>;
    ALTER USER <username> password '<password>';
    \quit

Znovu přihlásili toopsql jako vaše uživatele:

    psql

A importovat hello data do nové databáze:

    CREATE DATABASE spam;
    \c spam
    CREATE TABLE data (word_freq_make real, word_freq_address real, word_freq_all real, word_freq_3d real,word_freq_our real, word_freq_over real, word_freq_remove real, word_freq_internet real,word_freq_order real, word_freq_mail real, word_freq_receive real, word_freq_will real,word_freq_people real, word_freq_report real, word_freq_addresses real, word_freq_free real,word_freq_business real, word_freq_email real, word_freq_you real, word_freq_credit real,word_freq_your real, word_freq_font real, word_freq_000 real, word_freq_money real,word_freq_hp real, word_freq_hpl real, word_freq_george real, word_freq_650 real, word_freq_lab real,word_freq_labs real, word_freq_telnet real, word_freq_857 real, word_freq_data real,word_freq_415 real, word_freq_85 real, word_freq_technology real, word_freq_1999 real,word_freq_parts real, word_freq_pm real, word_freq_direct real, word_freq_cs real, word_freq_meeting real,word_freq_original real, word_freq_project real, word_freq_re real, word_freq_edu real,word_freq_table real, word_freq_conference real, char_freq_semicolon real, char_freq_leftParen real,char_freq_leftBracket real, char_freq_exclamation real, char_freq_dollar real, char_freq_pound real, capital_run_length_average real, capital_run_length_longest real, capital_run_length_total real, spam integer);
    \copy data FROM /home/<username>/spambase.data DELIMITER ',' CSV;
    \quit

Teď umožňuje procházet hello data a spustit některé dotazy pomocí **Squirrel SQL**, grafický nástroj, který umožňuje pracovat s databází prostřednictvím ovladač JDBC.

tooget spuštěna, spusťte Squirrel SQL z nabídky aplikace hello. tooset až hello ovladače:

* Vyberte **Windows**, pak **zobrazit ovladače**.
* Klikněte pravým tlačítkem na **PostgreSQL** a vyberte **upravit ovladač**.
* Vyberte **velmi třídy cesta**, pak **přidat**.
* Zadejte ***/usr/share/java/jdbcdrivers/postgresql-9.4.1208.jre6.jar*** pro hello **název souboru** a
* Vyberte **otevřete**.
* Vyberte ovladače seznamu a pak vyberte **org.postgresql.Driver** v **název třídy**a vyberte **OK**.

tooset hello připojení toohello místní server:

* Vyberte **Windows**, pak **zobrazit aliasy.**
* Zvolte hello  **+**  toomake tlačítko Nový alias.
* Pojmenujte ji *nevyžádané pošty databáze*, zvolte **PostgreSQL** v hello **ovladač** rozevíracího seznamu.
* Nastavení adresy URL hello příliš*jdbc:postgresql://localhost/spam*.
* Zadejte vaše *uživatelské jméno* a *heslo*.
* Klikněte na **OK**.
* tooopen hello **připojení** okna, klikněte dvakrát na hello ***nevyžádané pošty databáze*** alias.
* Vyberte **Connect** (Připojit).

toorun některé dotazy:

* Vyberte hello **SQL** kartě.
* Například zadejte jednoduchý dotaz `SELECT * from data;` v textovém poli dotazu hello hello horní části karty SQL hello.
* Stiskněte klávesu **zadejte Ctrl** toorun ho. Ve výchozím nastavení Squirrel SQL vrátí hello prvních 100 řádků z dotazu.

Nejsou k dispozici mnoho další dotazy, že můžete spustit tooexplore tato data. Například jak funguje hello frekvence hello slov *zkontrolujte* liší nevyžádané pošty se šunkou?

    SELECT avg(word_freq_make), spam from data group by spam;

Nebo jaké jsou vlastnosti hello e-mailů, které často obsahují *3d*?

    SELECT * from data order by word_freq_3d desc;

Většina e-mailů, které mají vysokou výskytem *3d* jsou zjevně nevyžádané pošty, tak může být užitečná pro tvorbu prediktivního modelu tooclassify hello e-mailů.

Pokud jste chtěli tooperform machine learning s daty uloženými v databázi PostgreSQL, zvažte použití [MADlib](http://madlib.incubator.apache.org/).

## <a name="sql-server-data-warehouse"></a>SQL Server datového skladu
Azure SQL Data Warehouse je cloudová, škálovatelná databáze, která dokáže zpracovávat ohromné objemy dat, relačních i nerelačních. Další informace najdete v tématu [co je Azure SQL Data Warehouse?](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md)

tooconnect toohello datového skladu a vytvořte hello tabulky hello spusťte následující příkaz z příkazového řádku:

    sqlcmd -S <server-name>.database.windows.net -d <database-name> -U <username> -P <password> -I

Pak v příkazovém řádku sqlcmd hello:

    CREATE TABLE spam (word_freq_make real, word_freq_address real, word_freq_all real, word_freq_3d real,word_freq_our real, word_freq_over real, word_freq_remove real, word_freq_internet real,word_freq_order real, word_freq_mail real, word_freq_receive real, word_freq_will real,word_freq_people real, word_freq_report real, word_freq_addresses real, word_freq_free real,word_freq_business real, word_freq_email real, word_freq_you real, word_freq_credit real,word_freq_your real, word_freq_font real, word_freq_000 real, word_freq_money real,word_freq_hp real, word_freq_hpl real, word_freq_george real, word_freq_650 real, word_freq_lab real,word_freq_labs real, word_freq_telnet real, word_freq_857 real, word_freq_data real,word_freq_415 real, word_freq_85 real, word_freq_technology real, word_freq_1999 real,word_freq_parts real, word_freq_pm real, word_freq_direct real, word_freq_cs real, word_freq_meeting real,word_freq_original real, word_freq_project real, word_freq_re real, word_freq_edu real,word_freq_table real, word_freq_conference real, char_freq_semicolon real, char_freq_leftParen real,char_freq_leftBracket real, char_freq_exclamation real, char_freq_dollar real, char_freq_pound real, capital_run_length_average real, capital_run_length_longest real, capital_run_length_total real, spam integer) WITH (CLUSTERED COLUMNSTORE INDEX, DISTRIBUTION = ROUND_ROBIN);
    GO

Kopírování dat pomocí bcp:

    bcp spam in spambaseHeaders.data -q -c -t  ',' -S <server-name>.database.windows.net -d <database-name> -U <username> -P <password> -F 1 -r "\r\n"

> [!NOTE]
> konce řádků Hello ve staženém souboru hello jsou styl systému Windows, ale bcp očekává stylu systému UNIX, takže potřebujeme tootell bcp, která s hello příznak - r.
>
>

A dotazování pomocí sqlcmd:

    select top 10 spam, char_freq_dollar from spam;
    GO

Také můžete dotazovat pomocí Squirrel SQL. Podobný postup PostgreSQL, hello ovladač JDBC MSSQL serveru Microsoft, pomocí kterého lze nalézt v ***/usr/share/java/jdbcdrivers/sqljdbc42.jar***.

## <a name="next-steps"></a>Další kroky
Přehled témata, která vás provede procesem hello úlohy, které tvoří hello procesu vědecké zpracování dat v Azure najdete v tématu [proces vědecké účely dat Team](http://aka.ms/datascienceprocess).

Popis dalších návody začátku do konce, které ukazují postup hello v hello proces vědecké účely Team dat u konkrétních scénářů najdete v tématu [proces vědecké účely dat Team návody](data-science-process-walkthroughs.md). návody Hello také ilustrují, jak toocombine cloudové a místní nástrojů a služeb do pracovního postupu nebo kanálu toocreate inteligentního aplikace.
