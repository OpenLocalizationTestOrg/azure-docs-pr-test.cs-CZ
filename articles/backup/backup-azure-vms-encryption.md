---
title: "Zálohování a obnovení šifrovat virtuálních počítačů pomocí Azure Backup"
description: "Tento článek pojednává o zálohování a obnovení prostředí pro virtuální počítače šifrována pomocí Azure Disk Encryption."
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
ms.openlocfilehash: 89492cda8eff23509bee7693bb5f7e6ab5eb10d1
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-back-up-and-restore-encrypted-virtual-machines-with-azure-backup"></a>Jak zálohovat a obnovit zašifrovaná virtuálních počítačů s Azure Backup
V tomto článku bude zmíněn kroky pro zálohování a obnovení virtuálních počítačů pomocí služby Azure Backup. Také poskytuje podrobnosti o podporovaných scénářích, požadavky a řešení potíží pro chybových případech.

## <a name="supported-scenarios"></a>Podporované scénáře
> [!NOTE]
> * Zálohování a obnovení šifrovaných virtuálních počítačů je podporována pouze pro správce prostředků, které jsou nasazené virtuální počítače. Není podporováno pro klasické virtuální počítače. <br>
> * Je podporován pro virtuální počítače Windows a Linux pomocí Azure Disk Encryption, která využívá funkci oborový standard BitLocker funkce Windows a DM-Crypt systému Linux zajistit šifrování disků. <br>
> * Je podporována pouze pro virtuální počítače, které jsou šifrované pomocí klíče šifrování nástroje BitLocker a šifrovací klíče. Není podporována u virtuálních počítačů šifrovat jenom pomocí šifrovacího klíče nástroje BitLocker. <br>
>
>

