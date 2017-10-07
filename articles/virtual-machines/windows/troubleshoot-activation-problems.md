---
title: "aaaTroubleshoot Windows virtuální počítač aktivace problémy v Azure | Microsoft Docs"
description: "Poskytuje hello řešení potíží s kroky pro řešení potíží s aktivace systému Windows virtuálního počítače v Azure"
services: virtual-machines-windows, azure-resource-manager
documentationcenter: 
author: genlin
manager: willchen
editor: 
tags: top-support-issue, azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/26/2017
ms.author: genli
ms.openlocfilehash: 4ebdac66166e80dcd68cd9e2931b30a29ac01eef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-azure-windows-virtual-machine-activation-problems"></a>Řešení problémů aktivace virtuálního počítače Azure Windows

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Pokud máte potíže při aktivaci Azure Windows virtuální počítač (VM), který je vytvořený z vlastní image, můžete hello informací uvedených v potíže dokumentu tootroubleshoot hello. 

## <a name="symptom"></a>Příznaky

Při pokusu o tooactivate virtuálního počítače Windows Azure, se zobrazí chybová zpráva vypadá přibližně hello následující ukázka:

**Chyba: 0xC004F074 hello LicensingService softwaru oznámila, že hello počítač nepodařilo aktivovat. Klíč ManagementService (KMS) není možné kontaktovat. Naleznete v protokolu událostí aplikace hello Další informace.**

## <a name="cause"></a>Příčina
Obecně platí problémů s aktivací virtuálního počítače Azure dojít, pokud není nakonfiguroval hello virtuální počítač s Windows pomocí hello příslušný instalační klíč klienta služby správy KLÍČŮ nebo virtuální počítač s Windows hello má toohello problém připojením služby správy KLÍČŮ Azure (kms.core.windows.net, port 1668). 

## <a name="solution"></a>Řešení

