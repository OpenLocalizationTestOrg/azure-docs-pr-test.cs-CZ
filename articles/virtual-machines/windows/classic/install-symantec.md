---
title: "aaaInstall Symantec Endpoint Protection na virtuální počítač s Windows v Azure | Microsoft Docs"
description: "Zjistěte, jak tooinstall a nakonfigurovat rozšíření zabezpečení hello Symantec Endpoint Protection na nový nebo existující virtuální počítač Azure vytvořené pomocí modelu nasazení Classic hello."
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
ms.openlocfilehash: cb6083366185c26c9f43ff94d835cd90725de5b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooinstall-and-configure-symantec-endpoint-protection-on-a-windows-vm"></a><span data-ttu-id="7cab8-103">Jak tooinstall a konfigurace Symantec Endpoint Protection na virtuální počítač s Windows</span><span class="sxs-lookup"><span data-stu-id="7cab8-103">How tooinstall and configure Symantec Endpoint Protection on a Windows VM</span></span>
> [!IMPORTANT] 
> <span data-ttu-id="7cab8-104">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="7cab8-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="7cab8-105">Tento článek se zabývá pomocí modelu nasazení Classic hello.</span><span class="sxs-lookup"><span data-stu-id="7cab8-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="7cab8-106">Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="7cab8-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span>

<span data-ttu-id="7cab8-107">Tento článek ukazuje, jak tooinstall a nakonfigurovat klienta hello Symantec Endpoint Protection na existující virtuální počítač (VM) s Windows serverem.</span><span class="sxs-lookup"><span data-stu-id="7cab8-107">This article shows you how tooinstall and configure hello Symantec Endpoint Protection client on an existing virtual machine (VM) running Windows Server.</span></span> <span data-ttu-id="7cab8-108">Tato úplná klienta zahrnuje služby jako virů a spywaru ochrany, brány firewall a prevence vniknutí.</span><span class="sxs-lookup"><span data-stu-id="7cab8-108">This full client includes services such as virus and spyware protection, firewall, and intrusion prevention.</span></span> <span data-ttu-id="7cab8-109">Hello klienta se instaluje jako rozšíření zabezpečení pomocí hello agenta virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="7cab8-109">hello client is installed as a security extension by using hello VM Agent.</span></span>

<span data-ttu-id="7cab8-110">Pokud máte stávající předplatné od společnosti Symantec pro místní řešení, můžete ho tooprotect virtuální počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="7cab8-110">If you have an existing subscription from Symantec for an on-premises solution, you can use it tooprotect your Azure virtual machines.</span></span> <span data-ttu-id="7cab8-111">Pokud vám ještě nejsou zákazníka, můžete zaregistrovat zkušební předplatné.</span><span class="sxs-lookup"><span data-stu-id="7cab8-111">If you're not a customer yet, you can sign up for a trial subscription.</span></span> <span data-ttu-id="7cab8-112">Další informace o tomto řešení najdete v tématu [Symantec Endpoint Protection na platformě Azure společnosti Microsoft][Symantec].</span><span class="sxs-lookup"><span data-stu-id="7cab8-112">For more information about this solution, see [Symantec Endpoint Protection on Microsoft's Azure platform][Symantec].</span></span> <span data-ttu-id="7cab8-113">Tato stránka také obsahuje odkazy toolicensing informace a pokyny pro instalaci klienta hello, pokud jste již zákazník Symantec.</span><span class="sxs-lookup"><span data-stu-id="7cab8-113">This page also has links toolicensing information and instructions for installing hello client if you're already a Symantec customer.</span></span>

## <a name="install-symantec-endpoint-protection-on-an-existing-vm"></a><span data-ttu-id="7cab8-114">Nainstalujte Symantec Endpoint Protection na existující virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="7cab8-114">Install Symantec Endpoint Protection on an existing VM</span></span>
<span data-ttu-id="7cab8-115">Než začnete, je třeba hello následující:</span><span class="sxs-lookup"><span data-stu-id="7cab8-115">Before you begin, you need hello following:</span></span>

* <span data-ttu-id="7cab8-116">modul Azure PowerShell text Hello, verze 0.8.2 nebo novější, na počítači v práci.</span><span class="sxs-lookup"><span data-stu-id="7cab8-116">hello Azure PowerShell module, version 0.8.2 or later, on your work computer.</span></span> <span data-ttu-id="7cab8-117">Můžete zkontrolovat hello verzi prostředí Azure PowerShell, který jste nainstalovali s hello **Get-Module azure | verze formátu tabulky** příkaz.</span><span class="sxs-lookup"><span data-stu-id="7cab8-117">You can check hello version of Azure PowerShell that you have installed with hello **Get-Module azure | format-table version** command.</span></span> <span data-ttu-id="7cab8-118">Pokyny a odkaz nejnovější verzi toohello najdete v tématu [jak tooInstall a konfigurace prostředí Azure PowerShell][PS].</span><span class="sxs-lookup"><span data-stu-id="7cab8-118">For instructions and a link toohello latest version, see [How tooInstall and Configure Azure PowerShell][PS].</span></span> <span data-ttu-id="7cab8-119">Přihlaste se pomocí předplatného Azure tooyour `Add-AzureAccount`.</span><span class="sxs-lookup"><span data-stu-id="7cab8-119">Log in tooyour Azure subscription using `Add-AzureAccount`.</span></span>
* <span data-ttu-id="7cab8-120">Hello agenta virtuálního počítače se systémem hello virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="7cab8-120">hello VM Agent running on hello Azure Virtual Machine.</span></span>

