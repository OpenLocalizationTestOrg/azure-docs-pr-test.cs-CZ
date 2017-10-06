---
title: "aaaCreate virtuálního počítače s Linuxem v Azure ze šablony | Microsoft Docs"
description: "Jak toouse hello Azure CLI 2.0 toocreate virtuálního počítače s Linuxem ze šablony Resource Manageru"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 721b8378-9e47-411e-842c-ec3276d3256a
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 05/12/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f4b8c4103cccbae13c679ff2a2cac928a0e8e809
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-linux-virtual-machine-with-azure-resource-manager-templates"></a>Jak toocreate virtuální počítač s Linuxem pomocí šablony Azure Resource Manager
Tento článek ukazuje, jak tooquickly nasazení Linux virtuálního počítače (VM) pomocí šablony Azure Resource Manager a hello 2.0 rozhraní příkazového řádku Azure. Můžete také provést tyto kroky hello [Azure CLI 1.0](create-ssh-secured-vm-from-template-nodejs.md).


## <a name="templates-overview"></a>Přehled šablon
Šablony Azure Resource Manageru jsou soubory JSON, které definují hello infrastruktury a konfigurace řešení Azure. Pomocí šablony můžete řešení opakovaně nasadit v průběhu životního cyklu a mít přitom jistotu, že se prostředky nasadí konzistentně. toolearn Další informace o formátu hello hello šablony a jak vytvořit, najdete v části [vytvoření vaší první šablony Azure Resource Manager](../../azure-resource-manager/resource-manager-create-first-template.md). tooview hello syntaxe JSON pro typy prostředků, najdete v části [definování zdrojů v šablonách Azure Resource Manager](/azure/templates/).


## <a name="create-resource-group"></a>Vytvoření skupiny prostředků
Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure. Před virtuálního počítače je třeba vytvořit skupinu prostředků. Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroupVM* v hello *eastus* oblasti:

```azurecli
az group create --name myResourceGroup --location eastus
```

## <a name="create-virtual-machine"></a>Vytvoření virtuálního počítače
Hello následující příklad vytvoří virtuální počítač z [této šablony Azure Resource Manageru](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json) s [vytvořit nasazení skupiny az](/cli/azure/group/deployment#create). Zadejte hodnotu hello vlastní veřejného klíče SSH, jako je například hello obsah *~/.ssh/id_rsa.pub*. Pokud potřebujete toocreate dvojici klíčů SSH, přečtěte si [jak toocreate a použití SSH klíče dvojice pro virtuální počítače s Linuxem v Azure](mac-create-ssh-keys.md).

```azurecli
az group deployment create --resource-group myResourceGroup \
  --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json \
  --parameters '{"sshKeyData": {"value": "ssh-rsa AAAAB3N{snip}B9eIgoZ"}}'
```

V tomto příkladu jste zadali šablonu uložené v Githubu. Můžete také stáhnout nebo vytvořit šablonu a zadejte místní cestu hello s hello stejné `--template-file` parametr.

tooSSH tooyour virtuálních počítačů, získat hello veřejnou IP adresu s [az sítě veřejné ip zobrazit](/cli/azure/network/public-ip#show):

```azurecli
az network public-ip show \
    --resource-group myResourceGroup \
    --name sshPublicIP \
    --query [ipAddress] \
    --output tsv
```

Pak můžete SSH tooyour virtuálního počítače jako za normálních okolností:

```bash
ssh azureuser@<ipAddress>
```

## <a name="next-steps"></a>Další kroky
V tomto příkladu jste vytvořili základní virtuální počítač s Linuxem. Pro další šablony správce prostředků, které zahrnují aplikační architektury nebo vytvořit složitější prostředí, procházet hello [galerii šablon Azure rychlý Start](https://azure.microsoft.com/documentation/templates/).
