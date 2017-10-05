---
title: "Nainstalovat službu Symantec Endpoint Protection v systému Windows virtuálního počítače v Azure | Microsoft Docs"
description: "Zjistěte, jak nainstalovat a nakonfigurovat rozšíření zabezpečení Symantec Endpoint Protection na nového nebo existujícího virtuálního počítače Azure vytvořené pomocí modelu nasazení Classic."
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 19dcebc7-da6b-4510-907b-d64088e81fa2
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: iainfou
ms.openlocfilehash: 1603ebc7ee3c29277f30fbb802bdd8205b92d648
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-install-and-configure-symantec-endpoint-protection-on-a-windows-vm"></a><span data-ttu-id="bb58a-103">Jak nainstalovat a nakonfigurovat Symantec Endpoint Protection na virtuálním počítači s Windows</span><span class="sxs-lookup"><span data-stu-id="bb58a-103">How to install and configure Symantec Endpoint Protection on a Windows VM</span></span>
> [!IMPORTANT] 
> <span data-ttu-id="bb58a-104">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="bb58a-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="bb58a-105">Tento článek se zabývá pomocí modelu nasazení Classic.</span><span class="sxs-lookup"><span data-stu-id="bb58a-105">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="bb58a-106">Microsoft doporučuje, aby byl ve většině nových nasazení použit model Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="bb58a-106">Microsoft recommends that most new deployments use the Resource Manager model.</span></span>

<span data-ttu-id="bb58a-107">Tento článek ukazuje, jak nainstalovat a nakonfigurovat klienta Symantec Endpoint Protection na existující virtuální počítač (VM) s Windows serverem.</span><span class="sxs-lookup"><span data-stu-id="bb58a-107">This article shows you how to install and configure the Symantec Endpoint Protection client on an existing virtual machine (VM) running Windows Server.</span></span> <span data-ttu-id="bb58a-108">Tato úplná klienta zahrnuje služby jako virů a spywaru ochrany, brány firewall a prevence vniknutí.</span><span class="sxs-lookup"><span data-stu-id="bb58a-108">This full client includes services such as virus and spyware protection, firewall, and intrusion prevention.</span></span> <span data-ttu-id="bb58a-109">Klient je nainstalován jako rozšíření zabezpečení pomocí agenta virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="bb58a-109">The client is installed as a security extension by using the VM Agent.</span></span>

<span data-ttu-id="bb58a-110">Pokud máte stávající předplatné od společnosti Symantec pro místní řešení, můžete chránit virtuální počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="bb58a-110">If you have an existing subscription from Symantec for an on-premises solution, you can use it to protect your Azure virtual machines.</span></span> <span data-ttu-id="bb58a-111">Pokud vám ještě nejsou zákazníka, můžete zaregistrovat zkušební předplatné.</span><span class="sxs-lookup"><span data-stu-id="bb58a-111">If you're not a customer yet, you can sign up for a trial subscription.</span></span> <span data-ttu-id="bb58a-112">Další informace o tomto řešení najdete v tématu [Symantec Endpoint Protection na platformě Azure společnosti Microsoft][Symantec].</span><span class="sxs-lookup"><span data-stu-id="bb58a-112">For more information about this solution, see [Symantec Endpoint Protection on Microsoft's Azure platform][Symantec].</span></span> <span data-ttu-id="bb58a-113">Tato stránka také obsahuje odkazy na licenční informace a pokyny pro instalaci klienta, pokud jste již zákazník Symantec.</span><span class="sxs-lookup"><span data-stu-id="bb58a-113">This page also has links to licensing information and instructions for installing the client if you're already a Symantec customer.</span></span>

## <a name="install-symantec-endpoint-protection-on-an-existing-vm"></a><span data-ttu-id="bb58a-114">Nainstalujte Symantec Endpoint Protection na existující virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="bb58a-114">Install Symantec Endpoint Protection on an existing VM</span></span>
<span data-ttu-id="bb58a-115">Než začnete, budete potřebovat následující:</span><span class="sxs-lookup"><span data-stu-id="bb58a-115">Before you begin, you need the following:</span></span>

* <span data-ttu-id="bb58a-116">Modul Azure PowerShell, verze 0.8.2 nebo novější, na počítači v práci.</span><span class="sxs-lookup"><span data-stu-id="bb58a-116">The Azure PowerShell module, version 0.8.2 or later, on your work computer.</span></span> <span data-ttu-id="bb58a-117">Můžete zkontrolovat verzi prostředí Azure PowerShell, který jste nainstalovali s **Get-Module azure | verze formátu tabulky** příkaz.</span><span class="sxs-lookup"><span data-stu-id="bb58a-117">You can check the version of Azure PowerShell that you have installed with the **Get-Module azure | format-table version** command.</span></span> <span data-ttu-id="bb58a-118">Pokyny a odkaz na nejnovější verzi naleznete v tématu [postup instalace a konfigurace prostředí Azure PowerShell][PS].</span><span class="sxs-lookup"><span data-stu-id="bb58a-118">For instructions and a link to the latest version, see [How to Install and Configure Azure PowerShell][PS].</span></span> <span data-ttu-id="bb58a-119">Přihlaste se k předplatnému Azure pomocí `Add-AzureAccount`.</span><span class="sxs-lookup"><span data-stu-id="bb58a-119">Log in to your Azure subscription using `Add-AzureAccount`.</span></span>
* <span data-ttu-id="bb58a-120">Virtuální počítač agenta spuštěného na virtuálním počítači Azure.</span><span class="sxs-lookup"><span data-stu-id="bb58a-120">The VM Agent running on the Azure Virtual Machine.</span></span>

