---
title: "Načtení dat do úložiště Azure prostředí pro analýzu | Microsoft Docs"
description: "Přesun dat z a do služby Azure Blob Storage"
services: machine-learning,storage
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: b8fbef77-3e80-4911-8e84-23dbf42c9bee
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: bradsev
ms.openlocfilehash: 7d7da4f6dfed03d470c5b5706aaf412c07096120
ms.sourcegitcommit: bc8d39fa83b3c4a66457fba007d215bccd8be985
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="load-data-into-storage-environments-for-analytics"></a>Načtení dat do prostředí úložiště pro analýzu
Proces Team dat. vědecké účely vyžaduje, aby se data požity nebo načíst do různých prostředí jiného úložiště ji zpracovat nebo analyzovat nejvhodnějším způsobem v každé fázi procesu. Běžně používané ke zpracování cíle dat patří Azure Blob Storage, databáze SQL Azure, SQL Server na virtuální počítač Azure, HDInsight (Hadoop) a Azure Machine Learning. 

[!INCLUDE [cap-ingest-data-selector](../../../includes/cap-ingest-data-selector.md)]

To **nabídky** odkazy na témata, které popisují, jak pro načítání dat do těchto cílové prostředí, kde se data ukládat a zpracovávat.

Technická a obchodních potřeb, jakož i Počáteční umístění, formátování a velikost dat určí cílové prostředí, do kterých musí data konzumaci k dosažení cílů analýzy. Je běžné pro scénář vyžadují dat do přesouvat mezi několika prostředích k dosažení řadu úloh, které jsou potřebné k vytvoření prediktivního modelu. Toto pořadí úloh mohou zahrnovat například zkoumání dat, předběžné zpracování, čištění, nižší vzorkování a školení modelu.

