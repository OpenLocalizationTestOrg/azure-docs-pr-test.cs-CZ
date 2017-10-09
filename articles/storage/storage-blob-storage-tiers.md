---
title: "aaaAzure studeného úložiště pro objekty BLOB | Microsoft Docs"
description: "Úrovně úložiště služby Azure Blob Storage umožňují vybrat si cenově výhodné úložiště pro data objektů podle toho, jak často k nim chcete přistupovat. úroveň studeného úložiště Hello je optimalizovaná pro data, která se využívají méně často."
services: storage
documentationcenter: 
author: michaelhauss
manager: vamshik
editor: tysonn
ms.assetid: eb33ed4f-1b17-4fd6-82e2-8d5372800eef
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/05/2017
ms.author: mihauss
ms.openlocfilehash: 56fae3fdae31a6958ebae01fd96a0366e70ae938
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-blob-storage-hot-and-cool-storage-tiers"></a>Azure Blob Storage: Horká a studená úroveň úložiště
## <a name="overview"></a>Přehled
Azure Storage nabízí dvě úrovně úložiště pro ukládání objektů Blob. Data, abyste mohli data ukládat co nejhospodárněji – to znamená podle toho, jak je používáte. Hello Azure **úroveň horkého úložiště** je optimalizovaná pro ukládání dat, která se využívají často. Hello Azure **úroveň studeného úložiště** je optimalizovaná pro ukládání dat, která se nevyužívají dlohotrvající. Data v úroveň studeného úložiště hello může tolerovat trochu horší dostupnost, ale stále vyžaduje vysoká odolnost a podobnou charakteristikou čas přístupu a propustnost jako u horkých dat.. U studených dat jsou poplatky za uložení výrazně levnější, ovšem za cenu mírně horší dostupnosti a vyšších nákladů na přístup.

V současné době data uložená v cloudu hello narůstají exponenciální rychlostí. toomanage stojí zvětšující potřebujete úložiště, je užitečné tooorganize vaše data na základě atributů jako frekvence přístup a plánované dobu uchování. Data uložená v cloudu hello může být lišit podle toho, jak je vygenerována, zpracování a přístupné přes celé jeho životnosti. Některá data se během svojí existence využívají nebo mění často. Některá data se využívají často časná v průběhu své životnosti, s přístupem k vyřazení výrazně jako stáří dat hello. Některá data zůstane nečinnosti v cloudu hello a zřídka, pokud vůbec někdy získat přístup k jednou uložené.

Pro každý z těchto scénářů využívání je vhodná jiná úroveň úložiště, která je optimalizovaná pro určitý vzor přístupu. S hello Úvod vrstvy horkého a studeného úložiště Azure Blob storage teď zohlednit tuto potřebu různých druhů úložišť na různých cenových úrovních.

## <a name="blob-storage-accounts"></a>Účty úložiště Blob
**Účty úložiště Blob** jsou účty specializovaného úložiště pro ukládání nestrukturovaných dat jako blobů (objektů) v úložišti Azure Storage. S účty úložiště Blob teď můžete mezi vrstvami toostore horkého a studeného úložiště vaše méně často používaná data nižší náklady na úložiště a úložiště přistupovat častěji horkých dat. na nižší cenou za přístup. Účty úložiště BLOB jsou podobné tooyour existující účty úložiště pro obecné účely a sdílet všechny hello vysokou odolnost, dostupnost, škálovatelnost a výkon funkce, které použijete v současné době včetně 100 % konzistentnost rozhraní API pro objekty BLOB bloku a doplňovací objekty BLOB.

> [!NOTE]
> Účty úložiště Blob podporují pouze objekty blob bloku a doplňovací objekty blob, nepodporují objekty blob stránky.
> 
> 

Účty úložiště BLOB zpřístupňují hello **úroveň přístupu** atribut, který vám umožní toospecify hello úroveň úložiště jako **horká** nebo **nástrojů** v závislosti na hello data uložená v hello účet. Pokud dojde ke změně v hello vzor používání vašich údajů, mohou také přepínat mezi tyto vrstvy úložiště kdykoli.

