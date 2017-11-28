---
title: "aaaAuthor vlastních modulů R v Azure Machine Learning | Microsoft Docs"
description: "Rychlý start pro vytváření vlastních modulů R v Azure Machine Learning."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 6cbc628a-7e60-42ce-9f90-20aaea7ba630
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 03/24/2017
ms.author: bradsev;ankarlof
ms.openlocfilehash: 8007c2abe20a4ab990f38b6d09bc4e6834ad2082
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="author-custom-r-modules-in-azure-machine-learning"></a><span data-ttu-id="7b1aa-103">Vytváření vlastních modulů R ve službě Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="7b1aa-103">Author custom R modules in Azure Machine Learning</span></span>
<span data-ttu-id="7b1aa-104">Toto téma popisuje, jak tooauthor a nasadit vlastní modul R v Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-104">This topic describes how tooauthor and deploy a custom R module in Azure Machine Learning.</span></span> <span data-ttu-id="7b1aa-105">Vysvětluje, co jsou vlastních modulů R a jaké soubory jsou použité toodefine je.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-105">It explains what custom R modules are and what files are used toodefine them.</span></span> <span data-ttu-id="7b1aa-106">Ukazuje, jak tooconstruct hello soubory, které definují modul a jak tooregister hello modul pro nasazení v pracovním prostoru Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-106">It illustrates how tooconstruct hello files that define a module and how tooregister hello module for deployment in a Machine Learning workspace.</span></span> <span data-ttu-id="7b1aa-107">Hello elementy a atributy použité v definici hello vlastní modul hello jsou pak podrobněji popsané v.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-107">hello elements and attributes used in hello definition of hello custom module are then described in more detail.</span></span> <span data-ttu-id="7b1aa-108">Jak toouse pomocné funkce a soubory a několik výstupů také popsané.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-108">How toouse auxiliary functionality and files and multiple outputs is also discussed.</span></span> 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="what-is-a-custom-r-module"></a><span data-ttu-id="7b1aa-109">Co je vlastní modul R?</span><span class="sxs-lookup"><span data-stu-id="7b1aa-109">What is a custom R module?</span></span>
<span data-ttu-id="7b1aa-110">A **vlastní modul** je modul definovaný uživatelem, který může být nahrán tooyour prostoru a provést v rámci experimentu Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-110">A **custom module** is a user-defined module that can be uploaded tooyour workspace and executed as part of an Azure Machine Learning experiment.</span></span> <span data-ttu-id="7b1aa-111">A **vlastní modul R** je vlastní modul, který provede uživatelsky definované funkce R.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-111">A **custom R module** is a custom module that executes a user-defined R function.</span></span> <span data-ttu-id="7b1aa-112">**R** je programovací jazyk pro statistické výpočty a obrázky, které se často používá podle vědců statistikami a dat pro implementaci algoritmy.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-112">**R** is a programming language for statistical computing and graphics that is widely used by statisticians and data scientists for implementing algorithms.</span></span> <span data-ttu-id="7b1aa-113">V současné době R je jediným podporovaným v vlastní moduly, ale podpory pro další jazyky je naplánována na budoucích verzích jazykem hello.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-113">Currently, R is hello only language supported in custom modules, but support for additional languages is scheduled for future releases.</span></span>

<span data-ttu-id="7b1aa-114">Vlastní moduly mají **prvotřídní stav** v Azure Machine Learning v hello smyslu, že je lze použít stejně jako ostatní moduly.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-114">Custom modules have **first-class status** in Azure Machine Learning in hello sense that they can be used just like any other module.</span></span> <span data-ttu-id="7b1aa-115">Mohou být provedeny další moduly zahrnuté v publikované experimenty nebo vizualizace.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-115">They can be executed with other modules, included in published experiments or in visualizations.</span></span> <span data-ttu-id="7b1aa-116">Mít kontrolu nad hello algoritmus implementovaný modulem hello, hello používá toobe vstupní a výstupní porty, hello modelování parametry a další různé chování za běhu.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-116">You have control over hello algorithm implemented by hello module, hello input and output ports toobe used, hello modeling parameters, and other various runtime behaviors.</span></span> <span data-ttu-id="7b1aa-117">Experimentu, který obsahuje vlastní moduly můžete také publikovat do hello Cortana Intelligence Gallery pro snadné sdílení.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-117">An experiment that contains custom modules can also be published into hello Cortana Intelligence Gallery for easy sharing.</span></span>

## <a name="files-in-a-custom-r-module"></a><span data-ttu-id="7b1aa-118">Soubory ve vlastní modul R</span><span class="sxs-lookup"><span data-stu-id="7b1aa-118">Files in a custom R module</span></span>
<span data-ttu-id="7b1aa-119">Vlastní modul R je definováno soubor .zip, který obsahuje minimálně dva soubory:</span><span class="sxs-lookup"><span data-stu-id="7b1aa-119">A custom R module is defined by a .zip file that contains, at a minimum, two files:</span></span>

* <span data-ttu-id="7b1aa-120">A **zdrojový soubor** , která implementuje funkce hello R vystavené hello modulu</span><span class="sxs-lookup"><span data-stu-id="7b1aa-120">A **source file** that implements hello R function exposed by hello module</span></span>
* <span data-ttu-id="7b1aa-121">**Soubor definice XML** rozhraní hello vlastní modul, který popisuje</span><span class="sxs-lookup"><span data-stu-id="7b1aa-121">An **XML definition file** that describes hello custom module interface</span></span>

<span data-ttu-id="7b1aa-122">Další pomocné soubory můžou být součástí hello souboru .zip, který poskytuje funkce, která je přístupná z vlastní modul hello.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-122">Additional auxiliary files can also be included in hello .zip file that provides functionality that can be accessed from hello custom module.</span></span> <span data-ttu-id="7b1aa-123">Tato možnost je podrobněji hello **argumenty** součástí hello referenčním oddílu **elementů v souboru definice XML hello** následující ukázka hello rychlý start.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-123">This option is discussed in hello **Arguments** part of hello reference section **Elements in hello XML definition file** following hello quickstart example.</span></span>

## <a name="quickstart-example-define-package-and-register-a-custom-r-module"></a><span data-ttu-id="7b1aa-124">Příklad rychlý start: Definujte, balíčků a zaregistrovat vlastní modul R</span><span class="sxs-lookup"><span data-stu-id="7b1aa-124">Quickstart example: define, package, and register a custom R module</span></span>
<span data-ttu-id="7b1aa-125">Tento příklad ukazuje, jak tooconstruct hello soubory vyžadované vlastní modul R, zabalit do souboru zip a pak zaregistrovat hello modulu v pracovním prostoru Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-125">This example illustrates how tooconstruct hello files required by a custom R module, package them into a zip file, and then register hello module in your Machine Learning workspace.</span></span> <span data-ttu-id="7b1aa-126">Hello příklad zip balíčku a ukázkové soubory si můžete stáhnout z [CustomAddRows.zip stáhnout soubor](http://go.microsoft.com/fwlink/?LinkID=524916&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="7b1aa-126">hello example zip package and sample files can be downloaded from [Download CustomAddRows.zip file](http://go.microsoft.com/fwlink/?LinkID=524916&clcid=0x409).</span></span>

