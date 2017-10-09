---
title: "aaaAuditing a protokolování - Microsoft Threat modelování nástroj – Azure | Microsoft Docs"
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
ms.openlocfilehash: f28988833eba251b6ceb8bbd47cde37e871af609
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="security-frame-auditing-and-logging--mitigations"></a>Zabezpečení rámce: Auditování a protokolování | Způsoby zmírnění rizik 
| Produktům a službám | Článek |
| --------------- | ------- |
| **Dynamics CRM**    | <ul><li>[Identifikovat citlivé entity ve vašem řešení a implementovat auditování změn](#sensitive-entities)</li></ul> |
| **Webové aplikace** | <ul><li>[Ujistěte se, že se vynucuje auditování a protokolování u aplikace hello](#auditing)</li><li>[Zkontrolujte, zda jsou na místě oběh protokolu a oddělení](#log-rotation)</li><li>[Ujistěte se, aplikace hello není přihlašujete citlivá uživatelská data](#log-sensitive-data)</li><li>[Ujistěte se, že auditu a soubory protokolů mají omezený přístup](#log-restricted-access)</li><li>[Ujistěte se, že jsou zaznamenány události správy uživatelů](#user-management)</li><li>[Zkontrolujte, zda text hello systému integrované obranu proti zneužití](#inbuilt-defenses)</li><li>[Povolit protokolování diagnostiky pro webové aplikace v Azure App Service](#diagnostics-logging)</li></ul> |
| **Database** | <ul><li>[Ujistěte se, že přihlášení auditování je povolena v systému SQL Server](#identify-sensitive-entities)</li><li>[Povolení detekce hrozeb v Azure SQL](#threat-detection)</li></ul> |
| **Azure Storage** | <ul><li>[Použití Azure Storage Analytics tooaudit přístupu Azure Storage](#analytics)</li></ul> |
| **WCF** | <ul><li>[Implementace dostatečná protokolování](#sufficient-logging)</li><li>[Implementace dostatečná auditu selhání zpracování](#audit-failure-handling)</li></ul> |
| **Webové rozhraní API** | <ul><li>[Ujistěte se, že se vynucuje auditování a protokolování u webového rozhraní API](#logging-web-api)</li></ul> |
| **Brána pole IoT** | <ul><li>[Ujistěte se, že odpovídající auditování a protokolování se vynucuje u brána pole](#logging-field-gateway)</li></ul> |
| **Brána IoT cloudu** | <ul><li>[Ujistěte se, že odpovídající auditování a protokolování se vynucuje u brány cloudu](#logging-cloud-gateway)</li></ul> |

## <a id="sensitive-entities"></a>Identifikovat citlivé entity ve vašem řešení a implementovat auditování změn

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Dynamics CRM | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky**                   | Určit entity ve vašem řešení obsahující citlivá data a implementovat auditování změn v těchto entit a polí |

## <a id="auditing"></a>Ujistěte se, že se vynucuje auditování a protokolování u aplikace hello

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky**                   | Povolte auditování a protokolování na všechny součásti. Protokoly auditu měli zaznamenat kontextu uživatele. Identifikujte všechny důležité události a zaznamenat tyto události do protokolu. Implementace centralizované protokolování |

## <a id="log-rotation"></a>Zkontrolujte, zda jsou na místě oběh protokolu a oddělení

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky**                   | <p>Oběh protokolu se používá v systému správy, ve kterém datem protokolu soubory jsou archivovány automatizovaný proces. Servery, které používají velké aplikace často protokolovat každou žádost: hello stěně objemné protokoly, je oběh protokolu způsob toolimit hello celková velikost protokolů hello zároveň umožňuje analýzu poslední události. </p><p>Oddělení protokolu v podstatě znamená, že budete mít toostore útokům vaše soubory protokolu na jiný oddíl jako kde operačního systému nebo aplikace běží v pořadí tooavert odmítnutí služby nebo hello přechod na starší verzi aplikace jeho výkon</p>|

## <a id="log-sensitive-data"></a>Ujistěte se, aplikace hello není přihlašujete citlivá uživatelská data

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky**                   | <p>Kontrola protokolu citlivé údaje, aby uživatel odešle tooyour lokality. Kontrola úmyslné protokolování, jakož i vedlejší účinky, které jsou způsobeny problémy návrhu. Příklady citlivých dat:</p><ul><li>Přihlašovací údaje uživatele</li><li>Číslo sociálního pojištění nebo jiné identifikační údaje</li><li>Čísla platebních karet nebo jiné finanční informace</li><li>Informace o stavu</li><li>Privátní klíče nebo jiná data, které by mohly být použité toodecrypt šifrované informace</li><li>Systém nebo aplikace, informace, které mohou být použity toomore efektivně útokům aplikace hello</li></ul>|

## <a id="log-restricted-access"></a>Ujistěte se, že auditu a soubory protokolů mají omezený přístup

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky**                   | <p>Zkontrolujte, zda jsou správně nastaveny tooensure přístup práva toolog soubory. Účty aplikace by měl mít přístup jen pro zápis a operátory a pracovníky technické podpory by měl mít přístup jen pro čtení, podle potřeby.</p><p>Účty správců jsou hello pouze účty, které by měl mít úplný přístup. Zkontrolujte, že seznamů ACL systému Windows v protokolu soubory tooensure jsou správně s omezeným přístupem:</p><ul><li>Účty aplikace mají mít přístup jen pro zápis</li><li>Operátory a pracovníky technické podpory musí mít oprávnění jen pro čtení, podle potřeby</li><li>Správci jsou hello pouze účty, které by měl mít úplný přístup</li></ul>|

## <a id="user-management"></a>Ujistěte se, že jsou zaznamenány události správy uživatelů

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky**                   | <p>Zkontrolujte, zda aplikace hello monitoruje událostí správy uživatele jako je přihlášení úspěšné i neúspěšné uživatelů, resetování hesel, změny hesel, uzamčení účtu, registrace uživatele. To pomáhá toodetect a reagovat toopotentially podezřelého chování. Umožňuje také dat nástroje operations toogather; například tootrack, kdo přistupuje k aplikaci hello</p>|

## <a id="inbuilt-defenses"></a>Zkontrolujte, zda text hello systému integrované obranu proti zneužití

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky**                   | <p>Ovládací prvky musí být na místě, které vyvolávají výjimka zabezpečení v případě zneužití aplikací. Například pokud ověření vstupu je na místě a útočník pokusí tooinject škodlivého kódu, který neodpovídá hello regex, vyvolána výjimka zabezpečení může být může být předběžné zneužít systému</p><p>Doporučujeme například toohave výjimky zabezpečení přihlášení a akce prováděné na hello následující problémy:</p><ul><li>Ověření vstupu</li><li>Porušení proti útokům CSRF</li><li>Útok hrubou silou (horní limit pro počet požadavků na uživatele na prostředků)</li><li>Narušení nahrávání souborů</li><ul>|

## <a id="diagnostics-logging"></a>Povolit protokolování diagnostiky pro webové aplikace v Azure App Service

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | EnvironmentType – Azure |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | <p>Azure poskytuje integrované diagnostiky tooassist s laděním aplikační službu webové aplikace. Platí také tooAPI a mobilní aplikace. Webové aplikace služby App Service poskytují diagnostické funkce pro protokolování informací z hello webový server a hello webové aplikace.</p><p>Tyto jsou logicky rozdělené do webového serveru diagnostics a application diagnostics</p>|

## <a id="identify-sensitive-entities"></a>Ujistěte se, že přihlášení auditování je povolena v systému SQL Server

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Databáze | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Konfigurace auditování přihlášení](https://msdn.microsoft.com/library/ms175850.aspx) |
| **Kroky** | <p>Auditování databáze serveru přihlášení musí být povoleno toodetect nebo potvrďte za účelem uhodnutí hesla útoky. Je důležité toocapture neúspěšné pokusy o přihlášení. Další výhodou zaznamenávání obou pokusů o přihlášení úspěšné i neúspěšné poskytuje během forenzního vyšetřování</p>|

## <a id="threat-detection"></a>Povolení detekce hrozeb v Azure SQL

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Databáze | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | SQL Azure |
| **Atributy**              | Verze SQL - V12 |
| **Odkazy**              | [Začínáme s detekce hrozeb databáze SQL](https://azure.microsoft.com/documentation/articles/sql-database-threat-detection-get-started/)|
| **Kroky** |<p>Detekce hrozeb zjistila nezvyklé databázové aktivity, které indikují potenciální bezpečnostní hrozby toohello databáze. Poskytuje novou vrstvu zabezpečení, což umožňuje zákazníkům toodetect a toopotential hrozeb reagovat, když k nim dojde tím, že poskytuje výstrahy zabezpečení na neobvyklé aktivity.</p><p>Uživatele můžete prozkoumat hello podezřelé události pomocí audity Azure SQL Database toodetermine pokud vyplývají z tooaccess pokusu o porušení nebo zneužití data v databázi hello.</p><p>Detekce hrozeb je jednoduchý tooaddress potenciální hrozby toohello databáze bez hello nutné toobe expert zabezpečení nebo spravovat pokročilým zabezpečením monitorování systémů</p>|

## <a id="analytics"></a>Použití Azure Storage Analytics tooaudit přístupu Azure Storage

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Azure Storage | 
| **SDL fáze**               | Nasazení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici |
| **Odkazy**              | [Pomocí Storage Analytics toomonitor autorizace typu](https://azure.microsoft.com/documentation/articles/storage-security-guide/#storage-analytics) |
| **Kroky** | <p>Pro každý účet úložiště jeden můžete povolit protokolování tooperform Azure Storage Analytics a ukládat data metriky. Hello úložiště analýzy protokolů obsahují důležité informace, jako je například metodu ověřování používanou někdo při přístupu k úložišti.</p><p>To může být velmi užitečné, pokud jsou úzce ochrana toostorage přístup. V úložišti objektů Blob můžete například nastavit všechny kontejnery tooprivate hello a implementovat hello použití služby SAS v celé vaší aplikace. Zkontrolujte, zda text hello protokoly pravidelně toosee Pokud objektů BLOB ke kterým se přistupuje pomocí klíčů k účtu úložiště hello, které mohou indikovat porušení zabezpečení, nebo pokud jsou objekty BLOB hello veřejné, ale nemělo by být.</p>|

## <a id="sufficient-logging"></a>Implementace dostatečná protokolování

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | WCF | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Rozhraní .NET framework |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [obohacení království](https://vulncat.fortify.com/en/vulncat/index.html) |
| **Kroky** | <p>Nedostatečná Hello správné kontrolní záznam incidentu zabezpečení může zabránit spuštění forenzní úsilí. Windows Communication Foundation (WCF) nabízí hello možnost toolog úspěšné nebo neúspěšné pokusy o přihlášení.</p><p>Protokolování neúspěšných pokusů o přihlášení může upozornit, že správci potenciální útoky hrubou silou. Protokolování událostí úspěšné ověření podobně, zadejte záznam pro audit užitečné při ohrožení legitimní účtu. Povolení funkce auditu zabezpečení služby WCF na |

### <a name="example"></a>Příklad
Následuje příklad konfigurace auditování povolené Hello
```
<system.serviceModel>
    <behaviors>
        <serviceBehaviors>
            <behavior name=""NewBehavior"">
                <serviceSecurityAudit auditLogLocation=""Default""
                suppressAuditFailure=""false"" 
                serviceAuthorizationAuditLevel=""SuccessAndFailure""
                messageAuthenticationAuditLevel=""SuccessAndFailure"" />
                ...
            </behavior>
        </servicebehaviors>
    </behaviors>
</system.serviceModel>
```

## <a id="audit-failure-handling"></a>Implementace dostatečná auditu selhání zpracování

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | WCF | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Rozhraní .NET framework |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [obohacení království](https://vulncat.fortify.com/en/vulncat/index.html) |
| **Kroky** | <p>Je nakonfigurovaný vyvinuté řešení toogenerate výjimku, pokud se nezdaří toowrite tooan auditní protokol. Pokud je nakonfigurovaný WCF toothrow výjimku, pokud je protokol auditování tooan nelze toowrite, programu hello nebudete nijak upozorněni hello selhání a může přestat docházet k auditování událostí zabezpečení.</p>|

### <a name="example"></a>Příklad
Hello `<behavior/>` prvek hello WCF konfiguračního souboru níže dá pokyn WCF toonot informovala, že aplikace hello WCF selže toowrite tooan auditní protokol.
````
<behaviors>
    <serviceBehaviors>
        <behavior name="NewBehavior">
            <serviceSecurityAudit auditLogLocation="Application"
            suppressAuditFailure="true"
            serviceAuthorizationAuditLevel="Success"
            messageAuthenticationAuditLevel="Success" />
        </behavior>
    </serviceBehaviors>
</behaviors>
````
Konfigurace programu hello toonotify WCF vždy, když je protokol auditování tooan nelze toowrite. Hello program by měl mít příslušné schéma alternativní oznámení v místě tooalert hello organizace nejsou neudržují záznamy auditu. 

## <a id="logging-web-api"></a>Ujistěte se, že se vynucuje auditování a protokolování u webového rozhraní API

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Web API | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | Povolte auditování a protokolování na webovým rozhraním API. Protokoly auditu měli zaznamenat kontextu uživatele. Identifikujte všechny důležité události a zaznamenat tyto události do protokolu. Implementace centralizované protokolování |

## <a id="logging-field-gateway"></a>Ujistěte se, že odpovídající auditování a protokolování se vynucuje u brána pole

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Brána pole IoT | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | <p>Pokud se více zařízení připojují tooa brána pole, zajistěte, aby pokusy o připojení a stavu ověření (úspěch nebo neúspěch) pro jednotlivá zařízení jsou přihlášení a udržovaný na hello brána pole.</p><p>Také v případech, kde je brána pole zachování hello IoT Hub přihlašovací údaje pro jednotlivá zařízení, ujistěte se, že auditování se provádí tyto přihlašovací údaje jsou načteny. Vývoj proces tooperiodically nahrávání hello protokoly tooAzure IoT Hub úložiště pro dlouhodobé uchovávání.</p> |

## <a id="logging-cloud-gateway"></a>Ujistěte se, že odpovídající auditování a protokolování se vynucuje u brány cloudu

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Brána IoT cloudu | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Úvod tooIoT rozbočovače operations monitorování](https://azure.microsoft.com/documentation/articles/iot-hub-operations-monitoring/) |
| **Kroky** | <p>Návrh pro shromažďování a ukládání dat auditu shromáždit prostřednictvím monitorování nástroje Operations IoT Hub. Povolte hello následující monitorování kategorií:</p><ul><li>Operace identity zařízení</li><li>Komunikace zařízení cloud</li><li>Komunikace cloud zařízení</li><li>Připojení</li><li>Nahrávání souborů</li></ul>|
