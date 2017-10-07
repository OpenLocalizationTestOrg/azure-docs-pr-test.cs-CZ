---
title: "Funkce toohelp aaaSecurity chránit hybridní zálohování, které používají Azure Backup | Microsoft Docs"
description: "Zjistěte, jak funkce toouse zabezpečení v Azure Backup toomake zálohy bezpečnější"
services: backup
documentationcenter: 
author: JPallavi
manager: vijayts
editor: 
ms.assetid: 47bc8423-0a08-4191-826d-3f52de0b4cb8
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/08/2017
ms.author: pajosh
ms.openlocfilehash: 17a0f5e877f84af53c15062ec4a8df480383125e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="security-features-toohelp-protect-hybrid-backups-that-use-azure-backup"></a>Zabezpečení funkce toohelp chránit hybridní zálohování, které používají Azure Backup
Pochybnostmi problémy se zabezpečením, jako je malware, Ransomware, který a narušení, se zvyšuje. Tyto problémy se zabezpečením může být drahé z hlediska peníze a data. tooguard před těmito útoky Azure Backup teď poskytuje zabezpečení funkce toohelp Chraňte hybridní zálohy. Tento článek popisuje, jak tooenable a použijte tyto funkce, pomocí agenta služeb zotavení Azure a serveru Azure Backup. Tyto funkce patří:

- **Prevence**. Další úroveň ověřování je přidána vždy, když se provádí kritické operace, jako je změna přístupové heslo. Toto ověření je tooensure, které mohou být tyto operace provést pouze uživatelé, kteří mají platné přihlašovací údaje Azure.
- **Výstrahy**. E-mailové oznámení odeslána, se provádí správce předplatného toohello vždy, když kritické operace jako odstraňují se záložní data. Tento e-mail zajistí, že tento uživatel hello rychle dozví o takové akce.
- **Obnovení**. Odstraněné zálohovaná data se uchovávají pro další 14 dní od data hello hello odstranění. Tím se zajistí obnovitelnost hello data v daném časovém období, takže není nedošlo ke ztrátě dat, i když se stane útoku. Navíc větší počet bodů obnovení minimální udržované tooguard proti poškozená data.

> [!NOTE]
> Funkce zabezpečení by nemělo být povoleno, pokud používáte infrastrukturu jako službu (IaaS) zálohování virtuálních počítačů. Tyto funkce ještě nejsou k dispozici pro zálohování virtuálních počítačů IaaS, takže povolením nebude mít žádný vliv. Funkce zabezpečení lze povolit pouze v případě, že používáte: <br/>
>  * **Azure Backup agent**. Verze agenta minimální 2.0.9052. Po povolení těchto funkcí, měli byste upgradovat toothis agenta verze tooperform kritické operace. <br/>
>  * **Azure Backup Server**. Minimální Azure Backup agent verze 2.0.9052 s Azure Backup Server aktualizací 1. <br/>
>  * **System Center Data Protection Manager**. Minimální Azure Backup agent verze 2.0.9052 Data Protection Manager 2012 R2 UR12 nebo UR2 Data Protection Manager 2016. <br/> 


> [!NOTE]
> Tyto funkce jsou dostupné pouze pro trezor služeb zotavení. Všechny hello nově vytvořený trezory služeb zotavení mají tyto funkce ve výchozím nastavení povolené. Pro existující trezory služeb zotavení uživatelé tyto funkce povolit pomocí hello kroků uvedených v následující části hello. Po hello, které funkce jsou povolené se vztahují počítače agenta služeb zotavení hello tooall, instance serveru Azure Backup a v úložišti hello zaregistrovaný servery Data Protection Manager. Povolením tohoto nastavení je jednorázová akce a tyto funkce nelze zakázat po povolení je.
>

## <a name="enable-security-features"></a>Povolení funkcí zabezpečení
Při vytváření trezoru služeb zotavení, můžete použít všechny funkce zabezpečení hello. Pokud pracujete s existující trezor, povolte funkce zabezpečení pomocí následujících kroků:

1. Přihlaste se toohello portálu Azure pomocí přihlašovacích údajů Azure.
2. Vyberte **Procházet**a typ **služeb zotavení**.

    ![Snímek obrazovky Azure portálu zpřístupní možnost procházení](./media/backup-azure-security-feature/browse-to-rs-vaults.png) <br/>

    Zobrazí se seznam Hello trezory služeb zotavení. Z tohoto seznamu vyberte trezor. Otevře se řídicí panel vybraného trezoru Hello.
3. Hello seznamu položek, které se zobrazí v části hello trezor, v části **nastavení**, klikněte na tlačítko **vlastnosti**.

    ![Možnosti trezoru služeb zotavení – snímek obrazovky](./media/backup-azure-security-feature/vault-list-properties.png)
