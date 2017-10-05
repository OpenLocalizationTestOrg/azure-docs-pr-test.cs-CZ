---
title: "Publikování aplikací HDInsight - Azure | Microsoft Docs"
description: "Naučte se vytvářet a publikovat aplikace HDInsight."
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
ms.openlocfilehash: 6aa66cac35bc317fc87003e6c3d824544c53de88
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="publish-hdinsight-applications-into-the-azure-marketplace"></a>Publikování aplikací HDInsight do Azure Marketplace.
Aplikace HDInsight je aplikace, kterou uživatelé mohou nainstalovat na clusteru HDInsight se systémem Linux. Tyto aplikace mohou být vytvořeny společností Microsoft, nezávislými dodavateli softwaru (ISV) nebo vámi samotnými. V tomto článku se dozvíte, jak publikovat aplikace HDInsight do Azure Marketplace.  Obecné informace o publikování na webu Azure Marketplace naleznete v tématu [publikování nabídky v Azure Marketplace](../marketplace-publishing/marketplace-publishing-getting-started.md).

Aplikace HDInsight využívají model *Přineste si vlastní licenci (BYOL)*, kde je poskytovatel aplikace zodpovědný za licencování aplikace pro koncové uživatele a koncovým uživatelům se účtují poplatky z Azure za prostředky, které vytvářejí, například cluster HDInsight a jeho virtuální počítače / uzly. Fakturace za samotné aplikace se v současné době neprovádí prostřednictvím Azure.

Další HDInsight týkající se aplikace článku:

* [Instalace aplikací HDInsight](hdinsight-apps-install-applications.md): Naučte se instalovat aplikace HDInsight do svých clusterů.
* [Instalace vlastních aplikací HDInsight](hdinsight-apps-install-custom-applications.md): Naučte se instalovat a testovat vlastní aplikace HDInsight.

## <a name="prerequisites"></a>Požadavky
Odeslat vlastní aplikace na webu marketplace, musíte mít vytvořit a otestovat vlastní aplikaci. Viz následující články:

* [Instalace vlastních aplikací HDInsight](hdinsight-apps-install-custom-applications.md): Naučte se instalovat a testovat vlastní aplikace HDInsight.

Je nutné také zaregistrovaný vývojářského účtu. Viz [Publikování nabídky pro Azure Marketplace](../marketplace-publishing/marketplace-publishing-getting-started.md) a [Vytvoření účtu Microsoft Developer](../marketplace-publishing/marketplace-publishing-accounts-creation-registration.md).

## <a name="define-application"></a>Definování aplikace
Existují dva kroky zahrnuté pro publikování aplikací do Azure Marketplace.  Nejprve definujte soubor **createUiDef.json** a uveďte, se kterými clustery je vaše aplikace kompatibilní a pak publikujte šablonu z portálu Azure. V následující části je ukázkový soubor createUiDef.json.

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
| typy |Typy clusterů, se kterými je aplikace kompatibilní. |Hadoop, HBase, Storm, Spark (nebo jejich kombinace) |
| vrstvy |Úrovně clusterů, se kterými je aplikace kompatibilní. |Standardní, Premium (nebo obě) |
| verze |Typy clusterů HDInsight, se kterými je aplikace kompatibilní. |3.4 |

