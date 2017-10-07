---
title: "Poznámky k verzi 1.2 aktualizace řady aaaStorSimple 8000 | Microsoft Docs"
description: "Popisuje nové funkce hello, problémy a řešení pro zařízení StorSimple 8000 řady aktualizace 1.2."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 6c9aae87-6f77-44b8-b7fa-ebbdc9d8517c
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7f564b794573fc3302ab15732e8dd85632ab9243
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="update-12-release-notes-for-your-storsimple-8000-series-device"></a>Aktualizovat 1.2 poznámky k verzi pro vaše zařízení řady StorSimple 8000

## <a name="overview"></a>Přehled
Hello následující poznámky k verzi popisují hello nové funkce a identifikovat hello kritické otevřené problémy pro StorSimple 8000 řady aktualizace 1.2. Také obsahují seznam hello StorSimple softwaru, ovladačů a aktualizace firmwaru disku součástí této verze. 

Aktualizace 1.2 může být zařízení StorSimple použité tooany systémem verze (GA), Update 0.1, aktualizace 0,2 nebo Update 0.3 softwaru. Aktualizace 1.2 není k dispozici, pokud vaše zařízení používá Update 1 nebo Update 1.1. Pokud vaše zařízení používá verzi (GA), [kontaktovat Microsoft Support](storsimple-contact-microsoft-support.md) tooassist k instalaci této aktualizace.

Následující seznamy hello zařízení software verze tabulky odpovídající tooUpdates 1, 1.1 a 1.2 Hello.

| Pokud systém aktualizace... | Toto je vaše zařízení verze softwaru. |
| --- | --- |
| Aktualizace 1.2 |6.3.9600.17584 |
| Aktualizace 1.1 |6.3.9600.17521 |
| Aktualizace 1.0 |6.3.9600.17491 |

Přečtěte si hello informace obsažené v uvolněte hello poznámky před nasazením hello aktualizovat v řešení StorSimple. Další informace najdete v tématu Jak příliš[nainstalovat 1.2 aktualizace zařízení StorSimple](storsimple-install-update-1.md). 

> [!IMPORTANT]
> * Tato aktualizace (včetně aktualizací Windows hello) trvá přibližně 5 až 10 hodin tooinstall. 
> * Aktualizace 1.2 má softwaru, LSI ovladače a aktualizace firmwaru disku. tooinstall, postupujte podle pokynů hello v [nainstalovat 1.2 aktualizace zařízení StorSimple](storsimple-install-update-1.md).
> * Pro nové verze, nemusí zobrazit aktualizace okamžitě, protože provedeme postupné zavádění hello aktualizací. Kontrola aktualizací za pár dní znovu jako tyto brzy bude dostupná.
> 
> 

## <a name="whats-new-in-update-12"></a>Co je nového v aktualizaci 1.2
Tyto funkce byly prvního vydání s Update 1, která byla učiněna k dispozici tooa omezenou sadu uživatelů. Verze hello 1.2 aktualizace je většina uživatelů StorSimple hello zobrazen hello následující nové funkce a vylepšení:

* **Migrace ze zařízení řady too8000 řady 5000 7000** – tato verze zavádí novou funkci migrace, která umožňuje hello StorSimple toomigrate uživatelé zařízení řady 5000 7000 fyzického zařízení jejich data tooa StorSimple 8000 řady nebo virtuální zařízení. funkce migrace Hello má dvě klíčové funkce:                                                                  
  
  * **Kontinuita podnikových procesů**, povolením migrace existujících dat na 5000 7000 řady zařízení too8000 řady zařízení.
  * **Vylepšené funkce nabídky hello 8000 řady zařízení**, jako je například efektivní centralizovaná správa více zařízení prostřednictvím služby StorSimple Manager, lepší třída hardwaru a aktualizovat firmware, virtuální zařízení, data mobility Funkce a v budoucích plán hello.
    
    Odkazovat toohello [příručka k migraci](http://www.microsoft.com/download/details.aspx?id=47322) podrobnosti o tom toomigrate zařízení řady StorSimple 5000 7000 řady tooan 8000. 
* **Dostupnost v hello portálu Azure Government** – StorSimple je nyní k dispozici na portálu Azure Government hello. V tématu Jak příliš[nasazení zařízení StorSimple v hello portálu Azure Government](storsimple-deployment-walkthrough-gov.md).
* **Podpora pro ostatní poskytovatele cloudových služeb** – hello ostatní poskytovatele cloudových služeb podporované jsou Amazon S3, Amazon S3 s RRS, HP a OpenStack (beta).
* **Aktualizovat toolatest rozhraní API úložiště** – v této verzi se StorSimple byl aktualizován služby Azure Storage nejnovější toohello rozhraní API. Řadu zařízení StorSimple 8000 s verzí softwaru před aktualizací 1 (verze 0.1, 0.2 a 0.3) používají verze hello starší než 17 července 2009 API služby úložiště Azure. Jak je uvedeno v hello aktualizovat [oznámení o odebrání verze služby úložiště](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/10/19/microsoft-azure-storage-service-version-removal-update-extension-to-2016.aspx), podle 1 srpna 2016, bude tato rozhraní API zastaralá. Je nutné použít předchozí tooAugust hello StorSimple 8000 řady Update 1 1, 2016. Pokud jsou selhání toodo tak, zařízení StorSimple přestane fungovat správně.
* **Podpora pro zóny redundantní úložiště (ZRS)** – hello upgradu toohello nejnovější verzi hello rozhraní API úložiště, bude podporovat řady StorSimple 8000 hello zóny redundantní úložiště (ZRS) v přidání tooLocally redundantní úložiště (LRS) a geograficky redundantní Úložiště (GRS). Odkazovat toothis [článek na možnosti redundance úložiště Azure](../storage/common/storage-redundancy.md) podrobnosti ZRS.
* **Rozšířené počáteční nasazení a aktualizace prostředí** – v této verzi hello instalace a aktualizace procesy vylepšily. instalace Hello prostřednictvím Průvodce instalací hello je lepší tooprovide zpětné vazby toohello uživatele, pokud hello nastavení konfigurace a brány firewall sítě jsou nesprávné. Další diagnostiky rutiny byly zadány toohelp k řešení potíží s sítě hello zařízení. V tématu hello [řešení potíží s nasazení článku](storsimple-troubleshoot-deployment.md) Další informace o nové diagnostické rutiny hello používá pro řešení potíží.

## <a name="issues-fixed-in-update-12"></a>Chyby v aktualizaci 1.2
Hello následující tabulka obsahuje souhrn problémy, které jsme vyřešili v aktualizace 1.2, 1.1 a 1.    

| Ne. | Funkce | Problém | Pevné v aktualizaci | Platí toophysical zařízení | Platí toovirtual zařízení |
| --- | --- | --- | --- | --- | --- |
| 1 |Prostředí Windows PowerShell pro StorSimple |Pokud je uživatel zařízení StorSimple hello vzdálený přístup pomocí Windows Powershellu pro StorSimple a pak spustit Průvodce instalací hello, došlo k co nejdříve Data 0 byl vstup IP havárie. Tato chyba je nyní opravena v aktualizaci 1. |Update 1 |Ano |Ano |
| 2 |Obnovení továrního nastavení |V některých případech, pokud jste provedli obnovení továrního nastavení, zařízení StorSimple hello se aktivovala zablokuje a zobrazí tato zpráva: **toofactory resetování probíhá (fáze 8)**. To došlo k během provádění rutiny hello stisknutím CTRL + C. Tato chyba je nyní opravena. |Update 1 |Ano |Ne |
| 3 |Obnovení továrního nastavení |Po objekt factory kontroleru selhání duální resetovat, byly povoleny tooproceed s registrací zařízení. Výsledkem nepodporované konfiguraci systému. V Update 1 se zobrazí chybová zpráva a registrace blokováno na zařízení, že má neúspěšné obnovit tovární nastavení. |Update 1 |Ano |Ne |
| 4 |Obnovení továrního nastavení |V některých případech byly vyvolány false kladné neshoda výstrahy. Nesprávný neshoda výstrahy se budou generovat už na zařízení se systémem Update 1. |Update 1 |Ano |Ne |
| 5 |Obnovení továrního nastavení |Pokud byl přerušen obnovení továrního nastavení předchozí toocompletion hello režimu obnovení zařízení zadali a neumožňuje vám tooaccess Windows Powershellu pro StorSimple. Tato chyba je nyní opravena. |Update 1 |Ano |Ne |
| 6 |Zotavení po havárii |Chyby zotavení po havárii byla opravena, ve kterém by zotavení po Havárii selhala při zjišťování hello záloh v hello cílové zařízení. |Update 1 |Ano |Ano |
| 7 |Monitorování LED |V některých případech monitorování LED na hello zadní zařízení neobsahoval pokyn správný stav. byl DIODU Hello modrá vypnuté. DATA 0 a 1 LED dat byly přerušované, i když nebyla nakonfigurovaná tato rozhraní. Hello problém byl opraven a monitorování LED teď hello správný stav. |Update 1 |Ano |Ne |
| 8 |Monitorování LED |V určitých případech po použití Update 1, hello blue light na hello řadič služby active vypnuté a díky tomu je pevný tooidentify hello aktivního řadiče. Tento problém byl opraven v této verzi opravy. |Aktualizace 1.2 |Ano |Ne |
| 9 |Síťová rozhraní |V předchozích verzích se může zařízení StorSimple pomocí směrovat brány nakonfigurovat přejít do režimu offline. V této verzi hello metriky rozhraní data 0 byl proveden hello nejnižší; Proto i v případě jiných síťových rozhraní povolenou podporu cloudu, veškerý provoz cloudové hello z hello zařízení, budou směrovány prostřednictvím Data 0. |Update 1 |Ano |Ano |
| 10 |Zálohování |Chyby v Update 1, který chybu způsobil zálohy toofail po napravení 24 dnů v hello oprava verze 1.1 aktualizace. |Aktualizace 1.1 |Ano |Ano |
| 11 |Zálohování |Chyby v předchozích verzích za následek nízký výkon pro cloudové snímky s nízkou změnu sazby. V tomto vydání opravy byl opraven této chyby. |Aktualizace 1.2 |Ano |Ano |
| 12 |Aktualizace |Chyby v Update 1, který ohlásil selhání upgradu a způsobila hello řadiče toogo do režimu obnovení, byl opraven v této verzi opravy. |Aktualizace 1.2 |Ano |Ano |

## <a name="known-issues-in-update-12"></a>Známé problémy v aktualizaci 1.2
Hello následující tabulka obsahuje souhrn známých problémů v této verzi.

| Ne. | Funkce | Problém | Komentáře nebo alternativní řešení | Platí toophysical zařízení | Platí toovirtual zařízení |
| --- | --- | --- | --- | --- | --- |
| 1 |Disk kvora |Ve výjimečných případech odpojení hello většinu disků ve skříni EBOD hello zařízení 8600 jsou výsledkem žádný disk kvora, pak hello fondu úložiště bude offline. Zůstane offline, i když jsou znovu připojeny disky hello. |Budete potřebovat tooreboot hello zařízení. Pokud hello potíže potrvají, kontaktujte prosím Microsoft Support pro další kroky. |Ano |Ne |
| 2 |Nesprávný řadiče ID |Když se provádí nahrazení řadiče, může řadič 0 zobrazí jako řadič 1. Při nahrazení řadiče při načtení hello bitovou kopii z uzlu sdílené hello, hello řadiče ID můžete zobrazují původně jako ID řadiče sdílené hello. Ve výjimečných případech může být toto chování vidět také po restartu systému. |Není třeba žádné akce uživatele. Tato situace bude automaticky vyřešen po dokončení nahrazení řadiče hello. |Ano |Ne |
| 3 |Účty úložiště |Použití účtu úložiště hello úložiště služby toodelete hello se o nepodporovaný scénář. To povede tooa situace, ve kterém nelze načíst data uživatele. |Ano |Ano | |
| 4 |Převzetí služeb při selhání zařízení |Více převzetí služeb kontejner svazků z hello stejný zdroj zařízení toodifferent cílové zařízení není podporován. Převzetí služeb při selhání zařízení jednom zařízení neaktivní toomultiple zařízení bude hello kontejnery svazků na hello nejprve převzít služby při selhání zařízení dojít ke ztrátě dat vlastnictví. Po takové převzetí služeb při selhání, budou tyto kontejnery svazků zobrazí nebo chovat jinak při zobrazení v hello portál Azure classic. | |Ano |Ne |
| 5 |Instalace |Během StorSimple adaptéru pro instalaci služby SharePoint je nutné tooprovide zařízení IP v pořadí pro toofinish hello instalace úspěšně. | |Ano |Ne |
| 6 |Webový proxy server |Pokud má vaše konfigurace webového proxy serveru HTTPS jako hello zadaný protokol, pak bude mít vliv komunikaci služby zařízení a hello zařízení přejde do režimu offline. Podpora balíčky se budou generovat také v procesu hello spotřebovávat značné množství prostředků vašeho zařízení. |Ujistěte se, že adresa URL proxy serveru webové hello má HTTP jako hello zadaný protokol. Další informace, přejděte příliš[konfigurace webového proxy serveru pro vaše zařízení](storsimple-configure-web-proxy.md). |Ano |Ne |
| 7 |Webový proxy server |Pokud nakonfigurujete a povolíte webový proxy server na registrované zařízení, bude potřebovat toorestart hello aktivního řadiče na vašem zařízení. | |Ano |Ne |
| 8 |Cloud vysokou latencí a vysokou vstupně-výstupní úlohy |Když v zařízení StorSimple dojde kombinaci cloudu velmi vysoké latenci (pořadí sekund) a vysoké vstupně-výstupní úlohy, svazky hello zařízení přejděte do sníženou stavu a hello vstupně-výstupních operací může selhat s chybou "zařízení není připraveno". |Budete potřebovat řadiče zařízení hello toomanually restartování nebo provést převzetí služeb při selhání toorecover zařízení z této situaci. |Ano |Ne |
| 9 |Azure PowerShell |Při použití rutiny StorSimple hello **Get-AzureStorSimpleStorageAccountCredential &#124; Select-Object - nejprve 1 - čekání** tooselect hello první objekt, takže můžete vytvořit nový **VolumeContainer** objektu hello rutina vrátí všechny objekty hello. |Zabalení hello rutiny v závorkách následujícím způsobem: **(Get-Azure-StorSimpleStorageAccountCredential) &#124; Select-Object - nejprve 1 - čekání** |Ano |Ano |
| 10 |Migrace |Při předávání více kontejnery svazků pro migraci, je přesné jenom pro první kontejneru svazků hello hello ÉTA pro poslední zálohu. Kromě toho paralelní migrace se spustí po hello první 4 záloh v kontejneru svazků první hello se migrují. |Doporučujeme, abyste současně migrovat jeden kontejner svazků. |Ano |Ne |
| 11 |Migrace |Po obnovení hello nejsou přidány svazky toohello zálohování zásady nebo hello skupinu virtuálního disku. |Je nutné tooadd tyto svazky tooa zásady zálohování v pořadí toocreate zálohy. |Ano |Ano |
| 12 |Migrace |Po dokončení migrace hello hello 5000/7000 řady zařízení nesmí mít přístup k hello migrovat data kontejnery. |Doporučujeme vám odstranit hello migrovat data kontejnery po hello migrace je kompletní a potvrzené. |Ano |Ne |
| 13 |Klon a zotavení po Havárii |Zařízení StorSimple softwarem Update 1 nelze klonovat nebo provést zotavení po havárii tooa zařízení 1 softwarem před aktualizací. |Budete potřebovat tooupdate hello cílové zařízení tooUpdate 1 tooallow těchto operací |Ano |Ano |
| 14 |Migrace |Zálohu konfigurace pro migraci na zařízení řady 5000 7000 může selhat, pokud existují skupiny svazek s žádné přidružené svazky. |Odstranit všechny skupiny hello prázdný svazek s žádné přidružené svazky a poté opakujte hello zálohu konfigurace. |Ano |Ne |

## <a name="physical-device-updates-in-update-12"></a>Aktualizace fyzického zařízení v aktualizaci 1.2
Pokud oprava aktualizace 1.2 je použité tooa fyzického zařízení (s tooUpdate předchozí verze 1), změní se verze softwaru hello too6.3.9600.17584.

## <a name="controller-and-firmware-updates-in-update-12"></a>Řadiče a aktualizace firmwaru v aktualizaci 1.2
Tato verze aktualizuje hello ovladače a firmware hello disku na svém zařízení.

* Další informace o aktualizaci řadiče hello SAS najdete v tématu [aktualizace 1 pro řadičů LSI SAS v Microsoft Azure StorSimple zařízení](https://support.microsoft.com/kb/3043005). 
* Další informace o aktualizaci firmwaru hello disku najdete v tématu [disku firmware aktualizace 1 pro Microsoft Azure StorSimple zařízení](https://support.microsoft.com/kb/3063416).

## <a name="virtual-device-updates-in-update-12"></a>Aktualizace virtuální zařízení v aktualizaci 1.2
Tato aktualizace nemůže být použité toohello virtuální zařízení. Nové virtuální zařízení bude nutné toobe vytvořili. 

## <a name="next-steps"></a>Další kroky
* [Nainstalujte aktualizace 1.2 zařízení](storsimple-install-update-1.md).

