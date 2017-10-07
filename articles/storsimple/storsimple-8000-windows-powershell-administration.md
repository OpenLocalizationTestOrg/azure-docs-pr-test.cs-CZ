---
title: "aaaPowerShell ke správě zařízení StorSimple | Microsoft Docs"
description: "Zjistěte, jak toouse Windows Powershellu pro StorSimple toomanage zařízení StorSimple."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 04/03/2017
ms.author: alkohli@microsoft.com
ms.openlocfilehash: e9e4bd025933cdef68b861d93749a107d1689536
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-windows-powershell-for-storsimple-tooadminister-your-device"></a>Pomocí prostředí Windows PowerShell pro StorSimple tooadminister zařízení

## <a name="overview"></a>Přehled

Poskytuje rozhraní příkazového řádku Windows Powershellu pro StorSimple, které můžete použít toomanage zařízení s Microsoft Azure StorSimple. Doporučuje se název hello, je založené na prostředí Windows PowerShell, rozhraní příkazového řádku, která je součástí omezené prostředí runspace. Z pohledu hello hello uživatele v příkazovém řádku hello omezené prostředí runspace s omezeným přístupem verzi prostředí Windows PowerShell se zobrazí. Toto rozhraní při zachování některé základní možnosti hello prostředí Windows PowerShell, má další vyhrazený rutin, které jsou s ohledem na správu zařízení s Microsoft Azure StorSimple.

Tento článek popisuje hello Windows Powershellu pro StorSimple funkce, včetně připojení toothis rozhraní a obsahuje odkazy toostep podrobné postupy nebo pracovní postupy, které můžete provádět pomocí tohoto rozhraní. pracovní postupy Hello zahrnují jak tooregister zařízení, nakonfigurujte rozhraní sítě hello na vašem zařízení, nainstalovat aktualizace, které vyžadují hello toobe zařízení v režimu údržby, změňte stav zařízení hello, a vyřešte všechny problémy, které mohou nastat.

Po přečtení tohoto článku, budete moci:

* Připojte zařízení StorSimple tooyour pomocí Windows Powershellu pro StorSimple.
* Spravovat zařízení StorSimple pomocí Windows Powershellu pro StorSimple.
* Získejte nápovědu ve Windows Powershellu pro StorSimple.

