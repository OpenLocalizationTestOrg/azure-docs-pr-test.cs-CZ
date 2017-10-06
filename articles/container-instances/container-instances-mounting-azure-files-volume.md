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
# <a name="mounting-an-azure-file-share-with-azure-container-instances"></a>Připojení sdílenou složku Azure s instancemi Azure kontejneru

Ve výchozím nastavení jsou bezstavové instancí kontejnerů Azure. Pokud kontejner hello dojde k chybě nebo zastaví, všechny její stav bude ztracena. toopersist stavu nad rámec hello životnost hello kontejneru, musíte připojit svazek z externího úložiště. Tento článek ukazuje, jak toomount Azure sdílení souborů pro použití s instancemi Azure kontejneru.

## <a name="create-an-azure-file-share"></a>Vytvořte sdílenou složku Azure

Před použitím sdílenou složku Azure s Azure kontejner instancí, je třeba ji vytvořit. Spusťte následující skript toocreate hello úložiště účet toohost hello sdílené složky a hello sdílet sám sebe. Všimněte si, že název účtu úložiště hello musí být globálně jedinečné, takže hello skript přidá řetězec základní toohello náhodná hodnota.

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

## <a name="acquire-storage-account-access-details"></a>Získat podrobnosti o přístup k účtu úložiště

toomount soubor Azure sdílet jako svazek v Azure kontejner instancí, je třeba tří hodnot: hello název účtu úložiště, název sdílené složky hello a přístupový klíč k úložišti hello. 

Pokud jste použili hello skript výše, název účtu úložiště hello byl vytvořen s hodnotou náhodných na konci hello. tooquery hello v posledním řetězci (včetně hello náhodných část), použijte hello následující příkazy:

```azurecli-interactive
STORAGE_ACCOUNT=$(az storage account list --resource-group myResourceGroup --query "[?contains(name,'mystorageaccount')].[name]" -o tsv)
echo $STORAGE_ACCOUNT
```

Hello název sdílené složky je již znám (je *acishare* ve skriptu hello výše), takže všechny, které zůstanou je klíč účtu úložiště hello, které lze nalézt pomocí hello následující příkaz:

```azurecli-interactive
$STORAGE_KEY=$(az storage account keys list --resource-group myResourceGroup --account-name $STORAGE_ACCOUNT --query "[0].value" -o tsv)
echo $STORAGE_KEY
```

## <a name="store-storage-account-access-details-with-azure-key-vault"></a>Ukládání podrobností pro přístup k účtu úložiště Azure klíče trezoru

Klíče účtu úložiště chránit přístup k datům tooyour, proto doporučujeme uložit je do v Azure klíče trezoru. 

Vytvoření trezoru klíčů s hello rozhraní příkazového řádku Azure:

```azurecli-interactive
KEYVAULT_NAME=aci-keyvault
az keyvault create -n $KEYVAULT_NAME --enabled-for-template-deployment -g myResourceGroup
```

Hello `enabled-for-template-deployment` přepínač umožňuje Azure Resource Manager toopull tajné klíče z trezoru klíčů v době nasazení.

Úložiště hello klíč účtu úložiště jako nový sdílený tajný klíč v trezoru klíčů hello:

```azurecli-interactive
KEYVAULT_SECRET_NAME=azurefilesstoragekey
az keyvault secret set --vault-name $KEYVAULT_NAME --name $KEYVAULT_SECRET_NAME --value $STORAGE_KEY
```

## <a name="mount-hello-volume"></a>Připojit svazek s hello

Připojení sdílenou složku Azure jako svazek v kontejneru je dvoustupňový proces. Zadejte způsob hello svazek připojený v rámci jeden nebo více kontejnerů hello ve skupině hello se nejdřív zadejte hello podrobnosti složky hello jako součást definice hello kontejner skupiny.

Přidat svazky hello toodefine chcete toomake k dispozici pro připojení, `volumes` pole definice skupiny toohello kontejneru v hello šablony Azure Resource Manageru a pak na ně odkazovat v definici hello hello jednotlivých kontejnerů.

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

Šablona Hello obsahuje hello název účtu úložiště a klíč jako parametry, které lze zadat do souboru samostatné parametry. soubor parametrů hello toopopulate, budete potřebovat tří hodnot: hello název účtu úložiště, hello ID prostředku Azure key Vault a hello tajný název trezoru klíčů, kterou jste použili klíč úložiště toostore hello. Pokud jste postupovali podle předchozích kroků, můžete získat tyto hodnoty s hello následující skript:

```azurecli-interactive
echo $STORAGE_ACCOUNT
echo $KEYVAULT_SECRET_NAME
az keyvault show --name $KEYVAULT_NAME --query [id] -o tsv
```

Vložte hello hodnoty do souboru parametrů hello:

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

## <a name="deploy-hello-container-and-manage-files"></a>Kontejner hello nasazení a správa souborů

Pomocí šablony hello definované můžete vytvořit kontejner hello a připojit svazek s jeho pomocí hello rozhraní příkazového řádku Azure. Za předpokladu, že hello soubor šablony je s názvem *azuredeploy.json* a tento soubor parametrů hello jmenuje *azuredeploy.parameters.json*, pak je hello příkazového řádku:

```azurecli-interactive
az group deployment create --name hellofilesdeployment --template-file azuredeploy.json --parameters @azuredeploy.parameters.json --resource-group myResourceGroup
```

Po spuštění hello kontejneru, můžete hello jednoduché webové aplikace nasazené prostřednictvím hello **seanmckenna/aci-hellofiles** bitovou kopii, toohello Správa souborů v hello Azure sdílené složky v cestě hello připojení, který jste zadali. Získejte ip adresu hello hello webové aplikace pomocí hello následující:

```azurecli-interactive
az container show --resource-group myResourceGroup --name hellofiles -o table
```

Můžete použít nástroje, jako je hello [Microsoft Azure Storage Explorer](http://storageexplorer.com) tooretrieve a zkontrolovat hello souboru writen toohello sdílené složky.

>[!NOTE]
> toolearn Další informace o použití šablon Azure Resource Manageru, soubory parametrů a nasazení s hello Azure CLI, najdete v části [nasazení prostředků pomocí šablony Resource Manageru a rozhraní příkazového řádku Azure](../azure-resource-manager/resource-group-template-deploy-cli.md).

## <a name="next-steps"></a>Další kroky

- Nasazení pro váš první kontejner pomocí hello Azure kontejner instancí [rychlý start](container-instances-quickstart.md)
- Další informace o hello [vztah mezi instancemi kontejner Azure a orchestrators kontejneru](container-instances-orchestrator-relationship.md)
