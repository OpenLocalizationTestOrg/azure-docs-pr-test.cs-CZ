---
title: "aaaLoad data do úložiště Azure prostředí pro analýzu | Microsoft Docs"
description: "Přesun dat tooand z Azure Blob Storage"
services: machine-learning,storage
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: b8fbef77-3e80-4911-8e84-23dbf42c9bee
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2017
ms.author: bradsev
ms.openlocfilehash: 0fea2290991f9fa63d9e46c3a657000e27d95289
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="load-data-into-storage-environments-for-analytics"></a><span data-ttu-id="30388-103">Načtení dat do prostředí úložiště pro analýzu</span><span class="sxs-lookup"><span data-stu-id="30388-103">Load data into storage environments for analytics</span></span>
<span data-ttu-id="30388-104">Hello Team datové vědy proces vyžaduje, aby se data požity nebo načíst do různých prostředí toobe jiného úložiště zpracovat nebo analyzovat nejvhodnějším způsobem hello v každé fázi procesu hello.</span><span class="sxs-lookup"><span data-stu-id="30388-104">hello Team Data Science Process requires that data be ingested or loaded into a variety of different storage environments toobe processed or analyzed in hello most appropriate way in each stage of hello process.</span></span> <span data-ttu-id="30388-105">Běžně používané ke zpracování cíle dat patří Azure Blob Storage, databáze SQL Azure, SQL Server na virtuální počítač Azure, HDInsight (Hadoop) a Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="30388-105">Data destinations commonly used for processing include Azure Blob Storage, SQL Azure databases, SQL Server on Azure VM, HDInsight (Hadoop), and Azure Machine Learning.</span></span> 

[!INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

<span data-ttu-id="30388-106">To **nabídky** odkazy tootopics, které popisují, jak tooingest data do těchto cílové prostředí, kde je hello data ukládat a zpracovávat.</span><span class="sxs-lookup"><span data-stu-id="30388-106">This **menu** links tootopics that describe how tooingest data into these target environments where hello data is stored and processed.</span></span>

<span data-ttu-id="30388-107">Technická a obchodních potřeb, jakož i hello počáteční umístění, formátování a velikost dat určí hello cílové prostředí, do které hello dat musí toobe požity tooachieve hello cíle analýzy.</span><span class="sxs-lookup"><span data-stu-id="30388-107">Technical and business needs, as well as hello initial location, format and size of your data will determine hello target environments into which hello data needs toobe ingested tooachieve hello goals of your analysis.</span></span> <span data-ttu-id="30388-108">Je běžné pro toobe scénář toorequire data přesunout mezi několika prostředí tooachieve hello řadu úloh vyžaduje tooconstruct prediktivního modelu.</span><span class="sxs-lookup"><span data-stu-id="30388-108">It is not uncommon for a scenario toorequire data toobe moved between several environments tooachieve hello variety of tasks required tooconstruct a predictive model.</span></span> <span data-ttu-id="30388-109">Toto pořadí úloh mohou zahrnovat například zkoumání dat, předběžné zpracování, čištění, nižší vzorkování a školení modelu.</span><span class="sxs-lookup"><span data-stu-id="30388-109">This sequence of tasks can include, for example, data exploration, pre-processing, cleaning, down-sampling, and model training.</span></span>

