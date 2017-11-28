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
# <a name="create-hadoop-clusters-using-hello-azure-rest-api"></a><span data-ttu-id="a2648-103">Vytváření clusterů systému Hadoop pomocí hello REST API služby Azure</span><span class="sxs-lookup"><span data-stu-id="a2648-103">Create Hadoop clusters using hello Azure REST API</span></span>

[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

<span data-ttu-id="a2648-104">Zjistěte, jak toocreate HDInsight clusteru pomocí šablony Azure Resource Manager a hello rozhraní REST API Azure.</span><span class="sxs-lookup"><span data-stu-id="a2648-104">Learn how toocreate an HDInsight cluster using an Azure Resource Manager template and hello Azure REST API.</span></span>

<span data-ttu-id="a2648-105">Hello rozhraní REST API Azure vám umožní operace správy tooperform na služby hostované v hello platformy Azure, včetně hello vytváření nových prostředků, jako je například clusterů HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a2648-105">hello Azure REST API allows you tooperform management operations on services hosted in hello Azure platform, including hello creation of new resources such as HDInsight clusters.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a2648-106">Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="a2648-106">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="a2648-107">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="a2648-107">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

> [!NOTE]
> <span data-ttu-id="a2648-108">Hello kroky hello použití tohoto dokumentu [curl (https://curl.haxx.se/)](https://curl.haxx.se/) toocommunicate nástroj s hello rozhraní REST API Azure.</span><span class="sxs-lookup"><span data-stu-id="a2648-108">hello steps in this document use hello [curl (https://curl.haxx.se/)](https://curl.haxx.se/) utility toocommunicate with hello Azure REST API.</span></span>

## <a name="create-a-template"></a><span data-ttu-id="a2648-109">Vytvoření šablony</span><span class="sxs-lookup"><span data-stu-id="a2648-109">Create a template</span></span>

<span data-ttu-id="a2648-110">Šablony Azure Resource Manageru jsou dokumenty JSON, které popisují **skupiny prostředků** a všechny prostředky v ní (například HDInsight.) Tento přístup na základě šablon můžete toodefine hello prostředky, které potřebujete pro HDInsight v jedné šabloně.</span><span class="sxs-lookup"><span data-stu-id="a2648-110">Azure Resource Manager templates are JSON documents that describe a **resource group** and all resources in it (such as HDInsight.) This template-based approach allows you toodefine hello resources that you need for HDInsight in one template.</span></span>

<span data-ttu-id="a2648-111">Hello následující dokumentu JSON je fúzi hello šablony a parametry soubory z [https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password](https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password), která vytvoří systémem Linux cluster pomocí hello toosecure a heslo SSH uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="a2648-111">hello following JSON document is a merger of hello template and parameters files from [https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password](https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password), which creates a Linux-based cluster using a password toosecure hello SSH user account.</span></span>

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

<span data-ttu-id="a2648-112">V tomto příkladu se používá v hello kroky v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="a2648-112">This example is used in hello steps in this document.</span></span> <span data-ttu-id="a2648-113">Nahraďte hello příklad *hodnoty* v hello **parametry** oddíl s hello hodnoty pro váš cluster.</span><span class="sxs-lookup"><span data-stu-id="a2648-113">Replace hello example *values* in hello **Parameters** section with hello values for your cluster.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a2648-114">Šablona Hello používá hello výchozí počet uzlů pracovního procesu (4) pro cluster služby HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a2648-114">hello template uses hello default number of worker nodes (4) for an HDInsight cluster.</span></span> <span data-ttu-id="a2648-115">Pokud máte v plánu na víc než 32 uzlů pracovního procesu, je nutné vybrat velikost hlavního uzlu s alespoň s 8 jádry a 14 GB paměti ram.</span><span class="sxs-lookup"><span data-stu-id="a2648-115">If you plan on more than 32 worker nodes, then you must select a head node size with at least 8 cores and 14 GB ram.</span></span>
>
> <span data-ttu-id="a2648-116">Další informace o velikosti uzlu a souvisejících nákladů, najdete v části [HDInsight ceny](https://azure.microsoft.com/pricing/details/hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="a2648-116">For more information on node sizes and associated costs, see [HDInsight pricing](https://azure.microsoft.com/pricing/details/hdinsight/).</span></span>

## <a name="log-in-tooyour-azure-subscription"></a><span data-ttu-id="a2648-117">Přihlaste se tooyour předplatného Azure</span><span class="sxs-lookup"><span data-stu-id="a2648-117">Log in tooyour Azure subscription</span></span>

<span data-ttu-id="a2648-118">Postupujte podle kroků hello popsaných v [Začínáme s Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2) a připojte se pomocí hello předplatné tooyour `az login` příkaz.</span><span class="sxs-lookup"><span data-stu-id="a2648-118">Follow hello steps documented in [Get started with Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2) and connect tooyour subscription using hello `az login` command.</span></span>

## <a name="create-a-service-principal"></a><span data-ttu-id="a2648-119">Vytvoření instančního objektu</span><span class="sxs-lookup"><span data-stu-id="a2648-119">Create a service principal</span></span>

> [!NOTE]
> <span data-ttu-id="a2648-120">Tyto kroky nejsou stručné verzi hello *vytvoření instančního objektu s heslem* části hello [použití Azure CLI toocreate objekt služby prostředků tooaccess](../azure-resource-manager/resource-group-authenticate-service-principal-cli.md#create-service-principal-with-password) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="a2648-120">These steps are an abridged version of hello *Create service principal with password* section of hello [Use Azure CLI toocreate a service principal tooaccess resources](../azure-resource-manager/resource-group-authenticate-service-principal-cli.md#create-service-principal-with-password) document.</span></span> <span data-ttu-id="a2648-121">Tyto kroky vytvoření instančního objektu, který je použité tooauthenticate toohello REST API služby Azure.</span><span class="sxs-lookup"><span data-stu-id="a2648-121">These steps create a service principal that is used tooauthenticate toohello Azure REST API.</span></span>

1. <span data-ttu-id="a2648-122">Z příkazového řádku použijte následující příkaz toolist hello předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="a2648-122">From a command line, use hello following command toolist your Azure subscriptions.</span></span>

   ```bash
   az account list --query '[].{Subscription_ID:id,Tenant_ID:tenantId,Name:name}'  --output table
   ```

    <span data-ttu-id="a2648-123">V seznamu hello, vyberte předplatné hello má toouse a Všimněte si hello **ID_ODBĚRU** a __Tenant_ID__ sloupce.</span><span class="sxs-lookup"><span data-stu-id="a2648-123">In hello list, select hello subscription that you want toouse and note hello **Subscription_ID** and __Tenant_ID__ columns.</span></span> <span data-ttu-id="a2648-124">Uložte tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="a2648-124">Save these values.</span></span>

2. <span data-ttu-id="a2648-125">Použijte následující příkaz toocreate aplikace v Azure Active Directory hello.</span><span class="sxs-lookup"><span data-stu-id="a2648-125">Use hello following command toocreate an application in Azure Active Directory.</span></span>

   ```bash
   az ad app create --display-name "exampleapp" --homepage "https://www.contoso.org" --identifier-uris "https://www.contoso.org/example" --password <Your password> --query 'appId'
   ```

    <span data-ttu-id="a2648-126">Nahraďte hodnoty hello hello `--display-name`, `--homepage`, a `--identifier-uris` vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="a2648-126">Replace hello values for hello `--display-name`, `--homepage`, and `--identifier-uris` with your own values.</span></span> <span data-ttu-id="a2648-127">Zadejte heslo pro nový záznam služby Active Directory hello.</span><span class="sxs-lookup"><span data-stu-id="a2648-127">Provide a password for hello new Active Directory entry.</span></span>

   > [!NOTE]
   > <span data-ttu-id="a2648-128">Hello `--home-page` a `--identifier-uris` hodnoty nepotřebujete tooreference vlastní webové stránky hostované na hello Internetu.</span><span class="sxs-lookup"><span data-stu-id="a2648-128">hello `--home-page` and `--identifier-uris` values don't need tooreference an actual web page hosted on hello internet.</span></span> <span data-ttu-id="a2648-129">Musí být jedinečné identifikátory URI.</span><span class="sxs-lookup"><span data-stu-id="a2648-129">They must be unique URIs.</span></span>

   <span data-ttu-id="a2648-130">Hello hodnota vrácená z tohoto příkazu je hello __ID aplikace__ pro novou aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="a2648-130">hello value returned from this command is hello __App ID__ for hello new application.</span></span> <span data-ttu-id="a2648-131">Uložte tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="a2648-131">Save this value.</span></span>

3. <span data-ttu-id="a2648-132">Použití hello následující příkaz toocreate hlavní název služby pomocí hello **ID aplikace**.</span><span class="sxs-lookup"><span data-stu-id="a2648-132">Use hello following command toocreate a service principal using hello **App ID**.</span></span>

   ```bash
   az ad sp create --id <App ID> --query 'objectId'
   ```

     <span data-ttu-id="a2648-133">Hello hodnota vrácená z tohoto příkazu je hello __ID objektu__.</span><span class="sxs-lookup"><span data-stu-id="a2648-133">hello value returned from this command is hello __Object ID__.</span></span> <span data-ttu-id="a2648-134">Uložte tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="a2648-134">Save this value.</span></span>

4. <span data-ttu-id="a2648-135">Přiřadit hello **vlastníka** role toohello službu objektu zabezpečení pomocí hello **ID objektu** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="a2648-135">Assign hello **Owner** role toohello service principal using hello **Object ID** value.</span></span> <span data-ttu-id="a2648-136">Použití hello **ID předplatného** jste dříve získali.</span><span class="sxs-lookup"><span data-stu-id="a2648-136">Use hello **subscription ID** you obtained earlier.</span></span>

   ```bash
   az role assignment create --assignee <Object ID> --role Owner --scope /subscriptions/<Subscription ID>/
   ```

## <a name="get-an-authentication-token"></a><span data-ttu-id="a2648-137">Získat ověřovací token</span><span class="sxs-lookup"><span data-stu-id="a2648-137">Get an authentication token</span></span>

<span data-ttu-id="a2648-138">Použijte následující příkaz tooretrieve ověřovací token hello:</span><span class="sxs-lookup"><span data-stu-id="a2648-138">Use hello following command tooretrieve an authentication token:</span></span>

```bash
curl -X "POST" "https://login.microsoftonline.com/$TENANTID/oauth2/token" \
-H "Cookie: flight-uxoptin=true; stsservicecookie=ests; x-ms-gateway-slice=productionb; stsservicecookie=ests" \
-H "Content-Type: application/x-www-form-urlencoded" \
--data-urlencode "client_id=$APPID" \
--data-urlencode "grant_type=client_credentials" \
--data-urlencode "client_secret=$PASSWORD" \
--data-urlencode "resource=https://management.azure.com/"
```

<span data-ttu-id="a2648-139">Nastavit `$TENANTID`, `$APPID`, a `$PASSWORD` toohello hodnoty získané nebo použít dříve.</span><span class="sxs-lookup"><span data-stu-id="a2648-139">Set `$TENANTID`, `$APPID`, and `$PASSWORD` toohello values obtained or used previously.</span></span>

<span data-ttu-id="a2648-140">Pokud tento požadavek je úspěšné, obdrží odpověď 200 řady a hello odpovědi obsahuje dokument JSON.</span><span class="sxs-lookup"><span data-stu-id="a2648-140">If this request is successful, you receive a 200 series response and hello response body contains a JSON document.</span></span>

<span data-ttu-id="a2648-141">Hello dokumentu JSON vrácený tuto žádost obsahuje element s názvem **access_token**.</span><span class="sxs-lookup"><span data-stu-id="a2648-141">hello JSON document returned by this request contains an element named **access_token**.</span></span> <span data-ttu-id="a2648-142">Hello hodnotu **access_token** je použité tooauthentication požadavky toohello REST API.</span><span class="sxs-lookup"><span data-stu-id="a2648-142">hello value of **access_token** is used tooauthentication requests toohello REST API.</span></span>

```json
{
    "token_type":"Bearer",
    "expires_in":"3599",
    "expires_on":"1463409994",
    "not_before":"1463406094",
    "resource":"https://management.azure.com/","access_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWoNBVGZNNXBPWWlKSE1iYTlnb0VLWSIsImtpZCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSJ9.eyJhdWQiOiJodHRwczovL21hbmFnZW1lbnQuYXp1cmUuY29tLyIsImlzcyI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI2Ny8iLCJpYXQiOjE0NjM0MDYwOTQsIm5iZiI6MTQ2MzQwNjA5NCwiZXhwIjoxNDYzNDA5OTk5LCJhcHBpZCI6IjBlYzcyMzM0LTZkMDMtNDhmYi04OWU1LTU2NTJiODBiZDliYiIsImFwcGlkYWNyIjoiMSIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI0Ny8iLCJvaWQiOiJlNjgxZTZiMi1mZThkLTRkZGUtYjZiMS0xNjAyZDQyNWQzOWYiLCJzdWIiOiJlNjgxZTZiMi1mZThkLTRkZGUtYjZiMS0xNjAyZDQyNWQzOWYiLCJ0aWQiOiI3MmY5ODhiZi04NmYxLTQxYWYtOTFhYi0yZDdjZDAxMWRiNDciLCJ2ZXIiOiIxLjAifQ.nJVERbeDHLGHn7ZsbVGBJyHOu2PYhG5dji6F63gu8XN2Cvol3J1HO1uB4H3nCSt9DTu_jMHqAur_NNyobgNM21GojbEZAvd0I9NY0UDumBEvDZfMKneqp7a_cgAU7IYRcTPneSxbD6wo-8gIgfN9KDql98b0uEzixIVIWra2Q1bUUYETYqyaJNdS4RUmlJKNNpENllAyHQLv7hXnap1IuzP-f5CNIbbj9UgXxLiOtW5JhUAwWLZ3-WMhNRpUO2SIB7W7tQ0AbjXw3aUYr7el066J51z5tC1AK9UC-mD_fO_HUP6ZmPzu5gLA6DxkIIYP3grPnRVoUDltHQvwgONDOw"
}
```

## <a name="create-a-resource-group"></a><span data-ttu-id="a2648-143">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="a2648-143">Create a resource group</span></span>

<span data-ttu-id="a2648-144">Použijte hello následující toocreate skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="a2648-144">Use hello following toocreate a resource group.</span></span>

* <span data-ttu-id="a2648-145">Nastavit `$SUBSCRIPTIONID` ID předplatného toohello přijal při vytváření hello instanční objekt.</span><span class="sxs-lookup"><span data-stu-id="a2648-145">Set `$SUBSCRIPTIONID` toohello subscription ID received while creating hello service principal.</span></span>
* <span data-ttu-id="a2648-146">Nastavit `$ACCESSTOKEN` obdrželi v předchozím kroku hello toohello přístupový token.</span><span class="sxs-lookup"><span data-stu-id="a2648-146">Set `$ACCESSTOKEN` toohello access token received in hello previous step.</span></span>
* <span data-ttu-id="a2648-147">Nahraďte `DATACENTERLOCATION` s hello datového centra chcete skupinu prostředků hello toocreate a prostředky v.</span><span class="sxs-lookup"><span data-stu-id="a2648-147">Replace `DATACENTERLOCATION` with hello data center you wish toocreate hello resource group, and resources, in.</span></span> <span data-ttu-id="a2648-148">Například 'jihu USA".</span><span class="sxs-lookup"><span data-stu-id="a2648-148">For example, 'South Central US'.</span></span>
* <span data-ttu-id="a2648-149">Nastavit `$RESOURCEGROUPNAME` název toohello chcete toouse pro tuto skupinu:</span><span class="sxs-lookup"><span data-stu-id="a2648-149">Set `$RESOURCEGROUPNAME` toohello name you wish toouse for this group:</span></span>

```bash
curl -X "PUT" "https://management.azure.com/subscriptions/$SUBSCRIPTIONID/resourcegroups/$RESOURCEGROUPNAME?api-version=2015-01-01" \
    -H "Authorization: Bearer $ACCESSTOKEN" \
    -H "Content-Type: application/json" \
    -d $'{
"location": "DATACENTERLOCATION"
}'
```

<span data-ttu-id="a2648-150">Pokud tento požadavek je úspěšné, obdrží odpověď 200 řady a hello odpovědi obsahuje dokument JSON obsahující informace o skupině hello.</span><span class="sxs-lookup"><span data-stu-id="a2648-150">If this request is successful, you receive a 200 series response and hello response body contains a JSON document containing information about hello group.</span></span> <span data-ttu-id="a2648-151">Hello `"provisioningState"` element obsahuje hodnotu `"Succeeded"`.</span><span class="sxs-lookup"><span data-stu-id="a2648-151">hello `"provisioningState"` element contains a value of `"Succeeded"`.</span></span>

## <a name="create-a-deployment"></a><span data-ttu-id="a2648-152">Vytvoření nasazení</span><span class="sxs-lookup"><span data-stu-id="a2648-152">Create a deployment</span></span>

<span data-ttu-id="a2648-153">Použijte následující příkaz toodeploy hello skupiny prostředků šablony toohello hello.</span><span class="sxs-lookup"><span data-stu-id="a2648-153">Use hello following command toodeploy hello template toohello resource group.</span></span>

* <span data-ttu-id="a2648-154">Nastavit `$DEPLOYMENTNAME` název toohello chcete toouse pro toto nasazení.</span><span class="sxs-lookup"><span data-stu-id="a2648-154">Set `$DEPLOYMENTNAME` toohello name you wish toouse for this deployment.</span></span>

```bash
curl -X "PUT" "https://management.azure.com/subscriptions/$SUBSCRIPTIONID/resourcegroups/$RESOURCEGROUPNAME/providers/microsoft.resources/deployments/$DEPLOYMENTNAME?api-version=2015-01-01" \
-H "Authorization: Bearer $ACCESSTOKEN" \
-H "Content-Type: application/json" \
-d "{set your body string toohello template and parameters}"
```

> [!NOTE]
> <span data-ttu-id="a2648-155">Pokud jste uložili soubor tooa hello šablony, můžete použít následující příkaz místo hello `-d "{ template and parameters}"`:</span><span class="sxs-lookup"><span data-stu-id="a2648-155">If you saved hello template tooa file, you can use hello following command instead of `-d "{ template and parameters}"`:</span></span>
>
> `--data-binary "@/path/to/file.json"`

<span data-ttu-id="a2648-156">Pokud tento požadavek je úspěšné, obdrží odpověď 200 řady a hello odpovědi obsahuje dokument JSON obsahující informace o operaci nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="a2648-156">If this request is successful, you receive a 200 series response and hello response body contains a JSON document containing information about hello deployment operation.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a2648-157">nasazení Hello byla odeslána, ale nebyl dokončen.</span><span class="sxs-lookup"><span data-stu-id="a2648-157">hello deployment has been submitted, but has not completed.</span></span> <span data-ttu-id="a2648-158">Může trvat několik minut, obvykle přibližně 15 pro nasazení toocomplete hello.</span><span class="sxs-lookup"><span data-stu-id="a2648-158">It can take several minutes, usually around 15, for hello deployment toocomplete.</span></span>

## <a name="check-hello-status-of-a-deployment"></a><span data-ttu-id="a2648-159">Zkontrolujte stav hello nasazení</span><span class="sxs-lookup"><span data-stu-id="a2648-159">Check hello status of a deployment</span></span>

<span data-ttu-id="a2648-160">Stav hello toocheck hello nasazení, hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="a2648-160">toocheck hello status of hello deployment, use hello following command:</span></span>

```bash
curl -X "GET" "https://management.azure.com/subscriptions/$SUBSCRIPTIONID/resourcegroups/$RESOURCEGROUPNAME/providers/microsoft.resources/deployments/$DEPLOYMENTNAME?api-version=2015-01-01" \
-H "Authorization: Bearer $ACCESSTOKEN" \
-H "Content-Type: application/json"
```

<span data-ttu-id="a2648-161">Tento příkaz vrátí dokumentu JSON obsahující informace o operaci nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="a2648-161">This command returns a JSON document containing information about hello deployment operation.</span></span> <span data-ttu-id="a2648-162">Hello `"provisioningState"` element obsahuje hello stav nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="a2648-162">hello `"provisioningState"` element contains hello status of hello deployment.</span></span> <span data-ttu-id="a2648-163">Pokud tento prvek obsahuje hodnotu `"Succeeded"`, pak nasazení hello byla úspěšně dokončena.</span><span class="sxs-lookup"><span data-stu-id="a2648-163">If this element contains a value of `"Succeeded"`, then hello deployment has completed successfully.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="a2648-164">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="a2648-164">Troubleshoot</span></span>

<span data-ttu-id="a2648-165">Pokud narazíte na problémy s vytvářením clusterů HDInsight, podívejte se na [požadavky na řízení přístupu](hdinsight-administer-use-portal-linux.md#create-clusters).</span><span class="sxs-lookup"><span data-stu-id="a2648-165">If you run into issues with creating HDInsight clusters, see [access control requirements](hdinsight-administer-use-portal-linux.md#create-clusters).</span></span>

## <a name="next-steps"></a><span data-ttu-id="a2648-166">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a2648-166">Next steps</span></span>

<span data-ttu-id="a2648-167">Teď, když jste úspěšně vytvořili clusteru služby HDInsight, použijte následující toolearn jak hello toowork k vašemu clusteru.</span><span class="sxs-lookup"><span data-stu-id="a2648-167">Now that you have successfully created an HDInsight cluster, use hello following toolearn how toowork with your cluster.</span></span>

### <a name="hadoop-clusters"></a><span data-ttu-id="a2648-168">Clustery Hadoop</span><span class="sxs-lookup"><span data-stu-id="a2648-168">Hadoop clusters</span></span>

* [<span data-ttu-id="a2648-169">Použití Hivu se službou HDInsight</span><span class="sxs-lookup"><span data-stu-id="a2648-169">Use Hive with HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="a2648-170">Použití Pigu se službou HDInsight</span><span class="sxs-lookup"><span data-stu-id="a2648-170">Use Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="a2648-171">Používání nástroje MapReduce s HDInsight</span><span class="sxs-lookup"><span data-stu-id="a2648-171">Use MapReduce with HDInsight</span></span>](hdinsight-use-mapreduce.md)

### <a name="hbase-clusters"></a><span data-ttu-id="a2648-172">Clustery HBase</span><span class="sxs-lookup"><span data-stu-id="a2648-172">HBase clusters</span></span>

* [<span data-ttu-id="a2648-173">Začínáme s HBase v HDInsight</span><span class="sxs-lookup"><span data-stu-id="a2648-173">Get started with HBase on HDInsight</span></span>](hdinsight-hbase-tutorial-get-started-linux.md)
* [<span data-ttu-id="a2648-174">Vývoj aplikací v jazyce Java pro HBase v HDInsight</span><span class="sxs-lookup"><span data-stu-id="a2648-174">Develop Java applications for HBase on HDInsight</span></span>](hdinsight-hbase-build-java-maven-linux.md)

### <a name="storm-clusters"></a><span data-ttu-id="a2648-175">Clustery Storm</span><span class="sxs-lookup"><span data-stu-id="a2648-175">Storm clusters</span></span>

* [<span data-ttu-id="a2648-176">Vývoj topologie Java pro Storm v HDInsight</span><span class="sxs-lookup"><span data-stu-id="a2648-176">Develop Java topologies for Storm on HDInsight</span></span>](hdinsight-storm-develop-java-topology.md)
* [<span data-ttu-id="a2648-177">Použití komponent, Python v Storm v HDInsight</span><span class="sxs-lookup"><span data-stu-id="a2648-177">Use Python components in Storm on HDInsight</span></span>](hdinsight-storm-develop-python-topology.md)
* [<span data-ttu-id="a2648-178">Nasazení a monitorování topologie se Storm v HDInsight</span><span class="sxs-lookup"><span data-stu-id="a2648-178">Deploy and monitor topologies with Storm on HDInsight</span></span>](hdinsight-storm-deploy-monitor-topology-linux.md)