4. V části **nastavení zabezpečení**, klikněte na tlačítko **aktualizace**.

    ![Vlastnosti trezoru služeb zotavení – snímek obrazovky](./media/backup-azure-security-feature/security-settings-update.png)

    Hello aktualizace odkaz otevře hello **nastavení zabezpečení** okno, které poskytuje shrnutí funkcí hello a umožňuje vám povolit je.
5. Z rozevíracího seznamu hello **nakonfigurovali ověřování Azure Multi-Factor Authentication?**, vyberte hodnotu tooconfirm, pokud jste povolili [Azure Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication.md). Pokud je povolena, zobrazí se výzva tooauthenticate z jiného zařízení (například mobilních telefonů) při přihlášení toohello portálu Azure.

   Když provedete kritické operace zálohování, máte tooenter zabezpečení PIN kódu, dostupné na portálu Azure hello. Povolení ověřování Azure Multi-Factor Authentication přidává další vrstvu zabezpečení. Jenom oprávnění uživatelé s platné přihlašovací údaje Azure a ověřit z druhé zařízení, mají přístup k hello portálu Azure.
6. Vyberte nastavení zabezpečení toosave **povolit** a klikněte na tlačítko **Uložit**. Můžete vybrat **povolit** pouze po výběru hodnoty z hello **nakonfigurovali ověřování Azure Multi-Factor Authentication?** seznamu v předchozím kroku hello.

    ![Snímek obrazovky nastavení zabezpečení](./media/backup-azure-security-feature/enable-security-settings-dpm-update.png)

## <a name="recover-deleted-backup-data"></a>Zálohovaná data obnovit odstraněnou
Zálohování uchovává odstraněné záložní data pro další 14 dnů a nedojde k odstranění jeho okamžitě, pokud hello **zastavení zálohování pomocí zálohování dat odstranit** operace. toorestore tato data v období 14 dnů hello, proveďte následující kroky, v závislosti na tom, co používáte hello:

Pro **agenta služeb zotavení Azure** uživatelů:

1. Pokud hello počítač, kde byly děje zálohy je stále k dispozici, použijte [obnovit data toohello stejný počítač](backup-azure-restore-windows-server.md#use-instant-restore-to-recover-data-to-the-same-machine) ve službě Azure Recovery Services, toorecover ze všech hello staré body obnovení.
2. Pokud tento počítač není k dispozici, použijte [obnovení tooan alternativní počítač](backup-azure-restore-windows-server.md#use-instant-restore-to-restore-data-to-an-alternate-machine) toouse jiný počítač tooget služeb zotavení Azure tato data.

Pro **serveru Azure Backup** uživatelů:

1. Pokud je stále k dispozici hello server, kde byly děje zálohování, znovu nastavit ochranu zdroje dat hello odstranit a používat hello **obnovit Data** funkci toorecover ze všech hello staré body obnovení.
2. Pokud tento server není k dispozici, použijte [obnovit data z jiného serveru Azure Backup](backup-azure-alternate-dpm-server.md) toouse jiného serveru Azure Backup instance tooget tato data.

Pro **Data Protection Manager** uživatelů:

1. Pokud je stále k dispozici hello server, kde byly děje zálohování, znovu nastavit ochranu zdroje dat hello odstranit a používat hello **obnovit Data** funkci toorecover ze všech hello staré body obnovení.
2. Pokud tento server není k dispozici, použijte [přidat externí DPM](backup-azure-alternate-dpm-server.md) toouse jiné tooget serveru Data Protection Manager tato data.

## <a name="prevent-attacks"></a>Zabránit útokům
Bylo přidáno kontroly toomake se, že pouze platné uživatelé mohou provádět různé operace. Jedná se o přidáním další úrovně ověřování a udržování minimální uchování pro účely obnovení.

### <a name="authentication-tooperform-critical-operations"></a>Kritické operace tooperform ověřování
Jako součást přidáním další úrovně ověřování pro kritické operace, jste výzvami tooenter zabezpečení PIN kódu při provádění **zastavit ochranu s odstranit data** a **změnit heslo** operace .

tooreceive Tento PIN kód:

1. Přihlaste se toohello portálu Azure.
2. Procházet příliš**trezor služeb zotavení** > **nastavení** > **vlastnosti**.
3. V části **zabezpečení PIN**, klikněte na tlačítko **generování**. Otevře se okno, které obsahuje toobe hello kódu PIN zadaná v hello služeb zotavení Azure agenta uživatelské rozhraní.
    Tento PIN kód je platný pouze pět minut, a získá automaticky generovány po uplynutí této doby.

### <a name="maintain-a-minimum-retention-range"></a>Udržovat minimální uchování
tooensure, že stále existují platný počet obnovení body k dispozici, byly přidány hello následující kontroly:

- Pro denní uchování, minimálně **sedm** dní uchovávání by mělo být provedeno.
- Pro týdenní uchování, minimálně **čtyři** týdny uchovávání by mělo být provedeno.
- Pro měsíční uchování, minimálně **tři** měsíců uchovávání by mělo být provedeno.
- Pro roční uchování, minimálně **jeden** rok uchovávání by mělo být provedeno.

## <a name="notifications-for-critical-operations"></a>Oznámení pro kritické operace
Obvykle když kritické operace, dobrý den, správce předplatného je odeslat e-mail s oznámením o podrobnosti o operaci hello. Další e-mailovou příjemce těchto oznámení můžete nakonfigurovat pomocí hello portálu Azure.

funkce zabezpečení Hello uvedených v tomto článku poskytují mechanismy obrany proti cílenými útoky. Je důležité, pokud se stane útoku, tyto funkce umožňují hello možnost toorecover vaše data.

## <a name="troubleshooting-errors"></a>Řešení potíží s chybami
| Operace | Podrobnosti o chybě | Řešení |
| --- | --- | --- |
| Změny zásad |zásady zálohování Hello se nepodařilo upravit. Chyba: hello aktuální operace se nezdařila z důvodu tooan vnitřní chybě služby [0x29834]. Opakujte operaci hello po určité době. Pokud hello potíže potrvají, kontaktujte prosím podporu společnosti Microsoft. |**Příčina:**<br/>Tato chyba se dodává při nastavení zabezpečení povoleno, zkuste rozsah uchování tooreduce nižší než minimální hodnoty hello uveden výše a na nepodporovanou verzi (podporované verze jsou určené v první poznamenejte si tento článek). <br/>**Doporučená akce:**<br/> V takovém případě by měl nastavit dobu uchování výše hello minimální doba uchovávání období (sedm dní denně, čtyři týdny týdně, tři týdny, měsíčně nebo jeden rok pro roční) tooproceed zásadám související s aktualizací. Volitelně žádoucí by tooupdate agenta zálohování, Azure Backup Server nebo DPM UR tooleverage všech hello zabezpečení aktualizace. |
| Změnit heslo |Zabezpečení zadán kód PIN není správný. (ID: 100130) Zadejte správný kód PIN zabezpečení toocomplete hello tuto operaci. |**Příčina:**<br/> Tato chyba se dodává při zadání kódu PIN zabezpečení neplatná nebo vypršela její platnost při provádění kritické operace (jako jsou změnit heslo). <br/>**Doporučená akce:**<br/> toocomplete hello operace, je nutné zadat platný kód PIN zabezpečení. tooget hello PIN kód, přihlášení tooAzure portál a přejděte trezor služeb tooRecovery > Nastavení > Vlastnosti > generování kódu PIN zabezpečení. Pomocí tohoto kódu PIN toochange hesla. |
| Změnit heslo |Operace se nezdařila. ID: 120002 |**Příčina:**<br/>Tato chyba se dodává při nastavení zabezpečení povoleno, zkuste heslo toochange a na nepodporovanou verzi (platná verze zadané v první poznamenejte si tento článek).<br/>**Doporučená akce:**<br/> toochange heslo, musíte nejdřív aktualizovat verze agenta zálohování toominimum minimální 2.0.9052, Azure Backup server toominimum update 1 nebo DPM toominimum DPM 2012 R2 UR12 nebo UR2 2016 aplikace DPM (stažení odkazy níže), zadejte platný kód PIN zabezpečení. tooget hello PIN kód, přihlášení tooAzure portál a přejděte trezor služeb tooRecovery > Nastavení > Vlastnosti > generování kódu PIN zabezpečení. Pomocí tohoto kódu PIN toochange hesla. |

## <a name="next-steps"></a>Další kroky
* [Začínáme s trezor služeb zotavení Azure](backup-azure-vms-first-look-arm.md) tooenable tyto funkce.
* [Stáhněte si nejnovější verzi agenta služeb zotavení Azure hello](http://aka.ms/azurebackup_agent) toohelp chránit počítače se systémem Windows a zabezpečte zálohovaných dat před útoky.
* [Stažení hello nejnovější Server Azure Backup](https://aka.ms/latest_azurebackupserver) toohelp chránit úlohy a zabezpečte zálohovaných dat před útoky.
* [Stáhněte si UR12 pro System Center 2012 R2 Data Protection Manager](https://support.microsoft.com/help/3209592/update-rollup-12-for-system-center-2012-r2-data-protection-manager) nebo [stáhnout UR2 pro System Center 2016 Data Protection Manager](https://support.microsoft.com/help/3209593/update-rollup-2-for-system-center-2016-data-protection-manager) toohelp chránit úlohy a zabezpečte zálohovaných dat před útoky.
