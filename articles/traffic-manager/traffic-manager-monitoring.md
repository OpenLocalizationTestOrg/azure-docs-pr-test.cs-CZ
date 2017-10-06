---
title: "monitorování koncového bodu aaaAzure Traffic Manageru | Microsoft Docs"
description: "Tento článek vám může pomoct pochopit, jak Traffic Manager používá monitorování koncového bodu a koncový bod automatické převzetí služeb při selhání toohelp Azure zákazníků nasazení aplikací s vysokou dostupností"
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: fff25ac3-d13a-4af9-8916-7c72e3d64bc7
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/22/2017
ms.author: kumud
ms.openlocfilehash: b4862499c88bdb1951833d06199b034a07ac7576
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="traffic-manager-endpoint-monitoring"></a>Monitorování koncového bodu Traffic Manageru

Azure Traffic Manager zahrnuje monitorování vestavěným koncovým bodem a koncový bod automatické převzetí služeb při selhání. Tato funkce umožňuje poskytovat vysokou dostupnost aplikace, které jsou odolné tooendpoint selhání, včetně selhání oblast Azure.

## <a name="configure-endpoint-monitoring"></a>Konfigurace monitorování koncového bodu

tooconfigure monitorování koncového bodu, je nutné zadat hello následující nastavení na svůj profil Traffic Manageru:

