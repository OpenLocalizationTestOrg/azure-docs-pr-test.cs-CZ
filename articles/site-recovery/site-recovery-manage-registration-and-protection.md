---
title: "aaaRemove servery a zakažte | Microsoft Docs"
description: "Tento článek popisuje, jak toounregister servery ze služby Site Recovery trezoru a toodisable ochranu pro virtuální počítače a fyzické servery."
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
ms.openlocfilehash: 95f20433f782c93685ad4bae93c6bc0e2d2f2356
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="remove-servers-and-disable-protection"></a>Odebrání serverů a zakázání ochrany

Hello služba Azure Site Recovery přispívá tooyour provozní kontinuitu a strategie po havárii (BCDR). Hello služeb orchestruje replikaci, převzetí služeb při selhání a obnovení virtuálních počítačů a fyzických serverů. Počítače může být replikované tooAzure nebo tooa sekundárního místního datového centra. Rychlý přehled najdete v článku [Co je Azure Site Recovery?](site-recovery-overview.md)

Tento článek popisuje, jak trezoru toounregister servery ze služeb zotavení v hello portál Azure a jak toodisable ochranu pro počítače chráněné službou Site Recovery.

POST jakékoli dotazy nebo připomínky v hello dolní části tohoto článku nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="unregister-a-connected-configuration-server"></a>Zrušit registraci připojené konfigurační server

Pokud budete replikovat virtuální počítače VMware nebo fyzických serverů tooAzure Windows nebo Linuxem, můžete zrušit registraci připojené konfigurační server z trezoru následujícím způsobem:

1. Zakažte ochranu počítače. V **chráněné položky** > **replikované položky**, klikněte pravým tlačítkem na počítač hello > **odstranit**.
2. Zrušit přidružení žádné zásady. V **infrastruktura Site Recovery** > **pro VMWare a fyzické počítače** > **zásady replikace**, dvakrát klikněte na hello přidružené zásady. Klikněte pravým tlačítkem na hello konfigurační server > **zrušení spojení**.
3. Odeberte všechny další místní proces a hlavních cílových serverů. V **infrastruktura Site Recovery** > **pro VMWare a fyzické počítače** > **konfigurační servery**, klikněte pravým tlačítkem na server hello > **Odstranit**.
4. Odstraňte konfigurační server hello.
5. Ručně odinstalujte službu Mobility hello systémem hello hlavní cílový server (bude jím buď samostatný server nebo systémem hello konfigurační server).
6. Odinstalujte všechny servery další proces.
7. Odinstalujte hello konfigurační server.
8. Na konfiguračním serveru hello odinstalujte hello instanci databáze MySQL, která byla nainstalována pomocí Site Recovery.
9. V registru hello hello konfigurace serveru odstranit klíč hello ``HKEY_LOCAL_MACHINE\Software\Microsoft\Azure Site Recovery``.

## <a name="unregister-a-unconnected-configuration-server"></a>Zrušit registraci serveru bez připojení konfigurace

Pokud budete replikovat virtuální počítače VMware nebo fyzických serverů tooAzure Windows nebo Linuxem, můžete zrušit registraci serveru bez připojení konfiguraci z úložiště následujícím způsobem:

1. Zakažte ochranu počítače. V **chráněné položky** > **replikované položky**, klikněte pravým tlačítkem na počítač hello > **odstranit**. Vyberte **přestat spravovat počítač hello**.
2. Odeberte všechny další místní proces a hlavních cílových serverů. V **infrastruktura Site Recovery** > **pro VMWare a fyzické počítače** > **konfigurační servery**, klikněte pravým tlačítkem na server hello > **Odstranit**.
3. Odstraňte konfigurační server hello.
4. Ručně odinstalujte službu Mobility hello systémem hello hlavní cílový server (bude jím buď samostatný server nebo systémem hello konfigurační server).
5. Odinstalujte všechny servery další proces.
6. Odinstalujte hello konfigurační server.
7. Na konfiguračním serveru hello odinstalujte hello instanci databáze MySQL, která byla nainstalována pomocí Site Recovery.
8. V registru hello hello konfigurace serveru odstranit klíč hello ``HKEY_LOCAL_MACHINE\Software\Microsoft\Azure Site Recovery``.

## <a name="unregister-a-connected-vmm-server"></a>Zrušení registrace připojený k serveru VMM

Jako osvědčený postup doporučujeme zrušit registraci serveru VMM hello, pokud byl připojený tooAzure. Tím se zajistí, že jsou správně vyčistit nastavení na serverech VMM hello (a na jiných serverech VMM s spárované cloudy). Bez připojení serveru byste měli odebrat, pouze pokud je trvalé potíže s připojením. Pokud není připojený hello VMM server, budete potřebovat toomanually spustit skript tooclean nastavení.