>[!NOTE]
>Pokud používáte síť site-to-site VPN a vynuceného tunelování, najdete v části [Azure použijte vlastní směruje tooenable aktivace služby správy KLÍČŮ se vynucené tunelování](http://blogs.msdn.com/b/mast/archive/2015/05/20/use-azure-custom-routes-to-enable-kms-activation-with-forced-tunneling.aspx). 
>
>Pokud používáte ExpressRoute a budete mít výchozí trasa publikování naleznete v tématu [virtuální počítač Azure může převzetí služeb při selhání tooactivate ExpressRoute](http://blogs.msdn.com/b/mast/archive/2015/12/01/azure-vm-may-fail-to-activate-over-expressroute.aspx).

### <a name="step-1-configure-hello-appropriate-kms-client-setup-key-for-windows-server-2016-and-windows-server-2012-r2"></a>Krok 1 konfigurovat hello vhodné služby správy KLÍČŮ klientský Instalační klíč (pro Windows Server 2016 a Windows Server 2012 R2)

Pro hello virtuální počítač, který je vytvořený z vlastní image systému Windows Server 2016 nebo Windows Server 2012 R2 je nutné nakonfigurovat hello vhodné služby správy KLÍČŮ Instalační klíč klienta pro hello virtuálních počítačů.

Tento krok se nevztahuje tooWindows 2012 nebo Windows 2008 R2. Používá funkci hello automatizace aktivace virtuálního počítače (AVMA), která je podporován pouze Windows Server 2016 nebo Windows Server 2012 R2.

1. Spustit **slmgr.vbs/dlv** na příkazovém řádku se zvýšenými oprávněními. Zkontrolujte hello popisnou hodnotu ve výstupu hello a pak stanovit, zda byl vytvořen z prodejní (prodejní kanál) nebo médium licenci svazek (VOLUME_KMSCLIENT):
  
    ```
    cscript c:\windows\system32\slmgr.vbs /dlv
    ```

2. Pokud **slmgr.vbs/dlv** zobrazí prodejní kanál, spusťte následující příkazy tooset hello hello [Instalační klíč klienta služby správy KLÍČŮ](https://technet.microsoft.com/library/jj612867%28v=ws.11%29.aspx?f=255&MSPPError=-2147217396) hello verzi systému Windows Server používá a vynutit tooretry aktivace: 

    ```
    cscript c:\windows\system32\slmgr.vbs /ipk <KMS client setup key>

    cscript c:\windows\system32\slmgr.vbs /ato
     ```

    Například pro Windows Server 2016 Datacenter, spustili byste následující příkaz hello:

    ```
    cscript c:\windows\system32\slmgr.vbs /ipk CB7KF-BWN84-R7R2Y-793K2-8XDDG
    ```

### <a name="step-2-verify-hello-connectivity-between-hello-vm-and-azure-kms-service"></a>Krok 2 Ověřte, zda text hello připojení mezi hello virtuálního počítače a služby správy KLÍČŮ Azure

1. Stažení a extrakci hello [Pspingu](http:/technet.microsoft.com/en-us/sysinternals/jj729731.aspx) nástroj tooa místní složky v hello virtuální počítač, který nemusí aktivovat. 

2. Přejděte tooStart, vyhledávání v prostředí Windows PowerShell, klikněte pravým tlačítkem na prostředí Windows PowerShell a potom vyberte spustit jako správce.

3. Zajistěte, aby tento hello je virtuální počítač nakonfigurovaný toouse hello správný server služby správy KLÍČŮ Azure. toodo to spuštěním následujícího příkazu:
  
    ```
    iex “$env:windir\system32\cscript.exe $env:windir\system32\slmgr.vbs /skms
    kms.core.windows.net:1688
    ```
    příkaz Hello by měla vrátit: název počítače služby správy klíčů tookms.core.windows.net:1688 úspěšně nastaven.

4. Ověřte pomocí Pspingu, že máte připojení k serveru služby správy KLÍČŮ toohello. Přepínač toohello složky, které jste extrahovali hello Pstools.zip stahování a pak spusťte následující hello:
  
    ```
    \psping.exe kms.core.windows.net:1688
    ```
  
  Ujistěte se, že se zobrazí v hello sekundu poslední řádek výstupu hello,: odeslané = 4, přijaté = 4, Ztraceno = 0 (0 % ztráty).

  Pokud ztráty je větší než 0 (nula), nemá připojení k serveru služby správy KLÍČŮ toohello hello virtuálních počítačů. V této situaci pokud hello virtuálního počítače ve virtuální síti a má vlastní server DNS určena, ujistěte se, že server DNS je schopný tooresolve kms.core.windows.net. Nebo změňte hello DNS server tooone kms.core.windows.net přeložit.

  Všimněte si, že pokud odeberete všechny servery DNS z virtuální sítě, virtuální počítače používat interní DNS služby Azure. Tato služba může vyřešit kms.core.windows.net.
  
Ověřte také, že aby brána firewall hello hosta nebyla nakonfigurována tak, aby by blokovat pokusy o aktivaci.

5. Po ověření, tookms.core.windows.net úspěšné připojení, spusťte následující příkaz v tomto řádku se zvýšenými oprávněními prostředí Windows PowerShell hello. Tento příkaz se pokusí aktivace vícekrát.

    ```
    1..12 | % { iex “$env:windir\system32\cscript.exe $env:windir\system32\slmgr.vbs /ato” ; start-sleep 5 }
    ```

Úspěšné aktivaci vrátí informace, které se podobá následující hello:

**Aktivace Windows(R) ServerDatacenter edition (12345678-1234-1234-1234-12345678)... Produkt úspěšně aktivoval.**

## <a name="faq"></a>Nejčastější dotazy 

### <a name="i-created-hello-windows-server-2016-from-azure-marketplace-do-i-need-tooconfigure-kms-key-for-activating-hello-windows-server-2016"></a>Vytvořený hello systému Windows Server 2016 z webu Azure Marketplace. Potřebuji tooconfigure klíč služby správy KLÍČŮ pro aktivaci hello systému Windows Server 2016? 
 
Ne. bitová kopie Hello v Azure Marketplace je hello vhodné služby správy KLÍČŮ Instalační klíč klienta už nakonfigurovaná. 

### <a name="does-windows-activation-work-hello-same-way-regardless-if-hello-vm-is-using-azure-hybrid-use-benefit-hub-or-not"></a>Pracovní aktivace Windows hello stejný způsobem, bez ohledu na to pokud hello virtuální počítač používá Azure hybridní použití zvýhodnění (HUB) nebo ne? 
 
Ano. 
 
### <a name="what-happens-if-windows-activation-period-expires"></a>Co se stane, když vyprší platnost období aktivace systému Windows? 
 
Když hello poskytnutá lhůta vyprší a není ještě aktivována Windows, Windows Server 2008 R2 a novějších verzích Windows zobrazí další oznámení o aktivaci. tapeta plochy Hello zůstane černé a Windows Update bude instalovat, zabezpečení a jenom důležité aktualizace, ale není nepovinný aktualizace. Zobrazit oznámení hello oddíl na konci hello hello [licenční podmínky](http://technet.microsoft.com/en-us/library/ff793403.aspx) stránky.   

## <a name="need-help-contact-support"></a>Potřebujete pomoct? Obraťte se na podporu.
Pokud stále potřebujete pomoc, [obraťte se na podporu](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget rychle vyřešit problém.
