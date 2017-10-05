---
title: "Vytvořit vlastní R moduly v Azure Machine Learning | Microsoft Docs"
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
ms.openlocfilehash: 964ddb551a475243891abce8a2b835e65569a4ca
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="author-custom-r-modules-in-azure-machine-learning"></a><span data-ttu-id="6fb5c-103">Vytváření vlastních modulů R ve službě Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="6fb5c-103">Author custom R modules in Azure Machine Learning</span></span>
<span data-ttu-id="6fb5c-104">Toto téma popisuje, jak vytvořit a nasadit vlastní modul R v Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-104">This topic describes how to author and deploy a custom R module in Azure Machine Learning.</span></span> <span data-ttu-id="6fb5c-105">Vysvětluje, co jsou vlastních modulů R a jaké soubory se používají k definovat.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-105">It explains what custom R modules are and what files are used to define them.</span></span> <span data-ttu-id="6fb5c-106">Ukazuje, jak vytvořit soubory, které definují modul a jak registrace modulu pro nasazení v pracovním prostoru Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-106">It illustrates how to construct the files that define a module and how to register the module for deployment in a Machine Learning workspace.</span></span> <span data-ttu-id="6fb5c-107">Elementy a atributy používané v definici vlastní modul jsou pak popsány podrobněji.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-107">The elements and attributes used in the definition of the custom module are then described in more detail.</span></span> <span data-ttu-id="6fb5c-108">Postup použití pomocného funkce a soubory a několik výstupů také popsané.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-108">How to use auxiliary functionality and files and multiple outputs is also discussed.</span></span> 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="what-is-a-custom-r-module"></a><span data-ttu-id="6fb5c-109">Co je vlastní modul R?</span><span class="sxs-lookup"><span data-stu-id="6fb5c-109">What is a custom R module?</span></span>
<span data-ttu-id="6fb5c-110">A **vlastní modul** je modul definovaný uživatelem, který může být nahrán do pracovního prostoru a provést v rámci experimentu Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-110">A **custom module** is a user-defined module that can be uploaded to your workspace and executed as part of an Azure Machine Learning experiment.</span></span> <span data-ttu-id="6fb5c-111">A **vlastní modul R** je vlastní modul, který provede uživatelsky definované funkce R.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-111">A **custom R module** is a custom module that executes a user-defined R function.</span></span> <span data-ttu-id="6fb5c-112">**R** je programovací jazyk pro statistické výpočty a obrázky, které se často používá podle vědců statistikami a dat pro implementaci algoritmy.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-112">**R** is a programming language for statistical computing and graphics that is widely used by statisticians and data scientists for implementing algorithms.</span></span> <span data-ttu-id="6fb5c-113">V současné době R je jediným podporovaným v vlastní moduly, ale podpory pro další jazyky je naplánována na budoucích verzích jazykem.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-113">Currently, R is the only language supported in custom modules, but support for additional languages is scheduled for future releases.</span></span>

<span data-ttu-id="6fb5c-114">Vlastní moduly mají **prvotřídní stav** v Azure Machine Learning v tom smyslu, že je lze použít stejně jako ostatní moduly.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-114">Custom modules have **first-class status** in Azure Machine Learning in the sense that they can be used just like any other module.</span></span> <span data-ttu-id="6fb5c-115">Mohou být provedeny další moduly zahrnuté v publikované experimenty nebo vizualizace.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-115">They can be executed with other modules, included in published experiments or in visualizations.</span></span> <span data-ttu-id="6fb5c-116">Budete mít kontrolu nad algoritmus implementovaný modul, vstup a výstupních portů, který se má použít, modelování parametry a další různé chování za běhu.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-116">You have control over the algorithm implemented by the module, the input and output ports to be used, the modeling parameters, and other various runtime behaviors.</span></span> <span data-ttu-id="6fb5c-117">Experimentu, který obsahuje vlastní moduly můžete také publikovat na webu Cortana Intelligence Gallery pro snadné sdílení.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-117">An experiment that contains custom modules can also be published into the Cortana Intelligence Gallery for easy sharing.</span></span>

## <a name="files-in-a-custom-r-module"></a><span data-ttu-id="6fb5c-118">Soubory ve vlastní modul R</span><span class="sxs-lookup"><span data-stu-id="6fb5c-118">Files in a custom R module</span></span>
<span data-ttu-id="6fb5c-119">Vlastní modul R je definováno soubor .zip, který obsahuje minimálně dva soubory:</span><span class="sxs-lookup"><span data-stu-id="6fb5c-119">A custom R module is defined by a .zip file that contains, at a minimum, two files:</span></span>

* <span data-ttu-id="6fb5c-120">A **zdrojový soubor** , která implementuje funkce R vystavené modulu</span><span class="sxs-lookup"><span data-stu-id="6fb5c-120">A **source file** that implements the R function exposed by the module</span></span>
* <span data-ttu-id="6fb5c-121">**Soubor definice XML** rozhraní vlastní modul, který popisuje</span><span class="sxs-lookup"><span data-stu-id="6fb5c-121">An **XML definition file** that describes the custom module interface</span></span>

<span data-ttu-id="6fb5c-122">Další pomocné soubory můžou být součástí souboru .zip, který poskytuje funkce, která je přístupná z vlastní modul.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-122">Additional auxiliary files can also be included in the .zip file that provides functionality that can be accessed from the custom module.</span></span> <span data-ttu-id="6fb5c-123">Tato možnost je podrobněji **argumenty** část oddílu referenční **elementy v definičním souboru XML** následující ukázka rychlý start.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-123">This option is discussed in the **Arguments** part of the reference section **Elements in the XML definition file** following the quickstart example.</span></span>

## <a name="quickstart-example-define-package-and-register-a-custom-r-module"></a><span data-ttu-id="6fb5c-124">Příklad rychlý start: Definujte, balíčků a zaregistrovat vlastní modul R</span><span class="sxs-lookup"><span data-stu-id="6fb5c-124">Quickstart example: define, package, and register a custom R module</span></span>
<span data-ttu-id="6fb5c-125">Tento příklad ukazuje, jak sestavit soubory vyžadované vlastní modul R, zabalit do souboru zip a potom proveďte registraci modulu v pracovním prostoru Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-125">This example illustrates how to construct the files required by a custom R module, package them into a zip file, and then register the module in your Machine Learning workspace.</span></span> <span data-ttu-id="6fb5c-126">Příklad zip balíčku a ukázkové soubory si můžete stáhnout z [CustomAddRows.zip stáhnout soubor](http://go.microsoft.com/fwlink/?LinkID=524916&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="6fb5c-126">The example zip package and sample files can be downloaded from [Download CustomAddRows.zip file](http://go.microsoft.com/fwlink/?LinkID=524916&clcid=0x409).</span></span>

