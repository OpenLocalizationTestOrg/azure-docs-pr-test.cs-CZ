---
title: "Přizpůsobení clusterů HDInsight pomocí akcí skriptů - Azure | Microsoft Docs"
description: "Přidáte vlastní komponenty ke clusterům HDInsight se systémem Linux pomocí akcí skriptů. Akce skriptů jsou skripty Bash, které lze použít k přizpůsobení konfigurace clusteru nebo přidání další služby a nástroje, jako je Hue, Solr nebo R."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 48e85f53-87c1-474f-b767-ca772238cc13
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: larryfr
ms.openlocfilehash: 0c5d00b6cb9f68a1a0e474f81c969eb1b5654c67
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="customize-linux-based-hdinsight-clusters-using-script-action"></a>Přizpůsobení clusterů HDInsight se systémem Linux pomocí akce skriptu

HDInsight nabízí možnost konfigurace názvem **akce skriptu** vlastní skripty, které přizpůsobují clusteru, který spustí. Tyto skripty se používají k instalaci dalších součástí a změně nastavení konfigurace. Akce skriptu lze během nebo po vytvoření clusteru.

> [!IMPORTANT]
> Možnost používat akcí skriptů v již spuštěného clusteru je pouze k dispozici pro clustery HDInsight se systémem Linux.
>
> HDInsight od verze 3.4 výše používá výhradně operační systém Linux. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

Akce skriptu lze také publikovat na webu Azure Marketplace jako aplikace HDInsight. Některé příklady v tomto dokumentu ukazují, jak můžete instalovat aplikace HDInsight pomocí skriptu akce příkazů z prostředí PowerShell a .NET SDK. Další informace o aplikace HDInsight naleznete v tématu [HDInsight publikování aplikace do Azure Marketplace](hdinsight-apps-publish-applications.md).

## <a name="permissions"></a>Oprávnění

Pokud používáte cluster HDInsight připojený k doméně, jsou k dispozici dva Ambari oprávnění, které jsou potřebné k pomocí akcí skriptů v clusteru:

* **AMBARI. Spustit\_vlastní\_příkaz**: role správce Ambari toto oprávnění má ve výchozím nastavení.
* **CLUSTER. Spustit\_vlastní\_příkaz**: obě Správce clusteru HDInsight a Ambari správce toto oprávnění mají ve výchozím nastavení.

Další informace o práci s oprávněními v doméně prostředí HDInsight najdete v tématu [Správa clusterů HDInsight připojený k doméně](hdinsight-domain-joined-manage.md).

## <a name="access-control"></a>Řízení přístupu

Pokud si nejste správce nebo vlastníka předplatného Azure, váš účet musí mít alespoň **Přispěvatel** přístup ke skupině prostředků, která obsahuje clusteru HDInsight.

