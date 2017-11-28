---
title: "sestavení aaaDevelop U-SQL pro úlohy Azure Data Lake Analytics | Microsoft Docs"
description: "Zjistěte, jak toodevelop sestavení toobe používá a znovu použít v Data Lake Analytics úlohy. "
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
ms.openlocfilehash: 86dd17b25e0967306ed36bb5b7f3178d9409d53d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="develop-u-sql-assemblies-for-azure-data-lake-analytics-jobs"></a><span data-ttu-id="de23f-103">Vývoj sestavení U-SQL pro úlohy Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="de23f-103">Develop U-SQL assemblies for Azure Data Lake Analytics jobs</span></span>
<span data-ttu-id="de23f-104">Zjistěte, jak tooturn kódu do sestavení toobe používá a znovu použít v úloh Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="de23f-104">Learn how tooturn code-behind into assemblies toobe used and reused in Data Lake Analytics jobs.</span></span> 

<span data-ttu-id="de23f-105">U-SQL umožňuje snadno tooadd vlastní vlastní kód v jazyce .net, například C#, VB.Net nebo F #.</span><span class="sxs-lookup"><span data-stu-id="de23f-105">U-SQL makes it easy tooadd your own custom code in .Net languages, such as C#, VB.Net or F#.</span></span> <span data-ttu-id="de23f-106">Vlastní modul runtime toosupport můžete nasadit i další jazyky.</span><span class="sxs-lookup"><span data-stu-id="de23f-106">You can even deploy your own runtime toosupport other languages.</span></span>