## <a name="the-source-file"></a><span data-ttu-id="6fb5c-127">Zdrojový soubor</span><span class="sxs-lookup"><span data-stu-id="6fb5c-127">The source file</span></span>
<span data-ttu-id="6fb5c-128">Podívejte se na příklad z **řádky přidat vlastní** modul, který upravuje standardní implementace **přidat řádky** modul použít ke zřetězení řádky (připomínky) z dvě datové sady (datových rámců).</span><span class="sxs-lookup"><span data-stu-id="6fb5c-128">Consider the example of a **Custom Add Rows** module that modifies the standard implementation of the **Add Rows** module used to concatenate rows (observations) from two datasets (data frames).</span></span> <span data-ttu-id="6fb5c-129">Standardní **přidat řádky** modulu připojí řádky druhé vstupní datové sady na konec první použití vstupní datové sady `rbind` algoritmus.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-129">The standard **Add Rows** module appends the rows of the second input dataset to the end of the first input dataset using the `rbind` algorithm.</span></span> <span data-ttu-id="6fb5c-130">Vlastní `CustomAddRows` funkce podobně přijímá dvě datové sady, ale také přijme parametr Boolean odkládacího souboru jako další vstup.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-130">The customized `CustomAddRows` function similarly accepts two datasets, but also accepts a Boolean swap parameter as an additional input.</span></span> <span data-ttu-id="6fb5c-131">Pokud parametr odkládacího souboru je nastaven na **FALSE**, vrátí sadu dat jako standardní implementace.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-131">If the swap parameter is set to **FALSE**, it returns the same data set as the standard implementation.</span></span> <span data-ttu-id="6fb5c-132">Ale pokud je parametr swap **TRUE**, funkce připojí řádky první vstupní datové sady na konec druhý datovou sadu místo.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-132">But if the swap parameter is **TRUE**, the function appends rows of first input dataset to the end of the second dataset instead.</span></span> <span data-ttu-id="6fb5c-133">CustomAddRows.R soubor, který obsahuje implementaci r `CustomAddRows` funkce vystavené **řádky přidat vlastní** modul má následující kód R.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-133">The CustomAddRows.R file that contains the implementation of the R `CustomAddRows` function exposed by the **Custom Add Rows** module has the following R code.</span></span>

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

### <a name="the-xml-definition-file"></a><span data-ttu-id="6fb5c-134">Soubor definice XML</span><span class="sxs-lookup"><span data-stu-id="6fb5c-134">The XML definition file</span></span>
<span data-ttu-id="6fb5c-135">To vystavit `CustomAddRows` funkce jako modul služby Azure Machine Learning, soubor definice XML musí být vytvořený zadat jak **řádky přidat vlastní** modul by měl vzhled a chování.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-135">To expose this `CustomAddRows` function as an Azure Machine Learning module, an XML definition file must be created to specify how the **Custom Add Rows** module should look and behave.</span></span> 

    <!-- Defined a module using an R Script -->
    <Module name="Custom Add Rows">
        <Owner>Microsoft Corporation</Owner>
        <Description>Appends one dataset to another. Dataset 2 is concatenated to Dataset 1 when Swap is FALSE, and vice versa when Swap is TRUE.</Description>

    <!-- Specify the base language, script file and R function to use for this module. -->        
        <Language name="R" 
         sourceFile="CustomAddRows.R" 
         entryPoint="CustomAddRows" />  

    <!-- Define module input and output ports -->
    <!-- Note: The values of the id attributes in the Input and Arg elements must match the parameter names in the R Function CustomAddRows defined in CustomAddRows.R. -->
        <Ports>
            <Input id="dataset1" name="Dataset 1" type="DataTable">
                <Description>First input dataset</Description>
            </Input>
            <Input id="dataset2" name="Dataset 2" type="DataTable">
                <Description>Second input dataset</Description>
            </Input>
            <Output id="dataset" name="Dataset" type="DataTable">
                <Description>The combined dataset</Description>
            </Output>
        </Ports>

    <!-- Define module parameters -->
        <Arguments>
            <Arg id="swap" name="Swap" type="bool" >
                <Description>Swap input datasets.</Description>
            </Arg>
        </Arguments>
    </Module>


<span data-ttu-id="6fb5c-136">Je důležité si uvědomit, že hodnota **id** atributy **vstup** a **Arg** elementů v souboru XML musí odpovídat názvům parametr funkce kód R CustomAddRows.R souboru přesně: (*dataset1*, *dataset2*, a *swap* v příkladu).</span><span class="sxs-lookup"><span data-stu-id="6fb5c-136">It is critical to note that the value of the **id** attributes of the **Input** and **Arg** elements in the XML file must match the function parameter names of the R code in the CustomAddRows.R file EXACTLY: (*dataset1*, *dataset2*, and *swap* in the example).</span></span> <span data-ttu-id="6fb5c-137">Podobně, hodnota **entryPoint** atribut **jazyk** element musí přesně odpovídat názvu funkce ve skriptu jazyka R: (*CustomAddRows* v příkladu) .</span><span class="sxs-lookup"><span data-stu-id="6fb5c-137">Similarly, the value of the **entryPoint** attribute of the **Language** element must match the name of the function in the R script EXACTLY: (*CustomAddRows* in the example).</span></span> 

<span data-ttu-id="6fb5c-138">Naproti tomu **id** atribut pro **výstup** element neodpovídá žádné proměnné ve skriptu R.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-138">In contrast, the **id** attribute for the **Output** element does not correspond to any variables in the R script.</span></span> <span data-ttu-id="6fb5c-139">Pokud je vyžadován více než jeden výstup, jednoduše vrácení seznamu z funkce R s výsledky umístit *ve stejném pořadí* jako **výstupy** elementy jsou deklarované v souboru XML.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-139">When more than one output is required, simply return a list from the R function with results placed *in the same order* as **Outputs** elements are declared in the XML file.</span></span>

### <a name="package-and-register-the-module"></a><span data-ttu-id="6fb5c-140">Balíček a registrace modulu</span><span class="sxs-lookup"><span data-stu-id="6fb5c-140">Package and register the module</span></span>
<span data-ttu-id="6fb5c-141">Uložit tyto dva soubory jako *CustomAddRows.R* a *CustomAddRows.xml* a pak zip společně do dvou souborech *CustomAddRows.zip* souboru.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-141">Save these two files as *CustomAddRows.R* and *CustomAddRows.xml* and then zip the two files together into a *CustomAddRows.zip* file.</span></span>

<span data-ttu-id="6fb5c-142">Chcete-li zaregistrovat je v pracovním prostoru Machine Learning, přejděte do pracovního prostoru v nástroji Machine Learning Studio, klikněte na **+ nový** tlačítko v dolní a zvolte **modulu -> z ZIP balíčku** nahrát nový **Řádky přidat vlastní** modulu.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-142">To register them in your Machine Learning workspace, go to your workspace in the Machine Learning Studio, click the **+NEW** button on the bottom and choose **MODULE -> FROM ZIP PACKAGE** to upload the new **Custom Add Rows** module.</span></span>

