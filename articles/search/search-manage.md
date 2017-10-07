---
title: "aaaService správy pro službu Azure Search v hello portálu Azure"
description: "Spravujte Azure Search, hostované cloudové vyhledávací službě v Microsoft Azure, pomocí hello portálu Azure."
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
tags: azure-portal
ms.assetid: c87d1fdd-b3b8-4702-a753-6d7e29dbe0a2
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 06/18/2017
ms.author: heidist
ms.openlocfilehash: 9bb33660d93e068e0f35b856cba0a41c92623644
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="service-administration-for-azure-search-in-hello-azure-portal"></a>Služba správy pro službu Azure Search v hello portálu Azure
> [!div class="op_single_selector"]
> * [Azure Portal](search-manage.md)
> * [PowerShell](search-manage-powershell.md)
> * [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.search)
> * [Python](https://pypi.python.org/pypi/azure-mgmt-search/0.1.0)> 

Služba Azure Search je plně spravovaná, cloudové vyhledávací služba používá pro vytváření bohatě hledání do vlastních aplikací. Tento článek se zabývá hello *služby správy* úlohy, které můžete provádět v hello [portál Azure](https://portal.azure.com) pro vyhledávací službu, která jste již zřízeno. *Služba správy* je lightweight standardně omezené toohello následující úlohy:

* Spravovat a zabezpečit přístup toohello *klíče api Key* použitý pro čtení nebo zápis tooyour service.
* Změnou hello přidělení replik a oddíly upravte kapacitu služby.
* Sledování využití prostředků, omezení relativní toomaximum vaše vrstvy služby.

**Není v oboru** 

*Správa obsahu* (nebo index management) odkazuje toooperations například analýza vyhledávání provoz toounderstand dotazu svazku, zjistit, které podmínky osoby hledání a jak úspěšné vyhledávání výsledky jsou v vedení toospecific zákazníků dokumenty v indexu. Nápovědu v této oblasti najdete v tématu [Analýza provozu vyhledávání pro službu Azure Search](search-traffic-analytics.md).

*Dotazování výkonu* je také nad rámec tohoto článku hello. Další informace najdete v tématu [monitorování metriky využití a dotaz](search-monitor-usage.md) a [výkonu a optimalizace](search-performance-optimization.md).

*Upgrade* není správce úloh. Protože prostředky se přidělují, když je služba hello zřízena, přesunutí tooa jiné vrstvě vyžaduje novou službu. Podrobnosti najdete v tématu [vytvoření služby Azure Search](search-create-service-portal.md).

<a id="admin-rights"></a>

## <a name="administrator-rights"></a>Práva správce
Zřizování nebo vyřazení z provozu samotnou službu hello lze provést pomocí Správce předplatného Azure nebo spolusprávce.

V rámci služby má každý, kdo má přístup adresa URL služby toohello a rozhraní api-key Správce služby toohello přístup pro čtení a zápis. Poskytuje přístup pro čtení a zápis hello možnost tooadd, odstranění nebo změnu objektů serveru, včetně klíče api Key, indexy, indexery, zdrojů dat, plány a přiřazení rolí, jak jsou implementované pomocí [role RBAC definované](#rbac).

Všechny interakce uživatele s Azure Search spadá do jedné z těchto režimů: pro čtení a zápis služba toohello oprávnění jen pro čtení (dotazu oprávnění) nebo služba toohello přístup (oprávnění správce). Další informace najdete v tématu [spravovat hello klíče api Key](#manage-keys).

<a id="sys-info"></a>

## <a name="set-rbac-roles-for-administrative-access"></a>Nastavte role RBAC pro přístup pro správu
Azure poskytuje [modelu globální autorizace na základě rolí](../active-directory/role-based-access-control-configure.md) pro všechny služby, které jsou spravované prostřednictvím portálu hello nebo rozhraní API Resource Manager. Role vlastník, Přispěvatel a čtečky určete hello úroveň služby správy pro uživatele služby Active Directory, skupiny a role přiřazené tooeach objekty zabezpečení. 

Pro službu Azure Search určit oprávnění RBAC hello následující úlohy správy:

| Role | Úkol |
| --- | --- |
| Vlastník |Vytvořit nebo odstranit hello služby nebo libovolného objektu na hello služby, včetně klíče api Key, indexy, indexery, indexer zdroje dat a indexeru plány.<p>Zobrazit stav služby, včetně počtu a velikosti úložiště.<p>Přidání nebo odstranění členství role (pouze vlastníka můžete spravovat členství role).<p>Správci předplatného a vlastníky služby mají automatické členství v roli vlastníky hello. |
| Přispěvatel |Stejnou úroveň přístupu jako vlastník, minus RBAC správu rolí. Například Přispěvatel můžete zobrazit a znovu vygenerovat `api-key`, ale nelze upravit členství v rolích. |
| Čtenář |Zobrazení stavu a dotaz klíče služby. Členové této role nelze změnit konfiguraci služby, ani můžete zobrazit klíče správce. |

Role neudělujte oprávnění přístupu, toohello koncový bod služby. Hledání operací služby, jako je například Správa indexu, naplňování indexu a dotazy na data vyhledávání, jsou řízena pomocí klíče api Key, není role. Další informace najdete v tématu "Autorizace pro správu a operace dat" v [co je řízení přístupu na základě Role](../active-directory/role-based-access-control-what-is.md).

<a id="secure-keys"></a>
## <a name="logging-and-system-information"></a>Informace o protokolování a systému
Služba Azure Search nevystavuje soubory protokolu pro jednotlivé služby buď prostřednictvím portálu hello nebo programovací rozhraní. Na úroveň Basic hello a vyšší Microsoft monitoruje všechny služby Azure Search 99,9 % dostupnosti za smlouvy o úrovni služeb (SLA). Pokud služba hello je pomalé nebo propustnost žádostí nedosahuje prahové hodnoty SLA, projděte si hello protokolu soubory k dispozici toothem a adresa hello problém týmy podpory.

Z hlediska obecné informace o službě můžete získat informace v hello následující způsoby:

* Hello portálu na řídicím panelu služby hello, prostřednictvím oznámení, vlastnosti a stavové zprávy.
* Pomocí [prostředí PowerShell](search-manage-powershell.md) nebo hello [REST API pro správu](https://docs.microsoft.com/rest/api/searchmanagement/) příliš[získat vlastnosti služby](https://docs.microsoft.com/rest/api/searchmanagement/services), nebo stav na využití prostředků indexu.
* Prostřednictvím [Analýza provozu vyhledávání](search-traffic-analytics.md), jak je uvedeno dříve.

<a id="manage-keys"></a>

## <a name="manage-api-keys"></a>Spravovat klíče api Key
Všechny služby vyhledávání tooa požadavky nutné rozhraní api klíč, který byl vygenerován speciálně pro vaši službu. Tento klíč rozhraní api je hello jedinou mechanismus pro ověřování koncového bodu služby přístup tooyour vyhledávání. 

Klíč rozhraní api je řetězec tvořený náhodně generované číslic a písmen. Prostřednictvím [oprávnění RBAC](#rbac), můžete odstranit nebo čtení hello klíčů, ale klíč nelze nahradit heslem definovaný uživatelem. 

Dva typy klíčů jsou použité tooaccess vaši službu vyhledávání:

* Správce (platné pro všechny operace čtení a zápis službě hello)
* Dotaz (platné pro operace jen pro čtení, například dotazy pro index)

Rozhraní api-key správce je vytvořen po zřízení služby hello. Existují dva klíče správce, určený jako *primární* a *sekundární* tookeep je přímo, ale ve skutečnosti se zaměňovat. Každá služba má dva klíče správce, takže můžete se vrátit jednu bez ztráty přístupu tooyour služby. Můžete obnovit buď klíč správce, ale nemůžete přidat počet klíčů toohello celkový správce. Je delší než dva klíče správce jednu službu vyhledávání.

Klíče dotazu jsou navrženy pro klientské aplikace, které volají přímo vyhledávání. Můžete vytvořit až too50 klíče dotazu. V kódu aplikace zadejte adresu URL hello vyhledávání a toohello služby dotazu klíč api-key tooallow oprávnění jen pro čtení. Kód aplikace také určuje index hello používá vaše aplikace. Koncový bod společně hello, klíč rozhraní api pro přístup jen pro čtení a cílový index definovat hello oboru a úroveň přístupu hello připojení z klientské aplikace.

tooget nebo znovu vygenerovat klíče api Key, řídicí panel služby otevřete hello. Klikněte na tlačítko **klíče** tooslide otevřete stránku hello správy klíčů. Příkazy pro opětovné generování nebo vytváření klíčů jsou hello horní části stránky hello. Ve výchozím nastavení se vytvoří pouze klíče správce. Klíče dotazu api Key musí být vytvořeny ručně.

 ![][9]

<a id="rbac"></a>

## <a name="secure-api-keys"></a>Zabezpečovací klíče rozhraní api
Zabezpečení klíčů je zajištěn tak, že omezíte přístup přes portál hello nebo rozhraní správce prostředků (prostředí PowerShell nebo rozhraní příkazového řádku). Jak jsme uvedli, správci předplatného můžete zobrazit a obnovit všechny klíče rozhraní api. Z důvodu zkontrolujte toounderstand přiřazení role, který má přístup toohello Správce klíčů.

1. Na řídicím panelu služby hello klikněte na tlačítko hello přístup ikonu oknem uživatelé tooslide otevřete hello.
   ![][7]
2. V seznamu uživatelé Zkontrolujte stávající přiřazení rolí. Podle očekávání, správci předplatného už máte plný přístup toohello service pomocí roli vlastníka hello.
3. Klikněte na tlačítko Další, toodrill **správci předplatného** a potom rozbalte hello role přiřazení seznamu toosee, který má práva správce společné na vaši službu vyhledávání.

Jiný způsob tooview přístupová oprávnění je tooclick **role** v okně uživatelé hello. Díky tomu zobrazí dostupných rolí a hello číslo přiřazené tooeach role uživatele nebo skupiny.

<a id="sub-5"></a>

## <a name="monitor-resource-usage"></a>Sledování využití prostředků
V hello řídicí panel sledování prostředků je omezené toohello informace zobrazené na řídicí panel služby hello a několik metriky, které můžete získat pomocí dotazu služby hello. Na řídicím panelu služby hello, v části hello využití můžete rychle určit, jestli jsou oddílu prostředků úrovně pro vaše aplikace.

Pomocí hello rozhraní API služby vyhledávání, můžete získat počet dokumentů a indexy. Existují pevných limitů přidružené tyto počty podle hello cenová úroveň. Další informace najdete v tématu [omezení služby Search](search-limits-quotas-capacity.md). 

* [Získat statistiku indexu](https://docs.microsoft.com/rest/api/searchservice/Get-Index-Statistics)
* [Počet dokumentů](https://docs.microsoft.com/rest/api/searchservice/count-documents)

> [!NOTE]
> Ukládání do mezipaměti chování můžete dočasně overstate omezení. Například při použití hello sdílené služby, můžete se setkat dokumentu počet přes hello pevný limit 10 000 dokumentů. přehánění Hello je dočasný a bude zjištěn na hello další kontroly vynucení omezení. 
> 
> 

## <a name="disaster-recovery-and-service-outages"></a>Po havárii výpadkům obnovení a služby

I když jsme můžete vyřazení dat, Azure Search neposkytuje rychlých převzetí služeb při selhání služby hello Pokud dojde k výpadku na úrovni hello clusteru nebo datového centra. Pokud cluster selže v hello datového centra, hello provozní tým rozpozná a pracovní toorestore služby. Během obnovení služby bude docházet výpadku. Můžete požádat služby kredity toocompensate spojeným s nedostupností služby za hello [smlouvy o úrovni služeb (SLA)](https://azure.microsoft.com/support/legal/sla/search/v1_0/). 

Pokud byly služby nepřetržitě je vyžadována v případě hello závažné selhání mimo kontrolu společnosti Microsoft, může [zřídit další službu](search-create-service-portal.md) v jiné oblasti a indexy tooensure strategie implementace geografickou replikaci jsou plně redundantní v rámci všech služeb.

Zákazníci, kteří používají [indexery](search-indexer-overview.md) toopopulate a aktualizace indexů dokáže zpracovat zotavení po havárii prostřednictvím specifické pro geograficky indexery využití hello stejném datovém zdroji. Dvě služby v různých oblastech, každé spuštění indexeru, mohou indexu z hello stejný zdroj dat tooachieve geografická redundance. Pokud indexování ze zdrojů dat, které jsou geograficky redundantní, mějte na paměti, že Azure Search indexery může provádět pouze v přírůstkové indexování z primární repliky. V případě převzetí služeb při selhání být jisti toore bodu hello indexer toohello nové primární replice. 

Pokud nepoužijete indexery, použijete by vaše aplikace kód toopush objektů a dat toodifferent služby vyhledávání paralelně. Další informace najdete v tématu [výkonu a optimalizace ve službě Azure Search](search-performance-optimization.md).

## <a name="backup-and-restore"></a>Zálohování a obnovení

Protože Azure Search není řešení primární datové úložiště, není poskytována formální mechanismus pro samoobslužné služby zálohování a obnovení. Kód aplikace používá k vytváření a naplňování indexu, je fakticky možnost obnovení hello, pokud omylem odstraníte indexu. 

toorebuild na index, byste odstranit (za předpokladu, že existuje), znovu vytvořte index hello ve službě hello a znovu načtěte načtením dat z primární data store. Alternativně můžete uživatele oslovit příliš[zákaznickou podporu]() toosalvage indexy, pokud je regionální výpadku.


<a id="scale"></a>

## <a name="scale-up-or-down"></a>Vertikálně navýšit nebo snížit kapacitu
Všechny služby vyhledávání začíná minimálně jednu repliku a jeden oddíl. Pokud jste se přihlásili k [vrstvy, která poskytuje vyhrazených prostředcích](search-limits-quotas-capacity.md), klikněte na tlačítko hello **ŠKÁLOVÁNÍ** dlaždici ve využití prostředků tooadjust řídicí panel služby hello.

Když přidáte kapacity prostřednictvím buď prostředků, hello používá služba je automaticky. Je potřeba žádná další akce z vaší strany, ale je k mírnému zpoždění před je realizován hello dopad hello nový prostředek. 15 minut nebo další tooprovision může trvat další prostředky.

 ![][10]

### <a name="add-replicas"></a>Přidat repliky
Zvýšení dotazů za sekundu (QPS) nebo dosažení vysoké dostupnosti se provádí přidáním repliky. Každou repliku má jednu kopii indexu, takže přidání jeden další repliky překládá tooone další index k dispozici pro zpracování žádosti o služby dotazu. Minimálně 3 repliky jsou požadovány pro zajištění vysoké dostupnosti (viz [plánování kapacity](search-capacity-planning.md) podrobnosti).

Vyhledávací službu s další repliky můžete načíst vyrovnávat požadavky na dotaz přes větší počet indexů. Danou úrovní dotazu svazku, propustnost dotazu přechází toobe rychleji, když existují další kopie hello index k dispozici tooservice hello požadavku. Pokud dochází k latence dotazu, můžete očekávat kladný dopad na výkon, jakmile hello další repliky jsou online.

I když propustnost dotazu se zvyšuje při přidávání repliky, nemá přesněji dvakrát nebo Trojitá při přidávání služby tooyour repliky. Všechny aplikace vyhledávání jsou subjektu tooexternal faktorů, které můžete dotýkat výkonu dotazů. Složitých dotazů a latence sítě jsou dva faktory, které přispívají toovariations v dobu odezvy na dotazy.

### <a name="add-partitions"></a>Přidat oddíly
Většina aplikací service má integrovanou potřebu další repliky spíše než oddíly. Pro případy, kdy je potřeba počet vyšší dokumentu můžete přidat oddíly, pokud jste zaregistrovali do služby na úrovni Standard. Základní úroveň nejsou k dispozici pro další oddíly.

Přidá úroveň Standard hello oddíly v násobcích 12 (konkrétně 1, 2, 3, 4, 6 a 12). Toto je artefakt horizontálního dělení. Index se vytvoří ve 12 horizontálních oddílů, které může všechny uložené na 1 oddílu nebo rovnoměrně rozdělené do 2, 3, 4, 6 a 12 oddíly (jeden horizontálního oddílu na oddíl).

### <a name="remove-replicas"></a>Odstranit repliky
Po období vysoké dotazu svazků můžete snížit replik po zatížením dotazy vyhledávání mít normalized (například po svátek prodeje jsou přes).

toodo se přesunout hello repliky posuvníku back tooa nižší číslo. Neexistují žádné další kroky potřebné z vaší strany. Virtuální počítače v datovém centru hello snížení počtu repliky hello ztratí. Vaše operace přijímání dotazu a data se teď spustí na méně virtuálních počítačích než před. minimální omezení Hello je jedna replika.

### <a name="remove-partitions"></a>Odebrat oddíly
Rozdíl od odebírání repliky, který vyžaduje žádné další úsilí na druhé straně, může mít některé pracovní toodo, pokud používáte další úložiště, než se může snížit. Například pokud řešení používá tři oddíly, downsizing tooone nebo dva oddíly vygenerují chybu pokud hello nového prostoru úložiště je menší než požadovaný. Podle očekávání, vaše volby se toodelete indexy nebo dokumenty v rámci přidružený index toofree místo nebo zachovat aktuální konfiguraci hello.

Neexistuje žádná metoda zjišťování, která vám ukáže, které horizontálních oddílů indexu jsou uložené na konkrétní oddíly. Každý oddíl obsahuje přibližně 25 GB úložiště, takže bude potřeba tooreduce tooa velikost úložiště, který může být obslouženo hello počet oddílů, které máte. Pokud chcete, aby toorevert tooone oddíl, všechny 12 horizontálních oddílů potřebovat toofit.

toohelp s budoucí plánování, můžete chtít toocheck úložiště (pomocí [získat statistiku Index](https://docs.microsoft.com/rest/api/searchservice/Get-Index-Statistics)) toosee kolik ve skutečnosti použít. 

<a id="advanced-deployment"></a>

## <a name="best-practices-on-scale-and-deployment"></a>Osvědčené postupy na škálování a nasazení
Toto video 30 minut zkontroluje osvědčené postupy pro pokročilé scénáře nasazení, včetně úloh zeměpisné polohy. Můžete také zjistit [výkonu a optimalizace ve službě Azure Search](search-performance-optimization.md) pro tento hello titulní stránky nápovědy stejné body.

> [!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON319/player]
> 
> 

<a id="next-steps"></a>

## <a name="next-steps"></a>Další kroky
Jakmile porozumíte hello koncepty za správu služby, zvažte použití [prostředí PowerShell](search-manage-powershell.md) tooautomate úlohy.

Také doporučujeme přečtení hello [výkonu a optimalizace článku](search-performance-optimization.md).

Další doporučení je toowatch hello video si poznamenali v předchozím oddílu hello. Poskytuje hlubší pokrytí hello postupů uvedených v této části.

<!--Image references-->
[7]: ./media/search-manage/rbac-icon.png
[8]: ./media/search-manage/Azure-Search-Manage-1-URL.png
[9]: ./media/search-manage/Azure-Search-Manage-2-Keys.png
[10]: ./media/search-manage/Azure-Search-Manage-3-ScaleUp.png



