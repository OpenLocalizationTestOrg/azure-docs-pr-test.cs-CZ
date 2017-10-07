---
title: "aaaLearn o hello nejnovější verzí hostovaného operačního systému Azure | Microsoft Docs"
description: "Hello nejnovější verze informace a kompatibility sady SDK pro Azure Cloud Services hostovaného operačního systému."
services: cloud-services
documentationcenter: na
author: raiye
manager: timlt
editor: 
ms.assetid: 6306cafe-1153-44c7-8554-623b03d59a34
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 8/24/2017
ms.author: raiye
ms.openlocfilehash: 7274f5a68a32ce91bdede77e1443cdb8053c07ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-guest-os-releases-and-sdk-compatibility-matrix"></a>Azure verze hostovaného operačního systému a kompatibilních sad SDK
Poskytuje aktuální informace o hello nejnovější hostovaného operačního systému Azure se uvolní pro cloudové služby. Tyto informace vám pomůžou naplánovat způsob upgradu než hostovaného operačního systému je zakázáno. Pokud nakonfigurujete vaše role toouse *automatické* aktualizace hostovaného operačního systému, jak je popsáno v [nastavení aktualizace operačního systému hosta Azure][Azure Guest OS Update Settings], není důležité, abyste si přečetli tuto stránku.

> [!IMPORTANT]
> Tato stránka se vztahuje tooCloud služby webové a pracovní role, které běží nad hostovaného operačního systému. Provede **nelze použít** tooIaaS virtuálních počítačů.
>
>


> [!NOTE]
> Hello informačního kanálu RSS, byla nedávno zastaralé. Sledovat aktualizace na nové informačního kanálu už brzy!
>
>

