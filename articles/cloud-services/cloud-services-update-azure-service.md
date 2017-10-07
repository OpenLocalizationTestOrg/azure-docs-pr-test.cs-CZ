---
title: "aaaHow tooupdate cloudové služby | Microsoft Docs"
description: "Zjistěte, jak tooupdate cloudových služeb v Azure. Zjistěte, jak aktualizace v cloudové službě pokračuje tooensure dostupnosti."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: c6a8b5e6-5c99-454c-9911-5c7ae8d1af63
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: adegeo
ms.openlocfilehash: 7e4c8bd46e51a555b4309ea8927d120e8efcf0ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooupdate-a-cloud-service"></a>Jak tooupdate cloudové služby

Aktualizace cloudové služby, včetně jeho role a hostovaný operační systém, je proces tři krok. Nejprve hello binární soubory a konfigurační soubory pro hello novou cloudovou službu nebo verze operačního systému, musí se nahrát. V dalším kroku Azure si vyhrazuje výpočetní a síťové prostředky pro hello cloudové služby na základě požadavků hello hello novou verzi cloud service. Nakonec Azure provede nová verze s postupného upgradu tooincrementally aktualizace hello klienta toohello nebo hostovaný operační systém, při zachování vaší dostupnosti. Tento článek popisuje hello podrobnosti o tomto posledním kroku – hello vrácení upgradu.

## <a name="update-an-azure-service"></a>Aktualizace služby Azure
Azure umožňuje instance role uspořádat do logických skupin názvem domén upgradu (UD). Upgradu domény (UD) jsou logické skupiny instancí role, které jsou aktualizované jako skupina.  Azure aktualizace a cloudové služby jeden UD současně, což umožňuje instancí v jiných UDs toocontinue obsluhující provoz.

