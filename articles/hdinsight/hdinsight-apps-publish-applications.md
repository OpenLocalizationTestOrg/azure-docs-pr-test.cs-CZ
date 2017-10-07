---
title: aplikace HDInsight aaaPublish - Azure | Microsoft Docs
description: "Zjistěte, jak toocreate a publikovat aplikace HDInsight."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 14aef891-7a37-4cf1-8f7d-ca923565c783
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/25/2017
ms.author: jgao
ms.openlocfilehash: 7da0aa53828563e50ef372df901e1ba541fb40be
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="publish-hdinsight-applications-into-hello-azure-marketplace"></a>Publikování aplikací HDInsight do Azure Marketplace hello
Aplikace HDInsight je aplikace, kterou uživatelé mohou nainstalovat na clusteru HDInsight se systémem Linux. Tyto aplikace mohou být vytvořeny společností Microsoft, nezávislými dodavateli softwaru (ISV) nebo vámi samotnými. V tomto článku se dozvíte, jak toopublish aplikace HDInsight do hello Azure Marketplace.  Obecné informace o publikování na webu Azure Marketplace hello najdete v tématu [publikovat toohello nabídka Azure Marketplace](../marketplace-publishing/marketplace-publishing-getting-started.md).

Aplikace HDInsight využívají hello *přineste si vlastní licenci (BYOL)* model, kde je poskytovatel aplikace zodpovědný za licencování aplikace hello tooend – uživatelé a koncovým uživatelům se účtují poplatky z Azure pro hello prostředky budou Vytvořte, jako je například hello HDInsight cluster a jeho virtuální počítače/uzly. V současné době fakturace pro vlastní aplikace hello neprovádí prostřednictvím Azure.

Další HDInsight týkající se aplikace článku:

* [Instalace aplikací HDInsight](hdinsight-apps-install-applications.md): Zjistěte, jak tooinstall tooyour aplikace HDInsight clustery.
* [Instalace vlastních aplikací HDInsight](hdinsight-apps-install-custom-applications.md): Zjistěte, jak tooinstall a testování vlastních aplikací HDInsight.

## <a name="prerequisites"></a>Požadavky
toosubmit marketplace toohello vaší vlastní aplikaci, musí být vytvořená a otestovat vlastní aplikaci. V tématu hello následující články:

* [Instalace vlastních aplikací HDInsight](hdinsight-apps-install-custom-applications.md): Zjistěte, jak tooinstall a testování vlastních aplikací HDInsight.

Je nutné také zaregistrovaný vývojářského účtu. V tématu [publikovat toohello nabídka Azure Marketplace](../marketplace-publishing/marketplace-publishing-getting-started.md) a [vytvoření účtu Microsoft Developer](../marketplace-publishing/marketplace-publishing-accounts-creation-registration.md).

## <a name="define-application"></a>Definování aplikace
Existují dva kroky zahrnuté pro publikování aplikace toohello Azure Marketplace.  Nejprve definujte **createUiDef.json** tooindicate soubor, který clusterů aplikace kompatibilní se; a pak publikujte šablonu hello z hello portálu Azure. Hello následující části je ukázkový soubor createUiDef.json.

    {
        "handler": "Microsoft.HDInsight",
        "version": "0.0.1-preview",
        "clusterFilters": {
            "types": ["Hadoop", "HBase", "Storm", "Spark"],
            "tiers": ["Standard", "Premium"],
            "versions": ["3.4"]
        }
    }


| Pole | Popis | Možné hodnoty |
| --- | --- | --- |
| typy |typy clusterů Hello, které aplikace hello je kompatibilní s. |Hadoop, HBase, Storm, Spark (nebo jejich kombinace) |
| vrstvy |Hello úrovně clusterů, které je kompatibilní s aplikací hello. |Standardní, Premium (nebo obě) |
| verze |typy clusterů HDInsight, které je kompatibilní s aplikací hello Hello. |3.4 |

