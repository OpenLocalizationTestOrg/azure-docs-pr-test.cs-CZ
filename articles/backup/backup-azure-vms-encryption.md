---
title: "zašifrovány aaaBackup a obnovení virtuálních počítačů pomocí služby Azure Backup"
description: "V tomto článku bude zmíněn hello zálohování a obnovení prostředí pro virtuální počítače šifrována pomocí Azure Disk Encryption."
services: backup
documentationcenter: 
author: JPallavi
manager: vijayts
editor: 
ms.assetid: 8387f186-7d7b-400a-8fc3-88a85403ea63
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/27/2017
ms.author: pajosh;markgal;trinadhk
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c19ef6f58e3259949535dab32a55aaf7a8c658fd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooback-up-and-restore-encrypted-virtual-machines-with-azure-backup"></a>Jak šifrované tooback zálohu a obnovení virtuálních počítačů s Azure Backup
V tomto článku bude zmíněn kroky toobackup a obnovení virtuálních počítačů pomocí služby Azure Backup. Také poskytuje podrobnosti o podporovaných scénářích, požadavky a řešení potíží pro chybových případech.

## <a name="supported-scenarios"></a>Podporované scénáře
> [!NOTE]
> * Zálohování a obnovení šifrovaných virtuálních počítačů je podporována pouze pro správce prostředků, které jsou nasazené virtuální počítače. Není podporováno pro klasické virtuální počítače. <br>
> * Je podporován pro virtuální počítače Windows a Linux pomocí Azure Disk Encryption, která využívá hello oborový standard BitLocker funkce systému Windows a DM-Crypt funkce Linux tooprovide šifrování disků. <br>
> * Je podporována pouze pro virtuální počítače, které jsou šifrované pomocí klíče šifrování nástroje BitLocker a šifrovací klíče. Není podporována u virtuálních počítačů šifrovat jenom pomocí šifrovacího klíče nástroje BitLocker. <br>
>
>

