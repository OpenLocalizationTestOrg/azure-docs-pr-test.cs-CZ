---
title: "Rozhraní příkazového řádku Azure ukázky Windows | Microsoft Docs"
description: "Rozhraní příkazového řádku Azure ukázky Windows"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 02/27/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: f4b2e8a5583855df7472af3fbef01ac641caf6bf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-cli-samples-for-windows-virtual-machines"></a><span data-ttu-id="8df02-103">Virtuální počítače Azure CLI ukázky pro Windows</span><span class="sxs-lookup"><span data-stu-id="8df02-103">Azure CLI Samples for Windows virtual machines</span></span>

<span data-ttu-id="8df02-104">Následující tabulka obsahuje odkazy na bash skripty vytvořené pomocí rozhraní příkazového řádku Azure, které nasadit virtuální počítače s Windows.</span><span class="sxs-lookup"><span data-stu-id="8df02-104">The following table includes links to bash scripts built using the Azure CLI that deploy Windows virtual machines.</span></span>

| | |
|---|---|
|<span data-ttu-id="8df02-105">**Vytváření virtuálních počítačů**</span><span class="sxs-lookup"><span data-stu-id="8df02-105">**Create virtual machines**</span></span>||
| [<span data-ttu-id="8df02-106">Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="8df02-106">Create a virtual machine</span></span>](./../scripts/virtual-machines-windows-cli-sample-create-vm-quick-create.md?toc=%2fcli%2fazure%2ftoc.json) | <span data-ttu-id="8df02-107">Vytvoří virtuální počítač Windows s minimální konfigurací.</span><span class="sxs-lookup"><span data-stu-id="8df02-107">Creates a Windows virtual machine with minimal configuration.</span></span> |
| [<span data-ttu-id="8df02-108">Vytvoření kompletně nakonfigurovaný virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="8df02-108">Create a fully configured virtual machine</span></span>](./../scripts/virtual-machines-windows-cli-sample-create-vm.md?toc=%2fcli%2fazure%2ftoc.json) | <span data-ttu-id="8df02-109">Vytvoří skupinu prostředků, virtuální počítač a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="8df02-109">Creates a resource group, virtual machine, and all related resources.</span></span>|
| [<span data-ttu-id="8df02-110">Vytvoření vysoce dostupné virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="8df02-110">Create highly available virtual machines</span></span>](./../scripts/virtual-machines-windows-cli-sample-nlb.md?toc=%2fcli%2fazure%2ftoc.json) | <span data-ttu-id="8df02-111">Vytvoří několik virtuálních počítačů v s vysokou dostupností a konfigurace skupinu s vyrovnáváním zatížení.</span><span class="sxs-lookup"><span data-stu-id="8df02-111">Creates several virtual machines in a highly available and load balanced configuration.</span></span> |
| [<span data-ttu-id="8df02-112">Vytvořte virtuální počítač a spusťte skript konfigurace</span><span class="sxs-lookup"><span data-stu-id="8df02-112">Create a VM and run configuration script</span></span>](./../scripts/virtual-machines-windows-cli-sample-create-vm-iis.md?toc=%2fcli%2fazure%2ftoc.json) | <span data-ttu-id="8df02-113">Vytvoří virtuální počítač a používá rozšíření Azure vlastní skript k instalaci služby IIS.</span><span class="sxs-lookup"><span data-stu-id="8df02-113">Creates a virtual machine and uses the Azure Custom Script extension to install IIS.</span></span> |
| [<span data-ttu-id="8df02-114">Vytvořte virtuální počítač a spusťte konfigurace DSC</span><span class="sxs-lookup"><span data-stu-id="8df02-114">Create a VM and run DSC configuration</span></span>](./../scripts/virtual-machines-windows-cli-sample-create-iis-using-dsc.md?toc=%2fcli%2fazure%2ftoc.json) | <span data-ttu-id="8df02-115">Vytvoří virtuální počítač a rozšíření Azure požadovaného stavu konfigurace (DSC) používá k instalaci služby IIS.</span><span class="sxs-lookup"><span data-stu-id="8df02-115">Creates a virtual machine and uses the Azure Desired State Configuration (DSC) extension to install IIS.</span></span> |
|<span data-ttu-id="8df02-116">**Síť virtuálních počítačů**</span><span class="sxs-lookup"><span data-stu-id="8df02-116">**Network virtual machines**</span></span>||
| [<span data-ttu-id="8df02-117">Zabezpečení síťového provozu mezi virtuálními počítači</span><span class="sxs-lookup"><span data-stu-id="8df02-117">Secure network traffic between virtual machines</span></span>](./../scripts/virtual-machines-windows-cli-sample-create-vm-nsg.md?toc=%2fcli%2fazure%2ftoc.json) | <span data-ttu-id="8df02-118">Vytvoří dva virtuální počítače, všechny související prostředky a skupiny zabezpečení k interní a externí sítě (NSG).</span><span class="sxs-lookup"><span data-stu-id="8df02-118">Creates two virtual machines, all related resources, and an internal and external network security groups (NSG).</span></span> |
|<span data-ttu-id="8df02-119">**Zabezpečený virtuální počítače**</span><span class="sxs-lookup"><span data-stu-id="8df02-119">**Secure virtual machines**</span></span>||
| [<span data-ttu-id="8df02-120">Šifrování virtuálních počítačů a datovými disky</span><span class="sxs-lookup"><span data-stu-id="8df02-120">Encrypt a VM and data disks</span></span>](./../scripts/virtual-machines-windows-cli-sample-encrypt-vm.md?toc=%2fcli%2fazure%2ftoc.json) | <span data-ttu-id="8df02-121">Vytvoří Azure Key Vault, šifrovací klíč a instanční objekt a potom šifruje virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="8df02-121">Creates an Azure Key Vault, encryption key, and service principal, then encrypts a VM.</span></span> |
|<span data-ttu-id="8df02-122">**Monitorování virtuálních počítačů**</span><span class="sxs-lookup"><span data-stu-id="8df02-122">**Monitor virtual machines**</span></span>||
| [<span data-ttu-id="8df02-123">Monitorování virtuálních počítačů pomocí služby Operations Management Suite</span><span class="sxs-lookup"><span data-stu-id="8df02-123">Monitor a VM with Operations Management Suite</span></span>](./../scripts/virtual-machines-windows-cli-sample-create-vm-oms.md?toc=%2fcli%2fazure%2ftoc.json) | <span data-ttu-id="8df02-124">Vytvoří virtuální počítač, nainstaluje agenta nástroje Operations Management Suite a zaregistruje virtuální počítač v pracovním prostoru OMS.</span><span class="sxs-lookup"><span data-stu-id="8df02-124">Creates a virtual machine, installs the Operations Management Suite agent, and enrolls the VM in an OMS Workspace.</span></span>  |
| | |