## <a name="application-install-script"></a>Skript instalace aplikace
Vždy, když je nainstalována aplikace v clusteru (stávající nebo novou), se vytvoří hraniční uzel a instalační skript aplikace běží na něm.
  > [!IMPORTANT]
  > Název názvy instalačních skriptů aplikace musí být jedinečný pro konkrétní cluster v následujícím formátu.
  > 
  > name": "[concat('hue-install-v0','-' ,uniquestring(‘applicationName’)]"
  > 
  > Všimněte si, že existují tři části názvu skriptu:
  > 
  > 1. Předpona názvu skriptu, která zahrnuje název aplikace nebo název relevantní pro aplikaci.
  > 2. A „-“ pro lepší čitelnost.
  > 3. Jedinečnou funkci řetězce s názvem aplikace jako parametrem.
  > 
  > Příkladem je výše zakončení: hue-install-v0-4wkahss55hlas v seznamu akcí trvalého skriptu. Ukázku datové části JSON naleznete na adrese [https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/Hue/azuredeploy.json](https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/Hue/azuredeploy.json).
  > 
Skript instalace musíte mít následující vlastnosti:
1. Zajistěte, aby byl skript idempotent. Několik volání do skriptu by měl mít stejný výsledek.
2. Tento skript by měl být správně verzí. Používejte jiné umístění pro skript při upgradu nebo testovat změny tak, aby zákazníkům, které se pokoušíte nainstalovat aplikaci, nejsou nijak ovlivněny. 
3. Přidáte odpovídající protokolování do skriptů v každém bodu. Obvykle jsou protokoly skriptu jediný způsob, jak ladění problémů s instalací aplikace.
4. Zajistěte, aby volání externí služby nebo prostředky odpovídající opakování tak, aby instalace není ovlivněn přechodný síťový problémy.
5. Pokud váš skript spouští na uzlech služby, zajistěte, aby služby jsou monitorovány a nakonfigurována na automatické spouštění v případě restartování uzlu.

## <a name="package-application"></a>Instalace balíčku
Vytvořte soubor zip, který obsahuje všechny požadované soubory pro instalaci aplikace HDInsight. Budete potřebovat soubor zip v [publikování aplikace](#publish-application).

* [createUiDefinition.json](#define-application).
* mainTemplate.json. Viz ukázka v části [Instalace vlastních aplikací HDInsight](hdinsight-apps-install-custom-applications.md).
* Všechny požadované skripty.

> [!NOTE]
> Soubory aplikace (včetně souborů webové aplikace, pokud existuje) mohou být umístěny na jakémkoli veřejně přístupném koncovém bodu.
> 

## <a name="publish-application"></a>Publikování aplikace
K publikování aplikace HDInsight postupujte podle následujících kroků:

1. Přihlaste se do [portálu Azure Publishing](https://publish.windowsazure.com/).
2. Klikněte na položku **Šablony řešení** vlevo a vytvořte novou šablonu řešení.
3. Zadejte název a potom klikněte na **Vytvořit novou šablonu řešení**.
4. Pokud jste tak dosud neučinili, klikněte na tlačítko **Účet centra vývojářů pro vytvoření a zapojte se do programu Azure** pro registraci vaší společnosti.  Viz [Vytvoření účtu Microsoft Developer](../marketplace-publishing/marketplace-publishing-accounts-creation-registration.md).
5. Klikněte na tlačítko **Pro zahájení definovat některé topologie**. Šablona řešení je "nadřazená" pro všechny její topologie. V jedné šabloně nabídky/řešení můžete definovat více topologií. Pokud je nabídka vložena do přípravy, je vložena se svým topologiím. 
6. Zadejte název topologie a potom klikněte na symbol plus.
7. Zadejte novou verzi a klikněte na symbol plus.
8. Nahrajte soubor zip připravený v [Balicí aplikaci](#package-application).  
9. Klikněte na tlačítko **Požádat o certifikaci**. Tým certifikace společnosti Microsoft soubory zkontroluje a topologii certifikuje.

## <a name="next-steps"></a>Další kroky
* [Instalace aplikací HDInsight](hdinsight-apps-install-applications.md): Naučte se instalovat aplikace HDInsight do svých clusterů.
* [Instalace vlastních aplikací HDInsight](hdinsight-apps-install-custom-applications.md): naučit se nasazovat nepublikované aplikace HDInsight do HDInsight.
* [Přizpůsobení clusterů HDInsight v systému Linux pomocí akce skriptu](hdinsight-hadoop-customize-cluster-linux.md): další informace o použití akce skriptu k instalaci dalších aplikací.
* [Vytvoření linuxových clusterů Hadoop v HDInsight pomocí šablon Azure Resource Manageru](hdinsight-hadoop-create-linux-clusters-arm-templates.md): zjistěte, jak voláním šablon Resource Manageru vytvoříte clustery HDInsight.
* [Použití prázdných hraničních uzlů v HDInsight](hdinsight-apps-use-edge-node.md): Zjistěte, jak lze pomocí prázdných hraničních uzlů přistupovat ke clusteru HDInsight, testovat aplikace HDInsight a hostovat aplikace HDInsight.

