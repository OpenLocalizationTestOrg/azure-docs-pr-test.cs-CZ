---
title: "aaaUse prázdný edge uzly clusterů systému Hadoop v HDInsight - Azure | Microsoft Docs"
description: "Jak tooadd prázdný hraniční uzel tooan HDInsight clusteru, který může být použit jako klient a potom testovacího nebo hostitele aplikace HDInsight."
services: hdinsight
editor: cgronlun
manager: jhubbard
author: mumian
tags: azure-portal
documentationcenter: 
ms.assetid: cdc7d1b4-15d7-4d4d-a13f-c7d3a694b4fb
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: jgao
ms.openlocfilehash: 9c910905b51f2fe92e6e5d47d86a32bd5247c2cd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-empty-edge-nodes-on-hadoop-clusters-in-hdinsight"></a>Použít prázdný edge uzly na clustery systému Hadoop v HDInsight

Zjistěte, jak tooadd prázdnou okraj uzlu clusteru HDInsight tooan. Prázdný hraniční uzel je virtuální počítač s Linuxem s hello stejné nástrojích klienta nainstalován a nakonfigurován jako hello headnodes, ale žádné služby Hadoop systémem. Hello hraniční uzel můžete použít pro přístup k hello clusteru, testování vaší klientské aplikace a hostování vaší klientské aplikace. 

Při vytváření clusteru hello můžete přidat prázdný hraniční uzel tooan stávajícího clusteru HDInsight, tooa nového clusteru. Přidání prázdný hraniční uzel se provádí pomocí šablony Azure Resource Manager.  Hello následující příklad ukazuje, jak se provádí pomocí šablony:

    "resources": [
        {
            "name": "[concat(parameters('clusterName'),'/', variables('applicationName'))]",
            "type": "Microsoft.HDInsight/clusters/applications",
            "apiVersion": "2015-03-01-preview",
            "dependsOn": [ "[concat('Microsoft.HDInsight/clusters/',parameters('clusterName'))]" ],
            "properties": {
                "marketPlaceIdentifier": "EmptyNode",
                "computeProfile": {
                    "roles": [{
                        "name": "edgenode",
                        "targetInstanceCount": 1,
                        "hardwareProfile": {
                            "vmSize": "Standard_D3"
                        }
                    }]
                },
                "installScriptActions": [{
                    "name": "[concat('emptynode','-' ,uniquestring(variables('applicationName')))]",
                    "uri": "[parameters('installScriptAction')]",
                    "roles": ["edgenode"]
                }],
                "uninstallScriptActions": [],
                "httpsEndpoints": [],
                "applicationType": "CustomApplication"
            }
        }
    ],

Jak je znázorněno v ukázce hello, můžete volitelně volat [skript akce](hdinsight-hadoop-customize-cluster-linux.md) tooperform další konfiguraci, například při instalaci [Apache Hue](hdinsight-hadoop-hue-linux.md) v uzlu edge hello. Hello skript akce skriptu musí být veřejně přístupné na webu hello.  Například pokud hello skript je uložený v úložišti Azure, použijte veřejné kontejnery nebo veřejné objekty BLOB.

