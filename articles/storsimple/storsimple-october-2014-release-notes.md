---
title: "aaaStorSimple 8000 aktualizovat 0,1 poznámky k verzi | Microsoft Docs"
description: "Popisuje hello nové funkce a opravy, otevřete problémy a k dispozici řešení pro hello října 2014 verze Microsoft Azure StorSimple (Update 0.1)."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: fd35e3c3-4770-460c-999d-f72ab7053a20
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 09/21/2016
ms.author: alkohli
ms.openlocfilehash: 684e31cb0b356ec59a9d6c245e5d2bc062cc4f0d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-8000-series-update-01-release-notes--october-2014"></a>Aktualizace řady StorSimple 8000 0,1 poznámky – říjen 2014
## <a name="overview"></a>Přehled
Následující poznámky k verzi Hello identifikovat hello kritické otevřete problémy pro aktualizaci řady StorSimple 8000 0,1 vydané v října 2014. Také obsahují seznam hello StorSimple softwaru a aktualizace firmwaru, které jsou součástí této verze. Toto je první verze hello po vydání řady StorSimple 8000 verze hello byl proveden v červenec 2014 všeobecně dostupná a odpovídá verzi toosoftware 6.3.9600.17312.  

Doporučujeme, abyste vyhledat a použít všechny dostupné aktualizace ihned po instalaci zařízení hello. Můžete také zapnout automatické aktualizace toodownload a nainstalovat důležité aktualizace od společnosti Microsoft při jejich vydání. Další informace najdete v tématu Jak příliš[aktualizace zařízení StorSimple](storsimple-update-device.md).  

Přečtěte si hello informace obsažené v hello poznámky k verzi, než nasadíte aktualizace hello v řešení StorSimple.  

> [!IMPORTANT]
> * Pomocí služby StorSimple Manager hello a není prostředí Windows PowerShell pro StorSimple tooinstall hello říjen aktualizací.  
> * aktualizace Hello trvají obvykle toocomplete o 3 hodiny.  
> * Říjen verzi zařízení StorSimple Hello neobsahuje žádné virtuální zařízení aktualizace toohello StorSimple. Můžete také použít všechny dostupné aktualizace systému Windows, včetně poslední opravy zabezpečení, ale neuvidíte změny ve verzi pro virtuální zařízení hello.  
> 
> 

Ujistěte se, zda text hello následující požadované součásti jsou nesplnění předchozí tooupdating zařízení StorSimple.  

