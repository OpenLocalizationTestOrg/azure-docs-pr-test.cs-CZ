---
title: "Konfigurace zabezpečení aaaSplit sloučení | Microsoft Docs"
description: "Nastavit x409 certifikáty pro šifrování"
metakeywords: Elastic Database certificates security
services: sql-database
documentationcenter: 
manager: jhubbard
author: torsteng
ms.assetid: f9e89c57-61a0-484f-b787-82dae2349cb6
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/27/2016
ms.author: torsteng
ms.openlocfilehash: 511c04be0598d8a0889aa3e3fcf02be0bf0e96cb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="split-merge-security-configuration"></a>Konfigurace zabezpečení rozdělení sloučení
toouse hello rozdělení či sloučení služby, je nutné správně nakonfigurovat zabezpečení. Služba Hello je součástí funkce hello elastické škálování sady Microsoft Azure SQL Database. Další informace najdete v tématu [elastické škálování rozdělení a sloučení kurz služby](sql-database-elastic-scale-configure-deploy-split-and-merge.md).

## <a name="configuring-certificates"></a>Konfigurace certifikátů
Certifikáty jsou nakonfigurovat dvěma způsoby. 

1. [tooConfigure hello certifikát SSL](#to-configure-the-ssl-certificate)
2. [tooConfigure klientské certifikáty](#to-configure-client-certificates) 

## <a name="tooobtain-certificates"></a>tooobtain certifikáty
Certifikáty nelze získat z veřejné certifikační autority (CA) nebo z hello [certifikát služby systému Windows](http://msdn.microsoft.com/library/windows/desktop/aa376539.aspx). Jedná se o upřednostňovaný hello metody tooobtain certifikáty.

Pokud nejsou k dispozici tyto možnosti, můžete vygenerovat **certifikáty podepsané svým držitelem**.

## <a name="tools-toogenerate-certificates"></a>Nástroje pro certifikáty toogenerate
* [MakeCert.exe](http://msdn.microsoft.com/library/bfsktky3.aspx)
* [Pvk2pfx.exe](http://msdn.microsoft.com/library/windows/hardware/ff550672.aspx)

### <a name="toorun-hello-tools"></a>toorun hello nástroje
* Z příkazového řádku pro vývojáře Visual Studia, najdete v části [Visual Studio – příkazový řádek](http://msdn.microsoft.com/library/ms229859.aspx) 
  
    Pokud nainstalovaná, přejděte na:
  
        %ProgramFiles(x86)%\Windows Kits\x.y\bin\x86 
* Získat hello WDK z [Windows 8.1: stažení sad a nástrojů](http://msdn.microsoft.com/windows/hardware/gg454513#drivers)

## <a name="tooconfigure-hello-ssl-certificate"></a>certifikát SSL tooconfigure hello
Certifikát SSL požadovaný tooencrypt hello komunikace a ověření serveru hello. Zvolte hello nejvhodnějšímu hello tři scénáře a spusťte všechny jeho kroky:

### <a name="create-a-new-self-signed-certificate"></a>Vytvořit nový certifikát podepsaný svým držitelem
1. [Vytvořit certifikát podepsaný svým držitelem](#create-a-self-signed-certificate)
2. [Vytvoření souboru PFX pro certifikát SSL podepsaný svým držitelem](#create-pfx-file-for-self-signed-ssl-certificate)
3. [Nahrajte certifikát SSL tooCloud služby](#upload-ssl-certificate-to-cloud-service)
4. [Aktualizovat certifikát SSL v konfiguračním souboru služby](#update-ssl-certificate-in-service-configuration-file)
5. [Import certifikační autorita protokolu SSL](#import-ssl-certification-authority)

### <a name="toouse-an-existing-certificate-from-hello-certificate-store"></a>uložení toouse existující certifikát od hello certifikátu
1. [Exportovat certifikát SSL z úložiště certifikátů.](#export-ssl-certificate-from-certificate-store)
2. [Nahrajte certifikát SSL tooCloud služby](#upload-ssl-certificate-to-cloud-service)
3. [Aktualizovat certifikát SSL v konfiguračním souboru služby](#update-ssl-certificate-in-service-configuration-file)

### <a name="toouse-an-existing-certificate-in-a-pfx-file"></a>toouse existující certifikát v souboru PFX
1. [Nahrajte certifikát SSL tooCloud služby](#upload-ssl-certificate-to-cloud-service)
2. [Aktualizovat certifikát SSL v konfiguračním souboru služby](#update-ssl-certificate-in-service-configuration-file)

## <a name="tooconfigure-client-certificates"></a>tooconfigure klientské certifikáty
Klientské certifikáty se vyžadují v pořadí tooauthenticate požadavky toohello služby. Zvolte hello nejvhodnějšímu hello tři scénáře a spusťte všechny jeho kroky:

### <a name="turn-off-client-certificates"></a>Vypnout klientské certifikáty
1. [Vypněte ověřování na základě certifikátu klienta](#turn-off-client-certificate-based-authentication)

### <a name="issue-new-self-signed-client-certificates"></a>Vydat nové certifikáty podepsané svým držitelem klienta
1. [Vytvořit podepsaný certifikační autoritou](#create-a-self-signed-certification-authority)
2. [Nahrajte certifikát certifikační Autority tooCloud služby](#upload-ca-certificate-to-cloud-service)
3. [Aktualizovat certifikát certifikační Autority v konfiguračním souboru služby](#update-ca-certificate-in-service-configuration-file)
4. [Problém klientské certifikáty](#issue-client-certificates)
5. [Vytvářet soubory PFX pro certifikáty klientů](#create-pfx-files-for-client-certificates)
6. [Import certifikátu klienta](#Import-Client-Certificate)
7. [Zkopírujte kryptografické otisky certifikátu klienta](#copy-client-certificate-thumbprints)
8. [Konfigurace klientů povolené v hello konfigurační soubor služby](#configure-allowed-clients-in-the-service-configuration-file)

### <a name="use-existing-client-certificates"></a>Použít existující klientské certifikáty
1. [Najít veřejného klíče certifikační Autority](#find-ca-public-key)
2. [Nahrajte certifikát certifikační Autority tooCloud služby](#Upload-CA-certificate-to-cloud-service)
3. [Aktualizovat certifikát certifikační Autority v konfiguračním souboru služby](#Update-CA-Certificate-in-Service-Configuration-File)
4. [Zkopírujte kryptografické otisky certifikátu klienta](#Copy-Client-Certificate-Thumbprints)
5. [Konfigurace klientů povolené v hello konfigurační soubor služby](#configure-allowed-clients-in-the-service-configuration-file)
6. [Konfigurace Kontrola odvolání certifikátu klienta](#Configure-Client-Certificate-Revocation-Check)

## <a name="allowed-ip-addresses"></a>Povolené IP adresy
Koncové body služby toohello přístup může být omezená toospecific rozsahy IP adres.

## <a name="tooconfigure-encryption-for-hello-store"></a>tooconfigure šifrování pro úložiště hello
Certifikát je požadovaná tooencrypt hello pověření, které jsou uloženy v úložišti metadat hello. Zvolte hello nejvhodnějšímu hello tři scénáře a spusťte všechny jeho kroky:

### <a name="use-a-new-self-signed-certificate"></a>Použití nového certifikátu podepsaného svým držitelem
1. [Vytvořit certifikát podepsaný svým držitelem](#create-a-self-signed-certificate)
2. [Vytvořit soubor PFX pro šifrovací certifikát podepsaný svým držitelem](#create-pfx-file-for-self-signed-ssl-certificate)
3. [Nahrajte certifikát pro šifrování tooCloud služby](#upload-encryption-certificate-to-cloud-service)
4. [Aktualizovat certifikát šifrování v konfiguračním souboru služby](#update-encryption-certificate-in-service-configuration-file)

### <a name="use-an-existing-certificate-from-hello-certificate-store"></a>Použít stávající certifikát z úložiště certifikátů hello
1. [Exportovat šifrovací certifikát z úložiště certifikátů.](#export-encryption-certificate-from-certificate-store)
2. [Nahrajte certifikát pro šifrování tooCloud služby](#upload-encryption-certificate-to-cloud-service)
3. [Aktualizovat certifikát šifrování v konfiguračním souboru služby](#update-encryption-certificate-in-service-configuration-file)

### <a name="use-an-existing-certificate-in-a-pfx-file"></a>Použít stávající certifikát v souboru PFX
1. [Nahrajte certifikát pro šifrování tooCloud služby](#upload-encryption-certificate-to-cloud-service)
2. [Aktualizovat certifikát šifrování v konfiguračním souboru služby](#update-encryption-certificate-in-service-configuration-file)

## <a name="hello-default-configuration"></a>Hello výchozí konfigurace
Výchozí konfigurace Hello odmítne koncový bod HTTP toohello všechny přístup. Toto je doporučená nastavení, protože hello požadavky toothese koncové body mohou provádět citlivé údaje, jako jsou přihlašovací údaje databáze hello.
Výchozí konfigurace Hello umožňuje koncový bod HTTPS toohello všechny přístup. Toto nastavení může být další s omezeným přístupem.

### <a name="changing-hello-configuration"></a>Změna hello konfigurace
Hello skupiny pravidly řízení přístupu, které se vztahují tooand koncový bod se konfigurují v hello  **<EndpointAcls>**  část v hello **konfigurační soubor služby**.

    <EndpointAcls>
      <EndpointAcl role="SplitMergeWeb" endPoint="HttpIn" accessControl="DenyAll" />
      <EndpointAcl role="SplitMergeWeb" endPoint="HttpsIn" accessControl="AllowAll" />
    </EndpointAcls>

Hello pravidel ve skupině řízení přístupu, které jsou nakonfigurované v <AccessControl name=""> části konfigurační soubor služby hello. 

Formát Hello je vysvětleno v dokumentaci seznamů řízení přístupu k síti.
Například tooallow pouze IP adresy v hello rozsah 100.100.0.0 too100.100.255.255 tooaccess hello koncový bod HTTPS, pravidla hello bude vypadat takto:

    <AccessControl name="Retricted">
      <Rule action="permit" description="Some" order="1" remoteSubnet="100.100.0.0/16"/>
      <Rule action="deny" description="None" order="2" remoteSubnet="0.0.0.0/0" />
    </AccessControl>
    <EndpointAcls>
    <EndpointAcl role="SplitMergeWeb" endPoint="HttpsIn" accessControl="Restricted" />

## <a name="denial-of-service-prevention"></a>Odmítnutí služby prevence
Existují dva různé mechanismy podporované toodetect a zabránit útokům odmítnutí služby:

* Omezit počet souběžných požadavků na vzdáleného hostitele (ve výchozím nastavení vypnuté)
* Omezit rychlost přístupu na vzdáleného hostitele (na ve výchozím nastavení)

Tyto jsou založené na hello funkce, které jsou popsána v dynamické IP zabezpečení ve službě IIS. Při změně této konfigurace pozor hello následující faktory:

* chování Hello proxy servery a zařízení překlad síťových adres přes hello vzdáleného hostitele informace
* Každý prostředek tooany žádosti v hello webovou roli považuje za (například načítání skripty, obrázky atd.)

## <a name="restricting-number-of-concurrent-accesses"></a>Omezuje počet souběžných přístupů
jsou Hello nastavení, které konfiguraci tohoto chování:

    <Setting name="DynamicIpRestrictionDenyByConcurrentRequests" value="false" />
    <Setting name="DynamicIpRestrictionMaxConcurrentRequests" value="20" />

Změňte DynamicIpRestrictionDenyByConcurrentRequests tootrue tooenable tuto ochranu.

## <a name="restricting-rate-of-access"></a>Omezení rychlost přístupu
jsou Hello nastavení, které konfiguraci tohoto chování:

    <Setting name="DynamicIpRestrictionDenyByRequestRate" value="true" />
    <Setting name="DynamicIpRestrictionMaxRequests" value="100" />
    <Setting name="DynamicIpRestrictionRequestIntervalInMilliseconds" value="2000" />

## <a name="configuring-hello-response-tooa-denied-request"></a>Konfigurace hello odpovědi tooa odmítla žádost
Hello následující nastavení konfiguruje tooa odpovědi hello odmítla žádost:

    <Setting name="DynamicIpRestrictionDenyAction" value="AbortRequest" />
Pro ostatní podporované hodnoty naleznete v dokumentaci toohello pro dynamické IP zabezpečení ve službě IIS.

## <a name="operations-for-configuring-service-certificates"></a>Operace pro certifikáty služeb konfigurace
Toto téma se týká jenom pro referenci. Postupujte podle hello konfiguračních kroků uvedených v:

* Nakonfigurujte certifikát protokolu SSL hello
* Nakonfigurujte klientské certifikáty

## <a name="create-a-self-signed-certificate"></a>Vytvořit certifikát podepsaný svým držitelem
Spusťte:

    makecert ^
      -n "CN=myservice.cloudapp.net" ^
      -e MM/DD/YYYY ^
      -r -cy end -sky exchange -eku "1.3.6.1.5.5.7.3.1" ^
      -a sha1 -len 2048 ^
      -sv MySSL.pvk MySSL.cer

toocustomize:

* -n se adresa URL služby hello. Zástupné znaky ("CN = * .cloudapp .net") a jsou podporovány alternativní názvy (CN=myservice1.cloudapp.net, CN=myservice2.cloudapp.net").
* -e s datem vypršení platnosti certifikátu hello vytvořit silné heslo a zadejte jej po zobrazení výzvy.

## <a name="create-pfx-file-for-self-signed-ssl-certificate"></a>Vytvořit soubor PFX pro certifikát SSL podepsaný svým držitelem
Spusťte:

        pvk2pfx -pvk MySSL.pvk -spc MySSL.cer

Zadejte heslo a poté exportujte certifikát pomocí těchto možností:

* Ano, exportovat soukromý klíč hello
* Exportovat všechny rozšířené vlastnosti

## <a name="export-ssl-certificate-from-certificate-store"></a>Exportovat certifikát SSL z úložiště certifikátů.
* Najít certifikát
* Klikněte na tlačítko Akce -> všechny úlohy -> Export...
* Export certifikátu do. Soubor PFX pomocí těchto možností:
  * Ano, exportovat soukromý klíč hello
  * Pokud je to možné zahrnout všechny certifikáty na cestě k certifikátu hello * exportovat všechny rozšířené vlastnosti

## <a name="upload-ssl-certificate-toocloud-service"></a>Nahrát služba toocloud certifikátu SSL
Nahrajte certifikát s hello existující nebo vygenerovat. Soubor PFX s hello dvojici klíčů SSL:

* Zadejte heslo hello ochrana hello privátního klíče

## <a name="update-ssl-certificate-in-service-configuration-file"></a>Aktualizovat certifikát SSL v konfiguračním souboru služby
Aktualizace hodnoty kryptografického otisku hello hello následující nastavení v konfiguračním souboru služby hello s hello kryptografický otisk certifikátu hello nahrán toohello cloudové služby:

    <Certificate name="SSL" thumbprint="" thumbprintAlgorithm="sha1" />

## <a name="import-ssl-certification-authority"></a>Import certifikační autorita protokolu SSL
Postupujte podle těchto kroků v všechny účet nebo počítač, který bude komunikovat se službou hello:

* Dvakrát klikněte na hello. Soubor CER v Průzkumníku Windows
* V dialogovém okně hello certifikátu klikněte na tlačítko Nainstalovat certifikát...
* Import certifikátu do úložiště důvěryhodných kořenových certifikačních autorit hello

## <a name="turn-off-client-certificate-based-authentication"></a>Vypněte ověřování na základě certifikátu klienta
Je podporován pouze na základě certifikátu ověření klienta a jejím zakázáním umožní pro koncové body služby toohello veřejný přístup, pokud ostatní mechanismy jsou na místě (například Microsoft Azure Virtual Network).

Změna těchto nastavení toofalse v hello služby konfigurační soubor tooturn hello funkce:

    <Setting name="SetupWebAppForClientCertificates" value="false" />
    <Setting name="SetupWebserverForClientCertificates" value="false" />

V nastavení certifikátu hello certifikační Autority, zkopírujte hello certifikátu se stejným kryptografickým otiskem jako hello SSL:

    <Certificate name="CA" thumbprint="" thumbprintAlgorithm="sha1" />

## <a name="create-a-self-signed-certification-authority"></a>Vytvořit podepsaný certifikační autoritou
Provést následující kroky toocreate tooact certifikát podepsaný svým držitelem jako certifikační autorita hello:

    makecert ^
    -n "CN=MyCA" ^
    -e MM/DD/YYYY ^
     -r -cy authority -h 1 ^
     -a sha1 -len 2048 ^
      -sr localmachine -ss my ^
      MyCA.cer

toocustomize ho

* -e s datem vypršení platnosti certifikační hello

## <a name="find-ca-public-key"></a>Najít veřejného klíče certifikační Autority
Všechny certifikáty klienta musí být vydány certifikační autoritou důvěryhodné službou hello. Najít hello veřejného klíče toohello certifikační autorita, která vydala hello klientské certifikáty, které budou toobe pro ověřování v pořadí tooupload ho používají toohello cloudové služby.

Pokud není k dispozici soubor hello hello veřejným klíčem, můžete ho exportujte z úložiště certifikátů hello:

* Najít certifikát
  * Vyhledejte klientský certifikát vydaný hello stejné certifikační autority
* Dvakrát klikněte na certifikát hello.
* Vyberte kartu hello cestě k certifikátu v dialogovém okně pro certifikát hello.
* Dvakrát klikněte na položku hello certifikační Autority v cestě hello.
* Poznamenejte hello vlastnosti certifikátu.
* Zavřít hello **certifikát** dialogové okno.
* Najít certifikát
  * Vyhledejte hello certifikační Autority uvedených výše.
* Klikněte na tlačítko Akce -> všechny úlohy -> Export...
* Export certifikátu do. CER pomocí těchto možností:
  * **Ne, neexportovat privátní klíč hello**
  * Pokud je to možné zahrnout všechny certifikáty na cestě k certifikátu hello.
  * Exportujte všechny rozšířené vlastnosti.

## <a name="upload-ca-certificate-toocloud-service"></a>Nahrát služby toocloud certifikát certifikační Autority
Nahrajte certifikát s hello existující nebo vygenerovat. Soubor CER s veřejným klíčem hello certifikační Autority.

## <a name="update-ca-certificate-in-service-configuration-file"></a>Certifikát certifikační Autority aktualizace v konfiguračním souboru služby
Aktualizace hodnoty kryptografického otisku hello hello následující nastavení v konfiguračním souboru služby hello s hello kryptografický otisk certifikátu hello nahrán toohello cloudové služby:

    <Certificate name="CA" thumbprint="" thumbprintAlgorithm="sha1" />

Aktualizujte hodnotu hello hello následující nastavení se hello stejné kryptografický otisk:

    <Setting name="AdditionalTrustedRootCertificationAuthorities" value="" />

## <a name="issue-client-certificates"></a>Vystavovat certifikáty klienta
Každé jednotlivé autorizovaný tooaccess hello služby by měl mít klientský certifikát vydaný pro his/hers výhradní použití a by si měli vybrat, že his/hers vlastní silné heslo tooprotect jeho privátní klíč. 

Následující kroky Hello musí být spuštěn v hello byl vygenerované a uložené stejný počítač, kde hello certifikátu podepsaného svým držitelem certifikační Autority:

    makecert ^
      -n "CN=My ID" ^
      -e MM/DD/YYYY ^
      -cy end -sky exchange -eku "1.3.6.1.5.5.7.3.2" ^
      -a sha1 -len 2048 ^
      -in "MyCA" -ir localmachine -is my ^
      -sv MyID.pvk MyID.cer

Přizpůsobení:

* -n s ID pro toohello klienta, která bude ověřena pomocí tohoto certifikátu
* -e s datem vypršení platnosti certifikátu hello
* MyID.pvk a MyID.cer s jedinečné názvy souborů pro tento certifikát klienta

Tento příkaz zobrazí výzvu pro heslo toobe vytvořen a pak se použije jednou. Použijte silné heslo.

## <a name="create-pfx-files-for-client-certificates"></a>Vytvářet soubory PFX pro klientské certifikáty
Pro každý certifikát generovaného klienta spusťte příkaz:

    pvk2pfx -pvk MyID.pvk -spc MyID.cer

Přizpůsobení:

    MyID.pvk and MyID.cer with hello filename for hello client certificate

Zadejte heslo a poté exportujte certifikát pomocí těchto možností:

* Ano, exportovat soukromý klíč hello
* Exportovat všechny rozšířené vlastnosti
* jednotlivé toowhom Hello, kterou se vydává tento certifikát by měl vybrat heslo export hello

## <a name="import-client-certificate"></a>Import certifikátu klienta
Každého uživatele, pro kterého klientský certifikát vystavil naimportovat pár klíčů hello v hello počítače si pomocí toocommunicate službou hello:

* Dvakrát klikněte na hello. Soubor PFX v Průzkumníku Windows
* Import certifikátu do hello osobní úložiště s alespoň tuto možnost:
  * Zahrnout všechny rozšířené vlastnosti zaškrtnutí

## <a name="copy-client-certificate-thumbprints"></a>Zkopírujte kryptografické otisky certifikátu klienta
Každého uživatele, pro kterého klientský certifikát vystavil musí postupujte podle těchto kroků v pořadí tooobtain hello kryptografický otisk his/hers certifikát, který bude přidán konfigurační soubor služby toohello:

* Spustit certmgr.exe
* Vyberte kartu Osobní hello
* Klikněte dvakrát na certifikátu klienta hello toobe používaného pro ověřování
* V dialogu hello certifikátu, které se otevře vyberte kartu s podrobnostmi hello
* Ujistěte se, že všechny zobrazit zobrazení
* Vyberte hello pole s názvem kryptografický otisk v seznamu hello
* Zkopírujte hodnotu hello kryptografický otisk hello ** odstranit neviditelné znaky znakové sady Unicode před první číslice hello ** odstranit všechny mezery

## <a name="configure-allowed-clients-in-hello-service-configuration-file"></a>Konfigurace klientů povolené v konfiguračním souboru služby hello
Aktualizujte hodnotu hello hello následující nastavení v konfiguračním souboru služby hello čárkami oddělený seznam hello kryptografické otisky certifikátů klienta hello povolená služba toohello access:

    <Setting name="AllowedClientCertificateThumbprints" value="" />

## <a name="configure-client-certificate-revocation-check"></a>Konfigurace Kontrola odvolání certifikátu klienta
Výchozí nastavení Hello nekontroluje s hello certifikační autority pro stav odvolání certifikátu klienta. kontroluje tooturn na hello, pokud hello certifikační autority, který vydal hello klientské certifikáty podporuje tyto kontroly, změnit následující nastavení na jednu z hodnot hello definované v hello X509RevocationMode výčtu hello:

    <Setting name="ClientCertificateRevocationCheck" value="NoCheck" />

## <a name="create-pfx-file-for-self-signed-encryption-certificates"></a>Vytvořit soubor PFX pro certifikáty podepsané svým držitelem šifrování
Šifrovací certifikát spusťte tento příkaz:

    pvk2pfx -pvk MyID.pvk -spc MyID.cer

Přizpůsobení:

    MyID.pvk and MyID.cer with hello filename for hello encryption certificate

Zadejte heslo a poté exportujte certifikát pomocí těchto možností:

* Ano, exportovat soukromý klíč hello
* Exportovat všechny rozšířené vlastnosti
* Hello heslo budete potřebovat při nahrávání hello certifikát toohello cloudové služby.

## <a name="export-encryption-certificate-from-certificate-store"></a>Exportovat šifrovací certifikát z úložiště certifikátů.
* Najít certifikát
* Klikněte na tlačítko Akce -> všechny úlohy -> Export...
* Export certifikátu do. Soubor PFX pomocí těchto možností: 
  * Ano, exportovat soukromý klíč hello
  * Pokud je to možné zahrnout všechny certifikáty na cestě k certifikátu hello 
* Exportovat všechny rozšířené vlastnosti

## <a name="upload-encryption-certificate-toocloud-service"></a>Nahrát šifrovací certifikát toocloud služby
Nahrajte certifikát s hello existující nebo vygenerovat. Soubor PFX s pár klíčů hello šifrování:

* Zadejte heslo hello ochrana hello privátního klíče

## <a name="update-encryption-certificate-in-service-configuration-file"></a>Aktualizovat certifikát šifrování v konfiguračním souboru služby
Aktualizace hodnoty kryptografického otisku hello hello následující nastavení v konfiguračním souboru služby hello s hello kryptografický otisk certifikátu hello nahrán toohello cloudové služby:

    <Certificate name="DataEncryptionPrimary" thumbprint="" thumbprintAlgorithm="sha1" />

## <a name="common-certificate-operations"></a>Běžné operace certifikátu
* Nakonfigurujte certifikát protokolu SSL hello
* Nakonfigurujte klientské certifikáty

## <a name="find-certificate"></a>Najít certifikát
Postupujte následovně:

1. Spusťte mmc.exe.
2. Soubor -> Přidat nebo odebrat modul Snap-in...
3. Vyberte **certifikáty**.
4. Klikněte na tlačítko **Přidat**.
5. Vyberte umístění úložiště certifikátu hello.
6. Klikněte na **Dokončit**.
7. Klikněte na **OK**.
8. Rozbalte položku **certifikáty**.
9. Rozbalte uzel úložiště certifikátů hello.
10. Rozbalte hello certifikát podřízený uzel.
11. Vyberte certifikát v seznamu hello.

## <a name="export-certificate"></a>Export certifikátu
V hello **Průvodce exportem certifikátu**:

1. Klikněte na **Další**.
2. Vyberte **Ano**, pak **Export hello privátní klíč**.
3. Klikněte na **Další**.
4. Vyberte formát souboru požadovaného výstup hello.
5. Kontrola hello požadované možnosti.
6. Zkontrolujte **heslo**.
7. Zadejte silné heslo a potvrďte ho.
8. Klikněte na **Další**.
9. Zadejte nebo vyhledejte název souboru, kde toostore hello certifikátu (použijte. Příponu PFX).
10. Klikněte na **Další**.
11. Klikněte na **Dokončit**.
12. Klikněte na **OK**.

## <a name="import-certificate"></a>Importovat certifikát
V Průvodci importem certifikátu hello:

1. Vyberte umístění úložiště hello.
   
   * Vyberte **aktuální uživatel** -li jen procesů spuštěných v rámci aktuální uživatel přistupuje službě hello
   * Vyberte **místního počítače** Pokud jiné procesy v tomto počítači bude mít přístup hello služby
2. Klikněte na **Další**.
3. Pokud import ze souboru, zkontrolujte cestu k souboru hello.
4. Pokud import. Soubor PFX:
   1. Zadejte heslo hello ochranu privátního klíče hello
   2. Vyberte možnosti importu
5. Vyberte certifikáty "Místo" v hello následující úložiště
6. Klikněte na **Browse** (Procházet).
7. Vyberte požadované úložiště hello.
8. Klikněte na **Dokončit**.
   
   * Pokud jste vybrali hello úložiště Důvěryhodné kořenové certifikační autority, klikněte na tlačítko **Ano**.
9. Klikněte na tlačítko **OK** na všechny systémy windows dialogové okno.

## <a name="upload-certificate"></a>Nahrání certifikátu
V hello [portálu Azure](https://portal.azure.com/)

1. Vyberte **cloudových služeb**.
2. Vyberte hello cloudové služby.
3. V horní nabídce hello, klikněte na tlačítko **certifikáty**.
4. Na dolním panelu hello, klikněte na tlačítko **nahrát**.
5. Vyberte soubor certifikátu hello.
6. Pokud je. Soubor PFX souboru, zadejte heslo hello hello privátní klíč.
7. Po dokončení kopírování hello kryptografický otisk certifikátu z hello novou položku v seznamu hello.

## <a name="other-security-considerations"></a>Další aspekty zabezpečení
nastavení SSL Hello popsané v tomto dokumentu zašifrování komunikace mezi hello služby a jeho klienty, pokud se používá koncový bod HTTPS hello. To je důležité, protože přihlašovací údaje pro přístup k databázi a další potenciálně citlivé informace, které jsou obsaženy v hello komunikace. Upozorňujeme však, že služba hello ukládá vnitřní stav, včetně přihlašovacích údajů, v jeho interní tabulky v hello Microsoft Azure SQL database, která jste zadali pro metadata úložiště v rámci vašeho předplatného Microsoft Azure. Tuto databázi byl definován jako součást hello následující nastavení v konfiguračním souboru služby (. Soubor .CSCFG): 

    <Setting name="ElasticScaleMetadata" value="Server=…" />

Přihlašovací údaje uložené v této databázi se šifrují. Však jako osvědčený postup, zajistěte webové a pracovní role vaše nasazení služby jsou uchovány až toodate a co se mají přístup toohello metadata databáze a hello certifikátu se používá k šifrování a dešifrování uložené přihlašovací údaje. 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

