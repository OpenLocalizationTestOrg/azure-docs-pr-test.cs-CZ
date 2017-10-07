---
title: "virtuální počítač s Linuxem pomocí šablony Azure s Azure CLI 1.0 aaaCreate | Microsoft Docs"
description: "Vytvoření virtuálního počítače s Linuxem v Azure pomocí Azure CLI 1.0 hello a šablonu Azure Resource Manager."
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/12/2017
ms.author: v-livech
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b694cc8247a8431b7ef4b24cc7dc2b4cdb9660ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-linux-vm-using-hello-azure-cli-10-an-azure-resource-manager-template"></a>Jak hello toocreate a virtuální počítač s Linuxem pomocí Azure CLI 1.0 šablonu Azure Resource Manager
Tento článek ukazuje, jak tooquickly nasadit virtuální počítač s Linuxem pomocí hello Azure CLI 1.0 a šablonu Azure Resource Manager. článek Hello vyžaduje:

* účet Azure ([získejte bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/))
* Hello [Azure CLI 1.0](../../cli-install-nodejs.md) přihlášení `azure login`.
* Hello rozhraní příkazového řádku Azure *musí být v* režimu Azure Resource Manager `azure config mode arm`.

Šablonu virtuálního počítače s Linuxem můžete také rychle nasadit pomocí hello [portál Azure](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="cli-versions-toocomplete-hello-task"></a>Úloha hello toocomplete verze rozhraní příkazového řádku
Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku:

- [Azure CLI 1.0](#quick-command-summary) – naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modelů (v tomto článku)
- [Azure CLI 2.0](create-ssh-secured-vm-from-template.md) -naší nové generace rozhraní příkazového řádku pro model nasazení správy prostředků hello

## <a name="quick-command-summary"></a>Rychlý přehled příkazu
```azurecli
azure group create \
    -n myResourceGroup \
    -l westus \
    --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

## <a name="detailed-walkthrough"></a>Podrobný postup
Šablony umožňují toocreate virtuální počítače na platformě Azure s nastavením, který má toocustomize během spuštění hello, nastavení, jako je uživatelská jména a názvy hostitelů. Pro tento článek spouštíme šablonu Azure využívající virtuální počítač Ubuntu společně se skupinu zabezpečení sítě (NSG) s portem 22 otevřeným pro SSH.

Šablony Azure Resource Manageru jsou soubory JSON, které jde použít k jednoduchým jednorázovým úlohám, jako je spuštění virtuálního počítače Ubuntu VM pro účely tohoto článku.  Šablony Azure může být také použít tooconstruct komplexní konfigurace Azure celých prostředí jako zásobníky nasazení testování, vývojového nebo produkčního prostředí.

## <a name="create-hello-linux-vm"></a>Vytvoření virtuálního počítače s Linuxem hello
Následující příklad ukazuje kód jak Hello toocall `azure group create` toocreate prostředek skupiny a nasadit virtuální počítač Linux zabezpečením SSH hello současně pomocí [této šablony Azure Resource Manageru](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json). Nezapomeňte, že v příkladu potřebujete toouse názvy, které jsou jedinečné tooyour prostředí. Tento příklad používá *myResourceGroup* jako hello název skupiny prostředků, a *Můjvp* jako hello název virtuálního počítače.

```azurecli
azure group create \
    --name myResourceGroup \
    --location westus \
    --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

výstup Hello by měl vypadat podobně jako následující bloku výstup hello:

```azurecli
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Creating resource group myResourceGroup
info:    Created resource group myResourceGroup
info:    Supply values for hello following parameters
sshKeyData: ssh-rsa AAAAB3Nza<..ssh public key text..>VQgwjNjQ== myAdminUser@myVM
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "azuredeploy"
data:    Id:                  /subscriptions/<..subid text..>/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK
```

Například nasazení virtuálního počítače pomocí hello `--template-uri` parametr.  Můžete také stáhnout nebo vytvořit šablonu místně a předat hello šablony pomocí hello `--template-file` parametr s cesta k souboru šablony toohello jako argument. Hello příkazového řádku Azure CLI vás vyzve k zadání parametrů hello požadovaných šablonou hello.

## <a name="next-steps"></a>Další kroky
Hledání hello [galerii šablon](https://azure.microsoft.com/documentation/templates/) toodiscover jaké aplikace rozhraní toodeploy Další.

