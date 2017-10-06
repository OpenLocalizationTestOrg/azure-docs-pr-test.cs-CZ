---
title: "aaaConnect vzdáleně tooyour zařízení StorSimple | Microsoft Docs"
description: "Vysvětluje, jak tooconfigure zařízení pro vzdálenou správu a jak tooconnect tooWindows Powershellu pro StorSimple prostřednictvím protokolu HTTP nebo HTTPS."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/07/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 38b6a6350891b9f6f8fdfc55880b2f47105d947c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-remotely-tooyour-storsimple-8000-series-device"></a>Vzdálené připojení zařízení řady StorSimple 8000 tooyour

## <a name="overview"></a>Přehled

Můžete vzdáleně připojit zařízení tooyour pomocí prostředí Windows PowerShell. Když připojíte tímto způsobem, neuvidíte žádné nabídky. (Zobrazí nabídky pouze v případě, že používáte hello konzoly sériového portu v zařízení tooconnect hello.) Díky vzdálené komunikace Windows Powershellu připojit tooa konkrétní prostředí runspace. Můžete také zadat hello jazyk zobrazení.

Další informace o použití toomanage vzdálenou komunikaci prostředí Windows PowerShell zařízení, přejděte příliš[pomocí Windows Powershellu pro StorSimple tooadminister zařízení StorSimple](storsimple-8000-windows-powershell-administration.md).

Tento kurz vysvětluje, jak tooconfigure zařízení pro vzdálenou správu a pak tooconnect tooWindows Powershellu pro StorSimple. Můžete použít protokol HTTP nebo HTTPS tooremotely připojit prostřednictvím Windows PowerShell. Ale při rozhodování jak tooconnect tooWindows Powershellu pro StorSimple, zvažte hello následující informace:

* Připojení přímo toohello konzoly sériového portu zařízení je bezpečné, ale není připojování konzoly sériového portu toohello přes síťové přepínače. Buďte opatrní hello rizika zabezpečení při připojování konzoly sériového portu zařízení toohello přes síťové přepínače.
* Připojení přes relaci protokolu HTTP může nabízí lepší zabezpečení než připojení prostřednictvím sériové konzoly hello přes síť hello. I když to není hello nejbezpečnější metodou, je přijatelné v důvěryhodných sítích.
* Připojení prostřednictvím relace HTTPS se certifikát podepsaný svým držitelem je hello nejbezpečnější a doporučená možnost hello.

Můžete vzdáleně připojit toohello rozhraní Windows PowerShell. Zařízení StorSimple tooyour vzdáleného přístupu prostřednictvím rozhraní Windows PowerShell hello však není povoleno ve výchozím nastavení. Musíte nejdřív povolit vzdálenou správu na hello zařízení a potom na hello klienta, který je použité tooaccess zařízení.

Hello kroky popsané v tomto článku byly provedeny v hostitelském systému, systémem Windows Server 2012 R2.

## <a name="connect-through-http"></a>Připojení prostřednictvím protokolu HTTP

Připojení tooWindows Powershellu pro StorSimple přes relaci protokolu HTTP poskytuje lepší zabezpečení než připojení prostřednictvím sériové konzoly hello zařízení StorSimple. I když to není hello nejbezpečnější metodou, je přijatelné v důvěryhodných sítích.

Můžete použít buď hello portálu nebo hello konzoly sériového portu tooconfigure vzdálenou správu Azure. Vyberte z následujících postupů hello:

