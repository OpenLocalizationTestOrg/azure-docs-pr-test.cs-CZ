---
title: "Ukázky pro vytváření dat Azure Azure PowerShell | Microsoft Docs"
description: "Ukázek Azure PowerShell - skripty, které vám pomůžou vytvořit a spravovat datové továrny."
services: data-factory
author: spelluru
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2018
ms.author: spelluru
ms.openlocfilehash: 9e361f42b25a1b3a97eacfb1ec1bc82900881806
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/23/2018
---
# <a name="azure-powershell-samples-for-azure-data-factory"></a>Ukázek Azure PowerShell pro Azure Data Factory

Následující tabulka obsahuje odkazy na ukázkové skripty prostředí Azure PowerShell pro Azure Data Factory.

> [!NOTE]
> Tento článek se týká verze 2 služby Data Factory, která je aktuálně ve verzi Preview. Pokud používáte verzi 1 služby Data Factory, který je všeobecně dostupná (GA), přečtěte si téma [ukázky pro vytváření dat version1](v1/data-factory-samples.md).

| |  |
|---|---|
|**Kopírování dat**||
|[Kopírování objektů BLOB ze složky do jiné složky v Azure Blob Storage](scripts/copy-azure-blob-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Tento skript prostředí PowerShell zkopíruje objekty BLOB ze složky v úložišti objektů Blob Azure do jiné složky v stejné úložiště objektů Blob. |
|[Kopírování dat z místního SQL serveru do Azure Blob Storage](scripts/hybrid-copy-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Tento skript prostředí PowerShell kopíruje data z místní databáze SQL serveru do Azure blob storage. |
|[Hromadné kopírování](scripts/bulk-copy-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Tento ukázkový skript prostředí PowerShell kopíruje data z více tabulek v databázi Azure SQL do služby Azure SQL data warehouse. |
|[Přírůstkové kopie](scripts/incremental-copy-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Tento ukázkový skript prostředí PowerShell načte pouze nové nebo aktualizované záznamy ze zdrojového úložiště dat do úložiště dat podřízený po počáteční úplnou kopii dat ze zdroje jímky. |
|**Transformace dat**||
|[Transformace dat pomocí clusteru Spark](scripts/transform-data-spark-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Tento skript prostředí PowerShell transformuje data spuštěním programu na clusteru Spark. |
|**Navýšení a shift balíčky SSIS do Azure**||
|[Vytvoření modulu runtime integrace Azure SSIS](scripts/deploy-azure-ssis-integration-runtime-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Toto prostředí PowerShell skriptu zřizuje Azure-SSIS integrační modul runtime, který spouští balíčky integrační služby SSIS (SQL Server) v Azure. |



