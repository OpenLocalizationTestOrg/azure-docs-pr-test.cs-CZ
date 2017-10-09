---
title: "aaaTroubleshoot Azure File storage problémy v systému Linux | Microsoft Docs"
description: "Řešení potíží s Azure File storage problémy v systému Linux"
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
ms.date: 07/11/2017
ms.author: genli
ms.openlocfilehash: 4bdc3c6ed2e48f245060a03632fca9bd14d33545
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-azure-file-storage-problems-in-linux"></a>Řešení potíží s Azure File storage problémy v systému Linux

V tomto článku jsou uvedeny běžné problémy, které jsou související tooMicrosoft Azure File storage při připojení z klientů Linux. Umožňuje také možné příčiny a řešení těchto problémů.

<a id="permissiondenied"></a>
## <a name="permission-denied-disk-quota-exceeded-when-you-try-tooopen-a-file"></a>"[bylo odepřeno oprávnění] byla překročena disková kvóta" když se pokusíte tooopen soubor

V systému Linux obdržíte chybovou zprávu, která se podobá následující hello:

**<filename>[bylo odepřeno oprávnění] Byla překročena kvóta disku**

### <a name="cause"></a>Příčina

Dosáhli jste hello horní limit počtu souběžných otevřete obslužných rutin, které jsou povoleny pro soubor.

### <a name="solution"></a>Řešení

