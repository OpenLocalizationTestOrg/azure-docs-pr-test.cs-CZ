---
title: "aaaInstall vlastní vlastních aplikací v Azure HDInsight Hadoop | Microsoft Docs"
description: "Zjistěte, jak tooinstall aplikace HDInsight na aplikace HDInsight."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: e556b29c-8176-4bc5-a90b-aa01abfd3aee
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/25/2017
ms.author: jgao
ms.openlocfilehash: ed3148f2c4d4d2b568d84e44fa6d76bb5a001902
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="install-custom-hadoop-applications-on-azure-hdinsight"></a>Instalace vlastních aplikací Hadoop v Azure HDInsight

V tomto článku se dozvíte, jak tooinstall Hadoop aplikace v Azure HDInsight, která nebyla publikována toohello portálu Azure. aplikace Hello budete instalovat v tomto článku je [Hue](http://gethue.com/).

Aplikace HDInsight je aplikace, kterou uživatelé mohou nainstalovat na clusteru HDInsight se systémem Linux.  Tyto aplikace mohou být vytvořeny společností Microsoft, nezávislými dodavateli softwaru (ISV) nebo vámi samotnými.  

Další související články:

* [Instalace aplikací HDInsight](hdinsight-apps-install-applications.md): Zjistěte, jak tooinstall tooyour aplikace HDInsight clustery.
* [Publikování aplikací HDInsight](hdinsight-apps-publish-applications.md): Zjistěte, jak toopublish vaše vlastní tooAzure aplikace HDInsight Marketplace.
* [MSDN: Instalace aplikace HDInsight](https://msdn.microsoft.com/library/mt706515.aspx): Zjistěte, jak toodefine aplikace HDInsight.

## <a name="prerequisites"></a>Požadavky
Pokud chcete tooinstall aplikace HDInsight na stávající cluster HDInsight, musí mít cluster služby HDInsight. toocreate, najdete v části [vytvořit clustery se](hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster). Aplikace HDInsight můžete také nainstalovat při vytváření clusteru HDInsight.

## <a name="install-hdinsight-applications"></a>Instalace aplikací HDInsight
Aplikace HDInsight lze nainstalovat při vytváření clusteru nebo tooan stávajícího clusteru HDInsight. Postup definování šablon Azure Resource Manageru, viz část [MSDN: instalace aplikace HDInsight](https://msdn.microsoft.com/library/mt706515.aspx).

Hello soubory potřebné pro nasazení této aplikace (Hue):

* [azuredeploy.JSON](https://github.com/hdinsight/Iaas-Applications/blob/master/Hue/azuredeploy.json): hello šablony Resource Manageru pro instalaci aplikace HDInsight. Viz [MSDN: instalace aplikace HDInsight](https://msdn.microsoft.com/library/mt706515.aspx) pro vývoj vlastní šablony Resource Manageru.
* [HUE-install_v0.sh](https://github.com/hdinsight/Iaas-Applications/blob/master/Hue/scripts/Hue-install_v0.sh): hello akce skriptu volaná šablonou Resource Manager hello pro konfiguraci hello hraniční uzel.
* [HUE-binaries.tgz](https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv01/hue-binaries-14-04.tgz): hello binární soubor Hue volaný ze souboru hui-install_v0.sh.
* [HUE-binární soubory-14-04.tgz](https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv01/hue-binaries-14-04.tgz): hello binární soubor Hue volaný ze souboru hui-install_v0.sh.
* [webwasb tomcat.tar.gz](https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv01/webwasb-tomcat.tar.gz): Ukázková webová aplikace (Tomcat) volaná ze souboru hui-install_v0.sh.

**tooinstall Hue tooan stávající cluster HDInsight**

1. Klikněte na tlačítko hello toosign bitové kopie v tooAzure a otevřete hello šablony Resource Manageru v hello portálu Azure.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhdinsight%2FIaas-Applications%2Fmaster%2FHue%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-apps-install-custom-applications/deploy-to-azure.png" alt="Deploy tooAzure"></a>

    Toto tlačítko otevře šablonu Resource Manageru na hello portálu Azure.  Hello šablony Resource Manageru se nachází v [https://github.com/hdinsight/Iaas-Applications/tree/master/Hue](https://github.com/hdinsight/Iaas-Applications/tree/master/Hue).  toolearn jak toowrite této šablony Resource Manageru, najdete v části [MSDN: instalace aplikace HDInsight](https://msdn.microsoft.com/library/mt706515.aspx).
2. Z hello **parametry** okno, zadejte následující hello:

   * **Název clusteru**: Zadejte název hello hello clusteru, kde chcete tooinstall hello aplikace. Tento cluster musí být existující cluster.
3. Klikněte na tlačítko **OK** toosave hello parametry.
4. Z hello **vlastní nasazení** okno, zadejte **skupiny prostředků**.  Hello skupina prostředků je kontejner, který seskupuje hello cluster, účet závislého úložiště hello a další prostředky. Je potřeba toouse hello stejné skupině prostředků jako hello cluster.
5. Klikněte na tlačítko **Smluvní podmínky** a pak klikněte na tlačítko **Vytvořit**.
6. Ověřte hello **Pin toodashboard** zaškrtávací políčko je zaškrtnuto a potom klikněte na **vytvořit**. Zobrazí stav instalace hello z řídicího panelu hello dlaždice připnuté toohello portálu a hello portálu oznámení (kliknutím na ikonu zvonku hello hello nahoře portálu hello).  Jak dlouho trvá přibližně 10 minut tooinstall hello aplikace.

**tooinstall Hue při vytváření clusteru**

1. Klikněte na tlačítko hello toosign bitové kopie v tooAzure a otevřete hello šablony Resource Manageru v hello portálu Azure.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fhdinsightapps%2Fcreate-linux-based-hadoop-cluster-in-hdinsight.json" target="_blank"><img src="./media/hdinsight-apps-install-custom-applications/deploy-to-azure.png" alt="Deploy tooAzure"></a>

    Toto tlačítko otevře šablonu Resource Manageru na hello portálu Azure.  Hello šablony Resource Manageru se nachází v [https://hditutorialdata.blob.core.windows.net/hdinsightapps/create-linux-based-hadoop-cluster-in-hdinsight.json](https://hditutorialdata.blob.core.windows.net/hdinsightapps/create-linux-based-hadoop-cluster-in-hdinsight.json).  toolearn jak toowrite této šablony Resource Manageru, najdete v části [MSDN: instalace aplikace HDInsight](https://msdn.microsoft.com/library/mt706515.aspx).
2. Postupujte podle hello instrukce toocreate clusteru a instalace aplikace Hue. Další informace o vytváření clusterů HDInsight naleznete v tématu [Vytváření clusterů Hadoop založených na Linuxu v nástroji HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

Kromě toho toohello portálu Azure, můžete použít také [prostředí Azure PowerShell](hdinsight-hadoop-create-linux-clusters-arm-templates.md#deploy-with-powershell) a [rozhraní příkazového řádku Azure](hdinsight-hadoop-create-linux-clusters-arm-templates.md#deploy-with-cli) toocall šablony Resource Manageru.

## <a name="validate-hello-installation"></a>Ověření instalace hello
Můžete zkontrolovat stav aplikace hello na hello instalace aplikace Azure portálu toovalidate hello. Kromě toho můžete také ověřit, všechny koncové body HTTP vychází dle očekávání a webovou stránku hello pokud existuje:

**portál Hue tooopen hello**

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Klikněte na tlačítko **clustery HDInsight** v levé nabídce hello.  Pokud ho nevidíte, klikněte na tlačítko **Procházet** a pak klikněte na tlačítko **Clustery HDInsight**.
3. Klikněte na tlačítko hello clusteru, kam jste nainstalovali aplikaci hello.
4. Z hello **nastavení** okně klikněte na tlačítko **aplikace** pod hello **Obecné** kategorie. Zobrazí se **hue** uvedené v hello **nainstalované aplikace** okno.
5. Klikněte na tlačítko **hue** z hello seznamu toolist hello vlastností.  
6. Klikněte na tlačítko hello webová stránka odkaz toovalidate hello webu; otevření koncového bodu hello HTTP v prohlížeče toovalidate hello uživatelské rozhraní webu Hue, otevřete hello SSH koncového bodu pomocí SSH. Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="troubleshoot-hello-installation"></a>Řešení potíží s instalací hello
Můžete zkontrolovat stav instalace aplikace hello hello portálu oznámení (kliknutím na ikonu zvonku hello hello nahoře portálu hello).

Pokud se nezdařila instalace aplikace, můžete zobrazit hello chybové zprávy a informace ladění ze 3 míst:

* Aplikace HDInsight: obecné informace o chybě.

    Otevřete hello clusteru z portálu hello a klikněte na tlačítko aplikace hello okno nastavení:

    ![hdinsight aplikace aplikace chyba instalace](./media/hdinsight-apps-install-applications/hdinsight-apps-error.png)
* Akce skriptu HDInsight: Pokud chybová zpráva aplikace HDInsight hello indikuje selhání akce skriptu, další informace o selhání skriptu hello zobrazí v podokně Akce skriptu hello.

    V okně Nastavení hello klikněte na tlačítko akce skriptu. Zobrazí historii akcí skriptu hello chybové zprávy

    ![hdinsight aplikace skript akce chyba](./media/hdinsight-apps-install-applications/hdinsight-apps-script-action-error.png)
* Webovému uživatelskému rozhraní Ambari: Pokud hello instalační skript se hello příčinu selhání hello, použijte webové uživatelské rozhraní Ambari toocheck úplných protokolů týkajících se skriptů instalace hello.

    Další informace naleznete v tématu [Poradce při potížích](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting).

## <a name="remove-hdinsight-applications"></a>Odstranění aplikací HDInsight
Existuje několik způsobů toodelete HDInsight aplikací.

### <a name="use-portal"></a>Použít portál
**tooremove aplikaci pomocí portálu hello**

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Klikněte na tlačítko **clustery HDInsight** v levé nabídce hello.  Pokud ho nevidíte, klikněte na tlačítko **Procházet** a pak klikněte na tlačítko **Clustery HDInsight**.
3. Klikněte na tlačítko hello clusteru, kam jste nainstalovali aplikaci hello.
4. Z hello **nastavení** okně klikněte na tlačítko **aplikace** pod hello **Obecné** kategorie. Zobrazí se seznam nainstalovaných aplikací. V tomto kurzu **hue** uvedené v hello **nainstalované aplikace** okno.
5. Klikněte pravým tlačítkem na aplikace hello tooremove a pak klikněte na **odstranit**.
6. Klikněte na tlačítko **Ano** tooconfirm.

Z portálu hello můžete také odstranit hello cluster nebo odstranit skupinu prostředků hello, která obsahuje aplikaci hello.

### <a name="use-azure-powershell"></a>Použití Azure Powershell
Pomocí Azure PowerShell, můžete odstranit hello cluster nebo odstranit skupinu prostředků hello. Viz téma [Odstranění clusterů pomocí Azure PowerShell](hdinsight-administer-use-powershell.md#delete-clusters).

### <a name="use-azure-cli"></a>Použití Azure CLI
Použití Azure CLI, můžete odstranit hello cluster nebo odstranit skupinu prostředků hello. Viz téma [Odstranění clusterů pomocí Azure CLI](hdinsight-administer-use-command-line.md#delete-clusters).

## <a name="next-steps"></a>Další kroky
* [MSDN: Instalace aplikace HDInsight](https://msdn.microsoft.com/library/mt706515.aspx): Zjistěte, jak toodevelop šablony Resource Manageru pro nasazení aplikací HDInsight.
* [Instalace aplikací HDInsight](hdinsight-apps-install-applications.md): Zjistěte, jak tooinstall tooyour aplikace HDInsight clustery.
* [Publikování aplikací HDInsight](hdinsight-apps-publish-applications.md): Zjistěte, jak toopublish vaše vlastní tooAzure aplikace HDInsight Marketplace.
* [Přizpůsobení clusterů HDInsight se systémem Linux pomocí akce skriptu](hdinsight-hadoop-customize-cluster-linux.md): Zjistěte, jak toouse akce skriptu tooinstall další aplikace.
* [Vytvořit clustery se systémem Linux Hadoop v HDInsight pomocí šablony Resource Manageru](hdinsight-hadoop-create-linux-clusters-arm-templates.md): Zjistěte, jak toocreate šablony Resource Manageru toocall HDInsight clustery.
* [Použít prázdný edge uzly v HDInsight](hdinsight-apps-use-edge-node.md): Zjistěte, jak toouse prázdnou okraj uzel pro přístup ke clusteru HDInsight, testování aplikací HDInsight a hostování aplikace HDInsight.
