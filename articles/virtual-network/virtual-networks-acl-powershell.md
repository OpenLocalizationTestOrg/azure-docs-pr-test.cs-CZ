---
title: "Správa seznamů řízení přístupu koncového bodu Azure | Prostředí PowerShell | Classic | Microsoft Docs"
description: "Naučte se spravovat seznamy ACL v prostředí PowerShell"
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
ms.assetid: c84e40af-f351-4572-b3f0-d572d46bafe7
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.openlocfilehash: c3476908447380ccd7e8b9c0f1c2a55ae763cc1e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="manage-endpoint-access-control-lists-using-powershell-in-the-classic-deployment-model"></a><span data-ttu-id="8bb76-103">Správa seznamů řízení přístupu koncový bod pomocí prostředí PowerShell v modelu nasazení classic</span><span class="sxs-lookup"><span data-stu-id="8bb76-103">Manage endpoint access control lists using PowerShell in the classic deployment model</span></span>
<span data-ttu-id="8bb76-104">Můžete vytvořit a spravovat sítě seznamy řízení přístupu (ACL) pro koncové body pomocí prostředí Azure PowerShell nebo na portálu Management Portal.</span><span class="sxs-lookup"><span data-stu-id="8bb76-104">You can create and manage Network Access Control Lists (ACLs) for endpoints by using Azure PowerShell or in the Management Portal.</span></span> <span data-ttu-id="8bb76-105">V tomto tématu najdete postupy pro seznam ACL běžné úkoly, které můžete dokončit pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="8bb76-105">In this topic, you'll find procedures for ACL common tasks that you can complete using PowerShell.</span></span> <span data-ttu-id="8bb76-106">Seznam prostředí Azure PowerShell rutin najdete v části [rutiny pro správu Azure](http://go.microsoft.com/fwlink/?LinkId=317721).</span><span class="sxs-lookup"><span data-stu-id="8bb76-106">For the list of Azure PowerShell cmdlets see [Azure Management Cmdlets](http://go.microsoft.com/fwlink/?LinkId=317721).</span></span> <span data-ttu-id="8bb76-107">Další informace o seznamy řízení přístupu najdete v tématu [co je seznamu pro řízení přístupu sítě (ACL)?](virtual-networks-acl.md).</span><span class="sxs-lookup"><span data-stu-id="8bb76-107">For more information about ACLs, see [What is a Network Access Control List (ACL)?](virtual-networks-acl.md).</span></span> <span data-ttu-id="8bb76-108">Pokud chcete spravovat vaše seznamy ACL pomocí portálu pro správu, najdete v části [jak nastavit koncové body k virtuálnímu počítači](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="8bb76-108">If you want to manage your ACLs by using the Management Portal, see [How to Set Up Endpoints to a Virtual Machine](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

## <a name="manage-network-acls-by-using-azure-powershell"></a><span data-ttu-id="8bb76-109">Spravovat seznamy ACL sítě pomocí prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="8bb76-109">Manage Network ACLs by using Azure PowerShell</span></span>
<span data-ttu-id="8bb76-110">Můžete použít rutiny prostředí Azure PowerShell k vytváření, odebrat a konfigurovat (set) sítě seznamy řízení přístupu (ACL).</span><span class="sxs-lookup"><span data-stu-id="8bb76-110">You can use Azure PowerShell cmdlets to create, remove, and configure (set) Network Access Control Lists (ACLs).</span></span> <span data-ttu-id="8bb76-111">Jsme zahrnuli několik příkladů způsoby, kterými můžete nakonfigurovat seznam ACL pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="8bb76-111">We've included a few examples of some of the ways you can configure an ACL using PowerShell.</span></span>

<span data-ttu-id="8bb76-112">Načtení úplný seznam rutin prostředí PowerShell seznamu ACL, můžete použít jednu z těchto věcí:</span><span class="sxs-lookup"><span data-stu-id="8bb76-112">To retrieve a complete list of the ACL PowerShell cmdlets, you can use either of the following:</span></span>

    Get-Help *AzureACL*
    Get-Command -Noun AzureACLConfig

