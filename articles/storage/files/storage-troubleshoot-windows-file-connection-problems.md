---
title: "aaaTroubleshoot Azure File storage problémy v systému Windows | Microsoft Docs"
description: "Řešení potíží s Azure File storage problémy v systému Windows"
services: storage
documentationcenter: 
author: genlin
manager: willchen
editor: na
tags: storage
ms.service: storage
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: genli
ms.openlocfilehash: 19529d8af5d98790e2e381cd21ad4d0284acb124
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-azure-file-storage-problems-in-windows"></a>Poradce při potížích úložiště Azure File ve Windows

V tomto článku jsou uvedeny běžné problémy, které jsou související tooMicrosoft Azure File storage při připojení klientů se systémem Windows. Umožňuje také možné příčiny a řešení těchto problémů. Kromě toho toohello řešení potíží s kroky v tomto článku, můžete také použít [AzFileDiagnostics](https://gallery.technet.microsoft.com/Troubleshooting-tool-for-a9fa1fe5) zajistit, že Windows hello klienta prostředí má správné požadavky. AzFileDiagnostics automatizuje detekce většinu hello příznaků uvedených v tomto článku a pomáhá nastavení vašeho prostředí tooget optimální výkon. Tyto informace můžete také najít v hello [Azure sdíleným Poradce při potížích s](https://support.microsoft.com/help/4022301/troubleshooter-for-azure-files-shares) poskytující tooassist kroky můžete s problémy sdílí soubory Azure připojení, mapování/připojení.


<a id="error53-67-87"></a>
## <a name="error-53-error-67-or-error-87-when-you-mount-or-unmount-an-azure-file-share"></a>Chyba 53, chyba 67 nebo 87 Chyba při připojení nebo odpojení sdílenou složku Azure

Pokusíte-li toomount sdílenou z místní nebo jiném datovém centru, může se zobrazit hello následujícím chybám:

- Došlo k systémové chybě 53. Hello síťová cesta nebyla nalezena.
- Došlo k systémové chybě 67. Název sítě Hello nebyl nalezen.
- Došlo k systémové chybě 87. Hello parametr je nesprávný.

### <a name="cause-1-unencrypted-communication-channel"></a>Příčina 1: Nezašifrované komunikační kanál

Připojení tooAzure sdílené složky jsou zablokované, pokud není šifrován hello komunikační kanál, a pokud se pokus o připojení hello není z bezpečnostních důvodů hello stejného datového centra, kde jsou umístěny hello sdílené složky Azure. Šifrování kanálu komunikace je k dispozici pouze v případě, že hello uživatele klientského operačního systému podporuje šifrování protokolu SMB.

Windows 8, Windows Server 2012 a novějších verzích každý systém vyjednávat požadavků, které zahrnují protokolu SMB 3.0, který podporuje šifrování.

### <a name="solution-for-cause-1"></a>Řešení pro příčina 1

Připojení z klienta, který provede některou z následujících hello:

- Splňuje požadavky hello Windows 8 a Windows Server 2012 nebo novější verze
- Připojí z virtuálního počítače v hello stejné datovém centru jako hello účtu úložiště Azure, který se používá pro sdílenou složku hello Azure

### <a name="cause-2-port-445-is-blocked"></a>2 příčina: Port 445 je blokován.

Systémová chyba 53 nebo systémové chybě 67 může dojít, pokud je blokovaný port 445 odchozí komunikaci tooan Azure File storage datacenter. Souhrn hello toosee poskytovatelů internetových služeb, které povolí nebo zakáže přístup z port 445, přejděte příliš[TechNet](http://social.technet.microsoft.com/wiki/contents/articles/32346.azure-summary-of-isps-that-allow-disallow-access-from-port-445.aspx).

toounderstand ať to je důvod hello za zpráva "Chyba systému 53" hello, můžete použít Portqry tooquery hello TCP:445 koncový bod. Pokud koncový bod TCP:445 hello se zobrazí jako filtrované, hello TCP port je blokován. Tady je příklad dotazu:

  `g:\DataDump\Tools\Portqry>PortQry.exe -n [storage account name].file.core.windows.net -p TCP -e 445`

Pokud TCP port 445 je blokován pravidlem podél hello síťovou cestu, zobrazí se následující výstup hello:

  `TCP port 445 (microsoft-ds service): FILTERED`

Další informace o tom, najdete v části toouse Portqry, [Popis nástroje příkazového řádku Portqry.exe hello](https://support.microsoft.com/help/310099).

### <a name="solution-for-cause-2"></a>Řešení pro příčina 2

Práce s vaše IT oddělení tooopen port 445 odchozí příliš[rozsahy Azure IP](https://www.microsoft.com/download/details.aspx?id=41653).

### <a name="cause-3-ntlmv1-is-enabled"></a>Příčina 3: NTLMv1 je povolen.

Systémová chyba 53 nebo systémové chybě 87 může dojít, pokud NTLMv1 komunikace je zapnuta hello klienta. Úložiště Azure File podporuje jenom ověřování NTLMv2. S NTLMv1 povoleno vytvoří klienta méně bezpečné. Proto je blokován komunikace pro Azure File storage. 

toodetermine jestli je to hello příčinu chyby hello, ověřte, že tento hello následující podklíč registru je nastavena hodnota tooa 3:

**HKLM\SYSTEM\CurrentControlSet\Control\Lsa > LmCompatibilityLevel**

Další informace najdete v tématu hello [LmCompatibilityLevel](https://technet.microsoft.com/library/cc960646.aspx) tématu na webu TechNet.

### <a name="solution-for-cause-3"></a>Řešení pro příčina 3

Vrátit hello **LmCompatibilityLevel** hodnotu toohello výchozí hodnotu 3 v hello následující podklíč registru:

  **HKLM\SYSTEM\CurrentControlSet\Control\Lsa**

<a id="error1816"></a>
## <a name="error-1816-not-enough-quota-is-available-tooprocess-this-command-when-you-copy-tooan-azure-file-share"></a>Chyba 1816 "není dostatek kvóty je k dispozici tooprocess tento příkaz" Pokud zkopírujete tooan Azure sdílené složky

### <a name="cause"></a>Příčina

Chyba 1816 se stane, když dostanete hello horní limit počtu souběžných otevřete obslužných rutin, které jsou povoleny pro soubor v počítači hello kde připojené hello sdílené složky.

### <a name="solution"></a>Řešení

Snižte hello počet souběžných otevřených obslužných rutin ukončením některé obslužné rutiny a poté opakujte. Další informace najdete v tématu [kontrolní seznam výkonu a škálovatelnosti služby Microsoft Azure Storage](../common/storage-performance-checklist.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

<a id="slowfilecopying"></a>
## <a name="slow-file-copying-tooand-from-azure-file-storage-in-windows"></a>Pomalé souboru kopírování tooand z Azure File storage ve Windows

Při pokusu o tootransfer soubory toohello Azure File service, může se zobrazit nízký výkon.

- Pokud nemáte konkrétní požadavky minimální velikost vstupně-výstupních operací, doporučujeme použít 1 MB, protože hello velikost vstupně-výstupní operace pro zajištění optimálního výkonu.
-   Pokud znáte hello konečné velikosti souboru, který bude rozšiřovat s zapíše a váš software nemá problémy s kompatibilitou, když hello unwritten tail na hello soubor obsahuje nuly, potom sadu hello velikost souboru předem místo provedení každém zápisu rozšíření zápisu.
-   Použijte metodu pravé kopie hello:
    -   Použití [AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#file-copy) pro všechny přenosy mezi dvěma sdílenými složkami.
    -   Použití [Robocopy](https://blogs.msdn.microsoft.com/granth/2009/12/07/multi-threaded-robocopy-for-faster-copies/) mezi sdílené složky na místním počítači.

### <a name="considerations-for-windows-81-or-windows-server-2012-r2"></a>Důležité informace pro Windows 8.1 nebo Windows Server 2012 R2

Pro klienty se systémem Windows 8.1 nebo Windows Server 2012 R2, ujistěte se, že hello [KB3114025](https://support.microsoft.com/help/3114025) je nainstalována oprava hotfix. Tato oprava hotfix zlepšuje hello Create a zavřete obslužné rutiny.

Můžete spustit následující skript toocheck, zda byla nainstalována oprava hotfix hello hello:

`reg query HKLM\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters\Policies`

Pokud je nainstalována oprava hotfix, hello následující výstup se zobrazí:

`HKEY_Local_MACHINE\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters\Policies {96c345ef-3cac-477b-8fcd-bea1a564241c} REG_DWORD 0x1`

> [!Note]
> Bitové kopie systému Windows Server 2012 R2 v Azure Marketplace nainstalována oprava hotfix KB3114025 nainstalované ve výchozím nastavení, od prosince 2015.

<a id="shareismissing"></a>
## <a name="no-folder-with-a-drive-letter-in-my-computer"></a>Žádná složka s písmenem jednotky v **tento počítač**

Pokud namapujete sdílenou složku Azure jako správce pomocí příkazu net use, sdílené složky hello se zobrazí toobe chybí.

### <a name="cause"></a>Příčina

Ve výchozím prohlížeči souborů Windows nejde spustit jako správce. Spuštěním příkazu net use z příkazového řádku pro správu, mapovat síťovou jednotku hello jako správce. Protože připojené jednotky jsou zaměřené na uživatele, nejsou zobrazeny hello uživatelský účet, který je přihlášen hello jednotky, pokud jsou připojeny v jiného uživatelského účtu.

### <a name="solution"></a>Řešení
Připojte sdílenou složku hello z příkazového řádku bez oprávnění správce. Alternativně můžete postupovat podle [Toto téma TechNet](https://technet.microsoft.com/library/ee844140.aspx) tooconfigure hello **EnableLinkedConnections** hodnotu registru.

<a id="netuse"></a>
## <a name="net-use-command-fails-if-hello-storage-account-contains-a-forward-slash"></a>Pomocí příkazu net use selže, pokud účet úložiště hello obsahuje lomítkem

### <a name="cause"></a>Příčina

pomocí příkazu net use Hello interpretuje jako možnost příkazového řádku lomítkem (/). Pokud vaše uživatelské jméno účtu začíná lomítkem, mapování jednotky hello se nezdaří.

### <a name="solution"></a>Řešení

Můžete použít některý z následujících kroků toowork vyřešit problém hello hello:

- Spusťte následující příkaz prostředí PowerShell hello:

  `New-SmbMapping -LocalPath y: -RemotePath \\server\share -UserName accountName -Password "password can contain / and \ etc" `

  Z dávkového souboru můžete spustit příkaz hello tímto způsobem:

  `Echo new-smbMapping ... | powershell -command –`

- Uveďte dvojité uvozovky hello klíče toowork vyřešit tento problém – pokud je první znak hello hello lomítkem. Pokud se jedná, buď použijte hello interaktivním režimu a zadejte své heslo samostatně nebo obnovit vaše klíče tooget klíč, který nezačíná lomítkem.

<a id="cannotaccess"></a>
## <a name="application-or-service-cannot-access-a-mounted-azure-file-storage-drive"></a>Aplikace nebo služba nedostupná připojenou jednotku úložiště Azure File

### <a name="cause"></a>Příčina

Na uživatele jsou připojené jednotky. Pokud vaše aplikace nebo služba běží pod jiným uživatelským účtem než hello ten, který připojenou jednotku hello, nezobrazí aplikace hello hello jednotky.

### <a name="solution"></a>Řešení

Použijte jednu z následujících řešení hello:

-   Připojit jednotku hello z hello stejným uživatelským účtem, který obsahuje aplikace hello. Můžete použít nástroje, jako je PsExec.
- Předejte hello název účtu úložiště a klíč uživatele hello jméno a heslo parametry příkazu net use hello.

Po budete postupovat podle těchto pokynů, může se zobrazit následující chybová zpráva při spuštění příkazu net use pro účet služby systému nebo síťových hello hello: "došlo k systémové chybě 1312. Zadané přihlašovací relace neexistuje. Ho může již byla ukončena." Pokud k tomu dojde, ujistěte se, tímto uživatelským jménem, který je předán toonet použití hello obsahuje informace o doméně (například: "[název účtu úložiště]. file.core.windows .net").

<a id="doesnotsupportencryption"></a>
## <a name="error-you-are-copying-a-file-tooa-destination-that-does-not-support-encryption"></a>Chyba "Kopírujete cíl tooa souboru, který nepodporuje šifrování"

Po zkopírování souboru přes síť hello hello souboru jsou dešifrována na hello zdrojový počítač, přenáší ve formátu prostého textu a znovu zašifrována v cílovém umístění hello. Však může se zobrazit následující chyby, pokud se pokoušíte toocopy zašifrovaný soubor hello: "Kopírujete hello cíl tooa souboru, který nepodporuje šifrování."

### <a name="cause"></a>Příčina
Tomuto problému může dojít, pokud používáte systému souborů EFS (ENCRYPTING File System). Šifrované nástrojem BitLocker soubory mohou být tooAzure zkopírovaný soubor úložiště. Azure File storage však nepodporuje systém souborů EFS systému souborů NTFS.

### <a name="workaround"></a>Alternativní řešení
toocopy soubor hello síti, můžete musí nejprve ho dešifrovat. Použijte jednu z následujících metod hello:

- Použití hello **zkopírujte /d** příkaz. Umožňuje hello šifrované soubory toobe uložena jako dešifrované soubory v cílovém umístění hello.
- Nastavte hello následující klíč registru:
  - Cesta = HKLM\Software\Policies\Microsoft\Windows\System
  - Typ hodnoty = DWORD
  - Název = CopyFileAllowDecryptedRemoteDestination
  - Hodnota = 1

Mějte na paměti, že tento klíč registru hello nastavení ovlivňuje všechny operace kopírování, které jsou vytvářeny toonetwork sdílené složky.

## <a name="need-help-contact-support"></a>Potřebujete pomoct? Obraťte se na podporu.
Pokud stále potřebujete pomoc, [obraťte se na podporu](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget rychle vyřešit váš problém.
