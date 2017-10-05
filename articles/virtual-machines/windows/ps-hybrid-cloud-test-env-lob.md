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
# <a name="set-up-a-web-based-lob-application-in-a-hybrid-cloud-for-testing"></a>Nastavení webové aplikace LOB v hybridním cloudu pro testování
Toto téma vás provede jednotlivými kroky vytváření simulované hybridní cloudové prostředí pro testování webových řádek obchodní (LOB) aplikace hostované ve službě Microsoft Azure. Zde je Výsledná konfigurace.

![](./media/ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph3.png)

Tato konfigurace zahrnuje:

* Simulované místní síti hostované v Azure (VNet testovací laboratoř).
* Mezi různými místy virtuální síť hostované v Azure (TestVNET).
* Připojení k síti VPN VNet-to-VNet.
* Webové LOB serveru, systému SQL server a sekundární řadič domény ve virtuální síti TestVNET.

Tato konfigurace poskytuje základ a běžné počáteční bod, ze kterého můžete:

* Vývoj a testování obchodních aplikací, které jsou hostované v Internetové informační služby (IIS) u back-end databáze SQL Server 2014 v Azure.
* Proveďte testování této simulované hybridní cloudové IT úlohy.

Existují tři hlavní fáze k nastavení tohoto testovacího prostředí hybridního cloudu:

1. Nastavení prostředí simulované hybridní cloud.
2. Nakonfigurujte počítač serveru SQL (SQL1).
3. Nakonfigurujte server LOB (LOB1).

Tato úloha vyžaduje předplatné Azure. Pokud máte předplatné MSDN nebo v sadě Visual Studio, najdete v části [platební měsíční Azure pro předplatitele sady Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).

