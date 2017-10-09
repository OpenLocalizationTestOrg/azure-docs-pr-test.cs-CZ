---
title: "seznamy řízení aaaManage přístup koncového bodu Azure | Prostředí PowerShell | Classic | Microsoft Docs"
description: "Zjistěte, jak toomanage seznamy ACL v prostředí PowerShell"
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
ms.openlocfilehash: a7ca241ea108a266085bfb689b742d781e58da1c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-endpoint-access-control-lists-using-powershell-in-hello-classic-deployment-model"></a><span data-ttu-id="62bd9-103">Správa seznamů řízení přístupu koncový bod pomocí prostředí PowerShell v modelu nasazení classic hello</span><span class="sxs-lookup"><span data-stu-id="62bd9-103">Manage endpoint access control lists using PowerShell in hello classic deployment model</span></span>
<span data-ttu-id="62bd9-104">Můžete vytvořit a spravovat sítě seznamy řízení přístupu (ACL) pro koncové body pomocí prostředí Azure PowerShell nebo v hello portálu pro správu.</span><span class="sxs-lookup"><span data-stu-id="62bd9-104">You can create and manage Network Access Control Lists (ACLs) for endpoints by using Azure PowerShell or in hello Management Portal.</span></span> <span data-ttu-id="62bd9-105">V tomto tématu najdete postupy pro seznam ACL běžné úkoly, které můžete dokončit pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="62bd9-105">In this topic, you'll find procedures for ACL common tasks that you can complete using PowerShell.</span></span> <span data-ttu-id="62bd9-106">Hello seznam prostředí Azure PowerShell rutin najdete v části [rutiny pro správu Azure](http://go.microsoft.com/fwlink/?LinkId=317721).</span><span class="sxs-lookup"><span data-stu-id="62bd9-106">For hello list of Azure PowerShell cmdlets see [Azure Management Cmdlets](http://go.microsoft.com/fwlink/?LinkId=317721).</span></span> <span data-ttu-id="62bd9-107">Další informace o seznamy řízení přístupu najdete v tématu [co je seznamu pro řízení přístupu sítě (ACL)?](virtual-networks-acl.md).</span><span class="sxs-lookup"><span data-stu-id="62bd9-107">For more information about ACLs, see [What is a Network Access Control List (ACL)?](virtual-networks-acl.md).</span></span> <span data-ttu-id="62bd9-108">Pokud toomanage chcete pomocí portálu pro správu hello vaše seznamy ACL, najdete v části [jak tooSet až koncové body tooa virtuální počítač](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="62bd9-108">If you want toomanage your ACLs by using hello Management Portal, see [How tooSet Up Endpoints tooa Virtual Machine](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

## <a name="manage-network-acls-by-using-azure-powershell"></a><span data-ttu-id="62bd9-109">Spravovat seznamy ACL sítě pomocí prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="62bd9-109">Manage Network ACLs by using Azure PowerShell</span></span>
<span data-ttu-id="62bd9-110">Můžete použít toocreate rutin prostředí Azure PowerShell, odebrat a nakonfigurovat (set) sítě seznamy řízení přístupu (ACL).</span><span class="sxs-lookup"><span data-stu-id="62bd9-110">You can use Azure PowerShell cmdlets toocreate, remove, and configure (set) Network Access Control Lists (ACLs).</span></span> <span data-ttu-id="62bd9-111">Jsme zahrnuli několik příkladů hello způsoby, kterými můžete nakonfigurovat seznam ACL pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="62bd9-111">We've included a few examples of some of hello ways you can configure an ACL using PowerShell.</span></span>

<span data-ttu-id="62bd9-112">Úplný seznam rutin prostředí PowerShell seznamu ACL hello tooretrieve, můžete použít některý z následujících hello:</span><span class="sxs-lookup"><span data-stu-id="62bd9-112">tooretrieve a complete list of hello ACL PowerShell cmdlets, you can use either of hello following:</span></span>

    Get-Help *AzureACL*
    Get-Command -Noun AzureACLConfig

