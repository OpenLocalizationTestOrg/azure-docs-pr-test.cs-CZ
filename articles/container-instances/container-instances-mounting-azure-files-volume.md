---
title: "Připojení Azure Files svazku v Azure kontejner instancí"
description: "Zjistěte, jak připojit Azure Files svazek k uchování stavu s instancemi Azure kontejneru"
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/01/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 4248a3769ba8a0fb067b3904d55d487fe67e5778
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="mounting-an-azure-file-share-with-azure-container-instances"></a><span data-ttu-id="cd161-103">Připojení sdílenou složku Azure s instancemi Azure kontejneru</span><span class="sxs-lookup"><span data-stu-id="cd161-103">Mounting an Azure file share with Azure Container Instances</span></span>

<span data-ttu-id="cd161-104">Ve výchozím nastavení jsou bezstavové instancí kontejnerů Azure.</span><span class="sxs-lookup"><span data-stu-id="cd161-104">By default, Azure Container Instances are stateless.</span></span> <span data-ttu-id="cd161-105">Pokud kontejner dojde k chybě nebo zastaví, všechny její stav bude ztracena.</span><span class="sxs-lookup"><span data-stu-id="cd161-105">If the container crashes or stops, all of its state is lost.</span></span> <span data-ttu-id="cd161-106">K zachování stavu nad rámec životnost kontejneru, je nutné připojit svazek z externího úložiště.</span><span class="sxs-lookup"><span data-stu-id="cd161-106">To persist state beyond the lifetime of the container, you must mount a volume from an external store.</span></span> <span data-ttu-id="cd161-107">Tento článek ukazuje, jak připojit sdílenou složku Azure pro použití s instancemi Azure kontejneru.</span><span class="sxs-lookup"><span data-stu-id="cd161-107">This article shows how to mount an Azure file share for use with Azure Container Instances.</span></span>

## <a name="create-an-azure-file-share"></a><span data-ttu-id="cd161-108">Vytvořte sdílenou složku Azure</span><span class="sxs-lookup"><span data-stu-id="cd161-108">Create an Azure file share</span></span>

<span data-ttu-id="cd161-109">Před použitím sdílenou složku Azure s Azure kontejner instancí, je třeba ji vytvořit.</span><span class="sxs-lookup"><span data-stu-id="cd161-109">Before using an Azure file share with Azure Container Instances, you must create it.</span></span> <span data-ttu-id="cd161-110">Spusťte následující skript pro vytvoření účtu úložiště pro hostování sdílené složky a sdílené složky sám sebe.</span><span class="sxs-lookup"><span data-stu-id="cd161-110">Run the following script to create a storage account to host the file share and the share itself.</span></span> <span data-ttu-id="cd161-111">Všimněte si, že název účtu úložiště musí být globálně jedinečné, takže skript přidá do základní řetězec náhodná hodnota.</span><span class="sxs-lookup"><span data-stu-id="cd161-111">Note that the storage account name must be globally unique, so the script adds a random value to the base string.</span></span>

```azurecli-interactive
# Change these four parameters
ACI_PERS_STORAGE_ACCOUNT_NAME=mystorageaccount$RANDOM
ACI_PERS_RESOURCE_GROUP=myResourceGroup
ACI_PERS_LOCATION=eastus
ACI_PERS_SHARE_NAME=acishare

# Create the storage account with the parameters
az storage account create -n $ACI_PERS_STORAGE_ACCOUNT_NAME -g $ACI_PERS_RESOURCE_GROUP -l $ACI_PERS_LOCATION --sku Standard_LRS

# Export the connection string as an environment variable, this is used when creating the Azure file share
export AZURE_STORAGE_CONNECTION_STRING=`az storage account show-connection-string -n $ACI_PERS_STORAGE_ACCOUNT_NAME -g $ACI_PERS_RESOURCE_GROUP -o tsv`

# Create the share
az storage share create -n $ACI_PERS_SHARE_NAME
```

