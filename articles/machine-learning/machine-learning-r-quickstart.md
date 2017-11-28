---
title: kurz aaaQuickstart pro jazyk R pro Machine Learning | Microsoft Docs
description: "Pomocí této R programování kurz tooget rychle začít s používáním hello R jazyk s Azure Machine Learning Studio toocreate prognózy řešení."
keywords: "Rychlý start, jazyk r, programovací jazyk r, programovací kurzu r"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 99a3a0fd-b359-481a-b236-66868deccd96
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: garye
ms.openlocfilehash: 9995f8728f4d7bf9a5c15412015e4cf769cdac96
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="quickstart-tutorial-for-hello-r-programming-language-for-azure-machine-learning"></a><span data-ttu-id="3f44d-104">Rychlý úvodní kurz pro hello R programovací jazyk pro Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="3f44d-104">Quickstart tutorial for hello R programming language for Azure Machine Learning</span></span>

<!-- Stephen F Elston, Ph.D. -->

## <a name="introduction"></a><span data-ttu-id="3f44d-105">Úvod</span><span class="sxs-lookup"><span data-stu-id="3f44d-105">Introduction</span></span>
<span data-ttu-id="3f44d-106">Tento rychlý úvodní kurz vám pomůže rychle spustit rozšíření Azure Machine Learning pomocí programovacího jazyka hello R.</span><span class="sxs-lookup"><span data-stu-id="3f44d-106">This quickstart tutorial helps you quickly start extending Azure Machine Learning by using hello R programming language.</span></span> <span data-ttu-id="3f44d-107">Postupujte podle tohoto R programování kurz toocreate, testování a spouštění R kódu v rámci Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="3f44d-107">Follow this R programming tutorial toocreate, test and execute R code within Azure Machine Learning.</span></span> <span data-ttu-id="3f44d-108">Při práci prostřednictvím kurzu vytvoříte pomocí jazyka hello R v Azure Machine Learning kompletního řešení prognózy.</span><span class="sxs-lookup"><span data-stu-id="3f44d-108">As you work through tutorial, you will create a complete forecasting solution by using hello R language in Azure Machine Learning.</span></span>  

<span data-ttu-id="3f44d-109">Microsoft Azure Machine Learning obsahuje mnoho výkonné machine learning a data moduly manipulaci s.</span><span class="sxs-lookup"><span data-stu-id="3f44d-109">Microsoft Azure Machine Learning contains many powerful machine learning and data manipulation modules.</span></span> <span data-ttu-id="3f44d-110">jazyk R výkonné Hello popsán jako hello lingua franca obchodu Analytics.</span><span class="sxs-lookup"><span data-stu-id="3f44d-110">hello powerful R language has been described as hello lingua franca of analytics.</span></span> <span data-ttu-id="3f44d-111">Manipulace s analytickými funkcemi a data v Azure Machine Learning brouka, lze rozšířit pomocí R. Tato kombinace poskytne hello škálovatelnost a snadné nasazení Azure Machine Learning hello flexibilitu a hloubkové analytics R.</span><span class="sxs-lookup"><span data-stu-id="3f44d-111">Happily, analytics and data manipulation in Azure Machine Learning can be extended by using R. This combination provides hello scalability and ease of deployment of Azure Machine Learning with hello flexibility and deep analytics of R.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

### <a name="forecasting-and-hello-dataset"></a><span data-ttu-id="3f44d-112">Vytváření prognóz a hello datové sady</span><span class="sxs-lookup"><span data-stu-id="3f44d-112">Forecasting and hello dataset</span></span>
<span data-ttu-id="3f44d-113">Prognózy je metoda analytical široce zaměstnání a velmi užitečné.</span><span class="sxs-lookup"><span data-stu-id="3f44d-113">Forecasting is a widely employed and quite useful analytical method.</span></span> <span data-ttu-id="3f44d-114">Běžné používá rozsah od predikci prodeje sezónní položek, určení úrovní optimální inventáře, toopredicting makroekonomické proměnné.</span><span class="sxs-lookup"><span data-stu-id="3f44d-114">Common uses range from predicting sales of seasonal items, determining optimal inventory levels, toopredicting macroeconomic variables.</span></span> <span data-ttu-id="3f44d-115">Prognózy se většinou děje s modely časové řady.</span><span class="sxs-lookup"><span data-stu-id="3f44d-115">Forecasting is typically done with time series models.</span></span>

<span data-ttu-id="3f44d-116">Časových řad dat jsou data ve kterém hello hodnoty mají čas index.</span><span class="sxs-lookup"><span data-stu-id="3f44d-116">Time series data is data in which hello values have a time index.</span></span> <span data-ttu-id="3f44d-117">index Hello čas může být pravidelné, např. každý měsíc nebo každou minutu nebo nestandardní.</span><span class="sxs-lookup"><span data-stu-id="3f44d-117">hello time index can be regular, e.g. every month or every minute, or irregular.</span></span> <span data-ttu-id="3f44d-118">Model časové řady podle data časové řady.</span><span class="sxs-lookup"><span data-stu-id="3f44d-118">A time series model is based on time series data.</span></span> <span data-ttu-id="3f44d-119">programovací jazyk Hello R obsahuje flexibilní framework a rozsáhlé analýzy pro data časové řady.</span><span class="sxs-lookup"><span data-stu-id="3f44d-119">hello R programming language contains a flexible framework and extensive analytics for time series data.</span></span>

<span data-ttu-id="3f44d-120">V této příručce rychlý start jsme bude pracovat s produkční dojnic kalifornské a ceny data.</span><span class="sxs-lookup"><span data-stu-id="3f44d-120">In this quickstart guide we will be working with California dairy production and pricing data.</span></span> <span data-ttu-id="3f44d-121">Tato data zahrnují měsíční informace o provozním hello mléčných produktů a hello cena mléka fat komoditním srovnávacího testu.</span><span class="sxs-lookup"><span data-stu-id="3f44d-121">This data includes monthly information on hello production of several dairy products and hello price of milk fat, a benchmark commodity.</span></span>

<span data-ttu-id="3f44d-122">data Hello používané v tomto článku, spolu se skripty R, mohou být [stáhnout zde][download].</span><span class="sxs-lookup"><span data-stu-id="3f44d-122">hello data used in this article, along with R scripts, can be [downloaded here][download].</span></span> <span data-ttu-id="3f44d-123">Tato data byla původně syntetizován z informací dostupných ze hello univerzity Wisconsin v http://future.aae.wisc.edu/tab/production.html.</span><span class="sxs-lookup"><span data-stu-id="3f44d-123">This data was originally synthesized from information available from hello University of Wisconsin at http://future.aae.wisc.edu/tab/production.html.</span></span>

### <a name="organization"></a><span data-ttu-id="3f44d-124">Organizace</span><span class="sxs-lookup"><span data-stu-id="3f44d-124">Organization</span></span>
<span data-ttu-id="3f44d-125">Jsme proběhne prostřednictvím několik kroků, jak se dozvíte, jak toocreate, testování a spouštění kódu manipulaci s R analytics a data v prostředí Azure Machine Learning hello.</span><span class="sxs-lookup"><span data-stu-id="3f44d-125">We will progress through several steps as you learn how toocreate, test and execute analytics and data manipulation R code in hello Azure Machine Learning environment.</span></span>  

* <span data-ttu-id="3f44d-126">Nejprve se podíváme na hello základy používání hello R jazyk v prostředí Azure Machine Learning Studio hello.</span><span class="sxs-lookup"><span data-stu-id="3f44d-126">First we will explore hello basics of using hello R language in hello Azure Machine Learning Studio environment.</span></span>
* <span data-ttu-id="3f44d-127">Potom jsme průběhu toodiscussing různé aspekty vstupně-výstupních operací pro data, kódu jazyka R a grafiky v prostředí Azure Machine Learning hello.</span><span class="sxs-lookup"><span data-stu-id="3f44d-127">Then we progress toodiscussing various aspects of I/O for data, R code and graphics in hello Azure Machine Learning environment.</span></span>
* <span data-ttu-id="3f44d-128">První část hello naše řešení prognózy jsme pak bude vytvořit tak, že vytvoříte kód pro čištění dat a transformace.</span><span class="sxs-lookup"><span data-stu-id="3f44d-128">We will then construct hello first part of our forecasting solution by creating code for data cleaning and transformation.</span></span>
* <span data-ttu-id="3f44d-129">Naše data připravený provedeme analýzu hello korelací mezi několik hello proměnných v naší datové sadě.</span><span class="sxs-lookup"><span data-stu-id="3f44d-129">With our data prepared we will perform an analysis of hello correlations between several of hello variables in our dataset.</span></span>
* <span data-ttu-id="3f44d-130">Nakonec vytvoříme předpovědi modelu sezónní časové řady pro produkci mléka.</span><span class="sxs-lookup"><span data-stu-id="3f44d-130">Finally, we will create a seasonal time series forecasting model for milk production.</span></span>

## <span data-ttu-id="3f44d-131"><a id="mlstudio"></a>Komunikovat s jazyk R v Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="3f44d-131"><a id="mlstudio"></a>Interact with R language in Machine Learning Studio</span></span>
<span data-ttu-id="3f44d-132">Tato část vás provede některé základní informace o interakci s hello R programovací jazyk v prostředí Machine Learning Studio hello.</span><span class="sxs-lookup"><span data-stu-id="3f44d-132">This section takes you through some basics of interacting with hello R programming language in hello Machine Learning Studio environment.</span></span> <span data-ttu-id="3f44d-133">jazyk Hello R poskytuje výkonný nástroj toocreate přizpůsobit analytics a data manipulaci s moduly prostředí Azure Machine Learning hello.</span><span class="sxs-lookup"><span data-stu-id="3f44d-133">hello R language provides a powerful tool toocreate customized analytics and data manipulation modules within hello Azure Machine Learning environment.</span></span>

<span data-ttu-id="3f44d-134">Použiji Rstudia toodevelop, testování a ladění kódu jazyka R v malém měřítku.</span><span class="sxs-lookup"><span data-stu-id="3f44d-134">I will use RStudio toodevelop, test and debug R code on a small scale.</span></span> <span data-ttu-id="3f44d-135">Tento kód je pak vyjímání a vkládání do [spustit skript jazyka R] [ execute-r-script] modulu v připravené toorun Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="3f44d-135">This code is then cut and paste into an [Execute R Script][execute-r-script] module in Machine Learning Studio ready toorun.</span></span>  

### <a name="hello-execute-r-script-module"></a><span data-ttu-id="3f44d-136">modul pro spuštění skriptu jazyka R Hello</span><span class="sxs-lookup"><span data-stu-id="3f44d-136">hello Execute R Script module</span></span>
<span data-ttu-id="3f44d-137">V rámci Machine Learning Studio, R skripty se spouštějí v rámci hello [spustit skript jazyka R] [ execute-r-script] modulu.</span><span class="sxs-lookup"><span data-stu-id="3f44d-137">Within Machine Learning Studio, R scripts are run within hello [Execute R Script][execute-r-script] module.</span></span> <span data-ttu-id="3f44d-138">Příkladem hello [spustit skript jazyka R] [ execute-r-script] modulu v nástroji Machine Learning Studio je znázorněno na obrázku 1.</span><span class="sxs-lookup"><span data-stu-id="3f44d-138">An example of hello [Execute R Script][execute-r-script] module in Machine Learning Studio is shown in Figure 1.</span></span>

 ![Programovací jazyk R: modulu spuštění skriptu R hello vybrané v nástroji Machine Learning Studio][1]

<span data-ttu-id="3f44d-140">*Obrázek 1. zobrazení modulu spuštění skriptu R hello vybrané prostředí Machine Learning Studio Hello.*</span><span class="sxs-lookup"><span data-stu-id="3f44d-140">*Figure 1. hello Machine Learning Studio environment showing hello Execute R Script module selected.*</span></span>

<span data-ttu-id="3f44d-141">Odkazy tooFigure 1, podíváme se na některé z klíčových částí hello hello Machine Learning Studio prostředí pro práci s hello [spustit skript jazyka R] [ execute-r-script] modulu.</span><span class="sxs-lookup"><span data-stu-id="3f44d-141">Referring tooFigure 1, let's look at some of hello key parts of hello Machine Learning Studio environment for working with hello [Execute R Script][execute-r-script] module.</span></span>

* <span data-ttu-id="3f44d-142">v prostředním podokně hello jsou uvedeny Hello moduly v experimentu hello.</span><span class="sxs-lookup"><span data-stu-id="3f44d-142">hello modules in hello experiment are shown in hello center pane.</span></span>
* <span data-ttu-id="3f44d-143">Hello horní část hello pravém podokně okna tooview obsahuje a upravit skripty R.</span><span class="sxs-lookup"><span data-stu-id="3f44d-143">hello upper part of hello right pane contains a window tooview and edit your R scripts.</span></span>  
* <span data-ttu-id="3f44d-144">Hello dolní části pravém podokně jsou uvedeny některé vlastnosti hello [spustit skript jazyka R][execute-r-script].</span><span class="sxs-lookup"><span data-stu-id="3f44d-144">hello lower part of right pane shows some properties of hello [Execute R Script][execute-r-script].</span></span> <span data-ttu-id="3f44d-145">Protokoly chyb a výstupu hello můžete zobrazit kliknutím na příslušné body hello tento podokna.</span><span class="sxs-lookup"><span data-stu-id="3f44d-145">You can view hello error and output logs by clicking on hello appropriate spots of this pane.</span></span>

<span data-ttu-id="3f44d-146">Jsme bude samozřejmě být hovoříte o hello [spustit skript jazyka R] [ execute-r-script] podrobněji na hello zbytek tohoto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="3f44d-146">We will, of course, be discussing hello [Execute R Script][execute-r-script] in greater detail in hello rest of this document.</span></span>

<span data-ttu-id="3f44d-147">Při práci s funkcemi, komplexní R, doporučujeme úpravy, testování a ladění ve Rstudia.</span><span class="sxs-lookup"><span data-stu-id="3f44d-147">When working with complex R functions, I recommend that you edit, test and debug in RStudio.</span></span> <span data-ttu-id="3f44d-148">Stejně jako u jakékoli vývoj softwaru postupně rozšiřovat kódu a testování na malé jednoduchých testovacích případech.</span><span class="sxs-lookup"><span data-stu-id="3f44d-148">As with any software development, extend your code incrementally and test it on small simple test cases.</span></span> <span data-ttu-id="3f44d-149">Potom kopírování a vkládání funkcí do hello R skript okno hello [spustit skript jazyka R] [ execute-r-script] modulu.</span><span class="sxs-lookup"><span data-stu-id="3f44d-149">Then cut and paste your functions into hello R script window of hello [Execute R Script][execute-r-script] module.</span></span> <span data-ttu-id="3f44d-150">Tento přístup umožňuje tooharness jak hello Rstudia integrované vývojové prostředí (IDE) a hello power Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="3f44d-150">This approach allows you tooharness both hello RStudio integrated development environment (IDE) and hello power of Azure Machine Learning.</span></span>  

#### <a name="execute-r-code"></a><span data-ttu-id="3f44d-151">Spuštění kódu jazyka R</span><span class="sxs-lookup"><span data-stu-id="3f44d-151">Execute R code</span></span>
<span data-ttu-id="3f44d-152">Žádný kód R v hello [spustit skript jazyka R] [ execute-r-script] modulu spustí při spuštění hello experiment kliknutím na hello **spustit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3f44d-152">Any R code in hello [Execute R Script][execute-r-script] module will execute when you run hello experiment by clicking on hello **Run** button.</span></span> <span data-ttu-id="3f44d-153">Po dokončení provádění zaškrtnutí se zobrazí na hello [spustit skript jazyka R] [ execute-r-script] ikonu.</span><span class="sxs-lookup"><span data-stu-id="3f44d-153">When execution has completed, a check mark will appear on hello [Execute R Script][execute-r-script] icon.</span></span>

#### <a name="defensive-r-coding-for-azure-machine-learning"></a><span data-ttu-id="3f44d-154">Obranným R kódování pro Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="3f44d-154">Defensive R coding for Azure Machine Learning</span></span>
<span data-ttu-id="3f44d-155">Pokud vyvíjíte kódu jazyka R pro vyslovení, webové služby pomocí Azure Machine Learning, byste měli výborný naplánovat, jak váš kód se bude zabývat vstup neočekávaná data a výjimky.</span><span class="sxs-lookup"><span data-stu-id="3f44d-155">If you are developing R code for, say, a web service by using Azure Machine Learning, you should definitely plan how your code will deal with an unexpected data input and exceptions.</span></span> <span data-ttu-id="3f44d-156">toomaintain přehlednost I nebyly zahrnuty hodně způsobem hello kontrola nebo zpracování výjimek v většinu příklady kódu hello vidět.</span><span class="sxs-lookup"><span data-stu-id="3f44d-156">toomaintain clarity, I have not included much in hello way of checking or exception handling in most of hello code examples shown.</span></span> <span data-ttu-id="3f44d-157">Ale jak budeme pokračovat I získáte několik příkladů, funkce pomocí funkce pro zpracování výjimek na R.</span><span class="sxs-lookup"><span data-stu-id="3f44d-157">However, as we proceed I will give you several examples of functions by using R's exception handling capability.</span></span>  

