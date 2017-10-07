---
title: "Architektura služby Azure Active Directory aaaUnderstand | Microsoft Docs"
description: "Vysvětluje, co je klient Azure AD a jak toomanage Azure přes Azure Active Directory"
services: active-directory
documentationcenter: 
author: MarkusVi
writer: v-lorisc
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/02/2017
ms.author: markvi
ms.openlocfilehash: 799943c012dcc309907ed3c36372038a0aad222a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="understand-azure-active-directory-architecture"></a>Vysvětlení architektury Azure Active Directory
Azure Active Directory (Azure AD) umožňuje toosecurely můžete spravovat přístup tooAzure služby a prostředky pro vaše uživatele. Součástí Azure AD je kompletní sada funkcí pro správu identit. Informace o funkcích služby Azure AD najdete v tématu [Co je Azure Active Directory?](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-whatis)

S Azure AD můžete vytvořit a spravovat uživatele a skupiny a tooallow oprávnění povolit a odepřít přístup tooenterprise prostředky. Informace o správě identity najdete v tématu [hello základy správy identit Azure](https://docs.microsoft.com/en-us/azure/active-directory/fundamentals-identity).

## <a name="azure-ad-architecture"></a>Architektura Azure AD
Geograficky distribuovaná architektura služby Azure AD kombinuje rozsáhlé monitorování, automatické přesměrování spojnic, převzetí služeb při selhání a možnosti obnovení nám umožňují toodeliver podnikové úrovni dostupnosti a výkonu tooour zákazníkům.

Hello následující prvky architektury jsou popsaná v tomto článku:
 *  Návrh architektury služeb
 *  Škálovatelnost 
 *  Nepřetržitá dostupnost
 *  Datová centra

### <a name="service-architecture-design"></a>Návrh architektury služeb
jednotky škálování Hello bylo škálovatelné, vysoce dostupné a bohaté dat systému je prostřednictvím nezávislé stavební bloky nebo jednotek škálování pro hello Azure AD datové vrstvy nejběžnější toobuild způsobem, se nazývají *oddíly*. 

Hello Datová vrstva obsahuje několik front-endové služby, které poskytují funkce pro čtení a zápis. Hello obrázek ukazuje, jak jsou komponenty hello oddílu adresáře jedním distribuované v rámci geograficky distrubuted datových centrech. 

  ![Oddíly s jedním adresářem](./media/active-directory-architecture/active-directory-architecture.png)

součásti Hello architektury služby Azure AD zahrnují repliku primární a sekundární repliky.

**Primární replika**

Hello *primární repliky* obdrží všechny *zapíše* pro oddíl hello patří do. Žádné zápisu operace je před vrácením úspěch toohello volajícího, aby byla zajištěna odolnost geograficky redundantní zápisů okamžitě replikované tooa sekundární replika v jiném datovém centru.

**Sekundární repliky**

Všechna *čtení* adresáře se obsluhují ze *sekundárních replik* v datových centrech, které jsou fyzicky umístěné v různých zeměpisných oblastech. Protože data se replikují asynchronně, existuje mnoho sekundárních replik. Čtení adresáře, jako jsou žádosti o ověření, jsou obsluhovány z datových center, které jsou zavřít tooour zákazníků. Hello sekundární repliky jsou zodpovědní za čtení škálovatelnost.

### <a name="scalability"></a>Škálovatelnost

Je škálovatelnost schopnost hello toomeet tooexpand služby zvyšuje požadavky na výkon. Zapsat škálovatelnost se dosahuje segmentace dat hello. Škálovatelnost pro čtení se dosahuje replikace dat z jednoho oddílu toomultiple sekundární repliky distribuované v rámci hello, world.

Požadavky z adresáře aplikace jsou obecně směrované toohello datacentru, které jsou fyzicky nejblíže. Zápisů jsou transparentně přesměrovaného toohello primární repliky tooprovide pro čtení a zápis konzistence. Sekundární repliky výrazně rozšířit hello škálování oddílů, protože hello adresáře jsou obvykle slouží čtení většinu času hello.

Aplikace Directory připojit toohello nejbližší datových centrech. To zvyšuje výkon a proto je možné horizontální navýšení kapacity. Vzhledem k tomu, že oddíl adresáře může mít mnoho sekundárních replikách, mohou být umístěny blíže toohello directory klienti sekundární repliky. Pouze interní adresář služby součásti, které jsou náročné na zápis cíl hello aktivní primární replika přímo.

### <a name="continuous-availability"></a>Nepřetržitá dostupnost

Dostupnost (nebo provozu) definuje hello schopnost tooperform systému bez přerušení. Vysoká dostupnost Hello klíče tooAzure na AD je, že našich služeb můžete rychle posunutí provoz v několika mnoha místech pracujícími datových centrech. Každé datové centrum je nezávislé, což umožňuje, aby režimy selhání spolu vzájemně nesouvisely.

Návrh oddílu Azure AD je zjednodušená porovnání toohello enterprise návrhem služby AD, což je důležité pro škálování nahoru hello systému. Využili jsme návrh s jedinou předlohou, který zahrnuje pečlivě orchestrovaný a deterministický proces převzetí služeb při selhání primární repliky.

**Odolnost proti chybám**

Systém je další dostupné, pokud je odolný vůči chybám selhání toohardware, sítě a softwaru. Pro každý oddíl v adresáři hello existuje hlavní repliku vysokou dostupností: hello primární repliky. Pouze oddíl toohello zápisů jsou prováděny v této replice. Tato replika je nepřetržitě a úzce monitorována a zápisy může být okamžitě posunuté tooanother repliky (který se stane hello nový primární) Pokud je zjištěno selhání. Během převzetí služeb při selhání může dojít ke ztrátě dostupnosti zápisu, obvykle na 1 až 2 minuty. Dostupnost čtení to během této doby neovlivní.

Operace čtení (které outnumber zápisy podle mnoho pořadí podle velikosti), jdou pouze toosecondary repliky. Vzhledem k tomu, že sekundární repliky jsou idempotent, ke ztrátě všech jednu repliku v daném oddílu je možné snadno vyrovnávat přesměrováním hello čtení tooanother repliky, obvykle v hello stejného datového centra.

**Odolnost dat**

Zápis je trvale potvrzena tooat minimálně dva datového centra předchozí tooit potvrzování. K tomu dojde nejprve potvrzení zápisu hello na hello primární a pak okamžitě replikace hello zápisu tooat minimálně jeden další datového centra. To zajistí, že potenciální závažné ztrátě hello data center hostování hello primární není dojít ke ztrátě dat.

Azure AD udržuje nulu [cíl doba obnovení (RTO)](https://en.wikipedia.org/wiki/Recovery_time_objective) pro vystavování tokenů a directory čte a v pořadí hello minut (~ 5 minut) RTO directory zapisuje. Zajišťujeme také nulový [cíl bodu obnovení (RPO)](https://en.wikipedia.org/wiki/Recovery_point_objective) neztratíme data při převzetí služeb při selhání.

### <a name="data-centers"></a>Datová centra

Azure AD repliky jsou uloženy v datových centrech umístěné v rámci hello, world. Další informace najdete v tématu věnovaném [datovým centrům Azure](https://azure.microsoft.com/en-us/overview/datacenters).

Azure AD funguje napříč datovými centry s hello následující vlastnosti:

 * Ověřování, grafu a další služby AD, jsou umístěny za hello služba brány. Hello brány spravuje Vyrovnávání zatížení z těchto služeb. Pokud se pomocí transakčních sond stavu zjistí, že některý server není v pořádku, automaticky provede převzetí služeb při selhání. Podle těchto sondy stavu, hello brány dynamicky směruje provoz toohealthy datových centrech.
 * Pro *čte*, hello directory má sekundární repliky a odpovídající front-endové služby v konfiguraci aktivní aktivní provoz v několika datových centrech. V případě selhání celého datového centra přenosy dat budou probíhat automaticky směrované tooa jiném datovém centru.
 *  Pro *zapíše*, hello adresář bude převzetí služeb při selhání primární (hlavní) repliky napříč datovými centry prostřednictvím plánované (nový primární je synchronizované tooold primární) nebo postupy nouzový převzetí služeb při selhání. Odolnost dat se dosahuje replikace žádné potvrzení tooat minimálně dva datových centrech.

**Konzistence dat**

Hello directory model je jedním z konzistence typu případné. Typické jedním z problémů s distribuovaných systémů asynchronně replikaci je, že hello data vrácená z "konkrétní" repliky nemusí být až toodate. 

Azure AD poskytuje konzistenci pro čtení a zápis pro aplikace cílený na sekundární repliku ve směrování její zápisy toohello primární replika a synchronně stahování hello zapíše zpět toohello sekundární repliky.

Aplikace zapíše pomocí hello Graph API Azure AD jsou abstrahované zachovala spřažení tooa directory repliky konzistenci pro čtení a zápis. Hello služby Azure AD Graph udržuje logické relace, která má spřažení tooa sekundární repliky pro čtení; Spřažení je zachycen v "token repliky", který hello mezipamětí služby grafu pomocí distribuované mezipaměti. Tento token se pak použije pro následující operace v hello stejné logické relace. 

 >[!NOTE]
 >Zápisy se okamžitě replikují byly vystaveny čtení toohello sekundární repliky toowhich hello logické relace.
 >

**Ochrana záloh**

Hello directory implementuje obnovitelného odstranění místo pevný odstranění pro uživatele a klienty pro snadné obnovení v případě náhodného odstranění podle zákazníka. Pokud správce klienta, ať už náhodně odstraní uživatele, mohou snadno vrátit zpět a obnovit hello odstranit uživatele. 

Azure AD implementuje denní zálohy všech dat, a proto může autoritativně obnovit data v případě jakýchkoli logických odstranění nebo poškození. Naše datová vrstva využívá kódy pro opravu chyb. Může tak kontrolovat a automaticky opravovat určité typy diskových chyb.

**Metriky a monitorování**

Spouštění služby s vysokou dostupností vyžaduje špičkové metriky a možnosti monitorování. Azure AD průběžně analyzuje a reportuje metriky stavu klíčových služeb a kritéria úspěchu pro každou ze svých služeb. Neustále vyvíjíme a ladíme metriky, monitorování a generování výstrah pro jednotlivé scénáře, v rámci jednotlivých služeb Azure AD a napříč všemi službami.

Pokud žádné služby Azure AD nefungují podle očekávání, jsme okamžitě trvat akce toorestore funkce co nejrychleji. Hello nejdůležitější metriky Azure AD sleduje je jak rychle jsme můžete zjišťovat a zmírnit zákazníka nebo live potíže webu. Jsme investovat výraznou v monitorování a výstrahy toominimize čas toodetect (cíl TTD: < 5 minut) a provozní připravenosti toominimize čas toomitigate (TTM cíl: < 30 minut).

**Bezpečný provoz**

Zavádíme provozní kontrolní mechanismy, jako je vícefaktorové ověřování (MFA) pro všechny operace a také auditování všech operací. Kromě toho používáme zvýšení oprávnění za běhu systému toogrant nezbytné dočasný přístup pro všechny provozní úlohy na vyžádání průběžně. Další informace najdete v tématu [hello důvěryhodné cloudu](https://azure.microsoft.com/en-us/support/trust-center).

## <a name="next-steps"></a>Další kroky
[Příručka pro vývojáře pro službu Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-developers-guide)

