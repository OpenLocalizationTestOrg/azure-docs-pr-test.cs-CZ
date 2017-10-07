---
title: "Vzor aplikací webové aaaMulti klienta | Microsoft Docs"
description: "Vyhledá architektury přehledy a vzory návrhu, které popisují, jak tooimplement více klientů webovou aplikaci na platformě Azure."
services: 
documentationcenter: .net
author: wadepickett
manager: wpickett
editor: 
ms.assetid: 4f0281d2-1555-42b0-a99d-1222fade0b0f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 06/05/2015
ms.author: wpickett
ms.openlocfilehash: 3b7822c8ca4aa50d295a174973ae4746c230ba66
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="multitenant-applications-in-azure"></a>Víceklientské aplikace v Azure
Víceklientské aplikace je sdílený prostředek, který umožňuje samostatné uživatele, nebo "klientů," aplikace hello tooview, jako by byl svoje vlastní. Typický scénář, který poskytuje vlastní tooa víceklientské aplikace je jedním ve které všichni uživatelé hello aplikace chcete toocustomize hello uživatelské prostředí ale jinak mít hello stejné základní obchodní požadavky. Příkladem velké víceklientské aplikace jsou Office 365, Outlook.com a visualstudio.com.

Z hlediska poskytovatele aplikace hello výhod víceklientskou architekturu většinou se týkají toooperational a efektivnost nákladů. Jednu verzi vaší aplikace můžete podle potřeb hello mnoho klientů nebo zákazníků, povolení úlohy správy, jako jsou monitorování, optimalizace výkonu, údržba softwaru a zálohování dat konsolidace systému.

Hello následující text uvádí seznam hello nejdůležitější cíle a požadavky z hlediska zprostředkovatele.

* **Zřizování**: musí být schopný tooprovision nové klienty pro aplikaci hello.  Pro víceklientské aplikace s velký počet klientů, je obvykle nutné tooautomate, tento proces, pomocí umožňuje samoobslužné zřizování.
* **Udržovatelnost**: musí být schopný tooupgrade hello aplikace a provádět další úlohy údržby v okamžiku, kdy několik klientů používají.
* **Monitorování**: vám musí být schopný toomonitor hello aplikace na všechny časy tooidentify všechny problémy a tootroubleshoot je. To zahrnuje monitorování, jak je každého klienta pomocí aplikace hello.

Správně implementovaná víceklientské aplikace poskytuje následující hello výhody toousers.

* **Izolace**: hello aktivity jednotlivých klientů, které neovlivňují hello použití aplikace hello ostatních klientů. Klienty nelze přistupovat k datům uživatele toho druhého. Zobrazí se toohello klienta jako v případě, že má výhradní použití aplikace hello.
* **Dostupnost**: jednotlivých klientů má toobe aplikace hello neustále k dispozici, případně s záruky, které jsou definované v SLA. Znovu hello aktivity ostatních klientů by neměly ovlivnit dostupnost hello aplikace hello.
* **Škálovatelnost**: aplikace hello škáluje toomeet hello vyžádání jednotlivých klientů. Hello přítomnosti a akce ostatních klientů by neměla vliv hello výkon aplikace hello.
* **Náklady na**: náklady jsou nižší než spuštěna aplikace typu vyhrazené, jednoho klienta, protože víceklientská architektura umožňuje hello sdílení prostředků.
* **Možnosti přizpůsobení**. Hello možnost toocustomize hello aplikace pro jednoho klienta různými způsoby, například přidání nebo odebrání funkcí, změna barvy a loga nebo i přidání vlastní kód nebo skriptu.

Stručně řečeno, zatímco několik rozhodnutí, které je nutné provést do účtu tooprovide vysoce škálovatelná služba, existují také počet hello cíle a požadavky, které jsou společné toomany víceklientské aplikace. Nemusí být některá relevantní v konkrétních scénářů a význam hello jednotlivé cíle a požadavky se budou lišit v každém scénáři. Jako poskytovatel víceklientské aplikace hello budete také mít cíle a požadavky, jako splňuje hello klientům cíle a požadavky, ziskovosti, fakturace, více úrovní služeb, zřizování, udržovatelnosti monitorování a automatizace.

Další informace o další aspekty návrhu víceklientské aplikace najdete v tématu [hostování víceklientské aplikace na platformě Azure][Hosting a Multi-Tenant Application on Azure]. Informace o běžných vzorech architektury dat databázových aplikací softwaru s více tenanty jako služby (SaaS) naleznete v části [Vzory návrhu pro aplikace SaaS s více tenanty s databází Azure SQL Database](sql-database/sql-database-design-patterns-multi-tenancy-saas-applications.md). 

Azure poskytuje řadu funkcí, které umožňují tooaddress hello klíče problémy vzniklé při návrhu víceklientské systému.

**Izolace**

