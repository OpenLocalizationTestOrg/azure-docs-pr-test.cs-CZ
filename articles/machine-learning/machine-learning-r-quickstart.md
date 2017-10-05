---
title: "Rychlý úvodní kurz pro jazyk R pro Machine Learning | Microsoft Docs"
description: "Použijte tento kurz programovací R začít rychle vytvářet prognózy řešení pomocí jazyka R s Azure Machine Learning Studio."
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
ms.openlocfilehash: 598f5ce445e520b6cdc347c80f7f3dcbc9c2c9e5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="quickstart-tutorial-for-the-r-programming-language-for-azure-machine-learning"></a><span data-ttu-id="be211-104">Stručný úvodní kurz k programovacímu jazyku R pro službu Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="be211-104">Quickstart tutorial for the R programming language for Azure Machine Learning</span></span>

<!-- Stephen F Elston, Ph.D. -->

## <a name="introduction"></a><span data-ttu-id="be211-105">Úvod</span><span class="sxs-lookup"><span data-stu-id="be211-105">Introduction</span></span>
<span data-ttu-id="be211-106">Tento rychlý úvodní kurz vám pomůže rychle spustit pomocí programovacího jazyka R rozšíření Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="be211-106">This quickstart tutorial helps you quickly start extending Azure Machine Learning by using the R programming language.</span></span> <span data-ttu-id="be211-107">V tomto kurzu R programování pro vytvoření, testování a spouštění R kódu v rámci Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="be211-107">Follow this R programming tutorial to create, test and execute R code within Azure Machine Learning.</span></span> <span data-ttu-id="be211-108">Při práci prostřednictvím kurzu vytvoříte pomocí jazyka R v Azure Machine Learning kompletního řešení prognózy.</span><span class="sxs-lookup"><span data-stu-id="be211-108">As you work through tutorial, you will create a complete forecasting solution by using the R language in Azure Machine Learning.</span></span>  

<span data-ttu-id="be211-109">Microsoft Azure Machine Learning obsahuje mnoho výkonné machine learning a data moduly manipulaci s.</span><span class="sxs-lookup"><span data-stu-id="be211-109">Microsoft Azure Machine Learning contains many powerful machine learning and data manipulation modules.</span></span> <span data-ttu-id="be211-110">Efektivní jazyk R popsán jako lingua franca obchodu Analytics.</span><span class="sxs-lookup"><span data-stu-id="be211-110">The powerful R language has been described as the lingua franca of analytics.</span></span> <span data-ttu-id="be211-111">Manipulace s analytickými funkcemi a data v Azure Machine Learning brouka, lze rozšířit pomocí R. Tato kombinace poskytne škálovatelnost a snadné nasazení Azure Machine Learning flexibilitu a hloubkové analytics R.</span><span class="sxs-lookup"><span data-stu-id="be211-111">Happily, analytics and data manipulation in Azure Machine Learning can be extended by using R. This combination provides the scalability and ease of deployment of Azure Machine Learning with the flexibility and deep analytics of R.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

### <a name="forecasting-and-the-dataset"></a><span data-ttu-id="be211-112">Vytváření prognóz a datové sady</span><span class="sxs-lookup"><span data-stu-id="be211-112">Forecasting and the dataset</span></span>
<span data-ttu-id="be211-113">Prognózy je metoda analytical široce zaměstnání a velmi užitečné.</span><span class="sxs-lookup"><span data-stu-id="be211-113">Forecasting is a widely employed and quite useful analytical method.</span></span> <span data-ttu-id="be211-114">Běžné používá rozsah od predikci prodeje sezónní položek, určení úrovní optimální inventáře pro predikci makroekonomické proměnné.</span><span class="sxs-lookup"><span data-stu-id="be211-114">Common uses range from predicting sales of seasonal items, determining optimal inventory levels, to predicting macroeconomic variables.</span></span> <span data-ttu-id="be211-115">Prognózy se většinou děje s modely časové řady.</span><span class="sxs-lookup"><span data-stu-id="be211-115">Forecasting is typically done with time series models.</span></span>

<span data-ttu-id="be211-116">Časových řad dat jsou data ve kterém mají hodnoty indexu čas.</span><span class="sxs-lookup"><span data-stu-id="be211-116">Time series data is data in which the values have a time index.</span></span> <span data-ttu-id="be211-117">Index čas může být pravidelné, např. každý měsíc nebo každou minutu nebo nestandardní.</span><span class="sxs-lookup"><span data-stu-id="be211-117">The time index can be regular, e.g. every month or every minute, or irregular.</span></span> <span data-ttu-id="be211-118">Model časové řady podle data časové řady.</span><span class="sxs-lookup"><span data-stu-id="be211-118">A time series model is based on time series data.</span></span> <span data-ttu-id="be211-119">R programovací jazyk obsahuje flexibilní framework a rozsáhlé analýzy pro data časové řady.</span><span class="sxs-lookup"><span data-stu-id="be211-119">The R programming language contains a flexible framework and extensive analytics for time series data.</span></span>

<span data-ttu-id="be211-120">V této příručce rychlý start jsme bude pracovat s produkční dojnic kalifornské a ceny data.</span><span class="sxs-lookup"><span data-stu-id="be211-120">In this quickstart guide we will be working with California dairy production and pricing data.</span></span> <span data-ttu-id="be211-121">Tato data zahrnují měsíční informace o provozní mléčných produktů a cenu mléka fat komoditním srovnávacího testu.</span><span class="sxs-lookup"><span data-stu-id="be211-121">This data includes monthly information on the production of several dairy products and the price of milk fat, a benchmark commodity.</span></span>

<span data-ttu-id="be211-122">Data použitá v tomto článku, spolu se skripty R, může být [stáhnout zde][download].</span><span class="sxs-lookup"><span data-stu-id="be211-122">The data used in this article, along with R scripts, can be [downloaded here][download].</span></span> <span data-ttu-id="be211-123">Tato data byla původně syntetizován z informací z univerzity Wisconsin v http://future.aae.wisc.edu/tab/production.html.</span><span class="sxs-lookup"><span data-stu-id="be211-123">This data was originally synthesized from information available from the University of Wisconsin at http://future.aae.wisc.edu/tab/production.html.</span></span>

### <a name="organization"></a><span data-ttu-id="be211-124">Organizace</span><span class="sxs-lookup"><span data-stu-id="be211-124">Organization</span></span>
<span data-ttu-id="be211-125">Jsme proběhne prostřednictvím několik kroků, jak zjistíte, jak k vytváření, testování a spouštění kódu manipulaci s R analytics a data v prostředí Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="be211-125">We will progress through several steps as you learn how to create, test and execute analytics and data manipulation R code in the Azure Machine Learning environment.</span></span>  

* <span data-ttu-id="be211-126">Nejprve se podíváme na základní informace o použití jazyka R v prostředí Azure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="be211-126">First we will explore the basics of using the R language in the Azure Machine Learning Studio environment.</span></span>
* <span data-ttu-id="be211-127">Potom jsme průběhu hovoříte o různé aspekty vstupně-výstupních operací pro data, kódu jazyka R a grafiky v prostředí Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="be211-127">Then we progress to discussing various aspects of I/O for data, R code and graphics in the Azure Machine Learning environment.</span></span>
* <span data-ttu-id="be211-128">První část naše řešení prognózy jsme se pak vytvořit tak, že vytvoříte kód pro čištění dat a transformace.</span><span class="sxs-lookup"><span data-stu-id="be211-128">We will then construct the first part of our forecasting solution by creating code for data cleaning and transformation.</span></span>
* <span data-ttu-id="be211-129">Naše data připravený provedeme analýzu korelací mezi několika proměnných v naší datové sadě.</span><span class="sxs-lookup"><span data-stu-id="be211-129">With our data prepared we will perform an analysis of the correlations between several of the variables in our dataset.</span></span>
* <span data-ttu-id="be211-130">Nakonec vytvoříme předpovědi modelu sezónní časové řady pro produkci mléka.</span><span class="sxs-lookup"><span data-stu-id="be211-130">Finally, we will create a seasonal time series forecasting model for milk production.</span></span>

## <span data-ttu-id="be211-131"><a id="mlstudio"></a>Komunikovat s jazyk R v Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="be211-131"><a id="mlstudio"></a>Interact with R language in Machine Learning Studio</span></span>
<span data-ttu-id="be211-132">Tato část vás provede některé základní informace o interakci s programovací jazyk R v prostředí nástroje Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="be211-132">This section takes you through some basics of interacting with the R programming language in the Machine Learning Studio environment.</span></span> <span data-ttu-id="be211-133">Jazyk R poskytuje výkonný nástroj pro vytvoření přizpůsobené analýzy a moduly manipulaci dat v prostředí Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="be211-133">The R language provides a powerful tool to create customized analytics and data manipulation modules within the Azure Machine Learning environment.</span></span>

<span data-ttu-id="be211-134">K vývoji, testování a ladění kódu jazyka R v malém měřítku použiji Rstudia.</span><span class="sxs-lookup"><span data-stu-id="be211-134">I will use RStudio to develop, test and debug R code on a small scale.</span></span> <span data-ttu-id="be211-135">Tento kód je pak vyjímání a vkládání do [spustit skript jazyka R] [ execute-r-script] modulu v nástroji Machine Learning Studio připraven ke spuštění.</span><span class="sxs-lookup"><span data-stu-id="be211-135">This code is then cut and paste into an [Execute R Script][execute-r-script] module in Machine Learning Studio ready to run.</span></span>  

### <a name="the-execute-r-script-module"></a><span data-ttu-id="be211-136">Modul spustit skript jazyka R</span><span class="sxs-lookup"><span data-stu-id="be211-136">The Execute R Script module</span></span>
<span data-ttu-id="be211-137">V rámci Machine Learning Studio, R skripty se spouštějí v rámci [spustit skript jazyka R] [ execute-r-script] modulu.</span><span class="sxs-lookup"><span data-stu-id="be211-137">Within Machine Learning Studio, R scripts are run within the [Execute R Script][execute-r-script] module.</span></span> <span data-ttu-id="be211-138">Příkladem [spustit skript jazyka R] [ execute-r-script] modulu v nástroji Machine Learning Studio je znázorněno na obrázku 1.</span><span class="sxs-lookup"><span data-stu-id="be211-138">An example of the [Execute R Script][execute-r-script] module in Machine Learning Studio is shown in Figure 1.</span></span>

 ![Programovací jazyk R: spuštění skriptu R modulu vybrané v nástroji Machine Learning Studio][1]

<span data-ttu-id="be211-140">*Obrázek 1. Zobrazuje vybraný modul spustit skript jazyka R prostředí Machine Learning Studio.*</span><span class="sxs-lookup"><span data-stu-id="be211-140">*Figure 1. The Machine Learning Studio environment showing the Execute R Script module selected.*</span></span>

<span data-ttu-id="be211-141">Odkaz na obrázek 1, podíváme se na některé z klíčových částí Machine Learning Studio prostředí pro práci s [spustit skript jazyka R] [ execute-r-script] modulu.</span><span class="sxs-lookup"><span data-stu-id="be211-141">Referring to Figure 1, let's look at some of the key parts of the Machine Learning Studio environment for working with the [Execute R Script][execute-r-script] module.</span></span>

* <span data-ttu-id="be211-142">V prostředním podokně se zobrazí moduly v experimentu.</span><span class="sxs-lookup"><span data-stu-id="be211-142">The modules in the experiment are shown in the center pane.</span></span>
* <span data-ttu-id="be211-143">Horní část v pravém podokně obsahuje okno zobrazení a úprava skripty R.</span><span class="sxs-lookup"><span data-stu-id="be211-143">The upper part of the right pane contains a window to view and edit your R scripts.</span></span>  
* <span data-ttu-id="be211-144">Dolní části pravém podokně jsou uvedeny některé vlastnosti [spustit skript jazyka R][execute-r-script].</span><span class="sxs-lookup"><span data-stu-id="be211-144">The lower part of right pane shows some properties of the [Execute R Script][execute-r-script].</span></span> <span data-ttu-id="be211-145">Protokoly chyb a výstupu můžete zobrazit kliknutím na příslušné body této podokna.</span><span class="sxs-lookup"><span data-stu-id="be211-145">You can view the error and output logs by clicking on the appropriate spots of this pane.</span></span>

<span data-ttu-id="be211-146">Jsme bude samozřejmě být hovoříte o [spustit skript jazyka R] [ execute-r-script] podrobněji ve zbývající části tohoto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="be211-146">We will, of course, be discussing the [Execute R Script][execute-r-script] in greater detail in the rest of this document.</span></span>

<span data-ttu-id="be211-147">Při práci s funkcemi, komplexní R, doporučujeme úpravy, testování a ladění ve Rstudia.</span><span class="sxs-lookup"><span data-stu-id="be211-147">When working with complex R functions, I recommend that you edit, test and debug in RStudio.</span></span> <span data-ttu-id="be211-148">Stejně jako u jakékoli vývoj softwaru postupně rozšiřovat kódu a testování na malé jednoduchých testovacích případech.</span><span class="sxs-lookup"><span data-stu-id="be211-148">As with any software development, extend your code incrementally and test it on small simple test cases.</span></span> <span data-ttu-id="be211-149">Potom kopírování a vkládání funkcí do okno skriptu R [spustit skript jazyka R] [ execute-r-script] modulu.</span><span class="sxs-lookup"><span data-stu-id="be211-149">Then cut and paste your functions into the R script window of the [Execute R Script][execute-r-script] module.</span></span> <span data-ttu-id="be211-150">Tento přístup umožňuje plně využívat Rstudia integrované vývojové prostředí (IDE) a možnosti Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="be211-150">This approach allows you to harness both the RStudio integrated development environment (IDE) and the power of Azure Machine Learning.</span></span>  

#### <a name="execute-r-code"></a><span data-ttu-id="be211-151">Spuštění kódu jazyka R</span><span class="sxs-lookup"><span data-stu-id="be211-151">Execute R code</span></span>
<span data-ttu-id="be211-152">Žádný kód R v [spustit skript jazyka R] [ execute-r-script] modulu se spustí, když spustíte kliknutím na experiment **spustit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="be211-152">Any R code in the [Execute R Script][execute-r-script] module will execute when you run the experiment by clicking on the **Run** button.</span></span> <span data-ttu-id="be211-153">Po dokončení provádění zaškrtnutí se zobrazí na [spustit skript jazyka R] [ execute-r-script] ikonu.</span><span class="sxs-lookup"><span data-stu-id="be211-153">When execution has completed, a check mark will appear on the [Execute R Script][execute-r-script] icon.</span></span>

#### <a name="defensive-r-coding-for-azure-machine-learning"></a><span data-ttu-id="be211-154">Obranným R kódování pro Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="be211-154">Defensive R coding for Azure Machine Learning</span></span>
<span data-ttu-id="be211-155">Pokud vyvíjíte kódu jazyka R pro vyslovení, webové služby pomocí Azure Machine Learning, byste měli výborný naplánovat, jak váš kód se bude zabývat vstup neočekávaná data a výjimky.</span><span class="sxs-lookup"><span data-stu-id="be211-155">If you are developing R code for, say, a web service by using Azure Machine Learning, you should definitely plan how your code will deal with an unexpected data input and exceptions.</span></span> <span data-ttu-id="be211-156">Pokud chcete zachovat přehlednost, nebyly připojená mnohem cestě kontrolují nebo zpracování výjimek v většinu příklady kódu ukazuje.</span><span class="sxs-lookup"><span data-stu-id="be211-156">To maintain clarity, I have not included much in the way of checking or exception handling in most of the code examples shown.</span></span> <span data-ttu-id="be211-157">Ale jak budeme pokračovat I získáte několik příkladů, funkce pomocí funkce pro zpracování výjimek na R.</span><span class="sxs-lookup"><span data-stu-id="be211-157">However, as we proceed I will give you several examples of functions by using R's exception handling capability.</span></span>  

