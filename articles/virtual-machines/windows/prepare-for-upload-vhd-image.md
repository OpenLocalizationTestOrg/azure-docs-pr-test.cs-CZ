---
title: "aaaPrepare tooAzure tooupload virtuálního pevného disku Windows | Microsoft Docs"
description: "Jak tooprepare Windows VHD nebo VHDX před nahráním tooAzure"
services: virtual-machines-windows
documentationcenter: 
author: glimoli
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 7802489d-33ec-4302-82a4-91463d03887a
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: genli
ms.openlocfilehash: 530390e4c6a4f66ddfd4da23338f9bb3708c299f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="prepare-a-windows-vhd-or-vhdx-tooupload-tooazure"></a>Příprava tooAzure tooupload Windows VHD nebo VHDX
Před nahráním Windows virtuálních počítačů (VM) z místní tooMicrosoft Azure, musíte připravit hello virtuální pevný disk (VHD nebo VHDX). Azure podporuje jenom virtuální počítače generace 1, které jsou ve formátu souboru VHD hello a mají pevnou velikostí disku. virtuální pevný disk je 1,023 GB Hello maximální velikost povolená pro hello. Generace 1 virtuální počítač z hello VHDX souborů tooVHD systému a z dynamicky se zvětšující disku, velikost toofixed můžete převést. Ale generaci Virtuálního počítače nelze změnit. Další informace najdete v tématu [měli vytvořit generace 1 nebo 2 virtuálních počítačů Hyper-v](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v).

Další informace o zásadách hello podpory pro virtuální počítač Azure, najdete v části [podporu serveru softwaru společnosti Microsoft pro virtuální počítače Microsoft Azure](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines).

