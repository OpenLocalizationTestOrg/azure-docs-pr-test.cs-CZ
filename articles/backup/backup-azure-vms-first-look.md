---
title: "První pohled: Zálohování virtuálních počítačů Azure s trezorem zálohování | Dokumentace Microsoftu"
description: "Pomocí klasického portálu tooback hello do trezoru služby Backup tooa virtuálních počítačích Azure. Tento kurz vysvětluje všechny fáze, včetně vytváření hello úložiště záloh, registrace hello virtuálních počítačů, vytvoření zásady zálohování a spouštění hello úlohu prvotního zálohování."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
ms.assetid: 722820dc-b65f-425c-a9e5-c1946e896a87
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/2/2017
ms.author: markgal;
ms.openlocfilehash: 77700e69eab9faccbc7ef923e1eb4e1f97be75d7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="first-look-backing-up-azure-virtual-machines"></a>První pohled: Zálohování virtuálních počítačů Azure
> [!div class="op_single_selector"]
> * [Ochrana virtuálních počítačů v trezoru služby Recovery Services](backup-azure-vms-first-look-arm.md)
> * [Ochrana virtuálních počítačů Azure s trezorem zálohování](backup-azure-vms-first-look.md)
>
>

Tento kurz vás provede kroky hello k zálohování virtuálních počítačů (VM) Azure tooa zálohování trezoru služby v Azure. Tento článek popisuje hello klasického modelu nebo model nasazení portálu Service Manager, pro zálohování virtuálních počítačů. Hello následující postup se vztahuje pouze tooBackup trezory vytvořit na portálu classic hello. Společnost Microsoft doporučuje pro nová nasazení pomocí modelu Resource Manager hello.

Pokud vás zajímá zálohování trezor služeb zotavení tooa virtuálních počítačů, které patří tooa skupinu prostředků, přečtěte si téma [první pohled: ochrana virtuálních počítačů s trezoru služeb zotavení](backup-azure-vms-first-look-arm.md).

toosuccessfully dokončit následující hello kurzu, musí existovat tyto požadavky:

* Vytvořili jste virtuální počítač v rámci svého předplatného Azure.
* Hello virtuálních počítačů má připojení tooAzure veřejné IP adresy. Další informace naleznete v tématu [Připojení k síti](backup-azure-vms-prepare.md#network-connectivity).


> [!NOTE]
> Azure obsahuje dva modely nasazení pro vytváření a práci s prostředky: [Resource Manager a Classic](../azure-resource-manager/resource-manager-deployment-model.md). Tento kurz je určen pro použití s virtuální počítače vytvořené na portálu classic hello.
>
>

## <a name="create-a-backup-vault"></a>Vytvoření trezoru záloh
Trezor záloh je entita, která ukládá všechny hello zálohy a body obnovení, které byly vytvořeny v čase. Hello trezor záloh obsahuje také zásady zálohování hello, které jsou zálohované použité toohello virtuálních počítačů.

> [!IMPORTANT]
> Od března 2017 se už můžete trezory Backup portálu classic toocreate hello.
> Můžete upgradovat vaše trezory služeb tooRecovery trezory Backup. Podrobnosti najdete v tématu hello článku [upgradu tooa trezoru zálohování trezor služeb zotavení](backup-azure-upgrade-backup-to-recovery-services.md). Společnost Microsoft doporučuje tooupgrade zálohování trezory tooRecovery trezory služeb.<br/> Po 15 říjen 2017 nemůžete použít trezory Backup toocreate prostředí PowerShell. **Do 1. listopadu 2017:**
>- Všechny zbývající trezory Backup bude automaticky upgradovaný tooRecovery trezory služeb.
>- Můžete nebudou moct tooaccess zálohovaných dat na portálu classic hello. Místo toho použijte hello Azure portálu tooaccess zálohovaných dat v trezory služeb zotavení.
>

## <a name="discover-and-register-azure-virtual-machines"></a>Vyhledání a registrace virtuálních počítačů Azure
Před registrací hello virtuálních počítačů k trezoru, spusťte tooidentify proces zjišťování hello nových virtuálních počítačů. Tento příkaz vrátí seznam virtuálních počítačů v rámci předplatného hello, společně s dalšími informacemi, jako je název hello cloudové služby a oblast hello.

1. Přihlaste se toohello [portálu Azure Classic](http://manage.windowsazure.com/)
2. V hello portál Azure classic, klikněte na **služeb zotavení** tooopen hello seznamu trezorů služeb zotavení.
    ![Výběr úlohy](./media/backup-azure-vms-first-look/recovery-services-icon.png)
3. Hello seznamu trezorů vyberte trezor tooback hello si virtuální počítač.

    Když vyberete svůj trezor, otevře se v hello **rychlý Start** stránky
4. Z nabídky hello trezoru, klikněte na tlačítko **registrované položky**.

    ![Výběr úlohy](./media/backup-azure-vms-first-look/configure-registered-items.png)
5. Z hello **typ** nabídce vyberte možnost **virtuální počítač Azure**.

    ![Výběr úlohy](./media/backup-azure-vms/discovery-select-workload.png)
6. Klikněte na tlačítko **DISCOVER** v hello dolní části stránky hello.
    ![Tlačítko Vyhledat](./media/backup-azure-vms/discover-button-only.png)

    proces zjišťování Hello může trvat několik minut hello virtuální počítače jsou se v tabulce. Je oznámení dole hello úvodní obrazovka, která vás informuje, zda je spuštěn proces hello.

    ![Vyhledání virtuálních počítačů](./media/backup-azure-vms/discovering-vms.png)

    Hello oznámení změny po dokončení procesu hello.

    ![Vyhledávání dokončeno](./media/backup-azure-vms-first-look/discovery-complete.png)
7. Klikněte na tlačítko **ZAREGISTROVAT** v hello dolní části stránky hello.
    ![Tlačítko Registrovat](./media/backup-azure-vms-first-look/register-icon.png)
8. V hello **registrovat položky** místní nabídky, vyberte hello virtuálních počítačů, které chcete tooregister.

   > [!TIP]
   > Najednou může být registrováno více virtuálních počítačů.
   >
   >

    Pro každý virtuální počítač, který jste vybrali, se vytvoří úloha.
9. Klikněte na tlačítko **zobrazit úlohy** v hello oznámení toogo toohello **úlohy** stránky.

    ![Registrace úlohy](./media/backup-azure-vms/register-create-job.png)

    virtuální počítač Hello se také zobrazí v hello seznamu registrovaných položek společně hello stav operace Registrování hello.

    ![Stav registrace 1](./media/backup-azure-vms/register-status01.png)

    Po dokončení operace hello hello stav se změní tooreflect hello *zaregistrován* stavu.

    ![Stav registrace 2](./media/backup-azure-vms/register-status02.png)

## <a name="install-hello-vm-agent-on-hello-virtual-machine"></a>Nainstalujte agenta virtuálního počítače hello hello virtuálního počítače
Hello agenta virtuálního počítače Azure musí být nainstalován na hello virtuálního počítače, které jsou pro toowork rozšíření hello zálohování Azure. Pokud virtuální počítač byl vytvořen z Galerie Azure hello, hello agenta virtuálního počítače je již nainstalován na hello virtuálních počítačů; můžete přeskočit příliš[ochranu virtuálních počítačů](backup-azure-vms-first-look.md#create-the-backup-policy).

Pokud virtuální počítač přenesen z překážek místní datacentra, hello virtuální počítač pravděpodobně neobsahuje hello nainstalovaný Agent virtuálního počítače. Hello agenta virtuálního počítače je nutné nainstalovat na virtuální počítač hello před pokračováním tooprotect hello virtuálních počítačů. Podrobné pokyny k instalaci hello agenta virtuálního počítače, najdete v části hello [agenta virtuálního počítače části článku zálohování virtuálních počítačů hello](backup-azure-vms-prepare.md#vm-agent).

## <a name="create-hello-backup-policy"></a>Vytvořit zásady zálohování hello
Předtím, než aktivujete úlohu prvotního zálohování hello, nastavte plán hello při pořizování snímků záloh. Hello naplánovat, kdy jsou pořízeny snímků zálohy a hello dobu tyto snímky jsou zachována, je hello zásady zálohování. informace o zachování Hello je založena na Dědečka. otec SYN schématu rotace záloh.

1. Přejděte toohello zálohy trezoru pod **služeb zotavení** v hello portálu Azure Classic a klikněte na tlačítko **registrované položky**.
2. Vyberte **virtuální počítač Azure** z rozevírací nabídky hello.

    ![Výběr úlohy v portálu](./media/backup-azure-vms/select-workload.png)
3. Klikněte na tlačítko **CHRÁNIT** v hello dolní části stránky hello.
    ![Klikněte na Chránit](./media/backup-azure-vms-first-look/protect-icon.png)

    Hello **Průvodce ochranou položek** se zobrazí a uvádí *pouze* virtuálních počítačů, které jsou zaregistrované a nechráněné.

    ![Škálování konfigurace ochrany](./media/backup-azure-vms/protect-at-scale.png)
4. Vyberte hello virtuálních počítačů, které chcete tooprotect.

    Pokud existují dvě nebo více virtuálních počítačů s hello stejný název, použijte hello Cloudová služba toodistinguish mezi virtuálními počítači hello.
5. Na hello **Konfigurace ochrany** nabídce vyberte existující zásadu nebo vytvořte nové zásady tooprotect hello virtuálních počítačů, které jste určili.

    Nové trezory Backup mít výchozí zásady přidružený k trezoru hello. Tato zásada pořizuje každý večer a denní snímek hello se uchovávají po dobu 30 dnů. Ke každé zásadě zálohování může být přidružených více virtuálních počítačů. Ale hello virtuálního počítače lze přidružit pouze jeden zásadám najednou.

    ![Ochrana pomocí nové zásady](./media/backup-azure-vms/policy-schedule.png)

   > [!NOTE]
   > Zásada zálohování obsahuje schéma uchovávání pro hello naplánované zálohování. Pokud vyberete existující zásady zálohování, bude v dalším kroku hello možnosti uchovávání nelze toomodify hello.
   >
   >
6. Na **rozsah uchování** definovat hello denní, týdenní, měsíční nebo roční rozsah pro konkrétní body zálohy hello.

    ![Virtuální počítač je zálohovaný s bodem obnovení](./media/backup-azure-vms/long-term-retention.png)

    Zásady uchovávání informací určuje hello dobu pro uložení zálohy. Můžete zadat různé zásady uchovávání informací na základě při pořízení zálohy hello.
7. Klikněte na tlačítko **úlohy** seznamu hello tooview **Konfigurace ochrany** úlohy.

    ![Úloha konfigurace ochrany](./media/backup-azure-vms/protect-configureprotection.png)

    Teď, když jste vytvořili hello zásad, přejděte toohello další krok a spusťte prvotní zálohování hello.

## <a name="initial-backup"></a>Prvotní zálohování
Jakmile je virtuální počítač je chráněný pomocí zásad, můžete zobrazit tuto relaci na hello **chráněné položky** kartě. Dokud hello proběhne prvotní zálohování, hello **stav ochrany** zobrazuje jako **chráněno – (čekání na prvotní zálohování)**. Ve výchozím nastavení, je prvním plánovaným zálohováním hello hello *prvotní zálohování*.

![Zálohování čeká na zpracování](./media/backup-azure-vms-first-look/protection-pending-border.png)

toostart hello prvotní zálohování nyní:

1. Na hello **chráněné položky** klikněte na tlačítko **zálohovat nyní** v hello dolní části stránky hello.
    ![Ikona Zálohovat nyní](./media/backup-azure-vms-first-look/backup-now-icon.png)

    Hello služba Azure Backup vytvoří úlohu zálohování pro operaci prvotního zálohování hello.
2. Klikněte na tlačítko hello **úlohy** kartě tooview hello seznamu úloh.

    ![Probíhá zálohování](./media/backup-azure-vms-first-look/protect-inprogress.png)

    Po dokončení prvotní zálohování, stav hello hello virtuálního počítače v hello **chráněné položky** karta je *chráněné*.

    ![Virtuální počítač je zálohovaný s bodem obnovení](./media/backup-azure-vms/protect-backedupvm.png)

   > [!NOTE]
   > Zálohování virtuálních počítačů je místní proces. Z jedné oblasti úložiště tooa záloh, které v jiné oblasti nelze zálohovat virtuální počítače. Ano pro každou oblast Azure, který má virtuální počítače, které je třeba zálohovat toobe, je nutné vytvořit alespoň jeden trezor záloh v této oblasti.
   >
   >

## <a name="next-steps"></a>Další kroky
Když jste teď úspěšně zálohovali virtuální počítač, je několik dalších kroků, které by vás mohly zajímat. Hello nejlogičtějším krokem je toofamiliarize sami s obnovováním dat tooa virtuálních počítačů. Nicméně existují úlohy správy, které vám pomohou pochopit, jak tookeep bezpečné dat a minimalizovat náklady.

* [Správa a monitorování virtuálních počítačů](backup-azure-manage-vms.md)
* [Obnovení virtuálních počítačů](backup-azure-restore-vms.md)
* [Pokyny při řešení potíží](backup-azure-vms-troubleshoot.md)

## <a name="questions"></a>Máte dotazy?
Pokud máte otázky, nebo pokud se některé funkce, které byste chtěli toosee zahrnuty, [pošlete nám svůj názor](http://aka.ms/azurebackup_feedback).
