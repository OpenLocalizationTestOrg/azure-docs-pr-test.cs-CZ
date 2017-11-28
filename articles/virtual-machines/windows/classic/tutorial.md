---
title: "aaaCreate virtuálního počítače v hello portálu Azure | Microsoft Docs"
description: "Vytvoření virtuálního počítače s Windows v hello portálu Azure."
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
ms.openlocfilehash: 3848a3d06a7ce377633db78ea385826ed40f94fd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-machine-running-windows-in-hello-azure-portal"></a><span data-ttu-id="bea65-103">Vytvoření virtuálního počítače se systémem Windows v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="bea65-103">Create a virtual machine running Windows in hello Azure portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="bea65-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="bea65-104">Azure portal</span></span>](tutorial.md)
> * [<span data-ttu-id="bea65-105">PowerShell: Nasazení Classic</span><span class="sxs-lookup"><span data-stu-id="bea65-105">PowerShell: Classic deployment</span></span>](create-powershell.md)
>
>

<br>

> [!IMPORTANT]
> <span data-ttu-id="bea65-106">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="bea65-106">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="bea65-107">Tento článek se zabývá pomocí modelu nasazení Classic hello.</span><span class="sxs-lookup"><span data-stu-id="bea65-107">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="bea65-108">Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="bea65-108">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="bea65-109">Zjistěte, jak příliš[proveďte tyto kroky, pomocí modelu nasazení Resource Manager hello](../../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) pomocí hello **portál Azure**.</span><span class="sxs-lookup"><span data-stu-id="bea65-109">Learn how too[perform these steps using hello Resource Manager deployment model](../../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) using hello **Azure portal**.</span></span>

<span data-ttu-id="bea65-110">Tento kurz ukazuje, jak toocreate Azure virtuálního počítače (VM) s Windows v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="bea65-110">This tutorial shows you how toocreate an Azure virtual machine (VM) running Windows in hello Azure portal.</span></span> <span data-ttu-id="bea65-111">Jako příklad použijeme bitovou kopii systému Windows Server, ale který je jen jednou z hello mnoho Image Azure nabízí.</span><span class="sxs-lookup"><span data-stu-id="bea65-111">We'll use a Windows Server image as an example, but that's just one of hello many images Azure offers.</span></span> <span data-ttu-id="bea65-112">Všimněte si, že tato volba image závisí na vaše předplatné.</span><span class="sxs-lookup"><span data-stu-id="bea65-112">Note that your image choices depend on your subscription.</span></span> <span data-ttu-id="bea65-113">Například stolní bitových kopií systému Windows může být k dispozici tooMSDN odběratele.</span><span class="sxs-lookup"><span data-stu-id="bea65-113">For example, Windows desktop images may be available tooMSDN subscribers.</span></span>

<span data-ttu-id="bea65-114">V této části se dozvíte, jak toouse hello **řídicí panel** v hello Azure tooselect portálu a poté vytvořit hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="bea65-114">This section shows you how toouse hello **Dashboard** in hello Azure portal tooselect and then create hello virtual machine.</span></span>

<span data-ttu-id="bea65-115">Můžete také vytvořit virtuální počítače pomocí [vlastní image](createupload-vhd.md).</span><span class="sxs-lookup"><span data-stu-id="bea65-115">You can also create VMs using [your own images](createupload-vhd.md).</span></span> <span data-ttu-id="bea65-116">toolearn o tomto a dalších metod najdete v části [různé způsoby toocreate virtuálního počítače s Windows](../../virtual-machines-windows-creation-choices.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="bea65-116">toolearn about this and other methods, see [Different ways toocreate a Windows virtual machine](../../virtual-machines-windows-creation-choices.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<!-- 02/27/2017 Video removed as it was based on hello classic portal. -->

## <span data-ttu-id="bea65-117"><a id="createvirtualmachine"></a>Vytvoření hello virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="bea65-117"><a id="createvirtualmachine"> </a>Create hello virtual machine</span></span>
[!INCLUDE [virtual-machines-create-WindowsVM](../../../../includes/virtual-machines-create-windowsvm.md)]

## <a name="next-steps"></a><span data-ttu-id="bea65-118">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bea65-118">Next steps</span></span>
* <span data-ttu-id="bea65-119">Zjistěte, jak příliš[vytvoření virtuálního počítače pomocí modelu nasazení Resource Manager hello](../../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="bea65-119">Learn how too[create a VM using hello Resource Manager deployment model](../../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) in hello Azure portal.</span></span>
* <span data-ttu-id="bea65-120">Přihlaste se toohello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="bea65-120">Log on toohello virtual machine.</span></span> <span data-ttu-id="bea65-121">Pokyny najdete v tématu [přihlásit tooa virtuální počítač se systémem Windows Server](connect-logon.md).</span><span class="sxs-lookup"><span data-stu-id="bea65-121">For instructions, see [Log on tooa virtual machine running Windows Server](connect-logon.md).</span></span>
* <span data-ttu-id="bea65-122">Připojte datový disk toostore.</span><span class="sxs-lookup"><span data-stu-id="bea65-122">Attach a disk toostore data.</span></span> <span data-ttu-id="bea65-123">Můžete připojit prázdné disky a disky, které obsahují data.</span><span class="sxs-lookup"><span data-stu-id="bea65-123">You can attach both empty disks and disks that contain data.</span></span> <span data-ttu-id="bea65-124">Pokyny najdete v tématu hello [připojit datový disk tooa virtuálního počítače s Windows vytvořené pomocí modelu nasazení classic hello](attach-disk.md).</span><span class="sxs-lookup"><span data-stu-id="bea65-124">For instructions, see hello [Attach a data disk tooa Windows virtual machine created with hello classic deployment model](attach-disk.md).</span></span>
