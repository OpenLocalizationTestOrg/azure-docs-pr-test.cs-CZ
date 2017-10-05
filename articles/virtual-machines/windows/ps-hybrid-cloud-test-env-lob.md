---
title: "OBCHODNÍ aplikace testovacího prostředí | Microsoft Docs"
description: "Naučte se vytvářet webové, řádek obchodní aplikace v hybridním prostředí cloudu pro IT pro nebo vývoj, testování."
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
ms.openlocfilehash: 8e9a380458bfede80ed0fc1ee7e62b0caec09501
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="set-up-a-web-based-lob-application-in-a-hybrid-cloud-for-testing"></a><span data-ttu-id="abb10-103">Nastavení webové aplikace LOB v hybridním cloudu pro testování</span><span class="sxs-lookup"><span data-stu-id="abb10-103">Set up a web-based LOB application in a hybrid cloud for testing</span></span>
<span data-ttu-id="abb10-104">Toto téma vás provede jednotlivými kroky vytváření simulované hybridní cloudové prostředí pro testování webových řádek obchodní (LOB) aplikace hostované ve službě Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="abb10-104">This topic steps you through creating a simulated hybrid cloud environment for testing a web-based line of business (LOB) application hosted in Microsoft Azure.</span></span> <span data-ttu-id="abb10-105">Zde je Výsledná konfigurace.</span><span class="sxs-lookup"><span data-stu-id="abb10-105">Here is the resulting configuration.</span></span>

![](./media/ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph3.png)

<span data-ttu-id="abb10-106">Tato konfigurace zahrnuje:</span><span class="sxs-lookup"><span data-stu-id="abb10-106">This configuration consists of:</span></span>

* <span data-ttu-id="abb10-107">Simulované místní síti hostované v Azure (VNet testovací laboratoř).</span><span class="sxs-lookup"><span data-stu-id="abb10-107">A simulated on-premises network hosted in Azure (the TestLab VNet).</span></span>
* <span data-ttu-id="abb10-108">Mezi různými místy virtuální síť hostované v Azure (TestVNET).</span><span class="sxs-lookup"><span data-stu-id="abb10-108">A cross-premises virtual network hosted in Azure (TestVNET).</span></span>
* <span data-ttu-id="abb10-109">Připojení k síti VPN VNet-to-VNet.</span><span class="sxs-lookup"><span data-stu-id="abb10-109">A VNet-to-VNet VPN connection.</span></span>
* <span data-ttu-id="abb10-110">Webové LOB serveru, systému SQL server a sekundární řadič domény ve virtuální síti TestVNET.</span><span class="sxs-lookup"><span data-stu-id="abb10-110">A web-based LOB server, SQL server, and secondary domain controller in the TestVNET virtual network.</span></span>

<span data-ttu-id="abb10-111">Tato konfigurace poskytuje základ a běžné počáteční bod, ze kterého můžete:</span><span class="sxs-lookup"><span data-stu-id="abb10-111">This configuration provides a basis and common starting point from which you can:</span></span>

* <span data-ttu-id="abb10-112">Vývoj a testování obchodních aplikací, které jsou hostované v Internetové informační služby (IIS) u back-end databáze SQL Server 2014 v Azure.</span><span class="sxs-lookup"><span data-stu-id="abb10-112">Develop and test LOB applications hosted on Internet Information Services (IIS) with a SQL Server 2014 database backend in Azure.</span></span>
* <span data-ttu-id="abb10-113">Proveďte testování této simulované hybridní cloudové IT úlohy.</span><span class="sxs-lookup"><span data-stu-id="abb10-113">Perform testing of this simulated hybrid cloud-based IT workload.</span></span>

<span data-ttu-id="abb10-114">Existují tři hlavní fáze k nastavení tohoto testovacího prostředí hybridního cloudu:</span><span class="sxs-lookup"><span data-stu-id="abb10-114">There are three major phases to setting up this hybrid cloud test environment:</span></span>

