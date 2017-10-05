---
title: "Přehled agenta virtuálního počítače Azure | Microsoft Docs"
description: "Přehled agenta virtuálního počítače Azure"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 0a1f212e-053e-4a39-9910-8d622959f594
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/28/2017
ms.author: nepeters
ms.openlocfilehash: accfd5f0fec69175e584528ff9f6db66402cb89e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-virtual-machine-agent-overview"></a><span data-ttu-id="6f17e-103">Přehled služby Azure agenta virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="6f17e-103">Azure Virtual Machine Agent overview</span></span>

<span data-ttu-id="6f17e-104">Agent virtuálního počítače Microsoft Azure (VM Agent) je zabezpečené, lightweight proces, který spravuje interakci virtuálních počítačů s Kontroleru prostředků infrastruktury Azure.</span><span class="sxs-lookup"><span data-stu-id="6f17e-104">The Microsoft Azure Virtual Machine Agent (VM Agent) is a secured, lightweight process that manages VM interaction with the Azure Fabric Controller.</span></span> <span data-ttu-id="6f17e-105">Agent virtuálního počítače má primární roli v povolení a spuštění rozšíření virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="6f17e-105">The VM Agent has a primary role in enabling and executing Azure virtual machine extensions.</span></span> <span data-ttu-id="6f17e-106">Konfigurace nasazení virtuálních počítačů, jako je instalace a konfigurace softwaru příspěvku povolení rozšíření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="6f17e-106">VM Extensions enabling post deployment configuration of virtual machines, such as installing and configuring software.</span></span> <span data-ttu-id="6f17e-107">Rozšíření virtuálního počítače také povolit obnovení funkce jako je resetování hesla pro správu virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="6f17e-107">Virtual machine extensions also enable recovery features such as resetting the administrative password of a virtual machine.</span></span> <span data-ttu-id="6f17e-108">Bez agenta virtuálního počítače Azure nelze spustit, rozšíření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="6f17e-108">Without the Azure VM Agent, virtual machine extensions cannot be run.</span></span>

<span data-ttu-id="6f17e-109">Tento dokument podrobně popisuje instalaci, zjištění a odebrání agenta virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="6f17e-109">This document details installation, detection, and removal of the Azure Virtual Machine Agent.</span></span>

## <a name="install-the-vm-agent"></a><span data-ttu-id="6f17e-110">Nainstalujte agenta virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="6f17e-110">Install the VM Agent</span></span>

### <a name="azure-gallery-image"></a><span data-ttu-id="6f17e-111">Obrázek v galerii Azure</span><span class="sxs-lookup"><span data-stu-id="6f17e-111">Azure gallery image</span></span>

<span data-ttu-id="6f17e-112">Na všechny nasazené z Galerie Azure bitové kopie virtuálního počítače Windows ve výchozím nastavení je nainstalovaný Agent virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="6f17e-112">The Azure VM Agent is installed by default on any Windows virtual machine deployed from an Azure Gallery image.</span></span> <span data-ttu-id="6f17e-113">Při nasazování bitové kopie Galerie Azure z portálu, prostředí PowerShell, rozhraní příkazového řádku nebo šablonu Azure Resource Manager, je také nainstalován Agent virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="6f17e-113">When deploying an Azure gallery image from the Portal, PowerShell, Command Line Interface, or an Azure Resource Manager template, the Azure VM Agent is also be installed.</span></span> 

### <a name="manual-installation"></a><span data-ttu-id="6f17e-114">Ruční instalace</span><span class="sxs-lookup"><span data-stu-id="6f17e-114">Manual installation</span></span>

