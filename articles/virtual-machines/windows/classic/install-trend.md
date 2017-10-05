---
title: "Nainstalujte Trend malých hloubkové zabezpečení na virtuálním počítači | Microsoft Docs"
description: "Tento článek popisuje postup instalace a konfigurace Trend Micro zabezpečení na virtuálním počítači vytvořené pomocí modelu nasazení Classic v Azure."
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: e991b635-f1e2-483f-b7ca-9d53e7c22e2a
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 03/30/2017
ms.author: iainfou
ms.openlocfilehash: 911b8f12472dcbda3e6bfeb8c97bf1d04a63e1dd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-install-and-configure-trend-micro-deep-security-as-a-service-on-a-windows-vm"></a><span data-ttu-id="9b89e-103">Jak nainstalovat a nakonfigurovat Trend Micro Deep Security na virtuálním počítači s Windows jako službu</span><span class="sxs-lookup"><span data-stu-id="9b89e-103">How to install and configure Trend Micro Deep Security as a Service on a Windows VM</span></span>
> [!IMPORTANT]
> <span data-ttu-id="9b89e-104">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="9b89e-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="9b89e-105">Tento článek se zabývá pomocí modelu nasazení Classic.</span><span class="sxs-lookup"><span data-stu-id="9b89e-105">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="9b89e-106">Microsoft doporučuje, aby byl ve většině nových nasazení použit model Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="9b89e-106">Microsoft recommends that most new deployments use the Resource Manager model.</span></span>

<span data-ttu-id="9b89e-107">Tento článek ukazuje, jak nainstalovat a nakonfigurovat Trend Micro hluboké Security jako služba na nový nebo existující virtuální počítač (VM) s Windows serverem.</span><span class="sxs-lookup"><span data-stu-id="9b89e-107">This article shows you how to install and configure Trend Micro Deep Security as a Service on a new or existing virtual machine (VM) running Windows Server.</span></span> <span data-ttu-id="9b89e-108">Hloubkové zabezpečení jako služba zahrnuje ochrana proti malwaru, brána firewall, narušení prevence systém a monitorování integrity.</span><span class="sxs-lookup"><span data-stu-id="9b89e-108">Deep Security as a Service includes anti-malware protection, a firewall, an intrusion prevention system, and integrity monitoring.</span></span>

<span data-ttu-id="9b89e-109">Klient je nainstalován jako rozšíření zabezpečení prostřednictvím agenta virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="9b89e-109">The client is installed as a security extension via the VM Agent.</span></span> <span data-ttu-id="9b89e-110">Na nový virtuální počítač nainstalujte hloubkové Agent zabezpečení jako Agent virtuálního počítače je vytvářena automaticky nástrojem portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="9b89e-110">On a new virtual machine, you install the Deep Security Agent, as the VM Agent is created automatically by the Azure portal.</span></span>

<span data-ttu-id="9b89e-111">Existující virtuální počítač vytvořený na portálu classic, rozhraní příkazového řádku Azure nebo PowerShell nemusí mít agenta virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="9b89e-111">An existing VM created using the classic portal, the Azure CLI, or PowerShell might not have a VM agent.</span></span> <span data-ttu-id="9b89e-112">Pro stávajícího virtuálního počítače, který nemá agenta virtuálního počítače potřebujete stáhněte a nainstalujte ji jako první.</span><span class="sxs-lookup"><span data-stu-id="9b89e-112">For an existing virtual machine that doesn't have the VM Agent, you need to download and install it first.</span></span> <span data-ttu-id="9b89e-113">Tento článek se týká obou situacích.</span><span class="sxs-lookup"><span data-stu-id="9b89e-113">This article covers both situations.</span></span>