<span data-ttu-id="3f44d-158">Pokud potřebujete podrobnější zacházení s zpracovávání výjimek v jazyce R, I doporučujeme, abyste si přečetli hello hello knihy podle Wickham uvedené v příslušných částech [příloha B – další čtení](#appendixb).</span><span class="sxs-lookup"><span data-stu-id="3f44d-158">If you need a more complete treatment of R exception handling, I recommend you read hello applicable sections of hello book by Wickham listed in [Appendix B - Further Reading](#appendixb).</span></span>

#### <a name="debug-and-test-r-in-machine-learning-studio"></a><span data-ttu-id="3f44d-159">Ladění a testování R v Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="3f44d-159">Debug and test R in Machine Learning Studio</span></span>
<span data-ttu-id="3f44d-160">tooreiterate, I doporučujeme testování a ladění kódu R v malém měřítku v Rstudia.</span><span class="sxs-lookup"><span data-stu-id="3f44d-160">tooreiterate, I recommend you test and debug your R code on a small scale in RStudio.</span></span> <span data-ttu-id="3f44d-161">Ale existují případy, kdy budete potřebovat tootrack dolů problémy kód R v hello [spustit skript jazyka R] [ execute-r-script] sám sebe.</span><span class="sxs-lookup"><span data-stu-id="3f44d-161">However, there are cases where you will need tootrack down R code problems in hello [Execute R Script][execute-r-script] itself.</span></span> <span data-ttu-id="3f44d-162">Kromě toho je vhodné toocheck výsledky v Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="3f44d-162">In addition, it is good practice toocheck your results in Machine Learning Studio.</span></span>

<span data-ttu-id="3f44d-163">Výstup hello provádění kódu R a na platformě Azure Machine Learning hello především v souboru výstup.log nebyl nalezen.</span><span class="sxs-lookup"><span data-stu-id="3f44d-163">Output from hello execution of your R code and on hello Azure Machine Learning platform is found primarily in output.log.</span></span> <span data-ttu-id="3f44d-164">Některé další informace se zobrazí v error.log.</span><span class="sxs-lookup"><span data-stu-id="3f44d-164">Some additional information will be seen in error.log.</span></span>  

<span data-ttu-id="3f44d-165">Pokud při běhu kódu R dojde k chybě v nástroji Machine Learning Studio, vaše první postupu by měl být toolook v error.log.</span><span class="sxs-lookup"><span data-stu-id="3f44d-165">If an error occurs in Machine Learning Studio while running your R code, your first course of action should be toolook at error.log.</span></span> <span data-ttu-id="3f44d-166">Tento soubor může obsahovat užitečné chybové zprávy toohelp pochopit a vaše chybu opravit.</span><span class="sxs-lookup"><span data-stu-id="3f44d-166">This file can contain useful error messages toohelp you understand and correct your error.</span></span> <span data-ttu-id="3f44d-167">tooview error.log, kliknutím na tlačítko **zobrazení v protokolu chyb** na hello **podokno properties** pro hello [spustit skript jazyka R] [ execute-r-script] obsahující chyby hello.</span><span class="sxs-lookup"><span data-stu-id="3f44d-167">tooview error.log, click on **View error log** on hello **properties pane** for hello [Execute R Script][execute-r-script] containing hello error.</span></span>

<span data-ttu-id="3f44d-168">Například byl spuštěn v následujícím kódu jazyka R nedefinované proměnné y, hello [spustit skript jazyka R] [ execute-r-script] modul:</span><span class="sxs-lookup"><span data-stu-id="3f44d-168">For example, I ran hello following R code, with an undefined variable y, in an [Execute R Script][execute-r-script] module:</span></span>

    x <- 1.0
    z <- x + y

<span data-ttu-id="3f44d-169">Tento kód se nezdaří tooexecute, což vede k chybě.</span><span class="sxs-lookup"><span data-stu-id="3f44d-169">This code fails tooexecute, resulting in an error condition.</span></span> <span data-ttu-id="3f44d-170">Kliknutím na **zobrazení v protokolu chyb** na hello **podokno properties** vytváří hello zobrazení znázorňuje obrázek 2.</span><span class="sxs-lookup"><span data-stu-id="3f44d-170">Clicking on **View error log** on hello **properties pane** produces hello display shown in Figure 2.</span></span>

  ![Chybová zpráva otevíraném][2]

<span data-ttu-id="3f44d-172">*Obrázek 2. Chybová zpráva automaticky otevírané okno.*</span><span class="sxs-lookup"><span data-stu-id="3f44d-172">*Figure 2. Error message pop-up.*</span></span>

<span data-ttu-id="3f44d-173">Zdá se, potřebujeme toolook v souboru výstup.log toosee hello R chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="3f44d-173">It looks like we need toolook in output.log toosee hello R error message.</span></span> <span data-ttu-id="3f44d-174">Klikněte na hello [spustit skript jazyka R] [ execute-r-script] a potom klikněte na hello **zobrazení souboru výstup.log** položky hello **podokno properties** toohello vpravo.</span><span class="sxs-lookup"><span data-stu-id="3f44d-174">Click on hello [Execute R Script][execute-r-script] and then click on hello **View output.log** item on hello **properties pane** toohello right.</span></span> <span data-ttu-id="3f44d-175">Otevře se nové okno prohlížeče a zobrazuje následující hello.</span><span class="sxs-lookup"><span data-stu-id="3f44d-175">A new browser window opens, and I see hello following.</span></span>

    [Critical]     Error: Error 0063: hello following error occurred during evaluation of R script:
    ---------- Start of error message from R ----------
    object 'y' not found


    object 'y' not found
    ----------- End of error message from R -----------

<span data-ttu-id="3f44d-176">Tato chybová zpráva obsahuje žádný výskyt nečekaných událostí a jednoznačně identifikuje hello problém.</span><span class="sxs-lookup"><span data-stu-id="3f44d-176">This error message contains no surprises and clearly identifies hello problem.</span></span>

<span data-ttu-id="3f44d-177">Hodnota hello tooinspect všech objektů v R, můžete vytisknout tyto hodnoty toohello souboru výstup.log souboru.</span><span class="sxs-lookup"><span data-stu-id="3f44d-177">tooinspect hello value of any object in R, you can print these values toohello output.log file.</span></span> <span data-ttu-id="3f44d-178">Hello pravidla pro zkoumání objekt hodnoty jsou v podstatě stejný hello jako interaktivní relace R zařízením.</span><span class="sxs-lookup"><span data-stu-id="3f44d-178">hello rules for examining object values are essentially hello same as in an interactive R session.</span></span> <span data-ttu-id="3f44d-179">Pokud zadáte název proměnné na řádek, například hodnota hello hello objektu bude tištěné toohello souboru výstup.log souboru.</span><span class="sxs-lookup"><span data-stu-id="3f44d-179">For example, if you type a variable name on a line, hello value of hello object will be printed toohello output.log file.</span></span>  

#### <a name="packages-in-machine-learning-studio"></a><span data-ttu-id="3f44d-180">Balíčky v nástroji Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="3f44d-180">Packages in Machine Learning Studio</span></span>
<span data-ttu-id="3f44d-181">Azure Machine Learning se dodává s více než 350 předinstalované balíčky jazyk R.</span><span class="sxs-lookup"><span data-stu-id="3f44d-181">Azure Machine Learning comes with over 350 preinstalled R language packages.</span></span> <span data-ttu-id="3f44d-182">Můžete použít následující kód v hello hello [spustit skript jazyka R] [ execute-r-script] modulu tooretrieve seznam hello předinstalována balíčky.</span><span class="sxs-lookup"><span data-stu-id="3f44d-182">You can use hello following code in hello [Execute R Script][execute-r-script] module tooretrieve a list of hello preinstalled packages.</span></span>

    data.set <- data.frame(installed.packages())
    maml.mapOutputPort("data.set")

<span data-ttu-id="3f44d-183">Pokud nerozumíte hello poslední řádek tohoto kódu v době hello, přečtěte si.</span><span class="sxs-lookup"><span data-stu-id="3f44d-183">If you don't understand hello last line of this code at hello moment, read on.</span></span> <span data-ttu-id="3f44d-184">Hello zbytek tohoto dokumentu hojně pojednává využití R v prostředí Azure Machine Learning hello.</span><span class="sxs-lookup"><span data-stu-id="3f44d-184">In hello rest of this document we will extensively discuss using R in hello Azure Machine Learning environment.</span></span>

### <a name="introduction-toorstudio"></a><span data-ttu-id="3f44d-185">TooRStudio Úvod</span><span class="sxs-lookup"><span data-stu-id="3f44d-185">Introduction tooRStudio</span></span>
<span data-ttu-id="3f44d-186">Rstudia je často používaný IDE pro R. Pro úpravy, testování a ladění některé hello R kód použitý v této úvodní příručce použiji Rstudia.</span><span class="sxs-lookup"><span data-stu-id="3f44d-186">RStudio is a widely used IDE for R. I will use RStudio for editing, testing and debugging some of hello R code used in this quick start guide.</span></span> <span data-ttu-id="3f44d-187">Jakmile kódu jazyka R otestované a připravené, jednoduše kopírování a vkládání z editoru Rstudia hello do nástroje Machine Learning Studio [spustit skript jazyka R] [ execute-r-script] modulu.</span><span class="sxs-lookup"><span data-stu-id="3f44d-187">Once R code is tested and ready, you simply cut and paste from hello RStudio editor into a Machine Learning Studio [Execute R Script][execute-r-script] module.</span></span>  

<span data-ttu-id="3f44d-188">Pokud nemáte hello R programovací jazyk nainstalovaný na počítači klientů, doporučujeme, že udělejte to teď.</span><span class="sxs-lookup"><span data-stu-id="3f44d-188">If you do not have hello R programming language installed on your desktop machine, I recommend you do so now.</span></span> <span data-ttu-id="3f44d-189">Ke stažení zdarma jazyka R s otevřeným zdrojem jsou k dispozici na hello komplexní R archivu sítě (CRAN) v [http://www.r-project.org/](http://www.r-project.org/).</span><span class="sxs-lookup"><span data-stu-id="3f44d-189">Free downloads of open source R language are available at hello Comprehensive R Archive Network (CRAN) at [http://www.r-project.org/](http://www.r-project.org/).</span></span> <span data-ttu-id="3f44d-190">Nejsou k dispozici pro Windows, Mac OS a Linux a UNIX stáhne.</span><span class="sxs-lookup"><span data-stu-id="3f44d-190">There are downloads available for Windows, Mac OS, and Linux/UNIX.</span></span> <span data-ttu-id="3f44d-191">Zvolte blízkým zrcadlení a postupujte podle pokynů ke stažení hello.</span><span class="sxs-lookup"><span data-stu-id="3f44d-191">Choose a nearby mirror and follow hello download directions.</span></span> <span data-ttu-id="3f44d-192">Kromě toho CRAN obsahuje širokou řadu užitečné balíčky manipulaci s analytickými funkcemi a data.</span><span class="sxs-lookup"><span data-stu-id="3f44d-192">In addition, CRAN contains a wealth of useful analytics and data manipulation packages.</span></span>

<span data-ttu-id="3f44d-193">Pokud jste nový tooRStudio, by měl stáhněte a nainstalujte verzi pro stolní počítače hello.</span><span class="sxs-lookup"><span data-stu-id="3f44d-193">If you are new tooRStudio, you should download and install hello desktop version.</span></span> <span data-ttu-id="3f44d-194">Můžete najít hello Rstudia soubory ke stažení pro Windows, Mac OS a Linux a UNIX v http://www.rstudio.com/products/RStudio/.</span><span class="sxs-lookup"><span data-stu-id="3f44d-194">You can find hello RStudio downloads for Windows, Mac OS, and Linux/UNIX at http://www.rstudio.com/products/RStudio/.</span></span> <span data-ttu-id="3f44d-195">Postupujte podle pokynů hello tooinstall Rstudia k dispozici na stolního počítače.</span><span class="sxs-lookup"><span data-stu-id="3f44d-195">Follow hello directions provided tooinstall RStudio on your desktop machine.</span></span>  

<span data-ttu-id="3f44d-196">Kurz Úvod tooRStudio je k dispozici na https://support.rstudio.com/hc/sections/200107586-Using-RStudio.</span><span class="sxs-lookup"><span data-stu-id="3f44d-196">A tutorial introduction tooRStudio is available at https://support.rstudio.com/hc/sections/200107586-Using-RStudio.</span></span>

<span data-ttu-id="3f44d-197">Poskytují I některé další informace o používání Rstudia v [příloha A][appendixa].</span><span class="sxs-lookup"><span data-stu-id="3f44d-197">I provide some additional information on using RStudio in [Appendix A][appendixa].</span></span>  

## <span data-ttu-id="3f44d-198"><a id="scriptmodule"></a>Získání dat a deaktivovat modulu spuštění skriptu R hello</span><span class="sxs-lookup"><span data-stu-id="3f44d-198"><a id="scriptmodule"></a>Get data in and out of hello Execute R Script module</span></span>
<span data-ttu-id="3f44d-199">V této části probereme, jak načíst data do a z hello [spustit skript jazyka R] [ execute-r-script] modulu.</span><span class="sxs-lookup"><span data-stu-id="3f44d-199">In this section we will discuss how you get data into and out of hello [Execute R Script][execute-r-script] module.</span></span> <span data-ttu-id="3f44d-200">Jsme zkontrolujete jak toohandle různé datové typy číst do a z hello [spustit skript jazyka R] [ execute-r-script] modulu.</span><span class="sxs-lookup"><span data-stu-id="3f44d-200">We will review how toohandle various data types read into and out of hello [Execute R Script][execute-r-script] module.</span></span>

<span data-ttu-id="3f44d-201">Hello kompletní kód pro tento oddíl je v souboru zip hello, které jste dříve stáhli.</span><span class="sxs-lookup"><span data-stu-id="3f44d-201">hello complete code for this section is in hello zip file you downloaded earlier.</span></span>

### <a name="load-and-check-data-in-machine-learning-studio"></a><span data-ttu-id="3f44d-202">Načtení a zkontrolujte, zda data v nástroji Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="3f44d-202">Load and check data in Machine Learning Studio</span></span>
#### <span data-ttu-id="3f44d-203"><a id="loading"></a>Načíst hello datové sady</span><span class="sxs-lookup"><span data-stu-id="3f44d-203"><a id="loading"></a>Load hello dataset</span></span>
<span data-ttu-id="3f44d-204">Spustíme načtením hello **csdairydata.csv** souboru do Azure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="3f44d-204">We will start by loading hello **csdairydata.csv** file into Azure Machine Learning Studio.</span></span>

* <span data-ttu-id="3f44d-205">Spuštění prostředí Azure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="3f44d-205">Start your Azure Machine Learning Studio environment.</span></span>
* <span data-ttu-id="3f44d-206">Klikněte na **+ nový** v hello nižší levé straně obrazovky a vyberte **datovou sadu**.</span><span class="sxs-lookup"><span data-stu-id="3f44d-206">Click on **+ NEW** at hello lower left of your screen and select **Dataset**.</span></span>
* <span data-ttu-id="3f44d-207">Vyberte **z místního souboru**a potom **Procházet** tooselect hello souboru.</span><span class="sxs-lookup"><span data-stu-id="3f44d-207">Select **From Local File**, and then **Browse** tooselect hello file.</span></span>
* <span data-ttu-id="3f44d-208">Ujistěte se, zda jste vybrali **souboru obecné CSV s hlavičkou (.csv)** jako typ hello hello datové sady.</span><span class="sxs-lookup"><span data-stu-id="3f44d-208">Make sure you have selected **Generic CSV file with header (.csv)** as hello type for hello dataset.</span></span>
* <span data-ttu-id="3f44d-209">Klikněte na tlačítko zaškrtnutí hello.</span><span class="sxs-lookup"><span data-stu-id="3f44d-209">Click hello check mark.</span></span>
* <span data-ttu-id="3f44d-210">Až se nahrají hello datovou sadu, měli byste vidět nová datová sada hello kliknutím na hello **datové sady** kartě.</span><span class="sxs-lookup"><span data-stu-id="3f44d-210">After hello dataset has been uploaded, you should see hello new dataset by clicking on hello **Datasets** tab.</span></span>  

#### <a name="create-an-experiment"></a><span data-ttu-id="3f44d-211">Vytvoření experimentu</span><span class="sxs-lookup"><span data-stu-id="3f44d-211">Create an experiment</span></span>
<span data-ttu-id="3f44d-212">Teď, když máme některá data v nástroji Machine Learning Studio, potřebujeme toocreate analýzu hello toodo experimentu.</span><span class="sxs-lookup"><span data-stu-id="3f44d-212">Now that we have some data in Machine Learning Studio, we need toocreate an experiment toodo hello analysis.</span></span>  

* <span data-ttu-id="3f44d-213">Klikněte na **+ nový** v hello snížit vlevo a vyberte **experimentu**, pak **prázdný Experiment**.</span><span class="sxs-lookup"><span data-stu-id="3f44d-213">Click on **+ NEW** at hello lower left and select **Experiment**, then **Blank Experiment**.</span></span>
* <span data-ttu-id="3f44d-214">Můžete tak, že vyberete název experimentu a úpravy, hello **na vytvořit experimentu...**  nadpis v horní části hello hello stránky.</span><span class="sxs-lookup"><span data-stu-id="3f44d-214">You can name your experiment by selecting, and modifying, hello **Experiment created on ...** title at hello top of hello page.</span></span> <span data-ttu-id="3f44d-215">Například změna příliš**certifikační Autority mlékárny Analysis**.</span><span class="sxs-lookup"><span data-stu-id="3f44d-215">For example, changing it too**CA Dairy Analysis**.</span></span>
* <span data-ttu-id="3f44d-216">Na levé straně hello hello experimentu stránky rozbalte **uložit datové sady**a potom **Moje datové sady**.</span><span class="sxs-lookup"><span data-stu-id="3f44d-216">On hello left of hello experiment page, expand **Saved Datasets**, and then **My Datasets**.</span></span> <span data-ttu-id="3f44d-217">Měli byste vidět hello **cadairydata.csv** který jste dříve nahráli.</span><span class="sxs-lookup"><span data-stu-id="3f44d-217">You should see hello **cadairydata.csv** that you uploaded earlier.</span></span>
* <span data-ttu-id="3f44d-218">Přetažení hello **datovou sadu csdairydata.csv** do experimentu hello.</span><span class="sxs-lookup"><span data-stu-id="3f44d-218">Drag and drop hello **csdairydata.csv dataset** onto hello experiment.</span></span>
* <span data-ttu-id="3f44d-219">V hello **vyhledávání experimentovat položky** pole na hello horní části levého podokna hello, typ [spustit skript jazyka R][execute-r-script].</span><span class="sxs-lookup"><span data-stu-id="3f44d-219">In hello **Search experiment items** box on hello top of hello left pane, type [Execute R Script][execute-r-script].</span></span> <span data-ttu-id="3f44d-220">Zobrazí se modul hello jsou uvedeny v seznamu hledání hello.</span><span class="sxs-lookup"><span data-stu-id="3f44d-220">You will see hello module appear in hello search list.</span></span>
* <span data-ttu-id="3f44d-221">Přetažení hello [spustit skript jazyka R] [ execute-r-script] modulu do vaší palety.</span><span class="sxs-lookup"><span data-stu-id="3f44d-221">Drag and drop hello [Execute R Script][execute-r-script] module onto your pallet.</span></span>  
* <span data-ttu-id="3f44d-222">Připojit hello výstup hello **datovou sadu csdairydata.csv** krajní levé vstup toohello (**Dataset1**) z hello [spustit skript jazyka R][execute-r-script].</span><span class="sxs-lookup"><span data-stu-id="3f44d-222">Connect hello output of hello **csdairydata.csv dataset** toohello leftmost input (**Dataset1**) of hello [Execute R Script][execute-r-script].</span></span>
* <span data-ttu-id="3f44d-223">**Nezapomeňte tooclick na 'Uložit'!**</span><span class="sxs-lookup"><span data-stu-id="3f44d-223">**Don't forget tooclick on 'Save'!**</span></span>  

<span data-ttu-id="3f44d-224">V tomto okamžiku by měl experimentu vypadat podobně jako obrázek 3.</span><span class="sxs-lookup"><span data-stu-id="3f44d-224">At this point your experiment should look something like Figure 3.</span></span>

![Hello certifikační Autority mlékárny Analysis experimentovat s datovou sadu a modul pro spuštění skriptu jazyka R][3]

<span data-ttu-id="3f44d-226">*Obrázek 3. Hello certifikační Autority mlékárny Analysis experimentovat s datovou sadu a spustit skript jazyka R modulu.*</span><span class="sxs-lookup"><span data-stu-id="3f44d-226">*Figure 3. hello CA Dairy Analysis experiment with dataset and Execute R Script module.*</span></span>

#### <a name="check-on-hello-data"></a><span data-ttu-id="3f44d-227">Zkontrolujte data hello</span><span class="sxs-lookup"><span data-stu-id="3f44d-227">Check on hello data</span></span>
<span data-ttu-id="3f44d-228">Pojďme Podíváme se na hello data, která mají byl načten do našich experimentu.</span><span class="sxs-lookup"><span data-stu-id="3f44d-228">Let's have a look at hello data we have loaded into our experiment.</span></span> <span data-ttu-id="3f44d-229">V experimentu hello, klikněte na výstup hello hello **datovou sadu cadairydata.csv** a vyberte **vizualizovat**.</span><span class="sxs-lookup"><span data-stu-id="3f44d-229">In hello experiment, click on hello output of hello **cadairydata.csv dataset** and select **visualize**.</span></span> <span data-ttu-id="3f44d-230">Měli byste vidět něco podobného jako na obrázku 4.</span><span class="sxs-lookup"><span data-stu-id="3f44d-230">You should see something like Figure 4.</span></span>  

![Souhrn hello cadairydata.csv datové sady][4]

<span data-ttu-id="3f44d-232">*Obrázek 4. Shrnutí hello cadairydata.csv datovou sadu.*</span><span class="sxs-lookup"><span data-stu-id="3f44d-232">*Figure 4. Summary of hello cadairydata.csv dataset.*</span></span>

<span data-ttu-id="3f44d-233">V tomto zobrazení vidíte mnoho užitečných informací.</span><span class="sxs-lookup"><span data-stu-id="3f44d-233">In this view we see a lot of useful information.</span></span> <span data-ttu-id="3f44d-234">My uvidíme text hello první několik řádků z této datové sady.</span><span class="sxs-lookup"><span data-stu-id="3f44d-234">We can see hello first several rows of that dataset.</span></span> <span data-ttu-id="3f44d-235">Pokud jsme vyberte sloupec, hello statistiky části zobrazíte další informace o sloupci hello.</span><span class="sxs-lookup"><span data-stu-id="3f44d-235">If we select a column, hello Statistics section shows more information about hello column.</span></span> <span data-ttu-id="3f44d-236">Typ funkce řádek hello příkladu nám jaké typy dat Azure Machine Learning Studio přiřazené toohello sloupce.</span><span class="sxs-lookup"><span data-stu-id="3f44d-236">For example, hello Feature Type row shows us what data types Azure Machine Learning Studio assigned toohello column.</span></span> <span data-ttu-id="3f44d-237">Rychle zkontrolovat tímto způsobem je kontrolu dobrý správností než začneme toodo jakékoli závažné pracovní.</span><span class="sxs-lookup"><span data-stu-id="3f44d-237">Having a quick look like this is a good sanity check before we start toodo any serious work.</span></span>

### <a name="first-r-script"></a><span data-ttu-id="3f44d-238">První skript jazyka R</span><span class="sxs-lookup"><span data-stu-id="3f44d-238">First R script</span></span>
<span data-ttu-id="3f44d-239">Umožňuje vytvořit jednoduché první skriptu tooexperiment R s v Azure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="3f44d-239">Let's create a simple first R script tooexperiment with in Azure Machine Learning Studio.</span></span> <span data-ttu-id="3f44d-240">I vytvoření a otestování hello následující skript v Rstudia.</span><span class="sxs-lookup"><span data-stu-id="3f44d-240">I have created and tested hello following script in RStudio.</span></span>  

    ## Only one of hello following two lines should be used
    ## If running in Machine Learning Studio, use hello first line with maml.mapInputPort()
    ## If in RStudio, use hello second line with read.csv()
    cadairydata <- maml.mapInputPort(1)
    # cadairydata  <- read.csv("cadairydata.csv", header = TRUE, stringsAsFactors = FALSE)
    str(cadairydata)
    pairs(~ Cotagecheese.Prod + Icecream.Prod + Milk.Prod + N.CA.Fat.Price, data = cadairydata)
    ## hello following line should be executed only when running in
    ## Azure Machine Learning Studio
    maml.mapOutputPort('cadairydata')

<span data-ttu-id="3f44d-241">Nyní je třeba tootransfer tento skript tooAzure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="3f44d-241">Now I need tootransfer this script tooAzure Machine Learning Studio.</span></span> <span data-ttu-id="3f44d-242">Může stačí vyjmout a vložit.</span><span class="sxs-lookup"><span data-stu-id="3f44d-242">I could simply cut and paste.</span></span> <span data-ttu-id="3f44d-243">Ale v takovém případě bude přenést Moje skript jazyka R prostřednictvím soubor zip.</span><span class="sxs-lookup"><span data-stu-id="3f44d-243">However, in this case, I will transfer my R script via a zip file.</span></span>

### <a name="data-input-toohello-execute-r-script-module"></a><span data-ttu-id="3f44d-244">Modul spustit skript jazyka R toohello vstupních dat</span><span class="sxs-lookup"><span data-stu-id="3f44d-244">Data input toohello Execute R Script module</span></span>
<span data-ttu-id="3f44d-245">Pojďme Podíváme se na hello vstupy toohello [spustit skript jazyka R] [ execute-r-script] modulu.</span><span class="sxs-lookup"><span data-stu-id="3f44d-245">Let's have a look at hello inputs toohello [Execute R Script][execute-r-script] module.</span></span> <span data-ttu-id="3f44d-246">V tomto příkladu jsme bude číst data dojnic kalifornské hello do hello [spustit skript jazyka R] [ execute-r-script] modulu.</span><span class="sxs-lookup"><span data-stu-id="3f44d-246">In this example we will read hello California dairy data into hello [Execute R Script][execute-r-script] module.</span></span>  

<span data-ttu-id="3f44d-247">Existují tři možné vstupy pro hello [spustit skript jazyka R] [ execute-r-script] modulu.</span><span class="sxs-lookup"><span data-stu-id="3f44d-247">There are three possible inputs for hello [Execute R Script][execute-r-script] module.</span></span> <span data-ttu-id="3f44d-248">Může používat kterékoli nebo všechny tyto vstupy, v závislosti na vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="3f44d-248">You may use any one or all of these inputs, depending on your application.</span></span> <span data-ttu-id="3f44d-249">Je také perfektně přiměřené toouse R skript, který nemá žádný vstup vůbec.</span><span class="sxs-lookup"><span data-stu-id="3f44d-249">It is also perfectly reasonable toouse an R script that takes no input at all.</span></span>  

<span data-ttu-id="3f44d-250">Podívejme se na každý z těchto vstupů přecházející z levé tooright.</span><span class="sxs-lookup"><span data-stu-id="3f44d-250">Let's look at each of these inputs, going from left tooright.</span></span> <span data-ttu-id="3f44d-251">Uvidíte hello názvy jednotlivých hello vstupy umístěním kurzoru přes hello vstup a čtení hello popisku.</span><span class="sxs-lookup"><span data-stu-id="3f44d-251">You can see hello names of each of hello inputs by placing your cursor over hello input and reading hello tooltip.</span></span>  

#### <a name="script-bundle"></a><span data-ttu-id="3f44d-252">Skript sady</span><span class="sxs-lookup"><span data-stu-id="3f44d-252">Script Bundle</span></span>
<span data-ttu-id="3f44d-253">Hello vstupu skriptu sady vám umožní toopass hello obsahu souboru zip do [spustit skript jazyka R] [ execute-r-script] modulu.</span><span class="sxs-lookup"><span data-stu-id="3f44d-253">hello Script Bundle input allows you toopass hello contents of a zip file into [Execute R Script][execute-r-script] module.</span></span> <span data-ttu-id="3f44d-254">Můžete použít jednu z následujících příkazů tooread hello obsahu souboru zip hello do vašeho kódu jazyka R hello.</span><span class="sxs-lookup"><span data-stu-id="3f44d-254">You can use one of hello following commands tooread hello contents of hello zip file into your R code.</span></span>

    source("src/yourfile.R") # Reads a zipped R script
    load("src/yourData.rdata") # Reads a zipped R data file

> [!NOTE]
> <span data-ttu-id="3f44d-255">Azure Machine Learning zpracovává soubory v hello zip, jako kdyby se hello src / adresáře, proto musíte tooprefix názvy váš soubor s tímto názvem adresáře.</span><span class="sxs-lookup"><span data-stu-id="3f44d-255">Azure Machine Learning treats files in hello zip as if they are in hello src/ directory, so you need tooprefix your file names with this directory name.</span></span> <span data-ttu-id="3f44d-256">Například pokud hello zip obsahuje soubory hello `yourfile.R` a `yourData.rdata` hello kořenové hello zip, by adres jako `src/yourfile.R` a `src/yourData.rdata` při použití `source` a `load`.</span><span class="sxs-lookup"><span data-stu-id="3f44d-256">For example, if hello zip contains hello files `yourfile.R` and `yourData.rdata` in hello root of hello zip, you would address these as `src/yourfile.R` and `src/yourData.rdata` when using `source` and `load`.</span></span>
> 
> 

<span data-ttu-id="3f44d-257">Už jsme probírali načítání datové sady v [načítání datovou sadu hello](#loading).</span><span class="sxs-lookup"><span data-stu-id="3f44d-257">We already discussed loading datasets in [Loading hello dataset](#loading).</span></span> <span data-ttu-id="3f44d-258">Po vytvoření a testování hello R skript uvedené v předchozí části hello hello následující:</span><span class="sxs-lookup"><span data-stu-id="3f44d-258">Once you have created and tested hello R script shown in hello previous section, do hello following:</span></span>

1. <span data-ttu-id="3f44d-259">Uložte skript hello R do. R soubor.</span><span class="sxs-lookup"><span data-stu-id="3f44d-259">Save hello R script into a .R file.</span></span> <span data-ttu-id="3f44d-260">Můžu volat Moje soubor skriptu "simpleplot. R".</span><span class="sxs-lookup"><span data-stu-id="3f44d-260">I call my script file "simpleplot.R".</span></span> <span data-ttu-id="3f44d-261">Zde je hello obsah.</span><span class="sxs-lookup"><span data-stu-id="3f44d-261">Here's hello contents.</span></span>
   
        ## Only one of hello following two lines should be used
        ## If running in Machine Learning Studio, use hello first line with maml.mapInputPort()
        ## If in RStudio, use hello second line with read.csv()
        cadairydata <- maml.mapInputPort(1)
        # cadairydata  <- read.csv("cadairydata.csv", header = TRUE, stringsAsFactors = FALSE)
        str(cadairydata)
        pairs(~ Cotagecheese.Prod + Icecream.Prod + Milk.Prod + N.CA.Fat.Price, data = cadairydata)
        ## hello following line should be executed only when running in
        ## Azure Machine Learning Studio
        maml.mapOutputPort('cadairydata')
2. <span data-ttu-id="3f44d-262">Vytvořte soubor zip a zkopírujte skript do tohoto souboru zip.</span><span class="sxs-lookup"><span data-stu-id="3f44d-262">Create a zip file and copy your script into this zip file.</span></span> <span data-ttu-id="3f44d-263">V systému Windows, klikněte pravým tlačítkem na soubor hello a vyberte **poslat**a potom **komprimované složky**.</span><span class="sxs-lookup"><span data-stu-id="3f44d-263">On Windows, you can right-click on hello file and select **Send to**, and then **Compressed folder**.</span></span> <span data-ttu-id="3f44d-264">Tím se vytvoří nový soubor zip obsahující hello "simpleplot. Soubor R".</span><span class="sxs-lookup"><span data-stu-id="3f44d-264">This will create a new zip file containing hello "simpleplot.R" file.</span></span>
3. <span data-ttu-id="3f44d-265">Přidat váš soubor toohello **datové sady** v nástroji Machine Learning Studio, určení typu hello jako **zip**.</span><span class="sxs-lookup"><span data-stu-id="3f44d-265">Add your file toohello **datasets** in Machine Learning Studio, specifying hello type as **zip**.</span></span> <span data-ttu-id="3f44d-266">Teď byste měli vidět soubor zip hello v vaše datové sady.</span><span class="sxs-lookup"><span data-stu-id="3f44d-266">You should now see hello zip file in your datasets.</span></span>
4. <span data-ttu-id="3f44d-267">Přetažení soubor zip hello z **datové sady** na hello **ML Studio plátno**.</span><span class="sxs-lookup"><span data-stu-id="3f44d-267">Drag and drop hello zip file from **datasets** onto hello **ML Studio canvas**.</span></span>
5. <span data-ttu-id="3f44d-268">Připojit hello výstup hello **zip data** ikonu toohello **skript sady** vstupní z hello [spustit skript jazyka R] [ execute-r-script] modulu.</span><span class="sxs-lookup"><span data-stu-id="3f44d-268">Connect hello output of hello **zip data** icon toohello **Script Bundle** input of hello [Execute R Script][execute-r-script] module.</span></span>
6. <span data-ttu-id="3f44d-269">Typ hello `source()` funkce nahraďte názvem souboru zip do hello kódu – okno pro hello [spustit skript jazyka R] [ execute-r-script] modulu.</span><span class="sxs-lookup"><span data-stu-id="3f44d-269">Type hello `source()` function with your zip file name into hello code window for hello [Execute R Script][execute-r-script] module.</span></span> <span data-ttu-id="3f44d-270">V případě Moje zadali `source("src/simpleplot.R")`.</span><span class="sxs-lookup"><span data-stu-id="3f44d-270">In my case I typed `source("src/simpleplot.R")`.</span></span>  
7. <span data-ttu-id="3f44d-271">Ujistěte se, kliknete na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="3f44d-271">Make sure you click **Save**.</span></span>

<span data-ttu-id="3f44d-272">Po dokončení těchto kroků se hello [spustit skript jazyka R] [ execute-r-script] modul provede hello R skript v souboru zip hello při spuštění experimentu hello.</span><span class="sxs-lookup"><span data-stu-id="3f44d-272">Once these steps are complete, hello [Execute R Script][execute-r-script] module will execute hello R script in hello zip file when hello experiment is run.</span></span> <span data-ttu-id="3f44d-273">V tomto okamžiku by měl experimentu vypadat podobně jako obrázek 5.</span><span class="sxs-lookup"><span data-stu-id="3f44d-273">At this point your experiment should look something like Figure 5.</span></span>

![Experiment pomocí komprimované skript jazyka R][6]

<span data-ttu-id="3f44d-275">*Obrázek 5. Experiment pomocí komprimované R skript.*</span><span class="sxs-lookup"><span data-stu-id="3f44d-275">*Figure 5. Experiment using zipped R script.*</span></span>

#### <a name="dataset1"></a><span data-ttu-id="3f44d-276">Dataset1</span><span class="sxs-lookup"><span data-stu-id="3f44d-276">Dataset1</span></span>
<span data-ttu-id="3f44d-277">Obdélníková tabulku data tooyour R kód můžete předat pomocí hello Dataset1 vstup.</span><span class="sxs-lookup"><span data-stu-id="3f44d-277">You can pass a rectangular table of data tooyour R code by using hello Dataset1 input.</span></span> <span data-ttu-id="3f44d-278">V našich hello jednoduchého skriptu `maml.mapInputPort(1)` funkce čte hello data z portu 1.</span><span class="sxs-lookup"><span data-stu-id="3f44d-278">In our simple script hello `maml.mapInputPort(1)` function reads hello data from port 1.</span></span> <span data-ttu-id="3f44d-279">Tato data přiřazen název proměnné tooa dataframe ve vašem kódu.</span><span class="sxs-lookup"><span data-stu-id="3f44d-279">This data is then assigned tooa dataframe variable name in your code.</span></span> <span data-ttu-id="3f44d-280">V našem jednoduchého skriptu provede hello první řádek kódu hello přiřazení.</span><span class="sxs-lookup"><span data-stu-id="3f44d-280">In our simple script hello first line of code performs hello assignment.</span></span>

    cadairydata <- maml.mapInputPort(1)

<span data-ttu-id="3f44d-281">Spusťte experimentu tak, že kliknete na hello **spustit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3f44d-281">Execute your experiment by clicking on hello **Run** button.</span></span> <span data-ttu-id="3f44d-282">Po dokončení provádění hello, klikněte na hello [spustit skript jazyka R] [ execute-r-script] modul a potom klikněte na **zobrazit výstup protokol** v podokně Vlastnosti hello.</span><span class="sxs-lookup"><span data-stu-id="3f44d-282">When hello execution finishes, click on hello [Execute R Script][execute-r-script] module and then click **View output log** on hello properties pane.</span></span> <span data-ttu-id="3f44d-283">Nová stránka by se zobrazit v prohlížeči zobrazující hello obsah souboru souboru výstup.log hello.</span><span class="sxs-lookup"><span data-stu-id="3f44d-283">A new page should appear in your browser showing hello contents of hello output.log file.</span></span> <span data-ttu-id="3f44d-284">Když přejděte dolů by měl vidět něco podobného jako následující hello.</span><span class="sxs-lookup"><span data-stu-id="3f44d-284">When you scroll down you should see something like hello following.</span></span>

    [ModuleOutput] InputDataStructure
    [ModuleOutput]
    [ModuleOutput] {
    [ModuleOutput]  "InputName":Dataset1
    [ModuleOutput]  "Rows":228
    [ModuleOutput]  "Cols":9
    [ModuleOutput]  "ColumnTypes":System.Int32,3,System.Double,5,System.String,1
    [ModuleOutput] }

<span data-ttu-id="3f44d-285">Dále dolů hello stránka je podrobnější informace o hello sloupce, které bude vypadat podobně jako následující hello.</span><span class="sxs-lookup"><span data-stu-id="3f44d-285">Farther down hello page is more detailed information on hello columns, which will look something like hello following.</span></span>

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput]
    [ModuleOutput] 'data.frame':    228 obs. of  9 variables:
    [ModuleOutput]
    [ModuleOutput]  $ Column 0         : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput]
    [ModuleOutput]  $ Year.Month       : num  1995 1995 1995 1995 1995 ...
    [ModuleOutput]
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput]
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput]
    [ModuleOutput]  $ Month            : chr  "Jan" "Feb" "Mar" "Apr" ...
    [ModuleOutput]
    [ModuleOutput]  $ Cotagecheese.Prod: num  4.37 3.69 4.54 4.28 4.47 ...
    [ModuleOutput]
    [ModuleOutput]  $ Icecream.Prod    : num  51.6 56.1 68.5 65.7 73.7 ...
    [ModuleOutput]
    [ModuleOutput]  $ Milk.Prod        : num  2.11 1.93 2.16 2.13 2.23 ...
    [ModuleOutput]
    [ModuleOutput]  $ N.CA.Fat.Price   : num  0.98 0.892 0.892 0.897 0.897 ...

