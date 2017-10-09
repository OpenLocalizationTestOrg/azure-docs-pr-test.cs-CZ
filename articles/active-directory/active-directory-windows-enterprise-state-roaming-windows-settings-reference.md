---
title: "odkaz nastavení roamingu aaaWindows 10 | Microsoft Docs"
description: "Úplný seznam všech hello nastavení, které budou roamované nebo zálohovat ve Windows 10."
services: active-directory
keywords: roaming, stav Enterprise windows cloud
documentationcenter: 
author: tanning
manager: femila
editor: curtand
ms.assetid: 17cffc3e-2928-4235-91f7-a685bd6bdcbf
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/08/2017
ms.author: markvi
ms.openlocfilehash: 381e2220b698bb0e477c207984ff96c03ed132ca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="windows-10-roaming-settings-reference"></a>Referenční informace k nastavení roamingu pro Windows 10
Hello následuje úplný seznam všech hello nastavení, které budou roamované nebo zálohovat ve Windows 10. 

## <a name="devices-and-endpoints"></a>Koncové body a zařízení
Viz následující tabulka pro souhrn hello zařízení a typy účtů, které jsou podporovány hello synchronizace, zálohování, hello a obnovte framework ve Windows 10.

| Typ účtu a operace | Desktop | Mobilní |
| --- | --- | --- |
| Azure Active Directory: synchronizace |Ano |Ne |
| Azure Active Directory: zálohování a obnovení |Ne |Ne |
| Účet Microsoft: synchronizace |Ano |Ano |
| Účet Microsoft: zálohování a obnovení |Ne |Ano |

## <a name="what-is-backup"></a>Co je zálohování?
Nastavení systému Windows se obecně synchronizaci ve výchozím nastavení, ale některá nastavení jsou jenom zálohovat, jako je například hello seznam nainstalovaných aplikací na zařízení. Zálohování je pro mobilní zařízení pouze a aktuálně nejsou k dispozici pro uživatele Enterprise State Roaming. Zálohování používá účet Microsoft a ukládá hello nastavení a data aplikací do OneDrive. Pokud uživatel zakáže synchronizace v zařízení hello pomocí hello nastavení aplikace, data aplikací, který se standardně synchronizuje se změní na zálohování jenom. Zálohovaná data jsou přístupné pouze prostřednictvím operace obnovení hello během hello prvního spuštění počítače nového zařízení. Zálohování je možné zakázat prostřednictvím nastavení zařízení hello a můžete spravovat a je Odstraněná v účtu OneDrive hello uživatele.

## <a name="windows-settings-overview"></a>Přehled nastavení systému Windows
Hello následující skupiny nastavení jsou k dispozici pro synchronizaci nastavení koncoví uživatelé tooenable nebo zakáže na zařízení s Windows 10.

