---
title: "Vytváření clusterů systému Hadoop pomocí rozhraní API REST Azure - Azure | Microsoft Docs"
description: "Naučte se vytvářet clustery HDInsight odesláním šablon Azure Resource Manageru do rozhraní API REST Azure."
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
ms.openlocfilehash: a36a41c231472ceeeb46d02ddb65549b1c79728a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="create-hadoop-clusters-using-the-azure-rest-api"></a><span data-ttu-id="05b18-103">Vytváření clusterů systému Hadoop pomocí REST API služby Azure</span><span class="sxs-lookup"><span data-stu-id="05b18-103">Create Hadoop clusters using the Azure REST API</span></span>

[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

<span data-ttu-id="05b18-104">Informace o vytváření clusteru HDInsight pomocí šablony Azure Resource Manager a REST API služby Azure.</span><span class="sxs-lookup"><span data-stu-id="05b18-104">Learn how to create an HDInsight cluster using an Azure Resource Manager template and the Azure REST API.</span></span>

<span data-ttu-id="05b18-105">Rozhraní REST API Azure umožňuje provádět operace správy na služby hostované v Azure platformy, včetně vytváření nových prostředků, jako je například clusterů HDInsight.</span><span class="sxs-lookup"><span data-stu-id="05b18-105">The Azure REST API allows you to perform management operations on services hosted in the Azure platform, including the creation of new resources such as HDInsight clusters.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="05b18-106">HDInsight od verze 3.4 výše používá výhradně operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="05b18-106">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="05b18-107">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="05b18-107">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

> [!NOTE]
> <span data-ttu-id="05b18-108">Kroky v tomto dokumentu pomocí [curl (https://curl.haxx.se/)](https://curl.haxx.se/) nástroj ke komunikaci s REST API služby Azure.</span><span class="sxs-lookup"><span data-stu-id="05b18-108">The steps in this document use the [curl (https://curl.haxx.se/)](https://curl.haxx.se/) utility to communicate with the Azure REST API.</span></span>

## <a name="create-a-template"></a><span data-ttu-id="05b18-109">Vytvoření šablony</span><span class="sxs-lookup"><span data-stu-id="05b18-109">Create a template</span></span>

<span data-ttu-id="05b18-110">Šablony Azure Resource Manageru jsou dokumenty JSON, které popisují **skupiny prostředků** a všechny prostředky v ní (například HDInsight.) Tento přístup na základě šablon můžete zadat prostředky, které potřebujete pro HDInsight v jedné šabloně.</span><span class="sxs-lookup"><span data-stu-id="05b18-110">Azure Resource Manager templates are JSON documents that describe a **resource group** and all resources in it (such as HDInsight.) This template-based approach allows you to define the resources that you need for HDInsight in one template.</span></span>

<span data-ttu-id="05b18-111">V následujícím dokumentu JSON je fúze ze souborů šablony a parametry [https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password](https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password), která vytvoří cluster se systémem Linux pomocí hesla pro zabezpečení účtu uživatele SSH.</span><span class="sxs-lookup"><span data-stu-id="05b18-111">The following JSON document is a merger of the template and parameters files from [https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password](https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password), which creates a Linux-based cluster using a password to secure the SSH user account.</span></span>

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
                           "description": "The type of the HDInsight cluster to create."
                       }
                   },
                   "clusterName": {
                       "type": "string",
                       "metadata": {
                           "description": "The name of the HDInsight cluster to create."
                       }
                   },
                   "clusterLoginUserName": {
                       "type": "string",
                       "metadata": {
                           "description": "These credentials can be used to submit jobs to the cluster and to log into cluster dashboards."
                       }
                   },
                   "clusterLoginPassword": {
                       "type": "securestring",
                       "metadata": {
                           "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
                       }
                   },
                   "sshUserName": {
                       "type": "string",
                       "metadata": {
                           "description": "These credentials can be used to remotely access the cluster."
                       }
                   },
                   "sshPassword": {
                       "type": "securestring",
                       "metadata": {
                           "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
                       }
                   },
                   "clusterStorageAccountName": {
                       "type": "string",
                       "metadata": {
                           "description": "The name of the storage account to be created and be used as the cluster's storage."
                       }
                   },
                   "clusterWorkerNodeCount": {
                       "type": "int",
                       "defaultValue": 4,
                       "metadata": {
                           "description": "The number of nodes in the HDInsight cluster."
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

<span data-ttu-id="05b18-112">V tomto příkladu se používá v kroky v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="05b18-112">This example is used in the steps in this document.</span></span> <span data-ttu-id="05b18-113">Nahradit příklad *hodnoty* v **parametry** část s hodnotami pro váš cluster.</span><span class="sxs-lookup"><span data-stu-id="05b18-113">Replace the example *values* in the **Parameters** section with the values for your cluster.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="05b18-114">Šablona používá výchozí počet uzlů pracovního procesu (4) pro cluster služby HDInsight.</span><span class="sxs-lookup"><span data-stu-id="05b18-114">The template uses the default number of worker nodes (4) for an HDInsight cluster.</span></span> <span data-ttu-id="05b18-115">Pokud máte v plánu na víc než 32 uzlů pracovního procesu, je nutné vybrat velikost hlavního uzlu s alespoň s 8 jádry a 14 GB paměti ram.</span><span class="sxs-lookup"><span data-stu-id="05b18-115">If you plan on more than 32 worker nodes, then you must select a head node size with at least 8 cores and 14 GB ram.</span></span>
>
> <span data-ttu-id="05b18-116">Další informace o velikosti uzlu a souvisejících nákladů, najdete v části [HDInsight ceny](https://azure.microsoft.com/pricing/details/hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="05b18-116">For more information on node sizes and associated costs, see [HDInsight pricing](https://azure.microsoft.com/pricing/details/hdinsight/).</span></span>

## <a name="log-in-to-your-azure-subscription"></a><span data-ttu-id="05b18-117">Přihlášení k předplatnému Azure</span><span class="sxs-lookup"><span data-stu-id="05b18-117">Log in to your Azure subscription</span></span>

<span data-ttu-id="05b18-118">Postupujte podle kroků popsaných v [Začínáme s Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2) a připojte se k předplatnému pomocí `az login` příkaz.</span><span class="sxs-lookup"><span data-stu-id="05b18-118">Follow the steps documented in [Get started with Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2) and connect to your subscription using the `az login` command.</span></span>

## <a name="create-a-service-principal"></a><span data-ttu-id="05b18-119">Vytvoření instančního objektu</span><span class="sxs-lookup"><span data-stu-id="05b18-119">Create a service principal</span></span>

> [!NOTE]
> <span data-ttu-id="05b18-120">Tyto kroky nejsou stručné verzi *vytvoření instančního objektu s heslem* části [použití Azure CLI pro vytvoření objektu služby pro přístup k prostředkům](../azure-resource-manager/resource-group-authenticate-service-principal-cli.md#create-service-principal-with-password) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="05b18-120">These steps are an abridged version of the *Create service principal with password* section of the [Use Azure CLI to create a service principal to access resources](../azure-resource-manager/resource-group-authenticate-service-principal-cli.md#create-service-principal-with-password) document.</span></span> <span data-ttu-id="05b18-121">Tyto kroky vytvořit objekt služby, který se používá k ověření na rozhraní REST API Azure.</span><span class="sxs-lookup"><span data-stu-id="05b18-121">These steps create a service principal that is used to authenticate to the Azure REST API.</span></span>

1. <span data-ttu-id="05b18-122">Z příkazového řádku použijte následující příkaz k zobrazení seznamu vašich předplatných Azure.</span><span class="sxs-lookup"><span data-stu-id="05b18-122">From a command line, use the following command to list your Azure subscriptions.</span></span>

   ```bash
   az account list --query '[].{Subscription_ID:id,Tenant_ID:tenantId,Name:name}'  --output table
   ```

    <span data-ttu-id="05b18-123">V seznamu vyberte předplatné, který chcete použít a poznamenejte si **ID_ODBĚRU** a __Tenant_ID__ sloupce.</span><span class="sxs-lookup"><span data-stu-id="05b18-123">In the list, select the subscription that you want to use and note the **Subscription_ID** and __Tenant_ID__ columns.</span></span> <span data-ttu-id="05b18-124">Uložte tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="05b18-124">Save these values.</span></span>

2. <span data-ttu-id="05b18-125">Použijte následující příkaz k vytvoření aplikace v Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="05b18-125">Use the following command to create an application in Azure Active Directory.</span></span>

   ```bash
   az ad app create --display-name "exampleapp" --homepage "https://www.contoso.org" --identifier-uris "https://www.contoso.org/example" --password <Your password> --query 'appId'
   ```

    <span data-ttu-id="05b18-126">Nahraďte hodnoty `--display-name`, `--homepage`, a `--identifier-uris` vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="05b18-126">Replace the values for the `--display-name`, `--homepage`, and `--identifier-uris` with your own values.</span></span> <span data-ttu-id="05b18-127">Zadejte heslo pro nový záznam služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="05b18-127">Provide a password for the new Active Directory entry.</span></span>

   > [!NOTE]
   > <span data-ttu-id="05b18-128">`--home-page` a `--identifier-uris` hodnoty nemusíte odkazovat na vlastní webové stránky hostované na Internetu.</span><span class="sxs-lookup"><span data-stu-id="05b18-128">The `--home-page` and `--identifier-uris` values don't need to reference an actual web page hosted on the internet.</span></span> <span data-ttu-id="05b18-129">Musí být jedinečné identifikátory URI.</span><span class="sxs-lookup"><span data-stu-id="05b18-129">They must be unique URIs.</span></span>

   <span data-ttu-id="05b18-130">Hodnota vrácená z tohoto příkazu je __ID aplikace__ pro novou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="05b18-130">The value returned from this command is the __App ID__ for the new application.</span></span> <span data-ttu-id="05b18-131">Uložte tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="05b18-131">Save this value.</span></span>

3. <span data-ttu-id="05b18-132">Použijte následující příkaz k vytvoření objektu služby pomocí **ID aplikace**.</span><span class="sxs-lookup"><span data-stu-id="05b18-132">Use the following command to create a service principal using the **App ID**.</span></span>

   ```bash
   az ad sp create --id <App ID> --query 'objectId'
   ```

     <span data-ttu-id="05b18-133">Hodnota vrácená z tohoto příkazu je __ID objektu__.</span><span class="sxs-lookup"><span data-stu-id="05b18-133">The value returned from this command is the __Object ID__.</span></span> <span data-ttu-id="05b18-134">Uložte tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="05b18-134">Save this value.</span></span>

4. <span data-ttu-id="05b18-135">Přiřazení **vlastníka** role hlavní služby pomocí **ID objektu** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="05b18-135">Assign the **Owner** role to the service principal using the **Object ID** value.</span></span> <span data-ttu-id="05b18-136">Použití **ID předplatného** jste dříve získali.</span><span class="sxs-lookup"><span data-stu-id="05b18-136">Use the **subscription ID** you obtained earlier.</span></span>

   ```bash
   az role assignment create --assignee <Object ID> --role Owner --scope /subscriptions/<Subscription ID>/
   ```

## <a name="get-an-authentication-token"></a><span data-ttu-id="05b18-137">Získat ověřovací token</span><span class="sxs-lookup"><span data-stu-id="05b18-137">Get an authentication token</span></span>

<span data-ttu-id="05b18-138">Chcete-li získat ověřovací token, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="05b18-138">Use the following command to retrieve an authentication token:</span></span>

```bash
curl -X "POST" "https://login.microsoftonline.com/$TENANTID/oauth2/token" \
-H "Cookie: flight-uxoptin=true; stsservicecookie=ests; x-ms-gateway-slice=productionb; stsservicecookie=ests" \
-H "Content-Type: application/x-www-form-urlencoded" \
--data-urlencode "client_id=$APPID" \
--data-urlencode "grant_type=client_credentials" \
--data-urlencode "client_secret=$PASSWORD" \
--data-urlencode "resource=https://management.azure.com/"
```

<span data-ttu-id="05b18-139">Nastavit `$TENANTID`, `$APPID`, a `$PASSWORD` na hodnoty získané nebo použít dříve.</span><span class="sxs-lookup"><span data-stu-id="05b18-139">Set `$TENANTID`, `$APPID`, and `$PASSWORD` to the values obtained or used previously.</span></span>

<span data-ttu-id="05b18-140">Pokud tento požadavek je úspěšné, obdrží odpověď 200 řady a text odpovědi obsahuje dokument JSON.</span><span class="sxs-lookup"><span data-stu-id="05b18-140">If this request is successful, you receive a 200 series response and the response body contains a JSON document.</span></span>

<span data-ttu-id="05b18-141">Dokument JSON vrácený tuto žádost obsahuje element s názvem **access_token**.</span><span class="sxs-lookup"><span data-stu-id="05b18-141">The JSON document returned by this request contains an element named **access_token**.</span></span> <span data-ttu-id="05b18-142">Hodnota **access_token** slouží k žádosti o ověření rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="05b18-142">The value of **access_token** is used to authentication requests to the REST API.</span></span>

```json
{
    "token_type":"Bearer",
    "expires_in":"3599",
    "expires_on":"1463409994",
    "not_before":"1463406094",
    "resource":"https://management.azure.com/","access_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWoNBVGZNNXBPWWlKSE1iYTlnb0VLWSIsImtpZCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSJ9.eyJhdWQiOiJodHRwczovL21hbmFnZW1lbnQuYXp1cmUuY29tLyIsImlzcyI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI2Ny8iLCJpYXQiOjE0NjM0MDYwOTQsIm5iZiI6MTQ2MzQwNjA5NCwiZXhwIjoxNDYzNDA5OTk5LCJhcHBpZCI6IjBlYzcyMzM0LTZkMDMtNDhmYi04OWU1LTU2NTJiODBiZDliYiIsImFwcGlkYWNyIjoiMSIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI0Ny8iLCJvaWQiOiJlNjgxZTZiMi1mZThkLTRkZGUtYjZiMS0xNjAyZDQyNWQzOWYiLCJzdWIiOiJlNjgxZTZiMi1mZThkLTRkZGUtYjZiMS0xNjAyZDQyNWQzOWYiLCJ0aWQiOiI3MmY5ODhiZi04NmYxLTQxYWYtOTFhYi0yZDdjZDAxMWRiNDciLCJ2ZXIiOiIxLjAifQ.nJVERbeDHLGHn7ZsbVGBJyHOu2PYhG5dji6F63gu8XN2Cvol3J1HO1uB4H3nCSt9DTu_jMHqAur_NNyobgNM21GojbEZAvd0I9NY0UDumBEvDZfMKneqp7a_cgAU7IYRcTPneSxbD6wo-8gIgfN9KDql98b0uEzixIVIWra2Q1bUUYETYqyaJNdS4RUmlJKNNpENllAyHQLv7hXnap1IuzP-f5CNIbbj9UgXxLiOtW5JhUAwWLZ3-WMhNRpUO2SIB7W7tQ0AbjXw3aUYr7el066J51z5tC1AK9UC-mD_fO_HUP6ZmPzu5gLA6DxkIIYP3grPnRVoUDltHQvwgONDOw"
}
```

## <a name="create-a-resource-group"></a><span data-ttu-id="05b18-143">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="05b18-143">Create a resource group</span></span>

<span data-ttu-id="05b18-144">Následující informace vám pomůžou vytvořit skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="05b18-144">Use the following to create a resource group.</span></span>

* <span data-ttu-id="05b18-145">Nastavit `$SUBSCRIPTIONID` k předplatnému ID přijal při vytváření objektu služby.</span><span class="sxs-lookup"><span data-stu-id="05b18-145">Set `$SUBSCRIPTIONID` to the subscription ID received while creating the service principal.</span></span>
* <span data-ttu-id="05b18-146">Nastavit `$ACCESSTOKEN` do tokenu přístupu přijaté v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="05b18-146">Set `$ACCESSTOKEN` to the access token received in the previous step.</span></span>
* <span data-ttu-id="05b18-147">Nahraďte `DATACENTERLOCATION` s Chcete vytvořit skupinu prostředků a prostředky v datovém centru.</span><span class="sxs-lookup"><span data-stu-id="05b18-147">Replace `DATACENTERLOCATION` with the data center you wish to create the resource group, and resources, in.</span></span> <span data-ttu-id="05b18-148">Například 'jihu USA".</span><span class="sxs-lookup"><span data-stu-id="05b18-148">For example, 'South Central US'.</span></span>
* <span data-ttu-id="05b18-149">Nastavit `$RESOURCEGROUPNAME` na název, který chcete použít pro tuto skupinu:</span><span class="sxs-lookup"><span data-stu-id="05b18-149">Set `$RESOURCEGROUPNAME` to the name you wish to use for this group:</span></span>

```bash
curl -X "PUT" "https://management.azure.com/subscriptions/$SUBSCRIPTIONID/resourcegroups/$RESOURCEGROUPNAME?api-version=2015-01-01" \
    -H "Authorization: Bearer $ACCESSTOKEN" \
    -H "Content-Type: application/json" \
    -d $'{
"location": "DATACENTERLOCATION"
}'
```

<span data-ttu-id="05b18-150">Pokud tento požadavek je úspěšné, obdrží odpověď 200 řady a text odpovědi obsahuje dokument JSON obsahující informace o této skupině.</span><span class="sxs-lookup"><span data-stu-id="05b18-150">If this request is successful, you receive a 200 series response and the response body contains a JSON document containing information about the group.</span></span> <span data-ttu-id="05b18-151">`"provisioningState"` Element obsahuje hodnotu `"Succeeded"`.</span><span class="sxs-lookup"><span data-stu-id="05b18-151">The `"provisioningState"` element contains a value of `"Succeeded"`.</span></span>

## <a name="create-a-deployment"></a><span data-ttu-id="05b18-152">Vytvoření nasazení</span><span class="sxs-lookup"><span data-stu-id="05b18-152">Create a deployment</span></span>

<span data-ttu-id="05b18-153">Použijte následující příkaz k nasazení šablony do skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="05b18-153">Use the following command to deploy the template to the resource group.</span></span>

* <span data-ttu-id="05b18-154">Nastavit `$DEPLOYMENTNAME` na název, který chcete použít pro toto nasazení.</span><span class="sxs-lookup"><span data-stu-id="05b18-154">Set `$DEPLOYMENTNAME` to the name you wish to use for this deployment.</span></span>

```bash
curl -X "PUT" "https://management.azure.com/subscriptions/$SUBSCRIPTIONID/resourcegroups/$RESOURCEGROUPNAME/providers/microsoft.resources/deployments/$DEPLOYMENTNAME?api-version=2015-01-01" \
-H "Authorization: Bearer $ACCESSTOKEN" \
-H "Content-Type: application/json" \
-d "{set your body string to the template and parameters}"
```

> [!NOTE]
> <span data-ttu-id="05b18-155">Pokud jste uložili soubor šablony, můžete použít následující příkaz místo `-d "{ template and parameters}"`:</span><span class="sxs-lookup"><span data-stu-id="05b18-155">If you saved the template to a file, you can use the following command instead of `-d "{ template and parameters}"`:</span></span>
>
> `--data-binary "@/path/to/file.json"`

<span data-ttu-id="05b18-156">Pokud tento požadavek je úspěšné, obdrží odpověď 200 řady a text odpovědi obsahuje dokument JSON obsahující informace o operaci nasazení.</span><span class="sxs-lookup"><span data-stu-id="05b18-156">If this request is successful, you receive a 200 series response and the response body contains a JSON document containing information about the deployment operation.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="05b18-157">Nasazení byla odeslána, ale nebyl dokončen.</span><span class="sxs-lookup"><span data-stu-id="05b18-157">The deployment has been submitted, but has not completed.</span></span> <span data-ttu-id="05b18-158">Může trvat několik minut, obvykle přibližně 15, pro dokončení nasazení.</span><span class="sxs-lookup"><span data-stu-id="05b18-158">It can take several minutes, usually around 15, for the deployment to complete.</span></span>

## <a name="check-the-status-of-a-deployment"></a><span data-ttu-id="05b18-159">Zkontrolujte stav nasazení</span><span class="sxs-lookup"><span data-stu-id="05b18-159">Check the status of a deployment</span></span>

<span data-ttu-id="05b18-160">Pokud chcete zkontrolovat stav nasazení, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="05b18-160">To check the status of the deployment, use the following command:</span></span>

```bash
curl -X "GET" "https://management.azure.com/subscriptions/$SUBSCRIPTIONID/resourcegroups/$RESOURCEGROUPNAME/providers/microsoft.resources/deployments/$DEPLOYMENTNAME?api-version=2015-01-01" \
-H "Authorization: Bearer $ACCESSTOKEN" \
-H "Content-Type: application/json"
```

<span data-ttu-id="05b18-161">Tento příkaz vrátí dokumentu JSON obsahující informace o operaci nasazení.</span><span class="sxs-lookup"><span data-stu-id="05b18-161">This command returns a JSON document containing information about the deployment operation.</span></span> <span data-ttu-id="05b18-162">`"provisioningState"` Element obsahuje stav nasazení.</span><span class="sxs-lookup"><span data-stu-id="05b18-162">The `"provisioningState"` element contains the status of the deployment.</span></span> <span data-ttu-id="05b18-163">Pokud tento prvek obsahuje hodnotu `"Succeeded"`, pak nasazení bylo úspěšně dokončeno.</span><span class="sxs-lookup"><span data-stu-id="05b18-163">If this element contains a value of `"Succeeded"`, then the deployment has completed successfully.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="05b18-164">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="05b18-164">Troubleshoot</span></span>

<span data-ttu-id="05b18-165">Pokud narazíte na problémy s vytvářením clusterů HDInsight, podívejte se na [požadavky na řízení přístupu](hdinsight-administer-use-portal-linux.md#create-clusters).</span><span class="sxs-lookup"><span data-stu-id="05b18-165">If you run into issues with creating HDInsight clusters, see [access control requirements](hdinsight-administer-use-portal-linux.md#create-clusters).</span></span>

## <a name="next-steps"></a><span data-ttu-id="05b18-166">Další kroky</span><span class="sxs-lookup"><span data-stu-id="05b18-166">Next steps</span></span>

<span data-ttu-id="05b18-167">Teď, když jste úspěšně vytvořili clusteru služby HDInsight, použijte následující informace o práci s vašeho clusteru.</span><span class="sxs-lookup"><span data-stu-id="05b18-167">Now that you have successfully created an HDInsight cluster, use the following to learn how to work with your cluster.</span></span>

### <a name="hadoop-clusters"></a><span data-ttu-id="05b18-168">Clustery Hadoop</span><span class="sxs-lookup"><span data-stu-id="05b18-168">Hadoop clusters</span></span>

* [<span data-ttu-id="05b18-169">Použití Hivu se službou HDInsight</span><span class="sxs-lookup"><span data-stu-id="05b18-169">Use Hive with HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="05b18-170">Použití Pigu se službou HDInsight</span><span class="sxs-lookup"><span data-stu-id="05b18-170">Use Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="05b18-171">Používání nástroje MapReduce s HDInsight</span><span class="sxs-lookup"><span data-stu-id="05b18-171">Use MapReduce with HDInsight</span></span>](hdinsight-use-mapreduce.md)

### <a name="hbase-clusters"></a><span data-ttu-id="05b18-172">Clustery HBase</span><span class="sxs-lookup"><span data-stu-id="05b18-172">HBase clusters</span></span>

* [<span data-ttu-id="05b18-173">Začínáme s HBase v HDInsight</span><span class="sxs-lookup"><span data-stu-id="05b18-173">Get started with HBase on HDInsight</span></span>](hdinsight-hbase-tutorial-get-started-linux.md)
* [<span data-ttu-id="05b18-174">Vývoj aplikací v jazyce Java pro HBase v HDInsight</span><span class="sxs-lookup"><span data-stu-id="05b18-174">Develop Java applications for HBase on HDInsight</span></span>](hdinsight-hbase-build-java-maven-linux.md)

### <a name="storm-clusters"></a><span data-ttu-id="05b18-175">Clustery Storm</span><span class="sxs-lookup"><span data-stu-id="05b18-175">Storm clusters</span></span>

* [<span data-ttu-id="05b18-176">Vývoj topologie Java pro Storm v HDInsight</span><span class="sxs-lookup"><span data-stu-id="05b18-176">Develop Java topologies for Storm on HDInsight</span></span>](hdinsight-storm-develop-java-topology.md)
* [<span data-ttu-id="05b18-177">Použití komponent, Python v Storm v HDInsight</span><span class="sxs-lookup"><span data-stu-id="05b18-177">Use Python components in Storm on HDInsight</span></span>](hdinsight-storm-develop-python-topology.md)
* [<span data-ttu-id="05b18-178">Nasazení a monitorování topologie se Storm v HDInsight</span><span class="sxs-lookup"><span data-stu-id="05b18-178">Deploy and monitor topologies with Storm on HDInsight</span></span>](hdinsight-storm-deploy-monitor-topology-linux.md)