![Nahrát Zip](./media/machine-learning-custom-r-modules/upload-from-zip-package.png)

<span data-ttu-id="6fb5c-144">**Řádky přidat vlastní** modulu je nyní připravena ke kterým přistupují experimentů Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-144">The **Custom Add Rows** module is now ready to be accessed by your Machine Learning experiments.</span></span>

## <a name="elements-in-the-xml-definition-file"></a><span data-ttu-id="6fb5c-145">Elementy v definičním souboru XML</span><span class="sxs-lookup"><span data-stu-id="6fb5c-145">Elements in the XML definition file</span></span>
### <a name="module-elements"></a><span data-ttu-id="6fb5c-146">Modul elementy</span><span class="sxs-lookup"><span data-stu-id="6fb5c-146">Module elements</span></span>
<span data-ttu-id="6fb5c-147">**Modulu** element se používá k definování vlastní modul v souboru XML.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-147">The **Module** element is used to define a custom module in the XML file.</span></span> <span data-ttu-id="6fb5c-148">Více modulů lze definovat v jednom souboru XML pomocí několika **modulu** elementy.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-148">Multiple modules can be defined in one XML file using multiple **module** elements.</span></span> <span data-ttu-id="6fb5c-149">Každý modulu v pracovním prostoru musí mít jedinečný název.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-149">Each module in your workspace must have a unique name.</span></span> <span data-ttu-id="6fb5c-150">Registrovat vlastní modul se stejným názvem jako stávající vlastní modul a nahrazuje existující modul s tímto novým připojením.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-150">Register a custom module with the same name as an existing custom module and it replaces the existing module with the new one.</span></span> <span data-ttu-id="6fb5c-151">Vlastní moduly lze však zaregistrována stejný název jako existující modul Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-151">Custom modules can, however, be registered with the same name as an existing Azure Machine Learning module.</span></span> <span data-ttu-id="6fb5c-152">Pokud ano, se objeví v **vlastní** kategorii palety modulů.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-152">If so, they appear in the **Custom** category of the module palette.</span></span>

    <Module name="Custom Add Rows" isDeterministic="false"> 
        <Owner>Microsoft Corporation</Owner>
        <Description>Appends one dataset to another...</Description>/> 


<span data-ttu-id="6fb5c-153">V rámci **modulu** elementu, můžete určit dva další volitelné prvky:</span><span class="sxs-lookup"><span data-stu-id="6fb5c-153">Within the **Module** element, you can specify two additional optional elements:</span></span>

* <span data-ttu-id="6fb5c-154">**vlastníka** element, který se vloží do modulu</span><span class="sxs-lookup"><span data-stu-id="6fb5c-154">an **Owner** element that is embedded into the module</span></span>  
* <span data-ttu-id="6fb5c-155">**popis** element, který obsahuje text, který se zobrazí v rychlé nápovědy pro modul a pokud ponecháte modul v uživatelském rozhraní Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-155">a **Description** element that contains text that is displayed in quick help for the module and when you hover over the module in the Machine Learning UI.</span></span>

<span data-ttu-id="6fb5c-156">Pravidla pro omezení znaků v elementech modul:</span><span class="sxs-lookup"><span data-stu-id="6fb5c-156">Rules for characters limits in the Module elements:</span></span>

* <span data-ttu-id="6fb5c-157">Hodnota **název** atribut **modulu** element nesmí být delší než 64 znaků.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-157">The value of the **name** attribute in the **Module** element must not exceed 64 characters in length.</span></span> 
* <span data-ttu-id="6fb5c-158">Obsah **popis** element nesmí být delší než 128 znaků.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-158">The content of the **Description** element must not exceed 128 characters in length.</span></span>
* <span data-ttu-id="6fb5c-159">Obsah **vlastníka** element nesmí být delší než 32 znaků.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-159">The content of the **Owner** element must not exceed 32 characters in length.</span></span>

<span data-ttu-id="6fb5c-160">Výsledky modulu může být deterministický nebo nondeterministic.* * ve výchozím nastavení, všechny moduly se považují za deterministický.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-160">A module's results can be deterministic or nondeterministic.** By default, all modules are considered to be deterministic.</span></span> <span data-ttu-id="6fb5c-161">To znamená, danou neměnné sadu vstupní parametry a data, modul by měl vrátit stejné výsledky eacRAND nebo functionh čas, který je spuštěn.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-161">That is, given an unchanging set of input parameters and data, the module should return the same results eacRAND or a functionh time it is run.</span></span> <span data-ttu-id="6fb5c-162">Vzhledem k tomuto chování, Azure Machine Learning Studio pouze opakovaně moduly, které jsou označeny jako deterministický, pokud parametr nebo vstupní data změnila.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-162">Given this behavior, Azure Machine Learning Studio only reruns modules marked as deterministic if a parameter or the input data has changed.</span></span> <span data-ttu-id="6fb5c-163">Vrací výsledky uložené v mezipaměti také poskytuje mnohem rychlejší provádění experimenty.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-163">Returning the cached results also provides much faster execution of experiments.</span></span>

<span data-ttu-id="6fb5c-164">Jsou funkce, které jsou deterministický, jako je například rand – nebo funkci, která vrátí aktuální datum nebo čas.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-164">There are functions that are nondeterministic, such as RAND or a function that returns the current date or time.</span></span> <span data-ttu-id="6fb5c-165">Pokud modul používá nedeterministická funkce, můžete určit, že modul bude Nedeterministický nastavením nepovinný **isDeterministic** atribut **FALSE**.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-165">If your module uses a nondeterministic function, you can specify that the module is non-deterministic by setting the optional **isDeterministic** attribute to **FALSE**.</span></span> <span data-ttu-id="6fb5c-166">To zajistí, že modul se znovu spustí při každém spuštění experimentu, i pokud nedošlo ke změně modulu vstup a parametry.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-166">This insures that the module is rerun whenever the experiment is run, even if the module input and parameters have not changed.</span></span> 