### <a name="create-a-network-acl-with-rules-that-permit-access-from-a-remote-subnet"></a><span data-ttu-id="8bb76-113">Vytvoření seznamu ACL sítě pomocí pravidel, které umožňují přístup ze vzdálené podsíti</span><span class="sxs-lookup"><span data-stu-id="8bb76-113">Create a Network ACL with rules that permit access from a remote subnet</span></span>
<span data-ttu-id="8bb76-114">Následující příklad ukazuje způsob, jak vytvořit nový seznamu ACL, který obsahuje pravidla.</span><span class="sxs-lookup"><span data-stu-id="8bb76-114">The example below illustrates a way to create a new ACL that contains rules.</span></span> <span data-ttu-id="8bb76-115">Tento seznam ACL se pak použije pro koncový bod virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="8bb76-115">This ACL is then applied to a virtual machine endpoint.</span></span> <span data-ttu-id="8bb76-116">Pravidla seznamu ACL v následujícím příkladu povolí přístup ze vzdálené podsíti.</span><span class="sxs-lookup"><span data-stu-id="8bb76-116">The ACL rules in the example below will allow access from a remote subnet.</span></span> <span data-ttu-id="8bb76-117">Chcete-li vytvořit nové sítě ACL s povolení pravidla pro vzdálené podsíti, otevřete Azure PowerShell ISE.</span><span class="sxs-lookup"><span data-stu-id="8bb76-117">To create a new Network ACL with permit rules for a remote subnet, open an Azure PowerShell ISE.</span></span> <span data-ttu-id="8bb76-118">Zkopírujte a vložte níže, konfigurace skriptu s vlastními hodnotami skript a spusťte skript.</span><span class="sxs-lookup"><span data-stu-id="8bb76-118">Copy and paste the script below, configuring the script with your own values, and then run the script.</span></span>

1. <span data-ttu-id="8bb76-119">Vytvořte nový objekt seznamu ACL sítě.</span><span class="sxs-lookup"><span data-stu-id="8bb76-119">Create the new network ACL object.</span></span>
   
        $acl1 = New-AzureAclConfig
2. <span data-ttu-id="8bb76-120">Nastavte pravidlo, která umožňuje přístup ze vzdálené podsíti.</span><span class="sxs-lookup"><span data-stu-id="8bb76-120">Set a rule that permits access from a remote subnet.</span></span> <span data-ttu-id="8bb76-121">V následujícím příkladu můžete nastavit pravidlo *100* (která má přednost před pravidlo 200 a vyšší) umožňující vzdálené podsíti *10.0.0.0/8* přístup k koncový bod virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="8bb76-121">In the example below, you set rule *100* (which has priority over rule 200 and higher) to allow the remote subnet *10.0.0.0/8* access to the virtual machine endpoint.</span></span> <span data-ttu-id="8bb76-122">Nahraďte hodnoty vlastními požadavky na konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="8bb76-122">Replace the values with your own configuration requirements.</span></span> <span data-ttu-id="8bb76-123">Název "Konfigurace služby SharePoint ACL" by měl být nahrazen popisný název, který chcete volat toto pravidlo.</span><span class="sxs-lookup"><span data-stu-id="8bb76-123">The name "SharePoint ACL config" should be replaced with the friendly name that you want to call this rule.</span></span>
   
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 100 `
            –Action permit –RemoteSubnet "10.0.0.0/8" `
            –Description "SharePoint ACL config"
3. <span data-ttu-id="8bb76-124">Pro další pravidla opakujte rutinu a nahraďte hodnoty s vlastní požadavky na konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="8bb76-124">For additional rules, repeat the cmdlet, replacing the values with your own configuration requirements.</span></span> <span data-ttu-id="8bb76-125">Nezapomeňte změnit pravidlo číslo pořadí tak, aby odrážela pořadí, ve kterém chcete pravidla, která má být použita.</span><span class="sxs-lookup"><span data-stu-id="8bb76-125">Be sure to change the rule number Order to reflect the order in which you want the rules to be applied.</span></span> <span data-ttu-id="8bb76-126">Nižší číslo pravidla mají přednost před vyšší číslo.</span><span class="sxs-lookup"><span data-stu-id="8bb76-126">The lower rule number takes precedence over the higher number.</span></span>
   
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 200 `
            –Action permit –RemoteSubnet "157.0.0.0/8" `
            –Description "web frontend ACL config"
4. <span data-ttu-id="8bb76-127">Dále můžete buď vytvořit nový koncový bod (Přidat) nebo nastavit seznam řízení přístupu pro existující koncový bod (Set).</span><span class="sxs-lookup"><span data-stu-id="8bb76-127">Next, you can either create a new endpoint (Add) or set the ACL for an existing endpoint (Set).</span></span> <span data-ttu-id="8bb76-128">V tomto příkladu jsme přidáte nový koncový bod virtuálního počítače názvem "web" a aktualizovat koncový bod virtuálního počítače s nastavením seznamu ACL.</span><span class="sxs-lookup"><span data-stu-id="8bb76-128">In this example, we will add a new virtual machine endpoint called "web" and update the virtual machine endpoint with the ACL settings.</span></span>
   
        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Add-AzureEndpoint –Name "web" –Protocol tcp –Localport 80 - PublicPort 80 –ACL $acl1 `
        | Update-AzureVM
5. <span data-ttu-id="8bb76-129">V dalším kroku kombinace rutiny a spusťte skript.</span><span class="sxs-lookup"><span data-stu-id="8bb76-129">Next, combine the cmdlets and run the script.</span></span> <span data-ttu-id="8bb76-130">V tomto příkladu rutiny kombinované bude vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="8bb76-130">For this example, the combined cmdlets would look like this:</span></span>
   
        $acl1 = New-AzureAclConfig
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 100 `
            –Action permit –RemoteSubnet "10.0.0.0/8" `
            –Description "Sharepoint ACL config"
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 200 `
            –Action permit –RemoteSubnet "157.0.0.0/8" `
            –Description "web frontend ACL config"
        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        |Add-AzureEndpoint –Name "web" –Protocol tcp –Localport 80 - PublicPort 80 –ACL $acl1 `
        |Update-AzureVM

