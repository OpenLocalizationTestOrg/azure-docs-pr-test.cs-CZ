---
title: "aaaLOB aplikace testovacího prostředí | Microsoft Docs"
description: "Zjistěte, jak webové, toocreate obchodní aplikace v hybridním cloudové prostředí pro IT pro nebo vývoj, testování."
services: virtual-machines-windows
documentationcenter: 
author: JoeDavies-MSFT
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 92d2d8ce-60ed-4512-95e5-a7fe3b0ca00b
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 09/30/2016
ms.author: josephd
ms.openlocfilehash: e9f825bbb1cbeb841ee3c974ebf6721dc236c311
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-a-web-based-lob-application-in-a-hybrid-cloud-for-testing"></a><span data-ttu-id="38e4c-103">Nastavení webové aplikace LOB v hybridním cloudu pro testování</span><span class="sxs-lookup"><span data-stu-id="38e4c-103">Set up a web-based LOB application in a hybrid cloud for testing</span></span>
<span data-ttu-id="38e4c-104">Toto téma vás provede jednotlivými kroky vytváření simulované hybridní cloudové prostředí pro testování webových řádek obchodní (LOB) aplikace hostované ve službě Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="38e4c-104">This topic steps you through creating a simulated hybrid cloud environment for testing a web-based line of business (LOB) application hosted in Microsoft Azure.</span></span> <span data-ttu-id="38e4c-105">Zde je hello Výsledná konfigurace.</span><span class="sxs-lookup"><span data-stu-id="38e4c-105">Here is hello resulting configuration.</span></span>

![](./media/ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph3.png)

<span data-ttu-id="38e4c-106">Tato konfigurace zahrnuje:</span><span class="sxs-lookup"><span data-stu-id="38e4c-106">This configuration consists of:</span></span>

* <span data-ttu-id="38e4c-107">Simulované místní síti hostované v Azure (hello testovací laboratoř virtuální sítě).</span><span class="sxs-lookup"><span data-stu-id="38e4c-107">A simulated on-premises network hosted in Azure (hello TestLab VNet).</span></span>
* <span data-ttu-id="38e4c-108">Mezi různými místy virtuální síť hostované v Azure (TestVNET).</span><span class="sxs-lookup"><span data-stu-id="38e4c-108">A cross-premises virtual network hosted in Azure (TestVNET).</span></span>
* <span data-ttu-id="38e4c-109">Připojení k síti VPN VNet-to-VNet.</span><span class="sxs-lookup"><span data-stu-id="38e4c-109">A VNet-to-VNet VPN connection.</span></span>
* <span data-ttu-id="38e4c-110">Webové LOB serveru, systému SQL server a sekundární řadič domény ve virtuální síti TestVNET hello.</span><span class="sxs-lookup"><span data-stu-id="38e4c-110">A web-based LOB server, SQL server, and secondary domain controller in hello TestVNET virtual network.</span></span>

<span data-ttu-id="38e4c-111">Tato konfigurace poskytuje základ a běžné počáteční bod, ze kterého můžete:</span><span class="sxs-lookup"><span data-stu-id="38e4c-111">This configuration provides a basis and common starting point from which you can:</span></span>

* <span data-ttu-id="38e4c-112">Vývoj a testování obchodních aplikací, které jsou hostované v Internetové informační služby (IIS) u back-end databáze SQL Server 2014 v Azure.</span><span class="sxs-lookup"><span data-stu-id="38e4c-112">Develop and test LOB applications hosted on Internet Information Services (IIS) with a SQL Server 2014 database backend in Azure.</span></span>
* <span data-ttu-id="38e4c-113">Proveďte testování této simulované hybridní cloudové IT úlohy.</span><span class="sxs-lookup"><span data-stu-id="38e4c-113">Perform testing of this simulated hybrid cloud-based IT workload.</span></span>

<span data-ttu-id="38e4c-114">Existují tři hlavní fáze toosetting toto hybridní cloud testovací prostředí:</span><span class="sxs-lookup"><span data-stu-id="38e4c-114">There are three major phases toosetting up this hybrid cloud test environment:</span></span>

