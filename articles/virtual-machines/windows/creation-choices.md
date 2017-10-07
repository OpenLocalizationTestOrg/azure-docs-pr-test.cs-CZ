---
title: "aaaDifferent způsoby toocreate virtuální počítač s Windows v Azure | Microsoft Docs"
description: "Uvádí různé způsoby toocreate hello virtuálního počítače s Windows pomocí Resource Manageru."
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
ms.openlocfilehash: 2928d4daa9b44c4d3a5083092a82c9a7f7c69fae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="different-ways-toocreate-a-windows-virtual-machine"></a><span data-ttu-id="ecc52-103">Různé způsoby toocreate virtuálního počítače s Windows</span><span class="sxs-lookup"><span data-stu-id="ecc52-103">Different ways toocreate a Windows virtual machine</span></span>

<span data-ttu-id="ecc52-104">Azure nabízí různé způsoby toocreate vzhledem k tomu, že virtuální počítače jsou vhodné pro různé uživatele a účely virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="ecc52-104">Azure offers different ways toocreate a virtual machine because virtual machines are suited for different users and purposes.</span></span> <span data-ttu-id="ecc52-105">To znamená, je nutné toomake některé možnosti o hello virtuální počítač a jak toocreate ho.</span><span class="sxs-lookup"><span data-stu-id="ecc52-105">This means that you need toomake some choices about hello virtual machine and how toocreate it.</span></span> <span data-ttu-id="ecc52-106">Tento článek poskytuje souhrn tyto volby a propojuje tooinstructions.</span><span class="sxs-lookup"><span data-stu-id="ecc52-106">This article gives you a summary of these choices and links tooinstructions.</span></span>

## <a name="azure-portal"></a><span data-ttu-id="ecc52-107">portál Azure</span><span class="sxs-lookup"><span data-stu-id="ecc52-107">Azure portal</span></span>
<span data-ttu-id="ecc52-108">Použití hello portál Azure je jednoduchý způsob tootry na virtuálním počítači, zejména v případě, že jste právě začínáte s Azure.</span><span class="sxs-lookup"><span data-stu-id="ecc52-108">Using hello Azure portal is a simple way tootry out a virtual machine, especially if you're just starting out with Azure.</span></span> 

[<span data-ttu-id="ecc52-109">Vytvoření virtuálního počítače se systémem Windows pomocí portálu hello</span><span class="sxs-lookup"><span data-stu-id="ecc52-109">Create a virtual machine running Windows using hello portal</span></span>](../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="template"></a><span data-ttu-id="ecc52-110">Šablona</span><span class="sxs-lookup"><span data-stu-id="ecc52-110">Template</span></span>
<span data-ttu-id="ecc52-111">Virtuální počítače vyžaduje kombinaci prostředků (například s dostupností sad a účty úložiště).</span><span class="sxs-lookup"><span data-stu-id="ecc52-111">Virtual machines require a combination of resources (such as a availability sets and storage accounts).</span></span> <span data-ttu-id="ecc52-112">Namísto nasazení a správa každého prostředku samostatně, můžete vytvořit šablonu Azure Resource Manager, která nasazuje a zřizuje všechny hello prostředky v rámci jediné koordinované operace.</span><span class="sxs-lookup"><span data-stu-id="ecc52-112">Rather than deploying and managing each resource separately, you can create an Azure Resource Manager template that deploys and provisions all of hello resources in a single, coordinated operation.</span></span>

* [<span data-ttu-id="ecc52-113">Vytvoření virtuálního počítače s Windows pomocí šablony Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="ecc52-113">Create a Windows virtual machine with a Resource Manager template</span></span>](ps-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="azure-powershell"></a><span data-ttu-id="ecc52-114">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="ecc52-114">Azure PowerShell</span></span>
<span data-ttu-id="ecc52-115">Pokud dáváte přednost práci v příkazovém řádku, můžete použít Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ecc52-115">If you prefer working in a command shell, you can use Azure PowerShell.</span></span>

* [<span data-ttu-id="ecc52-116">Vytvoření virtuálního počítače s Windows pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="ecc52-116">Create a Windows VM using PowerShell</span></span>](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="visual-studio"></a><span data-ttu-id="ecc52-117">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ecc52-117">Visual Studio</span></span>
<span data-ttu-id="ecc52-118">Pomocí sady Visual Studio toobuild, spravovat a nasazení virtuálních počítačů pomocí nástroje hello Azure pro sadu Visual Studio a hello Azure SDK.</span><span class="sxs-lookup"><span data-stu-id="ecc52-118">Use Visual Studio toobuild, manage, and deploy VMs with hello Azure Tools for Visual Studio and hello Azure SDK.</span></span>

[<span data-ttu-id="ecc52-119">Nástroje Azure pro sadu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ecc52-119">Azure Tools for Visual Studio</span></span>](https://www.visualstudio.com/features/azure-tools-vs)

