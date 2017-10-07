---
title: "aaaManage Azure Backup trezory a servery Azure pomocí modelu nasazení classic hello | Microsoft Docs"
description: "Použijte tento kurz toolearn jak toomanage Azure Backup trezory a servery."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: tysonn
ms.assetid: f175eb12-0905-437f-91fd-eaee03ab6e81
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: markgal;
ms.openlocfilehash: 6c38b04f4a76604bfd639a9b2d58237ce44e2386
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-backup-vaults-and-servers-using-hello-classic-deployment-model"></a>Správa trezoru zálohování Azure a servery pomocí modelu nasazení classic hello
> [!div class="op_single_selector"]
> * [Resource Manager](backup-azure-manage-windows-server.md)
> * [Classic](backup-azure-manage-windows-server-classic.md)
>
>

V tomto článku najdete přehled úlohy zálohování správy hello, které jsou k dispozici prostřednictvím hello portál Azure classic a hello Microsoft Azure Backup agent.

> [!IMPORTANT]
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../azure-resource-manager/resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.

> [!IMPORTANT]
> Teď můžete upgradovat vaše trezory služeb tooRecovery trezory Backup. Podrobnosti najdete v tématu hello článku [upgradu tooa trezoru zálohování trezor služeb zotavení](backup-azure-upgrade-backup-to-recovery-services.md). Společnost Microsoft doporučuje tooupgrade zálohování trezory tooRecovery trezory služeb.<br/> Po 15 říjen 2017 nemůžete použít trezory Backup toocreate prostředí PowerShell. **Do 1. listopadu 2017:**
>- Všechny zbývající trezory Backup bude automaticky upgradovaný tooRecovery trezory služeb.
>- Můžete nebudou moct tooaccess zálohovaných dat na portálu classic hello. Místo toho použijte hello Azure portálu tooaccess zálohovaných dat v trezory služeb zotavení.
>

