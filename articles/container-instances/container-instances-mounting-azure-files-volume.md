---
title: "aaaMounting svazek Azure soubory v Azure kontejner instancí"
description: "Zjistěte, jak toomount Azure soubory stavu toopersist svazek s instancemi Azure kontejneru"
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
ms.openlocfilehash: d87215e06d5e5af40bfebcad17768ee45ccabbb2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="mounting-an-azure-file-share-with-azure-container-instances"></a><span data-ttu-id="8e27a-103">Připojení sdílenou složku Azure s instancemi Azure kontejneru</span><span class="sxs-lookup"><span data-stu-id="8e27a-103">Mounting an Azure file share with Azure Container Instances</span></span>

<span data-ttu-id="8e27a-104">Ve výchozím nastavení jsou bezstavové instancí kontejnerů Azure.</span><span class="sxs-lookup"><span data-stu-id="8e27a-104">By default, Azure Container Instances are stateless.</span></span> <span data-ttu-id="8e27a-105">Pokud kontejner hello dojde k chybě nebo zastaví, všechny její stav bude ztracena.</span><span class="sxs-lookup"><span data-stu-id="8e27a-105">If hello container crashes or stops, all of its state is lost.</span></span> <span data-ttu-id="8e27a-106">toopersist stavu nad rámec hello životnost hello kontejneru, musíte připojit svazek z externího úložiště.</span><span class="sxs-lookup"><span data-stu-id="8e27a-106">toopersist state beyond hello lifetime of hello container, you must mount a volume from an external store.</span></span> <span data-ttu-id="8e27a-107">Tento článek ukazuje, jak toomount Azure sdílení souborů pro použití s instancemi Azure kontejneru.</span><span class="sxs-lookup"><span data-stu-id="8e27a-107">This article shows how toomount an Azure file share for use with Azure Container Instances.</span></span>

## <a name="create-an-azure-file-share"></a><span data-ttu-id="8e27a-108">Vytvořte sdílenou složku Azure</span><span class="sxs-lookup"><span data-stu-id="8e27a-108">Create an Azure file share</span></span>

<span data-ttu-id="8e27a-109">Před použitím sdílenou složku Azure s Azure kontejner instancí, je třeba ji vytvořit.</span><span class="sxs-lookup"><span data-stu-id="8e27a-109">Before using an Azure file share with Azure Container Instances, you must create it.</span></span> <span data-ttu-id="8e27a-110">Spusťte následující skript toocreate hello úložiště účet toohost hello sdílené složky a hello sdílet sám sebe.</span><span class="sxs-lookup"><span data-stu-id="8e27a-110">Run hello following script toocreate a storage account toohost hello file share and hello share itself.</span></span> <span data-ttu-id="8e27a-111">Všimněte si, že název účtu úložiště hello musí být globálně jedinečné, takže hello skript přidá řetězec základní toohello náhodná hodnota.</span><span class="sxs-lookup"><span data-stu-id="8e27a-111">Note that hello storage account name must be globally unique, so hello script adds a random value toohello base string.</span></span>

```azurecli-interactive
# Change these four parameters
ACI_PERS_STORAGE_ACCOUNT_NAME=mystorageaccount$RANDOM
ACI_PERS_RESOURCE_GROUP=myResourceGroup
ACI_PERS_LOCATION=eastus
ACI_PERS_SHARE_NAME=acishare

# Create hello storage account with hello parameters
az storage account create -n $ACI_PERS_STORAGE_ACCOUNT_NAME -g $ACI_PERS_RESOURCE_GROUP -l $ACI_PERS_LOCATION --sku Standard_LRS

# Export hello connection string as an environment variable, this is used when creating hello Azure file share
export AZURE_STORAGE_CONNECTION_STRING=`az storage account show-connection-string -n $ACI_PERS_STORAGE_ACCOUNT_NAME -g $ACI_PERS_RESOURCE_GROUP -o tsv`

# Create hello share
az storage share create -n $ACI_PERS_SHARE_NAME
```

## <a name="acquire-storage-account-access-details"></a><span data-ttu-id="8e27a-112">Získat podrobnosti o přístup k účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="8e27a-112">Acquire storage account access details</span></span>

<span data-ttu-id="8e27a-113">toomount soubor Azure sdílet jako svazek v Azure kontejner instancí, je třeba tří hodnot: hello název účtu úložiště, název sdílené složky hello a přístupový klíč k úložišti hello.</span><span class="sxs-lookup"><span data-stu-id="8e27a-113">toomount an Azure file share as a volume in Azure Container Instances, you need three values: hello storage account name, hello share name, and hello storage access key.</span></span> 

<span data-ttu-id="8e27a-114">Pokud jste použili hello skript výše, název účtu úložiště hello byl vytvořen s hodnotou náhodných na konci hello.</span><span class="sxs-lookup"><span data-stu-id="8e27a-114">If you used hello script above, hello storage account name was created with a random value at hello end.</span></span> <span data-ttu-id="8e27a-115">tooquery hello v posledním řetězci (včetně hello náhodných část), použijte hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="8e27a-115">tooquery hello final string (including hello random portion), use hello following commands:</span></span>

