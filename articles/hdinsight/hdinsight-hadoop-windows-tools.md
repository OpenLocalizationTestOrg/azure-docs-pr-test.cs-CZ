---
title: "aaaUse na počítači s Windows s Hadoop v HDInsight - Azure | Microsoft Docs"
description: "Pracovat z Windows PC v Hadoop v HDInsight. Spravovat a dotaz clusterů pomocí nástroje PowerShell, sady Visual Studio a Linux. Vývoj řešení pro velká data pomocí rozhraní .NET."
services: hdinsight
keywords: "hadoop v systému windows, hadoop pro windows"
author: cjgronlund
manager: jhubbard
ms.author: cgronlun
ms.date: 05/17/2017
ms.topic: article
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.openlocfilehash: 7c93f16e93349c0b8fb1abd55320c2c172b93aa9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="work-in-hello-hadoop-ecosystem-on-hdinsight-from-a-windows-pc"></a>Práce v hello ekosystém Hadoop v HDInsight z počítačů s Windows

Další informace o vývoj a možnosti správy na počítači s Windows hello pro práci v hello ekosystému Hadoop v HDInsight. 

HDInsight je založena na Apache Hadoop a komponent systému Hadoop, technologie open source vyvinuté v systému Linux. HDInsight verze 3.4 a vyšší používá hello distribuční Ubuntu Linux jako hello základní operační systém pro hello cluster. Však můžete pracovat s HDInsight z klienta Windows nebo vývojového prostředí systému Windows.

## <a name="use-powershell-for-deployment-and-management-tasks"></a>Pomocí prostředí PowerShell pro nasazení a Správa úloh
Prostředí Azure PowerShell je skriptovací prostředí, můžete použít toocontrol a automatizaci nasazení a Správa úloh v HDInsight ze systému Windows.

Příklady úloh, které můžete provést pomocí prostředí PowerShell:

* [Vytvoření clusterů pomocí prostředí PowerShell](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)
* [Spouštění dotazů Hive pomocí prostředí PowerShell](hdinsight-hadoop-use-hive-powershell.md)
* [Správa clusterů pomocí prostředí PowerShell](hdinsight-administer-use-powershell.md)

Postupujte podle kroků příliš[instalace a konfigurace prostředí Azure Powershell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) tooget hello nejnovější verzi. Pokud máte skripty, které je třeba upravit toobe toouse hello nové rutiny pro Azure Resource Manager, najdete v části [migrovat tooAzure Resource Manager vývojové nástroje založené pro clustery služby HDInsight](hdinsight-hadoop-development-using-azure-resource-manager.md).

