---
title: "Azure Active Directory Domain Services: Příručka pro správu | Microsoft Docs"
description: "Připojení virtuálního počítače s Windows k spravované doméně pomocí prostředí Azure PowerShell a modelu nasazení classic."
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 9143b843-7327-43c3-baab-6e24a18db25e
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
ms.openlocfilehash: 1bb1546fb616131a1e1868a0d0610c4cad5d73e2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="join-a-windows-server-virtual-machine-to-a-managed-domain-using-powershell"></a><span data-ttu-id="f8302-103">Připojení virtuálního počítače Windows serveru ke spravované doméně pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="f8302-103">Join a Windows Server virtual machine to a managed domain using PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="f8302-104">Portál Azure classic – Windows</span><span class="sxs-lookup"><span data-stu-id="f8302-104">Azure classic portal - Windows</span></span>](active-directory-ds-admin-guide-join-windows-vm.md)
> * [<span data-ttu-id="f8302-105">PowerShell – Windows</span><span class="sxs-lookup"><span data-stu-id="f8302-105">PowerShell - Windows</span></span>](active-directory-ds-admin-guide-join-windows-vm-classic-powershell.md)
>
>

<br>

> [!IMPORTANT]
> <span data-ttu-id="f8302-106">Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="f8302-106">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="f8302-107">Tento článek se věnuje použití klasického modelu nasazení.</span><span class="sxs-lookup"><span data-stu-id="f8302-107">This article covers using the classic deployment model.</span></span> <span data-ttu-id="f8302-108">Azure AD Domain Services modelu Resource Manager aktuálně nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="f8302-108">Azure AD Domain Services does not currently support the Resource Manager model.</span></span>
>
>

<span data-ttu-id="f8302-109">Tyto kroky ukazují, jak přizpůsobit sadu příkazů prostředí Azure PowerShell, které vytvářejí a předem nakonfigurujte virtuální počítač systému Windows Azure pomocí stavebním blokem přístup.</span><span class="sxs-lookup"><span data-stu-id="f8302-109">These steps show you how to customize a set of Azure PowerShell commands that create and preconfigure a Windows-based Azure virtual machine by using a building block approach.</span></span> <span data-ttu-id="f8302-110">Tyto kroky můžete vytvořit virtuální počítač systému Windows Azure a připojte ho k spravované doméně služby Azure AD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="f8302-110">These steps help you build a Windows-based Azure virtual machine and join it to an Azure AD Domain Services managed domain.</span></span>

<span data-ttu-id="f8302-111">Tyto kroky použijte přístup doplňovat-v-the-prázdné hodnoty pro vytvoření sady příkazů prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="f8302-111">These steps follow a fill-in-the-blanks approach for creating Azure PowerShell command sets.</span></span> <span data-ttu-id="f8302-112">Tento přístup může být užitečné, pokud začínáte používat prostředí PowerShell nebo pokud budete chtít vědět, jaké hodnoty Pokud chcete zadat pro úspěšné konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="f8302-112">This approach can be useful if you are new to PowerShell or you want to know what values to specify for successful configuration.</span></span> <span data-ttu-id="f8302-113">Pokročilí uživatelé prostředí PowerShell můžete provést příkazy a nahraďte vlastní hodnoty pro proměnné (řádky začínající "$").</span><span class="sxs-lookup"><span data-stu-id="f8302-113">Advanced PowerShell users can take the commands and substitute their own values for the variables (the lines beginning with "$").</span></span>

<span data-ttu-id="f8302-114">Pokud jste tak ještě neučinili, postupujte podle pokynů v [postup instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) nainstalujte prostředí Azure PowerShell v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="f8302-114">If you haven't done so already, use the instructions in [How to install and configure Azure PowerShell](/powershell/azure/overview) to install Azure PowerShell on your local computer.</span></span> <span data-ttu-id="f8302-115">Potom otevřete příkazový řádek prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="f8302-115">Then, open a Windows PowerShell command prompt.</span></span>

