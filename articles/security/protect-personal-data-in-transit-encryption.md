---
title: "aaaProtect osobních údajů v přenosu pomocí šifrování v Azure | Microsoft Docs"
description: "Použití šifrování v Azure tooprotect osobní data"
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/22/2017
ms.author: barclayn
ms.custom: 
ms.openlocfilehash: 218ad3f49326e8dec299a6d92b18116da65eae71
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-encryption-technologies-protect-personal-data-in-transit-with-encryption"></a>Azure šifrovací technologie: ochrana osobních údajů v přenosu s šifrováním

Tento článek vám pomůže pochopit a používat Azure šifrovací technologie toosecure data při přenosu. 

Ochranu osobních údajů hello osobních údajů při přenosu v síti hello je důležitou součástí strategie zabezpečení víceúrovňová obrany zabezpečení. Šifrování během přenosu je navrženou tooprevent útočník, který zabrání přenosů nebudou moct tooview nebo použijte hello data.

## <a name="scenario"></a>Scénář

Velké výletních společnosti, centrálou ve Spojených státech amerických hello je rozšířit jeho itineráře toooffer operace v hello Středozemního, Jaderského a baltský moři, jakož i hello Britské ostrovy. toosupport těchto úsilí získala menší výletních Víceřádkový na základě v Itálii Německo, Dánsko a hello Spojené království 

Hello společnost používá Microsoft Azure toostore firemní data v cloudu hello. To zahrnuje osobní identifikovatelné údaje, například jména, adresy, telefonních čísel a informace o kreditní kartě z jeho základní globální zákazníka. Ve všech umístěních zahrnuje také tradiční informace lidských zdrojů, jako je adresy, telefonních čísel, daň identifikačními čísly a lékařské informace o zaměstnance společnosti. Hello výletních řádku také udržuje velké databáze potřebu a věrnost program členů, která zahrnuje osobní údaje tootrack vztahů se zákazníky aktuální a starší.

Osobní údaje zákazníků je zadána v hello databáze ze vzdálených pobočkách hello společnosti a z agentů cesta nachází kolem hello, world. Dokumenty, které obsahují informace o zákazníkovi se přenesou pomocí hello síťového tooAzure úložiště.

## <a name="problem-statement"></a>Popis problému

Hello společnosti musí chránit hello ochranu osobních údajů zákazníků a osobní data zaměstnanců při jeho je v tooand přenosu ze služby Azure.

## <a name="company-goal"></a>Cílem společnosti

Hello tooensure cílem společnosti, když vypnout disku musí být zašifrovaná osobní data. Pokud neoprávněné osoby intercept hello vypnout disku osobní data, musí být ve formuláři, který bude zobrazovat nečitelná. Použití šifrování by měla být snadná nebo zcela transparentní, uživatelům a správcům.

## <a name="solutions"></a>Řešení

Azure services poskytují několik nástrojů a technologií toohelp chránit osobní údaje při přenosu.

### <a name="azure-storage"></a>Azure Storage

Data, která je uložená v cloudu hello musí cestují z hello klienta, který může být fyzicky umístěné kdekoli v hello, world toohello datového centra Azure. Pokud tato data jsou načítána pro uživatele, se přenáší znovu v hello opačným směrem. Data, která je při přenosu přes hello veřejného Internetu je vždy hrozí zachycení útočníků. Je důležité tooprotect hello soukromí osobní data pomocí šifrování transportní vrstvy toosecure jako ji přesune mezi umístěními.

Hello protokol HTTPS nabízí v porovnání s hello Internet zabezpečené, šifrované komunikační kanál. HTTPS musí být použité tooaccess objekty ve službě Azure Storage a při volání rozhraní REST API. Vynutit používání protokolu HTTPS hello při použití [sdílené přístupové podpisy](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1) (SAS) toodelegate přístup tooAzure úložiště objektů. Existují dva typy SAS: SAS služby a SAS účtu.

#### <a name="how-do-i-construct-a-service-sas"></a>Jak vytvořit SAS služby?

Prostředek služby SAS delegáti přístup tooa jen v jedné služby úložiště hello (služba objektů blob, fronty, tabulka nebo souboru). tooconstruct Service SAS, hello následující:

1. Zadejte hello pole verze Signed

2. Zadejte hello podepsaná prostředků (objektů Blob a služby pouze souboru)

3. Zadejte parametry dotazu tooOverride hlavičky odpovědi (služby objektů Blob a služby pouze souboru)

4. Zadejte název tabulky (pouze služby Table) hello

5. Zadejte hello zásad přístupu

6. Zadejte hello Interval platnosti podpisu

8. Zadejte oprávnění

9. Zadejte IP adresu nebo rozsah IP adres

10. Zadejte hello protokolu HTTP

