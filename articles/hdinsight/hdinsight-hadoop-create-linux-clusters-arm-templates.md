---
title: "aaaCreate Hadoop clusterů pomocí šablon - Azure HDInsight | Microsoft Docs"
description: "Zjistěte, jak toocreate clusterů pro HDInsight pomocí šablony Resource Manageru"
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 00a80dea-011f-44f0-92a4-25d09db9d996
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/30/2017
ms.author: jgao
ms.openlocfilehash: 92a6c1d888e401a11537dba34f188245ac17f448
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-hadoop-clusters-in-hdinsight-by-using-resource-manager-templates"></a>Vytvoření clusterů systému Hadoop v HDInsight pomocí šablony Resource Manageru
[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

V tomto článku se dozvíte několik způsobů toocreate Azure HDInsight clustery s šablon Azure Resource Manageru. Další informace najdete v tématu [nasazení aplikace pomocí šablony Azure Resource Manageru](../azure-resource-manager/resource-group-template-deploy.md). toolearn o jiných nástrojů pro vytvoření clusteru a funkcí, klikněte na tlačítko hello karta selektor na hello horní části této stránky nebo najdete [metody vytváření clusterů](hdinsight-hadoop-provision-linux-clusters.md#cluster-setup-methods).

## <a name="prerequisites"></a>Požadavky
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

toofollow hello pokyny v tomto článku, budete potřebovat:

* [Předplatné](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Prostředí Azure PowerShell nebo Azure CLI.

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell-and-cli.md)]

### <a name="resource-manager-templates"></a>Šablony Resource Manageru
Šablonu Resource Manager umožňuje snadno toocreate hello následující pro vaši aplikaci v rámci jediné koordinované operace:
* Clustery prostředí HDInsight a jejich závislé prostředky (například hello výchozí účet úložiště)
* Další prostředky (například Azure SQL Database toouse Apache Sqoop)

V šabloně hello definujete hello prostředky, které jsou potřebné pro aplikace hello. Je také zadat hodnoty tooinput parametry nasazení pro různá prostředí. Šablona Hello se skládá z JSON a výrazy, že používáte tooconstruct hodnoty pro vaše nasazení.

Můžete najít ukázky šablony HDInsight v [šablon Azure rychlý Start](https://azure.microsoft.com/resources/templates/?term=hdinsight). Použít napříč platformami [Visual Studio Code](https://code.visualstudio.com/#alt-downloads) s hello [Resource Manager rozšíření](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools) nebo textový editor toosave hello šablony do souboru na pracovní stanici. Zjistíte, jak toocall hello šablony pomocí různých metod.

Další informace o šablonách Resource Manager najdete v části hello následující články:

* [Vytváření šablon Azure Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md)
* [Nasazení aplikace pomocí šablony Azure Resource Manager](../azure-resource-manager/resource-group-template-deploy.md)

## <a name="generate-templates"></a>Generování šablon

Pomocí hello portálu Azure můžete nakonfigurovat všechny vlastnosti hello clusteru a potom uložte hello šablonu ještě před nasazením. Můžete je znovu hello šablony.

**toogenerate a šablony pomocí hello portálu Azure**

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Klikněte na tlačítko **nový** v levé nabídce hello, klikněte na tlačítko **Intelligence + analýzy**a potom klikněte na **HDInsight**.
3. Postupujte podle vlastnosti tooenter pokyny hello. Můžete použít buď hello **rychle vytvořit** nebo hello **vlastní** možnost.
4. Na hello **Souhrn** , klikněte na **stáhnout šablonu a parametry**:

    ![Vytvoření stažení šablony správce prostředků clusteru HDInsight Hadoop](./media/hdinsight-hadoop-create-linux-clusters-arm-templates/hdinsight-create-cluster-resource-manager-template-download.png)

    Zobrazí seznam hello soubor šablony, soubor parametrů a kódu ukázky používá toodeploy hello šablonu:

    ![HDInsight Hadoop vytvoření clusteru možnosti stahování šablony Resource Manageru](./media/hdinsight-hadoop-create-linux-clusters-arm-templates/hdinsight-create-cluster-resource-manager-template-download-options.png)

    Tady můžete stáhnout hello šablonu, uložit knihovna šablon tooyour nebo nasazení šablony hello.

    tooaccess šablonu v knihovně, klikněte na tlačítko **další služby** z levé nabídce hello a pak klikněte na tlačítko **šablony** (v části hello **jiných** kategorie).

    > [!Note]
    > soubor šablony a parametry Hello musí použít společně. Jinak můžete získat neočekávané výsledky. Například hello výchozí **clusterKind** hodnota vlastnosti je vždy **hadoop**, bez ohledu na tom, co jste zadejte před stažením šablony hello.



## <a name="deploy-with-powershell"></a>Nasazení s využitím PowerShellu

Tento postup vytvoří Hadoop cluster v HDInsight.

1. Uložte soubor JSON hello v hello [příloha](#appx-a-arm-template) tooyour pracovní stanice. V hello skript prostředí PowerShell, je název souboru hello `C:\HDITutorials-ARM\hdinsight-arm-template.json`.
2. V případě potřeby nastavte hello parametry a proměnné.
3. Spusťte hello šablony pomocí hello následující skript prostředí PowerShell:

        ####################################
        # Set these variables
        ####################################
        #region - used for creating Azure service names
        $nameToken = "<Enter an Alias>"
        $templateFile = "C:\HDITutorials-ARM\hdinsight-arm-template.json"
        #endregion

        ####################################
        # Service names and variables
        ####################################
        #region - service names
        $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

        $resourceGroupName = $namePrefix + "rg"
        $hdinsightClusterName = $namePrefix + "hdi"
        $defaultStorageAccountName = $namePrefix + "store"
        $defaultBlobContainerName = $hdinsightClusterName

        $location = "East US 2"

        $armDeploymentName = $namePrefix
        #endregion

        ####################################
        # Connect tooAzure
        ####################################
        #region - Connect tooAzure subscription
        Write-Host "`nConnecting tooyour Azure subscription ..." -ForegroundColor Green
        try{Get-AzureRmContext}
        catch{Login-AzureRmAccount}
        #endregion

        # Create a resource group
        New-AzureRmResourceGroup -Name $resourceGroupName -Location $Location

        # Create cluster and hello dependent storage account
        $parameters = @{clusterName="$hdinsightClusterName"}

        New-AzureRmResourceGroupDeployment `
            -Name $armDeploymentName `
            -ResourceGroupName $resourceGroupName `
            -TemplateFile $templateFile `
            -TemplateParameterObject $parameters

        # List cluster
        Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName

    Hello skript prostředí PowerShell lze konfigurovat pouze hello název clusteru. název účtu úložiště Hello je pevně zakódovaná v šabloně hello. Jste heslo uživatele clusteru výzvami tooenter hello. (výchozí uživatelské jméno hello **správce**.) Také jste heslo uživatele SSH výzvami tooenter hello. (uživatelské jméno SSH výchozí hello **sshuser**.)  

Další informace najdete v tématu [nasadit v prostředí PowerShell](../azure-resource-manager/resource-group-template-deploy.md#deploy-local-template).

## <a name="deploy-with-cli"></a>Nasazení pomocí rozhraní příkazového řádku
Následující ukázka Hello používá rozhraní příkazového řádku Azure (CLI). Vytvoří cluster a jeho účet závislého úložiště a kontejneru voláním šablony Resource Manageru:

    azure login
    azure config mode arm
    azure group create -n hdi1229rg -l "East US"
    azure group deployment create --resource-group "hdi1229rg" --name "hdi1229" --template-file "C:\HDITutorials-ARM\hdinsight-arm-template.json"

Jste tooenter výzvami:
* Název clusteru Hello.
* heslo uživatele Hello clusteru. (výchozí uživatelské jméno hello **správce**.)
* heslo uživatele SSH Hello. (uživatelské jméno SSH výchozí hello **sshuser**.)

Hello následující kód obsahuje vložené parametry:

    azure group deployment create --resource-group "hdi1229rg" --name "hdi1229" --template-file "c:\Tutorials\HDInsightARM\create-linux-based-hadoop-cluster-in-hdinsight.json" --parameters '{\"clusterName\":{\"value\":\"hdi1229\"},\"clusterLoginPassword\":{\"value\":\"Pass@word1\"},\"sshPassword\":{\"value\":\"Pass@word1\"}}'

## <a name="deploy-with-hello-rest-api"></a>Nasazení s hello REST API
V tématu [nasadit s hello REST API](../azure-resource-manager/resource-group-template-deploy-rest.md).

## <a name="deploy-with-visual-studio"></a>Nasazení s využitím sady Visual Studio
 Pomocí sady Visual Studio toocreate projekt skupiny prostředků a nasadit tooAzure hello uživatelském rozhraní. Vyberete typ hello tooinclude prostředky ve vašem projektu. Tyto prostředky se automaticky přidají toohello šablony Resource Manageru. projekt Hello také poskytuje šablony hello toodeploy skript prostředí PowerShell.

Úvod toousing Visual Studio se skupinami prostředků, najdete v části [vytvoření a nasazení skupin prostředků Azure pomocí sady Visual Studio](../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).

## <a name="troubleshoot"></a>Řešení potíží

Pokud narazíte na problémy s vytvářením clusterů HDInsight, podívejte se na [požadavky na řízení přístupu](hdinsight-administer-use-portal-linux.md#create-clusters).

## <a name="next-steps"></a>Další kroky
V tomto článku jste se naučili několik způsobů toocreate clusteru služby HDInsight. toolearn více, najdete v části hello následující články:

* Příklad nasazení prostředků prostřednictvím klientské knihovny hello .NET, naleznete v části [nasadit prostředky pomocí knihovny .NET a šablonu](../virtual-machines/windows/csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Podrobný příklad nasazení aplikace naleznete v tématu [zřídit a nasadit mikroslužeb předvídatelné v Azure](../app-service-web/app-service-deploy-complex-application-predictably.md).
* Pokyny k nasazení vašeho prostředí toodifferent řešení najdete v tématu [vývojová a testovací prostředí v Microsoft Azure](../solution-dev-test-environments.md).
* toolearn o hello části hello šablony Azure Resource Manageru, najdete v části [vytváření šablon](../azure-resource-manager/resource-group-authoring-templates.md).
* Seznam funkcí hello v šablonu Azure Resource Manager můžete použít, najdete v části [funkce šablon](../azure-resource-manager/resource-group-template-functions.md).

## <a name="appendix-resource-manager-template-toocreate-a-hadoop-cluster"></a>Dodatek: Resource Manager šablony toocreate clusteru Hadoop
Hello následující šablony Azure Resource Manager vytvoří cluster systémem Linux Hadoop se účet závislého úložiště Azure hello.

> [!NOTE]
> Tato ukázka obsahuje informace o konfiguraci pro metaúložiště Hive a metaúložiště Oozie. Odebrání oddílu hello nebo nakonfigurujte hello části před použitím šablony hello.
>
>

    {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "clusterName": {
        "type": "string",
        "metadata": {
            "description": "hello name of hello HDInsight cluster toocreate."
        }
        },
        "clusterLoginUserName": {
        "type": "string",
        "defaultValue": "admin",
        "metadata": {
            "description": "These credentials can be used toosubmit jobs toohello cluster and toolog into cluster dashboards."
        }
        },
        "clusterLoginPassword": {
        "type": "securestring",
        "metadata": {
            "description": "hello password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
        }
        },
        "sshUserName": {
        "type": "string",
        "defaultValue": "sshuser",
        "metadata": {
            "description": "These credentials can be used tooremotely access hello cluster."
        }
        },
        "sshPassword": {
        "type": "securestring",
        "metadata": {
            "description": "hello password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
        }
        },
        "location": {
        "type": "string",
        "defaultValue": "East US",
        "allowedValues": [
            "East US",
            "East US 2",
            "North Central US",
            "South Central US",
            "West US",
            "North Europe",
            "West Europe",
            "East Asia",
            "Southeast Asia",
            "Japan East",
            "Japan West",
            "Australia East",
            "Australia Southeast"
        ],
        "metadata": {
            "description": "hello location where all azure resources will be deployed."
        }
        },
        "clusterType": {
        "type": "string",
        "defaultValue": "hadoop",
        "allowedValues": [
            "hadoop",
            "hbase",
            "storm",
            "spark"
        ],
        "metadata": {
            "description": "hello type of hello HDInsight cluster toocreate."
        }
        },
        "clusterWorkerNodeCount": {
        "type": "int",
        "defaultValue": 2,
        "metadata": {
            "description": "hello number of nodes in hello HDInsight cluster."
        }
        }
    },
    "variables": {
        "defaultApiVersion": "2015-05-01-preview",
        "clusterApiVersion": "2015-03-01-preview",
        "clusterStorageAccountName": "[concat(parameters('clusterName'),'store')]"
    },
    "resources": [
        {
        "name": "[variables('clusterStorageAccountName')]",
        "type": "Microsoft.Storage/storageAccounts",
        "location": "[parameters('location')]",
        "apiVersion": "[variables('defaultApiVersion')]",
        "dependsOn": [ ],
        "tags": { },
        "properties": {
            "accountType": "Standard_LRS"
        }
        },
        {
        "name": "[parameters('clusterName')]",
        "type": "Microsoft.HDInsight/clusters",
        "location": "[parameters('location')]",
        "apiVersion": "[variables('clusterApiVersion')]",
        "dependsOn": [ "[concat('Microsoft.Storage/storageAccounts/',variables('clusterStorageAccountName'))]" ],
        "tags": {

        },
        "properties": {
            "clusterVersion": "3.4",
            "osType": "Linux",
            "tier": "standard",
            "clusterDefinition": {
            "kind": "[parameters('clusterType')]",
            "configurations": {
                "gateway": {
                "restAuthCredential.isEnabled": true,
                "restAuthCredential.username": "[parameters('clusterLoginUserName')]",
                "restAuthCredential.password": "[parameters('clusterLoginPassword')]"
                },
                "hive-site": {
                    "javax.jdo.option.ConnectionDriverName": "com.microsoft.sqlserver.jdbc.SQLServerDriver",
                    "javax.jdo.option.ConnectionURL": "jdbc:sqlserver://myadla0901dbserver.database.windows.net;database=myhive20160901;encrypt=true;trustServerCertificate=true;create=false;loginTimeout=300",
                    "javax.jdo.option.ConnectionUserName": "johndole",
                    "javax.jdo.option.ConnectionPassword": "myPassword$"
                },
                "hive-env": {
                    "hive_database": "Existing MSSQL Server database with SQL authentication",
                    "hive_database_name": "myhive20160901",
                    "hive_database_type": "mssql",
                    "hive_existing_mssql_server_database": "myhive20160901",
                    "hive_existing_mssql_server_host": "myadla0901dbserver.database.windows.net",
                    "hive_hostname": "myadla0901dbserver.database.windows.net"
                },
                "oozie-site": {
                    "oozie.service.JPAService.jdbc.driver": "com.microsoft.sqlserver.jdbc.SQLServerDriver",
                    "oozie.service.JPAService.jdbc.url": "jdbc:sqlserver://myadla0901dbserver.database.windows.net;database=myhive20160901;encrypt=true;trustServerCertificate=true;create=false;loginTimeout=300",
                    "oozie.service.JPAService.jdbc.username": "johndole",
                    "oozie.service.JPAService.jdbc.password": "myPassword$",
                    "oozie.db.schema.name": "oozie"
                },
                "oozie-env": {
                    "oozie_database": "Existing MSSQL Server database with SQL authentication",
                    "oozie_database_name": "myhive20160901",
                    "oozie_database_type": "mssql",
                    "oozie_existing_mssql_server_database": "myhive20160901",
                    "oozie_existing_mssql_server_host": "myadla0901dbserver.database.windows.net",
                    "oozie_hostname": "myadla0901dbserver.database.windows.net"
                }            
            }
            },
            "storageProfile": {
            "storageaccounts": [
                {
                "name": "[concat(variables('clusterStorageAccountName'),'.blob.core.windows.net')]",
                "isDefault": true,
                "container": "[parameters('clusterName')]",
                "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('clusterStorageAccountName')), variables('defaultApiVersion')).keys[0].value]"
                }
            ]
            },
            "computeProfile": {
            "roles": [
                {
                "name": "headnode",
                "targetInstanceCount": "2",
                "hardwareProfile": {
                    "vmSize": "Standard_D3"
                },
                "osProfile": {
                    "linuxOperatingSystemProfile": {
                    "username": "[parameters('sshUserName')]",
                    "password": "[parameters('sshPassword')]"
                    }
                }
                },
                {
                "name": "workernode",
                "targetInstanceCount": "[parameters('clusterWorkerNodeCount')]",
                "hardwareProfile": {
                    "vmSize": "Standard_D3"
                },
                "osProfile": {
                    "linuxOperatingSystemProfile": {
                    "username": "[parameters('sshUserName')]",
                    "password": "[parameters('sshPassword')]"
                    }
                }
                }
            ]
            }
        }
        }
    ],
    "outputs": {
        "cluster": {
        "type": "object",
        "value": "[reference(resourceId('Microsoft.HDInsight/clusters',parameters('clusterName')))]"
        }
    }
    }

## <a name="appendix-resource-manager-template-toocreate-a-spark-cluster"></a>Dodatek: Resource Manager šablony toocreate clusteru Spark

Tato část obsahuje šablony Resource Manageru, které můžete použít toocreate clusteru HDInsight Spark. Tato šablona zahrnuje konfigurace pro `spark-defaults` a `spark-thrift-sparkconf` (pro clustery Spark 1.6) a `spark2-defaults` a `spark2-thrift-sparkconf` (pro clustery Spark 2). Kromě toho toothis, HDInsight vypočítá a nastaví konfigurace, jako `spark.executor.instances`, `spark.executor.memory`, a `spark.executor.cores` na základě velikosti clusteru hello. 

Pokud nastavíte všechny jeden parametr v části v rámci samotné hello šablony, HDInsight nepodporuje výpočtu a nastavit další parametry hello hello stejné části. Například parametr `spark.executor.instances` v hello `spark-defaults` konfigurace. Pokud nastavíte parametr jiné (například `spark.yarn.exector.memoryOverhead`) v hello `spark-defaults` konfigurace, HDInsight nepodporuje výpočtu a nastavit hello `spark.executor.instances` také parametr.

    {
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "0.9.0.0",
    "parameters": {
        "clusterName": {
            "type": "string",
            "metadata": {
                "description": "hello name of hello HDInsight cluster toocreate."
            }
        },
        "clusterLoginUserName": {
            "type": "string",
            "defaultValue": "admin",
            "metadata": {
                "description": "These credentials can be used toosubmit jobs toohello cluster and toolog into cluster dashboards."
            }
        },
        "clusterLoginPassword": {
            "type": "securestring",
            "metadata": {
                "description": "hello password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
            }
        },
        "location": {
            "type": "string",
            "defaultValue": "southcentralus",
            "metadata": {
                "description": "hello location where all azure resources will be deployed."
            }
        },
        "clusterVersion": {
            "type": "string",
            "defaultValue": "3.5",
            "metadata": {
                "description": "HDInsight cluster version."
            }
        },
        "clusterWorkerNodeCount": {
            "type": "int",
            "defaultValue": 4,
            "metadata": {
                "description": "hello number of nodes in hello HDInsight cluster."
            }
        },
        "clusterKind": {
            "type": "string",
            "defaultValue": "SPARK",
            "metadata": {
                "description": "hello type of hello HDInsight cluster toocreate."
            }
        },
        "sshUserName": {
            "type": "string",
            "defaultValue": "sshuser",
            "metadata": {
                "description": "These credentials can be used tooremotely access hello cluster."
            }
        },
        "sshPassword": {
            "type": "securestring",
            "metadata": {
                "description": "hello password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
            }
        }
    },
    "variables": {
        "defaultApiVersion": "2017-06-01",
        "clusterStorageAccountName": "[concat(parameters('clusterName'),'store')]"
    },
    "resources": [
        {
        "name": "[variables('clusterStorageAccountName')]",
        "type": "Microsoft.Storage/storageAccounts",
        "location": "[parameters('location')]",
        "apiVersion": "[variables('defaultApiVersion')]",
        "dependsOn": [ ],
        "tags": { },
        "properties": {
            "accountType": "Standard_LRS"
        }
        },
    {
            "apiVersion": "2015-03-01-preview",
            "name": "[parameters('clusterName')]",
            "type": "Microsoft.HDInsight/clusters",
            "location": "[parameters('location')]",
            "dependsOn": [],
            "properties": {
                "clusterVersion": "[parameters('clusterVersion')]",
                "osType": "Linux",
                "tier": "standard",
                "clusterDefinition": {
                    "kind": "[parameters('clusterKind')]",
                    "configurations": {
                        "gateway": {
                            "restAuthCredential.isEnabled": true,
                            "restAuthCredential.username": "[parameters('clusterLoginUserName')]",
                            "restAuthCredential.password": "[parameters('clusterLoginPassword')]"
                        },
                        "spark-defaults": {
                            "spark.executor.cores": "2"
                        },
                        "spark-thrift-sparkconf": {
                            "spark.yarn.executor.memoryOverhead": "896"
                        }
                    }
                },
                "storageProfile": {
                    "storageaccounts": [
                        {
                            "name": "[concat(variables('clusterStorageAccountName'),'.blob.core.windows.net')]",
                            "isDefault": true,
                            "container": "[parameters('clusterName')]",
                            "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('clusterStorageAccountName')), variables('defaultApiVersion')).keys[0].value]"
                        }
                    ]
                },
                "computeProfile": {
                    "roles": [
                        {
                            "name": "headnode",
                            "minInstanceCount": 1,
                            "targetInstanceCount": 2,
                            "hardwareProfile": {
                                "vmSize": "Standard_D12"
                            },
                            "osProfile": {
                                "linuxOperatingSystemProfile": {
                                    "username": "[parameters('sshUserName')]",
                                    "password": "[parameters('sshPassword')]"
                                }
                            },
                            "virtualNetworkProfile": null,
                            "scriptActions": []
                        },
                        {
                            "name": "workernode",
                            "minInstanceCount": 1,
                            "targetInstanceCount": 4,
                            "hardwareProfile": {
                                "vmSize": "Standard_D4"
                            },
                            "osProfile": {
                                "linuxOperatingSystemProfile": {
                                    "username": "[parameters('sshUserName')]",
                                    "password": "[parameters('sshPassword')]"
                                    }
                                },
                                "virtualNetworkProfile": null,
                                "scriptActions": []
                            }
                        ]
                    }
                }
            }
        ]
    }
