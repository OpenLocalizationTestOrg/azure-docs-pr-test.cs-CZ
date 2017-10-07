---
title: "pomocí akcí skriptů - Azure clusterů HDInsight aaaCustomize | Microsoft Docs"
description: "Přidáte volitelné součásti, které na základě tooLinux HDInsight clustery pomocí akcí skriptů. Akce skriptů jsou skripty Bash, které lze použít toocustomize konfigurace clusteru hello nebo přidejte další služby a nástroje, jako je Hue, Solr nebo R."
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
ms.openlocfilehash: ff22680a8a50b21985f6941f1edaf1dcf863d13f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="customize-linux-based-hdinsight-clusters-using-script-action"></a>Přizpůsobení clusterů HDInsight se systémem Linux pomocí akce skriptu

HDInsight nabízí možnost konfigurace názvem **akce skriptu** vlastní skripty, které přizpůsobují hello clusteru, který spustí. Tyto skripty jsou použité tooinstall další součásti a změňte nastavení konfigurace. Akce skriptu lze během nebo po vytvoření clusteru.

> [!IMPORTANT]
> Hello možnost toouse akcí skriptů v již spuštěného clusteru je dostupná pouze pro clustery HDInsight se systémem Linux.
>
> Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

Skript akce, může být také publikované toohello Azure Marketplace jako aplikace HDInsight. Některé příklady hello v tomto dokumentu ukazují, jak můžete instalovat aplikace HDInsight pomocí skriptu akce příkazů z prostředí PowerShell a hello .NET SDK. Další informace o aplikace HDInsight naleznete v tématu [HDInsight publikování aplikace do Azure Marketplace hello](hdinsight-apps-publish-applications.md).

## <a name="permissions"></a>Oprávnění

Pokud používáte cluster HDInsight připojený k doméně, jsou k dispozici dva Ambari oprávnění, které jsou požadovány při použití akce skriptu s clusterem hello:

* **AMBARI. Spustit\_vlastní\_příkaz**: role správce Ambari hello toto oprávnění má ve výchozím nastavení.
* **CLUSTER. Spustit\_vlastní\_příkaz**: obě hello Správce clusteru HDInsight a Ambari správce toto oprávnění mají ve výchozím nastavení.

Další informace o práci s oprávněními v doméně prostředí HDInsight najdete v tématu [Správa clusterů HDInsight připojený k doméně](hdinsight-domain-joined-manage.md).

## <a name="access-control"></a>Řízení přístupu

Pokud si nejste hello správce nebo vlastníka předplatného Azure, váš účet musí mít alespoň **Přispěvatel** skupinu prostředků toohello přístupu, která obsahuje clusteru HDInsight hello.

