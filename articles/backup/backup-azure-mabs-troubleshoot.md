---
title: aaaTroubleshoot serveru Azure Backup | Microsoft Docs
description: "Řešení potíží s instalací, registraci Azure Backup Server a zálohování a obnovení úloh aplikace"
services: backup
documentationcenter: 
author: pvrk
manager: shreeshd
editor: 
ms.assetid: 2d73c349-0fc8-4ca8-afd8-8c9029cb8524
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/08/2017
ms.author: pullabhk;markgal;
ms.openlocfilehash: 2386cfcfbf7cc686b5c051d2e1a3a884481ccdc2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-azure-backup-server"></a>Odstraňování potíží Azure Backup Serveru

Řešení potíží s chyb zjištěných při pomocí serveru Azure Backup informace uvedené v následující tabulce hello.


## <a name="installation-issues"></a>Problémy instalace

| Operace | Podrobnosti o chybě | Alternativní řešení |
|-----------|---------------|------------|
|Instalace | Instalační program nemohl aktualizovat metadata registru. Tato aktualizace selhání mohlo tooover využití spotřebu úložiště. tooavoid tento prosím aktualizace hello ořezávání odolný systém souborů položky registru. | Upravte klíč registru hello, SYSTEM\CurrentControlSet\Control\FileSystem\RefsEnableInlineTrim. Nastavte hodnotu hello Dword too1. |
|Instalace | Instalační program nemohl aktualizovat metadata registru. Tato aktualizace selhání mohlo tooover využití spotřebu úložiště. tooavoid tento prosím aktualizace hello položky registru SnapOptimization svazku. | Vytvořte klíč registru hello SOFTWARE\Microsoft Manager\Configuration\VolSnapOptimization\WriteIds ochrany dat, s hodnotou prázdný řetězec. |