### <a name="language-definition"></a><span data-ttu-id="6fb5c-167">Definice jazyka</span><span class="sxs-lookup"><span data-stu-id="6fb5c-167">Language Definition</span></span>
<span data-ttu-id="6fb5c-168">**Jazyk** element v souboru definice XML slouží k určení vlastní modul jazyk.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-168">The **Language** element in your XML definition file is used to specify the custom module language.</span></span> <span data-ttu-id="6fb5c-169">V současné době R je jediný podporovaný jazyk.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-169">Currently, R is the only supported language.</span></span> <span data-ttu-id="6fb5c-170">Hodnota **zdrojový soubor** atribut musí být název souboru R, který obsahuje funkce, které má být volána při spuštění modulu.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-170">The value of the **sourceFile** attribute must be the name of the R file that contains the function to call when the module is run.</span></span> <span data-ttu-id="6fb5c-171">Tento soubor musí být součástí balíček zip.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-171">This file must be part of the zip package.</span></span> <span data-ttu-id="6fb5c-172">Hodnota **entryPoint** atribut je název funkce volané a platný funkci definovanou s ve zdrojovém souboru se musí shodovat.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-172">The value of the **entryPoint** attribute is the name of the function being called and must match a valid function defined with in the source file.</span></span>

    <Language name="R" sourceFile="CustomAddRows.R" entryPoint="CustomAddRows" />


### <a name="ports"></a><span data-ttu-id="6fb5c-173">Porty</span><span class="sxs-lookup"><span data-stu-id="6fb5c-173">Ports</span></span>
<span data-ttu-id="6fb5c-174">Vstupní a výstupní porty pro vlastní modul jsou určené v podřízených elementů **porty** část definičního soubor XML.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-174">The input and output ports for a custom module are specified in child elements of the **Ports** section of the XML definition file.</span></span> <span data-ttu-id="6fb5c-175">Pořadí těchto prvků určuje rozložení zkušeného (UX) uživatelé.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-175">The order of these elements determines the layout experienced (UX) by users.</span></span> <span data-ttu-id="6fb5c-176">Prvním podřízeným objektem **vstupní** nebo **výstup** uvedené v **porty** vstupní port nejvíce vlevo v UX Learning počítač se změní na element souboru XML</span><span class="sxs-lookup"><span data-stu-id="6fb5c-176">The first child **input** or **output** listed in the **Ports** element of the XML file becomes the left-most input port in the Machine Learning UX.</span></span>
<span data-ttu-id="6fb5c-177">Každý vstupní a výstupní port je pravděpodobně volitelný **popis** podřízený element, který určuje text, který zobrazí, když najedete myší portu v uživatelském rozhraní Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-177">Each input and output port may have an optional **Description** child element that specifies the text shown when you hover the mouse cursor over the port in the Machine Learning UI.</span></span>

<span data-ttu-id="6fb5c-178">**Porty pravidla**:</span><span class="sxs-lookup"><span data-stu-id="6fb5c-178">**Ports Rules**:</span></span>

* <span data-ttu-id="6fb5c-179">Maximální počet **vstupní a výstupní porty** je 8 pro každou.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-179">Maximum number of **input and output ports** is 8 for each.</span></span>

### <a name="input-elements"></a><span data-ttu-id="6fb5c-180">Elementy vstupu</span><span class="sxs-lookup"><span data-stu-id="6fb5c-180">Input elements</span></span>
<span data-ttu-id="6fb5c-181">Vstupní porty umožňují předat data do funkce R a pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-181">Input ports allow you to pass data to your R function and workspace.</span></span> <span data-ttu-id="6fb5c-182">**Datové typy** jsou podporovány pro vstupní porty jsou následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="6fb5c-182">The **data types** that are supported for input ports are as follows:</span></span> 

<span data-ttu-id="6fb5c-183">**DataTable:** tento typ je předán funkce R jako data.frame.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-183">**DataTable:** This type is passed to your R function as a data.frame.</span></span> <span data-ttu-id="6fb5c-184">Ve skutečnosti všechny typy (například CSV soubory nebo soubory ARFF), které jsou podporovány nástrojem Machine Learning a že jsou kompatibilní s **DataTable** se převedou na data.frame automaticky.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-184">In fact, any types (for example, CSV files or ARFF files) that are supported by Machine Learning and that are compatible with **DataTable** are converted to a data.frame automatically.</span></span> 

        <Input id="dataset1" name="Input 1" type="DataTable" isOptional="false">
            <Description>Input Dataset 1</Description>
           </Input>

<span data-ttu-id="6fb5c-185">**Id** atribut spojený s každou **DataTable** vstupní port musí mít jedinečnou hodnotu a tato hodnota musí odpovídat jeho odpovídající parametr v R funkce s názvem.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-185">The **id** attribute associated with each **DataTable** input port must have a unique value and this value must match its corresponding named parameter in your R function.</span></span>
<span data-ttu-id="6fb5c-186">Volitelné **DataTable** porty, které nejsou předány jako vstup v experimentu mají hodnotu **NULL** předaný funkci R a volitelné zip porty jsou ignorovány, pokud vstup není připojen.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-186">Optional **DataTable** ports that are not passed as input in an experiment have the value **NULL** passed to the R function and optional zip ports are ignored if the input is not connected.</span></span> <span data-ttu-id="6fb5c-187">**IsOptional** atribut je volitelný pro oba **DataTable** a **Zip** typy a je *false* ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-187">The **isOptional** attribute is optional for both the **DataTable** and **Zip** types and is *false* by default.</span></span>

<span data-ttu-id="6fb5c-188">**ZIP:** vlastní moduly může přijmout soubor zip jako vstup.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-188">**Zip:** Custom modules can accept a zip file as input.</span></span> <span data-ttu-id="6fb5c-189">Tento vstup je vybaleno do R pracovní adresář vaší – funkce</span><span class="sxs-lookup"><span data-stu-id="6fb5c-189">This input is unpacked into the R working directory of your function</span></span>

        <Input id="zippedData" name="Zip Input" type="Zip" IsOptional="false">
            <Description>Zip files to be extracted to the R working directory.</Description>
           </Input>

<span data-ttu-id="6fb5c-190">Pro vlastních modulů R tak, aby odpovídaly žádné parametry funkce R nemá id pro Zip port.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-190">For custom R modules, the id for a Zip port does not have to match any parameters of the R function.</span></span> <span data-ttu-id="6fb5c-191">Je to proto, že je soubor zip automaticky extrahovány do R pracovní adresář.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-191">This is because the zip file is automatically extracted to the R working directory.</span></span>

<span data-ttu-id="6fb5c-192">**Vstupní pravidla:**</span><span class="sxs-lookup"><span data-stu-id="6fb5c-192">**Input Rules:**</span></span>

