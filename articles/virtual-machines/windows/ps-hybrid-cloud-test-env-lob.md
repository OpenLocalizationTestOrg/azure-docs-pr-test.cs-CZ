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
# <a name="set-up-a-web-based-lob-application-in-a-hybrid-cloud-for-testing"></a>Nastavení webové aplikace LOB v hybridním cloudu pro testování
Toto téma vás provede jednotlivými kroky vytváření simulované hybridní cloudové prostředí pro testování webových řádek obchodní (LOB) aplikace hostované ve službě Microsoft Azure. Zde je hello Výsledná konfigurace.

![](./media/ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph3.png)

Tato konfigurace zahrnuje:

* Simulované místní síti hostované v Azure (hello testovací laboratoř virtuální sítě).
* Mezi různými místy virtuální síť hostované v Azure (TestVNET).
* Připojení k síti VPN VNet-to-VNet.
* Webové LOB serveru, systému SQL server a sekundární řadič domény ve virtuální síti TestVNET hello.

Tato konfigurace poskytuje základ a běžné počáteční bod, ze kterého můžete:

* Vývoj a testování obchodních aplikací, které jsou hostované v Internetové informační služby (IIS) u back-end databáze SQL Server 2014 v Azure.
* Proveďte testování této simulované hybridní cloudové IT úlohy.

Existují tři hlavní fáze toosetting toto hybridní cloud testovací prostředí:

1. Nastavte hello simulované hybridní cloudové prostředí.
2. Nakonfigurujte hello počítač serveru SQL (SQL1).
3. Nakonfigurujte server hello LOB (LOB1).

Tato úloha vyžaduje předplatné Azure. Pokud máte předplatné MSDN nebo v sadě Visual Studio, najdete v části [platební měsíční Azure pro předplatitele sady Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).

