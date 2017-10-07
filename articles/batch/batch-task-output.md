---
title: "aaaPersist výsledků nebo protokoly z dokončení úlohy a úkoly tooa data store - Azure Batch | Microsoft Docs"
description: "Další informace o různé možnosti pro zachování dat výstup z úlohy a úkoly služby Batch. Můžete zachovat data tooAzure úložiště nebo tooanother data uložit."
services: batch
author: tamram
manager: timlt
editor: 
ms.assetid: 16e12d0e-958c-46c2-a6b8-7843835d830e
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 06/16/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f0b11e387f1694e1ce3e9573db7f6013f0154cad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="persist-job-and-task-output"></a>Trvalý výstup úloh a funkcí

[!INCLUDE [batch-task-output-include](../../includes/batch-task-output-include.md)]

Několik běžných příkladů výstupu úlohy patří:

- Soubory vytvořené při hello úloh procesy vstupní data.
- Soubory protokolu přidružené k provedení úlohy. 

Tento článek popisuje různé možnosti pro zachování úloh výstup hello scénáře a pro které je nejvhodnější jednotlivých možností.   

## <a name="about-hello-batch-file-conventions-standard"></a>O standardní hello konvence dávkového souboru

Batch definuje volitelná sada konvence pro pojmenování souborů výstupní úloh ve službě Azure Storage. Hello [dávkového souboru konvence standard](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions) popisuje tyto konvence. standardní konvence souboru Hello určuje názvy hello hello cílový kontejner a objektů blob cesty ve službě Azure Storage pro danou výstupní soubor založený na názvy hello hello úlohy a úlohy.

Až tooyou je, jestli se rozhodnete toouse hello souboru konvence standard pro pojmenování výstupní datové soubory. Ale chcete, můžete pojmenovat také cílový kontejner hello a objektů blob. Pokud používáte hello standardní soubor konvence pro pojmenování výstupní soubory, pak výstupní soubory jsou k dispozici pro zobrazení v hello [portál Azure][portal].

Existuje několik různých způsobů, které můžete použít standardní hello souboru konvence:

- Pokud používáte hello Batch služby API toopersist výstupní soubory, můžete tooname cílové kontejnery a objekty BLOB podle toohello souboru konvence standardní. Hello rozhraní API služby Batch můžete toopersist výstupní soubory z kódu klienta beze změny aplikace úkolů.
- Pokud vyvíjíte pomocí rozhraní .NET, můžete použít hello [konvence souboru Azure Batch library pro .NET][nuget_package]. Výhodou použití této knihovny je, že podporuje dotazování výstupní soubory podle tootheir ID nebo účel. integrované funkce dotazování Hello umožňuje snadno tooaccess výstupní soubory z klientské aplikace, nebo z jiných úloh. Úloha aplikace však musí být upravené toocall souboru konvence knihovny. Další informace najdete v tématu hello odkaz pro hello [souboru konvence knihovna pro .NET](https://msdn.microsoft.com/library/microsoft.azure.batch.conventions.files.aspx).
- Pokud vyvíjíte s jiném jazyce než v rozhraní .NET, můžete implementovat hello souboru konvence standardní ve vaší aplikaci.

## <a name="design-considerations-for-persisting-output"></a>Aspekty návrhu pro zachování výstup 

Při navrhování řešení Batch, zvažte následující hello faktory související toojob a výstupy úlohy.

* **Výpočetní uzel životnost**: výpočetní uzly jsou často přechodný, zejména ve fondech škálování povolené. Výstup z úlohy, která běží na uzlu je k dispozici pouze tehdy, když existuje hello uzlu a pouze v rámci dobu uchování hello souboru jste nastavili pro úlohu hello. Pokud úloha vytváří výstupu, který může být potřeba po dokončení úlohy hello, musíte nahrát hello úloh jeho výstupní soubory tooa úložiště odolné například Azure Storage.

* **Výstup úložiště**: Azure Storage se doporučuje jako úložiště dat pro výstup úlohy, ale můžete použít jakékoli odolné úložiště. Zápis úloh výstup tooAzure úložiště je integrována do hello rozhraní API služby Batch. Pokud používáte jinou formu odolná úložiště, budete potřebovat výstup úlohy toowrite hello aplikace logiky toopersist sami.   

* **Výstup načtení**: výstup úlohy můžete načíst přímo z hello výpočetních uzlů ve vašem fondu nebo z Azure Storage nebo jiného úložiště dat, pokud držena formou výstup úlohy. tooretrieve úlohu výstupu přímo z výpočetním uzlu, budete potřebovat hello název souboru a jeho výstupní umístění na uzlu hello. Pokud jste zachovat tooAzure výstup úlohy úložiště, musíte soubor toohello hello úplnou cestu v Azure Storage toodownload hello výstupní soubory s hello sada SDK úložiště Azure.

* **Zobrazení výstupu**: když přejdete úloha Batch tooa v hello Azure portálu a vyberte možnost **soubory na uzlu,**, zobrazí se všechny soubory přidružené k hello úloh, ne jenom hello výstupní soubory, které vás zajímají. Soubory na výpočetních uzlech znovu, jsou k dispozici pouze tehdy, když existuje hello uzlu a pouze v rámci hello dobu uchovávání souboru jste nastavili pro úlohu hello. Úloha tooview výstup, jste trvalé tooAzure úložiště, můžete použít hello portál Azure nebo klienta aplikace Azure Storage například hello [Azure Storage Explorer][storage_explorer]. tooview výstupní data ve službě Azure Storage s hello portál nebo jiný nástroj, je nutné znát umístění souboru hello a tooit přejděte přímo.

## <a name="options-for-persisting-output"></a>Možnosti pro zachování výstup

V závislosti na vašem scénáři jsou několik různý přístup, může trvat toopersist výstup úlohy:

- Pomocí rozhraní API služby Batch hello.  
- Použijte knihovnu hello konvence souboru Batch pro .NET.  
- Implementujte hello dávkového souboru konvence standardní ve vaší aplikaci.
- Implementujte řešení přesun vlastního souboru.

Hello následující oddíly popisují každou přístup podrobněji.

### <a name="use-hello-batch-service-api"></a>Použít rozhraní API služby Batch hello

S verzí 2017-05-01, hello služba Batch přidává podporu pro zadání výstupní soubory ve službě Azure Storage pro data úloh při jste [přidat úloha tooa úlohu](https://docs.microsoft.com/rest/api/batchservice/add-a-task-to-a-job) nebo [Přidání kolekce úloh tooa úlohy](https://docs.microsoft.com/rest/api/batchservice/add-a-collection-of-tasks-to-a-job).

Hello rozhraní API služby Batch podporuje zachování úloh tooan dat účet služby Azure Storage z fondů vytvořené pomocí hello konfigurace virtuálního počítače. S hello rozhraní API služby Batch můžete zachovat data úloh beze změny hello aplikace, která spustí úlohu. Volitelně můžete řídit toohello [dávkového souboru konvence standard](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions) pro pojmenovávání souborů hello, který zachovat tooAzure úložiště. 

Pomocí úlohy toopersist hello rozhraní API služby Batch při výstup:

- Chcete toopersist data z úlohy Batch a úkolech Správce úloh ve fondech vytvořené pomocí hello konfigurace virtuálního počítače.
- Chcete kontejner Azure Storage tooan toopersist dat s libovolný název.
- Chcete-li toopersist data tooan Azure Storage kontejner s názvem podle toohello [dávkového souboru konvence standard](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions). 

> [!NOTE]
> Hello rozhraní API služby Batch nepodporuje zachování dat z úloh spuštěných ve fondech vytvořené pomocí konfigurace hello cloudové služby. Informace o zachování úloh výstup z fondů systémem konfigurace hello cloudových služeb najdete v tématu [zachovat úloh a úkolů tooAzure dat úložiště s knihovnou hello konvence souboru Batch pro .NET toopersist](batch-task-output-file-conventions.md)
> 
> 

Další informace o zachování výstup úlohy s hello rozhraní API služby Batch najdete v tématu [zachovat data tooAzure úloh úložiště s hello rozhraní API služby Batch](batch-task-output-files.md). Viz také hello [PersistOutputs] [github_persistoutputs] ukázkový projekt na Githubu, které ukazuje, jak toouse hello Batch Klientská knihovna pro .NET toopersist úloh výstup toodurable úložiště.

### <a name="use-hello-batch-file-conventions-library-for-net"></a>Použití knihovny hello konvence souboru Batch pro .NET

Sestavování řešení Batch s použitím jazyka C# a .NET mohou vývojáři hello [souboru konvence knihovna pro .NET] [ nuget_package] toopersist úloh dat tooan účtu Azure Storage podle toohello [dávkového souboru Konvence standardní](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions). Hello souboru konvence knihovny zpracovává přesunutí výstupní soubory tooAzure úložiště a pojmenování cílové kontejnery a objekty BLOB dobře známé způsobem.

je bez nutnosti hello Hello souboru konvence knihovny podporuje dotazování výstupní soubory podle ID nebo účel, což snadno toolocate dokončit souboru identifikátory URI. 

Použijte hello dávkového souboru konvence knihovny pro rozhraní .NET toopersist úlohu při výstup:

- Chcete data tooAzure toostream úložiště při běhu úlohy hello.
- Chcete data toopersist z fondů vytvořené pomocí konfigurace hello cloudové služby nebo konfigurace virtuálního počítače hello.
- Klientské aplikace nebo dalších úloh v hello úlohy toolocate potřeb a stáhněte soubory výstup úlohy podle ID nebo podle účelu. 
- Chcete tooperform odkazující kontrola nebo časná nahrávání počáteční výsledků.
- Chcete tooview výstup úlohy v hello portálu Azure.

Další informace o zachování výstup úlohy s knihovnou hello souboru konvence pro rozhraní .NET najdete v tématu [zachovat úloh a úkolů tooAzure dat úložiště s knihovnou hello konvence souboru Batch pro .NET toopersist ](batch-task-output-file-conventions.md). Viz také hello [PersistOutputs] [github_persistoutputs] ukázkový projekt na Githubu, které ukazuje, jak toouse hello souboru konvence knihovna pro .NET toopersist úlohu výstup toodurable úložiště.

Hello [PersistOutputs] [github_persistoutputs] ukázkového projektu na Githubu ukazuje, jak toouse hello Batch Klientská knihovna pro .NET toopersist úloh výstup toodurable úložiště.

### <a name="implement-hello-batch-file-conventions-standard"></a>Implementace konvence souboru Batch standardní hello

Pokud používáte jiný jazyk než rozhraní .NET, můžete implementovat hello [dávkového souboru konvence standard](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions) ve vaší vlastní aplikaci. 

Můžete chtít tooimplement hello standardní soubor konvence pojmenování sami když chcete Principy schéma pojmenování, nebo když chcete tooview výstup úlohy v hello portálu Azure.

### <a name="implement-a-custom-file-movement-solution"></a>Implementace řešení přesun vlastního souboru

Můžete také implementovat vlastní řešení Přesun celého souboru. Použít tato metoda při:

- Chcete toopersist úloh tooa data jiného úložiště dat než Azure Storage. tooupload soubory tooa úložiště dat jako Azure SQL nebo Azure DataLake, můžete vytvořit vlastní skript nebo spustitelný soubor tooupload toothat umístění. Potom ji můžete volat po spuštění vaší primární spustitelný soubor na příkazovém řádku hello. V uzlu systému Windows, například může volat tyto dva příkazy:`doMyWork.exe && uploadMyFilesToSql.exe`
- Chcete tooperform odkazující kontrola nebo časná nahrávání počáteční výsledků.
- Chcete toomaintain podrobnou kontrolu nad zpracování chyb. Například můžete tooimplement nahrát vlastní řešení, pokud chcete toouse úloh závislostí akcí tootake určité akce založené na konkrétní úkol ukončovací kód. Další informace o Akce závislosti úkolů najdete v tématu [vytvoření závislosti úkolů toorun úlohy, které jsou závislé na dalších úkolech,](batch-task-dependencies.md). 

## <a name="next-steps"></a>Další kroky

- Zkoumání použití hello nové funkce v hello rozhraní API služby Batch toopersist úloh data v [zachovat data tooAzure úloh úložiště s hello rozhraní API služby Batch](batch-task-output-files.md).
- Další informace o použití hello dávkového souboru konvence knihovna pro .NET v [zachovat úloh a úkolů tooAzure dat úložiště s knihovnou hello konvence souboru Batch pro .NET toopersist ](batch-task-output-file-conventions.md).
- V tématu hello [PersistOutputs] [github_persistoutputs] ukázkového projektu na Githubu, které ukazuje, jak toouse obě hello klientské knihovny Batch pro .NET a hello souboru konvence knihovna pro .NET toopersist úloh výstup toodurable úložiště.

[nuget_package]: https://www.nuget.org/packages/Microsoft.Azure.Batch.Conventions.Files
[portal]: https://portal.azure.com
[storage_explorer]: http://storageexplorer.com/