### <a name="create-a-network-acl-with-rules-that-permit-access-from-a-remote-subnet"></a><span data-ttu-id="62bd9-113">Vytvoření seznamu ACL sítě pomocí pravidel, které umožňují přístup ze vzdálené podsíti</span><span class="sxs-lookup"><span data-stu-id="62bd9-113">Create a Network ACL with rules that permit access from a remote subnet</span></span>
<span data-ttu-id="62bd9-114">Hello následující příklad ukazuje způsob toocreate nového seznamu ACL, který obsahuje pravidla.</span><span class="sxs-lookup"><span data-stu-id="62bd9-114">hello example below illustrates a way toocreate a new ACL that contains rules.</span></span> <span data-ttu-id="62bd9-115">Tento seznam ACL se potom použije tooa koncový bod virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="62bd9-115">This ACL is then applied tooa virtual machine endpoint.</span></span> <span data-ttu-id="62bd9-116">pravidla seznamu ACL Hello v následujícím příkladu hello vám umožní přístup ze vzdálené podsíti.</span><span class="sxs-lookup"><span data-stu-id="62bd9-116">hello ACL rules in hello example below will allow access from a remote subnet.</span></span> <span data-ttu-id="62bd9-117">toocreate nového seznamu ACL sítě s povolení pravidel pro vzdálené podsíti, otevřete PowerShell ISE Azure.</span><span class="sxs-lookup"><span data-stu-id="62bd9-117">toocreate a new Network ACL with permit rules for a remote subnet, open an Azure PowerShell ISE.</span></span> <span data-ttu-id="62bd9-118">Zkopírujte a vložte níže, nakonfigurováním skriptu hello vlastními hodnotami hello skript a spusťte skript hello.</span><span class="sxs-lookup"><span data-stu-id="62bd9-118">Copy and paste hello script below, configuring hello script with your own values, and then run hello script.</span></span>

1. <span data-ttu-id="62bd9-119">Vytvořte nový objekt seznamu ACL sítě hello.</span><span class="sxs-lookup"><span data-stu-id="62bd9-119">Create hello new network ACL object.</span></span>
   
        $acl1 = New-AzureAclConfig
2. <span data-ttu-id="62bd9-120">Nastavte pravidlo, která umožňuje přístup ze vzdálené podsíti.</span><span class="sxs-lookup"><span data-stu-id="62bd9-120">Set a rule that permits access from a remote subnet.</span></span> <span data-ttu-id="62bd9-121">V příkladu hello níže můžete nastavit pravidlo *100* (která má přednost před pravidlo 200 a vyšší) tooallow hello vzdálené podsíti *10.0.0.0/8* přístup toohello koncový bod virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="62bd9-121">In hello example below, you set rule *100* (which has priority over rule 200 and higher) tooallow hello remote subnet *10.0.0.0/8* access toohello virtual machine endpoint.</span></span> <span data-ttu-id="62bd9-122">Nahraďte hodnoty hello vlastní požadavky na konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="62bd9-122">Replace hello values with your own configuration requirements.</span></span> <span data-ttu-id="62bd9-123">Název Hello "Konfigurace služby SharePoint ACL" by měl být nahrazen hello popisný název, který má toocall toto pravidlo.</span><span class="sxs-lookup"><span data-stu-id="62bd9-123">hello name "SharePoint ACL config" should be replaced with hello friendly name that you want toocall this rule.</span></span>
   
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 100 `
            –Action permit –RemoteSubnet "10.0.0.0/8" `
            –Description "SharePoint ACL config"
3. <span data-ttu-id="62bd9-124">Pro další pravidla opakujte hello rutinu a nahraďte hodnoty hello s vlastní požadavky na konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="62bd9-124">For additional rules, repeat hello cmdlet, replacing hello values with your own configuration requirements.</span></span> <span data-ttu-id="62bd9-125">Být, že toochange hello pravidlo číslo pořadí tooreflect hello pořadí, ve kterém chcete toobe hello pravidla použít.</span><span class="sxs-lookup"><span data-stu-id="62bd9-125">Be sure toochange hello rule number Order tooreflect hello order in which you want hello rules toobe applied.</span></span> <span data-ttu-id="62bd9-126">Hello nižší číslo pravidla mají přednost před hello vyšší číslo.</span><span class="sxs-lookup"><span data-stu-id="62bd9-126">hello lower rule number takes precedence over hello higher number.</span></span>
   
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 200 `
            –Action permit –RemoteSubnet "157.0.0.0/8" `
            –Description "web frontend ACL config"