## <a name="hello-source-file"></a><span data-ttu-id="7b1aa-127">zdrojový soubor Hello</span><span class="sxs-lookup"><span data-stu-id="7b1aa-127">hello source file</span></span>
<span data-ttu-id="7b1aa-128">Zvažte příklad hello **řádky přidat vlastní** modul, který upravuje hello standardní implementace hello **přidat řádky** modul používá tooconcatenate řádky (připomínky) z dvě datové sady (datových rámců).</span><span class="sxs-lookup"><span data-stu-id="7b1aa-128">Consider hello example of a **Custom Add Rows** module that modifies hello standard implementation of hello **Add Rows** module used tooconcatenate rows (observations) from two datasets (data frames).</span></span> <span data-ttu-id="7b1aa-129">Hello standardní **přidat řádky** modulu připojí hello řádky hello druhý vstupní datové sady toohello konce hello první vstupní datové sady pomocí hello `rbind` algoritmus.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-129">hello standard **Add Rows** module appends hello rows of hello second input dataset toohello end of hello first input dataset using hello `rbind` algorithm.</span></span> <span data-ttu-id="7b1aa-130">Hello přizpůsobit `CustomAddRows` funkce podobně přijímá dvě datové sady, ale také přijme parametr Boolean odkládacího souboru jako další vstup.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-130">hello customized `CustomAddRows` function similarly accepts two datasets, but also accepts a Boolean swap parameter as an additional input.</span></span> <span data-ttu-id="7b1aa-131">Pokud je parametr swap hello nastaven příliš**FALSE**, se vrátí hello sada dat jako hello standardní implementace.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-131">If hello swap parameter is set too**FALSE**, it returns hello same data set as hello standard implementation.</span></span> <span data-ttu-id="7b1aa-132">Ale pokud je parametr swap hello **TRUE**, funkce hello připojí řádky první vstupní datové sady toohello konce druhý datovou sadu hello místo.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-132">But if hello swap parameter is **TRUE**, hello function appends rows of first input dataset toohello end of hello second dataset instead.</span></span> <span data-ttu-id="7b1aa-133">Hello CustomAddRows.R soubor, který obsahuje hello implementace hello R `CustomAddRows` funkce vystavené hello **řádky přidat vlastní** modul má následující kód R hello.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-133">hello CustomAddRows.R file that contains hello implementation of hello R `CustomAddRows` function exposed by hello **Custom Add Rows** module has hello following R code.</span></span>

    CustomAddRows <- function(dataset1, dataset2, swap=FALSE) 
    {
        if (swap)
        {
            return (rbind(dataset2, dataset1));
        }
        else
        {
            return (rbind(dataset1, dataset2));
        } 
    } 

### <a name="hello-xml-definition-file"></a><span data-ttu-id="7b1aa-134">Soubor definice XML Hello</span><span class="sxs-lookup"><span data-stu-id="7b1aa-134">hello XML definition file</span></span>
<span data-ttu-id="7b1aa-135">tooexpose to `CustomAddRows` funkce jako modul služby Azure Machine Learning, soubor definice XML musí být vytvořen toospecify jak hello **řádky přidat vlastní** modul by měl vzhled a chování.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-135">tooexpose this `CustomAddRows` function as an Azure Machine Learning module, an XML definition file must be created toospecify how hello **Custom Add Rows** module should look and behave.</span></span> 

    <!-- Defined a module using an R Script -->
    <Module name="Custom Add Rows">
        <Owner>Microsoft Corporation</Owner>
        <Description>Appends one dataset tooanother. Dataset 2 is concatenated tooDataset 1 when Swap is FALSE, and vice versa when Swap is TRUE.</Description>

    <!-- Specify hello base language, script file and R function toouse for this module. -->        
        <Language name="R" 
         sourceFile="CustomAddRows.R" 
         entryPoint="CustomAddRows" />  

    <!-- Define module input and output ports -->
    <!-- Note: hello values of hello id attributes in hello Input and Arg elements must match hello parameter names in hello R Function CustomAddRows defined in CustomAddRows.R. -->
        <Ports>
            <Input id="dataset1" name="Dataset 1" type="DataTable">
                <Description>First input dataset</Description>
            </Input>
            <Input id="dataset2" name="Dataset 2" type="DataTable">
                <Description>Second input dataset</Description>
            </Input>
            <Output id="dataset" name="Dataset" type="DataTable">
                <Description>hello combined dataset</Description>
            </Output>
        </Ports>

    <!-- Define module parameters -->
        <Arguments>
            <Arg id="swap" name="Swap" type="bool" >
                <Description>Swap input datasets.</Description>
            </Arg>
        </Arguments>
    </Module>


<span data-ttu-id="7b1aa-136">Je důležité toonote, který hello hodnotu hello **id** atributy hello **vstup** a **Arg** elementů v souboru XML hello musí odpovídat názvy parametrů funkce hello hello R kód v souboru CustomAddRows.R hello přesně: (*dataset1*, *dataset2*, a *swap* v příkladu hello).</span><span class="sxs-lookup"><span data-stu-id="7b1aa-136">It is critical toonote that hello value of hello **id** attributes of hello **Input** and **Arg** elements in hello XML file must match hello function parameter names of hello R code in hello CustomAddRows.R file EXACTLY: (*dataset1*, *dataset2*, and *swap* in hello example).</span></span> <span data-ttu-id="7b1aa-137">Podobně hello hodnotu hello **entryPoint** atribut hello **jazyk** element musí přesně shodovat hello název funkce hello ve skriptu hello R: (*CustomAddRows* v příkladu hello).</span><span class="sxs-lookup"><span data-stu-id="7b1aa-137">Similarly, hello value of hello **entryPoint** attribute of hello **Language** element must match hello name of hello function in hello R script EXACTLY: (*CustomAddRows* in hello example).</span></span> 

<span data-ttu-id="7b1aa-138">Naproti tomu hello **id** atribut pro hello **výstup** element neodpovídá tooany proměnné ve skriptu hello R.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-138">In contrast, hello **id** attribute for hello **Output** element does not correspond tooany variables in hello R script.</span></span> <span data-ttu-id="7b1aa-139">Když se vyžaduje více než jeden výstup, jednoduše vrácení seznamu z funkce hello R s výsledky umístit *v hello stejné pořadí* jako **výstupy** elementy jsou deklarované v souboru XML hello.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-139">When more than one output is required, simply return a list from hello R function with results placed *in hello same order* as **Outputs** elements are declared in hello XML file.</span></span>

### <a name="package-and-register-hello-module"></a><span data-ttu-id="7b1aa-140">Balíček a registrace modulu hello</span><span class="sxs-lookup"><span data-stu-id="7b1aa-140">Package and register hello module</span></span>
<span data-ttu-id="7b1aa-141">Uložit tyto dva soubory jako *CustomAddRows.R* a *CustomAddRows.xml* a pak zip hello dva soubory společně na *CustomAddRows.zip* souboru.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-141">Save these two files as *CustomAddRows.R* and *CustomAddRows.xml* and then zip hello two files together into a *CustomAddRows.zip* file.</span></span>