* Motiv: pozadí plochy, dlaždici uživatele, pozice panelu, atd. 
* Nastavení aplikace Internet Explorer: procházení historie zadané adresy URL, oblíbených položek, atd. 
* Hesla: [schránku na pověření systému Windows](https://technet.microsoft.com/library/jj554668.aspx), včetně profilů sítě Wi-Fi 
* Jazykové předvolby: Kontrola pravopisu systému slovník nastavení jazyka 
* Usnadnění přístupu: Předčítání, klávesnice na obrazovce, Lupa 
* Další nastavení Windows: najdete v části Podrobnosti o nastavení systému Windows

![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-individual-sync-settings.png)

Synchronizace skupiny (oblíbených položek, čtení seznamu) nastavení prohlížeče Edge můžete povolit nebo zakázat koncoví uživatelé prostřednictvím prohlížeče Edge možnost nabídky nastavení.

![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-sync-content.png)

## <a name="windows-settings-details"></a>Podrobnosti o nastavení systému Windows
V následující tabulce hello, další položky ve sloupci skupina nastavení hello odkazuje toosettings, které můžete zakázali přechodem tooSettings > účty > synchronizace nastavení > Nastavení ostatní systému Windows. 

Interní položky ve sloupci skupina nastavení hello najdete toosettings služby a aplikace, které se dá deaktivovat jenom ze synchronizace v rámci hello aplikace nebo zakázáním synchronizace hello zařízení pomocí správy mobilních zařízení (MDM) nebo nastavení zásad skupiny.
Nastavení, která není přemístit nebo synchronizace nebude patřit tooa skupiny.

| Nastavení | Desktop | Mobilní | Skupina |
| --- | --- | --- | --- |
| **Účty**: obrázek účtu |Synchronizace |X |Motiv |
| **Účty**: Další nastavení účtu |X |X | |
| **Rozšířené mobilního širokopásmového připojení**: název sítě (umožňuje automatické zjišťování mobilní sítě Wi-Fi hotspotům přes Bluetooth) sdílení připojení k Internetu |X |X |Hesla |
| **Data aplikací**: jednotlivými aplikacemi může synchronizovat data |synchronizace zálohování |synchronizace zálohování |Interní |
| **Seznam aplikací**: seznam nainstalovaných aplikací |X |zálohování |Ostatní |
| **Bluetooth**: všechna nastavení Bluetooth |X |X | |
| **Příkazový řádek**: "Výchozí" nastavení příkazového řádku |Synchronizace |X | |
| **Přihlašovací údaje**: schránku na pověření |Synchronizace |Synchronizace |heslo |
| **Datum, čas a oblast**: automatické čas (synchronizace času Internet) |Synchronizace |Synchronizace |Jazyk |
| **Datum, čas a oblast**: 24 hodin |Synchronizace |X |Jazyk |
| **Datum, čas a oblast**: datum a čas |Synchronizace |X |Jazyk |
| **Datum, čas a oblast**: časové pásmo | |X |Jazyk |
| **Datum, čas a oblast**: letní čas |Synchronizace |X |Jazyk |
| **Datum, čas a oblast**: země nebo oblast |Synchronizace |X |Jazyk |
| **Datum, čas a oblast**: první den v týdnu |Synchronizace |X |Jazyk |
| **Datum, čas a oblast**: formát oblast (národní prostředí) |Synchronizace |X |Jazyk |
| **Datum, čas a oblast**: krátkodobých datum |Synchronizace |X |Jazyk |
| **Datum, čas a oblast**: dlouho datum |Synchronizace |X |Jazyk |
| **Datum, čas a oblast**: krátkého času |Synchronizace |X |Jazyk |
| **Datum, čas a oblast**: dlouho čas |Synchronizace |X |Jazyk |
| **Přizpůsobení plochy**: motiv plochy (pozadí, barva systému, výchozí systémové zvuky, šetřič obrazovky) |Synchronizace |X |Motiv |
| **Přizpůsobení plochy**: prezentace tapety |Synchronizace |X |Motiv |
| **Přizpůsobení plochy**: nastavení hlavního panelu (pozice, automaticky skrýt atd.) |Synchronizace |X |Motiv |
| **Přizpůsobení plochy**: rozložení obrazovky start |X |zálohování | |
| **Zařízení**: sdílené tiskárny, které jste se připojili příliš|X |X |ostatní |
| **Prohlížeč Edge**: čtení seznamu |Synchronizace |Synchronizace |Interní |
| **Prohlížeč Edge**: oblíbených položek |Synchronizace |Synchronizace |Interní |
| **Prohlížeč Edge**: top lokality <sup> [[1]](#footnote-1)</sup> |Synchronizace |Synchronizace |Interní |
| **Prohlížeč Edge**: zadané adresy URL <sup> [[1]](#footnote-1)</sup> |Synchronizace |Synchronizace |Interní |
| **Prohlížeč Edge**: oblíbených položek panelu nastavení <sup> [[1]](#footnote-1)</sup> |Synchronizace |Synchronizace |Interní |
| **Prohlížeč Edge**: Zobrazit tlačítko Domů hello <sup> [[1]](#footnote-1)</sup> |Synchronizace |Synchronizace |Interní |
| **Prohlížeč Edge**: blokovat automaticky otevíraná okna <sup> [[1]](#footnote-1)</sup> |Synchronizace |Synchronizace |Interní |
| **Prohlížeč Edge**: dotaz na jaké toodo s každého stažení <sup> [[1]](#footnote-1)</sup> |Synchronizace |Synchronizace |Interní |
| **Prohlížeč Edge**: nabízejí hesla toosave <sup> [[1]](#footnote-1)</sup> |Synchronizace |Synchronizace |Interní |
| **Prohlížeč Edge**: odeslání není sledování žádostí o <sup> [[1]](#footnote-1)</sup> |Synchronizace |Synchronizace |Interní |
| **Prohlížeč Edge**: uložit formuláře položky <sup> [[1]](#footnote-1)</sup> |Synchronizace |Synchronizace |Interní |
| **Prohlížeč Edge**: zobrazovat návrhy vyhledávání a lokality během zadávání <sup> [[1]](#footnote-1)</sup> |Synchronizace |Synchronizace |Interní |
| **Prohlížeč Edge**: soubory cookie předvoleb <sup> [[1]](#footnote-1)</sup> |Synchronizace |Synchronizace |Interní |
| **Prohlížeč Edge**: nechat lokality uložit licence chráněné médií v zařízení <sup> [[1]](#footnote-1)</sup> |Synchronizace |Synchronizace |Interní |
| **Prohlížeč Edge**: čtečky obrazovky nastavení <sup> [[1]](#footnote-1)</sup> |Synchronizace |Synchronizace |Interní |
| **Vysoký kontrast**: zapnutí nebo vypnutí |Synchronizace |X |usnadnění přístupu |
| **Vysoký kontrast**: nastavení motivů |Synchronizace |X |usnadnění přístupu |
| **Internet Explorer**: otevřete karty (adresa URL a název) |Synchronizace |Synchronizace |Internet Explorer |
| **Internet Explorer**: čtení seznamu |Synchronizace |Synchronizace |Internet Explorer |
| **Internet Explorer**: zadané adresy URL |Synchronizace |Synchronizace |Internet Explorer |
| **Internet Explorer**: procházení historie |Synchronizace |Synchronizace |Internet Explorer |
| **Internet Explorer**: oblíbených položek |Synchronizace |Synchronizace |Internet Explorer |
| **Internet Explorer**: vyloučené adresy URL |Synchronizace |Synchronizace |Internet Explorer |
| **Internet Explorer**: domovské stránky |Synchronizace |Synchronizace |Internet Explorer |
| **Internet Explorer**: návrhy domény |Synchronizace |Synchronizace |Internet Explorer |
| **Klávesnice**: uživatelé mohou zapnutí/vypnutí klávesnice na obrazovce |Synchronizace |X |usnadnění přístupu |
| **Klávesnice**: zapnout trvalé Ano (ve výchozím nastavení vypnuté) |Synchronizace |X |usnadnění přístupu |
| **Klávesnice**: Zapnutí filtru klíče (ve výchozím nastavení vypnuté) |Synchronizace |X |usnadnění přístupu |
| **Klávesnice**: zapnout přepnutí klíče (ve výchozím nastavení vypnuté) |Synchronizace |X |usnadnění přístupu |
| **Internet Explorer**: domény jazyk: čínština (zjednodušená Čínština) QWERTY - povolit samoobslužné učení |Synchronizace |X |Jazyk |
| **Jazyk**: CHS QWERTY - povolit dynamické candidate řazení |Synchronizace |X |Jazyk |
| **Jazyk**: CHS QWERTY - char-set zjednodušená čínština |Synchronizace |X |Jazyk |
| **Jazyk**: CHS QWERTY - char-set tradiční čínštině |Synchronizace |X |Jazyk |
| **Jazyk**: CHS QWERTY - přibližné pchin-jin |Synchronizace |zálohování |Jazyk |
| **Jazyk**: CHS QWERTY - přibližné páry |Synchronizace |zálohování |Jazyk |
| **Jazyk**: CHS QWERTY – úplná pchin-jin |Synchronizace |X |Jazyk |
| **Jazyk**: CHS QWERTY - dvojité pchin-jin |Synchronizace |X |Jazyk |
| **Jazyk**: CHS QWERTY - čtení automatické opravy |Synchronizace |X |Jazyk |
| **Jazyk**: CHS QWERTY - klíč přepínač C/E, posunutí |Synchronizace |X |Jazyk |
| **Jazyk**: CHS QWERTY - klíč přepínač C/E, Ctrl |Synchronizace |X |Jazyk |
| **Jazyk**: CHS WUBI - vstupní režimu jednoho znaku |Synchronizace |X |Jazyk |
| **Jazyk**: CHS WUBI - zobrazit hello zbývající kódování hello candidate |Synchronizace |X |Jazyk |
| **Jazyk**: CHS WUBI - zvukový signál při kódování 4 je neplatný |Synchronizace |X |Jazyk |
| **Jazyk**: CHT Ču - zahrnují CJK Ext-A |Synchronizace |X |Jazyk |
| **Jazyk**: editoru IME pro japonštinu - prediktivní zadáním a vlastní slova |Synchronizace |Synchronizace |Jazyk |
| **Jazyk**: korejština (KOR) editoru IME |X |X |Jazyk |
| **Jazyk**: rozpoznávání rukopisu |X |X |Jazyk |
| **Jazyk**: jazyk profilu |Synchronizace |zálohování |Jazyk |
| **Jazyk**: Kontrola pravopisu - automatické opravy a zvýraznění pravopisné chyby |Synchronizace |zálohování |Jazyk |
| **Jazyk**: seznam klávesnice |Synchronizace |zálohování |Jazyk |
| **Zamknout obrazovku**: všechny uzamknout nastavení obrazovky |X |X | |
| **Lupa**: zapnout nebo vypnout (hlavní přepínač). |X |X |Usnadnění přístupu |
| **Lupa**: inverzi barev vypnutí a zapnutí (ve výchozím nastavení vypnuté) |Synchronizace |X |Usnadnění přístupu |
| **Lupa**: sledování – postupujte podle fokus klávesnice hello |Synchronizace |X |Usnadnění přístupu |
| **Lupa**: sledování – postupujte podle hello myší |Synchronizace |X |Usnadnění přístupu |
| **Lupa**: spustit po přihlášení uživatelé (ve výchozím nastavení vypnuté) |Synchronizace |X |Usnadnění přístupu |
| **Myš**: když změníte velikost hello myší |Synchronizace |X |ostatní |
| **Myš**: změnit barvu hello myší |Synchronizace |X |ostatní |
| **Myš**: všechna ostatní nastavení |X |X | |
| **Program Předčítání**: Snadné spuštění |Synchronizace |X |Usnadnění přístupu |
| **Program Předčítání**: uživatelé mohou změnit Předčítání hovořícího výšky |Synchronizace |X |Usnadnění přístupu |
| **Program Předčítání**: uživatelů můžete zapnout nebo vypnout program Předčítání čtení odkazů pro společné položky (na ve výchozím nastavení) |Synchronizace |X |Usnadnění přístupu |
| **Program Předčítání**: uživatelů můžete zapnout nebo vypnout, zda lze poslouchat zadané znaky (na ve výchozím nastavení) |Synchronizace |X |Usnadnění přístupu |
| **Program Předčítání**: uživatelů můžete zapnout nebo vypnout, zda lze poslouchat typové slova (na ve výchozím nastavení) |Synchronizace |X |Usnadnění přístupu |
| **Program Předčítání**: mít vložení kurzoru následující Předčítání (na ve výchozím nastavení) |Synchronizace |X |Usnadnění přístupu |
| **Program Předčítání**: Povolit visual zvýraznění Předčítání kurzoru (na ve výchozím nastavení) |Synchronizace |X |Usnadnění přístupu |
| **Program Předčítání**: přehrávání zvuku upozornění (na ve výchozím nastavení) |Synchronizace |X |Usnadnění přístupu |
| **Program Předčítání**: aktivaci služby správy klíčů na hello touch klávesnice, při navýšení prstu (ve výchozím nastavení vypnuté) |Synchronizace |X |Usnadnění přístupu |
| **Usnadnění přístupu**: nastavte hello tloušťka blikající kurzor hello |Synchronizace |X |Usnadnění přístupu |
| **Usnadnění přístupu**: Odeberte obrázky na pozadí (ve výchozím nastavení vypnuté) |Synchronizace |X |Usnadnění přístupu |
| **Napájení a režimu spánku**: všechna nastavení |X |X | |
| **Přizpůsobení obrazovky Start**: zvýraznění barvu (pouze phone) |X |Synchronizace |Motiv |
| **Zadáním**: slovníku |Synchronizace |zálohování |Jazyk |
| **Zadáním**: Automatické opravy nesprávně zadaných aplikace word |Synchronizace |zálohování |Jazyk |
| **Zadáním**: zvýrazňovat slova překlepu |Synchronizace |zálohování |Jazyk |
| **Zadáním**: zobrazovat návrhy text při psaní |Synchronizace |zálohování |Jazyk |
| **Zadáním**: přidají mezeru po zvolit návrh textu |Synchronizace |zálohování |Jazyk |
| **Zadáním**: Přidání doby po I poklepání MEZERNÍK hello |Synchronizace |zálohování |Jazyk |
| **Zadáním**: velká počáteční hello první písmeno každého věty. |Synchronizace |zálohování |Jazyk |
| **Zadáním**: při I poklepání klávesu shift použít všechny velká písmena |Synchronizace |zálohování |Jazyk |
| **Zadáním**: přehrání zvuků klíče při psaní |Synchronizace |zálohování |Jazyk |
| **Zadáním**: data individuálního nastavení pro dotykové klávesnice |Synchronizace |zálohování |Jazyk |
| **Wi-Fi**: profily Wi-Fi (jenom WPA) |Synchronizace |Synchronizace |Hesla |

###### <a name="footnote-1"></a>Poznámka pod čarou 1
Minimální podporovaná verze operačního systému Windows Creators aktualizace (sestavení 15063). 

## <a name="related-topics"></a>Související témata
* [Přehled roamingu stavu Enterprise](active-directory-windows-enterprise-state-roaming-overview.md)
* [Povolit stav enterprise roaming v Azure Active Directory](active-directory-windows-enterprise-state-roaming-enable.md)
* [Nastavení a datový roaming – nejčastější dotazy](active-directory-windows-enterprise-state-roaming-faqs.md)
* [Nastavení MDM pro synchronizaci nastavení zásad skupiny a](active-directory-windows-enterprise-state-roaming-group-policy-settings.md)
* [Řešení potíží](active-directory-windows-enterprise-state-roaming-troubleshooting.md)
