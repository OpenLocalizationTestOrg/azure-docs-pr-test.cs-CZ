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
# <a name="load-data-into-storage-environments-for-analytics"></a>Načtení dat do prostředí úložiště pro analýzu
Hello Team datové vědy proces vyžaduje, aby se data požity nebo načíst do různých prostředí toobe jiného úložiště zpracovat nebo analyzovat nejvhodnějším způsobem hello v každé fázi procesu hello. Běžně používané ke zpracování cíle dat patří Azure Blob Storage, databáze SQL Azure, SQL Server na virtuální počítač Azure, HDInsight (Hadoop) a Azure Machine Learning. 

[!INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

To **nabídky** odkazy tootopics, které popisují, jak tooingest data do těchto cílové prostředí, kde je hello data ukládat a zpracovávat.

Technická a obchodních potřeb, jakož i hello počáteční umístění, formátování a velikost dat určí hello cílové prostředí, do které hello dat musí toobe požity tooachieve hello cíle analýzy. Je běžné pro toobe scénář toorequire data přesunout mezi několika prostředí tooachieve hello řadu úloh vyžaduje tooconstruct prediktivního modelu. Toto pořadí úloh mohou zahrnovat například zkoumání dat, předběžné zpracování, čištění, nižší vzorkování a školení modelu.

