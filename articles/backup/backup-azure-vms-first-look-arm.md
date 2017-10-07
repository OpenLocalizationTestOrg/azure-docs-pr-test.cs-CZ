---
title: "První pohled: chraňte virtuální počítače Azure pomocí trezoru Recovery Services | Dokumentace Microsoftu"
description: "Virtuální počítače Azure s trezorem Recovery Services. Pomocí záloh virtuálních počítačů nasazených Resource Manager, virtuálních počítačů nasazených Classic a úložiště virtuálních počítačů úrovně Premium, šifrované virtuálních počítačů, virtuální počítače na spravované disky tooprotect vaše data. Vytvoření a registrace trezoru Recovery Services. Registrace virtuálních počítačů, vytváření zásad a ochrana virtuálních počítačů v Azure."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keyword: backups; vm backup
ms.assetid: 45e773d6-c91f-4501-8876-ae57db517cd1
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 08/15/2017
ms.author: markgal;jimpark
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 70e4700abb76e16e32e1ead06ce1dbe277e1f0e5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-azure-virtual-machines-toorecovery-services-vaults"></a>Zálohování trezory služeb tooRecovery virtuální počítače Azure
> [!div class="op_single_selector"]
> * [Ochrana virtuálních počítačů v trezoru služby Recovery Services](backup-azure-vms-first-look-arm.md)
> * [Ochrana virtuálních počítačů v úložišti Backup Vault](backup-azure-vms-first-look.md)
>
>

Tento kurz vás provede hello kroky pro vytvoření trezoru služeb zotavení a zálohování Azure virtuálních počítačů (VM). Trezory Recovery Services chrání:

* Virtuální počítače nasazené Azure Resource Managerem
* Klasické virtuální počítače
* Virtuální počítače služby Storage úrovně Standard
* Virtuální počítače služby Storage úrovně Premium
* Virtuální počítače spuštěné na spravovaných discích
* Virtuální počítače jsou šifrované službou Azure Disk Encryption klíči BEK a KEK
* Zálohování konzistentní vzhledem k aplikacím virtuálních počítačů s Windows pomocí služby Stínová kopie svazku (VSS) a virtuálních počítačů s Linuxem pomocí vlastních předsnímkových a posnímkových skriptů

