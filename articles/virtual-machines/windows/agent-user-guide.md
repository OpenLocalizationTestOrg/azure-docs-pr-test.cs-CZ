---
title: "aaaAzure virtuální počítač agenta přehled | Microsoft Docs"
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
ms.date: 11/17/2016
ms.author: nepeters
ms.openlocfilehash: b03542b9a9c711000fab18ed82e9b17ee5510bbf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-virtual-machine-agent-overview"></a><span data-ttu-id="fe6a8-103">Přehled služby Azure agenta virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="fe6a8-103">Azure Virtual Machine Agent overview</span></span>

<span data-ttu-id="fe6a8-104">Hello agenta virtuálního počítače Microsoft Azure (AM Agent) je zabezpečené, lightweight proces, který spravuje interakci virtuálních počítačů s hello Kontroleru prostředků infrastruktury Azure.</span><span class="sxs-lookup"><span data-stu-id="fe6a8-104">hello Microsoft Azure Virtual Machine Agent (AM Agent) is a secured, lightweight process that manages VM interaction with hello Azure Fabric Controller.</span></span> <span data-ttu-id="fe6a8-105">Hello agenta virtuálního počítače má primární roli v povolení a spuštění rozšíření virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="fe6a8-105">hello VM Agent has a primary role in enabling and executing Azure virtual machine extensions.</span></span> <span data-ttu-id="fe6a8-106">Konfigurace nasazení virtuálních počítačů, jako je instalace a konfigurace softwaru příspěvku povolení rozšíření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="fe6a8-106">VM Extensions enabling post deployment configuration of virtual machines, such as installing and configuring software.</span></span> <span data-ttu-id="fe6a8-107">Rozšíření virtuálního počítače také povolit obnovení funkce jako je resetování hesla pro správu hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="fe6a8-107">Virtual machine extensions also enable recovery features such as resetting hello administrative password of a virtual machine.</span></span> <span data-ttu-id="fe6a8-108">Bez hello agenta virtuálního počítače Azure nelze spustit, rozšíření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="fe6a8-108">Without hello Azure VM Agent, virtual machine extensions cannot be run.</span></span>

<span data-ttu-id="fe6a8-109">Tento dokument podrobně popisuje instalaci, zjištění a odebrání hello agenta virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="fe6a8-109">This document details installation, detection, and removal of hello Azure Virtual Machine Agent.</span></span>

## <a name="install-hello-vm-agent"></a><span data-ttu-id="fe6a8-110">Nainstalujte hello agenta virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="fe6a8-110">Install hello VM Agent</span></span>

### <a name="azure-gallery-image"></a><span data-ttu-id="fe6a8-111">Obrázek v galerii Azure</span><span class="sxs-lookup"><span data-stu-id="fe6a8-111">Azure gallery image</span></span>

<span data-ttu-id="fe6a8-112">Hello agenta virtuálního počítače Azure je nainstalován na všechny nasazené z Galerie Azure bitové kopie virtuálního počítače Windows ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="fe6a8-112">hello Azure VM Agent is installed by default on any Windows virtual machine deployed from an Azure Gallery image.</span></span> <span data-ttu-id="fe6a8-113">Při nasazování bitové kopie Galerie Azure z hello portál, prostředí PowerShell, rozhraní příkazového řádku nebo šablonu Azure Resource Manager, hello agenta virtuálního počítače Azure se taky nainstalovat.</span><span class="sxs-lookup"><span data-stu-id="fe6a8-113">When deploying an Azure gallery image from hello Portal, PowerShell, Command Line Interface, or an Azure Resource Manager template, hello Azure VM Agent is also be installed.</span></span> 

### <a name="manual-installation"></a><span data-ttu-id="fe6a8-114">Ruční instalace</span><span class="sxs-lookup"><span data-stu-id="fe6a8-114">Manual installation</span></span>