* Ujistěte se, že oba řadiče zařízení běží před aktualizací. Pokud buď řadiče neběží, se nezdaří kontrola hello. tooverify, které hello řadiče jsou v dobrém stavu, přejděte příliš**stavu hardwaru** pod hello **údržby** stránky. Pokud jsou komponenty, **vyžadují pozornost**, obraťte se na Microsoft Support soubory protokolů.  
* Zajistěte, aby pevné IP adresy pro obě řadič 0 a řadič 1 jsou směrovatelné a můžete připojit toohello Internetu, které se používají pro obsluhu hello aktualizace toohello zařízení. Můžete použít hello [rutiny Test-Connection](https://technet.microsoft.com/library/hh849808.aspx) tooping známou adresu mimo hello sítě, jako je outlook.com, tooverify, který hello řadiče má toohello připojení mimo síť.  
* Zkontrolujte, zda že tento hello požadované porty pro odchozí spojení jsou k dispozici v zařízení StorSimple pro odchozí komunikaci. Další informace najdete v tématu hello [požadavcích sítě pro zařízení StorSimple](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device).  
* Pokud verze softwaru zařízení hello je starší než 6.3.9600.17312 (října 2014 aktualizace), zakažte hello Data 2 a Data 3 porty, pokud je povoleno, před spuštěním aktualizace hello. Pokud necháte hello Data 2 nebo Data 3 porty povolena při použití hello aktualizace, může to způsobit vaší toogo řadiče zařízení do režimu obnovení. Upozorňujeme, že když zakážete hello síťových rozhraní, všechny svazky hello přidružené bude převedeno do režimu offline a hello vstupně-výstupní operace se přeruší hello dobu aktualizace hello.  

## <a name="whats-new-in-hello-october-release"></a>Co je nového ve verzi říjen hello
Tato aktualizace zahrnuje hello následující vylepšení:

* Teď můžete použít toomanage uživatelského rozhraní služby StorSimple Manager hello řadičů zařízení. Hello správy, které zahrnují akce restartování, vypnutí, nebo zapnout řadiči. Další informace, přejděte příliš[řadiče zařízení StorSimple spravovat](storsimple-manage-device-controller.md).  
* Můžete naplánovat přidělení šířky pásma sítě WAN podle kombinace tooday v týdnu a čas během dne. To vám umožní toomake lepší využití šířky pásma sítě WAN mimo špičku. Různé šířky pásma šablony jsou povoleny pro kontejnery jiný svazek. Další informace, přejděte příliš[správu vašich šablon šířky pásma StorSimple](storsimple-manage-bandwidth-templates.md).  
* Můžete nakonfigurovat e-mailová oznámení tooproactively oznámit hello správci a jiné existující nebo které by mohly mít nadcházející problémy. Další informace, přejděte příliš[konfigurace nastavení výstrah](storsimple-manage-alerts.md#configure-alert-settings).  

## <a name="issues-fixed-in-hello-october-release"></a>Chyby v hello říjen verze
Hello následující tabulka obsahuje souhrn problémy, které jsme vyřešili v této aktualizaci.  

| Ne. | Funkce | Problém | Platí toophysical zařízení | Platí toovirtual zařízení |
| --- | --- | --- | --- | --- |
| 1 |Síťová rozhraní |V předchozích hello vydání, hello síťová rozhraní DATA 2 a DATA 3 měla vzájemně zaměněny v softwaru hello. Tato chyba byla opravena v této aktualizaci. Vymažte nastavení hello a před instalací aktualizace hello tato síťová rozhraní zakázat. Po instalaci aktualizace hello, budete mít tooreconfigure tato rozhraní. |Ano |Ne |
| 2 |Balíček pro podporu |V předchozí verzi hello, pokud jste spustili hello prostředí Windows PowerShell **Export HcsSupportPackage** protokoluje rutiny tooretrieve hello řadiče pro správu základní desky (BMC), hello operace se nezdařila s hello následující upozornění: "text hello Operace byla úspěšně dokončena tento řadič, ale na řadič sdílené hello z důvodu následující toohello se nezdařila, došlo k chybám. Prosím zkontrolujte, zda je v pořádku hello sdílené a jestli aktuální uzel hello můžou připojit sdílené toohello." Tento problém je teď vyřešený. |Ano |Ne |
| 3 |Převzetí služeb při selhání zařízení |V předchozí verzi hello se riziko nekonzistenci dat. Pokud **zjistit zálohování** úloha se nezdařila při selhání zařízení. Tento problém je teď vyřešený. |Ano |Ne |
| 4 |Převzetí služeb při selhání zařízení |V hello předchozí verzi, po selhání zařízení byly zálohy viditelné ale kontejneru svazků přidružené hello nebyly k dispozici na cílovém zařízení hello. Tento problém je teď vyřešený. |Ano |Ne |
| 5 |Převzetí služeb při selhání zařízení |V předchozí verzi hello se chyby ve výčtu hello záloh cloudu během operace obnovení registru hello, který může potenciálně vést toodata nekonzistence, pokud nebyly cloudu problémy s připojením. |Ano |Ne |
| 6 |Aktualizace firmwaru |V předchozí verzi hello hello úloha aktualizace firmwaru zařízení se nezdařila a zobrazí chybu, která uvádí, že rutiny hello nebyly rozpoznatelném, a že hello aktualizace se nezdařila v důsledku. Hello řadič pak přešel do režimu obnovení. Tento problém je teď vyřešený. |Ano |Ne |
| 7 |Instalace |Nyní bylo opraveno chyby způsobené hello zařízení není vytvoření bitové kopie správně během instalace. |Ano |Ne |
| 8 |Obnovení továrního nastavení |Nyní můžete volitelně Přeskočit kontrolu hello firmware pro obnovení továrního nastavení. Jedná se o změnu z předchozí verze hello. |Ano |Ne |
| 9 |Obnovení továrního nastavení |Při spuštění rutiny resetování objekt pro vytváření, v předchozí verzi hello provedeny kontroly verze firmwaru jenom pro některé součásti hardwaru. Po první restartování hello v procesu hello, což by mohlo způsobit hello resetování toofail byly provedeny další firmware kontroly. Tato oprava zajistí, že všechny kontroly firmware hello jsou provedeny při spuštění rutiny resetování hello objekt pro vytváření a před hello první systém restartovat. |Ano |Ne |
| 10 |Střídání klíče účtu úložiště |Hello **Invoke-HcsmServiceDataEncryptionKeyChange** rutinu použít klíče účtu úložiště hello toorotate teď výzvy hello uživatele tooenter hello služby datový šifrovací klíč. Jedná se o změnu z hello předchozí verze, ve které hello šifrovacího klíče dat služby byl předán jako parametr vložené. |Ano |Ne |
| 11 |Navrácení služeb po obnovení do 24 hodin |Během zotavení po havárii čištění hello na hello zdrojového zařízení nebyly provedeny toofail této aplikace, což způsobilo navrácení služeb po obnovení. Tato chyba byla opravena v této verzi. |Ano |Ne |

## <a name="known-issues-in-hello-october-release"></a>Známé problémy ve verzi říjen hello
Hello následující tabulka obsahuje souhrn známých problémů v této verzi.

| Ne. | Funkce | Problém | Komentáře nebo alternativní řešení | Platí toophysical zařízení | Platí toovirtual zařízení |
| --- | --- | --- | --- | --- | --- |
| 1 |Obnovení továrního nastavení |V některých případech při provádění obnovení továrního nastavení, hello zařízení StorSimple může být pomalé a zobrazit tato zpráva: **toofactory resetování probíhá (fáze 8)**. To se stane, když stisknutím CTRL + C, když probíhá hello rutiny. |Není stiskněte kombinaci kláves CTRL + C po inicializaci obnovení továrního nastavení. Pokud jste již v tomto stavu, kontaktujte prosím Microsoft Support pro další kroky. |Ano |Ne |
| 2 |Obnovení továrního nastavení |Proveďte není obnovení továrního nastavení zařízení StorSimple, která je aktualizována z verze GA tooOctober 2014. |Tato operace bude fungovat pouze pokud je nainstalovaná opravy. Obraťte se na tato požadovaná oprava tooget Microsoft Support. |Ano |Ne |
| 3 |Disk kvora |Ve výjimečných případech odpojení hello většinu disků ve skříni EBOD hello zařízení 8600 jsou výsledkem žádný disk kvora, pak hello fondu úložiště bude offline. Zůstane offline, i když jsou znovu připojeny disky hello. |Budete potřebovat tooreboot hello zařízení. Pokud hello potíže potrvají, kontaktujte prosím Microsoft Support pro další kroky. |Ano |Ne |
| 4 |Selhání snímku cloudu |Ve výjimečných případech může cloudový snímek nezdaří s chybou hello **zálohování dosažen limit maximální**. K tomu dojde, pokud být delší než 255 online klony na hello stejného zařízení, z hello stejné původního svazku, který byl odstraněn. | |Ano |Ano |
| 5 |Nesprávný řadiče ID |Když se provádí nahrazení řadiče, může řadič 0 zobrazí jako řadič 1. Při nahrazení řadiče při načtení hello bitovou kopii z uzlu sdílené hello, hello řadiče ID můžete zobrazují původně jako ID řadiče sdílené hello. Ve výjimečných případech může být toto chování vidět také po restartu systému. |Není třeba žádné akce uživatele. Tato situace bude automaticky vyřešen po dokončení nahrazení řadiče hello. |Ano |Ne |
| 6 |Monitorování grafy zařízení |V hello služby StorSimple Manager monitorování grafy hello zařízení nefungují, pokud základní nebo v konfiguraci proxy serveru pro zařízení hello hello je povolené ověřování NTLM. |Úprava konfigurace webového proxy serveru pro hello zařízení zaregistrovali služby StorSimple Manager tak, že ověřování nastavíte tooNONE hello. toodo se hello hello spuštění prostředí Windows PowerShell pro rutinu StorSimple Set-HcsWebProxy. |Ano |Ano |
| 7 |Účty úložiště |Použití účtu úložiště hello úložiště služby toodelete hello se o nepodporovaný scénář. To povede tooa situace, ve kterém nelze načíst data uživatele. | |Ano |Ano |
| 8 |Převzetí služeb při selhání zařízení |Více převzetí služeb kontejner svazků z hello stejný zdroj zařízení toodifferent cílové zařízení není podporován. |Převzetí služeb při selhání jednom zařízení neaktivní toomultiple zařízení bude hello kontejnery svazků na hello nejprve převzít služby při selhání zařízení dojít ke ztrátě dat vlastnictví. Po takové převzetí služeb při selhání, budou tyto kontejnery svazků zobrazí nebo chovat jinak při zobrazení v hello portál Azure classic. |Ano |Ne |
| 9 |Instalace |Během StorSimple adaptéru pro instalaci služby SharePoint je nutné tooprovide zařízení IP v pořadí pro toofinish hello instalace úspěšně. | |Ano |Ne |
| 10 |Webový proxy server |Pokud má vaše konfigurace webového proxy serveru HTTPS jako hello zadaný protokol, pak bude mít vliv komunikaci služby zařízení a hello zařízení přejde do režimu offline. Podpora balíčky se budou generovat také v procesu hello spotřebovávat značné množství prostředků vašeho zařízení. |Ujistěte se, že adresa URL proxy serveru webové hello má HTTP jako hello zadaný protokol. Další informace o příliš[konfigurace webového proxy serveru pro vaše zařízení](storsimple-configure-web-proxy.md). |Ano |Ne |
| 11 |Webový proxy server |Pokud nakonfigurujete a povolíte webový proxy server na registrované zařízení, bude potřebovat toorestart hello aktivního řadiče na vašem zařízení. | |Ano |Ne |
| 12 |Cloud vysokou latencí a vysokou vstupně-výstupní úlohy |Když v zařízení StorSimple dojde kombinaci cloudu velmi vysoké latenci (pořadí sekund) a vysoké vstupně-výstupní úlohy, svazky hello zařízení přejděte do sníženou stavu a hello vstupně-výstupních operací může selhat s chybou "zařízení není připraveno". |Budete potřebovat řadiče zařízení hello toomanually restartování nebo provést převzetí služeb při selhání toorecover zařízení z této situaci. |Ano |Ne |

## <a name="physical-device-updates-in-hello-october-release"></a>Aktualizace fyzického zařízení ve verzi říjen hello
Když tyto aktualizace jsou použité tooa fyzické zařízení, se změní verze softwaru hello too6.3.9600.17312. Pokud není uvedeno jinak, tyto poznámky k verzi platí tooall modely hello zařízení StorSimple. Další informace o těchto aktualizací najdete v tématu [aktualizace softwaru fyzického zařízení října 2014 pro Microsoft Azure StorSimple zařízení](http://support.microsoft.com/kb/2986997).  

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-hello-october-release"></a>Serial-attached SCSI (SAS) řadiče a aktualizace firmwaru v říjnu verzi hello
Tato verze aktualizuje hello ovladače a firmware hello na hello SAS ve fyzického zařízení. Další informace o aktualizaci řadiče hello SAS najdete v tématu [aktualizace – říjen 2014 pro řadičů LSI SAS v Microsoft Azure StorSimple zařízení](http://support.microsoft.com/kb/2987020).   

Tato verze se týká také firmware kumulativní aktualizace, která řeší problémy se spolehlivosti s hello zařízení hardwarové součásti. Další informace o aktualizaci firmwaru hello najdete v tématu [aktualizaci firmwaru října 2014 pro Microsoft Azure StorSimple zařízení](http://support.microsoft.com/kb/2987015).  

## <a name="virtual-device-updates-in-hello-october-release"></a>Aktualizace virtuální zařízení ve verzi říjen hello
Tato verze neobsahuje žádné aktualizace pro virtuální zařízení hello. Použití této aktualizace se nezmění hello softwaru verzi virtuálního zařízení.