<span data-ttu-id="9b89e-114">Pokud máte z Trend Micro aktuální předplatné pro místní řešení, můžete k ochraně virtuální počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="9b89e-114">If you have a current subscription from Trend Micro for an on-premises solution, you can use it to help protect your Azure virtual machines.</span></span> <span data-ttu-id="9b89e-115">Pokud vám ještě nejsou zákazníka, můžete zaregistrovat zkušební předplatné.</span><span class="sxs-lookup"><span data-stu-id="9b89e-115">If you're not a customer yet, you can sign up for a trial subscription.</span></span> <span data-ttu-id="9b89e-116">Další informace o tomto řešení najdete v příspěvku blogu Trend Micro [Microsoft Azure virtuálního počítače agenta rozšíření pro přímý zabezpečení](http://go.microsoft.com/fwlink/p/?LinkId=403945).</span><span class="sxs-lookup"><span data-stu-id="9b89e-116">For more information about this solution, see the Trend Micro blog post [Microsoft Azure VM Agent Extension For Deep Security](http://go.microsoft.com/fwlink/p/?LinkId=403945).</span></span>

## <a name="install-the-deep-security-agent-on-a-new-vm"></a><span data-ttu-id="9b89e-117">Nainstalujte agenta hloubkové zabezpečení na nový virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="9b89e-117">Install the Deep Security Agent on a new VM</span></span>

