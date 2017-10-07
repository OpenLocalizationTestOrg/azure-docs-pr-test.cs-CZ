---
title: "změny aaaTrack s Azure Log Analytics | Microsoft Docs"
description: "Hello řešení sledování změn v analýzy protokolů pomáhá identifikovat software a služby systému Windows změny ve vašem prostředí."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: f8040d5d-3c89-4f0c-8520-751c00251cb7
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2bb1938caad25101e167927200ac3ef495120fe0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="track-software-changes-in-your-environment-with-hello-change-tracking-solution"></a>Sledování změn softwaru ve vašem prostředí s hello řešení pro sledování změn

![Změnit symbol sledování](./media/log-analytics-change-tracking/change-tracking-symbol.png)

Tento článek vám pomůže použití hello řešení sledování změn v tooeasily Log Analytics identifikovat změny ve vašem prostředí. Hello řešení sleduje změny tooWindows a softwaru platformy Linux, soubory systému Windows a klíčů registru, služby systému Windows a Linux démoni. Identifikace změny konfigurace vám mohou pomoci přesně určit provozní problémy.

Nainstalujete hello řešení tooupdate hello typ agenta, který jste nainstalovali. Změny softwaru tooinstalled, služby systému Windows a Linux démoni na hello monitorované servery jsou přečíst. Hello data se pak odešlou toohello analýzy protokolů služby v hello cloud pro zpracování. Logika je použité toohello přijatých dat a hello Cloudová služba zaznamenává hello data. Pomocí hello informace na řídicím panelu sledování změn hello snadno uvidíte hello změny, které byly provedeny v serverové infrastruktuře.

## <a name="installing-and-configuring-hello-solution"></a>Instalace a konfigurace řešení hello
Použijte následující informace tooinstall hello a nakonfigurujte hello řešení.

* Musíte mít [Windows](log-analytics-windows-agents.md), [nástroje Operations Manager](log-analytics-om-agents.md), nebo [Linux](log-analytics-linux-agents.md) agent na každém počítači, kam chcete toomonitor změny.
* Přidat hello sledování změn řešení tooyour pracovním prostorem OMS z hello [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.ChangeTrackingOMS?tab=Overview). Nebo můžete přidat pomocí informací o hello v řešení hello [řešení přidat analýzy protokolů z hello řešení Galerie](log-analytics-add-solutions.md). Není nutná žádná další konfigurace.

### <a name="configure-linux-files-tootrack"></a>Konfigurovat soubory tootrack Linux
Pomocí následujících kroků tooconfigure soubory tootrack počítačů se systémem Linux hello.

1. Na portálu OMS hello, klikněte na tlačítko **nastavení** (symbol hello ozubené kolečko).
2. Na hello **nastavení** klikněte na tlačítko **Data**a potom klikněte na **sledování souboru Linux**.
3. Ve skupinovém rámečku, aby bylo možné sledování změn v souboru Linux, zadejte hello celou cestu, včetně hello název souboru hello má tootrack a pak klikněte na tlačítko hello **přidat** symbol. Například: "/etc/*.conf"
4. Klikněte na **Uložit**.  

> [!NOTE]
> Soubor Linux sledování obsahuje další možnosti, včetně directory sledování recrusion prostřednictvím adresářů a sledování zástupný znak.

### <a name="configure-windows-files-tootrack"></a>Konfigurace tootrack soubory Windows
Pomocí následujících kroků tooconfigure soubory tootrack na počítačích s Windows hello.

1. Na portálu OMS hello, klikněte na tlačítko **nastavení** (symbol hello ozubené kolečko).
2. Na hello **nastavení** klikněte na tlačítko **Data**a potom klikněte na **sledování souboru Windows**.
3. V části, aby bylo možné sledování změn v souboru systému Windows, zadejte celou cestu hello, včetně hello název souboru hello má tootrack a pak klikněte na tlačítko hello **přidat** symbol. Například: C:\Program Files (x86) \Internet Explorer\iexplore.exe nebo C:\Windows\System32\drivers\etc\hosts.
4. Klikněte na **Uložit**.  
   ![Sledování změn souborů systému Windows](./media/log-analytics-change-tracking/windows-file-change-tracking.png)

