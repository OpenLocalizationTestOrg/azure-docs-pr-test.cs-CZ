---
title: "Vývoj sestavení U-SQL pro úlohy Azure Data Lake Analytics | Microsoft Docs"
description: "Další informace jak vyvíjet sestavení, které chcete použít a znovu použít v úloh Data Lake Analytics. "
services: data-lake-analytics
documentationcenter: 
author: jejiang
manager: jhubbard
editor: cgronlun
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/30/2016
ms.author: jejiang
ms.openlocfilehash: c49f80f8dcd330d7f46726241e7178351b9cc28f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="develop-u-sql-assemblies-for-azure-data-lake-analytics-jobs"></a><span data-ttu-id="eb0ac-103">Vývoj sestavení U-SQL pro úlohy Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="eb0ac-103">Develop U-SQL assemblies for Azure Data Lake Analytics jobs</span></span>
<span data-ttu-id="eb0ac-104">Zjistěte, jak chcete-li kódu do sestavení, které chcete použít a znovu použít v úloh Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="eb0ac-104">Learn how to turn code-behind into assemblies to be used and reused in Data Lake Analytics jobs.</span></span> 

<span data-ttu-id="eb0ac-105">U-SQL umožňuje snadno přidat vlastní kód v jazyce .net, například C#, VB.Net nebo F #.</span><span class="sxs-lookup"><span data-stu-id="eb0ac-105">U-SQL makes it easy to add your own custom code in .Net languages, such as C#, VB.Net or F#.</span></span> <span data-ttu-id="eb0ac-106">Dokonce můžete nasadit vlastní modul runtime pro podporu dalších jazyků.</span><span class="sxs-lookup"><span data-stu-id="eb0ac-106">You can even deploy your own runtime to support other languages.</span></span>

