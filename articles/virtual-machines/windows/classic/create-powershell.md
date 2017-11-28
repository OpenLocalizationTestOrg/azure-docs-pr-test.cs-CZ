---
title: "aaaCreate virtuální počítač s Windows pomocí prostředí PowerShell | Microsoft Docs"
description: "Vytvořte virtuální počítače s Windows pomocí Azure PowerShell a modelu nasazení classic hello."
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
ms.openlocfilehash: 5339f458c1f08f6e1752a53191f19402fab8ea25
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-windows-virtual-machine-with-powershell-and-hello-classic-deployment-model"></a><span data-ttu-id="cad31-103">Vytvoření virtuálního počítače s Windows pomocí prostředí PowerShell a hello modelu nasazení classic</span><span class="sxs-lookup"><span data-stu-id="cad31-103">Create a Windows virtual machine with PowerShell and hello classic deployment model</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="cad31-104">Portál Azure – Windows</span><span class="sxs-lookup"><span data-stu-id="cad31-104">Azure portal - Windows</span></span>](tutorial.md)
> * [<span data-ttu-id="cad31-105">PowerShell – Windows</span><span class="sxs-lookup"><span data-stu-id="cad31-105">PowerShell - Windows</span></span>](create-powershell.md)
> 
> 

<br>