Jistí, o jaké hello hostovaného operačního systému je nebo jak hello pracovní verze hostovaného operačního systému? Čtení [to](#how-it-works) části.

## <a name="news-updates"></a>Nejnovější zprávy

###### <a name="august-24-2017"></a>**24 srpen 2017**
Vydala srpen hostovaného operačního systému.

###### <a name="august-3-2017"></a>**3. srpna 2017**
Vydala července hostovaného operačního systému.

###### <a name="july-19-2017"></a>**19. července 2017**
Zavedení července hostovaného operačního systému se spouští července 19 a má předpokládané verzi srpen 8.

###### <a name="july-7-2017"></a>**7. července 2017**
Vydala června hostovaného operačního systému.

###### <a name="june-16-2017"></a>**16 června 2017**
Zavedení června hostovaného operačního systému se spouští června 16 a má předpokládané verzi července 11.

###### <a name="june-5-2017"></a>**5 června 2017**
Může vydala hostovaného operačního systému.

###### <a name="may-17-2017"></a>**17 může 2017**
Z důvodu chyb zabezpečení tooa, jsme zakazování hello prosinec 2016 a ledna 2017 verzích operačního systému, která nemají hello [opravte] z portálu hello: WA-hosta-operačního systému-5.4_201612-01, WA-hosta-operačního systému-4.39_201612-01, WA-hosta-operačního systému-3.46_ WA 201612-01,-GUEST-OS-2.59_201701-01

###### <a name="may-12-2017"></a>**12 může 2017**
Zavedení může hostovaného operačního systému se spouští může 12 a má předpokládané verzi 13. června.

###### <a name="april-18-2017"></a>**18. dubna 2017**
Zavedení duben hostovaného operačního systému se spouští dne 18 a má předpokládané verzi může 9.

###### <a name="april-10-2017"></a>**10. dubna 2017**
Zavedení března hostovaného operačního systému spustit 14. března 2017 a vydala 10. dubna 2017.

###### <a name="january-10-2017"></a>**10. ledna 2017**
Hello leden hostovaného operačního systému obsahuje opravy, které mají vliv jenom operační systém řady 2 (Windows 2008 Server R2). Proto vydala pouze bitovou kopii operačního systému rodiny 2 hello (WA-GUEST-operačního systému-2.59_201701-01) pro tohoto měsíce. Pro všechny ostatní rodin OS hello prosinec operačního systému (201612 - 01) zůstane hello nejnovější.


## <a name="releases"></a>Verze
## <a name="family-5-releases"></a>Uvolní rodiny 5
**V systému Windows Server 2016**

Nainstalované rozhraní .NET framework: 4.0, 4.5, 4.5.1, 4.5.2, 4.6, 4.6.1, 4.6.2

> [!NOTE]
> Data s * jsou toochange subjektu.
>
> Heslo RDP pro operační systém řady 5 Hello musí být minimálně 10 znaků.
>

| Konfigurační řetězec | Datum vydání | Zakázat datum | Vypršela platnost datum |
| --- | --- | --- | --- |
| WA-GUEST-OS-5.10_201708-01 |24 srpen 2017 |POST 5.12 |Bude doplněno |
| WA-GUEST-OS-5.9_201707-01 |3. srpna 2017 |POST 5,11 |Bude doplněno |
| WA-GUEST-OS-5.8_201706-01 |7. července 2017 |POST 5.10 |Bude doplněno |
|~~WA-GUEST-OS-5.7_201705-01~~ |5 června 2017 |24 srpen 2017 |Bude doplněno |
|~~WA-GUEST-OS-5.6_201704-01~~ |9 může 2017 |3. srpna 2017 |Bude doplněno |
|~~WA-GUEST-OS-5.5_201703-01~~ |10. dubna 2017 |7. července 2017 |Bude doplněno |
|~~WA-GUEST-OS-5.4_201612-01~~ |10. ledna 2017 |5 června 2017|Bude doplněno |
|~~WA-GUEST-OS-5.3_201611-01~~ |14. prosince 2016 |9 může 2017 |Bude doplněno |
|~~WA-GUEST-OS-5.2_201610-02~~ |1. listopadu 2016 |10. dubna 2017 |Bude doplněno |

## <a name="family-4-releases"></a>Uvolní rodiny 4
**Windows Server 2012 R2**

Podporuje rozhraní .NET 4.0, 4.5, 4.5.1, 4.5.2

> [!NOTE]
> Data s * jsou toochange subjektu
>
>

| Konfigurační řetězec | Datum vydání | Zakázat datum | Vypršela platnost datum |
| --- | --- | --- | --- |
| WA-GUEST-OS-4.45_201708-01 |24 srpen 2017 |POST 4.47 |Bude doplněno |
| WA-GUEST-OS-4.44_201707-01 |3. srpna 2017 |POST 4.46 |Bude doplněno |
| WA-GUEST-OS-4.43_201706-01 |7. července 2017 |POST 4.45 |Bude doplněno |
|~~WA-GUEST-OS-4.42_201705-01~~ |5 června 2017 |24 srpen 2017 |Bude doplněno |
|~~WA-GUEST-OS-4.41_201704-01~~ |9 může 2017 |3. srpna 2017 |Bude doplněno |
|~~WA-GUEST-OS-4.40_201703-01~~ |10. dubna 2017 |7. července 2017 |Bude doplněno |
|~~WA-GUEST-OS-4.39_201612-01~~ |10. ledna 2017 |5 června 2017 |Bude doplněno |
|~~WA-GUEST-OS-4.38_201611-01~~ |14. prosince 2016 |9 může 2017 |Bude doplněno |
|~~WA-GUEST-OS-4.37_201610-02~~ |16. listopadu 2016 |10. dubna 2017 |Bude doplněno |
|~~WA-GUEST-OS-4.36_201609-01~~ |13. října 2016 |14. ledna 2017 |Bude doplněno |
|~~WA-GUEST-OS-4.35_201608-01~~ |13 září 2016 |16 prosinec 2016 |Bude doplněno |
|~~WA-GUEST-OS-4.34_201607-01~~ |8. srpna 2016 |13 od listopadu 2016 |Bude doplněno |


## <a name="family-3-releases"></a>Uvolní rodiny 3
**Windows Server 2012**

Podporuje rozhraní .NET 4.0, 4.5, 4.5.1, 4.5.2

> [!NOTE]
> Data s * jsou toochange subjektu
>
>

| Konfigurační řetězec | Datum vydání | Zakázat datum | Vypršela platnost datum |
| --- | --- | --- | --- |
| WA-GUEST-OS-3.52_201708-01 |24 srpen 2017 |POST 3.54 |Bude doplněno |
| WA-GUEST-OS-3.51_201707-01 |3. srpna 2017 |POST 3.53 |Bude doplněno |
| WA-GUEST-OS-3.50_201706-01 |7. července 2017 |POST 3.52 |Bude doplněno |
|~~WA-GUEST-OS-3.49_201705-01~~ |5 června 2017 |24 srpen 2017 |Bude doplněno |
|~~WA-GUEST-OS-3.48_201704-01~~ |9 může 2017 |3. srpna 2017 |Bude doplněno |
|~~WA-GUEST-OS-3.47_201703-01~~ |10. dubna 2017 |7. července 2017 |Bude doplněno |
|~~WA-GUEST-OS-3.46_201612-01~~ |10. ledna 2017 |5 června 2017 |Bude doplněno |
|~~WA-GUEST-OS-3.45_201611-01~~ |14. prosince 2016 |9 může 2017 |Bude doplněno |
|~~WA-GUEST-OS-3.44_201610-02~~ |16. listopadu 2016 |1 pravděpodobně 2017 |Bude doplněno |
|~~WA-GUEST-OS-3.43_201609-01~~ |13. října 2016 |14. ledna 2017 |Bude doplněno |
|~~WA-GUEST-OS-3.42_201608-01~~ |13 září 2016 |16 prosinec 2016 |Bude doplněno |
|~~WA-GUEST-OS-3.41_201607-01~~ |8. srpna 2016 |13 od listopadu 2016 |Bude doplněno |


## <a name="family-2-releases"></a>Uvolní řady 2
**Windows Server 2008 R2 SP1**

Podporuje rozhraní .NET 3.5, 4.0, 4.5, 4.5.1, 4.5.2

> [!NOTE]
> Data s * jsou toochange subjektu
>
>

| Konfigurační řetězec | Datum vydání | Zakázat datum | Vypršela platnost datum |
| --- | --- | --- | --- |
| WA-GUEST-OS-2.65_201708-01 |24 srpen 2017 |POST 2.67 |Bude doplněno |
| WA-GUEST-OS-2.64_201707-01 |3. srpna 2017 |POST 2,66 |Bude doplněno |
| WA-GUEST-OS-2.63_201706-01 |7. července 2017 |POST 2.65 |Bude doplněno |
|~~WA-GUEST-OS-2.62_201705-01~~ |5 června 2017 |24 srpen 2017 |Bude doplněno |
|~~WA-GUEST-OS-2.61_201704-01~~ |9 může 2017 |3. srpna 2017 |Bude doplněno |
|~~WA-GUEST-OS-2.60_201703-01~~ |10. dubna 2017 |7. července 2017 |Bude doplněno |
|~~WA-GUEST-OS-2.59_201701-01~~ |10. ledna 2017 |5 června 2017 |Bude doplněno |
|~~WA-GUEST-OS-2.58_201612-01~~ |10. ledna 2017 |9 může 2017|Bude doplněno |
|~~WA-GUEST-OS-2.57_201611-01~~ |14. prosince 2016 |10. dubna 2017 |Bude doplněno |
|~~WA-GUEST-OS-2.56_201610-02~~ |16. listopadu 2016 |10. února 2017 |Bude doplněno |
|~~WA-GUEST-OS-2.55_201609-01~~ |13. října 2016 |14. ledna 2017 |Bude doplněno |
|~~WA-GUEST-OS-2.54_201608-01~~ |13 září 2016 |16 prosinec 2016 |Bude doplněno |
|~~WA-GUEST-OS-2.53_201607-01~~ |8. srpna 2016 |13 od listopadu 2016 |Bude doplněno |



## <a name="msrc-patch-updates"></a>Střediska MSRC oprava aktualizací
Hello seznam oprav, které jsou součástí jednotlivých měsíční verze hostovaného operačního systému je k dispozici [sem][patches].

## <a name="sdk-support"></a>Podpora v sadě SDK
I když hello [zásady vyřazení pro hello Azure SDK] [ retire policy sdk] označuje, že verze výše 2.2 jsou podporované, konkrétní pouze hostovaného operačního systému rodiny můžete toouse starší verze. Je třeba použít hello nejnovější podporované SDK.

| Skupina hostovaných operačních systémů | Verze kompatibilní sady SDK |
| --- | --- |
| 5 |Verze 2.9.5.1+ |
| 4 |Verze 2.1 + |
| 3 |Verze 1.8 + |
| 2 |Verze 1.3 + |
| 1 |Verze 1.0 + |

## <a name="guest-os-release-information"></a>Informace o verzi operačního systému hosta
Existují tři kalendářních dat, které jsou důležité uvolní tooGuest operačního systému: **verze** datum, **zakázáno** datum, a **vypršení platnosti** datum. Hostovaného operačního systému se považuje za k dispozici, pokud je v hello portál a lze vybrat jako cíl hello hostovaného operačního systému. Když hostovaného operačního systému dosáhne hello **zakázáno** datum, odebere se z Azure. Ale všechny cloudové služby cílení na tomto hostovaného operačního systému bude stále fungovat normálně.

okno Hello mezi hello **zakázáno** datum a hello **vypršení platnosti** datum vám poskytne vyrovnávací paměti tooeasily přechod z jednoho hostovaného operačního systému tooone novější. Pokud používáte *automatické* jako vaše hostovaného operačního systému, vždy budete mít na nejnovější verzi hello a nemáte tooworry o vypršení platnosti.

Když hello **vypršení platnosti** datum předává, všechny cloudové služby stále pomocí tohoto hostovaného operačního systému bude zastaven, odstraní nebo vynucené tooupgrade. Další informace o zásadách vyřazení hello [sem][retirepolicy].

## <a name="guest-os-family-version-explanation"></a>Vysvětlení rodiny verze operačního systému hosta
rodiny Hello hostovaného operačního systému jsou založeny na vydaná verze systému Microsoft Windows Server. Hello hostovaného operačního systému je hello příslušný operační systém spuštěný v Azure Cloud Services. Každý hostovaného operačního systému má rodiny, verzí a vydání číslo.

* **Rodina hostovaného operačního systému**  
  Verze operačního systému Windows Server, založený na hostovaného operačního systému. Například *rodiny 3* je založena na systému Windows Server 2012.
* **Verze operačního systému hosta**  
  Plus příslušné bitové kopie konkrétní tooa hostovaného operačního systému rodiny [Microsoft Security Response Center (MSRC)] [ msrc] vytváří opravy, které jsou k dispozici v hello datum hello nové hostovaného operačního systému verze. Ne všechny opravy mohou být zahrnuty.

    Čísla začínají hodnotou 0 a zvýší o 1 pokaždé, když se přidá novou sadu aktualizací. Koncové nuly se zobrazují pouze pokud je to důležité. To znamená že verze 2.10 je jiné, mnohem novější verze než verze 2.1.
* **Verze hostovaného operačního systému**  
  Opětovné vydání verze hostovaného operačního systému. O opětovné vydání v případě Microsoft vyhledá problémy při testování; nutnosti změny. Hello nejnovější verzi vždy nahrazuje všechny předchozí verze, veřejné nebo ne. Hello portál Azure umožní uživatelům pouze toopick hello nejnovější verze pro danou verzi. Nasazení na předchozí vydání spuštěna nejsou obvykle platnost upgradovat v závislosti na závažnosti hello hello chyb.

V příkladu hello níže 2 je rodina hello, 12 je hello verze a verze hello je "rel2".

**Verze hostovaného operačního systému** – 2.12 rel2

**Konfigurační řetězec pro tuto verzi** -WA-GUEST-operačního systému-2.12_201208-02

Hello konfigurační řetězec pro hostovaného operačního systému má stejné informace součástí, společně s datum zobrazení, které opravy střediska MSRC považovány za tuto verzi. V tomto příkladu byly střediska MSRC opravy vrácená pro Windows Server 2008 R2 si tooand včetně srpen 2012 rozhodnuto o zařazení. Jsou zahrnuty pouze opravy konkrétně použití toothat verzi systému Windows Server. Například pokud střediska MSRC oprava se vztahuje tooMicrosoft Office, nebude zahrnutý vzhledem k tomu, že produkt není součástí základní bitovou kopii systému Windows Server hello.

## <a name="guest-os-system-update-process"></a>Proces aktualizace systému operačního systému hosta
Tato stránka obsahuje informace o budoucích verzí hostovaného operačního systému. Označili zákazníci chtějí mít tooknow, když dojde k verze, protože jejich role cloudové služby se restartuje, pokud jsou nastavená příliš "automatické" aktualizace. Verze hostovaného operačního systému obvykle dojít alespoň pět (5) dnů po hello střediska MSRC vydání aktualizace, ke kterému dochází na hello druhé úterý v každém měsíci. Nové verze zahrnují všechny hello relevantní střediska MSRC opravy pro každou skupinu hostovaného operačního systému.

Microsoft Azure je neustále vydání aktualizace. Hello hostovaného operačního systému je jenom jedna taková aktualizace v kanálu hello. Verze může být ovlivněn mnoha faktorech příliš mnoho toolist sem. Kromě toho Azure je spuštěná na oznámena stovky tisíc počítačů. To znamená, že je možné toogive přesné datum a čas, kdy se restartuje vaše role. Nemůžeme práce na plán toolimit nebo čas restartování počítače.

Když novou verzi Dobrý den, kdy je publikována hostovaného operačního systému, může trvat dobu toofully rozšíří na Azure. Protože služby jsou aktualizované toohello nové hostovaného operačního systému, jsou restartovaný ctít zásady aktualizaci domény. Služby sady toouse "Automatické" aktualizace získají verze první. Po aktualizaci hello uvidíte, že nová verze hostovaného operačního systému hello uvedené pro vaši službu v hello portálu Azure. Opětovná vydání může dojít během této doby. Některé verze může být nasazena v delší a automatického upgradu restartování počítače nelze provádět mnoho týdny po datu vydání oficiální hello. Jakmile hostovaného operačního systému k dispozici, pak explicitně můžete tuto verzi z portálu hello nebo v konfiguračním souboru.

Značnou část cenné informace o restartování a ukazatele toomore informace technické podrobnosti Host a hostitelským operačním systémem aktualizací, najdete v blogu MSDN hello post s názvem [restartuje kvůli Role Instance upgrady tooOS] [ restarts].

Pokud ručně aktualizovat vaše hostovaného operačního systému, přečtěte si téma hello [zásady vyřazení hostovaného operačního systému] [ retirepolicy] Další informace.

## <a name="guest-os-supportability-and-retirement-policy"></a>Možnosti podpory hostovaného operačního systému a zásady vyřazení
Hello zásad možnosti podpory a vyřazení hostovaného operačního systému je vysvětlen [sem][retirepolicy].

[Install .NET on a Cloud Service Role]: https://azure.microsoft.com/en-us/documentation/articles/cloud-services-dotnet-install-dotnet/?WT.mc_id=azurebg_email_Trans_963_RevisedNET_Update
[Azure Guest OS Update Settings]: cloud-services-how-to-configure.md
[ssl3 announcement]: http://azure.microsoft.com/blog/2014/12/09/azure-security-ssl-3-0-update/
[Microsoft Security Advisory 3009008]: https://technet.microsoft.com/library/security/3009008.aspx
[ssl3-fixit]: http://go.microsoft.com/?linkid=9863266
[MS14-066]: https://technet.microsoft.com/library/security/ms14-066.aspx
[MS14-046]: https://technet.microsoft.com/library/security/ms14-046.aspx
[retire policy sdk]: https://msdn.microsoft.com/library/dn479282.aspx
[server and gos]: https://msdn.microsoft.com/library/dn775043.aspx
[azuresupport]: http://azure.microsoft.com/support/options/
[net install pkg]: http://www.microsoft.com/download/details.aspx?id=42643
[msrc]: http://www.microsoft.com/security/msrc/default.aspx
[update guest os portal]: https://msdn.microsoft.com/library/gg433101.aspx
[update guest os svc]: https://msdn.microsoft.com/library/gg456324.aspx
[restarts]: http://blogs.msdn.com/b/kwill/archive/2012/09/19/role-instance-restarts-due-to-os-upgrades.aspx
[patches]: cloud-services-guestos-msrc-releases.md
[retirepolicy]: cloud-services-guestos-retirement-policy.md
[fam1retire]: cloud-services-guestos-family1-retirement.md
[opravte]: https://technet.microsoft.com/en-us/library/security/ms17-010.aspx
