---
title: "aaaRedeploy virtuální počítače s Linuxem v Azure | Microsoft Docs"
description: "Jak problémy tooredeploy Linuxové virtuální počítače v Azure toomitigate připojení SSH."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
tags: azure-resource-manager,top-support-issue
ms.assetid: e9530dd6-f5b0-4160-b36b-d75151d99eb7
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: support-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/23/2017
ms.author: iainfou
ms.openlocfilehash: 9adfd1b11f262d362133366b2bba5e69c70c9b82
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="redeploy-linux-virtual-machine-toonew-azure-node"></a><span data-ttu-id="9da79-103">Znovu nasaďte toonew Linux virtuálního počítače Azure uzlu</span><span class="sxs-lookup"><span data-stu-id="9da79-103">Redeploy Linux virtual machine toonew Azure node</span></span>
<span data-ttu-id="9da79-104">Pokud čelí potíží při řešení potíží s SSH nebo aplikace přistupovat ke tooa Linuxový virtuální počítač (VM) v Azure, mohou pomoci opětovného nasazení hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="9da79-104">If you face difficulties troubleshooting SSH or application access tooa Linux virtual machine (VM) in Azure, redeploying hello VM may help.</span></span> <span data-ttu-id="9da79-105">Při opětovném nasazování virtuálního počítače, přesune nový uzel tooa hello virtuálních počítačů v rámci hello infrastrukturu Azure a pak ho znovu zapne.</span><span class="sxs-lookup"><span data-stu-id="9da79-105">When you redeploy a VM, it moves hello VM tooa new node within hello Azure infrastructure and then powers it back on.</span></span> <span data-ttu-id="9da79-106">Možnosti konfigurace a přidružené prostředky zůstanou zachovány.</span><span class="sxs-lookup"><span data-stu-id="9da79-106">All your configuration options and associated resources are retained.</span></span> <span data-ttu-id="9da79-107">Tento článek ukazuje, jak tooredeploy a virtuálního počítače pomocí rozhraní příkazového řádku Azure nebo hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="9da79-107">This article shows you how tooredeploy a VM using Azure CLI or hello Azure portal.</span></span>

> [!NOTE]
> <span data-ttu-id="9da79-108">Po opětovném virtuálního počítače, dojde ke ztrátě dočasným diskovým hello a dynamické IP adresy přidružené k rozhraní virtuální sítě se aktualizují.</span><span class="sxs-lookup"><span data-stu-id="9da79-108">After you redeploy a VM, hello temporary disk is lost and dynamic IP addresses associated with virtual network interface are updated.</span></span> 

<span data-ttu-id="9da79-109">Můžete znovu nasadit virtuální počítač pomocí jedné z hello následující možnosti.</span><span class="sxs-lookup"><span data-stu-id="9da79-109">You can redeploy a VM using one of hello following options.</span></span> <span data-ttu-id="9da79-110">Je třeba pouze jednu možnost tooredeploy toochoose virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="9da79-110">You only need toochoose one option tooredeploy your VM:</span></span>

- [<span data-ttu-id="9da79-111">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="9da79-111">Azure CLI 2.0</span></span>](#azure-cli-20)
- [<span data-ttu-id="9da79-112">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="9da79-112">Azure CLI 1.0</span></span>](#azure-cli-10)
- [<span data-ttu-id="9da79-113">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="9da79-113">Azure portal</span></span>](#using-azure-portal)

## <a name="use-hello-azure-cli-20"></a><span data-ttu-id="9da79-114">Hello použití Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="9da79-114">Use hello Azure CLI 2.0</span></span>
<span data-ttu-id="9da79-115">Instalace hello nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) a přihlaste se pomocí účtu Azure tooan [az přihlášení](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="9da79-115">Install hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) and log in tooan Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="9da79-116">Opětovné nasazení virtuálního počítače s [az virtuálního počítače znovu ho zaveďte](/cli/azure/vm#redeploy).</span><span class="sxs-lookup"><span data-stu-id="9da79-116">Redeploy your VM with [az vm redeploy](/cli/azure/vm#redeploy).</span></span> <span data-ttu-id="9da79-117">Následující příklad opětovně nasadí Hello hello virtuálního počítače s názvem *Můjvp* v hello skupinu prostředků s názvem *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="9da79-117">hello following example redeploys hello VM named *myVM* in hello resource group named *myResourceGroup*:</span></span>

```azurecli
az vm redeploy --resource-group myResourceGroup --name myVM 
```

## <a name="use-hello-azure-cli-10"></a><span data-ttu-id="9da79-118">Hello použití Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="9da79-118">Use hello Azure CLI 1.0</span></span>
<span data-ttu-id="9da79-119">Nainstalujte hello [nejnovější Azure CLI 1.0](../../cli-install-nodejs.md), přihlaste se tooan účet Azure a ujistěte se, že jste v režimu Resource Manager (`azure config mode arm`).</span><span class="sxs-lookup"><span data-stu-id="9da79-119">Install hello [latest Azure CLI 1.0](../../cli-install-nodejs.md), log in tooan Azure account, and make sure that you are in Resource Manager mode (`azure config mode arm`).</span></span>

<span data-ttu-id="9da79-120">Následující příklad opětovně nasadí Hello hello virtuálního počítače s názvem *Můjvp* v hello skupinu prostředků s názvem *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="9da79-120">hello following example redeploys hello VM named *myVM* in hello resource group named *myResourceGroup*:</span></span>

```azurecli
azure vm redeploy --resource-group myResourceGroup --vm-name myVM 
```

[!INCLUDE [virtual-machines-common-redeploy-to-new-node](../../../includes/virtual-machines-common-redeploy-to-new-node.md)]

## <a name="next-steps"></a><span data-ttu-id="9da79-121">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9da79-121">Next steps</span></span>
<span data-ttu-id="9da79-122">Pokud máte problémy s připojením tooyour virtuálních počítačů, najdete konkrétní pomoc na [řešení potíží s připojení SSH](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) nebo [podrobné SSH při řešení potíží](detailed-troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="9da79-122">If you are having issues connecting tooyour VM, you can find specific help on [troubleshooting SSH connections](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) or [detailed SSH troubleshooting steps](detailed-troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="9da79-123">Pokud nelze získat přístup k aplikaci běžící na vašem virtuálním počítači, můžete si také přečíst [řešení potíží s aplikací](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="9da79-123">If you cannot access an application running on your VM, you can also read [application troubleshooting issues](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

