---
title: "Virtuální počítač Azure PowerShell ukázky | Microsoft Docs"
description: "Virtuální počítač Azure PowerShell ukázky"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/01/2017
ms.author: nepeters
ms.openlocfilehash: 799a017a241ed3d37bb95344de7d50e1be7d559c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-virtual-machine-powershell-samples"></a><span data-ttu-id="cd940-103">Ukázek Azure PowerShell virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="cd940-103">Azure Virtual Machine PowerShell samples</span></span>

<span data-ttu-id="cd940-104">Následující tabulka obsahuje odkazy na ukázky skripty prostředí PowerShell, které vytvářet a spravovat virtuální počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="cd940-104">The following table includes links to PowerShell scripts samples that create and manage Linux virtual machines.</span></span>

| | |
|---|---|
|<span data-ttu-id="cd940-105">**Vytváření virtuálních počítačů**</span><span class="sxs-lookup"><span data-stu-id="cd940-105">**Create virtual machines**</span></span>||
| [<span data-ttu-id="cd940-106">Vytvoření kompletně nakonfigurovaný virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="cd940-106">Create a fully configured virtual machine</span></span>](./../scripts/virtual-machines-linux-powershell-sample-create-vm.md?toc=%2fpowershell%2fmodule%2ftoc.json) | <span data-ttu-id="cd940-107">Vytvoří skupinu prostředků, virtuální počítač a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="cd940-107">Creates a resource group, virtual machine, and all related resources.</span></span>|
| [<span data-ttu-id="cd940-108">Vytvoření virtuálního počítače s Docker povoleno</span><span class="sxs-lookup"><span data-stu-id="cd940-108">Create a VM with Docker enabled</span></span>](./../scripts/virtual-machines-linux-powershell-sample-create-docker-host.md?toc=%2fpowershell%2fmodule%2ftoc.json) | <span data-ttu-id="cd940-109">Vytvoří virtuální počítač, nakonfiguruje tento virtuální počítač jako hostitele Docker a spouští kontejner NGINX.</span><span class="sxs-lookup"><span data-stu-id="cd940-109">Creates a virtual machine, configures this VM as a Docker host, and runs an NGINX container.</span></span> |
| [<span data-ttu-id="cd940-110">Vytvořte virtuální počítač a spusťte skript konfigurace</span><span class="sxs-lookup"><span data-stu-id="cd940-110">Create a VM and run configuration script</span></span>](./../scripts/virtual-machines-linux-powershell-sample-create-vm-nginx.md?toc=%2fpowershell%2fmodule%2ftoc.json) | <span data-ttu-id="cd940-111">Vytvoří virtuální počítač a používá rozšíření Azure vlastní skript k instalaci NGINX.</span><span class="sxs-lookup"><span data-stu-id="cd940-111">Creates a virtual machine and uses the Azure Custom Script extension to install NGINX.</span></span> |
| [<span data-ttu-id="cd940-112">Vytvoření virtuálního počítače s WordPress nainstalován</span><span class="sxs-lookup"><span data-stu-id="cd940-112">Create a VM with WordPress installed</span></span>](./../scripts/virtual-machines-linux-powershell-sample-create-vm-wordpress.md?toc=%2fpowershell%2fmodule%2ftoc.json) | <span data-ttu-id="cd940-113">Vytvoří virtuální počítač a instalace aplikace WordPress pomocí rozšíření Azure vlastních skriptů.</span><span class="sxs-lookup"><span data-stu-id="cd940-113">Creates a virtual machine and uses the Azure Custom Script extension to install WordPress.</span></span> |
|<span data-ttu-id="cd940-114">**Monitorování virtuálních počítačů**</span><span class="sxs-lookup"><span data-stu-id="cd940-114">**Monitor virtual machines**</span></span>||
| [<span data-ttu-id="cd940-115">Monitorování virtuálních počítačů pomocí služby Operations Management Suite</span><span class="sxs-lookup"><span data-stu-id="cd940-115">Monitor a VM with Operations Management Suite</span></span>](./../scripts/virtual-machines-linux-powershell-sample-create-vm-oms.md?toc=%2fpowershell%2fmodule%2ftoc.json) | <span data-ttu-id="cd940-116">Vytvoří virtuální počítač, nainstaluje agenta nástroje Operations Management Suite a zaregistruje virtuální počítač v pracovním prostoru OMS.</span><span class="sxs-lookup"><span data-stu-id="cd940-116">Creates a virtual machine, installs the Operations Management Suite agent, and enrolls the VM in an OMS Workspace.</span></span>  |
| | |