<span data-ttu-id="3f44d-286">Tyto výsledky se většinou podle očekávání, s 228 připomínky a 9 sloupců v hello dataframe.</span><span class="sxs-lookup"><span data-stu-id="3f44d-286">These results are mostly as expected, with 228 observations and 9 columns in hello dataframe.</span></span> <span data-ttu-id="3f44d-287">Jsme můžete zobrazit názvy sloupců hello, hello R datový typ a ukázku jednotlivých sloupců.</span><span class="sxs-lookup"><span data-stu-id="3f44d-287">We can see hello column names, hello R data type and a sample of each column.</span></span>

> [!NOTE]
> <span data-ttu-id="3f44d-288">Tento stejný tištěné výstup je pohodlně dostupná z hello R zařízení výstup hello [spustit skript jazyka R] [ execute-r-script] modulu.</span><span class="sxs-lookup"><span data-stu-id="3f44d-288">This same printed output is conveniently available from hello R Device output of hello [Execute R Script][execute-r-script] module.</span></span> <span data-ttu-id="3f44d-289">Podrobně probereme hello výstupy hello [spustit skript jazyka R] [ execute-r-script] modulu v další části hello.</span><span class="sxs-lookup"><span data-stu-id="3f44d-289">We will discuss hello outputs of hello [Execute R Script][execute-r-script] module in hello next section.</span></span>  
> 
> 

#### <a name="dataset2"></a><span data-ttu-id="3f44d-290">Dataset2</span><span class="sxs-lookup"><span data-stu-id="3f44d-290">Dataset2</span></span>
<span data-ttu-id="3f44d-291">Hello chování hello Dataset2 vstupu je identické toothat Dataset1.</span><span class="sxs-lookup"><span data-stu-id="3f44d-291">hello behavior of hello Dataset2 input is identical toothat of Dataset1.</span></span> <span data-ttu-id="3f44d-292">Pomocí tento vstup lze předat v druhé tabulce obdélníková dat do vašeho kódu jazyka R.</span><span class="sxs-lookup"><span data-stu-id="3f44d-292">Using this input you can pass a second rectangular table of data into your R code.</span></span> <span data-ttu-id="3f44d-293">Hello funkce `maml.mapInputPort(2)`, s argumentem hello 2, je použít toopass tato data.</span><span class="sxs-lookup"><span data-stu-id="3f44d-293">hello function `maml.mapInputPort(2)`, with hello argument 2, is used toopass this data.</span></span>  

### <a name="execute-r-script-outputs"></a><span data-ttu-id="3f44d-294">Spustit skript jazyka R výstupy</span><span class="sxs-lookup"><span data-stu-id="3f44d-294">Execute R Script outputs</span></span>
#### <a name="output-a-dataframe"></a><span data-ttu-id="3f44d-295">Výstup a dataframe</span><span class="sxs-lookup"><span data-stu-id="3f44d-295">Output a dataframe</span></span>
<span data-ttu-id="3f44d-296">Výstup můžete obsah hello dataframe R jako obdélníková tabulku přes port hello výsledek Dataset1 pomocí hello `maml.mapOutputPort()` funkce.</span><span class="sxs-lookup"><span data-stu-id="3f44d-296">You can output hello contents of an R dataframe as a rectangular table through hello Result Dataset1 port by using hello `maml.mapOutputPort()` function.</span></span> <span data-ttu-id="3f44d-297">V našem jednoduchý skript jazyka R se provádí pomocí hello následující řádek.</span><span class="sxs-lookup"><span data-stu-id="3f44d-297">In our simple R script this is performed by hello following line.</span></span>

    maml.mapOutputPort('cadairydata')

<span data-ttu-id="3f44d-298">Po spuštěné hello experiment, klikněte na hello výsledek Dataset1 výstupní port a potom klikněte na **vizualizovat**.</span><span class="sxs-lookup"><span data-stu-id="3f44d-298">After running hello experiment, click on hello Result Dataset1 output port and then click on **Visualize**.</span></span> <span data-ttu-id="3f44d-299">Měli byste vidět něco podobného jako obrázek 6.</span><span class="sxs-lookup"><span data-stu-id="3f44d-299">You should see something like Figure 6.</span></span>

![Hello vizualizaci hello výstup hello kalifornské dojnic dat][7]

<span data-ttu-id="3f44d-301">*Obrázek 6. Hello vizualizaci hello výstup hello kalifornské dojnic data.*</span><span class="sxs-lookup"><span data-stu-id="3f44d-301">*Figure 6. hello visualization of hello output of hello California dairy data.*</span></span>

<span data-ttu-id="3f44d-302">Tento výstup vypadá identické toohello vstup přesně tak, jak očekávali jsme.</span><span class="sxs-lookup"><span data-stu-id="3f44d-302">This output looks identical toohello input, exactly as we expected.</span></span>  

### <a name="r-device-output"></a><span data-ttu-id="3f44d-303">Výstup R zařízení</span><span class="sxs-lookup"><span data-stu-id="3f44d-303">R Device output</span></span>
<span data-ttu-id="3f44d-304">Hello zařízení výstup hello [spustit skript jazyka R] [ execute-r-script] modul obsahuje zprávy a grafický výstup.</span><span class="sxs-lookup"><span data-stu-id="3f44d-304">hello Device output of hello [Execute R Script][execute-r-script] module contains messages and graphics output.</span></span> <span data-ttu-id="3f44d-305">Standardní výstupní zařízení a standardní cíl chybové zprávy i z R jsou odesílány toohello R zařízení výstupní port.</span><span class="sxs-lookup"><span data-stu-id="3f44d-305">Both standard output and standard error messages from R are sent toohello R Device output port.</span></span>  

<span data-ttu-id="3f44d-306">tooview hello zařízení R výstup, klikněte na hello portu a potom na **vizualizovat**.</span><span class="sxs-lookup"><span data-stu-id="3f44d-306">tooview hello R Device output, click on hello port and then on **Visualize**.</span></span> <span data-ttu-id="3f44d-307">Vidíme hello standardní výstupní zařízení a standardní chyba ze skriptu hello R na obrázku 7.</span><span class="sxs-lookup"><span data-stu-id="3f44d-307">We see hello standard output and standard error from hello R script in Figure 7.</span></span>

![Standardní výstupní zařízení a standardní chyba z hello port zařízení R][8]

<span data-ttu-id="3f44d-309">*Na obrázku 7. Standardní výstupní zařízení a standardní chyba z hello R zařízení portu.*</span><span class="sxs-lookup"><span data-stu-id="3f44d-309">*Figure 7. Standard output and standard error from hello R Device port.*</span></span>

<span data-ttu-id="3f44d-310">Posouvání dolů jsme najdete v části hello grafického výstupu z našich skript jazyka R na obrázku 8.</span><span class="sxs-lookup"><span data-stu-id="3f44d-310">Scrolling down we see hello graphics output from our R script in Figure 8.</span></span>  

![Grafika výstup hello port zařízení R][9]

<span data-ttu-id="3f44d-312">*Obrázek 8. Grafika výstup z hello R zařízení portu.*</span><span class="sxs-lookup"><span data-stu-id="3f44d-312">*Figure 8. Graphics output from hello R Device port.*</span></span>  

## <span data-ttu-id="3f44d-313"><a id="filtering"></a>Filtrování dat a transformace</span><span class="sxs-lookup"><span data-stu-id="3f44d-313"><a id="filtering"></a>Data filtering and transformation</span></span>
<span data-ttu-id="3f44d-314">V této části provedeme některé základní data filtrování a operace transformace na hello kalifornské dojnic data.</span><span class="sxs-lookup"><span data-stu-id="3f44d-314">In this section we will perform some basic data filtering and transformation operations on hello California dairy data.</span></span> <span data-ttu-id="3f44d-315">Hello konci této části máme data ve formátu, který je vhodný pro sestavování analytického modelu.</span><span class="sxs-lookup"><span data-stu-id="3f44d-315">By hello end of this section we will have data in a format suitable for building an analytic model.</span></span>  

<span data-ttu-id="3f44d-316">Přesněji řečeno, v této části provedeme několik běžných čištění a transformace úlohy dat: zadejte transformaci, filtrování dataframes, přidání nové počítaných sloupcích a hodnota transformace.</span><span class="sxs-lookup"><span data-stu-id="3f44d-316">More specifically, in this section we will perform several common data cleaning and transformation tasks: type transformation, filtering on dataframes, adding new computed columns, and value transformations.</span></span> <span data-ttu-id="3f44d-317">Tato pozadí pomáhají řešit hello mnoho variant v reálných problémy.</span><span class="sxs-lookup"><span data-stu-id="3f44d-317">This background should help you deal with hello many variations encountered in real-world problems.</span></span>

<span data-ttu-id="3f44d-318">Hello dokončení R kód pro tento oddíl je k dispozici v souboru zip hello, které jste dříve stáhli.</span><span class="sxs-lookup"><span data-stu-id="3f44d-318">hello complete R code for this section is available in hello zip file you downloaded earlier.</span></span>

### <a name="type-transformations"></a><span data-ttu-id="3f44d-319">Typ transformace</span><span class="sxs-lookup"><span data-stu-id="3f44d-319">Type transformations</span></span>
<span data-ttu-id="3f44d-320">Teď, když jsme můžete číst data dojnic kalifornské hello do kódu hello R v hello [spustit skript jazyka R] [ execute-r-script] modulu, potřebujeme tooensure, že má hello data ve sloupcích hello hello určený typ a formát.</span><span class="sxs-lookup"><span data-stu-id="3f44d-320">Now that we can read hello California dairy data into hello R code in hello [Execute R Script][execute-r-script] module, we need tooensure that hello data in hello columns has hello intended type and format.</span></span>  