* **Protokol**. Vyberte protokol HTTP, HTTPS nebo TCP jako hello protokol, který Traffic Manager používá k zjišťování toocheck váš koncový bod jeho stav. Monitorování HTTPS neověřuje, zda váš certifikát SSL není platný – pouze zkontroluje tento hello certifikát je k dispozici.
* **Port**. Zvolte hello port používaný pro požadavek hello.
* **Cesta**. Toto nastavení konfigurace je platná pouze pro protokoly HTTP a HTTPS hello, u kterých se vyžaduje zadání nastavení cesta hello. Poskytuje toto nastavení pro hello TCP monitorování protokolu dojde k chybě. Pro protokol TCP dejte hello relativní cesta a název hello hello webovou stránku nebo soubor hello vytvořit tento hello monitorování přístupů. Lomítko (/) je zadána platná hodnota relativní cesty hello. Tato hodnota znamená, že soubor hello je v kořenovém adresáři hello (výchozí).
* **Zkušební fáze Interval**. Tato hodnota určuje, jak často se kontroluje koncový bod na jeho stav od agentů testování Traffic Manager. Můžete zadat sem dvě hodnoty: 30 sekund (normální zjišťování) a 10 sekund (rychlé zkušební fáze). Pokud jsou k dispozici žádné hodnoty, nastaví profil hello tooa výchozí hodnotu 30 sekund. Navštivte hello [Traffic Manager cenová](https://azure.microsoft.com/pricing/details/traffic-manager) toolearn stránky, další informace o rychlé testování ceny.
* **Počet selhání tolerovat**. Tato hodnota určuje, kolik selhání agenta testování Traffic Manager toleruje před označením tohoto koncového bodu jako chybné. Jeho hodnota může být v rozsahu od 0 do 9. Hodnota 0 znamená selhání jedné monitorování může způsobit tento koncový bod toobe označen jako chybný. Pokud není zadaná žádná hodnota, použije hello výchozí hodnotu 3.
* **Časový limit monitorování**. Tato vlastnost určuje hello množství času hello Traffic Manager testování agenta má čekat před vzhledem k tomu, který zkontrolujte selhání při sondu kontroly stavu odeslání toohello koncový bod. Pokud hello zjišťování Interval se nastavuje too30 sekund a pak můžete nastavit hodnotu časového limitu hello od 5 do 10 sekund. Pokud není zadaná žádná hodnota, použije se výchozí hodnota je 10 sekund. Pokud hello zjišťování Interval se nastavuje too10 sekund a pak můžete nastavit hodnotu časového limitu hello od 5 do 9 sekund. Pokud není zadaná žádná hodnota časového limitu, používá výchozí hodnotu 9 sekund.

![Monitorování koncového bodu Traffic Manageru](./media/traffic-manager-monitoring/endpoint-monitoring-settings.png)

**Obrázek 1: Traffic Manager koncového bodu monitorování**

## <a name="how-endpoint-monitoring-works"></a>Jak funguje monitorování koncového bodu

Pokud se hello monitorování protokol nastaven jako HTTP nebo HTTPS, testování agenta hello Traffic Manager provede koncového bodu GET požadavek toohello pomocí hello protokol, port a relativní cesta zadána. Získá zpět odpověď 200 – OK, že koncový bod se považuje v pořádku. Pokud je odpověď hello jinou hodnotu nebo, pokud není žádná odpověď v rámci hello časový limit zadaný, pak hello zjišťování agenta Traffic Manager se pokusí opětovně navázat podle nastavení tolerovat počet selhání toohello (znovu pokusí se provést v případě, toto nastavení je 0) . Pokud je vyšší než nastavení tolerovat počet selhání hello hello počet po sobě jdoucích selhání, se označí není v pořádku tohoto koncového bodu. 

Pokud hello monitorování protokolu TCP, hello Traffic Manager testování agent iniciuje žádost o připojení protokolu TCP pomocí hello zadaný port. Pokud koncový bod hello odpoví toohello požadavek odpovědi tooestablish hello připojení, že kontrola stavu je označena jako úspěšné a testování agenta hello Traffic Manager obnoví připojení TCP hello. Pokud je odpověď hello jinou hodnotu, nebo pokud není žádná odpověď v rámci hello časový limit zadaný, hello zjišťování agenta Traffic Manager se pokusí opětovně navázat podle nastavení tolerovat počet selhání toohello (znovu pokusí probíhají Pokud toto nastavení je 0). Pokud hello počet po sobě jdoucích selhání je vyšší než nastavení hello tolerovat počet selhání, se označí není v pořádku tohoto koncového bodu.

Ve všech případech Traffic Manager sondy z více umístění a se stane hello po sobě jdoucích selhání rozhodnutí v každé oblasti. Také to znamená, že koncových bodů obdrželi sondy stavu z Traffic Manager s frekvencí vyšší než nastavení hello používá pro zjišťování Interval.

>[!NOTE]
>Běžnou praxí na straně hello koncový bod pro HTTP nebo HTTPS protokol pro sledování, je tooimplement vlastní stránky v rámci vaší aplikace – například /health.aspx. Pomocí této cesty pro monitorování, můžete provést kontroly specifické pro aplikace, jako je kontrola čítače výkonu a ověřování databáze dostupnosti. Podle těchto vlastní kontroly, vrátí stránku hello odpovídající stavový kód HTTP.

Všechny koncové body v profilu Traffic Manageru sdílet nastavení monitorování. Pokud potřebujete toouse různých nastavení monitorování pro různými koncovými body, můžete vytvořit [vnořené profily Traffic Manageru](traffic-manager-nested-profiles.md#example-5-per-endpoint-monitoring-settings).

## <a name="endpoint-and-profile-status"></a>Stav koncového bodu a profilu

Můžete povolit nebo zakázat koncové body a profily Traffic Manageru. Však ke změně stavu koncových bodů také může docházet k výsledku Traffic Manager automatizované procesy a nastavení.

### <a name="endpoint-status"></a>Stav koncového bodu

Můžete povolit nebo zakázat konkrétní koncový bod. Základní služba Hello, která nemusí být ještě v pořádku, je poškozena. Ovládací prvky stavového koncový bod změna hello hello dostupnost hello koncového bodu v hello profil služby Traffic Manager. Pokud je koncový bod stav Zakázáno, Traffic Manager nekontroluje, jeho stav a hello koncový bod není zahrnutý v odpovědi DNS.

### <a name="profile-status"></a>Stav profilu

Nastavení stavu hello profilu můžete povolit nebo zakázat konkrétní profil. Při stav koncového bodu má vliv na jeden koncový bod, ovlivní stav profilu hello celého profilu, včetně všechny koncové body. Pokud zakážete profil, hello koncových bodů nejsou kontroly stavu a žádné koncové body jsou zahrnuty v odpovědi DNS. [NXDOMAIN](https://tools.ietf.org/html/rfc2308) pro dotaz DNS hello je vrácen kód odpovědi.

### <a name="endpoint-monitor-status"></a>Stav monitorování koncových bodů

Stav monitorování koncového bodu je generovaný Traffic Manager hodnotu, která se zobrazuje stav hello hello koncového bodu. Nelze změnit, tato nastavení ručně. stav monitorování koncového bodu Hello je kombinací hello výsledky monitorování koncového bodu a hello stav nakonfigurovaný koncový bod. v následující tabulce hello jsou uvedeny možné hodnoty Hello koncového bodu monitorování stavu:

| Stav profilu | Stav koncového bodu | Stav monitorování koncových bodů | Poznámky |
| --- | --- | --- | --- |
| Zakázáno |Povoleno |Neaktivní |Hello profilu bylo zakázáno. I když hello koncový bod stav povoleno, stav profilu hello (zakázáno) má přednost před. Koncové body v zakázaném profily nejsou monitorovány. Zadání kódu odpovědi NXDOMAIN se vrátí pro dotaz DNS hello. |
| &lt;všechny&gt; |Zakázáno |Zakázáno |koncový bod Hello byla zakázána. Zakázané koncových bodů nejsou monitorovány. koncový bod Hello není zahrnutý v odpovědi DNS, proto, že neobdrží provoz. |
| Povoleno |Povoleno |Online |koncový bod Hello je monitorována a je v pořádku. To je součástí odpovědí DNS a může přijímat provoz. |
| Povoleno |Povoleno |Snížený výkon |Monitorování kontroly stavu koncový bod se nedaří. koncový bod Hello není zahrnutý v odpovědi DNS a nepřijímá provoz. <br>"K výjimce toothis je, pokud jsou degradovány všechny koncové body, v takovém případě všechny z nich jsou považovány za toobe vrácený v odpovědi na dotaz hello).</br>|
| Povoleno |Povoleno |CheckingEndpoint |koncový bod Hello je monitorována, ale ještě nebyly přijaty hello výsledky první sondu hello. CheckingEndpoint je dočasný stav, ke kterému dochází obvykle okamžitě po přidání nebo povolení koncového bodu v profilu hello. Koncový bod v tomto stavu je zahrnutý v odpovědi DNS a může přijímat provoz. |
| Povoleno |Povoleno |Zastaveno |Hello cloudové služby nebo webovou aplikaci, která hello toois body koncový bod není spuštěna. Zkontrolujte hello cloudové služby nebo webovou aplikaci nastavení. To také může dojít, pokud je koncový bod hello zadejte vnořené koncový bod a hello podřízené profilu je zakázán nebo je neaktivní. <br>Koncový bod ve stavu Zastaveno není monitorován. To není zahrnutý v odpovědi DNS a neobdrží provoz. K výjimce toothis je, pokud jsou degradovány všechny koncové body, v takovém případě všechny z nich bude považovat za toobe vrácený v odpovědi na dotaz hello.</br>|

Podrobnosti o výpočtu stav monitorování koncových bodů pro vnořené koncové body najdete v tématu [vnořené profily Traffic Manageru](traffic-manager-nested-profiles.md).

### <a name="profile-monitor-status"></a>Stav monitorování profilu

stav monitorování profilu Hello je kombinací hello nakonfigurované stav profilu a hello koncového bodu monitorování stavu hodnoty pro všechny koncové body. Hello možné hodnoty jsou popsané v následující tabulce hello:

| Stav profilu (jak je nakonfigurováno) | Stav monitorování koncových bodů | Stav monitorování profilu | Poznámky |
| --- | --- | --- | --- |
| Zakázáno |&lt;všechny&gt; nebo profil s žádné koncové body definované. |Zakázáno |Hello profilu bylo zakázáno. |
| Povoleno |je Degradovaný stav Hello aspoň jeden koncový bod. |Snížený výkon |Zkontrolujte hello jednotlivých koncový bod stav hodnoty toodetermine které koncové body vyžadovat další pozornost. |
| Povoleno |Stav Hello aspoň jeden koncový bod je Online. Žádné koncové body mají stav snížený. |Online |Služba Hello přijímá provoz. Není vyžadována žádná další akce. |
| Povoleno |Stav Hello aspoň jeden koncový bod je CheckingEndpoint. Ve stavu Online nebo snížený nejsou žádné koncové body. |CheckingEndpoints |Tento přechod stavu nastane, když profil Pokud vytvořené nebo povolené. Stav koncového bodu Hello se kontroluje pro hello poprvé. |
| Povoleno |Hello stavy všech koncových bodů v profilu hello je zakázán nebo zastaven nebo profil hello nemá žádné koncové body definované. |Neaktivní |Žádné koncové body jsou aktivní, ale profil hello je stále povolené. |

## <a name="endpoint-failover-and-recovery"></a>Koncový bod převzetí služeb při selhání a obnovení

Traffic Manager pravidelně kontroluje stav hello každý koncový bod, včetně koncových bodů není v pořádku. Traffic Manager zjistí, že koncový bod se změní na v pořádku a přenese zpět do otočení.

Koncový bod je není v pořádku, pokud dojde k některé z hello následující události:
- Je-li hello monitorování protokolu HTTP nebo HTTPS:
    - Bez 200 odezvu obdrží (včetně kódu různých 2xx nebo 301/302 přesměrování).
- Je-li hello monitorování protokolu TCP: 
    - Odpověď jiného, než ACK nebo SYN ACK byl přijat v odpovědi žádost o SYNCHRONIZACI toohello odesílá Traffic Manager tooattempt navázání připojení.
- Časový limit. 
- Všechny ostatní připojení problém, což vede k hello koncový bod není právě dostupná.

Další informace o odstraňování potíží selhání kontroly najdete v tématu [řešení potíží s Degradovaný stav na Azure Traffic Manager](traffic-manager-troubleshooting-degraded.md). 

Hello následující časová osa na obrázku 2 je podrobný popis hello sledování, zpracovávají koncový bod Traffic Manager, který má hello následující nastavení: monitorování protokolu HTTP, testování interval je 30 sekund se počet. povolená selhání 3, hodnota časového limitu je 10 sekund a DNS TTL je 30 sekund.

![Traffic Manager pořadí převzetí služeb při selhání a navrácení služeb po obnovení koncového bodu](./media/traffic-manager-monitoring/timeline.png)

**Obrázek 2: Provoz manager koncový bod převzetí služeb při selhání a obnovení pořadí**

1. **ZÍSKAT**. Pro každý koncový bod hello monitorování systému služby Traffic Manager provede požadavek GET na hello cesta zadaná v nastavení sledování hello.
2. **200 OK**. Hello monitorování systému očekává toobe zpráva HTTP 200 OK vrátila v rámci 10 sekund. Při přijetí této odpovědi, rozpozná, že služba hello je k dispozici.
3. **30 sekundách mezi kontrolami**. Kontrola stavu Hello koncový bod se opakuje každých 30 sekund.
4. **Služba není k dispozici**. Služba Hello nedostupný. Traffic Manager nebude vědět, dokud hello další kontrolu stavu.
5. **Pokusy o tooaccess hello monitorování cesta**. provede požadavek GET Hello monitorování systému, ale neobdrží odpověď v době vymezené hello 10 sekund (případně jiný 200 může být přijata odpověď). Zkusí další třikrát v intervalech 30 sekund. Pokud jeden z pokusů hello je úspěšné, je resetovat hello počet pokusů.
6. **Nastavit stav tooDegraded**. Po čtvrtém po sobě jdoucích selhání označí hello systému pro monitorování stavu hello koncový bod není k dispozici jako Snížený.
7. **Přenosy jsou koncové body odkloněných tooother**. Hello názvových serverů DNS Traffic Manager se aktualizují a Traffic Manager už vrátí hello koncového bodu v dotazech tooDNS odpovědi. Nové připojení jsou řízené tooother koncové body k dispozici. Rekurzivní servery DNS a klienty DNS však může stále mezipaměti předchozí odpovědí DNS, které obsahují tento koncový bod. Klienti dál toouse hello endpoint do vypršení platnosti hello mezipaměť DNS. Jako hello mezipaměť DNS vyprší, klienti zkontrolujte nové dotazy DNS a jsou řízené toodifferent koncové body. Doba trvání mezipaměti Hello je řízena nastavením TTL hello hello profil služby Traffic Manager, například 30 sekund.
8. **Kontroluje stav pokračovat**. Traffic Manager pokračuje toocheck hello stavu hello koncového bodu, když má stav snížený. Traffic Manager zjistí, když se koncový bod hello vrátí toohealth.
9. **Služba přejde do režimu online**. Hello služby k dispozici. koncový bod Hello zachovává jeho stav snížený v Traffic Manageru, dokud hello monitorování systému, provede jeho další kontrolu stavu.
10. **Obnoví provoz tooservice**. Správce provozu se odešle požadavek GET a obdrží odpověď 200 OK stav. Hello služba vrátila tooa stavu v pořádku. Hello Traffic Manager názvové servery jsou aktualizovány a jejich začít toohand na název DNS služby hello v odpovědi DNS. Provoz vrátí koncový bod toohello jako vyprší odpovědí uložených v mezipaměti DNS, které vracejí další koncové body a jako existující připojení jsou koncové body tooother ukončena.

    > [!NOTE]
    > Protože Traffic Manager funguje v hello úroveň DNS, nelze ovlivnit existující připojení tooany koncový bod. Když ho přesměruje přenosy mezi koncovými body (buď nastavení změněné profilu, nebo při převzetí služeb při selhání a navrácení služeb po obnovení), přesměruje Traffic Manager nové koncové body tooavailable připojení. Ostatní koncové body, ale může pokračovat tooreceive provoz prostřednictvím připojení k existující, dokud tyto relace jsou ukončeny. tooenable toodrain provoz z existující připojení, aplikace měli omezit hello dobu trvání relace použít s každý koncový bod.

## <a name="traffic-routing-methods"></a>Metody směrování provozu

Koncový bod má stav snížený, je už vrátila v dotazech tooDNS odpovědi. Místo toho je alternativní koncový bod vybrali a vrácena. směrování provozu metoda Hello, který je nakonfigurovaný v profilu hello Určuje, jak je zvolen hello alternativní koncový bod.

* **Priorita**. Koncové body představují seznam seřazený podle priority. Vrátí se vždy Hello první dostupné koncovým bodem na seznamu hello. Pokud je Degradovaný stav koncový bod, je vrácena hello další dostupný koncový bod.
* **Vážené**. Žádný dostupný koncový bod je vybrán náhodně podle jejich váhy přiřazené a hello váhu hello další koncové body k dispozici.
* **Výkon**. Vrátí se Hello koncový bod nejbližší toohello koncového uživatele. Pokud tohoto koncového bodu není k dispozici, koncový bod náhodně vybere z všechny hello další koncové body k dispozici. Koncový bod náhodný výběr zabraňuje kaskádových selhání, který může nastat, když se stane přetížené hello nejbližší koncového bodu. Plány alternativní převzetí služeb při selhání pro směrování provozu výkonu můžete nakonfigurovat pomocí [vnořené profily Traffic Manageru](traffic-manager-nested-profiles.md#example-4-controlling-performance-traffic-routing-between-multiple-endpoints-in-the-same-region).
* **Zeměpisná**. koncový bod Hello mapovat tooserve hello geografické umístění na základě požadavku hello dotaz, je vrácen IP adresy. Pokud tohoto koncového bodu není k dispozici, jiný koncový bod nebude vybrané toofailover, protože geografické umístění lze mapovat pouze endpoint tooone v profilu (Další podrobnosti najdete v hello [– nejčastější dotazy](traffic-manager-FAQs.md#traffic-manager-geographic-traffic-routing-method)). Jako osvědčený postup, při použití geografické směrování doporučujeme zákazníkům profily Traffic Manageru toouse vnořené s více než jeden koncový bod jako koncové body hello hello profilu.

Další informace najdete v tématu [metody směrování provozu Traffic Manager](traffic-manager-routing-methods.md).

> [!NOTE]
> Jedna výjimka toonormal směrování provozu chování dochází, pokud všechny vhodné koncové body degradovaném stavu. Díky Traffic Manager pokusí "best effort" a *odpoví, jako by všechny hello snížený stav koncové body ve skutečnosti ve stavu online*. Toto chování je vhodnější toohello alternativní, který by byl toonot vrátí žádný koncový bod v hello odpověď DNS. Nejsou monitorovány zakázán nebo zastaven koncových bodů, proto nejsou považovány za vhodné pro provoz.
>
> Tuto podmínku běžně dochází nesprávná konfigurace hello služby, jako například:
>
> * Seznam řízení přístupu [ACL] blokování kontroly stavu hello Traffic Manager.
> * Nesprávná konfigurace hello monitorování portu nebo protokolu v hello profil správce provozu.
>
> Hello důsledků toto chování je, že pokud kontroly stavu Traffic Manager nejsou správně nakonfigurována, může se zdát, od hello provozu, jako když směrování Traffic Manager *je* funguje správně. Však v takovém případě převzetí služeb při selhání koncový bod nemůže dojít, které ovlivní celkové aplikace k dispozici. Je důležité toocheck, že profil hello zobrazuje Online stavu, není stav snížený. Online stav označuje, že hello Traffic Manager kontroluje stav fungují podle očekávání.

Další informace o řešení potíží se nezdařilo kontroly stavu, najdete v části [řešení potíží s Degradovaný stav na Azure Traffic Manager](traffic-manager-troubleshooting-degraded.md).



## <a name="next-steps"></a>Další kroky

Další informace [fungování Traffic Manager](traffic-manager-how-traffic-manager-works.md)

Další informace o hello [metody směrování provozu](traffic-manager-routing-methods.md) podporované nástrojem Traffic Manager

Zjistěte, jak příliš[vytvořit profil správce provozu](traffic-manager-manage-profiles.md)

[Řešení potíží s stav snížený](traffic-manager-troubleshooting-degraded.md) na koncový bod Traffic Manager