### <a name="configure-windows-registry-keys-tootrack"></a>Konfigurace tootrack klíče registru Windows
Pomocí následujících kroků tooconfigure registru klíče tootrack na počítačích s Windows hello.

1. Na portálu OMS hello, klikněte na tlačítko **nastavení** (symbol hello ozubené kolečko).
2. Na hello **nastavení** klikněte na tlačítko **Data**a potom klikněte na **sledování registru Windows**.
3. V části, aby bylo možné sledování změn v registru systému Windows, zadejte celý klíč hello má tootrack a pak klikněte na tlačítko hello **přidat** symbol.
4. Klikněte na **Uložit**.  
   ![Sledování změn registru systému Windows](./media/log-analytics-change-tracking/windows-registry-change-tracking.png)

### <a name="explanation-of-linux-file-collection-properties"></a>Vysvětlení vlastností kolekce Linux souboru
1. **Typ**
   * **Soubor** (sestavu metadata souboru - velikost, datum změny, hodnota hash, atd.)
   * **Adresář** (sestava metadat adresáře - velikost, datum změny, atd.)
2. **Odkazy** (zpracování Linux symlink odkazuje tooother soubory nebo adresáře)
   * **Ignorovat** (symbolických odkazů ignorovat během recurions toonot zahrnout hello soubory nebo adresáře odkazuje)
   * **Postupujte podle** (symbolických odkazů hello postupujte podle během rekurze tooalso zahrnout hello soubory nebo adresáře odkazuje)
   * **Spravovat** (podle symbolických hello odkazů a alter hello zpracování vrácená obsahu)

   > [!NOTE]   
   > Hello "Manage" možnost propojení se nedoporučuje. Načtení souboru obsahu není podporováno.

3. **Recurse** (Recurse prostřednictvím úrovně složky a sledovat všechny soubory, které splňuje příkazu cesty hello)
4. **Sudo** (Povolit přístup k souborům nebo adresářům, které vyžadují oprávnění sudo)

### <a name="limitations"></a>Omezení
Hello řešení pro sledování změn v současné době nepodporuje hello následující položky:

* Složky (adresáře) pro Windows Sledování souboru
* Rekurze pro sledování souboru systému Windows
* Zástupné znaky pro sledování souborů systému Windows
* Proměnné cest
* Systémy souborů síť
* Obsah souboru

Jiná omezení:

* Hello **maximální velikost souboru** sloupec a hodnoty jsou ve aktuální implementace hello nepoužívá.
* Pokud jste v cyklu sběru 30 minut hello shromažďovat více než 2 500 soubory, může docházet ke snížení výkonu řešení.
* Když je síťový provoz vysoké, záznamy změn může trvat až tooa maximálně toodisplay šest hodin.
* Pokud upravíte konfiguraci hello, zatímco počítač je vypnutý, můžete umístit počítač hello změny souborů, které byly součástí předchozí konfiguraci toohello.

## <a name="change-tracking-data-collection-details"></a>Změňte podrobnosti pro kolekce dat sledování
Sledování změn shromažďuje inventář softwaru a metadata služby systému Windows pomocí hello agentů, které jste povolili.

Hello následující tabulka uvádí metody shromažďování dat a další podrobnosti o tom, jak se data shromažďují pro sledování změn.

| Platforma | Přímé agenta | Agent nástroje Operations Manager | Agenta systému Linux | Azure Storage | Nástroj Operations Manager vyžaduje? | Dat agenta nástroje Operations Manager odeslána prostřednictvím skupiny pro správu | Frekvence kolekce |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Systém Windows a Linux | &#8226; | &#8226; | &#8226; |  |  | &#8226; | 5 minut too50 minut v závislosti na typu změnu hello. Viz následující tabulka informace hello. |


Hello následující tabulka uvádí četnost kolekce hello data pro hello typů změn.

