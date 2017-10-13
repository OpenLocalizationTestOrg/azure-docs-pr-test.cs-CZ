---
title: Nainstalujte nebo aktualizujte Mono v HDInsight - Azure | Microsoft Docs
description: "Další informace o použití na konkrétní verzi Mono s clusterem HDInsight. Mono slouží ke spouštění aplikací .NET v clusterech HDInsight se systémem Linux."
services: hdinsight
documentationCenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/31/2017
ms.author: larryfr
ms.custom: hdinsightactive
ms.openlocfilehash: fd284542e1de65f323f1e3a092689f847e025be6
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="install-or-update-mono-on-hdinsight"></a>Nainstalujte nebo aktualizujte Mono v HDInsight

Naučte se instalovat na konkrétní verzi nástroje [Mono](https://www.mono-project.com) na HDInsight 3.4 nebo vyšší.

Mono je nainstalován na HDInsight 3.4 a vyšší a slouží ke spouštění aplikací .NET. Informace o verzi Mono zahrnuté do každé verzi HDInsight, naleznete v části [Správa verzí komponenty HDInsight](hdinsight-component-versioning.md). Chcete-li nainstalovat jinou verzi v clusteru, pomocí akce skriptu v tomto dokumentu. 

## <a name="how-it-works"></a>Jak to funguje

Tento skript přijímá následující parametr:

* __Číslo verze mono__: verze Mono k instalaci. Verze musí být dostupný z [https://download.mono-project.com/repo/debian/dists/wheezy/snapshots/](https://download.mono-project.com/repo/debian/dists/wheezy/snapshots/).

Skript nainstaluje Mono následujících balíčků:

* __dokončení mono__

* __certifikační autority certifikáty mono__

## <a name="the-script"></a>Skript

__Skript umístění__: [https://hdiconfigactions.blob.core.windows.net/install-mono/install-mono.bash](https://hdiconfigactions.blob.core.windows.net/install-mono/install-mono.bash)

__Požadavky na__:

* Skript se musí použít na __hlavní uzly__ a __uzlů pracovního procesu__.

## <a name="to-use-the-script"></a>Pomocí skriptu

Informace o tom, jak pomocí tohoto skriptu s HDInsight naleznete v tématu [HDInsight se systémem Linux přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster) dokumentu. Můžete použít skript prostřednictvím portálu Azure, Azure PowerShell nebo rozhraní příkazového řádku Azure.

Během provádění akce dokumentu skriptu, použijte následující identifikátor URI:

    https://hdiconfigactions.blob.core.windows.net/install-mono/install-mono.bash

> [!NOTE]
> Při konfiguraci HDInsight pomocí tohoto skriptu, označí skript jako __trvalé__. Toto nastavení umožňuje HDInsight použít skript k pracovním uzlům přidávají prostřednictvím škálování operace.


## <a name="next-steps"></a>Další kroky

Jste se naučili postup instalaci nebo upgrade na konkrétní verzi Mono v HDInsight. Další informace o používání aplikací .NET s Mono v HDInsight najdete v následujících dokumentech:

* [Použití rozhraní .NET pro streamování MapReduce v HDInsight](hdinsight-hadoop-dotnet-csharp-mapreduce-streaming.md)
* [Použití rozhraní .NET v Hive a Pig v HDInsight](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md)
* [Vývoj řešení C# se Storm v HDInsight](hdinsight-storm-develop-csharp-visual-studio-topology.md)
* [Migrace řešení .NET do HDInsight se systémem Linux](hdinsight-hadoop-migrate-dotnet-to-linux.md)

Další informace o použití akce skriptu najdete v tématu [HDInsight se systémem Linux přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-customize-cluster-linux.md)