<span data-ttu-id="de23f-107">Hello nejjednodušší způsob, jak toouse vlastní kód je toouse hello nástrojů Data Lake pro Visual Studio kódu možnosti.</span><span class="sxs-lookup"><span data-stu-id="de23f-107">hello easiest way toouse custom code is toouse hello Data Lake Tools for Visual Studio’s code-behind capabilities.</span></span> <span data-ttu-id="de23f-108">Další informace najdete v tématu [kurz: vývoj skriptů U-SQL pomocí nástrojů Data Lake pro Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="de23f-108">For more information, see [Tutorial: develop U-SQL scripts using Data Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span></span> <span data-ttu-id="de23f-109">Existuje několik nevýhody použití kódu:</span><span class="sxs-lookup"><span data-stu-id="de23f-109">There are a few drawbacks of using code-behind:</span></span>

- <span data-ttu-id="de23f-110">pro každý skript odeslání získá nahrát Hello zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="de23f-110">hello source code gets uploaded for every script submission.</span></span>
- <span data-ttu-id="de23f-111">kódu nemohou být sdíleny s ostatními úlohami.</span><span class="sxs-lookup"><span data-stu-id="de23f-111">code-behind cannot be shared with other jobs.</span></span>

<span data-ttu-id="de23f-112">tooaddress tyto nevýhody můžete zapnout kódu do sestavení a zaregistrovat hello sestavení toohello Data Lake Analytics katalogu.</span><span class="sxs-lookup"><span data-stu-id="de23f-112">tooaddress these drawbacks, you can turn code-behind into assemblies, and register hello assemblies toohello Data Lake Analytics catalog.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="de23f-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="de23f-113">Prerequisites</span></span>
* <span data-ttu-id="de23f-114">Visual Studio 2017, Visual Studio 2015, Visual Studio 2013 update 4 nebo Visual Studio 2012 s nainstalovaným Visual C++</span><span class="sxs-lookup"><span data-stu-id="de23f-114">Visual Studio 2017, Visual Studio 2015, Visual Studio 2013 update 4, or Visual Studio 2012 with Visual C++ Installed</span></span>
* <span data-ttu-id="de23f-115">Microsoft Azure SDK pro .NET verze 2.5 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="de23f-115">Microsoft Azure SDK for .NET version 2.5 or above.</span></span>  <span data-ttu-id="de23f-116">Nainstalujte ji pomocí instalačního programu webové platformy hello nebo instalační program Visual Studio</span><span class="sxs-lookup"><span data-stu-id="de23f-116">Install it using hello Web platform installer or Visual Studio Installer</span></span>
* <span data-ttu-id="de23f-117">Účet Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="de23f-117">A Data Lake Analytics account.</span></span>  <span data-ttu-id="de23f-118">Viz [Začínáme s Azure Data Lake Analytics pomocí webu Azure Portal](data-lake-analytics-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="de23f-118">See [Get Started with Azure Data Lake Analytics using Azure portal](data-lake-analytics-get-started-portal.md).</span></span>
* <span data-ttu-id="de23f-119">Projděte hello [Začínáme s Azure Data Lake Analytics U-SQL Studio](data-lake-analytics-u-sql-get-started.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="de23f-119">Go through hello [Get started with Azure Data Lake Analytics U-SQL Studio](data-lake-analytics-u-sql-get-started.md) tutorial.</span></span>
* <span data-ttu-id="de23f-120">Připojte tooAzure.</span><span class="sxs-lookup"><span data-stu-id="de23f-120">Connect tooAzure.</span></span>
* <span data-ttu-id="de23f-121">Nahrání hello zdroje dat naleznete v tématu [Začínáme s Azure Data Lake Analytics U-SQL Studio](data-lake-analytics-u-sql-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="de23f-121">Upload hello source data, see [Get started with Azure Data Lake Analytics U-SQL Studio](data-lake-analytics-u-sql-get-started.md).</span></span> 

## <a name="develop-assemblies-for-u-sql"></a><span data-ttu-id="de23f-122">Vývoj sestavení pro U-SQL</span><span class="sxs-lookup"><span data-stu-id="de23f-122">Develop assemblies for U-SQL</span></span>

<span data-ttu-id="de23f-123">**toocreate a odeslání úlohy U-SQL**</span><span class="sxs-lookup"><span data-stu-id="de23f-123">**toocreate and submit a U-SQL job**</span></span>

1. <span data-ttu-id="de23f-124">Z hello **soubor** nabídky, klikněte na tlačítko **nový**a potom klikněte na **projektu**.</span><span class="sxs-lookup"><span data-stu-id="de23f-124">From hello **File** menu, click **New**, and then click **Project**.</span></span>
2. <span data-ttu-id="de23f-125">Rozbalte položku **nainstalovaná**, **šablony**, **Azure Data Lake**, **U-SQL(ADLA)**, vyberte hello **knihovny tříd (pro U-SQL Aplikace)** šablony a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="de23f-125">Expand **Installed**, **Templates**, **Azure Data Lake**, **U-SQL(ADLA)**, select hello **Class Library (For U-SQL Application)** template, and then click **OK**.</span></span>
3. <span data-ttu-id="de23f-126">Zápis kódu v Class1.cs.</span><span class="sxs-lookup"><span data-stu-id="de23f-126">Write your code in Class1.cs.</span></span>  <span data-ttu-id="de23f-127">Hello Následuje ukázka kódu.</span><span class="sxs-lookup"><span data-stu-id="de23f-127">hello following is a code sample.</span></span>

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
4. <span data-ttu-id="de23f-128">Klikněte na tlačítko hello **sestavení** nabídce a pak klikněte na tlačítko **sestavit řešení** toocreate hello dll.</span><span class="sxs-lookup"><span data-stu-id="de23f-128">Click hello **Build** menu, and then click **Build Solution** toocreate hello dll.</span></span>

## <a name="register-assemblies"></a><span data-ttu-id="de23f-129">Registrace sestavení</span><span class="sxs-lookup"><span data-stu-id="de23f-129">Register assemblies</span></span>

<span data-ttu-id="de23f-130">V tématu [použití Data Lake Analytics(U-SQL) katalogu](data-lake-analytics-use-u-sql-catalog.md).</span><span class="sxs-lookup"><span data-stu-id="de23f-130">See [Use Data Lake Analytics(U-SQL) catalog](data-lake-analytics-use-u-sql-catalog.md).</span></span>


## <a name="use-hello-assemblies"></a><span data-ttu-id="de23f-131">Použití sestavení hello</span><span class="sxs-lookup"><span data-stu-id="de23f-131">Use hello assemblies</span></span>

<span data-ttu-id="de23f-132">V tématu [pomocí nástrojů hello Azure Data Lake pro Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md).</span><span class="sxs-lookup"><span data-stu-id="de23f-132">See [Use hello Azure Data Lake Tools for Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="de23f-133">Viz také</span><span class="sxs-lookup"><span data-stu-id="de23f-133">See also</span></span>
* [<span data-ttu-id="de23f-134">Začínáme s Data Lake Analytics pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="de23f-134">Get started with Data Lake Analytics using PowerShell</span></span>](data-lake-analytics-get-started-powershell.md)
* [<span data-ttu-id="de23f-135">Začínáme s Data Lake Analytics pomocí portálu Azure hello</span><span class="sxs-lookup"><span data-stu-id="de23f-135">Get started with Data Lake Analytics using hello Azure portal</span></span>](data-lake-analytics-get-started-portal.md)
* [<span data-ttu-id="de23f-136">Pomocí nástrojů Data Lake pro Visual Studio pro vývoj aplikací U-SQL</span><span class="sxs-lookup"><span data-stu-id="de23f-136">Use Data Lake Tools for Visual Studio for developing U-SQL applications</span></span>](data-lake-analytics-data-lake-tools-get-started.md)
* [<span data-ttu-id="de23f-137">Použití Data Lake Analytics(U-SQL) katalogu</span><span class="sxs-lookup"><span data-stu-id="de23f-137">Use Data Lake Analytics(U-SQL) catalog</span></span>](data-lake-analytics-use-u-sql-catalog.md)
* [<span data-ttu-id="de23f-138">Pomocí nástrojů hello Azure Data Lake pro Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="de23f-138">Use hello Azure Data Lake Tools for Visual Studio Code</span></span>](data-lake-analytics-data-lake-tools-for-vscode.md)
