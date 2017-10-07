---
title: aaaRecover dat ze serveru Azure Backup | Microsoft Docs
description: "Obnovení dat hello jste chránili trezor služeb zotavení tooa všechny registrované toothat trezoru serveru Azure Backup."
services: backup
documentationcenter: 
author: nkolli1
manager: shreeshd
editor: 
ms.assetid: a55f8c6b-3627-42e1-9d25-ed3e4ab17b1f
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/18/2017
ms.author: adigan;giridham;trinadhk;markgal
ms.openlocfilehash: 74847880e646c3c4f198afe318f1db30363d137a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="recover-data-from-azure-backup-server"></a>Obnovení dat z Azure Backup Serveru
Můžete použít Azure Backup Server toorecover hello data, která jste zálohovali tooa, které trezor služeb zotavení. proces Hello to tak je integrována do konzoly pro správu serveru Azure Backup hello a je podobný postupu obnovení toohello pro jiné komponenty Azure Backup.

> [!NOTE]
> Tento článek se použije pro [System Center Data Protection Manager 2012 R2 s kumulativní aktualizací 7 nebo novější] (https://support.microsoft.com/en-us/kb/3065246), v kombinaci s hello [nejnovější verze agenta Azure Backup](http://aka.ms/azurebackup_agent).
>
>

toorecover dat ze serveru Azure Backup:

1. Z hello **obnovení** klikněte na kartě hello konzoly pro správu serveru Azure Backup **přidat externí DPM** (v hello levém horním rohu obrazovky hello).   
    ![Přidat externí DPM](./media/backup-azure-alternate-dpm-server/add-external-dpm.png)
2. Stažení nové **přihlašovací údaje trezoru** z trezoru hello přidružené hello **serveru Azure Backup** kde hello data obnovena, zvolte hello serveru Azure Backup hello seznamu serverů pro zálohování Azure zaregistrovat u trezoru služeb zotavení hello a zadejte hello **šifrovací přístupové heslo** přidružené hello serveru, jehož data obnovena.

    ![Přihlašovací údaje externí DPM](./media/backup-azure-alternate-dpm-server/external-dpm-credentials.png)

   > [!NOTE]
   > Pouze servery zálohování Azure spojené s hello stejné registrace trezoru mohou obnovit data druhé strany.
   >
   >

    Jakmile hello externího serveru Azure Backup je úspěšně přidán, můžete procházet hello data z externího serveru hello a hello místní Server Azure Backup z hello **obnovení** kartě.
3. Procházet hello seznam dostupných provozních serverů, které jsou chráněny hello externího serveru Azure Backup a vyberte hello odpovídající zdroj dat.

    ![Procházet externí DPM Server](./media/backup-azure-alternate-dpm-server/browse-external-dpm.png)
4. Vyberte **Dobrý den, měsíc a rok** z hello **body obnovení** rozevírací nabídky vyberte hello požadované **obnovení datum** pro při hello bod obnovení byl vytvořený a vyberte možnost hello **Čas obnovení**.

    V dolním podokně hello, která lze procházet a obnovit tooany umístění se zobrazí seznam souborů a složek.

    ![Body obnovení serveru externí aplikace DPM](./media/backup-azure-alternate-dpm-server/external-dpm-recoverypoint.png)
5. Klikněte pravým tlačítkem na příslušnou položku hello a klikněte na tlačítko **obnovit**.

    ![Externí obnovení aplikace DPM](./media/backup-azure-alternate-dpm-server/recover.png)
6. Zkontrolujte hello **Obnovit výběr**. Ověřte hello data a času záložní kopie hello obnovuje, jakož i hello zdroj, ze kterého byl vytvořen záložní kopie hello. Pokud není v pořádku hello výběr, klikněte na tlačítko **zrušit** toonavigate back toorecovery kartě tooselect odpovídající obnovení bodu. Pokud hello výběr správné, klikněte na tlačítko **Další**.

    ![Externí souhrnu obnovení aplikace DPM](./media/backup-azure-alternate-dpm-server/external-dpm-recovery-summary.png)
7. Vyberte **obnovit alternativního umístění tooan**. **Procházet** toohello správná umístění pro obnovení hello.

    ![Externí DPM alternativní umístění pro obnovení](./media/backup-azure-alternate-dpm-server/external-dpm-recovery-alternate-location.png)
8. Vyberte možnost hello související příliš**vytvořit kopii**, **přeskočit**, nebo **přepsat**.

   * **Vytvořit kopii** -vytvoří kopii souboru hello, pokud je kolize názvů.
   * **Přeskočit** – Pokud je kolize názvů, nedojde k odstranění souboru hello, což ponechá původní soubor hello.
   * **Přepsat** – Pokud je kolize názvů, přepíše existující kopii souboru hello hello.

     Vyberte příslušnou možnost hello příliš**obnovit zabezpečení**. Můžete použít nastavení zabezpečení hello hello cílového počítače, kde hello data obnovena nebo hello nastavení zabezpečení, které byly příslušné tooproduct v hello v době vytvoření bodu obnovení hello.

     Identifikovat zda **oznámení** se odesílají, jednou hello obnovení úspěšně dokončí.

     ![Externí DPM obnovení oznámení](./media/backup-azure-alternate-dpm-server/external-dpm-recovery-notifications.png)
9. Hello **Souhrn** obrazovky je uveden seznam možností hello, pokud vybraná. Po kliknutí na tlačítko **'Obnovit'**, hello dat je obnovené toohello příslušné místní umístění.

    ![Souhrn možnosti obnovení externí DPM](./media/backup-azure-alternate-dpm-server/external-dpm-recovery-options-summary.png)

   > [!NOTE]
   > Úloha obnovení Hello lze sledovat v hello **monitorování** kartě hello serveru Azure Backup.
   >
   >

    ![Monitorování obnovení](./media/backup-azure-alternate-dpm-server/monitoring-recovery.png)
10. Můžete kliknout na **vymazat externí DPM** na hello **obnovení** kartě hello DPM serveru tooremove hello zobrazení hello externího serveru aplikace DPM.

    ![Vymazat externí DPM](./media/backup-azure-alternate-dpm-server/clear-external-dpm.png)

## <a name="troubleshooting-error-messages"></a>Řešení potíží s chybové zprávy
| Ne. | Chybová zpráva | Řešení potíží |
|:---:|:--- |:--- |
| 1. |Tento server není registrovaný toohello úložišti zadanému pomocí přihlašovacích údajů trezoru hello. |**Příčina:** tato chyba se zobrazí, když soubor přihlašovacích údajů trezoru hello vybraný nepatří toohello trezor služeb zotavení přidružené k serveru Azure Backup Server, na které hello dojde k pokusu o obnovení. <br> **Řešení:** soubor přihlašovacích údajů trezoru stáhnout hello z hello služeb zotavení trezoru toowhich hello serveru Azure Backup je zaregistrován. |
| 2. |Buď obnovitelná data hello není k dispozici nebo hello vybraný server není DPM server. |**Příčina:** neexistují žádné jiné servery zálohování Azure registrované toohello trezor služeb zotavení, nebo hello servery ještě jste zatím neodeslali hello metadata nebo hello vybraný server není Server Azure Backup (neboli systému Windows Server nebo klienta Windows). <br> **Řešení:** Pokud existují další trezor služeb zotavení registrované toohello serverů zálohování Azure, zkontrolujte, že hello nejnovější Azure Backup agent nainstalovaný. <br>Pokud existují další trezor služeb zotavení registrované toohello serverů zálohování Azure, počkejte denně po instalaci toostart hello obnovení procesu. Hello noční úlohu odešlete hello metadata pro všechny zálohy toocloud hello chráněný. Hello data budou k dispozici pro obnovení. |
| 3. |Žádný jiný server DPM není registrovaný toothis trezoru. |**Příčina:** neexistují žádné další Azure zálohování serverů, které jsou registrované toohello trezoru, ze které hello je probíhají pokusy o obnovení.<br>**Řešení:** Pokud existují další trezor služeb zotavení registrované toohello serverů zálohování Azure, zkontrolujte, že hello nejnovější Azure Backup agent nainstalovaný.<br>Pokud existují další trezor služeb zotavení registrované toohello serverů zálohování Azure, počkejte denně po instalaci toostart hello obnovení procesu. Hello noční úlohu ukládání hello metadata pro všechny chráněné zálohy toocloud. Hello data budou k dispozici pro obnovení. |
| 4. |Hello šifrovací přístupové heslo neodpovídá přístupovému heslu přidruženému hello následující server:**<server name>** |**Příčina:** hello šifrovací přístupové heslo používané při hello šifrovat data hello z dat hello Azure zálohování serveru, který obnovuje neodpovídá hello šifrovací přístupové heslo. Hello agent je nelze toodecrypt hello data. Proto hello obnovení selže.<br>**Řešení:** zadejte hello přesně stejný šifrovací přístupové heslo přidružené k hello serveru Azure Backup, jejichž data obnovena. |

## <a name="frequently-asked-questions"></a>Nejčastější dotazy

### <a name="why-cant-i-add-an-external-dpm-server-after-installing-ur7-and-latest-azure-backup-agent"></a>Proč nelze přidat externí server aplikace DPM po instalaci kumulativní aktualizací 7 a nejnovější verze agenta Azure Backup?

Hello serverů DPM pomocí zdrojů dat, které jsou chráněné toohello cloudu (s použitím kumulativní aktualizace starší než kumulativní aktualizace 7), je nutné počkat alespoň jeden den po instalaci hello kumulativní aktualizací 7 a nejnovější agenta Azure Backup, toostart **přidat externí DPM server** . Hello jeden den časové období je potřebné tooupload hello metadata tooAzure skupiny ochrany aplikace DPM hello. Nahrání metadata skupiny ochrany hello poprvé prostřednictvím noční úlohu.

### <a name="what-is-hello-minimum-version-of-hello-microsoft-azure-recovery-services-agent-needed"></a>Co je hello minimální verzi agenta služeb zotavení Microsoft Azure hello potřeba?

Hello minimální verzi agenta služeb zotavení Microsoft Azure hello nebo agenta Azure Backup, požadované tooenable tato funkce je 2.0.8719.0.  verze agenta hello tooview: Otevřete ovládací panely  **>**  všechny položky  **>**  programy a funkce  **>**  Agent služeb zotavení Microsoft Azure. Pokud verze hello je menší než 2.0.8719.0, stáhněte a nainstalujte hello [nejnovější verze agenta Azure Backup](https://go.microsoft.com/fwLink/?LinkID=288905).

![Vymazat externí DPM](./media/backup-azure-alternate-dpm-server/external-dpm-azurebackupagentversion.png)

## <a name="next-steps"></a>Další kroky:
• [Azure Backup – nejčastější dotazy](backup-azure-backup-faq.md)