Další informace o ochraně virtuálních počítačů služby storage úrovně Premium, najdete v článku hello, [zálohování a obnovení virtuálních počítačů služby Storage úrovně Premium](backup-introduction-to-azure-backup.md#using-premium-storage-vms-with-azure-backup). Další informace o podpoře pro virtuální počítače se spravovanými disky najdete v tématu věnovaném [zálohování a obnovení virtuálních počítačů na spravovaných discích](backup-introduction-to-azure-backup.md#using-managed-disk-vms-with-azure-backup). Další informace o rozhraní s předsnímkovými a posnímkovými skripty pro zálohování virtuálních počítačů s Linuxem najdete v tématu Zálohování virtuálních počítačů s Linuxem konzistentní s aplikacemi pomocí předsnímkových a posnímkových skriptů (https://docs.microsoft.com/azure/backup/backup-azure-linux-app-consistent).

toofind si více o tom, co můžete je zálohování a co nemůžete odkazovat [sem](backup-azure-vms-prepare.md#limitations-when-backing-up-and-restoring-a-vm)

> [!NOTE]
> Tento kurz předpokládá již máte virtuální počítač ve vašem předplatném Azure a že jste zavedli opatření tooallow hello služby zálohování tooaccess hello virtuálních počítačů.
>
>

[!INCLUDE [learn-about-Azure-Backup-deployment-models](../../includes/backup-deployment-models.md)]

V závislosti na hello počet virtuálních počítačů, který má tooprotect, můžete začít z různých počáteční body. Pokud chcete tooback až více virtuálních počítačů v rámci jedné operace, přejděte trezor služeb zotavení toohello a [úlohu zálohování inicializace hello z panelu trezoru hello](backup-azure-vms-first-look-arm.md#configure-the-backup-job-from-the-recovery-services-vault). Pokud chcete tooback jeden virtuální počítač, můžete spustit úlohu zálohování hello z okna Správa virtuálních počítačů.

## <a name="configure-hello-backup-job-from-hello-vm-management-blade"></a>Nakonfigurujte úlohu zálohování hello z okna Správa virtuálních počítačů hello

Pomocí následujících kroků tooconfigure hello úlohu zálohování z okna Správa hello virtuálního počítače v hello portál Azure hello. Tyto kroky se nevztahují toohello virtuální počítače na portálu classic hello.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com/).
2. V nabídce centra hello, klikněte na tlačítko **více služeb** a v dialogovém okně filtru hello, zadejte **virtuální počítače**. Při psaní hello seznam filtrů prostředky. Až uvidíte položku Virtuální počítače, vyberte ji.

  ![V nabídce centra klikněte na dialogové okno text tooopen více služeb a zadejte virtuální počítače](./media/backup-azure-vms-first-look-arm/open-vm-from-hub.png)

  Zobrazí se seznam Hello virtuálních počítačů (VM) v rámci předplatného hello.

  ![Zobrazí se seznam Hello virtuálních počítačů v rámci předplatného hello.](./media/backup-azure-vms-first-look-arm/list-of-vms.png)

3. Hello seznamu vyberte virtuální počítač tooback nahoru.

  ![Zobrazí se seznam Hello virtuálních počítačů v rámci předplatného hello.](./media/backup-azure-vms-first-look-arm/list-of-vms-selected.png)

  Když vyberete hello virtuálních počítačů, hello seznam virtuálních počítačů posune toohello doleva a hello okna Správa virtuálního počítače a řídicí panel hello virtuální počítač, otevřete. </br>
 ![Okno správy virtuálního počítače](./media/backup-azure-vms-first-look-arm/vm-management-blade.png)

4. Na hello okna Správa virtuálních počítačů, v hello **nastavení** klikněte na tlačítko **zálohování**. </br>

  ![Možnost Zálohování v okně správy virtuálního počítače](./media/backup-azure-vms-first-look-arm/backup-option-vm-management-blade.png)

  Otevře se okno zálohování povolit Hello.

  ![Možnost Zálohování v okně správy virtuálního počítače](./media/backup-azure-vms-first-look-arm/vm-blade-enable-backup.png)

5. Hello trezor služeb zotavení, klikněte na tlačítko **vyberte existující** a vyberte z rozevíracího seznamu hello hello trezoru.

  ![Průvodce povolením zálohování](./media/backup-azure-vms-first-look-arm/vm-blade-enable-backup.png)

  Pokud neexistují žádné trezory služeb zotavení, nebo chcete toouse nové úložiště, klikněte na tlačítko **vytvořit nový** a zadejte název hello hello nový trezor. Nový trezor se vytvoří v hello stejnou skupinu prostředků a stejné umístění jako virtuální počítač hello. Pokud chcete toocreate trezoru služeb zotavení s různými hodnotami, o naleznete v části hello příliš[vytvoření trezoru služeb zotavení](backup-azure-vms-first-look-arm.md#create-a-recovery-services-vault-for-a-vm).

6. Klikněte na tlačítko Podrobnosti hello tooview hello zásady zálohování, **zálohování zásad**.

  Hello **zálohování zásad** okno otevře a naleznete zde podrobnosti hello hello vybrané zásady. Pokud existují další zásady, použijte rozevírací nabídky toochoose hello různé zásady zálohování. Pokud chcete toocreate zásadu, vyberte **vytvořit nový** z rozevírací nabídky hello. Pokyny k definování zásad zálohování naleznete v tématu [Definování zásad zálohování](backup-azure-vms-first-look-arm.md#defining-a-backup-policy). zásady zálohování změny toohello hello pro toosave a okno zálohování návratový toohello povolit, klikněte na tlačítko **OK**.

  ![Výběr zásady zálohování](./media/backup-azure-vms-first-look-arm/setting-rs-backup-policy-new-2.png)

7. V okně Zálohování hello povolit, klikněte na tlačítko **povolit zálohování** toodeploy hello zásad. Nasazení zásady hello přidruží k němu hello trezoru a hello virtuálních počítačů.

  ![Tlačítko Povolit zálohování](./media/backup-azure-vms-first-look-arm/vm-management-blade-enable-backup-button.png)

8. Můžete sledovat průběh konfigurace hello prostřednictvím hello oznámení, které se zobrazují v portálu hello. Hello následující příklad ukazuje, že nasazení spuštěné.

  ![Oznámení Povolení zálohování](./media/backup-azure-vms-first-look-arm/vm-management-blade-enable-backup-notification.png)

9. Po dokončení konfigurace průběh hello na hello okna Správa virtuálních počítačů, klikněte na tlačítko **zálohování** tooopen hello zálohované položky okno a zobrazení hello podrobnosti.

  ![Zobrazení Zálohovaná položka virtuálního počítače](./media/backup-azure-vms-first-look-arm/backup-item-view.png)

  Až po dokončení prvotní zálohování hello, **poslední zálohy stavu** zobrazuje jako **upozornění (prvotní zálohování čekající na vyřízení)**. dojde k toosee při hello příští plánovaná úloha zálohování, v části **zálohování zásad** klikněte na název hello hello zásady. okno zásady zálohování Hello otevře a zobrazuje čas hello hello naplánované zálohování.

10. toorun zálohu úlohy a vytvořit hello počátečního bodu obnovení, na hello zálohy trezoru klikněte na okno **zálohovat nyní**.

  ![Klikněte na tlačítko zálohování nyní toorun hello prvotní zálohování](./media/backup-azure-vms-first-look-arm/backup-now.png)

  Otevře se okno zálohovat nyní Hello.

  ![Zobrazí okno zálohovat nyní hello](./media/backup-azure-vms-first-look-arm/backup-now-blade-short.png)

11. Na hello zálohovat nyní okně klikněte na ikonu hello kalendáře, použijte hello kalendáře řízení tooselect hello poslední den tohoto bodu obnovení se zachovává a klikněte na tlačítko **zálohování**.

  ![Sada hello poslední den hello zálohovat nyní bod obnovení je zachován.](./media/backup-azure-vms-first-look-arm/backup-now-blade-calendar.png)

  Nasazení oznámení umožňují vědět byla spuštěna úloha zálohování hello, a že můžete monitorovat průběh hello hello úlohy na stránce úloh zálohování hello.

## <a name="configure-hello-backup-job-from-hello-recovery-services-vault"></a>Nakonfigurujte úlohu zálohování hello z hello trezor služeb zotavení
Úloha zálohování hello tooconfigure, dokončení hello následující kroky.  

1. Vytvoření trezoru služby Recovery Services pro virtuální počítač.
2. Hello použití portálu Azure tooselect scénáři nastavit zásady zálohování a vyhledejte položky tooprotect.
3. Spusťte prvotní zálohování hello.

## <a name="create-a-recovery-services-vault-for-a-vm"></a>Vytvoření trezoru Recovery Services pro virtuální počítač
Trezor služeb zotavení je entita, která ukládá všechny hello zálohy a body obnovení, které byly vytvořeny v čase. Hello trezor služeb zotavení obsahuje také zásady zálohování hello použít toohello chráněné virtuální počítače.

> [!NOTE]
> Zálohování virtuálního počítače je místní proces. Nelze zálohovat virtuální počítače z jednoho umístění tooa trezor služeb zotavení v jiném umístění. Ano pro každou oblast Azure, s toobe virtuální počítače zálohovat, musí existovat alespoň jeden trezor služeb zotavení v tomto umístění.
>
>

toocreate trezoru služeb zotavení:

1. Pokud jste tak již neučinili, přihlaste se toohello [portál Azure](https://portal.azure.com/) pomocí svého předplatného Azure.
2. V nabídce centra hello, klikněte na tlačítko **další služby** a v hello typ filtru dialogové okno **služeb zotavení**. Při psaní hello seznam filtrů prostředky. Až uvidíte trezory služeb zotavení v hello seznamu, klikněte na něj.

    ![Vytvoření trezoru Recovery Services – krok 1](./media/backup-try-azure-backup-in-10-mins/open-rs-vault-list.png) <br/>

    Pokud jsou v rámci předplatného hello trezory služeb zotavení, jsou uvedeny hello trezorů.

    ![Vytvoření trezoru Recovery Services – krok 2](./media/backup-azure-vms-first-look-arm/list-of-rs-vault.png)
3. Na hello **trezory služeb zotavení** nabídky, klikněte na tlačítko **přidat**.

    ![Vytvoření trezoru Recovery Services – krok 2](./media/backup-try-azure-backup-in-10-mins/rs-vault-menu.png)

    Služby zotavení Hello otevře se okno trezoru výzvou tooprovide **název**, **předplatné**, **skupiny prostředků**, a **umístění**.

    ![Vytvoření trezoru Recovery Services – krok 3](./media/backup-try-azure-backup-in-10-mins/rs-vault-step-3.png)

4. Pro **název**, zadejte popisný název tooidentify hello trezoru. Název Hello musí toobe jedinečný pro hello předplatného Azure. Zadejte název v rozsahu 2 až 50 znaků. Musí začínat písmenem a může obsahovat pouze písmena, číslice a pomlčky.

5. V hello **předplatné** pomocí hello rozevírací nabídky toochoose hello předplatného Azure. Pokud chcete použít jenom jedno předplatné, zobrazí se toto předplatné a toohello další krok můžete vynechat. Pokud si nejste jisti, jaké předplatné toouse, použít výchozí hello (nebo navrhované) předplatné. Více možností je dostupných, jen pokud je váš účet organizace přidružený k více předplatným Azure.

6. V hello **skupiny prostředků** části:

    * Vyberte **vytvořit nový** Pokud chcete, aby toocreate skupinu prostředků.
    Nebo
    * Vyberte **použít existující** a klikněte na tlačítko hello rozevírací nabídky toosee hello seznam dostupných skupin prostředků.

  Úplné informace o skupinách prostředků najdete v tématu hello [přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md).

7. Klikněte na tlačítko **umístění** tooselect hello zeměpisnou oblast trezoru hello. Tato volba určuje hello zeměpisnou oblast, kde vaše zálohovaná data se odesílají.

  > [!IMPORTANT]
  > Pokud si nejste jistí hello umístění, ve kterém je váš virtuální počítač, přejděte toohello seznam virtuálních počítačů hello portálu a zavřete dialogové okno Vytvoření trezoru hello. Pokud máte virtuální počítače v několika oblastech, vytvořte trezor služby Recovery Services v každé z nich. Vytvořte trezor hello v první oblasti hello před přechodem toohello další umístění. Není definován že žádný účet úložiště hello toospecify nutnost použít toostore hello zálohovaných dat – trezor služeb zotavení hello a hello služba Azure Backup automaticky zpracovat hello úložiště.
  >

8. Hello dolní části hello okno trezoru služeb zotavení, klikněte na **vytvořit**.

    Můžete to trvat několik minut, než hello toobe vytvořit trezor služeb zotavení. Sledujte oznámení stavu hello v horním pravém oblasti hello hello portálu. Po vytvoření svůj trezor se zobrazí v seznamu hello trezorů služeb zotavení. Pokud se trezor nezobrazí ani po několika minutách, klikněte na **Obnovit**.

    ![Kliknutí na tlačítko Obnovit](./media/backup-try-azure-backup-in-10-mins/refresh-button.png)</br>

    Jakmile se zobrazí váš trezor hello seznamu trezorů služeb zotavení, jste redundance úložiště připravené tooset hello.

Teď, když jste vytvořili trezor, zjistěte, jak tooset hello replikace úložiště.

### <a name="set-storage-replication"></a>Nastavení replikace úložiště
možnost replikace úložiště Hello umožňuje toochoose mezi geograficky redundantním úložištěm a místně redundantního úložiště. Ve výchozím nastavení má váš trezor nastavené geograficky redundantní úložiště. Pokud hello trezor služeb zotavení je vaši primární zálohu, nechte hello úložiště replikace možnost set toogeo redundantní úložiště. Pokud chcete levnější možnost, která není tak trvanlivá, vyberte místně redundantní úložiště. Další informace o [geograficky redundantní](../storage/common/storage-redundancy.md#geo-redundant-storage) a [místně redundantní](../storage/common/storage-redundancy.md#locally-redundant-storage) možnosti úložiště v hello [Přehled replikace Azure Storage](../storage/common/storage-redundancy.md).

nastavení replikace úložiště tooedit hello:

1. Z hello **trezory služeb zotavení** okně, vyberte hello nový trezor.

  ![Vyberte nový trezor hello ze seznamu hello trezor služeb zotavení](./media/backup-try-azure-backup-in-10-mins/rs-vault-list.png)

  Když vyberete hello trezoru, hello okno nastavení (*který má název trezoru hello v horní části hello*) a otevřete okno Podrobnosti trezoru hello.

  ![Zobrazení hello konfiguraci úložiště pro nový trezor](./media/backup-try-azure-backup-in-10-mins/set-storage-configuration-2.png)

2. V okně Nastavení hello nový trezor, použijte hello svislé snímku tooscroll dolů toohello části Správa a klikněte na tlačítko **infrastruktura zálohování**.
    Otevře se okno infrastruktura zálohování Hello.
3. V okně infrastruktura zálohování hello, klikněte na tlačítko **konfigurace zálohování** tooopen hello **konfigurace zálohování** okno.

    ![Nastavit hello konfiguraci úložiště pro nový trezor](./media/backup-try-azure-backup-in-10-mins/set-storage-configuration.png)
4. Zvolte hello vhodnou možnost replikace pro svůj trezor.

    ![volby konfigurace úložiště](./media/backup-try-azure-backup-in-10-mins/choose-storage-configuration.png)

    Ve výchozím nastavení má váš trezor nastavené geograficky redundantní úložiště. Pokud používáte Azure jako koncový bod primární úložiště záloh, pokračujte toouse **geograficky redundantní**. Pokud nepoužíváte Azure jako koncový bod primární úložiště záloh, zvolte **místně redundantní**, což snižuje náklady na úložiště Azure hello. Další informace o možnostech [geograficky redundantního](../storage/common/storage-redundancy.md#geo-redundant-storage) a [místně redundantního](../storage/common/storage-redundancy.md#locally-redundant-storage) úložiště najdete v tomto [přehledu redundance úložiště](../storage/common/storage-redundancy.md).


## <a name="select-a-backup-goal-set-policy-and-define-items-tooprotect"></a>Výběr cíle zálohování, nastavení zásad a definovat tooprotect položky
Před registrací virtuálních počítačů k trezoru, spusťte tooensure proces zjišťování hello, jsou identifikované všechny nové virtuální počítače, které byly přidány toohello předplatné. Hello proces se dotáže Azure hello seznam virtuálních počítačů v rámci předplatného hello, společně s dalšími informacemi, jako třeba název hello cloudové služby a oblast hello. V hello portálu Azure výraz scénář vyjadřuje toowhat budete tooput do trezoru služeb zotavení hello. Zásada je plán hello jak často a kdy jsou pořizovány body obnovení. Zásada také obsahuje hello rozsah uchování pro body obnovení hello.

1. Pokud již máte otevřete trezoru služeb zotavení, pokračujte toostep 2. Jinak, v nabídce centra hello, klikněte na tlačítko **další služby** a v hello seznamu prostředků zadejte **služeb zotavení** a klikněte na tlačítko **trezory služeb zotavení**.

    ![Vytvoření trezoru Recovery Services – krok 1](./media/backup-try-azure-backup-in-10-mins/open-rs-vault-list.png) <br/>

    Zobrazí se seznam Hello trezory služeb zotavení.

    ![Zobrazení hello služeb zotavení trezory seznamu](./media/backup-azure-arm-vms-prepare/rs-list-of-vaults.png)

    Hello seznamu trezorů služeb zotavení vyberte trezor tooopen jeho řídicího panelu.

     ![Otevřené okno trezoru](./media/backup-azure-arm-vms-prepare/new-vault-settings-blade.png)

2. V nabídce řídícího panelu trezoru hello, klikněte na tlačítko **zálohování** okno zálohování tooopen hello.

    ![Otevřené okno Zálohování](./media/backup-azure-arm-vms-prepare/backup-button.png)

    otevření okna zálohování a cíl zálohování Hello.

    ![Otevřené okno Scénář](./media/backup-azure-arm-vms-prepare/select-backup-goal-1.png)
3. V okně cíl zálohování hello, z hello **kde běží vaše úlohy** rozevírací nabídky vyberte Azure. Z hello **co chcete toobackup** rozevíracího seznamu, vyberte virtuální počítač a pak klikněte na tlačítko **OK**.

    Tyto akce zaregistrovat hello rozšíření virtuálního počítače k trezoru hello. Hello zavře se okno cíl zálohování a hello **zálohování zásad** otevře se okno.

    ![Otevřené okno Scénář](./media/backup-azure-arm-vms-prepare/select-backup-goal-2.png)

4. V okně zásady zálohování hello vyberte zásady zálohování hello chcete tooapply toohello trezoru.

    ![Výběr zásady zálohování](./media/backup-azure-arm-vms-prepare/setting-rs-backup-policy-new.png)

    v rozevírací nabídce hello jsou uvedeny podrobnosti Hello hello výchozích zásad. Pokud chcete toocreate zásadu, vyberte **vytvořit nový** z rozevírací nabídky hello. Pokyny k definování zásad zálohování naleznete v tématu [Definování zásad zálohování](backup-azure-vms-first-look-arm.md#defining-a-backup-policy).
    Klikněte na tlačítko **OK** tooassociate zásady zálohování hello hello trezoru.

    Dobrý den, zavře okno zásady zálohování a hello **vybrat virtuální počítače** otevře se okno.
5. V hello **vybrat virtuální počítače** okně zvolte hello tooassociate virtuální počítače s hello zadané zásady a klikněte na tlačítko **OK**.

    ![Výběr úlohy](./media/backup-azure-arm-vms-prepare/select-vms-to-backup.png)

    Hello vybraný virtuální počítač byl ověřen. Pokud nevidíte hello virtuální počítače, které jste očekávali toosee, zkontrolujte, zda existují v hello stejného umístění Azure jako trezor služeb zotavení hello. umístění Hello hello trezor služeb zotavení se zobrazí na panelu trezoru hello.

6. Teď, když jste definovali všechna nastavení pro hello trezor, v okně Zálohování hello, klikněte na tlačítko **povolit zálohování** toodeploy hello zásad toohello trezoru a hello virtuálních počítačů. Nasazení zásady zálohování hello nevytváří hello počátečního bodu obnovení pro virtuální počítač hello.

    ![Povolení zálohování](./media/backup-azure-arm-vms-prepare/vm-validated-click-enable.png)

Po povolení úspěšně hello zálohování, zásady zálohování spustí podle plánu. Pokračujte, ale tooinitiate hello první úlohu zálohování.

## <a name="initial-backup"></a>Prvotní zálohování
Jakmile zásady zálohování nasazený na virtuálním počítači hello, která nemá význam hello dat byly zálohovány. Ve výchozím nastavení hello prvním plánovaným zálohováním (definovaným v zásadě zálohování hello) je hello prvotní zálohování. Dokud prvotního zálohování hello hello stav poslední zálohy na hello **úlohy zálohování** ukazovat **upozornění (nedokončené prvotní zálohování)**.

![Zálohování čeká na zpracování](./media/backup-azure-vms-first-look-arm/initial-backup-not-run.png)

Pokud prvotní zálohování toobegin brzy, doporučujeme spustit **zálohovat nyní**.

toorun hello úlohu prvotního zálohování:

1. Na řídicím panelu trezoru hello, klikněte na číslo hello pod **zálohování položek**, nebo klikněte na tlačítko hello **zálohování položek** dlaždici. <br/>
  ![Ikona nastavení](./media/backup-azure-vms-first-look-arm/rs-vault-config-vm-back-up-now-1.png)

  Hello **zálohování položek** otevře se okno.

  ![Zálohování položek](./media/backup-azure-vms-first-look-arm/back-up-items-list.png)

2. Na hello **zálohování položek** okně, vyberte hello položky.

  ![Ikona nastavení](./media/backup-azure-vms-first-look-arm/back-up-items-list-selected.png)

  Hello **zálohování položek** seznamu otevře. <br/>

  ![Aktivovaná úloha zálohování.](./media/backup-azure-vms-first-look-arm/backup-items-not-run.png)

3. Na hello **zálohování položek** klikněte na symbol tří teček hello **...**  tooopen hello kontextové nabídky.

  ![Místní nabídka](./media/backup-azure-vms-first-look-arm/context-menu.png)

  Zobrazí se Hello kontextové nabídky.

  ![Místní nabídka](./media/backup-azure-vms-first-look-arm/context-menu-small.png)

4. V místní nabídce hello, klikněte na tlačítko **zálohovat nyní**.

  ![Místní nabídka](./media/backup-azure-vms-first-look-arm/context-menu-small-backup-now.png)

  Otevře se okno zálohovat nyní Hello.

  ![Zobrazí okno zálohovat nyní hello](./media/backup-azure-vms-first-look-arm/backup-now-blade-short.png)

5. Na hello zálohovat nyní okně klikněte na ikonu hello kalendáře, použijte hello kalendáře řízení tooselect hello poslední den tohoto bodu obnovení se zachovává a klikněte na tlačítko **zálohování**.

  ![Sada hello poslední den hello zálohovat nyní bod obnovení je zachován.](./media/backup-azure-vms-first-look-arm/backup-now-blade-calendar.png)

  Nasazení oznámení umožňují vědět byla spuštěna úloha zálohování hello, a že můžete monitorovat průběh hello hello úlohy na stránce úloh zálohování hello. V závislosti na velikosti hello vašeho virtuálního počítače vytváření hello prvotní zálohování může chvíli trvat.

6. tooview nebo sledovat stav hello hello prvotní zálohování, na panelu trezoru hello na hello **úlohy zálohování** klikněte na dlaždici **v průběhu**.

  ![Dlaždice Úlohy zálohování](./media/backup-azure-vms-first-look-arm/open-backup-jobs-1.png)

  Otevře se okno úlohy zálohování Hello.

  ![Dlaždice Úlohy zálohování](./media/backup-azure-vms-first-look-arm/backup-jobs-in-jobs-view-1.png)

  V hello **zálohování úloh** okně uvidíte hello stav všech úloh. Zkontrolujte, zda hello úloha zálohování pro virtuální počítač stále probíhá, nebo pokud se nedokončí. Po dokončení úlohy zálohování se stav hello je *dokončeno*.

  > [!NOTE]
  > Jako součást operace zálohování hello vydá hello služba Azure Backup příkaz toohello rozšířením zálohování v jednotlivých virtuálních počítačů tooflush všech zápisů a pořízení konzistentního snímku.
  >
  >


[!INCLUDE [backup-create-backup-policy-for-vm](../../includes/backup-create-backup-policy-for-vm.md)]

## <a name="install-hello-vm-agent-on-hello-virtual-machine"></a>Nainstalujte agenta virtuálního počítače hello hello virtuálního počítače
Tyto informace jsou poskytnuté pro případ potřeby. Hello agenta virtuálního počítače Azure musí být nainstalován na hello virtuálního počítače, které jsou pro toowork rozšíření hello zálohování Azure. Ale pokud virtuální počítač byl vytvořen z Galerie Azure hello, pak hello agenta virtuálního počítače již existuje v hello virtuálního počítače. Virtuální počítače, které se přenáší z místních datových center nebude nainstalována hello agenta virtuálního počítače. V takovém případě musí hello agenta virtuálního počítače toobe nainstalována. Pokud máte problémy se zálohováním hello virtuálního počítače Azure, zkontrolujte, zda text hello agenta virtuálního počítače Azure je správně nainstalovaná hello virtuálního počítače (viz následující tabulka hello). Pokud vytvoříte vlastní virtuální počítač, [zkontrolujte hello **hello instalace agenta virtuálního počítače** je zaškrtnuté políčko](../virtual-machines/windows/classic/agents-and-extensions.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) před zřízení hello virtuálního počítače.

Další informace o hello [agenta virtuálního počítače](https://go.microsoft.com/fwLink/?LinkID=390493&clcid=0x409) a [jak tooinstall ho](../virtual-machines/windows/classic/manage-extensions.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

Hello následující tabulka obsahuje další informace o hello virtuální počítač agenta pro Windows a virtuální počítače s Linuxem.

| **Operace** | **Windows** | **Linux** |
| --- | --- | --- |
| Instalace hello agenta virtuálního počítače |<li>Stáhněte a nainstalujte hello [MSI agenta](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Potřebujete instalaci hello toocomplete oprávnění správce. <li>[Aktualizovat vlastnosti virtuálního počítače hello](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) tooindicate, který hello agenta je nainstalovaná. |<li> Nainstalujte nejnovější hello [agenta systému Linux](https://github.com/Azure/WALinuxAgent) z Githubu. Potřebujete instalaci hello toocomplete oprávnění správce. <li> [Aktualizovat vlastnosti virtuálního počítače hello](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) tooindicate, který hello agenta je nainstalovaná. |
| Aktualizace hello agenta virtuálního počítače |Hello aktualizace agenta virtuálního počítače je stejně jednoduché jako přeinstalování hello [binárních souborů agenta virtuálního počítače](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). <br>Ujistěte se, že během aktualizace agenta virtuálního počítače hello neběží žádná operace zálohování. |Postupujte podle pokynů hello na [aktualizace agenta virtuálního počítače Linux hello](../virtual-machines/linux/update-agent.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). <br>Ujistěte se, že při hello Probíhá aktualizace agenta virtuálního počítače neběží žádná operace zálohování. |
| Ověření instalace agenta virtuálního počítače hello |<li>Přejděte toohello *C:\WindowsAzure\Packages* složky v hello virtuálního počítače Azure. <li>Byste měli najít přítomný soubor WaAppAgent.exe hello.<li> Klikněte pravým tlačítkem na soubor hello, přejděte příliš**vlastnosti**a potom vyberte hello **podrobnosti** kartě hello verze produktu pole by mělo být 2.6.1198.718 nebo vyšší. |Není k dispozici |

### <a name="backup-extension"></a>Rozšíření zálohování
Jednou hello agenta virtuálního počítače je nainstalována na virtuálním počítači hello, hello služba Azure Backup nainstaluje hello rozšíření zálohování toohello agenta virtuálního počítače. Hello služba Azure Backup bezproblémově upgraduje a opravy hello rozšíření zálohování bez dalšího zásahu uživatele.

Služba zálohování Hello nainstaluje rozšíření zálohování hello i v případě, že hello virtuální počítač neběží. Spuštění virtuálního počítače poskytuje hello největší šanci získání bod obnovení konzistentních s aplikací. Hello služby Azure Backup však pokračuje tooback až hello virtuálních počítačů, i když je vypnutý a rozšíření hello nebylo možné nainstalovat. Tento typ zálohy je známý jako Offline virtuální počítač a bod obnovení hello je *zhroutí konzistentní*.

## <a name="troubleshooting-information"></a>Informace o řešení potíží
Pokud máte problémy s plněním některých úkolů hello v tomto článku, projděte si [pokyny při řešení potíží](backup-azure-vms-troubleshoot.md).

## <a name="pricing"></a>Ceny
zálohování virtuálních počítačů Azure náklady Hello je založena na hello počet chráněné instance. Definici chráněné instance najdete v tématu [Co je chráněná instance](backup-introduction-to-azure-backup.md#what-is-a-protected-instance). Příklad výpočet nákladů hello zálohování virtuálních počítačů, naleznete v části [výpočet chráněné instance](backup-azure-vms-introduction.md#calculating-the-cost-of-protected-instances). V tématu hello cenách zálohování Azure stránka informace o [cenách zálohování](https://azure.microsoft.com/pricing/details/backup/).

## <a name="questions"></a>Máte dotazy?
Pokud máte otázky, nebo pokud se některé funkce, které byste chtěli toosee zahrnuty, [pošlete nám svůj názor](http://aka.ms/azurebackup_feedback).