<span data-ttu-id="eb0ac-107">Nejjednodušší způsob, jak použít vlastní kód je použití nástroje Data Lake pro Visual Studio kódu možnosti.</span><span class="sxs-lookup"><span data-stu-id="eb0ac-107">The easiest way to use custom code is to use the Data Lake Tools for Visual Studio’s code-behind capabilities.</span></span> <span data-ttu-id="eb0ac-108">Další informace najdete v tématu [kurz: vývoj skriptů U-SQL pomocí nástrojů Data Lake pro Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="eb0ac-108">For more information, see [Tutorial: develop U-SQL scripts using Data Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span></span> <span data-ttu-id="eb0ac-109">Existuje několik nevýhody použití kódu:</span><span class="sxs-lookup"><span data-stu-id="eb0ac-109">There are a few drawbacks of using code-behind:</span></span>

- <span data-ttu-id="eb0ac-110">Zdrojový kód získá nahrát pro každý skript odeslání.</span><span class="sxs-lookup"><span data-stu-id="eb0ac-110">The source code gets uploaded for every script submission.</span></span>
- <span data-ttu-id="eb0ac-111">kódu nemohou být sdíleny s ostatními úlohami.</span><span class="sxs-lookup"><span data-stu-id="eb0ac-111">code-behind cannot be shared with other jobs.</span></span>

<span data-ttu-id="eb0ac-112">Chcete-li vyřešit tyto nevýhody, můžete zapnout kódu do sestavení a registraci sestavení do katalogu Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="eb0ac-112">To address these drawbacks, you can turn code-behind into assemblies, and register the assemblies to the Data Lake Analytics catalog.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="eb0ac-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="eb0ac-113">Prerequisites</span></span>
* <span data-ttu-id="eb0ac-114">Visual Studio 2017, Visual Studio 2015, Visual Studio 2013 update 4 nebo Visual Studio 2012 s nainstalovaným Visual C++</span><span class="sxs-lookup"><span data-stu-id="eb0ac-114">Visual Studio 2017, Visual Studio 2015, Visual Studio 2013 update 4, or Visual Studio 2012 with Visual C++ Installed</span></span>
* <span data-ttu-id="eb0ac-115">Microsoft Azure SDK pro .NET verze 2.5 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="eb0ac-115">Microsoft Azure SDK for .NET version 2.5 or above.</span></span>  <span data-ttu-id="eb0ac-116">Nainstalujte ji pomocí instalačního programu webové platformy nebo instalační program Visual Studio</span><span class="sxs-lookup"><span data-stu-id="eb0ac-116">Install it using the Web platform installer or Visual Studio Installer</span></span>
* <span data-ttu-id="eb0ac-117">Účet Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="eb0ac-117">A Data Lake Analytics account.</span></span>  <span data-ttu-id="eb0ac-118">Viz [Začínáme s Azure Data Lake Analytics pomocí webu Azure Portal](data-lake-analytics-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="eb0ac-118">See [Get Started with Azure Data Lake Analytics using Azure portal](data-lake-analytics-get-started-portal.md).</span></span>
* <span data-ttu-id="eb0ac-119">Projděte [Začínáme s Azure Data Lake Analytics U-SQL Studio](data-lake-analytics-u-sql-get-started.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="eb0ac-119">Go through the [Get started with Azure Data Lake Analytics U-SQL Studio](data-lake-analytics-u-sql-get-started.md) tutorial.</span></span>
* <span data-ttu-id="eb0ac-120">Připojte k Azure.</span><span class="sxs-lookup"><span data-stu-id="eb0ac-120">Connect to Azure.</span></span>
* <span data-ttu-id="eb0ac-121">Nahrát zdrojová data, najdete v části [Začínáme s Azure Data Lake Analytics U-SQL Studio](data-lake-analytics-u-sql-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="eb0ac-121">Upload the source data, see [Get started with Azure Data Lake Analytics U-SQL Studio](data-lake-analytics-u-sql-get-started.md).</span></span> 

## <a name="develop-assemblies-for-u-sql"></a><span data-ttu-id="eb0ac-122">Vývoj sestavení pro U-SQL</span><span class="sxs-lookup"><span data-stu-id="eb0ac-122">Develop assemblies for U-SQL</span></span>

<span data-ttu-id="eb0ac-123">**K vytvoření a odeslání úlohy U-SQL**</span><span class="sxs-lookup"><span data-stu-id="eb0ac-123">**To create and submit a U-SQL job**</span></span>

1. <span data-ttu-id="eb0ac-124">V nabídce **Soubor** klikněte na položku **Nový** a potom klikněte na položku **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="eb0ac-124">From the **File** menu, click **New**, and then click **Project**.</span></span>
2. <span data-ttu-id="eb0ac-125">Rozbalte položku **nainstalovaná**, **šablony**, **Azure Data Lake**, **U-SQL(ADLA)**, vyberte **knihovny tříd (pro aplikace U-SQL)** šablony a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="eb0ac-125">Expand **Installed**, **Templates**, **Azure Data Lake**, **U-SQL(ADLA)**, select the **Class Library (For U-SQL Application)** template, and then click **OK**.</span></span>
3. <span data-ttu-id="eb0ac-126">Zápis kódu v Class1.cs.</span><span class="sxs-lookup"><span data-stu-id="eb0ac-126">Write your code in Class1.cs.</span></span>  <span data-ttu-id="eb0ac-127">Zde je ukázka kódu.</span><span class="sxs-lookup"><span data-stu-id="eb0ac-127">The following is a code sample.</span></span>

        using Microsoft.Analytics.Interfaces;

        namespace USQLApplication_codebehind
        {
            [SqlUserDefinedProcessor]
            public class MyProcessor : IProcessor
            {
                public override IRow Process(IRow input, IUpdatableRow output)
                {
                    output.Set(0, input.Get<string>(0));
                    output.Set(0, input.Get<string>(0));
                    return output.AsReadOnly();
                }
            }
        }
4. <span data-ttu-id="eb0ac-128">Klikněte na tlačítko **sestavení** nabídce a pak klikněte na tlačítko **sestavit řešení** vytvořit knihovnu dll.</span><span class="sxs-lookup"><span data-stu-id="eb0ac-128">Click the **Build** menu, and then click **Build Solution** to create the dll.</span></span>

## <a name="register-assemblies"></a><span data-ttu-id="eb0ac-129">Registrace sestavení</span><span class="sxs-lookup"><span data-stu-id="eb0ac-129">Register assemblies</span></span>

<span data-ttu-id="eb0ac-130">V tématu [použití Data Lake Analytics(U-SQL) katalogu](data-lake-analytics-use-u-sql-catalog.md).</span><span class="sxs-lookup"><span data-stu-id="eb0ac-130">See [Use Data Lake Analytics(U-SQL) catalog](data-lake-analytics-use-u-sql-catalog.md).</span></span>


## <a name="use-the-assemblies"></a><span data-ttu-id="eb0ac-131">Použití sestavení</span><span class="sxs-lookup"><span data-stu-id="eb0ac-131">Use the assemblies</span></span>

<span data-ttu-id="eb0ac-132">V tématu [použití nástroje Azure Data Lake pro Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md).</span><span class="sxs-lookup"><span data-stu-id="eb0ac-132">See [Use the Azure Data Lake Tools for Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="eb0ac-133">Viz také</span><span class="sxs-lookup"><span data-stu-id="eb0ac-133">See also</span></span>
* [<span data-ttu-id="eb0ac-134">Začínáme s Data Lake Analytics pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="eb0ac-134">Get started with Data Lake Analytics using PowerShell</span></span>](data-lake-analytics-get-started-powershell.md)
* [<span data-ttu-id="eb0ac-135">Začínáme s Data Lake Analytics pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="eb0ac-135">Get started with Data Lake Analytics using the Azure portal</span></span>](data-lake-analytics-get-started-portal.md)
* [<span data-ttu-id="eb0ac-136">Pomocí nástrojů Data Lake pro Visual Studio pro vývoj aplikací U-SQL</span><span class="sxs-lookup"><span data-stu-id="eb0ac-136">Use Data Lake Tools for Visual Studio for developing U-SQL applications</span></span>](data-lake-analytics-data-lake-tools-get-started.md)
* [<span data-ttu-id="eb0ac-137">Použití Data Lake Analytics(U-SQL) katalogu</span><span class="sxs-lookup"><span data-stu-id="eb0ac-137">Use Data Lake Analytics(U-SQL) catalog</span></span>](data-lake-analytics-use-u-sql-catalog.md)
* [<span data-ttu-id="eb0ac-138">Použití nástrojů Azure Data Lake pro Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="eb0ac-138">Use the Azure Data Lake Tools for Visual Studio Code</span></span>](data-lake-analytics-data-lake-tools-for-vscode.md)