1. <span data-ttu-id="abb10-115">Nastavení prostředí simulované hybridní cloud.</span><span class="sxs-lookup"><span data-stu-id="abb10-115">Set up the simulated hybrid cloud environment.</span></span>
2. <span data-ttu-id="abb10-116">Nakonfigurujte počítač serveru SQL (SQL1).</span><span class="sxs-lookup"><span data-stu-id="abb10-116">Configure the SQL server computer (SQL1).</span></span>
3. <span data-ttu-id="abb10-117">Nakonfigurujte server LOB (LOB1).</span><span class="sxs-lookup"><span data-stu-id="abb10-117">Configure the LOB server (LOB1).</span></span>

<span data-ttu-id="abb10-118">Tato úloha vyžaduje předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="abb10-118">This workload requires an Azure subscription.</span></span> <span data-ttu-id="abb10-119">Pokud máte předplatné MSDN nebo v sadě Visual Studio, najdete v části [platební měsíční Azure pro předplatitele sady Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span><span class="sxs-lookup"><span data-stu-id="abb10-119">If you have an MSDN or Visual Studio subscription, see [Monthly Azure credit for Visual Studio subscribers](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span></span>

<span data-ttu-id="abb10-120">Příklad produkční aplikace LOB, které jsou hostované v Azure, naleznete v části **obchodních aplikací** architektura plán, podle kterého na [diagramy architektura softwaru společnosti Microsoft a plány vyfotografovat](http://msdn.microsoft.com/dn630664).</span><span class="sxs-lookup"><span data-stu-id="abb10-120">For an example of a production LOB application hosted in Azure, see the **Line of business applications** architecture blueprint at [Microsoft Software Architecture Diagrams and Blueprints](http://msdn.microsoft.com/dn630664).</span></span>

## <a name="phase-1-set-up-the-simulated-hybrid-cloud-environment"></a><span data-ttu-id="abb10-121">Fáze 1: Nastavení simulované hybridní cloudové prostředí</span><span class="sxs-lookup"><span data-stu-id="abb10-121">Phase 1: Set up the simulated hybrid cloud environment</span></span>
<span data-ttu-id="abb10-122">Vytvořte [simulované hybridní cloud testovací prostředí](ps-hybrid-cloud-test-env-sim.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="abb10-122">Create the [simulated hybrid cloud test environment](ps-hybrid-cloud-test-env-sim.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="abb10-123">Protože toto testovací prostředí nevyžaduje přítomnost serveru APP1 na podsíti Corpnet, můžete ho vypnout teď.</span><span class="sxs-lookup"><span data-stu-id="abb10-123">Because this test environment does not require the presence of the APP1 server on the Corpnet subnet, you can shut it down for now.</span></span>

<span data-ttu-id="abb10-124">Toto je aktuální konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="abb10-124">This is your current configuration.</span></span>

![](./media/ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph1.png)

## <a name="phase-2-configure-the-sql-server-computer-sql1"></a><span data-ttu-id="abb10-125">Fáze 2: Konfigurace počítače serveru SQL (SQL1)</span><span class="sxs-lookup"><span data-stu-id="abb10-125">Phase 2: Configure the SQL server computer (SQL1)</span></span>
<span data-ttu-id="abb10-126">Z portálu Azure spusťte počítač DC2, v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="abb10-126">From the Azure portal, start the DC2 computer if needed.</span></span>

<span data-ttu-id="abb10-127">Dále vytvořte virtuální počítač pro počítač SQL1 pomocí těchto příkazů na příkazovém řádku prostředí Azure PowerShell v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="abb10-127">Next, create a virtual machine for SQL1 with these commands at an Azure PowerShell command prompt on your local computer.</span></span> <span data-ttu-id="abb10-128">Před spuštěním těchto příkazů, vyplňte hodnoty proměnné a odebrat < a > znaků.</span><span class="sxs-lookup"><span data-stu-id="abb10-128">Prior to running these commands, fill in the variable values and remove the < and > characters.</span></span>

    $rgName="<your resource group name>"
    $locName="<the Azure location of your resource group>"
    $saName="<your storage account name>"

    $vnet=Get-AzureRMVirtualNetwork -Name "TestVNET" -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "TestSubnet"
    $pip=New-AzureRMPublicIpAddress -Name SQL1-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $nic=New-AzureRMNetworkInterface -Name SQL1-NIC -ResourceGroupName $rgName -Location $locName -Subnet $subnet -PublicIpAddress $pip
    $vm=New-AzureRMVMConfig -VMName SQL1 -VMSize Standard_A4
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $vhdURI=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/SQL1-SQLDataDisk.vhd"
    Add-AzureRMVMDataDisk -VM $vm -Name "Data" -DiskSizeInGB 100 -VhdUri $vhdURI  -CreateOption empty

    $cred=Get-Credential -Message "Type the name and password of the local administrator account for the SQL Server computer." 
    $vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName SQL1 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftSQLServer -Offer SQL2014-WS2012R2 -Skus Standard -Version "latest"
    $vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/SQL1-OSDisk.vhd"
    $vm=Set-AzureRMVMOSDisk -VM $vm -Name "OSDisk" -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

<span data-ttu-id="abb10-129">Připojit k počítači SQL1 pomocí portálu Azure pomocí účtu místního správce počítače SQL1.</span><span class="sxs-lookup"><span data-stu-id="abb10-129">Use the Azure portal to connect to SQL1 using the local administrator account of SQL1.</span></span>

<span data-ttu-id="abb10-130">V dalším kroku nakonfigurujte pravidla brány Windows Firewall pro umožnění testování základní připojení a přenosy dat systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="abb10-130">Next, configure Windows Firewall rules to allow basic connectivity testing and SQL Server traffic.</span></span> <span data-ttu-id="abb10-131">Z prostředí Windows PowerShell na úrovni správce příkazového řádku na počítači SQL1 spuštění těchto příkazů.</span><span class="sxs-lookup"><span data-stu-id="abb10-131">From an administrator-level Windows PowerShell command prompt on SQL1, run these commands.</span></span>

    New-NetFirewallRule -DisplayName "SQL Server" -Direction Inbound -Protocol TCP -LocalPort 1433,1434,5022 -Action allow 
    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc2.corp.contoso.com

<span data-ttu-id="abb10-132">Příkaz ping by měl mít za následek čtyři úspěšné odpovědi od IP adresa 192.168.0.4.</span><span class="sxs-lookup"><span data-stu-id="abb10-132">The ping command should result in four successful replies from IP address 192.168.0.4.</span></span>

<span data-ttu-id="abb10-133">V dalším kroku přidejte další datový disk na počítači SQL1 jako nový svazek s písmenem jednotky F:.</span><span class="sxs-lookup"><span data-stu-id="abb10-133">Next, add the extra data disk on SQL1 as a new volume with the drive letter F:.</span></span>

1. <span data-ttu-id="abb10-134">V levém podokně ve Správci serveru klikněte na tlačítko **Souborová služba a služba úložiště**a potom klikněte na **disky**.</span><span class="sxs-lookup"><span data-stu-id="abb10-134">In the left pane of Server Manager, click **File and Storage Services**, and then click **Disks**.</span></span>
2. <span data-ttu-id="abb10-135">V podokně obsahu v **disky** klikněte na možnost **disku 2** (s **oddílu** nastavena na **neznámé**).</span><span class="sxs-lookup"><span data-stu-id="abb10-135">In the contents pane, in the **Disks** group, click **disk 2** (with the **Partition** set to **Unknown**).</span></span>
3. <span data-ttu-id="abb10-136">Klikněte na tlačítko **úlohy**a potom klikněte na **nový svazek**.</span><span class="sxs-lookup"><span data-stu-id="abb10-136">Click **Tasks**, and then click **New Volume**.</span></span>
4. <span data-ttu-id="abb10-137">Na stránce než můžete začít stránky Průvodce vytvořením svazku, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="abb10-137">On the Before you begin page of the New Volume Wizard, click **Next**.</span></span>
5. <span data-ttu-id="abb10-138">Na stránce vyberte server a disk stránky, klikněte na tlačítko **disku 2**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="abb10-138">On the Select the server and disk page, click **Disk 2**, and then click **Next**.</span></span> <span data-ttu-id="abb10-139">Po zobrazení výzvy klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="abb10-139">When prompted, click **OK**.</span></span>
6. <span data-ttu-id="abb10-140">Na zadání velikosti svazku stránky, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="abb10-140">On the Specify the size of the volume page, click **Next**.</span></span>
7. <span data-ttu-id="abb10-141">Na přiřazení disku písmeno nebo složka stránku, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="abb10-141">On the Assign to a drive letter or folder page, click **Next**.</span></span>
8. <span data-ttu-id="abb10-142">Na stránce nastavení systému vybrat soubor, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="abb10-142">On the Select file system settings page, click **Next**.</span></span>
9. <span data-ttu-id="abb10-143">Na stránce Potvrdit vybrané možnosti, klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="abb10-143">On the Confirm selections page, click **Create**.</span></span>
10. <span data-ttu-id="abb10-144">Po dokončení klikněte na tlačítko **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="abb10-144">When complete, click **Close**.</span></span>

<span data-ttu-id="abb10-145">Spuštěním těchto příkazů na příkazovém řádku prostředí Windows PowerShell na počítači SQL1:</span><span class="sxs-lookup"><span data-stu-id="abb10-145">Run these commands at the Windows PowerShell command prompt on SQL1:</span></span>

    md f:\Data
    md f:\Log
    md f:\Backup

<span data-ttu-id="abb10-146">V dalším kroku připojení počítače SQL1 k doméně CORP Windows Server Active Directory pomocí těchto příkazů v příkazovém řádku Windows Powershellu na počítači SQL1.</span><span class="sxs-lookup"><span data-stu-id="abb10-146">Next, join SQL1 to the CORP Windows Server Active Directory domain with these commands at the Windows PowerShell prompt on SQL1.</span></span>

    Add-Computer -DomainName corp.contoso.com
    Restart-Computer

<span data-ttu-id="abb10-147">Použití účtu CORP\User1 po zobrazení výzvy k zadání pověření účtu domény pro **Add-Computer** příkaz.</span><span class="sxs-lookup"><span data-stu-id="abb10-147">Use the CORP\User1 account when prompted to supply domain account credentials for the **Add-Computer** command.</span></span>

<span data-ttu-id="abb10-148">Po restartování počítače, použijte portál Azure pro připojení k počítači SQL1 *pomocí účtu místního správce SQL1*.</span><span class="sxs-lookup"><span data-stu-id="abb10-148">After restarting, use the Azure portal to connect to SQL1 *with the local administrator account of SQL1*.</span></span>

<span data-ttu-id="abb10-149">V dalším kroku nakonfigurujte SQL Server 2014, aby použila jednotku F: nové databáze a oprávnění účtu uživatele.</span><span class="sxs-lookup"><span data-stu-id="abb10-149">Next, configure SQL Server 2014 to use the F: drive for new databases and for user account permissions.</span></span>

1. <span data-ttu-id="abb10-150">Na obrazovce Start zadejte **SQL Server Management**a potom klikněte na **SQL Server 2014 Management Studio**.</span><span class="sxs-lookup"><span data-stu-id="abb10-150">From the Start screen, type **SQL Server Management**, and then click **SQL Server 2014 Management Studio**.</span></span>
2. <span data-ttu-id="abb10-151">V **připojit k serveru**, klikněte na tlačítko **Connect**.</span><span class="sxs-lookup"><span data-stu-id="abb10-151">In **Connect to Server**, click **Connect**.</span></span>
3. <span data-ttu-id="abb10-152">V podokně stromu Průzkumník objektů klikněte pravým tlačítkem na **SQL1**a potom klikněte na **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="abb10-152">In the Object Explorer tree pane, right-click **SQL1**, and then click **Properties**.</span></span>
4. <span data-ttu-id="abb10-153">V **vlastnosti serveru** okně klikněte na tlačítko **nastavení databáze**.</span><span class="sxs-lookup"><span data-stu-id="abb10-153">In the **Server Properties** window, click **Database Settings**.</span></span>
5. <span data-ttu-id="abb10-154">Vyhledejte **databáze výchozí umístění** a nastavte tyto hodnoty:</span><span class="sxs-lookup"><span data-stu-id="abb10-154">Locate the **Database default locations** and set these values:</span></span> 
   * <span data-ttu-id="abb10-155">Pro **Data**, zadejte cestu **f:\Data**.</span><span class="sxs-lookup"><span data-stu-id="abb10-155">For **Data**, type the path **f:\Data**.</span></span>
   * <span data-ttu-id="abb10-156">Pro **protokolu**, zadejte cestu **f:\Log**.</span><span class="sxs-lookup"><span data-stu-id="abb10-156">For **Log**, type the path **f:\Log**.</span></span>
   * <span data-ttu-id="abb10-157">Pro **zálohování**, zadejte cestu **f:\Backup**.</span><span class="sxs-lookup"><span data-stu-id="abb10-157">For **Backup**, type the path **f:\Backup**.</span></span>
   * <span data-ttu-id="abb10-158">Poznámka: Pouze nové databáze pomocí těchto umístění.</span><span class="sxs-lookup"><span data-stu-id="abb10-158">Note: Only new databases use these locations.</span></span>
6. <span data-ttu-id="abb10-159">Klikněte **OK** zavřete toto okno.</span><span class="sxs-lookup"><span data-stu-id="abb10-159">Click the **OK** to close the window.</span></span>
7. <span data-ttu-id="abb10-160">V **Průzkumník objektů** stromové struktuře otevřete **zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="abb10-160">In the **Object Explorer** tree pane, open **Security**.</span></span>
8. <span data-ttu-id="abb10-161">Klikněte pravým tlačítkem na **přihlášení** a pak klikněte na **nového přihlašovacího jména**.</span><span class="sxs-lookup"><span data-stu-id="abb10-161">Right-click **Logins** and then click **New Login**.</span></span>
9. <span data-ttu-id="abb10-162">V **přihlašovací jméno**, typ **CORP\User1**.</span><span class="sxs-lookup"><span data-stu-id="abb10-162">In **Login name**, type **CORP\User1**.</span></span>
10. <span data-ttu-id="abb10-163">Na **role serveru** klikněte na tlačítko **sysadmin**a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="abb10-163">On the **Server Roles** page, click **sysadmin**, and then click **OK**.</span></span>
11. <span data-ttu-id="abb10-164">Zavřete Microsoft SQL Server Management Studio.</span><span class="sxs-lookup"><span data-stu-id="abb10-164">Close Microsoft SQL Server Management Studio.</span></span>

<span data-ttu-id="abb10-165">Toto je aktuální konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="abb10-165">This is your current configuration.</span></span>

![](./media/ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph2.png)

## <a name="phase-3-configure-the-lob-server-lob1"></a><span data-ttu-id="abb10-166">Fáze 3: Konfigurace serveru LOB (LOB1)</span><span class="sxs-lookup"><span data-stu-id="abb10-166">Phase 3: Configure the LOB server (LOB1)</span></span>
<span data-ttu-id="abb10-167">Nejdřív vytvořte virtuální počítač pro LOB1 pomocí těchto příkazů na příkazovém řádku prostředí Azure PowerShell v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="abb10-167">First, create a virtual machine for LOB1 with these commands at the Azure PowerShell command prompt on your local computer.</span></span>

    $rgName="<your resource group name>"
    $locName="<your Azure location, such as West US>"
    $saName="<your storage account name>"

    $vnet=Get-AzureRMVirtualNetwork -Name "TestVNET" -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "TestSubnet"
    $pip=New-AzureRMPublicIpAddress -Name LOB1-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $nic=New-AzureRMNetworkInterface -Name LOB1-NIC -ResourceGroupName $rgName -Location $locName -Subnet $subnet -PublicIpAddress $pip
    $vm=New-AzureRMVMConfig -VMName LOB1 -VMSize Standard_A2
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $cred=Get-Credential -Message "Type the name and password of the local administrator account for LOB1."
    $vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName LOB1 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/LOB1-TestLab-OSDisk.vhd"
    $vm=Set-AzureRMVMOSDisk -VM $vm -Name LOB1-TestVNET-OSDisk -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

<span data-ttu-id="abb10-168">V dalším kroku se připojit k LOB1 pomocí přihlašovacích údajů účtu místního správce LOB1 pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="abb10-168">Next, use the Azure portal to connect to LOB1 with the credentials of the local administrator account of LOB1.</span></span>

<span data-ttu-id="abb10-169">V dalším kroku nakonfigurujte brány Windows Firewall pravidlo umožňující přenos pro testování základní připojení.</span><span class="sxs-lookup"><span data-stu-id="abb10-169">Next, configure a Windows Firewall rule to allow traffic for basic connectivity testing.</span></span> <span data-ttu-id="abb10-170">Z prostředí Windows PowerShell na úrovni správce příkazového řádku na LOB1 spuštění těchto příkazů.</span><span class="sxs-lookup"><span data-stu-id="abb10-170">From an administrator-level Windows PowerShell command prompt on LOB1, run these commands.</span></span>

    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc2.corp.contoso.com

<span data-ttu-id="abb10-171">Příkaz ping by měl mít za následek čtyři úspěšné odpovědi od IP adresa 192.168.0.4.</span><span class="sxs-lookup"><span data-stu-id="abb10-171">The ping command should result in four successful replies from IP address 192.168.0.4.</span></span>

<span data-ttu-id="abb10-172">V dalším kroku LOB1 připojení k doméně CORP Active Directory s těmito příkazy v příkazovém řádku Windows Powershellu.</span><span class="sxs-lookup"><span data-stu-id="abb10-172">Next, join LOB1 to the CORP Active Directory domain with these commands at the Windows PowerShell prompt.</span></span>

    Add-Computer -DomainName corp.contoso.com
    Restart-Computer

<span data-ttu-id="abb10-173">Použití účtu CORP\User1 po zobrazení výzvy k zadání pověření účtu domény pro **Add-Computer** příkaz.</span><span class="sxs-lookup"><span data-stu-id="abb10-173">Use the CORP\User1 account when prompted to supply domain account credentials for the **Add-Computer** command.</span></span>

<span data-ttu-id="abb10-174">Po restartování počítače, použijte portál Azure pro připojení k LOB1 s účtem CORP\User1 a heslo.</span><span class="sxs-lookup"><span data-stu-id="abb10-174">After restarting, use the Azure portal to connect to LOB1 with the CORP\User1 account and password.</span></span>

<span data-ttu-id="abb10-175">V dalším kroku konfigurace LOB1 pro službu IIS a otestování přístup z počítače CLIENT1.</span><span class="sxs-lookup"><span data-stu-id="abb10-175">Next, configure LOB1 for IIS and test access from CLIENT1.</span></span>

1. <span data-ttu-id="abb10-176">Ve Správci serveru klikněte na tlačítko **přidat role a funkce**.</span><span class="sxs-lookup"><span data-stu-id="abb10-176">From Server Manager, click **Add roles and features**.</span></span>
2. <span data-ttu-id="abb10-177">Na **před zahájením** klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="abb10-177">On the **Before you begin** page, click **Next**.</span></span>
3. <span data-ttu-id="abb10-178">Na **vybrat typ instalace** klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="abb10-178">On the **Select installation type** page, click **Next**.</span></span>
4. <span data-ttu-id="abb10-179">Na **vybrat cílový server** klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="abb10-179">On the **Select destination server** page, click **Next**.</span></span>
5. <span data-ttu-id="abb10-180">Na **role serveru** klikněte na tlačítko **webového serveru (IIS)** v seznamu **role**.</span><span class="sxs-lookup"><span data-stu-id="abb10-180">On the **Server roles** page, click **Web Server (IIS)** in the list of **Roles**.</span></span>
6. <span data-ttu-id="abb10-181">Po zobrazení výzvy klikněte na tlačítko **přidat funkce**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="abb10-181">When prompted, click **Add Features**, and then click **Next**.</span></span>
7. <span data-ttu-id="abb10-182">Na **vyberte funkce** klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="abb10-182">On the **Select features** page, click **Next**.</span></span>
8. <span data-ttu-id="abb10-183">Na **webového serveru (IIS)** klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="abb10-183">On the **Web Server (IIS)** page, click **Next**.</span></span>
9. <span data-ttu-id="abb10-184">Na **vybrat služby rolí** vyberte nebo zrušte zaškrtnutí políček u služeb, je nutné pro testování vašich obchodních aplikací a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="abb10-184">On the **Select role services** page, select or clear the check boxes for the services you need for testing your LOB application, and then click **Next**.</span></span>
10. <span data-ttu-id="abb10-185">Na **potvrdit vybrané možnosti instalace** klikněte na tlačítko **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="abb10-185">On the **Confirm installation selections** page, click **Install**.</span></span>
11. <span data-ttu-id="abb10-186">Počkejte na dokončení instalace komponent a pak klikněte na **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="abb10-186">Wait until the installation of components has completed and then click **Close**.</span></span>
12. <span data-ttu-id="abb10-187">Z portálu Azure připojit k počítači CLIENT1 pomocí pověření účtu CORP\User1 a pak spusťte aplikaci Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="abb10-187">From the Azure portal, connect to the CLIENT1 computer with the CORP\User1 account credentials, and then start Internet Explorer.</span></span>
13. <span data-ttu-id="abb10-188">Na panelu Adresa zadejte **http://lob1/** a stiskněte klávesu ENTER.</span><span class="sxs-lookup"><span data-stu-id="abb10-188">In the Address bar, type **http://lob1/** and then press ENTER.</span></span> <span data-ttu-id="abb10-189">Měli byste vidět výchozí webové stránky IIS 8.</span><span class="sxs-lookup"><span data-stu-id="abb10-189">You should see the default IIS 8 web page.</span></span>

<span data-ttu-id="abb10-190">Toto je aktuální konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="abb10-190">This is your current configuration.</span></span>

![](./media/ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph3.png)

<span data-ttu-id="abb10-191">Toto prostředí je nyní připraven k nasazení vaší webové aplikace na LOB1 a otestovat funkce z počítače CLIENT1 na podsíti Corpnet.</span><span class="sxs-lookup"><span data-stu-id="abb10-191">This environment is now ready for you to deploy your web-based application on LOB1 and test functionality from CLIENT1 on the Corpnet subnet.</span></span>

## <a name="next-step"></a><span data-ttu-id="abb10-192">Další krok</span><span class="sxs-lookup"><span data-stu-id="abb10-192">Next step</span></span>
* <span data-ttu-id="abb10-193">Přidat nový virtuální počítač pomocí [portál Azure](../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="abb10-193">Add a new virtual machine using the [Azure portal](../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
