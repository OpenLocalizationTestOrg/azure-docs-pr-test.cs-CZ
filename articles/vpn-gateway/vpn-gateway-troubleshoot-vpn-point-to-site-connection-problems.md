---
title: "potíže s připojením aaaTroubleshoot Azure point-to-site | Microsoft Docs"
description: "Zjistěte, jak tootroubleshoot potíže s připojením point-to-site."
services: vpn-gateway
documentationcenter: na
author: chadmath
manager: cshepard
editor: 
tags: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/23/2017
ms.author: genli
ms.openlocfilehash: 98d66074be62ad8c7153a903f69cb0d01f988cd2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-azure-point-to-site-connection-problems"></a>Řešení potíží: Problémy Azure připojení point-to-site

Tento článek obsahuje seznam běžných problémů připojení point-to-site, které můžete zaznamenat. Popisuje také možné příčiny a řešení těchto problémů.

## <a name="vpn-client-error-a-certificate-could-not-be-found"></a>Chyba klienta sítě VPN: certifikát nebyl nalezen.

### <a name="symptom"></a>Příznaky

Pokusíte-li pomocí klienta VPN hello tooconnect tooan virtuální síť Azure, můžete zobrazit hello následující chybová zpráva:

**Certifikát nebyl nalezen, který může být použit s Tento Extensible Authentication Protocol. (Chyba 798)**

### <a name="cause"></a>Příčina

K tomuto problému dochází, pokud chybí certifikát klienta hello **certifikáty – aktuální User\Personal\Certificates**.

### <a name="solution"></a>Řešení

Ujistěte se, že tento certifikát klienta hello je nainstalován v hello následující umístění úložiště certifikátů hello (Certmgr.msc):
 
**Certifikáty – aktuální User\Personal\Certificates**

Další informace o tom, jak tooinstall hello klientský certifikát, najdete v části [generování a exportu certifikátů pro připojení point-to-site](vpn-gateway-certificates-point-to-site.md).

> [!NOTE]
> Když importujete hello klientský certifikát, nevybírejte hello **povolit silnou ochranu privátního klíče** možnost.

## <a name="vpn-client-error-hello-message-received-was-unexpected-or-badly-formatted"></a>Chyba klienta sítě VPN: byla přijata zpráva hello neočekávaná nebo chybně formátovaná

### <a name="symptom"></a>Příznaky

Pokusíte-li pomocí klienta VPN hello tooconnect tooan virtuální síť Azure, můžete zobrazit hello následující chybová zpráva:

**přijatá zpráva Hello se neočekávaná nebo chybně formátovaná. (Chyba 0x80090326)**

### <a name="cause"></a>Příčina

K tomuto problému dochází, pokud není veřejný klíč certifikátu kořenové hello nahraje do hello Azure VPN gateway. Může také dojít, pokud klíč hello je poškozený nebo vypršela platnost.

### <a name="solution"></a>Řešení