* Segment webu klienty podle hlavičky hostitele s nebo bez komunikaci prostřednictvím protokolu SSL
* Segment webu klienty podle parametrů dotazu
* Webové služby v rolí pracovního procesu
  * Role pracovního procesu. která se obvykle zpracovávají data v hello back-end aplikace.
  * Webové role, které obvykle fungují jako front-endu hello pro aplikace.

**Úložiště**

Správa dat, například služby Azure SQL Database nebo úložiště Azure, jako je hello tabulce službu, která poskytuje služby pro ukládání velkého objemu nestrukturovaných dat a hello služby objektů Blob, který poskytuje služby toostore velké objemy nestrukturovaných textu nebo binární data, jako je například video, zvuk a obrázků.

* Zabezpečení víceklientské Data z databáze SQL odpovídající přihlášení serveru SQL Server za klienta.
* Pomocí tabulky Azure pro aplikace prostředky tak, že zadáte zásadu úrovně přístupu kontejneru, můžete hello možnost tooadjust oprávnění bez nutnosti tooissue nové adresy URL pro hello prostředky chráněné s podpisy sdíleného přístupu.
* Front Azure pro fronty Azure prostředky aplikace jsou běžně používané toodrive zpracování jménem klienty, ale může být také použít toodistribute pracovní nutné pro zřizování nebo správu.
* Fronty služby Service Bus pro prostředky aplikace této tooa pracovní nabízených oznámení sdílené služby, můžete použít jediné fronty kde každý odesílatele klienta má pouze oprávnění (jako je odvozen od deklarace identity vystavené ze serveru ACS) toopush toothat fronty, při pouze příjemci hello ze služby hello máte oprávnění toopull z hello fronty hello dat pocházejících od víc klientů.

**Připojení a zabezpečení služeb**

* Azure Service Bus, která se nachází mezi aplikacemi, což jim tooexchange zprávy volně párované způsobem pro lepší škálování a odolnosti infrastrukturu zasílání zpráv.

**Síťové služby**

Azure poskytuje několik síťové služby, které podporují ověřování a zlepšit možnosti správy hostovaných aplikací. Tyto služby patří hello následující:

* Azure umožňuje virtuální sítě můžete zřizovat a spravovat virtuální privátní sítě (VPN) v Azure stejně jako bezpečně propojit je s místní infrastrukturu IT.
* Správce provozu virtuálních sítí můžete vyrovnávat tooload příchozí provoz napříč více Azure hostované služby, zda používáte systém v hello stejném datovém centru nebo v různých datových centrech kolem hello, world.
* Azure Active Directory (Azure AD) je moderní, založené na REST služba, která poskytuje funkce řízení správy a přístupu identit pro cloudové aplikace. Pomocí služby Azure AD pro prostředky aplikace Azure AD tooprovides snadný způsob ověřování a autorizace uživatelů toogain přístup tooyour webové aplikace a služby současně hello funkce ověřování a autorizace toobe promítnou mimo vaší kód.
* Azure Service Bus poskytuje zabezpečené zasílání zpráv a funkce toku dat pro distribuované a hybridní aplikace, jako je komunikace mezi Azure hostované aplikace a místní aplikace a služby, bez nutnosti komplexní brány firewall a zabezpečení infrastruktury. Použití předávání přes Service Bus pro toohello prostředky aplikace služby, které jsou zveřejněné jako koncové body mohou patřit toohello klienta (například hostované mimo hello systému, například místní), nebo mohou být službám zřizovaným speciálně pro klienta (hello protože konkrétního klienta, citlivá data přenášena mezi nimi).

**Zřizování prostředků**

Azure poskytuje několik způsobů zřídit nové klienty pro aplikace hello. Pro víceklientské aplikace s velký počet klientů, je obvykle nutné tooautomate, tento proces, pomocí umožňuje samoobslužné zřizování.

* Role pracovního procesu umožňují tooprovision a deaktivace zřízení za prostředky klienta (například když nový klient přihlásí up nebo zruší), shromažďování metrik pro měření používat a spravovat škálování následující určité plánu nebo v odpovědi toohello překročení prahové hodnoty klíče ukazatele výkonu. Tento stejný atribut role mohou být také použít toopush se aktualizace a upgrady toohello řešení.
* Azure BLOB může být výpočetní použité tooprovision nebo prostředky předem inicializovaného úložiště pro nové klienty při současném poskytování kontejner přístupu na úrovni zásady tooprotect hello výpočetní služby balíčky, bitové kopie virtuálního pevného disku a dalším prostředkům.
* Možnosti pro zřizování prostředků databáze SQL pro klienta:
  
  * DDL ve skriptech nebo vložené jako prostředky v rámci sestavení
  * SQL Server 2008 R2 balíčky DAC nasazené prostřednictvím kódu programu.
  * Kopírování z hlavní referenční databáze
  * Používání databáze Import a Export tooprovision nové databáze ze souboru.

<!--links-->

[Hosting a Multi-Tenant Application on Azure]: http://msdn.microsoft.com/library/hh534480.aspx
[Designing Multitenant Applications on Azure]: http://msdn.microsoft.com/library/windowsazure/hh689716