1. <span data-ttu-id="38e4c-115">Nastavte hello simulované hybridní cloudové prostředí.</span><span class="sxs-lookup"><span data-stu-id="38e4c-115">Set up hello simulated hybrid cloud environment.</span></span>
2. <span data-ttu-id="38e4c-116">Nakonfigurujte hello počítač serveru SQL (SQL1).</span><span class="sxs-lookup"><span data-stu-id="38e4c-116">Configure hello SQL server computer (SQL1).</span></span>
3. <span data-ttu-id="38e4c-117">Nakonfigurujte server hello LOB (LOB1).</span><span class="sxs-lookup"><span data-stu-id="38e4c-117">Configure hello LOB server (LOB1).</span></span>

<span data-ttu-id="38e4c-118">Tato úloha vyžaduje předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="38e4c-118">This workload requires an Azure subscription.</span></span> <span data-ttu-id="38e4c-119">Pokud máte předplatné MSDN nebo v sadě Visual Studio, najdete v části [platební měsíční Azure pro předplatitele sady Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span><span class="sxs-lookup"><span data-stu-id="38e4c-119">If you have an MSDN or Visual Studio subscription, see [Monthly Azure credit for Visual Studio subscribers](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span></span>

<span data-ttu-id="38e4c-120">Příklad produkční aplikace LOB, které jsou hostované v Azure, naleznete v části hello **obchodních aplikací** architektura plán, podle kterého na [diagramy architektura softwaru společnosti Microsoft a plány vyfotografovat](http://msdn.microsoft.com/dn630664).</span><span class="sxs-lookup"><span data-stu-id="38e4c-120">For an example of a production LOB application hosted in Azure, see hello **Line of business applications** architecture blueprint at [Microsoft Software Architecture Diagrams and Blueprints](http://msdn.microsoft.com/dn630664).</span></span>

## <a name="phase-1-set-up-hello-simulated-hybrid-cloud-environment"></a><span data-ttu-id="38e4c-121">Fáze 1: Nastavení hello simulované hybridní cloudové prostředí</span><span class="sxs-lookup"><span data-stu-id="38e4c-121">Phase 1: Set up hello simulated hybrid cloud environment</span></span>
<span data-ttu-id="38e4c-122">Vytvoření hello [simulované hybridní cloud testovací prostředí](ps-hybrid-cloud-test-env-sim.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="38e4c-122">Create hello [simulated hybrid cloud test environment](ps-hybrid-cloud-test-env-sim.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="38e4c-123">Protože toto testovací prostředí nevyžaduje hello přítomnost server hello APP1 na podsíti Corpnet hello, můžete ho vypnout teď.</span><span class="sxs-lookup"><span data-stu-id="38e4c-123">Because this test environment does not require hello presence of hello APP1 server on hello Corpnet subnet, you can shut it down for now.</span></span>

<span data-ttu-id="38e4c-124">Toto je aktuální konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="38e4c-124">This is your current configuration.</span></span>

![](./media/ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph1.png)

## <a name="phase-2-configure-hello-sql-server-computer-sql1"></a><span data-ttu-id="38e4c-125">Fáze 2: Konfigurace hello počítač serveru SQL (SQL1)</span><span class="sxs-lookup"><span data-stu-id="38e4c-125">Phase 2: Configure hello SQL server computer (SQL1)</span></span>
<span data-ttu-id="38e4c-126">Z hello portálu Azure spusťte počítač DC2 hello v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="38e4c-126">From hello Azure portal, start hello DC2 computer if needed.</span></span>

<span data-ttu-id="38e4c-127">Dále vytvořte virtuální počítač pro počítač SQL1 pomocí těchto příkazů na příkazovém řádku prostředí Azure PowerShell v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="38e4c-127">Next, create a virtual machine for SQL1 with these commands at an Azure PowerShell command prompt on your local computer.</span></span> <span data-ttu-id="38e4c-128">Předchozí toorunning tyto příkazy vyplnit hello hodnoty proměnných a odebrat hello < a > znaků.</span><span class="sxs-lookup"><span data-stu-id="38e4c-128">Prior toorunning these commands, fill in hello variable values and remove hello < and > characters.</span></span>

    $rgName="<your resource group name>"
    $locName="<hello Azure location of your resource group>"
    $saName="<your storage account name>"

    $vnet=Get-AzureRMVirtualNetwork -Name "TestVNET" -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "TestSubnet"
    $pip=New-AzureRMPublicIpAddress -Name SQL1-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $nic=New-AzureRMNetworkInterface -Name SQL1-NIC -ResourceGroupName $rgName -Location $locName -Subnet $subnet -PublicIpAddress $pip
    $vm=New-AzureRMVMConfig -VMName SQL1 -VMSize Standard_A4
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $vhdURI=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/SQL1-SQLDataDisk.vhd"
    Add-AzureRMVMDataDisk -VM $vm -Name "Data" -DiskSizeInGB 100 -VhdUri $vhdURI  -CreateOption empty

    $cred=Get-Credential -Message "Type hello name and password of hello local administrator account for hello SQL Server computer." 
    $vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName SQL1 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftSQLServer -Offer SQL2014-WS2012R2 -Skus Standard -Version "latest"
    $vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/SQL1-OSDisk.vhd"
    $vm=Set-AzureRMVMOSDisk -VM $vm -Name "OSDisk" -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

<span data-ttu-id="38e4c-129">Použijte hello Azure portálu tooconnect tooSQL1 pomocí účtu místního správce hello SQL1.</span><span class="sxs-lookup"><span data-stu-id="38e4c-129">Use hello Azure portal tooconnect tooSQL1 using hello local administrator account of SQL1.</span></span>

<span data-ttu-id="38e4c-130">V dalším kroku nakonfigurujte testování základní konektivity tooallow pravidla brány Windows Firewall a přenosy dat systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="38e4c-130">Next, configure Windows Firewall rules tooallow basic connectivity testing and SQL Server traffic.</span></span> <span data-ttu-id="38e4c-131">Z prostředí Windows PowerShell na úrovni správce příkazového řádku na počítači SQL1 spuštění těchto příkazů.</span><span class="sxs-lookup"><span data-stu-id="38e4c-131">From an administrator-level Windows PowerShell command prompt on SQL1, run these commands.</span></span>

    New-NetFirewallRule -DisplayName "SQL Server" -Direction Inbound -Protocol TCP -LocalPort 1433,1434,5022 -Action allow 
    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc2.corp.contoso.com

<span data-ttu-id="38e4c-132">příkaz ping Hello by měl mít za následek čtyři úspěšné odpovědi od IP adresa 192.168.0.4.</span><span class="sxs-lookup"><span data-stu-id="38e4c-132">hello ping command should result in four successful replies from IP address 192.168.0.4.</span></span>

<span data-ttu-id="38e4c-133">Dál přidejte hello navíc datový disk na počítači SQL1 jako nový svazek s písmenem jednotky hello F:.</span><span class="sxs-lookup"><span data-stu-id="38e4c-133">Next, add hello extra data disk on SQL1 as a new volume with hello drive letter F:.</span></span>

1. <span data-ttu-id="38e4c-134">V levém podokně hello správce serveru, klikněte na **Souborová služba a služba úložiště**a potom klikněte na **disky**.</span><span class="sxs-lookup"><span data-stu-id="38e4c-134">In hello left pane of Server Manager, click **File and Storage Services**, and then click **Disks**.</span></span>
2. <span data-ttu-id="38e4c-135">V podokně obsahu hello v hello **disky** klikněte na možnost **disku 2** (s hello **oddílu** nastavit příliš**neznámé**).</span><span class="sxs-lookup"><span data-stu-id="38e4c-135">In hello contents pane, in hello **Disks** group, click **disk 2** (with hello **Partition** set too**Unknown**).</span></span>
3. <span data-ttu-id="38e4c-136">Klikněte na tlačítko **úlohy**a potom klikněte na **nový svazek**.</span><span class="sxs-lookup"><span data-stu-id="38e4c-136">Click **Tasks**, and then click **New Volume**.</span></span>
4. <span data-ttu-id="38e4c-137">Na hello před zahájením stránku hello Průvodce vytvořením svazku, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="38e4c-137">On hello Before you begin page of hello New Volume Wizard, click **Next**.</span></span>
5. <span data-ttu-id="38e4c-138">Na serveru vyberte hello hello a stránka disku, klikněte na tlačítko **disku 2**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="38e4c-138">On hello Select hello server and disk page, click **Disk 2**, and then click **Next**.</span></span> <span data-ttu-id="38e4c-139">Po zobrazení výzvy klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="38e4c-139">When prompted, click **OK**.</span></span>
6. <span data-ttu-id="38e4c-140">Na hello zadat velikost hello hello svazku stránky, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="38e4c-140">On hello Specify hello size of hello volume page, click **Next**.</span></span>
7. <span data-ttu-id="38e4c-141">Na hello přiřazovat tooa jednotky písmeno nebo složka stránky, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="38e4c-141">On hello Assign tooa drive letter or folder page, click **Next**.</span></span>
8. <span data-ttu-id="38e4c-142">Na stránce nastavení systému hello vybrat soubor, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="38e4c-142">On hello Select file system settings page, click **Next**.</span></span>
9. <span data-ttu-id="38e4c-143">Na stránce Potvrdit vybrané možnosti hello klikněte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="38e4c-143">On hello Confirm selections page, click **Create**.</span></span>
10. <span data-ttu-id="38e4c-144">Po dokončení klikněte na tlačítko **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="38e4c-144">When complete, click **Close**.</span></span>

<span data-ttu-id="38e4c-145">Spuštěním těchto příkazů na příkazovém řádku prostředí Windows PowerShell hello na počítači SQL1:</span><span class="sxs-lookup"><span data-stu-id="38e4c-145">Run these commands at hello Windows PowerShell command prompt on SQL1:</span></span>

    md f:\Data
    md f:\Log
    md f:\Backup

<span data-ttu-id="38e4c-146">V dalším kroku připojit k doméně CORP Windows Server Active Directory toohello SQL1 s těmito příkazy příkazového řádku Windows Powershellu hello na počítači SQL1.</span><span class="sxs-lookup"><span data-stu-id="38e4c-146">Next, join SQL1 toohello CORP Windows Server Active Directory domain with these commands at hello Windows PowerShell prompt on SQL1.</span></span>

    Add-Computer -DomainName corp.contoso.com
    Restart-Computer

<span data-ttu-id="38e4c-147">Použít účet hello CORP\User1 po zobrazení výzvy pověření účtu domény toosupply pro hello **Add-Computer** příkaz.</span><span class="sxs-lookup"><span data-stu-id="38e4c-147">Use hello CORP\User1 account when prompted toosupply domain account credentials for hello **Add-Computer** command.</span></span>

<span data-ttu-id="38e4c-148">Po restartování počítače, použijte hello Azure portálu tooconnect tooSQL1 *pomocí účtu místního správce hello SQL1*.</span><span class="sxs-lookup"><span data-stu-id="38e4c-148">After restarting, use hello Azure portal tooconnect tooSQL1 *with hello local administrator account of SQL1*.</span></span>

<span data-ttu-id="38e4c-149">V dalším kroku nakonfigurujte SQL Server 2014 toouse hello F: disku pro nové databáze a oprávnění účtu uživatele.</span><span class="sxs-lookup"><span data-stu-id="38e4c-149">Next, configure SQL Server 2014 toouse hello F: drive for new databases and for user account permissions.</span></span>

1. <span data-ttu-id="38e4c-150">Na úvodní obrazovce hello zadejte **SQL Server Management**a potom klikněte na **SQL Server 2014 Management Studio**.</span><span class="sxs-lookup"><span data-stu-id="38e4c-150">From hello Start screen, type **SQL Server Management**, and then click **SQL Server 2014 Management Studio**.</span></span>
2. <span data-ttu-id="38e4c-151">V **připojit tooServer**, klikněte na tlačítko **Connect**.</span><span class="sxs-lookup"><span data-stu-id="38e4c-151">In **Connect tooServer**, click **Connect**.</span></span>
3. <span data-ttu-id="38e4c-152">V podokně stromu hello Průzkumník objektů klikněte pravým tlačítkem na **SQL1**a potom klikněte na **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="38e4c-152">In hello Object Explorer tree pane, right-click **SQL1**, and then click **Properties**.</span></span>
4. <span data-ttu-id="38e4c-153">V hello **vlastnosti serveru** okně klikněte na tlačítko **nastavení databáze**.</span><span class="sxs-lookup"><span data-stu-id="38e4c-153">In hello **Server Properties** window, click **Database Settings**.</span></span>
5. <span data-ttu-id="38e4c-154">Vyhledejte hello **databáze výchozí umístění** a nastavte tyto hodnoty:</span><span class="sxs-lookup"><span data-stu-id="38e4c-154">Locate hello **Database default locations** and set these values:</span></span> 
   * <span data-ttu-id="38e4c-155">Pro **Data**, zadejte cestu hello **f:\Data**.</span><span class="sxs-lookup"><span data-stu-id="38e4c-155">For **Data**, type hello path **f:\Data**.</span></span>
   * <span data-ttu-id="38e4c-156">Pro **protokolu**, zadejte cestu hello **f:\Log**.</span><span class="sxs-lookup"><span data-stu-id="38e4c-156">For **Log**, type hello path **f:\Log**.</span></span>
   * <span data-ttu-id="38e4c-157">Pro **zálohování**, zadejte cestu hello **f:\Backup**.</span><span class="sxs-lookup"><span data-stu-id="38e4c-157">For **Backup**, type hello path **f:\Backup**.</span></span>
   * <span data-ttu-id="38e4c-158">Poznámka: Pouze nové databáze pomocí těchto umístění.</span><span class="sxs-lookup"><span data-stu-id="38e4c-158">Note: Only new databases use these locations.</span></span>
6. <span data-ttu-id="38e4c-159">Klikněte na tlačítko hello **OK** tooclose hello okno.</span><span class="sxs-lookup"><span data-stu-id="38e4c-159">Click hello **OK** tooclose hello window.</span></span>
7. <span data-ttu-id="38e4c-160">V hello **Průzkumník objektů** stromové struktuře otevřete **zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="38e4c-160">In hello **Object Explorer** tree pane, open **Security**.</span></span>
8. <span data-ttu-id="38e4c-161">Klikněte pravým tlačítkem na **přihlášení** a pak klikněte na **nového přihlašovacího jména**.</span><span class="sxs-lookup"><span data-stu-id="38e4c-161">Right-click **Logins** and then click **New Login**.</span></span>
9. <span data-ttu-id="38e4c-162">V **přihlašovací jméno**, typ **CORP\User1**.</span><span class="sxs-lookup"><span data-stu-id="38e4c-162">In **Login name**, type **CORP\User1**.</span></span>
10. <span data-ttu-id="38e4c-163">Na hello **role serveru** klikněte na tlačítko **sysadmin**a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="38e4c-163">On hello **Server Roles** page, click **sysadmin**, and then click **OK**.</span></span>
11. <span data-ttu-id="38e4c-164">Zavřete Microsoft SQL Server Management Studio.</span><span class="sxs-lookup"><span data-stu-id="38e4c-164">Close Microsoft SQL Server Management Studio.</span></span>

<span data-ttu-id="38e4c-165">Toto je aktuální konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="38e4c-165">This is your current configuration.</span></span>

![](./media/ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph2.png)

## <a name="phase-3-configure-hello-lob-server-lob1"></a><span data-ttu-id="38e4c-166">Fáze 3: Konfigurace serveru LOB hello (LOB1)</span><span class="sxs-lookup"><span data-stu-id="38e4c-166">Phase 3: Configure hello LOB server (LOB1)</span></span>
<span data-ttu-id="38e4c-167">Nejdřív vytvořte virtuální počítač pro LOB1 pomocí těchto příkazů na příkazovém řádku prostředí Azure PowerShell hello v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="38e4c-167">First, create a virtual machine for LOB1 with these commands at hello Azure PowerShell command prompt on your local computer.</span></span>

    $rgName="<your resource group name>"
    $locName="<your Azure location, such as West US>"
    $saName="<your storage account name>"

    $vnet=Get-AzureRMVirtualNetwork -Name "TestVNET" -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "TestSubnet"
    $pip=New-AzureRMPublicIpAddress -Name LOB1-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $nic=New-AzureRMNetworkInterface -Name LOB1-NIC -ResourceGroupName $rgName -Location $locName -Subnet $subnet -PublicIpAddress $pip
    $vm=New-AzureRMVMConfig -VMName LOB1 -VMSize Standard_A2
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $cred=Get-Credential -Message "Type hello name and password of hello local administrator account for LOB1."
    $vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName LOB1 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/LOB1-TestLab-OSDisk.vhd"
    $vm=Set-AzureRMVMOSDisk -VM $vm -Name LOB1-TestVNET-OSDisk -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

<span data-ttu-id="38e4c-168">Pak pomocí portálu Azure tooconnect tooLOB1 hello s přihlašovacími údaji hello účtu místního správce hello LOB1.</span><span class="sxs-lookup"><span data-stu-id="38e4c-168">Next, use hello Azure portal tooconnect tooLOB1 with hello credentials of hello local administrator account of LOB1.</span></span>

<span data-ttu-id="38e4c-169">V dalším kroku nakonfigurujte přenosem tooallow pravidla brány Windows Firewall pro testování základní připojení.</span><span class="sxs-lookup"><span data-stu-id="38e4c-169">Next, configure a Windows Firewall rule tooallow traffic for basic connectivity testing.</span></span> <span data-ttu-id="38e4c-170">Z prostředí Windows PowerShell na úrovni správce příkazového řádku na LOB1 spuštění těchto příkazů.</span><span class="sxs-lookup"><span data-stu-id="38e4c-170">From an administrator-level Windows PowerShell command prompt on LOB1, run these commands.</span></span>

    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc2.corp.contoso.com

<span data-ttu-id="38e4c-171">příkaz ping Hello by měl mít za následek čtyři úspěšné odpovědi od IP adresa 192.168.0.4.</span><span class="sxs-lookup"><span data-stu-id="38e4c-171">hello ping command should result in four successful replies from IP address 192.168.0.4.</span></span>

<span data-ttu-id="38e4c-172">V dalším kroku připojit k doméně služby Active Directory CORP toohello LOB1 s těmito příkazy v příkazovém řádku prostředí Windows PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="38e4c-172">Next, join LOB1 toohello CORP Active Directory domain with these commands at hello Windows PowerShell prompt.</span></span>

    Add-Computer -DomainName corp.contoso.com
    Restart-Computer

<span data-ttu-id="38e4c-173">Použít účet hello CORP\User1 po zobrazení výzvy pověření účtu domény toosupply pro hello **Add-Computer** příkaz.</span><span class="sxs-lookup"><span data-stu-id="38e4c-173">Use hello CORP\User1 account when prompted toosupply domain account credentials for hello **Add-Computer** command.</span></span>

<span data-ttu-id="38e4c-174">Po restartování počítače se pomocí portálu Azure tooconnect tooLOB1 hello hello CORP\User1 účet a heslo.</span><span class="sxs-lookup"><span data-stu-id="38e4c-174">After restarting, use hello Azure portal tooconnect tooLOB1 with hello CORP\User1 account and password.</span></span>

<span data-ttu-id="38e4c-175">V dalším kroku konfigurace LOB1 pro službu IIS a otestování přístup z počítače CLIENT1.</span><span class="sxs-lookup"><span data-stu-id="38e4c-175">Next, configure LOB1 for IIS and test access from CLIENT1.</span></span>

1. <span data-ttu-id="38e4c-176">Ve Správci serveru klikněte na tlačítko **přidat role a funkce**.</span><span class="sxs-lookup"><span data-stu-id="38e4c-176">From Server Manager, click **Add roles and features**.</span></span>
2. <span data-ttu-id="38e4c-177">Na hello **před zahájením** klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="38e4c-177">On hello **Before you begin** page, click **Next**.</span></span>
3. <span data-ttu-id="38e4c-178">Na hello **vybrat typ instalace** klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="38e4c-178">On hello **Select installation type** page, click **Next**.</span></span>
4. <span data-ttu-id="38e4c-179">Na hello **vybrat cílový server** klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="38e4c-179">On hello **Select destination server** page, click **Next**.</span></span>
5. <span data-ttu-id="38e4c-180">Na hello **role serveru** klikněte na tlačítko **webového serveru (IIS)** v seznamu hello **role**.</span><span class="sxs-lookup"><span data-stu-id="38e4c-180">On hello **Server roles** page, click **Web Server (IIS)** in hello list of **Roles**.</span></span>
6. <span data-ttu-id="38e4c-181">Po zobrazení výzvy klikněte na tlačítko **přidat funkce**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="38e4c-181">When prompted, click **Add Features**, and then click **Next**.</span></span>
7. <span data-ttu-id="38e4c-182">Na hello **vyberte funkce** klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="38e4c-182">On hello **Select features** page, click **Next**.</span></span>
8. <span data-ttu-id="38e4c-183">Na hello **webového serveru (IIS)** klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="38e4c-183">On hello **Web Server (IIS)** page, click **Next**.</span></span>
9. <span data-ttu-id="38e4c-184">Na hello **vybrat služby rolí** vyberte nebo zrušte zaškrtnutí políček hello hello služeb, je nutné pro testování vašich obchodních aplikací a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="38e4c-184">On hello **Select role services** page, select or clear hello check boxes for hello services you need for testing your LOB application, and then click **Next**.</span></span>
10. <span data-ttu-id="38e4c-185">Na hello **potvrdit vybrané možnosti instalace** klikněte na tlačítko **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="38e4c-185">On hello **Confirm installation selections** page, click **Install**.</span></span>
11. <span data-ttu-id="38e4c-186">Počkejte, dokud hello instalace součástí byla dokončena a pak klikněte na tlačítko **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="38e4c-186">Wait until hello installation of components has completed and then click **Close**.</span></span>
12. <span data-ttu-id="38e4c-187">Z hello portálu Azure připojte počítač CLIENT1 toohello pomocí pověření účtu CORP\User1 hello a pak spusťte aplikaci Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="38e4c-187">From hello Azure portal, connect toohello CLIENT1 computer with hello CORP\User1 account credentials, and then start Internet Explorer.</span></span>
13. <span data-ttu-id="38e4c-188">V panelu Adresa hello, zadejte **http://lob1/** a stiskněte klávesu ENTER.</span><span class="sxs-lookup"><span data-stu-id="38e4c-188">In hello Address bar, type **http://lob1/** and then press ENTER.</span></span> <span data-ttu-id="38e4c-189">Měli byste vidět hello výchozí IIS 8 webové stránky.</span><span class="sxs-lookup"><span data-stu-id="38e4c-189">You should see hello default IIS 8 web page.</span></span>

<span data-ttu-id="38e4c-190">Toto je aktuální konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="38e4c-190">This is your current configuration.</span></span>

![](./media/ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph3.png)

<span data-ttu-id="38e4c-191">Toto prostředí je teď připravené pro jste toodeploy webové aplikace na LOB1 a testovací funkce počítači CLIENT1 na podsíti Corpnet hello.</span><span class="sxs-lookup"><span data-stu-id="38e4c-191">This environment is now ready for you toodeploy your web-based application on LOB1 and test functionality from CLIENT1 on hello Corpnet subnet.</span></span>

## <a name="next-step"></a><span data-ttu-id="38e4c-192">Další krok</span><span class="sxs-lookup"><span data-stu-id="38e4c-192">Next step</span></span>
* <span data-ttu-id="38e4c-193">Přidat nový virtuální počítač pomocí hello [portál Azure](../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="38e4c-193">Add a new virtual machine using hello [Azure portal](../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