tooresolve tento problém, zkontrolujte stav hello hello kořenových certifikátů v hello Azure portálu toosee, zda byl odvolán. Pokud ho nebude odvolaný, zkuste toodelete hello kořenový certifikát a reupload. Další informace najdete v tématu [vytvářet certifikáty](vpn-gateway-howto-point-to-site-classic-azure-portal.md#generatecerts).

## <a name="vpn-client-error-a-certificate-chain-processed-but-terminated"></a>Chyba klienta sítě VPN: řetěz certifikátů zpracuje, ale byla ukončena 

### <a name="symptom"></a>Příznaky 

Pokusíte-li pomocí klienta VPN hello tooconnect tooan virtuální síť Azure, můžete zobrazit hello následující chybová zpráva:

**Řetěz certifikátů zpracuje, ale ukončen v kořenovém certifikátu, který není důvěryhodný vztah důvěryhodnosti zprostředkovatele hello.**

### <a name="solution"></a>Řešení

1. Ujistěte se, že hello následující certifikáty jsou ve správném umístění hello:

    | Certifikát | Umístění |
    | ------------- | ------------- |
    | AzureClient.pfx  | Aktuální User\Personal\Certificates |
    | Azuregateway -*GUID*. cloudapp.net  | Aktuální User\Trusted kořenové certifikační autority|
    | AzureGateway -*GUID*. cloudapp.net, AzureRoot.cer    | Místní počítač\Důvěryhodné kořenové certifikační autority|

2. Pokud hello certifikáty jsou již v umístění hello, zkuste toodelete hello certifikáty a je přeinstalovat. Hello  **azuregateway -*GUID*. cloudapp.net** certifikát se nachází ve hello konfiguračního balíčku klienta VPN, který jste si stáhli z portálu Azure hello. Můžete použít soubor archivers tooextract hello soubory z balíčku hello.

## <a name="file-download-error-target-uri-is-not-specified"></a>Chyba při stahování souborů: není zadaný cílový identifikátor URI

### <a name="symptom"></a>Příznaky

Zobrazí se následující chybová zpráva hello:

**Stahování souboru došlo k chybě. Není zadaný cílový identifikátor URI.**

### <a name="cause"></a>Příčina 

K tomuto problému dochází kvůli typ nesprávný brány. 

### <a name="solution"></a>Řešení

musí být Hello typ brány VPN **VPN**, a musí být hello typ sítě VPN **RouteBased**.

## <a name="vpn-client-error-azure-vpn-custom-script-failed"></a>Chyba klienta sítě VPN: Azure VPN vlastního skriptu se nezdařilo 

### <a name="symptom"></a>Příznaky

Pokusíte-li pomocí klienta VPN hello tooconnect tooan virtuální síť Azure, můžete zobrazit hello následující chybová zpráva:

**Vlastní skript (tooupdate vaše směrování tabulky) se nezdařilo. (Chyba 8007026f)**

### <a name="cause"></a>Příčina

Tento problém se může vyskytnout, pokud se pokoušíte připojení k síti VPN tooopen hello bodu lokality pomocí zástupce.

### <a name="solution"></a>Řešení 

Otevřete balíček VPN hello přímo místo otevřením z hello zástupce.

## <a name="cannot-install-hello-vpn-client"></a>Nelze nainstalovat klienta VPN hello

### <a name="cause"></a>Příčina 

Je certifikát další požadované tootrust hello VPN gateway pro vaši virtuální síť. certifikát Hello je součástí hello konfiguračního balíčku klienta VPN generovaný z hello portálu Azure.

### <a name="solution"></a>Řešení

Rozbalte balíček konfigurace klienta VPN hello a najít soubor .cer hello. tooinstall hello certifikátů, postupujte takto:

1. Otevřete mmc.exe.
2. Přidat hello **certifikáty** modul snap-in.
3. Vyberte hello **počítače** účet pro místní počítač hello.
4. Klikněte pravým tlačítkem na hello **důvěryhodné kořenové certifikační autority** uzlu. Klikněte na tlačítko **všech úloh** > **Import**a soubor .cer toohello procházet jste rozbalili ze hello balíček konfigurace klienta VPN.
5. Restartujte počítač hello. 
6. Zkuste klienta VPN tooinstall hello.

## <a name="azure-portal-error-failed-toosave-hello-vpn-gateway-and-hello-data-is-invalid"></a>Portál Azure Chyba: selhání toosave hello VPN gateway a hello dat je neplatný

### <a name="symptom"></a>Příznaky

Když zkusíte toosave hello změny pro bránu VPN hello v hello portálu Azure, zobrazí se hello následující chybová zpráva:

**Brána virtuální sítě se nezdařilo toosave &lt;* název brány*&gt;. Data pro certifikát &lt; *certifikátu ID* &gt; je invalid.* *

### <a name="cause"></a>Příčina 

Tento problém může dojít, pokud hello kořenový certifikát veřejný klíč, který jste nahráli obsahuje neplatný znak, jako je například mezerou.

### <a name="solution"></a>Řešení

Ujistěte se, že hello data v certifikátu hello neobsahuje neplatné znaky, jako je například konce řádků (návrat na začátek). Hello celý hodnota musí být dlouhý jeden řádek. Následující text Hello je ukázka hello certifikátu:

    -----BEGIN CERTIFICATE-----
    MIIC5zCCAc+gAwIBAgIQFSwsLuUrCIdHwI3hzJbdBjANBgkqhkiG9w0BAQsFADAW
    MRQwEgYDVQQDDAtQMlNSb290Q2VydDAeFw0xNzA2MTUwMjU4NDZaFw0xODA2MTUw
    MzE4NDZaMBYxFDASBgNVBAMMC1AyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEF
    AAOCAQ8AMIIBCgKCAQEAz8QUCWCxxxTrxF5yc5uUpL/bzwC5zZ804ltB1NpPa/PI
    sa5uwLw/YFb8XG/JCWxUJpUzS/kHUKFluqkY80U+fAmRmTEMq5wcaMhp3wRfeq+1
    G9OPBNTyqpnHe+i54QAnj1DjsHXXNL4AL1N8/TSzYTm7dkiq+EAIyRRMrZlYwije
    407ChxIp0stB84MtMShhyoSm2hgl+3zfwuaGXoJQwWiXh715kMHVTSj9zFechYd7
    5OLltoRRDyyxsf0qweTFKIgFj13Hn/bq/UJG3AcyQNvlCv1HwQnXO+hckVBB29wE
    sF8QSYk2MMGimPDYYt4ZM5tmYLxxxvGmrGhc+HWXzMeQIDAQABozEwLzAOBgNVHQ8B
    Af8EBAMCAgQwHQYDVR0OBBYEFBE9zZWhQftVLBQNATC/LHLvMb0OMA0GCSqGSIb3
    DQEBCwUAA4IBAQB7k0ySFUQu72sfj3BdNxrXSyOT4L2rADLhxxxiK0U6gHUF6eWz
    /0h6y4mNkg3NgLT3j/WclqzHXZruhWAXSF+VbAGkwcKA99xGWOcUJ+vKVYL/kDja
    gaZrxHlhTYVVmwn4F7DWhteFqhzZ89/W9Mv6p180AimF96qDU8Ez8t860HQaFkU6
    2Nw9ZMsGkvLePZZi78yVBDCWMogBMhrRVXG/xQkBajgvL5syLwFBo2kWGdC+wyWY
    U/Z+EK9UuHnn3Hkq/vXEzRVsYuaxchta0X2UNRzRq+o706l+iyLTpe6fnvW6ilOi
    e8Jcej7mzunzyjz4chN0/WVF94MtxbUkLkqP
    -----END CERTIFICATE-----

## <a name="azure-portal-error-failed-toosave-hello-vpn-gateway-and-hello-resource-name-is-invalid"></a>Portál Azure Chyba: selhání toosave hello VPN gateway a hello název prostředku je neplatný

### <a name="symptom"></a>Příznaky

Když zkusíte toosave hello změny pro bránu VPN hello v hello portálu Azure, zobrazí se hello následující chybová zpráva: 

**Brána virtuální sítě se nezdařilo toosave &lt;* název brány*&gt;. Název prostředku &lt; *název certifikátu zkuste tooupload* &gt; je neplatný **.

### <a name="cause"></a>Příčina

K tomuto problému dochází, protože název hello hello certifikátu obsahuje neplatný znak, jako je například mezerou. 

## <a name="azure-portal-error-vpn-package-file-download-error-503"></a>Portál Azure Chyba: Chyba stažení souboru balíčku VPN 503

### <a name="symptom"></a>Příznaky

Když zkusíte konfiguračního balíčku klienta VPN toodownload hello, zobrazí se hello následující chybová zpráva:

**Nepodařilo toodownload hello souboru. Podrobnosti o chybě: chyba 503. Hello server je zaneprázdněn.**
 
### <a name="solution"></a>Řešení

Tato chyba může být způsobeno k dočasným potížím sítě. Zkuste balíček VPN hello toodownload znovu za několik minut.

## <a name="azure-vpn-gateway-upgrade-all-p2s-clients-are-unable-tooconnect"></a>Upgrade služby Azure VPN Gateway: nelze tooconnect jsou klienti všechny P2S

### <a name="cause"></a>Příčina

Pokud je certifikát hello více než 50 procent prostřednictvím celé jeho životnosti certifikátu hello převracet.

### <a name="solution"></a>Řešení

tooresolve tento problém, vytvořte a znovu distribuovat noví klienti VPN toohello certifikáty. 

## <a name="too-many-vpn-clients-connected-at-once"></a>Příliš mnoho klientů VPN připojení najednou

Pro každou bránu VPN hello maximální povolený počet připojení je 128. Uvidíte hello celkový počet připojených klientů v hello portálu Azure.

## <a name="point-to-site-vpn-incorrectly-adds-a-route-for-100008-toohello-route-table"></a>Point-to-site VPN nesprávně přidá trasu pro 10.0.0.0/8 toohello směrovací tabulku

### <a name="symptom"></a>Příznaky

Při přidávání hello připojení VPN na klienta point-to-site hello klienta VPN hello nutné přidat směrování směrem k hello virtuální síť Azure. Pomocná služba Hello IP nutné přidat směrování pro podsíť hello klientů VPN hello. 

Hello rozsah klienta VPN patří tooa menší podsíti 10.0.0.0/8, jako je například 10.0.12.0/24. Místo trasu pro 10.0.12.0/24 se přidá trasu pro 10.0.0.0/8, který má vyšší prioritu. 

Tato nesprávná trasa dělí připojení s další místní sítě, které můžou patřit tooanother podsítí v rámci rozsahu hello 10.0.0.0/8, například 10.50.0.0/24, které nemají konkrétní trasy definované. 

### <a name="cause"></a>Příčina

Toto chování je záměrné pro klienty se systémem Windows. Když klient hello používá protokol PPP IPCP hello, získá hello IP adresu pro rozhraní tunelu hello ze serveru hello (hello brána sítě VPN v tomto případě). Ale kvůli omezení v protokolu hello hello klient nemá hello masku podsítě. Protože není k dispozici žádné další způsob tooget, hello klienta pokusí maska podsítě hello tooguess je založeno na třídě hello hello tunelového propojení rozhraní IP adresy. 

Proto je přidána trasu podle následující statických mapování hello: 

Pokud adresa patří tooclass A použít aplikaci--> podsíť/8

Pokud adresa patří tooclass--> B použít /16

Pokud adresa patří tooclass--> C použít /24

## <a name="vpn-client-cannot-access-network-file-shares"></a>Klient VPN nelze získat přístup k síťové sdílené složky

### <a name="symptom"></a>Příznaky

Klient VPN Hello připojil toohello virtuální síť Azure. Hello klienta nemá přístup k síťové sdílené složky.

### <a name="cause"></a>Příčina

Hello protokolu SMB se používá pro přístup ke sdílené složce souborů. Když je zahájeno připojení hello, klient VPN hello přidá hello relace pověření a dojde k selhání hello. Po navázání připojení hello hello klient je donucen toouse hello mezipaměti pověření pro ověřování pomocí protokolu Kerberos. Tento proces se spustí dotazy toohello Key Distribution Center (řadič domény) tooget token. Protože hello klient připojí z hello Internetu, nemusí být řadič domény nemůže tooreach hello. Proto hello klienta nelze převzetí služeb při selhání z tooNTLM protokolu Kerberos. 

Hello jenom čas, že klient hello se zobrazí výzva pro pověření je, když má platný certifikát (sítě SAN = UPN) vydaný toowhich domény hello je připojen. Hello klienta musí být také fyzicky připojené toohello doménové síti. V tomto případě klient hello pokusí toouse hello certifikátu a spojí toohello řadič domény. Potom hello Key Distribution Center vrátí chybu "KDC_ERR_C_PRINCIPAL_UNKNOWN". Klient Hello je vynucené toofail přes tooNTLM. 

### <a name="solution"></a>Řešení

toowork vyřešit hello problém, zakažte hello ukládání přihlašovacích údajů do domény z hello následující podklíč registru: 

    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\DisableDomainCreds - Set hello value too1 


## <a name="cannot-find-hello-point-to-site-vpn-connection-in-windows-after-reinstalling-hello-vpn-client"></a>Nelze najít připojení point-to-site VPN hello v systému Windows po opětovné instalaci klienta VPN hello

### <a name="symptom"></a>Příznaky

Odebrat připojení VPN point-to-site hello a znovu nainstalujete klienta VPN hello. V takovém případě není hello připojení k síti VPN byl úspěšně nakonfigurován. Nevidíte hello připojení VPN v hello **síťová připojení** nastavení v systému Windows.

### <a name="solution"></a>Řešení

tooresolve hello problém odstranit hello staré VPN konfigurační soubory klienta z **C:\Users\TheUserName\AppData\Roaming\Microsoft\Network\Connections**, a poté znovu spusťte instalační program klienta VPN hello.