<span data-ttu-id="7b1aa-142">je v pracovním prostoru Machine Learning, přejděte tooyour pracovního prostoru v hello Machine Learning Studio, klikněte na tlačítko hello tooregister **+ nový** tlačítko hello dolní a zvolte **modulu -> z balíček ZIP** tooupload Hello nové **řádky přidat vlastní** modulu.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-142">tooregister them in your Machine Learning workspace, go tooyour workspace in hello Machine Learning Studio, click hello **+NEW** button on hello bottom and choose **MODULE -> FROM ZIP PACKAGE** tooupload hello new **Custom Add Rows** module.</span></span>

![Nahrát Zip](./media/machine-learning-custom-r-modules/upload-from-zip-package.png)

<span data-ttu-id="7b1aa-144">Hello **řádky přidat vlastní** modul je nyní připraven toobe přístup experimentů Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-144">hello **Custom Add Rows** module is now ready toobe accessed by your Machine Learning experiments.</span></span>

## <a name="elements-in-hello-xml-definition-file"></a><span data-ttu-id="7b1aa-145">Elementy v souboru definice XML hello</span><span class="sxs-lookup"><span data-stu-id="7b1aa-145">Elements in hello XML definition file</span></span>
### <a name="module-elements"></a><span data-ttu-id="7b1aa-146">Modul elementy</span><span class="sxs-lookup"><span data-stu-id="7b1aa-146">Module elements</span></span>
<span data-ttu-id="7b1aa-147">Hello **modulu** element je použité toodefine vlastní modul v souboru XML hello.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-147">hello **Module** element is used toodefine a custom module in hello XML file.</span></span> <span data-ttu-id="7b1aa-148">Více modulů lze definovat v jednom souboru XML pomocí několika **modulu** elementy.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-148">Multiple modules can be defined in one XML file using multiple **module** elements.</span></span> <span data-ttu-id="7b1aa-149">Každý modulu v pracovním prostoru musí mít jedinečný název.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-149">Each module in your workspace must have a unique name.</span></span> <span data-ttu-id="7b1aa-150">Registrovat vlastní modul s hello stejný název jako existující vlastní modul a jeho nahradí existující modul hello hello nový.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-150">Register a custom module with hello same name as an existing custom module and it replaces hello existing module with hello new one.</span></span> <span data-ttu-id="7b1aa-151">Vlastní moduly lze však zaregistrována hello stejný název jako existující modul Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-151">Custom modules can, however, be registered with hello same name as an existing Azure Machine Learning module.</span></span> <span data-ttu-id="7b1aa-152">Pokud ano, se objeví v hello **vlastní** kategorii palety modulů hello.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-152">If so, they appear in hello **Custom** category of hello module palette.</span></span>

    <Module name="Custom Add Rows" isDeterministic="false"> 
        <Owner>Microsoft Corporation</Owner>
        <Description>Appends one dataset tooanother...</Description>/> 


<span data-ttu-id="7b1aa-153">V rámci hello **modulu** elementu, můžete určit dva další volitelné prvky:</span><span class="sxs-lookup"><span data-stu-id="7b1aa-153">Within hello **Module** element, you can specify two additional optional elements:</span></span>

* <span data-ttu-id="7b1aa-154">**vlastníka** element, který se vloží do modulu hello</span><span class="sxs-lookup"><span data-stu-id="7b1aa-154">an **Owner** element that is embedded into hello module</span></span>  
* <span data-ttu-id="7b1aa-155">**popis** element, který obsahuje text, který se zobrazí v rychlé nápovědy pro modul hello a pokud ponecháte hello modulu v hello Machine Learning uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-155">a **Description** element that contains text that is displayed in quick help for hello module and when you hover over hello module in hello Machine Learning UI.</span></span>

<span data-ttu-id="7b1aa-156">Pravidla pro omezení znaků v elementech modulu hello:</span><span class="sxs-lookup"><span data-stu-id="7b1aa-156">Rules for characters limits in hello Module elements:</span></span>

* <span data-ttu-id="7b1aa-157">Hello hodnotu hello **název** atribut v hello **modulu** element nesmí být delší než 64 znaků.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-157">hello value of hello **name** attribute in hello **Module** element must not exceed 64 characters in length.</span></span> 
* <span data-ttu-id="7b1aa-158">Hello obsah hello **popis** element nesmí být delší než 128 znaků.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-158">hello content of hello **Description** element must not exceed 128 characters in length.</span></span>
* <span data-ttu-id="7b1aa-159">Hello obsah hello **vlastníka** element nesmí být delší než 32 znaků.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-159">hello content of hello **Owner** element must not exceed 32 characters in length.</span></span>

<span data-ttu-id="7b1aa-160">Výsledky modulu může být deterministický nebo nondeterministic.* * ve výchozím nastavení, všechny moduly jsou považovány za toobe deterministický.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-160">A module's results can be deterministic or nondeterministic.** By default, all modules are considered toobe deterministic.</span></span> <span data-ttu-id="7b1aa-161">To znamená, zadána neměnné sadu vstupní parametry a data modulu hello by měl vrátit hello stejné výsledky eacRAND nebo functionh čas, který je spuštěn.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-161">That is, given an unchanging set of input parameters and data, hello module should return hello same results eacRAND or a functionh time it is run.</span></span> <span data-ttu-id="7b1aa-162">Vzhledem k tomuto chování, Azure Machine Learning Studio pouze opakovaně moduly, které jsou označeny jako deterministický, pokud parametr nebo hello vstupní data změnila.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-162">Given this behavior, Azure Machine Learning Studio only reruns modules marked as deterministic if a parameter or hello input data has changed.</span></span> <span data-ttu-id="7b1aa-163">Vrací výsledky do mezipaměti hello taky poskytuje mnohem rychlejší provádění experimenty.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-163">Returning hello cached results also provides much faster execution of experiments.</span></span>

<span data-ttu-id="7b1aa-164">Jsou funkce, které jsou deterministický, jako je například rand – nebo funkci, která vrátí hello aktuální datum nebo čas.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-164">There are functions that are nondeterministic, such as RAND or a function that returns hello current date or time.</span></span> <span data-ttu-id="7b1aa-165">Pokud modul používá nedeterministická funkce, můžete určit, že hello modul bude nedeterministickou podle nastavení hello volitelné **isDeterministic** atribut příliš**FALSE**.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-165">If your module uses a nondeterministic function, you can specify that hello module is non-deterministic by setting hello optional **isDeterministic** attribute too**FALSE**.</span></span> <span data-ttu-id="7b1aa-166">Díky tomu se budou, že Tenhle modul hello se znovu spustí při každém spuštění experimentu hello i nezměněné hello vstup modulu a parametry.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-166">This insures that hello module is rerun whenever hello experiment is run, even if hello module input and parameters have not changed.</span></span> 

