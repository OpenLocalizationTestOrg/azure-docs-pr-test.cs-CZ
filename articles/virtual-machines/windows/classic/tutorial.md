---
title: "Vytvoření virtuálního počítače na portálu Azure | Microsoft Docs"
description: "Vytvoření virtuálního počítače s Windows v portálu Azure."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 1871f823-ebd7-4eff-9a22-8e2411555595
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: cynthn
ms.openlocfilehash: 0981872ff819fdf49a9cc97afce3c212013ce76b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-virtual-machine-running-windows-in-the-azure-portal"></a><span data-ttu-id="34131-103">Vytvoření virtuálního počítače se systémem Windows na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="34131-103">Create a virtual machine running Windows in the Azure portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="34131-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="34131-104">Azure portal</span></span>](tutorial.md)
> * [<span data-ttu-id="34131-105">PowerShell: Nasazení Classic</span><span class="sxs-lookup"><span data-stu-id="34131-105">PowerShell: Classic deployment</span></span>](create-powershell.md)
>
>

<br>

> [!IMPORTANT]
> <span data-ttu-id="34131-106">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="34131-106">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="34131-107">Tento článek se zabývá pomocí modelu nasazení Classic.</span><span class="sxs-lookup"><span data-stu-id="34131-107">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="34131-108">Microsoft doporučuje, aby byl ve většině nových nasazení použit model Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="34131-108">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="34131-109">Zjistěte, jak [proveďte tyto kroky, pomocí modelu nasazení Resource Manager](../../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) pomocí **portál Azure**.</span><span class="sxs-lookup"><span data-stu-id="34131-109">Learn how to [perform these steps using the Resource Manager deployment model](../../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) using the **Azure portal**.</span></span>

<span data-ttu-id="34131-110">V tomto kurzu se dozvíte, jak vytvořit virtuální počítač Azure (VM) s Windows v portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="34131-110">This tutorial shows you how to create an Azure virtual machine (VM) running Windows in the Azure portal.</span></span> <span data-ttu-id="34131-111">Jako příklad použijeme bitovou kopii systému Windows Server, ale který je jen jednou z mnoha imagí, které Azure nabízí.</span><span class="sxs-lookup"><span data-stu-id="34131-111">We'll use a Windows Server image as an example, but that's just one of the many images Azure offers.</span></span> <span data-ttu-id="34131-112">Všimněte si, že tato volba image závisí na vaše předplatné.</span><span class="sxs-lookup"><span data-stu-id="34131-112">Note that your image choices depend on your subscription.</span></span> <span data-ttu-id="34131-113">Například může být plochy bitových kopií systému Windows k dispozici předplatitelům služby MSDN.</span><span class="sxs-lookup"><span data-stu-id="34131-113">For example, Windows desktop images may be available to MSDN subscribers.</span></span>

<span data-ttu-id="34131-114">V této části se dozvíte, jak používat **řídicí panel** na portálu Azure vyberte a poté vytvořit virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="34131-114">This section shows you how to use the **Dashboard** in the Azure portal to select and then create the virtual machine.</span></span>

<span data-ttu-id="34131-115">Můžete také vytvořit virtuální počítače pomocí [vlastní image](createupload-vhd.md).</span><span class="sxs-lookup"><span data-stu-id="34131-115">You can also create VMs using [your own images](createupload-vhd.md).</span></span> <span data-ttu-id="34131-116">Informace o tomto a dalších metod najdete v tématu [různé způsoby vytvoření virtuálního počítače s Windows](../../virtual-machines-windows-creation-choices.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="34131-116">To learn about this and other methods, see [Different ways to create a Windows virtual machine](../../virtual-machines-windows-creation-choices.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<!-- 02/27/2017 Video removed as it was based on the classic portal. -->

## <span data-ttu-id="34131-117"><a id="createvirtualmachine"></a>Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="34131-117"><a id="createvirtualmachine"> </a>Create the virtual machine</span></span>
[!INCLUDE [virtual-machines-create-WindowsVM](../../../../includes/virtual-machines-create-windowsvm.md)]

## <a name="next-steps"></a><span data-ttu-id="34131-118">Další kroky</span><span class="sxs-lookup"><span data-stu-id="34131-118">Next steps</span></span>
* <span data-ttu-id="34131-119">Zjistěte, jak [vytvoření virtuálního počítače pomocí modelu nasazení Resource Manager](../../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="34131-119">Learn how to [create a VM using the Resource Manager deployment model](../../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) in the Azure portal.</span></span>
* <span data-ttu-id="34131-120">Přihlaste se k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="34131-120">Log on to the virtual machine.</span></span> <span data-ttu-id="34131-121">Pokyny najdete v tématu [Přihlaste se k virtuálního počítače se systémem Windows Server](connect-logon.md).</span><span class="sxs-lookup"><span data-stu-id="34131-121">For instructions, see [Log on to a virtual machine running Windows Server](connect-logon.md).</span></span>
* <span data-ttu-id="34131-122">Připojte disk k uložení dat.</span><span class="sxs-lookup"><span data-stu-id="34131-122">Attach a disk to store data.</span></span> <span data-ttu-id="34131-123">Můžete připojit prázdné disky a disky, které obsahují data.</span><span class="sxs-lookup"><span data-stu-id="34131-123">You can attach both empty disks and disks that contain data.</span></span> <span data-ttu-id="34131-124">Pokyny najdete v tématu [připojit datový disk k virtuálnímu počítači Windows vytvořené pomocí modelu nasazení classic](attach-disk.md).</span><span class="sxs-lookup"><span data-stu-id="34131-124">For instructions, see the [Attach a data disk to a Windows virtual machine created with the classic deployment model](attach-disk.md).</span></span>