## <a name="prerequisites"></a>Požadavky
1. Virtuální počítač byla zašifrována pomocí [Azure Disk Encryption](../security/azure-security-disk-encryption.md). By se šifrovat, pomocí nástroje BitLocker šifrovací klíč a šifrovací klíče.
2. Vytvoření trezoru služeb zotavení a replikace úložiště nastavit pomocí kroků uvedených v článku hello [Příprava prostředí pro zálohování](backup-azure-arm-vms-prepare.md).
3. Zálohování Azure nebyla zadána [trezoru klíčů tooaccess oprávnění](#provide-permissions-to-azure-backup) obsahující klíče, šifrované tajné klíče pro virtuální počítače.

## <a name="backup-encrypted-vm"></a>Šifrované zálohování virtuálních počítačů
Použijte následující postup cíle zálohování tooset hello, definovat zásady, nakonfigurovat položky a spouští se zálohování.

### <a name="configure-backup"></a>Konfigurace zálohování
1. Pokud již máte otevřete trezoru služeb zotavení, pokračujte toonext krok. Pokud nemáte otevřený trezor služeb zotavení, ale jsou v hello portál Azure, v nabídce centra hello, klikněte na tlačítko **Procházet**.

   * V seznamu hello prostředků, zadejte **služeb zotavení**.
   * Během zadávání hello seznamu filtrů podle vašeho zadání. Až uvidíte **Trezory Recovery Services**, klikněte na ně.

      ![Vytvoření trezoru Recovery Services – krok 1](./media/backup-azure-vms-encryption/browse-to-rs-vaults.png) <br/>

     Zobrazí se seznam trezorů služeb zotavení Hello. Hello seznamu trezorů služeb zotavení vyberte trezor.

     Otevře se řídicí panel vybraného trezoru Hello.
2. Seznamu hello, položek, které se zobrazí v trezoru, klikněte na tlačítko **zálohování** okno zálohování tooopen hello.

      ![Otevřené okno Zálohování](./media/backup-azure-vms-encryption/select-backup.png)
3. V okně Zálohování hello, klikněte na tlačítko **cíl zálohování** okno cíl zálohování tooopen hello.

      ![Otevřené okno Scénář](./media/backup-azure-vms-encryption/select-backup-goal-one.png)
4. V okně cíl zálohování hello nastavte **kde běží vaše úlohy** tooAzure a **co chcete toobackup** tooVirtual počítač a potom klikněte na **OK**.

   Zavře okno cíl zálohování Hello a otevře se okno zásady zálohování hello.

   ![Otevřené okno Scénář](./media/backup-azure-vms-encryption/select-backup-goal-two.png)
5. V okně zásady zálohování hello, vyberte zásady zálohování hello tooapply toohello trezor a klikněte na **OK**.

      ![Výběr zásady zálohování](./media/backup-azure-vms-encryption/setting-rs-backup-policy-new.png)

    v hello podrobnosti jsou uvedeny podrobnosti Hello hello výchozích zásad. Pokud chcete toocreate zásadu, vyberte **vytvořit nový** z rozevírací nabídky hello. Po kliknutí na tlačítko **OK**, zásady zálohování hello souvisí s hello trezoru.

    Dále vyberte hello tooassociate virtuální počítače s úložištěm hello.
6. Zvolte hello šifrované virtuální počítače tooassociate s hello zadané zásady a klikněte na tlačítko **OK**.

      ![Vyberte šifrované virtuální počítače](./media/backup-azure-vms-encryption/selected-encrypted-vms.png)
7. Tato stránka zobrazuje zprávu o trezoru klíčů, zvolené virtuální počítače přidružené toohello zašifrovaná. Služba zálohování vyžaduje oprávnění jen pro čtení toohello klíčů a tajných klíčů v trezoru klíčů hello. Ji používá tato oprávnění toobackup klíč a tajný klíč, společně s hello přidružené virtuální počítače. **Musí dát oprávnění toobackup služby tooaccess klíče trezoru pro zálohování toowork**. Můžete zadat tato oprávnění pomocí [kroků uvedených v následující části hello](#provide-permissions-to-azure-backup).

      ![Šifrované zprávy virtuální počítače](./media/backup-azure-vms-encryption/encrypted-vm-warning-message.png)

      Teď, když jste definovali všechna nastavení trezoru hello, v okně Zálohování hello, klikněte na možnost povolit zálohování na hello dolní části stránky hello. Povolit zálohování nasadí hello zásad toohello trezor a virtuální počítače hello.
8. Hello další fázi v rámci přípravy instaluje hello agenta virtuálního počítače nebo provádění zda hello agenta virtuálního počítače je nainstalována. toodo hello stejný, použijte hello kroky uvedené v článku hello [Příprava prostředí pro zálohování](backup-azure-arm-vms-prepare.md).

### <a name="triggering-backup-job"></a>Spuštění úlohy zálohování
Použijte hello kroky uvedené v článku hello [toorecovery zálohování virtuálních počítačů Azure services trezoru](backup-azure-arm-vms.md) tootrigger úlohu zálohování.

### <a name="continue-backups-of-already-backed-up-vms-with-encryption-enabled"></a>Pokračovat zálohy již zálohovali virtuální počítače s šifrování povoleno.  
Pokud jste již probíhá zálohování do trezoru služeb zotavení virtuální počítače a mít povoleno šifrování později, musí dát oprávnění toobackup služby tooaccess klíče trezoru pro toocontinue zálohy. Můžete zadat tato oprávnění pomocí [kroky v následující části hello](#provide-permissions-to-azure-backup) nebo pomocí prostředí PowerShell kroky uvedené v **povolit zálohování** části [prostředí PowerShell dokumentaci](backup-azure-vms-automation.md). 

## <a name="provide-permissions-tooazure-backup"></a>Zadejte oprávnění tooAzure zálohování
Použijte následující kroky tooprovide odpovídající oprávnění tooAzure tooaccess klíče trezoru služby Backup hello a proveďte zálohu šifrované virtuálních počítačů:
1. Vyberte **více služeb** a vyhledejte **klíče trezory**.

    ![Trezor klíčů vyhledávání](./media/backup-azure-vms-encryption/search-key-vault.png)
    
2. Hello seznam trezorů klíčů vyberte trezor klíčů hello přidružené šifrované virtuální počítač, který se musí toobe zálohovat.

     ![Vyberte trezor klíčů](./media/backup-azure-vms-encryption/select-key-vault.png)
     
3. Klikněte na tlačítko **zásady přístupu** a potom **přidat nový**.

    ![Přidání zásady přístupu](./media/backup-azure-vms-encryption/select-key-vault-access-policy.png)
    
4. Klikněte na tlačítko **vyberte objekt** a typ **služba správy zálohování** v panelu vyhledávání hello. 

    ![Zálohování služby vyhledávání](./media/backup-azure-vms-encryption/search-backup-service.png)
    
5. Vyberte **služba správy zálohování** a klikněte na tlačítko Vybrat.

    ![Vyberte služby zálohování](./media/backup-azure-vms-encryption/select-backup-service.png)
    
6. Vyberte **Azure Backup** v konfigurace z šablony rozevírací nabídku. Předem vyplní hello požadované oprávnění v oprávnění klíče a tajné oprávnění rozevírací nabídku. 

    ![Vyberte zálohování Azure](./media/backup-azure-vms-encryption/select-backup-template.png)
    
7. Klikněte na **OK**. Všimněte si, že služba správy zálohování přidá v okně zásady přístupu. 

    ![Zásady přístupu služby zálohování](./media/backup-azure-vms-encryption/backup-service-access-policy.png)
    
8. Klikněte na **Uložit**. To vám poskytne hello požadované oprávnění tooAzure zálohování.

    ![Zásady přístupu služby zálohování](./media/backup-azure-vms-encryption/save-access-policy.png)

Jakmile úspěšně jsou uvedeny oprávnění, můžete pokračovat povolení zálohování pro virtuální počítače šifrovaná.

## <a name="restore-encrypted-vm"></a>Obnovení šifrovaných virtuálních počítačů
toorestore šifrování virtuálních počítačů, první obnovení disků pomocí kroků uvedených v části **obnovení zálohovaných disky** v [konfigurace obnovení výběru virtuálního počítače](backup-azure-arm-restore-vms.md#choosing-a-vm-restore-configuration). Potom můžete použít jednu z hello následující možnosti:
* Pomocí prostředí PowerShell hello kroků v [vytvoření virtuálního počítače z obnovené disků](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) toocreate úplné z obnovené disků virtuálních počítačů.
* NEBO, [použít šablonu generované v rámci obnovení disků](backup-azure-arm-restore-vms.md#use-templates-to-customize-restore-vm) toocreate virtuální počítače z obnovené disků. Šablony lze použít pouze pro body obnovení vytvořené po 26. dubna 2017.

## <a name="troubleshooting-errors"></a>Řešení potíží s chybami
| Operace | Podrobnosti o chybě | Řešení |
| --- | --- | --- |
| Zálohování |Ověření se nezdařilo, protože virtuální počítač je šifrován BEK samostatně. Zálohování se dá nastavit jenom pro virtuální počítače šifrován BEK a KEK. |Virtuální počítač by se šifrovat pomocí BEK a KEK. Nejprve dešifrovat hello virtuálních počítačů a šifrování pomocí BEK a KEK. Povolení zálohování po virtuálních počítačů jsou šifrována pomocí BEK a KEK. Další informace o tom, jak můžete [dešifrování a šifrování hello virtuálních počítačů](../security/azure-security-disk-encryption.md)  |
| Obnovení |Tento šifrovaný virtuální počítač nelze obnovit, protože trezoru klíčů, které jsou přidružené k tohoto virtuálního počítače neexistuje. |Vytvoření trezoru klíčů použijte [Začínáme s Azure Key Vault](../key-vault/key-vault-get-started.md). Najdete v článku hello [obnovit klíč trezoru klíčů a tajný klíč pomocí služby Azure Backup](backup-azure-restore-key-secret.md) toorestore klíč a tajný klíč, pokud jsou k dispozici není. |
| Obnovení |Tento šifrovaný virtuální počítač nelze obnovit, protože klíč a tajný klíč přidružený tento virtuální počítač nejsou k dispozici. |Najdete v článku hello [obnovit klíč trezoru klíčů a tajný klíč pomocí služby Azure Backup](backup-azure-restore-key-secret.md) toorestore klíč a tajný klíč, pokud jsou k dispozici není. |
| Obnovení |Služba zálohování nemá autorizace tooaccess prostředky ve vašem předplatném. |Jak je uvedeno nahoře, obnovení disky nejprve pomocí kroků uvedených v části **obnovení zálohovaných disky** v [konfigurace obnovení výběru virtuálního počítače](backup-azure-arm-restore-vms.md#choosing-a-vm-restore-configuration). Po této, uživatelské prostředí PowerShell příliš[vytvoření virtuálního počítače z obnovené disků](backup-azure-vms-automation.md#create-a-vm-from-restored-disks). |
|Zálohování | Služba Azure Backup nemá dostatečná oprávnění tooKey trezor pro zálohování šifrované virtuálních počítačů | Virtuální počítač by se šifrovat pomocí šifrovacího klíče nástroje BitLocker a šifrovací klíče. Potom by měla být povolená zálohování.  Služba zálohování je nutné zadat tyto oprávnění pomocí [kroků hello výše v části](#provide-permissions-to-azure-backup) nebo pomocí prostředí PowerShell kroků v hello **povolení ochrany** části hello prostředí PowerShell dokumentace v [AzureRM.RecoveryServices.Backup použití rutin tooback až virtuálních počítačů](backup-azure-vms-automation.md#back-up-azure-vms). |  