## <a name="acquire-storage-account-access-details"></a><span data-ttu-id="cd161-112">Získat podrobnosti o přístup k účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="cd161-112">Acquire storage account access details</span></span>

<span data-ttu-id="cd161-113">Chcete-li jako svazek v instancí kontejnerů Azure připojit sdílenou složku Azure, je třeba tří hodnot: název účtu úložiště, název sdílené složky a přístupový klíč úložiště.</span><span class="sxs-lookup"><span data-stu-id="cd161-113">To mount an Azure file share as a volume in Azure Container Instances, you need three values: the storage account name, the share name, and the storage access key.</span></span> 

<span data-ttu-id="cd161-114">Pokud jste použili výše uvedený skript, název účtu úložiště byl vytvořen s hodnotou náhodných na konci.</span><span class="sxs-lookup"><span data-stu-id="cd161-114">If you used the script above, the storage account name was created with a random value at the end.</span></span> <span data-ttu-id="cd161-115">Chcete-li prohledávat posledním řetězci (včetně náhodných část), použijte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="cd161-115">To query the final string (including the random portion), use the following commands:</span></span>

```azurecli-interactive
STORAGE_ACCOUNT=$(az storage account list --resource-group myResourceGroup --query "[?contains(name,'mystorageaccount')].[name]" -o tsv)
echo $STORAGE_ACCOUNT
```

<span data-ttu-id="cd161-116">Název sdílené složky je již znám (je *acishare* ve skriptu výše), takže všechny, že zůstanou je klíč účtu úložiště, které lze najít pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="cd161-116">The share name is already known (it is *acishare* in the script above), so all that remains is the storage account key, which can be found using the following command:</span></span>

```azurecli-interactive
$STORAGE_KEY=$(az storage account keys list --resource-group myResourceGroup --account-name $STORAGE_ACCOUNT --query "[0].value" -o tsv)
echo $STORAGE_KEY
```

## <a name="store-storage-account-access-details-with-azure-key-vault"></a><span data-ttu-id="cd161-117">Ukládání podrobností pro přístup k účtu úložiště Azure klíče trezoru</span><span class="sxs-lookup"><span data-stu-id="cd161-117">Store storage account access details with Azure key vault</span></span>

<span data-ttu-id="cd161-118">Klíče účtu úložiště chránit přístup k datům, proto doporučujeme uložit je do v Azure klíče trezoru.</span><span class="sxs-lookup"><span data-stu-id="cd161-118">Storage account keys protect access to your data, so we recommend storing them in an Azure key vault.</span></span> 

<span data-ttu-id="cd161-119">Vytvoření trezoru klíčů s Azure CLI:</span><span class="sxs-lookup"><span data-stu-id="cd161-119">Create a key vault with the Azure CLI:</span></span>

```azurecli-interactive
KEYVAULT_NAME=aci-keyvault
az keyvault create -n $KEYVAULT_NAME --enabled-for-template-deployment -g myResourceGroup
```

<span data-ttu-id="cd161-120">`enabled-for-template-deployment` Přepínač umožňuje Azure Resource Manageru pro vyžádání obsahu tajné klíče z trezoru klíčů v době nasazení.</span><span class="sxs-lookup"><span data-stu-id="cd161-120">The `enabled-for-template-deployment` switch allows Azure Resource Manager to pull secrets from your key vault at deployment time.</span></span>

<span data-ttu-id="cd161-121">Ukládání klíče účtu úložiště jako nový sdílený tajný klíč v trezoru klíčů:</span><span class="sxs-lookup"><span data-stu-id="cd161-121">Store the storage account key as a new secret in the key vault:</span></span>

```azurecli-interactive
KEYVAULT_SECRET_NAME=azurefilesstoragekey
az keyvault secret set --vault-name $KEYVAULT_NAME --name $KEYVAULT_SECRET_NAME --value $STORAGE_KEY
```

## <a name="mount-the-volume"></a><span data-ttu-id="cd161-122">Připojte svazek</span><span class="sxs-lookup"><span data-stu-id="cd161-122">Mount the volume</span></span>