<span data-ttu-id="3f44d-321">R je dynamicky zadávaných jazyk, což znamená, že datové typy jsou má z jednoho tooanother podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="3f44d-321">R is a dynamically typed language, which means that data types are coerced from one tooanother as required.</span></span> <span data-ttu-id="3f44d-322">Hello atomic datových typů v R zahrnují číselné literály, logické a znak.</span><span class="sxs-lookup"><span data-stu-id="3f44d-322">hello atomic data types in R include numeric, logical and character.</span></span> <span data-ttu-id="3f44d-323">typ multi-Factor Hello je použité toocompactly úložiště kategorizovaná data.</span><span class="sxs-lookup"><span data-stu-id="3f44d-323">hello factor type is used toocompactly store categorical data.</span></span> <span data-ttu-id="3f44d-324">Mnohem Další informace o typech dat najdete v odkazech na hello v [příloha B – další čtení](#appendixb).</span><span class="sxs-lookup"><span data-stu-id="3f44d-324">You can find much more information on data types in hello references in [Appendix B - Further reading](#appendixb).</span></span>

<span data-ttu-id="3f44d-325">Pokud tabulková data je pro čtení do R z externího zdroje, je vždy vhodné toocheck hello výsledná typy ve sloupcích hello.</span><span class="sxs-lookup"><span data-stu-id="3f44d-325">When tabular data is read into R from an external source, it is always a good idea toocheck hello resulting types in hello columns.</span></span> <span data-ttu-id="3f44d-326">Může být vhodné sloupec – znak typu, ale v mnoha případech to se zobrazí jako faktor nebo naopak.</span><span class="sxs-lookup"><span data-stu-id="3f44d-326">You may want a column of type character, but in many cases this will show up as factor or vice versa.</span></span> <span data-ttu-id="3f44d-327">V ostatních případech a sloupec, který si myslíte, že by měl být číselný je reprezentována textová data, například '1,23' než 1,23 jako plovoucí bodu číslo.</span><span class="sxs-lookup"><span data-stu-id="3f44d-327">In other cases a column you think should be numeric is represented by character data, e.g. '1.23' rather than 1.23 as a floating point number.</span></span>  

<span data-ttu-id="3f44d-328">Naštěstí je snadno tooconvert jeden typ tooanother tak dlouho, dokud je možné mapování.</span><span class="sxs-lookup"><span data-stu-id="3f44d-328">Fortunately, it is easy tooconvert one type tooanother, as long as mapping is possible.</span></span> <span data-ttu-id="3f44d-329">Například 'Nevada' nelze převést číselnou hodnotu, ale můžete ji převést tooa faktor (kategorií proměnné).</span><span class="sxs-lookup"><span data-stu-id="3f44d-329">For example, you cannot convert 'Nevada' into a numeric value, but you can convert it tooa factor (categorical variable).</span></span> <span data-ttu-id="3f44d-330">Například můžete převést číselnou 1 znak '1' nebo koeficient.</span><span class="sxs-lookup"><span data-stu-id="3f44d-330">As another example, you can convert a numeric 1 into a character '1' or a factor.</span></span>  

<span data-ttu-id="3f44d-331">Hello syntaxe pro některý z těchto převody je jednoduchý: `as.datatype()`.</span><span class="sxs-lookup"><span data-stu-id="3f44d-331">hello syntax for any of these conversions is simple: `as.datatype()`.</span></span> <span data-ttu-id="3f44d-332">Tyto funkce pro převod typů zahrnují následující hello.</span><span class="sxs-lookup"><span data-stu-id="3f44d-332">These type conversion functions include hello following.</span></span>

* `as.numeric()`
* `as.character()`
* `as.logical()`
* `as.factor()`

<span data-ttu-id="3f44d-333">Prohlížení hello datové typy sloupců hello zadáme v předchozí části hello: jsou všechny sloupce typu číselné, s výjimkou hello sloupec s názvem 'Měsíc', který je – znak typu.</span><span class="sxs-lookup"><span data-stu-id="3f44d-333">Looking at hello data types of hello columns we input in hello previous section: all columns are of type numeric, except for hello column labeled 'Month', which is of type character.</span></span> <span data-ttu-id="3f44d-334">Umožňuje převést tento faktor tooa a hello výsledky testu.</span><span class="sxs-lookup"><span data-stu-id="3f44d-334">Let's convert this tooa factor and test hello results.</span></span>  

<span data-ttu-id="3f44d-335">Odstraněný hello řádek, který vytvoří hello scatterplot matice a přidat řádek převádění Multi-Factor tooa sloupec "Měsíc" hello.</span><span class="sxs-lookup"><span data-stu-id="3f44d-335">I have deleted hello line that created hello scatterplot matrix and added a line converting hello 'Month' column tooa factor.</span></span> <span data-ttu-id="3f44d-336">V experimentu se právě kopírování a vkládání hello R kódu do kódu okno hello hello [spustit skript jazyka R] [ execute-r-script] modulu.</span><span class="sxs-lookup"><span data-stu-id="3f44d-336">In my experiment I will just cut and paste hello R code into hello code window of hello [Execute R Script][execute-r-script] Module.</span></span> <span data-ttu-id="3f44d-337">Můžete také aktualizovat soubor zip hello a nahrajte ho tooAzure Machine Learning Studio, ale tato akce trvá několik kroků.</span><span class="sxs-lookup"><span data-stu-id="3f44d-337">You could also update hello zip file and upload it tooAzure Machine Learning Studio, but this takes several steps.</span></span>  

    ## Only one of hello following two lines should be used
    ## If running in Machine Learning Studio, use hello first line with maml.mapInputPort()
    ## If in RStudio, use hello second line with read.csv()
    cadairydata <- maml.mapInputPort(1)
    # cadairydata  <- read.csv("cadairydata.csv", header = TRUE, stringsAsFactors = FALSE)
    ## Ensure hello coding is consistent and convert column tooa factor
    cadairydata$Month <- as.factor(cadairydata$Month)
    str(cadairydata) # Check hello result
    ## hello following line should be executed only when running in
    ## Azure Machine Learning Studio
    maml.mapOutputPort('cadairydata')

<span data-ttu-id="3f44d-338">Umožňuje spustit tento kód a podívejte se na výstup protokolu hello hello R skript.</span><span class="sxs-lookup"><span data-stu-id="3f44d-338">Let's execute this code and look at hello output log for hello R script.</span></span> <span data-ttu-id="3f44d-339">Hello relevantní data z protokolu hello je vidět na obrázku 9.</span><span class="sxs-lookup"><span data-stu-id="3f44d-339">hello relevant data from hello log is shown in Figure 9.</span></span>

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  9 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Column 0         : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year.Month       : num  1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 14 levels "Apr","April",..: 6 5 9 1 11 8 7 3 14 13 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  4.37 3.69 4.54 4.28 4.47 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  51.6 56.1 68.5 65.7 73.7 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  2.11 1.93 2.16 2.13 2.23 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  0.98 0.892 0.892 0.897 0.897 ...
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving variable  cadairydata  ..."
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving hello following item(s):  .maml.oport1"

<span data-ttu-id="3f44d-340">*Obrázek 9. Shrnutí hello dataframe s Multi-Factor proměnné.*</span><span class="sxs-lookup"><span data-stu-id="3f44d-340">*Figure 9. Summary of hello dataframe with a factor variable.*</span></span>

<span data-ttu-id="3f44d-341">Nyní by mělo být uvedeno Hello typ pro měsíc '**Multi-Factor s 14 úrovně**'.</span><span class="sxs-lookup"><span data-stu-id="3f44d-341">hello type for Month should now say '**Factor w/ 14 levels**'.</span></span> <span data-ttu-id="3f44d-342">Tento problém je vzhledem k tomu, že existují pouze po dobu 12 měsíců v roce hello.</span><span class="sxs-lookup"><span data-stu-id="3f44d-342">This is a problem since there are only 12 months in hello year.</span></span> <span data-ttu-id="3f44d-343">Můžete také zkontrolovat toosee, který hello typu v **vizualizovat** hello datovou sadu výsledků je port,**Categorical**'.</span><span class="sxs-lookup"><span data-stu-id="3f44d-343">You can also check toosee that hello type in **Visualize** of hello Result Dataset port is '**Categorical**'.</span></span>

<span data-ttu-id="3f44d-344">Hello problém je, že hello sloupec nebyl byla programového systematičtěji měsíc.</span><span class="sxs-lookup"><span data-stu-id="3f44d-344">hello problem is that hello 'Month' column has not been coded systematically.</span></span> <span data-ttu-id="3f44d-345">V některých případech se nazývá měsíc duben a k ostatním se zkracuje dubna. Ořízne hello řetězec too3 znaků jsme můžete tento problém vyřešit.</span><span class="sxs-lookup"><span data-stu-id="3f44d-345">In some cases a month is called April and in others it is abbreviated as Apr. We can solve this problem by trimming hello string too3 characters.</span></span> <span data-ttu-id="3f44d-346">Hello řádek kódu teď vypadá hello následující:</span><span class="sxs-lookup"><span data-stu-id="3f44d-346">hello line of code now looks like hello following:</span></span>

    ## Ensure hello coding is consistent and convert column tooa factor
    cadairydata$Month <- as.factor(substr(cadairydata$Month, 1, 3))

<span data-ttu-id="3f44d-347">Spusťte hello experiment a zobrazit protokol výstup hello.</span><span class="sxs-lookup"><span data-stu-id="3f44d-347">Rerun hello experiment and view hello output log.</span></span> <span data-ttu-id="3f44d-348">Hello očekává, že výsledky se zobrazí na obrázku 10.</span><span class="sxs-lookup"><span data-stu-id="3f44d-348">hello expected results are shown in Figure 10.</span></span>  

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  9 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Column 0         : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year.Month       : num  1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  4.37 3.69 4.54 4.28 4.47 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  51.6 56.1 68.5 65.7 73.7 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  2.11 1.93 2.16 2.13 2.23 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  0.98 0.892 0.892 0.897 0.897 ...
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving variable  cadairydata  ..."
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving hello following item(s):  .maml.oport1"

<span data-ttu-id="3f44d-349">*Obrázek 10. Shrnutí hello dataframe s správný počet úrovní faktor.*</span><span class="sxs-lookup"><span data-stu-id="3f44d-349">*Figure 10. Summary of hello dataframe with correct number of factor levels.*</span></span>

<span data-ttu-id="3f44d-350">Naše Multi-Factor proměnná teď má hello potřeby 12 úrovně.</span><span class="sxs-lookup"><span data-stu-id="3f44d-350">Our factor variable now has hello desired 12 levels.</span></span>

### <a name="basic-data-frame-filtering"></a><span data-ttu-id="3f44d-351">Filtrování rámce základní data</span><span class="sxs-lookup"><span data-stu-id="3f44d-351">Basic data frame filtering</span></span>
<span data-ttu-id="3f44d-352">R dataframes podporovat výkonné možnosti filtrování.</span><span class="sxs-lookup"><span data-stu-id="3f44d-352">R dataframes support powerful filtering capabilities.</span></span> <span data-ttu-id="3f44d-353">Datové sady, může být podsady pomocí logických filtry na řádky nebo sloupce.</span><span class="sxs-lookup"><span data-stu-id="3f44d-353">Datasets can be subsetted by using logical filters on either rows or columns.</span></span> <span data-ttu-id="3f44d-354">V mnoha případech se bude vyžadovat složitá kritéria filtru.</span><span class="sxs-lookup"><span data-stu-id="3f44d-354">In many cases, complex filter criteria will be required.</span></span> <span data-ttu-id="3f44d-355">odkazy na Hello v [příloha B – další čtení](#appendixb) obsahovat rozsáhlé příklady filtrování dataframes.</span><span class="sxs-lookup"><span data-stu-id="3f44d-355">hello references in [Appendix B - Further reading](#appendixb) contain extensive examples of filtering dataframes.</span></span>  

<span data-ttu-id="3f44d-356">Je jeden bit filtrování by měl provedeme v naší datové sadě.</span><span class="sxs-lookup"><span data-stu-id="3f44d-356">There is one bit of filtering we should do on our dataset.</span></span> <span data-ttu-id="3f44d-357">Pokud se podíváte na hello sloupců v hello cadairydata dataframe, zobrazí se dva nepotřebných sloupců.</span><span class="sxs-lookup"><span data-stu-id="3f44d-357">If you look at hello columns in hello cadairydata dataframe, you will see two unnecessary columns.</span></span> <span data-ttu-id="3f44d-358">Hello první sloupec obsahuje jenom číslo řádku, které není velmi užitečné.</span><span class="sxs-lookup"><span data-stu-id="3f44d-358">hello first column just holds a row number, which is not very useful.</span></span> <span data-ttu-id="3f44d-359">druhý sloupec Hello Year.Month, obsahuje redundantní informace.</span><span class="sxs-lookup"><span data-stu-id="3f44d-359">hello second column, Year.Month, contains redundant information.</span></span> <span data-ttu-id="3f44d-360">Tyto sloupce jsme snadno můžete vyloučit pomocí následující kód R hello.</span><span class="sxs-lookup"><span data-stu-id="3f44d-360">We can easily exclude these columns by using hello following R code.</span></span>

> [!NOTE]
> <span data-ttu-id="3f44d-361">Od teď na v této části, bude jenom zobrazit můžete hello další kód přidám v hello [spustit skript jazyka R] [ execute-r-script] modulu.</span><span class="sxs-lookup"><span data-stu-id="3f44d-361">From now on in this section, I will just show you hello additional code I am adding in hello [Execute R Script][execute-r-script] module.</span></span> <span data-ttu-id="3f44d-362">Přidat každý nový řádek **před** hello `str()` funkce.</span><span class="sxs-lookup"><span data-stu-id="3f44d-362">I will add each new line **before** hello `str()` function.</span></span> <span data-ttu-id="3f44d-363">Tato funkce tooverify používám v Azure Machine Learning Studio Moje výsledky.</span><span class="sxs-lookup"><span data-stu-id="3f44d-363">I use this function tooverify my results in Azure Machine Learning Studio.</span></span>
> 
> 

<span data-ttu-id="3f44d-364">Přidat následující řádek kódu toomy R v hello hello [spustit skript jazyka R] [ execute-r-script] modulu.</span><span class="sxs-lookup"><span data-stu-id="3f44d-364">I add hello following line toomy R code in hello [Execute R Script][execute-r-script] module.</span></span>

    # Remove two columns we do not need
    cadairydata <- cadairydata[, c(-1, -2)]

<span data-ttu-id="3f44d-365">Spusťte tento kód v experimentu a zkontrolujte výsledek hello z protokolu výstup hello.</span><span class="sxs-lookup"><span data-stu-id="3f44d-365">Run this code in your experiment and check hello result from hello output log.</span></span> <span data-ttu-id="3f44d-366">Tyto výsledky se zobrazí v obrázek 11.</span><span class="sxs-lookup"><span data-stu-id="3f44d-366">These results are shown in Figure 11.</span></span>

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  7 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  4.37 3.69 4.54 4.28 4.47 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  51.6 56.1 68.5 65.7 73.7 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  2.11 1.93 2.16 2.13 2.23 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  0.98 0.892 0.892 0.897 0.897 ...
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving variable  cadairydata  ..."
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving hello following item(s):  .maml.oport1"

<span data-ttu-id="3f44d-367">*Obrázek 11. Souhrn dataframe hello se dvěma sloupci odebrat.*</span><span class="sxs-lookup"><span data-stu-id="3f44d-367">*Figure 11. Summary of hello dataframe with two columns removed.*</span></span>

<span data-ttu-id="3f44d-368">Dobrá zpráva!</span><span class="sxs-lookup"><span data-stu-id="3f44d-368">Good news!</span></span> <span data-ttu-id="3f44d-369">Nemůžeme získat hello očekávané výsledky.</span><span class="sxs-lookup"><span data-stu-id="3f44d-369">We get hello expected results.</span></span>

### <a name="add-a-new-column"></a><span data-ttu-id="3f44d-370">Umožňuje přidat nový sloupec</span><span class="sxs-lookup"><span data-stu-id="3f44d-370">Add a new column</span></span>
<span data-ttu-id="3f44d-371">toocreate časové řady modely bude vhodné toohave sloupec obsahující hello měsíců od začátku hello hello časové řady.</span><span class="sxs-lookup"><span data-stu-id="3f44d-371">toocreate time series models it will be convenient toohave a column containing hello months since hello start of hello time series.</span></span> <span data-ttu-id="3f44d-372">Vytvoříme nový sloupec, Month.Count'.</span><span class="sxs-lookup"><span data-stu-id="3f44d-372">We will create a new column 'Month.Count'.</span></span>

<span data-ttu-id="3f44d-373">toohelp uspořádání hello kód vytvoříme naše první jednoduché funkce `num.month()`.</span><span class="sxs-lookup"><span data-stu-id="3f44d-373">toohelp organize hello code we will create our first simple function, `num.month()`.</span></span> <span data-ttu-id="3f44d-374">Pak budou použity této funkce toocreate nový sloupec v hello dataframe.</span><span class="sxs-lookup"><span data-stu-id="3f44d-374">We will then apply this function toocreate a new column in hello dataframe.</span></span> <span data-ttu-id="3f44d-375">nový kód Hello je následující.</span><span class="sxs-lookup"><span data-stu-id="3f44d-375">hello new code is as follows.</span></span>

    ## Create a new column with hello month count
    ## Function toofind hello number of months from hello first
    ## month of hello time series
    num.month <- function(Year, Month) {
      ## Find hello starting year
      min.year  <- min(Year)

      ## Compute hello number of months from hello start of hello time series
      12 * (Year - min.year) + Month - 1
    }

    ## Compute hello new column for hello dataframe
    cadairydata$Month.Count <- num.month(cadairydata$Year, cadairydata$Month.Number)

<span data-ttu-id="3f44d-376">Teď spustit experiment hello aktualizovat a použít hello výstup protokolu tooview hello výsledky.</span><span class="sxs-lookup"><span data-stu-id="3f44d-376">Now run hello updated experiment and use hello output log tooview hello results.</span></span> <span data-ttu-id="3f44d-377">Tyto výsledky se zobrazí obrázek 12.</span><span class="sxs-lookup"><span data-stu-id="3f44d-377">These results are shown in Figure 12.</span></span>

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  8 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  4.37 3.69 4.54 4.28 4.47 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  51.6 56.1 68.5 65.7 73.7 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  2.11 1.93 2.16 2.13 2.23 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  0.98 0.892 0.892 0.897 0.897 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Count      : num  0 1 2 3 4 5 6 7 8 9 ...
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving variable  cadairydata  ..."
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving hello following item(s):  .maml.oport1"

<span data-ttu-id="3f44d-378">*Obrázek 12. Shrnutí hello dataframe hello další sloupec.*</span><span class="sxs-lookup"><span data-stu-id="3f44d-378">*Figure 12. Summary of hello dataframe with hello additional column.*</span></span>

<span data-ttu-id="3f44d-379">Vypadá to vše funguje.</span><span class="sxs-lookup"><span data-stu-id="3f44d-379">It looks like everything is working.</span></span> <span data-ttu-id="3f44d-380">V našem dataframe máme hello nový sloupec s hello očekávaných hodnot.</span><span class="sxs-lookup"><span data-stu-id="3f44d-380">We have hello new column with hello expected values in our dataframe.</span></span>

### <a name="value-transformations"></a><span data-ttu-id="3f44d-381">Hodnota transformace</span><span class="sxs-lookup"><span data-stu-id="3f44d-381">Value transformations</span></span>
<span data-ttu-id="3f44d-382">V této části provedeme některé jednoduché transformace na hello hodnoty v některé z našich dataframe hello sloupce.</span><span class="sxs-lookup"><span data-stu-id="3f44d-382">In this section we will perform some simple transformations on hello values in some of hello columns of our dataframe.</span></span> <span data-ttu-id="3f44d-383">Hello R jazyk podporuje transformace téměř libovolná hodnota.</span><span class="sxs-lookup"><span data-stu-id="3f44d-383">hello R language supports nearly arbitrary value transformations.</span></span> <span data-ttu-id="3f44d-384">odkazy na Hello v [příloha B – další čtení](#appendixb) obsahovat rozsáhlé příklady.</span><span class="sxs-lookup"><span data-stu-id="3f44d-384">hello references in [Appendix B - Further Reading](#appendixb) contain extensive examples.</span></span>

<span data-ttu-id="3f44d-385">Pokud si prohlédnete hello hodnoty v hello souhrnných informací o našem dataframe měli byste vidět něco liché sem.</span><span class="sxs-lookup"><span data-stu-id="3f44d-385">If you look at hello values in hello summaries of our dataframe you should see something odd here.</span></span> <span data-ttu-id="3f44d-386">Další zmrzlinová než mléka vytváří v kalifornské?</span><span class="sxs-lookup"><span data-stu-id="3f44d-386">Is more ice cream than milk produced in California?</span></span> <span data-ttu-id="3f44d-387">Ne, samozřejmě není, jak to nemá smysl sad jako tento fakt může být toosome nás zmrzlinová lovers.</span><span class="sxs-lookup"><span data-stu-id="3f44d-387">No, of course not, as this makes no sense, sad as this fact may be toosome of us ice cream lovers.</span></span> <span data-ttu-id="3f44d-388">jednotky Hello se liší.</span><span class="sxs-lookup"><span data-stu-id="3f44d-388">hello units are different.</span></span> <span data-ttu-id="3f44d-389">cena Hello je jednotek nám libra mléka je v jednotkách 1 milion libra USA, zmrzlinová je v jednotkách 1 000 nám galony a byt sýr je v jednotkách 1 000 libra USA.</span><span class="sxs-lookup"><span data-stu-id="3f44d-389">hello price is in units of US pounds, milk is in units of 1 M US pounds, ice cream is in units of 1,000 US gallons, and cottage cheese is in units of 1,000 US pounds.</span></span> <span data-ttu-id="3f44d-390">Za předpokladu, že zmrzlinová váží asi 6,5 libra za spotřeby, můžeme snadno hello tyto hodnoty tak, aby byly všechny ve stejné jednotky 1000 libra tooconvert násobení.</span><span class="sxs-lookup"><span data-stu-id="3f44d-390">Assuming ice cream weighs about 6.5 pounds per gallon, we can easily do hello multiplication tooconvert these values so they are all in equal units of 1,000 pounds.</span></span>

<span data-ttu-id="3f44d-391">Pro náš model prognózy používáme multiplikativní model pro trendu a sezónní úpravu tato data.</span><span class="sxs-lookup"><span data-stu-id="3f44d-391">For our forecasting model we use a multiplicative model for trend and seasonal adjustment of this data.</span></span> <span data-ttu-id="3f44d-392">Transformace protokolu nám umožňuje toouse model lineární ke zjednodušení tohoto procesu.</span><span class="sxs-lookup"><span data-stu-id="3f44d-392">A log transformation allows us toouse a linear model, simplifying this process.</span></span> <span data-ttu-id="3f44d-393">Použijeme hello protokolu transformací v hello stejnou funkci, kdy se používá násobitel hello.</span><span class="sxs-lookup"><span data-stu-id="3f44d-393">We can apply hello log transformation in hello same function where hello multiplier is applied.</span></span>

<span data-ttu-id="3f44d-394">V hello následující kód, lze definovat novou funkci, `log.transform()`a použijte ho toohello řádky obsahující hello číselné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="3f44d-394">In hello following code, I define a new function, `log.transform()`, and apply it toohello rows containing hello numerical values.</span></span> <span data-ttu-id="3f44d-395">Hello R `Map()` funkce je použité tooapply hello `log.transform()` funkce toohello vybrané sloupce hello dataframe.</span><span class="sxs-lookup"><span data-stu-id="3f44d-395">hello R `Map()` function is used tooapply hello `log.transform()` function toohello selected columns of hello dataframe.</span></span> <span data-ttu-id="3f44d-396">`Map()`je příliš podobné`apply()` , ale umožňuje více než jeden seznam argumentů toohello funkce.</span><span class="sxs-lookup"><span data-stu-id="3f44d-396">`Map()` is similar too`apply()` but allows for more than one list of arguments toohello function.</span></span> <span data-ttu-id="3f44d-397">Všimněte si, že poskytuje seznam multiplikátory hello druhý argument toohello `log.transform()` funkce.</span><span class="sxs-lookup"><span data-stu-id="3f44d-397">Note that a list of multipliers supplies hello second argument toohello `log.transform()` function.</span></span> <span data-ttu-id="3f44d-398">Hello `na.omit()` funkce slouží jako trochu z čištění tooensure nemáme chybí nebo nedefinované hodnoty v hello dataframe.</span><span class="sxs-lookup"><span data-stu-id="3f44d-398">hello `na.omit()` function is used as a bit of cleanup tooensure we do not have missing or undefined values in hello dataframe.</span></span>

    log.transform <- function(invec, multiplier = 1) {
      ## Function for hello transformation, which is hello log
      ## of hello input value times a multiplier

      warningmessages <- c("ERROR: Non-numeric argument encountered in function log.transform",
                           "ERROR: Arguments toofunction log.transform must be greate than zero",
                           "ERROR: Aggurment multiplier toofuncition log.transform must be a scaler",
                           "ERROR: Invalid time seies value encountered in function log.transform"
                           )

      ## Check hello input arguments
      if(!is.numeric(invec) | !is.numeric(multiplier)) {warning(warningmessages[1]); return(NA)}  
      if(any(invec < 0.0) | any(multiplier < 0.0)) {warning(warningmessages[2]); return(NA)}
      if(length(multiplier) != 1) {{warning(warningmessages[3]); return(NA)}}

      ## Wrap hello multiplication in tryCatch
      ## If there is an exception, print hello warningmessage to
      ## standard error and return NA
      tryCatch(log(multiplier * invec),
               error = function(e){warning(warningmessages[4]); NA})
    }


    ## Apply hello transformation function toohello 4 columns
    ## of hello dataframe with production data
    multipliers  <- list(1.0, 6.5, 1000.0, 1000.0)
    cadairydata[, 4:7] <- Map(log.transform, cadairydata[, 4:7], multipliers)

    ## Get rid of any rows with NA values
    cadairydata <- na.omit(cadairydata)  

<span data-ttu-id="3f44d-399">Je s bit situaci v hello `log.transform()` funkce.</span><span class="sxs-lookup"><span data-stu-id="3f44d-399">There is quite a bit happening in hello `log.transform()` function.</span></span> <span data-ttu-id="3f44d-400">Většina tohoto kódu je kontrola potenciálních problémů spojených s argumenty hello nebo týkající se výjimky, které mohou stále nastat během hello výpočty.</span><span class="sxs-lookup"><span data-stu-id="3f44d-400">Most of this code is checking for potential problems with hello arguments or dealing with exceptions, which can still arise during hello computations.</span></span> <span data-ttu-id="3f44d-401">Tento kód jenom pár řádků ve skutečnosti hello výpočty.</span><span class="sxs-lookup"><span data-stu-id="3f44d-401">Only a few lines of this code actually do hello computations.</span></span>

<span data-ttu-id="3f44d-402">cílem Hello Obranným programování hello je tooprevent hello selhání jedné funkce, která zabraňuje zpracování pokračovat.</span><span class="sxs-lookup"><span data-stu-id="3f44d-402">hello goal of hello defensive programming is tooprevent hello failure of a single function that prevents processing from continuing.</span></span> <span data-ttu-id="3f44d-403">K náhlému selhání dlouho běžící analysis může být poměrně frustrující pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="3f44d-403">An abrupt failure of a long-running analysis can be quite frustrating for users.</span></span> <span data-ttu-id="3f44d-404">tooavoid, které se této situaci, výchozí, musí být zvolena návratové hodnoty, které omezí poškodit toodownstream zpracování.</span><span class="sxs-lookup"><span data-stu-id="3f44d-404">tooavoid this situation, default return values must be chosen that will limit damage toodownstream processing.</span></span> <span data-ttu-id="3f44d-405">Zpráva je také vytvořené tooalert uživatele, kteří se něco pokazilo přešel.</span><span class="sxs-lookup"><span data-stu-id="3f44d-405">A message is also produced tooalert users that something has gone wrong.</span></span>

<span data-ttu-id="3f44d-406">Pokud si nejste použité toodefensive programování v R, tento kód se může zdát trochu čtenáře.</span><span class="sxs-lookup"><span data-stu-id="3f44d-406">If you are not used toodefensive programming in R, all this code may seem a bit overwhelming.</span></span> <span data-ttu-id="3f44d-407">I vás provede důležitými kroky hello:</span><span class="sxs-lookup"><span data-stu-id="3f44d-407">I will walk you through hello major steps:</span></span>

1. <span data-ttu-id="3f44d-408">Vektor čtyři zprávy je definována.</span><span class="sxs-lookup"><span data-stu-id="3f44d-408">A vector of four messages is defined.</span></span> <span data-ttu-id="3f44d-409">Tyto zprávy jsou použité toocommunicate informace o některých hello možné chyby a výjimky, které se můžou vyskytnout s tímto kódem.</span><span class="sxs-lookup"><span data-stu-id="3f44d-409">These messages are used toocommunicate information about some of hello possible errors and exceptions that can occur with this code.</span></span>
2. <span data-ttu-id="3f44d-410">Návratu NA hodnotu v obou případech.</span><span class="sxs-lookup"><span data-stu-id="3f44d-410">I return a value of NA for each case.</span></span> <span data-ttu-id="3f44d-411">Existuje mnoho dalších možností, které by mohly mít méně vedlejší účinky.</span><span class="sxs-lookup"><span data-stu-id="3f44d-411">There are many other possibilities that might have fewer side effects.</span></span> <span data-ttu-id="3f44d-412">Vektor nul nebo hello původní vstupní vektoru, může například návratu.</span><span class="sxs-lookup"><span data-stu-id="3f44d-412">I could return a vector of zeroes, or hello original input vector, for example.</span></span>
3. <span data-ttu-id="3f44d-413">Kontroluje se spouštějí na hello argumenty toohello funkce.</span><span class="sxs-lookup"><span data-stu-id="3f44d-413">Checks are run on hello arguments toohello function.</span></span> <span data-ttu-id="3f44d-414">V každém případě pokud je detekována chyba, je vrácen výchozí hodnotu a zprávy je produkovaný hello `warning()` funkce.</span><span class="sxs-lookup"><span data-stu-id="3f44d-414">In each case, if an error is detected, a default value is returned and a message is produced by hello `warning()` function.</span></span> <span data-ttu-id="3f44d-415">Používám `warning()` místo `stop()` jako hello pozdější ukončí provádění, přesně co pokouším tooavoid.</span><span class="sxs-lookup"><span data-stu-id="3f44d-415">I am using `warning()` rather than `stop()` as hello latter will terminate execution, exactly what I am trying tooavoid.</span></span> <span data-ttu-id="3f44d-416">Všimněte si, že I mají zapisovat tento kód v procedurální styl, jako v tomto případě funkční přístup mailech vypadalo komplexní a skrytého.</span><span class="sxs-lookup"><span data-stu-id="3f44d-416">Note that I have written this code in a procedural style, as in this case a functional approach seemed complex and obscure.</span></span>
4. <span data-ttu-id="3f44d-417">výpočty protokolu Hello je uzavřen do `tryCatch()` tak, aby výjimky nezpůsobí tooprocessing náhlému zastavení.</span><span class="sxs-lookup"><span data-stu-id="3f44d-417">hello log computations are wrapped in `tryCatch()` so that exceptions will not cause an abrupt halt tooprocessing.</span></span> <span data-ttu-id="3f44d-418">Bez `tryCatch()` aktivováno R funkce povede signál k zastavení, která zajišťuje právě, většina chyb.</span><span class="sxs-lookup"><span data-stu-id="3f44d-418">Without `tryCatch()` most errors raised by R functions result in a stop signal, which does just that.</span></span>

<span data-ttu-id="3f44d-419">Spustit tento kód R v experimentu a podívejte se na hello vytisknout výstup do souboru souboru výstup.log hello.</span><span class="sxs-lookup"><span data-stu-id="3f44d-419">Execute this R code in your experiment and have a look at hello printed output in hello output.log file.</span></span> <span data-ttu-id="3f44d-420">Nyní uvidíte hello transformuje hodnoty hello čtyři sloupce v hello přihlašovat, jak ukazuje obrázek 13.</span><span class="sxs-lookup"><span data-stu-id="3f44d-420">You will now see hello transformed values of hello four columns in hello log, as shown in Figure 13.</span></span>

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  8 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  1.47 1.31 1.51 1.45 1.5 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  5.82 5.9 6.1 6.06 6.17 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  7.66 7.57 7.68 7.66 7.71 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  6.89 6.79 6.79 6.8 6.8 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Count      : num  0 1 2 3 4 5 6 7 8 9 ...
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving variable  cadairydata  ..."
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving hello following item(s):  .maml.oport1"

<span data-ttu-id="3f44d-421">*Obrázek 13. Souhrn hello transformuje hodnoty v hello dataframe.*</span><span class="sxs-lookup"><span data-stu-id="3f44d-421">*Figure 13. Summary of hello transformed values in hello dataframe.*</span></span>

<span data-ttu-id="3f44d-422">Vidíte, že byly transformovány hello hodnoty.</span><span class="sxs-lookup"><span data-stu-id="3f44d-422">We see hello values have been transformed.</span></span> <span data-ttu-id="3f44d-423">Nyní produkce mléka výrazně přesahuje všechny ostatní mléčný výrobek produkční, vrací, že jsme teď vyhledávání škálované protokolu.</span><span class="sxs-lookup"><span data-stu-id="3f44d-423">Milk production now greatly exceeds all other dairy product production, recalling that we are now looking at a log scale.</span></span>

<span data-ttu-id="3f44d-424">V tomto okamžiku je naše data vyčištěna a jsme připraveni pro některé modelování.</span><span class="sxs-lookup"><span data-stu-id="3f44d-424">At this point our data is cleaned up and we are ready for some modeling.</span></span> <span data-ttu-id="3f44d-425">Prohlížení hello vizualizace shrnutí hello výstupní datovou sadu výsledků z našich [spustit skript jazyka R] [ execute-r-script] modulu, zobrazí se vám sloupec "Měsíc" hello je 'Categorical' s 12 jedinečné hodnoty, znovu, stejně jako jsme chtěli .</span><span class="sxs-lookup"><span data-stu-id="3f44d-425">Looking at hello visualization summary for hello Result Dataset output of our [Execute R Script][execute-r-script] module, you will see hello 'Month' column is 'Categorical' with 12 unique values, again, just as we wanted.</span></span>

## <span data-ttu-id="3f44d-426"><a id="timeseries"></a>Časové řady objekty a analýzy korelace</span><span class="sxs-lookup"><span data-stu-id="3f44d-426"><a id="timeseries"></a>Time series objects and correlation analysis</span></span>
<span data-ttu-id="3f44d-427">V této části jsme se několika základních objektů řady čas R zkoumat a analyzovat hello korelací mezi určité proměnné hello.</span><span class="sxs-lookup"><span data-stu-id="3f44d-427">In this section we will explore a few basic R time series objects and analyze hello correlations between some of hello variables.</span></span> <span data-ttu-id="3f44d-428">Naším cílem je toooutput dataframe obsahující dobrý pairwise korelace informací o na několik pomalou.</span><span class="sxs-lookup"><span data-stu-id="3f44d-428">Our goal is toooutput a dataframe containing hello pairwise correlation information at several lags.</span></span>

<span data-ttu-id="3f44d-429">Hello dokončení R kód pro tento oddíl je v souboru zip hello, které jste dříve stáhli.</span><span class="sxs-lookup"><span data-stu-id="3f44d-429">hello complete R code for this section is in hello zip file you downloaded earlier.</span></span>

### <a name="time-series-objects-in-r"></a><span data-ttu-id="3f44d-430">Časové řady objekty v R</span><span class="sxs-lookup"><span data-stu-id="3f44d-430">Time series objects in R</span></span>
<span data-ttu-id="3f44d-431">Jak už jsme už zmínili, časové řady jsou řady hodnot dat indexované podle času.</span><span class="sxs-lookup"><span data-stu-id="3f44d-431">As already mentioned, time series are a series of data values indexed by time.</span></span> <span data-ttu-id="3f44d-432">R časové řady objekty jsou použité toocreate a spravovat hello čas index.</span><span class="sxs-lookup"><span data-stu-id="3f44d-432">R time series objects are used toocreate and manage hello time index.</span></span> <span data-ttu-id="3f44d-433">Existuje několik výhod toousing časové řady objekty.</span><span class="sxs-lookup"><span data-stu-id="3f44d-433">There are several advantages toousing time series objects.</span></span> <span data-ttu-id="3f44d-434">Časové řady objekty vás zbaví hello mnoho podrobnosti o správě hello časových řad index hodnoty, které jsou zapouzdřené v objektu hello.</span><span class="sxs-lookup"><span data-stu-id="3f44d-434">Time series objects free you from hello many details of managing hello time series index values that are encapsulated in hello object.</span></span> <span data-ttu-id="3f44d-435">Kromě toho časové řady objekty povolit toouse hello mnoho času řady metod pro vykreslení, tisk, modelování a podobně.</span><span class="sxs-lookup"><span data-stu-id="3f44d-435">In addition, time series objects allow you toouse hello many time series methods for plotting, printing, modeling, etc.</span></span>

<span data-ttu-id="3f44d-436">Hello POSIXct time series třída se často používá a je poměrně jednoduché.</span><span class="sxs-lookup"><span data-stu-id="3f44d-436">hello POSIXct time series class is commonly used and is relatively simple.</span></span> <span data-ttu-id="3f44d-437">Tato třída časové řady míry čas z hello začátek hello epoch, 1. ledna 1970.</span><span class="sxs-lookup"><span data-stu-id="3f44d-437">This time series class measures time from hello start of hello epoch, January 1, 1970.</span></span> <span data-ttu-id="3f44d-438">V tomto příkladu použijeme POSIXct časové řady objekty.</span><span class="sxs-lookup"><span data-stu-id="3f44d-438">We will use POSIXct time series objects in this example.</span></span> <span data-ttu-id="3f44d-439">Další často používaný R časové řady tříd objektů zahrnují zoo a xts, extensible časové řady.</span><span class="sxs-lookup"><span data-stu-id="3f44d-439">Other widely used R time series object classes include zoo and xts, extensible time series.</span></span>
<!-- Additional information on R time series objects is provided in hello references in Section 5.7. [commenting because this section doesn't exist, even in hello original] -->

### <a name="time-series-object-example"></a><span data-ttu-id="3f44d-440">Příklad objekt časové řady</span><span class="sxs-lookup"><span data-stu-id="3f44d-440">Time series object example</span></span>
<span data-ttu-id="3f44d-441">Můžeme začít pracovat s našem příkladu.</span><span class="sxs-lookup"><span data-stu-id="3f44d-441">Let's get started with our example.</span></span> <span data-ttu-id="3f44d-442">Přetáhnout myší **nové** [spustit skript jazyka R] [ execute-r-script] modulu do experimentu.</span><span class="sxs-lookup"><span data-stu-id="3f44d-442">Drag and drop a **new** [Execute R Script][execute-r-script] module into your experiment.</span></span> <span data-ttu-id="3f44d-443">Připojit hello výsledek Dataset1 výstupní port hello existující [spustit skript jazyka R] [ execute-r-script] modulu toohello Dataset1 vstupní port hello nové [spustit skript jazyka R] [ execute-r-script] modulu.</span><span class="sxs-lookup"><span data-stu-id="3f44d-443">Connect hello Result Dataset1 output port of hello existing [Execute R Script][execute-r-script] module toohello Dataset1 input port of hello new [Execute R Script][execute-r-script] module.</span></span>

<span data-ttu-id="3f44d-444">Jak bylo možné příklady první hello, jako jsme průběh prostřednictvím hello například v některé body, které se zobrazí I pouze hello přírůstkové další řádky kódu jazyka R při každém kroku.</span><span class="sxs-lookup"><span data-stu-id="3f44d-444">As I did for hello first examples, as we progress through hello example, at some points I will show only hello incremental additional lines of R code at each step.</span></span>  

#### <a name="reading-hello-dataframe"></a><span data-ttu-id="3f44d-445">Čtení hello dataframe</span><span class="sxs-lookup"><span data-stu-id="3f44d-445">Reading hello dataframe</span></span>
<span data-ttu-id="3f44d-446">Jako první krok můžeme načtení dataframe a ujistěte se, že se nám získat hello očekávané výsledky.</span><span class="sxs-lookup"><span data-stu-id="3f44d-446">As a first step, let's read in a dataframe and make sure we get hello expected results.</span></span> <span data-ttu-id="3f44d-447">Hello následující kód by měl provést úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="3f44d-447">hello following code should do hello job.</span></span>

    # Comment hello following if using RStudio
    cadairydata <- maml.mapInputPort(1)
    str(cadairydata) # Check hello results

<span data-ttu-id="3f44d-448">Nyní spusťte hello experiment.</span><span class="sxs-lookup"><span data-stu-id="3f44d-448">Now, run hello experiment.</span></span> <span data-ttu-id="3f44d-449">Hello protokolu nový tvar spustit skript jazyka R hello by měl vypadat jako obrázek 14.</span><span class="sxs-lookup"><span data-stu-id="3f44d-449">hello log of hello new Execute R Script shape should look like Figure 14.</span></span>

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  8 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  1.47 1.31 1.51 1.45 1.5 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  5.82 5.9 6.1 6.06 6.17 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  7.66 7.57 7.68 7.66 7.71 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  6.89 6.79 6.79 6.8 6.8 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Count      : num  0 1 2 3 4 5 6 7 8 9 ...

<span data-ttu-id="3f44d-450">*Obrázek 14. Shrnutí hello dataframe v modulu spustit skript jazyka R hello.*</span><span class="sxs-lookup"><span data-stu-id="3f44d-450">*Figure 14. Summary of hello dataframe in hello Execute R Script module.*</span></span>

<span data-ttu-id="3f44d-451">Tato data jsou hello očekává typů a formát.</span><span class="sxs-lookup"><span data-stu-id="3f44d-451">This data is of hello expected types and format.</span></span> <span data-ttu-id="3f44d-452">Upozorňujeme, že sloupec "Měsíc" hello je typ faktoru a očekává se, že hello počet úrovní.</span><span class="sxs-lookup"><span data-stu-id="3f44d-452">Note that hello 'Month' column is of type factor and has hello expected number of levels.</span></span>

#### <a name="creating-a-time-series-object"></a><span data-ttu-id="3f44d-453">Vytvoření objektu časové řady</span><span class="sxs-lookup"><span data-stu-id="3f44d-453">Creating a time series object</span></span>
<span data-ttu-id="3f44d-454">Potřebujeme tooadd dataframe tooour objekt řady a čas.</span><span class="sxs-lookup"><span data-stu-id="3f44d-454">We need tooadd a time series object tooour dataframe.</span></span> <span data-ttu-id="3f44d-455">Nahraďte hello následující text, který přidá nový sloupec třídy POSIXct hello aktuálního kódu.</span><span class="sxs-lookup"><span data-stu-id="3f44d-455">Replace hello current code with hello following, which adds a new column of class POSIXct.</span></span>

    # Comment hello following if using RStudio
    cadairydata <- maml.mapInputPort(1)

    ## Create a new column as a POSIXct object
    Sys.setenv(TZ = "PST8PDT")
    cadairydata$Time <- as.POSIXct(strptime(paste(as.character(cadairydata$Year), "-", as.character(cadairydata$Month.Number), "-01 00:00:00", sep = ""), "%Y-%m-%d %H:%M:%S"))

    str(cadairydata) # Check hello results

<span data-ttu-id="3f44d-456">Teď zkontrolujte protokol hello.</span><span class="sxs-lookup"><span data-stu-id="3f44d-456">Now, check hello log.</span></span> <span data-ttu-id="3f44d-457">By měl vypadat jako obrázek 15.</span><span class="sxs-lookup"><span data-stu-id="3f44d-457">It should look like Figure 15.</span></span>

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  9 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  1.47 1.31 1.51 1.45 1.5 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  5.82 5.9 6.1 6.06 6.17 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  7.66 7.57 7.68 7.66 7.71 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  6.89 6.79 6.79 6.8 6.8 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Count      : num  0 1 2 3 4 5 6 7 8 9 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Time             : POSIXct, format: "1995-01-01" "1995-02-01" ...

<span data-ttu-id="3f44d-458">*Obrázek 15. Shrnutí hello dataframe s objektem časové řady.*</span><span class="sxs-lookup"><span data-stu-id="3f44d-458">*Figure 15. Summary of hello dataframe with a time series object.*</span></span>

<span data-ttu-id="3f44d-459">Jsme můžete zjistit z hello souhrnné že Hello nové sloupce je ve skutečnosti třídy POSIXct.</span><span class="sxs-lookup"><span data-stu-id="3f44d-459">We can see from hello summary that hello new column is in fact of class POSIXct.</span></span>

### <a name="exploring-and-transforming-hello-data"></a><span data-ttu-id="3f44d-460">Zkoumání a transformace dat hello</span><span class="sxs-lookup"><span data-stu-id="3f44d-460">Exploring and transforming hello data</span></span>
<span data-ttu-id="3f44d-461">Podíváme se na určité proměnné hello v této datové sadě.</span><span class="sxs-lookup"><span data-stu-id="3f44d-461">Let's explore some of hello variables in this dataset.</span></span> <span data-ttu-id="3f44d-462">Matice scatterplot je dobře tooproduce rychle zkontrolovat.</span><span class="sxs-lookup"><span data-stu-id="3f44d-462">A scatterplot matrix is a good way tooproduce a quick look.</span></span> <span data-ttu-id="3f44d-463">I mě nahrazení hello `str()` funkce v předchozí kód R hello s hello následující řádek.</span><span class="sxs-lookup"><span data-stu-id="3f44d-463">I am replacing hello `str()` function in hello previous R code with hello following line.</span></span>

    pairs(~ Cotagecheese.Prod + Icecream.Prod + Milk.Prod + N.CA.Fat.Price, data = cadairydata, main = "Pairwise Scatterplots of dairy time series")

<span data-ttu-id="3f44d-464">Spustit tento kód a zobrazit, co se stane.</span><span class="sxs-lookup"><span data-stu-id="3f44d-464">Run this code and see what happens.</span></span> <span data-ttu-id="3f44d-465">vykreslení Hello vytvořeného v hello port R zařízení by měl vypadat jako obrázek 16.</span><span class="sxs-lookup"><span data-stu-id="3f44d-465">hello plot produced at hello R Device port should look like Figure 16.</span></span>

![Scatterplot matice vybrané proměnné][17]

<span data-ttu-id="3f44d-467">*Obrázek 16. Matice Scatterplot vybrané proměnné.*</span><span class="sxs-lookup"><span data-stu-id="3f44d-467">*Figure 16. Scatterplot matrix of selected variables.*</span></span>

<span data-ttu-id="3f44d-468">Je některé odd-looking struktura hello vztahy mezi tyto proměnné.</span><span class="sxs-lookup"><span data-stu-id="3f44d-468">There is some odd-looking structure in hello relationships between these variables.</span></span> <span data-ttu-id="3f44d-469">Možná to mohou nastat z trendů v datech hello a hello fakt, že jsme nebyly standardizované hello proměnné.</span><span class="sxs-lookup"><span data-stu-id="3f44d-469">Perhaps this arises from trends in hello data and from hello fact that we have not standardized hello variables.</span></span>

### <a name="correlation-analysis"></a><span data-ttu-id="3f44d-470">Analýza korelace</span><span class="sxs-lookup"><span data-stu-id="3f44d-470">Correlation analysis</span></span>
<span data-ttu-id="3f44d-471">tooperform korelace analýzy, že potřebujeme tooboth zrušte trendů a standardizovat hello proměnné.</span><span class="sxs-lookup"><span data-stu-id="3f44d-471">tooperform correlation analysis we need tooboth de-trend and standardize hello variables.</span></span> <span data-ttu-id="3f44d-472">Můžeme jednoduše použít hello R `scale()` funkci, která centra i škáluje proměnné.</span><span class="sxs-lookup"><span data-stu-id="3f44d-472">We could simply use hello R `scale()` function, which both centers and scales variables.</span></span> <span data-ttu-id="3f44d-473">Tato funkce může také pracovat rychleji.</span><span class="sxs-lookup"><span data-stu-id="3f44d-473">This function might well run faster.</span></span> <span data-ttu-id="3f44d-474">Ale chci tooshow je příkladem Obranným programing v jazyce R.</span><span class="sxs-lookup"><span data-stu-id="3f44d-474">However, I want tooshow you an example of defensive programing in R.</span></span>

<span data-ttu-id="3f44d-475">Hello `ts.detrend()` provádí následující funkce obou těchto operací.</span><span class="sxs-lookup"><span data-stu-id="3f44d-475">hello `ts.detrend()` function shown below performs both of these operations.</span></span> <span data-ttu-id="3f44d-476">Hello následující dva řádky kódu zrušte trendů hello data a pak standardizovat hello hodnoty.</span><span class="sxs-lookup"><span data-stu-id="3f44d-476">hello following two lines of code de-trend hello data and then standardize hello values.</span></span>

    ts.detrend <- function(ts, Time, min.length = 3){
      ## Function toode-trend and standardize a time series

      ## Define some messages if they are NULL  
      messages <- c('ERROR: ts.detrend requires arguments ts and Time toohave hello same length',
                    'ERROR: ts.detrend requires argument ts toobe of type numeric',
                    paste('WARNING: ts.detrend has encountered a time series with length less than', as.character(min.length)),
                    'ERROR: ts.detrend has encountered a Time argument not of class POSIXct',
                    'ERROR: Detrend regression has failed in ts.detrend',
                    'ERROR: Exception occurred in ts.detrend while standardizing time series in function ts.detrend'
      )
      # Create a vector of zeros tooreturn as a default in some cases
      zerovec  <- rep(length(ts), 0.0)

      # hello input arguments are not of hello same length, return ts and quit
      if(length(Time) != length(ts)) {warning(messages[1]); return(ts)}

      # If hello ts is not numeric, just return a zero vector and quit
      if(!is.numeric(ts)) {warning(messages[2]); return(zerovec)}

      # If hello ts is too short, just return it and quit
      if((ts.length <- length(ts)) < min.length) {warning(messages[3]); return(ts)}

      ## Check that hello Time variable is of class POSIXct
      if(class(cadairydata$Time)[[1]] != "POSIXct") {warning(messages[4]); return(ts)}

      ## De-trend hello time series by using a linear model
      ts.frame  <- data.frame(ts = ts, Time = Time)
      tryCatch({ts <- ts - fitted(lm(ts ~ Time, data = ts.frame))},
               error = function(e){warning(messages[5]); zerovec})

      tryCatch( {stdev <- sqrt(sum((ts - mean(ts))^2))/(ts.length - 1)
                 ts <- ts/stdev},
                error = function(e){warning(messages[6]); zerovec})

      ts
    }  
    ## Apply hello detrend.ts function toohello variables of interest
    df.detrend <- data.frame(lapply(cadairydata[, 4:7], ts.detrend, cadairydata$Time))

    ## Plot hello results toolook at hello relationships
    pairs(~ Cotagecheese.Prod + Icecream.Prod + Milk.Prod + N.CA.Fat.Price, data = df.detrend, main = "Pairwise Scatterplots of detrended standardized time series")

<span data-ttu-id="3f44d-477">Je s bit situaci v hello `ts.detrend()` funkce.</span><span class="sxs-lookup"><span data-stu-id="3f44d-477">There is quite a bit happening in hello `ts.detrend()` function.</span></span> <span data-ttu-id="3f44d-478">Většina tohoto kódu je kontrola potenciálních problémů spojených s argumenty hello nebo týkající se výjimky, které mohou stále nastat během hello výpočty.</span><span class="sxs-lookup"><span data-stu-id="3f44d-478">Most of this code is checking for potential problems with hello arguments or dealing with exceptions, which can still arise during hello computations.</span></span> <span data-ttu-id="3f44d-479">Tento kód jenom pár řádků ve skutečnosti hello výpočty.</span><span class="sxs-lookup"><span data-stu-id="3f44d-479">Only a few lines of this code actually do hello computations.</span></span>

<span data-ttu-id="3f44d-480">Už jsme projednat příkladem Obranným programování v [hodnota transformace](#valuetransformations).</span><span class="sxs-lookup"><span data-stu-id="3f44d-480">We have already discussed an example of defensive programming in [Value transformations](#valuetransformations).</span></span> <span data-ttu-id="3f44d-481">Obě bloky výpočtu je uzavřen do `tryCatch()`.</span><span class="sxs-lookup"><span data-stu-id="3f44d-481">Both computation blocks are wrapped in `tryCatch()`.</span></span> <span data-ttu-id="3f44d-482">Některé chyby má smysl tooreturn hello původní vstupní vektoru a v ostatních případech návratu vektoru nul.</span><span class="sxs-lookup"><span data-stu-id="3f44d-482">For some errors it makes sense tooreturn hello original input vector, and in other cases, I return a vector of zeros.</span></span>  

<span data-ttu-id="3f44d-483">Všimněte si, že hello lineární regrese používá pro zrušte trendů je to regrese časové řady.</span><span class="sxs-lookup"><span data-stu-id="3f44d-483">Note that hello linear regression used for de-trending is a time series regression.</span></span> <span data-ttu-id="3f44d-484">Proměnná předpověď Hello je objekt časové řady.</span><span class="sxs-lookup"><span data-stu-id="3f44d-484">hello predictor variable is a time series object.</span></span>  

<span data-ttu-id="3f44d-485">Jednou `ts.detrend()` je definována ho použijeme toohello proměnné zájem o náš dataframe.</span><span class="sxs-lookup"><span data-stu-id="3f44d-485">Once `ts.detrend()` is defined we apply it toohello variables of interest in our dataframe.</span></span> <span data-ttu-id="3f44d-486">Jsme musí coerce hello výsledný seznam vytvořený `lapply()` toodata dataframe pomocí `as.data.frame()`.</span><span class="sxs-lookup"><span data-stu-id="3f44d-486">We must coerce hello resulting list created by `lapply()` toodata dataframe by using `as.data.frame()`.</span></span> <span data-ttu-id="3f44d-487">Z důvodu Obranným aspektů `ts.detrend()`, tooprocess selhání jedné z proměnných hello nezabrání opravte zpracování hello ostatní.</span><span class="sxs-lookup"><span data-stu-id="3f44d-487">Because of defensive aspects of `ts.detrend()`, failure tooprocess one of hello variables will not prevent correct processing of hello others.</span></span>  

<span data-ttu-id="3f44d-488">poslední řádek kódu Hello vytvoří pairwise scatterplot.</span><span class="sxs-lookup"><span data-stu-id="3f44d-488">hello final line of code creates a pairwise scatterplot.</span></span> <span data-ttu-id="3f44d-489">Po spuštění kódu hello R, jsou výsledky hello hello scatterplot uvedené v obrázek 17.</span><span class="sxs-lookup"><span data-stu-id="3f44d-489">After running hello R code, hello results of hello scatterplot are shown in Figure 17.</span></span>

![Pairwise scatterplot zrušte trendu a standardizovaném časové řady][18]

<span data-ttu-id="3f44d-491">*Obrázek 17. Pairwise scatterplot zrušte trendu a standardizovaném časové řady.*</span><span class="sxs-lookup"><span data-stu-id="3f44d-491">*Figure 17. Pairwise scatterplot of de-trended and standardized time series.*</span></span>

<span data-ttu-id="3f44d-492">Tyto výsledky toothose znázorňuje obrázek 16, můžete porovnat.</span><span class="sxs-lookup"><span data-stu-id="3f44d-492">You can compare these results toothose shown in Figure 16.</span></span> <span data-ttu-id="3f44d-493">S hello trend odebrána a hello proměnné standardizované, vidíme mnohem menší struktury hello vztahy mezi tyto proměnné.</span><span class="sxs-lookup"><span data-stu-id="3f44d-493">With hello trend removed and hello variables standardized, we see a lot less structure in hello relationships between these variables.</span></span>

<span data-ttu-id="3f44d-494">Hello kód toocompute hello korelací jako objekty PVJS R je následující.</span><span class="sxs-lookup"><span data-stu-id="3f44d-494">hello code toocompute hello correlations as R ccf objects is as follows.</span></span>

    ## A function toocompute pairwise correlations from a
    ## list of time series value vectors
    pair.cor <- function(pair.ind, ts.list, lag.max = 1, plot = FALSE){
      ccf(ts.list[[pair.ind[1]]], ts.list[[pair.ind[2]]], lag.max = lag.max, plot = plot)
    }

    ## A list of hello pairwise indices
    corpairs <- list(c(1,2), c(1,3), c(1,4), c(2,3), c(2,4), c(3,4))

    ## Compute hello list of ccf objects
    cadairycorrelations <- lapply(corpairs, pair.cor, df.detrend)  

    cadairycorrelations

<span data-ttu-id="3f44d-495">Spuštění tento kód vytvoří hello protokolu znázorňuje obrázek 18.</span><span class="sxs-lookup"><span data-stu-id="3f44d-495">Running this code produces hello log shown in Figure 18.</span></span>

    [ModuleOutput] Loading objects:
    [ModuleOutput]   port1
    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] [[1]]
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] Autocorrelations of series 'X', by lag
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput]    -1     0     1 
    [ModuleOutput] 0.148 0.358 0.317 
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] [[2]]
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] Autocorrelations of series 'X', by lag
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput]     -1      0      1 
    [ModuleOutput] -0.395 -0.186 -0.238 
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] [[3]]
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] Autocorrelations of series 'X', by lag
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput]     -1      0      1 
    [ModuleOutput] -0.059 -0.089 -0.127 
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] [[4]]
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] Autocorrelations of series 'X', by lag
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput]    -1     0     1 
    [ModuleOutput] 0.140 0.294 0.293 
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] [[5]]
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] Autocorrelations of series 'X', by lag
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput]     -1      0      1 
    [ModuleOutput] -0.002 -0.074 -0.124 