## <a name="step-1-add-your-account"></a><span data-ttu-id="f8302-116">Krok 1: Přidání účtu</span><span class="sxs-lookup"><span data-stu-id="f8302-116">Step 1: Add your account</span></span>
1. <span data-ttu-id="f8302-117">Zadejte v příkazovém prostředí PowerShell, **Add-AzureAccount** a klikněte na tlačítko **Enter**.</span><span class="sxs-lookup"><span data-stu-id="f8302-117">At the PowerShell prompt, type **Add-AzureAccount** and click **Enter**.</span></span>
2. <span data-ttu-id="f8302-118">Zadejte e-mailová adresa spojená s předplatným Azure a klikněte na tlačítko **pokračovat**.</span><span class="sxs-lookup"><span data-stu-id="f8302-118">Type in the email address associated with your Azure subscription and click **Continue**.</span></span>
3. <span data-ttu-id="f8302-119">Zadejte heslo k vašemu účtu.</span><span class="sxs-lookup"><span data-stu-id="f8302-119">Type in the password for your account.</span></span>
4. <span data-ttu-id="f8302-120">Klikněte na tlačítko **přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="f8302-120">Click **Sign in**.</span></span>

## <a name="step-2-set-your-subscription-and-storage-account"></a><span data-ttu-id="f8302-121">Krok 2: Nastavení vaše předplatné a účet úložiště</span><span class="sxs-lookup"><span data-stu-id="f8302-121">Step 2: Set your subscription and storage account</span></span>
<span data-ttu-id="f8302-122">Spuštěním těchto příkazů na příkazovém řádku prostředí Windows PowerShell nastavte předplatné a účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="f8302-122">Set your Azure subscription and storage account by running these commands at the Windows PowerShell command prompt.</span></span> <span data-ttu-id="f8302-123">Nahraďte všechna data v uvozovkách, včetně < a > znaky s správné názvy.</span><span class="sxs-lookup"><span data-stu-id="f8302-123">Replace everything within the quotes, including the < and > characters, with the correct names.</span></span>

    $subscr="<subscription name>"
    $staccount="<storage account name>"
    Select-AzureSubscription -SubscriptionName $subscr –Current
    Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

<span data-ttu-id="f8302-124">Název správné předplatné můžete získat z vlastnosti Název_předplatného výstup **Get-AzureSubscription** příkaz.</span><span class="sxs-lookup"><span data-stu-id="f8302-124">You can get the correct subscription name from the SubscriptionName property of the output of the **Get-AzureSubscription** command.</span></span> <span data-ttu-id="f8302-125">Název účtu úložiště správné můžete získat z vlastnosti Label výstup **Get-AzureStorageAccount** příkaz po spuštění **Select-AzureSubscription** příkaz.</span><span class="sxs-lookup"><span data-stu-id="f8302-125">You can get the correct storage account name from the Label property of the output of the **Get-AzureStorageAccount** command after you run the **Select-AzureSubscription** command.</span></span>

## <a name="step-3-step-by-step-walkthrough---provision-the-virtual-machine-and-join-it-to-the-managed-domain"></a><span data-ttu-id="f8302-126">Krok 3: Podrobný návod - zřízení virtuálního počítače a připojte ho k spravované doméně</span><span class="sxs-lookup"><span data-stu-id="f8302-126">Step 3: Step-by-step walkthrough - provision the virtual machine and join it to the managed domain</span></span>
<span data-ttu-id="f8302-127">Zde je příslušného příkazu prostředí Azure PowerShell k vytvoření tohoto virtuálního počítače s prázdné řádky mezi každého bloku čitelnější.</span><span class="sxs-lookup"><span data-stu-id="f8302-127">Here is the corresponding Azure PowerShell command set to create this virtual machine, with blank lines between each block for readability.</span></span>

<span data-ttu-id="f8302-128">Zadejte informace o virtuálního počítače s Windows, které se má zřídit.</span><span class="sxs-lookup"><span data-stu-id="f8302-128">Specify information about the Windows virtual machine to be provisioned.</span></span>

    $family="Windows Server 2012 R2 Datacenter"
    $vmname="Contoso100-test"
    $vmsize="ExtraSmall"

