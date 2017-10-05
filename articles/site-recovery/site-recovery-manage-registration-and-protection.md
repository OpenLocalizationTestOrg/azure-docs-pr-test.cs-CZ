---
title: "Odeberte servery a zakažte | Microsoft Docs"
description: "Tento článek popisuje, jak zrušit registraci serveru z trezoru Site Recovery a zakažte ochranu pro virtuální počítače a fyzické servery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: cfreeman
editor: 
ms.assetid: ef1f31d5-285b-4a0f-89b5-0123cd422d80
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 03/27/2017
ms.author: raynew
ms.openlocfilehash: 43f92a35dc9b04584badd1c9f1152470246b5012
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="remove-servers-and-disable-protection"></a>Odebrání serverů a zakázání ochrany

Služba Azure Site Recovery přispívá ke strategii obchodní kontinuitu a po havárii (BCDR) obnovení. Služba orchestruje replikaci, převzetí služeb při selhání a obnovení virtuálních počítačů a fyzických serverů. Počítače je možné replikovat do Azure nebo do sekundárního místního datového centra. Rychlý přehled najdete v článku [Co je Azure Site Recovery?](site-recovery-overview.md)

Tento článek popisuje, jak zrušit registraci serveru z trezoru služeb zotavení na portálu Azure a jak zakázat ochranu pro počítače chráněné službou Site Recovery.