### <a name="language-definition"></a><span data-ttu-id="7b1aa-167">Definice jazyka</span><span class="sxs-lookup"><span data-stu-id="7b1aa-167">Language Definition</span></span>
<span data-ttu-id="7b1aa-168">Hello **jazyk** element v souboru definice XML je použité toospecify hello vlastní modul jazyk.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-168">hello **Language** element in your XML definition file is used toospecify hello custom module language.</span></span> <span data-ttu-id="7b1aa-169">V současné době R je hello pouze podporovaný jazyk.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-169">Currently, R is hello only supported language.</span></span> <span data-ttu-id="7b1aa-170">Hello hodnotu hello **zdrojový soubor** atributu musí být název hello hello R souboru, který obsahuje hello funkce toocall při spuštění modulu hello.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-170">hello value of hello **sourceFile** attribute must be hello name of hello R file that contains hello function toocall when hello module is run.</span></span> <span data-ttu-id="7b1aa-171">Tento soubor musí být součástí hello zip balíčku.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-171">This file must be part of hello zip package.</span></span> <span data-ttu-id="7b1aa-172">Hello hodnotu hello **entryPoint** atribut je název hello hello funkce volané a platný funkci definovanou s v hello zdrojový soubor se musí shodovat.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-172">hello value of hello **entryPoint** attribute is hello name of hello function being called and must match a valid function defined with in hello source file.</span></span>

    <Language name="R" sourceFile="CustomAddRows.R" entryPoint="CustomAddRows" />


### <a name="ports"></a><span data-ttu-id="7b1aa-173">Porty</span><span class="sxs-lookup"><span data-stu-id="7b1aa-173">Ports</span></span>
<span data-ttu-id="7b1aa-174">Hello vstupní a výstupní porty pro vlastní modul jsou určené v podřízených elementů hello **porty** část definičního soubor XML hello.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-174">hello input and output ports for a custom module are specified in child elements of hello **Ports** section of hello XML definition file.</span></span> <span data-ttu-id="7b1aa-175">Určuje pořadí Hello tyto prvky hello rozložení zkušeného (UX) uživatelé.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-175">hello order of these elements determines hello layout experienced (UX) by users.</span></span> <span data-ttu-id="7b1aa-176">prvním podřízeným objektem Hello **vstupní** nebo **výstup** uvedené v hello **porty** element souboru XML hello stane hello nejvíce vlevo vstupní port v hello Machine Learning UX</span><span class="sxs-lookup"><span data-stu-id="7b1aa-176">hello first child **input** or **output** listed in hello **Ports** element of hello XML file becomes hello left-most input port in hello Machine Learning UX.</span></span>
<span data-ttu-id="7b1aa-177">Každý vstupní a výstupní port je pravděpodobně volitelný **popis** podřízený element, který určuje textu hello zobrazí, když najedete myší hello hello port v hello Machine Learning uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-177">Each input and output port may have an optional **Description** child element that specifies hello text shown when you hover hello mouse cursor over hello port in hello Machine Learning UI.</span></span>

<span data-ttu-id="7b1aa-178">**Porty pravidla**:</span><span class="sxs-lookup"><span data-stu-id="7b1aa-178">**Ports Rules**:</span></span>

* <span data-ttu-id="7b1aa-179">Maximální počet **vstupní a výstupní porty** je 8 pro každou.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-179">Maximum number of **input and output ports** is 8 for each.</span></span>

### <a name="input-elements"></a><span data-ttu-id="7b1aa-180">Elementy vstupu</span><span class="sxs-lookup"><span data-stu-id="7b1aa-180">Input elements</span></span>
<span data-ttu-id="7b1aa-181">Vstupní porty povolí funkci tooyour R toopass dat a pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-181">Input ports allow you toopass data tooyour R function and workspace.</span></span> <span data-ttu-id="7b1aa-182">Hello **datové typy** jsou podporovány pro vstupní porty jsou následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="7b1aa-182">hello **data types** that are supported for input ports are as follows:</span></span> 

<span data-ttu-id="7b1aa-183">**DataTable:** tento typ je předán tooyour R funkce jako data.frame.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-183">**DataTable:** This type is passed tooyour R function as a data.frame.</span></span> <span data-ttu-id="7b1aa-184">Ve skutečnosti všechny typy (například CSV soubory nebo soubory ARFF), které jsou podporovány nástrojem Machine Learning a že jsou kompatibilní s **DataTable** jsou převedené tooa data.frame automaticky.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-184">In fact, any types (for example, CSV files or ARFF files) that are supported by Machine Learning and that are compatible with **DataTable** are converted tooa data.frame automatically.</span></span> 

        <Input id="dataset1" name="Input 1" type="DataTable" isOptional="false">
            <Description>Input Dataset 1</Description>
           </Input>

<span data-ttu-id="7b1aa-185">Hello **id** atribut spojený s každou **DataTable** vstupní port musí mít jedinečnou hodnotu a tato hodnota musí odpovídat jeho odpovídající parametr v R funkce s názvem.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-185">hello **id** attribute associated with each **DataTable** input port must have a unique value and this value must match its corresponding named parameter in your R function.</span></span>
<span data-ttu-id="7b1aa-186">Volitelné **DataTable** porty, které nejsou předány jako vstup v experimentu mít hodnotu hello **NULL** funkce předaný toohello R a volitelné zip porty jsou ignorovány, pokud hello vstup není připojen.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-186">Optional **DataTable** ports that are not passed as input in an experiment have hello value **NULL** passed toohello R function and optional zip ports are ignored if hello input is not connected.</span></span> <span data-ttu-id="7b1aa-187">Hello **isOptional** atribut je volitelný pro obě hello **DataTable** a **Zip** typy a je *false* ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-187">hello **isOptional** attribute is optional for both hello **DataTable** and **Zip** types and is *false* by default.</span></span>

<span data-ttu-id="7b1aa-188">**ZIP:** vlastní moduly může přijmout soubor zip jako vstup.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-188">**Zip:** Custom modules can accept a zip file as input.</span></span> <span data-ttu-id="7b1aa-189">Tento vstup je vybaleno do hello R pracovní adresář vaší – funkce</span><span class="sxs-lookup"><span data-stu-id="7b1aa-189">This input is unpacked into hello R working directory of your function</span></span>

        <Input id="zippedData" name="Zip Input" type="Zip" IsOptional="false">
            <Description>Zip files toobe extracted toohello R working directory.</Description>
           </Input>

<span data-ttu-id="7b1aa-190">Pro vlastních modulů R hello id pro Zip port nemá toomatch žádné parametry hello R funkce.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-190">For custom R modules, hello id for a Zip port does not have toomatch any parameters of hello R function.</span></span> <span data-ttu-id="7b1aa-191">Je to proto, že je soubor zip hello automaticky extrahované toohello R pracovní adresář.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-191">This is because hello zip file is automatically extracted toohello R working directory.</span></span>

<span data-ttu-id="7b1aa-192">**Vstupní pravidla:**</span><span class="sxs-lookup"><span data-stu-id="7b1aa-192">**Input Rules:**</span></span>

* <span data-ttu-id="7b1aa-193">Hello hodnotu hello **id** atribut hello **vstup** element musí být platný název proměnné R.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-193">hello value of hello **id** attribute of hello **Input** element must be a valid R variable name.</span></span>
* <span data-ttu-id="7b1aa-194">Hello hodnotu hello **id** atribut hello **vstup** element nesmí být delší než 64 znaků.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-194">hello value of hello **id** attribute of hello **Input** element must not be longer than 64 characters.</span></span>
* <span data-ttu-id="7b1aa-195">Hello hodnotu hello **název** atribut hello **vstup** element nesmí být delší než 64 znaků.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-195">hello value of hello **name** attribute of hello **Input** element must not be longer than 64 characters.</span></span>
* <span data-ttu-id="7b1aa-196">Hello obsah hello **popis** element nesmí být delší než 128 znaků.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-196">hello content of hello **Description** element must not be longer than 128 characters</span></span>
* <span data-ttu-id="7b1aa-197">Hello hodnotu hello **typ** atribut hello **vstup** element musí být *Zip* nebo *DataTable*.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-197">hello value of hello **type** attribute of hello **Input** element must be *Zip* or *DataTable*.</span></span>
* <span data-ttu-id="7b1aa-198">Hello hodnotu hello **isOptional** atribut hello **vstup** element není požadovaná (a je *false* ve výchozím nastavení není-li zadána); ale pokud je zadán, musí být *true* nebo *false*.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-198">hello value of hello **isOptional** attribute of hello **Input** element is not required (and is *false* by default when not specified); but if it is specified, it must be *true* or *false*.</span></span>