### <a name="remove-a-network-acl-rule-that-permits-access-from-a-remote-subnet"></a><span data-ttu-id="8bb76-131">Odebrat pravidlo seznamu ACL sítě, která umožňuje přístup ze vzdálené podsíti</span><span class="sxs-lookup"><span data-stu-id="8bb76-131">Remove a Network ACL rule that permits access from a remote subnet</span></span>
<span data-ttu-id="8bb76-132">Následující příklad ukazuje způsob, jak odebrat pravidlo seznamu ACL sítě.</span><span class="sxs-lookup"><span data-stu-id="8bb76-132">The example below illustrates a way to remove a network ACL rule.</span></span>  <span data-ttu-id="8bb76-133">Chcete-li odebrat pravidlo seznamu ACL sítě s povolení pravidla pro vzdálené podsíti, otevřete Azure PowerShell ISE.</span><span class="sxs-lookup"><span data-stu-id="8bb76-133">To remove a Network ACL rule with permit rules for a remote subnet, open an Azure PowerShell ISE.</span></span> <span data-ttu-id="8bb76-134">Zkopírujte a vložte níže, konfigurace skriptu s vlastními hodnotami skript a spusťte skript.</span><span class="sxs-lookup"><span data-stu-id="8bb76-134">Copy and paste the script below, configuring the script with your own values, and then run the script.</span></span>

1. <span data-ttu-id="8bb76-135">Prvním krokem je GET pro objekt sítě ACL pro koncový bod virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="8bb76-135">First step is to get the Network ACL object for the virtual machine endpoint.</span></span> <span data-ttu-id="8bb76-136">Budete pak odeberte pravidlo seznamu ACL.</span><span class="sxs-lookup"><span data-stu-id="8bb76-136">You'll then remove the ACL rule.</span></span> <span data-ttu-id="8bb76-137">V takovém případě jsme se odebrat ji podle ID pravidla.</span><span class="sxs-lookup"><span data-stu-id="8bb76-137">In this case, we are removing it by rule ID.</span></span> <span data-ttu-id="8bb76-138">ID pravidla 0 tato akce odebere pouze ze seznamu ACL.</span><span class="sxs-lookup"><span data-stu-id="8bb76-138">This will only remove the rule ID 0 from the ACL.</span></span> <span data-ttu-id="8bb76-139">Objekt seznamu ACL nejsou odebrány z koncový bod virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="8bb76-139">It does not remove the ACL object from the virtual machine endpoint.</span></span>
   
        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Get-AzureAclConfig –EndpointName "web" `
        | Set-AzureAclConfig –RemoveRule –ID 0 –ACL $acl1
2. <span data-ttu-id="8bb76-140">Dále musíte použít objekt sítě ACL pro koncový bod virtuálního počítače a aktualizovat virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="8bb76-140">Next, you must apply the Network ACL object to the virtual machine endpoint and update the virtual machine.</span></span>
   
        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Set-AzureEndpoint –ACL $acl1 –Name "web" `
        | Update-AzureVM

### <a name="remove-a-network-acl-from-a-virtual-machine-endpoint"></a><span data-ttu-id="8bb76-141">Odeberte seznam ACL sítě z koncový bod virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="8bb76-141">Remove a Network ACL from a virtual machine endpoint</span></span>
<span data-ttu-id="8bb76-142">V některých případech můžete chtít odebrat objekt seznamu ACL sítě z koncový bod virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="8bb76-142">In certain scenarios, you might want to remove a Network ACL object from a virtual machine endpoint.</span></span> <span data-ttu-id="8bb76-143">K tomu, otevřete PowerShell ISE Azure.</span><span class="sxs-lookup"><span data-stu-id="8bb76-143">To do that, open an Azure PowerShell ISE.</span></span> <span data-ttu-id="8bb76-144">Zkopírujte a vložte níže, konfigurace skriptu s vlastními hodnotami skript a spusťte skript.</span><span class="sxs-lookup"><span data-stu-id="8bb76-144">Copy and paste the script below, configuring the script with your own values, and then run the script.</span></span>

        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Remove-AzureAclConfig –EndpointName "web" `
        | Update-AzureVM

## <a name="next-steps"></a><span data-ttu-id="8bb76-145">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8bb76-145">Next steps</span></span>
[<span data-ttu-id="8bb76-146">Co je seznamu pro řízení přístupu sítě (ACL)?</span><span class="sxs-lookup"><span data-stu-id="8bb76-146">What is a Network Access Control List (ACL)?</span></span>](virtual-networks-acl.md)