<span data-ttu-id="3f44d-496">*Obrázek 18. Seznam PVJS objekty z dobrý pairwise korelace analýzy.*</span><span class="sxs-lookup"><span data-stu-id="3f44d-496">*Figure 18. List of ccf objects from hello pairwise correlation analysis.*</span></span>

<span data-ttu-id="3f44d-497">Je hodnota korelace pro každý prodleva.</span><span class="sxs-lookup"><span data-stu-id="3f44d-497">There is a correlation value for each lag.</span></span> <span data-ttu-id="3f44d-498">Žádná z těchto hodnot korelace není dostatečně velké na to toobe významné.</span><span class="sxs-lookup"><span data-stu-id="3f44d-498">None of these correlation values is large enough toobe significant.</span></span> <span data-ttu-id="3f44d-499">Jsme proto uzavřít, že jsme každou proměnnou modelu nezávisle.</span><span class="sxs-lookup"><span data-stu-id="3f44d-499">We can therefore conclude that we can model each variable independently.</span></span>

### <a name="output-a-dataframe"></a><span data-ttu-id="3f44d-500">Výstup a dataframe</span><span class="sxs-lookup"><span data-stu-id="3f44d-500">Output a dataframe</span></span>
<span data-ttu-id="3f44d-501">Pairwise korelací hello jsme mít počítá jako seznam R PVJS objektů.</span><span class="sxs-lookup"><span data-stu-id="3f44d-501">We have computed hello pairwise correlations as a list of R ccf objects.</span></span> <span data-ttu-id="3f44d-502">To představuje bit problému jako hello datovou sadu výsledků výstupní port ve skutečnosti vyžaduje dataframe.</span><span class="sxs-lookup"><span data-stu-id="3f44d-502">This presents a bit of a problem as hello Result Dataset output port really requires a dataframe.</span></span> <span data-ttu-id="3f44d-503">Navíc hello PVJS objektu je sám seznam a chceme, pouze hello hodnoty v hello první prvek seznamu, hello korelací v hello různé pomalou.</span><span class="sxs-lookup"><span data-stu-id="3f44d-503">Further, hello ccf object is itself a list and we want only hello values in hello first element of this list, hello correlations at hello various lags.</span></span>

