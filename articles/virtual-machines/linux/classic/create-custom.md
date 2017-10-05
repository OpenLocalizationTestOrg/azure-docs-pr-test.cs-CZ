---
title: "Vytvoření klasického virtuálního počítače s Linuxem pomocí Azure CLI 1.0 | Microsoft Docs"
description: "Naučte se vytvořit virtuální počítač s Linuxem pomocí Azure CLI 1.0 pomocí modelu nasazení Classic"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: f8071a2e-ed91-4f77-87d9-519a37e5364f
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/09/2017
ms.author: iainfou
ms.openlocfilehash: 8ddbacbbb70c0cf1a2537fab4d981a316610a4d7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-create-a-classic-linux-vm-with-the-azure-cli-10"></a><span data-ttu-id="1773f-103">Postup vytvoření klasické virtuální počítač s Linuxem pomocí rozhraní příkazového řádku Azure 1.0</span><span class="sxs-lookup"><span data-stu-id="1773f-103">How to Create a Classic Linux VM with the Azure CLI 1.0</span></span>
> [!IMPORTANT] 
> <span data-ttu-id="1773f-104">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="1773f-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="1773f-105">Tento článek se zabývá pomocí modelu nasazení Classic.</span><span class="sxs-lookup"><span data-stu-id="1773f-105">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="1773f-106">Microsoft doporučuje, aby byl ve většině nových nasazení použit model Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="1773f-106">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="1773f-107">Resource Manager verze, najdete v části [zde](../create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="1773f-107">For the Resource Manager version, see [here](../create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="1773f-108">Toto téma popisuje, jak vytvořit virtuální počítač s Linuxem (VM) s Azure CLI 1.0 pomocí modelu nasazení Classic.</span><span class="sxs-lookup"><span data-stu-id="1773f-108">This topic describes how to create a Linux virtual machine (VM) with the Azure CLI 1.0 using the Classic deployment model.</span></span> <span data-ttu-id="1773f-109">Můžeme použít bitovou kopii systému Linux z dostupných **bitové kopie** v Azure.</span><span class="sxs-lookup"><span data-stu-id="1773f-109">We use a Linux image from the available **IMAGES** on Azure.</span></span> <span data-ttu-id="1773f-110">Příkazy Azure CLI 1.0 poskytují následující možnosti konfigurace, mimo jiné:</span><span class="sxs-lookup"><span data-stu-id="1773f-110">The Azure CLI 1.0 commands give the following configuration choices, among others:</span></span>

* <span data-ttu-id="1773f-111">Připojení virtuálního počítače k virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="1773f-111">Connecting the VM to a virtual network</span></span>
* <span data-ttu-id="1773f-112">Přidání virtuálního počítače do existující cloudové služby</span><span class="sxs-lookup"><span data-stu-id="1773f-112">Adding the VM to an existing cloud service</span></span>
* <span data-ttu-id="1773f-113">Přidání virtuálního počítače do stávající účet úložiště</span><span class="sxs-lookup"><span data-stu-id="1773f-113">Adding the VM to an existing storage account</span></span>
* <span data-ttu-id="1773f-114">Přidání virtuálního počítače do skupiny dostupnosti nebo umístění</span><span class="sxs-lookup"><span data-stu-id="1773f-114">Adding the VM to an availability set or location</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1773f-115">Pokud chcete virtuální počítač používat virtuální síť, takže k nim mohla připojit přímo podle názvu hostitele nebo nastavte připojení mezi různými místy, ujistěte se, že zadáte virtuální sítě, při vytváření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="1773f-115">If you want your VM to use a virtual network so you can connect to it directly by hostname or set up cross-premises connections, make sure you specify the virtual network when you create the VM.</span></span> <span data-ttu-id="1773f-116">Virtuální počítač se dá nakonfigurovat pro připojení k virtuální síti, pouze když vytváříte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="1773f-116">A VM can be configured to join a virtual network only when you create the VM.</span></span> <span data-ttu-id="1773f-117">Podrobnosti virtuální sítě najdete v tématu [Přehled virtuálních sítí Azure](http://go.microsoft.com/fwlink/p/?LinkID=294063).</span><span class="sxs-lookup"><span data-stu-id="1773f-117">For details on virtual networks, see [Azure Virtual Network Overview](http://go.microsoft.com/fwlink/p/?LinkID=294063).</span></span>
> 
> 

## <a name="how-to-create-a-linux-vm-using-the-classic-deployment-model"></a><span data-ttu-id="1773f-118">Postup vytvoření virtuálního počítače s Linuxem pomocí modelu nasazení Classic</span><span class="sxs-lookup"><span data-stu-id="1773f-118">How to create a Linux VM using the Classic deployment model</span></span>
[!INCLUDE [virtual-machines-create-LinuxVM](../../../../includes/virtual-machines-create-linuxvm.md)]

