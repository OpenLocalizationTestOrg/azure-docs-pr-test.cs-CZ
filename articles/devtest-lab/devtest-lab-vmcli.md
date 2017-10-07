---
title: "aaaCreate a spravovat virtuální počítače v DevTest Labs pomocí rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Zjistěte, jak Azure DevTest Labs toocreate toouse a spravovat virtuální počítače s Azure CLI 2.0"
services: devtest-lab,virtual-machines
documentationcenter: na
author: lisawong19
manager: douge
editor: 
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: liwong
ms.openlocfilehash: 98ea3aa7b2489bff61971573aaf584984cd811e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-virtual-machines-with-devtest-labs-using-hello-azure-cli"></a>Vytvářet a spravovat virtuální počítače s použitím hello rozhraní příkazového řádku Azure DevTest Labs
Tento úvodní vám pomohou vytvoření, spuštění, připojením, aktualizaci a mazání vývojovém počítači ve svém testovacím prostředí. 

Než začnete:

* Pokud nebyl vytvořen testovacím prostředí, pokyny naleznete [zde](devtest-lab-create-lab.md).

* [Instalace rozhraní příkazového řádku 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli). toostart, spusťte az přihlášení toocreate připojení s Azure. 

## <a name="create-and-verify-hello-virtual-machine"></a>Vytvoření a ověření hello virtuálního počítače 
Vytvoření virtuálního počítače z image pořízenou prostřednictvím marketplace s ssh ověřování.
```azurecli
az lab vm create --lab-name sampleLabName --resource-group sampleLabResourceGroup --name sampleVMName --image "Ubuntu Server 16.04 LTS" --image-type gallery --size Standard_DS1_v2 --authentication-type  ssh --generate-ssh-keys --ip-configuration public 
```
> [!NOTE]
> Uveďte hello **skupinu prostředků pro testovací prostředí** v hello – parametr skupinu prostředků.
>

Pokud chcete toocreate virtuálního počítače pomocí vzorce, použijte hello – vzorce parametr v [az testovacího prostředí virtuálního počítače vytvořit](https://docs.microsoft.com/cli/azure/lab/vm#create).


Ověřte, že tento hello virtuálního počítače je k dispozici.
```azurecli
az lab vm show --lab-name sampleLabName --name sampleVMName --resource-group sampleResourceGroup --expand 'properties($expand=ComputeVm,NetworkInterface)' --query '{status: computeVm.statuses[0].displayStatus, fqdn: fqdn, ipAddress: networkInterface.publicIpAddress}'
```
```json
{
  "fqdn": "lisalabvm.southcentralus.cloudapp.azure.com",
  "ipAddress": "13.85.228.112",
  "status": "Provisioning succeeded"
}
```

## <a name="start-and-connect-toohello-virtual-machine"></a>Spuštění a připojení toohello virtuálního počítače
Spuštění virtuálního počítače.
```azurecli
az lab vm start --lab-name sampleLabName --name sampleVMName --resource-group sampleLabResourceGroup
```
> [!NOTE]
> Uveďte hello **skupinu prostředků pro testovací prostředí** v hello – parametr skupinu prostředků.
>

Připojit tooa virtuálních počítačů: [SSH](../virtual-machines/linux/mac-create-ssh-keys.md) nebo [vzdálené plochy](../virtual-machines/windows/connect-logon.md).
```bash
ssh userName@ipAddressOrfqdn 
```

## <a name="update-hello-virtual-machine"></a>Aktualizovat virtuální počítač hello
Použijte tooa artefaktů virtuálních počítačů.
```azurecli
az lab vm apply-artifacts --lab-name  sampleLabName --name sampleVMName  --resource-group sampleResourceGroup  --artifacts @/artifacts.json
```

```json
[
  {
    "artifactId": "/artifactSources/public repo/artifacts/linux-java",
    "parameters": []
  },
  {
    "artifactId": "/artifactSources/public repo/artifacts/linux-install-nodejs",
    "parameters": []
  },
  {
    "artifactId": "/artifactSources/public repo/artifacts/linux-apt-package",
    "parameters": [
      {
        "name": "packages",
        "value": "abcd"
      },
      {
        "name": "update",
        "value": "true"
      },
      {
        "name": "options",
        "value": ""
      }
    ]
  } 
]
```

Zobrazí seznam artefaktů, které jsou k dispozici v testovacím prostředí hello.
```azurecli
az lab vm show --lab-name sampleLabName --name sampleVMName --resource-group sampleResourceGroup --expand "properties(\$expand=artifacts)" --query 'artifacts[].{artifactId: artifactId, status: status}'
```
```json
{
  "artifactId": "/subscriptions/abcdeftgh1213123/resourceGroups/lisalab123RG822645/providers/Microsoft.DevTestLab/labs/lisalab123/artifactSources/public repo/artifacts/linux-install-nodejs",
  "status": "Succeeded"
}
```

## <a name="stop-and-delete-hello-virtual-machine"></a>Zastavení a odstranění virtuálního počítače hello    
Zastavení virtuálního počítače.
```azurecli
az lab vm stop --lab-name sampleLabName --name sampleVMName --resource-group sampleResourceGroup
```

Odstraňte virtuální počítač.
```azurecli
az lab vm delete --lab-name sampleLabName --name sampleVMName --resource-group sampleResourceGroup
```

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]