* [Používání hello Azure portálu tooenable vzdálené správy přes protokol HTTP](#use-the-azure-classic-portal-to-enable-remote-management-over-http)
* [Používání hello konzoly sériového portu tooenable vzdálené správy přes protokol HTTP](#use-the-serial-console-to-enable-remote-management-over-http)

Po povolení vzdálené správy, použijte následující postup tooprepare hello klienta pro připojení ke vzdálené hello.

* [Připravit hello klienta pro připojení ke vzdálené](#prepare-the-client-for-remote-connection)

### <a name="use-hello-azure-portal-tooenable-remote-management-over-http"></a>Používání hello Azure portálu tooenable vzdálené správy přes protokol HTTP

Proveďte následující kroky v hello Azure portálu tooenable vzdálené správy přes protokol HTTP hello.

#### <a name="tooenable-remote-management-through-hello-azure-portal"></a>tooenable vzdálenou správu prostřednictvím hello portálu Azure

1. Přejděte služby StorSimple Manager zařízení tooyour. Vyberte **zařízení** a pak vyberte a klikněte na zařízení hello chcete tooconfigure pro vzdálenou správu. Přejděte příliš**nastavení zařízení > zabezpečení**.
2. V hello **nastavení zabezpečení** okně klikněte na tlačítko **vzdálenou správu**.
3. V hello **vzdálenou správu** okně nastavit **povolit vzdálenou správu** příliš**Ano**.
4. Teď můžete zvolit tooconnect pomocí protokolu HTTP. (výchozí hello je tooconnect přes protokol HTTPS.) Ujistěte se, že je vybraný HTTP.
   
   > [!NOTE]
   > Připojení pomocí protokolu HTTP je přijatelné jenom v důvěryhodných sítích.
   
5. Klikněte na tlačítko **Uložit** a po zobrazení výzvy k potvrzení, vyberte **Ano**.

### <a name="use-hello-serial-console-tooenable-remote-management-over-http"></a>Používání hello konzoly sériového portu tooenable vzdálené správy přes protokol HTTP
Proveďte následující kroky na hello zařízení konzoly sériového portu tooenable vzdálenou správu hello.

#### <a name="tooenable-remote-management-through-hello-device-serial-console"></a>tooenable vzdálenou správu prostřednictvím konzoly sériového portu zařízení hello
1. V nabídce konzoly sériového portu hello vyberte možnost 1. Další informace o používání konzoly sériového portu hello na hello zařízení, přejděte příliš[připojení tooWindows Powershellu pro StorSimple prostřednictvím konzoly sériového portu zařízení](storsimple-8000-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console).
2. Hello řádku zadejte:`Enable-HcsRemoteManagement –AllowHttp`
3. Jsou oznámení o ohrožení zabezpečení hello pomocí protokolu HTTP tooconnect toohello zařízení. Po zobrazení výzvy potvrďte zadáním **Y**.
4. Ověřte, že je povolený protokol HTTP zadáním:`Get-HcsSystem`
5. Ověřte, že hello **RemoteManagementMode** pole ukazuje **HttpsAndHttpEnabled**.hello následující obrázek ukazuje tato nastavení v PuTTY.
   
     ![Sériového portu HTTPS a HTTP povoleno](./media/storsimple-remote-connect/HCS_SerialHttpsAndHttpEnabled.png)

### <a name="prepare-hello-client-for-remote-connection"></a>Připravit hello klienta pro připojení ke vzdálené
Proveďte následující kroky na vzdálenou správu serveru signálu hello klienta tooenable hello.

#### <a name="tooprepare-hello-client-for-remote-connection"></a>tooprepare hello klienta pro připojení ke vzdálené
1. Spusťte relaci prostředí Windows PowerShell jako správce.
2. Zadejte následující příkaz tooadd hello IP adresu ze seznamu důvěryhodných hostitelů hello StorSimple zařízení toohello klienta hello:
   
     `Set-Item wsman:\localhost\Client\TrustedHosts <device_ip> -Concatenate -Force`
   
     Nahraďte <*device_ip*> s hello IP adresa vašeho zařízení; například: 
   
     `Set-Item wsman:\localhost\Client\TrustedHosts 10.126.173.90 -Concatenate -Force`
3. Zadejte následující příkaz toosave hello přihlašovací údaje zařízení v proměnné hello: 
   
    ```
    $cred = Get-Credential
    ```
    
4. V dialogovém okně hello které se zobrazí:
   
   1. Zadejte jméno uživatele hello v tomto formátu: *device_ip\SSAdmin*.
   2. Zadejte heslo správce zařízení hello, který byl nastaven při konfiguraci zařízení hello pomocí Průvodce instalací hello. výchozí heslo Hello je *Heslo1*.
5. Spusťte relaci prostředí Windows PowerShell na zařízení hello zadáním následujícího příkazu:
   
     `Enter-PSSession -Credential $cred -ConfigurationName SSAdminConsole -ComputerName <device_ip>`
   
   > [!NOTE]
   > toocreate relaci prostředí Windows PowerShell pro použití s hello virtuálního zařízení StorSimple, připojit hello `–Port` parametr a zadejte hello veřejný port, který jste nakonfigurovali v vzdálenou komunikaci pro virtuální zařízení StorSimple.
   
   
V tomto okamžiku by měl mít zařízení aktivní toohello vzdálené relace prostředí Windows PowerShell.
   
![Vzdálená komunikace prostředí PowerShell pomocí protokolu HTTP](./media/storsimple-remote-connect/HCS_PSRemotingUsingHTTP.png)

## <a name="connect-through-https"></a>Připojení přes protokol HTTPS

Připojení tooWindows Powershellu pro StorSimple prostřednictvím relace HTTPS je hello nejbezpečnější a doporučená metoda zařízení Microsoft Azure StorSimple pro vzdálené připojení tooyour. Hello následující postupy popisují, jak tooset až hello sériové konzoly a klientské počítače tak, aby HTTPS tooconnect tooWindows prostředí PowerShell můžete použít pro StorSimple.

Můžete použít buď hello portálu nebo hello konzoly sériového portu tooconfigure vzdálenou správu Azure. Vyberte z následujících postupů hello:

* [Použít hello Azure portálu tooenable vzdálené správy přes protokol HTTPS](#use-the-azure-classic-portal-to-enable-remote-management-over-https)
* [Použít hello konzoly sériového portu tooenable vzdálené správy přes protokol HTTPS](#use-the-serial-console-to-enable-remote-management-over-https)

Po povolení vzdálené správy, použijte následující postupy tooprepare hello hostitele pro vzdálenou správu hello a připojení zařízení toohello hello vzdáleného hostitele.

* [Příprava pro vzdálenou správu hostitele hello](#prepare-the-host-for-remote-management)
* [Připojte zařízení toohello hello vzdáleného hostitele](#connect-to-the-device-from-the-remote-host)

### <a name="use-hello-azure-portal-tooenable-remote-management-over-https"></a>Použít hello Azure portálu tooenable vzdálené správy přes protokol HTTPS

Proveďte následující kroky v hello Azure portálu tooenable vzdálené správy přes protokol HTTPS hello.

#### <a name="tooenable-remote-management-over-https-from-hello-azure-portal"></a>tooenable vzdálené správy přes protokol HTTPS z hello portálu Azure

1. Přejděte služby StorSimple Manager zařízení tooyour. Vyberte **zařízení** a pak vyberte a klikněte na zařízení hello chcete tooconfigure pro vzdálenou správu. Přejděte příliš**nastavení zařízení > zabezpečení**.
2. V hello **nastavení zabezpečení** okně klikněte na tlačítko **vzdálenou správu**.
3. Nastavit **povolit vzdálenou správu** příliš**Ano**.
4. Teď můžete zvolit tooconnect pomocí protokolu HTTPS. (výchozí hello je tooconnect přes protokol HTTPS.) Ujistěte se, že je vybraný protokol HTTPS.
5. Klikněte na tlačítko... a pak klikněte na **Download Remote Management Certificate**. Zadejte umístění toosave tento soubor. Je nutné tento certifikát hello klientský nebo hostitelský počítač, že používáte zařízení toohello tooconnect tooinstall.
6. Klikněte na tlačítko **Uložit** a pak klikněte na **Ano** po zobrazení výzvy k potvrzení.

### <a name="use-hello-serial-console-tooenable-remote-management-over-https"></a>Použít hello konzoly sériového portu tooenable vzdálené správy přes protokol HTTPS

Proveďte následující kroky na hello zařízení konzoly sériového portu tooenable vzdálenou správu hello.

#### <a name="tooenable-remote-management-through-hello-device-serial-console"></a>tooenable vzdálenou správu prostřednictvím konzoly sériového portu zařízení hello
1. V nabídce konzoly sériového portu hello vyberte možnost 1. Další informace o používání konzoly sériového portu hello na hello zařízení, přejděte příliš[připojení tooWindows Powershellu pro StorSimple prostřednictvím konzoly sériového portu zařízení](storsimple-8000-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console).
2. Hello řádku zadejte:
   
     `Enable-HcsRemoteManagement`
   
    To by měl povolit protokol HTTPS na vašem zařízení.
3. Ověřte, že je povoleno HTTPS zadáním: 
   
     `Get-HcsSystem`
   
    Ujistěte se, že hello **RemoteManagementMode** pole ukazuje **HttpsEnabled**.hello následující obrázek ukazuje tato nastavení v PuTTY.
   
     ![Sériového portu HTTPS povoleno](./media/storsimple-remote-connect/HCS_SerialHttpsEnabled.png)
4. Z výstupu hello `Get-HcsSystem`, zkopírujte sériové číslo zařízení hello hello a uložit pro pozdější použití.
   
   > [!NOTE]
   > sériové číslo Hello mapuje toohello název CN certifikátu hello.
   
5. Získejte certifikát pro vzdálenou správu zadáním: 
   
     `Get-HcsRemoteManagementCert`
   
    Zobrazí se následující toohello podobně jako na certifikát.
   
    ![Získání certifikátu vzdálené správy](./media/storsimple-remote-connect/HCS_GetRemoteManagementCertificate.png)
6. Zkopírujte hello informace v certifikátu hello z **---BEGIN CERTIFICATE---** příliš**---END CERTIFICATE---** do textového editoru, například Poznámkový blok a uložte ho jako soubor .cer. (Tento soubor tooyour vzdálený hostitel se zkopírujte při přípravě hello hostitele.)
   
   > [!NOTE]
   > toogenerate nový certifikát, použijte hello `Set-HcsRemoteManagementCert` rutiny.
   
### <a name="prepare-hello-host-for-remote-management"></a>Příprava pro vzdálenou správu hostitele hello

tooprepare hello hostitelského počítače pro vzdálené připojení, který používá relace HTTPS, proveďte následující postupy hello:

* [Soubor .cer hello importu do kořenového úložiště hello hello klienta nebo vzdálený hostitel](#to-import-the-certificate-on-the-remote-host).
* [Přidání souboru hostitelů hello zařízení sériová čísla toohello na vzdáleného hostitele](#to-add-device-serial-numbers-to-the-remote-host).

Každá z předchozích postupech hello je popsána níže.

#### <a name="tooimport-hello-certificate-on-hello-remote-host"></a>tooimport hello certifikát u vzdáleného hostitele hello
1. Klikněte pravým tlačítkem na soubor .cer hello a vyberte **instalace certifikátu**. Tím se spustí hello Průvodce importem certifikátu.
   
    ![Průvodce importem certifikátu 1](./media/storsimple-remote-connect/HCS_CertificateImportWizard1.png)
2. Pro **umístění úložiště**, vyberte **místního počítače**a potom klikněte na **Další**.
3. Vyberte **všechny certifikáty umístit v následujícím úložiště hello**a potom klikněte na **Procházet**. Přejděte toohello kořenového úložiště vzdáleného hostitele a pak klikněte na tlačítko **Další**.
   
    ![Průvodce importem certifikátu 2](./media/storsimple-remote-connect/HCS_CertificateImportWizard2.png)
4. Klikněte na **Dokončit**. Zobrazí zpráva informující o úspěšném importu hello.
   
    ![Průvodce importem certifikátu 3](./media/storsimple-remote-connect/HCS_CertificateImportWizard3.png)

#### <a name="tooadd-device-serial-numbers-toohello-remote-host"></a>tooadd zařízení sériová čísla toohello vzdáleného hostitele
1. Spusťte jako správce program Poznámkový blok a pak otevřete soubor hosts hello umístěný ve \Windows\System32\Drivers\etc.
2. Přidejte následující tři položky tooyour hostitele soubor hello: **DATA 0 IP adresu**, **řadič 0 pevné IP adresy**, a **řadič 1 pevné IP adresy**.
3. Zadejte sériové číslo zařízení hello, který jste předtím uložili. Tato IP adresa toohello mapování, jak ukazuje následující obrázek hello. Pro řadič 0 a řadič 1 připojit **Controller0** a **Controller1** na konci hello hello sériové číslo (CN název).
   
    ![Přidání souboru toohosts název CN](./media/storsimple-remote-connect/HCS_AddingCNNameToHostsFile.png)
4. Uložení souboru hostitelů hello.

### <a name="connect-toohello-device-from-hello-remote-host"></a>Připojte zařízení toohello hello vzdáleného hostitele

Pomocí prostředí Windows PowerShell a SSL tooenter na SSAdmin relaci na vašem zařízení ze vzdáleného hostitele nebo klienta. relace SSAdmin Hello mapuje toooption 1 v hello [konzoly sériového portu](storsimple-8000-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console) nabídky vašeho zařízení.

Proveďte následující postup u hello počítačů, ze kterého mají být připojení ke vzdálené prostředí Windows PowerShell toomake hello hello.

#### <a name="tooenter-an-ssadmin-session-on-hello-device-by-using-windows-powershell-and-ssl"></a>tooenter na relaci SSAdmin na zařízení hello pomocí prostředí Windows PowerShell a SSL
1. Spusťte relaci prostředí Windows PowerShell jako správce.
2. Přidání důvěryhodných hostitelů hello zařízení IP adres toohello klienta zadáním:
   
     `Set-Item wsman:\localhost\Client\TrustedHosts <device_ip> -Concatenate -Force`
   
    Kde <*device_ip*> je hello IP adresa zařízení, třeba: 
   
     `Set-Item wsman:\localhost\Client\TrustedHosts 10.126.173.90 -Concatenate -Force`
3. toocreate nových přihlašovacích údajů, zadejte:
   
     `$cred = New-Object pscredential @("<IP of target device>\SSAdmin", (ConvertTo-SecureString -Force -AsPlainText "<Device Administrator Password>"))`
   
    Kde <*IP cílové zařízení*> je IP adresa hello DATA 0 pro vaše zařízení; například **10.126.173.90** jak je znázorněno v předchozích bitovou kopii souboru hostitelů hello hello. Zadejte také hello heslo správce pro vaše zařízení.
4. Vytvořte relaci zadáním:
   
     `$session = New-PSSession -UseSSL -ComputerName <Serial number of target device> -Credential $cred -ConfigurationName "SSAdminConsole"`
   
    Pro parametr - ComputerName hello v hello rutiny, zadejte hello <*sériové číslo cílového zařízení*>. Toto sériové číslo bylo mapováno toohello IP adresu DATA 0 v souboru hostitelů hello na vzdáleném hostiteli; například **SHX0991003G44MT** jak ukazuje následující obrázek hello.
5. Zadejte:
   
     `Enter-PSSession $session`
6. Budete potřebovat toowait několik minut a pak bude zařízení připojených tooyour přes HTTPS přes protokol SSL. Zobrazí zprávu, která označuje, že jsou připojené tooyour zařízení.
   
    ![Vzdálená komunikace prostředí PowerShell pomocí protokolu HTTPS a SSL](./media/storsimple-remote-connect/HCS_PSRemotingUsingHTTPSAndSSL.png)

## <a name="next-steps"></a>Další kroky

* Další informace o [pomocí prostředí Windows PowerShell tooadminister zařízení StorSimple](storsimple-8000-windows-powershell-administration.md).
* Další informace o [pomocí hello tooadminister service Manager zařízení StorSimple zařízení StorSimple](storsimple-8000-manager-service-administration.md).