> [!NOTE]
> * Prostředí Windows PowerShell pro StorSimple rutiny umožňují toomanage zařízení StorSimple z konzoly sériového portu nebo vzdáleně přes vzdálenou komunikaci prostředí Windows PowerShell. Další informace o jednotlivých hello jednotlivých rutin, které lze použít v tomto rozhraní přejděte příliš[reference k rutině pro prostředí Windows PowerShell pro StorSimple](https://technet.microsoft.com/library/dn688168.aspx).
> * jsou rutiny Azure PowerShell StorSimple Hello jinou kolekci rutin, které povolí tooautomate StorSimple na úrovni služby a úlohy migrace z příkazového řádku hello. Další informace o hello rutin Azure Powershellu pro StorSimple přejděte toohello [reference k rutině Azure StorSimple](https://docs.microsoft.com/powershell/servicemanagement/azure.storsimple/v3.1.0/azure.storsimple).


Pro zařízení StorSimple pomocí jedné z následujících metod hello se můžete dostat hello prostředí Windows PowerShell:

* [Připojit tooStorSimple konzoly sériového portu zařízení](#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console)
* [Vzdálené připojení tooStorSimple pomocí prostředí Windows PowerShell](#connect-remotely-to-storsimple-using-windows-powershell-for-storsimple)

## <a name="connect-toowindows-powershell-for-storsimple-via-hello-device-serial-console"></a>Připojení tooWindows Powershellu pro StorSimple prostřednictvím konzoly sériového portu zařízení hello

Můžete [stáhněte si PuTTY](http://www.putty.org/) nebo podobné emulaci terminálu softwaru tooconnect tooWindows Powershellu pro StorSimple. Je třeba tooconfigure PuTTY konkrétně tooaccess hello zařízení Microsoft Azure StorSimple. Hello následující témata obsahují podrobné kroky, jak tooconfigure PuTTy a připojte toohello zařízení. Rovněž jsou vysvětleny různé možnosti nabídky v hello konzoly sériového portu.

### <a name="putty-settings"></a>Nastavení PuTTY

Ujistěte se, že používáte hello následujících rozhraní Windows PowerShell toohello tooconnect PuTTY nastavení z konzoly sériového portu hello.

#### <a name="tooconfigure-putty"></a>tooconfigure PuTTY

1. V hello PuTTY **Rekonfigurace** dialogové okno, v hello **kategorie** podokně, vyberte **klávesnice**.
2. Ujistěte se, že hello následující možnosti jsou vybrané (jsou to hello výchozí nastavení při spuštění relace).
   
   | Položka klávesnice | Vyberte |
   | --- | --- |
   | BACKSPACE klíč |Ovládací prvek-? (127) |
   | Home a End klíče |Standard |
   | Funkční klávesy a klávesnici |ESC [n ~ |
   | Počáteční stav kurzoru klíče |Normální |
   | Počáteční stav numerické klávesnici |Normální |
   | Povolení funkcí navíc klávesnice |CTRL + ALT + se liší od AltGr |
   
    ![Podporovaná nastavení Putty](./media/storsimple-windows-powershell-administration/IC740877.png)
3. Klikněte na tlačítko **Použít**.
4. V hello **kategorie** podokně, vyberte **překlad**.
5. V hello **vzdáleného znakovou sadu** pole se seznamem, vyberte **UTF-8**.
6. V části **zpracování řádku kreslení znaků**, vyberte **body kódu Unicode pomocí řádku kreslení**. Hello následující snímek obrazovky ukazuje hello správné PuTTY výběry.
   
    ![Nastavení Putty UTF](./media/storsimple-windows-powershell-administration/IC740878.png)
7. Klikněte na tlačítko **Použít**.

Teď můžete použít PuTTY tooconnect toohello konzole sériového portu zařízení provedením následujících kroků hello.

[!INCLUDE [storsimple-use-putty](../../includes/storsimple-use-putty.md)]

### <a name="about-hello-serial-console"></a>O hello konzoly sériového portu

Při přístupu k rozhraní Windows PowerShell hello zařízení StorSimple prostřednictvím sériové konzoly hello, se zobrazí zpráva hlavičky, za nímž následuje možnosti nabídky.

zpráva hlavičky Hello obsahuje základní informace zařízení StorSimple například hello model, název, verzi nainstalovaného softwaru a stav hello řadič, ke které přistupujete. Hello následující obrázek ukazuje příklad zpráva hlavičky.

![Zpráva sériové hlavičky](./media/storsimple-windows-powershell-administration/IC741098.png)

> [!IMPORTANT]
> Hello banner zpráva tooidentify můžete použít, ať už jste hello řadiče připojené toois _Active_ nebo _pasivní_.

Následující obrázek ukazuje Hello hello různé možnosti prostředí runspace, které jsou k dispozici v nabídce konzoly sériového portu hello.

![Zaregistrovat zařízení 2](./media/storsimple-windows-powershell-administration/IC740906.png)

Můžete vybrat z hello následující nastavení:

1. **Přihlaste se pomocí úplný přístup** této možnosti lze tooconnect (se správnými přihlašovacími údaji hello) toohello **SSAdminConsole** prostředí runspace na místní řadiči hello. (je místní řadič hello hello řadič, který se právě používají prostřednictvím hello konzoly sériového portu zařízení StorSimple.) Tuto možnost lze také použít tooallow tooaccess Microsoft Support neomezený prostředí runspace (relaci podpory) tootroubleshoot všechny možné zařízení problémy. Když použijete možnost 1 toolog na, můžete povolit hello Microsoft Support engineer prostředí runspace tooaccess neomezený spuštěním ke konkrétní rutině. Podrobnosti najdete příliš[spustit relaci podporu](storsimple-8000-contact-microsoft-support.md#start-a-support-session-in-windows-powershell-for-storsimple).
   
2. **Přihlaste se toopeer řadiče s úplným přístupem** tato možnost je hello stejná jako možnost 1, s tím rozdílem, že se můžete připojit (se správnými přihlašovacími údaji hello) toohello **SSAdminConsole** prostředí runspace na řadiči sdílené hello. Vzhledem k tomu, že je zařízení StorSimple hello vysoké dostupnosti zařízení s dva řadiče v konfiguraci aktivní pasivní, sdílené odkazuje toohello jiný řadič v hello zařízení, které se připojujete pomocí konzoly sériového portu hello).
   Podobně jako toooption 1, tato možnost může být také použít tooallow Microsoft Support tooaccess neomezený prostředí runspace na řadiči sdílené.

3. **Připojení s omezeným přístupem** tato možnost je použít tooaccess rozhraní Windows PowerShell v režimu omezené. Zobrazí se výzva k zadání pověření pro přístup k. Tato možnost se připojí další tooa omezený prostředí runspace ve srovnání toooptions 1 a 2.  Některé hello úkoly, které jsou k dispozici prostřednictvím možnost 1, **nelze* provést v této prostředí runspace jsou:
   
   * Obnovit tovární nastavení toohello
   * Změna hesla hello
   * Povolit nebo zakázat podporu přístupu
   * Instalace aktualizací
   * Instalaci oprav hotfix

    > [!NOTE]
    > To je hello preferované možnost, pokud jste zapomněli jste heslo správce zařízení hello a nelze se připojit prostřednictvím možnost 1 nebo 2.

4. **Změnit jazyk** této možnosti lze toochange hello jazyk zobrazení v rozhraní Windows PowerShell hello. podporované jazyky Hello jsou angličtina, japonština, ruština, francouzština, korejština – Jih, španělština, italština, němčina, čínština a Brazilská portugalština.

## <a name="connect-remotely-toostorsimple-using-windows-powershell-for-storsimple"></a>Vzdálené připojení tooStorSimple pomocí Windows Powershellu pro StorSimple

Můžete použít zařízení StorSimple tooyour tooconnect vzdálenou komunikaci prostředí Windows PowerShell. Když připojíte tímto způsobem, neuvidíte nabídky. (Byste vidět nabídky, pouze pokud používáte hello konzoly sériového portu v zařízení tooconnect hello. Vzdálené připojení přejdete přímo toohello ekvivalent "možnost 1 – úplný přístup" na hello konzoly sériového portu.) Díky vzdálené komunikace Windows Powershellu připojit tooa konkrétní prostředí runspace. Můžete také zadat hello jazyk zobrazení.

Hello jazyk zobrazení je nezávislá hello jazyk, který nastavíte pomocí hello **změnit jazyk** možnost v nabídce konzoly sériového portu hello. Vzdáleného prostředí PowerShell automaticky převezmou hello národního prostředí hello zařízení ze se připojujete, pokud není zadaný žádný.

> [!NOTE]
> Pokud pracujete s Microsoft Azure virtuální hostitele a cloudu zařízení StorSimple, můžete použít prostředí Windows PowerShell vzdálené komunikace a hello virtuální hostitel tooconnect toohello cloudu zařízení. Pokud jste nastavili umístění sdílené složky na hostiteli hello, na které toosave informace z relace prostředí Windows PowerShell text hello, byste měli vědět této hello _Everyone_ hlavní zahrnuje jen ověření uživatelé. Proto pokud jste nastavili tak přístup ke sdílené složce tooallow hello podle _Everyone_ a připojíte bez zadání pověření, hello neověřené anonymní objekt, který se použije a zobrazí se chyba. toofix potíže na hello sdílet hostitele, musíte povolit účet Guest hello a pak umožnit hello hosta účet úplný přístup toohello sdílení nebo je nutné zadat platné přihlašovací údaje společně s hello rutiny prostředí Windows PowerShell.


Pomocí protokolu HTTP nebo HTTPS tooconnect přes vzdálenou komunikaci prostředí Windows PowerShell. Používejte hello pokyny v hello následující kurzy:

* [Připojit vzdáleně pomocí protokolu HTTP](storsimple-remote-connect.md#connect-through-http)
* [Vzdálené připojení pomocí protokolu HTTPS](storsimple-remote-connect.md#connect-through-https)

## <a name="connection-security-considerations"></a>Aspekty zabezpečení připojení

Při rozhodování jak tooconnect tooWindows Powershellu pro StorSimple, zvažte následující hello:

* Připojení přímo toohello konzoly sériového portu zařízení je bezpečné, ale není připojování konzoly sériového portu toohello přes síťové přepínače. Buďte opatrní hello rizika zabezpečení při připojování toodevice sériové přes síťové přepínače.
* Připojení přes relaci protokolu HTTP může nabízí lepší zabezpečení než připojení prostřednictvím sériové konzoly hello přes síť. I když to není hello nejbezpečnější metodou, je přijatelné v důvěryhodných sítích.
* Připojení prostřednictvím relace HTTPS je hello nejbezpečnější a doporučená možnost hello.

## <a name="administer-your-storsimple-device-using-windows-powershell-for-storsimple"></a>Spravovat zařízení StorSimple pomocí Windows Powershellu pro StorSimple

Hello následující tabulka zobrazuje souhrn všech hello běžné úlohy správy a komplexní pracovní postupy, které lze provést v rámci rozhraní Windows PowerShell hello zařízení StorSimple. Další informace o každém pracovním postupu klikněte na tlačítko hello odpovídající záznam v tabulce hello.

#### <a name="windows-powershell-for-storsimple-workflows"></a>Prostředí Windows PowerShell pro pracovní postupy StorSimple

| Pokud chcete, aby toodo to... | Pomocí tohoto postupu. |
| --- | --- |
| Registrace zařízení |[Konfigurace a registrace zařízení hello pomocí Windows Powershellu pro StorSimple](storsimple-8000-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple) |
| Konfigurace webového proxy serveru</br>Nastavení proxy serveru webové zobrazení |[Konfigurace webového proxy serveru pro zařízení StorSimple](storsimple-8000-configure-web-proxy.md) |
| Upravit nastavení 0 síťového rozhraní DATA na zařízení |[Upravit síťového rozhraní DATA 0 pro zařízení StorSimple](storsimple-8000-modify-data-0.md) |
| Zastavit řadič </br> Restartování nebo vypnutí řadič </br> Vypněte zařízení</br>Resetování hello zařízení toofactory výchozí nastavení |[Správa řadiče zařízení](storsimple-8000-manage-device-controller.md) |
| Nainstalujte aktualizace režimu údržby a opravy hotfix |[Aktualizace zařízení](storsimple-update-device.md) |
| Přechod do režimu údržby </br>Ukončení režimu údržby |[Režimy zařízení StorSimple](storsimple-8000-device-modes.md) |
| Vytvoření balíčku pro podporu</br>Dešifrování a upravit balíčku pro podporu |[Vytvoření a Správa balíčku pro podporu](storsimple-8000-create-manage-support-package.md) |
| Spusťte relaci podpory</br> |[Spusťte relaci podpory v prostředí Windows PowerShell pro StorSimple](storsimple-8000-create-manage-support-package.md#create-a-support-package) |

## <a name="get-help-in-windows-powershell-for-storsimple"></a>Získání nápovědy ve Windows Powershellu pro StorSimple

Rutina nápovědy ve Windows Powershellu pro StorSimple je k dispozici. Online, aktuální verzi této nápovědy je také k dispozici, který můžete použít tooupdate hello nápovědy ve vašem systému.

Získání nápovědy v toto rozhraní je podobné toothat v prostředí Windows PowerShell a většina hello souvisejících s rutinami, budou fungovat. Nápověda pro prostředí Windows PowerShell online můžete najít v hello knihovně TechNet: [skriptování v prostředí Windows PowerShell](http://go.microsoft.com/fwlink/?LinkID=108518).

Hello Následuje stručný popis typů hello nápovědy pro toto rozhraní prostředí Windows PowerShell, včetně jak tooupdate hello nápovědy.

### <a name="tooget-help-for-a-cmdlet"></a>tooget nápovědy pro rutinu

* tooget nápovědy pro všechny rutiny nebo funkce, hello použijte následující příkaz:`Get-Help <cmdlet-name>`
* tooget online nápovědy pro všechny rutiny, použijte rutinu předchozí hello s hello `-Online` parametr:`Get-Help <cmdlet-name> -Online`
* Pro úplnou nápovědu, můžete použít hello `–Full` parametr a příklady, použijte hello `–Examples` parametr.

### <a name="tooupdate-help"></a>tooupdate nápovědy

Můžete snadno aktualizovat hello nápovědy v rozhraní Windows PowerShell hello. Proveďte následující kroky tooupdate hello Nápověda systému hello.

#### <a name="tooupdate-cmdlet-help"></a>rutiny tooupdate nápovědy
1. Spusťte prostředí Windows PowerShell s hello **spustit jako správce** možnost.
2. Hello příkazového řádku zadejte:`Update-Help`
3. Hello aktualizovat budou nainstalovány soubory nápovědy.
4. Když jsou instalovány soubory nápovědy hello, zadejte: `Get-Help Get-Command`. Tím zobrazíte seznam rutin, pro který je k dispozici nápověda.

> [!NOTE]
> tooget seznam všech dostupných rutin hello v prostředí runspace, přihlaste toohello odpovídající možnost nabídky a spusťte hello `Get-Command` rutiny.


## <a name="next-steps"></a>Další kroky

Pokud máte problémy s vaším zařízením StorSimple při provádění mezi hello výše pracovní postupy, podívejte se příliš[nástroje pro řešení potíží s nasazením StorSimple](storsimple-troubleshoot-deployment.md#tools-for-troubleshooting-storsimple-deployments).