| **změnit typ** | **frekvence** | **Nemá****agenta****odeslat rozdíly, když se najde?**  |
| --- | --- | --- |
| Registru systému Windows | 50 minut | Ne |
| Soubor systému Windows | 30 minut | Ano. Pokud se nezměnila za 24 hodin, se budou odesílat snímku. |
| Soubor Linux | 15 minut | Ano. Pokud se nezměnila za 24 hodin, se budou odesílat snímku. |
| Služby pro Windows | 30 minut | Ano, každých 30 minut, kdy jsou zjištěny změny. Každých 24 hodin snímku se pošle bez ohledu na změnu. Ano, hello snímku se odesílají i tam, kde neexistují žádné změny. |
| Démoni Linux | 5 minut | Ano. Pokud se nezměnila za 24 hodin, se budou odesílat snímku. |
| Windows software | 30 minut | Ano, každých 30 minut, kdy jsou zjištěny změny. Každých 24 hodin snímku se pošle bez ohledu na změnu. Ano, hello snímku se odesílají i tam, kde neexistují žádné změny. |
| Softwaru platformy Linux | 5 minut | Ano. Pokud se nezměnila za 24 hodin, se budou odesílat snímku. |

### <a name="registry-key-change-tracking"></a>Sledování změn klíče registru

Analýzy protokolů provede registru systému Windows, monitorování a sledování s hello řešení pro sledování změn. účelem Hello monitorování změny tooregistry klíče je body rozšiřitelnosti toopinpoint, kde můžete aktivovat kód třetích stran a malwarem. Hello následující seznam ukazuje hello výchozí klíče registru, jsou sledovány hello řešení a proč každý sledován.

- Nastavení HKEY\_místní\_MACHINE\Software\Microsoft\Windows\CurrentVersion\Group Policy\Scripts\Startup
    - Monitorování skripty, které spustí při spuštění.
- Nastavení HKEY\_místní\_MACHINE\Software\Microsoft\Windows\CurrentVersion\Group Policy\Scripts\Shutdown
    - Monitorování skripty, které běží při vypnutí.
- Nastavení HKEY\_místní\_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Run
    - Monitoruje klíče, které jsou načteny před hello uživatel přihlásí tootheir účet systému Windows. Hello klíč se používá pro 32bitová aplikace spuštěné na 64bitových počítačích.
- Nastavení HKEY\_místní\_MACHINE\SOFTWARE\Microsoft\Active Setup\Installed součásti
    - Sleduje změny v nastavení tooapplication.
- Nastavení HKEY\_místní\_MACHINE\Software\Classes\Directory\ShellEx\ContextMenuHandlers
    - Monitorování běžných automatické spuštění položky, které připojit přímo do Průzkumníka Windows a obvykle spuštění v rámci procesu Explorer.exe.
- Nastavení HKEY\_místní\_MACHINE\Software\Classes\Directory\Shellex\CopyHookHandlers
    - Monitorování běžných automatické spuštění položky, které připojit přímo do Průzkumníka Windows a obvykle spuštění v rámci procesu Explorer.exe.
- Nastavení HKEY\_místní\_MACHINE\Software\Classes\Directory\Background\ShellEx\ContextMenuHandlers
    - Monitorování běžných automatické spuštění položky, které připojit přímo do Průzkumníka Windows a obvykle spuštění v rámci procesu Explorer.exe.
- Nastavení HKEY\_místní\_MACHINE\Software\Microsoft\Windows\CurrentVersion\Explorer\ShellIconOverlayIdentifiers
    - Monitorování pro ikonu překrytí registraci obslužné rutiny.
- Nastavení HKEY\_místní\_MACHINE\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Explorer\ShellIconOverlayIdentifiers
    - Monitorování pro ikonu překrytí registrace obslužné rutiny pro 32bitové aplikace spuštěné na 64bitových počítačích.
- Nastavení HKEY\_místní\_MACHINE\Software\Microsoft\Windows\CurrentVersion\Explorer\Browser pomocné objekty
    - Monitorování pro moduly nový prohlížeč Pomocník objekt plug-in pro Internet Explorer. Použít tooaccess hello Model DOM (Document Object) hello aktuální stránku a toocontrol navigace.
