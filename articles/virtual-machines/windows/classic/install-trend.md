---
title: "aaaInstall Trend Micro hluboké Security na virtuálním počítači | Microsoft Docs"
description: "Tento článek popisuje, jak tooinstall a konfigurace Trend Micro zabezpečení na virtuálním počítači vytvořené pomocí modelu nasazení Classic hello v Azure."
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
ms.openlocfilehash: dc5492db07a37a2296df5da673183a14c6d5b1f2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooinstall-and-configure-trend-micro-deep-security-as-a-service-on-a-windows-vm"></a><span data-ttu-id="6a785-103">Jak tooinstall a nakonfigurujte Trend Micro hluboké Security jako služba na virtuální počítač s Windows</span><span class="sxs-lookup"><span data-stu-id="6a785-103">How tooinstall and configure Trend Micro Deep Security as a Service on a Windows VM</span></span>
> [!IMPORTANT]
> <span data-ttu-id="6a785-104">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="6a785-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="6a785-105">Tento článek se zabývá pomocí modelu nasazení Classic hello.</span><span class="sxs-lookup"><span data-stu-id="6a785-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="6a785-106">Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="6a785-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span>

<span data-ttu-id="6a785-107">Tento článek ukazuje, jak tooinstall a nakonfigurujte Trend Micro hluboké Security jako služba na nový nebo existující virtuální počítač (VM) s Windows serverem.</span><span class="sxs-lookup"><span data-stu-id="6a785-107">This article shows you how tooinstall and configure Trend Micro Deep Security as a Service on a new or existing virtual machine (VM) running Windows Server.</span></span> <span data-ttu-id="6a785-108">Hloubkové zabezpečení jako služba zahrnuje ochrana proti malwaru, brána firewall, narušení prevence systém a monitorování integrity.</span><span class="sxs-lookup"><span data-stu-id="6a785-108">Deep Security as a Service includes anti-malware protection, a firewall, an intrusion prevention system, and integrity monitoring.</span></span>

<span data-ttu-id="6a785-109">Hello klienta se instaluje jako rozšíření zabezpečení prostřednictvím hello agenta virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="6a785-109">hello client is installed as a security extension via hello VM Agent.</span></span> <span data-ttu-id="6a785-110">Na nový virtuální počítač nainstalujte hello hloubkové Agent zabezpečení, jako hello agenta virtuálního počítače je vytvářena automaticky nástrojem hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="6a785-110">On a new virtual machine, you install hello Deep Security Agent, as hello VM Agent is created automatically by hello Azure portal.</span></span>

<span data-ttu-id="6a785-111">Existující virtuální počítač vytvořený pomocí portálu classic hello, hello rozhraní příkazového řádku Azure nebo PowerShell nemusí mít agenta virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="6a785-111">An existing VM created using hello classic portal, hello Azure CLI, or PowerShell might not have a VM agent.</span></span> <span data-ttu-id="6a785-112">Pro stávajícího virtuálního počítače, který nemá hello agenta virtuálního počítače třeba toodownload a nainstalujte ji jako první.</span><span class="sxs-lookup"><span data-stu-id="6a785-112">For an existing virtual machine that doesn't have hello VM Agent, you need toodownload and install it first.</span></span> <span data-ttu-id="6a785-113">Tento článek se týká obou situacích.</span><span class="sxs-lookup"><span data-stu-id="6a785-113">This article covers both situations.</span></span>

<span data-ttu-id="6a785-114">Pokud máte z Trend Micro aktuální předplatné pro místní řešení, můžete ho toohelp chránit virtuální počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="6a785-114">If you have a current subscription from Trend Micro for an on-premises solution, you can use it toohelp protect your Azure virtual machines.</span></span> <span data-ttu-id="6a785-115">Pokud vám ještě nejsou zákazníka, můžete zaregistrovat zkušební předplatné.</span><span class="sxs-lookup"><span data-stu-id="6a785-115">If you're not a customer yet, you can sign up for a trial subscription.</span></span> <span data-ttu-id="6a785-116">Další informace o tomto řešení najdete v příspěvku blogu Trend Micro hello [Microsoft Azure virtuálního počítače agenta rozšíření pro přímý zabezpečení](http://go.microsoft.com/fwlink/p/?LinkId=403945).</span><span class="sxs-lookup"><span data-stu-id="6a785-116">For more information about this solution, see hello Trend Micro blog post [Microsoft Azure VM Agent Extension For Deep Security](http://go.microsoft.com/fwlink/p/?LinkId=403945).</span></span>