> [!IMPORTANT] 
> <span data-ttu-id="cad31-106">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="cad31-106">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="cad31-107">Tento článek se zabývá pomocí modelu nasazení Classic hello.</span><span class="sxs-lookup"><span data-stu-id="cad31-107">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="cad31-108">Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="cad31-108">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="cad31-109">Zjistěte, jak příliš[proveďte tyto kroky, pomocí modelu Resource Manager hello](../../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="cad31-109">Learn how too[perform these steps using hello Resource Manager model](../../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="cad31-110">Tyto kroky vám ukážou, jak toocustomize sadu Azure PowerShell příkazy, vytvoření a nastavení systému Windows Azure virtuálního počítače pomocí stavebním blokem přístup.</span><span class="sxs-lookup"><span data-stu-id="cad31-110">These steps show you how toocustomize a set of Azure PowerShell commands that create and preconfigure a Windows-based Azure virtual machine by using a building block approach.</span></span> <span data-ttu-id="cad31-111">Můžete použít tento proces tooquickly vytvořit sadu příkazů pro nový virtuální počítač systému Windows a rozbalte stávajícího nasazení, nebo toocreate více příkaz sad, které rychle vytváří vlastní vývoj/testování nebo pro IT prostředí.</span><span class="sxs-lookup"><span data-stu-id="cad31-111">You can use this process tooquickly create a command set for a new Windows-based virtual machine and expand an existing deployment or toocreate multiple command sets that quickly build out a custom dev/test or IT pro environment.</span></span>

<span data-ttu-id="cad31-112">Tyto kroky použijte přístup doplňovat-v-the-prázdné hodnoty pro vytvoření sady příkazů prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="cad31-112">These steps follow a fill-in-the-blanks approach for creating Azure PowerShell command sets.</span></span> <span data-ttu-id="cad31-113">Tento přístup může být užitečné, pokud jsou nové tooPowerShell nebo jenom chcete tooknow toospecify jaké hodnoty pro úspěšné konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="cad31-113">This approach can be useful if you are new tooPowerShell or you just want tooknow what values toospecify for successful configuration.</span></span> <span data-ttu-id="cad31-114">Pokročilí uživatelé prostředí PowerShell můžete pořídit hello příkazy a nahraďte vlastní hodnoty pro proměnné hello (hello řádky začínající "$").</span><span class="sxs-lookup"><span data-stu-id="cad31-114">Advanced PowerShell users can take hello commands and substitute their own values for hello variables (hello lines beginning with "$").</span></span>

<span data-ttu-id="cad31-115">Pokud jste tak ještě neučinili, použijte pokyny hello v [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) tooinstall prostředí Azure PowerShell v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="cad31-115">If you haven't done so already, use hello instructions in [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) tooinstall Azure PowerShell on your local computer.</span></span> <span data-ttu-id="cad31-116">Potom otevřete příkazový řádek prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="cad31-116">Then, open a Windows PowerShell command prompt.</span></span>

## <a name="step-1-add-your-account"></a><span data-ttu-id="cad31-117">Krok 1: Přidání účtu</span><span class="sxs-lookup"><span data-stu-id="cad31-117">Step 1: Add your account</span></span>
1. <span data-ttu-id="cad31-118">Zadejte v příkazovém prostředí PowerShell hello **Add-AzureAccount** a klikněte na tlačítko **Enter**.</span><span class="sxs-lookup"><span data-stu-id="cad31-118">At hello PowerShell prompt, type **Add-AzureAccount** and click **Enter**.</span></span> 
2. <span data-ttu-id="cad31-119">Zadejte e-mailovou adresu hello spojené s předplatným Azure a klikněte na tlačítko **pokračovat**.</span><span class="sxs-lookup"><span data-stu-id="cad31-119">Type in hello email address associated with your Azure subscription and click **Continue**.</span></span> 
3. <span data-ttu-id="cad31-120">Zadejte heslo hello k vašemu účtu.</span><span class="sxs-lookup"><span data-stu-id="cad31-120">Type in hello password for your account.</span></span> 
4. <span data-ttu-id="cad31-121">Klikněte na tlačítko **přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="cad31-121">Click **Sign in**.</span></span>

## <a name="step-2-set-your-subscription-and-storage-account"></a><span data-ttu-id="cad31-122">Krok 2: Nastavení vaše předplatné a účet úložiště</span><span class="sxs-lookup"><span data-stu-id="cad31-122">Step 2: Set your subscription and storage account</span></span>
<span data-ttu-id="cad31-123">Spuštěním těchto příkazů na příkazovém řádku prostředí Windows PowerShell hello nastavte předplatné a účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="cad31-123">Set your Azure subscription and storage account by running these commands at hello Windows PowerShell command prompt.</span></span> <span data-ttu-id="cad31-124">Nahraďte vše v rámci hello uvozovky, včetně hello < a > znaky, správné názvy hello.</span><span class="sxs-lookup"><span data-stu-id="cad31-124">Replace everything within hello quotes, including hello < and > characters, with hello correct names.</span></span>

    $subscr="<subscription name>"
    $staccount="<storage account name>"
    Select-AzureSubscription -SubscriptionName $subscr –Current
    Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

<span data-ttu-id="cad31-125">Můžete získat název správné předplatné hello hello Název_předplatného vlastnost hello výstup hello **Get-AzureSubscription** příkaz.</span><span class="sxs-lookup"><span data-stu-id="cad31-125">You can get hello correct subscription name from hello SubscriptionName property of hello output of hello **Get-AzureSubscription** command.</span></span> <span data-ttu-id="cad31-126">Název účtu hello správné úložiště můžete získat z hello vlastnosti Label hello výstup hello **Get-AzureStorageAccount** příkaz po spuštění hello **Select-AzureSubscription** příkaz.</span><span class="sxs-lookup"><span data-stu-id="cad31-126">You can get hello correct storage account name from hello Label property of hello output of hello **Get-AzureStorageAccount** command after you run hello **Select-AzureSubscription** command.</span></span>

## <a name="step-3-determine-hello-imagefamily"></a><span data-ttu-id="cad31-127">Krok 3: Určení hello ImageFamily</span><span class="sxs-lookup"><span data-stu-id="cad31-127">Step 3: Determine hello ImageFamily</span></span>
<span data-ttu-id="cad31-128">Dále musíte toodetermine hello ImageFamily nebo hodnota popisek pro odpovídající toohello hello konkrétní image virtuálního počítače Azure chcete toocreate.</span><span class="sxs-lookup"><span data-stu-id="cad31-128">Next, you need toodetermine hello ImageFamily or Label value for hello specific image corresponding toohello Azure virtual machine you want toocreate.</span></span> <span data-ttu-id="cad31-129">Můžete získat hello seznam dostupných hodnot ImageFamily pomocí tohoto příkazu.</span><span class="sxs-lookup"><span data-stu-id="cad31-129">You can get hello list of available ImageFamily values with this command.</span></span>

    Get-AzureVMImage | select ImageFamily -Unique

<span data-ttu-id="cad31-130">Tady jsou některé příklady ImageFamily hodnot pro počítače se systémem Windows:</span><span class="sxs-lookup"><span data-stu-id="cad31-130">Here are some examples of ImageFamily values for Windows-based computers:</span></span>

* <span data-ttu-id="cad31-131">Windows Server 2012 R2 Datacenter</span><span class="sxs-lookup"><span data-stu-id="cad31-131">Windows Server 2012 R2 Datacenter</span></span>
* <span data-ttu-id="cad31-132">Windows Server 2008 R2 SP1</span><span class="sxs-lookup"><span data-stu-id="cad31-132">Windows Server 2008 R2 SP1</span></span>
* <span data-ttu-id="cad31-133">Windows Server 2016 Technical Preview 4</span><span class="sxs-lookup"><span data-stu-id="cad31-133">Windows Server 2016 Technical Preview 4</span></span>
* <span data-ttu-id="cad31-134">SQL Server 2012 SP1 Enterprise na systém Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="cad31-134">SQL Server 2012 SP1 Enterprise on Windows Server 2012</span></span>

<span data-ttu-id="cad31-135">Pokud zjistíte hello bitovou kopii, kterou hledáte, otevřete novou instanci hello textového editoru volba nebo hello prostředí PowerShell integrovaném skriptovacím prostředí (ISE).</span><span class="sxs-lookup"><span data-stu-id="cad31-135">If you find hello image you are looking for, open a fresh instance of hello text editor of your choice or hello PowerShell Integrated Scripting Environment (ISE).</span></span> <span data-ttu-id="cad31-136">Zkopírujte následující hello do hello nový textový soubor nebo hello PowerShell ISE, nahraďte hodnotu ImageFamily hello.</span><span class="sxs-lookup"><span data-stu-id="cad31-136">Copy hello following into hello new text file or hello PowerShell ISE, substituting hello ImageFamily value.</span></span>

    $family="<ImageFamily value>"
    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

<span data-ttu-id="cad31-137">V některých případech název bitové kopie hello se hello vlastnosti Label místo hello ImageFamily hodnotu.</span><span class="sxs-lookup"><span data-stu-id="cad31-137">In some cases, hello image name is in hello Label property instead of hello ImageFamily value.</span></span> <span data-ttu-id="cad31-138">Pokud nebyl nalezen hello bitovou kopii, která hledáte pomocí vlastnosti ImageFamily hello, seznam vlastností jejich popisek pomocí tohoto příkazu hello bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="cad31-138">If you didn't find hello image that you are looking for using hello ImageFamily property, list hello images by their Label property with this command.</span></span>

    Get-AzureVMImage | select Label -Unique

<span data-ttu-id="cad31-139">Pokud zjistíte hello správné bitové kopie pomocí tohoto příkazu, otevřete novou instanci hello textového editoru volba nebo hello PowerShell ISE.</span><span class="sxs-lookup"><span data-stu-id="cad31-139">If you find hello right image with this command, open a fresh instance of hello text editor of your choice or hello PowerShell ISE.</span></span> <span data-ttu-id="cad31-140">Zkopírujte následující hello do hello nový textový soubor nebo hello PowerShell ISE, nahraďte hodnotu popisku hello.</span><span class="sxs-lookup"><span data-stu-id="cad31-140">Copy hello following into hello new text file or hello PowerShell ISE, substituting hello Label value.</span></span>

    $label="<Label value>"
    $image = Get-AzureVMImage | where { $_.Label -eq $label } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

## <a name="step-4-build-your-command-set"></a><span data-ttu-id="cad31-141">Krok 4: Vytvoření vaší sadu příkazů</span><span class="sxs-lookup"><span data-stu-id="cad31-141">Step 4: Build your command set</span></span>
<span data-ttu-id="cad31-142">Sestavení hello zbytek příkazu nastavit tak, že zkopírujete příslušnou sadu bloků níže hello do své nový textový soubor nebo hello ISE a pak zadáním hodnoty proměnné hello a odebírání hello < a > znaků.</span><span class="sxs-lookup"><span data-stu-id="cad31-142">Build hello rest of your command set by copying hello appropriate set of blocks below into your new text file or hello ISE and then filling in hello variable values and removing hello < and > characters.</span></span> <span data-ttu-id="cad31-143">V tématu hello dva [příklady](#examples) na konci hello tohoto článku představu o konečný výsledek hello.</span><span class="sxs-lookup"><span data-stu-id="cad31-143">See hello two [examples](#examples) at hello end of this article for an idea of hello final result.</span></span>

<span data-ttu-id="cad31-144">Spuštění příkazu nastavte volbou jedné z těchto dvou příkaz bloky (povinné).</span><span class="sxs-lookup"><span data-stu-id="cad31-144">Start your command set by choosing one of these two command blocks (required).</span></span>

<span data-ttu-id="cad31-145">Možnost 1: Zadejte název virtuálního počítače a velikost.</span><span class="sxs-lookup"><span data-stu-id="cad31-145">Option 1: Specify a virtual machine name and a size.</span></span>

    $vmname="<machine name>"
    $vmsize="<Specify one: Small, Medium, Large, ExtraLarge, A5, A6, A7, A8, A9>"
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

<span data-ttu-id="cad31-146">Možnost 2: Zadejte název, velikost a název sady dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="cad31-146">Option 2: Specify a name, size, and availability set name.</span></span>

    $vmname="<machine name>"
    $vmsize="<Specify one: Small, Medium, Large, ExtraLarge, A5, A6, A7, A8, A9>"
    $availset="<set name>"
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image -AvailabilitySetName $availset

<span data-ttu-id="cad31-147">Hello InstanceSize hodnoty pro D – DS- a G-series virtuálních počítačů naleznete v části [virtuálního počítače a Cloud velikost služeb pro Azure](https://msdn.microsoft.com/library/azure/dn197896.aspx).</span><span class="sxs-lookup"><span data-stu-id="cad31-147">For hello InstanceSize values for D-, DS-, or G-series virtual machines, see [Virtual Machine and Cloud Service Sizes for Azure](https://msdn.microsoft.com/library/azure/dn197896.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="cad31-148">Pokud máte smlouvu Enterprise Agreement pomocí programu Software Assurance a chcete využít tootake hello systému Windows Server [výhody použití hybridní](https://azure.microsoft.com/pricing/hybrid-use-benefit/), přidejte **- LicenseType** parametr toohello  **Nové AzureVMConfig** rutiny předávání hello hodnotu **Windows_Server** pro hello typické případy použití.</span><span class="sxs-lookup"><span data-stu-id="cad31-148">If you have an Enterprise Agreement with Software Assurance, and intend tootake advantage of hello Windows Server [Hybrid Use Benefit](https://azure.microsoft.com/pricing/hybrid-use-benefit/), add the **-LicenseType** parameter toohello **New-AzureVMConfig** cmdlet, passing hello value **Windows_Server** for hello typical use case.</span></span>  <span data-ttu-id="cad31-149">Ujistěte se, že používáte bitovou kopii, kterou jste nahráli; standardní bitové kopie z hello Galerie nelze použít s hello hybridní použít výhody.</span><span class="sxs-lookup"><span data-stu-id="cad31-149">Be sure you are using an image you have uploaded; you cannot use a standard image from hello Gallery with hello Hybrid Use Benefit.</span></span>
> 
> 

<span data-ttu-id="cad31-150">Volitelně můžete pro samostatné počítače se systémem Windows zadejte hello účet místního správce a heslo.</span><span class="sxs-lookup"><span data-stu-id="cad31-150">Optionally, for a standalone Windows computer, specify hello local administrator account and password.</span></span>

    $cred=Get-Credential -Message "Type hello name and password of hello local administrator account."
    $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.Username -Password $cred.GetNetworkCredential().Password

<span data-ttu-id="cad31-151">Zvolte silné heslo.</span><span class="sxs-lookup"><span data-stu-id="cad31-151">Choose a strong password.</span></span> <span data-ttu-id="cad31-152">toocheck jeho síly, najdete v části [kontrolu hesla: pomocí silného hesla](https://www.microsoft.com/security/pc-security/password-checker.aspx).</span><span class="sxs-lookup"><span data-stu-id="cad31-152">toocheck its strength, see [Password Checker: Using Strong Passwords](https://www.microsoft.com/security/pc-security/password-checker.aspx).</span></span>

<span data-ttu-id="cad31-153">Volitelně tooadd hello Windows počítače tooan existující doméně Active Directory, zadejte účet místního správce hello a heslo, hello domény a hello jméno a heslo účtu domény.</span><span class="sxs-lookup"><span data-stu-id="cad31-153">Optionally, tooadd hello Windows computer tooan existing Active Directory domain, specify hello local administrator account and password, hello domain, and hello name and password of a domain account.</span></span>

    $cred1=Get-Credential –Message "Type hello name and password of hello local administrator account."
    $cred2=Get-Credential –Message "Now type hello name (not including hello domain) and password of an account that has permission tooadd hello machine toohello domain."
    $domaindns="<FQDN of hello domain that hello machine is joining>"
    $domacctdomain="<domain of hello account that has permission tooadd hello machine toohello domain>"
    $vm1 | Add-AzureProvisioningConfig -AdminUsername $cred1.Username -Password $cred1.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $cred2.Username -DomainPassword $cred2.GetNetworkCredential().Password -JoinDomain $domaindns

<span data-ttu-id="cad31-154">Další možnosti předběžné konfigurace pro virtuální počítače se systémem Windows najdete v tématu hello syntaxe hello **Windows** a **třídy WindowsDomain** parametr nastaví v [ Přidat AzureProvisioningConfig](/powershell/module/azure/add-azureprovisioningconfig).</span><span class="sxs-lookup"><span data-stu-id="cad31-154">For additional pre-configuration options for Windows-based virtual machines, see hello syntax for hello **Windows** and **WindowsDomain** parameter sets in [Add-AzureProvisioningConfig](/powershell/module/azure/add-azureprovisioningconfig).</span></span>

<span data-ttu-id="cad31-155">Virtuální počítač hello volitelně přiřadíte konkrétní IP adresu, označuje jako statické vyhrazené IP adresy.</span><span class="sxs-lookup"><span data-stu-id="cad31-155">Optionally, assign hello virtual machine a specific IP address, known as a static DIP.</span></span>

    $vm1 | Set-AzureStaticVNetIP -IPAddress <IP address>

<span data-ttu-id="cad31-156">Můžete ověřit, že je k dispozici s konkrétní IP adresu:</span><span class="sxs-lookup"><span data-stu-id="cad31-156">You can verify that a specific IP address is available with:</span></span>

    Test-AzureStaticVNetIP –VNetName <VNet name> –IPAddress <IP address>

<span data-ttu-id="cad31-157">Volitelně můžete přiřadíte konkrétní podsítě tooa hello virtuálního počítače ve virtuální sítě Azure.</span><span class="sxs-lookup"><span data-stu-id="cad31-157">Optionally, assign hello virtual machine tooa specific subnet in an Azure virtual network.</span></span>

    $vm1 | Set-AzureSubnet -SubnetNames "<name of hello subnet>"

<span data-ttu-id="cad31-158">Volitelně lze přidáte jeden datový disk toohello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="cad31-158">Optionally, add a single data disk toohello virtual machine.</span></span>

    $disksize=<size of hello disk in GB>
    $disklabel="<hello label on hello disk>"
    $lun=<Logical Unit Number (LUN) of hello disk>
    $hcaching="<Specify one: ReadOnly, ReadWrite, None>"
    $vm1 | Add-AzureDataDisk -CreateNew -DiskSizeInGB $disksize -DiskLabel $disklabel -LUN $lun -HostCaching $hcaching

<span data-ttu-id="cad31-159">Pro řadič domény služby Active Directory, nastavte $hcaching příliš "Žádný".</span><span class="sxs-lookup"><span data-stu-id="cad31-159">For an Active Directory domain controller, set $hcaching too"None".</span></span>

<span data-ttu-id="cad31-160">Volitelně lze přidáte hello sadu virtuálních počítačů tooan existující Vyrovnávání zatížení sítě pro externí přenosy.</span><span class="sxs-lookup"><span data-stu-id="cad31-160">Optionally, add hello virtual machine tooan existing load-balanced set for external traffic.</span></span>

    $protocol="<Specify one: tcp, udp>"
    $localport=<port number of hello internal port>
    $pubport=<port number of hello external port>
    $endpointname="<name of hello endpoint>"
    $lbsetname="<name of hello existing load-balanced set>"
    $probeprotocol="<Specify one: tcp, http>"
    $probeport=<TCP or HTTP port number of probe traffic>
    $probepath="<URL path for probe traffic>"
    $vm1 | Add-AzureEndpoint -Name $endpointname -Protocol $protocol -LocalPort $localport -PublicPort $pubport -LBSetName $lbsetname -ProbeProtocol $probeprotocol -ProbePort $probeport -ProbePath $probepath

<span data-ttu-id="cad31-161">Nakonec vyberte jednu z těchto bloků požadovaný příkaz pro vytvoření hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="cad31-161">Finally, choose one of these required command blocks for creating hello virtual machine.</span></span>

<span data-ttu-id="cad31-162">Možnost 1: Vytvoření hello virtuálního počítače ve stávající cloudovou službu.</span><span class="sxs-lookup"><span data-stu-id="cad31-162">Option 1: Create hello virtual machine in an existing cloud service.</span></span>

    New-AzureVM –ServiceName "<short name of hello cloud service>" -VMs $vm1

<span data-ttu-id="cad31-163">krátký název Hello hello cloudové služby je hello název, který se zobrazí v seznamu hello cloudové služby v hello portál Azure nebo v seznamu hello skupin prostředků v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="cad31-163">hello short name of hello cloud service is hello name that appears in hello list of Cloud Services in hello Azure portal or in hello list of Resource Groups in hello Azure portal.</span></span>

<span data-ttu-id="cad31-164">Možnost 2: Vytvoření hello virtuálního počítače do existující cloudové služby a virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="cad31-164">Option 2: Create hello virtual machine in an existing cloud service and virtual network.</span></span>

    $svcname="<short name of hello cloud service>"
    $vnetname="<name of hello virtual network>"
    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname

## <a name="step-5-run-your-command-set"></a><span data-ttu-id="cad31-165">Krok 5: Spuštění vaší sadu příkazů</span><span class="sxs-lookup"><span data-stu-id="cad31-165">Step 5: Run your command set</span></span>
<span data-ttu-id="cad31-166">Zkontrolovat sadu příkazů prostředí Azure PowerShell text hello, které jste vytvořili v textovém editoru nebo hello PowerShell ISE skládající se z více bloků příkazy z kroku 4.</span><span class="sxs-lookup"><span data-stu-id="cad31-166">Review hello Azure PowerShell command set you built in your text editor or hello PowerShell ISE consisting of multiple blocks of commands from step 4.</span></span> <span data-ttu-id="cad31-167">Ujistěte se, zda jste zadali všechny proměnné hello potřeby a zda mají hello správné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="cad31-167">Ensure that you have specified all hello needed variables and that they have hello correct values.</span></span> <span data-ttu-id="cad31-168">Také se ujistěte, že jste odstranili všechny hello < a > znaků.</span><span class="sxs-lookup"><span data-stu-id="cad31-168">Also make sure that you have removed all hello < and > characters.</span></span>

<span data-ttu-id="cad31-169">Pokud používáte textového editoru, příkaz copy hello nastavte toohello schránky a klikněte pravým tlačítkem na otevřete příkazový řádek prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="cad31-169">If you are using a text editor, copy hello command set toohello clipboard and then right-click your open Windows PowerShell command prompt.</span></span> <span data-ttu-id="cad31-170">To bude vydávat sadu příkazů hello jako řadu příkazů prostředí PowerShell a vytvořte virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="cad31-170">This will issue hello command set as a series of PowerShell commands and create your Azure virtual machine.</span></span> <span data-ttu-id="cad31-171">Alternativně spusťte příkaz hello nastavit v hello PowerShell ISE.</span><span class="sxs-lookup"><span data-stu-id="cad31-171">Alternately, run hello command set in hello PowerShell ISE.</span></span>

<span data-ttu-id="cad31-172">Pokud vytvoříte tento virtuální počítač znovu nebo podobné jeden, můžete:</span><span class="sxs-lookup"><span data-stu-id="cad31-172">If you will be creating this virtual machine again or a similar one, you can:</span></span>

* <span data-ttu-id="cad31-173">Uložte tento příkaz nastavit jako soubor skriptu prostředí PowerShell (*.ps1).</span><span class="sxs-lookup"><span data-stu-id="cad31-173">Save this command set as a PowerShell script file (*.ps1).</span></span>
* <span data-ttu-id="cad31-174">Uložit tento příkaz nastavit jako runbook služby Azure Automation v hello **účty Automation** části hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="cad31-174">Save this command set as an Azure Automation runbook in hello **Automation Accounts** section of hello Azure portal.</span></span>

## <span data-ttu-id="cad31-175"><a id="examples"></a>Příklady</span><span class="sxs-lookup"><span data-stu-id="cad31-175"><a id="examples"></a>Examples</span></span>
<span data-ttu-id="cad31-176">Tady jsou dva příklady použití hello postup uvedený výš toobuild prostředí Azure PowerShell příkaz sad, které vytvoření systému Windows Azure virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="cad31-176">Here are two examples of using hello steps above toobuild Azure PowerShell command sets that create Windows-based Azure virtual machines.</span></span>

### <a name="example-1"></a><span data-ttu-id="cad31-177">Příklad 1</span><span class="sxs-lookup"><span data-stu-id="cad31-177">Example 1</span></span>
<span data-ttu-id="cad31-178">Potřebuji prostředí PowerShell příkaz nastavit toocreate hello počáteční virtuálního počítače pro řadič domény služby Active Directory, který:</span><span class="sxs-lookup"><span data-stu-id="cad31-178">I need a PowerShell command set toocreate hello initial virtual machine for an Active Directory domain controller that:</span></span>

* <span data-ttu-id="cad31-179">Využívá image systému Windows Server 2012 R2 Datacenter hello.</span><span class="sxs-lookup"><span data-stu-id="cad31-179">Uses hello Windows Server 2012 R2 Datacenter image.</span></span>
* <span data-ttu-id="cad31-180">Obsahuje název hello AZDC1.</span><span class="sxs-lookup"><span data-stu-id="cad31-180">Has hello name AZDC1.</span></span>
* <span data-ttu-id="cad31-181">Je samostatný počítač.</span><span class="sxs-lookup"><span data-stu-id="cad31-181">Is a standalone computer.</span></span>
* <span data-ttu-id="cad31-182">Má k dispozici další data disk 20 GB.</span><span class="sxs-lookup"><span data-stu-id="cad31-182">Has an additional data disk of 20 GB.</span></span>
* <span data-ttu-id="cad31-183">Má hello statickou IP adresu 192.168.244.4.</span><span class="sxs-lookup"><span data-stu-id="cad31-183">Has hello static IP address 192.168.244.4.</span></span>
* <span data-ttu-id="cad31-184">Je v podsíti back-end hello hello AZDatacenter virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="cad31-184">Is in hello BackEnd subnet of hello AZDatacenter virtual network.</span></span>
* <span data-ttu-id="cad31-185">Je v cloudové službě Azure-Válcovny hello.</span><span class="sxs-lookup"><span data-stu-id="cad31-185">Is in hello Azure-TailspinToys cloud service.</span></span>

<span data-ttu-id="cad31-186">Zde je hello odpovídající prostředí Azure PowerShell příkaz set toocreate tento virtuální počítač s prázdné řádky mezi každého bloku čitelnější.</span><span class="sxs-lookup"><span data-stu-id="cad31-186">Here is hello corresponding Azure PowerShell command set toocreate this virtual machine, with blank lines between each block for readability.</span></span>

    $family="Windows Server 2012 R2 Datacenter"
    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1
    $vmname="AZDC1"
    $vmsize="Medium"
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    $cred=Get-Credential -Message "Type hello name and password of hello local administrator account."
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

### <a name="example-2"></a><span data-ttu-id="cad31-187">Příklad 2</span><span class="sxs-lookup"><span data-stu-id="cad31-187">Example 2</span></span>
<span data-ttu-id="cad31-188">Potřebuji prostředí PowerShell sady příkazů toocreate virtuálního počítače pro server obchodní který:</span><span class="sxs-lookup"><span data-stu-id="cad31-188">I need a PowerShell command set toocreate a virtual machine for a line-of-business server that:</span></span>

* <span data-ttu-id="cad31-189">Využívá image systému Windows Server 2012 R2 Datacenter hello.</span><span class="sxs-lookup"><span data-stu-id="cad31-189">Uses hello Windows Server 2012 R2 Datacenter image.</span></span>
* <span data-ttu-id="cad31-190">Obsahuje název hello LOB1.</span><span class="sxs-lookup"><span data-stu-id="cad31-190">Has hello name LOB1.</span></span>
* <span data-ttu-id="cad31-191">Je členem domény corp.contoso.com hello.</span><span class="sxs-lookup"><span data-stu-id="cad31-191">Is a member of hello corp.contoso.com domain.</span></span>
* <span data-ttu-id="cad31-192">Má k dispozici další data disk 200 GB.</span><span class="sxs-lookup"><span data-stu-id="cad31-192">Has an additional data disk of 200 GB.</span></span>
* <span data-ttu-id="cad31-193">Je v podsíti front-endu hello hello AZDatacenter virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="cad31-193">Is in hello FrontEnd subnet of hello AZDatacenter virtual network.</span></span>
* <span data-ttu-id="cad31-194">Je v cloudové službě Azure-Válcovny hello.</span><span class="sxs-lookup"><span data-stu-id="cad31-194">Is in hello Azure-TailspinToys cloud service.</span></span>

<span data-ttu-id="cad31-195">Zde je hello odpovídající prostředí Azure PowerShell příkaz set toocreate tohoto virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="cad31-195">Here is hello corresponding Azure PowerShell command set toocreate this virtual machine.</span></span>

    $family="Windows Server 2012 R2 Datacenter"
    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1
    $vmname="LOB1"
    $vmsize="Large"
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    $cred1=Get-Credential –Message "Type hello name and password of hello local administrator account."
    $cred2=Get-Credential –Message "Now type hello name (not including hello domain) and password of an account that has permission tooadd hello machine toohello domain."
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


## <a name="next-steps"></a><span data-ttu-id="cad31-196">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cad31-196">Next steps</span></span>
<span data-ttu-id="cad31-197">Pokud budete potřebovat disk s operačním systémem, která je větší než 127 GB, můžete [rozbalte hello operačního systému disku](../../virtual-machines-windows-expand-os-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="cad31-197">If you need an OS disk that is larger than 127 GB, you can [expand hello OS drive](../../virtual-machines-windows-expand-os-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