1. Zastavení replikace virtuálních počítačů v cloudech VMM server má tooremove hello.
2. Odstraňte veškerá jeho mapování sítě používá cloudy na serveru VMM má toodelete hello. V **infrastruktura Site Recovery** > **pro System Center VMM** > **mapování sítě**, klikněte pravým tlačítkem na mapování sítě hello >  **Odstranit**.
3. Zrušit přidružení zásad replikace z cloudy na serveru VMM má tooremove hello.  V **infrastruktura Site Recovery** > **pro System Center VMM** >  **zásady replikace**, dvakrát klikněte na hello přidružené zásady. Klikněte pravým tlačítkem na cloudu hello > **zrušení spojení**.
4. Odstraňte hello VMM server nebo aktivním uzlu VMM. V **infrastruktura Site Recovery** > **pro System Center VMM** > **servery VMM**, klikněte pravým tlačítkem na server hello >  **Odstranit**.
5. Odinstalujte hello zprostředkovatele na serveru VMM hello ručně. Pokud máte cluster, odeberte ze všech uzlů.
6. Pokud replikujete tooAzure, ručně odeberte agenta služeb zotavení Microsoft hello z hostitelů Hyper-V v cloudech hello odstranit.



### <a name="unregister-an-unconnected-vmm-server"></a>Zrušit registraci serveru VMM bez připojení