Kromě toho při vytváření clusteru služby HDInsight, někdo s alespoň **Přispěvatel** přístup k předplatnému Azure musí již dříve zaregistrovali zprostředkovatele pro HDInsight. Registrace zprostředkovatele se provede, když uživatel s přístupem Přispěvatel poprvé vytvoří prostředek v rámci příslušného předplatného. Stejného výsledku lze i bez vytvoření prostředku dosáhnout [registrací poskytovatele prostřednictvím REST](https://msdn.microsoft.com/library/azure/dn790548.aspx).

Další informace o práci se správou přístupu najdete v následujících dokumentech:

* [Začínáme se správou přístupu na webu Azure Portal](../active-directory/role-based-access-control-what-is.md)
* [Použití přiřazení rolí ke správě přístupu k prostředkům předplatného Azure](../active-directory/role-based-access-control-configure.md)

## <a name="understanding-script-actions"></a>Vysvětlení akcí skriptů

Akce skriptu je jednoduše Bash skript, který zadáte identifikátorů URI a parametry. Skript se spustí na uzlech v clusteru HDInsight. Dále jsou vlastnosti a funkce akce skriptu.

* Musí být uložen v identifikátoru URI, který je přístupný z clusteru HDInsight. Toto jsou možné úložiště umístění:

    * **Azure Data Lake Store** účet, který je přístupný pro HDInsight cluster. Informace o používání Azure Data Lake Store s HDInsight naleznete v tématu [vytvoření clusteru HDInsight s Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).

        Při použití skriptu uložených v Data Lake Store, formát identifikátoru URI je `adl://DATALAKESTOREACCOUNTNAME.azuredatalakestore.net/path_to_file`.

        > [!NOTE]
        > Objekt služby, které HDInsight používá pro přístup k Data Lake Store, musí mít přístup pro čtení do skriptu.

    * Objekt blob v **účet úložiště Azure** který je buď primární, nebo další účet úložiště pro HDInsight cluster. HDInsight je udělen přístup k oběma typy účtů úložiště při vytváření clusteru.

    * Soubor veřejného sdílení služby objektů Blob v Azure, Githubu, OneDrive, Dropbox, např.

        Příklad najdete v části identifikátory URI, [příklad skript akce skripty](#example-script-action-scripts) části.

        > [!WARNING]
        > HDInsight podporuje pouze __pro obecné účely__ účty Azure Storage. Nepodporuje aktuálně __úložiště objektů Blob__ typ účtu.

* Může být omezené **spustili pouze určité typy uzlů**, příklad hlavních uzlech nebo uzlů pracovního procesu.

  > [!NOTE]
  > Při použití s HDInsight Premium, můžete zadat, aby skript by měl být použit hraničního uzlu.

* Může být **trvalé** nebo **ad hoc**.

    **Trvalé** skripty se použijí k pracovním uzlům přidat do clusteru po spuštění skriptu. Například při rozšiřování prostředků clusteru.

    Trvalá akce se skripty může také použít změny na jiný typ uzlu, jako je například hlavního uzlu.

  > [!IMPORTANT]
  > Trvalé akce se skripty musí mít jedinečný název.

    **Ad hoc** nejsou trvalé skripty. Uplatňují nejsou k pracovním uzlům přidat do clusteru po má skript spustili. Následně můžete zvýšit úroveň ad hoc skriptu do trvalého skriptu nebo snížení úrovně trvalého skriptu do ad hoc skriptu.

  > [!IMPORTANT]
  > Skript akce, které používají při vytváření clusteru jsou automaticky nastavené jako trvalé.
  >
  > Skripty, které nejsou selhání jako trvalé, i když konkrétně znamenat, že by měl být.

* Můžete přijmout **parametry** používané ve skriptu během provádění.
* Spustit s **kořenový úrovně oprávnění** na uzlech clusteru.
* Umožňuje použít **portál Azure**, **prostředí Azure PowerShell**, **rozhraní příkazového řádku Azure**, nebo **HDInsight .NET SDK**

Cluster uchovává historii všechny skripty, které mají byly spuštěny. Historie je užitečné, když potřebujete zjistit ID skript pro operace povýšení nebo degradování.

> [!IMPORTANT]
> Neexistuje žádný způsob automatického vrátit zpět změny provedené akce skriptu. Buď ručně změny nebo zadejte skript, který je obrátí.


### <a name="script-action-in-the-cluster-creation-process"></a>Akce skriptu v procesu vytváření clusteru

Skript akce, které používají při vytváření clusteru se mírně liší od skript, který u stávajícího clusteru byla spuštěna akce:

* Skript je **automaticky trvalou**.
* A **selhání** ve skriptu, může způsobit selhání procesu vytváření clusteru.

Následující diagram znázorňuje, když během procesu vytváření provedení akce skriptu:

![Přizpůsobení cluster HDInsight a fáze při vytváření clusteru][img-hdi-cluster-states]

Skript se spustí při HDInsight je konfigurován. V této fázi se skript spustí paralelně na všechny zadané uzly v clusteru a spustí s oprávněními kořenové na uzlech.

> [!NOTE]
> Protože skript se spustí s kořenové úrovně oprávnění na uzlech clusteru, můžete provádět operace, jako je spouštění a zastavování služeb, včetně služby související s Hadoop. Pokud zastavíte služby, je třeba zkontrolovat, že služba Ambari a další služby související s Hadoop jsou spuštěny před dokončení spuštění skriptu. Tyto služby jsou nezbytné pro úspěšně zjistit stav a stav clusteru při jeho vytvoření.


Při vytváření clusteru můžete použít různé akce skriptu najednou. Tyto skripty jsou spouštěny v pořadí, ve kterém byly zadány.

> [!IMPORTANT]
> Akce skriptu musí dokončit během 60 minut nebo vypršení časového limitu. Při zřizování clusteru, bude skript spuštěn souběžně jiné procesy instalací a konfigurací. Soutěž o zdroje, jako je například šířky pásma procesoru čas nebo v síti může způsobit skript trvá déle než ve vašem vývojovém prostředí dokončit.
>
> Chcete-li minimalizovat čas potřebný pro spuštění skriptu, vyhněte se úlohy, jako je stažení a kompilování aplikací ze zdroje. Předem zkompilovat aplikace a uložení binárního souboru ve službě Azure Storage.


### <a name="script-action-on-a-running-cluster"></a>Akce skriptu na spuštěného clusteru

Na rozdíl od skript, který byla spuštěna akce používají při vytváření clusteru, selhání ve skriptu na již spuštěnému clusteru automaticky nezpůsobí clusteru ke změně stavu selhání. Po dokončení skriptu clusteru by měl vrátit do stavu "spuštění".

> [!IMPORTANT]
> I v případě, že má cluster spuštěném stavu., mohou obsahovat nefunkční věcí skript, který selhal. Skript může například odstranit soubory, které cluster potřebuje.
>
> Akce skriptů spusťte s oprávněními kořenové, proto byste měli porozumět, skript se před každým jejím použitím v clusteru.

Při použití skript do clusteru, je stav clusteru změní z **systémem** k **platných**, pak **HDInsight konfigurace**a nakonec zpět do  **Spuštění** pro úspěšné skripty. Stav skriptu je zaznamenána v historii akcí skriptu a tyto informace můžete použít k určení, zda skript byla úspěšná nebo neúspěšná. Například `Get-AzureRmHDInsightScriptActionHistory` rutiny prostředí PowerShell můžete použít k zobrazení stavu skriptu. Vrací informace podobná následující text:

    ScriptExecutionId : 635918532516474303
    StartTime         : 8/14/2017 7:40:55 PM
    EndTime           : 8/14/2017 7:41:05 PM
    Status            : Succeeded

> [!NOTE]
> Pokud jste změnili heslo uživatele (správce) clusteru po vytvoření clusteru, skript, který spustil akce pro tento cluster mohou selhat. Pokud máte jakékoli trvalé akce se skripty této cílové uzly pracovního procesu, tyto skripty se pravděpodobně nezdaří při změně měřítka clusteru.

## <a name="example-script-action-scripts"></a>Příklad akce skriptu skripty

Skript akce skripty můžete použít následující nástroje:

* portál Azure
* Azure PowerShell
* Azure CLI
* Sada HDInsight .NET SDK

HDInsight poskytuje skripty, které v clusterech HDInsight nainstalovat následující součásti:

| Name (Název) | Skript |
| --- | --- |
| **Přidejte účet služby Azure Storage** |https://hdiconfigactions.BLOB.Core.Windows.NET/linuxaddstorageaccountv01/Add-Storage-Account-v01.SH. V tématu [přidejte další úložiště do clusteru HDInsight](hdinsight-hadoop-add-storage.md). |
| **Instalace aplikace Hue** |https://hdiconfigactions.BLOB.Core.Windows.NET/linuxhueconfigactionv02/Install-Hue-uber-v02.SH. V tématu [instalace a použití clusterů v HDInsight Hue](hdinsight-hadoop-hue-linux.md). |
| **Nainstalujte Presto** |https://RAW.githubusercontent.com/hdinsight/Presto-hdinsight/Master/installpresto.SH. V tématu [instalace a použití clusterů v HDInsight Presto](hdinsight-hadoop-install-presto.md). |
| **Nainstalujte Solr** |https://hdiconfigactions.BLOB.Core.Windows.NET/linuxsolrconfigactionv01/solr-Installer-v01.SH. V tématu [instalace a použití clusterů v HDInsight Solr](hdinsight-hadoop-solr-install-linux.md). |
| **Nainstalujte Giraph** |https://hdiconfigactions.BLOB.Core.Windows.NET/linuxgiraphconfigactionv01/giraph-Installer-v01.SH. V tématu [instalace a použití clusterů v HDInsight Giraph](hdinsight-hadoop-giraph-install-linux.md). |
| **Předběžné načtení knihovny Hive** |https://hdiconfigactions.BLOB.Core.Windows.NET/linuxsetupcustomhivelibsv01/Setup-customhivelibs-v01.SH. V tématu [přidat Hive knihovny v clusterech HDInsight](hdinsight-hadoop-add-hive-libraries.md). |
| **Instalace nebo aktualizace Mono** | https://hdiconfigactions.BLOB.Core.Windows.NET/Install-mono/Install-mono.bash. V tématu [instalace nebo aktualizace Mono v HDInsight](hdinsight-hadoop-install-mono.md). |

## <a name="use-a-script-action-during-cluster-creation"></a>Použití akce skriptu při vytváření clusteru

Tato část obsahuje příklady o různých způsobech skriptových akcí můžete při vytváření clusteru HDInsight.

### <a name="use-a-script-action-during-cluster-creation-from-the-azure-portal"></a>Použití akce skriptu při vytváření clusteru z portálu Azure

1. Zahájení vytváření clusteru, jak je popsáno v [vytvoření Hadoop clusterů v HDInsight](hdinsight-hadoop-provision-linux-clusters.md). Zastavit, když přejdete __clusteru Souhrn__ části.

2. Z __clusteru Souhrn__ vyberte __upravit__ propojení pro __upřesňující nastavení__.

    ![Upřesnit nastavení odkaz](./media/hdinsight-hadoop-customize-cluster-linux/advanced-settings-link.png)

3. Z __upřesňující nastavení__ vyberte __skript akce__. Z __skript akce__ vyberte __+ nové odeslání__

    ![Odeslat novou akci skriptu](./media/hdinsight-hadoop-customize-cluster-linux/add-script-action.png)

4. Použití __vyberte skript__ položka předem vytvořené skriptů. Chcete-li použít vlastní skript, vyberte __vlastní__ a pak zadejte __název__ a __Bash skriptu URI__ vašeho skriptu.

    ![Skript přidáte ve formuláři vyberte skript](./media/hdinsight-hadoop-customize-cluster-linux/select-script.png)

    Následující tabulka popisuje prvky na formuláři:

    | Vlastnost | Hodnota |
    | --- | --- |
    | Vyberte skript | Chcete-li použít vlastní skript, vyberte __vlastní__. Vyberte jednu z zadaný skripty, jinak hodnota. |
    | Name (Název) |Zadejte název akce skriptu. |
    | Skript bash identifikátor URI |Zadejte identifikátor URI skriptu, která je volána, chcete-li přizpůsobit clusteru. |
    | HEAD nebo Worker nebo Zookeeper |Zadejte uzly (**Head**, **pracovní**, nebo **ZooKeeper**) podle kterého se spouští skript přizpůsobení. |
    | Parametry |Zadejte parametry, pokud se vyžadují skriptem. |

    Použití __zachovat tuto akci skriptu__ položka zajistit, že skript se použije během operace škálování.

5. Vyberte __vytvořit__ Uložte skript. Pak můžete použít __+ odeslání nové__ přidat další skript.

    ![Více akcí skriptů](./media/hdinsight-hadoop-customize-cluster-linux/multiple-scripts.png)

    Po dokončení přidávání skriptů, použijte __vyberte__ tlačítko a potom __Další__ tlačítko se vraťte do __clusteru Souhrn__ části.

3. Pokud chcete vytvořit cluster, vyberte __vytvořit__ z __clusteru Souhrn__ výběr.

### <a name="use-a-script-action-from-azure-resource-manager-templates"></a>Použití akce skriptu z šablon Azure Resource Manageru

Příklady v této části ukazují, jak použít skript akce pomocí šablony Azure Resource Manager.

#### <a name="before-you-begin"></a>Než začnete

* Informace o konfiguraci pracovní stanice ke spouštění rutin prostředí HDInsight Powershell naleznete v tématu [nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azure/overview).
* Pokyny k vytvoření šablony najdete v tématu [šablon pro tvorbu Azure Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md).
* Pokud jste ještě nepoužívali prostředí Azure PowerShell s Resource Managerem, přečtěte si téma [použití Azure Powershellu s Azure Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md).

#### <a name="create-clusters-using-script-action"></a>Vytvořit clustery pomocí akce skriptu

1. Následující šablony zkopírujte do umístění v počítači. Tato šablona nainstaluje Giraph headnodes a pracovní uzly v clusteru. Můžete také ověřit, zda šablona JSON je platný. Vložit obsah do šablony [JSONLint](http://jsonlint.com/), online ověření nástroj JSON.

            {
            "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
            "contentVersion": "1.0.0.0",
            "parameters": {
                "clusterLocation": {
                    "type": "string",
                    "defaultValue": "West US",
                    "allowedValues": [ "West US" ]
                },
                "clusterName": {
                    "type": "string"
                },
                "clusterUserName": {
                    "type": "string",
                    "defaultValue": "admin"
                },
                "clusterUserPassword": {
                    "type": "securestring"
                },
                "sshUserName": {
                    "type": "string",
                    "defaultValue": "username"
                },
                "sshPassword": {
                    "type": "securestring"
                },
                "clusterStorageAccountName": {
                    "type": "string"
                },
                "clusterStorageAccountResourceGroup": {
                    "type": "string"
                },
                "clusterStorageType": {
                    "type": "string",
                    "defaultValue": "Standard_LRS",
                    "allowedValues": [
                        "Standard_LRS",
                        "Standard_GRS",
                        "Standard_ZRS"
                    ]
                },
                "clusterStorageAccountContainer": {
                    "type": "string"
                },
                "clusterHeadNodeCount": {
                    "type": "int",
                    "defaultValue": 1
                },
                "clusterWorkerNodeCount": {
                    "type": "int",
                    "defaultValue": 2
                }
            },
            "variables": {
            },
            "resources": [
                {
                    "name": "[parameters('clusterStorageAccountName')]",
                    "type": "Microsoft.Storage/storageAccounts",
                    "location": "[parameters('clusterLocation')]",
                    "apiVersion": "2015-05-01-preview",
                    "dependsOn": [ ],
                    "tags": { },
                    "properties": {
                        "accountType": "[parameters('clusterStorageType')]"
                    }
                },
                {
                    "name": "[parameters('clusterName')]",
                    "type": "Microsoft.HDInsight/clusters",
                    "location": "[parameters('clusterLocation')]",
                    "apiVersion": "2015-03-01-preview",
                    "dependsOn": [
                        "[concat('Microsoft.Storage/storageAccounts/', parameters('clusterStorageAccountName'))]"
                    ],
                    "tags": { },
                    "properties": {
                        "clusterVersion": "3.2",
                        "osType": "Linux",
                        "clusterDefinition": {
                            "kind": "hadoop",
                            "configurations": {
                                "gateway": {
                                    "restAuthCredential.isEnabled": true,
                                    "restAuthCredential.username": "[parameters('clusterUserName')]",
                                    "restAuthCredential.password": "[parameters('clusterUserPassword')]"
                                }
                            }
                        },
                        "storageProfile": {
                            "storageaccounts": [
                                {
                                    "name": "[concat(parameters('clusterStorageAccountName'),'.blob.core.windows.net')]",
                                    "isDefault": true,
                                    "container": "[parameters('clusterStorageAccountContainer')]",
                                    "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('clusterStorageAccountName')), '2015-05-01-preview').key1]"
                                }
                            ]
                        },
                        "computeProfile": {
                            "roles": [
                                {
                                    "name": "headnode",
                                    "targetInstanceCount": "[parameters('clusterHeadNodeCount')]",
                                    "hardwareProfile": {
                                        "vmSize": "Large"
                                    },
                                    "osProfile": {
                                        "linuxOperatingSystemProfile": {
                                            "username": "[parameters('sshUserName')]",
                                            "password": "[parameters('sshPassword')]"
                                        }
                                    },
                                    "scriptActions": [
                                        {
                                            "name": "installGiraph",
                                            "uri": "https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh",
                                            "parameters": ""
                                        }
                                    ]
                                },
                                {
                                    "name": "workernode",
                                    "targetInstanceCount": "[parameters('clusterWorkerNodeCount')]",
                                    "hardwareProfile": {
                                        "vmSize": "Large"
                                    },
                                    "osProfile": {
                                        "linuxOperatingSystemProfile": {
                                            "username": "[parameters('sshUserName')]",
                                            "password": "[parameters('sshPassword')]"
                                        }
                                    },
                                    "scriptActions": [
                                        {
                                            "name": "installR",
                                            "uri": "https://hdiconfigactions.blob.core.windows.net/linuxrconfigactionv01/r-installer-v01.sh",
                                            "parameters": ""
                                        }
                                    ]
                                }
                            ]
                        }
                    }
                }
            ],
            "outputs": {
                "cluster":{
                    "type" : "object",
                    "value" : "[reference(resourceId('Microsoft.HDInsight/clusters',parameters('clusterName')))]"
                }
            }
        }
2. Spuštění prostředí Azure PowerShell a přihlaste se k účtu Azure. Po zadání přihlašovacích údajů, příkaz vrátí informace o vašem účtu.

        Add-AzureRmAccount

        Id                             Type       ...
        --                             ----
        someone@example.com            User       ...
3. Pokud máte více předplatných, zadejte ID předplatného, které chcete použít pro nasazení.

        Select-AzureRmSubscription -SubscriptionID <YourSubscriptionId>

    > [!NOTE]
    > Můžete použít `Get-AzureRmSubscription` získat seznam Všechna předplatná spojená s vaším účtem, který obsahuje ID předplatného pro každé z nich.

4. Pokud nemáte existující skupinu prostředků, vytvořte skupinu prostředků. Zadejte název skupiny prostředků a umístění, které potřebujete pro vaše řešení. Se vrátí souhrn novou skupinu prostředků.

        New-AzureRmResourceGroup -Name myresourcegroup -Location "West US"

        ResourceGroupName : myresourcegroup
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
                            Actions  NotActions
                            =======  ==========
                            *
        ResourceId        : /subscriptions/######/resourceGroups/ExampleResourceGroup

5. Chcete-li vytvořit nasazení skupiny prostředků, spusťte **New-AzureRmResourceGroupDeployment** příkaz a zadejte potřebné parametry. Parametry patří následující data:

    * Název pro vaše nasazení
    * Název skupiny prostředků
    * Cesta nebo adresa URL šablonu, kterou jste vytvořili.

  Pokud vaše šablona vyžaduje žádné parametry, musíte zadat také tyto parametry. V takovém případě akce skriptu k instalaci R na clusteru nevyžaduje žádné parametry.

        New-AzureRmResourceGroupDeployment -Name mydeployment -ResourceGroupName myresourcegroup -TemplateFile <PathOrLinkToTemplate>

    Zobrazí se výzva k zadání hodnot pro parametry definované v šabloně.

1. Když nasazený skupinu prostředků, zobrazí se souhrn nasazení.

          DeploymentName    : mydeployment
          ResourceGroupName : myresourcegroup
          ProvisioningState : Succeeded
          Timestamp         : 8/14/2017 7:00:27 PM
          Mode              : Incremental
          ...

2. Pokud se nasazení nezdaří, můžete použít následující rutiny získejte informace o chyby.

        Get-AzureRmResourceGroupDeployment -ResourceGroupName myresourcegroup -ProvisioningState Failed

### <a name="use-a-script-action-during-cluster-creation-from-azure-powershell"></a>Použití akce skriptu při vytváření clusteru z prostředí Azure PowerShell

V této části používáme [přidat AzureRmHDInsightScriptAction](https://msdn.microsoft.com/library/mt603527.aspx) rutiny k vyvolání skripty pomocí akce skriptu k přizpůsobení clusteru. Než budete pokračovat, ujistěte se, jste nainstalovali a nakonfigurovali Azure PowerShell. Informace o konfiguraci pracovní stanice ke spouštění rutin prostředí HDInsight PowerShell naleznete v tématu [nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azure/overview).

Následující skript ukazuje, jak se má použít akci skriptu při vytváření clusteru pomocí prostředí PowerShell:

[!code-powershell[hlavní](../../powershell_scripts/hdinsight/use-script-action/use-script-action.ps1?range=5-90)]

To může trvat několik minut, než je vytvořen cluster.

### <a name="use-a-script-action-during-cluster-creation-from-the-hdinsight-net-sdk"></a>Použití akce skriptu při vytváření clusteru ze sady SDK rozhraní .NET HDInsight

.NET SDK služby HDInsight poskytuje klientské knihovny, které usnadňuje práci s HDInsight z aplikace .NET. Ukázka kódu, najdete v části [vytvořit systémem Linux clusterů v HDInsight pomocí sady .NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md#use-script-action).

## <a name="apply-a-script-action-to-a-running-cluster"></a>Použít akci skriptu do clusteru s podporou spuštěná

V této části zjistěte, jak lze aplikovat akce skriptu do clusteru s podporou spuštěné.

### <a name="apply-a-script-action-to-a-running-cluster-from-the-azure-portal"></a>Použít akci skriptu do clusteru s podporou spuštěné z portálu Azure

1. Z [portál Azure](https://portal.azure.com), vyberte clusteru HDInsight.

2. Přehled clusteru HDInsight, vyberte **akcí skriptů** dlaždici.

    ![Dlaždice akce skriptu](./media/hdinsight-hadoop-customize-cluster-linux/scriptactionstile.png)

   > [!NOTE]
   > Můžete také vybrat **všechna nastavení** a pak vyberte **akcí skriptů** v části nastavení.

3. Z horní části akcí skriptů, vyberte **odeslání nové**.

    ![Přidat skript do clusteru s podporou spuštěná](./media/hdinsight-hadoop-customize-cluster-linux/add-script-running-cluster.png)

4. Použití __vyberte skript__ položka předem vytvořené skriptů. Chcete-li použít vlastní skript, vyberte __vlastní__ a pak zadejte __název__ a __Bash skriptu URI__ vašeho skriptu.

    ![Skript přidáte ve formuláři vyberte skript](./media/hdinsight-hadoop-customize-cluster-linux/select-script.png)

    Následující tabulka popisuje prvky na formuláři:

    | Vlastnost | Hodnota |
    | --- | --- |
    | Vyberte skript | Chcete-li použít vlastní skript, vyberte __vlastní__. Jinak vyberte možnost poskytnutého skriptu. |
    | Name (Název) |Zadejte název akce skriptu. |
    | Skript bash identifikátor URI |Zadejte identifikátor URI skriptu, která je volána, chcete-li přizpůsobit clusteru. |
    | HEAD nebo Worker nebo Zookeeper |Zadejte uzly (**Head**, **pracovní**, nebo **ZooKeeper**) podle kterého se spouští skript přizpůsobení. |
    | Parametry |Zadejte parametry, pokud se vyžadují skriptem. |

    Použití __zachovat tuto akci skriptu__ položku a ujistěte se, skript se použije během operace škálování.

5. Nakonec použijte **vytvořit** tlačítko použít skript do clusteru.

### <a name="apply-a-script-action-to-a-running-cluster-from-azure-powershell"></a>Použít akci skriptu do clusteru s podporou spuštěné z prostředí Azure PowerShell

Než budete pokračovat, ujistěte se, jste nainstalovali a nakonfigurovali Azure PowerShell. Informace o konfiguraci pracovní stanice ke spouštění rutin prostředí HDInsight PowerShell naleznete v tématu [nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azure/overview).

Následující příklad ukazuje, jak se má použít akce skriptu do spuštěného clusteru:

[!code-powershell[hlavní](../../powershell_scripts/hdinsight/use-script-action/use-script-action.ps1?range=105-117)]

Po dokončení operace, zobrazí se informace podobná následující text:

    OperationState  : Succeeded
    ErrorMessage    :
    Name            : Giraph
    Uri             : https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh
    Parameters      :
    NodeTypes       : {HeadNode, WorkerNode}

### <a name="apply-a-script-action-to-a-running-cluster-from-the-azure-cli"></a>Použít akci skriptu do clusteru s podporou spuštěné z rozhraní příkazového řádku Azure

Než budete pokračovat, ujistěte se, je nainstalován a nakonfigurován rozhraní příkazového řádku Azure. Další informace najdete v tématu [nainstalovat Azure CLI](../cli-install-nodejs.md).

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

1. Chcete-li přepnout do režimu Azure Resource Manager, použijte následující příkaz na příkazovém řádku:

        azure config mode arm

2. Použijte následující k ověření vašeho předplatného Azure.

        azure login

3. Použijte následující příkaz pro použití akce skriptu do clusteru s podporou spuštěná

        azure hdinsight script-action create <clustername> -g <resourcegroupname> -n <scriptname> -u <scriptURI> -t <nodetypes>

    Pokud nezadáte parametry pro tento příkaz, zobrazí se výzva pro ně. Pokud skript určete s `-u` přijímá parametry, můžete je zadat pomocí `-p` parametr.

    Uzel platné typy jsou `headnode`, `workernode`, a `zookeeper`. Pokud skript má být použita na více typy uzlů, určit typy oddělených ';'. Například, `-n headnode;workernode`.

    Chcete-li zachovat skript, přidejte `--persistOnSuccess`. Vám může také zachovat skript později pomocí `azure hdinsight script-action persisted set`.

    Po dokončení úlohy, zobrazí se výstup podobný následujícímu:

        info:    Executing command hdinsight script-action create
        + Executing Script Action on HDInsight cluster
        data:    Operation Info
        data:    ---------------
        data:    Operation status:
        data:    Operation ID:  b707b10e-e633-45c0-baa9-8aed3d348c13
        info:    hdinsight script-action create command OK

### <a name="apply-a-script-action-to-a-running-cluster-using-rest-api"></a>Použít akci skriptu do clusteru s podporou spuštěná pomocí rozhraní REST API

V tématu [spuštění akcí skriptů v clusteru s podporou spuštěné](https://msdn.microsoft.com/library/azure/mt668441.aspx).

### <a name="apply-a-script-action-to-a-running-cluster-from-the-hdinsight-net-sdk"></a>Pro cluster běžící akce skriptu ze sady SDK rozhraní .NET HDInsight

Příklad použití sady .NET SDK skripty do clusteru, naleznete v části [https://github.com/Azure-Samples/hdinsight-dotnet-script-action](https://github.com/Azure-Samples/hdinsight-dotnet-script-action).

## <a name="view-history-promote-and-demote-script-actions"></a>Zobrazit historii, povýšení a snížení akcí skriptů

### <a name="using-the-azure-portal"></a>Použití webu Azure Portal

1. Z [portál Azure](https://portal.azure.com), vyberte clusteru HDInsight.

2. Přehled clusteru HDInsight, vyberte **akcí skriptů** dlaždici.

    ![Dlaždice akce skriptu](./media/hdinsight-hadoop-customize-cluster-linux/scriptactionstile.png)

   > [!NOTE]
   > Můžete také vybrat **všechna nastavení** a pak vyberte **akcí skriptů** v části nastavení.

4. Historie skripty pro tento cluster se zobrazí v části Akce skriptu. Tyto informace zahrnují seznam trvalou skripty. Na tomto snímku obrazovky uvidíte, že Solr byl skript spustili na tomto clusteru. Na snímku obrazovky nezobrazuje žádné trvalé skripty.

    ![Oddíl akce skriptu](./media/hdinsight-hadoop-customize-cluster-linux/script-action-history.png)

5. Výběr skript z historie zobrazí v části Vlastnosti pro tento skript. Z horní části obrazovky se můžete znovu spustit skript nebo povyšte ho.

    ![Vlastnosti akce skriptu](./media/hdinsight-hadoop-customize-cluster-linux/promote-script-actions.png)

6. Můžete také **...**  napravo od položky v oddílu akce skriptu k provádění akcí.

    ![Skript akce... využití](./media/hdinsight-hadoop-customize-cluster-linux/deletepromoted.png)

### <a name="using-azure-powershell"></a>Použití Azure Powershell

| Použijte následující... | K... |
| --- | --- |
| Get-AzureRmHDInsightPersistedScriptAction |Načíst informace o trvalé akce se skripty |
| Get-AzureRmHDInsightScriptActionHistory |Načíst historii akcí skriptu použít v clusteru nebo podrobnosti specifického skriptu |
| Set-AzureRmHDInsightPersistedScriptAction |Zvýší úroveň akci ad hoc skriptu pro akci trvalého skriptu |
| Remove-AzureRmHDInsightPersistedScriptAction |Sníží úroveň akcí trvalého skriptu na ad hoc akci |

> [!IMPORTANT]
> Pomocí `Remove-AzureRmHDInsightPersistedScriptAction` nevrátí zpět akci, kterou provádí skript. Tato rutina odebere jenom příznak trvalý.

Následující ukázkový skript ukazuje použití rutiny povýšit, pak snížení úrovně skriptu.

[!code-powershell[hlavní](../../powershell_scripts/hdinsight/use-script-action/use-script-action.ps1?range=123-140)]

### <a name="using-the-azure-cli"></a>Použití Azure CLI

| Použijte následující... | K... |
| --- | --- |
| `azure hdinsight script-action persisted list <clustername>` |Načtení seznamu akcí trvalého skriptu |
| `azure hdinsight script-action persisted show <clustername> <scriptname>` |Načíst informace o konkrétní trvalého skriptu akce |
| `azure hdinsight script-action history list <clustername>` |Načíst historii akcí skriptu použít do clusteru |
| `azure hdinsight script-action history show <clustername> <scriptname>` |Načíst informace o konkrétní skript akce |
| `azure hdinsight script action persisted set <clustername> <scriptexecutionid>` |Zvýší úroveň akci ad hoc skriptu pro akci trvalého skriptu |
| `azure hdinsight script-action persisted delete <clustername> <scriptname>` |Sníží úroveň akcí trvalého skriptu na ad hoc akci |

> [!IMPORTANT]
> Pomocí `azure hdinsight script-action persisted delete` nevrátí zpět akci, kterou provádí skript. Tato rutina odebere jenom příznak trvalý.

### <a name="using-the-hdinsight-net-sdk"></a>Pomocí HDInsight .NET SDK

Příklad použití sady .NET SDK k načtení historie skriptu z clusteru, zvýšení úrovně nebo snížení úrovně skriptů najdete v tématu [https://github.com/Azure-Samples/hdinsight-dotnet-script-action](https://github.com/Azure-Samples/hdinsight-dotnet-script-action).

> [!NOTE]
> Tento příklad také ukazuje, jak instalace aplikace HDInsight pomocí sady .NET SDK.

## <a name="support-for-open-source-software-used-on-hdinsight-clusters"></a>Podpora pro open-source softwaru použít na clustery HDInsight

Služba Microsoft Azure HDInsight používá prostředí technologie open source vytvořen kolem Hadoop. Microsoft Azure poskytuje obecné úroveň podpory pro technologie open source. Další informace najdete v tématu **podporu oboru** části [nejčastější dotazy týkající se podpory Azure web](https://azure.microsoft.com/support/faq/). Služba HDInsight poskytuje další úroveň podpory pro integrované komponenty.

Existují dva typy open-source komponent, které jsou k dispozici ve službě HDInsight:

* **Integrované komponenty** -tyto komponenty jsou předinstalované na clustery HDInsight a poskytují základní funkce služby clusteru. Například YARN ResourceManager, Hive dotazovací jazyk (HiveQL) a knihovně Mahout patří do této kategorie. Úplný seznam součástí clusteru je k dispozici v [co je nového ve verzích clusterů systému Hadoop poskytovaných v HDInsight](hdinsight-component-versioning.md).
* **Vlastní komponenty** -, jako uživatel clusteru, můžete nainstalovat nebo použít v vaše úlohy žádné součásti k dispozici v komunitě nebo vytvořené vámi.

> [!WARNING]
> Součásti, které jsou součástí clusteru HDInsight jsou plně podporovány. Microsoft Support pomáhá izolovat a vyřešení problémů týkajících se těchto součástí.
>
> Vlastní komponenty získat vyvineme podporu k pomoci při další řešení problému. Podporu společnosti Microsoft může být schopni vyřešit problém nebo mohou požádat, abyste zaujmout dostupné kanály pro technologie s otevřeným zdrojem, kterých se nachází hluboké znalosti pro tuto technologii. Například existuje mnoho komunity webů, které lze použít jako: [fórum MSDN pro HDInsight](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Také Apache projekty mají na projektu serverů [http://apache.org](http://apache.org), například: [Hadoop](http://hadoop.apache.org/).

Služba HDInsight poskytuje několik způsobů, jak používat vlastní komponenty. Stejnou úroveň podpory platí bez ohledu na to, jak je součást použít nebo nainstalované v clusteru. Následující seznam popisuje nejběžnější způsoby vlastní komponenty lze v clusterech HDInsight:

1. Úloha odeslání - Hadoop nebo jiné typy úloh, které spustit nebo používat vlastní komponenty lze odeslat do clusteru.

2. Přizpůsobení clusteru – při vytváření clusteru, můžete zadat další nastavení a vlastní součásti, které jsou nainstalovány na uzlech clusteru.

3. Ukázky - oblíbených vlastní součásti, Microsoft a ostatní mohou poskytnout ukázky použití těchto součástí v clusterech HDInsight. Tyto soubory jsou uvedeny bez podpory.

## <a name="troubleshooting"></a>Řešení potíží

Webovému uživatelskému rozhraní Ambari slouží k zobrazení informací zaznamenaných akcí skriptů. Pokud skript selže při vytváření clusteru, protokoly jsou také k dispozici v výchozí účet úložiště přidružený ke clusteru. Tato část obsahuje informace o tom, jak načíst protokolů pomocí obou těchto možností.

### <a name="using-the-ambari-web-ui"></a>Pomocí Ambari webového uživatelského rozhraní

1. V prohlížeči přejděte na https://CLUSTERNAME.azurehdinsight.net. Nahraďte název clusteru s názvem clusteru HDInsight.

    Po zobrazení výzvy zadejte název účtu správce (správce) a heslo pro cluster. Možná budete muset znovu zadat přihlašovací údaje správce v webového formuláře.

2. Z panelu v horní části stránky, vyberte **ops** položku. Zobrazí se seznam aktuální a předchozí operace provedené na clusteru prostřednictvím Ambari.

    ![Panel uživatelského rozhraní Ambari web s ops vybrané](./media/hdinsight-hadoop-customize-cluster-linux/ambari-nav.png)

3. Najděte položky, které mají **spustit\_customscriptaction** v **Operations** sloupce. Tyto položky se vytvoří při spuštění akce skriptu.

    ![Snímek obrazovky operací](./media/hdinsight-hadoop-customize-cluster-linux/ambariscriptaction.png)

    Chcete-li zobrazit výstup STDOUT a STDERR, vyberte položku run\customscriptaction a procházení odkazy. Tento výstup se vygeneruje, když bude skript spuštěn a můžou obsahovat užitečné informace.

### <a name="access-logs-from-the-default-storage-account"></a>Přístup k protokolům z výchozí účet úložiště

Pokud se vytvoření clusteru se nezdaří z důvodu chyby akce skriptu, protokoly jsou přístupné z účtu úložiště clusteru.

* Protokoly úložiště jsou dostupné v `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\CLUSTER_NAME\DATE`.

    ![Snímek obrazovky operací](./media/hdinsight-hadoop-customize-cluster-linux/script_action_logs_in_storage.png)

    V tomto adresáři protokoly jsou headnode, workernode a uzly zookeeper uspořádány samostatně. Tady je několik příkladů:

    * **Headnode** - `<uniqueidentifier>AmbariDb-hn0-<generated_value>.cloudapp.net`

    * **Pracovního uzlu** - `<uniqueidentifier>AmbariDb-wn0-<generated_value>.cloudapp.net`

    * **Zookeeper uzlu** - `<uniqueidentifier>AmbariDb-zk0-<generated_value>.cloudapp.net`

* Účet úložiště se nahraje všechny stdout a stderr odpovídající hostitele. Existuje **výstup -\*.txt** a **chyby -\*.txt** pro jednotlivé akce skriptu. *.Txt výstupní soubor obsahuje informace o identifikátoru URI skript, který nebyl spustit na hostiteli. Například

        'Start downloading script locally: ', u'https://hdiconfigactions.blob.core.windows.net/linuxrconfigactionv01/r-installer-v01.sh'

* Je možné, opakovaně vytvořit cluster akce skriptu se stejným názvem. V takovém případě můžete odlišit relevantní protokoly na základě názvu složky datum. Struktura složek pro cluster s podporou (mycluster) na různá data vytvořit například zobrazí podobná následující položky protokolu:

    `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\mycluster\2015-10-04` `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\mycluster\2015-10-05`

* Pokud vytvoříte cluster akce skriptu se stejným názvem ve stejný den, můžete k identifikaci příslušných protokolových souborů jedinečnou předponu.

* Pokud vytvoříte cluster s podporou téměř 12:00 AM (půlnoc), je možné, že soubory protokolu rozprostřít do dvou dnů. V takových případech uvidíte dvě složky na jiné datum pro stejného clusteru.

* Nahrávání souborů protokolu ke kontejneru výchozí může trvat až 5 minut, zejména u velkých clusterech. Ano Pokud chcete získat přístup v protokolech, nedoporučuje se mazat okamžitě clusteru neúspěšné provedení akce skriptu.

### <a name="ambari-watchdog"></a>Ambari sledovacího zařízení

> [!WARNING]
> Neměňte heslo pro Ambari sledovací zařízení (hdinsightwatchdog) v clusteru HDInsight se systémem Linux. Změna hesla pro tento účet dělí umožňuje spouštění nového akcí skriptů v clusteru HDInsight.

### <a name="cant-import-name-blobservice"></a>Nelze importovat název BlobService

__Příznaky__: selhání akce skriptu. Při operaci zobrazení Ambari, zobrazí se text podobná následující chybě:

```
Traceback (most recent call list):
  File "/var/lib/ambari-agent/cache/custom_actions/scripts/run_customscriptaction.py", line 21, in <module>
    from azure.storage.blob import BlobService
ImportError: cannot import name BlobService
```

__Příčina__: k této chybě dojde, pokud upgradujete klienta Python Azure Storage, který je součástí clusteru HDInsight. HDInsight očekává klienta Azure Storage 0.20.0.

__Řešení__: Chcete-li tuto chybu vyřešit, ručně připojit k každého uzlu clusteru pomocí `ssh` a znovu nainstalujte požadovanou verzi klienta správný úložiště pomocí následujícího příkazu:

```
sudo pip install azure-storage==0.20.0
```

Informace o připojení ke clusteru pomocí protokolu SSH naleznete v tématu [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

### <a name="history-doesnt-show-scripts-used-during-cluster-creation"></a>Historie nezobrazí skripty použité při vytváření clusteru

Pokud váš cluster byl vytvořen ještě před 15. března 2016, nemusíte vidět položku v historii akcí skriptu. Pokud změníte velikost clusteru po 15. března 2016, skripty, pomocí při vytváření clusteru zobrazí v historii, jako jsou nastavení použita na nové uzly v clusteru jako součást operace změny velikosti.

Existují dvě výjimky:

* Pokud je váš cluster vytvořený před 1. září 2015. Toto datum je při zavedených akce skriptu. Žádného clusteru vytvořené před tímto datem nelze použít akcí skriptů pro vytvoření clusteru.

* Pokud používají různé akce skriptu při vytváření clusteru a používá stejný název pro několik skriptů, nebo stejný název, stejným identifikátorem URI, ale různé parametry pro několik skriptů. V těchto případech dojít k následující chybě:

    Žádné nový skript, který může být akce byla spuštěna na tomto clusteru z důvodu konfliktu názvů skript v existující skripty. Vytvoření skriptu názvy uvedených v clusteru musí být všechny jedinečné. Existující skripty jsou spuštěné v změny velikosti.

## <a name="next-steps"></a>Další kroky

* [Vývoj skriptů akce skriptu pro HDInsight](hdinsight-hadoop-script-actions-linux.md)
* [Nainstalovat a používat Solr clustery prostředí HDInsight](hdinsight-hadoop-solr-install-linux.md)
* [Nainstalovat a používat Giraph clustery prostředí HDInsight](hdinsight-hadoop-giraph-install-linux.md)
* [Přidejte další úložiště do clusteru HDInsight](hdinsight-hadoop-add-storage.md)

[img-hdi-cluster-states]: ./media/hdinsight-hadoop-customize-cluster-linux/HDI-Cluster-state.png "Fáze při vytváření clusteru"