<span data-ttu-id="fe6a8-115">agent virtuálního počítače Windows Hello lze ručně nainstalovat pomocí balíčku Instalační služby systému Windows.</span><span class="sxs-lookup"><span data-stu-id="fe6a8-115">hello Windows VM agent can be manually installed using a Windows installer package.</span></span> <span data-ttu-id="fe6a8-116">Ruční instalace může být nutné při vytvoření image vlastní virtuální počítač, který se nasadí v Azure.</span><span class="sxs-lookup"><span data-stu-id="fe6a8-116">Manual installation may be necessary when creating a custom virtual machine image that will be deployed in Azure.</span></span> <span data-ttu-id="fe6a8-117">hello toomanually instalace agenta virtuálního počítače Windows, stažení instalačního programu hello agenta virtuálního počítače z tohoto umístění [stáhnout agenta virtuálního počítače Azure Windows](http://go.microsoft.com/fwlink/?LinkID=394789).</span><span class="sxs-lookup"><span data-stu-id="fe6a8-117">toomanually install hello Windows VM Agent, download hello VM Agent installer from this location [Windows Azure VM Agent Download](http://go.microsoft.com/fwlink/?LinkID=394789).</span></span> 

<span data-ttu-id="fe6a8-118">dvakrát klikněte na soubor Instalační služby systému windows hello může být instalován Hello agenta virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="fe6a8-118">hello VM Agent can be installed by double-clicking hello windows installer file.</span></span> <span data-ttu-id="fe6a8-119">Pro automatizované nebo bezobslužné instalace agenta virtuálního počítače hello spusťte následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="fe6a8-119">For an automated or unattended installation of hello VM agent, run hello following command.</span></span>

```cmd
msiexec.exe /i WindowsAzureVmAgent.2.7.1198.778.rd_art_stable.160617-1120.fre /quiet
```

## <a name="detect-hello-vm-agent"></a><span data-ttu-id="fe6a8-120">Zjištění hello agenta virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="fe6a8-120">Detect hello VM Agent</span></span>

### <a name="powershell"></a><span data-ttu-id="fe6a8-121">PowerShell</span><span class="sxs-lookup"><span data-stu-id="fe6a8-121">PowerShell</span></span>

<span data-ttu-id="fe6a8-122">modul Powershellu pro Azure Resource Manager Hello lze použít tooretrieve informace o virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="fe6a8-122">hello Azure Resource Manager PowerShell module can be used tooretrieve information about Azure Virtual Machines.</span></span> <span data-ttu-id="fe6a8-123">Spuštění `Get-AzureRmVM` vrátí s bit informace včetně hello zřizování stav hello agenta virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="fe6a8-123">Running `Get-AzureRmVM` returns quite a bit of information including hello provisioning state for hello Azure VM Agent.</span></span>

```PowerShell
Get-AzureRmVM
```

<span data-ttu-id="fe6a8-124">Hello následuje pouze podmnožinu hello `Get-AzureRmVM` výstup.</span><span class="sxs-lookup"><span data-stu-id="fe6a8-124">hello following is just a subset of hello `Get-AzureRmVM` output.</span></span> <span data-ttu-id="fe6a8-125">Všimněte si hello `ProvisionVMAgent` vlastnost vnořit `OSProfile`, tato vlastnost může být použité toodetermine, pokud agent virtuálního počítače hello byl nasazený toohello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="fe6a8-125">Notice hello `ProvisionVMAgent` property nested inside `OSProfile`, this property can be used toodetermine if hello VM agent has been deployed toohello virtual machine.</span></span>

```PowerShell
OSProfile                  :
  ComputerName             : myVM
  AdminUsername            : muUserName
  WindowsConfiguration     :
    ProvisionVMAgent       : True
    EnableAutomaticUpdates : True
```

<span data-ttu-id="fe6a8-126">Hello následující skript se dá použít tooreturn stručným seznam názvy virtuálních počítačů a stav hello hello agenta virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="fe6a8-126">hello following script can be used tooreturn a concise list of virtual machine names and hello state of hello VM Agent.</span></span>

```PowerShell
$vms = Get-AzureRmVM

foreach ($vm in $vms) {
    $agent = $vm | Select -ExpandProperty OSProfile | Select -ExpandProperty Windowsconfiguration | Select ProvisionVMAgent
    Write-Host $vm.Name $agent.ProvisionVMAgent
}
```

### <a name="manual-detection"></a><span data-ttu-id="fe6a8-127">Ruční detekce</span><span class="sxs-lookup"><span data-stu-id="fe6a8-127">Manual Detection</span></span>

<span data-ttu-id="fe6a8-128">Při přihlášení tooa virtuálního počítače Windows Azure, Správce úloh může být použité tooexamine spuštěných procesů.</span><span class="sxs-lookup"><span data-stu-id="fe6a8-128">When logged in tooa Windows Azure VM, task manager can be used tooexamine running processes.</span></span> <span data-ttu-id="fe6a8-129">toocheck pro hello agenta virtuálního počítače Azure, otevřete Správce úloh > klikněte na kartu s podrobnostmi hello a vyhledejte název procesu `WindowsAzureGuestAgent.exe`.</span><span class="sxs-lookup"><span data-stu-id="fe6a8-129">toocheck for hello Azure VM Agent, open Task Manager > click hello details tab, and look for a process name `WindowsAzureGuestAgent.exe`.</span></span> <span data-ttu-id="fe6a8-130">Hello přítomnost tento proces naznačuje, že je nainstalovaný agent virtuálního počítače tohoto hello.</span><span class="sxs-lookup"><span data-stu-id="fe6a8-130">hello presence of this process indicates that hello VM agent is installed.</span></span>

## <a name="upgrade-hello-vm-agent"></a><span data-ttu-id="fe6a8-131">Upgrade hello agenta virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="fe6a8-131">Upgrade hello VM Agent</span></span>

<span data-ttu-id="fe6a8-132">Hello Azure virtuální počítač agenta pro Windows je automaticky upgradován.</span><span class="sxs-lookup"><span data-stu-id="fe6a8-132">hello Azure VM Agent for Windows is automatically upgraded.</span></span> <span data-ttu-id="fe6a8-133">Nové virtuální počítače jsou nasazené tooAzure, obdrží hello nejnovější agenta virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="fe6a8-133">As new virtual machines are deployed tooAzure, they receive hello latest VM agent.</span></span> <span data-ttu-id="fe6a8-134">Vlastní Image virtuálních počítačů musí být ručně aktualizovaných tooinclude hello nového virtuálního počítače agenta.</span><span class="sxs-lookup"><span data-stu-id="fe6a8-134">Custom VM images should be manually updated tooinclude hello new VM agent.</span></span>