### <a name="output-elements"></a><span data-ttu-id="7b1aa-199">Výstup elementy</span><span class="sxs-lookup"><span data-stu-id="7b1aa-199">Output elements</span></span>
<span data-ttu-id="7b1aa-200">**Standardní výstupní porty:** výstupních portů jsou namapované toohello návratové hodnoty z funkce R, který můžete použít následující moduly.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-200">**Standard output ports:** Output ports are mapped toohello return values from your R function, which can then be used by subsequent modules.</span></span> <span data-ttu-id="7b1aa-201">*DataTable* hello pouze standardní výstupní port typu v současné době podporován.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-201">*DataTable* is hello only standard output port type supported currently.</span></span> <span data-ttu-id="7b1aa-202">(Podpora pro *inteligentních algoritmů* a *transformuje* je chystaný.) A *DataTable* výstup je definován jako:</span><span class="sxs-lookup"><span data-stu-id="7b1aa-202">(Support for *Learners* and *Transforms* is forthcoming.) A *DataTable* output is defined as:</span></span>

    <Output id="dataset" name="Dataset" type="DataTable">
        <Description>Combined dataset</Description>
    </Output>

<span data-ttu-id="7b1aa-203">Výstupy ve vlastních modulů R text hello hodnotu hello **id** atribut nemá toocorrespond s nic v hello R skript, ale musí být jedinečný.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-203">For outputs in custom R modules, hello value of hello **id** attribute does not have toocorrespond with anything in hello R script, but it must be unique.</span></span> <span data-ttu-id="7b1aa-204">Pro jeden modul výstupu, musí být hello návratovou hodnotou z funkce hello R *data.frame*.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-204">For a single module output, hello return value from hello R function must be a *data.frame*.</span></span> <span data-ttu-id="7b1aa-205">V pořadí toooutput více než jeden objekt podporovaného typu, hello odpovídající výstupních portů musíte toobe zadaný v souboru definice XML hello a objekty hello musí toobe vrácena jako seznam.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-205">In order toooutput more than one object of a supported data type, hello appropriate output ports need toobe specified in hello XML definition file and hello objects need toobe returned as a list.</span></span> <span data-ttu-id="7b1aa-206">objekty výstup Hello přiřazené toooutput porty z levé tooright, které odráží hello pořadí, ve kterém jsou umístěny hello objekty v hello vrátil seznam.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-206">hello output objects are assigned toooutput ports from left tooright, reflecting hello order in which hello objects are placed in hello returned list.</span></span>

<span data-ttu-id="7b1aa-207">Například, pokud chcete, aby toomodify hello **řádky přidat vlastní** toooutput modulu hello původní dvě datové sady, *dataset1* a *dataset2*, kromě připojený k nové toohello datové sady, *datovou sadu*, (v pořadí, z levé tooright jako: *datovou sadu*, *dataset1*, *dataset2*), pak zadejte hello výstup porty v souboru CustomAddRows.xml hello následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="7b1aa-207">For example, if you want toomodify hello **Custom Add Rows** module toooutput hello original two datasets, *dataset1* and *dataset2*, in addition toohello new joined dataset, *dataset*, (in an order, from left tooright, as: *dataset*, *dataset1*, *dataset2*), then define hello output ports in hello CustomAddRows.xml file as follows:</span></span>

    <Ports> 
        <Output id="dataset" name="Dataset Out" type="DataTable"> 
            <Description>New Dataset</Description> 
        </Output> 
        <Output id="dataset1_out" name="Dataset 1 Out" type="DataTable"> 
            <Description>First Dataset</Description> 
        </Output> 
        <Output id="dataset2_out" name="Dataset 2 Out" type="DataTable"> 
            <Description>Second Dataset</Description> 
        </Output> 
        <Input id="dataset1" name="Dataset 1" type="DataTable"> 
            <Description>First Input Table</Description>
        </Input> 
        <Input id="dataset2" name="Dataset 2" type="DataTable"> 
            <Description>Second Input Table</Description> 
        </Input> 
    </Ports> 


<span data-ttu-id="7b1aa-208">A vrátí hello seznam objektů v seznamu ve správném pořadí hello v 'CustomAddRows.R':</span><span class="sxs-lookup"><span data-stu-id="7b1aa-208">And return hello list of objects in a list in hello correct order in ‘CustomAddRows.R’:</span></span>

    CustomAddRows <- function(dataset1, dataset2, swap=FALSE) { 
        if (swap) { dataset <- rbind(dataset2, dataset1)) } 
        else { dataset <- rbind(dataset1, dataset2)) 
        } 
    return (list(dataset, dataset1, dataset2)) 
    } 

<span data-ttu-id="7b1aa-209">**Vizualizace výstup:** můžete také určit na výstupní port typu *vizualizace*, které zobrazí výstup hello hello R grafiky zařízení a konzoly výstup.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-209">**Visualization output:** You can also specify an output port of type *Visualization*, which displays hello output from hello R graphics device and console output.</span></span> <span data-ttu-id="7b1aa-210">Tento port není součástí funkce výstup hello R a nebudou v konfliktu s hello pořadí hello další výstup typy portu.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-210">This port is not part of hello R function output and does not interfere with hello order of hello other output port types.</span></span> <span data-ttu-id="7b1aa-211">tooadd vizualizace port toohello vlastní moduly, přidejte **výstup** element s hodnotou *vizualizace* pro jeho **typ** atribut:</span><span class="sxs-lookup"><span data-stu-id="7b1aa-211">tooadd a visualization port toohello custom modules, add an **Output** element with a value of *Visualization* for its **type** attribute:</span></span>

    <Output id="deviceOutput" name="View Port" type="Visualization">
      <Description>View hello R console graphics device output.</Description>
    </Output>

<span data-ttu-id="7b1aa-212">**Výstup pravidla:**</span><span class="sxs-lookup"><span data-stu-id="7b1aa-212">**Output Rules:**</span></span>

* <span data-ttu-id="7b1aa-213">Hello hodnotu hello **id** atribut hello **výstup** element musí být platný název proměnné R.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-213">hello value of hello **id** attribute of hello **Output** element must be a valid R variable name.</span></span>
* <span data-ttu-id="7b1aa-214">Hello hodnotu hello **id** atribut hello **výstup** element nesmí být delší než 32 znaků.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-214">hello value of hello **id** attribute of hello **Output** element must not be longer than 32 characters.</span></span>
* <span data-ttu-id="7b1aa-215">Hello hodnotu hello **název** atribut hello **výstup** element nesmí být delší než 64 znaků.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-215">hello value of hello **name** attribute of hello **Output** element must not be longer than 64 characters.</span></span>
* <span data-ttu-id="7b1aa-216">Hello hodnotu hello **typ** atribut hello **výstup** element musí být *vizualizace*.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-216">hello value of hello **type** attribute of hello **Output** element must be *Visualization*.</span></span>