* <span data-ttu-id="6fb5c-193">Hodnota **id** atribut **vstup** element musí být platný název proměnné R.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-193">The value of the **id** attribute of the **Input** element must be a valid R variable name.</span></span>
* <span data-ttu-id="6fb5c-194">Hodnota **id** atribut **vstup** element nesmí být delší než 64 znaků.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-194">The value of the **id** attribute of the **Input** element must not be longer than 64 characters.</span></span>
* <span data-ttu-id="6fb5c-195">Hodnota **název** atribut **vstup** element nesmí být delší než 64 znaků.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-195">The value of the **name** attribute of the **Input** element must not be longer than 64 characters.</span></span>
* <span data-ttu-id="6fb5c-196">Obsah **popis** element nesmí být delší než 128 znaků.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-196">The content of the **Description** element must not be longer than 128 characters</span></span>
* <span data-ttu-id="6fb5c-197">Hodnota **typ** atribut **vstup** element musí být *Zip* nebo *DataTable*.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-197">The value of the **type** attribute of the **Input** element must be *Zip* or *DataTable*.</span></span>
* <span data-ttu-id="6fb5c-198">Hodnota **isOptional** atribut **vstup** element není požadovaná (a je *false* ve výchozím nastavení není-li zadána); ale pokud je zadán, musí být *true* nebo *false*.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-198">The value of the **isOptional** attribute of the **Input** element is not required (and is *false* by default when not specified); but if it is specified, it must be *true* or *false*.</span></span>

### <a name="output-elements"></a><span data-ttu-id="6fb5c-199">Výstup elementy</span><span class="sxs-lookup"><span data-stu-id="6fb5c-199">Output elements</span></span>
<span data-ttu-id="6fb5c-200">**Standardní výstupní porty:** výstupních portů jsou namapované na vrácené hodnoty z funkce R, který můžete použít následující moduly.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-200">**Standard output ports:** Output ports are mapped to the return values from your R function, which can then be used by subsequent modules.</span></span> <span data-ttu-id="6fb5c-201">*DataTable* je typ pouze standardní výstupní port v současné době podporován.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-201">*DataTable* is the only standard output port type supported currently.</span></span> <span data-ttu-id="6fb5c-202">(Podpora pro *inteligentních algoritmů* a *transformuje* je chystaný.) A *DataTable* výstup je definován jako:</span><span class="sxs-lookup"><span data-stu-id="6fb5c-202">(Support for *Learners* and *Transforms* is forthcoming.) A *DataTable* output is defined as:</span></span>

    <Output id="dataset" name="Dataset" type="DataTable">
        <Description>Combined dataset</Description>
    </Output>

<span data-ttu-id="6fb5c-203">Pro výstupy ve vlastních modulů R, hodnota **id** atribut nemá tak, aby odpovídaly s nic v R skript, ale musí být jedinečný.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-203">For outputs in custom R modules, the value of the **id** attribute does not have to correspond with anything in the R script, but it must be unique.</span></span> <span data-ttu-id="6fb5c-204">Pro jeden modul výstupu, musí být návratovou hodnotou z funkce R *data.frame*.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-204">For a single module output, the return value from the R function must be a *data.frame*.</span></span> <span data-ttu-id="6fb5c-205">Chcete-li více než jeden objekt podporovaného typu výstupu odpovídající výstupních portů potřeba zadat v definičním souboru XML a objekty musí být vrácen jako seznam.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-205">In order to output more than one object of a supported data type, the appropriate output ports need to be specified in the XML definition file and the objects need to be returned as a list.</span></span> <span data-ttu-id="6fb5c-206">Objektů výstupního jsou přiřazeny k vypsání porty zleva doprava, která znázorňuje pořadí, ve kterém jsou objekty umístěn ve vráceném seznamu.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-206">The output objects are assigned to output ports from left to right, reflecting the order in which the objects are placed in the returned list.</span></span>

<span data-ttu-id="6fb5c-207">Například, pokud chcete upravit **řádky přidat vlastní** modulu výstup původní dvě datové sady, *dataset1* a *dataset2*, kromě nového připojené k datové sadě, *datovou sadu*, (v pořadí, zleva doprava, jako: *datovou sadu*, *dataset1*, *dataset2*), pak zadejte výstupních portů v CustomAddRows.xml souboru následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="6fb5c-207">For example, if you want to modify the **Custom Add Rows** module to output the original two datasets, *dataset1* and *dataset2*, in addition to the new joined dataset, *dataset*, (in an order, from left to right, as: *dataset*, *dataset1*, *dataset2*), then define the output ports in the CustomAddRows.xml file as follows:</span></span>

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


<span data-ttu-id="6fb5c-208">A vrátí seznam objektů v seznamu ve správném pořadí v 'CustomAddRows.R':</span><span class="sxs-lookup"><span data-stu-id="6fb5c-208">And return the list of objects in a list in the correct order in ‘CustomAddRows.R’:</span></span>

    CustomAddRows <- function(dataset1, dataset2, swap=FALSE) { 
        if (swap) { dataset <- rbind(dataset2, dataset1)) } 
        else { dataset <- rbind(dataset1, dataset2)) 
        } 
    return (list(dataset, dataset1, dataset2)) 
    } 

<span data-ttu-id="6fb5c-209">**Vizualizace výstup:** můžete také určit na výstupní port typu *vizualizace*, zobrazuje výstup z výstupu R grafiky zařízení a konzoly.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-209">**Visualization output:** You can also specify an output port of type *Visualization*, which displays the output from the R graphics device and console output.</span></span> <span data-ttu-id="6fb5c-210">Tento port není součástí výstup funkce R a nebudou v konfliktu s pořadí ostatních typů výstupní port.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-210">This port is not part of the R function output and does not interfere with the order of the other output port types.</span></span> <span data-ttu-id="6fb5c-211">Pokud chcete přidat do vlastní moduly vizualizace port, přidejte **výstup** element s hodnotou *vizualizace* pro jeho **typ** atribut:</span><span class="sxs-lookup"><span data-stu-id="6fb5c-211">To add a visualization port to the custom modules, add an **Output** element with a value of *Visualization* for its **type** attribute:</span></span>

    <Output id="deviceOutput" name="View Port" type="Visualization">
      <Description>View the R console graphics device output.</Description>
    </Output>

<span data-ttu-id="6fb5c-212">**Výstup pravidla:**</span><span class="sxs-lookup"><span data-stu-id="6fb5c-212">**Output Rules:**</span></span>

* <span data-ttu-id="6fb5c-213">Hodnota **id** atribut **výstup** element musí být platný název proměnné R.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-213">The value of the **id** attribute of the **Output** element must be a valid R variable name.</span></span>
* <span data-ttu-id="6fb5c-214">Hodnota **id** atribut **výstup** element nesmí být delší než 32 znaků.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-214">The value of the **id** attribute of the **Output** element must not be longer than 32 characters.</span></span>
* <span data-ttu-id="6fb5c-215">Hodnota **název** atribut **výstup** element nesmí být delší než 64 znaků.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-215">The value of the **name** attribute of the **Output** element must not be longer than 64 characters.</span></span>
* <span data-ttu-id="6fb5c-216">Hodnota **typ** atribut **výstup** element musí být *vizualizace*.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-216">The value of the **type** attribute of the **Output** element must be *Visualization*.</span></span>