<span data-ttu-id="6f17e-115">Agent virtuálního počítače s Windows můžete ručně nainstalovat pomocí balíčku Instalační služby systému Windows.</span><span class="sxs-lookup"><span data-stu-id="6f17e-115">The Windows VM agent can be manually installed using a Windows installer package.</span></span> <span data-ttu-id="6f17e-116">Ruční instalace může být nutné při vytvoření image vlastní virtuální počítač, který se nasadí v Azure.</span><span class="sxs-lookup"><span data-stu-id="6f17e-116">Manual installation may be necessary when creating a custom virtual machine image that will be deployed in Azure.</span></span> <span data-ttu-id="6f17e-117">Při ruční instalaci agenta virtuálního počítače Windows, stáhněte si instalační program agenta virtuálního počítače z tohoto umístění [stáhnout agenta virtuálního počítače Azure Windows](http://go.microsoft.com/fwlink/?LinkID=394789).</span><span class="sxs-lookup"><span data-stu-id="6f17e-117">To manually install the Windows VM Agent, download the VM Agent installer from this location [Windows Azure VM Agent Download](http://go.microsoft.com/fwlink/?LinkID=394789).</span></span> 

<span data-ttu-id="6f17e-118">Dvojitým kliknutím na soubor Instalační služby systému windows můžete nainstalovat agenta virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="6f17e-118">The VM Agent can be installed by double-clicking the windows installer file.</span></span> <span data-ttu-id="6f17e-119">Pro automatizované nebo bezobslužné instalace agenta virtuálního počítače spusťte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="6f17e-119">For an automated or unattended installation of the VM agent, run the following command.</span></span>

```cmd
msiexec.exe /i WindowsAzureVmAgent.2.7.1198.778.rd_art_stable.160617-1120.fre /quiet
```

## <a name="detect-the-vm-agent"></a><span data-ttu-id="6f17e-120">Zjištění agenta virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="6f17e-120">Detect the VM Agent</span></span>

### <a name="powershell"></a><span data-ttu-id="6f17e-121">PowerShell</span><span class="sxs-lookup"><span data-stu-id="6f17e-121">PowerShell</span></span>

<span data-ttu-id="6f17e-122">Modul Powershellu pro Azure Resource Manager můžete využít k načtení informací o virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="6f17e-122">The Azure Resource Manager PowerShell module can be used to retrieve information about Azure Virtual Machines.</span></span> <span data-ttu-id="6f17e-123">Spuštění `Get-AzureRmVM` vrátí s bit informace včetně Stav zřizování pro agenta virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="6f17e-123">Running `Get-AzureRmVM` returns quite a bit of information including the provisioning state for the Azure VM Agent.</span></span>

```PowerShell
Get-AzureRmVM
```

<span data-ttu-id="6f17e-124">Toto je pouze podmnožinu `Get-AzureRmVM` výstup.</span><span class="sxs-lookup"><span data-stu-id="6f17e-124">The following is just a subset of the `Get-AzureRmVM` output.</span></span> <span data-ttu-id="6f17e-125">Upozornění `ProvisionVMAgent` vlastnost vnořit `OSProfile`, tato vlastnost slouží k určení, pokud agent virtuálního počítače byla nasazena do virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="6f17e-125">Notice the `ProvisionVMAgent` property nested inside `OSProfile`, this property can be used to determine if the VM agent has been deployed to the virtual machine.</span></span>

```PowerShell
OSProfile                  :
  ComputerName             : myVM
  AdminUsername            : muUserName
  WindowsConfiguration     :
    ProvisionVMAgent       : True
    EnableAutomaticUpdates : True
```

<span data-ttu-id="6f17e-126">Následující skript lze použít k vrácení stručným seznamu názvy virtuálních počítačů a stav agenta virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="6f17e-126">The following script can be used to return a concise list of virtual machine names and the state of the VM Agent.</span></span>

```PowerShell
$vms = Get-AzureRmVM

foreach ($vm in $vms) {
    $agent = $vm | Select -ExpandProperty OSProfile | Select -ExpandProperty Windowsconfiguration | Select ProvisionVMAgent
    Write-Host $vm.Name $agent.ProvisionVMAgent
}
```

### <a name="manual-detection"></a><span data-ttu-id="6f17e-127">Ruční detekce</span><span class="sxs-lookup"><span data-stu-id="6f17e-127">Manual Detection</span></span>

<span data-ttu-id="6f17e-128">Při přihlášení k virtuálnímu počítači Windows Azure, Správce úloh můžete použít k prozkoumání spuštěných procesů.</span><span class="sxs-lookup"><span data-stu-id="6f17e-128">When logged in to a Windows Azure VM, task manager can be used to examine running processes.</span></span> <span data-ttu-id="6f17e-129">Pokud chcete zkontrolovat pro agenta virtuálního počítače Azure, otevřete Správce úloh > klikněte na kartu s podrobnostmi a vyhledejte název procesu `WindowsAzureGuestAgent.exe`.</span><span class="sxs-lookup"><span data-stu-id="6f17e-129">To check for the Azure VM Agent, open Task Manager > click the details tab, and look for a process name `WindowsAzureGuestAgent.exe`.</span></span> <span data-ttu-id="6f17e-130">Přítomnost tento proces naznačuje, že je nainstalovaný agent virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="6f17e-130">The presence of this process indicates that the VM agent is installed.</span></span>

## <a name="upgrade-the-vm-agent"></a><span data-ttu-id="6f17e-131">Upgrade agenta virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="6f17e-131">Upgrade the VM Agent</span></span>

<span data-ttu-id="6f17e-132">Virtuální počítač agenta k Azure pro systém Windows automaticky upgradován.</span><span class="sxs-lookup"><span data-stu-id="6f17e-132">The Azure VM Agent for Windows is automatically upgraded.</span></span> <span data-ttu-id="6f17e-133">Při nasazování nových virtuálních počítačů do Azure, obdrží nejnovější agenta virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="6f17e-133">As new virtual machines are deployed to Azure, they receive the latest VM agent.</span></span> <span data-ttu-id="6f17e-134">Vlastní Image virtuálních počítačů by měl ručně aktualizovalo se o nový agent virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="6f17e-134">Custom VM images should be manually updated to include the new VM agent.</span></span>