### <a name="arguments"></a><span data-ttu-id="7b1aa-217">Argumenty</span><span class="sxs-lookup"><span data-stu-id="7b1aa-217">Arguments</span></span>
<span data-ttu-id="7b1aa-218">Další data mohou být předána toohello R funkce prostřednictvím modulu parametry, které jsou definovány v hello **argumenty** elementu.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-218">Additional data can be passed toohello R function via module parameters which are defined in hello **Arguments** element.</span></span> <span data-ttu-id="7b1aa-219">Tyto parametry se zobrazí v podokně nejvíce vpravo vlastnosti hello hello Machine Learning uživatelského rozhraní při vybrání hello modulu.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-219">These parameters appear in hello rightmost properties pane of hello Machine Learning UI when hello module is selected.</span></span> <span data-ttu-id="7b1aa-220">Argumenty může být jakýkoli z typů hello podporované nebo můžete vytvořit vlastní výčet v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-220">Arguments can be any of hello supported types or you can create a custom enumeration when needed.</span></span> <span data-ttu-id="7b1aa-221">Podobné toohello **porty** elementů **argumenty** elementy může mít volitelný **popis** element, který určuje text hello, která se zobrazuje při přesunutí ukazatele myši hello přes hello název parametru.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-221">Similar toohello **Ports** elements, **Arguments** elements can have an optional **Description** element that specifies hello text that appears when you hover hello mouse over hello parameter name.</span></span>
<span data-ttu-id="7b1aa-222">Volitelné vlastnosti pro modul, jako je například výchozí hodnota, minValue a maxValue přidáním tooany argument jako atributy tooa **vlastnosti** elementu.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-222">Optional properties for a module, such as defaultValue, minValue, and maxValue can be added tooany argument as attributes tooa **Properties** element.</span></span> <span data-ttu-id="7b1aa-223">Platný vlastnosti hello **vlastnosti** element závisí na typu argument hello a jsou popsané s typy argumentů hello podporované v další části hello.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-223">Valid properties for hello **Properties** element depend on hello argument type and are described with hello supported argument types in hello next section.</span></span> <span data-ttu-id="7b1aa-224">Argumenty s hello **isOptional** vlastností nastavenou příliš**"true"** nevyžadují hello uživatele tooenter hodnotu.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-224">Arguments with hello **isOptional** property set too**"true"** do not require hello user tooenter a value.</span></span> <span data-ttu-id="7b1aa-225">Pokud hodnota není k dispozici toohello argument, není předán hello argument funkce toohello vstupního bodu.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-225">If a value is not provided toohello argument, then hello argument is not passed toohello entry point function.</span></span> <span data-ttu-id="7b1aa-226">Argumenty hello vstupní bod funkce, které mají toobe volitelné nutné explicitně zpracovávat hello funkce, například přiřadit výchozí hodnotu NULL v hello vstupní bod funkce definici.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-226">Arguments of hello entry point function that are optional need toobe explicitly handled by hello function, e.g. assigned a default value of NULL in hello entry point function definition.</span></span> <span data-ttu-id="7b1aa-227">Nepovinný argument pouze vynutí hello jiná omezení argument, tj. min nebo max, pokud je hodnota zadaná uživatelem hello.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-227">An optional argument will only enforce hello other argument constraints, i.e. min or max, if a value is provided by hello user.</span></span>
<span data-ttu-id="7b1aa-228">Stejně jako u vstupy a výstupy, je velmi důležité, že každý z parametrů hello mít jedinečné id hodnoty s nimi spojených.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-228">As with inputs and outputs, it is critical that each of hello parameters have unique id values associated with them.</span></span> <span data-ttu-id="7b1aa-229">V našem úvodní příklad hello přidružené id a parametru byla *swap*.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-229">In our quick start example hello associated id/parameter was *swap*.</span></span>

### <a name="arg-element"></a><span data-ttu-id="7b1aa-230">Arg – element</span><span class="sxs-lookup"><span data-stu-id="7b1aa-230">Arg element</span></span>
<span data-ttu-id="7b1aa-231">Parametr modulu je definován pomocí hello **Arg** podřízený element hello **argumenty** část definičního soubor XML hello.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-231">A module parameter is defined using hello **Arg** child element of hello **Arguments** section of hello XML definition file.</span></span> <span data-ttu-id="7b1aa-232">Stejně jako u hello podřízených elementů v hello **porty** část, hello řazení parametrů v hello **argumenty** oddíl definuje hello rozložení v hello UX</span><span class="sxs-lookup"><span data-stu-id="7b1aa-232">As with hello child elements in hello **Ports** section, hello ordering of parameters in hello **Arguments** section defines hello layout encountered in hello UX.</span></span> <span data-ttu-id="7b1aa-233">Hello parametry jsou shora dolů v hello uživatelského rozhraní v hello stejné pořadí, ve kterém jsou definovány v souboru XML hello.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-233">hello parameters appear from top down in hello UI in hello same order in which they are defined in hello XML file.</span></span> <span data-ttu-id="7b1aa-234">Hello typy podporované nástrojem Machine Learning pro parametry jsou zde uvedeny.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-234">hello types supported by Machine Learning for parameters are listed here.</span></span> 

<span data-ttu-id="7b1aa-235">**int** – parametru typu (32 bitů) celé číslo.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-235">**int** – an Integer (32-bit) type parameter.</span></span>

    <Arg id="intValue1" name="Int Param" type="int">
        <Properties min="0" max="100" default="0" />
        <Description>Integer Parameter</Description>
    </Arg>


* <span data-ttu-id="7b1aa-236">*Volitelné vlastnosti*: **min**, **maximální**, **výchozí** a **isOptional**</span><span class="sxs-lookup"><span data-stu-id="7b1aa-236">*Optional Properties*: **min**, **max**, **default** and **isOptional**</span></span>

<span data-ttu-id="7b1aa-237">**dvojité** – parametr typu double.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-237">**double** – a double type parameter.</span></span>

    <Arg id="doubleValue1" name="Double Param" type="double">
        <Properties min="0.000" max="0.999" default="0.3" />
        <Description>Double Parameter</Description>
    </Arg>


* <span data-ttu-id="7b1aa-238">*Volitelné vlastnosti*: **min**, **maximální**, **výchozí** a **isOptional**</span><span class="sxs-lookup"><span data-stu-id="7b1aa-238">*Optional Properties*: **min**, **max**, **default** and **isOptional**</span></span>

<span data-ttu-id="7b1aa-239">**BOOL** – logického parametru, která je reprezentována zaškrtávací políčko UX</span><span class="sxs-lookup"><span data-stu-id="7b1aa-239">**bool** – a Boolean parameter that is represented by a check-box in UX.</span></span>

    <Arg id="boolValue1" name="Boolean Param" type="bool">
        <Properties default="true" />
        <Description>Boolean Parameter</Description>
    </Arg>