### <a name="arguments"></a><span data-ttu-id="6fb5c-217">Argumenty</span><span class="sxs-lookup"><span data-stu-id="6fb5c-217">Arguments</span></span>
<span data-ttu-id="6fb5c-218">Další data mohou být předána do funkce R prostřednictvím modulu parametry, které jsou definovány v **argumenty** elementu.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-218">Additional data can be passed to the R function via module parameters which are defined in the **Arguments** element.</span></span> <span data-ttu-id="6fb5c-219">Tyto parametry se zobrazí v podokně nejvíce vpravo vlastnosti UI Machine Learning, pokud je vybrána modul.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-219">These parameters appear in the rightmost properties pane of the Machine Learning UI when the module is selected.</span></span> <span data-ttu-id="6fb5c-220">Argumenty může být jakýkoli z podporovaných typů nebo můžete vytvořit vlastní výčet v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-220">Arguments can be any of the supported types or you can create a custom enumeration when needed.</span></span> <span data-ttu-id="6fb5c-221">Podobně jako **porty** elementů **argumenty** elementy může mít volitelný **popis** element, který určuje text, který se zobrazí, když ukazatel myši Název parametru.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-221">Similar to the **Ports** elements, **Arguments** elements can have an optional **Description** element that specifies the text that appears when you hover the mouse over the parameter name.</span></span>
<span data-ttu-id="6fb5c-222">Volitelné vlastnosti pro modul, jako je například výchozí hodnota, minValue a maxValue lze přidat na některý argument jako atributy, které mají **vlastnosti** elementu.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-222">Optional properties for a module, such as defaultValue, minValue, and maxValue can be added to any argument as attributes to a **Properties** element.</span></span> <span data-ttu-id="6fb5c-223">Platný vlastnosti **vlastnosti** element závisí na typ argumentu a jsou popsány s typy argumentů podporované v další části.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-223">Valid properties for the **Properties** element depend on the argument type and are described with the supported argument types in the next section.</span></span> <span data-ttu-id="6fb5c-224">Argumenty s **isOptional** vlastnost nastavena na hodnotu **"true"** nevyžadují, aby uživatel zadal hodnotu.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-224">Arguments with the **isOptional** property set to **"true"** do not require the user to enter a value.</span></span> <span data-ttu-id="6fb5c-225">Pokud není zadána hodnota pro argument, není argument předaný funkci vstupního bodu.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-225">If a value is not provided to the argument, then the argument is not passed to the entry point function.</span></span> <span data-ttu-id="6fb5c-226">Argumenty funkce vstupního bodu, které jsou volitelné nutné explicitně zpracovávat funkce, například přiřadit výchozí hodnotu NULL v definici funkce vstupního bodu.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-226">Arguments of the entry point function that are optional need to be explicitly handled by the function, e.g. assigned a default value of NULL in the entry point function definition.</span></span> <span data-ttu-id="6fb5c-227">Nepovinný argument pouze vynutí jiných argument omezení, tj. min nebo max, pokud je hodnota zadaná uživatelem.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-227">An optional argument will only enforce the other argument constraints, i.e. min or max, if a value is provided by the user.</span></span>
<span data-ttu-id="6fb5c-228">Stejně jako u vstupy a výstupy, je velmi důležité, že všechny parametry mají hodnoty jedinečné id s nimi spojených.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-228">As with inputs and outputs, it is critical that each of the parameters have unique id values associated with them.</span></span> <span data-ttu-id="6fb5c-229">V našem příkladu úvodní přidružené id a parametru se *swap*.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-229">In our quick start example the associated id/parameter was *swap*.</span></span>

### <a name="arg-element"></a><span data-ttu-id="6fb5c-230">Arg – element</span><span class="sxs-lookup"><span data-stu-id="6fb5c-230">Arg element</span></span>
<span data-ttu-id="6fb5c-231">Parametr modulu je definován pomocí **Arg** podřízený element **argumenty** část definičního soubor XML.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-231">A module parameter is defined using the **Arg** child element of the **Arguments** section of the XML definition file.</span></span> <span data-ttu-id="6fb5c-232">Stejně jako u podřízených elementů v **porty** části řazení parametry v **argumenty** oddíl definuje rozložení v UX</span><span class="sxs-lookup"><span data-stu-id="6fb5c-232">As with the child elements in the **Ports** section, the ordering of parameters in the **Arguments** section defines the layout encountered in the UX.</span></span> <span data-ttu-id="6fb5c-233">Parametry zobrazí shora dolů v uživatelském rozhraní ve stejném pořadí, ve kterém jsou definovány v souboru XML.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-233">The parameters appear from top down in the UI in the same order in which they are defined in the XML file.</span></span> <span data-ttu-id="6fb5c-234">Typy podporované nástrojem Machine Learning pro parametry jsou zde uvedeny.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-234">The types supported by Machine Learning for parameters are listed here.</span></span> 

<span data-ttu-id="6fb5c-235">**int** – parametru typu (32 bitů) celé číslo.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-235">**int** – an Integer (32-bit) type parameter.</span></span>

    <Arg id="intValue1" name="Int Param" type="int">
        <Properties min="0" max="100" default="0" />
        <Description>Integer Parameter</Description>
    </Arg>


* <span data-ttu-id="6fb5c-236">*Volitelné vlastnosti*: **min**, **maximální**, **výchozí** a **isOptional**</span><span class="sxs-lookup"><span data-stu-id="6fb5c-236">*Optional Properties*: **min**, **max**, **default** and **isOptional**</span></span>

<span data-ttu-id="6fb5c-237">**dvojité** – parametr typu double.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-237">**double** – a double type parameter.</span></span>

    <Arg id="doubleValue1" name="Double Param" type="double">
        <Properties min="0.000" max="0.999" default="0.3" />
        <Description>Double Parameter</Description>
    </Arg>


* <span data-ttu-id="6fb5c-238">*Volitelné vlastnosti*: **min**, **maximální**, **výchozí** a **isOptional**</span><span class="sxs-lookup"><span data-stu-id="6fb5c-238">*Optional Properties*: **min**, **max**, **default** and **isOptional**</span></span>

<span data-ttu-id="6fb5c-239">**BOOL** – logického parametru, která je reprezentována zaškrtávací políčko UX</span><span class="sxs-lookup"><span data-stu-id="6fb5c-239">**bool** – a Boolean parameter that is represented by a check-box in UX.</span></span>

    <Arg id="boolValue1" name="Boolean Param" type="bool">
        <Properties default="true" />
        <Description>Boolean Parameter</Description>
    </Arg>



* <span data-ttu-id="6fb5c-240">*Volitelné vlastnosti*: **výchozí** -false v případě není nastavena.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-240">*Optional Properties*: **default** - false if not set</span></span>

