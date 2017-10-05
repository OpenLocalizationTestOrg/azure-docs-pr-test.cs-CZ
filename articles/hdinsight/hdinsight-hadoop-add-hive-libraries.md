---
title: "Přidání knihovny Hive během vytváření clusteru HDInsight - Azure | Microsoft Docs"
description: "Informace o postupu přidání knihovny Hive (soubory jar) do clusteru HDInsight při vytváření clusteru."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 2fd74b8d-c006-45c6-a9e2-72ff5d2d978a
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/12/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: 3412864384961e8820d6700c1bf22a4cae64ba4b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="add-custom-hive-libraries-when-creating-your-hdinsight-cluster"></a>Přidat vlastní knihovny Hive, při vytváření clusteru HDInsight

Pokud máte knihovny, které často používáte s nástrojem Hive v HDInsight, tento dokument obsahuje informace o použití akce skriptu pro předběžné načtení knihovny při vytváření clusteru. Knihovny přidány, pomocí kroků v tomto dokumentu jsou globálně dostupnou v podregistru - není nutné používat [přidat JAR](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Cli) k jejich načtení.

## <a name="how-it-works"></a>Jak to funguje

Při vytváření clusteru, Volitelně můžete zadat akce skriptu, který spouští skript na uzlech clusteru při jejich vytváření. Skript v tomto dokumentu přijímá jeden parametr, který je WASB umístění, které obsahuje předem načíst knihovny (uložené jako soubory jar).

Při vytváření clusteru, skript vytvoří výčet soubory, kopíruje je do `/usr/lib/customhivelibs/` přidá je do adresáře na head a pracovní uzly, pak `hive.aux.jars.path` vlastnost v `core-site.xml` souboru. Na clusterech se systémem Linux, aktualizuje také `hive-env.sh` soubor s umístění souborů.

> [!NOTE]
> Pomocí akcí skriptů v tomto článku zpřístupní v knihovnách v následujících scénářích:
>
> * **HDInsight se systémem Linux** – při použití Hive klienta, **WebHCat**, a **HiveServer2**.
> * **HDInsight se systémem Windows** – při použití Hive klienta a **WebHCat**.

## <a name="the-script"></a>Skript

**Umístění skriptu**

Pro **clusterech se systémem Linux**: [https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh)

Pro **clusterech se systémem Windows**: [https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1](https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1)

> [!IMPORTANT]
> HDInsight od verze 3.4 výše používá výhradně operační systém Linux. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

**Požadavky**

* Skripty se musí použít pro obě **hlavní uzly** a **uzlů pracovního procesu**.

* JAR chcete nainstalovat, musí být uložen v úložišti objektů Blob Azure v **jediný kontejner**.

* Účet úložiště obsahuje knihovnu souborů jar **musí** propojit během vytváření clusteru HDInsight. Musí být buď výchozí účet úložiště, nebo účet přidané prostřednictvím __volitelné konfiguraci__.

* WASB cesta ke kontejneru musí být zadána jako parametr pro akce skriptu. Například, pokud kromě souborů JAR jsou uloženy v kontejneru nazvaném **knihovny** na účet úložiště s názvem **mystorage**, bude parametr  **wasb://libs@mystorage.blob.core.windows.net/** .

  > [!NOTE]
  > Tento dokument předpokládá, že jste již vytvoření účtu úložiště, kontejner objektů blob a odeslat soubory do ní.
  >
  > Pokud jste nevytvořili účet úložiště, můžete to udělat tak prostřednictvím [portál Azure](https://portal.azure.com). Potom můžete pomocí nástroje, jako [Azure Storage Explorer](http://storageexplorer.com/) vytvořit kontejner v účtu a odeslat soubory do ní.

## <a name="create-a-cluster-using-the-script"></a>Vytvoření clusteru s podporou pomocí skriptu

> [!NOTE]
> Následující postup vytvoření clusteru HDInsight se systémem Linux. K vytvoření clusteru se systémem Windows, vyberte **Windows** jako cluster operačního systému při vytváření clusteru a použití skriptu systému Windows (PowerShell) namísto skriptů bash.
>
> Prostředí Azure PowerShell nebo sady SDK rozhraní .NET HDInsight můžete také použít k vytvoření clusteru pomocí tohoto skriptu. Další informace o použití těchto metod najdete v tématu [HDInsight přizpůsobit clustery pomocí akcí skriptů](hdinsight-hadoop-customize-cluster-linux.md).

1. Spuštění zřizování clusteru pomocí kroků v [zřizování clusterů HDInsight v Linuxu](hdinsight-hadoop-provision-linux-clusters.md), ale se nedokončí zřizování.

2. Na **volitelné konfiguraci** vyberte **akcí skriptů**a zadejte následující informace:

   * **NÁZEV**: Zadejte popisný název akce skriptu.

   * **Identifikátor URI skriptu**: https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh

   * **HEAD**: zaškrtnete tuto možnost.

   * **PRACOVNÍ**: zaškrtnete tuto možnost.

   * **ZOOKEEPER**: nechat prázdné.

   * **Parametry**: Zadejte adresu WASB účtu úložiště a kontejneru, který obsahuje JAR. Například  **wasb://libs@mystorage.blob.core.windows.net/** .

3. V dolní části **akcí skriptů**, použijte **vyberte** tlačítko Uložit konfiguraci.

4. Na **volitelné konfiguraci** vyberte **propojených účtech Storage** a vyberte **přidejte klíč k úložišti** odkaz. Vyberte účet úložiště, který obsahuje JAR a potom pomocí **vyberte** tlačítka pro uložení nastavení a návrat **volitelné konfiguraci** okno.

5. Použití **vyberte** tlačítko v dolní části **volitelné konfiguraci** okno a uložte informace o volitelné konfiguraci.

6. Pokračovat zřizování clusteru, jak je popsáno v [zřizování clusterů HDInsight v Linuxu](hdinsight-hadoop-provision-linux-clusters.md).

Po dokončení vytváření clusteru, budete moci použít JAR přidány prostřednictvím tohoto skriptu z Hive, bez nutnosti použití `ADD JAR` příkaz.

## <a name="next-steps"></a>Další kroky

Další informace o práci s Hive naleznete v tématu [používání Hive s HDInsight](hdinsight-use-hive.md)
