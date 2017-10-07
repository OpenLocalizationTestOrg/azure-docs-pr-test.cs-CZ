---
title: "Azure Active Directory Domain Services: Příručka pro správu | Microsoft Docs"
description: "Připojení k doméně systému Windows virtuálního počítače tooa spravované pomocí prostředí Azure PowerShell a modelu nasazení classic hello."
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
ms.openlocfilehash: 21bc5930d84c5368a120f9d81f09320e2a0168fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="join-a-windows-server-virtual-machine-tooa-managed-domain-using-powershell"></a><span data-ttu-id="39eab-103">Připojení k systému Windows Server virtuálního počítače tooa spravované doméně pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="39eab-103">Join a Windows Server virtual machine tooa managed domain using PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="39eab-104">Portál Azure classic – Windows</span><span class="sxs-lookup"><span data-stu-id="39eab-104">Azure classic portal - Windows</span></span>](active-directory-ds-admin-guide-join-windows-vm.md)
> * [<span data-ttu-id="39eab-105">PowerShell – Windows</span><span class="sxs-lookup"><span data-stu-id="39eab-105">PowerShell - Windows</span></span>](active-directory-ds-admin-guide-join-windows-vm-classic-powershell.md)
>
>

<br>

> [!IMPORTANT]
> <span data-ttu-id="39eab-106">Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="39eab-106">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="39eab-107">Tento článek se zabývá pomocí modelu nasazení classic hello.</span><span class="sxs-lookup"><span data-stu-id="39eab-107">This article covers using hello classic deployment model.</span></span> <span data-ttu-id="39eab-108">Azure AD Domain Services hello modelu Resource Manager aktuálně nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="39eab-108">Azure AD Domain Services does not currently support hello Resource Manager model.</span></span>
>
>

<span data-ttu-id="39eab-109">Tyto kroky vám ukážou, jak toocustomize sadu Azure PowerShell příkazy, vytvoření a nastavení systému Windows Azure virtuálního počítače pomocí stavebním blokem přístup.</span><span class="sxs-lookup"><span data-stu-id="39eab-109">These steps show you how toocustomize a set of Azure PowerShell commands that create and preconfigure a Windows-based Azure virtual machine by using a building block approach.</span></span> <span data-ttu-id="39eab-110">Tyto kroky můžete vytvořit virtuální počítač systému Windows Azure a připojení k spravované doméně tooan Azure AD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="39eab-110">These steps help you build a Windows-based Azure virtual machine and join it tooan Azure AD Domain Services managed domain.</span></span>

<span data-ttu-id="39eab-111">Tyto kroky použijte přístup doplňovat-v-the-prázdné hodnoty pro vytvoření sady příkazů prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="39eab-111">These steps follow a fill-in-the-blanks approach for creating Azure PowerShell command sets.</span></span> <span data-ttu-id="39eab-112">Tento přístup může být užitečné, pokud jsou nové tooPowerShell nebo chcete co toospecify hodnoty pro tooknow pro úspěšné konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="39eab-112">This approach can be useful if you are new tooPowerShell or you want tooknow what values toospecify for successful configuration.</span></span> <span data-ttu-id="39eab-113">Pokročilí uživatelé prostředí PowerShell můžete pořídit hello příkazy a nahraďte vlastní hodnoty pro proměnné hello (hello řádky začínající "$").</span><span class="sxs-lookup"><span data-stu-id="39eab-113">Advanced PowerShell users can take hello commands and substitute their own values for hello variables (hello lines beginning with "$").</span></span>

<span data-ttu-id="39eab-114">Pokud jste tak ještě neučinili, použijte pokyny hello v [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) tooinstall prostředí Azure PowerShell v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="39eab-114">If you haven't done so already, use hello instructions in [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) tooinstall Azure PowerShell on your local computer.</span></span> <span data-ttu-id="39eab-115">Potom otevřete příkazový řádek prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="39eab-115">Then, open a Windows PowerShell command prompt.</span></span>