4. <span data-ttu-id="62bd9-127">Potom můžete vytvořit nový koncový bod (Přidat) nebo nastavit hello seznamu ACL pro existující koncový bod (Set).</span><span class="sxs-lookup"><span data-stu-id="62bd9-127">Next, you can either create a new endpoint (Add) or set hello ACL for an existing endpoint (Set).</span></span> <span data-ttu-id="62bd9-128">V tomto příkladu přidáme nový koncový bod virtuálního počítače s názvem "web" a aktualizací hello koncový bod virtuálního počítače s hello nastavení seznamu ACL.</span><span class="sxs-lookup"><span data-stu-id="62bd9-128">In this example, we will add a new virtual machine endpoint called "web" and update hello virtual machine endpoint with hello ACL settings.</span></span>
   
        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Add-AzureEndpoint –Name "web" –Protocol tcp –Localport 80 - PublicPort 80 –ACL $acl1 `
        | Update-AzureVM
5. <span data-ttu-id="62bd9-129">V dalším kroku kombinovat hello rutiny a spusťte skript hello.</span><span class="sxs-lookup"><span data-stu-id="62bd9-129">Next, combine hello cmdlets and run hello script.</span></span> <span data-ttu-id="62bd9-130">V tomto příkladu hello kombinované rutiny bude vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="62bd9-130">For this example, hello combined cmdlets would look like this:</span></span>
   
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

### <a name="remove-a-network-acl-rule-that-permits-access-from-a-remote-subnet"></a><span data-ttu-id="62bd9-131">Odebrat pravidlo seznamu ACL sítě, která umožňuje přístup ze vzdálené podsíti</span><span class="sxs-lookup"><span data-stu-id="62bd9-131">Remove a Network ACL rule that permits access from a remote subnet</span></span>
<span data-ttu-id="62bd9-132">Hello následující příklad ukazuje způsob tooremove pravidlo seznamu ACL sítě.</span><span class="sxs-lookup"><span data-stu-id="62bd9-132">hello example below illustrates a way tooremove a network ACL rule.</span></span>  <span data-ttu-id="62bd9-133">tooremove pravidlo seznamu ACL sítě s povolení pravidel pro vzdálené podsíti, otevřete PowerShell ISE Azure.</span><span class="sxs-lookup"><span data-stu-id="62bd9-133">tooremove a Network ACL rule with permit rules for a remote subnet, open an Azure PowerShell ISE.</span></span> <span data-ttu-id="62bd9-134">Zkopírujte a vložte níže, nakonfigurováním skriptu hello vlastními hodnotami hello skript a spusťte skript hello.</span><span class="sxs-lookup"><span data-stu-id="62bd9-134">Copy and paste hello script below, configuring hello script with your own values, and then run hello script.</span></span>

1. <span data-ttu-id="62bd9-135">Prvním krokem je objekt sítě ACL hello tooget pro koncový bod virtuálního počítače hello.</span><span class="sxs-lookup"><span data-stu-id="62bd9-135">First step is tooget hello Network ACL object for hello virtual machine endpoint.</span></span> <span data-ttu-id="62bd9-136">Budete pak odeberte pravidlo seznamu ACL hello.</span><span class="sxs-lookup"><span data-stu-id="62bd9-136">You'll then remove hello ACL rule.</span></span> <span data-ttu-id="62bd9-137">V takovém případě jsme se odebrat ji podle ID pravidla.</span><span class="sxs-lookup"><span data-stu-id="62bd9-137">In this case, we are removing it by rule ID.</span></span> <span data-ttu-id="62bd9-138">ID pravidla hello 0 tato akce odebere pouze z hello seznamu ACL.</span><span class="sxs-lookup"><span data-stu-id="62bd9-138">This will only remove hello rule ID 0 from hello ACL.</span></span> <span data-ttu-id="62bd9-139">Objekt seznamu ACL hello nejsou odebrány z hello koncový bod virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="62bd9-139">It does not remove hello ACL object from hello virtual machine endpoint.</span></span>
   
        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Get-AzureAclConfig –EndpointName "web" `
        | Set-AzureAclConfig –RemoveRule –ID 0 –ACL $acl1
2. <span data-ttu-id="62bd9-140">Dále musíte použít hello seznamu ACL sítě objekt toohello koncový bod virtuálního počítače a aktualizovat hello virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="62bd9-140">Next, you must apply hello Network ACL object toohello virtual machine endpoint and update hello virtual machine.</span></span>
   
        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Set-AzureEndpoint –ACL $acl1 –Name "web" `
        | Update-AzureVM

### <a name="remove-a-network-acl-from-a-virtual-machine-endpoint"></a><span data-ttu-id="62bd9-141">Odeberte seznam ACL sítě z koncový bod virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="62bd9-141">Remove a Network ACL from a virtual machine endpoint</span></span>
<span data-ttu-id="62bd9-142">V některých případech můžete chtít tooremove objekt seznamu ACL sítě od koncový bod virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="62bd9-142">In certain scenarios, you might want tooremove a Network ACL object from a virtual machine endpoint.</span></span> <span data-ttu-id="62bd9-143">toodo, otevřete PowerShell ISE Azure.</span><span class="sxs-lookup"><span data-stu-id="62bd9-143">toodo that, open an Azure PowerShell ISE.</span></span> <span data-ttu-id="62bd9-144">Zkopírujte a vložte níže, nakonfigurováním skriptu hello vlastními hodnotami hello skript a spusťte skript hello.</span><span class="sxs-lookup"><span data-stu-id="62bd9-144">Copy and paste hello script below, configuring hello script with your own values, and then run hello script.</span></span>

        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Remove-AzureAclConfig –EndpointName "web" `
        | Update-AzureVM

## <a name="next-steps"></a><span data-ttu-id="62bd9-145">Další kroky</span><span class="sxs-lookup"><span data-stu-id="62bd9-145">Next steps</span></span>
[<span data-ttu-id="62bd9-146">Co je seznamu pro řízení přístupu sítě (ACL)?</span><span class="sxs-lookup"><span data-stu-id="62bd9-146">What is a Network Access Control List (ACL)?</span></span>](virtual-networks-acl.md)