<span data-ttu-id="6fb5c-241">**řetězec**: standardní řetězec</span><span class="sxs-lookup"><span data-stu-id="6fb5c-241">**string**: a standard string</span></span>

    <Arg id="stringValue1" name="My string Param" type="string">
        <Properties isOptional="true" />
        <Description>String Parameter 1</Description>
    </Arg>    

* <span data-ttu-id="6fb5c-242">*Volitelné vlastnosti*: **výchozí** a **isOptional**</span><span class="sxs-lookup"><span data-stu-id="6fb5c-242">*Optional Properties*: **default** and **isOptional**</span></span>

<span data-ttu-id="6fb5c-243">**ColumnPicker**: Parametr výběr sloupce.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-243">**ColumnPicker**: a column selection parameter.</span></span> <span data-ttu-id="6fb5c-244">Tento typ se vykreslí v uživatelského jako zvolit sloupce.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-244">This type renders in the UX as a column chooser.</span></span> <span data-ttu-id="6fb5c-245">**Vlastnost** element zde slouží k určení id portu, ze které se vybírají sloupce, které musí být cílový port typ *DataTable*.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-245">The **Property** element is used here to specify the id of the port from which columns are selected, where the target port type must be *DataTable*.</span></span> <span data-ttu-id="6fb5c-246">Výsledek výběr sloupce je předaný funkci R jako seznam řetězců obsahujících názvy vybraných sloupců.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-246">The result of the column selection is passed to the R function as a list of strings containing the selected column names.</span></span> 

        <Arg id="colset" name="Column set" type="ColumnPicker">      
          <Properties portId="datasetIn1" allowedTypes="Numeric" default="NumericAll"/>
          <Description>Column set</Description>
        </Arg>


* <span data-ttu-id="6fb5c-247">*Požadované vlastnosti*: **portId** -odpovídá id elementu vstupu s typem *DataTable*.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-247">*Required Properties*: **portId** - matches the id of an Input element with type *DataTable*.</span></span>
* <span data-ttu-id="6fb5c-248">*Volitelné vlastnosti*:</span><span class="sxs-lookup"><span data-stu-id="6fb5c-248">*Optional Properties*:</span></span>
  
  * <span data-ttu-id="6fb5c-249">**allowedTypes** -filtry typy sloupci, ze kterého můžete vybrat.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-249">**allowedTypes** - Filters the column types from which you can pick.</span></span> <span data-ttu-id="6fb5c-250">Platné hodnoty patří:</span><span class="sxs-lookup"><span data-stu-id="6fb5c-250">Valid values include:</span></span> 
    
    * <span data-ttu-id="6fb5c-251">číselné</span><span class="sxs-lookup"><span data-stu-id="6fb5c-251">Numeric</span></span>
    * <span data-ttu-id="6fb5c-252">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="6fb5c-252">Boolean</span></span>
    * <span data-ttu-id="6fb5c-253">Kategorické</span><span class="sxs-lookup"><span data-stu-id="6fb5c-253">Categorical</span></span>
    * <span data-ttu-id="6fb5c-254">Řetězec</span><span class="sxs-lookup"><span data-stu-id="6fb5c-254">String</span></span>
    * <span data-ttu-id="6fb5c-255">Štítek</span><span class="sxs-lookup"><span data-stu-id="6fb5c-255">Label</span></span>
    * <span data-ttu-id="6fb5c-256">Funkce</span><span class="sxs-lookup"><span data-stu-id="6fb5c-256">Feature</span></span>
    * <span data-ttu-id="6fb5c-257">Hodnocení</span><span class="sxs-lookup"><span data-stu-id="6fb5c-257">Score</span></span>
    * <span data-ttu-id="6fb5c-258">Všechny</span><span class="sxs-lookup"><span data-stu-id="6fb5c-258">All</span></span>
  * <span data-ttu-id="6fb5c-259">**výchozí** -platný výchozí možnosti pro výběr sloupce patří:</span><span class="sxs-lookup"><span data-stu-id="6fb5c-259">**default** - Valid default selections for the column picker include:</span></span> 
    
    * <span data-ttu-id="6fb5c-260">Žádný</span><span class="sxs-lookup"><span data-stu-id="6fb5c-260">None</span></span>
    * <span data-ttu-id="6fb5c-261">NumericFeature</span><span class="sxs-lookup"><span data-stu-id="6fb5c-261">NumericFeature</span></span>
    * <span data-ttu-id="6fb5c-262">NumericLabel</span><span class="sxs-lookup"><span data-stu-id="6fb5c-262">NumericLabel</span></span>
    * <span data-ttu-id="6fb5c-263">NumericScore</span><span class="sxs-lookup"><span data-stu-id="6fb5c-263">NumericScore</span></span>
    * <span data-ttu-id="6fb5c-264">NumericAll</span><span class="sxs-lookup"><span data-stu-id="6fb5c-264">NumericAll</span></span>
    * <span data-ttu-id="6fb5c-265">BooleanFeature</span><span class="sxs-lookup"><span data-stu-id="6fb5c-265">BooleanFeature</span></span>
    * <span data-ttu-id="6fb5c-266">BooleanLabel</span><span class="sxs-lookup"><span data-stu-id="6fb5c-266">BooleanLabel</span></span>
    * <span data-ttu-id="6fb5c-267">BooleanScore</span><span class="sxs-lookup"><span data-stu-id="6fb5c-267">BooleanScore</span></span>
    * <span data-ttu-id="6fb5c-268">BooleanAll</span><span class="sxs-lookup"><span data-stu-id="6fb5c-268">BooleanAll</span></span>
    * <span data-ttu-id="6fb5c-269">CategoricalFeature</span><span class="sxs-lookup"><span data-stu-id="6fb5c-269">CategoricalFeature</span></span>
    * <span data-ttu-id="6fb5c-270">CategoricalLabel</span><span class="sxs-lookup"><span data-stu-id="6fb5c-270">CategoricalLabel</span></span>
    * <span data-ttu-id="6fb5c-271">CategoricalScore</span><span class="sxs-lookup"><span data-stu-id="6fb5c-271">CategoricalScore</span></span>
    * <span data-ttu-id="6fb5c-272">CategoricalAll</span><span class="sxs-lookup"><span data-stu-id="6fb5c-272">CategoricalAll</span></span>
    * <span data-ttu-id="6fb5c-273">StringFeature</span><span class="sxs-lookup"><span data-stu-id="6fb5c-273">StringFeature</span></span>
    * <span data-ttu-id="6fb5c-274">StringLabel</span><span class="sxs-lookup"><span data-stu-id="6fb5c-274">StringLabel</span></span>
    * <span data-ttu-id="6fb5c-275">StringScore</span><span class="sxs-lookup"><span data-stu-id="6fb5c-275">StringScore</span></span>
    * <span data-ttu-id="6fb5c-276">StringAll</span><span class="sxs-lookup"><span data-stu-id="6fb5c-276">StringAll</span></span>
    * <span data-ttu-id="6fb5c-277">AllLabel</span><span class="sxs-lookup"><span data-stu-id="6fb5c-277">AllLabel</span></span>
    * <span data-ttu-id="6fb5c-278">AllFeature</span><span class="sxs-lookup"><span data-stu-id="6fb5c-278">AllFeature</span></span>
    * <span data-ttu-id="6fb5c-279">AllScore</span><span class="sxs-lookup"><span data-stu-id="6fb5c-279">AllScore</span></span>
    * <span data-ttu-id="6fb5c-280">Všechny</span><span class="sxs-lookup"><span data-stu-id="6fb5c-280">All</span></span>

