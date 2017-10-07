---
title: "Vytvoření - Azure clusteru knihovny Hive aaaAdd během HDInsight | Microsoft Docs"
description: "Zjistěte, jak knihovny Hive tooadd (souborů jar), tooan HDInsight clusteru při vytváření clusteru."
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
ms.openlocfilehash: 2e028a07c3248205def0789af2c262a0774a8f19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-custom-hive-libraries-when-creating-your-hdinsight-cluster"></a>Přidat vlastní knihovny Hive, při vytváření clusteru HDInsight

Pokud máte knihovny, které často používáte s nástrojem Hive v HDInsight, tento dokument obsahuje informace o používání knihovny hello akce skriptu toopre zatížení při vytváření clusteru. Knihovny přidány pomocí hello kroky v tomto dokumentu jsou globálně dostupnou v podregistru – neexistuje žádná potřeba toouse [přidat JAR](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Cli) tooload je.

## <a name="how-it-works"></a>Jak to funguje

Při vytváření clusteru, Volitelně můžete zadat akce skriptu, který spouští skript na uzlech clusteru hello při jejich vytváření. Hello skript v tomto dokumentu přijímá jeden parametr, který je WASB umístění, které obsahuje toobe knihovny (uložené jako soubory jar) hello předem načtená.

Při vytváření clusteru hello skript vytvoří výčet hello soubory, kopíruje je toohello `/usr/lib/customhivelibs/` adresáře na head a pracovní uzly, potom se přidají toohello `hive.aux.jars.path` vlastnost hello `core-site.xml` souboru. Na clusterech se systémem Linux, aktualizuje také hello `hive-env.sh` soubor s hello umístění souborů hello.

> [!NOTE]
> Pomocí akcí skriptů hello v tomto článku zpřístupní hello knihovny v hello následující scénáře:
>
> * **HDInsight se systémem Linux** – při použití hello klienta Hive, **WebHCat**, a **HiveServer2**.
> * **HDInsight se systémem Windows** – Pokud pomocí klienta hello Hive a **WebHCat**.

## <a name="hello-script"></a>skript Hello

**Umístění skriptu**

Pro **clusterech se systémem Linux**: [https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh)

Pro **clusterech se systémem Windows**: [https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1](https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1)

> [!IMPORTANT]
> Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

**Požadavky**

* skripty Hello musí být použité tooboth hello **hlavní uzly** a **uzlů pracovního procesu**.

* Hello JAR chcete tooinstall musí být uložen v úložišti objektů Blob Azure v **jediný kontejner**.

* účet úložiště Hello obsahující hello knihovně souborů jar **musí** být během vytváření clusteru HDInsight propojené toohello. Musí být buď hello výchozí účet úložiště, nebo účet přidané prostřednictvím __volitelné konfiguraci__.

* Hello WASB cesta toohello kontejneru musí být zadány jako parametr toohello akce skriptu. Například pokud hello JAR jsou uloženy v kontejneru nazvaném **knihovny** na účet úložiště s názvem **mystorage**, bude parametr hello  **wasb://libs@mystorage.blob.core.windows.net/** .

  > [!NOTE]
  > Tento dokument předpokládá, je již vytvoření účtu úložiště, kontejner objektů blob a tooit soubory nahrané hello.
  >
  > Pokud jste dosud nevytvořili účet úložiště, můžete tak učinit pomocí hello [portál Azure](https://portal.azure.com). Potom můžete pomocí nástroje, jako [Azure Storage Explorer](http://storageexplorer.com/) toocreate kontejneru v účtu hello a nahrání souborů tooit.

## <a name="create-a-cluster-using-hello-script"></a>Vytvoření clusteru s podporou pomocí skriptu hello

> [!NOTE]
> Hello postupem vytvoření clusteru HDInsight se systémem Linux. toocreate cluster systému Windows vyberte **Windows** jako hello clusteru operačního systému při vytváření clusteru hello a pomocí skriptu Windows (PowerShell) hello místo hello bash skriptu.
>
> Můžete také použít Azure PowerShell nebo hello SDK rozhraní .NET HDInsight toocreate clusteru pomocí tohoto skriptu. Další informace o použití těchto metod najdete v tématu [HDInsight přizpůsobit clustery pomocí akcí skriptů](hdinsight-hadoop-customize-cluster-linux.md).

1. Spuštění zřizování clusteru pomocí kroků hello v [zřizování clusterů HDInsight v Linuxu](hdinsight-hadoop-provision-linux-clusters.md), ale se nedokončí zřizování.

2. Na hello **volitelné konfiguraci** vyberte **akcí skriptů**a zadejte hello následující informace:

   * **NÁZEV**: Zadejte popisný název akce skriptu hello.

   * **Identifikátor URI skriptu**: https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh

   * **HEAD**: zaškrtnete tuto možnost.

   * **PRACOVNÍ**: zaškrtnete tuto možnost.

   * **ZOOKEEPER**: nechat prázdné.

   * **Parametry**: Zadejte hello WASB adresu toohello úložiště a kontejneru účtu, který obsahuje kromě souborů JAR hello. Například  **wasb://libs@mystorage.blob.core.windows.net/** .

3. Na konci hello hello **akcí skriptů**, použijte hello **vyberte** tlačítko toosave hello konfigurace.

4. Na hello **volitelné konfiguraci** vyberte **propojených účtech Storage** a vyberte hello **přidejte klíč k úložišti** odkaz. Vyberte účet úložiště hello, který obsahuje kromě souborů JAR hello a potom pomocí hello **vyberte** nastavení toosave tlačítka a návratové hello **volitelné konfiguraci** okno.

5. Použití hello **vyberte** tlačítko dole hello hello **volitelné konfiguraci** okno toosave hello volitelné konfiguraci informace.

6. Pokračovat zřizování hello clusteru, jak je popsáno v [zřizování clusterů HDInsight v Linuxu](hdinsight-hadoop-provision-linux-clusters.md).

Po dokončení vytváření clusteru, budete moct toouse hello JAR přidány prostřednictvím tohoto skriptu z Hive, bez nutnosti toouse hello `ADD JAR` příkaz.

## <a name="next-steps"></a>Další kroky

Další informace o práci s Hive naleznete v tématu [používání Hive s HDInsight](hdinsight-use-hive.md)