> [!NOTE]
> Změna hello úroveň úložiště může být spojené další poplatky. Najdete v tématu hello [ceny a fakturace](storage-blob-storage-tiers.md#pricing-and-billing) další podrobnosti.
> 
> 

Příklad scénáře použití pro úroveň horkého úložiště hello patří:

* Data, která se v aktivního používání nebo očekávané toobe využívají (čtení z a zapsaný na) často.
* Data, která jsou určená ke zpracování a eventuální migraci toohello studené úrovni úložiště.

Příklad scénáře použití pro úroveň studeného úložiště hello patří:

* Záloha, archivace a datové sady pro zotavení po havárii.
* Starší obsah a média není již nezobrazují často, ale je k dispozici očekávané toobe okamžitě.
* Velkých datových sad, které je třeba toobe ukládají náklady efektivně, když je více dat shromážděných pro pozdější zpracování. (*např.* dlouhodobé uložení vědeckých dat, nezpracovaná telemetrická data z výrobního závodu)
* Původní (hrubá, nezpracovaná) data, která je potřeba zachovat i po jejich zpracování do konečné, použitelné podoby. (*např.* původní multimediální záznamy po překódování do jiných formátů)
* Dodržování předpisů a archivaci dat, který potřebuje toobe uložené po dlouhou dobu a téměř nikdy přistupuje. (*např.* záznamy z bezpečnostních kamer, staré rentgenové snímky / snímky magnetické rezonance pro zdravotnická zařízení, zvukové záznamy a přepisy zákaznických hovorů pro finanční služby)

Další informace o účtech úložiště najdete v tématu [Účty Azure Storage](storage-create-storage-account.md).

Pro aplikace, které vyžadují jenom bloku a doplňovacích objektů blob storage, doporučujeme používat účty úložiště Blob, tootake výhod hello rozlišené cenový model vrstvené úložiště. Chápeme ale, že to nemusí být možné za určitých okolností kde pomocí úložiště pro obecné účely, které by bylo účty hello toogo způsobem, jako:

* Je třeba toouse tabulky, fronty, nebo soubory a chcete, objektů BLOB uložené v hello stejný účet úložiště. Všimněte si, že žádné toostoring technickou výhodu v hello stejný účet, kromě nutnosti hello stejné sdílených klíčů.
* Model nasazení Classic hello toouse stále potřebujete. Účty úložiště BLOB jsou dostupné prostřednictvím modelu nasazení Azure Resource Manager hello jenom.
* Je nutné toouse objekty BLOB stránky. Účty úložiště Blob nepodporují objekty blob stránky. Pokud nemáte nějaký zvláštní nebo konkrétní důvod používat objekty blob stránky, obvykle doporučujeme používat objekty blob bloku.
* Použijte verzi hello [Storage Services REST API](https://msdn.microsoft.com/library/azure/dd894041.aspx) která je starší než 2014-02-14 nebo klientskou knihovnu verze nižší než 4.x a nemůžete svoji aplikaci upgradovat.

> [!NOTE]
> Účty Blob Storage jsou aktuálně podporované ve všech oblastech Azure.
> 
> 

## <a name="comparison-between-hello-storage-tiers"></a>Porovnání mezi vrstvami úložiště hello
Hello následující tabulka obsahuje porovnání hello mezi dvěma vrstvami úložiště hello:

<table border="1" cellspacing="0" cellpadding="0" style="border: 1px solid #000000;">
<col width="250">
<col width="250">
<col width="250">
<tbody>
<tr>
    <td><strong><center></center></strong></td>
    <td><strong><center>Úroveň horkého úložiště</center></strong></td>
    <td><strong><center>Úroveň studeného úložiště</center></strong></td
</tr>
<tr>
    <td><strong><center>Dostupnost</center></strong></td>
    <td><center>99,9 %</center></td>
    <td><center>99 %</center></td>
</tr>
<tr>
    <td><strong><center>Dostupnost<br>(přístupy pro čtení RA-GRS)</center></strong></td>
    <td><center>99,99 %</center></td>
    <td><center>99,9 %</center></td>
</tr>
<tr>
    <td><strong><center>Poplatky za používání</center></strong></td>
    <td><center>Vyšší poplatky za uložení<br>Nižší poplatky za přístup a transakci</center></td>
    <td><center>Nižší poplatky za uložení<br>Vyšší poplatky za přístup a transakci</center></td>
</tr>
<tr>
    <td><strong><center>Minimální velikost objektu<center></strong></td>
    <td colspan="2"><center>–</center></td>
</tr>
<tr>
    <td><strong><center>Minimální doba uložení<center></strong></td>
    <td colspan="2"><center>–</center></td>
</tr>
<tr>
    <td><strong><center>Latence<br>(Doba toofirst bajtu)<center></strong></td>
    <td colspan="2"><center>milisekundy</center></td>
</tr>
<tr>
    <td><strong><center>Škálovatelnost a cíle výkonnosti<center></strong></td>
    <td colspan="2"><center>Stejné jako u účtů úložiště pro obecné účely</center></td>
</tr>
</tbody>
</table>

> [!NOTE]
> Úložiště objektů BLOB účty podporu hello stejný výkon a škálovatelnost cíle jako účty úložiště pro obecné účely. Další informace najdete v tématu [Škálovatelnost a cíle výkonnosti Azure Storage Scalability](storage-scalability-targets.md).
> 
> 

## <a name="pricing-and-billing"></a>Ceny a fakturace
Účty úložiště BLOB používají nový cenový model pro úložiště objektů blob podle úrovně úložiště hello. Při používání účtu úložiště Blob, platí následující aspekty fakturace hello:

* **Náklady na úložiště**: kromě toohello množství dat uložených hello náklady na ukládání dat se liší v závislosti na úrovni úložiště hello. je Hello za gigabajt nižší úroveň studeného úložiště hello než pro úroveň horkého úložiště hello.
* **Náklady na přístup k datům**: pro data v hello úroveň studeného úložiště vám bude účtována poplatek za gigabajt dat přístup pro čtení a zápisy.
* **Cena za transakci**: Na obě úrovně se vztahuje poplatek za jednotlivé transakce. Cena za transakci hello pro hello úroveň studeného úložiště je však vyšší než pro úroveň horkého úložiště hello.
* **Cena za geografickou replikaci datové přenosy**: platí jen tooaccounts s nastavenou geografickou replikací, včetně GRS a RA-GRS. Přenos dat geografické replikace je zpoplatněný podle sazby za GB.
* **Cena za odchozí přenosy dat**: Odchozí přenosy dat (dat přenesených směrem z oblasti Azure) jsou zpoplatněné podle využití šířky pásma sazbou za GB, stejně jako je tomu u účtů úložiště pro obecné účely.
* **Změna úrovně úložiště hello**: Změna úrovně úložiště hello ze studeného toohot zpoplatněná rovna tooreading poplatků všechna data hello existující v hello účet úložiště pro každý přechod. Na hello bude druhé straně, změna úrovně úložiště hello z aktivního toocool zdarma.

> [!NOTE]
> V pořadí tooallow uživatelé tootry out hello nové úrovně úložiště a ověřit funkčnost služby po jejím spuštění, hello zdarma pro změnu úroveň úložiště hello z nástrojů toohot zákazníci až do 30. června 2016. Od 1. července 2016 se bude hello poplatků použité tooall přechody ze studeného toohot. Další informace o hello cenový model pro účty úložiště Blob najdete [Azure Storage – ceny](https://azure.microsoft.com/pricing/details/storage/) stránky. Další informace o datech hello odchozí přenos poplatky najdete [o cenách přenosů dat](https://azure.microsoft.com/pricing/details/data-transfers/) stránky.
> 
> 

## <a name="quick-start"></a>Rychlý start
V této části si předvedeme následující scénáře s využitím hello portál Azure hello:

* Jak toocreate účtu úložiště Blob.
* Jak toomanage účtu úložiště Blob.

### <a name="using-hello-azure-portal"></a>Pomocí hello portálu Azure
#### <a name="create-a-blob-storage-account-using-hello-azure-portal"></a>Vytvoření účtu úložiště Blob pomocí portálu Azure hello
1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. V nabídce centra hello vyberte **nový** > **Data + úložiště** > **účet úložiště**.
3. Zadejte název účtu úložiště.
   
    Tento název musí být globálně jedinečný; používá se jako součást hello URL použít tooaccess hello objekty v účtu úložiště hello.  
4. Vyberte **Resource Manager** jako model nasazení hello.
   
    Vrstvené úložiště lze použít pouze s účty úložiště Resource Manager; Toto je doporučená model nasazení pro nové prostředky hello. Další informace, podívejte se na hello [přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md).  
5. V rozevíracím seznamu hello typ účtu, vyberte **úložiště objektů Blob**.
   
    Toto je, kde můžete vybrat hello typ účtu úložiště. Vrstvené úložiště není k dispozici v úložiště pro obecné účely; je k dispozici v hello typ účtu úložiště Blob.     
   
    Všimněte si, že při výběru tato úroveň výkonu hello je tooStandard nastavit. Vrstvené úložiště není k dispozici úroveň výkonu hello Premium.
6. Vyberte možnost hello replikace pro účet úložiště hello: **LRS**, **GRS**, nebo **RA-GRS**. Výchozí hodnota Hello je **RA-GRS**.
   
    LRS = místně redundantního úložiště; GRS = geograficky redundantní úložiště (2 oblastech); RA-GRS je geograficky redundantní úložiště s přístupem pro čtení (2 oblasti čtení přístup k toohello druhý).
   
    Další informace o možnostech replikace v Azure Storage najdete v článku [Replikace Azure Storage](storage-redundancy.md).
7. Vyberte hello úroveň správné úložiště pro vaše potřeby: Sada hello **úroveň přístupu** tooeither **nástrojů** nebo **horká**. Výchozí hodnota Hello je **horká**.
8. Vyberte hello předplatné, ve kterém chcete toocreate hello nový účet úložiště.
9. Zadejte novou skupinu prostředků nebo vyberte existující skupinu prostředků. Další informace o skupinách prostředků najdete v článku [Přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md).
10. Vyberte oblast hello vašeho účtu úložiště.
11. Klikněte na tlačítko **vytvořit** účet úložiště toocreate hello.

#### <a name="change-hello-storage-tier-of-a-blob-storage-account-using-hello-azure-portal"></a>Úroveň úložiště hello účtu úložiště Blob pomocí portálu Azure hello změnit.
1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. účet úložiště tooyour toonavigate, vyberte všechny prostředky a potom vyberte svůj účet úložiště.
3. V okně Nastavení hello, klikněte na tlačítko **konfigurace** tooview nebo změna konfigurace účtu hello.
4. Vyberte hello úroveň správné úložiště pro vaše potřeby: Sada hello **úroveň přístupu** tooeither **nástrojů** nebo **horká**.
5. Kliknutím na Uložit hello horní části okna hello.

> [!NOTE]
> Změna hello úroveň úložiště může být spojené další poplatky. Najdete v tématu hello [ceny a fakturace](storage-blob-storage-tiers.md#pricing-and-billing) další podrobnosti.
> 
> 

## <a name="evaluating-and-migrating-tooblob-storage-accounts"></a>Vyhodnocení a migrace účtů úložiště tooBlob
Hello účelem v této části je toohelp uživatelé toomake plynulého přechod toousing účty úložiště Blob. Jako uživatel stojíte před jednou z těchto dvou možností:

* Máte stávající účet úložiště pro obecné účely a chcete tooevaluate tooa změnu účtu úložiště Blob se hello správné úložiště vrstvou.
* Jste se rozhodli toouse účtu úložiště Blob nebo již máte jeden a chcete tooevaluate, zda byste měli používat vrstvy úložiště aktivní nebo nástrojů hello.

V obou případech hello firmy of první pořadí je tooestimate hello náklady na ukládání a přístup k datům uložené v účtu úložiště Blob a který porovnávat s náklady na aktuální.

### <a name="evaluating-blob-storage-account-tiers"></a>Vyhodnocení úrovní účtu úložiště Blob
V pořadí tooestimate hello náklady na ukládání a přístup k datům, které jsou uložené v účtu úložiště Blob budete potřebovat tooevaluate svůj aktuální vzor používání nebo přibližná vaší očekávané využití vzoru. Obecně platí budete chtít tooknow:

* Spotřebu úložiště – Kolik dat ukládáte a jak se toto množství měsíc od měsíce mění?
* Váš přístup vzor úložiště – kolik dat se číst z a účet napsané toohello (včetně nových dat)? Ke kolika transakcím dochází při přístupu k datům a o jaké transakce se jedná?

#### <a name="monitoring-existing-storage-accounts"></a>Monitorování existujících účtů úložiště
toomonitor stávající úložiště účtů a shromažďovat tato data, můžete nastavit, používání Azure Storage Analytics, která provádí protokolování a poskytuje data metriky pro účet úložiště.
Analytika úložiště můžete ukládat metriky, které zahrnují transakce souhrnné statistiky a kapacity data o toohello žádosti o služby úložiště objektů Blob pro účty úložiště pro obecné účely jak účty úložiště Blob.
Tato data jsou ukládána v dobře známé tabulek v hello stejný účet úložiště.

Další podrobnosti najdete na stránce [o metrikách Storage Analytics](https://msdn.microsoft.com/library/azure/hh343258.aspx) a [o tabulkovém schématu metrik Storage Analytics](https://msdn.microsoft.com/library/azure/hh343264.aspx).

> [!NOTE]
> Účty úložiště BLOB zpřístupňují koncový bod služby tabulek hello pouze pro ukládání a přístup k datům hello metriky pro tento účet.
> 
> 

Spotřeba úložiště hello toomonitor pro hello služby úložiště objektů Blob, budete potřebovat metriky kapacity tooenable hello.
Tuto funkci povolíte, kapacity data jsou zaznamenány každý den pro účet úložiště služby objektů Blob a zaznamená jako záznam tabulky, které jsou zapsány toohello *$MetricsCapacityBlob* tabulky v rámci hello stejný účet úložiště.

hello toomonitor hello data vzor přístupu pro službu úložiště objektů Blob, budete potřebovat tooenable hello hodinové transakce metriky na úrovni rozhraní API.
Tuto funkci povolíte, na rozhraní API transakce jsou agregovat každou hodinu a zaznamenává jako záznam tabulky, které jsou zapsány toohello *$MetricsHourPrimaryTransactionsBlob* tabulky v rámci hello stejný účet úložiště. Hello *$MetricsHourSecondaryTransactionsBlob* záznamů tabulky hello transakce toohello sekundárního koncového bodu v případě účtů úložiště RA-GRS.

> [!NOTE]
> Pokud máte účet úložiště pro obecné účely, ve kterém jsou uložené objekty blob stránek a disky virtuálních počítačů vedle dat doplňovacích objektů blob, odhad tímto postupem provést nepůjde. Je to proto, že budete mít nijak rozlišovací kapacity a transakce metriky na základě hello typu Objekt blob pro pouze blokovat a doplňovací objekty BLOB, které lze migrovat tooa účtu úložiště Blob.
> 
> 

tooget dobrým odhadem spotřeby dat a vzor přístupu, doporučujeme vybrat dobu uchování hello metriky, který je typický pro vaše regulární využití a odvodit.
Jednou z možností je tooretain hello metriky dat pro 7 dní a shromažďování hello data každý týden pro analýzu na konci hello měsíci hello.
Další možností je tooretain hello metriky, dat pro hello posledních 30 dnů a shromáždění a analýza dat hello na konci hello hello období 30 dnů.

Podrobnosti o povolení, shromažďování a zobrazování dat metrik naleznete v tématu [Povolení metrik Azure Storage a prohlížení dat metrik](storage-enable-and-view-metrics.md).

> [!NOTE]
> Uložení, zobrazování a stahování analyzovaných data je účtováno stejně jako běžná uživatelská data.
> 
> 

#### <a name="utilizing-usage-metrics-tooestimate-costs"></a>Využitím náklady tooestimate metriky využití
##### <a name="storage-costs"></a>Cena za uložení
Hello nejnovější záznam v tabulce metriky kapacity hello *$MetricsCapacityBlob* s klíčem řádku hello *'údaje'* ukazuje hello kapacitu úložiště, které jsou využívány službou data uživatele.
Hello nejnovější záznam v tabulce metriky kapacity hello *$MetricsCapacityBlob* s klíčem řádku hello *'analytics'* ukazuje hello hello analýzy protokolů spotřebovávají kapacitu úložiště.

Tato celková kapacita spotřebovávají oba uživatele data a analýzy protokolů (Pokud je povoleno) lze použít tooestimate hello náklady na ukládání dat v účtu úložiště hello.
Hello stejnou metodu lze použít také k odhadování náklady na úložiště pro bloku a doplňovacích objektů BLOB v účtech úložiště pro obecné účely.

##### <a name="transaction-costs"></a>Cena za transakce
Součet Hello *'TotalBillableRequests'*, přes všechny záznamy pro rozhraní API v transakci hello metriky tabulka udává hello celkový počet transakcí pro toto konkrétní rozhraní API. *například*, hello celkový počet *'Getblob –'* transakce v daném časovém období, lze vypočítat podle hello součet celkové fakturovatelné požadavků pro všechny záznamy s klíčem řádku hello *' uživatele. Getblob – '*.

V pořadí tooestimate transakce náklady na účty úložiště Blob budete potřebovat toobreak dolů hello transakce do tří skupin vzhledem k tomu, že jsou cenově jinak.

* Transakce zápisu jako *PutBlob*, *PutBlock*, *PutBlockList*, *AppendBlock*, *ListBlobs*, *ListContainers*, *CreateContainer*, *SnapshotBlob* a *CopyBlob*.
* Transakce odstranění jako *DeleteBlob* a *DeleteContainer*.
* Veškeré ostatní transakce.

Pořadí tooestimate transakce náklady pro účty úložiště pro obecné účely je nutné tooaggregate všechny transakce, bez ohledu na operaci hello nebo rozhraní API.

##### <a name="data-access-and-geo-replication-data-transfer-costs"></a>Přístup k datům a cena za přenos dat – geografická replikace
Během analytika úložiště neposkytuje hello množství dat číst z a zapisovat tooa účet úložiště, ho můžete být zhruba odhadnout na základě hello transakce metriky tabulky.
Součet Hello *'TotalIngress'* napříč všechny záznamy pro rozhraní API v hello transakce metriky tabulka udává celkovou velikost hello příchozí přenos dat v bajtech pro toto konkrétní rozhraní API.
Podobně hello součet *'TotalEgress'* označuje celkovou velikost hello odchozí data v bajtech.

V pořadí tooestimate hello data nákladů na přístup pro účty úložiště Blob budete potřebovat toobreak dolů hello transakce do dvou skupin.

* Hello množství dat načtených z účtu úložiště hello se dá odhadnout prohlížením hello součet *'TotalEgress'* pro především hello *'Getblob –'* a *'CopyBlob'* operace.
* Hello množství dat zapsaných toohello účet úložiště se dá odhadnout prohlížením hello součet *'TotalIngress'* pro především hello *'PutBlob'*, *'PutBlock'*, *'CopyBlob'* a *'AppendBlock'* operace.

Hello náklady přenosu dat geografická replikace pro účty úložiště, lze vypočítat také pomocí hello odhad pro hello množství dat, které jsou napsané v případě GRS nebo RA-GRS účtu úložiště objektů Blob.

> [!NOTE]
> Například podrobnější o výpočet hello náklady na použití úroveň hello aktivní nebo studeného úložiště, proveďte prosím podívejte se na hello – nejčastější dotazy s názvem *"jaké jsou aktivní a nástrojů úrovní přístupu a jak měli určit které jeden toouse?"* v hello [stránce s cenami úložiště Azure](https://azure.microsoft.com/pricing/details/storage/).
> 
> 

### <a name="migrating-existing-data"></a>Migrace existujících dat
Účet úložiště Blob se specializuje pouze na ukládání objektů blob bloku a doplňovacích objektů blob. Existující účty úložiště pro obecné účely, které umožňují toostore tabulky, fronty, soubory a disky, jakož i objekty BLOB, nemůže být převedená tooBlob účty úložiště. toouse hello vrstvy úložiště, budete potřebovat toocreate nové účty úložiště Blob a svoje existující data migrovat do hello nově vytvořené účty.

Můžete použít následující metody toomigrate existujících dat do účtů úložiště Blob z místní úložiště zařízení, z poskytovatelů úložiště cloudu třetích stran nebo z existující účty úložiště pro obecné účely v Azure hello:

#### <a name="azcopy"></a>AzCopy
AzCopy je nástroj příkazového řádku Windows určený pro vysoce výkonné kopírování dat tooand ze služby Azure Storage. Do svého účtu úložiště Blob, můžete použít AzCopy toocopy data do svého účtu úložiště Blob z vaší existující účty úložiště pro obecné účely nebo tooupload data z vaší místní úložiště zařízení.

Další podrobnosti najdete v tématu [přenos dat pomocí příkazového řádku Azcopy hello](storage-use-azcopy.md).

#### <a name="data-movement-library"></a>Knihovna pro přesun dat
Azure knihovna pro přesun dat úložiště pro .NET je založená na hello přesun základních dat, využívá nástroj AzCopy. Hello knihovna je určená pro vysoce výkonné, podobně jako tooAzCopy operace přenosu dat spolehlivé a snadné. To vám umožní tootake naplno využívat výhody hello funkcí AzCopy ve vaší aplikaci nativně bez nutnosti toodeal spouštět a sledovat externí instance nástroje AzCopy.

Další informace najdete v tématu [Knihovna pro přesun dat v Azure Storage pro .Net](https://github.com/Azure/azure-storage-net-data-movement).

#### <a name="rest-api-or-client-library"></a>Rozhraní REST API nebo knihovna klienta
Můžete vytvořit vlastní aplikaci toomigrate dat do účtu úložiště Blob pomocí jedné z knihoven klienta Azure hello nebo hello Azure storage services REST API. Azure Storage poskytuje množství knihoven klienta pro různé jazyky a platformy, jako například .NET, Java, C++, Node.JS, PHP, Ruby nebo Python. Hello knihovny klienta nabízí pokročilé funkce, jako je například logika opakovaných pokusů, protokolování a paralelní ukládání. Také můžete vyvíjet přímo na hello REST API, které může zavolat jakýkoli jazyk schopný požadavky HTTP/HTTPS.

Další informace najdete v tématu [Začínáme s úložištěm Azure Blob](storage-dotnet-how-to-use-blobs.md).

> [!NOTE]
> Objekty BLOB šifrované pomocí šifrování na straně klienta ukládají metadata šifrování uložená s objektem blob hello. Je absolutně nezbytné, že všechny mechanizmus kopírování zajistil, hello metadata objektu blob a hlavně hello metadata šifrování, je zachovaná. Pokud zkopírujete objekty BLOB hello bez těchto metadat, obsah hello objektu blob bude nenávratně ztracený. Podrobnější informace o šifrování metadat najdete v článku o [Azure Storage a šifrování na straně klienta](storage-client-side-encryption.md).
> 
> 

## <a name="faqs"></a>Nejčastější dotazy
1. **Jsou existující účty úložiště stále dostupné?**
   
    Ano, existující účty úložiště jsou stále dostupné a jejich funkce ani cena se nemění.  Jejich nemají hello možnost toochoose vrstvy úložiště a nebude mít ani v budoucnu hello.
2. **Proč a kdy bych měl/a začít používat účty úložiště Blob?**
   
    Účty úložiště BLOB se specializují na ukládání objektů BLOB a umožňují nám toointroduce nové funkce. Do budoucna, účty úložiště Blob jsou hello doporučená způsob pro ukládání objektů BLOB, jako funkce hierarchických úložišť a vrstvení budou zavedeny založené na tento typ účtu. Je však až tooyou Chcete-li toomigrate na základě požadavků vaší firmy.
3. **Můžete převést Můj stávající účet tooa úložiště účtu úložiště Blob?**
   
    Ne. Účet úložiště blob je jiný druh účtu úložiště a budete potřebovat toocreate je nový a migrovat data, jak je popsáno výše.
4. **Můžete ukládat objekty v obou vrstvách úložiště v hello stejný účet?**
   
    Hello *'úroveň přístupu,* atribut, který určuje úroveň úložiště hello je nastaven na úrovni účtu a platí tooall objekty v daném účtu. Atribut úrovně přístupu hello nelze nastavit na úrovni objektů.
5. **Můžete změnit úroveň úložiště hello svého účtu úložiště Blob?**
   
    Ano. Bude moct toochange hello úložiště vrstvy a nastavení hello *'úroveň přístupu,* atribut na účet úložiště hello. Změna úrovně úložiště hello uplatní tooall objekty uložené v účtu hello. Úroveň úložiště hello změnu z aktivního toocool nebudou vám narůstat poplatky, při Změna ze studené toohot zpoplatněná za náklady na GB pro čtení všech hello dat v účtu hello.
6. **Jak často můžete změnit úroveň úložiště hello svého účtu úložiště Blob?**
   
    Když jsme Nevynucovat omezení toho, jak často se smí měnit úroveň úložiště hello, Upozorňujeme, že změna úrovně úložiště hello ze studeného toohot vás znamená výrazné náklady. Změna úrovně úložiště hello často nedoporučujeme.
7. **Bude hello objektů BLOB v hello úroveň studeného úložiště chovat jinak než ty, které v úroveň horkého úložiště hello hello?**
   
    Objekty BLOB v hello horké úrovni úložiště mají hello stejnou latenci jako objekty BLOB v účtech úložiště pro obecné účely. Objekty BLOB v hello studené úrovni úložiště mají podobnou latenci (v milisekundách) jako objekty BLOB v účtech úložiště pro obecné účely.
   
    Objekty BLOB v hello úroveň studeného úložiště budou mít trochu nižší dostupnost úrovní služeb (SLA) než objekty BLOB hello uložené v hello úroveň horkého úložiště. Další informace najdete v tématu [SLA pro úložiště](https://azure.microsoft.com/support/legal/sla/storage).
8. **Můžu do účtů úložiště Blob ukládat objekty blob stránky a disky virtuálních počítačů?**
   
    Účty úložiště Blob podporují pouze objekty blob bloku a doplňovací objekty blob, nepodporují objekty blob stránky. Disky virtuálních počítačů Azure se opírají o objekty BLOB stránky a v důsledku účty úložiště Blob nelze použít toostore disky virtuálního počítače. Je ale možné toostore zálohy hello disky virtuálního počítače jako objekty BLOB bloku v účtu úložiště Blob.
9. **Bude potřeba toochange mé existující aplikace toouse účty úložiště Blob?**
   
    Účty úložiště Blob mají rozhraní API 100% konzistentní s účty úložiště pro obecné účely pro objekty blob bloku a doplňující objekty blob. Jak dlouho, dokud vaše aplikace používá objekty BLOB bloku nebo doplňovací objekty BLOB a používáte verze 2014-02-14 hello hello [Storage Services REST API](https://msdn.microsoft.com/library/azure/dd894041.aspx) nebo novější, vaše aplikace by měla fungovat. Pokud používáte starší verzi protokolu hello, budete potřebovat tooupdate novou verzi vaší aplikace toouse hello tak jako toowork bezproblémově s oběma typy účtů úložiště. Obecně platí vždy doporučujeme používat nejnovější verzi hello bez ohledu na to, jaký typ účtu úložiště používáte.
10. **Budu muset něco dělat jinak?**
    
    Účty úložiště BLOB jsou velmi podobné tooa účty úložiště pro obecné účely pro ukládání bloku a doplňovací objekty BLOB a podporují všechny klíčové funkce hello Azure Storage, včetně vysoké odolnosti a dostupnosti, škálovatelnosti, výkonnosti a zabezpečení. Než hello funkcí a omezení konkrétní tooBlob úložiště účtů a jeho vrstvy úložiště, které jsme je popsali výše, by všechno hello else zůstává stejné.

## <a name="next-steps"></a>Další kroky
### <a name="evaluate-blob-storage-accounts"></a>Posouzení účtů úložiště Blob
[Ověření dostupnosti účtů úložiště Blob v jednotlivých oblastech](https://azure.microsoft.com/regions/#services)

[Zapnutí metrik Azure Storage a vyhodnocení používání aktuálních účtů úložiště](storage-enable-and-view-metrics.md)

[Posouzení cen úložiště Blob v jednotlivých oblastech](https://azure.microsoft.com/pricing/details/storage/)

[Posouzení cen přenosu dat](https://azure.microsoft.com/pricing/details/data-transfers/)

### <a name="start-using-blob-storage-accounts"></a>Začátek používání účtů úložiště Blob
[Začínáme s úložištěm Azure Blob](storage-dotnet-how-to-use-blobs.md)

[Přesun dat tooand ze služby Azure Storage](storage-moving-data.md)

[Přenos dat pomocí příkazového řádku Azcopy hello](storage-use-azcopy.md)

[Procházení a prozkoumání účtů úložiště](http://storageexplorer.com/)