## <a name="utilities-you-can-run-in-a-browser"></a>Nástroje můžete spustit v prohlížeči
Hello následující nástroje mají webové uživatelské rozhraní, které běží v prohlížeči:
* **[Cloudové prostředí Azure (preview)](https://docs.microsoft.com/azure/cloud-shell/quickstart)**  je prostředí příkazového řádku, interaktivní, který je spuštěn v prohlížeči a uvnitř hello portálu Azure.
* **[Webové uživatelské rozhraní Ambari](hdinsight-hadoop-manage-ambari.md)**  správy a monitorování nástroje, které jsou k dispozici v hello portálu Azure, kterou lze použít toomanage různé druhy úloh, jako například:
    * [Pomocí Ambari hello REST API](hdinsight-hadoop-manage-ambari-rest-api.md)
    * [Zobrazení Ambari Hive](hdinsight-hadoop-use-hive-ambari-view.md)
    * [Tez zobrazení Ambari](hdinsight-debug-ambari-tez-view.md)

## <a name="data-lake-hadoop-tools-for-visual-studio"></a>Nástroje data Lake (Hadoop) pro Visual Studio
Pomocí nástrojů Data Lake pro Visual Studio toodeploy a správa topologií Storm. Nástroje data Lake nainstaluje taky hello SCP.NET SDK, která vám umožní toodevelop topologie C# Storm pomocí sady Visual Studio.

Než budete pokračovat toohello následující příklady, [nainstalujte a nástrojů Data Lake pro Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md). 

Příklady úloh, které můžete provést pomocí sady Visual Studio a nástrojů Data Lake pro Visual Studio:
* [Nasazení a správa topologií Storm ze sady Visual Studio](hdinsight-storm-deploy-monitor-topology-linux.md)
* [Vývoj C# topologií pro Storm pomocí sady Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md). Příklad šablony zahrnují Hello bits pro topologií Storm můžete připojit toodatabases, jako je například Azure Cosmos databáze a databáze SQL.

## <a name="visual-studio-and-hello-net-sdk"></a>Visual Studio a hello .NET SDK 

Můžete použít Visual Studio s clustery toomanage hello .NET SDK a vývoji velkých objemů dat aplikací. Můžete použít jiné integrovaného vývojového prostředí pro hello následující úlohy, ale příklady jsou uvedeny v sadě Visual Studio.

Příklady úloh, které můžete provést hello .NET SDK v sadě Visual Studio:
* [Vytváření clusterů a práci v HDInsight z aplikace rozhraní .NET Framework](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md)
* [Spouštění dotazů Hive pomocí sady .NET SDK hello](hdinsight-hadoop-use-hive-dotnet-sdk.md)
* [Uživatelem definované funkce jazyka C# pomocí Hive a Pig streamování na Hadoop](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md)

> TIP Pokud používáte rozhraní .NET řešení s clustery HDInsight se systémem Windows, je vhodná doba tooplan migrace na základě tooLinux clustery. Další informace najdete v tématu [migrovat .NET řešení pro HDInsight se systémem Windows HDInsight se systémem tooLinux](hdinsight-hadoop-migrate-dotnet-to-linux.md).

## <a name="intellij-idea-and-eclipse-ide-for-spark-clusters"></a>Intellij IDEA a Eclipse IDE pro clustery Spark
Obě [Intellij IDEA](https://www.jetbrains.com/idea/download) a hello [Eclipse IDE](https://www.eclipse.org/downloads/) lze provádět:
* Vývoj a odesílání aplikací Scala Spark na clusteru HDInsight Spark.
* Přístup k prostředkům clusteru Spark.
* Vytvořte a spusťte aplikací Scala Spark místně.

Tyto články Zobrazit jak: 
* Intellij IDEA: [Spark vytvořit aplikace s použitím nástrojů Azure pro Intellij modulu plug-in hello a hello Scala SDK.](hdinsight-apache-spark-intellij-tool-plugin.md)
* Eclipse IDE nebo Scala IDE pro Eclipse: [Spark vytvořit aplikace a hello nástrojů Azure pro Eclipse](hdinsight-apache-spark-eclipse-tool-plugin.md) 


## <a name="notebooks-on-spark-for-data-scientists"></a>Poznámkové bloky na Spark pro datových vědců 
Clustery Apache Spark v HDInsight zahrnuje poznámkových bloků Zeppelin a jádra, které lze použít s poznámkovými bloky Jupyter. 

* [Zjistěte, jak toouse jádra na Spark clusterů s aplikací Spark tootest poznámkové bloky Jupyter](hdinsight-apache-spark-zeppelin-notebook.md)
* [Zjistěte, jak poznámkových bloků Zeppelin toouse na Spark clusterů toorun Spark úlohy](hdinsight-apache-spark-jupyter-notebook-kernels.md) 


## <a name="run-linux-based-tools-and-technologies-on-windows"></a>Spuštění nástrojů systémem Linux a technologií v systému Windows

Pokud narazíte na situaci, kdy je nutné použít nástroj nebo technologie, která je k dispozici v systému Linux, zvažte hello následující možnosti:

* **Bash (beta) ve Windows 10** poskytuje subsystému Linux v systému Windows. Bash vám umožní toodirectly spouštět nástroje Linux bez nutnosti toomaintain vyhrazené instalace systému Linux. [Nainstalujte a spusťte hello Bash beta ve Windows 10](https://msdn.microsoft.com/commandline/wsl/install_guide)
* **Docker pro systém Windows** umožňuje přístup nástroje toomany systémem Linux a můžete spustit přímo ze systému Windows. Například můžete Docker toorun hello Beeline klienta pro Hive přímo ze systému Windows. Můžete také pomocí Docker toorun poznámkového bloku Jupyter místní a vzdálené připojení tooSpark v HDInsight. [Začínáme s Docker pro Windows](https://docs.docker.com/docker-for-windows/)
* **[MobaXTerm](http://mobaxterm.mobatek.net/)**  umožňuje systému souborů toographically procházet hello clusteru přes připojení SSH.

## <a name="next-steps"></a>Další kroky
Pokud jste nový tooworking v clusterech se systémem Linux, najdete v části hello postupujte podle články:
* [Nastavit Hadoop, Kafka, Spark nebo jiné clustery](hdinsight-hadoop-provision-linux-clusters.md)
* [Tipy pro clustery služby HDInsight v Linuxu](hdinsight-hadoop-linux-information.md)