<span data-ttu-id="3f44d-504">Následující kód extrahuje hello prodleva hodnoty ze seznamu hello PVJS objekty, které jsou sami seznamy Hello.</span><span class="sxs-lookup"><span data-stu-id="3f44d-504">hello following code extracts hello lag values from hello list of ccf objects, which are themselves lists.</span></span>

    df.correlations <- data.frame(do.call(rbind, lapply(cadairycorrelations, '[[', 1)))

    c.names <- c("correlation pair", "-1 lag", "0 lag", "+1 lag")
    r.names  <- c("Corr Cot Cheese - Ice Cream",
                  "Corr Cot Cheese - Milk Prod",
                  "Corr Cot Cheese - Fat Price",
                  "Corr Ice Cream - Mik Prod",
                  "Corr Ice Cream - Fat Price",
                  "Corr Milk Prod - Fat Price")

    ## Build a dataframe with hello row names column and the
    ## correlation data frame and assign hello column names
    outframe <- cbind(r.names, df.correlations)
    colnames(outframe) <- c.names
    outframe


    ## WARNING!
    ## hello following line works only in Azure Machine Learning
    ## When running in RStudio, this code will result in an error
    #maml.mapOutputPort('outframe')

<span data-ttu-id="3f44d-505">první řádek kódu Hello je trochu složité a vysvětlení mohou pomoci porozumět.</span><span class="sxs-lookup"><span data-stu-id="3f44d-505">hello first line of code is a bit tricky, and some explanation may help you understand it.</span></span> <span data-ttu-id="3f44d-506">Práce z hello uvnitř odhlašování máme hello následující:</span><span class="sxs-lookup"><span data-stu-id="3f44d-506">Working from hello inside out we have hello following:</span></span>