> [!Note]
> Hello pokyny v tomto článku platí toohello 64bitovou verzi Windows Server 2008 R2 a novější operační systém Windows server. Informace o spuštěných 32bitovou verzi operačního systému v Azure najdete v tématu [podpora pro 32bitové operační systémy ve virtuálních počítačích Azure](https://support.microsoft.com/help/4021388/support-for-32-bit-operating-systems-in-azure-virtual-machines).

## <a name="convert-hello-virtual-disk-toovhd-and-fixed-size-disk"></a>Převést virtuální disk tooVHD hello a disk pevné velikosti 
Pokud potřebujete tooconvert vyžaduje vaše virtuální disk toohello formát pro Azure, použijte jednu z metod hello v této části. Zálohujte hello virtuálních počítačů před spuštěním procesu převodu hello virtuální disk a ujistěte se, že hello virtuálního pevného disku Windows na místním serveru hello správně funguje. Vyřešte všechny chyby v rámci hello virtuální počítač před zkuste tooconvert nebo nahrajte ho tooAzure.

Převedete-li hello disk, vytvořte virtuální počítač, který používá hello převést disk. Spuštění a přihlášení toofinish virtuálních počítačů toohello Příprava hello virtuálních počítačů pro nahrávání.

### <a name="convert-disk-using-hyper-v-manager"></a>Převedení disku pomocí Správce technologie Hyper-V
1. Otevřete Správce technologie Hyper-V a vyberte místního počítače na levé straně hello. V nabídce hello výše hello seznamu počítačů, klikněte na tlačítko **akce** > **upravit Disk**.
2. Na hello **najít virtuální pevný Disk** obrazovky, vyhledejte a vyberte virtuální disk.
3. Na hello **zvolit akci** obrazovky a pak vyberte **převést** a **Další**.
4. Pokud potřebujete tooconvert z VHDX, vyberte **virtuálního pevného disku** a pak klikněte na tlačítko **další**
5. Pokud potřebujete tooconvert z dynamicky se zvětšující disku, vyberte **pevnou velikost** a pak klikněte na tlačítko **další**
6. Vyhledejte a vyberte cestu toosave hello nový soubor virtuálního pevného disku do.
7. Klikněte na **Dokončit**.

>[!NOTE]
>Hello příkazy v tomto článku se musí spustit v relaci prostředí PowerShell zvýšenými oprávněními.

### <a name="convert-disk-by-using-powershell"></a>Převést disk pomocí prostředí PowerShell
Můžete převést virtuální disk s použitím hello [Convert-VHD](http://technet.microsoft.com/library/hh848454.aspx) příkazu v prostředí Windows PowerShell. Vyberte **spustit jako správce** při spuštění prostředí PowerShell. 

Hello následující ukázkový příkaz převede z VHDX tooVHD a z dynamicky se zvětšující toofixed – velikost disku:

```Powershell
Convert-VHD –Path c:\test\MY-VM.vhdx –DestinationPath c:\test\MY-NEW-VM.vhd -VHDType Fixed
```
V tomto příkazu nahraďte hello hodnota pro "-cestu" s hello cesta toohello virtuální pevný disk, který chcete tooconvert a hello hodnota pro "-DestinationPath" s hello novou cestu a název hello převést disk.

### <a name="convert-from-vmware-vmdk-disk-format"></a>Převod z formátu VMware VMDK disku
Pokud obsahuje bitovou kopii virtuálního počítače s Windows hello [formát souboru VMDK](https://en.wikipedia.org/wiki/VMDK), převeďte ho tooa virtuální pevný disk pomocí hello [převaděč virtuálních počítačů Microsoft](https://www.microsoft.com/download/details.aspx?id=42497). Další informace najdete v článku blog hello [jak tooConvert virtuální pevný disk tooHyper V VMware VMDK](http://blogs.msdn.com/b/timomta/archive/2015/06/11/how-to-convert-a-vmware-vmdk-to-hyper-v-vhd.aspx).

## <a name="set-windows-configurations-for-azure"></a>Nastavení konfigurace systému Windows Azure

Pro hello virtuální počítač, který máte v plánu tooupload tooAzure, spusťte všechny příkazy v hello následující kroky z [okno příkazového řádku se zvýšenými oprávněními](https://technet.microsoft.com/library/cc947813.aspx):

1. Odeberte všechny statické trvalé trasy v tabulce směrování hello:
   
   * Tabulka směrování tooview hello spustit `route print` hello příkazového řádku.
   * Zkontrolujte hello **trvalost trasy** oddíly. Pokud je trvalé trasy, použijte [trasy odstranění](https://technet.microsoft.com/library/cc739598.apx) tooremove ho.
2. Odebrání proxy serveru WinHTTP hello:
   
    ```PowerShell
    netsh winhttp reset proxy
    ```
3. Nastavit zásady sítě SAN hello disku příliš[Onlineall](https://technet.microsoft.com/library/gg252636.aspx). 
   
    ```PowerShell
    diskpart 
    ```
    V hello otevřete okno příkazového řádku zadejte hello následující příkazy:

     ```DISKPART
    san policy=onlineall
    exit   
    ```

4. Nastavit čas koordinovaný světový čas (UTC) pro systém Windows a hello nastaven typ spouštění služby Systémový čas (w32time) hello příliš**automaticky**:
   
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\TimeZoneInformation' -name "RealTimeIsUniversal" 1 -Type DWord

    Set-Service -Name w32time -StartupType Auto
    ```
5. Nastavit hello power profil toohello **vysoký výkon**:

    ```PowerShell
    powercfg /setactive SCHEME_MIN
    ```

## <a name="check-hello-windows-services"></a>Zkontrolujte služby systému Windows hello
Ujistěte se, že každý z následujících služeb Windows hello je nastavená toohello **Windows výchozí hodnoty**. Toto jsou minimální čísla hello služeb, které musí být nastaven toomake se, že tento hello virtuálních počítačů má připojení. tooreset hello spuštění nastavení, spusťte následující příkazy hello:
   
```PowerShell
Set-Service -Name bfe -StartupType Auto
Set-Service -Name dhcp -StartupType Auto
Set-Service -Name dnscache -StartupType Auto
Set-Service -Name IKEEXT -StartupType Auto
Set-Service -Name iphlpsvc -StartupType Auto
Set-Service -Name netlogon -StartupType Manual
Set-Service -Name netman -StartupType Manual
Set-Service -Name nsi -StartupType Auto
Set-Service -Name termService -StartupType Manual
Set-Service -Name MpsSvc -StartupType Auto
Set-Service -Name RemoteRegistry -StartupType Auto
```

## <a name="update-remote-desktop-registry-settings"></a>Aktualizace nastavení registru vzdálené plochy
Ujistěte se, že hello následující nastavení jsou správně nakonfigurované pro připojení ke vzdálené ploše:

>[!Note] 
>Může se zobrazit chybová zpráva, když spustíte hello **Set-ItemProperty-cesta ' HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal služby - název &lt;název objektu&gt; &lt;hodnotu&gt;** v těchto krocích. Hello chybovou zprávu můžete bezpečně ignorovat. Znamená to, že pouze hello domény není vkládání tato konfigurace prostřednictvím objektu zásad skupiny.
>
>

1. Je povolen protokol RDP (Remote Desktop):
   
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server' -name "fDenyTSConnections" -Value 0 -Type DWord

    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services' -name "fDenyTSConnections" -Value 0 -Type DWord
    ```
   
2. nastavená Hello portu RDP (výchozí port 3389):
   
    ```PowerShell
   Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -name "PortNumber" 3389 -Type DWord
    ```
    Při nasazení virtuálního počítače, hello výchozí pravidla se vytvářejí na portu 3389. Pokud chcete, aby číslo portu hello toochange, udělat po nasazení hello virtuálních počítačů v Azure.

3. naslouchací proces Hello naslouchá v každé síťové rozhraní:
   
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -name "LanAdapter" 0 -Type DWord
   ```
4. Konfigurace režimu hello ověřování na úrovni sítě pro připojení RDP hello:
   
    ```PowerShell
   Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -name "UserAuthentication" 1 -Type DWord

    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -name "SecurityLayer" 1 -Type DWord

    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -name "fAllowSecProtocolNegotiation" 1 -Type DWord
     ```

5. Nastavte hodnotu udržování hello:
    
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services' -name "KeepAliveEnable" 1 -Type DWord
    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services' -name "KeepAliveInterval" 1 -Type DWord
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -name "KeepAliveTimeout" 1 -Type DWord
    ```
6. Připojení:
    
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services' -name "fDisableAutoReconnect" 0 -Type DWord
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -name "fInheritReconnectSame" 1 -Type DWord
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -name "fReconnectSame" 0 -Type DWord
    ```
7. Omezit hello počet souběžných připojení:
    
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -name "MaxInstanceCount" 4294967295 -Type DWord
    ```
8. Pokud jsou všechny certifikáty podepsané svým držitelem vázáno RDP toohello naslouchací proces, odeberte je:
    
    ```PowerShell
    Remove-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -name "SSLCertificateSHA1Hash"
    ```
    Toto je toomake se, zda se můžete připojit od začátku hello při nasazování hello virtuálních počítačů. Také to můžete zkontrolovat po hello virtuální počítač nasazen v Azure, v případě potřeby v pozdější fázi.

9. Pokud hello virtuálních počítačů bude součástí domény, zkontrolujte, že všechny hello následující nastavení toomake jistotu, že hello předchozí nastavení se nevrátí. Hello zásady, které se musí kontrolovat jsou hello následující:
    
    - Je povolen protokol RDP:

         Počítač Konfigurace uživatele\Zásady\Nastavení systému Settings\Administrative Templates\ Windows\Vzdálená plocha\Hostitel relace vzdálené plochy\Připojení:
         
         **Povolit uživatelům tooconnect vzdáleně pomocí vzdálené plochy**

    - NLA zásad skupiny:

        Settings\Administrative Templates\Components\Remote plocha\Hostitel relace plochy plochy\Zabezpečení: 
        
        **Uživatel ověřování vyžadovat pro vzdálená připojení pomocí ověření úrovně sítě**
    
    - Ponechat Alive nastavení:

        Počítač Konfigurace uživatele\Zásady\Nastavení systému Settings\Administrative pro správu\Součásti systému Windows\Vzdálená plocha\Hostitel relace vzdálené plochy\Připojení: 
        
        **Nakonfigurujte interval udržování připojení**

    - Znovu připojte nastavení:

        Počítač Konfigurace uživatele\Zásady\Nastavení systému Settings\Administrative pro správu\Součásti systému Windows\Vzdálená plocha\Hostitel relace vzdálené plochy\Připojení: 
        
        **Automatické obnovení připojení**

    - Omezit počet hello nastavení připojení:

        Počítač Konfigurace uživatele\Zásady\Nastavení systému Settings\Administrative pro správu\Součásti systému Windows\Vzdálená plocha\Hostitel relace vzdálené plochy\Připojení: 
        
        **Omezení počtu připojení**

## <a name="configure-windows-firewall-rules"></a>Konfigurace pravidel brány Windows Firewall
1. Zapněte bránu Windows Firewall na hello tři profily (domény, Standard a veřejný):

   ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\services\SharedAccess\Parameters\FirewallPolicy\DomainProfile' -name "EnableFirewall" -Value 1 -Type DWord
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\services\SharedAccess\Parameters\FirewallPolicy\PublicProfile' -name "EnableFirewall" -Value 1 -Type DWord
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\services\SharedAccess\Parameters\FirewallPolicy\Standardprofile' -name "EnableFirewall" -Value 1 -Type DWord
   ```

2. Spusťte následující příkaz v prostředí PowerShell tooallow WinRM prostřednictvím hello tří profilů brány firewall (Domain, Private a Public) hello a povolte hello vzdáleného prostředí PowerShell služby:
   
   ```PowerShell
    Enable-PSRemoting -force
    netsh advfirewall firewall set rule dir=in name="Windows Remote Management (HTTP-In)" new enable=yes
    netsh advfirewall firewall set rule dir=in name="Windows Remote Management (HTTP-In)" new enable=yes
   ```
3. Povolte následující pravidla brány firewall, tooallow provoz protokolu RDP hello hello 

   ```PowerShell
    netsh advfirewall firewall set rule group="Remote Desktop" new enable=yes
   ```   
4. Povolení hello souboru a sdílení tiskáren pravidla, aby hello virtuálních počítačů může odpovídat příkazu ping tooa uvnitř hello virtuální sítě:

   ```PowerShell
    netsh advfirewall firewall set rule dir=in name="File and Printer Sharing (Echo Request - ICMPv4-In)" new enable=yes
   ``` 
5. Pokud hello virtuálních počítačů bude součástí domény, zkontrolujte následující nastavení toomake jistotu, že hello předchozí nastavení se nevrátí hello. Hello AD zásady, které se musí kontrolovat jsou hello následující:

    - Povolení profilů brány Firewall systému Windows hello

        Počítač Konfigurace uživatele\Zásady\Nastavení systému Settings\Administrative správu\Síť\Síťová Connection\Windows Firewall\Domain Profile\Windows brány Firewall: **chránit všechna síťová připojení**

       Počítač Konfigurace uživatele\Zásady\Nastavení systému Settings\Administrative správu\Síť\Síťová Connection\Windows Firewall\Standard Profile\Windows brány Firewall: **chránit všechna síťová připojení**

    - Povolení protokolu RDP 

        Počítač Konfigurace uživatele\Zásady\Nastavení systému Settings\Administrative správu\Síť\Síťová Connection\Windows Firewall\Domain Profile\Windows brány Firewall: **povolit příchozí výjimky vzdálené plochy**

        Počítač Konfigurace uživatele\Zásady\Nastavení systému Settings\Administrative správu\Síť\Síťová Connection\Windows Firewall\Standard Profile\Windows brány Firewall: **povolit příchozí výjimky vzdálené plochy**

    - Povolit protokol ICMP V4

        Počítač Konfigurace uživatele\Zásady\Nastavení systému Settings\Administrative správu\Síť\Síťová Connection\Windows Firewall\Domain Profile\Windows brány Firewall: **povolit výjimky protokolu ICMP**

        Počítač Konfigurace uživatele\Zásady\Nastavení systému Settings\Administrative správu\Síť\Síťová Connection\Windows Firewall\Standard Profile\Windows brány Firewall: **povolit výjimky protokolu ICMP**

## <a name="verify-vm-is-healthy-secure-and-accessible-with-rdp"></a>Ověřte, zda že virtuální počítač je v pořádku, zabezpečenou a přístupné pomocí protokolu RDP 
1. toomake zda hello disku je v pořádku a konzistentní, spustit operaci kontroly disku v dalším restartování virtuálního počítače hello:

    ```PowerShell
    Chkdsk /f
    ```
    Ujistěte se, že hello sestava zobrazí v pořádku a vyčištění disku.

2. Nastavení konfiguračních dat spouštění (BCD) hello. 

    > [!Note]
    > Zajistěte, aby spuštění těchto příkazů v okně CMD se zvýšenými oprávněními a **není** na prostředí PowerShell:
   
   ```CMD
   bcdedit /set {bootmgr} integrityservices enable
   
   bcdedit /set {default} device partition=C:
   
   bcdedit /set {default} integrityservices enable
   
   bcdedit /set {default} recoveryenabled Off
   
   bcdedit /set {default} osdevice partition=C:
   
   bcdedit /set {default} bootstatuspolicy IgnoreAllFailures
   ```
3. Ověřte, zda že je konzistentní tohoto úložiště Instrumentations správy Windows hello. tooperform se spuštění hello následující příkaz:

    ```PowerShell
    winmgmt /verifyrepository
    ```
    Pokud je poškozený hello úložiště, najdete v části [rozhraní WMI: poškození úložiště, nebo Ne](https://blogs.technet.microsoft.com/askperf/2014/08/08/wmi-repository-corruption-or-not).

4. Ujistěte se, že nepoužívá jiná aplikace hello portu 3389. Tento port se používá pro hello RDP služby v Azure. Můžete spustit **netstat - anob** toosee v používaných portů na hello virtuálních počítačů:

    ```PowerShell
    netstat -anob
    ```

5. Pokud má tooupload virtuálního pevného disku Windows hello je řadič domény, postupujte podle těchto kroků:

    A. Postupujte podle [tyto další kroky](https://support.microsoft.com/kb/2904015) tooprepare hello disku.

    B. Ujistěte se, že znáte hello heslo režimu obnovení adresářových služeb v případě, že máte toostart hello virtuálního počítače v režimu obnovení adresářových služeb v určitém okamžiku. Může být vhodné propojit toorefer toothis tooset hello [heslo režimu obnovení adresářových služeb](https://technet.microsoft.com/library/cc754363(v=ws.11).aspx).

6. Ujistěte se, že hello předdefinovaného účtu Administrator a heslo jsou známé tooyou. Možná chcete tooreset hello aktuální heslo místního správce a ujistěte se, že můžete použít tento účet toosign v tooWindows prostřednictvím hello připojení RDP. Objekt zásad skupiny "Povolit přihlášení prostřednictvím vzdálené plochy" hello řídí toto oprávnění přístupu. Tento objekt můžete zobrazit v hello Editor místních zásad skupiny v části:

    Počítač Konfigurace počítače\Nastavení systému Windows\Nastavení Settings\Local přiřazení uživatelských práv

7. Zkontrolujte, že hello následující AD zásady toomake jistotu, že nejsou blokuje přístup RDP prostřednictvím protokolu RDP ani z hello sítě:

    - Počítač Konfigurace počítače\Nastavení systému Windows\Nastavení Settings\Local Policies\User Rights Assignment\Deny přístup toothis počítači ze sítě hello

    - Počítač Konfigurace počítače\Nastavení systému Windows\Nastavení Settings\Local Policies\User Rights Assignment\Deny přihlášení prostřednictvím vzdálené plochy


8. Pomocí připojení RDP hello dosažitelný restartování hello virtuálních počítačů toomake se, že Windows je stále v pořádku. V tomto okamžiku můžete má toocreate virtuálního počítače ve vaší místní technologie Hyper-V toomake zda hello, které je zcela spouštění virtuálních počítačů a potom otestovat, zda je dostupný protokolu RDP.

9. Odeberte všechny filtry navíc rozhraní TDI, jako je například software, která analyzuje TCP paketů nebo další brány firewall. Také to můžete zkontrolovat po hello virtuální počítač nasazen v Azure, v případě potřeby v pozdější fázi.

10. Odinstalujte všechny ostatní software třetích stran a ovladač, který je součástí související toophysical nebo jakoukoli jinou technologii virtualizace.

### <a name="install-windows-updates"></a>Instalace aktualizací systému Windows
Konfigurace ideální Hello je příliš**mají úroveň oprav hello hello počítače na nejnovější hello**. Pokud to není možné, ujistěte se, že hello následující aktualizace jsou nainstalované:

|                       |                   |           |                                       Minimální verze x64       |                                      |                                      |                            |
|-------------------------|-------------------|------------------------------------|---------------------------------------------|--------------------------------------|--------------------------------------|----------------------------|
| Komponenta               | Binární            | Windows 7 a Windows Server 2008 R2 | Windows 8 a Windows Server 2012             | Windows 8.1 a Windows Server 2012 R2 | Windows 10 a Windows Server 2016 RS1 | RS2 Windows 10             |
| Úložiště                 | disk.sys          | 6.1.7601.23403 - KB3125574         | 6.2.9200.17638 / 6.2.9200.21757 - KB3137061 | 6.3.9600.18203 - KB3137061           | -                                    | -                          |
|                         | Storport.sys      | 6.1.7601.23403 - KB3125574         | 6.2.9200.17188 / 6.2.9200.21306 - KB3018489 | 6.3.9600.18573 - KB4022726           | 10.0.14393.1358 - KB4022715          | 10.0.15063.332             |
|                         | NTFS.sys          | 6.1.7601.23403 - KB3125574         | 6.2.9200.17623 / 6.2.9200.21743 - KB3121255 | 6.3.9600.18654 - KB4022726           | 10.0.14393.1198 - KB4022715          | 10.0.15063.447             |
|                         | Iologmsg.dll      | 6.1.7601.23403 - KB3125574         | 6.2.9200.16384 - KB2995387                  | -                                    | -                                    | -                          |
|                         | Classpnp.sys      | 6.1.7601.23403 - KB3125574         | 6.2.9200.17061 / 6.2.9200.21180 - KB2995387 | 6.3.9600.18334 - KB3172614           | 10.0.14393.953 - KB4022715           | -                          |
|                         | Volsnap.sys       | 6.1.7601.23403 - KB3125574         | 6.2.9200.17047 / 6.2.9200.21165 - KB2975331 | 6.3.9600.18265 - KB3145384           | -                                    | 10.0.15063.0               |
|                         | PartMgr.sys       | 6.1.7601.23403 - KB3125574         | 6.2.9200.16681 - KB2877114                  | 6.3.9600.17401 - KB3000850           | 10.0.14393.953 - KB4022715           | 10.0.15063.0               |
|                         | volmgr.sys        |                                    |                                             |                                      |                                      | 10.0.15063.0               |
|                         | Volmgrx.sys       | 6.1.7601.23403 - KB3125574         | -                                           | -                                    | -                                    | 10.0.15063.0               |
|                         | Msiscsi.sys       | 6.1.7601.23403 - KB3125574         | 6.2.9200.21006 - KB2955163                  | 6.3.9600.18624 - KB4022726           | 10.0.14393.1066 - KB4022715          | 10.0.15063.447             |
|                         | MSDSM.sys         | 6.1.7601.23403 - KB3125574         | 6.2.9200.21474 - KB3046101                  | 6.3.9600.18592 - KB4022726           | -                                    | -                          |
|                         | MPIO.sys          | 6.1.7601.23403 - KB3125574         | 6.2.9200.21190 - KB3046101                  | 6.3.9600.18616 - KB4022726           | 10.0.14393.1198 - KB4022715          | -                          |
|                         | Fveapi.dll        | 6.1.7601.23311 - KB3125574         | 6.2.9200.20930 - KB2930244                  | 6.3.9600.18294 - KB3172614           | 10.0.14393.576 - KB4022715           | -                          |
|                         | Fveapibase.dll    | 6.1.7601.23403 - KB3125574         | 6.2.9200.20930 - KB2930244                  | 6.3.9600.17415 - KB3172614           | 10.0.14393.206 - KB4022715           | -                          |
| Síť                 | netvsc.sys        | -                                  | -                                           | -                                    | 10.0.14393.1198 - KB4022715          | 10.0.15063.250 - KB4020001 |
|                         | mrxsmb10.sys      | 6.1.7601.23816 - KB4022722         | 6.2.9200.22108 - KB4022724                  | 6.3.9600.18603 - KB4022726           | 10.0.14393.479 - KB4022715           | 10.0.15063.483             |
|                         | mrxsmb20.sys      | 6.1.7601.23816 - KB4022722         | 6.2.9200.21548 - KB4022724                  | 6.3.9600.18586 - KB4022726           | 10.0.14393.953 - KB4022715           | 10.0.15063.483             |
|                         | Mrxsmb.sys        | 6.1.7601.23816 - KB4022722         | 6.2.9200.22074 - KB4022724                  | 6.3.9600.18586 - KB4022726           | 10.0.14393.953 - KB4022715           | 10.0.15063.0               |
|                         | aktualizace         | 6.1.7601.23761 - KB4022722         | 6.2.9200.22070 - KB4022724                  | 6.3.9600.18478 - KB4022726           | 10.0.14393.1358 - KB4022715          | 10.0.15063.447             |
|                         | ovladač HTTP.sys          | 6.1.7601.23403 - KB3125574         | 6.2.9200.17285 - KB3042553                  | 6.3.9600.18574 - KB4022726           | 10.0.14393.251 - KB4022715           | 10.0.15063.483             |
|                         | vmswitch.sys      | 6.1.7601.23727 - KB4022719         | 6.2.9200.22117 - KB4022724                  | 6.3.9600.18654 - KB4022726           | 10.0.14393.1358 - KB4022715          | 10.0.15063.138             |
| Jádro                    | Ntoskrnl.exe      | 6.1.7601.23807 - KB4022719         | 6.2.9200.22170 - KB4022718                  | 6.3.9600.18696 - KB4022726           | 10.0.14393.1358 - KB4022715          | 10.0.15063.483             |
| Vzdálená plocha | rdpcorets.dll     | 6.2.9200.21506 - KB4022719         | 6.2.9200.22104 - KB4022724                  | 6.3.9600.18619 - KB4022726           | 10.0.14393.1198 - KB4022715          | 10.0.15063.0               |
|                         | TERMSRV.dll       | 6.1.7601.23403 - KB3125574         | 6.2.9200.17048 - KB2973501                  | 6.3.9600.17415 - KB3000850           | 10.0.14393.0 - KB4022715             | 10.0.15063.0               |
|                         | termdd.sys        | 6.1.7601.23403 - KB3125574         | -                                           | -                                    | -                                    | -                          |
|                         | Win32k.sys        | 6.1.7601.23807 - KB4022719         | 6.2.9200.22168 - KB4022718                  | 6.3.9600.18698 - KB4022726           | 10.0.14393.594 - KB4022715           | -                          |
|                         | rdpdd.dll         | 6.1.7601.23403 - KB3125574         | -                                           | -                                    | -                                    | -                          |
|                         | Rdpwd.sys         | 6.1.7601.23403 - KB3125574         | -                                           | -                                    | -                                    | -                          |
| Zabezpečení                | Kvůli tooWannaCrypt | KB4012212                          | KB4012213                                   | KB4012213                            | KB4012606                            | KB4012606                  |
|                         |                   |                                    | KB4012216                                   |                                      | KB4013198                            | KB4013198                  |
|                         |                   | KB4012215                          | KB4012214                                   | KB4012216                            | KB4013429                            | KB4013429                  |
|                         |                   |                                    | KB4012217                                   |                                      | KB4013429                            | KB4013429                  |
       
### Když toouse sysprep<a id="step23"></a>    

Nástroj Sysprep je proces, který můžete spustit do instalace systému windows, který obnoví hello instalace systému hello a budou tak poskytovat "předinstalované hello prostředí" odebráním všechna osobní data a resetování několik komponent. Obvykle to uděláte Pokud chcete, aby toocreate šablony, ze kterého můžete nasadit několik ostatních virtuálních počítačů, které mají určitou konfiguraci. Tento postup se nazývá **zobecněn image**.

Pokud místo toho chcete pouze toocreate jeden virtuální počítač z jednoho disku nemáte toouse nástroje sysprep. V této situaci můžete pouze vytvořit virtuální počítač z co se označuje jako hello **specializované image**.

Další informace o tom, jak toocreate virtuálního počítače z specializované disku, najdete v části:

- [Vytvoření virtuálního počítače z specializované disku](create-vm-specialized.md)
- [Vytvoření virtuálního počítače z specializované disku VHD](https://azure.microsoft.com/resources/templates/201-vm-specialized-vhd/)

Pokud chcete, aby toocreate zobecněný bitové kopie, musíte toorun nástroje sysprep. Další informace o nástroji Sysprep najdete v tématu [jak tooUse nástroje Sysprep: Úvod](http://technet.microsoft.com/library/bb457073.aspx). 

Ne každý role nebo aplikace, který je nainstalován na počítači se systémem Windows podporuje tento generalizace. Před použitím tohoto postupu, získáte následující článek toomake opravdu toohello dané role hello daný počítač podporuje nástroj sysprep. Další informace najdete [podpora nástroje Sysprep pro role serveru](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).

### <a name="steps-toogeneralize-a-vhd"></a>Kroky toogeneralize virtuální pevný disk

>[!NOTE]
> Po spuštění sysprep.exe jako zadaný v hello následující kroky, vypněte hello virtuálních počítačů a nejsou znovu zapnout až do vytvoření bitové kopie z něj v Azure.

1. Přihlaste se toohello virtuální počítač s Windows.
2. Spustit **příkazového řádku** jako správce. 
3. Změnit adresář hello: **%windir%\system32\sysprep**a poté spusťte **sysprep.exe**.
3. V hello **nástroj pro přípravu systému** dialogové okno, vyberte **prostředí Out-of-Box zadejte systému (při prvním zapnutí)**a ujistěte se, že hello **generalizace** je zaškrtnuté políčko.

    ![Nástroj pro přípravu systému](media/prepare-for-upload-vhd-image/syspre.png)
4. V **možnosti vypnutí**, vyberte **vypnutí**.
5. Klikněte na **OK**.
6. Po dokončení nástroj Sysprep, vypněte hello virtuálních počítačů. Nepoužívejte **restartujte** tooshut dolů hello virtuálních počítačů.
7. Hello virtuálního pevného disku je nyní připraven toobe nahrát. Další informace o tom, jak toocreate virtuální počítač z zobecněný disk, najdete v části [nahrát zobecněný virtuální pevný disk a použít ho toocreate nové virtuální počítače v Azure](sa-upload-generalized.md).


## <a name="complete-recommended-configurations"></a>Dokončete doporučené konfigurace
Hello následující nastavení neovlivní odesílání virtuálního pevného disku. Ale důrazně doporučujeme nakonfigurovat je.

* Nainstalujte hello [agenta virtuální počítače Azure](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Potom můžete povolit rozšíření virtuálního počítače. rozšíření virtuálního počítače Hello implementovat většinu hello důležité funkce, může být vhodné toouse pomocí vašich virtuálních počítačů jako je resetování hesla, konfigurace RDP a tak dále. Další informace naleznete v tématu:

    - [Agent virtuálního počítače a rozšíření – část 1](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-1/)
    - [Agent virtuálního počítače a rozšíření – část 2](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/)
* Hello výpisu protokolu může být užitečné při řešení problémů s havárií systému Windows. Povolte hello výpisu protokolu kolekce:
  
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\CrashControl' -name "CrashDumpEnable" -Value "2" -Type DWord
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\CrashControl' -name "DumpFile" -Value "%SystemRoot%\MEMORY.DMP"
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\CrashControl' -name "AutoReboot" -Value 0 -Type DWord
    New-Item -Path 'HKLM:\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps'
    New-ItemProperty -Path 'HKLM:\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps' -name "DumpFolder" -Value "c:\CrashDumps"
    New-ItemProperty -Path 'HKLM:\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps' -name "DumpCount" -Value 10 -Type DWord
    New-ItemProperty -Path 'HKLM:\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps' -name "DumpType" -Value 2 -Type DWord
    Set-Service -Name WerSvc -StartupType Manual
    ```
    Pokud se zobrazí všechny chyby během žádné procedurální hello kroky v tomto článku, to znamená, že klíče registru hello již existuje. V takovém případě použijte hello místo následující příkazy:

    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\CrashControl' -name "CrashDumpEnable" -Value "2" -Type DWord
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\CrashControl' -name "DumpFile" -Value "%SystemRoot%\MEMORY.DMP"
    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps' -name "DumpFolder" -Value "c:\CrashDumps"
    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps' -name "DumpCount" -Value 10 -Type DWord
    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps' -name "DumpType" -Value 2 -Type DWord
    Set-Service -Name WerSvc -StartupType Manual
    ```
*  Po vytvoření hello virtuálních počítačů v Azure doporučujeme umístit hello stránkovacího souboru na výkon tooimprove svazku "Jednotka dočasné" hello. Můžete nastavit to následujícím způsobem:

    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management' -name "PagingFiles" -Value "D:\pagefile"
    ```
Pokud je datový disk, který je připojený toohello virtuálních počítačů, písmeno jednotky svazku hello dočasné jednotky je obvykle "D." Toto označení se může lišit v závislosti na počtu hello dostupných jednotek a hello nastavení, které provedete.

## <a name="next-steps"></a>Další kroky
* [Nahrát tooAzure bitové kopie virtuální počítač s Windows pro nasazení Resource Manager](upload-generalized-managed.md)