* <span data-ttu-id="7b1aa-240">*Volitelné vlastnosti*: **výchozí** -false v případě není nastavena.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-240">*Optional Properties*: **default** - false if not set</span></span>

<span data-ttu-id="7b1aa-241">**řetězec**: standardní řetězec</span><span class="sxs-lookup"><span data-stu-id="7b1aa-241">**string**: a standard string</span></span>

    <Arg id="stringValue1" name="My string Param" type="string">
        <Properties isOptional="true" />
        <Description>String Parameter 1</Description>
    </Arg>    

* <span data-ttu-id="7b1aa-242">*Volitelné vlastnosti*: **výchozí** a **isOptional**</span><span class="sxs-lookup"><span data-stu-id="7b1aa-242">*Optional Properties*: **default** and **isOptional**</span></span>

<span data-ttu-id="7b1aa-243">**ColumnPicker**: Parametr výběr sloupce.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-243">**ColumnPicker**: a column selection parameter.</span></span> <span data-ttu-id="7b1aa-244">Tento typ se vykreslí v hello UX jako zvolit sloupce.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-244">This type renders in hello UX as a column chooser.</span></span> <span data-ttu-id="7b1aa-245">Hello **vlastnost** element je id použité sem toospecify hello hello portu, ze které se vybírají sloupce, které musí být hello cílový port typ *DataTable*.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-245">hello **Property** element is used here toospecify hello id of hello port from which columns are selected, where hello target port type must be *DataTable*.</span></span> <span data-ttu-id="7b1aa-246">výsledek Hello hello sloupců výběru se předá toohello R funkce jako seznam řetězců obsahující názvy sloupců hello vybrané.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-246">hello result of hello column selection is passed toohello R function as a list of strings containing hello selected column names.</span></span> 

        <Arg id="colset" name="Column set" type="ColumnPicker">      
          <Properties portId="datasetIn1" allowedTypes="Numeric" default="NumericAll"/>
          <Description>Column set</Description>
        </Arg>


* <span data-ttu-id="7b1aa-247">*Požadované vlastnosti*: **portId** -odpovídá hello id elementu vstupu s typem *DataTable*.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-247">*Required Properties*: **portId** - matches hello id of an Input element with type *DataTable*.</span></span>
* <span data-ttu-id="7b1aa-248">*Volitelné vlastnosti*:</span><span class="sxs-lookup"><span data-stu-id="7b1aa-248">*Optional Properties*:</span></span>
  
  * <span data-ttu-id="7b1aa-249">**allowedTypes** -typy filtrů hello sloupec, ze kterého můžete vybrat.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-249">**allowedTypes** - Filters hello column types from which you can pick.</span></span> <span data-ttu-id="7b1aa-250">Platné hodnoty patří:</span><span class="sxs-lookup"><span data-stu-id="7b1aa-250">Valid values include:</span></span> 
    
    * <span data-ttu-id="7b1aa-251">číselné</span><span class="sxs-lookup"><span data-stu-id="7b1aa-251">Numeric</span></span>
    * <span data-ttu-id="7b1aa-252">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="7b1aa-252">Boolean</span></span>
    * <span data-ttu-id="7b1aa-253">Kategorické</span><span class="sxs-lookup"><span data-stu-id="7b1aa-253">Categorical</span></span>
    * <span data-ttu-id="7b1aa-254">Řetězec</span><span class="sxs-lookup"><span data-stu-id="7b1aa-254">String</span></span>
    * <span data-ttu-id="7b1aa-255">Štítek</span><span class="sxs-lookup"><span data-stu-id="7b1aa-255">Label</span></span>
    * <span data-ttu-id="7b1aa-256">Funkce</span><span class="sxs-lookup"><span data-stu-id="7b1aa-256">Feature</span></span>
    * <span data-ttu-id="7b1aa-257">Hodnocení</span><span class="sxs-lookup"><span data-stu-id="7b1aa-257">Score</span></span>
    * <span data-ttu-id="7b1aa-258">Všechny</span><span class="sxs-lookup"><span data-stu-id="7b1aa-258">All</span></span>
  * <span data-ttu-id="7b1aa-259">**výchozí** -platný výchozí možnosti pro výběr sloupce hello patří:</span><span class="sxs-lookup"><span data-stu-id="7b1aa-259">**default** - Valid default selections for hello column picker include:</span></span> 
    
    * <span data-ttu-id="7b1aa-260">Žádný</span><span class="sxs-lookup"><span data-stu-id="7b1aa-260">None</span></span>
    * <span data-ttu-id="7b1aa-261">NumericFeature</span><span class="sxs-lookup"><span data-stu-id="7b1aa-261">NumericFeature</span></span>
    * <span data-ttu-id="7b1aa-262">NumericLabel</span><span class="sxs-lookup"><span data-stu-id="7b1aa-262">NumericLabel</span></span>
    * <span data-ttu-id="7b1aa-263">NumericScore</span><span class="sxs-lookup"><span data-stu-id="7b1aa-263">NumericScore</span></span>
    * <span data-ttu-id="7b1aa-264">NumericAll</span><span class="sxs-lookup"><span data-stu-id="7b1aa-264">NumericAll</span></span>
    * <span data-ttu-id="7b1aa-265">BooleanFeature</span><span class="sxs-lookup"><span data-stu-id="7b1aa-265">BooleanFeature</span></span>
    * <span data-ttu-id="7b1aa-266">BooleanLabel</span><span class="sxs-lookup"><span data-stu-id="7b1aa-266">BooleanLabel</span></span>
    * <span data-ttu-id="7b1aa-267">BooleanScore</span><span class="sxs-lookup"><span data-stu-id="7b1aa-267">BooleanScore</span></span>
    * <span data-ttu-id="7b1aa-268">BooleanAll</span><span class="sxs-lookup"><span data-stu-id="7b1aa-268">BooleanAll</span></span>
    * <span data-ttu-id="7b1aa-269">CategoricalFeature</span><span class="sxs-lookup"><span data-stu-id="7b1aa-269">CategoricalFeature</span></span>
    * <span data-ttu-id="7b1aa-270">CategoricalLabel</span><span class="sxs-lookup"><span data-stu-id="7b1aa-270">CategoricalLabel</span></span>
    * <span data-ttu-id="7b1aa-271">CategoricalScore</span><span class="sxs-lookup"><span data-stu-id="7b1aa-271">CategoricalScore</span></span>
    * <span data-ttu-id="7b1aa-272">CategoricalAll</span><span class="sxs-lookup"><span data-stu-id="7b1aa-272">CategoricalAll</span></span>
    * <span data-ttu-id="7b1aa-273">StringFeature</span><span class="sxs-lookup"><span data-stu-id="7b1aa-273">StringFeature</span></span>
    * <span data-ttu-id="7b1aa-274">StringLabel</span><span class="sxs-lookup"><span data-stu-id="7b1aa-274">StringLabel</span></span>
    * <span data-ttu-id="7b1aa-275">StringScore</span><span class="sxs-lookup"><span data-stu-id="7b1aa-275">StringScore</span></span>
    * <span data-ttu-id="7b1aa-276">StringAll</span><span class="sxs-lookup"><span data-stu-id="7b1aa-276">StringAll</span></span>
    * <span data-ttu-id="7b1aa-277">AllLabel</span><span class="sxs-lookup"><span data-stu-id="7b1aa-277">AllLabel</span></span>
    * <span data-ttu-id="7b1aa-278">AllFeature</span><span class="sxs-lookup"><span data-stu-id="7b1aa-278">AllFeature</span></span>
    * <span data-ttu-id="7b1aa-279">AllScore</span><span class="sxs-lookup"><span data-stu-id="7b1aa-279">AllScore</span></span>
    * <span data-ttu-id="7b1aa-280">Všechny</span><span class="sxs-lookup"><span data-stu-id="7b1aa-280">All</span></span>