Kromě toho při vytváření clusteru služby HDInsight, někdo s alespoň **Přispěvatel** přístup toohello předplatné musí již dříve zaregistrovali hello zprostředkovatele pro HDInsight. Registrace zprostředkovatele se stane, když uživatel s předplatným toohello přístup Přispěvatel vytvoří prostředek pro hello poprvé v předplatném hello. Stejného výsledku lze i bez vytvoření prostředku dosáhnout [registrací poskytovatele prostřednictvím REST](https://msdn.microsoft.com/library/azure/dn790548.aspx).

Další informace o práci s správu přístupu najdete v tématu hello následující dokumenty:

* [Začínáme se správou přístupu v hello portálu Azure](../active-directory/role-based-access-control-what-is.md)
* [Používat roli přiřazení toomanage přístup tooyour předplatného Azure prostředky](../active-directory/role-based-access-control-configure.md)

## <a name="understanding-script-actions"></a>Vysvětlení akcí skriptů

Akce skriptu je jednoduše Bash skript, který zadáte identifikátorů URI a parametry. Hello skript se spustí na uzlech v clusteru HDInsight hello. Hello následují vlastnosti a funkce akce skriptu.

* Musí být uložen v identifikátoru URI, který je přístupný z clusteru HDInsight hello. Hello následují umístění možné úložiště:

    * **Azure Data Lake Store** účet, který je přístupný hello clusteru HDInsight. Informace o používání Azure Data Lake Store s HDInsight naleznete v tématu [vytvoření clusteru HDInsight s Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).

        Při použití skriptu uložených v Data Lake Store, formát identifikátoru URI hello je `adl://DATALAKESTOREACCOUNTNAME.azuredatalakestore.net/path_to_file`.

        > [!NOTE]
        > Hello služby hlavní HDInsight používá tooaccess Data Lake Store, musí mít přístup pro čtení toohello skriptu.

    * Objekt blob v **účet úložiště Azure** který je buď účet hello primární nebo další úložiště pro hello HDInsight cluster. HDInsight se udělí přístup tooboth tyto typy účtů úložiště při vytváření clusteru.

    * Soubor veřejného sdílení služby objektů Blob v Azure, Githubu, OneDrive, Dropbox, např.

        Například identifikátory URI, najdete v části hello [příklad skript akce skripty](#example-script-action-scripts) části.

        > [!WARNING]
        > HDInsight podporuje pouze __pro obecné účely__ účty Azure Storage. Aktuálně nepodporuje hello __úložiště objektů Blob__ typ účtu.

* Je možné omezit příliš**spustili pouze určité typy uzlů**, příklad hlavních uzlech nebo uzlů pracovního procesu.

  > [!NOTE]
  > Při použití s HDInsight Premium, můžete zadat, že hello skriptu by měl používat na hello hraniční uzel.

* Může být **trvalé** nebo **ad hoc**.

    **Trvalé** skriptů jsou použité tooworker uzly přidané toohello clusteru po spuštění skriptu hello. Například při rozšiřování prostředků clusteru hello.

    Typ uzlu tooanother změny, jako je například hlavního uzlu může platit taky trvalého skriptu.

  > [!IMPORTANT]
  > Trvalé akce se skripty musí mít jedinečný název.

    **Ad hoc** nejsou trvalé skripty. Nejsou se použité tooworker uzly přidané toohello clusteru po má spustili hello skriptu. Následně můžete povýšit ad hoc skriptů tooa jako trvalý, skript nebo snížení úrovně skript ad hoc tooan trvalého skriptu.

  > [!IMPORTANT]
  > Skript akce, které používají při vytváření clusteru jsou automaticky nastavené jako trvalé.
  >
  > Skripty, které nejsou selhání jako trvalé, i když konkrétně znamenat, že by měl být.

* Můžete přijmout **parametry** používané skriptem hello během provádění.
* Spustit s **kořenový úrovně oprávnění** na uzlech clusteru hello.
* Je možné prostřednictvím hello **portál Azure**, **prostředí Azure PowerShell**, **rozhraní příkazového řádku Azure**, nebo **HDInsight .NET SDK**

Hello clusteru uchovává historii všechny skripty, které mají byly spuštěny. Historie Hello je užitečné, když potřebujete toofind hello ID skriptu pro operace povýšení nebo degradování.

> [!IMPORTANT]
> Neexistuje žádný způsob, jak automatické tooundo hello změny provedené akce skriptu. Buď ručně reverse hello změny nebo zadejte skript, který je obrátí.


### <a name="script-action-in-hello-cluster-creation-process"></a>Akce skriptu v procesu vytváření clusteru hello

Skript akce, které používají při vytváření clusteru se mírně liší od skript, který u stávajícího clusteru byla spuštěna akce:

* je skript Hello **automaticky trvalou**.
* A **selhání** v hello skript může způsobit toofail procesu vytváření clusteru hello.

Hello následující diagram znázorňuje, pokud je během procesu vytváření hello provést akce skriptu:

![Přizpůsobení cluster HDInsight a fáze při vytváření clusteru][img-hdi-cluster-states]

Hello skript se spustí při HDInsight je konfigurován. V této fázi hello hello skript se spustí paralelně na všechny uzly zadané v hello clusteru a spustí s oprávněními kořenové na uzlech hello.

> [!NOTE]
> Protože hello skript se spustí s kořenové úrovně oprávnění na uzlech clusteru hello, můžete provádět operace, jako je spouštění a zastavování služeb, včetně služby související s Hadoop. Pokud zastavíte služby, je třeba zkontrolovat, že služba hello Ambari a další související s Hadoop služby jsou spuštěny před hello skriptu dokončení spuštění. Tyto služby jsou potřeba toosuccessfully určit hello stav a stav hello clusteru při jeho vytvoření.


Při vytváření clusteru můžete použít různé akce skriptu najednou. Tyto skripty jsou vyvolány v hello pořadí, ve kterém byly zadány.

> [!IMPORTANT]
> Akce skriptu musí dokončit během 60 minut nebo vypršení časového limitu. Při zřizování clusteru hello skript spustí souběžně jiné procesy instalací a konfigurací. Soutěž o zdroje, jako je například šířky pásma procesoru čas nebo v síti může způsobit hello skriptu tootake delší toofinish než ve vašem vývojovém prostředí.
>
> toominimize hello trvá toorun hello skriptu, když, vyhněte se úlohy, jako je stažení a kompilování aplikací ze zdroje. Předem zkompilovat aplikací a ukládat hello binární ve službě Azure Storage.


### <a name="script-action-on-a-running-cluster"></a>Akce skriptu na spuštěného clusteru

Na rozdíl od skript, který byla spuštěna akce používají při vytváření clusteru, selhání ve skriptu na již spuštěnému clusteru automaticky nezpůsobí stav hello clusteru toochange tooa se nezdařilo. Po dokončení skriptu hello clusteru by měl vrátit tooa "spuštěný".

> [!IMPORTANT]
> I v případě, že má hello cluster spuštěném stavu., hello selhání skriptu mohou obsahovat nefunkční věcí. Skript může například odstranit soubory potřebné hello cluster.
>
> Skripty akce spustit s oprávněními kořenové, ujistěte se, že chápete, skript se před každým jejím použitím tooyour clusteru.

Při použití skriptu tooa clusteru, změní stav clusteru hello toofrom **systémem** příliš**platných**, pak **HDInsight konfigurace**a nakonec zpět příliš**Systémem** pro úspěšné skripty. Stav skriptu Hello je zaznamenána v historii akcí skriptu hello a pomocí této toodetermine informace, zda skript hello byla úspěšná nebo neúspěšná. Například hello `Get-AzureRmHDInsightScriptActionHistory` rutiny prostředí PowerShell může být stav hello použité tooview skriptu. Vrátí informace podobné toohello následující text:

    ScriptExecutionId : 635918532516474303
    StartTime         : 8/14/2017 7:40:55 PM
    EndTime           : 8/14/2017 7:41:05 PM
    Status            : Succeeded

> [!NOTE]
> Pokud jste změnili heslo uživatele (správce) hello clusteru po vytvoření clusteru hello, skript, který spustil akce pro tento cluster mohou selhat. Pokud máte jakékoli trvalé akce se skripty této cílové uzly pracovního procesu, tyto skripty se pravděpodobně nezdaří při změně měřítka hello clusteru.

## <a name="example-script-action-scripts"></a>Příklad akce skriptu skripty

Skript akce skripty můžete použít hello následující nástroje:

* portál Azure
* Azure PowerShell
* Azure CLI
* Sada HDInsight .NET SDK

HDInsight poskytuje hello tooinstall skripty v clusterech HDInsight následující součásti:

| Name (Název) | Skript |
| --- | --- |
| **Přidejte účet služby Azure Storage** |https://hdiconfigactions.BLOB.Core.Windows.NET/linuxaddstorageaccountv01/Add-Storage-Account-v01.SH. V tématu [clusteru HDInsight tooan přidat další úložiště](hdinsight-hadoop-add-storage.md). |
| **Instalace aplikace Hue** |https://hdiconfigactions.BLOB.Core.Windows.NET/linuxhueconfigactionv02/Install-Hue-uber-v02.SH. V tématu [instalace a použití clusterů v HDInsight Hue](hdinsight-hadoop-hue-linux.md). |
| **Nainstalujte Presto** |https://RAW.githubusercontent.com/hdinsight/Presto-hdinsight/Master/installpresto.SH. V tématu [instalace a použití clusterů v HDInsight Presto](hdinsight-hadoop-install-presto.md). |
| **Nainstalujte Solr** |https://hdiconfigactions.BLOB.Core.Windows.NET/linuxsolrconfigactionv01/solr-Installer-v01.SH. V tématu [instalace a použití clusterů v HDInsight Solr](hdinsight-hadoop-solr-install-linux.md). |
| **Nainstalujte Giraph** |https://hdiconfigactions.BLOB.Core.Windows.NET/linuxgiraphconfigactionv01/giraph-Installer-v01.SH. V tématu [instalace a použití clusterů v HDInsight Giraph](hdinsight-hadoop-giraph-install-linux.md). |
| **Předběžné načtení knihovny Hive** |https://hdiconfigactions.BLOB.Core.Windows.NET/linuxsetupcustomhivelibsv01/Setup-customhivelibs-v01.SH. V tématu [přidat Hive knihovny v clusterech HDInsight](hdinsight-hadoop-add-hive-libraries.md). |
| **Instalace nebo aktualizace Mono** | https://hdiconfigactions.BLOB.Core.Windows.NET/Install-mono/Install-mono.bash. V tématu [instalace nebo aktualizace Mono v HDInsight](hdinsight-hadoop-install-mono.md). |

## <a name="use-a-script-action-during-cluster-creation"></a>Použití akce skriptu při vytváření clusteru

Tato část obsahuje příklady o různých způsobech hello skriptových akcí můžete při vytváření clusteru HDInsight.

### <a name="use-a-script-action-during-cluster-creation-from-hello-azure-portal"></a>Použití akce skriptu při vytváření clusteru z hello portálu Azure

1. Zahájení vytváření clusteru, jak je popsáno v [vytvoření Hadoop clusterů v HDInsight](hdinsight-hadoop-provision-linux-clusters.md). Zastavit při dosažení hello __clusteru Souhrn__ části.

2. Z hello __clusteru Souhrn__ části, vyberte hello __upravit__ propojení pro __upřesňující nastavení__.

    ![Upřesnit nastavení odkaz](./media/hdinsight-hadoop-customize-cluster-linux/advanced-settings-link.png)

3. Z hello __upřesňující nastavení__ vyberte __skript akce__. Z hello __skript akce__ vyberte __+ nové odeslání__

    ![Odeslat novou akci skriptu](./media/hdinsight-hadoop-customize-cluster-linux/add-script-action.png)

4. Použití hello __vyberte skript__ položka tooselect předem vytvořené skriptu. Vyberte toouse vlastní skript, __vlastní__ a pak zadejte hello __název__ a __Bash skriptu URI__ vašeho skriptu.

    ![Skript přidáte tak v podobě vyberte skript hello](./media/hdinsight-hadoop-customize-cluster-linux/select-script.png)

    Hello následující tabulka popisuje elementy hello ve formuláři hello:

    | Vlastnost | Hodnota |
    | --- | --- |
    | Vyberte skript | toouse vlastního skriptu, vyberte __vlastní__. Vyberte jednu z hello k dispozici, jinak hodnota skripty. |
    | Name (Název) |Zadejte název akce skriptu hello. |
    | Skript bash identifikátor URI |Zadejte hello URI toohello skript, který je vyvolaná toocustomize hello clusteru. |
    | HEAD nebo Worker nebo Zookeeper |Zadejte hello uzly (**Head**, **pracovní**, nebo **ZooKeeper**) na úpravy hello je skript spuštěn. |
    | Parametry |Zadejte parametry hello, pokud to vyžaduje hello skriptu. |

    Použití hello __zachovat tuto akci skriptu__ tooensure položku, která hello skriptu se použije během operace škálování.

5. Vyberte __vytvořit__ toosave hello skriptu. Pak můžete použít __+ odeslání nové__ tooadd další skript.

    ![Více akcí skriptů](./media/hdinsight-hadoop-customize-cluster-linux/multiple-scripts.png)

    Po dokončení přidávání skriptů, použijte hello __vyberte__ tlačítko a pak hello __Další__ tlačítko tooreturn toohello __clusteru Souhrn__ části.

3. toocreate hello cluster, vyberte __vytvořit__ z hello __clusteru Souhrn__ výběr.

### <a name="use-a-script-action-from-azure-resource-manager-templates"></a>Použití akce skriptu z šablon Azure Resource Manageru

Příklady Hello v této části ukazují, jak toouse skript akce pomocí šablony Azure Resource Manager.

#### <a name="before-you-begin"></a>Než začnete

* Informace o konfiguraci pracovní stanice toorun rutiny prostředí HDInsight Powershell naleznete v tématu [nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azure/overview).
* Pokyny najdete v části toocreate šablony [šablon pro tvorbu Azure Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md).
* Pokud jste ještě nepoužívali prostředí Azure PowerShell s Resource Managerem, přečtěte si téma [použití Azure Powershellu s Azure Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md).

#### <a name="create-clusters-using-script-action"></a>Vytvořit clustery pomocí akce skriptu

1. Zkopírujte hello následující šablony tooa umístění ve vašem počítači. Tato šablona nainstaluje Giraph hello headnodes a pracovní uzly v clusteru hello. Můžete také ověřit, zda šablona hello JSON je platný. Vložit obsah do šablony [JSONLint](http://jsonlint.com/), online ověření nástroj JSON.

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
2. Otevřete prostředí Azure PowerShell a přihlaste se tooyour účet Azure. Po zadání přihlašovacích údajů, hello příkaz vrátí informace o vašem účtu.

        Add-AzureRmAccount

        Id                             Type       ...
        --                             ----
        someone@example.com            User       ...
3. Pokud máte více předplatných, zadejte ID předplatného hello chcete toouse pro nasazení.

        Select-AzureRmSubscription -SubscriptionID <YourSubscriptionId>

    > [!NOTE]
    > Můžete použít `Get-AzureRmSubscription` tooget seznam Všechna předplatná spojená s vaším účtem, který zahrnuje hello ID odběru pro každé z nich.

4. Pokud nemáte existující skupinu prostředků, vytvořte skupinu prostředků. Zadejte název hello hello skupinu prostředků a umístění, které potřebujete pro vaše řešení. Se vrátí souhrn hello novou skupinu prostředků.

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

5. toocreate nasazení skupiny prostředků, spusťte hello **New-AzureRmResourceGroupDeployment** příkaz a zadejte potřebné parametry hello. Parametry Hello patří hello následující data:

    * Název pro vaše nasazení
    * Hello název vaší skupiny prostředků
    * Hello cesta nebo adresa URL toohello šablonu, kterou jste vytvořili.

  Pokud vaše šablona vyžaduje žádné parametry, musíte zadat také tyto parametry. V takovém případě hello skript akce tooinstall R na clusteru hello nevyžaduje žádné parametry.

        New-AzureRmResourceGroupDeployment -Name mydeployment -ResourceGroupName myresourcegroup -TemplateFile <PathOrLinkToTemplate>

    Jste výzvami tooprovide hodnoty pro parametry hello definované v šabloně hello.

1. Pokud skupina prostředků hello nasazen, se zobrazí shrnutí nasazení hello.

          DeploymentName    : mydeployment
          ResourceGroupName : myresourcegroup
          ProvisioningState : Succeeded
          Timestamp         : 8/14/2017 7:00:27 PM
          Mode              : Incremental
          ...

2. Pokud vaše nasazení selže, můžete použít následující rutiny tooget informace o selhání hello hello.

        Get-AzureRmResourceGroupDeployment -ResourceGroupName myresourcegroup -ProvisioningState Failed

### <a name="use-a-script-action-during-cluster-creation-from-azure-powershell"></a>Použití akce skriptu při vytváření clusteru z prostředí Azure PowerShell

V této části používáme hello [přidat AzureRmHDInsightScriptAction](https://msdn.microsoft.com/library/mt603527.aspx) rutiny tooinvoke skripty pomocí akce skriptu toocustomize cluster. Než budete pokračovat, ujistěte se, jste nainstalovali a nakonfigurovali Azure PowerShell. Informace o konfiguraci pracovní stanice toorun rutiny prostředí HDInsight PowerShell naleznete v tématu [nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azure/overview).

ukazuje, Hello následující skript jak tooapply akce skriptu při vytváření clusteru pomocí prostředí PowerShell:

[!code-powershell[main](../../powershell_scripts/hdinsight/use-script-action/use-script-action.ps1?range=5-90)]

Může trvat několik minut, než se vytvoří hello clusteru.

### <a name="use-a-script-action-during-cluster-creation-from-hello-hdinsight-net-sdk"></a>Použití akce skriptu při vytváření clusteru z hello HDInsight .NET SDK

Hello HDInsight .NET SDK poskytuje klientské knihovny, které umožňuje snazší toowork s HDInsight z aplikace .NET. Ukázka kódu, najdete v části [hello vytvořit systémem Linux clusterů v HDInsight pomocí sady .NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md#use-script-action).

## <a name="apply-a-script-action-tooa-running-cluster"></a>Použití akce skriptu tooa, spuštění clusteru

V této části se dozvíte, jak tooapply skript akce tooa spuštění clusteru.

### <a name="apply-a-script-action-tooa-running-cluster-from-hello-azure-portal"></a>Použití akce skriptu tooa, spuštění clusteru z hello portálu Azure

1. Z hello [portál Azure](https://portal.azure.com), vyberte clusteru HDInsight.

2. Přehled clusteru HDInsight hello, vyberte hello **akcí skriptů** dlaždici.

    ![Dlaždice akce skriptu](./media/hdinsight-hadoop-customize-cluster-linux/scriptactionstile.png)

   > [!NOTE]
   > Můžete také vybrat **všechna nastavení** a pak vyberte **akcí skriptů** z hello v oddílu nastavení.

3. Hello horní části hello oddílu akce skriptu, vyberte **odeslání nové**.

    ![Přidat skript tooa, spuštění clusteru](./media/hdinsight-hadoop-customize-cluster-linux/add-script-running-cluster.png)

4. Použití hello __vyberte skript__ položka tooselect předem vytvořené skriptu. Vyberte toouse vlastní skript, __vlastní__ a pak zadejte hello __název__ a __Bash skriptu URI__ vašeho skriptu.

    ![Skript přidáte tak v podobě vyberte skript hello](./media/hdinsight-hadoop-customize-cluster-linux/select-script.png)

    Hello následující tabulka popisuje elementy hello ve formuláři hello:

    | Vlastnost | Hodnota |
    | --- | --- |
    | Vyberte skript | toouse vlastního skriptu, vyberte __vlastní__. Jinak vyberte možnost poskytnutého skriptu. |
    | Name (Název) |Zadejte název akce skriptu hello. |
    | Skript bash identifikátor URI |Zadejte hello URI toohello skript, který je vyvolaná toocustomize hello clusteru. |
    | HEAD nebo Worker nebo Zookeeper |Zadejte hello uzly (**Head**, **pracovní**, nebo **ZooKeeper**) na úpravy hello je skript spuštěn. |
    | Parametry |Zadejte parametry hello, pokud to vyžaduje hello skriptu. |

    Použití hello __zachovat tuto akci skriptu__ položka toomake zda hello skriptu se použije během operace škálování.

5. Nakonec použijte hello **vytvořit** tlačítko tooapply hello skriptu toohello clusteru.

### <a name="apply-a-script-action-tooa-running-cluster-from-azure-powershell"></a>Použití akce skriptu tooa, spuštění clusteru z prostředí Azure PowerShell

Než budete pokračovat, ujistěte se, jste nainstalovali a nakonfigurovali Azure PowerShell. Informace o konfiguraci pracovní stanice toorun rutiny prostředí HDInsight PowerShell naleznete v tématu [nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azure/overview).

Hello následující příklad ukazuje, jak tooapply clusteru spuštěná tooa akce skriptu:

[!code-powershell[main](../../powershell_scripts/hdinsight/use-script-action/use-script-action.ps1?range=105-117)]

Po dokončení operace hello, zobrazí se informace podobné toohello následující text:

    OperationState  : Succeeded
    ErrorMessage    :
    Name            : Giraph
    Uri             : https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh
    Parameters      :
    NodeTypes       : {HeadNode, WorkerNode}

### <a name="apply-a-script-action-tooa-running-cluster-from-hello-azure-cli"></a>Použití akce skriptu tooa, spuštění clusteru z hello rozhraní příkazového řádku Azure

Než budete pokračovat, ujistěte se, je nainstalován a nakonfigurován hello rozhraní příkazového řádku Azure. Další informace najdete v tématu [hello instalace rozhraní příkazového řádku Azure](../cli-install-nodejs.md).

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

1. tooswitch tooAzure režimu Resource Manager, použijte následující příkaz v příkazovém řádku hello hello:

        azure config mode arm

2. Použijte hello následující tooauthenticate tooyour předplatného Azure.

        azure login

3. Použijte následující příkaz tooapply tooa akce skriptu spuštění clusteru hello

        azure hdinsight script-action create <clustername> -g <resourcegroupname> -n <scriptname> -u <scriptURI> -t <nodetypes>

    Pokud nezadáte parametry pro tento příkaz, zobrazí se výzva pro ně. Pokud hello skriptu zadejte s `-u` přijímá parametry, můžete je pomocí hello `-p` parametr.

    Uzel platné typy jsou `headnode`, `workernode`, a `zookeeper`. Pokud skript hello by měla být použitá toomultiple typy uzlů, určit typy hello oddělených ';'. Například, `-n headnode;workernode`.

    toopersist hello skript, přidejte hello `--persistOnSuccess`. Vám může také zachovat skriptu hello později pomocí `azure hdinsight script-action persisted set`.

    Po dokončení úlohy hello, zobrazí se výstup podobný toohello následující text:

        info:    Executing command hdinsight script-action create
        + Executing Script Action on HDInsight cluster
        data:    Operation Info
        data:    ---------------
        data:    Operation status:
        data:    Operation ID:  b707b10e-e633-45c0-baa9-8aed3d348c13
        info:    hdinsight script-action create command OK

### <a name="apply-a-script-action-tooa-running-cluster-using-rest-api"></a>Použití akce skriptu tooa, spuštění clusteru pomocí rozhraní REST API

V tématu [spuštění akcí skriptů v clusteru s podporou spuštěné](https://msdn.microsoft.com/library/azure/mt668441.aspx).

### <a name="apply-a-script-action-tooa-running-cluster-from-hello-hdinsight-net-sdk"></a>Použití akce skriptu tooa, spuštění clusteru z hello HDInsight .NET SDK

Příklad použití hello .NET SDK tooapply skripty tooa clusteru, naleznete v části [https://github.com/Azure-Samples/hdinsight-dotnet-script-action](https://github.com/Azure-Samples/hdinsight-dotnet-script-action).

## <a name="view-history-promote-and-demote-script-actions"></a>Zobrazit historii, povýšení a snížení akcí skriptů

### <a name="using-hello-azure-portal"></a>Pomocí hello portálu Azure

1. Z hello [portál Azure](https://portal.azure.com), vyberte clusteru HDInsight.

2. Přehled clusteru HDInsight hello, vyberte hello **akcí skriptů** dlaždici.

    ![Dlaždice akce skriptu](./media/hdinsight-hadoop-customize-cluster-linux/scriptactionstile.png)

   > [!NOTE]
   > Můžete také vybrat **všechna nastavení** a pak vyberte **akcí skriptů** z hello v oddílu nastavení.

4. Historie skripty pro tento cluster se zobrazí na hello oddílu akce skriptu. Tyto informace zahrnují seznam trvalou skripty. V hello následující snímek obrazovky uvidíte, že hello Solr byl skript spustili na tomto clusteru. snímek obrazovky Hello nezobrazuje žádné trvalé skripty.

    ![Oddíl akce skriptu](./media/hdinsight-hadoop-customize-cluster-linux/script-action-history.png)

5. Výběr skript z historie hello zobrazí hello oddíl vlastností pro tento skript. Z hello horní části obrazovky hello se můžete znovu spustit skript hello nebo povyšte ho.

    ![Vlastnosti akce skriptu](./media/hdinsight-hadoop-customize-cluster-linux/promote-script-actions.png)

6. Můžete taky hello **...**  toohello napravo od položky na hello akcí skriptů části tooperform akce.

    ![Skript akce... využití](./media/hdinsight-hadoop-customize-cluster-linux/deletepromoted.png)

### <a name="using-azure-powershell"></a>Použití Azure Powershell

| Použijte hello... | příliš... |
| --- | --- |
| Get-AzureRmHDInsightPersistedScriptAction |Načíst informace o trvalé akce se skripty |
| Get-AzureRmHDInsightScriptActionHistory |Načtení historie skript akce, které jsou použity toohello clusteru nebo podrobnosti specifického skriptu |
| Set-AzureRmHDInsightPersistedScriptAction |Zvýší úroveň ad hoc tooa akce skriptu trvalé akce skriptu |
| Remove-AzureRmHDInsightPersistedScriptAction |Sníží úroveň akci ad hoc tooan akcí trvalého skriptu |

> [!IMPORTANT]
> Pomocí `Remove-AzureRmHDInsightPersistedScriptAction` nezruší hello akce skriptu. Tato rutina odebere pouze trvalou příznak hello.

ukazuje, jak pomocí rutin toopromote hello Hello následující ukázkový skript a potom snížení úrovně skriptu.

[!code-powershell[main](../../powershell_scripts/hdinsight/use-script-action/use-script-action.ps1?range=123-140)]

### <a name="using-hello-azure-cli"></a>Pomocí hello rozhraní příkazového řádku Azure

| Použijte hello... | příliš... |
| --- | --- |
| `azure hdinsight script-action persisted list <clustername>` |Načtení seznamu akcí trvalého skriptu |
| `azure hdinsight script-action persisted show <clustername> <scriptname>` |Načíst informace o konkrétní trvalého skriptu akce |
| `azure hdinsight script-action history list <clustername>` |Načtení historie skript akce, které jsou použity toohello clusteru |
| `azure hdinsight script-action history show <clustername> <scriptname>` |Načíst informace o konkrétní skript akce |
| `azure hdinsight script action persisted set <clustername> <scriptexecutionid>` |Zvýší úroveň ad hoc tooa akce skriptu trvalé akce skriptu |
| `azure hdinsight script-action persisted delete <clustername> <scriptname>` |Sníží úroveň akci ad hoc tooan akcí trvalého skriptu |

> [!IMPORTANT]
> Pomocí `azure hdinsight script-action persisted delete` nezruší hello akce skriptu. Tato rutina odebere pouze trvalou příznak hello.

### <a name="using-hello-hdinsight-net-sdk"></a>Pomocí hello HDInsight .NET SDK

Příklad použití hello .NET SDK tooretrieve skriptu historie z clusteru, zvýšení úrovně nebo snížení úrovně skriptů najdete v tématu [https://github.com/Azure-Samples/hdinsight-dotnet-script-action](https://github.com/Azure-Samples/hdinsight-dotnet-script-action).

> [!NOTE]
> Tento příklad také ukazuje, jak hello tooinstall aplikace HDInsight pomocí sady .NET SDK.

## <a name="support-for-open-source-software-used-on-hdinsight-clusters"></a>Podpora pro open-source softwaru použít na clustery HDInsight

Hello služby Microsoft Azure HDInsight používá prostředí technologie open source vytvořen kolem Hadoop. Microsoft Azure poskytuje obecné úroveň podpory pro technologie open source. Další informace najdete v tématu hello **podporu oboru** části hello [nejčastější dotazy týkající se podpory Azure web](https://azure.microsoft.com/support/faq/). Hello služba HDInsight poskytuje další úroveň podpory pro integrované komponenty.

Existují dva typy open-source komponent, které jsou k dispozici v hello služby HDInsight:

* **Integrované komponenty** -tyto komponenty jsou předinstalované na clustery HDInsight a poskytují základní funkce služby hello clusteru. Například YARN ResourceManager, hello Hive dotazovací jazyk (HiveQL) a knihovna Mahout hello patří toothis kategorie. Úplný seznam součástí clusteru je k dispozici v [co je nového ve verzích clusterů systému Hadoop hello poskytovaných v HDInsight](hdinsight-component-versioning.md).
* **Vlastní komponenty** -, jako uživatel hello clusteru, můžete nainstalovat nebo použít v vaše úlohy žádné součásti k dispozici v komunitě hello nebo vytvořené vámi.

> [!WARNING]
> Součásti, které jsou součástí clusteru HDInsight hello jsou plně podporovány. Microsoft Support pomáhá tooisolate a vyřešit problémy související toothese součásti.
>
> Vlastní komponenty přijímat vyvineme podporu toohelp toofurther můžete vyřešit problém hello. Podporu společnosti Microsoft může být schopný tooresolve hello problém nebo může vás vyzvou tooengage dostupné kanály pro technologiích s otevřeným zdrojem hello kterých se nachází hluboké znalosti pro tuto technologii. Například existuje mnoho komunity webů, které lze použít jako: [fórum MSDN pro HDInsight](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Také Apache projekty mají na projektu serverů [http://apache.org](http://apache.org), například: [Hadoop](http://hadoop.apache.org/).

Hello služba HDInsight poskytuje několik způsobů toouse vlastní součásti. Dobrý den, které se vztahuje stejnou úroveň podpory, bez ohledu na to, jak je součást použít nebo nainstalovat na clusteru hello. Hello následující seznam popisuje hello nejběžnější způsoby, jak je možné vlastní komponenty v clusterech HDInsight:

1. Úloha odeslání - Hadoop nebo jiné typy úloh, které spustit nebo používat vlastní komponenty může být odeslaná toohello clusteru.

2. Přizpůsobení clusteru – při vytváření clusteru, můžete zadat další nastavení a vlastní součásti, které jsou nainstalovány na uzlech clusteru hello.

3. Ukázky - oblíbených vlastní součásti, Microsoft a ostatní mohou poskytnout ukázky použití těchto součástí v clusterech HDInsight hello. Tyto soubory jsou uvedeny bez podpory.

## <a name="troubleshooting"></a>Řešení potíží

Můžete použít Ambari webového uživatelského rozhraní tooview informací zaznamenaných akcí skriptů. Pokud skript hello selže při vytváření clusteru, hello protokoly jsou také k dispozici v hello výchozí účet úložiště přidruženého k hello clusteru. Tato část obsahuje informace o tom, jak tooretrieve hello protokolů pomocí obou těchto možností.

### <a name="using-hello-ambari-web-ui"></a>Pomocí hello webové uživatelské rozhraní Ambari

1. V prohlížeči přejděte toohttps://CLUSTERNAME.azurehdinsight.net. Nahraďte název clusteru s názvem hello clusteru HDInsight.

    Po zobrazení výzvy zadejte název účtu správce hello (správce) a heslo pro hello cluster. Přihlašovací údaje správce hello tooreenter může mít v webového formuláře.

2. Z panelu hello hello horní části stránky hello, vyberte hello **ops** položku. Zobrazí se seznam aktuální a předchozí operace provedené na clusteru hello prostřednictvím Ambari.

    ![Panel uživatelského rozhraní Ambari web s ops vybrané](./media/hdinsight-hadoop-customize-cluster-linux/ambari-nav.png)

3. Najít hello položky, které mají **spustit\_customscriptaction** v hello **Operations** sloupce. Tyto položky se vytvoří při spuštění akcí skriptů hello.

    ![Snímek obrazovky operací](./media/hdinsight-hadoop-customize-cluster-linux/ambariscriptaction.png)

    tooview hello STDOUT a STDERR výstup, vyberte položku run\customscriptaction hello a procházení hello odkazy. Tento výstup se vygeneruje, když hello skript spustí a můžou obsahovat užitečné informace.

### <a name="access-logs-from-hello-default-storage-account"></a>Přístup k protokolům z hello výchozí účet úložiště

Pokud se vytvoření clusteru hello nezdaří z důvodu tooa skript akce chyba, hello protokoly jsou přístupné z účtu úložiště clusteru hello.

* Hello protokol úložiště jsou k dispozici v `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\CLUSTER_NAME\DATE`.

    ![Snímek obrazovky operací](./media/hdinsight-hadoop-customize-cluster-linux/script_action_logs_in_storage.png)

    V tomto adresáři hello protokoly jsou headnode, workernode a uzly zookeeper uspořádány samostatně. Tady je několik příkladů:

    * **Headnode** - `<uniqueidentifier>AmbariDb-hn0-<generated_value>.cloudapp.net`

    * **Pracovního uzlu** - `<uniqueidentifier>AmbariDb-wn0-<generated_value>.cloudapp.net`

    * **Zookeeper uzlu** - `<uniqueidentifier>AmbariDb-zk0-<generated_value>.cloudapp.net`

* Všechny stdout a stderr hello odpovídající hostitele je nahrán toohello účet úložiště. Existuje **výstup -\*.txt** a **chyby -\*.txt** pro jednotlivé akce skriptu. Hello *.txt výstupní soubor obsahuje informace o hello URI hello skriptu, který nebyl spuštěn na hostiteli hello. Například

        'Start downloading script locally: ', u'https://hdiconfigactions.blob.core.windows.net/linuxrconfigactionv01/r-installer-v01.sh'

* Je možné, opakovaně vytvořit cluster akce skriptu s hello stejný název. V takovém případě můžete odlišit hello relevantní protokoly na základě názvu složky datum hello. Struktura složek hello pro cluster s podporou (mycluster) vytvořen na různá data se například zobrazí podobné toohello následující položky protokolu:

    `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\mycluster\2015-10-04` `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\mycluster\2015-10-05`

* Pokud vytvoříte cluster akce skriptu s hello stejný název v hello stejný den, můžete použít hello jedinečnou předponu tooidentify hello příslušných protokolových souborů.

* Pokud vytvoříte cluster s podporou téměř 12:00 AM (půlnoc), je možné, že soubory protokolu hello rozprostřít do dvou dnů. V takových případech uvidíte dvě složky na jiné datum pro hello stejného clusteru.

* Odesílání protokolů soubory toohello výchozí kontejner může trvat až minut too5, zejména u velkých clusterech. Ano Pokud chcete tooaccess hello protokoly, nedoporučuje se mazat okamžitě hello clusteru neúspěšné provedení akce skriptu.

### <a name="ambari-watchdog"></a>Ambari sledovacího zařízení

> [!WARNING]
> Neměňte heslo hello hello Ambari sledovací zařízení (hdinsightwatchdog) v clusteru HDInsight se systémem Linux. Změna hello heslo pro tento účet dělí hello možnost toorun nové akcí skriptů v clusteru HDInsight hello.

### <a name="cant-import-name-blobservice"></a>Nelze importovat název BlobService

__Příznaky__: hello selhání akce skriptu. Při zobrazení hello operaci Ambari, zobrazí se text podobné toohello následující chybě:

```
Traceback (most recent call list):
  File "/var/lib/ambari-agent/cache/custom_actions/scripts/run_customscriptaction.py", line 21, in <module>
    from azure.storage.blob import BlobService
ImportError: cannot import name BlobService
```

__Příčina__: k této chybě dojde, pokud upgradujete klienta hello Python Azure Storage, který je součástí clusteru HDInsight hello. HDInsight očekává klienta Azure Storage 0.20.0.

__Řešení__: tooresolve tato chyba, ručně připojte pomocí uzlu clusteru tooeach `ssh` a hello použijte následující příkaz tooreinstall hello verze klienta pro správné úložiště:

```
sudo pip install azure-storage==0.20.0
```

Informace o připojování toohello clusteru pomocí protokolu SSH naleznete v tématu [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

### <a name="history-doesnt-show-scripts-used-during-cluster-creation"></a>Historie nezobrazí skripty použité při vytváření clusteru

Pokud váš cluster byl vytvořen ještě před 15. března 2016, nemusíte vidět položku v historii akcí skriptu. Změníte-li velikost clusteru hello po 15. března 2016, hello skriptů pomocí při vytváření clusteru se zobrazí v historii jako uplatňují se, že toonew uzly v clusteru hello jako součást hello změnit velikost operace.

Existují dvě výjimky:

* Pokud je váš cluster vytvořený před 1. září 2015. Toto datum je při zavedených akce skriptu. Žádného clusteru vytvořené před tímto datem nelze použít akcí skriptů pro vytvoření clusteru.

* Pokud používají různé akce skriptu při vytváření clusteru a používá stejný název pro několik skriptů nebo hello hello stejný název, stejným identifikátorem URI, ale různé parametry pro několik skriptů. V těchto případech se zobrazí hello následující chybě:

    Žádné nový skript, který může být akce byla spuštěna na tomto clusteru z důvodu tooconflicting skriptu názvy v existující skripty. Vytvoření skriptu názvy uvedených v clusteru musí být všechny jedinečné. Existující skripty jsou spuštěné v změny velikosti.

## <a name="next-steps"></a>Další kroky

* [Vývoj skriptů akce skriptu pro HDInsight](hdinsight-hadoop-script-actions-linux.md)
* [Nainstalovat a používat Solr clustery prostředí HDInsight](hdinsight-hadoop-solr-install-linux.md)
* [Nainstalovat a používat Giraph clustery prostředí HDInsight](hdinsight-hadoop-giraph-install-linux.md)
* [Přidejte další úložiště clusteru HDInsight tooan](hdinsight-hadoop-add-storage.md)

[img-hdi-cluster-states]: ./media/hdinsight-hadoop-customize-cluster-linux/HDI-Cluster-state.png "Fáze při vytváření clusteru"