## <a name="registration-and-agent-related-issues"></a>Problémy související s registrací a Agent
| Operace | Podrobnosti o chybě | Alternativní řešení |
| --- | --- | --- |
| Registrace trezoru tooa | Neplatné přihlašovací údaje úložiště. Hello soubor je buď poškozený nebo není mít hello nejnovější pověření spojená se službou obnovení | toofix tuto chybu: <ol><li> Stáhněte si nejnovější soubor s přihlašovacími údaji hello z trezoru hello a zkuste to znovu <br>(NEBO)</li> <li> Pokud hello nad akcí nefunguje, zkuste stáhnout hello pověření tooa jiný místní adresář nebo vytvořit nový trezor <br>(NEBO)</li> <li> Zkuste aktualizovat hello datum a čas nastavení, jak je uvedeno v [tomto blogu](https://azure.microsoft.com/blog/troubleshooting-common-configuration-issues-with-azure-backup/) <br>(NEBO)</li> <li> Zkontrolujte, zda c:\windows\temp má více než 65000 soubory. Přesuňte umístění tooanother zastaralé soubory nebo odstraňte hello položek ve složce Temp hello <br>(NEBO)</li> <li> Zkontrolujte stav hello certifikátů <br> a. Otevřete "Spravovat certifikáty počítače" (v Ovládacích panelech hello) <br> b. Rozbalte uzel "Osobní" hello a jeho podřízený uzel "Certifikáty" <br> c.  Odebrat certifikát hello "Nástroje systému Windows Azure" <br> d. Opakujte registraci hello v klientovi Azure Backup hello <br> (NEBO) </li> <li> Zkontrolujte, zda všechna zásad skupiny, je na místě </li></ol> |
| Vkládání agentů tooprotected servery | Hello operace agenta se nezdařila kvůli chybě komunikace s hello službu DPM Agent Coordinator na \<ServerName > | **Pokud nebude fungovat hello Doporučená akce uvedené v produktu hello**, <ol><li> Pokud se připojujete počítači z nedůvěryhodné domény, postupujte podle [tyto](https://technet.microsoft.com/library/hh757801(v=sc.12).aspx) kroky <br> (NEBO) </li><li> Pokud se připojujete počítače z důvěryhodné domény, řešení problémů pomocí hello kroků uvedených v [tomto blogu](https://blogs.technet.microsoft.com/dpm/2012/02/06/data-protection-manager-agent-network-troubleshooting/) <br>(NEBO)</li><li> Zkuste zakázat antivirový v rámci řešení potíží. Pokud se řeší problém hello, upravte hello antivirový nastavení dle pokynů [v tomto článku] (https://technet.microsoft.com/library/hh757911.aspx)</li></ol> |
| Vkládání agentů tooprotected servery | Hello přihlašovací údaje pro server jsou neplatné | **Pokud nebude fungovat hello Doporučená akce uvedené v produktu hello**, <br> Zkuste tooinstall hello agenta ochrany ručně na provozním serveru hello zadané v [v tomto článku](https://technet.microsoft.com/library/hh758186(v=sc.12).aspx#BKMK_Manual)|
| Azure Backup Agent se služby Azure Backup nelze tooconnect toohello (ID: 100050) | Hello Azure Backup Agent se nepodařilo tooconnect toohello služby zálohování Azure. | **Pokud nebude fungovat hello Doporučená akce uvedené v produktu hello**, <br>1. Spusťte následující příkaz z řádku, zvýšené nástroje psexec -i -s "c:\Program Files\Internet Explorer\iexplore.exe" otevře se okno internet Exploreru. <br/> 2. Přejděte tooTools -> Možnosti Internetu -> připojení -> Nastavení místní sítě. <br/> 3. Ověřte nastavení proxy serveru pro účet System. Nastavení proxy serveru IP adresy a portu. <br/> 4. Zavřete Internet Explorer.|
| Instalace služby Azure Backup Agent se nezdařila | Hello instalace služeb zotavení Microsoft Azure se nezdařilo. Všechny změny provedené hello systému toohello instalace služeb zotavení Microsoft Azure byly vráceny zpět. (ID: 4024) | Ručně nainstalovat agenta Azure


## <a name="configuring-protection-group"></a>Konfigurace skupiny ochrany
| Operace | Podrobnosti o chybě | Alternativní řešení |
| --- | --- | --- |
| Konfigurace skupin ochrany | Aplikace DPM se nepodařil výčet součásti aplikace v chráněném počítači (chráněný počítač název) | Klikněte na 'Aktualizovat' na hello Konfigurace obrazovky uživatelského rozhraní pro skupinu ochrany na příslušné úrovni zdroj dat nebo součást hello |
| Konfigurace skupin ochrany | Nelze tooconfigure ochrany | Pokud je chráněný server hello systému SQL server, zkontrolujte, zda oprávnění role správce systému byly zadány toohello systémový účet (NTAuthority\System) v počítači hello chránit jak je uvedeno v [v tomto článku](https://technet.microsoft.com/library/hh757977(v=sc.12).aspx)
| Konfigurace skupin ochrany | Není dostatek volného místa ve fondu úložiště hello pro tuto skupinu ochrany | Hello disky, které jsou přidány fondu úložiště toohello [nesmí obsahovat oddíl](https://technet.microsoft.com/library/hh758075(v=sc.12).aspx). Odstraňte všechny existující svazky na discích hello a poté jej přidejte toohello fondu úložiště|
| Změny zásad |zásady zálohování Hello se nepodařilo upravit. Chyba: hello aktuální operace se nezdařila z důvodu tooan vnitřní chybě služby [0x29834]. Opakujte operaci hello po určité době. Pokud hello potíže potrvají, kontaktujte prosím podporu společnosti Microsoft. |**Příčina:**<br/>Tato chyba se dodává při nastavení zabezpečení povoleno, zkuste rozsah uchování tooreduce nižší než minimální hodnoty hello uveden výše a jsou v nepodporované verzi (nižší než verze MAB 2.0.9052 a Azure Backup server aktualizace 1). <br/>**Doporučená akce:**<br/> V takovém případě by měl nastavit dobu uchování výše hello minimální doba uchovávání období (sedm dní denně, čtyři týdny týdně, tři týdny, měsíčně nebo jeden rok pro roční) tooproceed zásadám související s aktualizací. Volitelně žádoucí by tooupdate agenta zálohování a Azure Backup Server tooleverage všechny hello aktualizace zabezpečení. |

## <a name="backup"></a>Zálohování
| Operace | Podrobnosti o chybě | Alternativní řešení |
| --- | --- | --- |
| Zálohování | Replika je nekonzistentní | Můžete najít další podrobnosti o příčinách hello nekonzistence repliky a relevantní návrhy [sem](https://technet.microsoft.com/library/cc161593.aspx) <br> <ol><li> V případě zálohování stavu systému nebo BMR zkontrolujte, zda je nainstalována zálohování serveru Windows či není na chráněný Server </li><li> Zkontrolujte místo související problémy na úložiště aplikace DPM na serveru aplikace DPM nebo MABS hello fondu a přidělit úložiště podle potřeby </li><li> Zkontrolujte stav hello hello služby Stínová kopie svazku na serveru hello chráněný. Pokud je v zakázaném stavu nastavit toostart ručně a spusťte službu hello na serveru hello. Poté přejděte zpět konzoly aplikace DPM nebo MABS toohello a spustí hello synchronizaci s kontrolou konzistence.</li></ol>|
| Zálohování | Došlo k neočekávané chybě během hello úlohy, hello zařízení není připraveno | **Pokud nebude fungovat hello Doporučená akce uvedené v produktu hello**, <br> <ol><li>Nastavte hello úložiště stínové kopie místo toounlimited hello položky ve skupině ochrany hello a spustit kontrolu konzistence hello <br></li> (NEBO) <li>Odstraňte existující skupiny ochrany hello a vytvořit více nové – jednu s jednotlivé položky v něm</li></ol> |
| Zálohování | Pokud zálohujete pouze stav systému, ověřte, pokud je dostatek volného místa na hello chráněný počítač toostore hello zálohy stav systému | <ol><li>Ověřte, zda že je nainstalován tento hello WSB na hello chráněný počítač</li><li>Ověřte, zda je dostatek místa na hello chráněný počítač k dispozici pro stav systému hello: hello toodo nejjednodušší způsob, jak je chráněný počítač toogo toohello, otevřete WSB a klikněte na tlačítko hello nastaveními a vyberte úplné obnovení systému. Hello uživatelského rozhraní vám potom oznámí, kolik místa je požadované pro tento. Otevřete WSB -> Místní zálohování-zálohování > plán -> Vybrat konfiguraci zálohování -> celého serveru (se zobrazí velikost). Použijte pro ověření této velikosti.</li></ol>
| Zálohování | Vytvoření bodu obnovení online se nezdařilo | Pokud je zobrazeno hello chybová zpráva "Windows Azure Backup Agent se nepodařilo toocreate snímek hello vybrané svazku", zkuste to prosím roste hello místo ve svazku bodu obnovení a repliky.
| Zálohování | Vytvoření bodu obnovení online se nezdařilo | Pokud je zobrazeno hello chybová zpráva "hello Windows Azure Backup Agent se nemůže připojit služba obengine uvedena toohello", ověřte, že hello obengine uvedena v seznamu hello spuštěných služeb v počítači hello existuje. Pokud hello služba obengine uvedena neběží použití hello "net start obengine uvedena" příkaz toostart hello služba obengine uvedena.
| Zálohování | Vytvoření bodu obnovení online se nezdařilo | Pokud je zobrazeno hello chybová zpráva "hello šifrovací přístupové heslo pro tento server není nastaven. Konfigurujte šifrovací přístupové heslo", zkuste konfigurace šifrovací přístupové heslo. Pokud se nezdaří, <br> <ol><li>Zkontrolujte, zda text hello pomocné umístění existuje, nebo ne. umístění Hello uvedený v registru hello HKEY_LOCAL_MACHINE\Software\Microsoft\Windows Azure Backup\Config s názvem "ScratchLocation" by měla existovat.</li><li> Pokud hello pomocné umístění existuje, pokuste se znovu registraci pomocí hello staré heslo. **Vždy, když konfigurujete šifrovací přístupové heslo, uložte jej na bezpečném místě**</li><ol>
| Zálohování | Selhání zálohování pro úplné obnovení systému | Pokud je velikost BMR velký, opakujte po přesunutí některé jednotky tooOS soubory aplikace |
| Zálohování | Chyba při přístupu k soubory nebo sdílené složky | Zkuste upravit nastavení antivirového programu hello jako navrhované [sem](https://technet.microsoft.com/library/hh757911.aspx)|
| Zálohování | Selhání úlohy vytvoření bodu obnovení online pro virtuální počítač VMware. Aplikace DPM došlo k chybě z VMware tooget vlastnost ChangeTracking informace. ErrorCode – FileFaultFault (ID 33621) |  1. Resetování hello ctk VMware, pro hello vliv na virtuální počítače <br/> Zkontrolujte, zda nezávislé disk není v místě VMWare <br/> Zastavte ochranu pro virtuální počítače hello vliv a znovu nastavit ochranu pomocí tlačítka Aktualizovat <br/> Spuštění kopie pro hello vliv na virtuální počítače|


## <a name="change-passphrase"></a>Změnit heslo
| Operace | Podrobnosti o chybě | Alternativní řešení |
| --- | --- | --- |
| Změnit heslo |Zabezpečení zadán kód PIN není správný. Zadejte správný kód PIN zabezpečení toocomplete hello tuto operaci. |**Příčina:**<br/> Tato chyba se dodává při zadání kódu PIN zabezpečení neplatná nebo vypršela její platnost při provádění kritické operace (jako jsou změnit heslo). <br/>**Doporučená akce:**<br/> toocomplete hello operace, je nutné zadat platný kód PIN zabezpečení. tooget hello PIN kód, přihlášení tooAzure portál a přejděte trezor služeb tooRecovery > Nastavení > Vlastnosti > generování kódu PIN zabezpečení. Pomocí tohoto kódu PIN toochange hesla. |
| Změnit heslo |Operace se nezdařila. ID: 120002 |**Příčina:**<br/>Tato chyba se dodává při nastavení zabezpečení povoleno, zkuste heslo toochange a na nepodporovanou verzi.<br/>**Doporučená akce:**<br/> toochange heslo, musí být první aktualizace verze agenta zálohování toominimum minimální 2.0.9052 a Azure Backup server toominimum aktualizace 1, pak zadejte platný kód PIN zabezpečení. tooget hello PIN kód, přihlášení tooAzure portál a přejděte trezor služeb tooRecovery > Nastavení > Vlastnosti > generování kódu PIN zabezpečení. Pomocí tohoto kódu PIN toochange hesla. |


## <a name="configure-email-notifications"></a>Konfigurace e-mailových oznámení
| Operace | Podrobnosti o chybě | Alternativní řešení |
| --- | --- | --- |
| Probíhá tooset si e-mailová oznámení pomocí účtu Office 365. | získání ID chyby: 2013| **Příčina:**<br/> Při operaci účet toouse Office 365 <br/> **Doporučená akce:**<br/> Hello nejprve thing tooensure je, že "Povolit anonymní předávání na přijímat konektor" pro váš server aplikace DPM je instalace na serveru Exchange. Zde je odkaz na postupy tooconfigure to: http://technet.microsoft.com/en-us/library/bb232021.aspx <br/> Pokud nelze použít interní předávací doména SMTP a potřebujete tooset díky serveru Office 365, nastavením služby IIS toobe relé pro tento. <br/> Budete potřebovat tooconfigure hello DPM serveru toobe možné toorelay hello SMTP tooO365 pomocí služby IIS https://technet.microsoft.com/en-us/library/aa995718(v=exchg.65).aspx toosetup IIS toorelay tooO365 <br/> Důležité upozornění: V kroku 3 -> g -> ii, že toouse být user@domain.com formátu a není doména\uživatel <br/> Aplikace DPM bodu toouse hello místní servername jako server SMTP, port 587 a pak hello uživatele e-mailu, který e-mailů hello musí pocházet z. <br/> Hello uživatelské jméno a heslo na stránce instalace aplikace DPM SMTP hello musí být účet domény v doméně hello, ve které aplikace DPM. <br/> Poznámka: Při změně adresy serveru hello SMTP, ujistěte se, hello toonew nastavení změnit, hello nastavení okno zavřít a znovu otevřete toobe se, že odráží hello novou hodnotu.  Jednoduše změna a testování vždycky nebude nová nastavení hello testování tímto způsobem je osvědčeným postupem. <br/> Kdykoli během tohoto procesu můžete vymazat toto nastavení zavřením konzoly aplikace DPM a úpravy hello následující klíče registru:<br/> HKLM\SOFTWARE\Microsoft\Microsoft Data Protection Manager\Notification\ <br/> Odstraňte SMTPPassword a SMTPUserName klíče. <br/> Můžete je přidat zpět v hello uživatelského rozhraní při znovu spusťte.
