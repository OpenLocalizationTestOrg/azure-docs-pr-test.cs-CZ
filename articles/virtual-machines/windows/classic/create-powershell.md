---
title: "Vytvoření virtuálního počítače Windows pomocí prostředí PowerShell | Microsoft Docs"
description: "Vytvořte virtuální počítače s Windows pomocí Azure PowerShell a modelu nasazení classic."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 42c0d4be-573c-45d1-b9b0-9327537702f7
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: cynthn
ms.openlocfilehash: 75c6cf17ee269ae169d9f2f748d0985ca07e454e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-windows-virtual-machine-with-powershell-and-the-classic-deployment-model"></a><span data-ttu-id="65657-103">Vytvoření virtuálního počítače s Windows pomocí prostředí PowerShell a modelu nasazení classic</span><span class="sxs-lookup"><span data-stu-id="65657-103">Create a Windows virtual machine with PowerShell and the classic deployment model</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="65657-104">Portál Azure – Windows</span><span class="sxs-lookup"><span data-stu-id="65657-104">Azure portal - Windows</span></span>](tutorial.md)
> * [<span data-ttu-id="65657-105">PowerShell – Windows</span><span class="sxs-lookup"><span data-stu-id="65657-105">PowerShell - Windows</span></span>](create-powershell.md)
> 
> 

<br>