## <a name="management-portal-tasks"></a>Úlohy správy portálu
1. Přihlaste se toohello [portálu pro správu](https://manage.windowsazure.com).
2. Klikněte na tlačítko **služeb zotavení**, pak klikněte na tlačítko hello název úložiště záloh tooview hello rychlý Start stránky.

    ![Služby zotavení](./media/backup-azure-manage-windows-server-classic/rs-left-nav.png)

Výběrem možnosti hello hello horní části stránky rychlý Start hello uvidíte hello dostupné úlohy správy.

![Spravovat karty](./media/backup-azure-manage-windows-server-classic/qs-page.png)

### <a name="dashboard"></a>Řídicí panel
Vyberte **řídicí panel** toosee hello přehled využití pro hello server. Hello **přehled využití** zahrnuje:

* Hello počet serverů Windows registrovaných toocloud
* Hello počet virtuálních počítačů Azure chráněna v cloudu
* Celkový úložný Hello využívat v Azure
* Hello stav posledních úloh

Dole hello hello řídicí panel můžete provádět následující úlohy hello:

* **Správa certifikátů** – Pokud certifikát byl server hello použité tooregister, můžete použít tento certifikát tooupdate hello. Pokud používáte přihlašovací údaje trezoru, nepoužívejte **Správa certifikátu**.
* **Odstranit** -odstraní hello aktuální úložiště záloh. Pokud se už používá úložiště záloh, můžete ho odstranit toofree do prostoru úložiště. **Odstranit** je aktivní jenom po z trezoru hello byly odstraněny všechny registrované servery.

![Úlohy zálohování řídicí panel](./media/backup-azure-manage-windows-server-classic/dashboard-tasks.png)

## <a name="registered-items"></a>Registrovaných položek
Vyberte **registrované položky** tooview hello názvy hello serverů, které jsou registrovány toothis trezoru.

![Registrovaných položek](./media/backup-azure-manage-windows-server-classic/registered-items.png)

Hello **typ** výchozí nastavení filtru tooAzure virtuálního počítače. Vyberte tooview hello názvy hello serverů, které jsou registrované toothis trezoru **systému Windows server** z hello rozevírací nabídce.

Odsud můžete provádět následující úlohy hello:

* **Povolit opakovanou registraci** – Pokud je vybraná tato možnost pro server, můžete použít hello **Průvodce registrací** hello na místním Microsoft Azure Backup agent tooregister hello serveru pomocí úložiště záloh hello podruhé . Může být nutné toore registrace z důvodu chyby tooan v certifikátu hello nebo pokud server měl toobe znovu sestavit.
* **Odstranit** -odstraní server z trezoru záloh hello. Všechny hello uložená data přidružená k hello serveru budou okamžitě odstraněna.

    ![Úlohy registrovaných položek](./media/backup-azure-manage-windows-server-classic/registered-items-tasks.png)

## <a name="protected-items"></a>Chráněné položky
Vyberte **chráněné položky** tooview hello položky, které byly zálohovány ze serverů hello.

![Chráněné položky](./media/backup-azure-manage-windows-server-classic/protected-items.png)

## <a name="configure"></a>Konfigurace
Z hello **konfigurace** kartě můžete vybrat možnost redundance hello odpovídající úložiště. Hello nejlepší čas tooselect hello úložiště redundance možnost je hned po vytvoření trezoru a předtím, než všechny počítače jsou registrované tooit.

> [!WARNING]
> Jakmile položku už registrovaný toohello trezoru, možnost redundance úložiště hello je uzamčen a nemůže být upraven.
>
>

![Konfigurace](./media/backup-azure-manage-windows-server-classic/configure.png)

Najdete v tomto článku Další informace [redundance úložiště](../storage/common/storage-redundancy.md).

## <a name="microsoft-azure-backup-agent-tasks"></a>Úlohy agenta Microsoft Azure Backup
### <a name="console"></a>Konzola
Otevřete hello **Microsoft Azure Backup agent** (najdete ho tak, že váš počítač pro *Microsoft Azure Backup*).

![Agenta zálohování](./media/backup-azure-manage-windows-server-classic/snap-in-search.png)

Z hello **akce** k dispozici na hello napravo od hello agenta zálohování konzoly můžete provádět následující úlohy správy hello:

* Registrace serveru
* Plán zálohování
* Zálohovat nyní
* Změnit vlastnosti

![Konzole akce agenta.](./media/backup-azure-manage-windows-server-classic/console-actions.png)

> [!NOTE]
> příliš**obnovit Data**, najdete v části [obnovit soubory tooa Windows server nebo klientský počítač systému Windows](backup-azure-restore-windows-server.md).
>
>

### <a name="modify-an-existing-backup"></a>Upravit existující zálohy
1. V rámci Microsoft Azure Backup agent hello klikněte na tlačítko **plánem zálohování**.

    ![Plán zálohování Windows serveru](./media/backup-azure-manage-windows-server-classic/schedule-backup.png)
2. V hello **Průvodce plánem zálohování** nechte hello **provádět změny toobackup položky nebo časy** možnost a klikněte na tlačítko **Další**.

    ![Upravit naplánovaného zálohování](./media/backup-azure-manage-windows-server-classic/modify-or-stop-a-scheduled-backup.png)
3. Pokud chcete tooadd nebo změnit položky na hello **vyberte položky tooBackup** obrazovce klikněte na tlačítko **přidat položky**.

    Můžete také nastavit **nastavení vyloučení** z této stránky v Průvodci hello. Pokud chcete tooexclude soubory nebo typy souborů číst hello postup pro přidání [nastavení vyloučení](#exclusion-settings).
4. Vyberte hello soubory a složky tooback nahoru a klikněte na **nevadí**.

    ![Přidání položek](./media/backup-azure-manage-windows-server-classic/add-items-modify.png)
5. Zadejte hello **plán zálohování** a klikněte na tlačítko **Další**.

    Můžete naplánovat denní (probíhající maximálně 3krát denně) nebo týdenní zálohování.

    ![Zadejte plán zálohování](./media/backup-azure-manage-windows-server-classic/specify-backup-schedule-modify-close.png)

   > [!NOTE]
   > Zadat plán zálohování hello je podrobně popsány v tomto [článku](backup-azure-backup-cloud-as-tape.md).
   >
   >
6. Vyberte hello **zásady uchovávání informací** pro záložní kopii hello a klikněte na tlačítko **Další**.

    ![Vyberte vaše zásady uchovávání informací](./media/backup-azure-manage-windows-server-classic/select-retention-policy-modify.png)
7. Na hello **potvrzení** obrazovce zkontrolovat hello informace a klikněte na tlačítko **Dokončit**.
8. Po dokončení Průvodce hello vytváření hello **plán zálohování**, klikněte na tlačítko **Zavřít**.

    Po úpravě ochrany, můžete ověřit, zálohování, ke které spouštějí správně podle budete toohello **úlohy** kartě a potvrzující, že změny se projeví v hello zálohování úloh.

### <a name="enable-network-throttling"></a>Povolit omezení šířky pásma sítě
agent Azure Backup Hello poskytuje omezování karta, která vám umožní toocontrol použití šířky pásma sítě při přenosu dat. Tento ovládací prvek může být užitečné, pokud potřebujete tooback dat během pracovní doby, ale nechcete, aby proces zálohování toointerfere hello s ostatní internetový provoz. Omezování dat přenos platí tooback nahoru a obnovit aktivity.  

tooenable omezení:

1. V hello **agenta zálohování**, klikněte na tlačítko **změnit vlastnosti**.
2. Vyberte hello **povolit využití šířky pásma Internetu u operací zálohování omezení** zaškrtávací políčko.

    ![Omezení šířky pásma sítě](./media/backup-azure-manage-windows-server-classic/throttling-dialog.png)
3. Jakmile jste povolili omezování, zadejte hello povolený šířky pásma pro přenos zálohovaných dat během **pracovní hodiny** a **nepracovní hodiny**.

    hodnoty šířky pásma Hello začít až 512 kilobitů za sekundu (Kbps) a můžete přejít do too1023 megabajtů za sekundu (Mbps). Můžete také určit hello zahájení a dokončení pro **pracovní hodiny**, a které dny v týdnu hello jsou považovány za pracovních dnů. čas Hello mimo určené pracovní hodiny hello je považovány toobe mimo pracovní dobu.
4. Klikněte na **OK**.

## <a name="exclusion-settings"></a>Nastavení vyloučení
1. Otevřete hello **Microsoft Azure Backup agent** (najdete ho tak, že váš počítač pro *Microsoft Azure Backup*).

    ![Otevřete agenta zálohování](./media/backup-azure-manage-windows-server-classic/snap-in-search.png)
2. V rámci Microsoft Azure Backup agent hello klikněte na tlačítko **plánem zálohování**.

    ![Plán zálohování Windows serveru](./media/backup-azure-manage-windows-server-classic/schedule-backup.png)
3. V Průvodci plánování zálohování hello nechte hello **provádět změny toobackup položky nebo časy** možnost a klikněte na tlačítko **Další**.

    ![Změna plánu](./media/backup-azure-manage-windows-server-classic/modify-or-stop-a-scheduled-backup.png)
4. Klikněte na tlačítko **nastavení vyloučení**.

    ![Vyberte položky tooexclude](./media/backup-azure-manage-windows-server-classic/exclusion-settings.png)
5. Klikněte na tlačítko **Přidat vyloučení**.

    ![Přidat vyloučení](./media/backup-azure-manage-windows-server-classic/add-exclusion.png)
6. Vyberte umístění hello a potom klikněte na **OK**.

    ![Vyberte umístění pro vyloučení](./media/backup-azure-manage-windows-server-classic/exclusion-location.png)
7. Přidat příponu souboru hello v hello **typ souboru** pole.

    ![Vyloučit podle typu souboru](./media/backup-azure-manage-windows-server-classic/exclude-file-type.png)

    Přidání rozšíření MP3

    ![Příklad typu souboru](./media/backup-azure-manage-windows-server-classic/exclude-mp3.png)

    tooadd jiné rozšíření, klikněte na tlačítko **Přidat vyloučení** a zadejte jinou příponu typu souboru (Přidání rozšíření .jpeg).

    ![Další příklad typu souboru](./media/backup-azure-manage-windows-server-classic/exclude-jpg.png)
8. Pokud jste přidali všechny hello rozšíření, klikněte na tlačítko **OK**.
9. Kliknutím na pokračovat prostřednictvím hello Průvodce plánem zálohování **Další** dokud hello **potvrzovací stránku**, pak klikněte na tlačítko **Dokončit**.

    ![Vyloučení potvrzení](./media/backup-azure-manage-windows-server-classic/finish-exclusions.png)

## <a name="next-steps"></a>Další kroky
* [Obnovení systému Windows Server nebo klienta Windows z Azure](backup-azure-restore-windows-server.md)
* toolearn Další informace o zálohování Azure, najdete v části [Přehled zálohování Azure](backup-introduction-to-azure-backup.md)
* Navštivte hello [fóru služby Azure Backup](http://go.microsoft.com/fwlink/p/?LinkId=290933)
