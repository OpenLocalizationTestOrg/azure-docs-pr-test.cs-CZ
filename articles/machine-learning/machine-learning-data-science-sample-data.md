---
title: "Ukázková data v kontejnerech objektů blob v Azure, SQL Server a tabulek Hive | Microsoft Docs"
description: "Jak chcete dynamicky prozkoumávat data uložená v různých enviromnents Azure."
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
ms.openlocfilehash: 0683be564a88ef54882e8d882d196851ecac243d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <span data-ttu-id="4c0b9-103"><a name="heading"></a>Ukázková data v kontejnerech objektů blob v Azure, SQL Server a tabulek Hive</span><span class="sxs-lookup"><span data-stu-id="4c0b9-103"><a name="heading"></a>Sample data in Azure blob containers, SQL Server, and Hive tables</span></span>
<span data-ttu-id="4c0b9-104">Tento dokument odkazy na témata, která vysvětluje postup ukázková data, která je uložená v jednom ze tří různých Azure umístění:</span><span class="sxs-lookup"><span data-stu-id="4c0b9-104">This document links to topics that covers how to sample data that is stored in one of three different Azure locations:</span></span>

* <span data-ttu-id="4c0b9-105">**K datům kontejneru objektů blob v Azure** je vzorkovat stáhnout prostřednictvím kódu programu a potom ho vzorkování s ukázkový kód Python.</span><span class="sxs-lookup"><span data-stu-id="4c0b9-105">**Azure blob container data** is sampled by downloading it programmatically and then sampling it with sample Python code.</span></span>
* <span data-ttu-id="4c0b9-106">**Data systému SQL Server** je shromažďovat vzorky pomocí SQL a programovací jazyk Python.</span><span class="sxs-lookup"><span data-stu-id="4c0b9-106">**SQL Server data** is sampled using both SQL and the Python Programming Language.</span></span> 
* <span data-ttu-id="4c0b9-107">**Hive dat v tabulce** je vzorkovat pomocí dotazů Hive.</span><span class="sxs-lookup"><span data-stu-id="4c0b9-107">**Hive table data** is sampled using Hive queries.</span></span>

<span data-ttu-id="4c0b9-108">Následující **nabídky** odkazy na témata, které popisují, jak ukázková data ze všech těchto prostředích úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="4c0b9-108">The following **menu** links to the topics that describe how to sample data from each of these Azure storage environments.</span></span> 

[!INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

<span data-ttu-id="4c0b9-109">Tato úloha vzorkování je krok v [tým datové vědy procesu (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span><span class="sxs-lookup"><span data-stu-id="4c0b9-109">This sampling task is a step in the [Team Data Science Process (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span></span>

<span data-ttu-id="4c0b9-110">**Proč vzorová data?**</span><span class="sxs-lookup"><span data-stu-id="4c0b9-110">**Why sample data?**</span></span>

<span data-ttu-id="4c0b9-111">Pokud je velké datové sady, které chcete analyzovat, je obvykle vhodné nižší ukázková data, která mají snížit velikost menší, ale reprezentativní a lepší správu bitlockeru.</span><span class="sxs-lookup"><span data-stu-id="4c0b9-111">If the dataset you plan to analyze is large, it's usually a good idea to down-sample the data to reduce it to a smaller but representative and more manageable size.</span></span> <span data-ttu-id="4c0b9-112">To usnadňuje pochopení dat, zkoumání a funkce inženýrství.</span><span class="sxs-lookup"><span data-stu-id="4c0b9-112">This facilitates data understanding, exploration, and feature engineering.</span></span> <span data-ttu-id="4c0b9-113">Jeho role v procesu Cortana Analytics je umožnit rychlé vytváření prototypů zpracování dat funkcí a modelů strojového učení.</span><span class="sxs-lookup"><span data-stu-id="4c0b9-113">Its role in the Cortana Analytics Process is to enable fast prototyping of the data processing functions and machine learning models.</span></span>