1. Zastavení replikace virtuálních počítačů v cloudech VMM server má tooremove hello.
2. Odstraňte veškerá jeho mapování sítě používá cloudy na serveru VMM hello, které chcete toodelete. V **infrastruktura Site Recovery** > **pro System Center VMM** > **mapování sítě**, klikněte pravým tlačítkem na mapování sítě hello >  **Odstranit**.
3. Poznamenejte si ID hello hello serveru VMM.
4. Zrušit přidružení zásad replikace z cloudy na serveru VMM má tooremove hello.  V **infrastruktura Site Recovery** > **pro System Center VMM** >  **zásady replikace**, dvakrát klikněte na hello přidružené zásady. Klikněte pravým tlačítkem na cloudu hello > **zrušení spojení**.
5. Odstraňte hello VMM server nebo aktivní uzel. V **infrastruktura Site Recovery** > **pro System Center VMM** > **servery VMM**, klikněte pravým tlačítkem na server hello >  **Odstranit**.
6. Stažení a spuštění hello [skript pro vyčištění](http://aka.ms/asr-cleanup-script-vmm) na serveru VMM hello. Otevřete prostředí PowerShell s hello **spustit jako správce** možnost zásady spouštění hello toochange pro obor hello výchozí (LocalMachine). Ve skriptu hello zadejte ID hello hello chcete tooremove serveru VMM. skript Hello odebere registrace a cloudového párování informace ze serveru hello.
5. Spusťte skript vyčištění hello na jiných serverech VMM, které obsahují cloudy, které jsou spárovaný s cloudy na serveru VMM má tooremove hello.
6. Spusťte skript vyčištění hello na žádné jiné pasivní uzly clusteru VMM mající nainstalovaný zprostředkovatel hello.
7. Odinstalujte hello zprostředkovatele na serveru VMM hello ručně. Pokud máte cluster, odeberte ze všech uzlů.
8. Pokud je replikace tooAzure, můžete odebrat agenta služeb zotavení Microsoft hello z hostitelů Hyper-V v cloudech hello odstranit.

## <a name="unregister-a-hyper-v-host-in-a-hyper-v-site"></a>Zrušit registraci hostitele Hyper-V v lokalitě Hyper-V

Hostitelé Hyper-V, které nejsou spravované přes VMM jsou shromážděná do lokality Hyper-V. Odeberte hostitele v lokalitě Hyper-V následujícím způsobem:

1. Zakážete replikaci pro virtuální počítače Hyper-V umístěný na hostiteli hello.
2. Zrušit přidružení zásad pro web hello Hyper-V. V **infrastruktura Site Recovery** > **pro weby technologie Hyper-V** >  **zásady replikace**, dvakrát klikněte na hello přidružené zásady. Klikněte pravým tlačítkem na hello lokality > **zrušení spojení**.
3. Odstraňte hostitele Hyper-V. V **infrastruktura Site Recovery** > **pro System Center VMM** > **hostitelů Hyper-V**, klikněte pravým tlačítkem na server hello >  **Odstranit**.
4. Odstranění hello technologie Hyper-V lokality po odebrání všech hostitelů z něj. V **infrastruktura Site Recovery** > **pro System Center VMM** > **lokalit Hyper-V**, klikněte pravým tlačítkem na hello lokality >  **Odstranit**.
5. Spusťte následující skript na každém hostiteli technologie Hyper-V, který jste odebrali hello. skript Hello vyčistí nastavení na serveru hello a zruší registraci hello trezoru.


        `` pushd .
        try
        {
             $windowsIdentity=[System.Security.Principal.WindowsIdentity]::GetCurrent()
             $principal=new-object System.Security.Principal.WindowsPrincipal($windowsIdentity)
             $administrators=[System.Security.Principal.WindowsBuiltInRole]::Administrator
             $isAdmin=$principal.IsInRole($administrators)
             if (!$isAdmin)
             {
                "Please run hello script as an administrator in elevated mode."
                $choice = Read-Host
                return;       
             }

            $error.Clear()    
            "This script will remove hello old Azure Site Recovery Provider related properties. Do you want toocontinue (Y/N) ?"
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
                "Stopping hello Azure Site Recovery service..."
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

            # First retrive all hello certificates toobe deleted
            $ASRcerts = Get-ChildItem -Path cert:\localmachine\my | where-object {$_.friendlyname.startswith('ASR_SRSAUTH_CERT_KEY_CONTAINER') -or $_.friendlyname.startswith('ASR_HYPER_V_HOST_CERT_KEY_CONTAINER')}
            # Open a cert store object
            $store = New-Object System.Security.Cryptography.X509Certificates.X509Store("My","LocalMachine")
            $store.Open('ReadWrite')
            # Delete hello certs
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

1. V **chráněné položky** > **replikované položky**, klikněte pravým tlačítkem na počítač hello > **odstranit**.
2. V **odebrat počítač**, vyberte jednu z těchto možností:
    - **Zakažte ochranu pro počítač hello (doporučeno)**. Použijte tuto možnost toostop replikace hello počítače. Nastavení obnovení lokality se automaticky vyčistí. Zobrazí se jenom tato možnost v hello následující okolnosti:
        - **Jste ke změně velikosti svazku virtuálního počítače hello**– při změně velikosti na svazku hello virtuální počítač přejde do kritického stavu. Vyberte tuto možnost toodisables ochranu při zachování bodů obnovení v Azure. Pokud povolíte ochranu počítače hello znovu, budou data hello hello po změně velikosti svazku přenášená tooAzure.
        - **Nedávno jste si vyzkoušeli převzetí služeb při selhání**– po spuštění převzetí služeb při selhání tootest prostředí, vyberte tuto možnost toostart znovu ochranu počítačů v místě. Zakáže každý virtuální počítač, a pak je třeba tooenable ochrany pro ně znovu. Zakázání hello počítač se toto nastavení nemá vliv hello virtuální počítač repliky v Azure. Nemáte odinstalujte službu Mobility hello z počítače hello.
    - **Přestat spravovat počítač hello**. Pokud vyberete tuto možnost, bude počítač hello pouze odebrat z trezoru hello. Místní nastavení ochrany pro počítač hello nebude mít vliv. tooremove nastavení v počítači hello a počítač hello tooremove z hello předplatné Azure, musíte tooclean hello nastavení odinstalací služby Mobility hello.

## <a name="disable-protection-for-a-hyper-v-vm-in-a-vmm-cloud"></a>Zakažte ochranu pro virtuální počítač technologie Hyper-V v cloudu VMM

1. V **chráněné položky** > **replikované položky**, klikněte pravým tlačítkem na počítač hello > **odstranit**.
2. V **odebrat počítač**, vyberte jednu z těchto možností:

    - **Zakažte ochranu pro počítač hello (doporučeno)**. Použijte tuto možnost toostop replikace hello počítače. Nastavení obnovení lokality se automaticky vyčistí.
    - **Přestat spravovat počítač hello**. Pokud vyberete tuto možnost, bude počítač hello pouze odebrat z trezoru hello. Místní nastavení ochrany pro počítač hello nebude mít vliv. tooremove nastavení v počítači hello a počítač hello tooremove z hello předplatné Azure, je nutné nastavení hello tooclean až ručně pomocí níže uvedených pokynů hello. Všimněte si, že pokud vyberete toodelete hello virtuální počítač a jeho pevné disky, budete vyloučení ze hello cílové umístění.

### <a name="clean-up-protection-settings---replication-tooa-secondary-vmm-site"></a>Vyčištění nastavení ochrany - replikace tooa sekundární lokalita VMM

Pokud jste vybrali **přestat spravovat počítač hello** a můžete replikovat tooa sekundární lokalitu, spusťte tento skript na primární server tooclean hello hello nastavení pro primární virtuální počítač hello. V konzole VMM hello klikněte na konzole VMM PowerShell hello tlačítko tooopen aplikace hello prostředí PowerShell. Nahraďte SQLVM1 hello název virtuálního počítače.

         ``$vm = get-scvirtualmachine -Name "SQLVM1"
         Set-SCVirtualMachine -VM $vm -ClearDRProtection``
2. Na sekundárním serveru VMM hello spusťte tento skript tooclean nastavení hello pro hello sekundární virtuální počítač:

        ``$vm = get-scvirtualmachine -Name "SQLVM1"
        Remove-SCVirtualMachine -VM $vm -Force``
3. Na sekundárním serveru VMM hello aktualizujte hello virtuální počítače na serveru hostitele technologie Hyper-V hello, takže hello sekundární virtuální počítač získá zjistit znovu v konzole VMM hello.
4. Hello výše kroky vymazány hello nastavení replikace na serveru VMM hello. Pokud chcete, aby toostop replikace pro virtuální počítač hello, spusťte následující skript jejda hello hello primární a sekundární virtuální počítače. Nahraďte SQLVM1 hello název virtuálního počítače.

        ``Remove-VMReplication –VMName “SQLVM1”``

### <a name="clean-up-protection-settings---replication-tooazure"></a>Vyčištění nastavení ochrany - tooAzure replikace

1. Pokud jste vybrali **přestat spravovat počítač hello** a můžete replikovat tooAzure, spusťte tento skript na serveru VMM hello zdroje z konzoly VMM hello pomocí prostředí PowerShell.
        ``$vm = get-scvirtualmachine -Name "SQLVM1"
        Set-SCVirtualMachine -VM $vm -ClearDRProtection``
2. Hello výše kroky vymazat hello nastavení replikace na serveru VMM hello. toostop replikace pro virtuální počítač hello spuštěná na serveru hostitele technologie Hyper-V hello, spusťte tento skript. Nahraďte SQLVM1 hello název virtuálního počítače a host01.contoso.com s názvem hello hello serveru hostitele technologie Hyper-V.

        ``$vmName = "SQLVM1"
        $hostName  = "host01.contoso.com"
        $vm = Get-WmiObject -Namespace "root\virtualization\v2" -Query "Select * From Msvm_ComputerSystem Where ElementName = '$vmName'" -computername $hostName
        $replicationService = Get-WmiObject -Namespace "root\virtualization\v2"  -Query "Select * From Msvm_ReplicationService"  -computername $hostName
        $replicationService.RemoveReplicationRelationship($vm.__PATH)``


## <a name="disable-protection-for-a-hyper-v-vm-in-a-hyper-v-site"></a>Zakažte ochranu pro virtuální počítač technologie Hyper-V v lokalitě Hyper-V

Tento postup použijte, pokud replikujete virtuální počítače Hyper-V tooAzure bez serveru VMM.

1. V **chráněné položky** > **replikované položky**, klikněte pravým tlačítkem na počítač hello > **odstranit**.
2. V **odebrat počítač**, můžete vybrat hello následující možnosti:

   - **Zakažte ochranu pro počítač hello (doporučeno)**. Použijte tuto možnost toostop replikace hello počítače. Nastavení obnovení lokality se automaticky vyčistí.
   - **Přestat spravovat počítač hello**. Pokud vyberete tuto možnost hello počítače odebere jenom z trezoru hello. Místní nastavení ochrany pro počítač hello nebude mít vliv. tooremove nastavení v počítači hello a tooremove hello virtuální počítač z hello předplatné Azure, je nutné nastavení hello tooclean si ručně. Pokud vyberete toodelete hello virtuální počítač a jeho pevné disky, které budou odebrány z hello cílové umístění.
3. Pokud jste vybrali **přestat spravovat počítač hello**, spusťte tento skript na serveru hostitele hello zdroj technologie Hyper-V, tooremove replikace pro virtuální počítač hello. Nahraďte SQLVM1 hello název virtuálního počítače.

        $vmName = "SQLVM1"
        $vm = Get-WmiObject -Namespace "root\virtualization\v2" -Query "Select * From Msvm_ComputerSystem Where ElementName = '$vmName'"
        $replicationService = Get-WmiObject -Namespace "root\virtualization\v2"  -Query "Select * From Msvm_ReplicationService"
        $replicationService.RemoveReplicationRelationship($vm.__PATH)