- Nastavení HKEY\_místní\_MACHINE\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Explorer\Browser pomocné objekty
    - Monitorování pro moduly nový prohlížeč Pomocník objekt plug-in pro Internet Explorer. Použít tooaccess hello Model DOM (Document Object) hello aktuální stránku a toocontrol navigace pro 32bitové aplikace spuštěné na 64bitových počítačích.
- Nastavení HKEY\_místní\_MACHINE\Software\Microsoft\Internet Explorer\Extensions
    - Monitorování pro nové rozšíření, Internet Explorer, jako je například vlastní nástroj nabídky a tlačítka panelu nástrojů vlastní.
- Nastavení HKEY\_místní\_MACHINE\Software\Wow6432Node\Microsoft\Internet Explorer\Extensions
    - Monitorování pro nové rozšíření, Internet Explorer, jako jsou nabídky vlastního nástroje a vlastní panel nástrojů tlačítka pro 32bitové aplikace spuštěné na 64bitových počítačích.
- Nastavení HKEY\_místní\_MACHINE\Software\Microsoft\Windows NT\CurrentVersion\Drivers32
    - Monitoruje hello 32bitové ovladače přidružené wavemapper, wave1 a wave2, msacm.imaadpcm, .msadpcm, .msgsm610 a čtyřznakového. Podobně jako část toohello [ovladače] v hello systému. Soubor INI.
- Nastavení HKEY\_místní\_MACHINE\Software\Wow6432Node\Microsoft\Windows NT\CurrentVersion\Drivers32
    - Monitorování hello 32bitové ovladače přidružené wavemapper, wave1 a wave2, msacm.imaadpcm, .msadpcm, .msgsm610 a čtyřznakového pro 32bitové aplikace spuštěné na 64bitových počítačích. Podobně jako část toohello [ovladače] v hello systému. Soubor INI.
- Nastavení HKEY\_místní\_MACHINE\System\CurrentControlSet\Control\Session Manager\KnownDlls
    - Monitorování hello seznam známých nebo běžně používané systémové knihovny DLL; Tento systém brání osobám v zneužitím slabé aplikace directory oprávnění podle vyřazením trojský kůň verze systémové knihovny DLL.
- Nastavení HKEY\_místní\_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon\Notify
    - Monitorování hello seznam balíčků možné tooreceive události oznámení z přihlášení do systému Windows, model podporu hello interaktivní přihlášení pro operační systém Windows hello.


## <a name="use-change-tracking"></a>Pomocí sledování změn
Po instalaci hello řešení můžete zobrazit souhrn hello změny monitorovaných serverů pomocí hello **sledování změn** na hello dlaždici **přehled** stránky v OMS.

![Obrázek sledování změn dlaždice](./media/log-analytics-change-tracking/change-tracking-tile.png)

Můžete zobrazit změny tooyour infrastruktury a potom přejít k podrobnostem podrobnosti o hello následujících kategorií:

* Změny podle konfigurace pro software a služby systému Windows
* Změny softwaru tooapplications a aktualizace pro jednotlivé servery
* Celkový počet změny softwaru pro jednotlivé aplikace
* Balíčky Linux
* Změny pro jednotlivé servery služby Windows
* Změny linuxového démona

![Obrázek sledování změn řídicí panel](./media/log-analytics-change-tracking/change-tracking-dash01.png)

![Obrázek sledování změn řídicí panel](./media/log-analytics-change-tracking/change-tracking-dash02.png)

### <a name="tooview-changes-for-any-change-type"></a>změny tooview pro libovolný typ
1. Na hello **přehled** klikněte na tlačítko hello **sledování změn** dlaždici.
2. Na hello **změnit sledování** řídicí panel, zkontrolovat souhrnné informace hello v jednom z hello změnit typ okna a pak klikněte na jednu tooview podrobné informace o něm v hello **hledání protokolů** stránky.
3. Na žádném z hello protokolu hledání stránky můžete zobrazit výsledky čas, podrobné výsledky a historii hledání protokolu. Můžete také filtrovat podle výsledků hello toonarrow omezující vlastnosti.

## <a name="next-steps"></a>Další kroky
* Použití [přihlásit analýzy protokolů hledání](log-analytics-log-searches.md) tooview podrobné dat sledování změn.