<span data-ttu-id="cd161-123">Připojení sdílenou složku Azure jako svazek v kontejneru je dvoustupňový proces.</span><span class="sxs-lookup"><span data-stu-id="cd161-123">Mounting an Azure file share as a volume in a container is a two-step process.</span></span> <span data-ttu-id="cd161-124">Nejdřív zadejte podrobnosti o sdílenou složku jako součást definice skupina kontejneru, pak je určit, jakým způsobem chcete svazek připojený do jedné nebo více kontejnerů ve skupině.</span><span class="sxs-lookup"><span data-stu-id="cd161-124">First, you provide the details of the share as part of defining the container group, then you specify how you want the volume mounted within one or more of the containers in the group.</span></span>

<span data-ttu-id="cd161-125">Chcete-li definovat svazky, které mají být k dispozici pro připojení, přidejte `volumes` pole do kontejneru skupiny definice v šabloně Azure Resource Manager a pak na ně odkazovat v definici jednotlivých kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="cd161-125">To define the volumes you want to make available for mounting, add a `volumes` array to the container group definition in the Azure Resource Manager template, then reference them in the definition of the individual containers.</span></span>

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageaccountname": {
      "type": "string"
    },
    "storageaccountkey": {
      "type": "securestring"
    }
  },
  "resources":[{
    "name": "hellofiles",
    "type": "Microsoft.ContainerInstance/containerGroups",
    "apiVersion": "2017-08-01-preview",
    "location": "[resourceGroup().location]",
    "properties": {
      "containers": [{
        "name": "hellofiles",
        "properties": {
          "image": "seanmckenna/aci-hellofiles",
          "resources": {
            "requests": {
              "cpu": 1,
              "memoryInGb": 1.5
            }
          },
          "ports": [{
            "port": 80
          }],
          "volumeMounts": [{
            "name": "myvolume",
            "mountPath": "/aci/logs/"
          }]
        }  
      }],
      "osType": "Linux",
      "ipAddress": {
        "type": "Public",
        "ports": [{
          "protocol": "tcp",
          "port": "80"
        }]
      },
      "volumes": [{
        "name": "myvolume",
        "azureFile": {
          "shareName": "acishare",
          "storageAccountName": "[parameters('storageaccountname')]",
          "storageAccountKey": "[parameters('storageaccountkey')]"
        }
      }]
    }
  }]
}
```

<span data-ttu-id="cd161-126">Šablona obsahuje název účtu úložiště a klíč jako parametry, které lze zadat do souboru samostatné parametry.</span><span class="sxs-lookup"><span data-stu-id="cd161-126">The template includes the storage account name and key as parameters, which can be provided in a separate parameters file.</span></span> <span data-ttu-id="cd161-127">K naplnění souboru parametrů, budete potřebovat tří hodnot: název účtu úložiště, ID prostředku trezoru klíčů Azure a tajný název trezoru klíčů, který jste použili k uložení klíče úložiště.</span><span class="sxs-lookup"><span data-stu-id="cd161-127">To populate the parameters file, you will need three values: the storage account name, the resource ID of your Azure key vault, and the key vault secret name that you used to store the storage key.</span></span> <span data-ttu-id="cd161-128">Pokud jste postupovali podle předchozích kroků, můžete získat tyto hodnoty s následující skript:</span><span class="sxs-lookup"><span data-stu-id="cd161-128">If you have followed previous steps, you can get these values with the following script:</span></span>

```azurecli-interactive
echo $STORAGE_ACCOUNT
echo $KEYVAULT_SECRET_NAME
az keyvault show --name $KEYVAULT_NAME --query [id] -o tsv
```

<span data-ttu-id="cd161-129">Vložení hodnoty do souboru parametrů:</span><span class="sxs-lookup"><span data-stu-id="cd161-129">Insert the values into the parameters file:</span></span>

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageaccountname": {
      "value": "<my_storage_account_name>"
    },    
   "storageaccountkey": {
      "reference": {
        "keyVault": {
          "id": "<my_keyvault_id>"
        },
        "secretName": "<my_storage_account_key_secret_name>"
      }
    }
  }
}
```

