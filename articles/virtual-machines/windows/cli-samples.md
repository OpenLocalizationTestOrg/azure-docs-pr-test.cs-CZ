---
title: "aaaAzure Windows ukázky rozhraní příkazového řádku | Microsoft Docs"
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
ms.openlocfilehash: 1eef61a24d14897dd0a88a3f467854cc21b1938d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cli-samples-for-windows-virtual-machines"></a><span data-ttu-id="980c6-103">Virtuální počítače Azure CLI ukázky pro Windows</span><span class="sxs-lookup"><span data-stu-id="980c6-103">Azure CLI Samples for Windows virtual machines</span></span>

<span data-ttu-id="980c6-104">Hello následující tabulka obsahuje odkazy toobash skripty, vytvořené pomocí rozhraní příkazového řádku Azure hello které nasadit virtuální počítače s Windows.</span><span class="sxs-lookup"><span data-stu-id="980c6-104">hello following table includes links toobash scripts built using hello Azure CLI that deploy Windows virtual machines.</span></span>

| | |
|---|---|
|<span data-ttu-id="980c6-105">**Vytváření virtuálních počítačů**</span><span class="sxs-lookup"><span data-stu-id="980c6-105">**Create virtual machines**</span></span>||
| [<span data-ttu-id="980c6-106">Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="980c6-106">Create a virtual machine</span></span>](./../scripts/virtual-machines-windows-cli-sample-create-vm-quick-create.md?toc=%2fcli%2fazure%2ftoc.json) | <span data-ttu-id="980c6-107">Vytvoří virtuální počítač Windows s minimální konfigurací.</span><span class="sxs-lookup"><span data-stu-id="980c6-107">Creates a Windows virtual machine with minimal configuration.</span></span> |
| [<span data-ttu-id="980c6-108">Vytvoření kompletně nakonfigurovaný virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="980c6-108">Create a fully configured virtual machine</span></span>](./../scripts/virtual-machines-windows-cli-sample-create-vm.md?toc=%2fcli%2fazure%2ftoc.json) | <span data-ttu-id="980c6-109">Vytvoří skupinu prostředků, virtuální počítač a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="980c6-109">Creates a resource group, virtual machine, and all related resources.</span></span>|
| [<span data-ttu-id="980c6-110">Vytvoření vysoce dostupné virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="980c6-110">Create highly available virtual machines</span></span>](./../scripts/virtual-machines-windows-cli-sample-nlb.md?toc=%2fcli%2fazure%2ftoc.json) | <span data-ttu-id="980c6-111">Vytvoří několik virtuálních počítačů v s vysokou dostupností a konfigurace skupinu s vyrovnáváním zatížení.</span><span class="sxs-lookup"><span data-stu-id="980c6-111">Creates several virtual machines in a highly available and load balanced configuration.</span></span> |
| [<span data-ttu-id="980c6-112">Vytvořte virtuální počítač a spusťte skript konfigurace</span><span class="sxs-lookup"><span data-stu-id="980c6-112">Create a VM and run configuration script</span></span>](./../scripts/virtual-machines-windows-cli-sample-create-vm-iis.md?toc=%2fcli%2fazure%2ftoc.json) | <span data-ttu-id="980c6-113">Vytvoří virtuální počítač a používá hello Azure vlastní skript rozšíření tooinstall služby IIS.</span><span class="sxs-lookup"><span data-stu-id="980c6-113">Creates a virtual machine and uses hello Azure Custom Script extension tooinstall IIS.</span></span> |
| [<span data-ttu-id="980c6-114">Vytvořte virtuální počítač a spusťte konfigurace DSC</span><span class="sxs-lookup"><span data-stu-id="980c6-114">Create a VM and run DSC configuration</span></span>](./../scripts/virtual-machines-windows-cli-sample-create-iis-using-dsc.md?toc=%2fcli%2fazure%2ftoc.json) | <span data-ttu-id="980c6-115">Vytvoří virtuální počítač a používá tooinstall rozšíření Azure požadovaného stavu konfigurace (DSC) hello služby IIS.</span><span class="sxs-lookup"><span data-stu-id="980c6-115">Creates a virtual machine and uses hello Azure Desired State Configuration (DSC) extension tooinstall IIS.</span></span> |
|<span data-ttu-id="980c6-116">**Síť virtuálních počítačů**</span><span class="sxs-lookup"><span data-stu-id="980c6-116">**Network virtual machines**</span></span>||
| [<span data-ttu-id="980c6-117">Zabezpečení síťového provozu mezi virtuálními počítači</span><span class="sxs-lookup"><span data-stu-id="980c6-117">Secure network traffic between virtual machines</span></span>](./../scripts/virtual-machines-windows-cli-sample-create-vm-nsg.md?toc=%2fcli%2fazure%2ftoc.json) | <span data-ttu-id="980c6-118">Vytvoří dva virtuální počítače, všechny související prostředky a skupiny zabezpečení k interní a externí sítě (NSG).</span><span class="sxs-lookup"><span data-stu-id="980c6-118">Creates two virtual machines, all related resources, and an internal and external network security groups (NSG).</span></span> |
|<span data-ttu-id="980c6-119">**Zabezpečený virtuální počítače**</span><span class="sxs-lookup"><span data-stu-id="980c6-119">**Secure virtual machines**</span></span>||
| [<span data-ttu-id="980c6-120">Šifrování virtuálních počítačů a datovými disky</span><span class="sxs-lookup"><span data-stu-id="980c6-120">Encrypt a VM and data disks</span></span>](./../scripts/virtual-machines-windows-cli-sample-encrypt-vm.md?toc=%2fcli%2fazure%2ftoc.json) | <span data-ttu-id="980c6-121">Vytvoří Azure Key Vault, šifrovací klíč a instanční objekt a potom šifruje virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="980c6-121">Creates an Azure Key Vault, encryption key, and service principal, then encrypts a VM.</span></span> |
|<span data-ttu-id="980c6-122">**Monitorování virtuálních počítačů**</span><span class="sxs-lookup"><span data-stu-id="980c6-122">**Monitor virtual machines**</span></span>||
| [<span data-ttu-id="980c6-123">Monitorování virtuálních počítačů pomocí služby Operations Management Suite</span><span class="sxs-lookup"><span data-stu-id="980c6-123">Monitor a VM with Operations Management Suite</span></span>](./../scripts/virtual-machines-windows-cli-sample-create-vm-oms.md?toc=%2fcli%2fazure%2ftoc.json) | <span data-ttu-id="980c6-124">Vytvoří virtuální počítač, nainstaluje agenta Operations Management Suite hello a zaregistruje hello virtuálních počítačů v pracovním prostoru OMS.</span><span class="sxs-lookup"><span data-stu-id="980c6-124">Creates a virtual machine, installs hello Operations Management Suite agent, and enrolls hello VM in an OMS Workspace.</span></span>  |
| | |