velikost virtuálního počítače uzlu edge Hello musí splňovat požadavky velikost virtuálního počítače pracovní hello HDInsight clusteru uzlu. Hello nedoporučuje pracovní uzel velikosti virtuálních počítačů, najdete v části [vytvoření Hadoop clusterů v HDInsight](hdinsight-hadoop-provision-linux-clusters.md#cluster-types).

Po vytvoření hraniční uzel, můžete připojit pomocí protokolu SSH toohello hraniční uzel a spusťte klienta nástroje tooaccess hello Hadoop clusteru v prostředí HDInsight.

> [!WARNING] 
> Pomocí prázdné hraniční uzel s HDInsight je aktuálně ve verzi preview. Vlastní komponenty, které jsou nainstalovány na uzlu edge hello získání vyvineme podpory společnosti Microsoft. Může to způsobit řešení problémů, na které narazíte. Nebo může být toocommunity odkazované prostředky pro další pomoc. Hello Toto jsou některé hello většina aktivní lokality pro získání pomoci z komunity hello:
>
> * [Fórum MSDN pro HDInsight](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight)
> * [http://stackoverflow.com](http://stackoverflow.com).
>
> Pokud používáte technologie Apache, možná budete moct toofind pomoc prostřednictvím hello Apache projektu weby na [http://apache.org](http://apache.org), jako je například hello [Hadoop](http://hadoop.apache.org/) lokality.

## <a name="add-an-edge-node-tooan-existing-cluster"></a>Přidání okraj uzlu tooan stávajícího clusteru
V této části použijte Správce prostředků šablony tooadd hraniční uzel tooan stávajícího clusteru HDInsight.  šablony Resource Manageru Hello lze nalézt v [Githubu](https://azure.microsoft.com/en-us/resources/templates/101-hdinsight-linux-add-edge-node/). šablony Resource Manageru Hello volá akci skriptu nacházející se v https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-hdinsight-linux-add-edge-node/scripts/EmptyNodeSetup.sh. skript Hello nebude provádět žádné akce.  Je toodemonstrate volání akce skriptu z šablony Resource Manageru.

**tooadd prázdný hraniční uzel tooan stávajícího clusteru**

1. Pokud ještě nemáte, vytvořte cluster služby HDInsight.  V tématu [kurz Hadoopu: začněte s Hadoop v HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).
2. Klikněte na tlačítko hello toosign bitové kopie v tooAzure a šablony Azure Resource Manageru otevřete hello v hello portálu Azure. 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-add-edge-node%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-apps-use-edge-node/deploy-to-azure.png" alt="Deploy tooAzure"></a>
3. Nakonfigurujte hello následující vlastnosti:
   
   * **Předplatné**: Vyberte předplatné Azure použitý k vytvoření clusteru hello.
   * **Skupina prostředků**: Skupina prostředků vyberte hello používá pro existující cluster HDInsight hello.
   * **Umístění**: Vybrat umístění hello hello stávajícího clusteru HDInsight.
   * **Název clusteru**: Zadejte název hello stávajícího clusteru HDInsight.
   * **Okraj velikost uzlu**: vyberte jednu z hello velikosti virtuálních počítačů. Hello velikost virtuálního počítače musí splňovat požadavky na velikost virtuálního počítače hello pracovní uzly. Hello nedoporučuje pracovní uzel velikosti virtuálních počítačů, najdete v části [vytvoření Hadoop clusterů v HDInsight](hdinsight-hadoop-provision-linux-clusters.md#cluster-types).
   * **Okraj uzlu předpony**: hello výchozí hodnota je **nové**.  Pomocí hello výchozí hodnoty, je název uzlu edge hello **nové edgenode**.  Můžete upravit předponu hello z portálu hello. Můžete také upravit hello úplný název ze šablony hello.

4. Zkontrolujte **souhlasím toohello podmínky a ujednání, které jsou uvedené výše**a potom klikněte na **nákupu** toocreate hello hraniční uzel.

>[!IMPORTANT]
> Ujistěte se, že skupina prostředků Azure pro existující cluster HDInsight hello tooselect hello.  Jinak hodnota zobrazí hello chybová zpráva "nelze provést požadovanou operaci na vnořeného prostředku. Nadřazený prostředek '&lt;ClusterName >' nebyl nalezen. "

## <a name="add-an-edge-node-when-creating-a-cluster"></a>Přidat hraniční uzel, při vytváření clusteru
V této části použijete s hraniční uzel, na cluster HDInsight toocreate šablony Resource Manageru.  šablony Resource Manageru Hello lze nalézt v hello [galerii šablon Azure rychlý Start](https://azure.microsoft.com/documentation/templates/101-hdinsight-linux-with-edge-node/). šablony Resource Manageru Hello volá akci skriptu nacházející se v https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-hdinsight-linux-with-edge-node/scripts/EmptyNodeSetup.sh. skript Hello nebude provádět žádné akce.  Je toodemonstrate volání akce skriptu z šablony Resource Manageru.

**tooadd prázdný hraniční uzel tooan stávajícího clusteru**

1. Pokud ještě nemáte, vytvořte cluster služby HDInsight.  V tématu [začněte s Hadoop v HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).
2. Klikněte na tlačítko hello toosign bitové kopie v tooAzure a šablony Azure Resource Manageru otevřete hello v hello portálu Azure. 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-with-edge-node%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-apps-use-edge-node/deploy-to-azure.png" alt="Deploy tooAzure"></a>
3. Nakonfigurujte hello následující vlastnosti:
   
   * **Předplatné**: Vyberte předplatné Azure použitý k vytvoření clusteru hello.
   * **Skupina prostředků**: Vytvořte novou skupinu prostředků určené pro hello cluster.
   * **Umístění**: Vyberte umístění pro skupinu prostředků hello.
   * **Název clusteru**: Zadejte název pro nový cluster toocreate hello.
   * **Uživatelské jméno přihlášení clusteru**: Zadejte uživatelské jméno hello Hadoop HTTP.  Hello výchozí název je **správce**.
   * **Heslo pro přihlášení clusteru**: Zadejte heslo uživatele hello Hadoop HTTP.
   * **SSH uživatelské jméno**: Zadejte uživatelské jméno SSH hello. Hello výchozí název je **sshuser**.
   * **SSH heslo**: Zadejte heslo uživatele SSH hello.
   * **Instalace akce skriptu**: ponechte výchozí hodnotu hello pro procházení tohoto kurzu.
     
     Některé vlastnosti byly pevně kódovaný v šabloně hello: typ clusteru, počet uzlů pracovního procesu clusteru, velikost uzlu Edge a název uzlu Edge.
4. Zkontrolujte **souhlasím toohello podmínky a ujednání, které jsou uvedené výše**a potom klikněte na **nákupu** toocreate hello clusteru k uzlu edge hello.

## <a name="access-an-edge-node"></a>Přístup uzlu edge
Hello hraniční uzel ssh koncový bod je &lt;EdgeNodeName >.&lt; Název clusteru >-ssh.azurehdinsight.net:22.  Například nové edgenode.myedgenode0914-ssh.azurehdinsight.net:22.

Hello hraniční uzel se objeví jako aplikace na hello portálu Azure.  poskytuje portálu Hello hello informace tooaccess hello okraj uzlu pomocí SSH.

**tooverify hello hraniční uzel koncový bod SSH**

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Otevřete hello HDInsight cluster s hraniční uzel.
3. Klikněte na tlačítko **aplikace** v okně clusteru hello. Zobrazí se hello hraniční uzel.  Hello výchozí název je **nové edgenode**.
4. Klikněte na tlačítko hello hraniční uzel. Zobrazí se koncový bod SSH hello.

**toouse Hive v uzlu edge hello**

1. Použití SSH tooconnect toohello hraniční uzel. Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Po připojení pomocí protokolu SSH toohello hraniční uzel, použijte následující příkaz tooopen hello Hive konzoly hello:
   
        hive
3. Spusťte následující příkaz tooshow tabulek Hive v clusteru hello hello:
   
        show tables;

## <a name="delete-an-edge-node"></a>Odstranit hraniční uzel
Hraniční uzel můžete odstranit z hello portálu Azure.

**tooaccess hraniční uzel**

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Otevřete hello HDInsight cluster s hraniční uzel.
3. Klikněte na tlačítko **aplikace** v okně clusteru hello. Zobrazí se seznam uzlů okraj.  
4. Klikněte pravým tlačítkem na hello hraniční uzel toodelete a pak klikněte na **odstranit**.
5. Klikněte na tlačítko **Ano** tooconfirm.

## <a name="next-steps"></a>Další kroky
V tomto článku jste se naučili jak tooadd hraniční uzel a jak tooaccess hello hraniční uzel. toolearn více, najdete v části hello následující články:

* [Instalace aplikací HDInsight](hdinsight-apps-install-applications.md): Zjistěte, jak tooinstall tooyour aplikace HDInsight clustery.
* [Instalace vlastních aplikací HDInsight](hdinsight-apps-install-custom-applications.md): Zjistěte, jak toodeploy publikování aplikace tooHDInsight HDInsight.
* [Publikování aplikací HDInsight](hdinsight-apps-publish-applications.md): Zjistěte, jak toopublish vaše vlastní tooAzure aplikace HDInsight Marketplace.
* [MSDN: Instalace aplikace HDInsight](https://msdn.microsoft.com/library/mt706515.aspx): Zjistěte, jak toodefine aplikace HDInsight.
* [Přizpůsobení clusterů HDInsight se systémem Linux pomocí akce skriptu](hdinsight-hadoop-customize-cluster-linux.md): Zjistěte, jak toouse akce skriptu tooinstall další aplikace.
* [Vytvořit clustery se systémem Linux Hadoop v HDInsight pomocí šablony Resource Manageru](hdinsight-hadoop-create-linux-clusters-arm-templates.md): Zjistěte, jak toocreate šablony Resource Manageru toocall HDInsight clustery.