11. Zadejte rozsahy přístup tabulky

12. Zadejte identifikátor podepsaná hello

13. Zadejte hello podpis

Další podrobné pokyny naleznete v tématu [vytváření SAS služby](https://docs.microsoft.com/rest/api/storageservices/Constructing-a-Service-SAS?redirectedfrom=MSDN).

#### <a name="how-do-i-construct-an-account-sas"></a>Jak vytvořit SAS účtu?

SAS účtu deleguje přístup tooresources v jednom nebo více služeb úložiště hello. Můžete taky delegovat přístup tooread, zápisu a operace odstranění pro kontejnery objektů blob, tabulky, fronty a sdílených složek, které se nedá vymezit přes SAS služby. Konstrukce SAS účtu je podobné toothat SAS služby. Podrobné pokyny najdete v tématu [vytváření SAS účtu.](https://docs.microsoft.com/rest/api/storageservices/Constructing-an-Account-SAS?redirectedfrom=MSDN)

#### <a name="how-do-i-enforce-https-when-calling-rest-apis"></a>Jak se při volání rozhraní REST API vynutit HTTPS?

tooenforce hello použití protokolu HTTPS při volání rozhraní REST API tooaccess objekty v účtech úložiště, můžete povolit zabezpečené přenosu požadované pro účet úložiště hello. 

1. V hello portálu Azure, vyberte **vytvořit účet úložiště**, nebo pro stávající účet úložiště, vyberte **nastavení** a potom **konfigurace**.

2. V části **zabezpečení přenosu požadované**, vyberte **povoleno**.

![Vytvoření účtu úložiště](media/protect-personal-data-in-transit-encryption/create-storage-account.png)

Podrobné pokyny, včetně způsobu tooenable zabezpečení přenosu požadované prostřednictvím kódu programu, najdete v části [vyžadují zabezpečení přenosu](https://docs.microsoft.com/azure/storage/storage-require-secure-transfer).

#### <a name="how-do-i-encrypt-data-in-azure-file-storage"></a>Jak šifrují data v Azure File Storage?

tooencrypt dat během přenosu s [Azure File Storage](https://docs.microsoft.com/azure/storage/storage-file-how-to-use-files-portal), můžete použít protokol SMB 3.x s Windows 10, 8 a 8.1 a Windows Server 2012 R2 a Windows Server 2016. Pokud používáte službu Azure souborů hello, žádné připojení bez šifrování se nezdaří, pokud "Zabezpečení přenosu požadované" je povolena. To zahrnuje scénáře s využitím protokolu SMB 2.1, protokolu SMB 3.0 bez šifrování a některých typů hello klient Linux SMB.

#### <a name="azure-client-side-encryption"></a>Azure šifrování na straně klienta

Další možnost ochrany osobních údajů při přenášena mezi klientská aplikace a Azure Storage je [šifrování na straně klienta](https://docs.microsoft.com/azure/storage/storage-client-side-encryption). Hello data jsou zašifrována před přenášením do úložiště Azure a pokud načítáte hello data ze služby Azure Storage, hello data se dešifrují po přijetí na straně klienta hello.

### <a name="azure-site-to-site-vpn"></a>Azure Site-to-Site VPN

Efektivní způsob tooprotect osobních dat během přenosu mezi podnikovou sítí nebo uživatele a hello virtuální síť Azure je toouse [site-to-site](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal) nebo [point-to-site](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal) virtuální privátní sítě (VPN). Připojení k síti VPN vytvoří zabezpečené šifrované tunelové propojení mezi hello Internetu.

#### <a name="how-do-i-create-a-site-to-site-vpn-connection"></a>Vytvoření připojení site-to-site VPN

Síť site-to-site VPN připojí více uživateli v podnikové síti tooAzure hello. toocreate připojení site-to-site v hello portál Azure, hello následující:

1. Vytvoření virtuální sítě.

2. Určení serveru DNS.

3. Vytvořte podsíť brány hello.

4. Vytvoření brány VPN hello. 

    ![](media/protect-personal-data-in-transit-encryption/vpn-step-01.png)

5. Vytvoření brány místní sítě hello.

    ![](media/protect-personal-data-in-transit-encryption/vpn-step-02.png)

6. Konfigurace zařízení VPN.

7. Vytvořte připojení VPN hello.

    ![](media/protect-personal-data-in-transit-encryption/vpn-step-03.png)

8. Ověřte připojení k síti VPN hello.

Podrobnější pokyny o tom, jak připojení toocreate site-to-site v hello Azure portálu, najdete v tématu [vytvořit Site-to-Site připojení v hello portálu Azure.] (https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)

#### <a name="how-do-i-create-a-point-to-site-vpn-connection"></a>Vytvoření připojení VPN typu point-to-site?

Síť VPN Point-to-Site vytvoří zabezpečené připojení z virtuální sítě jednotlivých klientských počítačů tooa. To je užitečné, pokud chcete tooconnect tooAzure ze vzdáleného umístění, například z domova nebo hotelů nebo konference center. toocreate připojení point-to-site v hello portálu Azure

1. Vytvoření virtuální sítě.

2. Přidáte podsíť brány.

3. Určení serveru DNS. (volitelné)

4. Vytvoření brány virtuální sítě.

5. Generovat certifikáty.

6. Přidejte fond adres klienta hello.

7. Nahrajte data hello kořenového certifikátu veřejného certifikátu.

8. Generování a nainstalujte balíček konfigurace klienta VPN hello.

9. Nainstalujte certifikát exportovaného klienta.

10. Připojte tooAzure.

11. Ověřte své propojení.

Další podrobné pokyny naleznete v tématu [konfigurace tooa připojení Point-to-Site virtuální síť ověřování pomocí certifikátů: portál Azure.](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal)

### <a name="ssltls"></a>SSL/TLS.

Společnost Microsoft doporučuje vždy použít data tooexchange protokoly SSL/TLS v různých umístěních. Organizace, které zvolte toouse [ExpressRoute](https://docs.microsoft.com/azure/expressroute/) toomove velkých datových sad přes vyhrazené vysokorychlostní propojení WAN můžete šifrovat taky hello data na úrovni aplikace pro zvýšení ochrany pomocí protokolu SSL/TLS nebo jiné protokoly hello.

### <a name="encryption-by-default"></a>Ve výchozím nastavení šifrování

Společnost Microsoft používá data tooprotect šifrování během přenosu mezi zákazníky a cloudové služby Azure. Pokud jsou interakci s Azure Storage prostřednictvím hello portálu Azure, všechny transakce dojít přes HTTPS.

[Transport Layer Security](https://en.wikipedia.org/wiki/Transport_Layer_Security) (TLS) je protokol hello že datových Center společnosti Microsoft se pokusí toonegotiate s klientské systémy, které se připojují tooMicrosoft cloudové služby. Protokol TLS poskytuje silné ověřování, ochrana soukromí zpráv a integritu (umožňuje detekovat zpráva manipulaci, zachycení a padělání), vzájemná funkční spolupráce, flexibilita algoritmu, snadné nasazení a používání.

[Ideální Forward Secrecy](https://en.wikipedia.org/wiki/Forward_secrecy) (PFS) také zaměstnání tak, aby každé připojení mezi systémy klienta zákazníků a cloudových služeb Microsoftu používat jedinečné klíče. Připojení tooMicrosoft cloudové služby také využít výhod šifrování RSA na základě 2 048 bitů délky klíčů. Hello kombinace protokolu TLS, délky klíčů RSA 2 048 bitů a PFS usnadňuje mnohem obtížnější někdo toointercept a přístup data, která je při přenosu mezi cloudové služby společnosti Microsoft a zákazníků.

Přenášená data zašifrována vždy v [Data Lake Store] (https://docs.microsoft.com/azure/data-lake-store/data-lake-store-security-overview). Kromě toho tooencrypting data předchozí toostoring toopersistent média, hello je také vždy zabezpečená data při přenosu pomocí protokolu HTTPS. HTTPS je hello pouze protokol, který je podporován pro rozhraní Data Lake Store REST hello.

## <a name="summary"></a>Souhrn

Hello společnosti můžete provádět svůj cíl ochrany osobní data a hello ochrany osobních údajů těchto údajů vynucování tooAzure připojení HTTPS úložiště, pomocí sdílené přístupové podpisy a povolení zabezpečení přenosu vyžaduje na účty úložiště hello. Pomocí připojení protokolu SMB 3.0 a implementaci šifrování na straně klienta taky chránit osobní údaje. Připojení VPN typu Site-to-site z hello toohello podnikové sítě virtuální síť Azure. připojení point-to-site VPN od jednotlivých uživatelů se vytvoří zabezpečené tunelové propojení, pomocí kterého můžete bezpečně cestují osobní data. Postupy šifrování výchozí společnosti Microsoft, budou další ochrany osobních údajů hello osobních údajů.

## <a name="next-steps"></a>Další kroky

- [Doporučené postupy zabezpečení služby Azure Data a šifrování](https://docs.microsoft.com/azure/security/azure-security-data-encryption-best-practices)

- [Plánování a návrh pro VPN Gateway](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design)

- [VPN Gateway – nejčastější dotazy](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-vpn-faq)

- [Zakoupení a konfigurace certifikátu protokolu SSL pro Azure App Service](https://docs.microsoft.com/azure/app-service-web/web-sites-purchase-ssl-web-site)
