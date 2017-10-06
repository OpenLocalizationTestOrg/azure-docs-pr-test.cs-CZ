---
title: aaaSample data v Azure blob kontejnery, SQL Server a tabulek Hive | Microsoft Docs
description: "Jak tooexplore data uložená v různých enviromnents Azure."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 80a9dfae-e3a6-4cfb-aecc-5701cfc7e39d
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: fashah;garye;bradsev
ms.openlocfilehash: 5a5295b59fa02f91da680fc7495a92ca135e26c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <span data-ttu-id="c78b8-103"><a name="heading"></a>Ukázková data v kontejnerech objektů blob v Azure, SQL Server a tabulek Hive</span><span class="sxs-lookup"><span data-stu-id="c78b8-103"><a name="heading"></a>Sample data in Azure blob containers, SQL Server, and Hive tables</span></span>
<span data-ttu-id="c78b8-104">Tento dokument odkazuje tootopics, které pokrývá jak toosample data, která je uložená v jednom ze tří různých Azure umístění:</span><span class="sxs-lookup"><span data-stu-id="c78b8-104">This document links tootopics that covers how toosample data that is stored in one of three different Azure locations:</span></span>

* <span data-ttu-id="c78b8-105">**K datům kontejneru objektů blob v Azure** je vzorkovat stáhnout prostřednictvím kódu programu a potom ho vzorkování s ukázkový kód Python.</span><span class="sxs-lookup"><span data-stu-id="c78b8-105">**Azure blob container data** is sampled by downloading it programmatically and then sampling it with sample Python code.</span></span>
* <span data-ttu-id="c78b8-106">**Data systému SQL Server** je shromažďovat vzorky pomocí SQL a hello programovací jazyk Python.</span><span class="sxs-lookup"><span data-stu-id="c78b8-106">**SQL Server data** is sampled using both SQL and hello Python Programming Language.</span></span> 
* <span data-ttu-id="c78b8-107">**Hive dat v tabulce** je vzorkovat pomocí dotazů Hive.</span><span class="sxs-lookup"><span data-stu-id="c78b8-107">**Hive table data** is sampled using Hive queries.</span></span>

<span data-ttu-id="c78b8-108">Následující Hello **nabídky** odkazy toohello témata, které popisují, jak toosample data ze všech těchto prostředích úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="c78b8-108">hello following **menu** links toohello topics that describe how toosample data from each of these Azure storage environments.</span></span> 

[!INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

<span data-ttu-id="c78b8-109">Tato úloha vzorkování je krok v hello [tým datové vědy procesu (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span><span class="sxs-lookup"><span data-stu-id="c78b8-109">This sampling task is a step in hello [Team Data Science Process (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span></span>

<span data-ttu-id="c78b8-110">**Proč vzorová data?**</span><span class="sxs-lookup"><span data-stu-id="c78b8-110">**Why sample data?**</span></span>

<span data-ttu-id="c78b8-111">Pokud datovou sadu hello plánování tooanalyze velká, je obvykle vhodné hello toodown ukázková data tooreduce ho tooa menší, ale reprezentativní a lepší správu bitlockeru velikost.</span><span class="sxs-lookup"><span data-stu-id="c78b8-111">If hello dataset you plan tooanalyze is large, it's usually a good idea toodown-sample hello data tooreduce it tooa smaller but representative and more manageable size.</span></span> <span data-ttu-id="c78b8-112">To usnadňuje pochopení dat, zkoumání a funkce inženýrství.</span><span class="sxs-lookup"><span data-stu-id="c78b8-112">This facilitates data understanding, exploration, and feature engineering.</span></span> <span data-ttu-id="c78b8-113">Jeho role v hello Cortana Analytics proces je rychlé vytváření prototypů tooenable hello zpracování dat funkcí a modelů strojového učení.</span><span class="sxs-lookup"><span data-stu-id="c78b8-113">Its role in hello Cortana Analytics Process is tooenable fast prototyping of hello data processing functions and machine learning models.</span></span>