<span data-ttu-id="bb58a-121">Nejprve ověří, že je na virtuálním počítači již nainstalován Agent virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="bb58a-121">First, verify that the VM Agent is already installed on the virtual machine.</span></span> <span data-ttu-id="bb58a-122">Zadejte název cloudové služby a název virtuálního počítače a potom spusťte následující příkazy příkazového řádku Azure PowerShell úrovni správce.</span><span class="sxs-lookup"><span data-stu-id="bb58a-122">Fill in the cloud service name and virtual machine name, and then run the following commands at an administrator-level Azure PowerShell command prompt.</span></span> <span data-ttu-id="bb58a-123">Nahraďte všechna data v uvozovkách, včetně < a > znaků.</span><span class="sxs-lookup"><span data-stu-id="bb58a-123">Replace everything within the quotes, including the < and > characters.</span></span>

> [!TIP]
> <span data-ttu-id="bb58a-124">Pokud si nejste jisti, Cloudová služba a názvy virtuálních počítačů, spusťte **Get-AzureVM** seznam názvy pro všechny virtuální počítače v aktuálním předplatném.</span><span class="sxs-lookup"><span data-stu-id="bb58a-124">If you don't know the cloud service and virtual machine names, run **Get-AzureVM** to list the names for all virtual machines in your current subscription.</span></span>

```powershell
$CSName = "<cloud service name>"
$VMName = "<virtual machine name>"
$vm = Get-AzureVM -ServiceName $CSName -Name $VMName
write-host $vm.VM.ProvisionGuestAgent
```

<span data-ttu-id="bb58a-125">Pokud **zápisu hostitele** příkaz zobrazí **True**, je nainstalovaný Agent virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="bb58a-125">If the **write-host** command displays **True**, the VM Agent is installed.</span></span> <span data-ttu-id="bb58a-126">Pokud se zobrazí **False**, najdete pokyny a odkaz na stažení v Azure příspěvku na blogu [agenta virtuálního počítače a rozšíření – část 2][Agent].</span><span class="sxs-lookup"><span data-stu-id="bb58a-126">If it displays **False**, see the instructions and a link to the download in the Azure blog post [VM Agent and Extensions - Part 2][Agent].</span></span>

<span data-ttu-id="bb58a-127">Pokud je nainstalován Agent virtuálního počítače, spusťte tyto příkazy pro instalaci agenta Symantec Endpoint Protection.</span><span class="sxs-lookup"><span data-stu-id="bb58a-127">If the VM Agent is installed, run these commands to install the Symantec Endpoint Protection agent.</span></span>

```powershell
$Agent = Get-AzureVMAvailableExtension -Publisher Symantec -ExtensionName SymantecEndpointProtection

Set-AzureVMExtension -Publisher Symantec –Version $Agent.Version -ExtensionName SymantecEndpointProtection \
    -VM $vm | Update-AzureVM
```

<span data-ttu-id="bb58a-128">Chcete-li ověřit, že rozšíření zabezpečení Symantec byl nainstalován a zda je aktuální:</span><span class="sxs-lookup"><span data-stu-id="bb58a-128">To verify that the Symantec security extension has been installed and is up-to-date:</span></span>

1. <span data-ttu-id="bb58a-129">Přihlaste se k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="bb58a-129">Log on to the virtual machine.</span></span> <span data-ttu-id="bb58a-130">Pokyny najdete v tématu [postup Přihlaste se k Windows serveru spuštěný virtuální počítač][Logon].</span><span class="sxs-lookup"><span data-stu-id="bb58a-130">For instructions, see [How to Log on to a Virtual Machine Running Windows Server][Logon].</span></span>
2. <span data-ttu-id="bb58a-131">Windows Server 2008 R2, klikněte na tlačítko **Start > Symantec Endpoint Protection**.</span><span class="sxs-lookup"><span data-stu-id="bb58a-131">For Windows Server 2008 R2, click **Start > Symantec Endpoint Protection**.</span></span> <span data-ttu-id="bb58a-132">Windows Server 2012 nebo Windows Server 2012 R2, na obrazovce start zadejte **Symantec**a potom klikněte na **Symantec Endpoint Protection**.</span><span class="sxs-lookup"><span data-stu-id="bb58a-132">For Windows Server 2012 or Windows Server 2012 R2, from the start screen, type **Symantec**, and then click **Symantec Endpoint Protection**.</span></span>
3. <span data-ttu-id="bb58a-133">Z **stav** kartě **stav Symantec Endpoint Protection** časové období, aktualizace nebo v případě potřeby restartovat.</span><span class="sxs-lookup"><span data-stu-id="bb58a-133">From the **Status** tab of the **Status-Symantec Endpoint Protection** window, apply updates or restart if needed.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bb58a-134">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="bb58a-134">Additional resources</span></span>
<span data-ttu-id="bb58a-135">[Postup Přihlaste se k virtuálního počítače se systémem Windows Server][Logon]</span><span class="sxs-lookup"><span data-stu-id="bb58a-135">[How to Log on to a Virtual Machine Running Windows Server][Logon]</span></span>

<span data-ttu-id="bb58a-136">[Rozšíření virtuálního počítače Azure a funkce][Ext]</span><span class="sxs-lookup"><span data-stu-id="bb58a-136">[Azure VM Extensions and Features][Ext]</span></span>

<!--Link references-->
[Symantec]: http://www.symantec.com/connect/blogs/symantec-endpoint-protection-now-microsoft-azure

[Create]:tutorial.md

[PS]: /powershell/azureps-cmdlets-docs

[Agent]: http://go.microsoft.com/fwlink/p/?LinkId=403947

[Logon]:connect-logon.md

[Ext]: http://go.microsoft.com/fwlink/p/?linkid=390493
