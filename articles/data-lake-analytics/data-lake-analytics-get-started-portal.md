---
title: "aaaGet Začínáme s Azure Data Lake Analytics pomocí portálu Azure | Microsoft Docs"
description: "Zjistěte, jak toouse hello Azure toocreate portálu účtu Data Lake Analytics, vytvoření úlohy Data Lake Analytics pomocí U-SQL a odeslání úlohy hello. "
services: data-lake-analytics
documentationcenter: 
author: edmacauley
manager: jhubbard
editor: cgronlun
ms.assetid: b1584d16-e0d2-4019-ad1f-f04be8c5b430
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/21/2017
ms.author: edmaca
ms.openlocfilehash: 6bb54404fa42cfed25b18bc2bfb7c72e6c361149
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-data-lake-analytics-using-azure-portal"></a><span data-ttu-id="ef105-103">Začínáme s Azure Data Lake Analytics s využitím webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="ef105-103">Get started with Azure Data Lake Analytics using Azure portal</span></span>
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

<span data-ttu-id="ef105-104">Zjistěte, jak toouse hello Azure toocreate portálu účtů Azure Data Lake Analytics, definování úloh v [U-SQL](data-lake-analytics-u-sql-get-started.md)a odeslání úlohy toohello Data Lake Analytics služby.</span><span class="sxs-lookup"><span data-stu-id="ef105-104">Learn how toouse hello Azure portal toocreate Azure Data Lake Analytics accounts, define jobs in [U-SQL](data-lake-analytics-u-sql-get-started.md), and submit jobs toohello Data Lake Analytics service.</span></span> <span data-ttu-id="ef105-105">Další informace o Data Lake Analytics najdete v tématu [Přehled Azure Data Lake Analytics](data-lake-analytics-overview.md).</span><span class="sxs-lookup"><span data-stu-id="ef105-105">For more information about Data Lake Analytics, see [Azure Data Lake Analytics overview](data-lake-analytics-overview.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ef105-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ef105-106">Prerequisites</span></span>

<span data-ttu-id="ef105-107">Než začnete tento kurz, musíte mít **předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="ef105-107">Before you begin this tutorial, you must have an **Azure subscription**.</span></span> <span data-ttu-id="ef105-108">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ef105-108">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="create-a-data-lake-analytics-account"></a><span data-ttu-id="ef105-109">Vytvoření účtu Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="ef105-109">Create a Data Lake Analytics account</span></span>

<span data-ttu-id="ef105-110">Nyní vytvoříte Data Lake Analytics a účet Data Lake Store v hello stejný čas.</span><span class="sxs-lookup"><span data-stu-id="ef105-110">Now, you will create a Data Lake Analytics and a Data Lake Store account at hello same time.</span></span>  <span data-ttu-id="ef105-111">Tento krok je jednoduchý a trvá jenom o toofinish 60 sekund.</span><span class="sxs-lookup"><span data-stu-id="ef105-111">This step is simple and only takes about 60 seconds toofinish.</span></span>

1. <span data-ttu-id="ef105-112">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ef105-112">Sign on toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="ef105-113">Klikněte na **Nový** >  **Data a analýzy** > **Data Lake Analytics**.</span><span class="sxs-lookup"><span data-stu-id="ef105-113">Click **New** >  **Data + Analytics** > **Data Lake Analytics**.</span></span>
3. <span data-ttu-id="ef105-114">Vyberte hodnoty pro hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="ef105-114">Select values for hello following items:</span></span>
   * <span data-ttu-id="ef105-115">**Název:** Pojmenujte svůj účet Data Lake Analytics (povolena jsou pouze malá písmena a číslice).</span><span class="sxs-lookup"><span data-stu-id="ef105-115">**Name**: Name your Data Lake Analytics account (Only lower case letters and numbers allowed).</span></span>
   * <span data-ttu-id="ef105-116">**Předplatné**: Zvolte předplatné Azure použité pro účet Analytics hello hello.</span><span class="sxs-lookup"><span data-stu-id="ef105-116">**Subscription**: Choose hello Azure subscription used for hello Analytics account.</span></span>
   * <span data-ttu-id="ef105-117">**Skupina prostředků**.</span><span class="sxs-lookup"><span data-stu-id="ef105-117">**Resource Group**.</span></span> <span data-ttu-id="ef105-118">Vyberte některou z existujících skupin prostředků Azure nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="ef105-118">Select an existing Azure Resource Group or create a new one.</span></span>
   * <span data-ttu-id="ef105-119">**Umístění**.</span><span class="sxs-lookup"><span data-stu-id="ef105-119">**Location**.</span></span> <span data-ttu-id="ef105-120">Vyberte datové centrum Azure pro účet Data Lake Analytics hello.</span><span class="sxs-lookup"><span data-stu-id="ef105-120">Select an Azure data center for hello Data Lake Analytics account.</span></span>
   * <span data-ttu-id="ef105-121">**Data Lake Store**: postupujte podle instrukcí toocreate hello nový účet Data Lake Store, nebo vyberte nějaký existující.</span><span class="sxs-lookup"><span data-stu-id="ef105-121">**Data Lake Store**: Follow hello instruction toocreate a new Data Lake Store account, or select an existing one.</span></span> 
4. <span data-ttu-id="ef105-122">Volitelně vyberte cenovou úroveň pro svůj účet Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="ef105-122">Optionally, select a pricing tier for your Data Lake Analytics account.</span></span>
5. <span data-ttu-id="ef105-123">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="ef105-123">Click **Create**.</span></span> 


## <a name="your-first-u-sql-script"></a><span data-ttu-id="ef105-124">Váš první skript U-SQL</span><span class="sxs-lookup"><span data-stu-id="ef105-124">Your first U-SQL script</span></span>

<span data-ttu-id="ef105-125">Následující text Hello je velmi jednoduchý skript U-SQL.</span><span class="sxs-lookup"><span data-stu-id="ef105-125">hello following text is a very simple U-SQL script.</span></span> <span data-ttu-id="ef105-126">Všechny dělá je definovat na malou datovou sadu v rámci skriptu hello a zapište si tuto datovou sadu se toohello výchozí Data Lake Store jako soubor s názvem `/data.csv`.</span><span class="sxs-lookup"><span data-stu-id="ef105-126">All it does is define a small dataset within hello script and then write that dataset out toohello default Data Lake Store as a file called `/data.csv`.</span></span>

```
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS 
              D( customer, amount );
OUTPUT @a
    too"/data.csv"
    USING Outputters.Csv();
```

## <a name="submit-a-u-sql-job"></a><span data-ttu-id="ef105-127">Odeslání úlohy U-SQL</span><span class="sxs-lookup"><span data-stu-id="ef105-127">Submit a U-SQL job</span></span>

1. <span data-ttu-id="ef105-128">Z hello účtu Data Lake Analytics, klikněte na tlačítko **nová úloha**.</span><span class="sxs-lookup"><span data-stu-id="ef105-128">From hello Data Lake Analytics account, click **New Job**.</span></span>
2. <span data-ttu-id="ef105-129">Vložte hello textu hello skript U-SQL, které jsou uvedené výše.</span><span class="sxs-lookup"><span data-stu-id="ef105-129">Paste in hello text of hello U-SQL script shown above.</span></span> 
3. <span data-ttu-id="ef105-130">Klikněte na **Odeslat úlohu**.</span><span class="sxs-lookup"><span data-stu-id="ef105-130">Click **Submit Job**.</span></span>   
4. <span data-ttu-id="ef105-131">Počkejte na změny stavu úlohy hello příliš**úspěšné**.</span><span class="sxs-lookup"><span data-stu-id="ef105-131">Wait until hello job status changes too**Succeeded**.</span></span>
5. <span data-ttu-id="ef105-132">Pokud hello úloha se nezdařila, přečtěte si téma [sledování úloh Data Lake Analytics a odstraňování potíží](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="ef105-132">If hello job failed, see [Monitor and troubleshoot Data Lake Analytics jobs](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md).</span></span>
6. <span data-ttu-id="ef105-133">Klikněte na tlačítko hello **výstup** a pak klikněte `data.csv`.</span><span class="sxs-lookup"><span data-stu-id="ef105-133">Click hello **Output** tab, and then click `data.csv`.</span></span> 

## <a name="see-also"></a><span data-ttu-id="ef105-134">Viz také</span><span class="sxs-lookup"><span data-stu-id="ef105-134">See also</span></span>

* <span data-ttu-id="ef105-135">tooget práce s vývojem aplikací U-SQL najdete v části [skriptů vyvíjet U-SQL pomocí nástrojů Data Lake pro Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="ef105-135">tooget started developing U-SQL applications, see [Develop U-SQL scripts using Data Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span></span>
* <span data-ttu-id="ef105-136">toolearn U-SQL, najdete v části [Začínáme s jazykem Azure Data Lake Analytics U-SQL](data-lake-analytics-u-sql-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="ef105-136">toolearn U-SQL, see [Get started with Azure Data Lake Analytics U-SQL language](data-lake-analytics-u-sql-get-started.md).</span></span>
* <span data-ttu-id="ef105-137">Informace týkající se úloh správy najdete v tématu [Správa služby Azure Data Lake Analytics pomocí webu Azure Portal](data-lake-analytics-manage-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="ef105-137">For management tasks, see [Manage Azure Data Lake Analytics using Azure portal](data-lake-analytics-manage-use-portal.md).</span></span>