```azurecli-interactive
STORAGE_ACCOUNT=$(az storage account list --resource-group myResourceGroup --query "[?contains(name,'mystorageaccount')].[name]" -o tsv)
echo $STORAGE_ACCOUNT
```

<span data-ttu-id="8e27a-116">Hello název sdílené složky je již znám (je *acishare* ve skriptu hello výše), takže všechny, které zůstanou je klíč účtu úložiště hello, které lze nalézt pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="8e27a-116">hello share name is already known (it is *acishare* in hello script above), so all that remains is hello storage account key, which can be found using hello following command:</span></span>

```azurecli-interactive
$STORAGE_KEY=$(az storage account keys list --resource-group myResourceGroup --account-name $STORAGE_ACCOUNT --query "[0].value" -o tsv)
echo $STORAGE_KEY
```

## <a name="store-storage-account-access-details-with-azure-key-vault"></a><span data-ttu-id="8e27a-117">Ukládání podrobností pro přístup k účtu úložiště Azure klíče trezoru</span><span class="sxs-lookup"><span data-stu-id="8e27a-117">Store storage account access details with Azure key vault</span></span>

<span data-ttu-id="8e27a-118">Klíče účtu úložiště chránit přístup k datům tooyour, proto doporučujeme uložit je do v Azure klíče trezoru.</span><span class="sxs-lookup"><span data-stu-id="8e27a-118">Storage account keys protect access tooyour data, so we recommend storing them in an Azure key vault.</span></span> 

<span data-ttu-id="8e27a-119">Vytvoření trezoru klíčů s hello rozhraní příkazového řádku Azure:</span><span class="sxs-lookup"><span data-stu-id="8e27a-119">Create a key vault with hello Azure CLI:</span></span>

```azurecli-interactive
KEYVAULT_NAME=aci-keyvault
az keyvault create -n $KEYVAULT_NAME --enabled-for-template-deployment -g myResourceGroup
```

<span data-ttu-id="8e27a-120">Hello `enabled-for-template-deployment` přepínač umožňuje Azure Resource Manager toopull tajné klíče z trezoru klíčů v době nasazení.</span><span class="sxs-lookup"><span data-stu-id="8e27a-120">hello `enabled-for-template-deployment` switch allows Azure Resource Manager toopull secrets from your key vault at deployment time.</span></span>

<span data-ttu-id="8e27a-121">Úložiště hello klíč účtu úložiště jako nový sdílený tajný klíč v trezoru klíčů hello:</span><span class="sxs-lookup"><span data-stu-id="8e27a-121">Store hello storage account key as a new secret in hello key vault:</span></span>

```azurecli-interactive
KEYVAULT_SECRET_NAME=azurefilesstoragekey
az keyvault secret set --vault-name $KEYVAULT_NAME --name $KEYVAULT_SECRET_NAME --value $STORAGE_KEY
```

## <a name="mount-hello-volume"></a><span data-ttu-id="8e27a-122">Připojit svazek s hello</span><span class="sxs-lookup"><span data-stu-id="8e27a-122">Mount hello volume</span></span>

<span data-ttu-id="8e27a-123">Připojení sdílenou složku Azure jako svazek v kontejneru je dvoustupňový proces.</span><span class="sxs-lookup"><span data-stu-id="8e27a-123">Mounting an Azure file share as a volume in a container is a two-step process.</span></span> <span data-ttu-id="8e27a-124">Zadejte způsob hello svazek připojený v rámci jeden nebo více kontejnerů hello ve skupině hello se nejdřív zadejte hello podrobnosti složky hello jako součást definice hello kontejner skupiny.</span><span class="sxs-lookup"><span data-stu-id="8e27a-124">First, you provide hello details of hello share as part of defining hello container group, then you specify how you want hello volume mounted within one or more of hello containers in hello group.</span></span>

<span data-ttu-id="8e27a-125">Přidat svazky hello toodefine chcete toomake k dispozici pro připojení, `volumes` pole definice skupiny toohello kontejneru v hello šablony Azure Resource Manageru a pak na ně odkazovat v definici hello hello jednotlivých kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="8e27a-125">toodefine hello volumes you want toomake available for mounting, add a `volumes` array toohello container group definition in hello Azure Resource Manager template, then reference them in hello definition of hello individual containers.</span></span>

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