## <a name="step-1-add-your-account"></a><span data-ttu-id="39eab-116">Krok 1: Přidání účtu</span><span class="sxs-lookup"><span data-stu-id="39eab-116">Step 1: Add your account</span></span>
1. <span data-ttu-id="39eab-117">Zadejte v příkazovém prostředí PowerShell hello **Add-AzureAccount** a klikněte na tlačítko **Enter**.</span><span class="sxs-lookup"><span data-stu-id="39eab-117">At hello PowerShell prompt, type **Add-AzureAccount** and click **Enter**.</span></span>
2. <span data-ttu-id="39eab-118">Zadejte e-mailovou adresu hello spojené s předplatným Azure a klikněte na tlačítko **pokračovat**.</span><span class="sxs-lookup"><span data-stu-id="39eab-118">Type in hello email address associated with your Azure subscription and click **Continue**.</span></span>
3. <span data-ttu-id="39eab-119">Zadejte heslo hello k vašemu účtu.</span><span class="sxs-lookup"><span data-stu-id="39eab-119">Type in hello password for your account.</span></span>
4. <span data-ttu-id="39eab-120">Klikněte na tlačítko **přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="39eab-120">Click **Sign in**.</span></span>

## <a name="step-2-set-your-subscription-and-storage-account"></a><span data-ttu-id="39eab-121">Krok 2: Nastavení vaše předplatné a účet úložiště</span><span class="sxs-lookup"><span data-stu-id="39eab-121">Step 2: Set your subscription and storage account</span></span>
<span data-ttu-id="39eab-122">Spuštěním těchto příkazů na příkazovém řádku prostředí Windows PowerShell hello nastavte předplatné a účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="39eab-122">Set your Azure subscription and storage account by running these commands at hello Windows PowerShell command prompt.</span></span> <span data-ttu-id="39eab-123">Nahraďte vše v rámci hello uvozovky, včetně hello < a > znaky, správné názvy hello.</span><span class="sxs-lookup"><span data-stu-id="39eab-123">Replace everything within hello quotes, including hello < and > characters, with hello correct names.</span></span>

    $subscr="<subscription name>"
    $staccount="<storage account name>"
    Select-AzureSubscription -SubscriptionName $subscr –Current
    Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

<span data-ttu-id="39eab-124">Můžete získat název správné předplatné hello hello Název_předplatného vlastnost hello výstup hello **Get-AzureSubscription** příkaz.</span><span class="sxs-lookup"><span data-stu-id="39eab-124">You can get hello correct subscription name from hello SubscriptionName property of hello output of hello **Get-AzureSubscription** command.</span></span> <span data-ttu-id="39eab-125">Název účtu hello správné úložiště můžete získat z hello vlastnosti Label hello výstup hello **Get-AzureStorageAccount** příkaz po spuštění hello **Select-AzureSubscription** příkaz.</span><span class="sxs-lookup"><span data-stu-id="39eab-125">You can get hello correct storage account name from hello Label property of hello output of hello **Get-AzureStorageAccount** command after you run hello **Select-AzureSubscription** command.</span></span>

## <a name="step-3-step-by-step-walkthrough---provision-hello-virtual-machine-and-join-it-toohello-managed-domain"></a><span data-ttu-id="39eab-126">Krok 3: Podrobný návod - zřízení hello virtuálního počítače a připojení k spravované doméně toohello</span><span class="sxs-lookup"><span data-stu-id="39eab-126">Step 3: Step-by-step walkthrough - provision hello virtual machine and join it toohello managed domain</span></span>
<span data-ttu-id="39eab-127">Zde je hello odpovídající prostředí Azure PowerShell příkaz set toocreate tento virtuální počítač s prázdné řádky mezi každého bloku čitelnější.</span><span class="sxs-lookup"><span data-stu-id="39eab-127">Here is hello corresponding Azure PowerShell command set toocreate this virtual machine, with blank lines between each block for readability.</span></span>

<span data-ttu-id="39eab-128">Zadejte informace o toobe virtuálního počítače Windows hello zřízený.</span><span class="sxs-lookup"><span data-stu-id="39eab-128">Specify information about hello Windows virtual machine toobe provisioned.</span></span>

    $family="Windows Server 2012 R2 Datacenter"
    $vmname="Contoso100-test"
    $vmsize="ExtraSmall"

