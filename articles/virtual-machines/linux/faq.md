---
title: "Nejčastější dotazy pro virtuální počítače s Linuxem v Azure | Microsoft Docs"
description: "Poskytuje odpovědi na některé časté otázky týkající se virtuální počítače Linux vytvořené pomocí modelu Resource Manager."
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-management
ms.assetid: 3648e09c-1115-4818-93c6-688d7a54a353
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: cynthn
ms.openlocfilehash: 0e06d21bd0b6ef807f38e41dcd50c9cd715607a3
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="frequently-asked-question-about-linux-virtual-machines"></a><span data-ttu-id="21d81-103">Časté otázky o virtuálních počítačích s Linuxem</span><span class="sxs-lookup"><span data-stu-id="21d81-103">Frequently asked question about Linux Virtual Machines</span></span>
<span data-ttu-id="21d81-104">Tento článek se zaměřuje na některé běžné dotazy týkající se virtuální počítače Linux vytvořené v Azure pomocí modelu nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="21d81-104">This article addresses some common questions about Linux virtual machines created in Azure using the Resource Manager deployment model.</span></span> <span data-ttu-id="21d81-105">Windows verzi tohoto tématu naleznete v části [často kladené otázky o virtuálních počítačích s Windows](../windows/faq.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="21d81-105">For the Windows version of this topic, see [Frequently asked question about Windows Virtual Machines](../windows/faq.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span></span>

## <a name="what-can-i-run-on-an-azure-vm"></a><span data-ttu-id="21d81-106">Co můžu spouštět na virtuálním počítači Azure?</span><span class="sxs-lookup"><span data-stu-id="21d81-106">What can I run on an Azure VM?</span></span>
<span data-ttu-id="21d81-107">Všichni předplatitelé můžou na virtuálním počítači Azure spouštět serverový software.</span><span class="sxs-lookup"><span data-stu-id="21d81-107">All subscribers can run server software on an Azure virtual machine.</span></span> <span data-ttu-id="21d81-108">Další informace najdete v tématu [Linux na Azure-Endorsed distribuce](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="21d81-108">For more information, see [Linux on Azure-Endorsed Distributions](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>

## <a name="how-much-storage-can-i-use-with-a-virtual-machine"></a><span data-ttu-id="21d81-109">Kolik úložiště můžu využít s virtuálním počítačem?</span><span class="sxs-lookup"><span data-stu-id="21d81-109">How much storage can I use with a virtual machine?</span></span>
<span data-ttu-id="21d81-110">Každý datový disk může mít velikost až 1 TB.</span><span class="sxs-lookup"><span data-stu-id="21d81-110">Each data disk can be up to 1 TB.</span></span> <span data-ttu-id="21d81-111">Počet datových disků, které můžete využít, závisí na velikosti virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="21d81-111">The number of data disks you can use depends on the size of the virtual machine.</span></span> <span data-ttu-id="21d81-112">Podrobnosti najdete v článku [Velikosti služeb Virtual Machines](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="21d81-112">For details, see [Sizes for Virtual Machines](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="21d81-113">Účet úložiště Azure poskytuje úložiště pro disk operačního systému a všechny datové disky.</span><span class="sxs-lookup"><span data-stu-id="21d81-113">An Azure storage account provides storage for the operating system disk and any data disks.</span></span> <span data-ttu-id="21d81-114">Každý disk je soubor .vhd uložený jako objekt blob stránky.</span><span class="sxs-lookup"><span data-stu-id="21d81-114">Each disk is a .vhd file stored as a page blob.</span></span> <span data-ttu-id="21d81-115">Podrobnosti o cenách najdete v tématu [Podrobnosti o cenách úložiště](https://azure.microsoft.com/pricing/details/storage/).</span><span class="sxs-lookup"><span data-stu-id="21d81-115">For pricing details, see [Storage Pricing Details](https://azure.microsoft.com/pricing/details/storage/).</span></span>

## <a name="how-can-i-access-my-virtual-machine"></a><span data-ttu-id="21d81-116">Jak lze získat přístup k virtuální počítač?</span><span class="sxs-lookup"><span data-stu-id="21d81-116">How can I access my virtual machine?</span></span>
<span data-ttu-id="21d81-117">Vytvořit vzdálené připojení k přihlášení k virtuálnímu počítači pomocí protokolu Secure Shell (SSH).</span><span class="sxs-lookup"><span data-stu-id="21d81-117">Establish a remote connection to log on to the virtual machine, using Secure Shell (SSH).</span></span> <span data-ttu-id="21d81-118">Přečtěte si pokyny o tom, jak připojit [ze systému Windows](ssh-from-windows.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) nebo [z Linuxu a Macu](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="21d81-118">See the instructions on how to connect [from Windows](ssh-from-windows.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) or [from Linux and Mac](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="21d81-119">SSH ve výchozím nastavení umožňuje maximálně 10 souběžných připojení.</span><span class="sxs-lookup"><span data-stu-id="21d81-119">By default, SSH allows a maximum of 10 concurrent connections.</span></span> <span data-ttu-id="21d81-120">Toto číslo můžete navýšit upravením konfiguračního souboru.</span><span class="sxs-lookup"><span data-stu-id="21d81-120">You can increase this number by editing the configuration file.</span></span>

<span data-ttu-id="21d81-121">Pokud máte problémy, podívejte se na [řešení potíží s Secure Shell (SSH) připojení](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="21d81-121">If you’re having problems, check out [Troubleshoot Secure Shell (SSH) connections](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="can-i-use-the-temporary-disk-devsdb1-to-store-data"></a><span data-ttu-id="21d81-122">Můžete použít dočasným diskovým (/ dev/sdb1) k uložení dat?</span><span class="sxs-lookup"><span data-stu-id="21d81-122">Can I use the temporary disk (/dev/sdb1) to store data?</span></span>
<span data-ttu-id="21d81-123">K ukládání dat nepoužívejte dočasným diskovým (/ dev/sdb1).</span><span class="sxs-lookup"><span data-stu-id="21d81-123">Don't use the temporary disk (/dev/sdb1) to store data.</span></span> <span data-ttu-id="21d81-124">Pro dočasné úložiště je pouze existuje.</span><span class="sxs-lookup"><span data-stu-id="21d81-124">It is only there for temporary storage.</span></span> <span data-ttu-id="21d81-125">Riskujete ztráty dat, které nelze obnovit.</span><span class="sxs-lookup"><span data-stu-id="21d81-125">You risk losing data that can’t be recovered.</span></span>

## <a name="can-i-copy-or-clone-an-existing-azure-vm"></a><span data-ttu-id="21d81-126">Můžete kopírovat nebo klonovat existující virtuální počítač Azure?</span><span class="sxs-lookup"><span data-stu-id="21d81-126">Can I copy or clone an existing Azure VM?</span></span>
<span data-ttu-id="21d81-127">Ano.</span><span class="sxs-lookup"><span data-stu-id="21d81-127">Yes.</span></span> <span data-ttu-id="21d81-128">Pokyny najdete v tématu [vytvoření kopie virtuální počítač s Linuxem v modelu nasazení Resource Manager](copy-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="21d81-128">For instructions, see [How to create a copy of a Linux virtual machine in the Resource Manager deployment model](copy-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="why-am-i-not-seeing-canada-central-and-canada-east-regions-through-azure-resource-manager"></a><span data-ttu-id="21d81-129">Proč nejsou zobrazeny Kanada centrální a Východní Kanada oblastí prostřednictvím Správce Azure Resource Manager?</span><span class="sxs-lookup"><span data-stu-id="21d81-129">Why am I not seeing Canada Central and Canada East regions through Azure Resource Manager?</span></span>
<span data-ttu-id="21d81-130">Dvě nové oblasti Kanada centrální a Východní Kanada nejsou automaticky registrované pro vytvoření virtuálního počítače pro existující předplatných Azure.</span><span class="sxs-lookup"><span data-stu-id="21d81-130">The two new regions of Canada Central and Canada East are not automatically registered for virtual machine creation for existing Azure subscriptions.</span></span> <span data-ttu-id="21d81-131">Tato registrace se provádí automaticky při nasazení virtuálního počítače prostřednictvím portálu Azure do žádné jiné oblasti pomocí Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="21d81-131">This registration is done automatically when a virtual machine is deployed through the Azure portal to any other region using Azure Resource Manager.</span></span> <span data-ttu-id="21d81-132">Po nasazení virtuálního počítače v jiné oblasti Azure nové oblasti musí být k dispozici pro následující virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="21d81-132">After a virtual machine is deployed to any other Azure region, the new regions should be available for subsequent virtual machines.</span></span>

## <a name="can-i-add-a-nic-to-my-vm-after-its-created"></a><span data-ttu-id="21d81-133">Mohu přidat síťový adaptér k virtuálnímu počítači po jeho vytvoření?</span><span class="sxs-lookup"><span data-stu-id="21d81-133">Can I add a NIC to my VM after it's created?</span></span>
<span data-ttu-id="21d81-134">Ano, to je nyní možné.</span><span class="sxs-lookup"><span data-stu-id="21d81-134">Yes, this is now possible.</span></span> <span data-ttu-id="21d81-135">Virtuální počítač nejdřív je potřeba zastavit deallocated.</span><span class="sxs-lookup"><span data-stu-id="21d81-135">The VM first needs to be stopped deallocated.</span></span> <span data-ttu-id="21d81-136">Potom můžete přidat nebo odebrat síťový adaptér (Pokud je poslední síťový adaptér ve virtuálním počítači).</span><span class="sxs-lookup"><span data-stu-id="21d81-136">Then you can add or remove a NIC (unless it's the last NIC on the VM).</span></span> 

## <a name="are-there-any-computer-name-requirements"></a><span data-ttu-id="21d81-137">Existují jakékoli požadavky na název počítače?</span><span class="sxs-lookup"><span data-stu-id="21d81-137">Are there any computer name requirements?</span></span>
<span data-ttu-id="21d81-138">Ano.</span><span class="sxs-lookup"><span data-stu-id="21d81-138">Yes.</span></span> <span data-ttu-id="21d81-139">Název počítače nesmí být delší než 64 znaků.</span><span class="sxs-lookup"><span data-stu-id="21d81-139">The computer name can be a maximum of 64 characters in length.</span></span> <span data-ttu-id="21d81-140">V tématu [pojmenování konvence pravidla a omezení](/architecture/best-practices/naming-conventions#naming-rules-and-restrictions?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Další informace ohledně pojmenování vašich prostředků.</span><span class="sxs-lookup"><span data-stu-id="21d81-140">See [Naming conventions rules and restrictions](/architecture/best-practices/naming-conventions#naming-rules-and-restrictions?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) for more information around naming your resources.</span></span>

## <a name="are-there-any-resource-group-name-requirements"></a><span data-ttu-id="21d81-141">Dochází k jakémukoli prostředku, požadavky na název skupiny?</span><span class="sxs-lookup"><span data-stu-id="21d81-141">Are there any resource group name requirements?</span></span>
<span data-ttu-id="21d81-142">Ano.</span><span class="sxs-lookup"><span data-stu-id="21d81-142">Yes.</span></span> <span data-ttu-id="21d81-143">Název skupiny prostředků může být maximálně 90 znaků.</span><span class="sxs-lookup"><span data-stu-id="21d81-143">The resource group name can be a maximum of 90 characters in length.</span></span> <span data-ttu-id="21d81-144">V tématu [pojmenování konvence pravidla a omezení](/architecture/best-practices/naming-conventions#naming-rules-and-restrictions?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Další informace o skupinách prostředků.</span><span class="sxs-lookup"><span data-stu-id="21d81-144">See [Naming conventions rules and restrictions](/architecture/best-practices/naming-conventions#naming-rules-and-restrictions?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) for more information about resource groups.</span></span>

## <a name="what-are-the-username-requirements-when-creating-a-vm"></a><span data-ttu-id="21d81-145">Jaké jsou požadavky na uživatelské jméno při vytváření virtuálního počítače?</span><span class="sxs-lookup"><span data-stu-id="21d81-145">What are the username requirements when creating a VM?</span></span>
<span data-ttu-id="21d81-146">Uživatelská jména musí být 1-64 znaků.</span><span class="sxs-lookup"><span data-stu-id="21d81-146">Usernames must be 1 - 64 characters in length.</span></span>

<span data-ttu-id="21d81-147">Následující uživatelská jména nejsou povoleny:</span><span class="sxs-lookup"><span data-stu-id="21d81-147">The following usernames are not allowed:</span></span>

<table>
    <tr>
        <td style="text-align:center"><span data-ttu-id="21d81-148">Správce</span><span class="sxs-lookup"><span data-stu-id="21d81-148">administrator</span></span> </td><td style="text-align:center"> <span data-ttu-id="21d81-149">Správce</span><span class="sxs-lookup"><span data-stu-id="21d81-149">admin</span></span> </td><td style="text-align:center"> <span data-ttu-id="21d81-150">Uživatel</span><span class="sxs-lookup"><span data-stu-id="21d81-150">user</span></span> </td><td style="text-align:center"> <span data-ttu-id="21d81-151">Uživatel1</span><span class="sxs-lookup"><span data-stu-id="21d81-151">user1</span></span></td>
    </tr>
    <tr>
        <td style="text-align:center"><span data-ttu-id="21d81-152">Test</span><span class="sxs-lookup"><span data-stu-id="21d81-152">test</span></span> </td><td style="text-align:center"> <span data-ttu-id="21d81-153">uživatel2</span><span class="sxs-lookup"><span data-stu-id="21d81-153">user2</span></span> </td><td style="text-align:center"> <span data-ttu-id="21d81-154">test1</span><span class="sxs-lookup"><span data-stu-id="21d81-154">test1</span></span> </td><td style="text-align:center"> <span data-ttu-id="21d81-155">UŽIVATEL3</span><span class="sxs-lookup"><span data-stu-id="21d81-155">user3</span></span></td>
    </tr>
    <tr>
        <td style="text-align:center"><span data-ttu-id="21d81-156">admin1</span><span class="sxs-lookup"><span data-stu-id="21d81-156">admin1</span></span> </td><td style="text-align:center"> <span data-ttu-id="21d81-157">1</span><span class="sxs-lookup"><span data-stu-id="21d81-157">1</span></span> </td><td style="text-align:center"> <span data-ttu-id="21d81-158">123</span><span class="sxs-lookup"><span data-stu-id="21d81-158">123</span></span> </td><td style="text-align:center"> <span data-ttu-id="21d81-159">A</span><span class="sxs-lookup"><span data-stu-id="21d81-159">a</span></span></td>
    </tr>
    <tr>
        <td style="text-align:center"><span data-ttu-id="21d81-160">actuser</span><span class="sxs-lookup"><span data-stu-id="21d81-160">actuser</span></span>  </td><td style="text-align:center"> <span data-ttu-id="21d81-161">ADM</span><span class="sxs-lookup"><span data-stu-id="21d81-161">adm</span></span> </td><td style="text-align:center"> <span data-ttu-id="21d81-162">admin2</span><span class="sxs-lookup"><span data-stu-id="21d81-162">admin2</span></span> </td><td style="text-align:center"> <span data-ttu-id="21d81-163">ASPNET</span><span class="sxs-lookup"><span data-stu-id="21d81-163">aspnet</span></span></td>
    </tr>
    <tr>
        <td style="text-align:center"><span data-ttu-id="21d81-164">zálohování</span><span class="sxs-lookup"><span data-stu-id="21d81-164">backup</span></span> </td><td style="text-align:center"> <span data-ttu-id="21d81-165">Konzola</span><span class="sxs-lookup"><span data-stu-id="21d81-165">console</span></span> </td><td style="text-align:center"> <span data-ttu-id="21d81-166">David</span><span class="sxs-lookup"><span data-stu-id="21d81-166">david</span></span> </td><td style="text-align:center"> <span data-ttu-id="21d81-167">hosta</span><span class="sxs-lookup"><span data-stu-id="21d81-167">guest</span></span></td>
    </tr>
    <tr>
        <td style="text-align:center"><span data-ttu-id="21d81-168">Jan</span><span class="sxs-lookup"><span data-stu-id="21d81-168">john</span></span> </td><td style="text-align:center"> <span data-ttu-id="21d81-169">Vlastník</span><span class="sxs-lookup"><span data-stu-id="21d81-169">owner</span></span> </td><td style="text-align:center"> <span data-ttu-id="21d81-170">kořenové</span><span class="sxs-lookup"><span data-stu-id="21d81-170">root</span></span> </td><td style="text-align:center"> <span data-ttu-id="21d81-171">server</span><span class="sxs-lookup"><span data-stu-id="21d81-171">server</span></span></td>
    </tr>
    <tr>
        <td style="text-align:center"><span data-ttu-id="21d81-172">SQL</span><span class="sxs-lookup"><span data-stu-id="21d81-172">sql</span></span> </td><td style="text-align:center"> <span data-ttu-id="21d81-173">Podpora</span><span class="sxs-lookup"><span data-stu-id="21d81-173">support</span></span> </td><td style="text-align:center"> <span data-ttu-id="21d81-174">support_388945a0</span><span class="sxs-lookup"><span data-stu-id="21d81-174">support_388945a0</span></span> </td><td style="text-align:center"> <span data-ttu-id="21d81-175">Sys</span><span class="sxs-lookup"><span data-stu-id="21d81-175">sys</span></span></td>
    </tr>
    <tr>
        <td style="text-align:center"><span data-ttu-id="21d81-176">test2</span><span class="sxs-lookup"><span data-stu-id="21d81-176">test2</span></span> </td><td style="text-align:center"> <span data-ttu-id="21d81-177">Test3</span><span class="sxs-lookup"><span data-stu-id="21d81-177">test3</span></span> </td><td style="text-align:center"> <span data-ttu-id="21d81-178">Uživatel4</span><span class="sxs-lookup"><span data-stu-id="21d81-178">user4</span></span> </td><td style="text-align:center"> <span data-ttu-id="21d81-179">user5</span><span class="sxs-lookup"><span data-stu-id="21d81-179">user5</span></span></td>
    </tr>
</table>


## <a name="what-are-the-password-requirements-when-creating-a-vm"></a><span data-ttu-id="21d81-180">Jaké jsou požadavky na heslo, když vytvoření virtuálního počítače?</span><span class="sxs-lookup"><span data-stu-id="21d81-180">What are the password requirements when creating a VM?</span></span>
<span data-ttu-id="21d81-181">Hesla musí mít délku 6 72 znaků a musí splňovat 3 z následujících 4 složitost:</span><span class="sxs-lookup"><span data-stu-id="21d81-181">Passwords must be 6 - 72 characters in length and meet 3 out of the following 4 complexity requirements:</span></span>

* <span data-ttu-id="21d81-182">Mít nižší znaků</span><span class="sxs-lookup"><span data-stu-id="21d81-182">Have lower characters</span></span>
* <span data-ttu-id="21d81-183">Horní znaky</span><span class="sxs-lookup"><span data-stu-id="21d81-183">Have upper characters</span></span>
* <span data-ttu-id="21d81-184">Mít číslice.</span><span class="sxs-lookup"><span data-stu-id="21d81-184">Have a digit</span></span>
* <span data-ttu-id="21d81-185">Mít speciální znak (regulární výraz odpovídat [\W_])</span><span class="sxs-lookup"><span data-stu-id="21d81-185">Have a special character (Regex match [\W_])</span></span>

<span data-ttu-id="21d81-186">Nejsou povoleny následující hesla:</span><span class="sxs-lookup"><span data-stu-id="21d81-186">The following passwords are not allowed:</span></span>

<table>
    <tr>
        <td style="text-align:center">abc@123</td>
        <td style="text-align:center"><span data-ttu-id="21d81-187">P@$$w0rd</span><span class="sxs-lookup"><span data-stu-id="21d81-187">P@$$w0rd</span></span></td>
        <td style="text-align:center">P@ssw0rd</td>
        <td style="text-align:center">P@ssword123</td>
        <td style="text-align:center"><span data-ttu-id="21d81-188">Pa$ $ aplikace word</span><span class="sxs-lookup"><span data-stu-id="21d81-188">Pa$$word</span></span></td>
    </tr>
    <tr>
        <td style="text-align:center">pass@word1</td>
        <td style="text-align:center"><span data-ttu-id="21d81-189">Heslo!</span><span class="sxs-lookup"><span data-stu-id="21d81-189">Password!</span></span></td>
        <td style="text-align:center"><span data-ttu-id="21d81-190">Heslo1</span><span class="sxs-lookup"><span data-stu-id="21d81-190">Password1</span></span></td>
        <td style="text-align:center"><span data-ttu-id="21d81-191">Password22</span><span class="sxs-lookup"><span data-stu-id="21d81-191">Password22</span></span></td>
        <td style="text-align:center"><span data-ttu-id="21d81-192">ILOVEYOU!</span><span class="sxs-lookup"><span data-stu-id="21d81-192">iloveyou!</span></span></td>
    </tr>
</table>
