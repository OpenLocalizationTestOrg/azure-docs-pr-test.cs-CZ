---
title: "aaaCreate klasické virtuální počítač s Linuxem pomocí hello 1.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Zjistěte, jak toocreate virtuální počítač s Linuxem pomocí Azure CLI 1.0 hello hello model nasazení Classic"
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
ms.openlocfilehash: 5c3c54e5d1444a79e8e609c76d04a3a621c8d03f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-classic-linux-vm-with-hello-azure-cli-10"></a><span data-ttu-id="cd377-103">Jak hello tooCreate a klasické virtuální počítač s Linuxem pomocí Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="cd377-103">How tooCreate a Classic Linux VM with hello Azure CLI 1.0</span></span>
> [!IMPORTANT] 
> <span data-ttu-id="cd377-104">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="cd377-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="cd377-105">Tento článek se zabývá pomocí modelu nasazení Classic hello.</span><span class="sxs-lookup"><span data-stu-id="cd377-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="cd377-106">Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="cd377-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="cd377-107">Verze hello Resource Manager, najdete v části [zde](../create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="cd377-107">For hello Resource Manager version, see [here](../create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="cd377-108">Toto téma popisuje, jak toocreate Linux virtuálního počítače (VM) s použitím hello Azure CLI 1.0 hello model nasazení Classic.</span><span class="sxs-lookup"><span data-stu-id="cd377-108">This topic describes how toocreate a Linux virtual machine (VM) with hello Azure CLI 1.0 using hello Classic deployment model.</span></span> <span data-ttu-id="cd377-109">Používáme image systému Linux z hello k dispozici **bitové kopie** v Azure.</span><span class="sxs-lookup"><span data-stu-id="cd377-109">We use a Linux image from hello available **IMAGES** on Azure.</span></span> <span data-ttu-id="cd377-110">příkazy Hello Azure CLI 1.0 poskytnout hello následující možnosti konfigurace, mimo jiné:</span><span class="sxs-lookup"><span data-stu-id="cd377-110">hello Azure CLI 1.0 commands give hello following configuration choices, among others:</span></span>

* <span data-ttu-id="cd377-111">Propojení virtuální sítě tooa hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="cd377-111">Connecting hello VM tooa virtual network</span></span>
* <span data-ttu-id="cd377-112">Přidávání virtuálních počítačů hello tooan stávající cloudovou službu</span><span class="sxs-lookup"><span data-stu-id="cd377-112">Adding hello VM tooan existing cloud service</span></span>
* <span data-ttu-id="cd377-113">Přidání hello virtuálních počítačů tooan stávající účet úložiště</span><span class="sxs-lookup"><span data-stu-id="cd377-113">Adding hello VM tooan existing storage account</span></span>
* <span data-ttu-id="cd377-114">Přidání nastavení dostupnosti tooan Virtuálního hello nebo umístění</span><span class="sxs-lookup"><span data-stu-id="cd377-114">Adding hello VM tooan availability set or location</span></span>

> [!IMPORTANT]
> <span data-ttu-id="cd377-115">Pokud chcete, aby váš počítač toouse virtuální sítě, aby se mohli připojit tooit přímo podle názvu hostitele nebo nastavte připojení mezi různými místy, ujistěte se, když vytvoříte hello virtuálního počítače zadejte hello virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="cd377-115">If you want your VM toouse a virtual network so you can connect tooit directly by hostname or set up cross-premises connections, make sure you specify hello virtual network when you create hello VM.</span></span> <span data-ttu-id="cd377-116">Virtuální počítač může být nakonfigurované toojoin virtuální sítě, pouze když vytváříte hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="cd377-116">A VM can be configured toojoin a virtual network only when you create hello VM.</span></span> <span data-ttu-id="cd377-117">Podrobnosti virtuální sítě najdete v tématu [Přehled virtuálních sítí Azure](http://go.microsoft.com/fwlink/p/?LinkID=294063).</span><span class="sxs-lookup"><span data-stu-id="cd377-117">For details on virtual networks, see [Azure Virtual Network Overview](http://go.microsoft.com/fwlink/p/?LinkID=294063).</span></span>
> 
> 

## <a name="how-toocreate-a-linux-vm-using-hello-classic-deployment-model"></a><span data-ttu-id="cd377-118">Jak hello toocreate a virtuální počítač s Linuxem pomocí modelu nasazení Classic</span><span class="sxs-lookup"><span data-stu-id="cd377-118">How toocreate a Linux VM using hello Classic deployment model</span></span>
[!INCLUDE [virtual-machines-create-LinuxVM](../../../../includes/virtual-machines-create-linuxvm.md)]