<span data-ttu-id="7cab8-121">Nejprve ověří, že hello agenta virtuálního počítače je již nainstalována na virtuálním počítači hello.</span><span class="sxs-lookup"><span data-stu-id="7cab8-121">First, verify that hello VM Agent is already installed on hello virtual machine.</span></span> <span data-ttu-id="7cab8-122">Zadejte název hello cloudové služby a název virtuálního počítače a pak spusťte následující příkazy příkazového řádku Azure PowerShell úrovni správce hello.</span><span class="sxs-lookup"><span data-stu-id="7cab8-122">Fill in hello cloud service name and virtual machine name, and then run hello following commands at an administrator-level Azure PowerShell command prompt.</span></span> <span data-ttu-id="7cab8-123">Nahraďte vše v rámci hello uvozovky, včetně hello < a > znaků.</span><span class="sxs-lookup"><span data-stu-id="7cab8-123">Replace everything within hello quotes, including hello < and > characters.</span></span>

> [!TIP]
> <span data-ttu-id="7cab8-124">Pokud si nejste jisti hello Cloudová služba a názvy virtuálních počítačů, spusťte **Get-AzureVM** toolist hello názvy pro všechny virtuální počítače v aktuálním předplatném.</span><span class="sxs-lookup"><span data-stu-id="7cab8-124">If you don't know hello cloud service and virtual machine names, run **Get-AzureVM** toolist hello names for all virtual machines in your current subscription.</span></span>

```powershell
$CSName = "<cloud service name>"
$VMName = "<virtual machine name>"
$vm = Get-AzureVM -ServiceName $CSName -Name $VMName
write-host $vm.VM.ProvisionGuestAgent
```

<span data-ttu-id="7cab8-125">Pokud hello **zápisu hostitele** příkaz zobrazí **True**, hello je nainstalovaný Agent virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="7cab8-125">If hello **write-host** command displays **True**, hello VM Agent is installed.</span></span> <span data-ttu-id="7cab8-126">Pokud se zobrazí **False**, najdete v části hello pokyny a odkaz toohello, stáhněte si v hello Azure příspěvku na blogu [agenta virtuálního počítače a rozšíření – část 2][Agent].</span><span class="sxs-lookup"><span data-stu-id="7cab8-126">If it displays **False**, see hello instructions and a link toohello download in hello Azure blog post [VM Agent and Extensions - Part 2][Agent].</span></span>

<span data-ttu-id="7cab8-127">Pokud je nainstalována hello agenta virtuálního počítače, spusťte tyto příkazy tooinstall hello Symantec Endpoint Protection agent.</span><span class="sxs-lookup"><span data-stu-id="7cab8-127">If hello VM Agent is installed, run these commands tooinstall hello Symantec Endpoint Protection agent.</span></span>

```powershell
$Agent = Get-AzureVMAvailableExtension -Publisher Symantec -ExtensionName SymantecEndpointProtection

Set-AzureVMExtension -Publisher Symantec –Version $Agent.Version -ExtensionName SymantecEndpointProtection \
    -VM $vm | Update-AzureVM
```

<span data-ttu-id="7cab8-128">tooverify, který hello rozšíření zabezpečení Symantec byla nainstalována a je aktuální:</span><span class="sxs-lookup"><span data-stu-id="7cab8-128">tooverify that hello Symantec security extension has been installed and is up-to-date:</span></span>

1. <span data-ttu-id="7cab8-129">Přihlaste se toohello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="7cab8-129">Log on toohello virtual machine.</span></span> <span data-ttu-id="7cab8-130">Pokyny najdete v tématu [jak tooLog na tooa virtuální počítač spuštěný Windows Server][Logon].</span><span class="sxs-lookup"><span data-stu-id="7cab8-130">For instructions, see [How tooLog on tooa Virtual Machine Running Windows Server][Logon].</span></span>
2. <span data-ttu-id="7cab8-131">Windows Server 2008 R2, klikněte na tlačítko **Start > Symantec Endpoint Protection**.</span><span class="sxs-lookup"><span data-stu-id="7cab8-131">For Windows Server 2008 R2, click **Start > Symantec Endpoint Protection**.</span></span> <span data-ttu-id="7cab8-132">Windows Server 2012 nebo Windows Server 2012 R2, hello úvodní obrazovce zadejte **Symantec**a potom klikněte na **Symantec Endpoint Protection**.</span><span class="sxs-lookup"><span data-stu-id="7cab8-132">For Windows Server 2012 or Windows Server 2012 R2, from hello start screen, type **Symantec**, and then click **Symantec Endpoint Protection**.</span></span>
3. <span data-ttu-id="7cab8-133">Z hello **stav** kartě hello **stav Symantec Endpoint Protection** časové období, aktualizace nebo v případě potřeby restartovat.</span><span class="sxs-lookup"><span data-stu-id="7cab8-133">From hello **Status** tab of hello **Status-Symantec Endpoint Protection** window, apply updates or restart if needed.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7cab8-134">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="7cab8-134">Additional resources</span></span>
<span data-ttu-id="7cab8-135">[Jak tooLog na tooa virtuální počítač spuštěný Windows Server][Logon]</span><span class="sxs-lookup"><span data-stu-id="7cab8-135">[How tooLog on tooa Virtual Machine Running Windows Server][Logon]</span></span>

<span data-ttu-id="7cab8-136">[Rozšíření virtuálního počítače Azure a funkce][Ext]</span><span class="sxs-lookup"><span data-stu-id="7cab8-136">[Azure VM Extensions and Features][Ext]</span></span>

<!--Link references-->
[Symantec]: http://www.symantec.com/connect/blogs/symantec-endpoint-protection-now-microsoft-azure

[Create]:tutorial.md

[PS]: /powershell/azureps-cmdlets-docs

[Agent]: http://go.microsoft.com/fwlink/p/?LinkId=403947

[Logon]:connect-logon.md

[Ext]: http://go.microsoft.com/fwlink/p/?linkid=390493