<span data-ttu-id="8e27a-126">Šablona Hello obsahuje hello název účtu úložiště a klíč jako parametry, které lze zadat do souboru samostatné parametry.</span><span class="sxs-lookup"><span data-stu-id="8e27a-126">hello template includes hello storage account name and key as parameters, which can be provided in a separate parameters file.</span></span> <span data-ttu-id="8e27a-127">soubor parametrů hello toopopulate, budete potřebovat tří hodnot: hello název účtu úložiště, hello ID prostředku Azure key Vault a hello tajný název trezoru klíčů, kterou jste použili klíč úložiště toostore hello.</span><span class="sxs-lookup"><span data-stu-id="8e27a-127">toopopulate hello parameters file, you will need three values: hello storage account name, hello resource ID of your Azure key vault, and hello key vault secret name that you used toostore hello storage key.</span></span> <span data-ttu-id="8e27a-128">Pokud jste postupovali podle předchozích kroků, můžete získat tyto hodnoty s hello následující skript:</span><span class="sxs-lookup"><span data-stu-id="8e27a-128">If you have followed previous steps, you can get these values with hello following script:</span></span>

```azurecli-interactive
echo $STORAGE_ACCOUNT
echo $KEYVAULT_SECRET_NAME
az keyvault show --name $KEYVAULT_NAME --query [id] -o tsv
```

<span data-ttu-id="8e27a-129">Vložte hello hodnoty do souboru parametrů hello:</span><span class="sxs-lookup"><span data-stu-id="8e27a-129">Insert hello values into hello parameters file:</span></span>

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

## <a name="deploy-hello-container-and-manage-files"></a><span data-ttu-id="8e27a-130">Kontejner hello nasazení a správa souborů</span><span class="sxs-lookup"><span data-stu-id="8e27a-130">Deploy hello container and manage files</span></span>

<span data-ttu-id="8e27a-131">Pomocí šablony hello definované můžete vytvořit kontejner hello a připojit svazek s jeho pomocí hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="8e27a-131">With hello template defined, you can create hello container and mount its volume using hello Azure CLI.</span></span> <span data-ttu-id="8e27a-132">Za předpokladu, že hello soubor šablony je s názvem *azuredeploy.json* a tento soubor parametrů hello jmenuje *azuredeploy.parameters.json*, pak je hello příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="8e27a-132">Assuming that hello template file is named *azuredeploy.json* and that hello parameters file is named *azuredeploy.parameters.json*, then hello command line is:</span></span>

```azurecli-interactive
az group deployment create --name hellofilesdeployment --template-file azuredeploy.json --parameters @azuredeploy.parameters.json --resource-group myResourceGroup
```

<span data-ttu-id="8e27a-133">Po spuštění hello kontejneru, můžete hello jednoduché webové aplikace nasazené prostřednictvím hello **seanmckenna/aci-hellofiles** bitovou kopii, toohello Správa souborů v hello Azure sdílené složky v cestě hello připojení, který jste zadali.</span><span class="sxs-lookup"><span data-stu-id="8e27a-133">Once hello container starts up, you can use hello simple web app deployed via hello **seanmckenna/aci-hellofiles** image, toohello manage files in hello Azure file share at hello mount path that you specified.</span></span> <span data-ttu-id="8e27a-134">Získejte ip adresu hello hello webové aplikace pomocí hello následující:</span><span class="sxs-lookup"><span data-stu-id="8e27a-134">Obtain hello ip address for hello web app via hello following:</span></span>

```azurecli-interactive
az container show --resource-group myResourceGroup --name hellofiles -o table
```

<span data-ttu-id="8e27a-135">Můžete použít nástroje, jako je hello [Microsoft Azure Storage Explorer](http://storageexplorer.com) tooretrieve a zkontrolovat hello souboru writen toohello sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="8e27a-135">You can use a tool like hello [Microsoft Azure Storage Explorer](http://storageexplorer.com) tooretrieve and inspect hello file writen toohello file share.</span></span>

>[!NOTE]
> <span data-ttu-id="8e27a-136">toolearn Další informace o použití šablon Azure Resource Manageru, soubory parametrů a nasazení s hello Azure CLI, najdete v části [nasazení prostředků pomocí šablony Resource Manageru a rozhraní příkazového řádku Azure](../azure-resource-manager/resource-group-template-deploy-cli.md).</span><span class="sxs-lookup"><span data-stu-id="8e27a-136">toolearn more about using Azure Resource Manager templates, parameter files, and deploying with hello Azure CLI, see [Deploy resources with Resource Manager templates and Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="8e27a-137">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8e27a-137">Next steps</span></span>

- <span data-ttu-id="8e27a-138">Nasazení pro váš první kontejner pomocí hello Azure kontejner instancí [rychlý start](container-instances-quickstart.md)</span><span class="sxs-lookup"><span data-stu-id="8e27a-138">Deploy for your first container using hello Azure Container Instances [quick start](container-instances-quickstart.md)</span></span>
- <span data-ttu-id="8e27a-139">Další informace o hello [vztah mezi instancemi kontejner Azure a orchestrators kontejneru](container-instances-orchestrator-relationship.md)</span><span class="sxs-lookup"><span data-stu-id="8e27a-139">Learn about hello [relationship between Azure Container Instances and container orchestrators](container-instances-orchestrator-relationship.md)</span></span>