1. <span data-ttu-id="3f44d-507">Hello '**[[**'operátor s argumentem hello'**1**se vybere hello vektor korelací v hello pomalou z první prvek seznamu objekt PVJS hello hello.</span><span class="sxs-lookup"><span data-stu-id="3f44d-507">hello '**[[**' operator with hello argument '**1**' selects hello vector of correlations at hello lags from hello first element of hello ccf object list.</span></span>
2. <span data-ttu-id="3f44d-508">Hello `do.call()` funkce se vztahuje hello `rbind()` funkce přes hello prvky hello seznamu vrátí podle `lapply()`.</span><span class="sxs-lookup"><span data-stu-id="3f44d-508">hello `do.call()` function applies hello `rbind()` function over hello elements of hello list returns by `lapply()`.</span></span>
3. <span data-ttu-id="3f44d-509">Hello `data.frame()` funkce převede hello výsledku vytvořeného `do.call()` tooa dataframe.</span><span class="sxs-lookup"><span data-stu-id="3f44d-509">hello `data.frame()` function coerces hello result produced by `do.call()` tooa dataframe.</span></span>

<span data-ttu-id="3f44d-510">Všimněte si, že názvy hello řádek ve sloupci hello dataframe.</span><span class="sxs-lookup"><span data-stu-id="3f44d-510">Note that hello row names are in a column of hello dataframe.</span></span> <span data-ttu-id="3f44d-511">Díky tomu zachovává hello řádek názvy, když jsou výstup hello [spustit skript jazyka R][execute-r-script].</span><span class="sxs-lookup"><span data-stu-id="3f44d-511">Doing so preserves hello row names when they are output from hello [Execute R Script][execute-r-script].</span></span>

<span data-ttu-id="3f44d-512">Spuštění kódu hello vytváří výstup hello znázorňuje obrázek 19 při I **vizualizovat** hello výstup v hello port datovou sadu výsledků.</span><span class="sxs-lookup"><span data-stu-id="3f44d-512">Running hello code produces hello output shown in Figure 19 when I **Visualize** hello output at hello Result Dataset port.</span></span> <span data-ttu-id="3f44d-513">názvy řádků Hello jsou hello prvního sloupce, tak, jak má.</span><span class="sxs-lookup"><span data-stu-id="3f44d-513">hello row names are in hello first column, as intended.</span></span>

![Výstup výsledků z hello korelace analýzy][20]

<span data-ttu-id="3f44d-515">*Obrázek 19. Výsledky výstup z hello korelace analýzy.*</span><span class="sxs-lookup"><span data-stu-id="3f44d-515">*Figure 19. Results output from hello correlation analysis.*</span></span>

## <span data-ttu-id="3f44d-516"><a id="seasonalforecasting"></a>Příklad časové řady: sezónní prognózy</span><span class="sxs-lookup"><span data-stu-id="3f44d-516"><a id="seasonalforecasting"></a>Time series example: seasonal forecasting</span></span>
<span data-ttu-id="3f44d-517">Naše data je teď ve formě vhodné pro analýzu a bylo zjištěno, že neexistují žádné významné korelací mezi hello proměnné.</span><span class="sxs-lookup"><span data-stu-id="3f44d-517">Our data is now in a form suitable for analysis, and we have determined there are no significant correlations between hello variables.</span></span> <span data-ttu-id="3f44d-518">Umožňuje přesunout a vytvořte časové řady modelu prognózy.</span><span class="sxs-lookup"><span data-stu-id="3f44d-518">Let's move on and create a time series forecasting model.</span></span> <span data-ttu-id="3f44d-519">Pomocí tohoto modelu jsme se prognózy pro hello kalifornské mléka produkční 12 měsíců od 2013.</span><span class="sxs-lookup"><span data-stu-id="3f44d-519">Using this model we will forecast California milk production for hello 12 months of 2013.</span></span>

<span data-ttu-id="3f44d-520">Naše předpovědi modelu bude mít dvě součásti, součást trendu a sezónní součást.</span><span class="sxs-lookup"><span data-stu-id="3f44d-520">Our forecasting model will have two components, a trend component and a seasonal component.</span></span> <span data-ttu-id="3f44d-521">dokončení prognózy Hello je produkt hello tyto dvě součásti.</span><span class="sxs-lookup"><span data-stu-id="3f44d-521">hello complete forecast is hello product of these two components.</span></span> <span data-ttu-id="3f44d-522">Tento typ modelu se označuje jako multiplikativní modelu.</span><span class="sxs-lookup"><span data-stu-id="3f44d-522">This type of model is known as a multiplicative model.</span></span> <span data-ttu-id="3f44d-523">alternativní Hello je sčítání model.</span><span class="sxs-lookup"><span data-stu-id="3f44d-523">hello alternative is an additive model.</span></span> <span data-ttu-id="3f44d-524">Už jsme provedli protokolu transformace toohello proměnné zájmu, což může tractable této analýze.</span><span class="sxs-lookup"><span data-stu-id="3f44d-524">We have already applied a log transformation toohello variables of interest, which makes this analysis tractable.</span></span>

<span data-ttu-id="3f44d-525">Hello dokončení R kód pro tento oddíl je v souboru zip hello, které jste dříve stáhli.</span><span class="sxs-lookup"><span data-stu-id="3f44d-525">hello complete R code for this section is in hello zip file you downloaded earlier.</span></span>

### <a name="creating-hello-dataframe-for-analysis"></a><span data-ttu-id="3f44d-526">Vytváření hello dataframe pro analýzu</span><span class="sxs-lookup"><span data-stu-id="3f44d-526">Creating hello dataframe for analysis</span></span>
<span data-ttu-id="3f44d-527">Začněte přidáním **nové** [spustit skript jazyka R] [ execute-r-script] modulu tooyour experimentu.</span><span class="sxs-lookup"><span data-stu-id="3f44d-527">Start by adding a **new** [Execute R Script][execute-r-script] module tooyour experiment.</span></span> <span data-ttu-id="3f44d-528">Připojit hello **datovou sadu výsledků** výstup hello existující [spustit skript jazyka R] [ execute-r-script] modulu toohello **Dataset1** vstupní hello nového modulu.</span><span class="sxs-lookup"><span data-stu-id="3f44d-528">Connect hello **Result Dataset** output of hello existing [Execute R Script][execute-r-script] module toohello **Dataset1** input of hello new module.</span></span> <span data-ttu-id="3f44d-529">výsledek Hello by měl vypadat podobně jako obrázek 20.</span><span class="sxs-lookup"><span data-stu-id="3f44d-529">hello result should look something like Figure 20.</span></span>

![Hello experimentovat s hello nový přidán modul spustit skript jazyka R][21]

<span data-ttu-id="3f44d-531">*Obrázek 20. Hello experimentovat s nového modulu spustit skript jazyka R hello přidat.*</span><span class="sxs-lookup"><span data-stu-id="3f44d-531">*Figure 20. hello experiment with hello new Execute R Script module added.*</span></span>

<span data-ttu-id="3f44d-532">Jako hello korelace analýzy, kterou jsme právě dokončili, potřebujeme tooadd sloupec s objektem POSIXct časové řady.</span><span class="sxs-lookup"><span data-stu-id="3f44d-532">As with hello correlation analysis we just completed, we need tooadd a column with a POSIXct time series object.</span></span> <span data-ttu-id="3f44d-533">Hello následující kód a to stejně.</span><span class="sxs-lookup"><span data-stu-id="3f44d-533">hello following code will do just this.</span></span>

    # If running in Machine Learning Studio, uncomment hello first line with maml.mapInputPort()
    cadairydata <- maml.mapInputPort(1)

    ## Create a new column as a POSIXct object
    Sys.setenv(TZ = "PST8PDT")
    cadairydata$Time <- as.POSIXct(strptime(paste(as.character(cadairydata$Year), "-", as.character(cadairydata$Month.Number), "-01 00:00:00", sep = ""), "%Y-%m-%d %H:%M:%S"))

    str(cadairydata)

<span data-ttu-id="3f44d-534">Spusťte tento kód a podívejte se na hello protokolu.</span><span class="sxs-lookup"><span data-stu-id="3f44d-534">Run this code and look at hello log.</span></span> <span data-ttu-id="3f44d-535">výsledek Hello by měl vypadat jako obrázek 21.</span><span class="sxs-lookup"><span data-stu-id="3f44d-535">hello result should look like Figure 21.</span></span>

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  9 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  1.47 1.31 1.51 1.45 1.5 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  5.82 5.9 6.1 6.06 6.17 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  7.66 7.57 7.68 7.66 7.71 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  6.89 6.79 6.79 6.8 6.8 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Count      : num  0 1 2 3 4 5 6 7 8 9 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Time             : POSIXct, format: "1995-01-01" "1995-02-01" ...

<span data-ttu-id="3f44d-536">*Obrázek 21. Přehled hello dataframe.*</span><span class="sxs-lookup"><span data-stu-id="3f44d-536">*Figure 21. A summary of hello dataframe.*</span></span>

<span data-ttu-id="3f44d-537">S tímto výsledkem jsme jsou připravené toostart Naše analýzy.</span><span class="sxs-lookup"><span data-stu-id="3f44d-537">With this result, we are ready toostart our analysis.</span></span>

### <a name="create-a-training-dataset"></a><span data-ttu-id="3f44d-538">Vytvoření datové sady školení</span><span class="sxs-lookup"><span data-stu-id="3f44d-538">Create a training dataset</span></span>
<span data-ttu-id="3f44d-539">S dataframe hello sestavený potřebujeme toocreate školení datové sady.</span><span class="sxs-lookup"><span data-stu-id="3f44d-539">With hello dataframe constructed we need toocreate a training dataset.</span></span> <span data-ttu-id="3f44d-540">Tato data budou zahrnovat všechny hello připomínky, s výjimkou hello posledních 12 hello roku 2013, což je naše testovací datové sady.</span><span class="sxs-lookup"><span data-stu-id="3f44d-540">This data will include all of hello observations except hello last 12, of hello year 2013, which is our test dataset.</span></span> <span data-ttu-id="3f44d-541">Hello následující kód podmnožin hello dataframe a vytvoří pozemků hello dojnic produkčního prostředí a cena proměnných.</span><span class="sxs-lookup"><span data-stu-id="3f44d-541">hello following code subsets hello dataframe and creates plots of hello dairy production and price variables.</span></span> <span data-ttu-id="3f44d-542">Potom vytvořit pozemků hello čtyři produkčního prostředí a cena proměnné.</span><span class="sxs-lookup"><span data-stu-id="3f44d-542">I then create plots of hello four production and price variables.</span></span> <span data-ttu-id="3f44d-543">Anonymní funkce je použité toodefine rozšiřuje pro vykreslení a pak provádějí iterace hello seznam hello další dva argumenty s `Map()`.</span><span class="sxs-lookup"><span data-stu-id="3f44d-543">An anonymous function is used toodefine some augments for plot, and then iterate over hello list of hello other two arguments with `Map()`.</span></span> <span data-ttu-id="3f44d-544">Pokud přemýšlíte o, pro smyčky by mít fungovala bez problémů v tomto poli, jsou správná.</span><span class="sxs-lookup"><span data-stu-id="3f44d-544">If you are thinking that a for loop would have worked fine here, you are correct.</span></span> <span data-ttu-id="3f44d-545">Ale vzhledem k tomu, že je funkční jazyk R I mě ukazuje funkční přístup.</span><span class="sxs-lookup"><span data-stu-id="3f44d-545">But, since R is a functional language I am showing you a functional approach.</span></span>

    cadairytrain <- cadairydata[1:216, ]

    Ylabs  <- list("Log CA Cotage Cheese Production, 1000s lb",
                   "Log CA Ice Cream Production, 1000s lb",
                   "Log CA Milk Production 1000s lb",
                   "Log North CA Milk Milk Fat Price per 1000 lb")

    Map(function(y, Ylabs){plot(cadairytrain$Time, y, xlab = "Time", ylab = Ylabs, type = "l")}, cadairytrain[, 4:7], Ylabs)

<span data-ttu-id="3f44d-546">Spuštění kódu hello vytvoří hello z výstupu R zařízení hello znázorňuje obrázek 22 ukazuje zeměpisný řadu časové řady.</span><span class="sxs-lookup"><span data-stu-id="3f44d-546">Running hello code produces hello series of time series plots from hello R Device output shown in Figure 22.</span></span> <span data-ttu-id="3f44d-547">Všimněte si, že časová osa hello je v jednotkách kalendářních dat, dobrý výhodou hello časové řady vykreslení metoda.</span><span class="sxs-lookup"><span data-stu-id="3f44d-547">Note that hello time axis is in units of dates, a nice benefit of hello time series plot method.</span></span>

![První z řady pozemků čas kalifornské dojnic produkčního prostředí a cena dat](./media/machine-learning-r-quickstart/unnamed-chunk-161.png)

![Druhý čas řady pozemků kalifornské dojnic produkčního prostředí a cena dat](./media/machine-learning-r-quickstart/unnamed-chunk-162.png)

![Třetí čas řady pozemků kalifornské dojnic produkčního prostředí a cena dat](./media/machine-learning-r-quickstart/unnamed-chunk-163.png)

![Čtvrtý čas řady pozemků kalifornské dojnic produkčního prostředí a cena dat](./media/machine-learning-r-quickstart/unnamed-chunk-164.png)

<span data-ttu-id="3f44d-552">*Obrázek 22. Čas řady pozemků produkci mléka kalifornské a cena data.*</span><span class="sxs-lookup"><span data-stu-id="3f44d-552">*Figure 22. Time series plots of California dairy production and price data.*</span></span>

### <a name="a-trend-model"></a><span data-ttu-id="3f44d-553">Model trendu</span><span class="sxs-lookup"><span data-stu-id="3f44d-553">A trend model</span></span>
<span data-ttu-id="3f44d-554">S vytvořili objekt řady čas a nutnosti seznámili hello dat, Začněme tooconstruct trend model hello kalifornské mléka provozními daty.</span><span class="sxs-lookup"><span data-stu-id="3f44d-554">Having created a time series object and having had a look at hello data, let's start tooconstruct a trend model for hello California milk production data.</span></span> <span data-ttu-id="3f44d-555">Jsme to lze provést pomocí regrese časové řady.</span><span class="sxs-lookup"><span data-stu-id="3f44d-555">We can do this with a time series regression.</span></span> <span data-ttu-id="3f44d-556">Je však vymazat z hello výkresu, jsme bude potřebovat víc než sklon a zachycení tooaccurately modelu hello zjištěnými trend hello Cvičná data.</span><span class="sxs-lookup"><span data-stu-id="3f44d-556">However, it is clear from hello plot that we will need more than a slope and intercept tooaccurately model hello observed trend in hello training data.</span></span>

<span data-ttu-id="3f44d-557">Zadané hello v menším měřítku hello dat, bude sestavení hello model pro trend Rstudia a pak kopírování a vkládání hello výsledný model do Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="3f44d-557">Given hello small scale of hello data, I will build hello model for trend in RStudio and then cut and paste hello resulting model into Azure Machine Learning.</span></span> <span data-ttu-id="3f44d-558">Rstudia poskytuje interaktivní prostředí pro tento typ interaktivní analýzu.</span><span class="sxs-lookup"><span data-stu-id="3f44d-558">RStudio provides an interactive environment for this type of interactive analysis.</span></span>

<span data-ttu-id="3f44d-559">Jako první pokus o bude proveden pokus polynomické regrese s zajišťuje až too3.</span><span class="sxs-lookup"><span data-stu-id="3f44d-559">As a first attempt, I will try a polynomial regression with powers up too3.</span></span> <span data-ttu-id="3f44d-560">Je-li skutečné riziko z přepsání hodí tyto druhy modelů.</span><span class="sxs-lookup"><span data-stu-id="3f44d-560">There is a real danger of over-fitting these kinds of models.</span></span> <span data-ttu-id="3f44d-561">Proto je nejlepší tooavoid nejvyšších podmínky.</span><span class="sxs-lookup"><span data-stu-id="3f44d-561">Therefore, it is best tooavoid high order terms.</span></span> <span data-ttu-id="3f44d-562">Hello `I()` funkce omezuje výklad obsah hello (interpretuje hello obsah, jako je") a umožňuje vám toowrite oznámena interpretovaný funkce v regresní rovnice.</span><span class="sxs-lookup"><span data-stu-id="3f44d-562">hello `I()` function inhibits interpretation of hello contents (interprets hello contents 'as is') and allows you toowrite a literally interpreted function in a regression equation.</span></span>

    milk.lm <- lm(Milk.Prod ~ Time + I(Month.Count^2) + I(Month.Count^3), data = cadairytrain)
    summary(milk.lm)

<span data-ttu-id="3f44d-563">Tím se vygeneruje následující hello.</span><span class="sxs-lookup"><span data-stu-id="3f44d-563">This generates hello following.</span></span>

    ##
    ## Call:
    ## lm(formula = Milk.Prod ~ Time + I(Month.Count^2) + I(Month.Count^3),
    ##     data = cadairytrain)
    ##
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max
    ## -0.12667 -0.02730  0.00236  0.02943  0.10586
    ##
    ## Coefficients:
    ##                   Estimate Std. Error t value Pr(>|t|)
    ## (Intercept)       6.33e+00   1.45e-01   43.60   <2e-16 ***
    ## Time              1.63e-09   1.72e-10    9.47   <2e-16 ***
    ## I(Month.Count^2) -1.71e-06   4.89e-06   -0.35    0.726
    ## I(Month.Count^3) -3.24e-08   1.49e-08   -2.17    0.031 *  
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ##
    ## Residual standard error: 0.0418 on 212 degrees of freedom
    ## Multiple R-squared:  0.941,    Adjusted R-squared:  0.94
    ## F-statistic: 1.12e+03 on 3 and 212 DF,  p-value: <2e-16

<span data-ttu-id="3f44d-564">Z hodnot P (Pr (> | t |)) v tento výstup vidíme, že hello kvadratických termín nemusí být důležité.</span><span class="sxs-lookup"><span data-stu-id="3f44d-564">From P values (Pr(>|t|)) in this output, we can see that hello squared term may not be significant.</span></span> <span data-ttu-id="3f44d-565">Použiji hello `update()` funkce toomodify tento model pomocí vyřazování hello spolehlivosti termín.</span><span class="sxs-lookup"><span data-stu-id="3f44d-565">I will use hello `update()` function toomodify this model by dropping hello squared term.</span></span>

    milk.lm <- update(milk.lm, . ~ . - I(Month.Count^2))
    summary(milk.lm)

<span data-ttu-id="3f44d-566">Tím se vygeneruje následující hello.</span><span class="sxs-lookup"><span data-stu-id="3f44d-566">This generates hello following.</span></span>

    ##
    ## Call:
    ## lm(formula = Milk.Prod ~ Time + I(Month.Count^3), data = cadairytrain)
    ##
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max
    ## -0.12597 -0.02659  0.00185  0.02963  0.10696
    ##
    ## Coefficients:
    ##                   Estimate Std. Error t value Pr(>|t|)
    ## (Intercept)       6.38e+00   4.07e-02   156.6   <2e-16 ***
    ## Time              1.57e-09   4.32e-11    36.3   <2e-16 ***
    ## I(Month.Count^3) -3.76e-08   2.50e-09   -15.1   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ##
    ## Residual standard error: 0.0417 on 213 degrees of freedom
    ## Multiple R-squared:  0.941,  Adjusted R-squared:  0.94
    ## F-statistic: 1.69e+03 on 2 and 213 DF,  p-value: <2e-16

<span data-ttu-id="3f44d-567">To vypadá lepší.</span><span class="sxs-lookup"><span data-stu-id="3f44d-567">This looks better.</span></span> <span data-ttu-id="3f44d-568">Všechny podmínky hello jsou významná.</span><span class="sxs-lookup"><span data-stu-id="3f44d-568">All of hello terms are significant.</span></span> <span data-ttu-id="3f44d-569">Ale hello 2e-16 hodnota je výchozí hodnota a by neměly být příliš vážně brány.</span><span class="sxs-lookup"><span data-stu-id="3f44d-569">However, hello 2e-16 value is a default value, and should not be taken too seriously.</span></span>  

<span data-ttu-id="3f44d-570">Jako testu správností vytvoříme s křivky trend hello vidět se časové řady graf hello kalifornské dojnic provozními daty.</span><span class="sxs-lookup"><span data-stu-id="3f44d-570">As a sanity test, let's make a time series plot of hello California dairy production data with hello trend curve shown.</span></span> <span data-ttu-id="3f44d-571">Následující kód v hello Azure Machine Learning hello je přidali [spustit skript jazyka R] [ execute-r-script] modelu (ne Rstudia) toocreate hello modelu a ujistěte se, vykreslení.</span><span class="sxs-lookup"><span data-stu-id="3f44d-571">I have added hello following code in hello Azure Machine Learning [Execute R Script][execute-r-script] model (not RStudio) toocreate hello model and make a plot.</span></span> <span data-ttu-id="3f44d-572">výsledek Hello vidíte na obrázku 23.</span><span class="sxs-lookup"><span data-stu-id="3f44d-572">hello result is shown in Figure 23.</span></span>

    milk.lm <- lm(Milk.Prod ~ Time + I(Month.Count^3), data = cadairytrain)

    plot(cadairytrain$Time, cadairytrain$Milk.Prod, xlab = "Time", ylab = "Log CA Milk Production 1000s lb", type = "l")
    lines(cadairytrain$Time, predict(milk.lm, cadairytrain), lty = 2, col = 2)

![Kalifornské mléka provozními daty s zobrazený model trendu](./media/machine-learning-r-quickstart/unnamed-chunk-18.png)

<span data-ttu-id="3f44d-574">*Obrázek 23. Kalifornské mléka provozními daty s trend vzoru.*</span><span class="sxs-lookup"><span data-stu-id="3f44d-574">*Figure 23. California milk production data with trend model shown.*</span></span>

<span data-ttu-id="3f44d-575">Zdá se, hello trend modelu dostatečně dobře vyhovuje hello data.</span><span class="sxs-lookup"><span data-stu-id="3f44d-575">It looks like hello trend model fits hello data fairly well.</span></span> <span data-ttu-id="3f44d-576">Další nezdá toobe důkaz přečerpání vhodnosti, například v křivce modelu hello rychlé pohyby lichá.</span><span class="sxs-lookup"><span data-stu-id="3f44d-576">Further, there does not seem toobe evidence of over-fitting, such as odd wiggles in hello model curve.</span></span>  

### <a name="seasonal-model"></a><span data-ttu-id="3f44d-577">Sezónní modelu</span><span class="sxs-lookup"><span data-stu-id="3f44d-577">Seasonal model</span></span>
<span data-ttu-id="3f44d-578">S modelem trend v dolním jsme vyžadují toopush na a zahrnují hello sezónní účinky.</span><span class="sxs-lookup"><span data-stu-id="3f44d-578">With a trend model in hand, we need toopush on and include hello seasonal effects.</span></span> <span data-ttu-id="3f44d-579">Použijeme hello měsíc roku hello jako fiktivní proměnné v hello lineární model toocapture hello po měsících vliv.</span><span class="sxs-lookup"><span data-stu-id="3f44d-579">We will use hello month of hello year as a dummy variable in hello linear model toocapture hello month-by-month effect.</span></span> <span data-ttu-id="3f44d-580">Poznámka: když zavedete Multi-Factor proměnné do modelu, nesmí počítaný hello zachycení.</span><span class="sxs-lookup"><span data-stu-id="3f44d-580">Note that when you introduce factor variables into a model, hello intercept must not be computed.</span></span> <span data-ttu-id="3f44d-581">Pokud není to uděláte, je-li přepsání zadané hello vzorec a R se vyřadit jeden hello potřeby faktory však pro průnik hello.</span><span class="sxs-lookup"><span data-stu-id="3f44d-581">If you do not do this, hello formula is over-specified and R will drop one of hello desired factors but keep hello intercept term.</span></span>

<span data-ttu-id="3f44d-582">Vzhledem k tomu, že máme model uspokojivé trend můžeme použít hello `update()` funkce tooadd hello nové podmínky toohello existující model.</span><span class="sxs-lookup"><span data-stu-id="3f44d-582">Since we have a satisfactory trend model we can use hello `update()` function tooadd hello new terms toohello existing model.</span></span> <span data-ttu-id="3f44d-583">Hello -1 ve vzorci aktualizace hello zahodí průnik hello.</span><span class="sxs-lookup"><span data-stu-id="3f44d-583">hello -1 in hello update formula drops hello intercept term.</span></span> <span data-ttu-id="3f44d-584">Pokračování v Rstudia pro chvíli hello:</span><span class="sxs-lookup"><span data-stu-id="3f44d-584">Continuing in RStudio for hello moment:</span></span>

    milk.lm2 <- update(milk.lm, . ~ . + Month - 1)
    summary(milk.lm2)

<span data-ttu-id="3f44d-585">Tím se vygeneruje následující hello.</span><span class="sxs-lookup"><span data-stu-id="3f44d-585">This generates hello following.</span></span>

    ##
    ## Call:
    ## lm(formula = Milk.Prod ~ Time + I(Month.Count^3) + Month - 1,
    ##     data = cadairytrain)
    ##
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max
    ## -0.06879 -0.01693  0.00346  0.01543  0.08726
    ##
    ## Coefficients:
    ##                   Estimate Std. Error t value Pr(>|t|)
    ## Time              1.57e-09   2.72e-11    57.7   <2e-16 ***
    ## I(Month.Count^3) -3.74e-08   1.57e-09   -23.8   <2e-16 ***
    ## MonthApr          6.40e+00   2.63e-02   243.3   <2e-16 ***
    ## MonthAug          6.38e+00   2.63e-02   242.2   <2e-16 ***
    ## MonthDec          6.38e+00   2.64e-02   241.9   <2e-16 ***
    ## MonthFeb          6.31e+00   2.63e-02   240.1   <2e-16 ***
    ## MonthJan          6.39e+00   2.63e-02   243.1   <2e-16 ***
    ## MonthJul          6.39e+00   2.63e-02   242.6   <2e-16 ***
    ## MonthJun          6.38e+00   2.63e-02   242.4   <2e-16 ***
    ## MonthMar          6.42e+00   2.63e-02   244.2   <2e-16 ***
    ## MonthMay          6.43e+00   2.63e-02   244.3   <2e-16 ***
    ## MonthNov          6.34e+00   2.63e-02   240.6   <2e-16 ***
    ## MonthOct          6.37e+00   2.63e-02   241.8   <2e-16 ***
    ## MonthSep          6.34e+00   2.63e-02   240.6   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ##
    ## Residual standard error: 0.0263 on 202 degrees of freedom
    ## Multiple R-squared:     1,    Adjusted R-squared:     1
    ## F-statistic: 1.42e+06 on 14 and 202 DF,  p-value: <2e-16

<span data-ttu-id="3f44d-586">Vidíme, že hello modelu už má pro průnik a 12 měsíc důležité faktory.</span><span class="sxs-lookup"><span data-stu-id="3f44d-586">We see that hello model no longer has an intercept term and has 12 significant month factors.</span></span> <span data-ttu-id="3f44d-587">Toto je přesně jsme chtěli toosee.</span><span class="sxs-lookup"><span data-stu-id="3f44d-587">This is exactly what we wanted toosee.</span></span>

<span data-ttu-id="3f44d-588">Provedeme jiný čas řady výkresu z hello kalifornské produkci mléka data toosee, jak dobře funguje hello sezónní modelu.</span><span class="sxs-lookup"><span data-stu-id="3f44d-588">Let's make another time series plot of hello California dairy production data toosee how well hello seasonal model is working.</span></span> <span data-ttu-id="3f44d-589">Následující kód v hello Azure Machine Learning hello je přidali [spustit skript jazyka R] [ execute-r-script] toocreate hello modelu a ujistěte se, vykreslení.</span><span class="sxs-lookup"><span data-stu-id="3f44d-589">I have added hello following code in hello Azure Machine Learning [Execute R Script][execute-r-script] toocreate hello model and make a plot.</span></span>

    milk.lm2 <- lm(Milk.Prod ~ Time + I(Month.Count^3) + Month - 1, data = cadairytrain)

    plot(cadairytrain$Time, cadairytrain$Milk.Prod, xlab = "Time", ylab = "Log CA Milk Production 1000s lb", type = "l")
    lines(cadairytrain$Time, predict(milk.lm2, cadairytrain), lty = 2, col = 2)

<span data-ttu-id="3f44d-590">Spuštění tohoto kódu v Azure Machine Learning vytvoří hello výkresu znázorňuje obrázek 24.</span><span class="sxs-lookup"><span data-stu-id="3f44d-590">Running this code in Azure Machine Learning produces hello plot shown in Figure 24.</span></span>

![Produkce mléka kalifornské s modelem včetně sezónní efekty](./media/machine-learning-r-quickstart/unnamed-chunk-20.png)

<span data-ttu-id="3f44d-592">*Obrázek 24. Produkce mléka kalifornské s modelem včetně sezónní účinky.*</span><span class="sxs-lookup"><span data-stu-id="3f44d-592">*Figure 24. California milk production with model including seasonal effects.*</span></span>

<span data-ttu-id="3f44d-593">Hello shody toohello data zobrazená v 24 obrázek je spíš podporovat.</span><span class="sxs-lookup"><span data-stu-id="3f44d-593">hello fit toohello data shown in Figure 24 is rather encouraging.</span></span> <span data-ttu-id="3f44d-594">Vyhledejte přiměřené hello sezónní efekt (měsíční variace) i hello trend.</span><span class="sxs-lookup"><span data-stu-id="3f44d-594">Both hello trend and hello seasonal effect (monthly variation) look reasonable.</span></span>

<span data-ttu-id="3f44d-595">Jako další kontrolu na našem modelu můžeme Podíváme se na toto políčko, budou hello.</span><span class="sxs-lookup"><span data-stu-id="3f44d-595">As another check on our model, let's have a look at hello residuals.</span></span> <span data-ttu-id="3f44d-596">Hello následující kód výpočtů hello předpovězené hodnoty z našich dva modely, vypočítá hello zbytky hello sezónní modelu a poté ukazuje zeměpisný tyto zbytky pro hello Cvičná data.</span><span class="sxs-lookup"><span data-stu-id="3f44d-596">hello following code computes hello predicted values from our two models, computes hello residuals for hello seasonal model, and then plots these residuals for hello training data.</span></span>

    ## Compute predictions from our models
    predict1  <- predict(milk.lm, cadairydata)
    predict2  <- predict(milk.lm2, cadairydata)

    ## Compute and plot hello residuals
    residuals <- cadairydata$Milk.Prod - predict2
    plot(cadairytrain$Time, residuals[1:216], xlab = "Time", ylab ="Residuals of Seasonal Model")

<span data-ttu-id="3f44d-597">vykreslení zbytkové Hello se zobrazí v obrázku 25.</span><span class="sxs-lookup"><span data-stu-id="3f44d-597">hello residual plot is shown in Figure 25.</span></span>

![Toto políčko, budou hello sezónní modelu pro hello Cvičná data](./media/machine-learning-r-quickstart/unnamed-chunk-21.png)

<span data-ttu-id="3f44d-599">*Obrázek 25. Toto políčko, budou hello sezónní modelu pro hello Cvičná data.*</span><span class="sxs-lookup"><span data-stu-id="3f44d-599">*Figure 25. Residuals of hello seasonal model for hello training data.*</span></span>

<span data-ttu-id="3f44d-600">Tyto zbytky vypadat přiměřené.</span><span class="sxs-lookup"><span data-stu-id="3f44d-600">These residuals look reasonable.</span></span> <span data-ttu-id="3f44d-601">Neexistuje žádná konkrétní struktura, s výjimkou hello účinku poklesu hello 2008-2009, který našeho modelu neanalyzuje zvlášť dobře.</span><span class="sxs-lookup"><span data-stu-id="3f44d-601">There is no particular structure, except hello effect of hello 2008-2009 recession, which our model does not account for particularly well.</span></span>

<span data-ttu-id="3f44d-602">vykreslení Hello znázorňuje obrázek 25 je užitečné pro zjišťování v hello toto políčko, budou všechny vzory závislá na čase.</span><span class="sxs-lookup"><span data-stu-id="3f44d-602">hello plot shown in Figure 25 is useful for detecting any time-dependent patterns in hello residuals.</span></span> <span data-ttu-id="3f44d-603">Hello explicitní přístup výpočetních a vykreslení hello toto políčko, budou I používá umístí hello toto políčko, budou v pořadí časů na vykreslení hello.</span><span class="sxs-lookup"><span data-stu-id="3f44d-603">hello explicit approach of computing and plotting hello residuals I used places hello residuals in time order on hello plot.</span></span> <span data-ttu-id="3f44d-604">Pokud na hello druhé straně I měl vykreslí `milk.lm$residuals`, vykreslení hello by byly v pořadí čas.</span><span class="sxs-lookup"><span data-stu-id="3f44d-604">If, on hello other hand, I had plotted `milk.lm$residuals`, hello plot would not have been in time order.</span></span>

<span data-ttu-id="3f44d-605">Můžete také použít `plot.lm()` tooproduce řady diagnostických pozemků.</span><span class="sxs-lookup"><span data-stu-id="3f44d-605">You can also use `plot.lm()` tooproduce a series of diagnostic plots.</span></span>

    ## Show hello diagnostic plots for hello model
    plot(milk.lm2, ask = FALSE)

<span data-ttu-id="3f44d-606">Tento kód vytvoří řady diagnostických pozemků znázorňuje obrázek 26.</span><span class="sxs-lookup"><span data-stu-id="3f44d-606">This code produces a series of diagnostic plots shown in Figure 26.</span></span>

![První diagnostiky pozemků pro model sezónní hello](./media/machine-learning-r-quickstart/unnamed-chunk-221.png)

![Druhý diagnostiky pozemků pro model sezónní hello](./media/machine-learning-r-quickstart/unnamed-chunk-222.png)

![Třetí diagnostiky pozemků pro model sezónní hello](./media/machine-learning-r-quickstart/unnamed-chunk-223.png)

![Čtvrtý diagnostiky pozemků pro model sezónní hello](./media/machine-learning-r-quickstart/unnamed-chunk-224.png)

<span data-ttu-id="3f44d-611">*Obrázek 26. Diagnostika ukazuje zeměpisný hello sezónní modelu.*</span><span class="sxs-lookup"><span data-stu-id="3f44d-611">*Figure 26. Diagnostic plots for hello seasonal model.*</span></span>

<span data-ttu-id="3f44d-612">Existuje několik vysoké míry bodů, které jsou určené v těchto pozemků, ale nic toocause velmi významný.</span><span class="sxs-lookup"><span data-stu-id="3f44d-612">There are a few highly influential points identified in these plots, but nothing toocause great concern.</span></span> <span data-ttu-id="3f44d-613">Navíc jsme můžete zobrazit z hello normální Q-Q výkresu že hello toto políčko, budou se zavřít toonormally distribuována, důležité předpokládá pro lineární modely.</span><span class="sxs-lookup"><span data-stu-id="3f44d-613">Further, we can see from hello Normal Q-Q plot that hello residuals are close toonormally distributed, an important assumption for linear models.</span></span>

### <a name="forecasting-and-model-evaluation"></a><span data-ttu-id="3f44d-614">Vyhodnocení prognózy a modelu</span><span class="sxs-lookup"><span data-stu-id="3f44d-614">Forecasting and model evaluation</span></span>
<span data-ttu-id="3f44d-615">Existuje pouze jeden další toocomplete toodo věc našem příkladu.</span><span class="sxs-lookup"><span data-stu-id="3f44d-615">There is just one more thing toodo toocomplete our example.</span></span> <span data-ttu-id="3f44d-616">Budeme potřebovat toocompute prognózy a měření hello chyba proti hello skutečná data.</span><span class="sxs-lookup"><span data-stu-id="3f44d-616">We need toocompute forecasts and measure hello error against hello actual data.</span></span> <span data-ttu-id="3f44d-617">Naše prognózy bude pro hello 12 měsíců od 2013.</span><span class="sxs-lookup"><span data-stu-id="3f44d-617">Our forecast will be for hello 12 months of 2013.</span></span> <span data-ttu-id="3f44d-618">Jsme můžete vypočítat k chybě měr pro prognózy toohello skutečné dat, která není součástí naší datové sadě školení.</span><span class="sxs-lookup"><span data-stu-id="3f44d-618">We can compute an error measure for this forecast toohello actual data that is not part of our training dataset.</span></span> <span data-ttu-id="3f44d-619">Kromě toho jsme můžete porovnat výkon na hello 18 let školení data toohello dobu 12 měsíců testovacích dat.</span><span class="sxs-lookup"><span data-stu-id="3f44d-619">Additionally, we can compare performance on hello 18 years of training data toohello 12 months of test data.</span></span>  

<span data-ttu-id="3f44d-620">Se používá počet metriky výkonu hello toomeasure modelů časové řady.</span><span class="sxs-lookup"><span data-stu-id="3f44d-620">A number of metrics are used toomeasure hello performance of time series models.</span></span> <span data-ttu-id="3f44d-621">V našem případě použijeme chyba hello kořenové směrodatná (RMS).</span><span class="sxs-lookup"><span data-stu-id="3f44d-621">In our case we will use hello root mean square (RMS) error.</span></span> <span data-ttu-id="3f44d-622">Hello následující funkce vypočítá chybě hello RMS mezi dvou řad.</span><span class="sxs-lookup"><span data-stu-id="3f44d-622">hello following function computes hello RMS error between two series.</span></span>  

    RMS.error <- function(series1, series2, is.log = TRUE, min.length = 2){
      ## Function toocompute hello RMS error or difference between two
      ## series or vectors

      messages <- c("ERROR: Input arguments toofunction RMS.error of wrong type encountered",
                    "ERROR: Input vector toofunction RMS.error is too short",
                    "ERROR: Input vectors toofunction RMS.error must be of same length",
                    "WARNING: Funtion rms.error has received invald input time series.")

      ## Check hello arguments
      if(!is.numeric(series1) | !is.numeric(series2) | !is.logical(is.log) | !is.numeric(min.length)) {
        warning(messages[1])
        return(NA)}

      if(length(series1) < min.length) {
        warning(messages[2])
        return(NA)}

      if((length(series1) != length(series2))) {
           warning(messages[3])
        return(NA)}

      ## If is.log is TRUE exponentiate hello values, else just copy
      if(is.log) {
        tryCatch( {
          temp1 <- exp(series1)
          temp2 <- exp(series2) },
          error = function(e){warning(messages[4]); NA}
        )
      } else {
        temp1 <- series1
        temp2 <- series2
      }

     ## Compute predictions from our models
    predict1  <- predict(milk.lm, cadairydata)
    predict2  <- predict(milk.lm2, cadairydata)

    ## Compute hello RMS error in a dataframe
      tryCatch( {
        sqrt(sum((temp1 - temp2)^2) / length(temp1))},
        error = function(e){warning(messages[4]); NA})
    }

<span data-ttu-id="3f44d-623">Stejně jako u hello `log.transform()` hello funkce jsme popsané v části "Hodnoty transformace", je poměrně velké množství kontrolu a výjimky obnovení kód chyby v této funkci.</span><span class="sxs-lookup"><span data-stu-id="3f44d-623">As with hello `log.transform()` function we discussed in hello "Value transformations" section, there is quite a lot of error checking and exception recovery code in this function.</span></span> <span data-ttu-id="3f44d-624">Principy Hello těmto nekompatibilitám jsou hello stejné.</span><span class="sxs-lookup"><span data-stu-id="3f44d-624">hello principles employed are hello same.</span></span> <span data-ttu-id="3f44d-625">Hello práci na dvou místech uzavřen do `tryCatch()`.</span><span class="sxs-lookup"><span data-stu-id="3f44d-625">hello work is done in two places wrapped in `tryCatch()`.</span></span> <span data-ttu-id="3f44d-626">Nejprve hello časové řady jsou exponentiated, protože Pracujeme s protokoly hello hello hodnot.</span><span class="sxs-lookup"><span data-stu-id="3f44d-626">First, hello time series are exponentiated, since we have been working with hello logs of hello values.</span></span> <span data-ttu-id="3f44d-627">Druhý se počítá hello skutečné chybové RMS.</span><span class="sxs-lookup"><span data-stu-id="3f44d-627">Second, hello actual RMS error is computed.</span></span>  

<span data-ttu-id="3f44d-628">Vybaven funkce toomeasure hello chybu RMS, umožňuje vytvářet a výstupní dataframe, obsahující chyby hello RMS.</span><span class="sxs-lookup"><span data-stu-id="3f44d-628">Equipped with a function toomeasure hello RMS error, let's build and output a dataframe containing hello RMS errors.</span></span> <span data-ttu-id="3f44d-629">Jsme bude obsahovat podmínky pro model trend hello samostatně a úplný model hello sezónní faktory.</span><span class="sxs-lookup"><span data-stu-id="3f44d-629">We will include terms for hello trend model alone and hello complete model with seasonal factors.</span></span> <span data-ttu-id="3f44d-630">Hello následující kód hello úlohy pomocí hello dva lineární modely, které jsme mít sestavený.</span><span class="sxs-lookup"><span data-stu-id="3f44d-630">hello following code does hello job by using hello two linear models we have constructed.</span></span>

    ## Compute hello RMS error in a dataframe
    ## Include hello row names in hello first column so they will
    ## appear in hello output of hello Execute R Script
    RMS.df  <-  data.frame(
    rowNames = c("Trend Model", "Seasonal Model"),
      Traing = c(
      RMS.error(predict1[1:216], cadairydata$Milk.Prod[1:216]),
      RMS.error(predict2[1:216], cadairydata$Milk.Prod[1:216])),
      Forecast = c(
        RMS.error(predict1[217:228], cadairydata$Milk.Prod[217:228]),
        RMS.error(predict2[217:228], cadairydata$Milk.Prod[217:228]))
    )
    RMS.df

    ## hello following line should be executed only when running in
    ## Azure Machine Learning Studio
    maml.mapOutputPort('RMS.df')

<span data-ttu-id="3f44d-631">Spuštění tohoto kódu vytváří výstup hello znázorňuje obrázek 27 na hello výstupním portem datové sady výsledků.</span><span class="sxs-lookup"><span data-stu-id="3f44d-631">Running this code produces hello output shown in Figure 27 at hello Result Dataset output port.</span></span>

![Porovnání RMS chyb pro modely hello][26]

<span data-ttu-id="3f44d-633">*Obrázek 27. Porovnání RMS chyb pro modely hello.*</span><span class="sxs-lookup"><span data-stu-id="3f44d-633">*Figure 27. Comparison of RMS errors for hello models.*</span></span>

<span data-ttu-id="3f44d-634">Z těchto výsledků vidíte, že přidání hello sezónní faktory toohello model snižuje chyby RMS hello výrazně.</span><span class="sxs-lookup"><span data-stu-id="3f44d-634">From these results, we see that adding hello seasonal factors toohello model reduces hello RMS error significantly.</span></span> <span data-ttu-id="3f44d-635">Příliš logicky hello RMS chybě hello cvičení dat je bit menší než hello prognózy.</span><span class="sxs-lookup"><span data-stu-id="3f44d-635">Not too surprisingly, hello RMS error for hello training data is a bit less than for hello forecast.</span></span>

## <span data-ttu-id="3f44d-636"><a id="appendixa"></a>Příloha A: Průvodce tooRStudio</span><span class="sxs-lookup"><span data-stu-id="3f44d-636"><a id="appendixa"></a>APPENDIX A: Guide tooRStudio</span></span>
<span data-ttu-id="3f44d-637">Rstudia je velmi dobře zdokumentovat, tak v tomto dodatku I zajistí některé odkazy toohello klíče oddílů tooget hello Rstudia dokumentace, kterou jste zahájili.</span><span class="sxs-lookup"><span data-stu-id="3f44d-637">RStudio is quite well documented, so in this appendix I will provide some links toohello key sections of hello RStudio documentation tooget you started.</span></span>

1. <span data-ttu-id="3f44d-638">Vytváření projektů</span><span class="sxs-lookup"><span data-stu-id="3f44d-638">Creating projects</span></span>
   
   <span data-ttu-id="3f44d-639">Můžete uspořádat a spravovat váš kód R do projektů pomocí Rstudia.</span><span class="sxs-lookup"><span data-stu-id="3f44d-639">You can organize and manage your R code into projects by using RStudio.</span></span> <span data-ttu-id="3f44d-640">Hello dokumentace, která používá projekty lze najít na https://support.rstudio.com/hc/articles/200526207-Using-Projects.</span><span class="sxs-lookup"><span data-stu-id="3f44d-640">hello documentation that uses projects can be found at https://support.rstudio.com/hc/articles/200526207-Using-Projects.</span></span>
   
   <span data-ttu-id="3f44d-641">Doporučujeme I postupujte podle těchto pokynů a vytvořte projekt hello R příklady kódu v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="3f44d-641">I recommend that you follow these directions and create a project for hello R code examples in this document.</span></span>  
2. <span data-ttu-id="3f44d-642">Úpravy a spouštění kódu jazyka R</span><span class="sxs-lookup"><span data-stu-id="3f44d-642">Editing and executing R code</span></span>
   
   <span data-ttu-id="3f44d-643">Rstudia poskytuje integrované prostředí pro úpravy a provádění kódu jazyka R.</span><span class="sxs-lookup"><span data-stu-id="3f44d-643">RStudio provides an integrated environment for editing and executing R code.</span></span> <span data-ttu-id="3f44d-644">Dokumentace lze najít na https://support.rstudio.com/hc/articles/200484448-Editing-and-Executing-Code.</span><span class="sxs-lookup"><span data-stu-id="3f44d-644">Documentation can be found at https://support.rstudio.com/hc/articles/200484448-Editing-and-Executing-Code.</span></span>
3. <span data-ttu-id="3f44d-645">Ladění</span><span class="sxs-lookup"><span data-stu-id="3f44d-645">Debugging</span></span>
   
   <span data-ttu-id="3f44d-646">Rstudia zahrnuje výkonné možnosti ladění.</span><span class="sxs-lookup"><span data-stu-id="3f44d-646">RStudio includes powerful debugging capabilities.</span></span> <span data-ttu-id="3f44d-647">Dokumentace pro tyto funkce jsou v https://support.rstudio.com/hc/articles/200713843-Debugging-with-RStudio.</span><span class="sxs-lookup"><span data-stu-id="3f44d-647">Documentation for these features is at https://support.rstudio.com/hc/articles/200713843-Debugging-with-RStudio.</span></span>
   
   <span data-ttu-id="3f44d-648">řešení potíží Funkce Hello zarážek jsou popsány v https://support.rstudio.com/hc/articles/200534337-Breakpoint-Troubleshooting.</span><span class="sxs-lookup"><span data-stu-id="3f44d-648">hello breakpoint troubleshooting features are documented at https://support.rstudio.com/hc/articles/200534337-Breakpoint-Troubleshooting.</span></span>

## <span data-ttu-id="3f44d-649"><a id="appendixb"></a>Příloha B: Další čtení</span><span class="sxs-lookup"><span data-stu-id="3f44d-649"><a id="appendixb"></a>APPENDIX B: Further reading</span></span>
<span data-ttu-id="3f44d-650">Tato R programování hello kurz obsahuje základní informace o tom, co jste třeba toouse hello jazyk R s Azure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="3f44d-650">This R programming tutorial covers hello basics of what you need toouse hello R language with Azure Machine Learning Studio.</span></span> <span data-ttu-id="3f44d-651">Pokud nejste obeznámeni s R, jsou k dispozici na CRAN dva úvodní informace:</span><span class="sxs-lookup"><span data-stu-id="3f44d-651">If you are not familiar with R, two introductions are available on CRAN:</span></span>

* <span data-ttu-id="3f44d-652">R pro začátečníky podle Emmanuel Paradis je vhodné místo toostart v http://cran.r-project.org/doc/contrib/Paradis-rdebuts_en.pdf.</span><span class="sxs-lookup"><span data-stu-id="3f44d-652">R for Beginners by Emmanuel Paradis is a good place toostart at http://cran.r-project.org/doc/contrib/Paradis-rdebuts_en.pdf.</span></span>  
* <span data-ttu-id="3f44d-653">Úvod tooR n. dokončeno</span><span class="sxs-lookup"><span data-stu-id="3f44d-653">An Introduction tooR by W. N.</span></span> <span data-ttu-id="3f44d-654">Venables et.</span><span class="sxs-lookup"><span data-stu-id="3f44d-654">Venables et.</span></span> <span data-ttu-id="3f44d-655">Al.</span><span class="sxs-lookup"><span data-stu-id="3f44d-655">al.</span></span> <span data-ttu-id="3f44d-656">Klient se přepne do trochu další hloubku, http://cran.r-project.org/doc/manuals/R-intro.html.</span><span class="sxs-lookup"><span data-stu-id="3f44d-656">goes into a bit more depth, at http://cran.r-project.org/doc/manuals/R-intro.html.</span></span>

<span data-ttu-id="3f44d-657">Neexistují mnoho knih na R, který můžete začít pracovat.</span><span class="sxs-lookup"><span data-stu-id="3f44d-657">There are many books on R that can help you get started.</span></span> <span data-ttu-id="3f44d-658">Zde najdete několik, které užitečné:</span><span class="sxs-lookup"><span data-stu-id="3f44d-658">Here are a few I find useful:</span></span>

* <span data-ttu-id="3f44d-659">Hello obrázky programování R: A prohlídka z statistické softwaru návrh podle Norman Matloff je vynikající Úvod tooprogramming v jazyce R.</span><span class="sxs-lookup"><span data-stu-id="3f44d-659">hello Art of R Programming: A Tour of Statistical Software Design by Norman Matloff is an excellent introduction tooprogramming in R.</span></span>  
* <span data-ttu-id="3f44d-660">R kuchařka podle Paul Teetor poskytuje problém a jeho řešení přístup toousing R.</span><span class="sxs-lookup"><span data-stu-id="3f44d-660">R Cookbook by Paul Teetor provides a problem and solution approach toousing R.</span></span>  
* <span data-ttu-id="3f44d-661">R v akce Robert Kabacoff je další užitečné úvodní adresáře.</span><span class="sxs-lookup"><span data-stu-id="3f44d-661">R in Action by Robert Kabacoff is another useful introductory book.</span></span> <span data-ttu-id="3f44d-662">Hello doprovodné rychlé R webu je užitečné prostředek v http://www.statmethods.net/.</span><span class="sxs-lookup"><span data-stu-id="3f44d-662">hello companion Quick R website is a useful resource at http://www.statmethods.net/.</span></span>
* <span data-ttu-id="3f44d-663">R Inferno podle Patrik popáleniny je, že je k dispozici zdarma http://www.burns-stat.com/documents/books/the-r-inferno/ překvapivě vážný adresáře, která pracuje s počtem složité a obtížně témata, která může být zjistil při programování v R. hello adresáře.</span><span class="sxs-lookup"><span data-stu-id="3f44d-663">R Inferno by Patrick Burns is a surprisingly humorous book that deals with a number of tricky and difficult topics that can be encountered when programming in R. hello book is available for free at http://www.burns-stat.com/documents/books/the-r-inferno/.</span></span>
* <span data-ttu-id="3f44d-664">Pokud chcete podrobné informace do Pokročilá témata v R, podívejte se na hello kniha Upřesnit R podle Hadley Wickham.</span><span class="sxs-lookup"><span data-stu-id="3f44d-664">If you want a deep dive into advanced topics in R, have a look at hello book Advanced R by Hadley Wickham.</span></span> <span data-ttu-id="3f44d-665">je k dispozici zdarma v http://adv-r.had.co.nz/ Hello online verze této příručky.</span><span class="sxs-lookup"><span data-stu-id="3f44d-665">hello online version of this book is available for free at http://adv-r.had.co.nz/.</span></span>

<span data-ttu-id="3f44d-666">Katalogu R časové řady balíčků naleznete v zobrazení úlohy CRAN hello pro analýzu časových řad: http://cran.r-project.org/web/views/TimeSeries.html.</span><span class="sxs-lookup"><span data-stu-id="3f44d-666">A catalogue of R time series packages can be found in hello CRAN Task View for time series analysis: http://cran.r-project.org/web/views/TimeSeries.html.</span></span> <span data-ttu-id="3f44d-667">Informace o určité časové řady objekt balíčky by měl naleznete v dokumentaci toohello pro tento balíček.</span><span class="sxs-lookup"><span data-stu-id="3f44d-667">For information on specific time series object packages, you should refer toohello documentation for that package.</span></span>

<span data-ttu-id="3f44d-668">Příručka Hello úvodní časové řady s R Paul Cowpertwait a Andrew Metcalfe obsahuje úvod toousing R pro analýzu časových řad.</span><span class="sxs-lookup"><span data-stu-id="3f44d-668">hello book Introductory Time Series with R by Paul Cowpertwait and Andrew Metcalfe provides an introduction toousing R for time series analysis.</span></span> <span data-ttu-id="3f44d-669">Mnoho více teoretické texty R příklady.</span><span class="sxs-lookup"><span data-stu-id="3f44d-669">Many more theoretical texts provide R examples.</span></span>

<span data-ttu-id="3f44d-670">Některé skvělé prostředků z Internetu:</span><span class="sxs-lookup"><span data-stu-id="3f44d-670">Some great internet resources:</span></span>

* <span data-ttu-id="3f44d-671">DataCamp: DataCamp učí R v hello pohodlí prohlížeč s video lekce a kódování cvičení.</span><span class="sxs-lookup"><span data-stu-id="3f44d-671">DataCamp: DataCamp teaches R in hello comfort of your browser with video lessons and coding exercises.</span></span> <span data-ttu-id="3f44d-672">Existují interaktivní kurzy o hello nejnovější R technik a balíčků.</span><span class="sxs-lookup"><span data-stu-id="3f44d-672">There are interactive tutorials on hello latest R techniques and packages.</span></span> <span data-ttu-id="3f44d-673">Přijmout hello volné interaktivní R kurz na https://www.datacamp.com/courses/introduction-to-r</span><span class="sxs-lookup"><span data-stu-id="3f44d-673">Take hello free interactive R tutorial at https://www.datacamp.com/courses/introduction-to-r</span></span>
* <span data-ttu-id="3f44d-674">Průvodce Začínáme pracovat s R z Programiz https://www.programiz.com/r-programming</span><span class="sxs-lookup"><span data-stu-id="3f44d-674">A guide on Getting started with R from Programiz https://www.programiz.com/r-programming</span></span>
* <span data-ttu-id="3f44d-675">Rychlý kurz R podle Jan černé z Clarkson univerzity http://www.cyclismo.org/tutorial/R/</span><span class="sxs-lookup"><span data-stu-id="3f44d-675">A quick R tutorial by Kelly Black from Clarkson University http://www.cyclismo.org/tutorial/R/</span></span>
* <span data-ttu-id="3f44d-676">60 + R prostředky uvedené v http://www.computerworld.com/article/2497464/business-intelligence-60-r-resources-to-improve-your-data-skills.html</span><span class="sxs-lookup"><span data-stu-id="3f44d-676">60+ R resources listed at http://www.computerworld.com/article/2497464/business-intelligence-60-r-resources-to-improve-your-data-skills.html</span></span>

<!--Image references-->
[1]: ./media/machine-learning-r-quickstart/fig1.png
[2]: ./media/machine-learning-r-quickstart/fig2.png
[3]: ./media/machine-learning-r-quickstart/fig3.png
[4]: ./media/machine-learning-r-quickstart/fig4.png
[5]: ./media/machine-learning-r-quickstart/fig5.png
[6]: ./media/machine-learning-r-quickstart/fig6.png
[7]: ./media/machine-learning-r-quickstart/fig7.png
[8]: ./media/machine-learning-r-quickstart/fig8.png
[9]: ./media/machine-learning-r-quickstart/fig9.png
[10]: ./media/machine-learning-r-quickstart/fig10.png
[11]: ./media/machine-learning-r-quickstart/fig11.png
[12]: ./media/machine-learning-r-quickstart/fig12.png
[13]: ./media/machine-learning-r-quickstart/fig13.png
[14]: ./media/machine-learning-r-quickstart/fig14.png
[15]: ./media/machine-learning-r-quickstart/fig15.png
[16]: ./media/machine-learning-r-quickstart/fig16.png
[17]: ./media/machine-learning-r-quickstart/fig17.png
[18]: ./media/machine-learning-r-quickstart/fig18.png
[19]: ./media/machine-learning-r-quickstart/fig19.png
[20]: ./media/machine-learning-r-quickstart/fig20.png
[21]: ./media/machine-learning-r-quickstart/fig21.png
[22]: ./media/machine-learning-r-quickstart/fig22.png

[26]: ./media/machine-learning-r-quickstart/fig26.png

<!--links-->
[appendixa]: #appendixa
[download]: https://azurebigdatatutorials.blob.core.windows.net/rquickstart/RFiles.zip


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