Hello výchozí počet domén upgradu je 5. Můžete zadat jiný počet domén upgradu včetně hello upgradeDomainCount atribut v souboru definice hello služby (.csdef). Další informace o atributu upgradeDomainCount hello najdete v tématu [WebRole schématu](https://msdn.microsoft.com/library/azure/gg557553.aspx) nebo [WorkerRole schématu](https://msdn.microsoft.com/library/azure/gg557552.aspx).

Když provedete aktualizaci na místě jednu nebo více rolí ve službě, aktualizuje Azure sady instancí role podle toowhich toohello upgradovací doméně, ke které patří. Azure aktualizací, které se všechny instance hello v dané doméně upgradu –, zastavení, aktualizuje, uvede zpět online – pak se posouvá do další domény hello. Tím zastavíte pouze hello instancí spuštěných v hello aktuální upgradovací doména, je zajištěno, že dojde k aktualizaci s hello nejmenší dopad toohello službou Azure. Další informace najdete v tématu [jak hello aktualizace pokračuje](#howanupgradeproceeds) dále v tomto článku.

> [!NOTE]
> Při hello podmínky **aktualizace** a **upgrade** mají mírně jiný význam v kontextu hello Azure, mohou být použity zcela zaměnitelným významem pro hello procesů a popisy hello funkcí v tomto dokumentu.
>
>

Služby, musíte definovat alespoň dvě instance role pro tuto roli toobe aktualizovat místní bez výpadků. Pokud služba hello obsahuje pouze jednu instanci jedné role, služby bude k dispozici, dokud hello místní aktualizace dokončí.

Toto téma obsahuje následující informace o Azure aktualizace hello:

* [Během aktualizace povoleny změny služby](#AllowedChanges)
* [Jak pokračuje upgradu](#howanupgradeproceeds)
* [Vrácení zpět aktualizace](#RollbackofanUpdate)
* [Spouštění více mutating operací na probíhající nasazení](#multiplemutatingoperations)
* [Distribuce rolí napříč doménami upgradu](#distributiondfroles)

<a name="AllowedChanges"></a>

## <a name="allowed-service-changes-during-an-update"></a>Během aktualizace povoleny změny služby
Hello následující tabulka uvádí, že hello povolené změny tooa služby během aktualizace:

| Změny povoleny toohosting, služeb a rolí | Aktualizace na místě | Dvoufázové instalace (prohození virtuálních IP adres) | Odstranit a znovu nasadit. |
| --- | --- | --- | --- |
| Verze operačního systému |Ano |Ano |Ano |
| Úrovně důvěryhodnosti .NET |Ano |Ano |Ano |
| Velikost virtuálního počítače<sup>1</sup> |Ano<sup>2</sup> |Ano |Ano |
| Nastavení místního úložiště |Pouze zvýšit<sup>2</sup> |Ano |Ano |
| Přidání nebo odebrání rolí ve službě |Ano |Ano |Ano |
| Počet instancí určité role |Ano |Ano |Ano |
| Číslo nebo typ koncových bodů pro služby |Ano<sup>2</sup> |Ne |Ano |
| Názvy a hodnoty nastavení konfigurace |Ano |Ano |Ano |
| Hodnoty (ale ne názvy) nastavení konfigurace |Ano |Ano |Ano |
| Přidat nové certifikáty |Ano |Ano |Ano |
| Změna existující certifikátů |Ano |Ano |Ano |
| Nasazování nového kódu |Ano |Ano |Ano |

<sup>1</sup> Změna velikosti omezená podmnožina toohello velikostí hello cloudové služby k dispozici.

<sup>2</sup> vyžaduje Azure SDK 1.5 nebo novější verze.

> [!WARNING]
> Změna velikosti virtuálního počítače hello zničí místní data.
>
>

během aktualizace nejsou podporovány Hello následující položky:

* Změna hello název role. Odeberte a pak přidejte hello role s novým názvem hello.
* Změnou hello počet domén upgradu.
* Snížení velikosti hello hello místních prostředků.

Pokud provádíte jiné aktualizace definice tooyour služby, jako je například snížení hello velikost místního prostředku, je nutné provést o aktualizaci prohození virtuální IP adresy. Další informace najdete v tématu [Prohodit nasazení](https://msdn.microsoft.com/library/azure/ee460814.aspx).

<a name="howanupgradeproceeds"></a>

## <a name="how-an-upgrade-proceeds"></a>Jak pokračuje upgradu
Můžete rozhodnout, jestli chcete tooupdate všechny hello role ve službě nebo jedné role ve službě hello. V obou případech jsou všechny instance každé role, probíhá upgrade a musí patřit toohello první doména upgradu byla zastavena, upgradovat a vrátí do režimu online. Jakmile jsou zpět do režimu online, hello instancí v druhé doméně upgradu hello jsou zastavena, upgradovat a znovu online. Cloudové služby může mít maximálně jeden upgradu v každém okamžiku aktivní. Hello upgrade je vždy adresovat hello nejnovější verzi hello cloudové služby.

Hello následující diagram ilustruje, jak hello upgradu při upgradu všech hello rolí ve službě hello:

![Upgrade služby](media/cloud-services-update-azure-service/IC345879.png "Upgrade služby")

Tento další diagram znázorňuje, jak bude pokračovat aktualizace hello, pokud provádíte upgrade jedné role:

![Upgrade role](media/cloud-services-update-azure-service/IC345880.png "upgradu role")  

Během automatické aktualizace hello Kontroleru prostředků infrastruktury Azure pravidelně vyhodnocuje stav hello hello cloudové služby toodetermine při bezpečné toowalk hello další UD. Tato vyhodnocení stavu se provádí na základě podle rolí a považuje pouze instance v nejnovější verzi hello (tj. instance z UDs, které již byly šel). Ověřuje, že minimální počet instancí role, pro každou roli, jste dosáhli uspokojivé stavu terminálu.

### <a name="role-instance-start-timeout"></a>Časový limit spuštění Instance role
Hello Kontroleru prostředků infrastruktury počká 30 minut pro každou roli instance tooreach do stavu spuštěno. Pokud dobu trvání časového limitu hello uplyne, hello Kontroleru prostředků infrastruktury bude pokračovat, proti toohello další instance role.

### <a name="impact-toodrive-data-during-cloud-service-upgrades"></a>Dopad toodrive dat při upgradech cloudové služby

Při upgradu služby z instance toomultiple jediné instance, které vaše služba bude se snížila při upgradu hello se provádí z důvodu toohello způsob upgradu služby Azure. Hello smlouvu o úrovni záruční služby dostupnost služeb se vztahuje pouze na tooservices, který se nasadí s více než jednu instanci. Hello následující seznam popisuje, jak hello data na každém disku je ovlivňován každý scénář upgradu služby Azure:

|Scénář|Jednotka C|Diskovou jednotku d|Jednotce E:|
|--------|-------|-------|-------|
|Restartování virtuálního počítače|Zachovají|Zachovají|Zachovají|
|Restartování portálu|Zachovají|Zachovají|Zničení|
|Obnovení z Image portálu|Zachovají|Zničení|Zničení|
|Místní Upgrade|Zachovají|Zachovají|Zničení|
|Migrace uzlu|Zničení|Zničení|Zničení|

Upozorňujeme, že, v hello výše seznamu hello jednotce E: představuje hello role kořenové jednotce a nesmí být pevně. Místo toho použijte hello **RoleRoot %** jednotky hello toorepresent proměnné prostředí.

toominimize hello prostoje při upgradu jedné instance služby, nasaďte nový s více instancemi služby toohello pracovní server a proveďte prohození virtuálních IP adres.

<a name="RollbackofanUpdate"></a>

## <a name="rollback-of-an-update"></a>Vrácení zpět aktualizace
Azure poskytuje flexibilitu při správě služby během aktualizace tím, že umožňuje zahájit další operace ve službě, po hello počáteční aktualizace žádost přijme hello Kontroleru prostředků infrastruktury Azure. Vrácení zpět lze provést pouze, pokud by aktualizace (Změna konfigurace) nebo upgradu je v hello **v průběhu** stavu na hello nasazení. Aktualizaci nebo upgradu považuje toobe v průběhu, dokud bude alespoň jedna instance hello služby, který ještě nebylo aktualizované toohello novou verzi. tootest povolení vrácení zpět. Zkontrolujte hodnotu hello hello RollbackAllowed příznaku, vrácený [získat nasazení](https://msdn.microsoft.com/library/azure/ee460804.aspx) a [získat vlastnosti cloudové služby](https://msdn.microsoft.com/library/azure/ee460806.aspx) operace, je nastaven tootrue.

> [!NOTE]
> Pouze vytváří smysl toocall vrácení zpět na **na místě** aktualizovat nebo upgradovat, protože upgrady prohození virtuální IP adresy zahrnují nahrazení jednu celý spuštěné instanci služby s jinou.
>
>

Vrácení změn v průběhu aktualizace má následující důsledky pro nasazení hello hello:

* Nejsou žádné instance role, které by ještě nebyl aktualizované nebo upgradované toohello novou verzi aktualizovat nebo upgradovat, protože tato instance je již spuštěn hello cílová verze služby hello.
* Instance žádné role, které už nebyly aktualizovány nebo upgradované toohello novou verzi balíčku služby hello (\*.cspkg) souboru nebo hello konfigurace služby (\*.cscfg) souboru (nebo oba soubory) jsou vrácený toohello před upgradem verze Tyto soubory.

To funkčně zajišťuje hello následující funkce:

* Hello [vrácení zpět aktualizovat nebo upgradovat](https://msdn.microsoft.com/library/azure/hh403977.aspx) operace, které může zavolat na aktualizaci konfigurace (aktivovány volání [změna konfigurace nasazení](https://msdn.microsoft.com/library/azure/ee460809.aspx)) nebo upgradu (aktivovány volání [ Nasazení upgradu](https://msdn.microsoft.com/library/azure/ee460793.aspx)), dokud existuje alespoň jedna instance ve službě hello která dosud nebyla aktualizována toohello novou verzi.
* Hello elementu uzamčen a hello RollbackAllowed elementu, které se vrátí jako součást text odpovědi hello hello [získat nasazení](https://msdn.microsoft.com/library/azure/ee460804.aspx) a [získat vlastnosti cloudové služby](https://msdn.microsoft.com/library/azure/ee460806.aspx) operace:

  1. element uzamčen Hello umožňuje toodetect při mutating operace může být volána v zadaném nasazení.
  2. Hello RollbackAllowed element vám umožní toodetect při hello [vrácení zpět aktualizace nebo upgradu](https://msdn.microsoft.com/library/azure/hh403977.aspx) operaci nelze volat u dané nasazení.

  V pořadí tooperform vrácení zpět nemáte toocheck hello uzamčen i hello RollbackAllowed elementy. Stačí tooconfirm zda RollbackAllowed nastaveno tootrue. Tyto prvky jsou vrácena pouze v případě, že tyto metody jsou vyvolány pomocí hello hlavičky žádosti nastavit také "x-ms-version: 2011-10-01" nebo novější. Další informace o hlavičkách verze najdete v tématu [Správa služby správy verzí](https://msdn.microsoft.com/library/azure/gg592580.aspx).

Existují některé situace, kde vrácení zpět aktualizace nebo není upgrade podporován, jedná se o následujícím způsobem:

* Snížení místních prostředků – Pokud hello aktualizace zvyšuje hello místních prostředků pro roli hello platformy Azure neumožňuje vrácení zpět.
* Omezení kvóty – Pokud hello aktualizace byla vertikálně operace, které jste se už nebude mít dostatečný výpočetní kvóty toocomplete hello vrácení zpět. Každé předplatné Azure má quota s ním spojená, která určuje maximální počet jader, které mohou být spotřebovávána všechny hostované služby, které patří toothat předplatné hello. Je-li provést vrácení dané aktualizace by měla být umístěna vaše předplatné přes kvóty a která nebude povolen vrácení zpět.
* Soupeření podmínku – Pokud hello počáteční aktualizace skončila, vrácení zpět. není možné.

Je například při vrácení zpět hello aktualizace mohou být užitečné, pokud používáte hello [nasazení upgradu](https://msdn.microsoft.com/library/azure/ee460793.aspx) operace v ručním režimu toocontrol hello rychlost, jakou hlavní místní upgrade tooyour Azure hostovaná služba se nasazuje.

Při zavedení hello hello upgradu zavoláte [nasazení upgradu](https://msdn.microsoft.com/library/azure/ee460793.aspx) v ručním režimu a začít toowalk upgradovacích domén. Pokud v určitém okamžiku při sledování hello upgradu můžete Všimněte si některé instancí rolí ve hello první upgradu domén, které byste zkontrolovat mít přestat reagovat, můžete zavolat hello [vrácení zpět aktualizace nebo upgradu](https://msdn.microsoft.com/library/azure/hh403977.aspx) operace na hello nasazení, který ponechá nezměněný hello instance, které nebyly dosud nebyly upgradovány a vrácení instance, které je upgradovat toohello předchozí balíček služby a konfiguraci.

<a name="multiplemutatingoperations"></a>

## <a name="initiating-multiple-mutating-operations-on-an-ongoing-deployment"></a>Spouštění více mutating operací na probíhající nasazení
V některých případech může být vhodné tooinitiate více souběžných mutating operací na probíhající nasazení. Například může provádět aktualizace služby a během této aktualizace je se nasazuje přes služby, chcete toomake některé změny, například tooroll hello aktualizace zpět, použít jinou aktualizaci nebo odstranění i hello nasazení. Případ, kdy to může být potřeba je, zda aktualizace služby obsahuje buggy kódu, což způsobí, že při selhání instance toorepeatedly upgradovaná role. V takovém případě hello Kontroleru prostředků infrastruktury Azure nebude moct toomake průběh v použití, který upgradovat, protože jsou v pořádku dostatečný počet instancí v upgradované domény hello. Tento stav se označují tooas *zablokované nasazení*. Vrácení zpět hello aktualizace nebo novou aktualizaci v horní části hello selhání jeden můžete unstick hello nasazení.

Jakmile hello prvotní žádost tooupdate nebo upgradu hello service obdržel hello Kontroleru prostředků infrastruktury Azure, můžete začít následných mutace operací. To znamená, že nemáte toowait pro počáteční operaci toocomplete hello před zahájením mutating jiná operace.

Inicializace druhá operace aktualizace během probíhající aktualizace první hello provede operaci vrácení zpět podobné toohello. Pokud je druhý aktualizace hello v automatickém režimu, první doména upgradu hello upgraduje okamžitě, by mohl vést tooinstances z několika domén upgradu offline v hello stejné bodu v čase.

Hello mutace operace jsou následujícím způsobem: [změna konfigurace nasazení](https://msdn.microsoft.com/library/azure/ee460809.aspx), [nasazení upgradu](https://msdn.microsoft.com/library/azure/ee460793.aspx), [stav nasazení aktualizace](https://msdn.microsoft.com/library/azure/ee460808.aspx), [odstranit Nasazení](https://msdn.microsoft.com/library/azure/ee460815.aspx), a [vrácení aktualizaci nebo upgradu](https://msdn.microsoft.com/library/azure/hh403977.aspx).

Dvě operace [získat nasazení](https://msdn.microsoft.com/library/azure/ee460804.aspx) a [získat vlastnosti cloudové služby](https://msdn.microsoft.com/library/azure/ee460806.aspx), vrátí hello uzamčen příznak, který může být toodetermine zkontrolován, zda mutating operace může být volána v zadaném nasazení.

V pořadí toocall hello verze těchto metod, která vrátí hodnotu hello uzamčen příznak, musíte nastavit hlavička požadavku příliš "x-ms-version: 2011-10-01" nebo pozdější. Další informace o hlavičkách verze najdete v tématu [Správa služby správy verzí](https://msdn.microsoft.com/library/azure/gg592580.aspx).

<a name="distributiondfroles"></a>

## <a name="distribution-of-roles-across-upgrade-domains"></a>Distribuce rolí napříč doménami upgradu
Instance role Azure rovnoměrně distribuuje mezi se stanoveným počtem upgradovacích domén, které lze konfigurovat jako součást souboru definice (.csdef) služby hello. maximální počet domén upgradu Hello je 20 a hello výchozí hodnota je 5. Další informace o tom, jak toomodify hello souboru definice služby najdete v tématu [Azure schématu definice služby (.csdef souboru)](cloud-services-model-and-package.md#csdef).

Například pokud vaše role má deset instancí, ve výchozím nastavení každé upgradované domény obsahuje dvě instance. Pokud vaše role má 14 instancí, potom čtyři domén upgradu hello obsahovat tři instance a páté domény obsahuje dva.

Domén upgradu jsou označeny index počítaný od nuly: první doména upgradu hello má ID 0, a druhá doména upgradu hello ID 1 a tak dále.

Hello následující diagram znázorňuje rozdělení služby než obsahuje dvě role při hello služby definuje dvě domény upgradu. osm instancí hello webovou roli a devět instancí role pracovního procesu hello je spuštěna služba Hello.

![Distribuce domén upgradu](media/cloud-services-update-azure-service/IC345533.png "distribuce domén upgradu")

> [!NOTE]
> Všimněte si, že Azure řídit přidělování instancí napříč doménami upgradu. Není možné toospecify instancí, které jsou přiděleny toowhich domény.
>
>

## <a name="next-steps"></a>Další kroky
[Jak tooManage cloudových služeb](cloud-services-how-to-manage.md)  
[Jak tooMonitor cloudových služeb](cloud-services-how-to-monitor.md)  
[Jak tooConfigure cloudových služeb](cloud-services-how-to-configure.md)  