> [!IMPORTANT] 
> <span data-ttu-id="65657-106">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="65657-106">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="65657-107">Tento článek se zabývá pomocí modelu nasazení Classic.</span><span class="sxs-lookup"><span data-stu-id="65657-107">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="65657-108">Microsoft doporučuje, aby byl ve většině nových nasazení použit model Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="65657-108">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="65657-109">Zjistěte, jak [provést tento postup pomocí modelu Resource Manageru](../../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="65657-109">Learn how to [perform these steps using the Resource Manager model](../../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="65657-110">Tyto kroky ukazují, jak přizpůsobit sadu příkazů prostředí Azure PowerShell, které vytvářejí a předem nakonfigurujte virtuální počítač systému Windows Azure pomocí stavebním blokem přístup.</span><span class="sxs-lookup"><span data-stu-id="65657-110">These steps show you how to customize a set of Azure PowerShell commands that create and preconfigure a Windows-based Azure virtual machine by using a building block approach.</span></span> <span data-ttu-id="65657-111">Tento proces slouží k rychlému vytváření sadu příkazů pro nový virtuální počítač systému Windows a rozbalte stávajícího nasazení nebo vytvořit více sady příkazů, které rychle vytvářet vlastní vývoj/testování nebo pro prostředí IT.</span><span class="sxs-lookup"><span data-stu-id="65657-111">You can use this process to quickly create a command set for a new Windows-based virtual machine and expand an existing deployment or to create multiple command sets that quickly build out a custom dev/test or IT pro environment.</span></span>

<span data-ttu-id="65657-112">Tyto kroky použijte přístup doplňovat-v-the-prázdné hodnoty pro vytvoření sady příkazů prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="65657-112">These steps follow a fill-in-the-blanks approach for creating Azure PowerShell command sets.</span></span> <span data-ttu-id="65657-113">Tento přístup může být užitečné, pokud začínáte používat prostředí PowerShell nebo jenom chcete vědět, jaké hodnoty chcete zadat pro úspěšné konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="65657-113">This approach can be useful if you are new to PowerShell or you just want to know what values to specify for successful configuration.</span></span> <span data-ttu-id="65657-114">Pokročilí uživatelé prostředí PowerShell můžete provést příkazy a nahraďte vlastní hodnoty pro proměnné (řádky začínající "$").</span><span class="sxs-lookup"><span data-stu-id="65657-114">Advanced PowerShell users can take the commands and substitute their own values for the variables (the lines beginning with "$").</span></span>

<span data-ttu-id="65657-115">Pokud jste tak ještě neučinili, postupujte podle pokynů v [postup instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) nainstalujte prostředí Azure PowerShell v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="65657-115">If you haven't done so already, use the instructions in [How to install and configure Azure PowerShell](/powershell/azure/overview) to install Azure PowerShell on your local computer.</span></span> <span data-ttu-id="65657-116">Potom otevřete příkazový řádek prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="65657-116">Then, open a Windows PowerShell command prompt.</span></span>

## <a name="step-1-add-your-account"></a><span data-ttu-id="65657-117">Krok 1: Přidání účtu</span><span class="sxs-lookup"><span data-stu-id="65657-117">Step 1: Add your account</span></span>
1. <span data-ttu-id="65657-118">Zadejte v příkazovém prostředí PowerShell, **Add-AzureAccount** a klikněte na tlačítko **Enter**.</span><span class="sxs-lookup"><span data-stu-id="65657-118">At the PowerShell prompt, type **Add-AzureAccount** and click **Enter**.</span></span> 
2. <span data-ttu-id="65657-119">Zadejte e-mailová adresa spojená s předplatným Azure a klikněte na tlačítko **pokračovat**.</span><span class="sxs-lookup"><span data-stu-id="65657-119">Type in the email address associated with your Azure subscription and click **Continue**.</span></span> 
3. <span data-ttu-id="65657-120">Zadejte heslo k vašemu účtu.</span><span class="sxs-lookup"><span data-stu-id="65657-120">Type in the password for your account.</span></span> 
4. <span data-ttu-id="65657-121">Klikněte na tlačítko **přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="65657-121">Click **Sign in**.</span></span>

## <a name="step-2-set-your-subscription-and-storage-account"></a><span data-ttu-id="65657-122">Krok 2: Nastavení vaše předplatné a účet úložiště</span><span class="sxs-lookup"><span data-stu-id="65657-122">Step 2: Set your subscription and storage account</span></span>
<span data-ttu-id="65657-123">Spuštěním těchto příkazů na příkazovém řádku prostředí Windows PowerShell nastavte předplatné a účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="65657-123">Set your Azure subscription and storage account by running these commands at the Windows PowerShell command prompt.</span></span> <span data-ttu-id="65657-124">Nahraďte všechna data v uvozovkách, včetně < a > znaky s správné názvy.</span><span class="sxs-lookup"><span data-stu-id="65657-124">Replace everything within the quotes, including the < and > characters, with the correct names.</span></span>

    $subscr="<subscription name>"
    $staccount="<storage account name>"
    Select-AzureSubscription -SubscriptionName $subscr –Current
    Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

<span data-ttu-id="65657-125">Název správné předplatné můžete získat z vlastnosti Název_předplatného výstup **Get-AzureSubscription** příkaz.</span><span class="sxs-lookup"><span data-stu-id="65657-125">You can get the correct subscription name from the SubscriptionName property of the output of the **Get-AzureSubscription** command.</span></span> <span data-ttu-id="65657-126">Název účtu úložiště správné můžete získat z vlastnosti Label výstup **Get-AzureStorageAccount** příkaz po spuštění **Select-AzureSubscription** příkaz.</span><span class="sxs-lookup"><span data-stu-id="65657-126">You can get the correct storage account name from the Label property of the output of the **Get-AzureStorageAccount** command after you run the **Select-AzureSubscription** command.</span></span>

## <a name="step-3-determine-the-imagefamily"></a><span data-ttu-id="65657-127">Krok 3: Určení ImageFamily</span><span class="sxs-lookup"><span data-stu-id="65657-127">Step 3: Determine the ImageFamily</span></span>
<span data-ttu-id="65657-128">Dále musíte určit ImageFamily nebo popisek hodnotu pro tento konkrétní obrázek odpovídající virtuální počítač Azure, kterou chcete vytvořit.</span><span class="sxs-lookup"><span data-stu-id="65657-128">Next, you need to determine the ImageFamily or Label value for the specific image corresponding to the Azure virtual machine you want to create.</span></span> <span data-ttu-id="65657-129">Můžete získat seznam dostupných hodnot ImageFamily pomocí tohoto příkazu.</span><span class="sxs-lookup"><span data-stu-id="65657-129">You can get the list of available ImageFamily values with this command.</span></span>

    Get-AzureVMImage | select ImageFamily -Unique

<span data-ttu-id="65657-130">Tady jsou některé příklady ImageFamily hodnot pro počítače se systémem Windows:</span><span class="sxs-lookup"><span data-stu-id="65657-130">Here are some examples of ImageFamily values for Windows-based computers:</span></span>

* <span data-ttu-id="65657-131">Windows Server 2012 R2 Datacenter</span><span class="sxs-lookup"><span data-stu-id="65657-131">Windows Server 2012 R2 Datacenter</span></span>
* <span data-ttu-id="65657-132">Windows Server 2008 R2 SP1</span><span class="sxs-lookup"><span data-stu-id="65657-132">Windows Server 2008 R2 SP1</span></span>
* <span data-ttu-id="65657-133">Windows Server 2016 Technical Preview 4</span><span class="sxs-lookup"><span data-stu-id="65657-133">Windows Server 2016 Technical Preview 4</span></span>
* <span data-ttu-id="65657-134">SQL Server 2012 SP1 Enterprise na systém Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="65657-134">SQL Server 2012 SP1 Enterprise on Windows Server 2012</span></span>

<span data-ttu-id="65657-135">Pokud zjistíte, bitovou kopii, kterou hledáte, otevřete novou instanci textového editoru zvoleného nebo prostředí PowerShell integrovaném skriptovacím prostředí (ISE).</span><span class="sxs-lookup"><span data-stu-id="65657-135">If you find the image you are looking for, open a fresh instance of the text editor of your choice or the PowerShell Integrated Scripting Environment (ISE).</span></span> <span data-ttu-id="65657-136">Zkopírujte následující do nového textového souboru nebo prostředí PowerShell ISE, nahraďte hodnotu ImageFamily.</span><span class="sxs-lookup"><span data-stu-id="65657-136">Copy the following into the new text file or the PowerShell ISE, substituting the ImageFamily value.</span></span>

    $family="<ImageFamily value>"
    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

<span data-ttu-id="65657-137">V některých případech je název bitové kopie ve vlastnosti popisek namísto hodnoty ImageFamily.</span><span class="sxs-lookup"><span data-stu-id="65657-137">In some cases, the image name is in the Label property instead of the ImageFamily value.</span></span> <span data-ttu-id="65657-138">Pokud nebyl nalezen bitovou kopii, kterou hledáte pomocí vlastnosti ImageFamily, seznam vlastností jejich popisek pomocí tohoto příkazu bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="65657-138">If you didn't find the image that you are looking for using the ImageFamily property, list the images by their Label property with this command.</span></span>

    Get-AzureVMImage | select Label -Unique

<span data-ttu-id="65657-139">Pokud zjistíte správné bitové kopie s Tento příkaz, otevřete novou instanci textového editoru zvoleného nebo prostředí PowerShell ISE.</span><span class="sxs-lookup"><span data-stu-id="65657-139">If you find the right image with this command, open a fresh instance of the text editor of your choice or the PowerShell ISE.</span></span> <span data-ttu-id="65657-140">Zkopírujte následující do nového textového souboru nebo prostředí PowerShell ISE, nahraďte hodnotu popisku.</span><span class="sxs-lookup"><span data-stu-id="65657-140">Copy the following into the new text file or the PowerShell ISE, substituting the Label value.</span></span>

    $label="<Label value>"
    $image = Get-AzureVMImage | where { $_.Label -eq $label } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

## <a name="step-4-build-your-command-set"></a><span data-ttu-id="65657-141">Krok 4: Vytvoření vaší sadu příkazů</span><span class="sxs-lookup"><span data-stu-id="65657-141">Step 4: Build your command set</span></span>
<span data-ttu-id="65657-142">Sestavení zbytek příkazu nastavte kopírování příslušné sady bloky níže do nového textového souboru nebo ISE a pak vyplněním hodnoty proměnné a odebrání < a > znaků.</span><span class="sxs-lookup"><span data-stu-id="65657-142">Build the rest of your command set by copying the appropriate set of blocks below into your new text file or the ISE and then filling in the variable values and removing the < and > characters.</span></span> <span data-ttu-id="65657-143">V tématu dva [příklady](#examples) na konci tohoto článku představu o konečný výsledek.</span><span class="sxs-lookup"><span data-stu-id="65657-143">See the two [examples](#examples) at the end of this article for an idea of the final result.</span></span>

<span data-ttu-id="65657-144">Spuštění příkazu nastavte volbou jedné z těchto dvou příkaz bloky (povinné).</span><span class="sxs-lookup"><span data-stu-id="65657-144">Start your command set by choosing one of these two command blocks (required).</span></span>

<span data-ttu-id="65657-145">Možnost 1: Zadejte název virtuálního počítače a velikost.</span><span class="sxs-lookup"><span data-stu-id="65657-145">Option 1: Specify a virtual machine name and a size.</span></span>

    $vmname="<machine name>"
    $vmsize="<Specify one: Small, Medium, Large, ExtraLarge, A5, A6, A7, A8, A9>"
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

<span data-ttu-id="65657-146">Možnost 2: Zadejte název, velikost a název sady dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="65657-146">Option 2: Specify a name, size, and availability set name.</span></span>

    $vmname="<machine name>"
    $vmsize="<Specify one: Small, Medium, Large, ExtraLarge, A5, A6, A7, A8, A9>"
    $availset="<set name>"
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image -AvailabilitySetName $availset

<span data-ttu-id="65657-147">Hodnoty InstanceSize pro D – DS- a G-series virtuálních počítačů naleznete v části [virtuálního počítače a Cloud velikost služeb pro Azure](https://msdn.microsoft.com/library/azure/dn197896.aspx).</span><span class="sxs-lookup"><span data-stu-id="65657-147">For the InstanceSize values for D-, DS-, or G-series virtual machines, see [Virtual Machine and Cloud Service Sizes for Azure](https://msdn.microsoft.com/library/azure/dn197896.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="65657-148">Pokud máte smlouvu Enterprise Agreement pomocí programu Software Assurance a chcete využít výhod Windows Server [výhody použití hybridní](https://azure.microsoft.com/pricing/hybrid-use-benefit/), přidejte **- LicenseType** parametru  **Nové AzureVMConfig** rutiny, předejte hodnotu **Windows_Server** pro typické případy použití.</span><span class="sxs-lookup"><span data-stu-id="65657-148">If you have an Enterprise Agreement with Software Assurance, and intend to take advantage of the Windows Server [Hybrid Use Benefit](https://azure.microsoft.com/pricing/hybrid-use-benefit/), add the **-LicenseType** parameter to the **New-AzureVMConfig** cmdlet, passing the value **Windows_Server** for the typical use case.</span></span>  <span data-ttu-id="65657-149">Ujistěte se, že používáte bitovou kopii, kterou jste nahráli; standardní bitové kopie z Galerie nelze použít s výhodou použití hybridní.</span><span class="sxs-lookup"><span data-stu-id="65657-149">Be sure you are using an image you have uploaded; you cannot use a standard image from the Gallery with the Hybrid Use Benefit.</span></span>
> 
> 

<span data-ttu-id="65657-150">Volitelně můžete pro samostatné počítače se systémem Windows zadejte účet místního správce a heslo.</span><span class="sxs-lookup"><span data-stu-id="65657-150">Optionally, for a standalone Windows computer, specify the local administrator account and password.</span></span>

    $cred=Get-Credential -Message "Type the name and password of the local administrator account."
    $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.Username -Password $cred.GetNetworkCredential().Password

<span data-ttu-id="65657-151">Zvolte silné heslo.</span><span class="sxs-lookup"><span data-stu-id="65657-151">Choose a strong password.</span></span> <span data-ttu-id="65657-152">Pokud chcete zkontrolovat její síly, najdete v části [kontrolu hesla: pomocí silného hesla](https://www.microsoft.com/security/pc-security/password-checker.aspx).</span><span class="sxs-lookup"><span data-stu-id="65657-152">To check its strength, see [Password Checker: Using Strong Passwords](https://www.microsoft.com/security/pc-security/password-checker.aspx).</span></span>

<span data-ttu-id="65657-153">Volitelně můžete přidat počítače se systémem Windows do existující domény služby Active Directory, zadejte účet místního správce a heslo, domény a název a heslo účtu domény.</span><span class="sxs-lookup"><span data-stu-id="65657-153">Optionally, to add the Windows computer to an existing Active Directory domain, specify the local administrator account and password, the domain, and the name and password of a domain account.</span></span>

    $cred1=Get-Credential –Message "Type the name and password of the local administrator account."
    $cred2=Get-Credential –Message "Now type the name (not including the domain) and password of an account that has permission to add the machine to the domain."
    $domaindns="<FQDN of the domain that the machine is joining>"
    $domacctdomain="<domain of the account that has permission to add the machine to the domain>"
    $vm1 | Add-AzureProvisioningConfig -AdminUsername $cred1.Username -Password $cred1.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $cred2.Username -DomainPassword $cred2.GetNetworkCredential().Password -JoinDomain $domaindns

<span data-ttu-id="65657-154">Další možnosti předběžné konfigurace pro virtuální počítače se systémem Windows, najdete v části Syntaxe **Windows** a **třídy WindowsDomain** parametr nastaví v [přidat AzureProvisioningConfig ](/powershell/module/azure/add-azureprovisioningconfig).</span><span class="sxs-lookup"><span data-stu-id="65657-154">For additional pre-configuration options for Windows-based virtual machines, see the syntax for the **Windows** and **WindowsDomain** parameter sets in [Add-AzureProvisioningConfig](/powershell/module/azure/add-azureprovisioningconfig).</span></span>

<span data-ttu-id="65657-155">Virtuální počítač volitelně přiřadíte konkrétní IP adresu, označuje jako statické vyhrazené IP adresy.</span><span class="sxs-lookup"><span data-stu-id="65657-155">Optionally, assign the virtual machine a specific IP address, known as a static DIP.</span></span>

    $vm1 | Set-AzureStaticVNetIP -IPAddress <IP address>

<span data-ttu-id="65657-156">Můžete ověřit, že je k dispozici s konkrétní IP adresu:</span><span class="sxs-lookup"><span data-stu-id="65657-156">You can verify that a specific IP address is available with:</span></span>

    Test-AzureStaticVNetIP –VNetName <VNet name> –IPAddress <IP address>

<span data-ttu-id="65657-157">Volitelně můžete přiřadíte virtuální počítač do konkrétní podsítě v virtuální sítě Azure.</span><span class="sxs-lookup"><span data-stu-id="65657-157">Optionally, assign the virtual machine to a specific subnet in an Azure virtual network.</span></span>

    $vm1 | Set-AzureSubnet -SubnetNames "<name of the subnet>"

<span data-ttu-id="65657-158">Volitelně můžete přidáte jeden datový disk k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="65657-158">Optionally, add a single data disk to the virtual machine.</span></span>

    $disksize=<size of the disk in GB>
    $disklabel="<the label on the disk>"
    $lun=<Logical Unit Number (LUN) of the disk>
    $hcaching="<Specify one: ReadOnly, ReadWrite, None>"
    $vm1 | Add-AzureDataDisk -CreateNew -DiskSizeInGB $disksize -DiskLabel $disklabel -LUN $lun -HostCaching $hcaching

<span data-ttu-id="65657-159">Pro řadič domény služby Active Directory nastavte $hcaching na "Žádný".</span><span class="sxs-lookup"><span data-stu-id="65657-159">For an Active Directory domain controller, set $hcaching to "None".</span></span>

<span data-ttu-id="65657-160">Volitelně můžete přidáte virtuální počítač do existující skupinu s vyrovnáváním zatížení pro externí přenosy.</span><span class="sxs-lookup"><span data-stu-id="65657-160">Optionally, add the virtual machine to an existing load-balanced set for external traffic.</span></span>

    $protocol="<Specify one: tcp, udp>"
    $localport=<port number of the internal port>
    $pubport=<port number of the external port>
    $endpointname="<name of the endpoint>"
    $lbsetname="<name of the existing load-balanced set>"
    $probeprotocol="<Specify one: tcp, http>"
    $probeport=<TCP or HTTP port number of probe traffic>
    $probepath="<URL path for probe traffic>"
    $vm1 | Add-AzureEndpoint -Name $endpointname -Protocol $protocol -LocalPort $localport -PublicPort $pubport -LBSetName $lbsetname -ProbeProtocol $probeprotocol -ProbePort $probeport -ProbePath $probepath

<span data-ttu-id="65657-161">Nakonec vyberte jednu z těchto bloků požadovaný příkaz pro vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="65657-161">Finally, choose one of these required command blocks for creating the virtual machine.</span></span>

<span data-ttu-id="65657-162">Možnost 1: Vytvoření virtuálního počítače ve stávající cloudovou službu.</span><span class="sxs-lookup"><span data-stu-id="65657-162">Option 1: Create the virtual machine in an existing cloud service.</span></span>

    New-AzureVM –ServiceName "<short name of the cloud service>" -VMs $vm1

<span data-ttu-id="65657-163">Krátký název cloudové služby je název, který se zobrazí v seznamu cloudové služby na portálu Azure nebo v seznamu skupin prostředků na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="65657-163">The short name of the cloud service is the name that appears in the list of Cloud Services in the Azure portal or in the list of Resource Groups in the Azure portal.</span></span>

<span data-ttu-id="65657-164">Možnost 2: Vytvoření virtuálního počítače do existující cloudové služby a virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="65657-164">Option 2: Create the virtual machine in an existing cloud service and virtual network.</span></span>

    $svcname="<short name of the cloud service>"
    $vnetname="<name of the virtual network>"
    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname

## <a name="step-5-run-your-command-set"></a><span data-ttu-id="65657-165">Krok 5: Spuštění vaší sadu příkazů</span><span class="sxs-lookup"><span data-stu-id="65657-165">Step 5: Run your command set</span></span>
<span data-ttu-id="65657-166">Prohlédněte si sadu Azure PowerShell příkaz, který jste vytvořili v textovém editoru nebo prostředí PowerShell ISE, který se skládá z více bloků příkazy z kroku 4.</span><span class="sxs-lookup"><span data-stu-id="65657-166">Review the Azure PowerShell command set you built in your text editor or the PowerShell ISE consisting of multiple blocks of commands from step 4.</span></span> <span data-ttu-id="65657-167">Ujistěte se, zda jste zadali všechny potřebné proměnné a zda mají správné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="65657-167">Ensure that you have specified all the needed variables and that they have the correct values.</span></span> <span data-ttu-id="65657-168">Také se ujistěte, že jste odstranili všechny < a > znaků.</span><span class="sxs-lookup"><span data-stu-id="65657-168">Also make sure that you have removed all the < and > characters.</span></span>

<span data-ttu-id="65657-169">Pokud používáte textový editor, zkopírujte do schránky sadu příkazů nástroje a potom klikněte pravým tlačítkem spusťte příkazový řádek prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="65657-169">If you are using a text editor, copy the command set to the clipboard and then right-click your open Windows PowerShell command prompt.</span></span> <span data-ttu-id="65657-170">To bude vydávat sadu příkazů nástroje jako řadu příkazů prostředí PowerShell a vytvořte virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="65657-170">This will issue the command set as a series of PowerShell commands and create your Azure virtual machine.</span></span> <span data-ttu-id="65657-171">Alternativně spusťte příkaz nastavit v integrovaném Skriptovacím prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="65657-171">Alternately, run the command set in the PowerShell ISE.</span></span>

<span data-ttu-id="65657-172">Pokud vytvoříte tento virtuální počítač znovu nebo podobné jeden, můžete:</span><span class="sxs-lookup"><span data-stu-id="65657-172">If you will be creating this virtual machine again or a similar one, you can:</span></span>

* <span data-ttu-id="65657-173">Uložte tento příkaz nastavit jako soubor skriptu prostředí PowerShell (*.ps1).</span><span class="sxs-lookup"><span data-stu-id="65657-173">Save this command set as a PowerShell script file (*.ps1).</span></span>
* <span data-ttu-id="65657-174">Uložit tento příkaz nastavit jako runbook služby Azure Automation v **účty Automation** části portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="65657-174">Save this command set as an Azure Automation runbook in the **Automation Accounts** section of the Azure portal.</span></span>

## <span data-ttu-id="65657-175"><a id="examples"></a>Příklady</span><span class="sxs-lookup"><span data-stu-id="65657-175"><a id="examples"></a>Examples</span></span>
<span data-ttu-id="65657-176">Tady jsou dva příklady použití výše uvedených kroků k sestavení sady příkazů prostředí Azure PowerShell, vytvořené na základě systému Windows Azure virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="65657-176">Here are two examples of using the steps above to build Azure PowerShell command sets that create Windows-based Azure virtual machines.</span></span>

### <a name="example-1"></a><span data-ttu-id="65657-177">Příklad 1</span><span class="sxs-lookup"><span data-stu-id="65657-177">Example 1</span></span>
<span data-ttu-id="65657-178">Potřebuji prostředí PowerShell sady příkazů vytvoření počáteční virtuálního počítače pro řadič domény služby Active Directory, který:</span><span class="sxs-lookup"><span data-stu-id="65657-178">I need a PowerShell command set to create the initial virtual machine for an Active Directory domain controller that:</span></span>

* <span data-ttu-id="65657-179">Používá bitovou kopii systému Windows Server 2012 R2 Datacenter.</span><span class="sxs-lookup"><span data-stu-id="65657-179">Uses the Windows Server 2012 R2 Datacenter image.</span></span>
* <span data-ttu-id="65657-180">Obsahuje název AZDC1.</span><span class="sxs-lookup"><span data-stu-id="65657-180">Has the name AZDC1.</span></span>
* <span data-ttu-id="65657-181">Je samostatný počítač.</span><span class="sxs-lookup"><span data-stu-id="65657-181">Is a standalone computer.</span></span>
* <span data-ttu-id="65657-182">Má k dispozici další data disk 20 GB.</span><span class="sxs-lookup"><span data-stu-id="65657-182">Has an additional data disk of 20 GB.</span></span>
* <span data-ttu-id="65657-183">Má statickou IP adresu 192.168.244.4.</span><span class="sxs-lookup"><span data-stu-id="65657-183">Has the static IP address 192.168.244.4.</span></span>
* <span data-ttu-id="65657-184">Je v podsíti back-end AZDatacenter virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="65657-184">Is in the BackEnd subnet of the AZDatacenter virtual network.</span></span>
* <span data-ttu-id="65657-185">Je v cloudové službě Azure-Válcovny.</span><span class="sxs-lookup"><span data-stu-id="65657-185">Is in the Azure-TailspinToys cloud service.</span></span>

<span data-ttu-id="65657-186">Zde je příslušného příkazu prostředí Azure PowerShell k vytvoření tohoto virtuálního počítače s prázdné řádky mezi každého bloku čitelnější.</span><span class="sxs-lookup"><span data-stu-id="65657-186">Here is the corresponding Azure PowerShell command set to create this virtual machine, with blank lines between each block for readability.</span></span>

    $family="Windows Server 2012 R2 Datacenter"
    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1
    $vmname="AZDC1"
    $vmsize="Medium"
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    $cred=Get-Credential -Message "Type the name and password of the local administrator account."
    $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.Username -Password $cred.GetNetworkCredential().Password

    $vm1 | Set-AzureSubnet -SubnetNames "BackEnd"

    $vm1 | Set-AzureStaticVNetIP -IPAddress 192.168.244.4

    $disksize=20
    $disklabel="DCData"
    $lun=0
    $hcaching="None"
    $vm1 | Add-AzureDataDisk -CreateNew -DiskSizeInGB $disksize -DiskLabel $disklabel -LUN $lun -HostCaching $hcaching

    $svcname="Azure-TailspinToys"
    $vnetname="AZDatacenter"
    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname

### <a name="example-2"></a><span data-ttu-id="65657-187">Příklad 2</span><span class="sxs-lookup"><span data-stu-id="65657-187">Example 2</span></span>
<span data-ttu-id="65657-188">Potřebuji prostředí PowerShell sady příkazů vytvoření virtuálního počítače pro server obchodní, který:</span><span class="sxs-lookup"><span data-stu-id="65657-188">I need a PowerShell command set to create a virtual machine for a line-of-business server that:</span></span>

* <span data-ttu-id="65657-189">Používá bitovou kopii systému Windows Server 2012 R2 Datacenter.</span><span class="sxs-lookup"><span data-stu-id="65657-189">Uses the Windows Server 2012 R2 Datacenter image.</span></span>
* <span data-ttu-id="65657-190">Obsahuje název LOB1.</span><span class="sxs-lookup"><span data-stu-id="65657-190">Has the name LOB1.</span></span>
* <span data-ttu-id="65657-191">Je členem domény corp.contoso.com.</span><span class="sxs-lookup"><span data-stu-id="65657-191">Is a member of the corp.contoso.com domain.</span></span>
* <span data-ttu-id="65657-192">Má k dispozici další data disk 200 GB.</span><span class="sxs-lookup"><span data-stu-id="65657-192">Has an additional data disk of 200 GB.</span></span>
* <span data-ttu-id="65657-193">Je v podsíti front-endu AZDatacenter virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="65657-193">Is in the FrontEnd subnet of the AZDatacenter virtual network.</span></span>
* <span data-ttu-id="65657-194">Je v cloudové službě Azure-Válcovny.</span><span class="sxs-lookup"><span data-stu-id="65657-194">Is in the Azure-TailspinToys cloud service.</span></span>

<span data-ttu-id="65657-195">Zde je příslušného příkazu prostředí Azure PowerShell k vytvoření tohoto virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="65657-195">Here is the corresponding Azure PowerShell command set to create this virtual machine.</span></span>

    $family="Windows Server 2012 R2 Datacenter"
    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1
    $vmname="LOB1"
    $vmsize="Large"
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    $cred1=Get-Credential –Message "Type the name and password of the local administrator account."
    $cred2=Get-Credential –Message "Now type the name (not including the domain) and password of an account that has permission to add the machine to the domain."
    $domaindns="corp.contoso.com"
    $domacctdomain="CORP"
    $vm1 | Add-AzureProvisioningConfig -AdminUsername $cred1.Username -Password $cred1.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $cred2.Username -DomainPassword $cred2.GetNetworkCredential().Password -JoinDomain $domaindns

    $vm1 | Set-AzureSubnet -SubnetNames "FrontEnd"

    $disksize=200
    $disklabel="LOBData"
    $lun=0
    $hcaching="ReadWrite"
    $vm1 | Add-AzureDataDisk -CreateNew -DiskSizeInGB $disksize -DiskLabel $disklabel -LUN $lun -HostCaching $hcaching

    $svcname="Azure-TailspinToys"
    $vnetname="AZDatacenter"
    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname


## <a name="next-steps"></a><span data-ttu-id="65657-196">Další kroky</span><span class="sxs-lookup"><span data-stu-id="65657-196">Next steps</span></span>
<span data-ttu-id="65657-197">Pokud budete potřebovat disk s operačním systémem, která je větší než 127 GB, můžete [rozbalte jednotku operačního systému](../../virtual-machines-windows-expand-os-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="65657-197">If you need an OS disk that is larger than 127 GB, you can [expand the OS drive](../../virtual-machines-windows-expand-os-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

