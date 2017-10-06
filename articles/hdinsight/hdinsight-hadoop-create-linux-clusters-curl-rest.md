---
title: "aaaCreate clusterů systému Hadoop pomocí rozhraní API REST Azure - Azure | Microsoft Docs"
description: "Zjistěte, jak toocreate HDInsight clustery odesláním toohello šablony Azure Resource Manager rozhraní REST API Azure."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 98be5893-2c6f-4dfa-95ec-d4d8b5b7dcb5
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/10/2017
ms.author: larryfr
ms.openlocfilehash: 87b585e5084eccdc3d7c57483deabb4ad6e32597
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-hadoop-clusters-using-hello-azure-rest-api"></a>Vytváření clusterů systému Hadoop pomocí hello REST API služby Azure

[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

Zjistěte, jak toocreate HDInsight clusteru pomocí šablony Azure Resource Manager a hello rozhraní REST API Azure.

Hello rozhraní REST API Azure vám umožní operace správy tooperform na služby hostované v hello platformy Azure, včetně hello vytváření nových prostředků, jako je například clusterů HDInsight.

> [!IMPORTANT]
> Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

> [!NOTE]
> Hello kroky hello použití tohoto dokumentu [curl (https://curl.haxx.se/)](https://curl.haxx.se/) toocommunicate nástroj s hello rozhraní REST API Azure.

## <a name="create-a-template"></a>Vytvoření šablony

Šablony Azure Resource Manageru jsou dokumenty JSON, které popisují **skupiny prostředků** a všechny prostředky v ní (například HDInsight.) Tento přístup na základě šablon můžete toodefine hello prostředky, které potřebujete pro HDInsight v jedné šabloně.

Hello následující dokumentu JSON je fúzi hello šablony a parametry soubory z [https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password](https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password), která vytvoří systémem Linux cluster pomocí hello toosecure a heslo SSH uživatelský účet.

   ```json
   {
       "properties": {
           "template": {
               "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
               "contentVersion": "1.0.0.0",
               "parameters": {
                   "clusterType": {
                       "type": "string",
                       "allowedValues": ["hadoop",
                       "hbase",
                       "storm",
                       "spark"],
                       "metadata": {
                           "description": "hello type of hello HDInsight cluster toocreate."
                       }
                   },
                   "clusterName": {
                       "type": "string",
                       "metadata": {
                           "description": "hello name of hello HDInsight cluster toocreate."
                       }
                   },
                   "clusterLoginUserName": {
                       "type": "string",
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
                   "clusterStorageAccountName": {
                       "type": "string",
                       "metadata": {
                           "description": "hello name of hello storage account toobe created and be used as hello cluster's storage."
                       }
                   },
                   "clusterWorkerNodeCount": {
                       "type": "int",
                       "defaultValue": 4,
                       "metadata": {
                           "description": "hello number of nodes in hello HDInsight cluster."
                       }
                   }
               },
               "variables": {
                   "defaultApiVersion": "2015-05-01-preview",
                   "clusterApiVersion": "2015-03-01-preview"
               },
               "resources": [{
                   "name": "[parameters('clusterStorageAccountName')]",
                   "type": "Microsoft.Storage/storageAccounts",
                   "location": "[resourceGroup().location]",
                   "apiVersion": "[variables('defaultApiVersion')]",
                   "dependsOn": [],
                   "tags": {

                   },
                   "properties": {
                       "accountType": "Standard_LRS"
                   }
               },
               {
                   "name": "[parameters('clusterName')]",
                   "type": "Microsoft.HDInsight/clusters",
                   "location": "[resourceGroup().location]",
                   "apiVersion": "[variables('clusterApiVersion')]",
                   "dependsOn": ["[concat('Microsoft.Storage/storageAccounts/',parameters('clusterStorageAccountName'))]"],
                   "tags": {

                   },
                   "properties": {
                       "clusterVersion": "3.5",
                       "osType": "Linux",
                       "clusterDefinition": {
                           "kind": "[parameters('clusterType')]",
                           "configurations": {
                               "gateway": {
                                   "restAuthCredential.isEnabled": true,
                                   "restAuthCredential.username": "[parameters('clusterLoginUserName')]",
                                   "restAuthCredential.password": "[parameters('clusterLoginPassword')]"
                               }
                           }
                       },
                       "storageProfile": {
                           "storageaccounts": [{
                               "name": "[concat(parameters('clusterStorageAccountName'),'.blob.core.windows.net')]",
                               "isDefault": true,
                               "container": "[parameters('clusterName')]",
                               "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('clusterStorageAccountName')), variables('defaultApiVersion')).key1]"
                           }]
                       },
                       "computeProfile": {
                           "roles": [{
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
                           }]
                       }
                   }
               }],
               "outputs": {
                   "cluster": {
                       "type": "object",
                       "value": "[reference(resourceId('Microsoft.HDInsight/clusters',parameters('clusterName')))]"
                   }
               }
           },
           "mode": "incremental",
           "Parameters": {
               "clusterName": {
                   "value": "newclustername"
               },
               "clusterType": {
                   "value": "hadoop"
               },
               "clusterStorageAccountName": {
                   "value": "newstoragename"
               },
               "clusterLoginUserName": {
                   "value": "admin"
               },
               "clusterLoginPassword": {
                   "value": "changeme"
               },
               "sshUserName": {
                   "value": "sshuser"
               },
               "sshPassword": {
                   "value": "changeme"
               }
           }
       }
   }
   ```

V tomto příkladu se používá v hello kroky v tomto dokumentu. Nahraďte hello příklad *hodnoty* v hello **parametry** oddíl s hello hodnoty pro váš cluster.

> [!IMPORTANT]
> Šablona Hello používá hello výchozí počet uzlů pracovního procesu (4) pro cluster služby HDInsight. Pokud máte v plánu na víc než 32 uzlů pracovního procesu, je nutné vybrat velikost hlavního uzlu s alespoň s 8 jádry a 14 GB paměti ram.
>
> Další informace o velikosti uzlu a souvisejících nákladů, najdete v části [HDInsight ceny](https://azure.microsoft.com/pricing/details/hdinsight/).

## <a name="log-in-tooyour-azure-subscription"></a>Přihlaste se tooyour předplatného Azure

Postupujte podle kroků hello popsaných v [Začínáme s Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2) a připojte se pomocí hello předplatné tooyour `az login` příkaz.

## <a name="create-a-service-principal"></a>Vytvoření instančního objektu

> [!NOTE]
> Tyto kroky nejsou stručné verzi hello *vytvoření instančního objektu s heslem* části hello [použití Azure CLI toocreate objekt služby prostředků tooaccess](../azure-resource-manager/resource-group-authenticate-service-principal-cli.md#create-service-principal-with-password) dokumentu. Tyto kroky vytvoření instančního objektu, který je použité tooauthenticate toohello REST API služby Azure.

1. Z příkazového řádku použijte následující příkaz toolist hello předplatné Azure.

   ```bash
   az account list --query '[].{Subscription_ID:id,Tenant_ID:tenantId,Name:name}'  --output table
   ```

    V seznamu hello, vyberte předplatné hello má toouse a Všimněte si hello **ID_ODBĚRU** a __Tenant_ID__ sloupce. Uložte tyto hodnoty.

2. Použijte následující příkaz toocreate aplikace v Azure Active Directory hello.

   ```bash
   az ad app create --display-name "exampleapp" --homepage "https://www.contoso.org" --identifier-uris "https://www.contoso.org/example" --password <Your password> --query 'appId'
   ```

    Nahraďte hodnoty hello hello `--display-name`, `--homepage`, a `--identifier-uris` vlastními hodnotami. Zadejte heslo pro nový záznam služby Active Directory hello.

   > [!NOTE]
   > Hello `--home-page` a `--identifier-uris` hodnoty nepotřebujete tooreference vlastní webové stránky hostované na hello Internetu. Musí být jedinečné identifikátory URI.

   Hello hodnota vrácená z tohoto příkazu je hello __ID aplikace__ pro novou aplikaci hello. Uložte tuto hodnotu.

3. Použití hello následující příkaz toocreate hlavní název služby pomocí hello **ID aplikace**.

   ```bash
   az ad sp create --id <App ID> --query 'objectId'
   ```

     Hello hodnota vrácená z tohoto příkazu je hello __ID objektu__. Uložte tuto hodnotu.

4. Přiřadit hello **vlastníka** role toohello službu objektu zabezpečení pomocí hello **ID objektu** hodnotu. Použití hello **ID předplatného** jste dříve získali.

   ```bash
   az role assignment create --assignee <Object ID> --role Owner --scope /subscriptions/<Subscription ID>/
   ```

## <a name="get-an-authentication-token"></a>Získat ověřovací token

Použijte následující příkaz tooretrieve ověřovací token hello:

```bash
curl -X "POST" "https://login.microsoftonline.com/$TENANTID/oauth2/token" \
-H "Cookie: flight-uxoptin=true; stsservicecookie=ests; x-ms-gateway-slice=productionb; stsservicecookie=ests" \
-H "Content-Type: application/x-www-form-urlencoded" \
--data-urlencode "client_id=$APPID" \
--data-urlencode "grant_type=client_credentials" \
--data-urlencode "client_secret=$PASSWORD" \
--data-urlencode "resource=https://management.azure.com/"
```

Nastavit `$TENANTID`, `$APPID`, a `$PASSWORD` toohello hodnoty získané nebo použít dříve.

Pokud tento požadavek je úspěšné, obdrží odpověď 200 řady a hello odpovědi obsahuje dokument JSON.

Hello dokumentu JSON vrácený tuto žádost obsahuje element s názvem **access_token**. Hello hodnotu **access_token** je použité tooauthentication požadavky toohello REST API.

```json
{
    "token_type":"Bearer",
    "expires_in":"3599",
    "expires_on":"1463409994",
    "not_before":"1463406094",
    "resource":"https://management.azure.com/","access_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWoNBVGZNNXBPWWlKSE1iYTlnb0VLWSIsImtpZCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSJ9.eyJhdWQiOiJodHRwczovL21hbmFnZW1lbnQuYXp1cmUuY29tLyIsImlzcyI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI2Ny8iLCJpYXQiOjE0NjM0MDYwOTQsIm5iZiI6MTQ2MzQwNjA5NCwiZXhwIjoxNDYzNDA5OTk5LCJhcHBpZCI6IjBlYzcyMzM0LTZkMDMtNDhmYi04OWU1LTU2NTJiODBiZDliYiIsImFwcGlkYWNyIjoiMSIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI0Ny8iLCJvaWQiOiJlNjgxZTZiMi1mZThkLTRkZGUtYjZiMS0xNjAyZDQyNWQzOWYiLCJzdWIiOiJlNjgxZTZiMi1mZThkLTRkZGUtYjZiMS0xNjAyZDQyNWQzOWYiLCJ0aWQiOiI3MmY5ODhiZi04NmYxLTQxYWYtOTFhYi0yZDdjZDAxMWRiNDciLCJ2ZXIiOiIxLjAifQ.nJVERbeDHLGHn7ZsbVGBJyHOu2PYhG5dji6F63gu8XN2Cvol3J1HO1uB4H3nCSt9DTu_jMHqAur_NNyobgNM21GojbEZAvd0I9NY0UDumBEvDZfMKneqp7a_cgAU7IYRcTPneSxbD6wo-8gIgfN9KDql98b0uEzixIVIWra2Q1bUUYETYqyaJNdS4RUmlJKNNpENllAyHQLv7hXnap1IuzP-f5CNIbbj9UgXxLiOtW5JhUAwWLZ3-WMhNRpUO2SIB7W7tQ0AbjXw3aUYr7el066J51z5tC1AK9UC-mD_fO_HUP6ZmPzu5gLA6DxkIIYP3grPnRVoUDltHQvwgONDOw"
}
```

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Použijte hello následující toocreate skupinu prostředků.

* Nastavit `$SUBSCRIPTIONID` ID předplatného toohello přijal při vytváření hello instanční objekt.
* Nastavit `$ACCESSTOKEN` obdrželi v předchozím kroku hello toohello přístupový token.
* Nahraďte `DATACENTERLOCATION` s hello datového centra chcete skupinu prostředků hello toocreate a prostředky v. Například 'jihu USA".
* Nastavit `$RESOURCEGROUPNAME` název toohello chcete toouse pro tuto skupinu:

```bash
curl -X "PUT" "https://management.azure.com/subscriptions/$SUBSCRIPTIONID/resourcegroups/$RESOURCEGROUPNAME?api-version=2015-01-01" \
    -H "Authorization: Bearer $ACCESSTOKEN" \
    -H "Content-Type: application/json" \
    -d $'{
"location": "DATACENTERLOCATION"
}'
```

Pokud tento požadavek je úspěšné, obdrží odpověď 200 řady a hello odpovědi obsahuje dokument JSON obsahující informace o skupině hello. Hello `"provisioningState"` element obsahuje hodnotu `"Succeeded"`.

## <a name="create-a-deployment"></a>Vytvoření nasazení

Použijte následující příkaz toodeploy hello skupiny prostředků šablony toohello hello.

* Nastavit `$DEPLOYMENTNAME` název toohello chcete toouse pro toto nasazení.

```bash
curl -X "PUT" "https://management.azure.com/subscriptions/$SUBSCRIPTIONID/resourcegroups/$RESOURCEGROUPNAME/providers/microsoft.resources/deployments/$DEPLOYMENTNAME?api-version=2015-01-01" \
-H "Authorization: Bearer $ACCESSTOKEN" \
-H "Content-Type: application/json" \
-d "{set your body string toohello template and parameters}"
```

> [!NOTE]
> Pokud jste uložili soubor tooa hello šablony, můžete použít následující příkaz místo hello `-d "{ template and parameters}"`:
>
> `--data-binary "@/path/to/file.json"`

Pokud tento požadavek je úspěšné, obdrží odpověď 200 řady a hello odpovědi obsahuje dokument JSON obsahující informace o operaci nasazení hello.

> [!IMPORTANT]
> nasazení Hello byla odeslána, ale nebyl dokončen. Může trvat několik minut, obvykle přibližně 15 pro nasazení toocomplete hello.

## <a name="check-hello-status-of-a-deployment"></a>Zkontrolujte stav hello nasazení

Stav hello toocheck hello nasazení, hello použijte následující příkaz:

```bash
curl -X "GET" "https://management.azure.com/subscriptions/$SUBSCRIPTIONID/resourcegroups/$RESOURCEGROUPNAME/providers/microsoft.resources/deployments/$DEPLOYMENTNAME?api-version=2015-01-01" \
-H "Authorization: Bearer $ACCESSTOKEN" \
-H "Content-Type: application/json"
```

Tento příkaz vrátí dokumentu JSON obsahující informace o operaci nasazení hello. Hello `"provisioningState"` element obsahuje hello stav nasazení hello. Pokud tento prvek obsahuje hodnotu `"Succeeded"`, pak nasazení hello byla úspěšně dokončena.

## <a name="troubleshoot"></a>Řešení potíží

Pokud narazíte na problémy s vytvářením clusterů HDInsight, podívejte se na [požadavky na řízení přístupu](hdinsight-administer-use-portal-linux.md#create-clusters).

## <a name="next-steps"></a>Další kroky

Teď, když jste úspěšně vytvořili clusteru služby HDInsight, použijte následující toolearn jak hello toowork k vašemu clusteru.

### <a name="hadoop-clusters"></a>Clustery Hadoop

* [Použití Hivu se službou HDInsight](hdinsight-use-hive.md)
* [Použití Pigu se službou HDInsight](hdinsight-use-pig.md)
* [Používání nástroje MapReduce s HDInsight](hdinsight-use-mapreduce.md)

### <a name="hbase-clusters"></a>Clustery HBase

* [Začínáme s HBase v HDInsight](hdinsight-hbase-tutorial-get-started-linux.md)
* [Vývoj aplikací v jazyce Java pro HBase v HDInsight](hdinsight-hbase-build-java-maven-linux.md)

### <a name="storm-clusters"></a>Clustery Storm

* [Vývoj topologie Java pro Storm v HDInsight](hdinsight-storm-develop-java-topology.md)
* [Použití komponent, Python v Storm v HDInsight](hdinsight-storm-develop-python-topology.md)
* [Nasazení a monitorování topologie se Storm v HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md)