Jakékoli dotazy nebo připomínky můžete publikovat na konci tohoto článku nebo na [fóru služby Azure Site Recovery](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="unregister-a-connected-configuration-server"></a>Zrušit registraci připojené konfigurační server

Pokud budete replikovat virtuální počítače VMware nebo fyzických serverů Windows nebo Linuxem do Azure, můžete zrušit registraci připojené konfigurační server z trezoru následujícím způsobem:

1. Zakažte ochranu počítače. V **chráněné položky** > **replikované položky**, pravým tlačítkem na počítač > **odstranit**.
2. Zrušit přidružení žádné zásady. V **infrastruktura Site Recovery** > **pro VMWare a fyzické počítače** > **zásady replikace**, dvakrát klikněte na panel s přiřazenou třídou zásady. Klikněte pravým tlačítkem na konfigurační server > **zrušení spojení**.
3. Odeberte všechny další místní proces a hlavních cílových serverů. V **infrastruktura Site Recovery** > **pro VMWare a fyzické počítače** > **konfigurační servery**, klikněte pravým tlačítkem na serveru > **Odstranit**.
4. Odstraňte konfigurační server.
5. Ručně odinstalujte službu Mobility na hlavním cílovém serveru spuštěna (bude jím buď samostatný server, nebo spuštěné na konfiguračním serveru).
6. Odinstalujte všechny servery další proces.
7. Odinstalujte konfigurační server.
8. Na konfiguračním serveru odinstalujte instanci databáze MySQL, která byla nainstalována pomocí Site Recovery.
9. V registru konfigurační server odstranit klíč ``HKEY_LOCAL_MACHINE\Software\Microsoft\Azure Site Recovery``.

## <a name="unregister-a-unconnected-configuration-server"></a>Zrušit registraci serveru bez připojení konfigurace

Pokud budete replikovat virtuální počítače VMware nebo fyzických serverů Windows nebo Linuxem do Azure, můžete zrušit registraci serveru bez připojení konfiguraci z úložiště následujícím způsobem:

1. Zakažte ochranu počítače. V **chráněné položky** > **replikované položky**, pravým tlačítkem na počítač > **odstranit**. Vyberte **přestat spravovat počítač**.
2. Odeberte všechny další místní proces a hlavních cílových serverů. V **infrastruktura Site Recovery** > **pro VMWare a fyzické počítače** > **konfigurační servery**, klikněte pravým tlačítkem na serveru > **Odstranit**.
3. Odstraňte konfigurační server.
4. Ručně odinstalujte službu Mobility na hlavním cílovém serveru spuštěna (bude jím buď samostatný server, nebo spuštěné na konfiguračním serveru).
5. Odinstalujte všechny servery další proces.
6. Odinstalujte konfigurační server.
7. Na konfiguračním serveru odinstalujte instanci databáze MySQL, která byla nainstalována pomocí Site Recovery.
8. V registru konfigurační server odstranit klíč ``HKEY_LOCAL_MACHINE\Software\Microsoft\Azure Site Recovery``.

## <a name="unregister-a-connected-vmm-server"></a>Zrušení registrace připojený k serveru VMM

Jako osvědčený postup doporučujeme zrušit registraci serveru VMM, pokud je připojený k Azure. Tím se zajistí, že jsou správně vyčistit nastavení na serveru VMM (a na jiných serverech VMM s spárované cloudy). Bez připojení serveru byste měli odebrat, pouze pokud je trvalé potíže s připojením. Pokud VMM server není připojený, musíte ručně spustit skript k vyčištění nastavení.

1. Zastavení replikace virtuálních počítačů v cloudu na VMM serveru, který chcete odebrat.
2. Odstraňte veškerá jeho mapování sítě používá cloudy na serveru VMM, který chcete odstranit. V **infrastruktura Site Recovery** > **pro System Center VMM** > **mapování sítě**, klikněte pravým tlačítkem na mapování sítě > **Odstranit**.
3. Zrušit přidružení zásad replikace z cloudy na serveru VMM, který chcete odebrat.  V **infrastruktura Site Recovery** > **pro System Center VMM** >  **zásady replikace**, dvakrát klikněte na přidružené zásady. Klikněte pravým tlačítkem na cloudu > **zrušení spojení**.
4. Odstraňte VMM server nebo aktivním uzlu VMM. V **infrastruktura Site Recovery** > **pro System Center VMM** > **servery VMM**, pravým tlačítkem na server > **odstranit** .
5. Zprostředkovatel na serveru VMM ručně odinstalujte. Pokud máte cluster, odeberte ze všech uzlů.
6. Pokud replikujete do Azure, ručně odeberte agenta služeb zotavení Microsoft z hostitelů Hyper-V v cloudech odstraněné.



### <a name="unregister-an-unconnected-vmm-server"></a>Zrušit registraci serveru VMM bez připojení

1. Zastavení replikace virtuálních počítačů v cloudu na VMM serveru, který chcete odebrat.
2. Odstraňte veškerá jeho mapování sítě používá cloudy na serveru VMM, který chcete odstranit. V **infrastruktura Site Recovery** > **pro System Center VMM** > **mapování sítě**, klikněte pravým tlačítkem na mapování sítě > **Odstranit**.
3. Poznamenejte si ID serveru VMM.
4. Zrušit přidružení zásad replikace z cloudy na serveru VMM, který chcete odebrat.  V **infrastruktura Site Recovery** > **pro System Center VMM** >  **zásady replikace**, dvakrát klikněte na přidružené zásady. Klikněte pravým tlačítkem na cloudu > **zrušení spojení**.
5. Odstraňte VMM server nebo aktivní uzel. V **infrastruktura Site Recovery** > **pro System Center VMM** > **servery VMM**, pravým tlačítkem na server > **odstranit** .
6. Stažení a spuštění [skript pro vyčištění](http://aka.ms/asr-cleanup-script-vmm) na serveru VMM. Otevřete prostředí PowerShell se **spustit jako správce** možnost, chcete-li změnit zásady spouštění pro obor výchozí (LocalMachine). Ve skriptu zadejte ID serveru VMM, který chcete odebrat. Skript odebere registrace a cloudového párování informací ze serveru.
5. Spusťte skript vyčištění na jiných serverech VMM, které obsahují cloudy, které jsou spárovaný s cloudy na serveru VMM, který chcete odebrat.
6. Spusťte skript vyčištění na žádné jiné pasivní uzly clusteru VMM které mají nainstalovaného zprostředkovatele.
7. Zprostředkovatel na serveru VMM ručně odinstalujte. Pokud máte cluster, odeberte ze všech uzlů.
8. Pokud jste replikaci do Azure, můžete odebrat agenta služeb zotavení Microsoft z hostitelů Hyper-V v cloudech odstraněné.

## <a name="unregister-a-hyper-v-host-in-a-hyper-v-site"></a>Zrušit registraci hostitele Hyper-V v lokalitě Hyper-V

Hostitelé Hyper-V, které nejsou spravované přes VMM jsou shromážděná do lokality Hyper-V. Odeberte hostitele v lokalitě Hyper-V následujícím způsobem:

1. Zakážete replikaci pro virtuální počítače Hyper-V umístěný na hostiteli.
2. Zrušit přidružení zásad pro web Hyper-V. V **infrastruktura Site Recovery** > **pro weby technologie Hyper-V** >  **zásady replikace**, dvakrát klikněte na přidružené zásady. Klikněte pravým tlačítkem na webu > **zrušení spojení**.
3. Odstraňte hostitele Hyper-V. V **infrastruktura Site Recovery** > **pro System Center VMM** > **hostitelů Hyper-V**, pravým tlačítkem na server >  **Odstranit**.
4. Po odebrání všech hostitelů z něj, odstraníte web Hyper-V. V **infrastruktura Site Recovery** > **pro System Center VMM** > **lokalit Hyper-V**, klikněte pravým tlačítkem na webu > **odstranit** .
5. Spusťte následující skript na každém hostiteli technologie Hyper-V, který jste odebrali. Skript vyčistí nastavení na serveru a zruší registraci v trezoru.


        `` pushd .
        try
        {
             $windowsIdentity=[System.Security.Principal.WindowsIdentity]::GetCurrent()
             $principal=new-object System.Security.Principal.WindowsPrincipal($windowsIdentity)
             $administrators=[System.Security.Principal.WindowsBuiltInRole]::Administrator
             $isAdmin=$principal.IsInRole($administrators)
             if (!$isAdmin)
             {
                "Please run the script as an administrator in elevated mode."
                $choice = Read-Host
                return;       
             }

            $error.Clear()    
            "This script will remove the old Azure Site Recovery Provider related properties. Do you want to continue (Y/N) ?"
            $choice =  Read-Host

            if (!($choice -eq 'Y' -or $choice -eq 'y'))
            {
            "Stopping cleanup."
            return;
            }

            $serviceName = "dra"
            $service = Get-Service -Name $serviceName
            if ($service.Status -eq "Running")
            {
                "Stopping the Azure Site Recovery service..."
                net stop $serviceName
            }

            $asrHivePath = "HKLM:\SOFTWARE\Microsoft\Azure Site Recovery"
            $registrationPath = $asrHivePath + '\Registration'
            $proxySettingsPath = $asrHivePath + '\ProxySettings'
            $draIdvalue = 'DraID'

            if (Test-Path $asrHivePath)
            {
                if (Test-Path $registrationPath)
                {
                    "Removing registration related registry keys."    
                    Remove-Item -Recurse -Path $registrationPath
                }

                if (Test-Path $proxySettingsPath)
            {
                    "Removing proxy settings"
                    Remove-Item -Recurse -Path $proxySettingsPath
                }

                $regNode = Get-ItemProperty -Path $asrHivePath
                if($regNode.DraID -ne $null)
                {            
                    "Removing DraId"
                    Remove-ItemProperty -Path $asrHivePath -Name $draIdValue
                }
                "Registry keys removed."
            }

            # First retrive all the certificates to be deleted
            $ASRcerts = Get-ChildItem -Path cert:\localmachine\my | where-object {$_.friendlyname.startswith('ASR_SRSAUTH_CERT_KEY_CONTAINER') -or $_.friendlyname.startswith('ASR_HYPER_V_HOST_CERT_KEY_CONTAINER')}
            # Open a cert store object
            $store = New-Object System.Security.Cryptography.X509Certificates.X509Store("My","LocalMachine")
            $store.Open('ReadWrite')
            # Delete the certs
            "Removing all related certificates"
            foreach ($cert in $ASRcerts)
            {
                $store.Remove($cert)
            }
        }catch
        {    
            [system.exception]
            Write-Host "Error occured" -ForegroundColor "Red"
            $error[0]
            Write-Host "FAILED" -ForegroundColor "Red"
        }
        popd``



## <a name="disable-protection-for-a-vmware-vm-or-physical-server"></a>Zakažte ochranu pro virtuální počítač VMware nebo fyzických serverů

1. V **chráněné položky** > **replikované položky**, pravým tlačítkem na počítač > **odstranit**.
2. V **odebrat počítač**, vyberte jednu z těchto možností:
    - **Zakažte ochranu pro počítač (doporučeno)**. Tuto možnost použijte k zastavení replikace na počítač. Nastavení obnovení lokality se automaticky vyčistí. Zobrazí se jenom tato možnost v následujících případech:
        - **Jste ke změně velikosti svazku virtuálního počítače**– při změně velikosti svazku virtuální počítač přejde do kritického stavu. Tato možnost zakáže ochrany a přitom zachovat body obnovení v Azure. Pokud povolíte ochranu pro tento počítač znovu, data pro změněnou velikostí svazku se převede do Azure.
        - **Nedávno jste si vyzkoušeli převzetí služeb při selhání**– po spuštění převzetí služeb při selhání pro testovací prostředí, vyberte tuto možnost, chcete-li začít s ochranou místní počítače znovu. Zakáže každý virtuální počítač a pak budete muset znovu povolte ochranu pro ně. Vypnutí počítače se toto nastavení nemá vliv virtuální počítač repliky v Azure. Nemáte odinstalujte službu Mobility z počítače.
    - **Přestat spravovat počítač**. Pokud vyberete tuto možnost, počítač se jenom odeberou z trezoru. Místní nastavení ochrany pro počítač nebude mít vliv. K odebrání nastavení na počítači a odeberte počítač z předplatného Azure, budete muset vyčistit nastavení odinstalací služba Mobility.

## <a name="disable-protection-for-a-hyper-v-vm-in-a-vmm-cloud"></a>Zakažte ochranu pro virtuální počítač technologie Hyper-V v cloudu VMM

1. V **chráněné položky** > **replikované položky**, pravým tlačítkem na počítač > **odstranit**.
2. V **odebrat počítač**, vyberte jednu z těchto možností:

    - **Zakažte ochranu pro počítač (doporučeno)**. Tuto možnost použijte k zastavení replikace na počítač. Nastavení obnovení lokality se automaticky vyčistí.
    - **Přestat spravovat počítač**. Pokud vyberete tuto možnost, počítač se jenom odeberou z trezoru. Místní nastavení ochrany pro počítač nebude mít vliv. K odebrání nastavení na počítači a odeberte počítač z předplatného Azure, budete muset ručně vyčistit nastavení podle pokynů níže. Všimněte si, že pokud vyberete možnost odstranit virtuální počítač a jeho pevné disky, jejich budete odeberou z cílového umístění.

### <a name="clean-up-protection-settings---replication-to-a-secondary-vmm-site"></a>Vyčištění nastavení ochrany - replikace do sekundární lokality VMM

Pokud jste vybrali **přestat spravovat počítač** a replikace do sekundární lokality, spusťte tento skript na primárním serveru a vyčistit nastavení pro primární virtuální počítač. V konzole VMM klikněte na tlačítko prostředí PowerShell pro otevření konzoly VMM PowerShell. Nahraďte SQLVM1 název virtuálního počítače.

         ``$vm = get-scvirtualmachine -Name "SQLVM1"
         Set-SCVirtualMachine -VM $vm -ClearDRProtection``
2. Na sekundárním serveru VMM spuštěním tohoto skriptu vyčistit nastavení pro sekundární virtuální počítač:

        ``$vm = get-scvirtualmachine -Name "SQLVM1"
        Remove-SCVirtualMachine -VM $vm -Force``
3. Na sekundárním serveru VMM aktualizujte virtuální počítače na serveru hostitele technologie Hyper-V tak, aby sekundární virtuální počítač získá zjistil znovu v konzole nástroje VMM.
4. Výše uvedené kroky vymazat nastavení replikace na serveru VMM. Pokud chcete na zastavení replikace pro virtuální počítač, spusťte následující skript jejda primární a sekundární virtuální počítače. Nahraďte SQLVM1 název virtuálního počítače.

        ``Remove-VMReplication –VMName “SQLVM1”``

### <a name="clean-up-protection-settings---replication-to-azure"></a>Vyčištění nastavení ochrany - replikaci do Azure.

1. Pokud jste vybrali **přestat spravovat počítač** a replikovat do Azure, spusťte tento skript na zdrojovém serveru VMM pomocí prostředí PowerShell pomocí konzoly VMM.
        ``$vm = get-scvirtualmachine -Name "SQLVM1"
        Set-SCVirtualMachine -VM $vm -ClearDRProtection``
2. Výše uvedené kroky vymazat nastavení replikace na serveru VMM. Na zastavení replikace pro virtuální počítač běží na serveru hostitele technologie Hyper-V, spusťte tento skript. Nahraďte SQLVM1 název virtuálního počítače a host01.contoso.com s názvem serveru hostitele technologie Hyper-V.

        ``$vmName = "SQLVM1"
        $hostName  = "host01.contoso.com"
        $vm = Get-WmiObject -Namespace "root\virtualization\v2" -Query "Select * From Msvm_ComputerSystem Where ElementName = '$vmName'" -computername $hostName
        $replicationService = Get-WmiObject -Namespace "root\virtualization\v2"  -Query "Select * From Msvm_ReplicationService"  -computername $hostName
        $replicationService.RemoveReplicationRelationship($vm.__PATH)``


## <a name="disable-protection-for-a-hyper-v-vm-in-a-hyper-v-site"></a>Zakažte ochranu pro virtuální počítač technologie Hyper-V v lokalitě Hyper-V

Tento postup použijte, pokud replikujete virtuální počítače Hyper-V do Azure bez serveru VMM.

1. V **chráněné položky** > **replikované položky**, pravým tlačítkem na počítač > **odstranit**.
2. V **odebrat počítač**, můžete vybrat následující možnosti:

   - **Zakažte ochranu pro počítač (doporučeno)**. Tuto možnost použijte k zastavení replikace na počítač. Nastavení obnovení lokality se automaticky vyčistí.
   - **Přestat spravovat počítač**. Pokud vyberete tuto možnost na počítač se jenom odeberou z trezoru. Místní nastavení ochrany pro počítač nebude mít vliv. K odebrání nastavení na počítači a odeberte virtuální počítač z předplatného Azure, budete muset ručně vyčistit nastavení. Pokud vyberete možnost odstranit virtuální počítač a jeho pevné disky, které budou odebrány z cílového umístění.
3. Pokud jste vybrali **přestat spravovat počítač**, spusťte tento skript na hostitelském serveru zdroj technologie Hyper-V, odebrat replikaci pro virtuální počítač. Nahraďte SQLVM1 název virtuálního počítače.

        $vmName = "SQLVM1"
        $vm = Get-WmiObject -Namespace "root\virtualization\v2" -Query "Select * From Msvm_ComputerSystem Where ElementName = '$vmName'"
        $replicationService = Get-WmiObject -Namespace "root\virtualization\v2"  -Query "Select * From Msvm_ReplicationService"
        $replicationService.RemoveReplicationRelationship($vm.__PATH)