## <a name="application-install-script"></a>Skript instalace aplikace
Vždy, když je nainstalována aplikace v clusteru (stávající nebo novou), se vytvoří hraniční uzel a je pro něj spustit skript instalace aplikace hello.
  > [!IMPORTANT]
  > Název Hello hello názvy instalačních skriptů aplikace musí být jedinečný pro konkrétní cluster s hello formátu.
  > 
  > name": "[concat('hue-install-v0','-' ,uniquestring(‘applicationName’)]"
  > 
  > Všimněte si, že existují tři název skriptu toohello částí:
  > 
  > 1. Předpona názvu skriptu, který obsahuje název aplikace hello nebo je název příslušné toohello aplikace.
  > 2. A „-“ pro lepší čitelnost.
  > 3. Jedinečnou funkci řetězce s názvem aplikace hello jako hello parametr.
  > 
  > Příkladem je výše hello skončilo stal: hue-install-v0-4wkahss55hlas v hello trvalé seznamu akce skriptu. Ukázku datové části JSON naleznete na adrese [https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/Hue/azuredeploy.json](https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/Hue/azuredeploy.json).
  > 
skript instalace Hello musí mít hello následující vlastnosti:
1. Zkontrolujte, zda text hello skriptu idempotent. Více volání by měl vytvořit skript toohello hello stejného výsledku.
2. Hello skriptu by měl být správně verzí. Používejte jiné umístění pro skript hello při upgradu nebo testovat změny tak, aby zákazníkům, kteří se snaží tooinstall hello aplikace, nejsou nijak ovlivněny. 
3. Přidáte skripty toohello odpovídající protokolování u každého bodu. Obvykle hello skriptu protokoly jsou pouze toodebug způsob hello problémů s instalací aplikace.
4. Zajistěte, aby volání tooexternal služby nebo prostředky odpovídající opakování tak, aby instalace hello nemá vliv přechodný síťový problémy.
5. Pokud váš skript spouští na uzlech hello služby, zajistěte, aby hello služby jsou monitorovány a nakonfigurovat toostart automaticky v případě restartování uzlu.

## <a name="package-application"></a>Instalace balíčku
Vytvořte soubor zip, který obsahuje všechny požadované soubory pro instalaci aplikace HDInsight. Soubor zip v potřebovat hello [publikování aplikace](#publish-application).

* [createUiDefinition.json](#define-application).
* mainTemplate.json. Viz ukázka v části [Instalace vlastních aplikací HDInsight](hdinsight-apps-install-custom-applications.md).
* Všechny požadované skripty.

> [!NOTE]
> Hello soubory aplikace (včetně souborů webové aplikace, pokud existuje) může být umístěn na jakékoli veřejně přístupném koncovém bodu.
> 

## <a name="publish-application"></a>Publikování aplikace
Postupujte podle následujících kroků toopublish aplikace HDInsight hello:

1. Přihlaste se toohello [portálu Azure Publishing](https://publish.windowsazure.com/).
2. Klikněte na tlačítko **šablony řešení** z levé toocreate hello novou šablonu řešení.
3. Zadejte název a potom klikněte na **Vytvořit novou šablonu řešení**.
4. Klikněte na tlačítko **účet Centra vývojářů pro vytvoření a připojení k Azure programu hello** tooregister vaší společnosti, pokud jste tak dosud neučinili.  Viz [Vytvoření účtu Microsoft Developer](../marketplace-publishing/marketplace-publishing-accounts-creation-registration.md).
5. Klikněte na tlačítko **definovat některé topologie tooget Začínáme**. Šablona řešení je "nadřazená" tooall svým topologiím. V jedné šabloně nabídky/řešení můžete definovat více topologií. Pokud je nabídka vložena toostaging, je vložena se svým topologiím. 
6. Zadejte název topologie a pak klikněte na znaménko plus hello.
7. Zadejte novou verzi a potom klikněte na symbol plus hello.
8. Soubor zip hello nahrávání připravené v [balíčku aplikace](#package-application).  
9. Klikněte na tlačítko **Požádat o certifikaci**. tým certifikace společnosti Microsoft Hello se zkontrolujte soubory hello a certifikovat hello topologie.

## <a name="next-steps"></a>Další kroky
* [Instalace aplikací HDInsight](hdinsight-apps-install-applications.md): Zjistěte, jak tooinstall tooyour aplikace HDInsight clustery.
* [Instalace vlastních aplikací HDInsight](hdinsight-apps-install-custom-applications.md): Zjistěte, jak toodeploy publikování aplikace tooHDInsight HDInsight.
* [Přizpůsobení clusterů HDInsight se systémem Linux pomocí akce skriptu](hdinsight-hadoop-customize-cluster-linux.md): Zjistěte, jak toouse akce skriptu tooinstall další aplikace.
* [Vytvořit clustery se systémem Linux Hadoop v HDInsight pomocí šablony Azure Resource Manager](hdinsight-hadoop-create-linux-clusters-arm-templates.md): Zjistěte, jak toocreate šablony Resource Manageru toocall HDInsight clustery.
* [Použít prázdný edge uzly v HDInsight](hdinsight-apps-use-edge-node.md): Zjistěte, jak toouse prázdnou okraj uzel pro přístup ke clusteru HDInsight, testování aplikací HDInsight a hostování aplikace HDInsight.

