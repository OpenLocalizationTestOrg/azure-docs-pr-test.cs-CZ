---
title: aaaInstall nebo aktualizovat Mono v HDInsight - Azure | Microsoft Docs
description: "Zjistěte, jak toouse na konkrétní verzi Mono s clusterem HDInsight. Mono je použité toorun aplikací .NET na clustery HDInsight se systémem Linux."
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
ms.openlocfilehash: 1e8a8aaeff231c93a9e232379448517b326da965
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="install-or-update-mono-on-hdinsight"></a>Nainstalujte nebo aktualizujte Mono v HDInsight

Zjistěte, jak tooinstall na konkrétní verzi nástroje [Mono](https://www.mono-project.com) na HDInsight 3.4 nebo vyšší.

Mono je nainstalován na HDInsight 3.4 a vyšší a je použité toorun aplikací .NET. Informace o verzi hello Mono zahrnuté do každé verzi HDInsight, naleznete v části [Správa verzí komponenty HDInsight](hdinsight-component-versioning.md). tooinstall jinou verzi v clusteru, použijte akci skriptu hello v tomto dokumentu. 

## <a name="how-it-works"></a>Jak to funguje

Tento skript přijímá hello následující parametr:

* __Číslo verze mono__: hello verzi Mono tooinstall. Hello verze musí být dostupný z [https://download.mono-project.com/repo/debian/dists/wheezy/snapshots/](https://download.mono-project.com/repo/debian/dists/wheezy/snapshots/).

skript Hello nainstaluje hello následující Mono balíčky:

* __dokončení mono__

* __certifikační autority certifikáty mono__

## <a name="hello-script"></a>skript Hello

__Skript umístění__: [https://hdiconfigactions.blob.core.windows.net/install-mono/install-mono.bash](https://hdiconfigactions.blob.core.windows.net/install-mono/install-mono.bash)

__Požadavky na__:

* skript Hello se musí použít na hello __hlavní uzly__ a __uzlů pracovního procesu__.

## <a name="toouse-hello-script"></a>toouse hello skriptu

Informace o tom, jak toouse tento skript s HDInsight, najdete v části hello [HDInsight se systémem Linux přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster) dokumentu. Můžete použít skript hello prostřednictvím hello portál Azure, Azure PowerShell nebo hello rozhraní příkazového řádku Azure.

Při následující hello dokumentu akce skriptu, použijte následující identifikátor URI hello:

    https://hdiconfigactions.blob.core.windows.net/install-mono/install-mono.bash

> [!NOTE]
> Při konfiguraci HDInsight pomocí tohoto skriptu, označte hello skriptu jako __trvalé__. Toto nastavení umožňuje HDInsight tooapply hello skriptu tooworker uzly přidávají prostřednictvím škálování operace.


## <a name="next-steps"></a>Další kroky

Jste se naučili, jak tooupgrade nebo nainstalujte na konkrétní verzi Mono v HDInsight. Další informace o používání aplikací .NET s Mono v HDInsight naleznete v tématu hello následující dokumenty:

* [Použití rozhraní .NET pro streamování MapReduce v HDInsight](hdinsight-hadoop-dotnet-csharp-mapreduce-streaming.md)
* [Použití rozhraní .NET v Hive a Pig v HDInsight](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md)
* [Vývoj řešení C# se Storm v HDInsight](hdinsight-storm-develop-csharp-visual-studio-topology.md)
* [Migrace řešení pro rozhraní .NET HDInsight se systémem tooLinux](hdinsight-hadoop-migrate-dotnet-to-linux.md)

Další informace o použití akce skriptu najdete v tématu [HDInsight se systémem Linux přizpůsobit clustery pomocí akce skriptu](hdinsight-hadoop-customize-cluster-linux.md)