## <a name="install-hello-deep-security-agent-on-a-new-vm"></a><span data-ttu-id="6a785-117">Nainstalujte hello hluboké Security Agent na nový virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="6a785-117">Install hello Deep Security Agent on a new VM</span></span>

<span data-ttu-id="6a785-118">Hello [portál Azure](http://portal.azure.com) umožňuje nainstalovat rozšíření zabezpečení Trend Micro hello při použití bitové kopie z hello **Marketplace** toocreate hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="6a785-118">hello [Azure portal](http://portal.azure.com) lets you install hello Trend Micro security extension when you use an image from hello **Marketplace** toocreate hello virtual machine.</span></span> <span data-ttu-id="6a785-119">Pokud vytváříte jeden virtuální počítač, pomocí portálu hello je snadný způsob tooadd ochrany z Trend Micro.</span><span class="sxs-lookup"><span data-stu-id="6a785-119">If you're creating a single virtual machine, using hello portal is an easy way tooadd protection from Trend Micro.</span></span>

<span data-ttu-id="6a785-120">Pomocí položky z hello **Marketplace** otevře průvodce, který vám pomůže nastavit hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="6a785-120">Using an entry from hello **Marketplace** opens a wizard that helps you set up hello virtual machine.</span></span> <span data-ttu-id="6a785-121">Použít hello **nastavení** okna, panel třetí hello hello průvodci tooinstall hello Trend Micro rozšíření zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="6a785-121">You use hello **Settings** blade, hello third panel of hello wizard, tooinstall hello Trend Micro security extension.</span></span>  <span data-ttu-id="6a785-122">Obecné pokyny najdete v tématu [vytvoření virtuálního počítače se systémem Windows v portálu Azure hello](tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="6a785-122">For general instructions, see [Create a virtual machine running Windows in hello Azure portal](tutorial.md).</span></span>

<span data-ttu-id="6a785-123">Když získáte toohello **nastavení** okno Průvodce hello hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="6a785-123">When you get toohello **Settings** blade of hello wizard, do hello following steps:</span></span>

1. <span data-ttu-id="6a785-124">Klikněte na tlačítko **rozšíření**, pak klikněte na tlačítko **přidat rozšíření** v podokně Další hello.</span><span class="sxs-lookup"><span data-stu-id="6a785-124">Click **Extensions**, then click **Add extension** in hello next pane.</span></span>

   ![Začněte přidávat hello rozšíření][1]

2. <span data-ttu-id="6a785-126">Vyberte **hloubkové Agent zabezpečení** v hello **nový prostředek** podokně.</span><span class="sxs-lookup"><span data-stu-id="6a785-126">Select **Deep Security Agent** in hello **New resource** pane.</span></span> <span data-ttu-id="6a785-127">V podokně hello hloubkové Agent zabezpečení, klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="6a785-127">In hello Deep Security Agent pane, click **Create**.</span></span>

   ![Identifikovat Agent hloubkové zabezpečení][2]

3. <span data-ttu-id="6a785-129">Zadejte hello **identifikátor klienta** a **heslo aktivace klienta** hello rozšíření.</span><span class="sxs-lookup"><span data-stu-id="6a785-129">Enter hello **Tenant Identifier** and **Tenant Activation Password** for hello extension.</span></span> <span data-ttu-id="6a785-130">Volitelně můžete zadat **identifikátor zásady zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="6a785-130">Optionally, you can enter a **Security Policy Identifier**.</span></span> <span data-ttu-id="6a785-131">Potom klikněte na **OK** tooadd hello klienta.</span><span class="sxs-lookup"><span data-stu-id="6a785-131">Then, click **OK** tooadd hello client.</span></span>

   ![Zadejte podrobnosti o rozšíření][3]

## <a name="install-hello-deep-security-agent-on-an-existing-vm"></a><span data-ttu-id="6a785-133">Nainstalujte hello hluboké Security Agent na existující virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="6a785-133">Install hello Deep Security Agent on an existing VM</span></span>
<span data-ttu-id="6a785-134">agent hello tooinstall na existující virtuální počítač, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="6a785-134">tooinstall hello agent on an existing VM, you need hello following items:</span></span>

* <span data-ttu-id="6a785-135">modul Azure PowerShell text Hello, verze 0.8.2 nebo novější, nainstalovaná v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="6a785-135">hello Azure PowerShell module, version 0.8.2 or newer, installed on your local computer.</span></span> <span data-ttu-id="6a785-136">Můžete zkontrolovat hello verzi prostředí Azure PowerShell, který jste nainstalovali pomocí hello **Get-Module azure | verze formátu tabulky** příkaz.</span><span class="sxs-lookup"><span data-stu-id="6a785-136">You can check hello version of Azure PowerShell that you have installed by using hello **Get-Module azure | format-table version** command.</span></span> <span data-ttu-id="6a785-137">Pokyny a odkaz nejnovější verzi toohello najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="6a785-137">For instructions and a link toohello latest version, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span> <span data-ttu-id="6a785-138">Přihlaste se pomocí předplatného Azure tooyour `Add-AzureAccount`.</span><span class="sxs-lookup"><span data-stu-id="6a785-138">Log in tooyour Azure subscription using `Add-AzureAccount`.</span></span>
* <span data-ttu-id="6a785-139">Hello Agent virtuálního počítače nainstalovaný na hello cílového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="6a785-139">hello VM Agent installed on hello target virtual machine.</span></span>

<span data-ttu-id="6a785-140">Nejprve ověří, že hello agenta virtuálního počítače je již nainstalována.</span><span class="sxs-lookup"><span data-stu-id="6a785-140">First, verify that hello VM Agent is already installed.</span></span> <span data-ttu-id="6a785-141">Zadejte název hello cloudové služby a název virtuálního počítače a pak spusťte následující příkazy příkazového řádku Azure PowerShell úrovni správce hello.</span><span class="sxs-lookup"><span data-stu-id="6a785-141">Fill in hello cloud service name and virtual machine name, and then run hello following commands at an administrator-level Azure PowerShell command prompt.</span></span> <span data-ttu-id="6a785-142">Nahraďte vše v rámci hello uvozovky, včetně hello < a > znaků.</span><span class="sxs-lookup"><span data-stu-id="6a785-142">Replace everything within hello quotes, including hello < and > characters.</span></span>

    $CSName = "<cloud service name>"
    $VMName = "<virtual machine name>"
    $vm = Get-AzureVM -ServiceName $CSName -Name $VMName
    write-host $vm.VM.ProvisionGuestAgent

<span data-ttu-id="6a785-143">Pokud si nejste jisti hello Cloudová služba a název virtuálního počítače, spusťte **Get-AzureVM** toodisplay že hello informace pro všechny virtuální počítače v aktuálním předplatném.</span><span class="sxs-lookup"><span data-stu-id="6a785-143">If you don't know hello cloud service and virtual machine name, run **Get-AzureVM** toodisplay that information for all hello virtual machines in your current subscription.</span></span>

<span data-ttu-id="6a785-144">Pokud hello **zápisu hostitele** příkaz vrátí **True**, hello je nainstalovaný Agent virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="6a785-144">If hello **write-host** command returns **True**, hello VM Agent is installed.</span></span> <span data-ttu-id="6a785-145">Vrátí-li **False**, najdete v části hello pokyny a odkaz toohello, stáhněte si v hello Azure příspěvku na blogu [agenta virtuálního počítače a rozšíření – část 2](http://go.microsoft.com/fwlink/p/?LinkId=403947).</span><span class="sxs-lookup"><span data-stu-id="6a785-145">If it returns **False**, see hello instructions and a link toohello download in hello Azure blog post [VM Agent and Extensions - Part 2](http://go.microsoft.com/fwlink/p/?LinkId=403947).</span></span>

<span data-ttu-id="6a785-146">Pokud je nainstalovaná hello agenta virtuálního počítače, spusťte tyto příkazy.</span><span class="sxs-lookup"><span data-stu-id="6a785-146">If hello VM Agent is installed, run these commands.</span></span>

    $Agent = Get-AzureVMAvailableExtension TrendMicro.DeepSecurity -ExtensionName TrendMicroDSA

    Set-AzureVMExtension -Publisher TrendMicro.DeepSecurity –Version $Agent.Version -ExtensionName TrendMicroDSA -VM $vm | Update-AzureVM

## <a name="next-steps"></a><span data-ttu-id="6a785-147">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6a785-147">Next steps</span></span>
<span data-ttu-id="6a785-148">Jak dlouho trvá několik minut, než toostart hello agent spuštěn, když je nainstalovaná.</span><span class="sxs-lookup"><span data-stu-id="6a785-148">It takes a few minutes for hello agent toostart running when it is installed.</span></span> <span data-ttu-id="6a785-149">Potom musíte tooactivate hloubkové zabezpečení na virtuálním počítači hello tak můžete spravovat pomocí hloubkové zabezpečení správce.</span><span class="sxs-lookup"><span data-stu-id="6a785-149">After that, you need tooactivate Deep Security on hello virtual machine so it can be managed by a Deep Security Manager.</span></span> <span data-ttu-id="6a785-150">Viz následující články pro další pokyny hello:</span><span class="sxs-lookup"><span data-stu-id="6a785-150">See hello following articles for additional instructions:</span></span>

* <span data-ttu-id="6a785-151">Trend na článek o tomto řešení [Instant-On cloudu zabezpečení pro Microsoft Azure](http://go.microsoft.com/fwlink/?LinkId=404101)</span><span class="sxs-lookup"><span data-stu-id="6a785-151">Trend's article about this solution, [Instant-On Cloud Security for Microsoft Azure](http://go.microsoft.com/fwlink/?LinkId=404101)</span></span>
* <span data-ttu-id="6a785-152">A [ukázkový skript prostředí Windows PowerShell](http://go.microsoft.com/fwlink/?LinkId=404100) tooconfigure hello virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="6a785-152">A [sample Windows PowerShell script](http://go.microsoft.com/fwlink/?LinkId=404100) tooconfigure hello virtual machine</span></span>
* <span data-ttu-id="6a785-153">[Pokyny](http://go.microsoft.com/fwlink/?LinkId=404099) pro ukázku hello</span><span class="sxs-lookup"><span data-stu-id="6a785-153">[Instructions](http://go.microsoft.com/fwlink/?LinkId=404099) for hello sample</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6a785-154">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="6a785-154">Additional resources</span></span>
<span data-ttu-id="6a785-155">[Jak toolog na tooa virtuální počítač se systémem Windows Server]</span><span class="sxs-lookup"><span data-stu-id="6a785-155">[How toolog on tooa virtual machine running Windows Server]</span></span>

<span data-ttu-id="6a785-156">[Rozšíření virtuálního počítače Azure a funkce]</span><span class="sxs-lookup"><span data-stu-id="6a785-156">[Azure VM Extensions and features]</span></span>

<!-- Image references -->
[1]: ./media/install-trend/new_vm_Blade3.png
[2]: ./media/install-trend/find_SecurityAgent.png
[3]: ./media/install-trend/SecurityAgentDetails.png

<!-- Link references -->
[Jak toolog na tooa virtuální počítač se systémem Windows Server]:connect-logon.md
[Rozšíření virtuálního počítače Azure a funkce]: http://go.microsoft.com/fwlink/p/?linkid=390493&clcid=0x409
