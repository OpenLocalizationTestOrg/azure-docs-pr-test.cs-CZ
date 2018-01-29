---
title: "MapReduce s Hadoop v HDInsight se systémem Linux - Azure pomocí rozhraní .NET | Microsoft Docs"
description: "Další informace o použití aplikace .NET pro streamování MapReduce na HDInsight se systémem Linux."
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
ms.date: 12/05/2017
ms.author: larryfr
ms.openlocfilehash: 2657db61ff3161cd87bc97edfe5f84f8b29cbcfb
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/05/2017
---
# <a name="migrate-net-solutions-for-windows-based-hdinsight-to-linux-based-hdinsight"></a>Migrace řešení .NET pro HDInsight se systémem Windows do HDInsight se systémem Linux

Clustery HDInsight se systémem Linux použijte [Mono (https://mono-project.com)](https://mono-project.com) ke spouštění aplikací .NET. Mono umožňuje použití rozhraní .NET komponent, jako jsou například aplikace MapReduce s HDInsight se systémem Linux. V tomto dokumentu zjistěte, jak migrovat .NET řešení vytvořená pro clustery HDInsight se systémem Windows pro práci s Mono na HDInsight se systémem Linux.

## <a name="mono-compatibility-with-net"></a>Monofonní kompatibilitu s rozhraním .NET

Je součástí HDInsight verze 3.6 mono verze 4.2.1. Další informace o verzi Mono zahrnuté do HDInsight naleznete v tématu [HDInsight verze součástí](hdinsight-component-versioning.md). K instalaci na konkrétní verzi Mono naleznete [instalovat nebo aktualizovat Mono](hdinsight-hadoop-install-mono.md) dokumentu.

Další informace o kompatibilitě mezi Mono a rozhraní .NET najdete v tématu [Mono kompatibility (http://www.mono-project.com/docs/about-mono/compatibility/)](http://www.mono-project.com/docs/about-mono/compatibility/) dokumentu.

> [!IMPORTANT]
> Rozhraní framework SCP.NET je kompatibilní s Mono. Další informace o používání SCP.NET s Mono najdete v tématu [pomocí Visual Studio pro vývoj topologií C# pro Apache Storm v HDInsight](storm/apache-storm-develop-csharp-visual-studio-topology.md).

## <a name="automated-portability-analysis"></a>Analýza automatizované přenositelnost

[.NET přenositelnost analyzátor](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer) lze použít k vygenerování sestavy problémům s kompatibilitou mezi aplikací a Mono. Pomocí následujících kroků nakonfigurujte analyzátor zkontrolujte vaši aplikaci pro Mono přenositelnost:

1. Nainstalujte [.NET přenositelnost analyzátor](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer). Během instalace vyberte verzi sady Visual Studio používat.

2. Ze sady Visual Studio 2015, vyberte __analyzovat__ > __přenositelnost Analyzátor nastavení__a ujistěte se, že __4.5__ se změnami __Mono__ části.

    ![4.5 změnami Mono části pro Analyzátor nastavení](./media/hdinsight-hadoop-migrate-dotnet-to-linux/portability-analyzer-settings.png)

    Vyberte __OK__ konfiguraci uložíte.

3. Vyberte __analyzovat__ > __analyzovat sestavení přenositelnost__. Vyberte sestavení, které obsahuje řešení a pak vyberte __otevřete__ zahájíte analýzu.

4. Po dokončení analýzy, vybrat __analyzovat__ > __zobrazit sestavy analýzy__. V __výsledky analýzy přenositelnost__, vyberte __otevřete sestavu__ otevřete sestavu.

    ![Dialogové okno výsledků analyzátoru přenositelnost](./media/hdinsight-hadoop-migrate-dotnet-to-linux/portability-analyzer-results.png)

> [!IMPORTANT]
> Analyzátoru nelze catch každých problém s vaším řešením. Například soubor cestu `c:\temp\file.txt` se považuje za normální, pokud Mono běží na systému Windows. Stejná cesta není platná na platformě Linux.

## <a name="manual-portability-analysis"></a>Analýza ruční přenositelnost

Proveďte ruční auditu podle informací uvedených v kódu [přenositelnost aplikace (http://www.mono-project.com/docs/getting-started/application-portability/)](http://www.mono-project.com/docs/getting-started/application-portability/) dokumentu.

## <a name="modify-and-build"></a>Upravit a sestavení

Můžete pokračovat pomocí sady Visual Studio pro vývoj řešení .NET pro HDInsight. Však musí zajistit, aby se projekt konfigurovány k použití rozhraní .NET Framework 4.5.

## <a name="deploy-and-test"></a>Nasazení a testování

Jakmile změníte řešení pomocí doporučení z analyzátor přenositelnost .NET nebo ruční analýzy, je nutné jej otestovat s HDInsight. Testování řešení na clusteru HDInsight se systémem Linux může odhalit jemně problémy, které je třeba opravit. Doporučujeme, abyste povolili další protokolování v aplikaci během testování.

Další informace o přístupu k protokolů najdete v následujících dokumentech:

* [Přístup k protokolům aplikací YARN ve službě HDInsight s Linuxem](hdinsight-hadoop-access-yarn-app-logs-linux.md)

## <a name="next-steps"></a>Další kroky

* [Použití jazyka C# s MapReduce v HDInsight](hadoop/apache-hadoop-dotnet-csharp-mapreduce-streaming.md)

* [Uživatelem definované funkce jazyka C# pomocí Hive a Pig](hadoop/apache-hadoop-hive-pig-udf-dotnet-csharp.md)

* [Vývoj C# topologií pro Storm v HDInsight](storm/apache-storm-develop-csharp-visual-studio-topology.md)