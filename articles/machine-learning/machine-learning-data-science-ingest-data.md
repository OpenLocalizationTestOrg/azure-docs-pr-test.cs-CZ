---
title: "Načtení dat do úložiště Azure prostředí pro analýzu | Microsoft Docs"
description: "Přesun dat z a do služby Azure Blob Storage"
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
ms.openlocfilehash: 7fbf3bfedca8fa57a5e9428c9399558992b4acbd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="load-data-into-storage-environments-for-analytics"></a><span data-ttu-id="71f58-103">Načtení dat do prostředí úložiště pro analýzu</span><span class="sxs-lookup"><span data-stu-id="71f58-103">Load data into storage environments for analytics</span></span>
<span data-ttu-id="71f58-104">Proces Team dat. vědecké účely vyžaduje, aby se data požity nebo načíst do různých prostředí jiného úložiště ji zpracovat nebo analyzovat nejvhodnějším způsobem v každé fázi procesu.</span><span class="sxs-lookup"><span data-stu-id="71f58-104">The Team Data Science Process requires that data be ingested or loaded into a variety of different storage environments to be processed or analyzed in the most appropriate way in each stage of the process.</span></span> <span data-ttu-id="71f58-105">Běžně používané ke zpracování cíle dat patří Azure Blob Storage, databáze SQL Azure, SQL Server na virtuální počítač Azure, HDInsight (Hadoop) a Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="71f58-105">Data destinations commonly used for processing include Azure Blob Storage, SQL Azure databases, SQL Server on Azure VM, HDInsight (Hadoop), and Azure Machine Learning.</span></span> 

[!INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

<span data-ttu-id="71f58-106">To **nabídky** odkazy na témata, které popisují, jak pro načítání dat do těchto cílové prostředí, kde se data ukládat a zpracovávat.</span><span class="sxs-lookup"><span data-stu-id="71f58-106">This **menu** links to topics that describe how to ingest data into these target environments where the data is stored and processed.</span></span>

<span data-ttu-id="71f58-107">Technická a obchodních potřeb, jakož i Počáteční umístění, formátování a velikost dat určí cílové prostředí, do kterých musí data konzumaci k dosažení cílů analýzy.</span><span class="sxs-lookup"><span data-stu-id="71f58-107">Technical and business needs, as well as the initial location, format and size of your data will determine the target environments into which the data needs to be ingested to achieve the goals of your analysis.</span></span> <span data-ttu-id="71f58-108">Je běžné pro scénář vyžadují dat do přesouvat mezi několika prostředích k dosažení řadu úloh, které jsou potřebné k vytvoření prediktivního modelu.</span><span class="sxs-lookup"><span data-stu-id="71f58-108">It is not uncommon for a scenario to require data to be moved between several environments to achieve the variety of tasks required to construct a predictive model.</span></span> <span data-ttu-id="71f58-109">Toto pořadí úloh mohou zahrnovat například zkoumání dat, předběžné zpracování, čištění, nižší vzorkování a školení modelu.</span><span class="sxs-lookup"><span data-stu-id="71f58-109">This sequence of tasks can include, for example, data exploration, pre-processing, cleaning, down-sampling, and model training.</span></span>