<span data-ttu-id="6fb5c-281">**Rozevírací seznam**: seznam uživatelská výčtového (rozevírací).</span><span class="sxs-lookup"><span data-stu-id="6fb5c-281">**DropDown**: a user-specified enumerated (dropdown) list.</span></span> <span data-ttu-id="6fb5c-282">Rozevírací seznam položek, které jsou určené v rámci **vlastnosti** pomocí elementu **položky** elementu.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-282">The dropdown items are specified within the **Properties** element using an **Item** element.</span></span> <span data-ttu-id="6fb5c-283">**Id** pro každou **položky** musí být jedinečný a platná proměnná R.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-283">The **id** for each **Item** must be unique and a valid R variable.</span></span> <span data-ttu-id="6fb5c-284">Hodnota **název** z **položky** slouží jako text, který se zobrazí a hodnotu, která je předaný funkci R.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-284">The value of the **name** of an **Item** serves as both the text that you see and the value that is passed to the R function.</span></span>

    <Arg id="color" name="Color" type="DropDown">
      <Properties default="red">
        <Item id="red" name="Red Value"/>
        <Item id="green" name="Green Value"/>
        <Item id="blue" name="Blue Value"/>
      </Properties>
      <Description>Select a color.</Description>
    </Arg>    

* <span data-ttu-id="6fb5c-285">*Volitelné vlastnosti*:</span><span class="sxs-lookup"><span data-stu-id="6fb5c-285">*Optional Properties*:</span></span>
  * <span data-ttu-id="6fb5c-286">**výchozí** -hodnotu pro vlastnost výchozí, musí být shodná s hodnotou id z jednoho z **položky** elementy.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-286">**default** - The value for the default property must correspond with an id value from one of the **Item** elements.</span></span>

### <a name="auxiliary-files"></a><span data-ttu-id="6fb5c-287">Pomocné soubory</span><span class="sxs-lookup"><span data-stu-id="6fb5c-287">Auxiliary Files</span></span>
<span data-ttu-id="6fb5c-288">Všechny soubory, které je umístěn v souboru ZIP vlastní modul bude k dispozici pro použití v době spuštění.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-288">Any file that is placed in your custom module ZIP file is going to be available for use during execution time.</span></span> <span data-ttu-id="6fb5c-289">Všechny adresáře struktury přítomen se zachovají.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-289">Any directory structures present are preserved.</span></span> <span data-ttu-id="6fb5c-290">To znamená, že soubor sourcing funguje stejné místně a při provádění Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-290">This means that file sourcing works the same locally and in Azure Machine Learning execution.</span></span> 

> [!NOTE]
> <span data-ttu-id="6fb5c-291">Všimněte si, že všechny soubory extrahují do adresáře, src, proto by měly mít všechny cesty ' src /' předponu.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-291">Notice that all files are extracted to ‘src’ directory so all paths should have ‘src/’ prefix.</span></span>
> 
> 

<span data-ttu-id="6fb5c-292">Řekněme například, že chcete odebrat všechny řádky s NAs z datové sady a také odebrat všechny duplicitní řádky před výstup do CustomAddRows a jste již zapsány R funkce, která nemá v souboru RemoveDupNARows.R:</span><span class="sxs-lookup"><span data-stu-id="6fb5c-292">For example, say you want to remove any rows with NAs from the dataset, and also remove any duplicate rows, before outputting it into CustomAddRows, and you’ve already written an R function that does that in a file RemoveDupNARows.R:</span></span>

    RemoveDupNARows <- function(dataFrame) {
        #Remove Duplicate Rows:
        dataFrame <- unique(dataFrame)
        #Remove Rows with NAs:
        finalDataFrame <- dataFrame[complete.cases(dataFrame),]
        return(finalDataFrame)
    }
<span data-ttu-id="6fb5c-293">Zdrojem může být pomocného souboru RemoveDupNARows.R ve funkci CustomAddRows:</span><span class="sxs-lookup"><span data-stu-id="6fb5c-293">You can source the auxiliary file RemoveDupNARows.R in the CustomAddRows function:</span></span>

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

<span data-ttu-id="6fb5c-294">V dalším kroku nahrajte soubor zip obsahující 'CustomAddRows.R', 'CustomAddRows.xml' a 'RemoveDupNARows.R' jako vlastní modul R.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-294">Next, upload a zip file containing ‘CustomAddRows.R’, ‘CustomAddRows.xml’, and ‘RemoveDupNARows.R’ as a custom R module.</span></span>

## <a name="execution-environment"></a><span data-ttu-id="6fb5c-295">Prostředí pro spuštění</span><span class="sxs-lookup"><span data-stu-id="6fb5c-295">Execution Environment</span></span>
<span data-ttu-id="6fb5c-296">Prostředí pro spuštění skriptu R používá stejnou verzi nástroje R jako **spustit skript jazyka R** modulu a můžete použít stejné výchozí balíčky.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-296">The execution environment for the R script uses the same version of R as the **Execute R Script** module and can use the same default packages.</span></span> <span data-ttu-id="6fb5c-297">Můžete také přidat další balíčky R pro vlastní modul zahrnutím v balíčku zip vlastní modul.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-297">You can also add additional R packages to your custom module by including them in the custom module zip package.</span></span> <span data-ttu-id="6fb5c-298">Jenom je načte ve vašem skriptu R stejně jako v prostředí R.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-298">Just load them in your R script as you would in your own R environment.</span></span> 

<span data-ttu-id="6fb5c-299">**Omezení prostředí pro spuštění** zahrnují:</span><span class="sxs-lookup"><span data-stu-id="6fb5c-299">**Limitations of the execution environment** include:</span></span>

* <span data-ttu-id="6fb5c-300">Systém souborů bez trvalé: soubory zapsané při spuštění vlastního modulu nejsou trvalé napříč více spustí stejný modulu.</span><span class="sxs-lookup"><span data-stu-id="6fb5c-300">Non-persistent file system: Files written when the custom module is run are not persisted across multiple runs of the same module.</span></span>
* <span data-ttu-id="6fb5c-301">Žádný přístup k síti</span><span class="sxs-lookup"><span data-stu-id="6fb5c-301">No network access</span></span>