<span data-ttu-id="7b1aa-281">**Rozevírací seznam**: seznam uživatelská výčtového (rozevírací).</span><span class="sxs-lookup"><span data-stu-id="7b1aa-281">**DropDown**: a user-specified enumerated (dropdown) list.</span></span> <span data-ttu-id="7b1aa-282">Hello rozevírací seznam položek, které jsou určené v rámci hello **vlastnosti** pomocí elementu **položky** elementu.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-282">hello dropdown items are specified within hello **Properties** element using an **Item** element.</span></span> <span data-ttu-id="7b1aa-283">Hello **id** pro každou **položky** musí být jedinečný a platná proměnná R.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-283">hello **id** for each **Item** must be unique and a valid R variable.</span></span> <span data-ttu-id="7b1aa-284">Hello hodnotu hello **název** z **položky** slouží jako text hello, který se zobrazí i hello hodnotu, která je předána toohello R funkce.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-284">hello value of hello **name** of an **Item** serves as both hello text that you see and hello value that is passed toohello R function.</span></span>

    <Arg id="color" name="Color" type="DropDown">
      <Properties default="red">
        <Item id="red" name="Red Value"/>
        <Item id="green" name="Green Value"/>
        <Item id="blue" name="Blue Value"/>
      </Properties>
      <Description>Select a color.</Description>
    </Arg>    

* <span data-ttu-id="7b1aa-285">*Volitelné vlastnosti*:</span><span class="sxs-lookup"><span data-stu-id="7b1aa-285">*Optional Properties*:</span></span>
  * <span data-ttu-id="7b1aa-286">**výchozí** – hello hodnota hello výchozí vlastnost, musí být shodná s hodnotou id z jednoho z hello **položky** elementy.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-286">**default** - hello value for hello default property must correspond with an id value from one of hello **Item** elements.</span></span>

### <a name="auxiliary-files"></a><span data-ttu-id="7b1aa-287">Pomocné soubory</span><span class="sxs-lookup"><span data-stu-id="7b1aa-287">Auxiliary Files</span></span>
<span data-ttu-id="7b1aa-288">Všechny soubory, které je umístěn v souboru ZIP vlastní modul je toobe má k dispozici pro použití v době spuštění.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-288">Any file that is placed in your custom module ZIP file is going toobe available for use during execution time.</span></span> <span data-ttu-id="7b1aa-289">Všechny adresáře struktury přítomen se zachovají.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-289">Any directory structures present are preserved.</span></span> <span data-ttu-id="7b1aa-290">To znamená, že stejný soubor zdrojové funguje hello místně a v Azure Machine Learning provádění.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-290">This means that file sourcing works hello same locally and in Azure Machine Learning execution.</span></span> 

> [!NOTE]
> <span data-ttu-id="7b1aa-291">Všimněte si, zda jsou všechny soubory extrahované too'src' directory proto musí mít všechny cesty ' src /' předponu.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-291">Notice that all files are extracted too‘src’ directory so all paths should have ‘src/’ prefix.</span></span>
> 
> 

<span data-ttu-id="7b1aa-292">Předpokládejme například že chcete tooremove žádné řádky s NAs z hello datové sady a taky před výstup do CustomAddRows odeberte všechny duplicitní řádky, a jste již zapsány R funkce, která nemá v souboru RemoveDupNARows.R:</span><span class="sxs-lookup"><span data-stu-id="7b1aa-292">For example, say you want tooremove any rows with NAs from hello dataset, and also remove any duplicate rows, before outputting it into CustomAddRows, and you’ve already written an R function that does that in a file RemoveDupNARows.R:</span></span>

    RemoveDupNARows <- function(dataFrame) {
        #Remove Duplicate Rows:
        dataFrame <- unique(dataFrame)
        #Remove Rows with NAs:
        finalDataFrame <- dataFrame[complete.cases(dataFrame),]
        return(finalDataFrame)
    }
<span data-ttu-id="7b1aa-293">Zdrojem může být hello pomocného souboru RemoveDupNARows.R v hello CustomAddRows funkce:</span><span class="sxs-lookup"><span data-stu-id="7b1aa-293">You can source hello auxiliary file RemoveDupNARows.R in hello CustomAddRows function:</span></span>

    CustomAddRows <- function(dataset1, dataset2, swap=FALSE) {
        source("src/RemoveDupNARows.R")
            if (swap) { 
                dataset <- rbind(dataset2, dataset1))
             } else { 
                  dataset <- rbind(dataset1, dataset2)) 
             } 
        dataset <- removeDupNARows(dataset)
        return (dataset)
    }

<span data-ttu-id="7b1aa-294">V dalším kroku nahrajte soubor zip obsahující 'CustomAddRows.R', 'CustomAddRows.xml' a 'RemoveDupNARows.R' jako vlastní modul R.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-294">Next, upload a zip file containing ‘CustomAddRows.R’, ‘CustomAddRows.xml’, and ‘RemoveDupNARows.R’ as a custom R module.</span></span>

## <a name="execution-environment"></a><span data-ttu-id="7b1aa-295">Prostředí pro spuštění</span><span class="sxs-lookup"><span data-stu-id="7b1aa-295">Execution Environment</span></span>
<span data-ttu-id="7b1aa-296">Hello prostředí pro spuštění skriptu hello R hello používá stejnou verzi R jako hello **spustit skript jazyka R** modulu a můžete použít hello stejné výchozí balíčky.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-296">hello execution environment for hello R script uses hello same version of R as hello **Execute R Script** module and can use hello same default packages.</span></span> <span data-ttu-id="7b1aa-297">Můžete také přidat další balíčky tooyour vlastní modul R zahrnutím hello vlastní modul zip balíčku.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-297">You can also add additional R packages tooyour custom module by including them in hello custom module zip package.</span></span> <span data-ttu-id="7b1aa-298">Jenom je načte ve vašem skriptu R stejně jako v prostředí R.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-298">Just load them in your R script as you would in your own R environment.</span></span> 

<span data-ttu-id="7b1aa-299">**Omezení prostředí pro spuštění hello** zahrnují:</span><span class="sxs-lookup"><span data-stu-id="7b1aa-299">**Limitations of hello execution environment** include:</span></span>

* <span data-ttu-id="7b1aa-300">Systém souborů bez trvalé: soubory zapsané při spuštění vlastního modulu hello nejsou trvalé napříč více spustí hello stejném modulu.</span><span class="sxs-lookup"><span data-stu-id="7b1aa-300">Non-persistent file system: Files written when hello custom module is run are not persisted across multiple runs of hello same module.</span></span>
* <span data-ttu-id="7b1aa-301">Žádný přístup k síti</span><span class="sxs-lookup"><span data-stu-id="7b1aa-301">No network access</span></span>

