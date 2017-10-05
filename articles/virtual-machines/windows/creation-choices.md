---
title: "Různé způsoby vytvoření virtuálního počítače s Windows v Azure | Microsoft Docs"
description: "Uvádí různé způsoby vytvoření virtuálního počítače s Windows pomocí Resource Manageru."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 809ba8f4-b54e-43c5-bbe3-8e710c49971f
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/02/2017
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5e51c49aac01a22d86c7c1a12b2f2ca7ddc056bc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="different-ways-to-create-a-windows-virtual-machine"></a><span data-ttu-id="9dd4b-103">Různé způsoby vytvoření virtuálního počítače s Windows</span><span class="sxs-lookup"><span data-stu-id="9dd4b-103">Different ways to create a Windows virtual machine</span></span>

<span data-ttu-id="9dd4b-104">Azure nabízí různé způsoby vytvoření virtuálního počítače, protože virtuální počítače jsou vhodné pro různé uživatele a účely.</span><span class="sxs-lookup"><span data-stu-id="9dd4b-104">Azure offers different ways to create a virtual machine because virtual machines are suited for different users and purposes.</span></span> <span data-ttu-id="9dd4b-105">To znamená, že budete muset některé rozhodování o virtuální počítač a jak ji vytvořit.</span><span class="sxs-lookup"><span data-stu-id="9dd4b-105">This means that you need to make some choices about the virtual machine and how to create it.</span></span> <span data-ttu-id="9dd4b-106">Tento článek poskytuje souhrn volby a odkazy na pokyny.</span><span class="sxs-lookup"><span data-stu-id="9dd4b-106">This article gives you a summary of these choices and links to instructions.</span></span>

## <a name="azure-portal"></a><span data-ttu-id="9dd4b-107">portál Azure</span><span class="sxs-lookup"><span data-stu-id="9dd4b-107">Azure portal</span></span>
<span data-ttu-id="9dd4b-108">Pomocí portálu Azure je jednoduchý způsob, jak vyzkoušet na virtuálním počítači, zejména v případě, že jste právě začínáte s Azure.</span><span class="sxs-lookup"><span data-stu-id="9dd4b-108">Using the Azure portal is a simple way to try out a virtual machine, especially if you're just starting out with Azure.</span></span> 

[<span data-ttu-id="9dd4b-109">Vytvoření virtuálního počítače s Windows pomocí portálu</span><span class="sxs-lookup"><span data-stu-id="9dd4b-109">Create a virtual machine running Windows using the portal</span></span>](../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="template"></a><span data-ttu-id="9dd4b-110">Šablona</span><span class="sxs-lookup"><span data-stu-id="9dd4b-110">Template</span></span>
<span data-ttu-id="9dd4b-111">Virtuální počítače vyžaduje kombinaci prostředků (například s dostupností sad a účty úložiště).</span><span class="sxs-lookup"><span data-stu-id="9dd4b-111">Virtual machines require a combination of resources (such as a availability sets and storage accounts).</span></span> <span data-ttu-id="9dd4b-112">Namísto nasazení a správa každého prostředku samostatně, můžete vytvořit šablonu Azure Resource Manager, která nasazuje a zřizuje všechny prostředky v rámci jediné koordinované operace.</span><span class="sxs-lookup"><span data-stu-id="9dd4b-112">Rather than deploying and managing each resource separately, you can create an Azure Resource Manager template that deploys and provisions all of the resources in a single, coordinated operation.</span></span>

* [<span data-ttu-id="9dd4b-113">Vytvoření virtuálního počítače s Windows pomocí šablony Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="9dd4b-113">Create a Windows virtual machine with a Resource Manager template</span></span>](ps-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="azure-powershell"></a><span data-ttu-id="9dd4b-114">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="9dd4b-114">Azure PowerShell</span></span>
<span data-ttu-id="9dd4b-115">Pokud dáváte přednost práci v příkazovém řádku, můžete použít Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="9dd4b-115">If you prefer working in a command shell, you can use Azure PowerShell.</span></span>

* [<span data-ttu-id="9dd4b-116">Vytvoření virtuálního počítače s Windows pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="9dd4b-116">Create a Windows VM using PowerShell</span></span>](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="visual-studio"></a><span data-ttu-id="9dd4b-117">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9dd4b-117">Visual Studio</span></span>
<span data-ttu-id="9dd4b-118">Pomocí sady Visual Studio k vytvoření, správě a nasazení virtuálních počítačů pomocí nástroje Azure pro Visual Studio a sady Azure SDK.</span><span class="sxs-lookup"><span data-stu-id="9dd4b-118">Use Visual Studio to build, manage, and deploy VMs with the Azure Tools for Visual Studio and the Azure SDK.</span></span>

[<span data-ttu-id="9dd4b-119">Nástroje Azure pro sadu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9dd4b-119">Azure Tools for Visual Studio</span></span>](https://www.visualstudio.com/features/azure-tools-vs)

