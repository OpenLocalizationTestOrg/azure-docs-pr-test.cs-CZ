---
title: "aaaStore a zobrazení diagnostických dat ve službě Azure Storage | Microsoft Docs"
description: "Získat Azure diagnostická data do úložiště Azure a jeho zobrazení"
services: cloud-services
documentationcenter: .net
author: rboucher
manager: jwhit
editor: tysonn
ms.assetid: 18e0780d-43e7-41e4-b8e9-f1fb9a36eb03
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/01/2016
ms.author: robb
ms.openlocfilehash: dd47a2ef6d6488c80c102c72b2ebf6ca6d2e473f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="store-and-view-diagnostic-data-in-azure-storage"></a>Úložiště a zobrazení diagnostických dat ve službě Azure Storage
Diagnostických dat není uložena trvale, pokud přenos toohello emulátor úložiště Microsoft Azure nebo tooAzure úložiště. Jednou v úložišti, prohlížení s jedním z několika dostupných nástrojů.

## <a name="specify-a-storage-account"></a>Zadejte účet úložiště
Zadáte účet hello úložiště, které chcete toouse v souboru ServiceConfiguration.cscfg souboru hello. informace o účtu Hello je definován jako připojovací řetězec v nastavení konfigurace. Hello následující příklad ukazuje hello výchozí připojovací řetězec pro nový projekt cloudové služby v sadě Visual Studio vytvořit:

```
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
    </ConfigurationSettings>
```

Tento připojovací řetězec tooprovide informace o účtu pro účet úložiště Azure můžete změnit.

V závislosti na typu hello diagnostických dat, jež jsou shromažďována používá Azure Diagnostics hello služby objektů Blob nebo služby Table hello. Hello následující tabulka uvádí hello datových zdrojů, které jsou nastavené jako trvalé a jejich formátu.

| Zdroj dat | Formát úložiště |
| --- | --- |
| Azure protokoly |Table |
| Protokoly služby IIS 7.0 |Objekt blob |
| Protokoly infrastruktury Azure Diagnostics |Table |
| Protokoly trasování požadavku se nezdařilo |Objekt blob |
| Protokoly událostí systému Windows |Table |
| Čítače výkonu |Table |
| Výpisy stavu systému |Objekt blob |
| Vlastní chybové protokoly |Objekt blob |

## <a name="transfer-diagnostic-data"></a>Přenos dat diagnostiky
Pro sadu SDK, 2.5 nebo novější může dojít, hello požadavek tootransfer diagnostických dat prostřednictvím hello konfigurační soubor. Diagnostická data můžou přenášet v naplánovaných intervalech jako zadaný v konfiguraci hello.

SDK 2.4 a předchozí můžete požádat tootransfer hello diagnostických dat prostřednictvím hello konfigurační soubor také jako prostřednictvím kódu programu. Hello programový přístup také umožňuje přenosy toodo na vyžádání.

> [!IMPORTANT]
> Při přenosu dat diagnostiky tooan účtu úložiště Azure se vynakládá pro hello prostředky úložiště, které používá diagnostická data.
> 
> 

## <a name="store-diagnostic-data"></a>Ukládání diagnostických dat
Data protokolu se ukládají v úložišti objektů Blob nebo Table s hello následující názvy:

**Tabulky**

* **WadLogsTable** – protokoly, které jsou napsané v kódu pomocí hello naslouchací proces trasování.
* **WADDiagnosticInfrastructureLogsTable** -diagnostiky změny monitorování a konfigurace.
* **WADDirectoriesTable** – adresářů tohoto monitorování diagnostiky hello je monitorování.  To zahrnuje protokoly služby IIS, služba IIS nezdařilo požadavek protokoly a vlastní adresářů.  Hello umístění souboru protokolu hello objektu blob je zadána v poli hello kontejneru a hello název objektu hello blob je v poli RelativePath hello.  Hello Absolutní_cesta pole určuje hello umístění a název souboru hello tak, jak byly na hello virtuální počítač Azure.
* **WADPerformanceCountersTable** – čítače výkonu.
* **WADWindowsEventLogsTable** – protokoly událostí systému Windows.

**Objekty blob**

* **kontejneru ovládacího prvku wad** – (jenom pro SDK 2.4 a předchozí) obsahuje hello XML konfigurační soubory, které se řídí hello Azure diagnostics.
* **wad. Služba iis failedreqlogfiles** – obsahuje informace z protokolů žádostí služby IIS se nezdařilo.
* **wad. Služba iis logfiles** – obsahuje informace o protokoly služby IIS.
* **"vlastní"** – vlastní kontejner podle konfigurace adresáře, které jsou monitorovány pomocí monitorování diagnostiky hello.  Hello název kontejneru objektu blob budou zadány v WADDirectoriesTable.

## <a name="tools-tooview-diagnostic-data"></a>Nástroje pro tooview diagnostických dat
Několik nástrojů jsou k dispozici tooview hello data po přenášená toostorage. Například:

* Průzkumník serveru v sadě Visual Studio – Pokud jste nainstalovali nástroje hello Azure pro sadu Microsoft Visual Studio, můžete hello uzlu úložiště Azure v Průzkumníku serveru tooview jen pro čtení objektů blob a tabulku dat z účtům Azure storage. Můžete zobrazit data z účtu emulátor místního úložiště a taky z účtů úložiště jste vytvořili pro Azure. Další informace najdete v tématu [procházení a Správa prostředků úložiště pomocí Průzkumníka serveru](../vs-azure-tools-storage-resources-server-explorer-browse-manage.md).
* [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) je samostatná aplikace, která vám umožní tooeasily práci s daty Azure Storage v systému Windows, na OSX a Linux.
* [Azure Management Studio](http://www.cerebrata.com/products/azure-management-studio/introduction) zahrnuje Azure Diagnostics Manager, která vám umožní tooview, stahování a správě hello diagnostická data shromažďovaná společností hello aplikace běžící v Azure.

## <a name="next-steps"></a>Další kroky
[Tok hello trasování v aplikaci s Azure Diagnostics cloudové služby](cloud-services-dotnet-diagnostics-trace-flow.md)