Příklad produkční aplikace LOB, které jsou hostované v Azure, naleznete v části hello **obchodních aplikací** architektura plán, podle kterého na [diagramy architektura softwaru společnosti Microsoft a plány vyfotografovat](http://msdn.microsoft.com/dn630664).

## <a name="phase-1-set-up-hello-simulated-hybrid-cloud-environment"></a>Fáze 1: Nastavení hello simulované hybridní cloudové prostředí
Vytvoření hello [simulované hybridní cloud testovací prostředí](ps-hybrid-cloud-test-env-sim.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Protože toto testovací prostředí nevyžaduje hello přítomnost server hello APP1 na podsíti Corpnet hello, můžete ho vypnout teď.

Toto je aktuální konfiguraci.

![](./media/ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph1.png)

## <a name="phase-2-configure-hello-sql-server-computer-sql1"></a>Fáze 2: Konfigurace hello počítač serveru SQL (SQL1)
Z hello portálu Azure spusťte počítač DC2 hello v případě potřeby.

Dále vytvořte virtuální počítač pro počítač SQL1 pomocí těchto příkazů na příkazovém řádku prostředí Azure PowerShell v místním počítači. Předchozí toorunning tyto příkazy vyplnit hello hodnoty proměnných a odebrat hello < a > znaků.

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

Použijte hello Azure portálu tooconnect tooSQL1 pomocí účtu místního správce hello SQL1.

V dalším kroku nakonfigurujte testování základní konektivity tooallow pravidla brány Windows Firewall a přenosy dat systému SQL Server. Z prostředí Windows PowerShell na úrovni správce příkazového řádku na počítači SQL1 spuštění těchto příkazů.

    New-NetFirewallRule -DisplayName "SQL Server" -Direction Inbound -Protocol TCP -LocalPort 1433,1434,5022 -Action allow 
    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc2.corp.contoso.com

příkaz ping Hello by měl mít za následek čtyři úspěšné odpovědi od IP adresa 192.168.0.4.

Dál přidejte hello navíc datový disk na počítači SQL1 jako nový svazek s písmenem jednotky hello F:.

1. V levém podokně hello správce serveru, klikněte na **Souborová služba a služba úložiště**a potom klikněte na **disky**.
2. V podokně obsahu hello v hello **disky** klikněte na možnost **disku 2** (s hello **oddílu** nastavit příliš**neznámé**).
3. Klikněte na tlačítko **úlohy**a potom klikněte na **nový svazek**.
4. Na hello před zahájením stránku hello Průvodce vytvořením svazku, klikněte na tlačítko **Další**.
5. Na serveru vyberte hello hello a stránka disku, klikněte na tlačítko **disku 2**a potom klikněte na **Další**. Po zobrazení výzvy klikněte na tlačítko **OK**.
6. Na hello zadat velikost hello hello svazku stránky, klikněte na tlačítko **Další**.
7. Na hello přiřazovat tooa jednotky písmeno nebo složka stránky, klikněte na tlačítko **Další**.
8. Na stránce nastavení systému hello vybrat soubor, klikněte na tlačítko **Další**.
9. Na stránce Potvrdit vybrané možnosti hello klikněte **vytvořit**.
10. Po dokončení klikněte na tlačítko **Zavřít**.

Spuštěním těchto příkazů na příkazovém řádku prostředí Windows PowerShell hello na počítači SQL1:

    md f:\Data
    md f:\Log
    md f:\Backup

V dalším kroku připojit k doméně CORP Windows Server Active Directory toohello SQL1 s těmito příkazy příkazového řádku Windows Powershellu hello na počítači SQL1.

    Add-Computer -DomainName corp.contoso.com
    Restart-Computer

Použít účet hello CORP\User1 po zobrazení výzvy pověření účtu domény toosupply pro hello **Add-Computer** příkaz.

Po restartování počítače, použijte hello Azure portálu tooconnect tooSQL1 *pomocí účtu místního správce hello SQL1*.

V dalším kroku nakonfigurujte SQL Server 2014 toouse hello F: disku pro nové databáze a oprávnění účtu uživatele.

1. Na úvodní obrazovce hello zadejte **SQL Server Management**a potom klikněte na **SQL Server 2014 Management Studio**.
2. V **připojit tooServer**, klikněte na tlačítko **Connect**.
3. V podokně stromu hello Průzkumník objektů klikněte pravým tlačítkem na **SQL1**a potom klikněte na **vlastnosti**.
4. V hello **vlastnosti serveru** okně klikněte na tlačítko **nastavení databáze**.
5. Vyhledejte hello **databáze výchozí umístění** a nastavte tyto hodnoty: 
   * Pro **Data**, zadejte cestu hello **f:\Data**.
   * Pro **protokolu**, zadejte cestu hello **f:\Log**.
   * Pro **zálohování**, zadejte cestu hello **f:\Backup**.
   * Poznámka: Pouze nové databáze pomocí těchto umístění.
6. Klikněte na tlačítko hello **OK** tooclose hello okno.
7. V hello **Průzkumník objektů** stromové struktuře otevřete **zabezpečení**.
8. Klikněte pravým tlačítkem na **přihlášení** a pak klikněte na **nového přihlašovacího jména**.
9. V **přihlašovací jméno**, typ **CORP\User1**.
10. Na hello **role serveru** klikněte na tlačítko **sysadmin**a potom klikněte na **OK**.
11. Zavřete Microsoft SQL Server Management Studio.

Toto je aktuální konfiguraci.

![](./media/ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph2.png)

## <a name="phase-3-configure-hello-lob-server-lob1"></a>Fáze 3: Konfigurace serveru LOB hello (LOB1)
Nejdřív vytvořte virtuální počítač pro LOB1 pomocí těchto příkazů na příkazovém řádku prostředí Azure PowerShell hello v místním počítači.

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

Pak pomocí portálu Azure tooconnect tooLOB1 hello s přihlašovacími údaji hello účtu místního správce hello LOB1.

V dalším kroku nakonfigurujte přenosem tooallow pravidla brány Windows Firewall pro testování základní připojení. Z prostředí Windows PowerShell na úrovni správce příkazového řádku na LOB1 spuštění těchto příkazů.

    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc2.corp.contoso.com

příkaz ping Hello by měl mít za následek čtyři úspěšné odpovědi od IP adresa 192.168.0.4.

V dalším kroku připojit k doméně služby Active Directory CORP toohello LOB1 s těmito příkazy v příkazovém řádku prostředí Windows PowerShell hello.

    Add-Computer -DomainName corp.contoso.com
    Restart-Computer

Použít účet hello CORP\User1 po zobrazení výzvy pověření účtu domény toosupply pro hello **Add-Computer** příkaz.

Po restartování počítače se pomocí portálu Azure tooconnect tooLOB1 hello hello CORP\User1 účet a heslo.

V dalším kroku konfigurace LOB1 pro službu IIS a otestování přístup z počítače CLIENT1.

1. Ve Správci serveru klikněte na tlačítko **přidat role a funkce**.
2. Na hello **před zahájením** klikněte na tlačítko **Další**.
3. Na hello **vybrat typ instalace** klikněte na tlačítko **Další**.
4. Na hello **vybrat cílový server** klikněte na tlačítko **Další**.
5. Na hello **role serveru** klikněte na tlačítko **webového serveru (IIS)** v seznamu hello **role**.
6. Po zobrazení výzvy klikněte na tlačítko **přidat funkce**a potom klikněte na **Další**.
7. Na hello **vyberte funkce** klikněte na tlačítko **Další**.
8. Na hello **webového serveru (IIS)** klikněte na tlačítko **Další**.
9. Na hello **vybrat služby rolí** vyberte nebo zrušte zaškrtnutí políček hello hello služeb, je nutné pro testování vašich obchodních aplikací a pak klikněte na tlačítko **Další**.
10. Na hello **potvrdit vybrané možnosti instalace** klikněte na tlačítko **nainstalovat**.
11. Počkejte, dokud hello instalace součástí byla dokončena a pak klikněte na tlačítko **Zavřít**.
12. Z hello portálu Azure připojte počítač CLIENT1 toohello pomocí pověření účtu CORP\User1 hello a pak spusťte aplikaci Internet Explorer.
13. V panelu Adresa hello, zadejte **http://lob1/** a stiskněte klávesu ENTER. Měli byste vidět hello výchozí IIS 8 webové stránky.

Toto je aktuální konfiguraci.

![](./media/ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph3.png)

Toto prostředí je teď připravené pro jste toodeploy webové aplikace na LOB1 a testovací funkce počítači CLIENT1 na podsíti Corpnet hello.

## <a name="next-step"></a>Další krok
* Přidat nový virtuální počítač pomocí hello [portál Azure](../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

