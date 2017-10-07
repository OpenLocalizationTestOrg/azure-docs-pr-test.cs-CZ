---
title: "aaaCommunication zabezpečení – Microsoft Threat modelování nástroj – Azure | Microsoft Docs"
description: "způsoby zmírnění hrozeb v hello nástroj modelování hrozeb"
services: security
documentationcenter: na
author: RodSan
manager: RodSan
editor: RodSan
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: rodsan
ms.openlocfilehash: 667829c75123f4dbe0b383fdaf8cd899802f9b16
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="security-frame-communication-security--mitigations"></a>Zabezpečení rámce: Zabezpečení komunikace | Způsoby zmírnění rizik 
| Produktům a službám | Článek |
| --------------- | ------- |
| **Centra událostí Azure** | <ul><li>[Zabezpečené komunikace tooEvent rozbočovač za použití protokolu SSL/TLS](#comm-ssltls)</li></ul> |
| **Dynamics CRM** | <ul><li>[Zkontrolujte účet služby, oprávnění a zkontrolujte, zda text hello vlastní služby nebo stránek ASP.NET respektují CRM na zabezpečení](#priv-aspnet)</li></ul> |
| **Azure Data Factory** | <ul><li>[Použití brány pro správu dat při připojování na místní systém SQL Server tooAzure objekt pro vytváření dat](#sqlserver-factory)</li></ul> |
| **Serveru identit** | <ul><li>[Zajistěte, aby všechny přenosy tooIdentity Server přes připojení HTTPS](#identity-https)</li></ul> |
| **Webové aplikace** | <ul><li>[Ověřte, že připojení SSL, TLS a DTLS tooauthenticate používat certifikáty X.509](#x509-ssltls)</li><li>[Nakonfigurovat certifikát SSL pro vlastní doménu, ve službě Azure App Service](#ssl-appservice)</li><li>[Vynutit provoz tooAzure všechny služby App Service přes připojení HTTPS](#appservice-https)</li><li>[Povolit zabezpečení striktní přenosu HTTP (HSTS)](#http-hsts)</li></ul> |
| **Database** | <ul><li>[Ujistěte se, SQL server šifrování a certifikát pro ověření platnosti připojení](#sqlserver-validation)</li><li>[Vynutit zašifrovaná komunikace tooSQL serveru](#encrypted-sqlserver)</li></ul> |
| **Azure Storage** | <ul><li>[Ujistěte se, že komunikace tooAzure úložiště je přes protokol HTTPS](#comm-storage)</li><li>[Ověřit hodnotu hash MD5 po stažení objektů blob, pokud nelze povolit protokol HTTPS](#md5-https)</li><li>[Použití protokolu SMB 3.0 kompatibilní klient tooensure během přenosu dat šifrování tooAzure sdílené složky](#smb-shares)</li></ul> |
| **Mobilního klienta** | <ul><li>[Implementace Připnutí certifikátu](#cert-pinning)</li></ul> |
| **WCF** | <ul><li>[Povolit protokol HTTPS – zabezpečený kanál přenosu](#https-transport)</li><li>[WCF: TooEncryptAndSign úrovně zabezpečení ochrany sadu zpráv](#message-protection)</li><li>[WCF: Použijte toorun nejméně privilegovaným účtem služby WCF](#least-account-wcf)</li></ul> |
| **Webové rozhraní API** | <ul><li>[Vynutit provoz tooWeb všechny rozhraní API přes připojení HTTPS](#webapi-https)</li></ul> |
| **Azure Redis Cache** | <ul><li>[Ujistěte se, že komunikace tooAzure Redis Cache je přes protokol SSL](#redis-ssl)</li></ul> |
| **Brána pole IoT** | <ul><li>[Zabezpečení komunikace brány tooField zařízení](#device-field)</li></ul> |
| **Brána IoT cloudu** | <ul><li>[Zabezpečení tooCloud zařízení brány komunikaci pomocí protokolu SSL/TLS](#device-cloud)</li></ul> |

## <a id="comm-ssltls"></a>Zabezpečené komunikace tooEvent rozbočovač za použití protokolu SSL/TLS

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Centra událostí Azure | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Ověřování a zabezpečení modelu přehled služby Event Hubs](https://azure.microsoft.com/documentation/articles/event-hubs-authentication-and-security-model-overview/) |
| **Kroky** | Zabezpečený protokol AMQP nebo HTTP připojení tooEvent rozbočovač za použití protokolu SSL/TLS |

## <a id="priv-aspnet"></a>Zkontrolujte účet služby, oprávnění a zkontrolujte, zda text hello vlastní služby nebo stránek ASP.NET respektují CRM na zabezpečení

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Dynamics CRM | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | Zkontrolujte účet služby, oprávnění a zkontrolujte, zda text hello vlastní služby nebo stránek ASP.NET respektují CRM na zabezpečení |

## <a id="sqlserver-factory"></a>Použití brány pro správu dat při připojování na místní systém SQL Server tooAzure objekt pro vytváření dat

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Azure Data Factory | 
| **SDL fáze**               | Nasazení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Typy propojené služby - Azure a místním |
| **Odkazy**              |[Přesun dat mezi místní ve a Azure Data Factory](https://azure.microsoft.com/documentation/articles/data-factory-move-data-between-onprem-and-cloud/#create-gateway), [Brána pro správu dat](https://azure.microsoft.com/documentation/articles/data-factory-data-management-gateway/) |
| **Kroky** | <p>Nástroj Data Management Gateway (DMG) Hello je požadovaná tooconnect toodata zdroje, které jsou chráněné za corpnet nebo brána firewall.</p><ol><li>Uzamčení počítače hello izoluje hello DMG nástroj a zabrání chybně fungující programy z poškození nebo sledování na zdrojovém počítači hello data. (Např.) musí být nainstalované nejnovější aktualizace, povolit minimální požadované porty řízené účty zřizování a auditování povolené, disku povolené šifrování atd.)</li><li>V pravidelných intervalech, nebo vždy, když heslo účtu služby DMG hello obnovuje se musí otočit klíč brány dat</li><li>Data tranzitů přes odkaz služby musí být šifrovaný.</li></ol> |

## <a id="identity-https"></a>Zajistěte, aby všechny přenosy tooIdentity Server přes připojení HTTPS

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Serveru identit | 
| **SDL fáze**               | Nasazení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [IdentityServer3 - klíčů, podpisů a šifrování](https://identityserver.github.io/Documentation/docsv2/configuration/crypto.html), [IdentityServer3 – nasazení](https://identityserver.github.io/Documentation/docsv2/advanced/deployment.html) |
| **Kroky** | Ve výchozím nastavení vyžaduje IdentityServer toocome všechna příchozí připojení přes protokol HTTPS. Je absolutně povinné, že komunikace s IdentityServer probíhá přes pouze zabezpečené přenosy. Existují určité scénáře nasazení jako snižování zátěže protokolu SSL kde můžete zmírnit tento požadavek. Zobrazit stránka nasazení serveru identit hello v hello odkazy na další informace. |

## <a id="x509-ssltls"></a>Ověřte, že připojení SSL, TLS a DTLS tooauthenticate používat certifikáty X.509

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | <p>Aplikace, které používají protokol SSL, TLS a DTLS musí plně ověření certifikátů X.509 hello hello entit, ke kterým se připojují k. To zahrnuje hello certifikáty pro ověření:</p><ul><li>Název domény</li><li>Data platnosti (začátku a datum vypršení platnosti)</li><li>Stav odvolání</li><li>Využití (například ověřování serveru u serverů, ověření klienta pro klienty)</li><li>Důvěřujete řetězu. Certifikáty musí být propojeny tooa kořenovou certifikační autoritu (CA), která je důvěryhodná hello platformy nebo explicitně nakonfigurované správcem hello</li><li>Délka klíče veřejný klíč certifikátu musí být > 2048 bitů</li><li>Algoritmus hash musí být SHA256 a vyšší |

## <a id="ssl-appservice"></a>Nakonfigurovat certifikát SSL pro vlastní doménu, ve službě Azure App Service

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | EnvironmentType – Azure |
| **Odkazy**              | [Povolit HTTPS pro aplikace v Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-configure-ssl-certificate/) |
| **Kroky** | Ve výchozím nastavení, Azure již umožňuje HTTPS pro každou aplikaci s certifikát se zástupným znakem pro hello *. azurewebsites.net domény. Podobně jako všechny zástupné domény, je však není tak bezpečné jako použití vlastní domény s vlastní certifikát [najdete](https://casecurity.org/2014/02/26/pros-and-cons-of-single-domain-multi-domain-and-wildcard-certificates/). Je doporučeno tooenable SSL pro hello vlastní doménu, které hello nasazené aplikace bude dostupná v|

## <a id="appservice-https"></a>Vynutit provoz tooAzure všechny služby App Service přes připojení HTTPS

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | EnvironmentType – Azure |
| **Odkazy**              | [Vynutit HTTPS v Azure App Service] https://azure.microsoft.com/documentation/articles/web-sites-configure-ssl-certificate/#4-enforce-https-on-your-app) |
| **Kroky** | <p>I když Azure již umožňuje HTTPS pro Azure aplikace služby s certifikát se zástupným znakem pro hello domény *. azurewebsites.net, Nevynucovat HTTPS. Návštěvníky může stále přístup k aplikaci hello pomocí protokolu HTTP, což může ohrozit zabezpečení aplikace hello a proto HTTPS obsahuje vynucené explicitně toobe. Aplikace ASP.NET MVC by měly používat hello [RequireHttps filtru](http://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) který vynutí zabezpečená toobe požadavku HTTP znovu zaslaného prostřednictvím protokolu HTTPS.</p><p>Případně hello přepisování adres URL modul, který je součástí služby Azure App Service může být použité tooenforce HTTPS. Modul přepisování adres URL umožňuje vývojářům toodefine pravidla, která jsou použité tooincoming požadavků, než hello žádosti jsou předávány tooyour aplikace. Přepisování adres URL pravidla jsou definovány v souboru web.config uložené v kořenovém hello aplikace hello</p>|

### <a name="example"></a>Příklad
Hello následující příklad obsahuje základní pravidlo přepisování adres URL, které vynutí toouse všechny příchozí provoz protokolu HTTPS
```XML
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <system.webServer>
    <rewrite>
      <rules>
        <rule name="Force HTTPS" enabled="true">
          <match url="(.*)" ignoreCase="false" />
          <conditions>
            <add input="{HTTPS}" pattern="off" />
          </conditions>
          <action type="Redirect" url="https://{HTTP_HOST}/{R:1}" appendQueryString="true" redirectType="Permanent" />
        </rule>
      </rules>
    </rewrite>
  </system.webServer>
</configuration>
```
Toto pravidlo funguje tak, že vrací stavový kód protokolu HTTP 301 (trvalé přesměrování) při hello uživatel požádá o stránku pomocí protokolu HTTP. Hello 301 přesměrování hello požadavek toohello stejnou adresu URL jako hello návštěvníka požadována, ale nahradí část s protokolem HTTP hello hello žádosti s protokolem HTTPS. Například by HTTP://contoso.com přesměrovaného tooHTTPS://contoso.com. 

## <a id="http-hsts"></a>Povolit zabezpečení striktní přenosu HTTP (HSTS)

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Zabezpečení striktní přenosu HTTP OWASP Tahák](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) |
| **Kroky** | <p>Striktní přenos HTTP zabezpečení (HSTS) je určený parametrem webovou aplikaci pomocí hello použijte hlavičky odpovědi speciální vylepšení zabezpečení opt-in. Jakmile podporovaného prohlížeče obdrží tuto hlavičku prohlížeč tohoto zabrání veškerou komunikaci před odesláním prostřednictvím protokolu HTTP toohello zadaná doména a místo toho odešle veškerou komunikaci přes protokol HTTPS. Rovněž zamezí klikněte na protokol HTTPS přes výzvy v prohlížečích.</p><p>tooimplement HSTS hello následující hlavičku odpovědi má toobe nakonfigurovaná pro web globálně, v kódu nebo v konfiguraci. Strict přenosu zabezpečení: maximální stáří = 300; includeSubDomains HSTS řeší následující hrozeb hello:</p><ul><li>Uživatel, záložky nebo ručně typy http://example.com a je subjektu tooa man-in-the-middle útočník: HSTS automaticky přesměruje tooHTTPS požadavky HTTP pro cílovou doménu hello</li><li>Webové aplikace, který je určený toobe čistě HTTPS nechtěně obsahuje odkazy HTTP nebo slouží obsahu prostřednictvím protokolu HTTP: HSTS automaticky přesměruje tooHTTPS požadavky HTTP pro cílovou doménu hello</li><li>Man-in-the-middle útočník pokusí toointercept provoz z postižené uživatele s využitím neplatný certifikát a doufá hello uživatele bude přijímat chybný certifikát hello: HSTS neumožňuje zprávu uživatele toooverride hello neplatný certifikát</li></ul>|

## <a id="sqlserver-validation"></a>Ujistěte se, SQL server šifrování a certifikát pro ověření platnosti připojení

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Databáze | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | SQL Azure  |
| **Atributy**              | Verze SQL - V12 |
| **Odkazy**              | [Osvědčené postupy na psaní zabezpečené připojovací řetězce pro databázi SQL.](http://social.technet.microsoft.com/wiki/contents/articles/2951.windows-azure-sql-database-connection-security.aspx#best) |
| **Kroky** | <p>Veškerá komunikace mezi SQL Database a klientské aplikace jsou šifrované pomocí Secure Sockets Layer (SSL) za všech okolností. Databáze SQL nepodporuje nezašifrované připojení. toovalidate certifikáty s nástroji, nebo kód aplikace explicitně požadovat šifrované připojení a nedůvěřujete certifikáty serveru hello. Pokud váš kód aplikace nebo nástroje vyžádat šifrované připojení, bude stále obdrží šifrované připojení</p><p>Však nemusí ověřit hello certifikáty serveru a tak nebude náchylný příliš útoky "man uprostřed hello". Nastavit certifikáty toovalidate ADO.NET kód aplikace, `Encrypt=True` a `TrustServerCertificate=False` v připojovací řetězec databáze hello. certifikáty toovalidate prostřednictvím systému SQL Server Management Studio otevřete dialogové okno tooServer připojit hello. Klikněte na možnost šifrování připojení na kartě Vlastnosti připojení hello</p>|

## <a id="encrypted-sqlserver"></a>Vynutit zašifrovaná komunikace tooSQL serveru

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Databáze | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Místní |
| **Atributy**              | SQL verze - MsSQL2016, SQL verze - MsSQL2012, verze SQL - MsSQL2014 |
| **Odkazy**              | [Povolit toohello šifrované připojení databázový stroj](https://msdn.microsoft.com/library/ms191192)  |
| **Kroky** | Povolení protokolu SSL šifrování zvyšuje úroveň zabezpečení hello dat přenášených v rámci sítí mezi instance systému SQL Server a aplikací. |

## <a id="comm-storage"></a>Ujistěte se, že komunikace tooAzure úložiště je přes protokol HTTPS

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Azure Storage | 
| **SDL fáze**               | Nasazení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Šifrování transportní vrstvy úložiště Azure – pomocí protokolu HTTPS](https://azure.microsoft.com/documentation/articles/storage-security-guide/#_encryption-in-transit) |
| **Kroky** | tooensure hello zabezpečení Azure Storage data na cestě, používat protokol HTTPS hello při každém volání hello rozhraní REST API nebo přístupu k nim objektů v úložišti. Navíc sdílené přístupové podpisy, které můžou být použité toodelegate přístup k objektům úložiště tooAzure, zahrnout toospecify možnost této pouze hello, které můžete použít protokol HTTPS, pokud se používá sdílené přístupové podpisy, zajistíte, že kdokoliv odesílání se propojení s tokeny SAS se Použijte správný protokol hello.|

## <a id="md5-https"></a>Ověřit hodnotu hash MD5 po stažení objektů blob, pokud nelze povolit protokol HTTPS

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Azure Storage | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | StorageType – objekt Blob |
| **Odkazy**              | [Přehled služby Windows Azure Blob MD5](https://blogs.msdn.microsoft.com/windowsazurestorage/2011/02/17/windows-azure-blob-md5-overview/) |
| **Kroky** | <p>Služba objektů Blob v systému Windows Azure poskytuje integritu dat tooensure mechanismy jak v transportní vrstvy a aplikace hello. Pokud z jakéhokoli důvodu, že potřebujete toouse HTTP místo protokolu HTTPS a práci s objekty BLOB bloku, můžete použít kontrolu MD5 toohelp ověří integritu hello objektů BLOB hello přenosu</p><p>To vám pomůže s ochranou z chyb sítě/transport layer, ale nemusí nutně jít s zprostředkující útoky. Pokud použijete protokol HTTPS, který poskytuje zabezpečení na úrovni přenosu, pak pomocí MD5 kontrola je redundantní a zbytečné.</p>|

## <a id="smb-shares"></a>Pomocí protokolu SMB 3.0 kompatibilní klient tooensure během přenosu dat šifrování tooAzure, které sdílené složky

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Mobilního klienta | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | StorageType – soubor |
| **Odkazy**              | [Azure File Storage](https://azure.microsoft.com/blog/azure-file-storage-now-generally-available/#comment-2529238931), [podpora protokolu SMB Azure File Storage pro klienty se systémem Windows](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-files/#_mount-the-file-share) |
| **Kroky** | Úložiště Azure File podporuje protokol HTTPS, při použití hello REST API, ale je často používané jako sdílené složky SMB připojen tooa virtuálních počítačů. SMB 2.1 nepodporuje šifrování, takže připojení jsou povoleny pouze v rámci hello stejné oblasti v Azure. Ale podporuje šifrování protokolu SMB 3.0 a lze použít s Windows Server 2012 R2, Windows 8, Windows 8.1 a Windows 10, což mezi oblastmi přístup a dokonce i přístup na ploše hello. |

## <a id="cert-pinning"></a>Implementace Připnutí certifikátu

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Azure Storage | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné, Windows Phone |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Certifikát a veřejný klíč připojení](https://www.owasp.org/index.php/Certificate_and_Public_Key_Pinning#.Net) |
| **Kroky** | <p>Připnutí certifikátu obranu proti útokům Man-In-The-Middle typu MITM (). Připnutí je proces hello přidružení hostitele k jejich očekávané X509 certifikát nebo veřejný klíč. Jakmile certifikát nebo veřejný klíč známý nebo je vidět pro hostitele, hello certifikát nebo veřejný klíč je přidružené nebo 'definovaného' toohello hostitele. </p><p>Proto když se pokusí nežádoucí osoba toodo útoky typu MITM SSL během klíči hello metody handshake SSL ze serveru útočníka nebudou se liší od hello připnutý klíče certifikátu a hello žádost se zahodí, takže si certifikát MITM Připnutí lze dosáhnout implementace je ServicePointManager – `ServerCertificateValidationCallback` delegovat.</p>|

### <a name="example"></a>Příklad
```C#
using System;
using System.Net;
using System.Net.Security;
using System.Security.Cryptography;

namespace CertificatePinningExample
{
    class CertificatePinningExample
    {
        /* Note: In this example, we're hardcoding a hello certificate's public key and algorithm for 
           demonstration purposes. In a real-world application, this should be stored in a secure
           configuration area that can be updated as needed. */

        private static readonly string PINNED_ALGORITHM = "RSA";

        private static readonly string PINNED_PUBLIC_KEY = "3082010A0282010100B0E75B7CBE56D31658EF79B3A1" +
            "294D506A88DFCDD603F6EF15E7F5BCBDF32291EC50B2B82BA158E905FE6A83EE044A48258B07FAC3D6356AF09B2" +
            "3EDAB15D00507B70DB08DB9A20C7D1201417B3071A346D663A241061C151B6EC5B5B4ECCCDCDBEA24F051962809" +
            "FEC499BF2D093C06E3BDA7D0BB83CDC1C2C6660B8ECB2EA30A685ADE2DC83C88314010FFC7F4F0F895EDDBE5C02" +
            "ABF78E50B708E0A0EB984A9AA536BCE61A0C31DB95425C6FEE5A564B158EE7C4F0693C439AE010EF83CA8155750" +
            "09B17537C29F86071E5DD8CA50EBD8A409494F479B07574D83EDCE6F68A8F7D40447471D05BC3F5EAD7862FA748" +
            "EA3C92A60A128344B1CEF7A0B0D94E50203010001";


        public static void Main(string[] args)
        {
            HttpWebRequest request = (HttpWebRequest)WebRequest.Create("https://azure.microsoft.com");
            request.ServerCertificateValidationCallback = (sender, certificate, chain, sslPolicyErrors) =>
            {
                if (certificate == null || sslPolicyErrors != SslPolicyErrors.None)
                {
                    // Error getting certificate or hello certificate failed basic validation
                    return false;
                }

                var targetKeyAlgorithm = new Oid(certificate.GetKeyAlgorithm()).FriendlyName;
                var targetPublicKey = certificate.GetPublicKeyString();
                
                if (targetKeyAlgorithm == PINNED_ALGORITHM &&
                    targetPublicKey == PINNED_PUBLIC_KEY)
                {
                    // Success, hello certificate matches hello pinned value.
                    return true;
                }
                // Reject, either hello key or hello algorithm does not match hello expected value.
                return false;
            };

            try
            {
                var response = (HttpWebResponse)request.GetResponse();
                Console.WriteLine($"Success, HTTP status code: {response.StatusCode}");
            }
            catch(Exception ex)
            {
                Console.WriteLine($"Failure, {ex.Message}");
            }
            Console.WriteLine("Press any key tooend.");
            Console.ReadKey();
        }
    }
}
```

## <a id="https-transport"></a>Povolit protokol HTTPS – zabezpečený kanál přenosu

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | WCF | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | NET Framework 3 |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [obohacení království](https://vulncat.fortify.com/en/vulncat/index.html) |
| **Kroky** | Konfigurace aplikace Hello se ujistěte, že protokol HTTPS se používá pro všechny informace toosensitive přístup.<ul><li>**Vysvětlení:** Pokud aplikace zpracovává citlivé informace a nepoužívá šifrování na úrovni zpráv a potom ho by měl být povoleny pouze toocommunicate přes přenosu šifrovaný kanál.</li><li>**DOPORUČENÍ:** zkontrolujte, zda je vypnutá přenos HTTP a místo toho povolit přenos HTTPS. Například nahradit hello `<httpTransport/>` s `<httpsTransport/>` značky. Nespoléhejte na tooguarantee síťové konfigurace (brány firewall), aplikace hello lze přistupovat pouze prostřednictvím zabezpečeného kanálu. Z filozofické hlediska by neměl aplikace hello závisí na hello sítě pro jeho zabezpečení.</li></ul><p>Z praktického hlediska hello osoby zodpovídají za zabezpečení sítě hello není vždy sledovat požadavky na zabezpečení hello aplikace hello vývoj.</p>|

## <a id="message-protection"></a>WCF: TooEncryptAndSign úrovně zabezpečení ochrany sadu zpráv

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | WCF | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Rozhraní .NET framework 3 |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [MSDN](https://msdn.microsoft.com/library/ff650862.aspx) |
| **Kroky** | <ul><li>**Vysvětlení:** nastavena úroveň ochrany příliš "žádný" by se zakázat ochranu zprávy. Důvěrnost a integrita je dosaženo s odpovídající úroveň nastavení.</li><li>**DOPORUČENÍ:**<ul><li>Když `Mode=None` – zakáže ochranu zpráv</li><li>Když `Mode=Sign` -přihlásí, ale nešifruje uvítací zprávu; by měl být použit při integritu dat je důležité</li><li>Když `Mode=EncryptAndSign` -přihlásí a šifruje uvítací zprávu</li></ul></li></ul><p>Zvažte vypnutí šifrování a při toovalidate hello integritu informace hello abyste nemuseli dělat starosti důvěrnost stačí jenom podepisování zprávy. To může být užitečné pro operace nebo servisní smlouvy ve které budete potřebovat toovalidate hello původní odesílatele, ale žádná citlivá data se přenáší. Při snížení úrovně ochrany hello, dávejte pozor hello zpráva neobsahuje žádné identifikovatelné osobní údaje (PII).</p>|

### <a name="example"></a>Příklad
Konfigurace služby hello a hello operaci tooonly přihlašovací hello zpráva se zobrazí v hello následující příklady. Příklad kontraktu služby `ProtectionLevel.Sign`: hello následuje příklad použití ProtectionLevel.Sign na úrovni hello kontrakt služby: 
```
[ServiceContract(Protection Level=ProtectionLevel.Sign] 
public interface IService 
  { 
  string GetData(int value); 
  } 
```

### <a name="example"></a>Příklad
Příklad kontrakt operaci `ProtectionLevel.Sign` (pro přesná kontrola): hello následuje příklad použití `ProtectionLevel.Sign` v hello OperationContract úroveň:

```
[OperationContract(ProtectionLevel=ProtectionLevel.Sign] 
string GetData(int value);
``` 

## <a id="least-account-wcf"></a>WCF: Použijte toorun nejméně privilegovaným účtem služby WCF

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | WCF | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Rozhraní .NET framework 3 |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [MSDN](https://msdn.microsoft.com/library/ff648826.aspx ) |
| **Kroky** | <ul><li>**Vysvětlení:** se nespustí služby WCF v části správce nebo účtu vysokou úrovní oprávnění. v případě ohrožení zabezpečení služeb způsobí vysoký dopad.</li><li>**DOPORUČENÍ:** použít nejméně privilegovaným účtem toohost, vaše WCF služby, protože se sníží prostor pro útoky vaší aplikace a omezit možné škody hello, pokud jsou napadení. Pokud účet služby hello vyžaduje další oprávnění na prostředcích infrastruktury, jako je například služby MSMQ, hello protokolu událostí, čítače výkonu a systému souborů hello, příslušná oprávnění má být poskytnut toothese prostředky tak, aby hello služby WCF úspěšně.</li></ul><p>Pokud vaše služba potřebuje tooaccess specifické prostředky jménem hello původní volající, použijte pro příjem dat autorizace kontrolu zosobnění a delegování tooflow hello identitu volajícího. Ve scénáři vývoj pomocí hello místní účet network service, což je speciální předdefinovaný účet, který má omezená oprávnění. V případě produkční vytvoření účtu služby nejméně privilegovaným vlastní doménu.</p>|

## <a id="webapi-https"></a>Vynutit provoz tooWeb všechny rozhraní API přes připojení HTTPS

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Web API | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | MVC5 MVC6 |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Vynucování SSL v Kontroleru webového rozhraní API](http://www.asp.net/web-api/overview/security/working-with-ssl-in-web-api) |
| **Kroky** | Pokud má aplikace HTTPS a vazbu protokolu HTTP, klienti mohou stále používat HTTP tooaccess hello lokality. tooprevent tuto, použijte tooensure filtru akce, které vyžadují tooprotected rozhraní API jsou vždy prostřednictvím protokolu HTTPS.|

### <a name="example"></a>Příklad 
Hello následující kód ukazuje filtr ověřování webového rozhraní API, který kontroluje pro protokol SSL: 
```C#
public class RequireHttpsAttribute : AuthorizationFilterAttribute
{
    public override void OnAuthorization(HttpActionContext actionContext)
    {
        if (actionContext.Request.RequestUri.Scheme != Uri.UriSchemeHttps)
        {
            actionContext.Response = new HttpResponseMessage(System.Net.HttpStatusCode.Forbidden)
            {
                ReasonPhrase = "HTTPS Required"
            };
        }
        else
        {
            base.OnAuthorization(actionContext);
        }
    }
}
```
Přidejte tento filtr tooany webového rozhraní API akce, které vyžadují protokol SSL: 
```C#
public class ValuesController : ApiController
{
    [RequireHttps]
    public HttpResponseMessage Get() { ... }
}
```
 
## <a id="redis-ssl"></a>Ujistěte se, že komunikace tooAzure Redis Cache je přes protokol SSL

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Azure Redis Cache | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Podpora Azure Redis SSL](https://azure.microsoft.com/documentation/articles/cache-faq/#when-should-i-enable-the-non-ssl-port-for-connecting-to-redis) |
| **Kroky** | Redis serveru SSL předinstalované hello nepodporuje, ale v Azure Redis Cache. Pokud se připojujete tooAzure Redis Cache a váš klient podporuje protokol SSL, jako je StackExchange.Redis, musí používat protokol SSL. Ve výchozím nastavení port bez SSL pro nové instance služby Azure Redis Cache zakázán. Ujistěte se, že výchozí nastavení zabezpečení hello se nezmění, dokud je závislost na podporu protokolu SSL pro klienty redis. |

Upozornění: Redis se navrženou toobe přístup důvěryhodné klienty uvnitř důvěryhodného prostředí. To znamená, že obvykle není dobrou nápad tooexpose hello Redis instance přímo toohello internet nebo obecně tooan prostředí, kde můžete přímý přístup k nedůvěryhodní klienti hello Redis TCP port nebo UNIX soketu. 

## <a id="device-field"></a>Zabezpečení komunikace brány tooField zařízení

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Brána pole IoT | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | Pro zařízení, na základě IP může hello komunikační protokol obvykle zapouzdřené v protokolu SSL/TLS kanál tooprotect dat během přenosu. Pro jiné protokoly, které nepodporují SSL/TLS zjistěte, jestli existují zabezpečené verze hello protokolu, které poskytují zabezpečení ve vrstvě přenosu nebo zprávy. |

## <a id="device-cloud"></a>Zabezpečení tooCloud zařízení brány komunikaci pomocí protokolu SSL/TLS

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Brána IoT cloudu | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Zvolte komunikační protokol](https://azure.microsoft.com/documentation/articles/iot-hub-devguide/#messaging) |
| **Kroky** | Zabezpečení protokolu HTTP nebo AMQP nebo protokoly MQTT pomocí protokolu SSL/TLS. |