Příklad produkční aplikace LOB, které jsou hostované v Azure, naleznete v části **obchodních aplikací** architektura plán, podle kterého na [diagramy architektura softwaru společnosti Microsoft a plány vyfotografovat](http://msdn.microsoft.com/dn630664).

## <a name="phase-1-set-up-the-simulated-hybrid-cloud-environment"></a>Fáze 1: Nastavení simulované hybridní cloudové prostředí
Vytvořte [simulované hybridní cloud testovací prostředí](ps-hybrid-cloud-test-env-sim.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Protože toto testovací prostředí nevyžaduje přítomnost serveru APP1 na podsíti Corpnet, můžete ho vypnout teď.

Toto je aktuální konfiguraci.

![](./media/ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph1.png)

## <a name="phase-2-configure-the-sql-server-computer-sql1"></a>Fáze 2: Konfigurace počítače serveru SQL (SQL1)
Z portálu Azure spusťte počítač DC2, v případě potřeby.

Dále vytvořte virtuální počítač pro počítač SQL1 pomocí těchto příkazů na příkazovém řádku prostředí Azure PowerShell v místním počítači. Před spuštěním těchto příkazů, vyplňte hodnoty proměnné a odebrat < a > znaků.

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

Připojit k počítači SQL1 pomocí portálu Azure pomocí účtu místního správce počítače SQL1.

V dalším kroku nakonfigurujte pravidla brány Windows Firewall pro umožnění testování základní připojení a přenosy dat systému SQL Server. Z prostředí Windows PowerShell na úrovni správce příkazového řádku na počítači SQL1 spuštění těchto příkazů.

    New-NetFirewallRule -DisplayName "SQL Server" -Direction Inbound -Protocol TCP -LocalPort 1433,1434,5022 -Action allow 
    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc2.corp.contoso.com

Příkaz ping by měl mít za následek čtyři úspěšné odpovědi od IP adresa 192.168.0.4.

V dalším kroku přidejte další datový disk na počítači SQL1 jako nový svazek s písmenem jednotky F:.

1. V levém podokně ve Správci serveru klikněte na tlačítko **Souborová služba a služba úložiště**a potom klikněte na **disky**.
2. V podokně obsahu v **disky** klikněte na možnost **disku 2** (s **oddílu** nastavena na **neznámé**).
3. Klikněte na tlačítko **úlohy**a potom klikněte na **nový svazek**.
4. Na stránce než můžete začít stránky Průvodce vytvořením svazku, klikněte na tlačítko **Další**.
5. Na stránce vyberte server a disk stránky, klikněte na tlačítko **disku 2**a potom klikněte na **Další**. Po zobrazení výzvy klikněte na tlačítko **OK**.
6. Na zadání velikosti svazku stránky, klikněte na tlačítko **Další**.
7. Na přiřazení disku písmeno nebo složka stránku, klikněte na tlačítko **Další**.
8. Na stránce nastavení systému vybrat soubor, klikněte na tlačítko **Další**.
9. Na stránce Potvrdit vybrané možnosti, klikněte na tlačítko **vytvořit**.
10. Po dokončení klikněte na tlačítko **Zavřít**.

Spuštěním těchto příkazů na příkazovém řádku prostředí Windows PowerShell na počítači SQL1:

    md f:\Data
    md f:\Log
    md f:\Backup

V dalším kroku připojení počítače SQL1 k doméně CORP Windows Server Active Directory pomocí těchto příkazů v příkazovém řádku Windows Powershellu na počítači SQL1.

    Add-Computer -DomainName corp.contoso.com
    Restart-Computer

Použití účtu CORP\User1 po zobrazení výzvy k zadání pověření účtu domény pro **Add-Computer** příkaz.

Po restartování počítače, použijte portál Azure pro připojení k počítači SQL1 *pomocí účtu místního správce SQL1*.

V dalším kroku nakonfigurujte SQL Server 2014, aby použila jednotku F: nové databáze a oprávnění účtu uživatele.

1. Na obrazovce Start zadejte **SQL Server Management**a potom klikněte na **SQL Server 2014 Management Studio**.
2. V **připojit k serveru**, klikněte na tlačítko **Connect**.
3. V podokně stromu Průzkumník objektů klikněte pravým tlačítkem na **SQL1**a potom klikněte na **vlastnosti**.
4. V **vlastnosti serveru** okně klikněte na tlačítko **nastavení databáze**.
5. Vyhledejte **databáze výchozí umístění** a nastavte tyto hodnoty: 
   * Pro **Data**, zadejte cestu **f:\Data**.
   * Pro **protokolu**, zadejte cestu **f:\Log**.
   * Pro **zálohování**, zadejte cestu **f:\Backup**.
   * Poznámka: Pouze nové databáze pomocí těchto umístění.
6. Klikněte **OK** zavřete toto okno.
7. V **Průzkumník objektů** stromové struktuře otevřete **zabezpečení**.
8. Klikněte pravým tlačítkem na **přihlášení** a pak klikněte na **nového přihlašovacího jména**.
9. V **přihlašovací jméno**, typ **CORP\User1**.
10. Na **role serveru** klikněte na tlačítko **sysadmin**a potom klikněte na **OK**.
11. Zavřete Microsoft SQL Server Management Studio.

Toto je aktuální konfiguraci.

![](./media/ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph2.png)

## <a name="phase-3-configure-the-lob-server-lob1"></a>Fáze 3: Konfigurace serveru LOB (LOB1)
Nejdřív vytvořte virtuální počítač pro LOB1 pomocí těchto příkazů na příkazovém řádku prostředí Azure PowerShell v místním počítači.

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

V dalším kroku se připojit k LOB1 pomocí přihlašovacích údajů účtu místního správce LOB1 pomocí portálu Azure.

V dalším kroku nakonfigurujte brány Windows Firewall pravidlo umožňující přenos pro testování základní připojení. Z prostředí Windows PowerShell na úrovni správce příkazového řádku na LOB1 spuštění těchto příkazů.

    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc2.corp.contoso.com

Příkaz ping by měl mít za následek čtyři úspěšné odpovědi od IP adresa 192.168.0.4.

V dalším kroku LOB1 připojení k doméně CORP Active Directory s těmito příkazy v příkazovém řádku Windows Powershellu.

    Add-Computer -DomainName corp.contoso.com
    Restart-Computer

Použití účtu CORP\User1 po zobrazení výzvy k zadání pověření účtu domény pro **Add-Computer** příkaz.

Po restartování počítače, použijte portál Azure pro připojení k LOB1 s účtem CORP\User1 a heslo.

V dalším kroku konfigurace LOB1 pro službu IIS a otestování přístup z počítače CLIENT1.

1. Ve Správci serveru klikněte na tlačítko **přidat role a funkce**.
2. Na **před zahájením** klikněte na tlačítko **Další**.
3. Na **vybrat typ instalace** klikněte na tlačítko **Další**.
4. Na **vybrat cílový server** klikněte na tlačítko **Další**.
5. Na **role serveru** klikněte na tlačítko **webového serveru (IIS)** v seznamu **role**.
6. Po zobrazení výzvy klikněte na tlačítko **přidat funkce**a potom klikněte na **Další**.
7. Na **vyberte funkce** klikněte na tlačítko **Další**.
8. Na **webového serveru (IIS)** klikněte na tlačítko **Další**.
9. Na **vybrat služby rolí** vyberte nebo zrušte zaškrtnutí políček u služeb, je nutné pro testování vašich obchodních aplikací a pak klikněte na tlačítko **Další**.
10. Na **potvrdit vybrané možnosti instalace** klikněte na tlačítko **nainstalovat**.
11. Počkejte na dokončení instalace komponent a pak klikněte na **Zavřít**.
12. Z portálu Azure připojit k počítači CLIENT1 pomocí pověření účtu CORP\User1 a pak spusťte aplikaci Internet Explorer.
13. Na panelu Adresa zadejte **http://lob1/** a stiskněte klávesu ENTER. Měli byste vidět výchozí webové stránky IIS 8.

Toto je aktuální konfiguraci.

![](./media/ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph3.png)

Toto prostředí je nyní připraven k nasazení vaší webové aplikace na LOB1 a otestovat funkce z počítače CLIENT1 na podsíti Corpnet.

## <a name="next-step"></a>Další krok
* Přidat nový virtuální počítač pomocí [portál Azure](../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

