---
title: "aaaUse .NET s MapReduce s Hadoop v HDInsight se systémem Linux - Azure | Microsoft Docs"
description: "Zjistěte, jak toouse aplikace .NET pro streamování MapReduce na HDInsight se systémem Linux."
services: hdinsight
documentationCenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/12/2017
ms.author: larryfr
ms.openlocfilehash: 5a4e6dc1b4dafa8cc40780e3371fa4b8ba3e3d48
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-net-solutions-for-windows-based-hdinsight-toolinux-based-hdinsight"></a>Migrace řešení .NET pro HDInsight se systémem Windows na základě tooLinux HDInsight

Clustery HDInsight se systémem Linux použijte [Mono (https://mono-project.com)](https://mono-project.com) toorun aplikací .NET. Mono umožňuje toouse .NET součásti, jako jsou například aplikace MapReduce s HDInsight se systémem Linux. V tomto dokumentu zjistěte, jak se vytváří toomigrate .NET řešení pro toowork clustery HDInsight se systémem Windows s Mono na HDInsight se systémem Linux.

## <a name="mono-compatibility-with-net"></a>Monofonní kompatibilitu s rozhraním .NET

Monofonní verze 4.2.1 je součástí HDInsight verze 3.5. Další informace o verzi hello Mono zahrnuté do HDInsight naleznete v tématu [HDInsight verze součástí](hdinsight-component-versioning.md). tooinstall na konkrétní verzi Mono, najdete v části hello [instalace nebo aktualizace Mono](hdinsight-hadoop-install-mono.md) dokumentu.

Podrobné informace o kompatibilitě mezi Mono a rozhraní .NET najdete v tématu hello [Mono kompatibility (http://www.mono-project.com/docs/about-mono/compatibility/)](http://www.mono-project.com/docs/about-mono/compatibility/) dokumentu.

> [!IMPORTANT]
> je kompatibilní s Mono Hello SCP.NET framework. Další informace o používání SCP.NET s Mono najdete v tématu [toodevelop C# topologií pomocí aplikace Visual Studio pro Apache Storm v HDInsight](hdinsight-storm-develop-csharp-visual-studio-topology.md).

## <a name="automated-portability-analysis"></a>Analýza automatizované přenositelnost

Hello [.NET přenositelnost analyzátor](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer) lze použít toogenerate sestavu problémům s kompatibilitou mezi aplikací a Mono. Použijte následující postup tooconfigure hello analyzátor toocheck hello vaší aplikace pro Mono přenositelnost:

1. Nainstalujte hello [.NET přenositelnost analyzátor](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer). Během instalace vyberte hello verzi sady Visual Studio toouse.

2. Ze sady Visual Studio 2015, vyberte __analyzovat__ > __přenositelnost Analyzátor nastavení__a ujistěte se, že __4.5__ se změnami hello __Mono__  části.

    ![4.5 změnami Mono části Nastavení analyzátor hello](./media/hdinsight-hadoop-migrate-dotnet-to-linux/portability-analyzer-settings.png)

    Vyberte __OK__ toosave hello konfigurace.

3. Vyberte __analyzovat__ > __analyzovat sestavení přenositelnost__. Vyberte hello sestavení, která obsahuje vaše řešení a pak vyberte __otevřete__ toobegin analýzy.

4. Po dokončení analýzy, vybrat __analyzovat__ > __zobrazit sestavy analýzy__. V __výsledky analýzy přenositelnost__, vyberte __otevřete sestavu__ tooopen sestavy.

    ![Dialogové okno výsledků analyzátoru přenositelnost](./media/hdinsight-hadoop-migrate-dotnet-to-linux/portability-analyzer-results.png)

> [!IMPORTANT]
> Analyzátor Hello nelze catch každých problém s vaším řešením. Například soubor cestu `c:\temp\file.txt` se považuje za normální, protože Mono běží na systému Windows a hello cesta je platná v daném kontextu. Ale hello cesta není platná na platformě Linux.

## <a name="manual-portability-analysis"></a>Analýza ruční přenositelnost

Proveďte ruční auditu kódu pomocí informací o hello v hello [přenositelnost aplikace (http://www.mono-project.com/docs/getting-started/application-portability/)](http://www.mono-project.com/docs/getting-started/application-portability/) dokumentu.

## <a name="modify-and-build"></a>Upravit a sestavení

Můžete pokračovat v sadě Visual Studio toobuild toouse řešení rozhraní .NET pro HDInsight. Ale můžete zajistit, aby byl tento projekt hello nakonfigurované toouse rozhraní .NET Framework 4.5.

## <a name="deploy-and-test"></a>Nasazení a testování

Jakmile změníte řešení pomocí hello doporučení z hello analyzátor přenositelnost .NET nebo ruční analýzy, je nutné jej otestovat s HDInsight. Testování hello řešení na clusteru HDInsight se systémem Linux může odhalit jemně problémy, které vyžadují toobe opravena. Doporučujeme, abyste povolili další protokolování v aplikaci během testování.

Další informace o přístupu k protokoly najdete v tématu hello následující dokumenty:

* [Přístup k protokolům aplikací YARN ve službě HDInsight s Linuxem](hdinsight-hadoop-access-yarn-app-logs-linux.md)

## <a name="next-steps"></a>Další kroky

* [Použití jazyka C# s MapReduce v HDInsight](hdinsight-hadoop-dotnet-csharp-mapreduce-streaming.md)

* [Uživatelem definované funkce jazyka C# pomocí Hive a Pig](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md)

* [Vývoj C# topologií pro Storm v HDInsight](hdinsight-storm-develop-csharp-visual-studio-topology.md)