## <a name="prerequisites"></a>Požadavky
1. Virtuální počítač byla zašifrována pomocí [Azure Disk Encryption](../security/azure-security-disk-encryption.md). By se šifrovat, pomocí nástroje BitLocker šifrovací klíč a šifrovací klíče.
2. Vytvoření trezoru služeb zotavení a replikace úložiště nastavit pomocí kroků uvedených v článku [Příprava prostředí pro zálohování](backup-azure-arm-vms-prepare.md).
3. Zálohování Azure nebyla zadána [oprávnění pro přístup k trezoru klíčů](#provide-permissions-to-azure-backup) obsahující klíče, šifrované tajné klíče pro virtuální počítače.

## <a name="backup-encrypted-vm"></a>Šifrované zálohování virtuálních počítačů
Pomocí následujících kroků nastavte cíle zálohování, definovat zásady a nakonfigurovat položky a spouští se zálohování.

### <a name="configure-backup"></a>Konfigurace zálohování
1. Pokud již máte otevřete trezoru služeb zotavení, pokračujte dalším krokem. Pokud nemáte otevřený trezor Služeb zotavení, ale jste na portálu Azure, klikněte na **Procházet** v Nabídce centra.

   * V seznamu prostředků zadejte **Recovery Services**.
   * Seznam se průběžně filtruje podle zadávaného textu. Až uvidíte **Trezory Recovery Services**, klikněte na ně.

      ![Vytvoření trezoru Recovery Services – krok 1](./media/backup-azure-vms-encryption/browse-to-rs-vaults.png) <br/>

     Objeví se seznam trezorů Služeb zotavení. Ze seznamu trezorů Služeb zotavení vyberte trezor.

     Otevře se řídicí panel vybraného trezoru.
2. Ze seznamu položek, která se zobrazí pod trezoru, klikněte na tlačítko **zálohování** otevřete okno zálohování.

      ![Otevřené okno Zálohování](./media/backup-azure-vms-encryption/select-backup.png)
3. Kliknutím na **Cíl zálohování** v okně Zálohování otevřete okno Cíl zálohování.

      ![Otevřené okno Scénář](./media/backup-azure-vms-encryption/select-backup-goal-one.png)
4. V okně cíl zálohování nastavte **kde běží vaše úlohy** do Azure a **co chcete zálohovat** k virtuálnímu počítači klikněte **OK**.

   Zavře se okno Cíl zálohování a otevře se okno Zásady zálohování.

   ![Otevřené okno Scénář](./media/backup-azure-vms-encryption/select-backup-goal-two.png)
5. V okně Zásady zálohování vyberte zásadu zálohování, kterou chcete použít pro trezor a klikněte na **OK**.

      ![Výběr zásady zálohování](./media/backup-azure-vms-encryption/setting-rs-backup-policy-new.png)

    Detaily výchozí zásady jsou uvedené v podrobnostech. Pokud chcete vytvořit zásadu, vyberte v rozevírací nabídce **Vytvořit novou**. Po kliknutí na **OK** je zásada zálohování přidružená k trezoru.

    Dále vyberte virtuální počítače, které se mají přidružit k trezoru.
6. Zvolit virtuální počítače šifrovaná přidružit k určené zásadě a klikněte na tlačítko **OK**.

      ![Vyberte šifrované virtuální počítače](./media/backup-azure-vms-encryption/selected-encrypted-vms.png)
7. Tato stránka zobrazuje zprávu o trezoru klíčů, které jsou přidružené k šifrované vybrané virtuální počítače. Služba zálohování vyžaduje přístup jen pro čtení k klíčů a tajných klíčů v trezoru klíčů. Používá tato oprávnění do záložní klíč a tajný klíč, společně s přidružené virtuální počítače. **Musí dát oprávnění ke službě zálohování přístup k trezoru klíčů pro zálohy pro práci**. Můžete zadat tato oprávnění pomocí [kroků uvedených v následující části](#provide-permissions-to-azure-backup).

      ![Šifrované zprávy virtuální počítače](./media/backup-azure-vms-encryption/encrypted-vm-warning-message.png)

      Teď, když jste definovali všechna nastavení trezoru, v okně Zálohování klikněte na možnost povolit zálohování v dolní části stránky. Povolit zálohování nasadí zásadu pro trezor a virtuální počítače.
8. V další fázi v rámci přípravy je instalace agenta virtuálního počítače nebo zajistit, že Agent virtuálního počítače je nainstalovaný. O stejnou akci, pomocí kroků uvedených v článku [Příprava prostředí pro zálohování](backup-azure-arm-vms-prepare.md).

### <a name="triggering-backup-job"></a>Spuštění úlohy zálohování
Postupujte podle kroků uvedených v článku [zálohování virtuálních počítačů Azure do trezoru služeb zotavení](backup-azure-arm-vms.md) k aktivační události úlohy zálohování.

### <a name="continue-backups-of-already-backed-up-vms-with-encryption-enabled"></a>Pokračovat zálohy již zálohovali virtuální počítače s šifrování povoleno.  
Pokud jste již probíhá zálohování do trezoru služeb zotavení virtuální počítače a mít povoleno šifrování později, musí udělit oprávnění ke službě zálohování přístup k trezoru klíčů pro zálohování pokračovat. Můžete zadat tato oprávnění pomocí [kroky v následující části](#provide-permissions-to-azure-backup) nebo pomocí prostředí PowerShell kroky uvedené v **povolit zálohování** části [prostředí PowerShell dokumentaci](backup-azure-vms-automation.md). 

## <a name="provide-permissions-to-azure-backup"></a>Zadejte oprávnění k Azure Backup
K zajištění odpovídající oprávnění k Azure Backup na přístupový klíč trezoru a zálohování virtuálních počítačů šifrované použijte následující kroky:
1. Vyberte **více služeb** a vyhledejte **klíče trezory**.

    ![Trezor klíčů vyhledávání](./media/backup-azure-vms-encryption/search-key-vault.png)
    
2. Ze seznamu trezorů klíčů vyberte trezor klíčů, které jsou přidružené k šifrované virtuálních počítačů, které je možné zálohovat.

     ![Vyberte trezor klíčů](./media/backup-azure-vms-encryption/select-key-vault.png)
     
3. Klikněte na tlačítko **zásady přístupu** a potom **přidat nový**.

    ![Přidání zásady přístupu](./media/backup-azure-vms-encryption/select-key-vault-access-policy.png)
    
4. Klikněte na tlačítko **vyberte objekt** a typ **služba správy zálohování** v panelu vyhledávání. 

    ![Zálohování služby vyhledávání](./media/backup-azure-vms-encryption/search-backup-service.png)
    
5. Vyberte **služba správy zálohování** a klikněte na tlačítko Vybrat.

    ![Vyberte služby zálohování](./media/backup-azure-vms-encryption/select-backup-service.png)
    
6. Vyberte **Azure Backup** v konfigurace z šablony rozevírací nabídku. Předem vyplní požadovaná oprávnění v oprávněních klíče a tajné oprávnění rozevírací nabídku. 

    ![Vyberte zálohování Azure](./media/backup-azure-vms-encryption/select-backup-template.png)
    
7. Klikněte na **OK**. Všimněte si, že služba správy zálohování přidá v okně zásady přístupu. 

    ![Zásady přístupu služby zálohování](./media/backup-azure-vms-encryption/backup-service-access-policy.png)
    
8. Klikněte na **Uložit**. Tím získáte požadovaná oprávnění k Azure Backup.

    ![Zásady přístupu služby zálohování](./media/backup-azure-vms-encryption/save-access-policy.png)

Jakmile úspěšně jsou uvedeny oprávnění, můžete pokračovat povolení zálohování pro virtuální počítače šifrovaná.

## <a name="restore-encrypted-vm"></a>Obnovení šifrovaných virtuálních počítačů
K obnovení virtuálního počítače šifrovaná, první obnovení disků pomocí kroků uvedených v části **obnovení zálohovaných disky** v [konfigurace obnovení výběru virtuálního počítače](backup-azure-arm-restore-vms.md#choosing-a-vm-restore-configuration). Potom můžete použít jednu z následujících možností:
* Pomocí prostředí PowerShell kroků uvedených v [vytvoření virtuálního počítače z obnovené disků](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) pro vytvoření úplné virtuálního počítače z obnovené disků.
* NEBO, [použít šablonu generované v rámci obnovení disků](backup-azure-arm-restore-vms.md#use-templates-to-customize-restore-vm) k vytvoření virtuálních počítačů z obnovené disků. Šablony lze použít pouze pro body obnovení vytvořené po 26. dubna 2017.

## <a name="troubleshooting-errors"></a>Řešení potíží s chybami
| Operace | Podrobnosti o chybě | Řešení |
| --- | --- | --- |
| Zálohování |Ověření se nezdařilo, protože virtuální počítač je šifrován BEK samostatně. Zálohování se dá nastavit jenom pro virtuální počítače šifrován BEK a KEK. |Virtuální počítač by se šifrovat pomocí BEK a KEK. Nejprve dešifrovat virtuálního počítače a šifrování pomocí BEK a KEK. Povolení zálohování po virtuálních počítačů jsou šifrována pomocí BEK a KEK. Další informace o tom, jak můžete [dešifrování a šifrování virtuálního počítače](../security/azure-security-disk-encryption.md)  |
| Obnovení |Tento šifrovaný virtuální počítač nelze obnovit, protože trezoru klíčů, které jsou přidružené k tohoto virtuálního počítače neexistuje. |Vytvoření trezoru klíčů použijte [Začínáme s Azure Key Vault](../key-vault/key-vault-get-started.md). Najdete v článku [obnovit klíč trezoru klíčů a tajný klíč pomocí služby Azure Backup](backup-azure-restore-key-secret.md) obnovit klíč a tajný klíč, pokud nejsou k dispozici. |
| Obnovení |Tento šifrovaný virtuální počítač nelze obnovit, protože klíč a tajný klíč přidružený tento virtuální počítač nejsou k dispozici. |Najdete v článku [obnovit klíč trezoru klíčů a tajný klíč pomocí služby Azure Backup](backup-azure-restore-key-secret.md) obnovit klíč a tajný klíč, pokud nejsou k dispozici. |
| Obnovení |Služba zálohování nemá oprávnění pro přístup k prostředkům ve vašem předplatném. |Jak je uvedeno nahoře, obnovení disky nejprve pomocí kroků uvedených v části **obnovení zálohovaných disky** v [konfigurace obnovení výběru virtuálního počítače](backup-azure-arm-restore-vms.md#choosing-a-vm-restore-configuration). Po této, uživatelské prostředí PowerShell k [vytvoření virtuálního počítače z obnovené disků](backup-azure-vms-automation.md#create-a-vm-from-restored-disks). |
|Zálohování | Služba Azure Backup nemá dostatečná oprávnění pro Key Vault pro zálohování šifrované virtuálních počítačů | Virtuální počítač by se šifrovat pomocí šifrovacího klíče nástroje BitLocker a šifrovací klíče. Potom by měla být povolená zálohování.  Služba zálohování je nutné zadat tyto oprávnění pomocí [kroky uvedené výše v části](#provide-permissions-to-azure-backup) nebo pomocí prostředí PowerShell kroků v **povolení ochrany** části dokumentace prostředí PowerShell v [AzureRM.RecoveryServices.Backup použití rutiny pro zálohování virtuálních počítačů](backup-azure-vms-automation.md#back-up-azure-vms). |  