<span data-ttu-id="be211-158">Pokud potřebujete podrobnější zacházení s zpracovávání výjimek v jazyce R, I doporučujeme číst v příslušných částech seznamu pomocí Wickham uvedené v [příloha B – další čtení](#appendixb).</span><span class="sxs-lookup"><span data-stu-id="be211-158">If you need a more complete treatment of R exception handling, I recommend you read the applicable sections of the book by Wickham listed in [Appendix B - Further Reading](#appendixb).</span></span>

#### <a name="debug-and-test-r-in-machine-learning-studio"></a><span data-ttu-id="be211-159">Ladění a testování R v Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="be211-159">Debug and test R in Machine Learning Studio</span></span>
<span data-ttu-id="be211-160">Chcete-li znovu opakují, I doporučujeme testování a ladění kódu R v malém měřítku v Rstudia.</span><span class="sxs-lookup"><span data-stu-id="be211-160">To reiterate, I recommend you test and debug your R code on a small scale in RStudio.</span></span> <span data-ttu-id="be211-161">Ale existují případy, kdy je potřeba sledovat potíže kód R v [spustit skript jazyka R] [ execute-r-script] sám sebe.</span><span class="sxs-lookup"><span data-stu-id="be211-161">However, there are cases where you will need to track down R code problems in the [Execute R Script][execute-r-script] itself.</span></span> <span data-ttu-id="be211-162">Kromě toho je vhodné zkontrolovat výsledky v nástroji Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="be211-162">In addition, it is good practice to check your results in Machine Learning Studio.</span></span>

<span data-ttu-id="be211-163">Výstup z provádění kódu R a na platformě Azure Machine Learning především v souboru výstup.log nebyl nalezen.</span><span class="sxs-lookup"><span data-stu-id="be211-163">Output from the execution of your R code and on the Azure Machine Learning platform is found primarily in output.log.</span></span> <span data-ttu-id="be211-164">Některé další informace se zobrazí v error.log.</span><span class="sxs-lookup"><span data-stu-id="be211-164">Some additional information will be seen in error.log.</span></span>  

<span data-ttu-id="be211-165">Pokud při běhu kódu R dojde k chybě v nástroji Machine Learning Studio, vaše první postupu by měl být podívat se na error.log.</span><span class="sxs-lookup"><span data-stu-id="be211-165">If an error occurs in Machine Learning Studio while running your R code, your first course of action should be to look at error.log.</span></span> <span data-ttu-id="be211-166">Tento soubor může obsahovat užitečné chybové zprávy, které vám pomůžou pochopit a vaše chybu opravit.</span><span class="sxs-lookup"><span data-stu-id="be211-166">This file can contain useful error messages to help you understand and correct your error.</span></span> <span data-ttu-id="be211-167">Chcete-li zobrazit error.log, klikněte na **zobrazení v protokolu chyb** na **podokno properties** pro [spustit skript jazyka R] [ execute-r-script] obsahující chyby.</span><span class="sxs-lookup"><span data-stu-id="be211-167">To view error.log, click on **View error log** on the **properties pane** for the [Execute R Script][execute-r-script] containing the error.</span></span>

<span data-ttu-id="be211-168">Například byl spuštěn následující kód R nedefinované proměnné y, v [spustit skript jazyka R] [ execute-r-script] modul:</span><span class="sxs-lookup"><span data-stu-id="be211-168">For example, I ran the following R code, with an undefined variable y, in an [Execute R Script][execute-r-script] module:</span></span>

    x <- 1.0
    z <- x + y

<span data-ttu-id="be211-169">Tento kód se nepodaří spustit, což vede k chybě.</span><span class="sxs-lookup"><span data-stu-id="be211-169">This code fails to execute, resulting in an error condition.</span></span> <span data-ttu-id="be211-170">Kliknutím na **zobrazení v protokolu chyb** na **podokno properties** vytvoří zobrazení znázorňuje obrázek 2.</span><span class="sxs-lookup"><span data-stu-id="be211-170">Clicking on **View error log** on the **properties pane** produces the display shown in Figure 2.</span></span>

  ![Chybová zpráva otevíraném][2]

<span data-ttu-id="be211-172">*Obrázek 2. Chybová zpráva automaticky otevírané okno.*</span><span class="sxs-lookup"><span data-stu-id="be211-172">*Figure 2. Error message pop-up.*</span></span>

<span data-ttu-id="be211-173">Zdá se, musíme naleznete v souboru výstup.log zobrazíte R chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="be211-173">It looks like we need to look in output.log to see the R error message.</span></span> <span data-ttu-id="be211-174">Klikněte na [spustit skript jazyka R] [ execute-r-script] a potom klikněte na **zobrazení souboru výstup.log** položku **podokno properties** vpravo.</span><span class="sxs-lookup"><span data-stu-id="be211-174">Click on the [Execute R Script][execute-r-script] and then click on the **View output.log** item on the **properties pane** to the right.</span></span> <span data-ttu-id="be211-175">Otevře nové okno prohlížeče a I v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="be211-175">A new browser window opens, and I see the following.</span></span>

    [Critical]     Error: Error 0063: The following error occurred during evaluation of R script:
    ---------- Start of error message from R ----------
    object 'y' not found


    object 'y' not found
    ----------- End of error message from R -----------

<span data-ttu-id="be211-176">Tato chybová zpráva obsahuje žádný výskyt nečekaných událostí a jednoznačně identifikuje problém.</span><span class="sxs-lookup"><span data-stu-id="be211-176">This error message contains no surprises and clearly identifies the problem.</span></span>

<span data-ttu-id="be211-177">Chcete-li zjistit hodnotu všech objektů v R, můžete vytisknout tyto hodnoty do souboru výstup.log souboru.</span><span class="sxs-lookup"><span data-stu-id="be211-177">To inspect the value of any object in R, you can print these values to the output.log file.</span></span> <span data-ttu-id="be211-178">Pravidla pro zkoumání hodnoty objektu jsou v podstatě stejný jako interaktivní relace R zařízením.</span><span class="sxs-lookup"><span data-stu-id="be211-178">The rules for examining object values are essentially the same as in an interactive R session.</span></span> <span data-ttu-id="be211-179">Například pokud zadáte název proměnné na řádek, hodnota objektu budou vytištěny do souboru výstup.log souboru.</span><span class="sxs-lookup"><span data-stu-id="be211-179">For example, if you type a variable name on a line, the value of the object will be printed to the output.log file.</span></span>  

#### <a name="packages-in-machine-learning-studio"></a><span data-ttu-id="be211-180">Balíčky v nástroji Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="be211-180">Packages in Machine Learning Studio</span></span>
<span data-ttu-id="be211-181">Azure Machine Learning se dodává s více než 350 předinstalované balíčky jazyk R.</span><span class="sxs-lookup"><span data-stu-id="be211-181">Azure Machine Learning comes with over 350 preinstalled R language packages.</span></span> <span data-ttu-id="be211-182">Můžete použít následující kód [spustit skript jazyka R] [ execute-r-script] modul načíst seznam v předinstalovaných balíčcích.</span><span class="sxs-lookup"><span data-stu-id="be211-182">You can use the following code in the [Execute R Script][execute-r-script] module to retrieve a list of the preinstalled packages.</span></span>

    data.set <- data.frame(installed.packages())
    maml.mapOutputPort("data.set")

<span data-ttu-id="be211-183">Pokud nerozumíte poslední řádek tohoto kódu v okamžiku, přečtěte si.</span><span class="sxs-lookup"><span data-stu-id="be211-183">If you don't understand the last line of this code at the moment, read on.</span></span> <span data-ttu-id="be211-184">Ve zbývající části tohoto dokumentu hojně pojednává o využití R v prostředí Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="be211-184">In the rest of this document we will extensively discuss using R in the Azure Machine Learning environment.</span></span>

### <a name="introduction-to-rstudio"></a><span data-ttu-id="be211-185">Úvod do Rstudia</span><span class="sxs-lookup"><span data-stu-id="be211-185">Introduction to RStudio</span></span>
<span data-ttu-id="be211-186">Rstudia je často používaný IDE pro R. Pro úpravy, testování a ladění některé kód R použité v této úvodní příručce použiji Rstudia.</span><span class="sxs-lookup"><span data-stu-id="be211-186">RStudio is a widely used IDE for R. I will use RStudio for editing, testing and debugging some of the R code used in this quick start guide.</span></span> <span data-ttu-id="be211-187">Jakmile kódu jazyka R otestované a připravené, jednoduše kopírování a vkládání z editoru Rstudia do nástroje Machine Learning Studio [spustit skript jazyka R] [ execute-r-script] modulu.</span><span class="sxs-lookup"><span data-stu-id="be211-187">Once R code is tested and ready, you simply cut and paste from the RStudio editor into a Machine Learning Studio [Execute R Script][execute-r-script] module.</span></span>  

<span data-ttu-id="be211-188">Pokud nemáte programovací jazyk R nainstalovaný na počítači klientů, doporučujeme, že udělejte to teď.</span><span class="sxs-lookup"><span data-stu-id="be211-188">If you do not have the R programming language installed on your desktop machine, I recommend you do so now.</span></span> <span data-ttu-id="be211-189">Ke stažení zdarma jazyka R s otevřeným zdrojem jsou k dispozici na komplexní R archivu sítě (CRAN) v [http://www.r-project.org/](http://www.r-project.org/).</span><span class="sxs-lookup"><span data-stu-id="be211-189">Free downloads of open source R language are available at the Comprehensive R Archive Network (CRAN) at [http://www.r-project.org/](http://www.r-project.org/).</span></span> <span data-ttu-id="be211-190">Nejsou k dispozici pro Windows, Mac OS a Linux a UNIX stáhne.</span><span class="sxs-lookup"><span data-stu-id="be211-190">There are downloads available for Windows, Mac OS, and Linux/UNIX.</span></span> <span data-ttu-id="be211-191">Zvolte blízkým zrcadlení a postupujte podle pokynů ke stažení.</span><span class="sxs-lookup"><span data-stu-id="be211-191">Choose a nearby mirror and follow the download directions.</span></span> <span data-ttu-id="be211-192">Kromě toho CRAN obsahuje širokou řadu užitečné balíčky manipulaci s analytickými funkcemi a data.</span><span class="sxs-lookup"><span data-stu-id="be211-192">In addition, CRAN contains a wealth of useful analytics and data manipulation packages.</span></span>

<span data-ttu-id="be211-193">Pokud jste ještě Rstudia, by měl stáhněte a nainstalujte verzi pro stolní počítače.</span><span class="sxs-lookup"><span data-stu-id="be211-193">If you are new to RStudio, you should download and install the desktop version.</span></span> <span data-ttu-id="be211-194">Můžete najít, že že rstudia soubory ke stažení pro Windows, Mac OS a Linux a UNIX v http://www.rstudio.com/products/RStudio/.</span><span class="sxs-lookup"><span data-stu-id="be211-194">You can find the RStudio downloads for Windows, Mac OS, and Linux/UNIX at http://www.rstudio.com/products/RStudio/.</span></span> <span data-ttu-id="be211-195">Postupujte podle pokynů k instalaci Rstudia na stolního počítače.</span><span class="sxs-lookup"><span data-stu-id="be211-195">Follow the directions provided to install RStudio on your desktop machine.</span></span>  

<span data-ttu-id="be211-196">Kurz Úvod do Rstudia je k dispozici na https://support.rstudio.com/hc/sections/200107586-Using-RStudio.</span><span class="sxs-lookup"><span data-stu-id="be211-196">A tutorial introduction to RStudio is available at https://support.rstudio.com/hc/sections/200107586-Using-RStudio.</span></span>

<span data-ttu-id="be211-197">Poskytují I některé další informace o používání Rstudia v [příloha A][appendixa].</span><span class="sxs-lookup"><span data-stu-id="be211-197">I provide some additional information on using RStudio in [Appendix A][appendixa].</span></span>  

## <span data-ttu-id="be211-198"><a id="scriptmodule"></a>Získání dat a deaktivovat modul spustit skript jazyka R</span><span class="sxs-lookup"><span data-stu-id="be211-198"><a id="scriptmodule"></a>Get data in and out of the Execute R Script module</span></span>
<span data-ttu-id="be211-199">V této části probereme, jak načíst data do a z [spustit skript jazyka R] [ execute-r-script] modulu.</span><span class="sxs-lookup"><span data-stu-id="be211-199">In this section we will discuss how you get data into and out of the [Execute R Script][execute-r-script] module.</span></span> <span data-ttu-id="be211-200">Bude jsme si, jak zpracovávat různé datové typy číst do a z [spustit skript jazyka R] [ execute-r-script] modulu.</span><span class="sxs-lookup"><span data-stu-id="be211-200">We will review how to handle various data types read into and out of the [Execute R Script][execute-r-script] module.</span></span>

<span data-ttu-id="be211-201">Kód dokončení pro tento oddíl je v souboru zip, které jste dříve stáhli.</span><span class="sxs-lookup"><span data-stu-id="be211-201">The complete code for this section is in the zip file you downloaded earlier.</span></span>

### <a name="load-and-check-data-in-machine-learning-studio"></a><span data-ttu-id="be211-202">Načtení a zkontrolujte, zda data v nástroji Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="be211-202">Load and check data in Machine Learning Studio</span></span>
#### <span data-ttu-id="be211-203"><a id="loading"></a>Načíst datové sady</span><span class="sxs-lookup"><span data-stu-id="be211-203"><a id="loading"></a>Load the dataset</span></span>
<span data-ttu-id="be211-204">Spustíme načtením **csdairydata.csv** souboru do Azure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="be211-204">We will start by loading the **csdairydata.csv** file into Azure Machine Learning Studio.</span></span>

* <span data-ttu-id="be211-205">Spuštění prostředí Azure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="be211-205">Start your Azure Machine Learning Studio environment.</span></span>
* <span data-ttu-id="be211-206">Klikněte na **+ nový** v levém dolním rohu obrazovky a vyberte **datovou sadu**.</span><span class="sxs-lookup"><span data-stu-id="be211-206">Click on **+ NEW** at the lower left of your screen and select **Dataset**.</span></span>
* <span data-ttu-id="be211-207">Vyberte **z místního souboru**a potom **Procházet** a vyberte soubor.</span><span class="sxs-lookup"><span data-stu-id="be211-207">Select **From Local File**, and then **Browse** to select the file.</span></span>
* <span data-ttu-id="be211-208">Ujistěte se, zda jste vybrali **souboru obecné CSV s hlavičkou (.csv)** jako typ pro datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="be211-208">Make sure you have selected **Generic CSV file with header (.csv)** as the type for the dataset.</span></span>
* <span data-ttu-id="be211-209">Kliknutím na značku zaškrtnutí.</span><span class="sxs-lookup"><span data-stu-id="be211-209">Click the check mark.</span></span>
* <span data-ttu-id="be211-210">Až se nahrají datovou sadu, měli byste vidět nová datová sada kliknutím na **datové sady** kartě.</span><span class="sxs-lookup"><span data-stu-id="be211-210">After the dataset has been uploaded, you should see the new dataset by clicking on the **Datasets** tab.</span></span>  

#### <a name="create-an-experiment"></a><span data-ttu-id="be211-211">Vytvoření experimentu</span><span class="sxs-lookup"><span data-stu-id="be211-211">Create an experiment</span></span>
<span data-ttu-id="be211-212">Teď, když máme některá data v nástroji Machine Learning Studio, je potřeba vytvořit nový experiment, chcete-li provést analýzu.</span><span class="sxs-lookup"><span data-stu-id="be211-212">Now that we have some data in Machine Learning Studio, we need to create an experiment to do the analysis.</span></span>  

* <span data-ttu-id="be211-213">Klikněte na **+ nový** na nižší vlevo a vyberte **experimentu**, pak **prázdný Experiment**.</span><span class="sxs-lookup"><span data-stu-id="be211-213">Click on **+ NEW** at the lower left and select **Experiment**, then **Blank Experiment**.</span></span>
* <span data-ttu-id="be211-214">Výběrem a úpravy, můžete pojmenovat experimentu **experimentu vytvořena na...**  nadpis v horní části stránky.</span><span class="sxs-lookup"><span data-stu-id="be211-214">You can name your experiment by selecting, and modifying, the **Experiment created on ...** title at the top of the page.</span></span> <span data-ttu-id="be211-215">Například změna jeho **certifikační Autority mlékárny Analysis**.</span><span class="sxs-lookup"><span data-stu-id="be211-215">For example, changing it to **CA Dairy Analysis**.</span></span>
* <span data-ttu-id="be211-216">Na levé straně stránky experiment, rozbalte položku **uložit datové sady**a potom **Moje datové sady**.</span><span class="sxs-lookup"><span data-stu-id="be211-216">On the left of the experiment page, expand **Saved Datasets**, and then **My Datasets**.</span></span> <span data-ttu-id="be211-217">Měli byste vidět **cadairydata.csv** který jste dříve nahráli.</span><span class="sxs-lookup"><span data-stu-id="be211-217">You should see the **cadairydata.csv** that you uploaded earlier.</span></span>
* <span data-ttu-id="be211-218">Přetáhnout myší **datovou sadu csdairydata.csv** do experimentu.</span><span class="sxs-lookup"><span data-stu-id="be211-218">Drag and drop the **csdairydata.csv dataset** onto the experiment.</span></span>
* <span data-ttu-id="be211-219">V **vyhledávání experimentovat položky** pole nahoře v levém podokně, typ [spustit skript jazyka R][execute-r-script].</span><span class="sxs-lookup"><span data-stu-id="be211-219">In the **Search experiment items** box on the top of the left pane, type [Execute R Script][execute-r-script].</span></span> <span data-ttu-id="be211-220">Zobrazí se v modulu se zobrazí v seznamu hledání.</span><span class="sxs-lookup"><span data-stu-id="be211-220">You will see the module appear in the search list.</span></span>
* <span data-ttu-id="be211-221">Přetáhnout myší [spustit skript jazyka R] [ execute-r-script] modulu do vaší palety.</span><span class="sxs-lookup"><span data-stu-id="be211-221">Drag and drop the [Execute R Script][execute-r-script] module onto your pallet.</span></span>  
* <span data-ttu-id="be211-222">Připojit výstup **csdairydata.csv datovou sadu** krajní levé vstupu (**Dataset1**) z [spustit skript jazyka R][execute-r-script].</span><span class="sxs-lookup"><span data-stu-id="be211-222">Connect the output of the **csdairydata.csv dataset** to the leftmost input (**Dataset1**) of the [Execute R Script][execute-r-script].</span></span>
* <span data-ttu-id="be211-223">**Nezapomeňte klikněte na 'Uložit'!**</span><span class="sxs-lookup"><span data-stu-id="be211-223">**Don't forget to click on 'Save'!**</span></span>  

<span data-ttu-id="be211-224">V tomto okamžiku by měl experimentu vypadat podobně jako obrázek 3.</span><span class="sxs-lookup"><span data-stu-id="be211-224">At this point your experiment should look something like Figure 3.</span></span>

![Analýza mlékárny certifikační Autority experimentovat s datovou sadu a modul pro spuštění skriptu jazyka R][3]

<span data-ttu-id="be211-226">*Obrázek 3. Analýza mlékárny certifikační Autority experimentovat s datovou sadu a spustit skript jazyka R modulu.*</span><span class="sxs-lookup"><span data-stu-id="be211-226">*Figure 3. The CA Dairy Analysis experiment with dataset and Execute R Script module.*</span></span>

#### <a name="check-on-the-data"></a><span data-ttu-id="be211-227">Zkontrolujte na data</span><span class="sxs-lookup"><span data-stu-id="be211-227">Check on the data</span></span>
<span data-ttu-id="be211-228">Pojďme Podíváme se na data, která mají byl načten do našich experimentu.</span><span class="sxs-lookup"><span data-stu-id="be211-228">Let's have a look at the data we have loaded into our experiment.</span></span> <span data-ttu-id="be211-229">V rámci experimentu klikněte na výstup **datovou sadu cadairydata.csv** a vyberte **vizualizovat**.</span><span class="sxs-lookup"><span data-stu-id="be211-229">In the experiment, click on the output of the **cadairydata.csv dataset** and select **visualize**.</span></span> <span data-ttu-id="be211-230">Měli byste vidět něco podobného jako na obrázku 4.</span><span class="sxs-lookup"><span data-stu-id="be211-230">You should see something like Figure 4.</span></span>  

![Souhrn cadairydata.csv datové sady][4]

<span data-ttu-id="be211-232">*Obrázek 4. Souhrn cadairydata.csv datové sady.*</span><span class="sxs-lookup"><span data-stu-id="be211-232">*Figure 4. Summary of the cadairydata.csv dataset.*</span></span>

<span data-ttu-id="be211-233">V tomto zobrazení vidíte mnoho užitečných informací.</span><span class="sxs-lookup"><span data-stu-id="be211-233">In this view we see a lot of useful information.</span></span> <span data-ttu-id="be211-234">Vidíme první několik řádků z této datové sady.</span><span class="sxs-lookup"><span data-stu-id="be211-234">We can see the first several rows of that dataset.</span></span> <span data-ttu-id="be211-235">Pokud jsme vyberte sloupec, v části Statistika zobrazíte další informace o sloupci.</span><span class="sxs-lookup"><span data-stu-id="be211-235">If we select a column, the Statistics section shows more information about the column.</span></span> <span data-ttu-id="be211-236">Typ funkce řádek příkladu nám jaké typy dat Azure Machine Learning Studio přiřazené ke sloupci.</span><span class="sxs-lookup"><span data-stu-id="be211-236">For example, the Feature Type row shows us what data types Azure Machine Learning Studio assigned to the column.</span></span> <span data-ttu-id="be211-237">Rychle zkontrolovat tímto způsobem je kontrolu dobrý správností než začneme jakékoli závažné práci.</span><span class="sxs-lookup"><span data-stu-id="be211-237">Having a quick look like this is a good sanity check before we start to do any serious work.</span></span>

### <a name="first-r-script"></a><span data-ttu-id="be211-238">První skript jazyka R</span><span class="sxs-lookup"><span data-stu-id="be211-238">First R script</span></span>
<span data-ttu-id="be211-239">Umožňuje vytvořit jednoduché první skript jazyka R a experimentovat s v Azure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="be211-239">Let's create a simple first R script to experiment with in Azure Machine Learning Studio.</span></span> <span data-ttu-id="be211-240">I vytvoření a otestování následující skript v Rstudia.</span><span class="sxs-lookup"><span data-stu-id="be211-240">I have created and tested the following script in RStudio.</span></span>  

    ## Only one of the following two lines should be used
    ## If running in Machine Learning Studio, use the first line with maml.mapInputPort()
    ## If in RStudio, use the second line with read.csv()
    cadairydata <- maml.mapInputPort(1)
    # cadairydata  <- read.csv("cadairydata.csv", header = TRUE, stringsAsFactors = FALSE)
    str(cadairydata)
    pairs(~ Cotagecheese.Prod + Icecream.Prod + Milk.Prod + N.CA.Fat.Price, data = cadairydata)
    ## The following line should be executed only when running in
    ## Azure Machine Learning Studio
    maml.mapOutputPort('cadairydata')

<span data-ttu-id="be211-241">Nyní je třeba přenést do Azure Machine Learning Studio tento skript.</span><span class="sxs-lookup"><span data-stu-id="be211-241">Now I need to transfer this script to Azure Machine Learning Studio.</span></span> <span data-ttu-id="be211-242">Může stačí vyjmout a vložit.</span><span class="sxs-lookup"><span data-stu-id="be211-242">I could simply cut and paste.</span></span> <span data-ttu-id="be211-243">Ale v takovém případě bude přenést Moje skript jazyka R prostřednictvím soubor zip.</span><span class="sxs-lookup"><span data-stu-id="be211-243">However, in this case, I will transfer my R script via a zip file.</span></span>

### <a name="data-input-to-the-execute-r-script-module"></a><span data-ttu-id="be211-244">Vložení dat do modulu spustit skript jazyka R</span><span class="sxs-lookup"><span data-stu-id="be211-244">Data input to the Execute R Script module</span></span>
<span data-ttu-id="be211-245">Pojďme Podíváme se na vstupy pro [spustit skript jazyka R] [ execute-r-script] modulu.</span><span class="sxs-lookup"><span data-stu-id="be211-245">Let's have a look at the inputs to the [Execute R Script][execute-r-script] module.</span></span> <span data-ttu-id="be211-246">V tomto příkladu jsme bude číst data dojnic kalifornské do [spustit skript jazyka R] [ execute-r-script] modulu.</span><span class="sxs-lookup"><span data-stu-id="be211-246">In this example we will read the California dairy data into the [Execute R Script][execute-r-script] module.</span></span>  

<span data-ttu-id="be211-247">Existují tři možné vstupy pro [spustit skript jazyka R] [ execute-r-script] modulu.</span><span class="sxs-lookup"><span data-stu-id="be211-247">There are three possible inputs for the [Execute R Script][execute-r-script] module.</span></span> <span data-ttu-id="be211-248">Může používat kterékoli nebo všechny tyto vstupy, v závislosti na vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="be211-248">You may use any one or all of these inputs, depending on your application.</span></span> <span data-ttu-id="be211-249">Je také perfektně vhodné použít R skript, který nemá žádný vstup vůbec.</span><span class="sxs-lookup"><span data-stu-id="be211-249">It is also perfectly reasonable to use an R script that takes no input at all.</span></span>  

<span data-ttu-id="be211-250">Podívejme se na každý z těchto vstupy, budete zleva doprava.</span><span class="sxs-lookup"><span data-stu-id="be211-250">Let's look at each of these inputs, going from left to right.</span></span> <span data-ttu-id="be211-251">Se zobrazí názvy jednotlivých ze vstupních údajů umístěním kurzoru přes vstupu a čtení popisek.</span><span class="sxs-lookup"><span data-stu-id="be211-251">You can see the names of each of the inputs by placing your cursor over the input and reading the tooltip.</span></span>  

#### <a name="script-bundle"></a><span data-ttu-id="be211-252">Skript sady</span><span class="sxs-lookup"><span data-stu-id="be211-252">Script Bundle</span></span>
<span data-ttu-id="be211-253">Vstupní sadu skriptu můžete předat obsahu souboru zip do [spustit skript jazyka R] [ execute-r-script] modulu.</span><span class="sxs-lookup"><span data-stu-id="be211-253">The Script Bundle input allows you to pass the contents of a zip file into [Execute R Script][execute-r-script] module.</span></span> <span data-ttu-id="be211-254">Jeden z následujících příkazů můžete načíst obsah souboru zip do vašeho kódu jazyka R.</span><span class="sxs-lookup"><span data-stu-id="be211-254">You can use one of the following commands to read the contents of the zip file into your R code.</span></span>

    source("src/yourfile.R") # Reads a zipped R script
    load("src/yourData.rdata") # Reads a zipped R data file

> [!NOTE]
> <span data-ttu-id="be211-255">Azure Machine Learning zpracovává soubory v souboru zip, jako kdyby se src / adresáře, takže potřebujete předpony názvy souborů s tímto názvem adresáře.</span><span class="sxs-lookup"><span data-stu-id="be211-255">Azure Machine Learning treats files in the zip as if they are in the src/ directory, so you need to prefix your file names with this directory name.</span></span> <span data-ttu-id="be211-256">Například, pokud souboru zip obsahuje soubory `yourfile.R` a `yourData.rdata` v kořenovém souboru zip, které by vyřešit jako `src/yourfile.R` a `src/yourData.rdata` při použití `source` a `load`.</span><span class="sxs-lookup"><span data-stu-id="be211-256">For example, if the zip contains the files `yourfile.R` and `yourData.rdata` in the root of the zip, you would address these as `src/yourfile.R` and `src/yourData.rdata` when using `source` and `load`.</span></span>
> 
> 

<span data-ttu-id="be211-257">Už jsme probírali načítání datové sady v [načítání datovou sadu](#loading).</span><span class="sxs-lookup"><span data-stu-id="be211-257">We already discussed loading datasets in [Loading the dataset](#loading).</span></span> <span data-ttu-id="be211-258">Po vytvoření a testování skriptu jazyka R uvedené v předchozí části, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="be211-258">Once you have created and tested the R script shown in the previous section, do the following:</span></span>

1. <span data-ttu-id="be211-259">Uložit do skriptu jazyka R. R soubor.</span><span class="sxs-lookup"><span data-stu-id="be211-259">Save the R script into a .R file.</span></span> <span data-ttu-id="be211-260">Můžu volat Moje soubor skriptu "simpleplot. R".</span><span class="sxs-lookup"><span data-stu-id="be211-260">I call my script file "simpleplot.R".</span></span> <span data-ttu-id="be211-261">Zde je obsah.</span><span class="sxs-lookup"><span data-stu-id="be211-261">Here's the contents.</span></span>
   
        ## Only one of the following two lines should be used
        ## If running in Machine Learning Studio, use the first line with maml.mapInputPort()
        ## If in RStudio, use the second line with read.csv()
        cadairydata <- maml.mapInputPort(1)
        # cadairydata  <- read.csv("cadairydata.csv", header = TRUE, stringsAsFactors = FALSE)
        str(cadairydata)
        pairs(~ Cotagecheese.Prod + Icecream.Prod + Milk.Prod + N.CA.Fat.Price, data = cadairydata)
        ## The following line should be executed only when running in
        ## Azure Machine Learning Studio
        maml.mapOutputPort('cadairydata')
2. <span data-ttu-id="be211-262">Vytvořte soubor zip a zkopírujte skript do tohoto souboru zip.</span><span class="sxs-lookup"><span data-stu-id="be211-262">Create a zip file and copy your script into this zip file.</span></span> <span data-ttu-id="be211-263">V systému Windows, klikněte pravým tlačítkem na soubor a vyberte **poslat**a potom **komprimované složky**.</span><span class="sxs-lookup"><span data-stu-id="be211-263">On Windows, you can right-click on the file and select **Send to**, and then **Compressed folder**.</span></span> <span data-ttu-id="be211-264">Tím se vytvoří nový soubor zip obsahující "simpleplot. Soubor R".</span><span class="sxs-lookup"><span data-stu-id="be211-264">This will create a new zip file containing the "simpleplot.R" file.</span></span>
3. <span data-ttu-id="be211-265">Přidání souboru do **datové sady** v nástroji Machine Learning Studio, určení typu jako **zip**.</span><span class="sxs-lookup"><span data-stu-id="be211-265">Add your file to the **datasets** in Machine Learning Studio, specifying the type as **zip**.</span></span> <span data-ttu-id="be211-266">Měli byste vidět teď tento soubor zip v vaše datové sady.</span><span class="sxs-lookup"><span data-stu-id="be211-266">You should now see the zip file in your datasets.</span></span>
4. <span data-ttu-id="be211-267">Přetáhnout myší ze souboru zip **datové sady** na **ML Studio plátno**.</span><span class="sxs-lookup"><span data-stu-id="be211-267">Drag and drop the zip file from **datasets** onto the **ML Studio canvas**.</span></span>
5. <span data-ttu-id="be211-268">Připojit výstup **zip data** ikonu **skript sady** vstupní z [spustit skript jazyka R] [ execute-r-script] modulu.</span><span class="sxs-lookup"><span data-stu-id="be211-268">Connect the output of the **zip data** icon to the **Script Bundle** input of the [Execute R Script][execute-r-script] module.</span></span>
6. <span data-ttu-id="be211-269">Typ `source()` nahraďte názvem souboru zip do okna kódu pro funkce [spustit skript jazyka R] [ execute-r-script] modulu.</span><span class="sxs-lookup"><span data-stu-id="be211-269">Type the `source()` function with your zip file name into the code window for the [Execute R Script][execute-r-script] module.</span></span> <span data-ttu-id="be211-270">V případě Moje zadali `source("src/simpleplot.R")`.</span><span class="sxs-lookup"><span data-stu-id="be211-270">In my case I typed `source("src/simpleplot.R")`.</span></span>  
7. <span data-ttu-id="be211-271">Ujistěte se, kliknete na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="be211-271">Make sure you click **Save**.</span></span>

<span data-ttu-id="be211-272">Po dokončení těchto kroků [spustit skript jazyka R] [ execute-r-script] modul provede R skript v souboru zip při spuštění experimentu.</span><span class="sxs-lookup"><span data-stu-id="be211-272">Once these steps are complete, the [Execute R Script][execute-r-script] module will execute the R script in the zip file when the experiment is run.</span></span> <span data-ttu-id="be211-273">V tomto okamžiku by měl experimentu vypadat podobně jako obrázek 5.</span><span class="sxs-lookup"><span data-stu-id="be211-273">At this point your experiment should look something like Figure 5.</span></span>

![Experiment pomocí komprimované skript jazyka R][6]

<span data-ttu-id="be211-275">*Obrázek 5. Experiment pomocí komprimované R skript.*</span><span class="sxs-lookup"><span data-stu-id="be211-275">*Figure 5. Experiment using zipped R script.*</span></span>

#### <a name="dataset1"></a><span data-ttu-id="be211-276">Dataset1</span><span class="sxs-lookup"><span data-stu-id="be211-276">Dataset1</span></span>
<span data-ttu-id="be211-277">Můžete předat obdélníková tabulku dat do kódu R pomocí Dataset1 vstup.</span><span class="sxs-lookup"><span data-stu-id="be211-277">You can pass a rectangular table of data to your R code by using the Dataset1 input.</span></span> <span data-ttu-id="be211-278">V našem jednoduchého skriptu `maml.mapInputPort(1)` funkce čte data z portu 1.</span><span class="sxs-lookup"><span data-stu-id="be211-278">In our simple script the `maml.mapInputPort(1)` function reads the data from port 1.</span></span> <span data-ttu-id="be211-279">Tato data pak přiřazen název proměnné dataframe ve vašem kódu.</span><span class="sxs-lookup"><span data-stu-id="be211-279">This data is then assigned to a dataframe variable name in your code.</span></span> <span data-ttu-id="be211-280">V našem jednoduchého skriptu provede první řádek kódu přiřazení.</span><span class="sxs-lookup"><span data-stu-id="be211-280">In our simple script the first line of code performs the assignment.</span></span>

    cadairydata <- maml.mapInputPort(1)

<span data-ttu-id="be211-281">Kliknutím na spustit experimentu **spustit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="be211-281">Execute your experiment by clicking on the **Run** button.</span></span> <span data-ttu-id="be211-282">Pokud je spuštění hotové, klikněte na [spustit skript jazyka R] [ execute-r-script] modul a potom klikněte na **zobrazit výstup protokol** v podokně Vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="be211-282">When the execution finishes, click on the [Execute R Script][execute-r-script] module and then click **View output log** on the properties pane.</span></span> <span data-ttu-id="be211-283">Nová stránka by se zobrazit v prohlížeči zobrazuje obsah souboru výstup.log souboru.</span><span class="sxs-lookup"><span data-stu-id="be211-283">A new page should appear in your browser showing the contents of the output.log file.</span></span> <span data-ttu-id="be211-284">Po přejděte dolů se měl zobrazit přibližně takto.</span><span class="sxs-lookup"><span data-stu-id="be211-284">When you scroll down you should see something like the following.</span></span>

    [ModuleOutput] InputDataStructure
    [ModuleOutput]
    [ModuleOutput] {
    [ModuleOutput]  "InputName":Dataset1
    [ModuleOutput]  "Rows":228
    [ModuleOutput]  "Cols":9
    [ModuleOutput]  "ColumnTypes":System.Int32,3,System.Double,5,System.String,1
    [ModuleOutput] }

<span data-ttu-id="be211-285">Dále dolní části stránky je že podrobnější informace o sloupce, které bude vypadat podobně jako následující.</span><span class="sxs-lookup"><span data-stu-id="be211-285">Farther down the page is more detailed information on the columns, which will look something like the following.</span></span>

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

<span data-ttu-id="be211-286">Tyto výsledky se většinou podle očekávání, s 228 připomínky a 9 sloupců v dataframe.</span><span class="sxs-lookup"><span data-stu-id="be211-286">These results are mostly as expected, with 228 observations and 9 columns in the dataframe.</span></span> <span data-ttu-id="be211-287">Jsme můžete zobrazit názvy sloupců, datový typ R a ukázku jednotlivých sloupců.</span><span class="sxs-lookup"><span data-stu-id="be211-287">We can see the column names, the R data type and a sample of each column.</span></span>

> [!NOTE]
> <span data-ttu-id="be211-288">Tento stejný tištěné výstup je pohodlně dostupná z výstupu R zařízení [spustit skript jazyka R] [ execute-r-script] modulu.</span><span class="sxs-lookup"><span data-stu-id="be211-288">This same printed output is conveniently available from the R Device output of the [Execute R Script][execute-r-script] module.</span></span> <span data-ttu-id="be211-289">Podrobně probereme výstupů [spustit skript jazyka R] [ execute-r-script] modulu v další části.</span><span class="sxs-lookup"><span data-stu-id="be211-289">We will discuss the outputs of the [Execute R Script][execute-r-script] module in the next section.</span></span>  
> 
> 

#### <a name="dataset2"></a><span data-ttu-id="be211-290">Dataset2</span><span class="sxs-lookup"><span data-stu-id="be211-290">Dataset2</span></span>
<span data-ttu-id="be211-291">Chování Dataset2 vstupu je shodná s Dataset1.</span><span class="sxs-lookup"><span data-stu-id="be211-291">The behavior of the Dataset2 input is identical to that of Dataset1.</span></span> <span data-ttu-id="be211-292">Pomocí tento vstup lze předat v druhé tabulce obdélníková dat do vašeho kódu jazyka R.</span><span class="sxs-lookup"><span data-stu-id="be211-292">Using this input you can pass a second rectangular table of data into your R code.</span></span> <span data-ttu-id="be211-293">Funkce `maml.mapInputPort(2)`, s argumentem 2, slouží k předávání tato data.</span><span class="sxs-lookup"><span data-stu-id="be211-293">The function `maml.mapInputPort(2)`, with the argument 2, is used to pass this data.</span></span>  

### <a name="execute-r-script-outputs"></a><span data-ttu-id="be211-294">Spustit skript jazyka R výstupy</span><span class="sxs-lookup"><span data-stu-id="be211-294">Execute R Script outputs</span></span>
#### <a name="output-a-dataframe"></a><span data-ttu-id="be211-295">Výstup a dataframe</span><span class="sxs-lookup"><span data-stu-id="be211-295">Output a dataframe</span></span>
<span data-ttu-id="be211-296">Výstup můžete obsah dataframe R jako obdélníková tabulku přes port výsledek Dataset1 pomocí `maml.mapOutputPort()` funkce.</span><span class="sxs-lookup"><span data-stu-id="be211-296">You can output the contents of an R dataframe as a rectangular table through the Result Dataset1 port by using the `maml.mapOutputPort()` function.</span></span> <span data-ttu-id="be211-297">V našem jednoduchý skript jazyka R se provádí pomocí následujícího řádku.</span><span class="sxs-lookup"><span data-stu-id="be211-297">In our simple R script this is performed by the following line.</span></span>

    maml.mapOutputPort('cadairydata')

<span data-ttu-id="be211-298">Po spuštění experimentu, klikněte na výstupní port Dataset1 výsledek a pak klikněte na **vizualizovat**.</span><span class="sxs-lookup"><span data-stu-id="be211-298">After running the experiment, click on the Result Dataset1 output port and then click on **Visualize**.</span></span> <span data-ttu-id="be211-299">Měli byste vidět něco podobného jako obrázek 6.</span><span class="sxs-lookup"><span data-stu-id="be211-299">You should see something like Figure 6.</span></span>

![Vizualizaci výstupních dat dojnic kalifornské][7]

<span data-ttu-id="be211-301">*Obrázek 6. Vizualizaci výstupních dat dojnic kalifornské.*</span><span class="sxs-lookup"><span data-stu-id="be211-301">*Figure 6. The visualization of the output of the California dairy data.*</span></span>

<span data-ttu-id="be211-302">Tento výstup vypadá stejný jako vstup, přesně tak, jak očekávali jsme.</span><span class="sxs-lookup"><span data-stu-id="be211-302">This output looks identical to the input, exactly as we expected.</span></span>  

### <a name="r-device-output"></a><span data-ttu-id="be211-303">Výstup R zařízení</span><span class="sxs-lookup"><span data-stu-id="be211-303">R Device output</span></span>
<span data-ttu-id="be211-304">Výstup zařízení [spustit skript jazyka R] [ execute-r-script] modul obsahuje zprávy a grafický výstup.</span><span class="sxs-lookup"><span data-stu-id="be211-304">The Device output of the [Execute R Script][execute-r-script] module contains messages and graphics output.</span></span> <span data-ttu-id="be211-305">Standardní výstupní zařízení a standardní cíl chybové zprávy i z R jsou odesílány na výstupní port R zařízení.</span><span class="sxs-lookup"><span data-stu-id="be211-305">Both standard output and standard error messages from R are sent to the R Device output port.</span></span>  

<span data-ttu-id="be211-306">Chcete-li zobrazit výstup R zařízení, klikněte na port a pak na **vizualizovat**.</span><span class="sxs-lookup"><span data-stu-id="be211-306">To view the R Device output, click on the port and then on **Visualize**.</span></span> <span data-ttu-id="be211-307">Vidíte standardní výstupní zařízení a standardní chyba ze skriptu R na obrázku 7.</span><span class="sxs-lookup"><span data-stu-id="be211-307">We see the standard output and standard error from the R script in Figure 7.</span></span>

![Standardní výstupní zařízení a standardní chyba z portu R zařízení][8]

<span data-ttu-id="be211-309">*Na obrázku 7. Standardní výstupní zařízení a standardní chyba z portu R zařízení.*</span><span class="sxs-lookup"><span data-stu-id="be211-309">*Figure 7. Standard output and standard error from the R Device port.*</span></span>

<span data-ttu-id="be211-310">Posouvání dolů nemůžeme zobrazit výstup grafiky z našich skript jazyka R na obrázku 8.</span><span class="sxs-lookup"><span data-stu-id="be211-310">Scrolling down we see the graphics output from our R script in Figure 8.</span></span>  

![Grafického výstupu z portu R zařízení][9]

<span data-ttu-id="be211-312">*Obrázek 8. Grafika výstup z portu R zařízení.*</span><span class="sxs-lookup"><span data-stu-id="be211-312">*Figure 8. Graphics output from the R Device port.*</span></span>  

## <span data-ttu-id="be211-313"><a id="filtering"></a>Filtrování dat a transformace</span><span class="sxs-lookup"><span data-stu-id="be211-313"><a id="filtering"></a>Data filtering and transformation</span></span>
<span data-ttu-id="be211-314">V této části provedeme některé operace transformace na datech dojnic kalifornské a základní data filtrování.</span><span class="sxs-lookup"><span data-stu-id="be211-314">In this section we will perform some basic data filtering and transformation operations on the California dairy data.</span></span> <span data-ttu-id="be211-315">Na konci této části máme data ve formátu, který je vhodný pro sestavování analytického modelu.</span><span class="sxs-lookup"><span data-stu-id="be211-315">By the end of this section we will have data in a format suitable for building an analytic model.</span></span>  

<span data-ttu-id="be211-316">Přesněji řečeno, v této části provedeme několik běžných čištění a transformace úlohy dat: zadejte transformaci, filtrování dataframes, přidání nové počítaných sloupcích a hodnota transformace.</span><span class="sxs-lookup"><span data-stu-id="be211-316">More specifically, in this section we will perform several common data cleaning and transformation tasks: type transformation, filtering on dataframes, adding new computed columns, and value transformations.</span></span> <span data-ttu-id="be211-317">Tato pozadí pomáhají řešit mnoho variant v reálných problémy.</span><span class="sxs-lookup"><span data-stu-id="be211-317">This background should help you deal with the many variations encountered in real-world problems.</span></span>

<span data-ttu-id="be211-318">Kód dokončení R pro tento oddíl je k dispozici v souboru zip, které jste dříve stáhli.</span><span class="sxs-lookup"><span data-stu-id="be211-318">The complete R code for this section is available in the zip file you downloaded earlier.</span></span>

### <a name="type-transformations"></a><span data-ttu-id="be211-319">Typ transformace</span><span class="sxs-lookup"><span data-stu-id="be211-319">Type transformations</span></span>
<span data-ttu-id="be211-320">Teď, když jsme můžete číst data dojnic kalifornské do kód R [spustit skript jazyka R] [ execute-r-script] modulu, musíme zkontrolujte, zda má určený typ a formát data ve sloupcích.</span><span class="sxs-lookup"><span data-stu-id="be211-320">Now that we can read the California dairy data into the R code in the [Execute R Script][execute-r-script] module, we need to ensure that the data in the columns has the intended type and format.</span></span>  

<span data-ttu-id="be211-321">R je dynamicky zadávaných jazyk, což znamená, že datové typy jsou má mezi sebou podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="be211-321">R is a dynamically typed language, which means that data types are coerced from one to another as required.</span></span> <span data-ttu-id="be211-322">Atomic datových typů v R zahrnují číselné literály, logické a znak.</span><span class="sxs-lookup"><span data-stu-id="be211-322">The atomic data types in R include numeric, logical and character.</span></span> <span data-ttu-id="be211-323">Faktor typ se používá k uložení kompaktně kategorizovaná data.</span><span class="sxs-lookup"><span data-stu-id="be211-323">The factor type is used to compactly store categorical data.</span></span> <span data-ttu-id="be211-324">Mnohem Další informace o typech dat najdete v odkazech na v [příloha B – další čtení](#appendixb).</span><span class="sxs-lookup"><span data-stu-id="be211-324">You can find much more information on data types in the references in [Appendix B - Further reading](#appendixb).</span></span>

<span data-ttu-id="be211-325">Když tabulková data je pro čtení do R z externího zdroje, vždycky je vhodné zkontrolovat výsledné typy ve sloupcích.</span><span class="sxs-lookup"><span data-stu-id="be211-325">When tabular data is read into R from an external source, it is always a good idea to check the resulting types in the columns.</span></span> <span data-ttu-id="be211-326">Může být vhodné sloupec – znak typu, ale v mnoha případech to se zobrazí jako faktor nebo naopak.</span><span class="sxs-lookup"><span data-stu-id="be211-326">You may want a column of type character, but in many cases this will show up as factor or vice versa.</span></span> <span data-ttu-id="be211-327">V ostatních případech a sloupec, který si myslíte, že by měl být číselný je reprezentována textová data, například '1,23' než 1,23 jako plovoucí bodu číslo.</span><span class="sxs-lookup"><span data-stu-id="be211-327">In other cases a column you think should be numeric is represented by character data, e.g. '1.23' rather than 1.23 as a floating point number.</span></span>  

<span data-ttu-id="be211-328">Naštěstí je snadné jeden typ převést do jiného, tak dlouho, dokud je možné mapování.</span><span class="sxs-lookup"><span data-stu-id="be211-328">Fortunately, it is easy to convert one type to another, as long as mapping is possible.</span></span> <span data-ttu-id="be211-329">Například 'Nevada' nelze převést číselnou hodnotu, ale můžete ji převést faktor (kategorií proměnné).</span><span class="sxs-lookup"><span data-stu-id="be211-329">For example, you cannot convert 'Nevada' into a numeric value, but you can convert it to a factor (categorical variable).</span></span> <span data-ttu-id="be211-330">Například můžete převést číselnou 1 znak '1' nebo koeficient.</span><span class="sxs-lookup"><span data-stu-id="be211-330">As another example, you can convert a numeric 1 into a character '1' or a factor.</span></span>  

<span data-ttu-id="be211-331">Syntaxe pro některý z těchto převody je jednoduchý: `as.datatype()`.</span><span class="sxs-lookup"><span data-stu-id="be211-331">The syntax for any of these conversions is simple: `as.datatype()`.</span></span> <span data-ttu-id="be211-332">Tyto funkce pro převod typů patří.</span><span class="sxs-lookup"><span data-stu-id="be211-332">These type conversion functions include the following.</span></span>

* `as.numeric()`
* `as.character()`
* `as.logical()`
* `as.factor()`

<span data-ttu-id="be211-333">Prohlížení datové typy sloupců nemůžeme vstup v předchozí části: jsou všechny sloupce typu číselné, s výjimkou sloupec s názvem 'Měsíc', který je – znak typu.</span><span class="sxs-lookup"><span data-stu-id="be211-333">Looking at the data types of the columns we input in the previous section: all columns are of type numeric, except for the column labeled 'Month', which is of type character.</span></span> <span data-ttu-id="be211-334">Umožňuje převést to faktor a testovat výsledky.</span><span class="sxs-lookup"><span data-stu-id="be211-334">Let's convert this to a factor and test the results.</span></span>  

<span data-ttu-id="be211-335">Odstraněný řádek, který vytvoří matice scatterplot a přidat řádek převádění sloupci, měsíc, faktor.</span><span class="sxs-lookup"><span data-stu-id="be211-335">I have deleted the line that created the scatterplot matrix and added a line converting the 'Month' column to a factor.</span></span> <span data-ttu-id="be211-336">V experimentu se právě kopírování a vkládání kódu jazyka R do okna kódu [spustit skript jazyka R] [ execute-r-script] modulu.</span><span class="sxs-lookup"><span data-stu-id="be211-336">In my experiment I will just cut and paste the R code into the code window of the [Execute R Script][execute-r-script] Module.</span></span> <span data-ttu-id="be211-337">Můžete také aktualizovat soubor zip a nahrajte ho do Azure Machine Learning Studio, ale tato akce trvá několik kroků.</span><span class="sxs-lookup"><span data-stu-id="be211-337">You could also update the zip file and upload it to Azure Machine Learning Studio, but this takes several steps.</span></span>  

    ## Only one of the following two lines should be used
    ## If running in Machine Learning Studio, use the first line with maml.mapInputPort()
    ## If in RStudio, use the second line with read.csv()
    cadairydata <- maml.mapInputPort(1)
    # cadairydata  <- read.csv("cadairydata.csv", header = TRUE, stringsAsFactors = FALSE)
    ## Ensure the coding is consistent and convert column to a factor
    cadairydata$Month <- as.factor(cadairydata$Month)
    str(cadairydata) # Check the result
    ## The following line should be executed only when running in
    ## Azure Machine Learning Studio
    maml.mapOutputPort('cadairydata')

<span data-ttu-id="be211-338">Umožňuje spustit tento kód a podívejte se na výstup protokolu pro R skript.</span><span class="sxs-lookup"><span data-stu-id="be211-338">Let's execute this code and look at the output log for the R script.</span></span> <span data-ttu-id="be211-339">Příslušných dat z protokolu je vidět na obrázku 9.</span><span class="sxs-lookup"><span data-stu-id="be211-339">The relevant data from the log is shown in Figure 9.</span></span>

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
    [ModuleOutput] [1] "Saving the following item(s):  .maml.oport1"

<span data-ttu-id="be211-340">*Obrázek 9. Shrnutí dataframe s Multi-Factor proměnné.*</span><span class="sxs-lookup"><span data-stu-id="be211-340">*Figure 9. Summary of the dataframe with a factor variable.*</span></span>

<span data-ttu-id="be211-341">Nyní by mělo být uvedeno na typ pro měsíc '**Multi-Factor s 14 úrovně**'.</span><span class="sxs-lookup"><span data-stu-id="be211-341">The type for Month should now say '**Factor w/ 14 levels**'.</span></span> <span data-ttu-id="be211-342">Tento problém je vzhledem k tomu, že existují pouze po dobu 12 měsíců v roce.</span><span class="sxs-lookup"><span data-stu-id="be211-342">This is a problem since there are only 12 months in the year.</span></span> <span data-ttu-id="be211-343">Můžete také zkontrolovat, zjistíte, že je typu v **vizualizovat** datové sady výsledků port je '**Categorical**'.</span><span class="sxs-lookup"><span data-stu-id="be211-343">You can also check to see that the type in **Visualize** of the Result Dataset port is '**Categorical**'.</span></span>

<span data-ttu-id="be211-344">Problém je, že sloupci, měsíc, nebyl systematičtěji programového.</span><span class="sxs-lookup"><span data-stu-id="be211-344">The problem is that the 'Month' column has not been coded systematically.</span></span> <span data-ttu-id="be211-345">V některých případech se nazývá měsíc duben a k ostatním se zkracuje dubna. Ořízne řetězec, který má 3 znaky jsme můžete tento problém vyřešit.</span><span class="sxs-lookup"><span data-stu-id="be211-345">In some cases a month is called April and in others it is abbreviated as Apr. We can solve this problem by trimming the string to 3 characters.</span></span> <span data-ttu-id="be211-346">Řádek kódu teď vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="be211-346">The line of code now looks like the following:</span></span>

    ## Ensure the coding is consistent and convert column to a factor
    cadairydata$Month <- as.factor(substr(cadairydata$Month, 1, 3))

<span data-ttu-id="be211-347">Spusťte experiment a zobrazte protokol výstup.</span><span class="sxs-lookup"><span data-stu-id="be211-347">Rerun the experiment and view the output log.</span></span> <span data-ttu-id="be211-348">Očekávané výsledky jsou zobrazené na obrázku 10.</span><span class="sxs-lookup"><span data-stu-id="be211-348">The expected results are shown in Figure 10.</span></span>  

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
    [ModuleOutput] [1] "Saving the following item(s):  .maml.oport1"

<span data-ttu-id="be211-349">*Obrázek 10. Shrnutí dataframe s správný počet úrovní faktor.*</span><span class="sxs-lookup"><span data-stu-id="be211-349">*Figure 10. Summary of the dataframe with correct number of factor levels.*</span></span>

<span data-ttu-id="be211-350">Naše Multi-Factor proměnná teď má požadovanou 12 úrovně.</span><span class="sxs-lookup"><span data-stu-id="be211-350">Our factor variable now has the desired 12 levels.</span></span>

### <a name="basic-data-frame-filtering"></a><span data-ttu-id="be211-351">Filtrování rámce základní data</span><span class="sxs-lookup"><span data-stu-id="be211-351">Basic data frame filtering</span></span>
<span data-ttu-id="be211-352">R dataframes podporovat výkonné možnosti filtrování.</span><span class="sxs-lookup"><span data-stu-id="be211-352">R dataframes support powerful filtering capabilities.</span></span> <span data-ttu-id="be211-353">Datové sady, může být podsady pomocí logických filtry na řádky nebo sloupce.</span><span class="sxs-lookup"><span data-stu-id="be211-353">Datasets can be subsetted by using logical filters on either rows or columns.</span></span> <span data-ttu-id="be211-354">V mnoha případech se bude vyžadovat složitá kritéria filtru.</span><span class="sxs-lookup"><span data-stu-id="be211-354">In many cases, complex filter criteria will be required.</span></span> <span data-ttu-id="be211-355">Odkazy v [příloha B – další čtení](#appendixb) obsahovat rozsáhlé příklady filtrování dataframes.</span><span class="sxs-lookup"><span data-stu-id="be211-355">The references in [Appendix B - Further reading](#appendixb) contain extensive examples of filtering dataframes.</span></span>  

<span data-ttu-id="be211-356">Je jeden bit filtrování by měl provedeme v naší datové sadě.</span><span class="sxs-lookup"><span data-stu-id="be211-356">There is one bit of filtering we should do on our dataset.</span></span> <span data-ttu-id="be211-357">Pokud se podíváte na sloupce v cadairydata dataframe, zobrazí se dva nepotřebných sloupců.</span><span class="sxs-lookup"><span data-stu-id="be211-357">If you look at the columns in the cadairydata dataframe, you will see two unnecessary columns.</span></span> <span data-ttu-id="be211-358">První sloupec obsahuje jenom číslo řádku, které není velmi užitečné.</span><span class="sxs-lookup"><span data-stu-id="be211-358">The first column just holds a row number, which is not very useful.</span></span> <span data-ttu-id="be211-359">Druhý sloupec Year.Month, obsahuje redundantní informace.</span><span class="sxs-lookup"><span data-stu-id="be211-359">The second column, Year.Month, contains redundant information.</span></span> <span data-ttu-id="be211-360">Pomocí následujícího kódu R jsme můžete snadno vyloučit tyto sloupce.</span><span class="sxs-lookup"><span data-stu-id="be211-360">We can easily exclude these columns by using the following R code.</span></span>

> [!NOTE]
> <span data-ttu-id="be211-361">Od této chvíle v této části I právě zjistíte další kód přidám v [spustit skript jazyka R] [ execute-r-script] modulu.</span><span class="sxs-lookup"><span data-stu-id="be211-361">From now on in this section, I will just show you the additional code I am adding in the [Execute R Script][execute-r-script] module.</span></span> <span data-ttu-id="be211-362">Přidat každý nový řádek **před** `str()` funkce.</span><span class="sxs-lookup"><span data-stu-id="be211-362">I will add each new line **before** the `str()` function.</span></span> <span data-ttu-id="be211-363">Mohu pomocí této funkce můžete ověřit Moje výsledky v Azure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="be211-363">I use this function to verify my results in Azure Machine Learning Studio.</span></span>
> 
> 

<span data-ttu-id="be211-364">I na můj kód R v přidejte následující řádek [spustit skript jazyka R] [ execute-r-script] modulu.</span><span class="sxs-lookup"><span data-stu-id="be211-364">I add the following line to my R code in the [Execute R Script][execute-r-script] module.</span></span>

    # Remove two columns we do not need
    cadairydata <- cadairydata[, c(-1, -2)]

<span data-ttu-id="be211-365">Spusťte tento kód v experimentu a zkontrolujte výsledek z výstupu protokolu.</span><span class="sxs-lookup"><span data-stu-id="be211-365">Run this code in your experiment and check the result from the output log.</span></span> <span data-ttu-id="be211-366">Tyto výsledky se zobrazí v obrázek 11.</span><span class="sxs-lookup"><span data-stu-id="be211-366">These results are shown in Figure 11.</span></span>

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
    [ModuleOutput] [1] "Saving the following item(s):  .maml.oport1"

<span data-ttu-id="be211-367">*Obrázek 11. Shrnutí dataframe s dva sloupce a bude odebrána.*</span><span class="sxs-lookup"><span data-stu-id="be211-367">*Figure 11. Summary of the dataframe with two columns removed.*</span></span>

<span data-ttu-id="be211-368">Dobrá zpráva!</span><span class="sxs-lookup"><span data-stu-id="be211-368">Good news!</span></span> <span data-ttu-id="be211-369">Nemůžeme získat očekávané výsledky.</span><span class="sxs-lookup"><span data-stu-id="be211-369">We get the expected results.</span></span>

### <a name="add-a-new-column"></a><span data-ttu-id="be211-370">Umožňuje přidat nový sloupec</span><span class="sxs-lookup"><span data-stu-id="be211-370">Add a new column</span></span>
<span data-ttu-id="be211-371">Chcete-li vytvořit modely časové řady bude vhodné mít sloupce obsahujícího měsíců od začátku časové řady.</span><span class="sxs-lookup"><span data-stu-id="be211-371">To create time series models it will be convenient to have a column containing the months since the start of the time series.</span></span> <span data-ttu-id="be211-372">Vytvoříme nový sloupec, Month.Count'.</span><span class="sxs-lookup"><span data-stu-id="be211-372">We will create a new column 'Month.Count'.</span></span>

<span data-ttu-id="be211-373">Kvůli lepší organizaci kód vytvoříme naše první jednoduché funkce `num.month()`.</span><span class="sxs-lookup"><span data-stu-id="be211-373">To help organize the code we will create our first simple function, `num.month()`.</span></span> <span data-ttu-id="be211-374">Pak budou použity této funkci můžete vytvořit nový sloupec v dataframe.</span><span class="sxs-lookup"><span data-stu-id="be211-374">We will then apply this function to create a new column in the dataframe.</span></span> <span data-ttu-id="be211-375">Nový kód je následující.</span><span class="sxs-lookup"><span data-stu-id="be211-375">The new code is as follows.</span></span>

    ## Create a new column with the month count
    ## Function to find the number of months from the first
    ## month of the time series
    num.month <- function(Year, Month) {
      ## Find the starting year
      min.year  <- min(Year)

      ## Compute the number of months from the start of the time series
      12 * (Year - min.year) + Month - 1
    }

    ## Compute the new column for the dataframe
    cadairydata$Month.Count <- num.month(cadairydata$Year, cadairydata$Month.Number)

<span data-ttu-id="be211-376">Teď spusťte aktualizované experiment a používat protokol výstup zobrazíte výsledky.</span><span class="sxs-lookup"><span data-stu-id="be211-376">Now run the updated experiment and use the output log to view the results.</span></span> <span data-ttu-id="be211-377">Tyto výsledky se zobrazí obrázek 12.</span><span class="sxs-lookup"><span data-stu-id="be211-377">These results are shown in Figure 12.</span></span>

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
    [ModuleOutput] [1] "Saving the following item(s):  .maml.oport1"

<span data-ttu-id="be211-378">*Obrázek 12. Shrnutí dataframe s další sloupec.*</span><span class="sxs-lookup"><span data-stu-id="be211-378">*Figure 12. Summary of the dataframe with the additional column.*</span></span>

<span data-ttu-id="be211-379">Vypadá to vše funguje.</span><span class="sxs-lookup"><span data-stu-id="be211-379">It looks like everything is working.</span></span> <span data-ttu-id="be211-380">V našem dataframe máme nový sloupec s očekávaných hodnot.</span><span class="sxs-lookup"><span data-stu-id="be211-380">We have the new column with the expected values in our dataframe.</span></span>

### <a name="value-transformations"></a><span data-ttu-id="be211-381">Hodnota transformace</span><span class="sxs-lookup"><span data-stu-id="be211-381">Value transformations</span></span>
<span data-ttu-id="be211-382">V této části provedeme některé jednoduché transformace na hodnoty v některé z našich dataframe sloupce.</span><span class="sxs-lookup"><span data-stu-id="be211-382">In this section we will perform some simple transformations on the values in some of the columns of our dataframe.</span></span> <span data-ttu-id="be211-383">Jazyk R podporuje transformace téměř libovolná hodnota.</span><span class="sxs-lookup"><span data-stu-id="be211-383">The R language supports nearly arbitrary value transformations.</span></span> <span data-ttu-id="be211-384">Odkazy v [příloha B – další čtení](#appendixb) obsahovat rozsáhlé příklady.</span><span class="sxs-lookup"><span data-stu-id="be211-384">The references in [Appendix B - Further Reading](#appendixb) contain extensive examples.</span></span>

<span data-ttu-id="be211-385">Pokud se podíváte na hodnoty v souhrnných informací o našem dataframe měli byste vidět něco liché sem.</span><span class="sxs-lookup"><span data-stu-id="be211-385">If you look at the values in the summaries of our dataframe you should see something odd here.</span></span> <span data-ttu-id="be211-386">Další zmrzlinová než mléka vytváří v kalifornské?</span><span class="sxs-lookup"><span data-stu-id="be211-386">Is more ice cream than milk produced in California?</span></span> <span data-ttu-id="be211-387">Ne, samozřejmě není, jak to nemá smysl sad jako tento fakt může být pro některé nám zmrzlinová lovers.</span><span class="sxs-lookup"><span data-stu-id="be211-387">No, of course not, as this makes no sense, sad as this fact may be to some of us ice cream lovers.</span></span> <span data-ttu-id="be211-388">Jednotky se liší.</span><span class="sxs-lookup"><span data-stu-id="be211-388">The units are different.</span></span> <span data-ttu-id="be211-389">Ceny se nám jednotky libra mléka je v jednotkách 1 milion libra USA, zmrzlinová je v jednotkách 1 000 nám galony a byt sýr je v jednotkách 1 000 libra USA.</span><span class="sxs-lookup"><span data-stu-id="be211-389">The price is in units of US pounds, milk is in units of 1 M US pounds, ice cream is in units of 1,000 US gallons, and cottage cheese is in units of 1,000 US pounds.</span></span> <span data-ttu-id="be211-390">Za předpokladu, že zmrzlinová váží asi 6,5 libra za spotřeby, můžete snadno provedeme násobení převést tyto hodnoty tak, aby byly všechny ve stejné jednotky libra 1 000.</span><span class="sxs-lookup"><span data-stu-id="be211-390">Assuming ice cream weighs about 6.5 pounds per gallon, we can easily do the multiplication to convert these values so they are all in equal units of 1,000 pounds.</span></span>

<span data-ttu-id="be211-391">Pro náš model prognózy používáme multiplikativní model pro trendu a sezónní úpravu tato data.</span><span class="sxs-lookup"><span data-stu-id="be211-391">For our forecasting model we use a multiplicative model for trend and seasonal adjustment of this data.</span></span> <span data-ttu-id="be211-392">Transformace protokolu budeme tak moct používat model lineární ke zjednodušení tohoto procesu.</span><span class="sxs-lookup"><span data-stu-id="be211-392">A log transformation allows us to use a linear model, simplifying this process.</span></span> <span data-ttu-id="be211-393">Transformace protokolu ve stejné funkci, kdy se používá násobitel jsme můžete použít.</span><span class="sxs-lookup"><span data-stu-id="be211-393">We can apply the log transformation in the same function where the multiplier is applied.</span></span>

<span data-ttu-id="be211-394">V následujícím kódu I definovat novou funkci, `log.transform()`a použijte ho pro řádky obsahující číselné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="be211-394">In the following code, I define a new function, `log.transform()`, and apply it to the rows containing the numerical values.</span></span> <span data-ttu-id="be211-395">R `Map()` funkce se používá k aplikování `log.transform()` funkce na vybrané sloupce dataframe.</span><span class="sxs-lookup"><span data-stu-id="be211-395">The R `Map()` function is used to apply the `log.transform()` function to the selected columns of the dataframe.</span></span> <span data-ttu-id="be211-396">`Map()`je podobná `apply()` , ale umožňuje více než jeden seznam argumentů funkce.</span><span class="sxs-lookup"><span data-stu-id="be211-396">`Map()` is similar to `apply()` but allows for more than one list of arguments to the function.</span></span> <span data-ttu-id="be211-397">Všimněte si, že poskytuje seznam multiplikátory druhý argument `log.transform()` funkce.</span><span class="sxs-lookup"><span data-stu-id="be211-397">Note that a list of multipliers supplies the second argument to the `log.transform()` function.</span></span> <span data-ttu-id="be211-398">`na.omit()` Funkce slouží jako kousek čištění k zajištění nemáme chybějící nebo nedefinované hodnoty dataframe.</span><span class="sxs-lookup"><span data-stu-id="be211-398">The `na.omit()` function is used as a bit of cleanup to ensure we do not have missing or undefined values in the dataframe.</span></span>

    log.transform <- function(invec, multiplier = 1) {
      ## Function for the transformation, which is the log
      ## of the input value times a multiplier

      warningmessages <- c("ERROR: Non-numeric argument encountered in function log.transform",
                           "ERROR: Arguments to function log.transform must be greate than zero",
                           "ERROR: Aggurment multiplier to funcition log.transform must be a scaler",
                           "ERROR: Invalid time seies value encountered in function log.transform"
                           )

      ## Check the input arguments
      if(!is.numeric(invec) | !is.numeric(multiplier)) {warning(warningmessages[1]); return(NA)}  
      if(any(invec < 0.0) | any(multiplier < 0.0)) {warning(warningmessages[2]); return(NA)}
      if(length(multiplier) != 1) {{warning(warningmessages[3]); return(NA)}}

      ## Wrap the multiplication in tryCatch
      ## If there is an exception, print the warningmessage to
      ## standard error and return NA
      tryCatch(log(multiplier * invec),
               error = function(e){warning(warningmessages[4]); NA})
    }


    ## Apply the transformation function to the 4 columns
    ## of the dataframe with production data
    multipliers  <- list(1.0, 6.5, 1000.0, 1000.0)
    cadairydata[, 4:7] <- Map(log.transform, cadairydata[, 4:7], multipliers)

    ## Get rid of any rows with NA values
    cadairydata <- na.omit(cadairydata)  

<span data-ttu-id="be211-399">Je s bit situaci v `log.transform()` funkce.</span><span class="sxs-lookup"><span data-stu-id="be211-399">There is quite a bit happening in the `log.transform()` function.</span></span> <span data-ttu-id="be211-400">Většina tento kód program kontroluje potenciální problémy s argumenty nebo práci s výjimky, které mohou stále nastat během výpočtů.</span><span class="sxs-lookup"><span data-stu-id="be211-400">Most of this code is checking for potential problems with the arguments or dealing with exceptions, which can still arise during the computations.</span></span> <span data-ttu-id="be211-401">Tento kód jenom pár řádků ve skutečnosti to výpočtů.</span><span class="sxs-lookup"><span data-stu-id="be211-401">Only a few lines of this code actually do the computations.</span></span>

<span data-ttu-id="be211-402">Aby se zabránilo selhání jedné funkce, která zabraňuje zpracování pokračovat je cílem Obranným programování.</span><span class="sxs-lookup"><span data-stu-id="be211-402">The goal of the defensive programming is to prevent the failure of a single function that prevents processing from continuing.</span></span> <span data-ttu-id="be211-403">K náhlému selhání dlouho běžící analysis může být poměrně frustrující pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="be211-403">An abrupt failure of a long-running analysis can be quite frustrating for users.</span></span> <span data-ttu-id="be211-404">Abyste předešli této situaci, musí být vybrána výchozí návratové hodnoty, která omezí škody podřízené zpracování.</span><span class="sxs-lookup"><span data-stu-id="be211-404">To avoid this situation, default return values must be chosen that will limit damage to downstream processing.</span></span> <span data-ttu-id="be211-405">Zprávy také vytváří které uživatele upozorní, že něco nepovede.</span><span class="sxs-lookup"><span data-stu-id="be211-405">A message is also produced to alert users that something has gone wrong.</span></span>

<span data-ttu-id="be211-406">Pokud není jste zvyklí Obranným programování v R, tento kód se může zdát trochu čtenáře.</span><span class="sxs-lookup"><span data-stu-id="be211-406">If you are not used to defensive programming in R, all this code may seem a bit overwhelming.</span></span> <span data-ttu-id="be211-407">I vás provede důležitými kroky:</span><span class="sxs-lookup"><span data-stu-id="be211-407">I will walk you through the major steps:</span></span>

1. <span data-ttu-id="be211-408">Vektor čtyři zprávy je definována.</span><span class="sxs-lookup"><span data-stu-id="be211-408">A vector of four messages is defined.</span></span> <span data-ttu-id="be211-409">Tyto zprávy se používají ke sdělování informací o některých možné chyby a výjimky, které se můžou vyskytnout s tímto kódem.</span><span class="sxs-lookup"><span data-stu-id="be211-409">These messages are used to communicate information about some of the possible errors and exceptions that can occur with this code.</span></span>
2. <span data-ttu-id="be211-410">Návratu NA hodnotu v obou případech.</span><span class="sxs-lookup"><span data-stu-id="be211-410">I return a value of NA for each case.</span></span> <span data-ttu-id="be211-411">Existuje mnoho dalších možností, které by mohly mít méně vedlejší účinky.</span><span class="sxs-lookup"><span data-stu-id="be211-411">There are many other possibilities that might have fewer side effects.</span></span> <span data-ttu-id="be211-412">Vektor nul nebo původní vstupní vektoru, může například návratu.</span><span class="sxs-lookup"><span data-stu-id="be211-412">I could return a vector of zeroes, or the original input vector, for example.</span></span>
3. <span data-ttu-id="be211-413">Kontroluje se spouštějí na argumenty funkce.</span><span class="sxs-lookup"><span data-stu-id="be211-413">Checks are run on the arguments to the function.</span></span> <span data-ttu-id="be211-414">V každém případě pokud je detekována chyba, je vrácen výchozí hodnotu a je produkovaný zprávu `warning()` funkce.</span><span class="sxs-lookup"><span data-stu-id="be211-414">In each case, if an error is detected, a default value is returned and a message is produced by the `warning()` function.</span></span> <span data-ttu-id="be211-415">Používám `warning()` místo `stop()` jako druhá možnost se ukončí provádění, přesně co chcete vyhnout.</span><span class="sxs-lookup"><span data-stu-id="be211-415">I am using `warning()` rather than `stop()` as the latter will terminate execution, exactly what I am trying to avoid.</span></span> <span data-ttu-id="be211-416">Všimněte si, že I mají zapisovat tento kód v procedurální styl, jako v tomto případě funkční přístup mailech vypadalo komplexní a skrytého.</span><span class="sxs-lookup"><span data-stu-id="be211-416">Note that I have written this code in a procedural style, as in this case a functional approach seemed complex and obscure.</span></span>
4. <span data-ttu-id="be211-417">Výpočty protokolu jsou uzavřen do `tryCatch()` tak, aby výjimky nezpůsobí náhlému zastavení na zpracování.</span><span class="sxs-lookup"><span data-stu-id="be211-417">The log computations are wrapped in `tryCatch()` so that exceptions will not cause an abrupt halt to processing.</span></span> <span data-ttu-id="be211-418">Bez `tryCatch()` aktivováno R funkce povede signál k zastavení, která zajišťuje právě, většina chyb.</span><span class="sxs-lookup"><span data-stu-id="be211-418">Without `tryCatch()` most errors raised by R functions result in a stop signal, which does just that.</span></span>

<span data-ttu-id="be211-419">Spustit tento kód R v experimentu a podíváme se na tištěných výstup v souboru výstup.log souboru.</span><span class="sxs-lookup"><span data-stu-id="be211-419">Execute this R code in your experiment and have a look at the printed output in the output.log file.</span></span> <span data-ttu-id="be211-420">Nyní uvidíte transformovaných hodnoty čtyři sloupce v protokolu, jak ukazuje obrázek 13.</span><span class="sxs-lookup"><span data-stu-id="be211-420">You will now see the transformed values of the four columns in the log, as shown in Figure 13.</span></span>

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
    [ModuleOutput] [1] "Saving the following item(s):  .maml.oport1"

<span data-ttu-id="be211-421">*Obrázek 13. Souhrn transformovaných hodnot v dataframe.*</span><span class="sxs-lookup"><span data-stu-id="be211-421">*Figure 13. Summary of the transformed values in the dataframe.*</span></span>

<span data-ttu-id="be211-422">Vidíte, že byly transformovány hodnoty.</span><span class="sxs-lookup"><span data-stu-id="be211-422">We see the values have been transformed.</span></span> <span data-ttu-id="be211-423">Nyní produkce mléka výrazně přesahuje všechny ostatní mléčný výrobek produkční, vrací, že jsme teď vyhledávání škálované protokolu.</span><span class="sxs-lookup"><span data-stu-id="be211-423">Milk production now greatly exceeds all other dairy product production, recalling that we are now looking at a log scale.</span></span>

<span data-ttu-id="be211-424">V tomto okamžiku je naše data vyčištěna a jsme připraveni pro některé modelování.</span><span class="sxs-lookup"><span data-stu-id="be211-424">At this point our data is cleaned up and we are ready for some modeling.</span></span> <span data-ttu-id="be211-425">Prohlížení vizualizace shrnutí výstupní datovou sadu výsledků z našich [spustit skript jazyka R] [ execute-r-script] modulu, zobrazí se vám sloupci, měsíc, je 'Categorical' s 12 jedinečné hodnoty, znovu, stejně jako jsme chtěli.</span><span class="sxs-lookup"><span data-stu-id="be211-425">Looking at the visualization summary for the Result Dataset output of our [Execute R Script][execute-r-script] module, you will see the 'Month' column is 'Categorical' with 12 unique values, again, just as we wanted.</span></span>

## <span data-ttu-id="be211-426"><a id="timeseries"></a>Časové řady objekty a analýzy korelace</span><span class="sxs-lookup"><span data-stu-id="be211-426"><a id="timeseries"></a>Time series objects and correlation analysis</span></span>
<span data-ttu-id="be211-427">V této části jsme se několika základních objektů řady čas R zkoumat a analyzovat korelací mezi určité proměnné.</span><span class="sxs-lookup"><span data-stu-id="be211-427">In this section we will explore a few basic R time series objects and analyze the correlations between some of the variables.</span></span> <span data-ttu-id="be211-428">Naším cílem je výstup dataframe obsahujícího informace o pairwise korelace na několik pomalou.</span><span class="sxs-lookup"><span data-stu-id="be211-428">Our goal is to output a dataframe containing the pairwise correlation information at several lags.</span></span>

<span data-ttu-id="be211-429">Kód dokončení R pro tento oddíl je v souboru zip, které jste dříve stáhli.</span><span class="sxs-lookup"><span data-stu-id="be211-429">The complete R code for this section is in the zip file you downloaded earlier.</span></span>

### <a name="time-series-objects-in-r"></a><span data-ttu-id="be211-430">Časové řady objekty v R</span><span class="sxs-lookup"><span data-stu-id="be211-430">Time series objects in R</span></span>
<span data-ttu-id="be211-431">Jak už jsme už zmínili, časové řady jsou řady hodnot dat indexované podle času.</span><span class="sxs-lookup"><span data-stu-id="be211-431">As already mentioned, time series are a series of data values indexed by time.</span></span> <span data-ttu-id="be211-432">R časové řady objekty se používají k vytváření a správě index čas.</span><span class="sxs-lookup"><span data-stu-id="be211-432">R time series objects are used to create and manage the time index.</span></span> <span data-ttu-id="be211-433">Existuje několik výhod používání objektů časové řady.</span><span class="sxs-lookup"><span data-stu-id="be211-433">There are several advantages to using time series objects.</span></span> <span data-ttu-id="be211-434">Časové řady objekty vás zbaví mnoho podrobnosti o správě časových řad index hodnoty, které jsou zapouzdřené v objektu.</span><span class="sxs-lookup"><span data-stu-id="be211-434">Time series objects free you from the many details of managing the time series index values that are encapsulated in the object.</span></span> <span data-ttu-id="be211-435">Kromě toho časové řady objekty umožňují používat mnoho způsobů časové řady pro vykreslení, tisk, modelování a podobně.</span><span class="sxs-lookup"><span data-stu-id="be211-435">In addition, time series objects allow you to use the many time series methods for plotting, printing, modeling, etc.</span></span>

<span data-ttu-id="be211-436">Třída POSIXct čas řady se často používá a je poměrně jednoduché.</span><span class="sxs-lookup"><span data-stu-id="be211-436">The POSIXct time series class is commonly used and is relatively simple.</span></span> <span data-ttu-id="be211-437">Toto časové řady třídy míry čas od začátku epoch, 1. ledna 1970.</span><span class="sxs-lookup"><span data-stu-id="be211-437">This time series class measures time from the start of the epoch, January 1, 1970.</span></span> <span data-ttu-id="be211-438">V tomto příkladu použijeme POSIXct časové řady objekty.</span><span class="sxs-lookup"><span data-stu-id="be211-438">We will use POSIXct time series objects in this example.</span></span> <span data-ttu-id="be211-439">Další často používaný R časové řady tříd objektů zahrnují zoo a xts, extensible časové řady.</span><span class="sxs-lookup"><span data-stu-id="be211-439">Other widely used R time series object classes include zoo and xts, extensible time series.</span></span>
<!-- Additional information on R time series objects is provided in the references in Section 5.7. [commenting because this section doesn't exist, even in the original] -->

### <a name="time-series-object-example"></a><span data-ttu-id="be211-440">Příklad objekt časové řady</span><span class="sxs-lookup"><span data-stu-id="be211-440">Time series object example</span></span>
<span data-ttu-id="be211-441">Můžeme začít pracovat s našem příkladu.</span><span class="sxs-lookup"><span data-stu-id="be211-441">Let's get started with our example.</span></span> <span data-ttu-id="be211-442">Přetáhnout myší **nové** [spustit skript jazyka R] [ execute-r-script] modulu do experimentu.</span><span class="sxs-lookup"><span data-stu-id="be211-442">Drag and drop a **new** [Execute R Script][execute-r-script] module into your experiment.</span></span> <span data-ttu-id="be211-443">Připojit existující výstupní port Dataset1 výsledek [spustit skript jazyka R] [ execute-r-script] modulu Dataset1 vstupní port nové [spustit skript jazyka R] [ execute-r-script] modulu.</span><span class="sxs-lookup"><span data-stu-id="be211-443">Connect the Result Dataset1 output port of the existing [Execute R Script][execute-r-script] module to the Dataset1 input port of the new [Execute R Script][execute-r-script] module.</span></span>

<span data-ttu-id="be211-444">Jako jsme to udělali pro první příklady, jak jsme průběhu procházení v příkladu, v některých bodech I se zobrazí pouze přírůstkové další řádky kódu jazyka R při každém kroku.</span><span class="sxs-lookup"><span data-stu-id="be211-444">As I did for the first examples, as we progress through the example, at some points I will show only the incremental additional lines of R code at each step.</span></span>  

#### <a name="reading-the-dataframe"></a><span data-ttu-id="be211-445">Čtení dataframe</span><span class="sxs-lookup"><span data-stu-id="be211-445">Reading the dataframe</span></span>
<span data-ttu-id="be211-446">Jako první krok můžeme načtení dataframe a ujistěte se, že se nám získat očekávané výsledky.</span><span class="sxs-lookup"><span data-stu-id="be211-446">As a first step, let's read in a dataframe and make sure we get the expected results.</span></span> <span data-ttu-id="be211-447">Následující kód by měl provést úlohu.</span><span class="sxs-lookup"><span data-stu-id="be211-447">The following code should do the job.</span></span>

    # Comment the following if using RStudio
    cadairydata <- maml.mapInputPort(1)
    str(cadairydata) # Check the results

<span data-ttu-id="be211-448">Teď spusťte experiment.</span><span class="sxs-lookup"><span data-stu-id="be211-448">Now, run the experiment.</span></span> <span data-ttu-id="be211-449">Protokol nový tvar spustit skript jazyka R by měl vypadat jako obrázek 14.</span><span class="sxs-lookup"><span data-stu-id="be211-449">The log of the new Execute R Script shape should look like Figure 14.</span></span>

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

<span data-ttu-id="be211-450">*Obrázek 14. Shrnutí dataframe v modulu skriptu jazyka R provést.*</span><span class="sxs-lookup"><span data-stu-id="be211-450">*Figure 14. Summary of the dataframe in the Execute R Script module.*</span></span>

<span data-ttu-id="be211-451">Tato data jsou očekávané typy a formát.</span><span class="sxs-lookup"><span data-stu-id="be211-451">This data is of the expected types and format.</span></span> <span data-ttu-id="be211-452">Upozorňujeme, že sloupec 'Měsíc' je typ faktoru a má očekávaný počet úrovní, které.</span><span class="sxs-lookup"><span data-stu-id="be211-452">Note that the 'Month' column is of type factor and has the expected number of levels.</span></span>

#### <a name="creating-a-time-series-object"></a><span data-ttu-id="be211-453">Vytvoření objektu časové řady</span><span class="sxs-lookup"><span data-stu-id="be211-453">Creating a time series object</span></span>
<span data-ttu-id="be211-454">Je potřeba přidat do našich dataframe objekt časové řady.</span><span class="sxs-lookup"><span data-stu-id="be211-454">We need to add a time series object to our dataframe.</span></span> <span data-ttu-id="be211-455">Nahraďte kód aktuální následující text, který přidá nový sloupec třídy POSIXct.</span><span class="sxs-lookup"><span data-stu-id="be211-455">Replace the current code with the following, which adds a new column of class POSIXct.</span></span>

    # Comment the following if using RStudio
    cadairydata <- maml.mapInputPort(1)

    ## Create a new column as a POSIXct object
    Sys.setenv(TZ = "PST8PDT")
    cadairydata$Time <- as.POSIXct(strptime(paste(as.character(cadairydata$Year), "-", as.character(cadairydata$Month.Number), "-01 00:00:00", sep = ""), "%Y-%m-%d %H:%M:%S"))

    str(cadairydata) # Check the results

<span data-ttu-id="be211-456">Nyní vyhledejte v protokolu.</span><span class="sxs-lookup"><span data-stu-id="be211-456">Now, check the log.</span></span> <span data-ttu-id="be211-457">By měl vypadat jako obrázek 15.</span><span class="sxs-lookup"><span data-stu-id="be211-457">It should look like Figure 15.</span></span>

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

<span data-ttu-id="be211-458">*Obrázek 15. Shrnutí dataframe s objektem časové řady.*</span><span class="sxs-lookup"><span data-stu-id="be211-458">*Figure 15. Summary of the dataframe with a time series object.*</span></span>

<span data-ttu-id="be211-459">Jsme můžete zobrazit z souhrn, který nového sloupce, který je ve skutečnosti třídy POSIXct.</span><span class="sxs-lookup"><span data-stu-id="be211-459">We can see from the summary that the new column is in fact of class POSIXct.</span></span>

### <a name="exploring-and-transforming-the-data"></a><span data-ttu-id="be211-460">Zkoumání a transformace dat</span><span class="sxs-lookup"><span data-stu-id="be211-460">Exploring and transforming the data</span></span>
<span data-ttu-id="be211-461">Podíváme se na určité proměnné v této datové sadě.</span><span class="sxs-lookup"><span data-stu-id="be211-461">Let's explore some of the variables in this dataset.</span></span> <span data-ttu-id="be211-462">Matice scatterplot je dobrý způsob, jak vytvořit rychle zkontrolovat.</span><span class="sxs-lookup"><span data-stu-id="be211-462">A scatterplot matrix is a good way to produce a quick look.</span></span> <span data-ttu-id="be211-463">I mě nahrazení `str()` funkce v předchozí kód R pomocí následujícího řádku.</span><span class="sxs-lookup"><span data-stu-id="be211-463">I am replacing the `str()` function in the previous R code with the following line.</span></span>

    pairs(~ Cotagecheese.Prod + Icecream.Prod + Milk.Prod + N.CA.Fat.Price, data = cadairydata, main = "Pairwise Scatterplots of dairy time series")

<span data-ttu-id="be211-464">Spustit tento kód a zobrazit, co se stane.</span><span class="sxs-lookup"><span data-stu-id="be211-464">Run this code and see what happens.</span></span> <span data-ttu-id="be211-465">Vykreslení vytvořeného v portu R zařízení by měl vypadat jako obrázek 16.</span><span class="sxs-lookup"><span data-stu-id="be211-465">The plot produced at the R Device port should look like Figure 16.</span></span>

![Scatterplot matice vybrané proměnné][17]

<span data-ttu-id="be211-467">*Obrázek 16. Matice Scatterplot vybrané proměnné.*</span><span class="sxs-lookup"><span data-stu-id="be211-467">*Figure 16. Scatterplot matrix of selected variables.*</span></span>

<span data-ttu-id="be211-468">Je některé odd-looking struktura vztahy mezi tyto proměnné.</span><span class="sxs-lookup"><span data-stu-id="be211-468">There is some odd-looking structure in the relationships between these variables.</span></span> <span data-ttu-id="be211-469">Možná to mohou nastat z trendů v datech a ze skutečnosti, že jsme nebyly standardizované proměnné.</span><span class="sxs-lookup"><span data-stu-id="be211-469">Perhaps this arises from trends in the data and from the fact that we have not standardized the variables.</span></span>

### <a name="correlation-analysis"></a><span data-ttu-id="be211-470">Analýza korelace</span><span class="sxs-lookup"><span data-stu-id="be211-470">Correlation analysis</span></span>
<span data-ttu-id="be211-471">K provedení analýzy korelace musíme zrušte trendů i standardizovat proměnné.</span><span class="sxs-lookup"><span data-stu-id="be211-471">To perform correlation analysis we need to both de-trend and standardize the variables.</span></span> <span data-ttu-id="be211-472">Můžeme jednoduše použít R `scale()` funkci, která centra i škáluje proměnné.</span><span class="sxs-lookup"><span data-stu-id="be211-472">We could simply use the R `scale()` function, which both centers and scales variables.</span></span> <span data-ttu-id="be211-473">Tato funkce může také pracovat rychleji.</span><span class="sxs-lookup"><span data-stu-id="be211-473">This function might well run faster.</span></span> <span data-ttu-id="be211-474">Ale chcete zobrazit příklad Obranným programing v jazyce R.</span><span class="sxs-lookup"><span data-stu-id="be211-474">However, I want to show you an example of defensive programing in R.</span></span>

<span data-ttu-id="be211-475">`ts.detrend()` Provádí následující funkce obou těchto operací.</span><span class="sxs-lookup"><span data-stu-id="be211-475">The `ts.detrend()` function shown below performs both of these operations.</span></span> <span data-ttu-id="be211-476">Následující dva řádky kódu zrušte trendů data a pak standardizovat hodnoty.</span><span class="sxs-lookup"><span data-stu-id="be211-476">The following two lines of code de-trend the data and then standardize the values.</span></span>

    ts.detrend <- function(ts, Time, min.length = 3){
      ## Function to de-trend and standardize a time series

      ## Define some messages if they are NULL  
      messages <- c('ERROR: ts.detrend requires arguments ts and Time to have the same length',
                    'ERROR: ts.detrend requires argument ts to be of type numeric',
                    paste('WARNING: ts.detrend has encountered a time series with length less than', as.character(min.length)),
                    'ERROR: ts.detrend has encountered a Time argument not of class POSIXct',
                    'ERROR: Detrend regression has failed in ts.detrend',
                    'ERROR: Exception occurred in ts.detrend while standardizing time series in function ts.detrend'
      )
      # Create a vector of zeros to return as a default in some cases
      zerovec  <- rep(length(ts), 0.0)

      # The input arguments are not of the same length, return ts and quit
      if(length(Time) != length(ts)) {warning(messages[1]); return(ts)}

      # If the ts is not numeric, just return a zero vector and quit
      if(!is.numeric(ts)) {warning(messages[2]); return(zerovec)}

      # If the ts is too short, just return it and quit
      if((ts.length <- length(ts)) < min.length) {warning(messages[3]); return(ts)}

      ## Check that the Time variable is of class POSIXct
      if(class(cadairydata$Time)[[1]] != "POSIXct") {warning(messages[4]); return(ts)}

      ## De-trend the time series by using a linear model
      ts.frame  <- data.frame(ts = ts, Time = Time)
      tryCatch({ts <- ts - fitted(lm(ts ~ Time, data = ts.frame))},
               error = function(e){warning(messages[5]); zerovec})

      tryCatch( {stdev <- sqrt(sum((ts - mean(ts))^2))/(ts.length - 1)
                 ts <- ts/stdev},
                error = function(e){warning(messages[6]); zerovec})

      ts
    }  
    ## Apply the detrend.ts function to the variables of interest
    df.detrend <- data.frame(lapply(cadairydata[, 4:7], ts.detrend, cadairydata$Time))

    ## Plot the results to look at the relationships
    pairs(~ Cotagecheese.Prod + Icecream.Prod + Milk.Prod + N.CA.Fat.Price, data = df.detrend, main = "Pairwise Scatterplots of detrended standardized time series")

<span data-ttu-id="be211-477">Je s bit situaci v `ts.detrend()` funkce.</span><span class="sxs-lookup"><span data-stu-id="be211-477">There is quite a bit happening in the `ts.detrend()` function.</span></span> <span data-ttu-id="be211-478">Většina tento kód program kontroluje potenciální problémy s argumenty nebo práci s výjimky, které mohou stále nastat během výpočtů.</span><span class="sxs-lookup"><span data-stu-id="be211-478">Most of this code is checking for potential problems with the arguments or dealing with exceptions, which can still arise during the computations.</span></span> <span data-ttu-id="be211-479">Tento kód jenom pár řádků ve skutečnosti to výpočtů.</span><span class="sxs-lookup"><span data-stu-id="be211-479">Only a few lines of this code actually do the computations.</span></span>

<span data-ttu-id="be211-480">Už jsme projednat příkladem Obranným programování v [hodnota transformace](#valuetransformations).</span><span class="sxs-lookup"><span data-stu-id="be211-480">We have already discussed an example of defensive programming in [Value transformations](#valuetransformations).</span></span> <span data-ttu-id="be211-481">Obě bloky výpočtu je uzavřen do `tryCatch()`.</span><span class="sxs-lookup"><span data-stu-id="be211-481">Both computation blocks are wrapped in `tryCatch()`.</span></span> <span data-ttu-id="be211-482">Některé chyby má smysl vrátit původní vstupní vektoru a v ostatních případech návratu vektoru nul.</span><span class="sxs-lookup"><span data-stu-id="be211-482">For some errors it makes sense to return the original input vector, and in other cases, I return a vector of zeros.</span></span>  

<span data-ttu-id="be211-483">Všimněte si, že lineární regrese, použít pro vytvoření trendu zrušte regrese časové řady.</span><span class="sxs-lookup"><span data-stu-id="be211-483">Note that the linear regression used for de-trending is a time series regression.</span></span> <span data-ttu-id="be211-484">Proměnná předpověď je objekt časové řady.</span><span class="sxs-lookup"><span data-stu-id="be211-484">The predictor variable is a time series object.</span></span>  

<span data-ttu-id="be211-485">Jednou `ts.detrend()` je definována jsme použít k proměnným zájem o náš dataframe.</span><span class="sxs-lookup"><span data-stu-id="be211-485">Once `ts.detrend()` is defined we apply it to the variables of interest in our dataframe.</span></span> <span data-ttu-id="be211-486">Jsme musí coerce – v rozevíracím seznamu vytvořené `lapply()` k dataframe dat pomocí `as.data.frame()`.</span><span class="sxs-lookup"><span data-stu-id="be211-486">We must coerce the resulting list created by `lapply()` to data dataframe by using `as.data.frame()`.</span></span> <span data-ttu-id="be211-487">Z důvodu Obranným aspektů `ts.detrend()`, se nezdařilo zpracovat jeden z proměnné nezabrání správné zpracování ostatní.</span><span class="sxs-lookup"><span data-stu-id="be211-487">Because of defensive aspects of `ts.detrend()`, failure to process one of the variables will not prevent correct processing of the others.</span></span>  

<span data-ttu-id="be211-488">Poslední řádek kódu vytvoří pairwise scatterplot.</span><span class="sxs-lookup"><span data-stu-id="be211-488">The final line of code creates a pairwise scatterplot.</span></span> <span data-ttu-id="be211-489">Po spuštění kódu jazyka R, jsou výsledky scatterplot uvedené v obrázek 17.</span><span class="sxs-lookup"><span data-stu-id="be211-489">After running the R code, the results of the scatterplot are shown in Figure 17.</span></span>

![Pairwise scatterplot zrušte trendu a standardizovaném časové řady][18]

<span data-ttu-id="be211-491">*Obrázek 17. Pairwise scatterplot zrušte trendu a standardizovaném časové řady.*</span><span class="sxs-lookup"><span data-stu-id="be211-491">*Figure 17. Pairwise scatterplot of de-trended and standardized time series.*</span></span>

<span data-ttu-id="be211-492">Tyto výsledky jsou uvedeny v 16 obrázek, můžete porovnat.</span><span class="sxs-lookup"><span data-stu-id="be211-492">You can compare these results to those shown in Figure 16.</span></span> <span data-ttu-id="be211-493">Trend odebrat a proměnné standardizované vidíte mnohem menší struktury vztahy mezi tyto proměnné.</span><span class="sxs-lookup"><span data-stu-id="be211-493">With the trend removed and the variables standardized, we see a lot less structure in the relationships between these variables.</span></span>

<span data-ttu-id="be211-494">Kód k výpočtu korelací jako objekty PVJS R je následující.</span><span class="sxs-lookup"><span data-stu-id="be211-494">The code to compute the correlations as R ccf objects is as follows.</span></span>

    ## A function to compute pairwise correlations from a
    ## list of time series value vectors
    pair.cor <- function(pair.ind, ts.list, lag.max = 1, plot = FALSE){
      ccf(ts.list[[pair.ind[1]]], ts.list[[pair.ind[2]]], lag.max = lag.max, plot = plot)
    }

    ## A list of the pairwise indices
    corpairs <- list(c(1,2), c(1,3), c(1,4), c(2,3), c(2,4), c(3,4))

    ## Compute the list of ccf objects
    cadairycorrelations <- lapply(corpairs, pair.cor, df.detrend)  

    cadairycorrelations

<span data-ttu-id="be211-495">Spuštění tento kód vytvoří protokol znázorňuje obrázek 18.</span><span class="sxs-lookup"><span data-stu-id="be211-495">Running this code produces the log shown in Figure 18.</span></span>

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

<span data-ttu-id="be211-496">*Obrázek 18. Seznam PVJS objekty z pairwise korelace analýzy.*</span><span class="sxs-lookup"><span data-stu-id="be211-496">*Figure 18. List of ccf objects from the pairwise correlation analysis.*</span></span>

<span data-ttu-id="be211-497">Je hodnota korelace pro každý prodleva.</span><span class="sxs-lookup"><span data-stu-id="be211-497">There is a correlation value for each lag.</span></span> <span data-ttu-id="be211-498">Žádná z těchto hodnot korelace není dostatečně velký pro za významný.</span><span class="sxs-lookup"><span data-stu-id="be211-498">None of these correlation values is large enough to be significant.</span></span> <span data-ttu-id="be211-499">Jsme proto uzavřít, že jsme každou proměnnou modelu nezávisle.</span><span class="sxs-lookup"><span data-stu-id="be211-499">We can therefore conclude that we can model each variable independently.</span></span>

### <a name="output-a-dataframe"></a><span data-ttu-id="be211-500">Výstup a dataframe</span><span class="sxs-lookup"><span data-stu-id="be211-500">Output a dataframe</span></span>
<span data-ttu-id="be211-501">Pairwise korelací jsme mít počítá jako seznam R PVJS objektů.</span><span class="sxs-lookup"><span data-stu-id="be211-501">We have computed the pairwise correlations as a list of R ccf objects.</span></span> <span data-ttu-id="be211-502">To představuje bit problému jako výstupním portem datové sady výsledků skutečně vyžaduje dataframe.</span><span class="sxs-lookup"><span data-stu-id="be211-502">This presents a bit of a problem as the Result Dataset output port really requires a dataframe.</span></span> <span data-ttu-id="be211-503">Navíc objekt PVJS je sám seznam a chceme, aby pouze hodnoty v první prvek seznamu, korelací v různých pomalou.</span><span class="sxs-lookup"><span data-stu-id="be211-503">Further, the ccf object is itself a list and we want only the values in the first element of this list, the correlations at the various lags.</span></span>

<span data-ttu-id="be211-504">Následující kód extrahuje hodnoty prodleva ze seznamu PVJS objekty, které jsou sami seznamy.</span><span class="sxs-lookup"><span data-stu-id="be211-504">The following code extracts the lag values from the list of ccf objects, which are themselves lists.</span></span>

    df.correlations <- data.frame(do.call(rbind, lapply(cadairycorrelations, '[[', 1)))

    c.names <- c("correlation pair", "-1 lag", "0 lag", "+1 lag")
    r.names  <- c("Corr Cot Cheese - Ice Cream",
                  "Corr Cot Cheese - Milk Prod",
                  "Corr Cot Cheese - Fat Price",
                  "Corr Ice Cream - Mik Prod",
                  "Corr Ice Cream - Fat Price",
                  "Corr Milk Prod - Fat Price")

    ## Build a dataframe with the row names column and the
    ## correlation data frame and assign the column names
    outframe <- cbind(r.names, df.correlations)
    colnames(outframe) <- c.names
    outframe


    ## WARNING!
    ## The following line works only in Azure Machine Learning
    ## When running in RStudio, this code will result in an error
    #maml.mapOutputPort('outframe')

<span data-ttu-id="be211-505">První řádek kódu je trochu složité a vysvětlení mohou pomoci porozumět.</span><span class="sxs-lookup"><span data-stu-id="be211-505">The first line of code is a bit tricky, and some explanation may help you understand it.</span></span> <span data-ttu-id="be211-506">Práce zevnitř ven. máme následující:</span><span class="sxs-lookup"><span data-stu-id="be211-506">Working from the inside out we have the following:</span></span>

1. <span data-ttu-id="be211-507">'**[[**'Operátor s argumentem'**1**se vybere vektoru korelací na pomalou z první prvek seznamu PVJS objektů.</span><span class="sxs-lookup"><span data-stu-id="be211-507">The '**[[**' operator with the argument '**1**' selects the vector of correlations at the lags from the first element of the ccf object list.</span></span>
2. <span data-ttu-id="be211-508">`do.call()` Funkce se vztahuje `rbind()` funkce přes prvky v seznamu vrátí podle `lapply()`.</span><span class="sxs-lookup"><span data-stu-id="be211-508">The `do.call()` function applies the `rbind()` function over the elements of the list returns by `lapply()`.</span></span>
3. <span data-ttu-id="be211-509">`data.frame()` Funkce převede typ výsledku vytvořeného `do.call()` k dataframe.</span><span class="sxs-lookup"><span data-stu-id="be211-509">The `data.frame()` function coerces the result produced by `do.call()` to a dataframe.</span></span>

<span data-ttu-id="be211-510">Všimněte si, že názvy řádků ve sloupci dataframe.</span><span class="sxs-lookup"><span data-stu-id="be211-510">Note that the row names are in a column of the dataframe.</span></span> <span data-ttu-id="be211-511">Provádění Ano uchovává řádek názvy, když se výstup z [spustit skript jazyka R][execute-r-script].</span><span class="sxs-lookup"><span data-stu-id="be211-511">Doing so preserves the row names when they are output from the [Execute R Script][execute-r-script].</span></span>

<span data-ttu-id="be211-512">Spuštění kódu vytvoří výstup ukazuje obrázek 19 při I **vizualizovat** výstup na port datovou sadu výsledků.</span><span class="sxs-lookup"><span data-stu-id="be211-512">Running the code produces the output shown in Figure 19 when I **Visualize** the output at the Result Dataset port.</span></span> <span data-ttu-id="be211-513">Názvy řádků jsou z prvního sloupce tak, jak má.</span><span class="sxs-lookup"><span data-stu-id="be211-513">The row names are in the first column, as intended.</span></span>

![Výstup výsledků z analýzy korelace][20]

<span data-ttu-id="be211-515">*Obrázek 19. Výstup z analysis korelace výsledky.*</span><span class="sxs-lookup"><span data-stu-id="be211-515">*Figure 19. Results output from the correlation analysis.*</span></span>

## <span data-ttu-id="be211-516"><a id="seasonalforecasting"></a>Příklad časové řady: sezónní prognózy</span><span class="sxs-lookup"><span data-stu-id="be211-516"><a id="seasonalforecasting"></a>Time series example: seasonal forecasting</span></span>
<span data-ttu-id="be211-517">Naše data je teď ve formě vhodné pro analýzu a bylo zjištěno, že neexistují žádné významné korelací mezi proměnné.</span><span class="sxs-lookup"><span data-stu-id="be211-517">Our data is now in a form suitable for analysis, and we have determined there are no significant correlations between the variables.</span></span> <span data-ttu-id="be211-518">Umožňuje přesunout a vytvořte časové řady modelu prognózy.</span><span class="sxs-lookup"><span data-stu-id="be211-518">Let's move on and create a time series forecasting model.</span></span> <span data-ttu-id="be211-519">Pomocí tohoto modelu jsme se prognózy produkce mléka kalifornské po dobu 12 měsíců produktu 2013.</span><span class="sxs-lookup"><span data-stu-id="be211-519">Using this model we will forecast California milk production for the 12 months of 2013.</span></span>

<span data-ttu-id="be211-520">Naše předpovědi modelu bude mít dvě součásti, součást trendu a sezónní součást.</span><span class="sxs-lookup"><span data-stu-id="be211-520">Our forecasting model will have two components, a trend component and a seasonal component.</span></span> <span data-ttu-id="be211-521">Dokončení prognózy je produkt tyto dvě součásti.</span><span class="sxs-lookup"><span data-stu-id="be211-521">The complete forecast is the product of these two components.</span></span> <span data-ttu-id="be211-522">Tento typ modelu se označuje jako multiplikativní modelu.</span><span class="sxs-lookup"><span data-stu-id="be211-522">This type of model is known as a multiplicative model.</span></span> <span data-ttu-id="be211-523">Alternativou je sčítání modelu.</span><span class="sxs-lookup"><span data-stu-id="be211-523">The alternative is an additive model.</span></span> <span data-ttu-id="be211-524">K proměnné, které vás zajímají, což může této analýze tractable jsme již nainstalovali protokolu transformace.</span><span class="sxs-lookup"><span data-stu-id="be211-524">We have already applied a log transformation to the variables of interest, which makes this analysis tractable.</span></span>

<span data-ttu-id="be211-525">Kód dokončení R pro tento oddíl je v souboru zip, které jste dříve stáhli.</span><span class="sxs-lookup"><span data-stu-id="be211-525">The complete R code for this section is in the zip file you downloaded earlier.</span></span>

### <a name="creating-the-dataframe-for-analysis"></a><span data-ttu-id="be211-526">Vytváření dataframe pro analýzu</span><span class="sxs-lookup"><span data-stu-id="be211-526">Creating the dataframe for analysis</span></span>
<span data-ttu-id="be211-527">Začněte přidáním **nové** [spustit skript jazyka R] [ execute-r-script] modulu experimentu.</span><span class="sxs-lookup"><span data-stu-id="be211-527">Start by adding a **new** [Execute R Script][execute-r-script] module to your experiment.</span></span> <span data-ttu-id="be211-528">Připojení **datovou sadu výsledků** výstup existující [spustit skript jazyka R] [ execute-r-script] modulu **Dataset1** vstupní části nového modulu.</span><span class="sxs-lookup"><span data-stu-id="be211-528">Connect the **Result Dataset** output of the existing [Execute R Script][execute-r-script] module to the **Dataset1** input of the new module.</span></span> <span data-ttu-id="be211-529">Výsledek by měl vypadat podobně jako obrázek 20.</span><span class="sxs-lookup"><span data-stu-id="be211-529">The result should look something like Figure 20.</span></span>

![Experiment s modulem nové spuštění skriptu jazyka R přidán][21]

<span data-ttu-id="be211-531">*Obrázek 20. Experiment s modulem nové spuštění skriptu jazyka R přidat.*</span><span class="sxs-lookup"><span data-stu-id="be211-531">*Figure 20. The experiment with the new Execute R Script module added.*</span></span>

<span data-ttu-id="be211-532">Jako korelace analýzy, kterou jsme právě dokončili, je potřeba přidat sloupec s objektem POSIXct časové řady.</span><span class="sxs-lookup"><span data-stu-id="be211-532">As with the correlation analysis we just completed, we need to add a column with a POSIXct time series object.</span></span> <span data-ttu-id="be211-533">Následující kód a to stejně.</span><span class="sxs-lookup"><span data-stu-id="be211-533">The following code will do just this.</span></span>

    # If running in Machine Learning Studio, uncomment the first line with maml.mapInputPort()
    cadairydata <- maml.mapInputPort(1)

    ## Create a new column as a POSIXct object
    Sys.setenv(TZ = "PST8PDT")
    cadairydata$Time <- as.POSIXct(strptime(paste(as.character(cadairydata$Year), "-", as.character(cadairydata$Month.Number), "-01 00:00:00", sep = ""), "%Y-%m-%d %H:%M:%S"))

    str(cadairydata)

<span data-ttu-id="be211-534">Spusťte tento kód a naleznete v protokolu.</span><span class="sxs-lookup"><span data-stu-id="be211-534">Run this code and look at the log.</span></span> <span data-ttu-id="be211-535">Výsledek by měl vypadat jako obrázek 21.</span><span class="sxs-lookup"><span data-stu-id="be211-535">The result should look like Figure 21.</span></span>

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

<span data-ttu-id="be211-536">*Obrázek 21. Přehled dataframe.*</span><span class="sxs-lookup"><span data-stu-id="be211-536">*Figure 21. A summary of the dataframe.*</span></span>

<span data-ttu-id="be211-537">S tímto výsledkem jsou připravení začít Naše analýzy.</span><span class="sxs-lookup"><span data-stu-id="be211-537">With this result, we are ready to start our analysis.</span></span>

### <a name="create-a-training-dataset"></a><span data-ttu-id="be211-538">Vytvoření datové sady školení</span><span class="sxs-lookup"><span data-stu-id="be211-538">Create a training dataset</span></span>
<span data-ttu-id="be211-539">S dataframe sestavený musíme vytvořit datovou sadu školení.</span><span class="sxs-lookup"><span data-stu-id="be211-539">With the dataframe constructed we need to create a training dataset.</span></span> <span data-ttu-id="be211-540">Tato data budou zahrnovat všechny připomínky kromě posledních 12 roku 2013, což je naše testovací datové sady.</span><span class="sxs-lookup"><span data-stu-id="be211-540">This data will include all of the observations except the last 12, of the year 2013, which is our test dataset.</span></span> <span data-ttu-id="be211-541">Následující kód podmnožin dataframe a vytvoří pozemků proměnných dojnic produkčního prostředí a ceny.</span><span class="sxs-lookup"><span data-stu-id="be211-541">The following code subsets the dataframe and creates plots of the dairy production and price variables.</span></span> <span data-ttu-id="be211-542">Potom vytvořit pozemků čtyři produkční a cena proměnné.</span><span class="sxs-lookup"><span data-stu-id="be211-542">I then create plots of the four production and price variables.</span></span> <span data-ttu-id="be211-543">Anonymní funkce se používá k definování rozšiřuje pro vykreslení a pak iterace v seznamu další dva argumenty s `Map()`.</span><span class="sxs-lookup"><span data-stu-id="be211-543">An anonymous function is used to define some augments for plot, and then iterate over the list of the other two arguments with `Map()`.</span></span> <span data-ttu-id="be211-544">Pokud přemýšlíte o, pro smyčky by mít fungovala bez problémů v tomto poli, jsou správná.</span><span class="sxs-lookup"><span data-stu-id="be211-544">If you are thinking that a for loop would have worked fine here, you are correct.</span></span> <span data-ttu-id="be211-545">Ale vzhledem k tomu, že je funkční jazyk R I mě ukazuje funkční přístup.</span><span class="sxs-lookup"><span data-stu-id="be211-545">But, since R is a functional language I am showing you a functional approach.</span></span>

    cadairytrain <- cadairydata[1:216, ]

    Ylabs  <- list("Log CA Cotage Cheese Production, 1000s lb",
                   "Log CA Ice Cream Production, 1000s lb",
                   "Log CA Milk Production 1000s lb",
                   "Log North CA Milk Milk Fat Price per 1000 lb")

    Map(function(y, Ylabs){plot(cadairytrain$Time, y, xlab = "Time", ylab = Ylabs, type = "l")}, cadairytrain[, 4:7], Ylabs)

<span data-ttu-id="be211-546">Spuštění kódu vytvoří řadu časové řady ukazuje zeměpisný z výstupu R zařízení znázorňuje obrázek 22.</span><span class="sxs-lookup"><span data-stu-id="be211-546">Running the code produces the series of time series plots from the R Device output shown in Figure 22.</span></span> <span data-ttu-id="be211-547">Všimněte si, že časová osa je v jednotkách kalendářních dat, dobrý výhodou časové řady vykreslení metoda.</span><span class="sxs-lookup"><span data-stu-id="be211-547">Note that the time axis is in units of dates, a nice benefit of the time series plot method.</span></span>

![První z řady pozemků čas kalifornské dojnic produkčního prostředí a cena dat](./media/machine-learning-r-quickstart/unnamed-chunk-161.png)

![Druhý čas řady pozemků kalifornské dojnic produkčního prostředí a cena dat](./media/machine-learning-r-quickstart/unnamed-chunk-162.png)

![Třetí čas řady pozemků kalifornské dojnic produkčního prostředí a cena dat](./media/machine-learning-r-quickstart/unnamed-chunk-163.png)

![Čtvrtý čas řady pozemků kalifornské dojnic produkčního prostředí a cena dat](./media/machine-learning-r-quickstart/unnamed-chunk-164.png)

<span data-ttu-id="be211-552">*Obrázek 22. Čas řady pozemků produkci mléka kalifornské a cena data.*</span><span class="sxs-lookup"><span data-stu-id="be211-552">*Figure 22. Time series plots of California dairy production and price data.*</span></span>

### <a name="a-trend-model"></a><span data-ttu-id="be211-553">Model trendu</span><span class="sxs-lookup"><span data-stu-id="be211-553">A trend model</span></span>
<span data-ttu-id="be211-554">S vytvořili objekt řady čas a nutnosti seznámili data, Začněme vytvořit model trend kalifornské mléka provozními daty.</span><span class="sxs-lookup"><span data-stu-id="be211-554">Having created a time series object and having had a look at the data, let's start to construct a trend model for the California milk production data.</span></span> <span data-ttu-id="be211-555">Jsme to lze provést pomocí regrese časové řady.</span><span class="sxs-lookup"><span data-stu-id="be211-555">We can do this with a time series regression.</span></span> <span data-ttu-id="be211-556">Je však vymazat z výkresu, který jsme bude potřebovat víc než sklon a zachycení k přesnému modelování zjištěnou trend v Cvičná data.</span><span class="sxs-lookup"><span data-stu-id="be211-556">However, it is clear from the plot that we will need more than a slope and intercept to accurately model the observed trend in the training data.</span></span>

<span data-ttu-id="be211-557">Zadané malém měřítku dat, bude sestavení modelu pro trend Rstudia a pak kopírování a vkládání výsledný model do Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="be211-557">Given the small scale of the data, I will build the model for trend in RStudio and then cut and paste the resulting model into Azure Machine Learning.</span></span> <span data-ttu-id="be211-558">Rstudia poskytuje interaktivní prostředí pro tento typ interaktivní analýzu.</span><span class="sxs-lookup"><span data-stu-id="be211-558">RStudio provides an interactive environment for this type of interactive analysis.</span></span>

<span data-ttu-id="be211-559">Jako první pokus o bude proveden pokus polynomické regrese s zajišťuje až 3.</span><span class="sxs-lookup"><span data-stu-id="be211-559">As a first attempt, I will try a polynomial regression with powers up to 3.</span></span> <span data-ttu-id="be211-560">Je-li skutečné riziko z přepsání hodí tyto druhy modelů.</span><span class="sxs-lookup"><span data-stu-id="be211-560">There is a real danger of over-fitting these kinds of models.</span></span> <span data-ttu-id="be211-561">Proto je vyhýbat se nejvyšších podmínky.</span><span class="sxs-lookup"><span data-stu-id="be211-561">Therefore, it is best to avoid high order terms.</span></span> <span data-ttu-id="be211-562">`I()` Funkce omezuje výklad obsah (interpretuje obsah, jako je") a umožňuje oznámena interpretovaný funkci napsat během rovnice regrese.</span><span class="sxs-lookup"><span data-stu-id="be211-562">The `I()` function inhibits interpretation of the contents (interprets the contents 'as is') and allows you to write a literally interpreted function in a regression equation.</span></span>

    milk.lm <- lm(Milk.Prod ~ Time + I(Month.Count^2) + I(Month.Count^3), data = cadairytrain)
    summary(milk.lm)

<span data-ttu-id="be211-563">Tím se vygeneruje následující.</span><span class="sxs-lookup"><span data-stu-id="be211-563">This generates the following.</span></span>

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

<span data-ttu-id="be211-564">Z hodnot P (Pr (> | t |)) v tento výstup, uvidíme, že kvadratických termín nemusí být důležité.</span><span class="sxs-lookup"><span data-stu-id="be211-564">From P values (Pr(>|t|)) in this output, we can see that the squared term may not be significant.</span></span> <span data-ttu-id="be211-565">Použiji `update()` funkce Upravit tento model umístěním kvadratických termín.</span><span class="sxs-lookup"><span data-stu-id="be211-565">I will use the `update()` function to modify this model by dropping the squared term.</span></span>

    milk.lm <- update(milk.lm, . ~ . - I(Month.Count^2))
    summary(milk.lm)

<span data-ttu-id="be211-566">Tím se vygeneruje následující.</span><span class="sxs-lookup"><span data-stu-id="be211-566">This generates the following.</span></span>

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

<span data-ttu-id="be211-567">To vypadá lepší.</span><span class="sxs-lookup"><span data-stu-id="be211-567">This looks better.</span></span> <span data-ttu-id="be211-568">Všechny podmínky jsou důležitá.</span><span class="sxs-lookup"><span data-stu-id="be211-568">All of the terms are significant.</span></span> <span data-ttu-id="be211-569">Hodnota 2e-16 však je výchozí hodnota a by neměly být příliš vážně brány.</span><span class="sxs-lookup"><span data-stu-id="be211-569">However, the 2e-16 value is a default value, and should not be taken too seriously.</span></span>  

<span data-ttu-id="be211-570">Jako testu správností vytvoříme vykreslení řad čas kalifornské produkci mléka dat s křivku trendu vidět.</span><span class="sxs-lookup"><span data-stu-id="be211-570">As a sanity test, let's make a time series plot of the California dairy production data with the trend curve shown.</span></span> <span data-ttu-id="be211-571">Přidaný následující kód v Azure Machine Learning [spustit skript jazyka R] [ execute-r-script] modelu (ne Rstudia) pro vytvoření modelu a ujistěte se, vykreslení.</span><span class="sxs-lookup"><span data-stu-id="be211-571">I have added the following code in the Azure Machine Learning [Execute R Script][execute-r-script] model (not RStudio) to create the model and make a plot.</span></span> <span data-ttu-id="be211-572">Výsledek vidíte na obrázku 23.</span><span class="sxs-lookup"><span data-stu-id="be211-572">The result is shown in Figure 23.</span></span>

    milk.lm <- lm(Milk.Prod ~ Time + I(Month.Count^3), data = cadairytrain)

    plot(cadairytrain$Time, cadairytrain$Milk.Prod, xlab = "Time", ylab = "Log CA Milk Production 1000s lb", type = "l")
    lines(cadairytrain$Time, predict(milk.lm, cadairytrain), lty = 2, col = 2)

![Kalifornské mléka provozními daty s zobrazený model trendu](./media/machine-learning-r-quickstart/unnamed-chunk-18.png)

<span data-ttu-id="be211-574">*Obrázek 23. Kalifornské mléka provozními daty s trend vzoru.*</span><span class="sxs-lookup"><span data-stu-id="be211-574">*Figure 23. California milk production data with trend model shown.*</span></span>

<span data-ttu-id="be211-575">Zdá se, trend modelu dostatečně dobře vyhovuje data.</span><span class="sxs-lookup"><span data-stu-id="be211-575">It looks like the trend model fits the data fairly well.</span></span> <span data-ttu-id="be211-576">Navíc není nejspíš důkaz přečerpání vhodnosti, například v křivce modelu rychlé pohyby lichá.</span><span class="sxs-lookup"><span data-stu-id="be211-576">Further, there does not seem to be evidence of over-fitting, such as odd wiggles in the model curve.</span></span>  

### <a name="seasonal-model"></a><span data-ttu-id="be211-577">Sezónní modelu</span><span class="sxs-lookup"><span data-stu-id="be211-577">Seasonal model</span></span>
<span data-ttu-id="be211-578">S modelem trend v dolním musíme push a zahrnují sezónní důsledky.</span><span class="sxs-lookup"><span data-stu-id="be211-578">With a trend model in hand, we need to push on and include the seasonal effects.</span></span> <span data-ttu-id="be211-579">Měsíc v roce použijeme jako proměnnou fiktivní ve model lineární k zachycení účinek po měsících.</span><span class="sxs-lookup"><span data-stu-id="be211-579">We will use the month of the year as a dummy variable in the linear model to capture the month-by-month effect.</span></span> <span data-ttu-id="be211-580">Poznámka: když zavedete Multi-Factor proměnné do modelu, nesmí počítaný intercept.</span><span class="sxs-lookup"><span data-stu-id="be211-580">Note that when you introduce factor variables into a model, the intercept must not be computed.</span></span> <span data-ttu-id="be211-581">Pokud není to uděláte, je přepsání zadané vzorec a R bude odstraňte jednu z požadovaného faktorů, ale ponechat termín zachycení.</span><span class="sxs-lookup"><span data-stu-id="be211-581">If you do not do this, the formula is over-specified and R will drop one of the desired factors but keep the intercept term.</span></span>

<span data-ttu-id="be211-582">Vzhledem k tomu, že máme model uspokojivé trend můžeme použít `update()` funkce pro přidání nové podmínky do existujícího modelu.</span><span class="sxs-lookup"><span data-stu-id="be211-582">Since we have a satisfactory trend model we can use the `update()` function to add the new terms to the existing model.</span></span> <span data-ttu-id="be211-583">-1 ve vzorci aktualizace zahodí termín zachycení.</span><span class="sxs-lookup"><span data-stu-id="be211-583">The -1 in the update formula drops the intercept term.</span></span> <span data-ttu-id="be211-584">Pokračování v Rstudia momentálně:</span><span class="sxs-lookup"><span data-stu-id="be211-584">Continuing in RStudio for the moment:</span></span>

    milk.lm2 <- update(milk.lm, . ~ . + Month - 1)
    summary(milk.lm2)

<span data-ttu-id="be211-585">Tím se vygeneruje následující.</span><span class="sxs-lookup"><span data-stu-id="be211-585">This generates the following.</span></span>

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

<span data-ttu-id="be211-586">Vidíte, že model už má některý výraz zachycení a má 12 měsíc důležité faktory.</span><span class="sxs-lookup"><span data-stu-id="be211-586">We see that the model no longer has an intercept term and has 12 significant month factors.</span></span> <span data-ttu-id="be211-587">Toto je přesně co jsme chtěli najdete v článku.</span><span class="sxs-lookup"><span data-stu-id="be211-587">This is exactly what we wanted to see.</span></span>

<span data-ttu-id="be211-588">Provedeme jiné výkresu čas řady dat produkci mléka kalifornské jak dobře funguje sezónní modelu.</span><span class="sxs-lookup"><span data-stu-id="be211-588">Let's make another time series plot of the California dairy production data to see how well the seasonal model is working.</span></span> <span data-ttu-id="be211-589">Přidaný následující kód v Azure Machine Learning [spustit skript jazyka R] [ execute-r-script] pro vytvoření modelu a ujistěte se, vykreslení.</span><span class="sxs-lookup"><span data-stu-id="be211-589">I have added the following code in the Azure Machine Learning [Execute R Script][execute-r-script] to create the model and make a plot.</span></span>

    milk.lm2 <- lm(Milk.Prod ~ Time + I(Month.Count^3) + Month - 1, data = cadairytrain)

    plot(cadairytrain$Time, cadairytrain$Milk.Prod, xlab = "Time", ylab = "Log CA Milk Production 1000s lb", type = "l")
    lines(cadairytrain$Time, predict(milk.lm2, cadairytrain), lty = 2, col = 2)

<span data-ttu-id="be211-590">Spuštění tohoto kódu v Azure Machine Learning vytvoří výkresu znázorňuje obrázek 24.</span><span class="sxs-lookup"><span data-stu-id="be211-590">Running this code in Azure Machine Learning produces the plot shown in Figure 24.</span></span>

![Produkce mléka kalifornské s modelem včetně sezónní efekty](./media/machine-learning-r-quickstart/unnamed-chunk-20.png)

<span data-ttu-id="be211-592">*Obrázek 24. Produkce mléka kalifornské s modelem včetně sezónní účinky.*</span><span class="sxs-lookup"><span data-stu-id="be211-592">*Figure 24. California milk production with model including seasonal effects.*</span></span>

<span data-ttu-id="be211-593">Přizpůsobit data zobrazená na obrázek 24 je spíš podporovat.</span><span class="sxs-lookup"><span data-stu-id="be211-593">The fit to the data shown in Figure 24 is rather encouraging.</span></span> <span data-ttu-id="be211-594">Vyhledejte přiměřené sezónní efekt (měsíční variace) i trend.</span><span class="sxs-lookup"><span data-stu-id="be211-594">Both the trend and the seasonal effect (monthly variation) look reasonable.</span></span>

<span data-ttu-id="be211-595">Jako další kontrolu na našem modelu můžeme Podíváme se na toto políčko, budou.</span><span class="sxs-lookup"><span data-stu-id="be211-595">As another check on our model, let's have a look at the residuals.</span></span> <span data-ttu-id="be211-596">Následující kód vypočítá předpovězené hodnoty z našich dva modely, vypočítá toto políčko, budou pro sezónní model a pak ukazuje zeměpisný tyto zbytky pro Cvičná data.</span><span class="sxs-lookup"><span data-stu-id="be211-596">The following code computes the predicted values from our two models, computes the residuals for the seasonal model, and then plots these residuals for the training data.</span></span>

    ## Compute predictions from our models
    predict1  <- predict(milk.lm, cadairydata)
    predict2  <- predict(milk.lm2, cadairydata)

    ## Compute and plot the residuals
    residuals <- cadairydata$Milk.Prod - predict2
    plot(cadairytrain$Time, residuals[1:216], xlab = "Time", ylab ="Residuals of Seasonal Model")

<span data-ttu-id="be211-597">Zbytkové výkresu vidíte na obrázku 25.</span><span class="sxs-lookup"><span data-stu-id="be211-597">The residual plot is shown in Figure 25.</span></span>

![Toto políčko, budou sezónní modelu pro Cvičná data](./media/machine-learning-r-quickstart/unnamed-chunk-21.png)

<span data-ttu-id="be211-599">*Obrázek 25. Toto políčko, budou sezónní modelu pro Cvičná data.*</span><span class="sxs-lookup"><span data-stu-id="be211-599">*Figure 25. Residuals of the seasonal model for the training data.*</span></span>

<span data-ttu-id="be211-600">Tyto zbytky vypadat přiměřené.</span><span class="sxs-lookup"><span data-stu-id="be211-600">These residuals look reasonable.</span></span> <span data-ttu-id="be211-601">Neexistuje žádná konkrétní struktura, s výjimkou účinek poklesu 2008-2009, který našeho modelu neanalyzuje zvlášť dobře.</span><span class="sxs-lookup"><span data-stu-id="be211-601">There is no particular structure, except the effect of the 2008-2009 recession, which our model does not account for particularly well.</span></span>

<span data-ttu-id="be211-602">Vykreslení znázorňuje obrázek 25 je užitečné pro zjišťování v toto políčko, budou všechny vzory závislá na čase.</span><span class="sxs-lookup"><span data-stu-id="be211-602">The plot shown in Figure 25 is useful for detecting any time-dependent patterns in the residuals.</span></span> <span data-ttu-id="be211-603">Explicitní přístup výpočetních a vykreslení toto políčko, budou I používá umístí toto políčko, budou v pořadí časů na vykreslení.</span><span class="sxs-lookup"><span data-stu-id="be211-603">The explicit approach of computing and plotting the residuals I used places the residuals in time order on the plot.</span></span> <span data-ttu-id="be211-604">Pokud na druhé straně I měl vykreslí `milk.lm$residuals`, v pořadí časů by byly vykreslení.</span><span class="sxs-lookup"><span data-stu-id="be211-604">If, on the other hand, I had plotted `milk.lm$residuals`, the plot would not have been in time order.</span></span>

<span data-ttu-id="be211-605">Můžete také použít `plot.lm()` k vytvoření řady diagnostických pozemků.</span><span class="sxs-lookup"><span data-stu-id="be211-605">You can also use `plot.lm()` to produce a series of diagnostic plots.</span></span>

    ## Show the diagnostic plots for the model
    plot(milk.lm2, ask = FALSE)

<span data-ttu-id="be211-606">Tento kód vytvoří řady diagnostických pozemků znázorňuje obrázek 26.</span><span class="sxs-lookup"><span data-stu-id="be211-606">This code produces a series of diagnostic plots shown in Figure 26.</span></span>

![První diagnostiky pozemků sezónní modelu](./media/machine-learning-r-quickstart/unnamed-chunk-221.png)

![Druhý diagnostiky pozemků sezónní modelu](./media/machine-learning-r-quickstart/unnamed-chunk-222.png)

![Třetí diagnostiky pozemků sezónní modelu](./media/machine-learning-r-quickstart/unnamed-chunk-223.png)

![Čtvrtý diagnostiky pozemků sezónní modelu](./media/machine-learning-r-quickstart/unnamed-chunk-224.png)

<span data-ttu-id="be211-611">*Obrázek 26. Diagnostika ukazuje zeměpisný sezónní modelu.*</span><span class="sxs-lookup"><span data-stu-id="be211-611">*Figure 26. Diagnostic plots for the seasonal model.*</span></span>

<span data-ttu-id="be211-612">Existuje několik vysoké míry body určené v těchto pozemků, ale nic způsobit velmi významný.</span><span class="sxs-lookup"><span data-stu-id="be211-612">There are a few highly influential points identified in these plots, but nothing to cause great concern.</span></span> <span data-ttu-id="be211-613">Navíc můžete vidíte z výkresu normální Q-Q, toto políčko, budou se za normálních okolností distribuované důležité předpokládá pro lineární modely.</span><span class="sxs-lookup"><span data-stu-id="be211-613">Further, we can see from the Normal Q-Q plot that the residuals are close to normally distributed, an important assumption for linear models.</span></span>

### <a name="forecasting-and-model-evaluation"></a><span data-ttu-id="be211-614">Vyhodnocení prognózy a modelu</span><span class="sxs-lookup"><span data-stu-id="be211-614">Forecasting and model evaluation</span></span>
<span data-ttu-id="be211-615">Je právě jeden krok udělat, aby dokončení našem příkladu.</span><span class="sxs-lookup"><span data-stu-id="be211-615">There is just one more thing to do to complete our example.</span></span> <span data-ttu-id="be211-616">Potřebujeme výpočetní prognózy a měřit chyba proti skutečná data.</span><span class="sxs-lookup"><span data-stu-id="be211-616">We need to compute forecasts and measure the error against the actual data.</span></span> <span data-ttu-id="be211-617">Naše prognózy bude po dobu 12 měsíců produktu 2013.</span><span class="sxs-lookup"><span data-stu-id="be211-617">Our forecast will be for the 12 months of 2013.</span></span> <span data-ttu-id="be211-618">Jsme můžete vypočítat k chybě měr pro Prognóza na skutečná data, která není součástí naší datové sadě školení.</span><span class="sxs-lookup"><span data-stu-id="be211-618">We can compute an error measure for this forecast to the actual data that is not part of our training dataset.</span></span> <span data-ttu-id="be211-619">Kromě toho jsme můžete porovnat výkon na 18 let Cvičná data do 12 měsíců od testovacích datech.</span><span class="sxs-lookup"><span data-stu-id="be211-619">Additionally, we can compare performance on the 18 years of training data to the 12 months of test data.</span></span>  

<span data-ttu-id="be211-620">Počet metriky slouží k měření výkonu modelů časové řady.</span><span class="sxs-lookup"><span data-stu-id="be211-620">A number of metrics are used to measure the performance of time series models.</span></span> <span data-ttu-id="be211-621">V našem případě použijeme chyba kořenové směrodatná (RMS).</span><span class="sxs-lookup"><span data-stu-id="be211-621">In our case we will use the root mean square (RMS) error.</span></span> <span data-ttu-id="be211-622">Následující funkce vypočítá chybě RMS mezi dvou řad.</span><span class="sxs-lookup"><span data-stu-id="be211-622">The following function computes the RMS error between two series.</span></span>  

    RMS.error <- function(series1, series2, is.log = TRUE, min.length = 2){
      ## Function to compute the RMS error or difference between two
      ## series or vectors

      messages <- c("ERROR: Input arguments to function RMS.error of wrong type encountered",
                    "ERROR: Input vector to function RMS.error is too short",
                    "ERROR: Input vectors to function RMS.error must be of same length",
                    "WARNING: Funtion rms.error has received invald input time series.")

      ## Check the arguments
      if(!is.numeric(series1) | !is.numeric(series2) | !is.logical(is.log) | !is.numeric(min.length)) {
        warning(messages[1])
        return(NA)}

      if(length(series1) < min.length) {
        warning(messages[2])
        return(NA)}

      if((length(series1) != length(series2))) {
           warning(messages[3])
        return(NA)}

      ## If is.log is TRUE exponentiate the values, else just copy
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

    ## Compute the RMS error in a dataframe
      tryCatch( {
        sqrt(sum((temp1 - temp2)^2) / length(temp1))},
        error = function(e){warning(messages[4]); NA})
    }

<span data-ttu-id="be211-623">Stejně jako u `log.transform()` funkce jsme popsané v části "Hodnoty transformace" je poměrně velké množství kontrolu a výjimky obnovení kód chyby v této funkci.</span><span class="sxs-lookup"><span data-stu-id="be211-623">As with the `log.transform()` function we discussed in the "Value transformations" section, there is quite a lot of error checking and exception recovery code in this function.</span></span> <span data-ttu-id="be211-624">Principy těmto nekompatibilitám jsou stejné.</span><span class="sxs-lookup"><span data-stu-id="be211-624">The principles employed are the same.</span></span> <span data-ttu-id="be211-625">Práce se provádí na dvou místech uzavřen do `tryCatch()`.</span><span class="sxs-lookup"><span data-stu-id="be211-625">The work is done in two places wrapped in `tryCatch()`.</span></span> <span data-ttu-id="be211-626">Nejprve časové řady jsou exponentiated, protože Pracujeme s protokoly hodnot.</span><span class="sxs-lookup"><span data-stu-id="be211-626">First, the time series are exponentiated, since we have been working with the logs of the values.</span></span> <span data-ttu-id="be211-627">Druhý se počítá skutečné chybové RMS.</span><span class="sxs-lookup"><span data-stu-id="be211-627">Second, the actual RMS error is computed.</span></span>  

<span data-ttu-id="be211-628">Vybaven funkci k měření chybu RMS, umožňuje vytvářet a výstupní dataframe, který obsahuje chyby služby RMS.</span><span class="sxs-lookup"><span data-stu-id="be211-628">Equipped with a function to measure the RMS error, let's build and output a dataframe containing the RMS errors.</span></span> <span data-ttu-id="be211-629">Jsme bude obsahovat podmínky pro samotné trend model a model dokončení sezónní faktory.</span><span class="sxs-lookup"><span data-stu-id="be211-629">We will include terms for the trend model alone and the complete model with seasonal factors.</span></span> <span data-ttu-id="be211-630">Následující kód nemá úlohy pomocí obou lineární modelů, které jsme mít sestavený.</span><span class="sxs-lookup"><span data-stu-id="be211-630">The following code does the job by using the two linear models we have constructed.</span></span>

    ## Compute the RMS error in a dataframe
    ## Include the row names in the first column so they will
    ## appear in the output of the Execute R Script
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

    ## The following line should be executed only when running in
    ## Azure Machine Learning Studio
    maml.mapOutputPort('RMS.df')

<span data-ttu-id="be211-631">Spuštění tento kód vytvoří výstup ukazuje obrázek 27 na výstupní port datovou sadu výsledků.</span><span class="sxs-lookup"><span data-stu-id="be211-631">Running this code produces the output shown in Figure 27 at the Result Dataset output port.</span></span>

![Porovnání RMS chyb pro modely][26]

<span data-ttu-id="be211-633">*Obrázek 27. Porovnání RMS chyb pro modely.*</span><span class="sxs-lookup"><span data-stu-id="be211-633">*Figure 27. Comparison of RMS errors for the models.*</span></span>

<span data-ttu-id="be211-634">Z těchto výsledků vidíte, že přidávání sezónní faktory do modelu snižuje chyby RMS výrazně.</span><span class="sxs-lookup"><span data-stu-id="be211-634">From these results, we see that adding the seasonal factors to the model reduces the RMS error significantly.</span></span> <span data-ttu-id="be211-635">Chyba služby RMS pro Cvičná data příliš logicky je chvíli menší než prognózy.</span><span class="sxs-lookup"><span data-stu-id="be211-635">Not too surprisingly, the RMS error for the training data is a bit less than for the forecast.</span></span>

## <span data-ttu-id="be211-636"><a id="appendixa"></a>Příloha A: Průvodce Rstudia</span><span class="sxs-lookup"><span data-stu-id="be211-636"><a id="appendixa"></a>APPENDIX A: Guide to RStudio</span></span>
<span data-ttu-id="be211-637">Rstudia je velmi dobře zdokumentovat, tak v tomto dodatku I zajistí některé odkazy na klíče části dokumentace Rstudia, které vám pomůžou začít.</span><span class="sxs-lookup"><span data-stu-id="be211-637">RStudio is quite well documented, so in this appendix I will provide some links to the key sections of the RStudio documentation to get you started.</span></span>

1. <span data-ttu-id="be211-638">Vytváření projektů</span><span class="sxs-lookup"><span data-stu-id="be211-638">Creating projects</span></span>
   
   <span data-ttu-id="be211-639">Můžete uspořádat a spravovat váš kód R do projektů pomocí Rstudia.</span><span class="sxs-lookup"><span data-stu-id="be211-639">You can organize and manage your R code into projects by using RStudio.</span></span> <span data-ttu-id="be211-640">Dokumentace, která používá projekty lze najít na https://support.rstudio.com/hc/articles/200526207-Using-Projects.</span><span class="sxs-lookup"><span data-stu-id="be211-640">The documentation that uses projects can be found at https://support.rstudio.com/hc/articles/200526207-Using-Projects.</span></span>
   
   <span data-ttu-id="be211-641">Doporučujeme I postupujte podle těchto pokynů a vytvoření projektu pro R příklady kódu v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="be211-641">I recommend that you follow these directions and create a project for the R code examples in this document.</span></span>  
2. <span data-ttu-id="be211-642">Úpravy a spouštění kódu jazyka R</span><span class="sxs-lookup"><span data-stu-id="be211-642">Editing and executing R code</span></span>
   
   <span data-ttu-id="be211-643">Rstudia poskytuje integrované prostředí pro úpravy a provádění kódu jazyka R.</span><span class="sxs-lookup"><span data-stu-id="be211-643">RStudio provides an integrated environment for editing and executing R code.</span></span> <span data-ttu-id="be211-644">Dokumentace lze najít na https://support.rstudio.com/hc/articles/200484448-Editing-and-Executing-Code.</span><span class="sxs-lookup"><span data-stu-id="be211-644">Documentation can be found at https://support.rstudio.com/hc/articles/200484448-Editing-and-Executing-Code.</span></span>
3. <span data-ttu-id="be211-645">Ladění</span><span class="sxs-lookup"><span data-stu-id="be211-645">Debugging</span></span>
   
   <span data-ttu-id="be211-646">Rstudia zahrnuje výkonné možnosti ladění.</span><span class="sxs-lookup"><span data-stu-id="be211-646">RStudio includes powerful debugging capabilities.</span></span> <span data-ttu-id="be211-647">Dokumentace pro tyto funkce jsou v https://support.rstudio.com/hc/articles/200713843-Debugging-with-RStudio.</span><span class="sxs-lookup"><span data-stu-id="be211-647">Documentation for these features is at https://support.rstudio.com/hc/articles/200713843-Debugging-with-RStudio.</span></span>
   
   <span data-ttu-id="be211-648">Řešení potíží funkce zarážek jsou popsány v https://support.rstudio.com/hc/articles/200534337-Breakpoint-Troubleshooting.</span><span class="sxs-lookup"><span data-stu-id="be211-648">The breakpoint troubleshooting features are documented at https://support.rstudio.com/hc/articles/200534337-Breakpoint-Troubleshooting.</span></span>

## <span data-ttu-id="be211-649"><a id="appendixb"></a>Příloha B: Další čtení</span><span class="sxs-lookup"><span data-stu-id="be211-649"><a id="appendixb"></a>APPENDIX B: Further reading</span></span>
<span data-ttu-id="be211-650">V tomto kurzu programovací R popisuje základy toho, co je potřeba použít jazyk R s Azure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="be211-650">This R programming tutorial covers the basics of what you need to use the R language with Azure Machine Learning Studio.</span></span> <span data-ttu-id="be211-651">Pokud nejste obeznámeni s R, jsou k dispozici na CRAN dva úvodní informace:</span><span class="sxs-lookup"><span data-stu-id="be211-651">If you are not familiar with R, two introductions are available on CRAN:</span></span>

* <span data-ttu-id="be211-652">R pro začátečníky podle Emmanuel Paradis je vhodná pro spuštění na http://cran.r-project.org/doc/contrib/Paradis-rdebuts_en.pdf.</span><span class="sxs-lookup"><span data-stu-id="be211-652">R for Beginners by Emmanuel Paradis is a good place to start at http://cran.r-project.org/doc/contrib/Paradis-rdebuts_en.pdf.</span></span>  
* <span data-ttu-id="be211-653">Úvod do R n. dokončeno</span><span class="sxs-lookup"><span data-stu-id="be211-653">An Introduction to R by W. N.</span></span> <span data-ttu-id="be211-654">Venables et.</span><span class="sxs-lookup"><span data-stu-id="be211-654">Venables et.</span></span> <span data-ttu-id="be211-655">Al.</span><span class="sxs-lookup"><span data-stu-id="be211-655">al.</span></span> <span data-ttu-id="be211-656">Klient se přepne do trochu další hloubku, http://cran.r-project.org/doc/manuals/R-intro.html.</span><span class="sxs-lookup"><span data-stu-id="be211-656">goes into a bit more depth, at http://cran.r-project.org/doc/manuals/R-intro.html.</span></span>

<span data-ttu-id="be211-657">Neexistují mnoho knih na R, který můžete začít pracovat.</span><span class="sxs-lookup"><span data-stu-id="be211-657">There are many books on R that can help you get started.</span></span> <span data-ttu-id="be211-658">Zde najdete několik, které užitečné:</span><span class="sxs-lookup"><span data-stu-id="be211-658">Here are a few I find useful:</span></span>

* <span data-ttu-id="be211-659">Obrázky R programování: prohlídka z statistické softwaru návrh podle Norman Matloff je vynikající Úvod do programování v jazyce R.</span><span class="sxs-lookup"><span data-stu-id="be211-659">The Art of R Programming: A Tour of Statistical Software Design by Norman Matloff is an excellent introduction to programming in R.</span></span>  
* <span data-ttu-id="be211-660">R kuchařka podle Paul Teetor poskytuje přístup problému a řešení pomocí R.</span><span class="sxs-lookup"><span data-stu-id="be211-660">R Cookbook by Paul Teetor provides a problem and solution approach to using R.</span></span>  
* <span data-ttu-id="be211-661">R v akce Robert Kabacoff je další užitečné úvodní adresáře.</span><span class="sxs-lookup"><span data-stu-id="be211-661">R in Action by Robert Kabacoff is another useful introductory book.</span></span> <span data-ttu-id="be211-662">Stránku rychlý R doprovodné je užitečné prostředek v http://www.statmethods.net/.</span><span class="sxs-lookup"><span data-stu-id="be211-662">The companion Quick R website is a useful resource at http://www.statmethods.net/.</span></span>
* <span data-ttu-id="be211-663">R Inferno podle Patrik popáleniny je překvapivě vážný adresáře, která pracuje s počtem složité a obtížně témata, která může být zjistil při programování v jazyce R. Seznamu je k dispozici zdarma na http://www.burns-stat.com/documents/books/the-r-inferno/.</span><span class="sxs-lookup"><span data-stu-id="be211-663">R Inferno by Patrick Burns is a surprisingly humorous book that deals with a number of tricky and difficult topics that can be encountered when programming in R. The book is available for free at http://www.burns-stat.com/documents/books/the-r-inferno/.</span></span>
* <span data-ttu-id="be211-664">Pokud chcete podrobné informace do Pokročilá témata v R, podívejte se na seznam upřesnit R podle Hadley Wickham.</span><span class="sxs-lookup"><span data-stu-id="be211-664">If you want a deep dive into advanced topics in R, have a look at the book Advanced R by Hadley Wickham.</span></span> <span data-ttu-id="be211-665">Online verze této příručky je k dispozici zdarma http://adv-r.had.co.nz/.</span><span class="sxs-lookup"><span data-stu-id="be211-665">The online version of this book is available for free at http://adv-r.had.co.nz/.</span></span>

<span data-ttu-id="be211-666">Katalog R časové řady balíčků naleznete v zobrazení úlohy CRAN pro analýzu časových řad: http://cran.r-project.org/web/views/TimeSeries.html.</span><span class="sxs-lookup"><span data-stu-id="be211-666">A catalogue of R time series packages can be found in the CRAN Task View for time series analysis: http://cran.r-project.org/web/views/TimeSeries.html.</span></span> <span data-ttu-id="be211-667">Informace o určité časové řady objektu balíčky by měl naleznete v dokumentaci pro tento balíček.</span><span class="sxs-lookup"><span data-stu-id="be211-667">For information on specific time series object packages, you should refer to the documentation for that package.</span></span>

<span data-ttu-id="be211-668">Příručka úvodní časové řady s R Paul Cowpertwait a Andrew Metcalfe obsahuje úvod do používání R pro analýzu časových řad.</span><span class="sxs-lookup"><span data-stu-id="be211-668">The book Introductory Time Series with R by Paul Cowpertwait and Andrew Metcalfe provides an introduction to using R for time series analysis.</span></span> <span data-ttu-id="be211-669">Mnoho více teoretické texty R příklady.</span><span class="sxs-lookup"><span data-stu-id="be211-669">Many more theoretical texts provide R examples.</span></span>

<span data-ttu-id="be211-670">Některé skvělé prostředků z Internetu:</span><span class="sxs-lookup"><span data-stu-id="be211-670">Some great internet resources:</span></span>

* <span data-ttu-id="be211-671">DataCamp: DataCamp učí R v pohodlí prohlížeč s video lekce a kódování cvičení.</span><span class="sxs-lookup"><span data-stu-id="be211-671">DataCamp: DataCamp teaches R in the comfort of your browser with video lessons and coding exercises.</span></span> <span data-ttu-id="be211-672">Existují interaktivní kurzy o nejnovější techniky R a balíčků.</span><span class="sxs-lookup"><span data-stu-id="be211-672">There are interactive tutorials on the latest R techniques and packages.</span></span> <span data-ttu-id="be211-673">Přijmout volné Interaktivní kurz R na https://www.datacamp.com/courses/introduction-to-r</span><span class="sxs-lookup"><span data-stu-id="be211-673">Take the free interactive R tutorial at https://www.datacamp.com/courses/introduction-to-r</span></span>
* <span data-ttu-id="be211-674">Průvodce Začínáme pracovat s R z Programiz https://www.programiz.com/r-programming</span><span class="sxs-lookup"><span data-stu-id="be211-674">A guide on Getting started with R from Programiz https://www.programiz.com/r-programming</span></span>
* <span data-ttu-id="be211-675">Rychlý kurz R podle Jan černé z Clarkson univerzity http://www.cyclismo.org/tutorial/R/</span><span class="sxs-lookup"><span data-stu-id="be211-675">A quick R tutorial by Kelly Black from Clarkson University http://www.cyclismo.org/tutorial/R/</span></span>
* <span data-ttu-id="be211-676">60 + R prostředky uvedené v http://www.computerworld.com/article/2497464/business-intelligence-60-r-resources-to-improve-your-data-skills.html</span><span class="sxs-lookup"><span data-stu-id="be211-676">60+ R resources listed at http://www.computerworld.com/article/2497464/business-intelligence-60-r-resources-to-improve-your-data-skills.html</span></span>

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