Snižte hello počet souběžných otevřených obslužných rutin ukončením některé obslužné rutiny a zopakujte operaci hello. Další informace najdete v tématu [kontrolní seznam výkonu a škálovatelnosti služby Microsoft Azure Storage](../common/storage-performance-checklist.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

<a id="slowfilecopying"></a>
## <a name="slow-file-copying-tooand-from-azure-file-storage-in-linux"></a>Pomalé souboru kopírování tooand z Azure File storage v systému Linux

-   Pokud nemáte konkrétní požadavky minimální velikost vstupně-výstupních operací, doporučujeme použít 1 MB, protože hello velikost vstupně-výstupní operace pro zajištění optimálního výkonu.
-   Pokud znáte hello konečné velikosti souboru, který bude rozšiřovat s použitím zápisy a váš software není zaznamenat problémy s kompatibilitou, když unwritten poškozené databáze za na hello soubor obsahuje nulami, nastavte velikost souboru hello předem místo provedení každém zápisu rozšíření zápis.
-   Použijte metodu pravé kopie hello:
    -   Použití [AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#file-copy) pro všechny přenosy mezi dvěma sdílenými složkami.
    -   Použití [Robocopy](https://blogs.msdn.microsoft.com/granth/2009/12/07/multi-threaded-robocopy-for-faster-copies/) mezi sdílené složky na místním počítači.

<a id="error112"></a>
## <a name="mount-error112-host-is-down-because-of-a-reconnection-time-out"></a>"Připojit error(112): hostitel je dolů" z důvodu vypršení časového limitu opětovné připojení

"112" připojení dojde k chybě v klientovi Linux hello hello klienta bylo nečinné po dlouhou dobu. Po delší dobu nečinnosti hello klient odpojí a časový limit připojení hello.  

### <a name="cause"></a>Příčina

Hello připojení může být v nečinnosti hello následujících důvodů:

-   Selhání komunikace sítě, které zabraňují znovu zřízení serveru toohello připojení TCP, při použití možnosti "soft" připojení výchozí hello
-   Nejnovější opravy opětovné připojení, které se nenacházejí v starší jádra

### <a name="solution"></a>Řešení

Tento problém opětovným připojením hello Linux jádra systému vyřešen nyní jako součást hello následující změny:

- [Oprava znovu připojit toonot odložení smb3 relace znovu připojit po obnovení připojení soketu](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/fs/cifs?id=4fcd1813e6404dd4420c7d12fb483f9320f0bf93)
-   [Volání služby echo ihned po obnovení připojení soketu](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=b8c600120fc87d53642476f48c8055b38d6e14c7)
-   [CIFS: Oprava poškození možné paměti při volání metody reconnect](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/?id=53e0e11efe9289535b060a51d4cf37c25e0d0f2b)
-   [CIFS: Opravte možné dvojité uzamykání mutex během opětovného připojení (pro v4.9 jádra a novější)](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/?id=96a988ffeb90dba33a71c3826086fe67c897a183)

Tyto změny však nemusí přesně, ještě tooall hello Linuxových distribucích. Tato oprava a ostatní opravy opětovné připojení jsou vytvářeny v hello následující oblíbených jádra Linux: 4.4.40, 4.8.16 a 4.9.1. Můžete získat tuto opravu upgradem tooone tyto verze doporučené jádra.

### <a name="workaround"></a>Alternativní řešení

Tento problém můžete vyřešit tak, že zadáte pevné připojení. Dokud nebude navázáno připojení, nebo dokud ji explicitně přerušení a může být použité tooprevent chyb z důvodu vypršení časových limitů sítě vynutí toowait hello klienta. Toto řešení však může způsobit neomezené čeká. Mít připravený toostop připojení v případě potřeby.

Pokud nemůžete provést upgrade toohello nejnovější verze jádra, můžete tento problém obejít tím, že soubor v hello sdílenou složku Azure zápisu tooevery 30 sekund nebo méně. Operace zápisu, musí se jednat například přepisování hello vytvořit nebo změnit data v souboru hello. Jinak může získat výsledky uložené v mezipaměti a operaci nemusí aktivovat hello opětovné připojení.

<a id="error115"></a>
## <a name="mount-error115-operation-now-in-progress-when-you-mount-azure-file-storage-by-using-smb-30"></a>"Připojit error(115): operace právě probíhá" při připojení Azure File storage pomocí protokolu SMB 3.0

### <a name="cause"></a>Příčina

Některých distribucích systému Linux zatím nepodporují funkcí šifrování v SMB 3.0 a uživatelé mohou zobrazit "115" chybová zpráva, pokud se uživatel pokusí toomount Azure File storage pomocí protokolu SMB 3.0 z důvodu chybějící funkce.

### <a name="solution"></a>Řešení

Šifrování funkce pro protokol SMB 3.0 pro Linux byla zavedena v 4.11 jádra. Tato funkce umožňuje připojení Azure sdílené položky z místní nebo jiné oblasti Azure. V době hello publikování tato funkce byla přeneseny zpět tooUbuntu č. 17.04 a Ubuntu 16.10. Pokud váš klient Linux SMB nepodporuje šifrování, připojit pomocí protokolu SMB 2.1 z Linux virtuálního počítače Azure, který je v Azure File storage hello stejném datovém centru jako hello účet úložiště souborů.

<a id="slowperformance"></a>
## <a name="slow-performance-on-an-azure-file-share-mounted-on-a-linux-vm"></a>Nízký výkon na sdílenou složku Azure připojit na virtuální počítač s Linuxem

### <a name="cause"></a>Příčina

Možnou příčinou nízký výkon je zakázána, ukládání do mezipaměti.

### <a name="solution"></a>Řešení

toocheck jestli ukládání do mezipaměti je zakázaná, vyhledejte hello **mezipaměti =** položku. 

**Mezipaměť = none** označuje, že se zakáže ukládání do mezipaměti.  Sdílené složky hello opětovné připojení pomocí příkazu připojit výchozí hello nebo explicitně přidáním hello **mezipaměti = striktní** je povolená možnost toohello připojení příkaz tooensure, který je výchozí režim ukládání do mezipaměti nebo "striktní" ukládání do mezipaměti.

V některých scénářích hello **serverino** možnost připojení může způsobit hello **ls** stat toorun příkaz pro každou položku adresáře. Toto chování výsledkem snížení výkonu, když jste výpis velký adresář. Možnosti připojení hello můžete zkontrolovat ve vaší **/etc/fstab** položku:

`//azureuser.file.core.windows.net/cifs /cifs cifs vers=3.0,serverino,username=xxx,password=xxx,dir_mode=0777,file_mode=0777`

Můžete také zkontrolovat, zda se správnými možnostmi hello používají spuštěním hello **sudo připojení | grep cifs** příkazu a kontrola její výstup, jako je například hello následující příklad výstupu:

`//mabiccacifs.file.core.windows.net/cifs on /cifs type cifs (rw,relatime,vers=3.0,sec=ntlmssp,cache=strict,username=xxx,domain=X,uid=0,noforceuid,gid=0,noforcegid,addr=192.168.10.1,file_mode=0777, dir_mode=0777,persistenthandles,nounix,serverino,mapposix,rsize=1048576,wsize=1048576,actimeo=1)`

Pokud hello **mezipaměti = striktní** nebo **serverino** možnost je k dispozici, odpojte a znovu připojit Azure File storage spuštěním příkazu hello připojení z hello [dokumentaci](../storage-how-to-use-files-linux.md). Potom spusťte opětovnou kontrolu této hello **/etc/fstab** položka má hello správnými možnostmi.

<a id="timestampslost"></a>
## <a name="time-stamps-were-lost-in-copying-files-from-windows-toolinux"></a>Časová razítka ztratily v kopírování souborů z Windows tooLinux

Na platformách, Linux a Unix, hello **cp -p** příkaz se nezdaří, pokud soubor 1 a 2 jsou vlastněny různé uživatele.

### <a name="cause"></a>Příčina

Hello příznak force **f** v COPYFILE výsledkem provádění **cp -p -f** v systému Unix. Tento příkaz se nezdaří ani toopreserve hello časové razítko hello souboru, kterou nevlastníte.

### <a name="workaround"></a>Alternativní řešení

Použijte hello uživatelský účet úložiště pro kopírování souborů hello:

- `Useadd : [storage account name]`
- `Passwd [storage account name]`
- `Su [storage account name]`
- `Cp -p filename.txt /share`

## <a name="need-help-contact-support"></a>Potřebujete pomoct? Obraťte se na podporu.

Pokud stále potřebujete pomoc, [obraťte se na podporu](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget rychle vyřešit váš problém.