<span data-ttu-id="9b89e-118">[Portál Azure](http://portal.azure.com) umožňuje nainstalovat rozšíření zabezpečení Trend Micro při použití bitovou kopii **Marketplace** k vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="9b89e-118">The [Azure portal](http://portal.azure.com) lets you install the Trend Micro security extension when you use an image from the **Marketplace** to create the virtual machine.</span></span> <span data-ttu-id="9b89e-119">Pokud vytváříte jeden virtuální počítač, pomocí portálu je snadný způsob, jak přidat ochranu z Trend Micro.</span><span class="sxs-lookup"><span data-stu-id="9b89e-119">If you're creating a single virtual machine, using the portal is an easy way to add protection from Trend Micro.</span></span>

<span data-ttu-id="9b89e-120">Pomocí položky z **Marketplace** otevře průvodce, který vám pomůže nastavit virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="9b89e-120">Using an entry from the **Marketplace** opens a wizard that helps you set up the virtual machine.</span></span> <span data-ttu-id="9b89e-121">Můžete použít **nastavení** okna, panel třetí průvodce, chcete-li nainstalovat rozšíření Trend Micro zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="9b89e-121">You use the **Settings** blade, the third panel of the wizard, to install the Trend Micro security extension.</span></span>  <span data-ttu-id="9b89e-122">Obecné pokyny najdete v tématu [vytvoření virtuálního počítače se systémem Windows na portálu Azure](tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="9b89e-122">For general instructions, see [Create a virtual machine running Windows in the Azure portal](tutorial.md).</span></span>

<span data-ttu-id="9b89e-123">Jakmile přejdete **nastavení** okno průvodce proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="9b89e-123">When you get to the **Settings** blade of the wizard, do the following steps:</span></span>

1. <span data-ttu-id="9b89e-124">Klikněte na tlačítko **rozšíření**, pak klikněte na tlačítko **přidat rozšíření** v podokně Další.</span><span class="sxs-lookup"><span data-stu-id="9b89e-124">Click **Extensions**, then click **Add extension** in the next pane.</span></span>

   ![Začít přidávat rozšíření][1]

2. <span data-ttu-id="9b89e-126">Vyberte **hloubkové Agent zabezpečení** v **nový prostředek** podokně.</span><span class="sxs-lookup"><span data-stu-id="9b89e-126">Select **Deep Security Agent** in the **New resource** pane.</span></span> <span data-ttu-id="9b89e-127">V podokně hloubkové Agent zabezpečení, klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="9b89e-127">In the Deep Security Agent pane, click **Create**.</span></span>

   ![Identifikovat Agent hloubkové zabezpečení][2]

3. <span data-ttu-id="9b89e-129">Zadejte **identifikátor klienta** a **heslo aktivace klienta** pro rozšíření.</span><span class="sxs-lookup"><span data-stu-id="9b89e-129">Enter the **Tenant Identifier** and **Tenant Activation Password** for the extension.</span></span> <span data-ttu-id="9b89e-130">Volitelně můžete zadat **identifikátor zásady zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="9b89e-130">Optionally, you can enter a **Security Policy Identifier**.</span></span> <span data-ttu-id="9b89e-131">Potom klikněte na **OK** přidat klienta.</span><span class="sxs-lookup"><span data-stu-id="9b89e-131">Then, click **OK** to add the client.</span></span>

   ![Zadejte podrobnosti o rozšíření][3]

## <a name="install-the-deep-security-agent-on-an-existing-vm"></a><span data-ttu-id="9b89e-133">Hloubkové zabezpečení agenta nainstalovat na existující virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="9b89e-133">Install the Deep Security Agent on an existing VM</span></span>
<span data-ttu-id="9b89e-134">Chcete-li nainstalovat agenta na existující virtuální počítač, je třeba následující položky:</span><span class="sxs-lookup"><span data-stu-id="9b89e-134">To install the agent on an existing VM, you need the following items:</span></span>

* <span data-ttu-id="9b89e-135">Modul Azure PowerShell, verze 0.8.2 nebo novější, nainstalovaná v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="9b89e-135">The Azure PowerShell module, version 0.8.2 or newer, installed on your local computer.</span></span> <span data-ttu-id="9b89e-136">Můžete zkontrolovat verzi prostředí Azure PowerShell, který jste nainstalovali pomocí **Get-Module azure | verze formátu tabulky** příkaz.</span><span class="sxs-lookup"><span data-stu-id="9b89e-136">You can check the version of Azure PowerShell that you have installed by using the **Get-Module azure | format-table version** command.</span></span> <span data-ttu-id="9b89e-137">Pokyny a odkaz na nejnovější verzi naleznete v tématu [postup instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="9b89e-137">For instructions and a link to the latest version, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span> <span data-ttu-id="9b89e-138">Přihlaste se k předplatnému Azure pomocí `Add-AzureAccount`.</span><span class="sxs-lookup"><span data-stu-id="9b89e-138">Log in to your Azure subscription using `Add-AzureAccount`.</span></span>
* <span data-ttu-id="9b89e-139">Agent virtuálního počítače nainstalovaný na cílový virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="9b89e-139">The VM Agent installed on the target virtual machine.</span></span>

<span data-ttu-id="9b89e-140">Nejprve ověří, že je již nainstalován Agent virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="9b89e-140">First, verify that the VM Agent is already installed.</span></span> <span data-ttu-id="9b89e-141">Zadejte název cloudové služby a název virtuálního počítače a potom spusťte následující příkazy příkazového řádku Azure PowerShell úrovni správce.</span><span class="sxs-lookup"><span data-stu-id="9b89e-141">Fill in the cloud service name and virtual machine name, and then run the following commands at an administrator-level Azure PowerShell command prompt.</span></span> <span data-ttu-id="9b89e-142">Nahraďte všechna data v uvozovkách, včetně < a > znaků.</span><span class="sxs-lookup"><span data-stu-id="9b89e-142">Replace everything within the quotes, including the < and > characters.</span></span>

    $CSName = "<cloud service name>"
    $VMName = "<virtual machine name>"
    $vm = Get-AzureVM -ServiceName $CSName -Name $VMName
    write-host $vm.VM.ProvisionGuestAgent

<span data-ttu-id="9b89e-143">Pokud si nejste jisti, Cloudová služba a název virtuálního počítače, spusťte **Get-AzureVM** zobrazit tyto informace pro všechny virtuální počítače v aktuálním předplatném.</span><span class="sxs-lookup"><span data-stu-id="9b89e-143">If you don't know the cloud service and virtual machine name, run **Get-AzureVM** to display that information for all the virtual machines in your current subscription.</span></span>

<span data-ttu-id="9b89e-144">Pokud **zápisu hostitele** příkaz vrátí **True**, je nainstalovaný Agent virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="9b89e-144">If the **write-host** command returns **True**, the VM Agent is installed.</span></span> <span data-ttu-id="9b89e-145">Vrátí-li **False**, najdete pokyny a odkaz na stažení v Azure příspěvku na blogu [agenta virtuálního počítače a rozšíření – část 2](http://go.microsoft.com/fwlink/p/?LinkId=403947).</span><span class="sxs-lookup"><span data-stu-id="9b89e-145">If it returns **False**, see the instructions and a link to the download in the Azure blog post [VM Agent and Extensions - Part 2](http://go.microsoft.com/fwlink/p/?LinkId=403947).</span></span>

<span data-ttu-id="9b89e-146">Pokud je nainstalován Agent virtuálního počítače, spusťte tyto příkazy.</span><span class="sxs-lookup"><span data-stu-id="9b89e-146">If the VM Agent is installed, run these commands.</span></span>

    $Agent = Get-AzureVMAvailableExtension TrendMicro.DeepSecurity -ExtensionName TrendMicroDSA

    Set-AzureVMExtension -Publisher TrendMicro.DeepSecurity –Version $Agent.Version -ExtensionName TrendMicroDSA -VM $vm | Update-AzureVM

## <a name="next-steps"></a><span data-ttu-id="9b89e-147">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9b89e-147">Next steps</span></span>
<span data-ttu-id="9b89e-148">Jak dlouho trvá několik minut začít spouštět po instalaci agenta.</span><span class="sxs-lookup"><span data-stu-id="9b89e-148">It takes a few minutes for the agent to start running when it is installed.</span></span> <span data-ttu-id="9b89e-149">Potom musíte aktivovat hloubkové zabezpečení na virtuálním počítači, můžete spravovat pomocí hloubkové zabezpečení správce.</span><span class="sxs-lookup"><span data-stu-id="9b89e-149">After that, you need to activate Deep Security on the virtual machine so it can be managed by a Deep Security Manager.</span></span> <span data-ttu-id="9b89e-150">Najdete v následujících článcích další pokyny:</span><span class="sxs-lookup"><span data-stu-id="9b89e-150">See the following articles for additional instructions:</span></span>

* <span data-ttu-id="9b89e-151">Trend na článek o tomto řešení [Instant-On cloudu zabezpečení pro Microsoft Azure](http://go.microsoft.com/fwlink/?LinkId=404101)</span><span class="sxs-lookup"><span data-stu-id="9b89e-151">Trend's article about this solution, [Instant-On Cloud Security for Microsoft Azure](http://go.microsoft.com/fwlink/?LinkId=404101)</span></span>
* <span data-ttu-id="9b89e-152">A [ukázkový skript prostředí Windows PowerShell](http://go.microsoft.com/fwlink/?LinkId=404100) ke konfiguraci virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="9b89e-152">A [sample Windows PowerShell script](http://go.microsoft.com/fwlink/?LinkId=404100) to configure the virtual machine</span></span>
* <span data-ttu-id="9b89e-153">[Pokyny](http://go.microsoft.com/fwlink/?LinkId=404099) pro ukázku</span><span class="sxs-lookup"><span data-stu-id="9b89e-153">[Instructions](http://go.microsoft.com/fwlink/?LinkId=404099) for the sample</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9b89e-154">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="9b89e-154">Additional resources</span></span>
<span data-ttu-id="9b89e-155">[Jak se přihlásit do virtuálního počítače se systémem Windows Server]</span><span class="sxs-lookup"><span data-stu-id="9b89e-155">[How to log on to a virtual machine running Windows Server]</span></span>

<span data-ttu-id="9b89e-156">[Rozšíření virtuálního počítače Azure a funkce]</span><span class="sxs-lookup"><span data-stu-id="9b89e-156">[Azure VM Extensions and features]</span></span>

<!-- Image references -->
[1]: ./media/install-trend/new_vm_Blade3.png
[2]: ./media/install-trend/find_SecurityAgent.png
[3]: ./media/install-trend/SecurityAgentDetails.png

<!-- Link references -->
<span data-ttu-id="9b89e-157">[Jak se přihlásit do virtuálního počítače se systémem Windows Server]:connect-logon.md</span><span class="sxs-lookup"><span data-stu-id="9b89e-157">[How to log on to a virtual machine running Windows Server]:connect-logon.md</span></span>
<span data-ttu-id="9b89e-158">[Rozšíření virtuálního počítače Azure a funkce]: http://go.microsoft.com/fwlink/p/?linkid=390493&clcid=0x409</span><span class="sxs-lookup"><span data-stu-id="9b89e-158">[Azure VM Extensions and features]: http://go.microsoft.com/fwlink/p/?linkid=390493&clcid=0x409</span></span>
