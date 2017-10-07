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
# <a name="create-and-manage-virtual-machines-with-devtest-labs-using-hello-azure-cli"></a><span data-ttu-id="15984-103">Vytvářet a spravovat virtuální počítače s použitím hello rozhraní příkazového řádku Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="15984-103">Create and manage virtual machines with DevTest Labs using hello Azure CLI</span></span>
<span data-ttu-id="15984-104">Tento úvodní vám pomohou vytvoření, spuštění, připojením, aktualizaci a mazání vývojovém počítači ve svém testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="15984-104">This quick start will guide you through creating, starting, connecting, updating and cleaning up a development machine in your lab.</span></span> 

<span data-ttu-id="15984-105">Než začnete:</span><span class="sxs-lookup"><span data-stu-id="15984-105">Before you begin:</span></span>

* <span data-ttu-id="15984-106">Pokud nebyl vytvořen testovacím prostředí, pokyny naleznete [zde](devtest-lab-create-lab.md).</span><span class="sxs-lookup"><span data-stu-id="15984-106">If a lab has not been created, instructions can be found [here](devtest-lab-create-lab.md).</span></span>

* <span data-ttu-id="15984-107">[Instalace rozhraní příkazového řádku 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="15984-107">[Install CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).</span></span> <span data-ttu-id="15984-108">toostart, spusťte az přihlášení toocreate připojení s Azure.</span><span class="sxs-lookup"><span data-stu-id="15984-108">toostart, run az login toocreate a connection with Azure.</span></span> 

## <a name="create-and-verify-hello-virtual-machine"></a><span data-ttu-id="15984-109">Vytvoření a ověření hello virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="15984-109">Create and verify hello virtual machine</span></span> 
<span data-ttu-id="15984-110">Vytvoření virtuálního počítače z image pořízenou prostřednictvím marketplace s ssh ověřování.</span><span class="sxs-lookup"><span data-stu-id="15984-110">Create a VM from a marketplace image with ssh authentication.</span></span>
```azurecli
az lab vm create --lab-name sampleLabName --resource-group sampleLabResourceGroup --name sampleVMName --image "Ubuntu Server 16.04 LTS" --image-type gallery --size Standard_DS1_v2 --authentication-type  ssh --generate-ssh-keys --ip-configuration public 
```
> [!NOTE]
> <span data-ttu-id="15984-111">Uveďte hello **skupinu prostředků pro testovací prostředí** v hello – parametr skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="15984-111">Put hello **lab's resource group** name in hello --resource-group parameter.</span></span>
>

<span data-ttu-id="15984-112">Pokud chcete toocreate virtuálního počítače pomocí vzorce, použijte hello – vzorce parametr v [az testovacího prostředí virtuálního počítače vytvořit](https://docs.microsoft.com/cli/azure/lab/vm#create).</span><span class="sxs-lookup"><span data-stu-id="15984-112">If you want toocreate a VM using a formula, use hello --formula parameter in [az lab vm create](https://docs.microsoft.com/cli/azure/lab/vm#create).</span></span>


<span data-ttu-id="15984-113">Ověřte, že tento hello virtuálního počítače je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="15984-113">Verify that hello VM is available.</span></span>
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

## <a name="start-and-connect-toohello-virtual-machine"></a><span data-ttu-id="15984-114">Spuštění a připojení toohello virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="15984-114">Start and connect toohello virtual machine</span></span>
<span data-ttu-id="15984-115">Spuštění virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="15984-115">Start a VM.</span></span>
```azurecli
az lab vm start --lab-name sampleLabName --name sampleVMName --resource-group sampleLabResourceGroup
```
> [!NOTE]
> <span data-ttu-id="15984-116">Uveďte hello **skupinu prostředků pro testovací prostředí** v hello – parametr skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="15984-116">Put hello **lab's resource group** name in hello --resource-group parameter.</span></span>
>

<span data-ttu-id="15984-117">Připojit tooa virtuálních počítačů: [SSH](../virtual-machines/linux/mac-create-ssh-keys.md) nebo [vzdálené plochy](../virtual-machines/windows/connect-logon.md).</span><span class="sxs-lookup"><span data-stu-id="15984-117">Connect tooa VM: [SSH](../virtual-machines/linux/mac-create-ssh-keys.md) or [Remote Desktop](../virtual-machines/windows/connect-logon.md).</span></span>
```bash
ssh userName@ipAddressOrfqdn 
```

## <a name="update-hello-virtual-machine"></a><span data-ttu-id="15984-118">Aktualizovat virtuální počítač hello</span><span class="sxs-lookup"><span data-stu-id="15984-118">Update hello virtual machine</span></span>
<span data-ttu-id="15984-119">Použijte tooa artefaktů virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="15984-119">Apply artifacts tooa VM.</span></span>
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

<span data-ttu-id="15984-120">Zobrazí seznam artefaktů, které jsou k dispozici v testovacím prostředí hello.</span><span class="sxs-lookup"><span data-stu-id="15984-120">List artifacts available in hello lab.</span></span>
```azurecli
az lab vm show --lab-name sampleLabName --name sampleVMName --resource-group sampleResourceGroup --expand "properties(\$expand=artifacts)" --query 'artifacts[].{artifactId: artifactId, status: status}'
```
```json
{
  "artifactId": "/subscriptions/abcdeftgh1213123/resourceGroups/lisalab123RG822645/providers/Microsoft.DevTestLab/labs/lisalab123/artifactSources/public repo/artifacts/linux-install-nodejs",
  "status": "Succeeded"
}
```

## <a name="stop-and-delete-hello-virtual-machine"></a><span data-ttu-id="15984-121">Zastavení a odstranění virtuálního počítače hello</span><span class="sxs-lookup"><span data-stu-id="15984-121">Stop and delete hello virtual machine</span></span>    
<span data-ttu-id="15984-122">Zastavení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="15984-122">Stop a VM.</span></span>
```azurecli
az lab vm stop --lab-name sampleLabName --name sampleVMName --resource-group sampleResourceGroup
```

<span data-ttu-id="15984-123">Odstraňte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="15984-123">Delete a VM.</span></span>
```azurecli
az lab vm delete --lab-name sampleLabName --name sampleVMName --resource-group sampleResourceGroup
```

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]