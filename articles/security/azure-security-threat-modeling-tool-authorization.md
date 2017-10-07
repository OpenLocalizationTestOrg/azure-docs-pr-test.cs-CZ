---
title: "aaaAuthorization - Microsoft Threat modelování nástroj – Azure | Microsoft Docs"
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
ms.openlocfilehash: 3ea7ae2b46baa8578e574e6006b98dfe172829e7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="security-frame-authorization--mitigations"></a>Zabezpečení rámce: Autorizace | Způsoby zmírnění rizik 
| Produktům a službám | Článek |
| --------------- | ------- |
| **Počítač hranice vztahů důvěryhodnosti** | <ul><li>[Zajistěte, aby byly správné seznamy řízení přístupu nakonfigurované toorestrict neoprávněného přístupu toodata na zařízení hello](#acl-restricted-access)</li><li>[Ujistěte se, že je v adresáři profil uživatele uložený obsah aplikace citlivé specifický pro uživatele](#sensitive-directory)</li><li>[Ujistěte se, že hello nasazené aplikace jsou spouštět s nejnižšími oprávněními](#deployed-privileges)</li></ul> |
| **Webové aplikace** | <ul><li>[Vynutit sekvenční krok pořadí při zpracování toky obchodní logiky](#sequential-logic)</li><li>[Implementace míru omezení mechanismus tooprevent – výčet](#rate-enumeration)</li><li>[Zajistěte, aby se místní správné ověření a je následovaný Princip nejnižších oprávnění](#principle-least-privilege)</li><li>[Obchodní logiky a prostředků přístup autorizačních rozhodnutích by neměl být na základě příchozí požadavek parametrů](#logic-request-parameters)</li><li>[Ujistěte se, že obsah a nejsou prostředky výčtové nebo zpřístupněno vynuceného procházení](#enumerable-browsing)</li></ul> |
| **Database** | <ul><li>[Zajistěte, aby nejmenší privilegovaných účtů používaných tooconnect tooDatabase serveru](#privileged-server)</li><li>[Implementace zabezpečení na úrovni řádku RLS tooprevent klientům v přístupu k výměně dat](#rls-tenants)</li><li>[Sysadmin role by měla mít pouze platné potřeby uživatelů](#sysadmin-users)</li></ul> |
| **Brána IoT cloudu** | <ul><li>[Připojit tooCloud brány pomocí nejméně privilegovaným tokeny](#cloud-least-privileged)</li></ul> |
| **Centra událostí Azure** | <ul><li>[Použít jen odesílání oprávnění klíče SAS ke generování tokenů zařízení](#sendonly-sas)</li><li>[Nepoužívejte přístupové tokeny, které poskytují toohello přímý přístup do centra událostí](#access-tokens-hub)</li><li>[TooEvent, které centra pomocí SAS klíče, že mají hello minimální oprávnění požadovaná pro připojení](#sas-minimum-permissions)</li></ul> |
| **Azure Documentdb** | <ul><li>[Použití prostředků tokeny tooconnect tooDocumentDB kdykoli je to možné](#resource-docdb)</li></ul> |
| **Hranice vztahů důvěryhodnosti Azure** | <ul><li>[Povolit podrobné přístup správu tooAzure předplatné pomocí RBAC](#grained-rbac)</li></ul> |
| **Hranice vztahů důvěryhodnosti Service Fabric** | <ul><li>[Omezit operace toocluster přístupu klienta pomocí RBAC](#cluster-rbac)</li></ul> |
| **Dynamics CRM** | <ul><li>[Provedení modelování zabezpečení a použití zabezpečení na úrovni pole případě požadavku](#modeling-field)</li></ul> |
| **Dynamics CRM portálu** | <ul><li>[Provedení modelování zabezpečení portálu účtů zachování Pamatujte, že hello model zabezpečení pro portál hello se liší od hello zbytek CRM](#portal-security)</li></ul> |
| **Azure Storage** | <ul><li>[Jemně odstupňovaná oprávnění grant na rozsahu entit v Azure Table Storage](#permission-entities)</li><li>[Povolit účet úložiště tooAzure řízení přístupu na základě rolí (RBAC) pomocí Azure Resource Manager](#rbac-azure-manager)</li></ul> |
| **Mobilního klienta** | <ul><li>[Implementovat implicitní jailbreaků nebo vytvoření kořenového adresáře detekce](#rooting-detection)</li></ul> |
| **WCF** | <ul><li>[Slabé odkazu ve WCF](#weak-class-wcf)</li><li>[Řízení autorizace implementace WCF](#wcf-authz)</li></ul> |
| **Webové rozhraní API** | <ul><li>[Implementace mechanismus správné autorizace v rozhraní ASP.NET Web API](#authz-aspnet)</li></ul> |
| **Zařízení IoT** | <ul><li>[Provedení kontroly autorizace v zařízení hello, pokud ji podporuje různé akce, které vyžadují různé úrovně oprávnění](#device-permission)</li></ul> |
| **Brána pole IoT** | <ul><li>[Provedení kontroly autorizace v hello brána pole, pokud ji podporuje různé akce, které vyžadují různé úrovně oprávnění](#field-permission)</li></ul> |

## <a id="acl-restricted-access"></a>Zajistěte, aby byly správné seznamy řízení přístupu nakonfigurované toorestrict neoprávněného přístupu toodata na zařízení hello

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Počítač hranice vztahů důvěryhodnosti | 
| **SDL fáze**               | Nasazení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | Zajistěte, aby byly správné seznamy řízení přístupu nakonfigurované toorestrict neoprávněného přístupu toodata na zařízení hello|

## <a id="sensitive-directory"></a>Ujistěte se, že je v adresáři profil uživatele uložený obsah aplikace citlivé specifický pro uživatele

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Počítač hranice vztahů důvěryhodnosti | 
| **SDL fáze**               | Nasazení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | Ujistěte se, že je v adresáři profil uživatele uložený obsah aplikace citlivé specifický pro uživatele. Toto je tooprevent více uživatelů hello počítač z přístup k datům uživatele toho druhého.|

## <a id="deployed-privileges"></a>Ujistěte se, že hello nasazené aplikace jsou spouštět s nejnižšími oprávněními

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Počítač hranice vztahů důvěryhodnosti | 
| **SDL fáze**               | Nasazení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | Ujistěte se, že hello nasazené aplikace spuštěna s nejnižšími oprávněními. |

## <a id="sequential-logic"></a>Vynutit sekvenční krok pořadí při zpracování toky obchodní logiky

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | V pořadí tooverify, tato fáze byl spuštěn uživatelem originální chcete tooenforce hello aplikace tooonly obchodní logiky toky procesu v sekvenčních krok pořadí s všechny kroky, které zpracovává realistické lidského včas a není zpracovat mimo pořadí, vynecháno kroky, zpracování kroky od jiného uživatele, nebo příliš rychle odeslána transakce.|

## <a id="rate-enumeration"></a>Implementace míru omezení mechanismus tooprevent – výčet

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | Zajistěte, aby citlivá identifikátory náhodné. Implementovat řízení CAPTCHA na stránkách anonymní. Ujistěte se, že chyby a výjimky by neměl odhalit konkrétní data|

## <a id="principle-least-privilege"></a>Zajistěte, aby se místní správné ověření a je následovaný Princip nejnižších oprávnění

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | <p>Princip Hello znamená poskytnete jenom oprávnění, které jsou nezbytné toothat uživatelé pracovní účet uživatele. Například zálohování uživatel nemusí softwaru tooinstall: proto hello zálohování uživatel má oprávnění pouze toorun zálohování a spojených s aplikací. Další oprávnění, například při instalaci nového softwaru, jsou zablokované. Hello princip platí také tooa osobní počítač uživatele, který obvykle pracuje v jako běžný uživatelský účet a otevře privilegovaného, chráněný heslem účtu (tj. superuživatel) pouze když hello absolutně vyžádá situace ho. </p><p>Tato zásada může být také použité tooyour webových aplikací. Místo výhradně v závislosti na metody ověřování na základě rolí pomocí relace, místo chceme, že tooassign oprávnění toousers prostřednictvím ověřování založeného na databázi systému. Relace v pořadí tooidentify jsme i nadále používat, když hello byl uživatel přihlášen správně, pouze teď místo přiřazení tohoto uživatele s konkrétní rolí jsme přiřaďte mu s oprávněními tooverify akce, které se privilegované tooperform systému hello. Také velký pro této metody se vždy, když má uživatel toobe přiřazené méně oprávnění, změny se použijí na hello chodu vzhledem k tomu, že hello přiřazení není závislá na hello relace, který jinak měl tooexpire nejprve.</p>|

## <a id="logic-request-parameters"></a>Obchodní logiky a prostředků přístup autorizačních rozhodnutích by neměl být na základě příchozí požadavek parametrů

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | Vždy, když se kontrola, zda je uživatel s omezeným přístupem tooreview zpracování určitých dat, hello přístupu, kterou by měla být omezení na straně serveru. Hello userID by měly být uložené v proměnné relace na přihlášení a měl by být použité tooretrieve uživatelská data z databáze hello |

### <a name="example"></a>Příklad
```SQL
SELECT data 
FROM personaldata 
WHERE userID=:id < - session var 
```
Útočník možné nelze nyní manipulovat a změnit hello aplikace operaci, protože hello identifikátor pro načítání hello data jsou zpracovávány straně serveru.

## <a id="enumerable-browsing"></a>Ujistěte se, že obsah a nejsou prostředky výčtové nebo zpřístupněno vynuceného procházení

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Webová aplikace | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | <p>Citlivé statické a konfigurační soubory by neměly zachovány v hello web-root. Pro obsahu není požadováno toobe veřejný přístup, který má být použita ovládací prvky nebo odebrání hello obsahu sám sebe.</p><p>Navíc vynuceného procházení obvykle spolu s hrubou silou techniky toogather informace pokusem tooaccess tolik adresy URL jako možné tooenumerate adresářů a souborů na serveru. Útočníci mohou vyhledat všechny varianty běžně existující soubory. Hledání souborů a hesla by zahrnovat například soubory včetně psswd.txt, password.htm, password.dat a dalších odlišností.</p><p>toomitigate to možnosti pro detekce útoku hrubou silou pokusí by měly být zahrnuty.</p>|

## <a id="privileged-server"></a>Zajistěte, aby nejmenší privilegovaných účtů používaných tooconnect tooDatabase serveru

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Databáze | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [SQL Database oprávnění hierarchie](https://msdn.microsoft.com/library/ms191465), [zabezpečitelné prostředky databáze SQL](https://msdn.microsoft.com/library/ms190401) |
| **Kroky** | Nejmenší privilegovaných účtů by měl být použité tooconnect toohello databáze. Přihlášení aplikace by měl být omezen hello databáze a by měla spustit pouze vybrané uložené procedury. Přihlášení aplikace by měla mít žádný přímý přístup k tabulce. |

## <a id="rls-tenants"></a>Implementace zabezpečení na úrovni řádku RLS tooprevent klientům v přístupu k výměně dat

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Databáze | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | SQL Azure a místní |
| **Atributy**              | MsSQL2016 verze - V12, verze SQL - SQL |
| **Odkazy**              | [Zabezpečení SQL serveru úrovni řádků (RLS)](https://msdn.microsoft.com/library/azure/dn765131.aspx) |
| **Kroky** | <p>Zabezpečení na úrovni řádků umožňuje zákazníkům toocontrol přístup toorows v tabulce databáze na základě charakteristik hello hello uživatele provádění dotazu (například skupinu členství nebo provádění kontextu).</p><p>Zabezpečení úrovni řádků (RLS) zjednodušuje hello návrhu a kódování zabezpečení ve vaší aplikaci. RLS umožňuje tooimplement omezení přístupu k datům řádek. Například pracovníci přístup pouze tyto řádky dat, které jsou příslušné tootheir oddělení zajistí nebo omezení zákazníka data přístup tooonly hello data relevantní tootheir společnosti.</p><p>logiku omezení přístupu Hello je umístěn v databázové vrstvy hello spíše než tokeny na základě dat hello v jiné aplikační vrstvě. databázový systém Hello platí omezení přístupu hello pokaždé, když dojde k pokusu o tento přístup k datům z libovolné úrovně. Díky systému zabezpečení hello spolehlivější a robustní snížením hello útoku hello zabezpečení systému.</p><p>|

Upozorňujeme, že RLS jako funkce se na pole databáze je použít jenom tooSQL serveru od 2016 a Azure SQL database. Pokud funkce RLS se na pole hello není implementována, je třeba zajistit, že je přístup k datům s omezeným přístupem pomocí zobrazení a postupy

## <a id="sysadmin-users"></a>Sysadmin role by měla mít pouze platné potřeby uživatelů

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Databáze | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [SQL Database oprávnění hierarchie](https://msdn.microsoft.com/library/ms191465), [zabezpečitelné prostředky databáze SQL](https://msdn.microsoft.com/library/ms190401) |
| **Kroky** | Členové pevné role serveru SysAdmin hello by měl být hodně omezené a nikdy nesmí obsahovat účty používané aplikace.  Zkontrolujte hello seznam uživatelů v roli hello a odeberte všechny nepotřebné účty|

## <a id="cloud-least-privileged"></a>Připojit tooCloud brány pomocí nejméně privilegovaným tokeny

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Brána IoT cloudu | 
| **SDL fáze**               | Nasazení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Volba brány - Azure IoT Hub |
| **Odkazy**              | [Řízení přístupu služby IOT Hub](https://azure.microsoft.com/documentation/articles/iot-hub-devguide/#Security) |
| **Kroky** | Zadejte nejnižší oprávnění oprávnění toovarious součásti, které se připojují tooCloud brány (IoT Hub). Typickým příkladem je – součást zřizování nebo správy zařízení používá registryread a zápis, procesor událostí (ASA) používá služba připojit. Jednotlivých zařízení se připojují přes přihlašovací údaje zařízení|

## <a id="sendonly-sas"></a>Použít jen odesílání oprávnění klíče SAS ke generování tokenů zařízení

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Centra událostí Azure | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Ověřování a zabezpečení modelu přehled služby Event Hubs](https://azure.microsoft.com/documentation/articles/event-hubs-authentication-and-security-model-overview/) |
| **Kroky** | Klíč SAS je použité toogenerate jednotlivých zařízení tokeny. Pomocí oprávnění jen pro odeslání SAS klíč při generování hello token zařízení pro daného vydavatele|

## <a id="access-tokens-hub"></a>Nepoužívejte přístupové tokeny, které poskytují toohello přímý přístup do centra událostí

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Centra událostí Azure | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Ověřování a zabezpečení modelu přehled služby Event Hubs](https://azure.microsoft.com/documentation/articles/event-hubs-authentication-and-security-model-overview/) |
| **Kroky** | Token, který uděluje přímý přístup do centra událostí toohello by se neměla poskytovat toohello zařízení. Pomocí nejnižšími oprávněními tokenu pro hello zařízení, které získávat přístup pouze tooa vydavatele by pomáhají identifikovat a pokud ji blokovaných najít toobe podvodný nebo dojde k ohrožení zařízení.|

## <a id="sas-minimum-permissions"></a>TooEvent, které centra pomocí SAS klíče, že mají hello minimální oprávnění požadovaná pro připojení

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Centra událostí Azure | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Ověřování a zabezpečení modelu přehled služby Event Hubs](https://azure.microsoft.com/documentation/articles/event-hubs-authentication-and-security-model-overview/) |
| **Kroky** | Zadejte nejnižší oprávnění oprávnění toovarious back-end aplikace, které se připojují toohello centra událostí. Generovat samostatné klíče SAS pro každou aplikaci back-end a umožněte jenom hello požadované oprávnění - toothem spravovat, Receive nebo Send.|

## <a id="resource-docdb"></a>Použití prostředků tokeny tooconnect tooCosmos DB, kdykoli je to možné

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Azure Documentdb | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | Token prostředku je přidružen k DocumentDB oprávnění zdroji a zachycení hello vztah mezi hello uživatel oprávnění databáze a hello tento uživatel má pro konkrétní prostředek DocumentDB aplikace (např. kolekce a dokumentu). Vždy použijte hello tokenu tooaccess prostředků DocumentDB, pokud klient hello nelze důvěřovat s nakládání s klíči hlavní nebo jen pro čtení - jako koncový uživatel aplikace, jako je mobilní nebo desktopové klienta. Pomocí hlavního klíče nebo klíče jen pro čtení z back-end aplikace, které můžete bezpečně uložit tyto klíče.|

## <a id="grained-rbac"></a>Povolit podrobné přístup správu tooAzure předplatné pomocí RBAC

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Hranice vztahů důvěryhodnosti Azure | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Používat roli přiřazení toomanage přístup tooyour předplatného Azure prostředky](https://azure.microsoft.com/documentation/articles/role-based-access-control-configure/)  |
| **Kroky** | Řízení přístupu na základě role v Azure umožňuje přesnou správu přístupu. Pomocí RBAC, můžete udělit pouze hello množství přístup tito uživatelé si musí tooperform svou práci.|

## <a id="cluster-rbac"></a>Omezit operace toocluster přístupu klienta pomocí RBAC

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Hranice vztahů důvěryhodnosti Service Fabric | 
| **SDL fáze**               | Nasazení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Prostředí – Azure |
| **Odkazy**              | [Řízení přístupu na základě rolí pro klienty Service Fabric](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security-roles/) |
| **Kroky** | <p>Azure Service Fabric podporuje dva typy ovládacích prvků různý přístup pro klienty, které jsou připojené tooa cluster Service Fabric: správce a uživatele. Řízení přístupu umožňuje hello toolimit clusteru správce přístup toocertain operace na clusteru pro různé skupiny uživatelů, lepší zabezpečení hello clusteru.</p><p>Správci mají plný přístup toomanagement možnosti (včetně možnosti pro čtení i zápis). Uživatelé, ve výchozím nastavení, mají pouze možnosti toomanagement přístup pro čtení (například použití dotazů) a hello možnost tooresolve aplikací a služeb.</p><p>V době vytváření clusteru hello tím, že poskytuje samostatné certifikáty pro každou zadáte hello dva klientské role (správce a klient).</p>|

## <a id="modeling-field"></a>Provedení modelování zabezpečení a použití zabezpečení na úrovni pole případě požadavku

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Dynamics CRM | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | Provedení modelování zabezpečení a použití zabezpečení na úrovni pole případě požadavku|

## <a id="portal-security"></a>Provedení modelování zabezpečení portálu účtů zachování Pamatujte, že hello model zabezpečení pro portál hello se liší od hello zbytek CRM

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Dynamics CRM portálu | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | Provedení modelování zabezpečení portálu účtů zachování Pamatujte, že hello model zabezpečení pro portál hello se liší od hello zbytek CRM|

## <a id="permission-entities"></a>Jemně odstupňovaná oprávnění grant na rozsahu entit v Azure Table Storage

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Azure Storage | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | StorageType – tabulka |
| **Odkazy**              | [Jak toodelegate přistupovat k tooobjects ve vašem účtu úložiště Azure, pomocí SAS](https://azure.microsoft.com/documentation/articles/storage-security-guide/#_data-plane-security) |
| **Kroky** | V některých scénářích firmy může být Azure Table Storage požadované toostore citlivá data, která je určen toodifferent strany. Například citlivá data, která se týkají toodifferent zemích. V takových případech SAS podpisy jde konstruovat zadáním hello oddílu a řádku rozsahy klíčů, tak, že uživatel může přistupovat k dat konkrétní tooa konkrétní země.| 

## <a id="rbac-azure-manager"></a>Povolit účet úložiště tooAzure řízení přístupu na základě rolí (RBAC) pomocí Azure Resource Manager

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Azure Storage | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [Jak toosecure úložiště účet pomocí řízení přístupu na základě rolí (RBAC)](https://azure.microsoft.com/documentation/articles/storage-security-guide/#management-plane-security) |
| **Kroky** | <p>Když vytvoříte nový účet úložiště, můžete vybrat model nasazení Classic nebo Azure Resource Manager. Hello Klasický model vytváření prostředků v Azure jenom umožňuje vše nebo nic přístup toohello předplatného a pak hello účet úložiště.</p><p>S hello modelu Azure Resource Manager uveďte účet úložiště hello v prostředku skupiny a řízení přístupu toohello správu rovině tohoto účtu konkrétní úložiště pomocí služby Azure Active Directory. Například můžete uživatelům konkrétní hello možnost tooaccess hello klíče účtu úložiště, můžete zobrazit informace o účtu úložiště hello jiných uživatelů, ale nemůže přístupové klíče účtu úložiště hello.</p>|

## <a id="rooting-detection"></a>Implementovat implicitní jailbreaků nebo vytvoření kořenového adresáře detekce

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Mobilního klienta | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | <p>Aplikace by měla chránit svou vlastní konfiguraci a uživatelská data v případě, pokud je telefonní root nebo jailbreak. Vytvoření kořenového adresáře nebo jailbreak nejnovější znamená neoprávněným přístupem, které běžným uživatelům nebude provádět na svých vlastních telefonech. Aplikace má proto logika pro detekci implicitní při spuštění aplikace, toodetect Pokud hello phone nemá root.</p><p>Logika pro detekci Hello můžete jednoduše přistupovat k soubory, které obvykle jenom kořenový uživatel získat přístup, například:</p><ul><li>/System/App/superuser.apk</li><li>/ sbin/su</li><li>/System/Bin/su</li><li>/System/xbin/su</li><li>/data/Local/xbin/su</li><li>/data/local/bin/su</li><li>/System/SD/xbin/su</li><li>/System/Bin/failsafe/su</li><li>/data/Local/su</li></ul><p>Pokud aplikace hello mohou přistupovat k některým z těchto souborů, označuje, zda text hello aplikace běží jako kořenové uživatele.</p>|

## <a id="weak-class-wcf"></a>Slabé odkazu ve WCF

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | WCF | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné, NET Framework 3 |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [obohacení království](https://vulncat.fortify.com/en/vulncat/index.html) |
| **Kroky** | <p>slabé třída odkaz, který může útočníkovi umožnit tooexecute Neautorizováno kód používá systém Hello. Hello program odkazuje uživatelsky definované třídy, která není jednoznačně identifikovat. Když .NET načte tato slabě identifikovaných třída hello CLR typ zavaděč vyhledá hello třídy v hello následující umístění v hello zadané pořadí:</p><ol><li>Pokud je znám hello sestavení hello typu hello zavaděč hledání hello konfiguračního souboru přesměrování umístění, GAC, hello aktuální sestavení pomocí informace o konfiguraci a hello základního adresáře aplikace</li><li>Pokud hello sestavení není znám, hledání zavaděč hello hello aktuální sestavení, mscorlib a umístění hello vrácený obslužné rutiny události TypeResolve hello</li><li>Toto pořadí hledání CLR lze upravit s háky například hello předávání typu mechanismus a hello AppDomain.TypeResolve událostí</li></ol><p>Pokud útočník dokáže pořadí hledání CLR hello vytvořením hello alternativní třída se stejným názvem a že hello CLR načte nejprve umístění do alternativního umístění, hello CLR nechtěně spustí hello útočník zadaný kód</p>|

### <a name="example"></a>Příklad
Hello `<behaviorExtensions/>` prvek hello WCF konfiguračního souboru níže dá pokyn WCF tooadd vlastní chování tooa konkrétní WCF rozšíření třídy.
```
<system.serviceModel>
    <extensions>
        <behaviorExtensions>
            <add name=""myBehavior"" type=""MyBehavior"" />
        </behaviorExtensions>
    </extensions>
</system.serviceModel>
```
Pomocí plně kvalifikované názvy (silné) jednoznačně identifikuje typu a ještě zvyšuje zabezpečení vašeho systému. Pomocí názvů plně kvalifikovaný při registraci typy v souboru machine.config a app.config soubory hello.

### <a name="example"></a>Příklad
Hello `<behaviorExtensions/>` prvek hello WCF konfiguračního souboru níže dá pokyn WCF tooadd důrazně odkazuje vlastní chování tooa konkrétní WCF rozšíření třídy.
```
<system.serviceModel>
    <extensions>
        <behaviorExtensions>
            <add name=""myBehavior"" type=""Microsoft.ServiceModel.Samples.MyBehaviorSection, MyBehavior,
            Version=1.0.0.0, Culture=neutral, PublicKeyToken=null"" />
        </behaviorExtensions>
    </extensions>
</system.serviceModel>
```

## <a id="wcf-authz"></a>Řízení autorizace implementace WCF

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | WCF | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné, NET Framework 3 |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [obohacení království](https://vulncat.fortify.com/en/vulncat/index.html) |
| **Kroky** | <p>Tato služba nevyužívá ovládací prvek autorizace. Když klient zavolá konkrétní službu WCF, poskytuje WCF různých režimů autorizace, které ověřte, zda že má tento hello volající metody služby hello tooexecute oprávnění na serveru hello. Pokud autorizace ovládací prvky nejsou povolené pro služby WCF, ověřeného uživatele můžete dosáhnout zvýšení úrovně oprávnění.</p>|

### <a name="example"></a>Příklad
Následující konfigurace Hello dá pokyn oprávnění hello WCF toonot kontrolu na úrovni klienta hello při provádění hello služby:
```
<behaviors>
    <serviceBehaviors>
        <behavior>
            ...
            <serviceAuthorization principalPermissionMode=""None"" />
        </behavior>
    </serviceBehaviors>
</behaviors>
```
Použití tooverify schéma autorizace služby, který hello volající metody služby hello je autorizovaný toodo tak. WCF poskytuje dva režimy a umožňuje hello definice schématu vlastní autorizace. režim UseWindowsGroups Hello používá uživatelé a role Windows a režimu UseAspNetRoles hello používá poskytovatele rolí prostředí ASP.NET, jako je SQL Server, tooauthenticate.

### <a name="example"></a>Příklad
Hello následující konfiguraci dá pokyn WCF toomake se, že tento klient hello je součástí skupiny Administrators hello před spuštěním služby přidat hello:
```
<behaviors>
    <serviceBehaviors>
        <behavior>
            ...
            <serviceAuthorization principalPermissionMode=""UseWindowsGroups"" />
        </behavior>
    </serviceBehaviors>
</behaviors>
```
Služba Hello je pak deklarován jako hello následující:
```
[PrincipalPermission(SecurityAction.Demand,
Role = ""Builtin\\Administrators"")]
public double Add(double n1, double n2)
{
double result = n1 + n2;
return result;
}
```

## <a id="authz-aspnet"></a>Implementace mechanismus správné autorizace v rozhraní ASP.NET Web API

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Web API | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné, MVC5 |
| **Atributy**              | Není k dispozici, zprostředkovatel Identity zprostředkovatel – AD FS, Identity – Azure AD |
| **Odkazy**              | [Ověřování a autorizace v rozhraní ASP.NET Web API](http://www.asp.net/web-api/overview/security/authentication-and-authorization-in-aspnet-web-api) |
| **Kroky** | <p>Informace o rolích pro uživatele aplikace hello může být odvozen z Azure AD nebo služby AD FS deklarace identity, pokud aplikace hello je využívá jako zprostředkovatel Identity nebo vlastní aplikace hello uvedli. V některém z těchto případech by měl ověřit provedení vlastní autorizace hello hello informací o roli uživatele.</p><p>Informace o rolích pro uživatele aplikace hello může být odvozen z Azure AD nebo služby AD FS deklarace identity, pokud aplikace hello je využívá jako zprostředkovatel Identity nebo vlastní aplikace hello uvedli. V některém z těchto případech by měl ověřit provedení vlastní autorizace hello hello informací o roli uživatele.</p>

### <a name="example"></a>Příklad
```C#
[AttributeUsage(AttributeTargets.Class | AttributeTargets.Method, Inherited = true, AllowMultiple = true)]
public class ApiAuthorizeAttribute : System.Web.Http.AuthorizeAttribute
{
        public async override Task OnAuthorizationAsync(HttpActionContext actionContext, CancellationToken cancellationToken)
        {
            if (actionContext == null)
            {
                throw new Exception();
            }

            if (!string.IsNullOrEmpty(base.Roles))
            {
                bool isAuthorized = ValidateRoles(actionContext);
                if (!isAuthorized)
                {
                    HandleUnauthorizedRequest(actionContext);
                }
            }

            base.OnAuthorization(actionContext);
        }

public bool ValidateRoles(actionContext)
{
   //Authorization logic here; returns true or false
}

}
```
Všechny hello řadiče a metody akce, které potřebuje tooprotected by měl být doplněny pomocí výše atribut.
```C#
[ApiAuthorize]
public class CustomController : ApiController
{
     //Application code goes here
}
```

## <a id="device-permission"></a>Provedení kontroly autorizace v zařízení hello, pokud ji podporuje různé akce, které vyžadují různé úrovně oprávnění

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Zařízení IoT | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | <p>Hello zařízení by měl povolit toocheck volající hello, pokud má volající hello hello požadované oprávnění tooperform hello akce požadované. Umožňuje například vyslovte hello zařízení je Smart Lock dveří, které můžete monitorovat z cloudu hello navíc poskytuje funkce, jako je vzdálené uzamčení dveře hello.</p><p>Hello Smart Lock dveře poskytuje odemykání funkce jenom v případě, že někdo fyzicky součástí téměř hello dvířka na kartě. V takovém případě hello implementace hello vzdálené příkazy a ovládání by mělo být provedeno tak, že neposkytuje všechny funkce toounlock hello dveře jako hello Cloudová brána není autorizovaný toosend dveří příkaz toounlock hello.</p>|

## <a id="field-permission"></a>Provedení kontroly autorizace v hello brána pole, pokud ji podporuje různé akce, které vyžadují různé úrovně oprávnění

| Název                   | Podrobnosti      |
| ----------------------- | ------------ |
| **Komponenta**               | Brána pole IoT | 
| **SDL fáze**               | Sestavení |  
| **Použít technologie** | Obecné |
| **Atributy**              | Není k dispozici  |
| **Odkazy**              | Není k dispozici  |
| **Kroky** | Hello brána pole by měl povolit toocheck volající hello, pokud má volající hello hello požadované oprávnění tooperform hello akce požadované. Například by měla být ke různých oprávnění pro uživatele správce používá rozhraní nebo rozhraní API tooconfigure pole v nebo s zařízení brány, které se připojují tooit.|