<span data-ttu-id="f8302-129">Hodnoty InstanceSize pro D – DS- a G-series virtuálních počítačů naleznete v části [virtuálního počítače a Cloud velikost služeb pro Azure](https://msdn.microsoft.com/library/azure/dn197896.aspx).</span><span class="sxs-lookup"><span data-stu-id="f8302-129">For the InstanceSize values for D-, DS-, or G-series virtual machines, see [Virtual Machine and Cloud Service Sizes for Azure](https://msdn.microsoft.com/library/azure/dn197896.aspx).</span></span>

<span data-ttu-id="f8302-130">Zadejte informace o spravované domény.</span><span class="sxs-lookup"><span data-stu-id="f8302-130">Provide information about the managed domain.</span></span>

    $domaindns="contoso100.com"
    $domacctdomain="contoso100"

<span data-ttu-id="f8302-131">Zadejte název cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="f8302-131">Specify the name of the cloud service.</span></span>

    $svcname="Contoso100-test"

<span data-ttu-id="f8302-132">Zadejte název virtuální sítě, ke které by měl být připojený virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="f8302-132">Specify the name of the virtual network to which the VM should be joined.</span></span> <span data-ttu-id="f8302-133">Ujistěte se, že spravované doméně AAD DS je k dispozici v této virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="f8302-133">Ensure that the AAD-DS managed domain is available in this virtual network.</span></span>

    $vnetname="MyPreviewVnet"

<span data-ttu-id="f8302-134">Vyberte obrázek virtuálního počítače, který má být použito k přidělení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f8302-134">Select the VM image to be used to provision the VM.</span></span>

    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

<span data-ttu-id="f8302-135">Konfigurace virtuálního počítače – název sady virtuálních počítačů, velikost instance & bitové kopie, který se má použít.</span><span class="sxs-lookup"><span data-stu-id="f8302-135">Configure the VM - set VM name, instance size & image to be used.</span></span>

    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

<span data-ttu-id="f8302-136">Získejte přihlašovací údaje místního správce pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="f8302-136">Obtain local administrator credentials for the VM.</span></span> <span data-ttu-id="f8302-137">Zvolte heslo, silné místního správce.</span><span class="sxs-lookup"><span data-stu-id="f8302-137">Choose a strong local administrator password.</span></span>

    $localadmincred=Get-Credential –Message "Type the name and password of the local administrator account."

<span data-ttu-id="f8302-138">Získejte přihlašovací údaje pro účet uživatele patří do skupiny "Správci AAD řadič domény, pro připojení virtuálních počítačů k spravované doméně.</span><span class="sxs-lookup"><span data-stu-id="f8302-138">Obtain credentials for a user account belonging to 'AAD DC Administrators' group to join VM to the managed domain.</span></span> <span data-ttu-id="f8302-139">Nezadávejte název domény – například v našem příkladu, určíme "bob" jako uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="f8302-139">Do not specify the domain name - for instance, in our example, we specify 'bob' as the user name.</span></span>

    $domainadmincred=Get-Credential –Message "Now type the name (DO NOT INCLUDE THE DOMAIN) and password of an account in the AAD DC Administrators group, that has permission to add the machine to the domain."

<span data-ttu-id="f8302-140">Konfigurace virtuálního počítače – zadejte požadované pověření systému & požadavek na připojení k doméně.</span><span class="sxs-lookup"><span data-stu-id="f8302-140">Configure the VM - specify domain join requirement & required credentials.</span></span>

    $vm1 | Add-AzureProvisioningConfig -AdminUsername $localadmincred.Username -Password $localadmincred.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $domainadmincred.Username -DomainPassword $domainadmincred.GetNetworkCredential().Password -JoinDomain $domaindns

<span data-ttu-id="f8302-141">Nastavte podsíť pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="f8302-141">Set a subnet for the VM.</span></span>

    $vm1 | Set-AzureSubnet -SubnetNames "Subnet-1"

<span data-ttu-id="f8302-142">Volitelné: Přejděte na adresu IP domény.</span><span class="sxs-lookup"><span data-stu-id="f8302-142">Optional: Point to the IP address of the domain.</span></span> <span data-ttu-id="f8302-143">Pokud nastavíte IP adresy služby Azure AD Domain Services spravované domény, servery DNS pro virtuální síť, tento krok není povinný.</span><span class="sxs-lookup"><span data-stu-id="f8302-143">If you set the IP addresses of the Azure AD Domain Services managed domain to be the DNS servers for the virtual network, this step is not required.</span></span>

    $dns = New-AzureDns -Name 'contoso100-dc1' -IPAddress '10.0.0.4'

<span data-ttu-id="f8302-144">Nyní zřídit virtuální počítač připojený k doméně systému Windows.</span><span class="sxs-lookup"><span data-stu-id="f8302-144">Now, provision the domain-joined Windows VM.</span></span>

    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname -Location "Central US" -DnsSettings $dns

<br>

## <a name="script-to-provision-a-windows-vm-and-automatically-join-it-to-an-aad-domain-services-managed-domain"></a><span data-ttu-id="f8302-145">Skript, který zřídit virtuální počítač s Windows a automaticky připojí k spravované doméně služby AAD Domain Services</span><span class="sxs-lookup"><span data-stu-id="f8302-145">Script to provision a Windows VM and automatically join it to an AAD Domain Services managed domain</span></span>
<span data-ttu-id="f8302-146">Tuto sadu příkazů prostředí PowerShell vytvoří virtuální počítač pro server obchodní, který:</span><span class="sxs-lookup"><span data-stu-id="f8302-146">This PowerShell command set creates a virtual machine for a line-of-business server that:</span></span>

* <span data-ttu-id="f8302-147">Používá bitovou kopii systému Windows Server 2012 R2 Datacenter.</span><span class="sxs-lookup"><span data-stu-id="f8302-147">Uses the Windows Server 2012 R2 Datacenter image.</span></span>
* <span data-ttu-id="f8302-148">Je velmi malé virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="f8302-148">Is an extra small virtual machine.</span></span>
* <span data-ttu-id="f8302-149">Obsahuje název Contoso100 testu.</span><span class="sxs-lookup"><span data-stu-id="f8302-149">Has the name Contoso100-test.</span></span>
* <span data-ttu-id="f8302-150">Je automaticky připojené do domény k spravované doméně contoso100.</span><span class="sxs-lookup"><span data-stu-id="f8302-150">Is automatically domain joined to the contoso100 managed domain.</span></span>
* <span data-ttu-id="f8302-151">Se přidá do stejné virtuální síti jako spravované domény.</span><span class="sxs-lookup"><span data-stu-id="f8302-151">Is added to the same virtual network as the managed domain.</span></span>

<span data-ttu-id="f8302-152">Zde je úplný ukázkový skript pro vytvoření virtuálního počítače s Windows a automaticky připojí k spravované doméně služby Azure AD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="f8302-152">Here is the full sample script to create the Windows virtual machine and automatically join it to the Azure AD Domain Services managed domain.</span></span>

    $family="Windows Server 2012 R2 Datacenter"
    $vmname="Contoso100-test"
    $vmsize="ExtraSmall"

    $domaindns="contoso100.com"
    $domacctdomain="contoso100"

    $svcname="Contoso100-test"
    $vnetname="MyPreviewVnet"

    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    $localadmincred=Get-Credential –Message "Type the name and password of the local administrator account."

    $domainadmincred=Get-Credential –Message "Now type the name (not including the domain) and password of an account in the AAD DC Administrators group, that has permission to add the machine to the domain."

    $vm1 | Add-AzureProvisioningConfig -AdminUsername $localadmincred.Username -Password $localadmincred.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $domainadmincred.Username -DomainPassword $domainadmincred.GetNetworkCredential().Password -JoinDomain $domaindns

    $vm1 | Set-AzureSubnet -SubnetNames "Subnet-1"

    $dns = New-AzureDns -Name 'contoso100-dc1' -IPAddress '10.0.0.4'

    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname -Location "Central US" -DnsSettings $dns

<br>

## <a name="related-content"></a><span data-ttu-id="f8302-153">Související obsah</span><span class="sxs-lookup"><span data-stu-id="f8302-153">Related Content</span></span>
* [<span data-ttu-id="f8302-154">Azure AD Domain Services – Příručka Začínáme</span><span class="sxs-lookup"><span data-stu-id="f8302-154">Azure AD Domain Services - Getting Started guide</span></span>](active-directory-ds-getting-started.md)
* [<span data-ttu-id="f8302-155">Správa spravované domény služby Azure AD Domain Services</span><span class="sxs-lookup"><span data-stu-id="f8302-155">Administer an Azure AD Domain Services managed domain</span></span>](active-directory-ds-admin-guide-administer-domain.md)