<span data-ttu-id="39eab-129">Hello InstanceSize hodnoty pro D – DS- a G-series virtuálních počítačů naleznete v části [virtuálního počítače a Cloud velikost služeb pro Azure](https://msdn.microsoft.com/library/azure/dn197896.aspx).</span><span class="sxs-lookup"><span data-stu-id="39eab-129">For hello InstanceSize values for D-, DS-, or G-series virtual machines, see [Virtual Machine and Cloud Service Sizes for Azure](https://msdn.microsoft.com/library/azure/dn197896.aspx).</span></span>

<span data-ttu-id="39eab-130">Zadejte informace o hello spravované domény.</span><span class="sxs-lookup"><span data-stu-id="39eab-130">Provide information about hello managed domain.</span></span>

    $domaindns="contoso100.com"
    $domacctdomain="contoso100"

<span data-ttu-id="39eab-131">Zadejte název hello hello cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="39eab-131">Specify hello name of hello cloud service.</span></span>

    $svcname="Contoso100-test"

<span data-ttu-id="39eab-132">Zadejte název hello hello virtuální sítě toowhich hello, které by měl být připojen virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="39eab-132">Specify hello name of hello virtual network toowhich hello VM should be joined.</span></span> <span data-ttu-id="39eab-133">Ujistěte se, AAD-DS spravované domény hello je k dispozici v této virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="39eab-133">Ensure that hello AAD-DS managed domain is available in this virtual network.</span></span>

    $vnetname="MyPreviewVnet"

<span data-ttu-id="39eab-134">Toobe bitové kopie virtuálního počítače vyberte hello používá tooprovision hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="39eab-134">Select hello VM image toobe used tooprovision hello VM.</span></span>

    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

<span data-ttu-id="39eab-135">Konfigurace virtuálního počítače – název sady virtuálních počítačů, hello instance velikost & bitové kopie toobe použít.</span><span class="sxs-lookup"><span data-stu-id="39eab-135">Configure hello VM - set VM name, instance size & image toobe used.</span></span>

    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

<span data-ttu-id="39eab-136">Získejte přihlašovací údaje místního správce pro hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="39eab-136">Obtain local administrator credentials for hello VM.</span></span> <span data-ttu-id="39eab-137">Zvolte heslo, silné místního správce.</span><span class="sxs-lookup"><span data-stu-id="39eab-137">Choose a strong local administrator password.</span></span>

    $localadmincred=Get-Credential –Message "Type hello name and password of hello local administrator account."

<span data-ttu-id="39eab-138">Získejte přihlašovací údaje pro účet uživatele patřící too'AAD řadič domény správci skupiny toojoin virtuálních počítačů toohello spravované domény.</span><span class="sxs-lookup"><span data-stu-id="39eab-138">Obtain credentials for a user account belonging too'AAD DC Administrators' group toojoin VM toohello managed domain.</span></span> <span data-ttu-id="39eab-139">Nezadávejte název domény hello – pro instance v našem příkladu určíme "bob" hello uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="39eab-139">Do not specify hello domain name - for instance, in our example, we specify 'bob' as hello user name.</span></span>

    $domainadmincred=Get-Credential –Message "Now type hello name (DO NOT INCLUDE hello DOMAIN) and password of an account in hello AAD DC Administrators group, that has permission tooadd hello machine toohello domain."

<span data-ttu-id="39eab-140">Konfigurace hello virtuálního počítače – zadejte požadované pověření systému & požadavek na připojení k doméně.</span><span class="sxs-lookup"><span data-stu-id="39eab-140">Configure hello VM - specify domain join requirement & required credentials.</span></span>

    $vm1 | Add-AzureProvisioningConfig -AdminUsername $localadmincred.Username -Password $localadmincred.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $domainadmincred.Username -DomainPassword $domainadmincred.GetNetworkCredential().Password -JoinDomain $domaindns

<span data-ttu-id="39eab-141">Nastavte pro podsíť hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="39eab-141">Set a subnet for hello VM.</span></span>

    $vm1 | Set-AzureSubnet -SubnetNames "Subnet-1"

<span data-ttu-id="39eab-142">Volitelné: Bod toohello IP adresa hello domény.</span><span class="sxs-lookup"><span data-stu-id="39eab-142">Optional: Point toohello IP address of hello domain.</span></span> <span data-ttu-id="39eab-143">Pokud nastavíte hello toobe spravované doméně služby Azure AD Domain Services hello hello IP adresy serverů DNS pro virtuální síť hello, tento krok není povinný.</span><span class="sxs-lookup"><span data-stu-id="39eab-143">If you set hello IP addresses of hello Azure AD Domain Services managed domain toobe hello DNS servers for hello virtual network, this step is not required.</span></span>

    $dns = New-AzureDns -Name 'contoso100-dc1' -IPAddress '10.0.0.4'

<span data-ttu-id="39eab-144">Nyní hello zřídit připojené k doméně virtuálního počítače s Windows.</span><span class="sxs-lookup"><span data-stu-id="39eab-144">Now, provision hello domain-joined Windows VM.</span></span>

    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname -Location "Central US" -DnsSettings $dns

<br>

## <a name="script-tooprovision-a-windows-vm-and-automatically-join-it-tooan-aad-domain-services-managed-domain"></a><span data-ttu-id="39eab-145">Skript tooprovision virtuální počítač s Windows a automaticky se připojí tooan služby AAD Domain Services spravované doméně</span><span class="sxs-lookup"><span data-stu-id="39eab-145">Script tooprovision a Windows VM and automatically join it tooan AAD Domain Services managed domain</span></span>
<span data-ttu-id="39eab-146">Tuto sadu příkazů prostředí PowerShell vytvoří virtuální počítač pro server obchodní, který:</span><span class="sxs-lookup"><span data-stu-id="39eab-146">This PowerShell command set creates a virtual machine for a line-of-business server that:</span></span>

* <span data-ttu-id="39eab-147">Využívá image systému Windows Server 2012 R2 Datacenter hello.</span><span class="sxs-lookup"><span data-stu-id="39eab-147">Uses hello Windows Server 2012 R2 Datacenter image.</span></span>
* <span data-ttu-id="39eab-148">Je velmi malé virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="39eab-148">Is an extra small virtual machine.</span></span>
* <span data-ttu-id="39eab-149">Obsahuje název hello Contoso100 testu.</span><span class="sxs-lookup"><span data-stu-id="39eab-149">Has hello name Contoso100-test.</span></span>
* <span data-ttu-id="39eab-150">Je automaticky domény připojený k toohello contoso100 spravované domény.</span><span class="sxs-lookup"><span data-stu-id="39eab-150">Is automatically domain joined toohello contoso100 managed domain.</span></span>
* <span data-ttu-id="39eab-151">Přidání toohello stejné virtuální síti jako hello spravované domény.</span><span class="sxs-lookup"><span data-stu-id="39eab-151">Is added toohello same virtual network as hello managed domain.</span></span>

<span data-ttu-id="39eab-152">Tady je hello úplnou ukázku najdete skript toocreate hello Windows virtuálního počítače a automaticky se připojí spravované domény toohello Azure AD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="39eab-152">Here is hello full sample script toocreate hello Windows virtual machine and automatically join it toohello Azure AD Domain Services managed domain.</span></span>

    $family="Windows Server 2012 R2 Datacenter"
    $vmname="Contoso100-test"
    $vmsize="ExtraSmall"

    $domaindns="contoso100.com"
    $domacctdomain="contoso100"

    $svcname="Contoso100-test"
    $vnetname="MyPreviewVnet"

    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    $localadmincred=Get-Credential –Message "Type hello name and password of hello local administrator account."

    $domainadmincred=Get-Credential –Message "Now type hello name (not including hello domain) and password of an account in hello AAD DC Administrators group, that has permission tooadd hello machine toohello domain."

    $vm1 | Add-AzureProvisioningConfig -AdminUsername $localadmincred.Username -Password $localadmincred.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $domainadmincred.Username -DomainPassword $domainadmincred.GetNetworkCredential().Password -JoinDomain $domaindns

    $vm1 | Set-AzureSubnet -SubnetNames "Subnet-1"

    $dns = New-AzureDns -Name 'contoso100-dc1' -IPAddress '10.0.0.4'

    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname -Location "Central US" -DnsSettings $dns

<br>

## <a name="related-content"></a><span data-ttu-id="39eab-153">Související obsah</span><span class="sxs-lookup"><span data-stu-id="39eab-153">Related Content</span></span>
* [<span data-ttu-id="39eab-154">Azure AD Domain Services – Příručka Začínáme</span><span class="sxs-lookup"><span data-stu-id="39eab-154">Azure AD Domain Services - Getting Started guide</span></span>](active-directory-ds-getting-started.md)
* [<span data-ttu-id="39eab-155">Správa spravované domény služby Azure AD Domain Services</span><span class="sxs-lookup"><span data-stu-id="39eab-155">Administer an Azure AD Domain Services managed domain</span></span>](active-directory-ds-admin-guide-administer-domain.md)
