---
title: "Hadoop aplikace jiných výrobců aaaInstall v Azure HDInsight | Microsoft Docs"
description: "Zjistěte, jak aplikací Hadoop tooinstall třetích stran v Azure HDInsight."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: eaf5904d-41e2-4a5f-8bec-9dde069039c2
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/16/2017
ms.author: jgao
ms.openlocfilehash: 00071517c81a17c01dccedf9e8dd5d0cabb38567
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="install-third-party-hadoop-applications-on-azure-hdinsight"></a>Instalace aplikací Hadoop jiných výrobců v Azure HDInsight

V tomto článku se dozvíte, jak tooinstall již publikované aplikace třetích stran Hadoop v Azure HDInsight. Pokyny pro instalaci vašich vlastních aplikací najdete v článku [Instalace vlastních aplikací HDInsight](hdinsight-apps-install-custom-applications.md).

Aplikace HDInsight je aplikace, kterou uživatelé mohou nainstalovat na clusteru HDInsight se systémem Linux. Tyto aplikace mohou být vytvořeny společností Microsoft, nezávislými dodavateli softwaru (ISV) nebo vámi samotnými.  

Aktuálně jsou dostupné čtyři publikované aplikace:

* **DATAIKU DDS v HDInsight**: Dataiku DSS (datové vědy Studio) je software, který umožňuje data tooprototype Odborníci v oblasti (datových vědců, obchodní analytici, vývojáři...), sestavení a nasazení vysoce specifických služeb, které transformace nezpracovaná data do zvládat obchodní předpovědi.
* **Datameer**: [Datameer](http://www.datameer.com/documentation/display/DAS50/Home?ls=Partners&lsd=Microsoft&c=Partners&cd=Microsoft) nabízí analytici toodiscover interaktivní způsob, jak analyzovat a vizualizace výsledků hello velkých objemů dat. Pro vyžádání obsahu v dalších zdrojů dat snadno toodiscover nové relace a získejte odpovědi hello, které potřebujete rychle.
* **Sběrač dat Streamsets pro HDnsight** poskytuje plné integrované vývojové prostředí (IDE), umožňuje návrh, testování, nasazení a správě any-to-any ingestování kanály, které OK sítě datového proudu a dávky dat a zahrnovat celou řadu transformace v proudu – všechny bez nutnosti toowrite vlastní kód. 
* **Obalového souboru CDAP pro HDInsight** poskytuje hello nejprve jednotná platforma pro integraci pro velké objemy dat, která snižuje hello tooproduction času pro data aplikací a dat jezera 80 %. Tato aplikace podporuje pouze clustery Standard HBase 3.4.
* **H2O umělé Intelligence pro HDInsight (Beta)** H2O šumivého horních podporuje následující algoritmy distribuovaného hello: GLM, Naïve Bayes, distribuované náhodných doménové struktury, přechodu počítač zvyšovat skóre, hluboké Neuronové sítě, přímým učení K-means, PCA, Zobecněný nízkou Rank modely, detekce anomálií a Autoencoders.
* **Kyligence Analytics Platform** Kyligence Analytics Platform (KAP) je připravené pro podniky datový sklad, který používá technologii Apache Kylin a Apache Hadoop, umožňuje dílčí sekundu latence dotazu pro datovou sadu masivním měřítku a zjednodušuje analýza dat pro podnikoví uživatelé a analytici. 
* **SnapLogic Hadooplex** hello SnapLogic Hadooplex systémem HDInsight umožňuje zákazníkům tooget toobusiness insights rychlejší zadáním přijímání dat samoobslužné služby a přípravy z prakticky libovolný zdroj toohello cloudu Microsoft Azure Platforma.
* **Serveru úloh Spark pro KNIME Spark vykonavatele** serveru úloh Spark pro vykonavatele KNIME Spark je použité tooconnect hello KNIME Analytics Platform tooHDInsight clustery.

Hello pokyny uvedené v tomto článku používají portál Azure. Můžete také export šablony Azure Resource Manageru hello z portálu hello nebo získejte kopii souboru šablony Resource Manageru hello od dodavatelů a použít Azure PowerShell a rozhraní příkazového řádku Azure toodeploy hello šablony.  Viz článek [Vytváření clusterů Hadoop na systému Linux v HDInsight pomocí šablon Resource Manageru](hdinsight-hadoop-create-linux-clusters-arm-templates.md).

## <a name="prerequisites"></a>Požadavky
Pokud chcete tooinstall aplikace HDInsight na stávající cluster HDInsight, musí mít cluster služby HDInsight. toocreate, najdete v části [vytvořit clustery se](hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster). Aplikace HDInsight můžete také nainstalovat při vytváření clusteru HDInsight.

## <a name="install-applications-tooexisting-clusters"></a>Instalace aplikace tooexisting clustery
Hello následující postup ukazuje, jak tooinstall HDInsight aplikace tooan stávajícího clusteru HDInsight.

**tooinstall aplikace HDInsight**

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Klikněte na tlačítko **clustery HDInsight** v levé nabídce hello.  Pokud ho nevidíte, klikněte na **Další služby** a pak klikněte na **Clustery HDInsight**.
3. Klikněte na cluster HDInsight.  Pokud ho ještě nemáte, vytvořte ho.  Viz článek [Vytvoření clusterů](hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster).
4. Klikněte na tlačítko **aplikace** pod hello **konfigurace** kategorie. Zobrazí se seznam nainstalovaných aplikací. Pokud nemůžete najít aplikace, která znamená, že žádné aplikace pro tuto verzi clusteru HDInsight hello.
   
    ![Nabídka portálu pro aplikace HDInsight](./media/hdinsight-apps-install-applications/hdinsight-apps-portal-menu.png)
5. Klikněte na tlačítko **přidat** z nabídky okno hello. 
   
    ![Nainstalované aplikace aplikací HDInsight](./media/hdinsight-apps-install-applications/hdinsight-apps-installed-apps.png)
   
    Zobrazí se seznam existujících aplikací HDInsight.
   
    ![Dostupné aplikace aplikací HDInsight](./media/hdinsight-apps-install-applications/hdinsight-apps-list.png)
6. Klikněte na jednu z aplikací hello přijměte právní podmínky hello a pak klikněte na tlačítko **vyberte**.

Zobrazí stav instalace hello z hello portálu oznámení (kliknutím na ikonu zvonku hello hello nahoře portálu hello). Po hello je aplikace nainstalována aplikace hello se zobrazí v okně hello nainstalované aplikace.

## <a name="install-applications-during-cluster-creation"></a>Instalace aplikací při vytváření clusteru
Aplikace HDInsight tooinstall možnost hello máte při vytváření clusteru. Během procesu hello instalace aplikací HDInsight po hello clusteru je vytvořen a je v běžícím stavu hello. Hello následující postup ukazuje, jak aplikace HDInsight tooinstall při vytváření clusteru.

**tooinstall aplikace HDInsight**

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Klikněte na **NOVÝ**, **Data + analýzy** a potom klikněte na **HDInsight**.
3. Zadejte **Název clusteru**: Tento název musí být globálně jedinečný.
4. Klikněte na tlačítko **předplatné** tooselect hello předplatné Azure, který se používá pro hello cluster.
5. Klikněte na **Vybrat typ clusteru** a potom vyberte následující:
   
   * **Typ clusteru**: Pokud si nejste jisti, jaké toochoose, vyberte **Hadoop**. Je nejoblíbenější typ clusteru hello.
   * **Operační systém**: Vyberte **Linux**.
   * **Verze**: použijte výchozí verze hello, pokud nevíte, jaká toochoose. Další informace najdete v článku [Verze clusterů HDInsight](hdinsight-component-versioning.md).
   * **Cluster vrstvy**: Azure HDInsight nabízí cloud nabídky velkých objemů dat hello ve dvou kategoriích: úrovně Standard a Premium vrstvy. Další informace najdete v článku [Úrovně clusterů](hdinsight-hadoop-provision-linux-clusters.md#cluster-tiers).
6. Klikněte na tlačítko **aplikace**, klikněte na jednu z hello publikované aplikace a potom klikněte na **vyberte**.
7. Klikněte na tlačítko **pověření** a pak zadejte heslo pro uživatele správce hello. Musíte taky tooenter **uživatelské jméno SSH** a buď **heslo** nebo **veřejný klíč**, což je použité tooauthenticate hello SSH uživatele. Pomocí veřejného klíče je hello doporučenému přístupu. Klikněte na tlačítko **vyberte** v hello dolní toosave hello přihlašovací údaje konfigurace.
8. Klikněte na tlačítko **zdroj dat**, vyberte jednu z hello existující účty úložiště, nebo vytvořit nový účet toobe úložiště, který je použít jako hello výchozí účet úložiště pro hello cluster.
9. Klikněte na tlačítko **skupiny prostředků** tooselect existující prostředek skupiny, nebo klikněte na tlačítko **nový** toocreate novou skupinu prostředků
10. Na hello **nový HDInsight Cluster** okno, ujistěte se, že **Pin tooStartboard** je vybrána a potom klikněte na **vytvořit**. 

## <a name="list-installed-hdinsight-apps-and-properties"></a>Zobrazení seznamu nainstalovaných aplikací HDInsight a jejich vlastností
Hello portál zobrazuje seznam hello nainstalované aplikace HDInsight pro cluster a hello vlastnosti každou nainstalovanou aplikaci.

**toolist HDInsight aplikace a zobrazit vlastnosti**

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Klikněte na tlačítko **clustery HDInsight** v levé nabídce hello.  Pokud ho nevidíte, klikněte na tlačítko **Procházet** a pak klikněte na tlačítko **Clustery HDInsight**.
3. Klikněte na cluster HDInsight.
4. Z hello **nastavení** okně klikněte na tlačítko **aplikace** pod hello **Obecné** kategorie. okno nainstalované aplikace Hello uvádí všechny hello nainstalované aplikace. 
   
    ![Nainstalované aplikace aplikací HDInsight](./media/hdinsight-apps-install-applications/hdinsight-apps-installed-apps-with-apps.png)
5. Klikněte na jednu z hello nainstalované aplikace tooshow hello vlastnost. seznamy vlastností okna Hello:
   
   * Název aplikace: Název aplikace.
   * Stav: Stav aplikace. 
   * Webová stránka: adresa URL hello hello webové aplikace, že jste nasadili toohello hraniční uzel. Hello pověření je stejný jako přihlašovací údaje uživatele hello HTTP, který jste nakonfigurovali pro hello cluster hello.
   * Koncový bod HTTP: hello pověření je stejný jako přihlašovací údaje uživatele hello HTTP, který jste nakonfigurovali pro hello cluster hello. 
   * Koncový bod SSH: můžete použít SSH tooconnect toohello hraniční uzel. pověření SSH Hello jsou hello stejná hodnota jako uživatel SSH hello pověření, která jste nakonfigurovali pro hello cluster. Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).
6. Klikněte pravým tlačítkem na aplikace hello toodelete aplikaci a pak klikněte na **odstranit** hello místní nabídce.

## <a name="connect-toohello-edge-node"></a>Připojit toohello hraniční uzel
Můžete připojit pomocí protokolu HTTP a SSH toohello hraniční uzel. informace o koncovém Hello naleznete z hello [portál](#list-installed-hdinsight-apps-and-properties). Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

přihlašovací údaje koncový bod HTTP Hello jsou pověření uživatele hello HTTP, které jste nakonfigurovali pro hello HDInsight cluster; přihlašovací údaje koncový bod SSH Hello jsou hello pověření SSH, které jste nakonfigurovali pro hello HDInsight cluster.

## <a name="troubleshoot"></a>Řešení potíží
V tématu [řešení potíží s instalací hello](hdinsight-apps-install-custom-applications.md#troubleshoot-the-installation).

## <a name="next-steps"></a>Další kroky
* [Instalace vlastních aplikací HDInsight](hdinsight-apps-install-custom-applications.md): Zjistěte, jak toodeploy publikování aplikace tooHDInsight HDInsight.
* [Publikování aplikací HDInsight](hdinsight-apps-publish-applications.md): Zjistěte, jak toopublish vaše vlastní tooAzure aplikace HDInsight Marketplace.
* [MSDN: Instalace aplikace HDInsight](https://msdn.microsoft.com/library/mt706515.aspx): Zjistěte, jak toodefine aplikace HDInsight.
* [Přizpůsobení clusterů HDInsight se systémem Linux pomocí akce skriptu](hdinsight-hadoop-customize-cluster-linux.md): Zjistěte, jak toouse akce skriptu tooinstall další aplikace.
* [Vytvořit clustery se systémem Linux Hadoop v HDInsight pomocí šablony Resource Manageru](hdinsight-hadoop-create-linux-clusters-arm-templates.md): Zjistěte, jak toocreate šablony Resource Manageru toocall HDInsight clustery.
* [Použít prázdný edge uzly v HDInsight](hdinsight-apps-use-edge-node.md): Zjistěte, jak toouse prázdnou okraj uzel pro přístup ke clusteru HDInsight, testování aplikací HDInsight a hostování aplikace HDInsight.