## <a name="deploy-the-container-and-manage-files"></a><span data-ttu-id="cd161-130">Nasaďte kontejner a správa souborů</span><span class="sxs-lookup"><span data-stu-id="cd161-130">Deploy the container and manage files</span></span>

<span data-ttu-id="cd161-131">Pomocí šablony definovaný můžete vytvořit kontejner a jeho svazek pomocí rozhraní příkazového řádku Azure připojit.</span><span class="sxs-lookup"><span data-stu-id="cd161-131">With the template defined, you can create the container and mount its volume using the Azure CLI.</span></span> <span data-ttu-id="cd161-132">Za předpokladu, že je název souboru šablony *azuredeploy.json* a s názvem souboru parametrů *azuredeploy.parameters.json*, pak na příkazovém řádku:</span><span class="sxs-lookup"><span data-stu-id="cd161-132">Assuming that the template file is named *azuredeploy.json* and that the parameters file is named *azuredeploy.parameters.json*, then the command line is:</span></span>

```azurecli-interactive
az group deployment create --name hellofilesdeployment --template-file azuredeploy.json --parameters @azuredeploy.parameters.json --resource-group myResourceGroup
```

<span data-ttu-id="cd161-133">Po spuštění kontejneru, můžete jednoduché webové aplikace nasazené prostřednictvím **seanmckenna/aci-hellofiles** bitovou kopii, Správa souborů v Azure sdílené složky v cestě připojení, který jste zadali.</span><span class="sxs-lookup"><span data-stu-id="cd161-133">Once the container starts up, you can use the simple web app deployed via the **seanmckenna/aci-hellofiles** image, to the manage files in the Azure file share at the mount path that you specified.</span></span> <span data-ttu-id="cd161-134">Získejte ip adresu pro webové aplikace pomocí následující:</span><span class="sxs-lookup"><span data-stu-id="cd161-134">Obtain the ip address for the web app via the following:</span></span>

```azurecli-interactive
az container show --resource-group myResourceGroup --name hellofiles -o table
```

<span data-ttu-id="cd161-135">Můžete použít nástroje, jako [Microsoft Azure Storage Explorer](http://storageexplorer.com) k načtení a zkontrolujte soubor writen ke sdílené složce.</span><span class="sxs-lookup"><span data-stu-id="cd161-135">You can use a tool like the [Microsoft Azure Storage Explorer](http://storageexplorer.com) to retrieve and inspect the file writen to the file share.</span></span>

>[!NOTE]
> <span data-ttu-id="cd161-136">Další informace o používání šablon Azure Resource Manageru najdete v tématu soubory parametrů a nasazení pomocí rozhraní příkazového řádku Azure [nasazení prostředků pomocí šablony Resource Manageru a rozhraní příkazového řádku Azure](../azure-resource-manager/resource-group-template-deploy-cli.md).</span><span class="sxs-lookup"><span data-stu-id="cd161-136">To learn more about using Azure Resource Manager templates, parameter files, and deploying with the Azure CLI, see [Deploy resources with Resource Manager templates and Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="cd161-137">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cd161-137">Next steps</span></span>

- <span data-ttu-id="cd161-138">Nasazení pro váš první kontejner pomocí instancí kontejneru Azure [rychlý start](container-instances-quickstart.md)</span><span class="sxs-lookup"><span data-stu-id="cd161-138">Deploy for your first container using the Azure Container Instances [quick start](container-instances-quickstart.md)</span></span>
- <span data-ttu-id="cd161-139">Další informace o [vztah mezi instancemi kontejner Azure a orchestrators kontejneru](container-instances-orchestrator-relationship.md)</span><span class="sxs-lookup"><span data-stu-id="cd161-139">Learn about the [relationship between Azure Container Instances and container orchestrators](container-instances-orchestrator-relationship.md)</span></span>
