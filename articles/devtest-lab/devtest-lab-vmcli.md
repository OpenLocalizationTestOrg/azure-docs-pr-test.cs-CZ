---
title: "Vytvářet a spravovat virtuální počítače v DevTest Labs pomocí rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Naučte se používat Azure DevTest Labs k vytváření a správě virtuálních počítačů pomocí Azure CLI 2.0"
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
ms.openlocfilehash: 42b0448c1bcdfa909715abd5075353d63cab8389
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-and-manage-virtual-machines-with-devtest-labs-using-the-azure-cli"></a><span data-ttu-id="7a3cf-103">Vytvářet a spravovat virtuální počítače s DevTest Labs pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="7a3cf-103">Create and manage virtual machines with DevTest Labs using the Azure CLI</span></span>
<span data-ttu-id="7a3cf-104">Tento úvodní vám pomohou vytvoření, spuštění, připojením, aktualizaci a mazání vývojovém počítači ve svém testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="7a3cf-104">This quick start will guide you through creating, starting, connecting, updating and cleaning up a development machine in your lab.</span></span> 

<span data-ttu-id="7a3cf-105">Než začnete:</span><span class="sxs-lookup"><span data-stu-id="7a3cf-105">Before you begin:</span></span>

* <span data-ttu-id="7a3cf-106">Pokud nebyl vytvořen testovacím prostředí, pokyny naleznete [zde](devtest-lab-create-lab.md).</span><span class="sxs-lookup"><span data-stu-id="7a3cf-106">If a lab has not been created, instructions can be found [here](devtest-lab-create-lab.md).</span></span>

* <span data-ttu-id="7a3cf-107">[Instalace rozhraní příkazového řádku 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="7a3cf-107">[Install CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).</span></span> <span data-ttu-id="7a3cf-108">Chcete-li spustit, spusťte az přihlášení k vytvoření připojení pomocí Azure.</span><span class="sxs-lookup"><span data-stu-id="7a3cf-108">To start, run az login to create a connection with Azure.</span></span> 

## <a name="create-and-verify-the-virtual-machine"></a><span data-ttu-id="7a3cf-109">Vytvoření a ověření, jestli virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="7a3cf-109">Create and verify the virtual machine</span></span> 
<span data-ttu-id="7a3cf-110">Vytvoření virtuálního počítače z image pořízenou prostřednictvím marketplace s ssh ověřování.</span><span class="sxs-lookup"><span data-stu-id="7a3cf-110">Create a VM from a marketplace image with ssh authentication.</span></span>
```azurecli
az lab vm create --lab-name sampleLabName --resource-group sampleLabResourceGroup --name sampleVMName --image "Ubuntu Server 16.04 LTS" --image-type gallery --size Standard_DS1_v2 --authentication-type  ssh --generate-ssh-keys --ip-configuration public 
```
> [!NOTE]
> <span data-ttu-id="7a3cf-111">Umístit **skupinu prostředků pro testovací prostředí** názvem v parametru – skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="7a3cf-111">Put the **lab's resource group** name in the --resource-group parameter.</span></span>
>

<span data-ttu-id="7a3cf-112">Pokud chcete vytvořit virtuální počítač pomocí vzorce, použijte--vzorce parametru v [az testovacího prostředí virtuálního počítače vytvořit](https://docs.microsoft.com/cli/azure/lab/vm#create).</span><span class="sxs-lookup"><span data-stu-id="7a3cf-112">If you want to create a VM using a formula, use the --formula parameter in [az lab vm create](https://docs.microsoft.com/cli/azure/lab/vm#create).</span></span>


<span data-ttu-id="7a3cf-113">Ověřte, že virtuální počítač je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="7a3cf-113">Verify that the VM is available.</span></span>
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

## <a name="start-and-connect-to-the-virtual-machine"></a><span data-ttu-id="7a3cf-114">Spuštění a připojení k virtuálnímu počítači</span><span class="sxs-lookup"><span data-stu-id="7a3cf-114">Start and connect to the virtual machine</span></span>
<span data-ttu-id="7a3cf-115">Spuštění virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="7a3cf-115">Start a VM.</span></span>
```azurecli
az lab vm start --lab-name sampleLabName --name sampleVMName --resource-group sampleLabResourceGroup
```
> [!NOTE]
> <span data-ttu-id="7a3cf-116">Umístit **skupinu prostředků pro testovací prostředí** názvem v parametru – skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="7a3cf-116">Put the **lab's resource group** name in the --resource-group parameter.</span></span>
>

<span data-ttu-id="7a3cf-117">Připojení k virtuálnímu počítači: [SSH](../virtual-machines/linux/mac-create-ssh-keys.md) nebo [vzdálené plochy](../virtual-machines/windows/connect-logon.md).</span><span class="sxs-lookup"><span data-stu-id="7a3cf-117">Connect to a VM: [SSH](../virtual-machines/linux/mac-create-ssh-keys.md) or [Remote Desktop](../virtual-machines/windows/connect-logon.md).</span></span>
```bash
ssh userName@ipAddressOrfqdn 
```

## <a name="update-the-virtual-machine"></a><span data-ttu-id="7a3cf-118">Aktualizovat virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="7a3cf-118">Update the virtual machine</span></span>
<span data-ttu-id="7a3cf-119">Artefakty se vztahují na virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="7a3cf-119">Apply artifacts to a VM.</span></span>
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

<span data-ttu-id="7a3cf-120">Zobrazí seznam artefaktů, které jsou k dispozici v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="7a3cf-120">List artifacts available in the lab.</span></span>
```azurecli
az lab vm show --lab-name sampleLabName --name sampleVMName --resource-group sampleResourceGroup --expand "properties(\$expand=artifacts)" --query 'artifacts[].{artifactId: artifactId, status: status}'
```
```json
{
  "artifactId": "/subscriptions/abcdeftgh1213123/resourceGroups/lisalab123RG822645/providers/Microsoft.DevTestLab/labs/lisalab123/artifactSources/public repo/artifacts/linux-install-nodejs",
  "status": "Succeeded"
}
```

## <a name="stop-and-delete-the-virtual-machine"></a><span data-ttu-id="7a3cf-121">Zastavení a odstranění virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="7a3cf-121">Stop and delete the virtual machine</span></span>    
<span data-ttu-id="7a3cf-122">Zastavení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="7a3cf-122">Stop a VM.</span></span>
```azurecli
az lab vm stop --lab-name sampleLabName --name sampleVMName --resource-group sampleResourceGroup
```

<span data-ttu-id="7a3cf-123">Odstraňte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="7a3cf-123">Delete a VM.</span></span>
```azurecli
az lab vm delete --lab-name sampleLabName --name sampleVMName --resource-group sampleResourceGroup
```

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]