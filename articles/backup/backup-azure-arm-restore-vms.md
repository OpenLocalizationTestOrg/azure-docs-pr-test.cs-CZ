---
title: "Zálohování Azure: Obnovení virtuálních počítačů pomocí hello portálu Azure | Microsoft Docs"
description: "Obnovení virtuálního počítače Azure z bodu obnovení pomocí portálu Azure"
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: "obnovení zálohy; jak toorestore; bod obnovení;"
ms.assetid: 372b87c6-3544-4dc5-bbc9-c742ca502159
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/15/2017
ms.author: markgal;trinadhk;
ms.openlocfilehash: f4f75d1da73c7760d2952afe80ff94918a08351c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-portal-toorestore-virtual-machines"></a>Použití portálu Azure toorestore virtuální počítače
> [!div class="op_single_selector"]
> * [Obnovení virtuálních počítačů v klasickém portálu](backup-azure-restore-vms.md)
> * [Obnovení virtuálních počítačů na portálu Azure](backup-azure-arm-restore-vms.md)
>
>

Ochrana dat pomocí snímky dat na definovaných intervalech. Tyto snímky jsou známé jako body obnovení a jsou uložené v trezory služeb zotavení. Pokud nebo, pokud je nutné toorepair nebo znovu sestavte virtuální počítač, můžete obnovit hello virtuálních počítačů z libovolného hello uložit body obnovení. Při obnovování bod obnovení, můžete vytvořit nový virtuální počítač, který je reprezentace v okamžiku zálohy virtuálního počítače, nebo obnovení disků a použít hello šablonu, která se dodává spolu se toocustomize hello obnovit virtuální počítač nebo provést obnovení jednotlivých souborů. Tento článek vysvětluje, jak toorestore virtuálních počítačů tooa nového virtuálního počítače nebo obnovení všech zálohovaných disky. Obnovení jednotlivých souborů, najdete v části příliš[obnovit soubory ze zálohy virtuálního počítače Azure](backup-azure-restore-files-from-vm.md)

![3-Ways-Restore-from-VM-Backup](./media/backup-azure-arm-restore-vms/azure-vm-backup-restore.png)

> [!NOTE]
> Azure má dva modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../azure-resource-manager/resource-manager-deployment-model.md). Tento článek obsahuje hello informace a postupy pro obnovení virtuální počítače nasazené pomocí modelu Resource Manager hello.
>
>

Obnovení virtuálního počítače nebo všechny disky z virtuálního počítače zálohování zahrnuje dva kroky:

1. Vyberte bod obnovení pro obnovení
2. Výběr hello obnovte typu – vytvoření nového virtuálního počítače nebo obnovení disků a zadejte požadované parametry. 

## <a name="select-restore-point-for-restore"></a>Vyberte bod obnovení pro obnovení
1. Přihlaste se toohello [portálu Azure](http://portal.azure.com/)
2. V nabídce Azure hello, klikněte na tlačítko **Procházet** a v seznamu hello služeb, zadejte **služeb zotavení**. Hello seznam služeb upraví toowhat, které zadáte. Až se zobrazí **trezory služeb zotavení**, vyberte ho.

    ![Otevřený trezor služeb zotavení](./media/backup-azure-arm-restore-vms/open-recovery-services-vault.png)

    Zobrazí se Hello seznamu trezorů v předplatném hello.

    ![Trezory seznam služeb zotavení](./media/backup-azure-arm-restore-vms/list-of-rs-vaults.png)
3. Hello seznamu vyberte hello trezoru přidružené hello chcete toorestore virtuálních počítačů. Po kliknutí na tlačítko hello trezoru, otevře se jeho řídicí panel.

    ![Trezory seznam služeb zotavení](./media/backup-azure-arm-restore-vms/select-vault-open-vault-blade.png)
4. Teď, když jste na řídicím panelu trezoru hello. Na hello **zálohování položek** dlaždici, klikněte na tlačítko **virtuální počítače Azure** toodisplay hello virtuální počítače přidružené k trezoru hello.

    ![panelu trezoru](./media/backup-azure-arm-restore-vms/vault-dashboard.png)

    Hello **zálohování položek** okno otevře a zobrazí se seznam hello virtuální počítače Azure.

    ![Seznam virtuálních počítačů v trezoru](./media/backup-azure-arm-restore-vms/list-of-vms-in-vault.png)
5. Hello seznamu vyberte řídicí panel hello tooopen virtuálních počítačů. Otevře se řídicí panel virtuálních počítačů Hello toohello monitorování oblast, která obsahuje dlaždice body obnovení hello.

    ![Seznam virtuálních počítačů v trezoru](./media/backup-azure-arm-restore-vms/vm-blade.png)
6. V nabídce řídícího panelu hello virtuálních počítačů, klikněte na tlačítko **obnovení**

    ![Seznam virtuálních počítačů v trezoru](./media/backup-azure-arm-restore-vms/vm-blade-menu-restore.png)

    Otevře se okno obnovení Hello.

    ![okno obnovení](./media/backup-azure-arm-restore-vms/restore-blade.png)
7. Na hello **obnovení** okně klikněte na tlačítko **bod obnovení** tooopen hello **obnovení vyberte bod** okno.

    ![okno obnovení](./media/backup-azure-arm-restore-vms/recovery-point-selector.png)

    Ve výchozím nastavení zobrazí dialogové okno hello všechny body obnovení z hello posledních 30 dnů. Použití hello **filtru** tooalter hello časové rozmezí hello obnovit body zobrazí. Ve výchozím nastavení zobrazí se body obnovení všech konzistence. Upravit **obnovit všechny body** filtrovat tooselect konkrétní konzistence bodů obnovení. Další informace o jednotlivých typech bodu obnovení najdete v tématu Vysvětlení hello [konzistenci dat](backup-azure-vms-introduction.md#data-consistency).  

   * **Obnovení bodu konzistence** z tohoto seznamu vyberte:
     * Body obnovení konzistentní havárií
     * Body obnovení konzistentní aplikací,
     * Body obnovení konzistentní se systémem souborů
     * Všechny body obnovení.  
8. Zvolte bod obnovení a klikněte na tlačítko **OK**.

    ![Vyberte bod obnovení](./media/backup-azure-arm-restore-vms/select-recovery-point.png)

    Hello **obnovení** nastavena hello bodu obnovení se zobrazí okno.

    ![bod obnovení je nastaven.](./media/backup-azure-arm-restore-vms/recovery-point-set.png)
9. Na hello **obnovení** okně **obnovit konfiguraci** automaticky otevře po nastavení bodu obnovení.

## <a name="choosing-a-vm-restore-configuration"></a>Výběr obnovení konfigurace virtuálního počítače
Teď, když jste vybrali hello bod obnovení, vyberte takovou konfiguraci pro obnovení virtuálního počítače. Vaše možnosti pro konfiguraci hello obnovit virtuální počítač se toouse: portál Azure nebo PowerShell.

1. Pokud si nejste již existuje, přejděte toohello **obnovení** okno. Zkontrolujte [bod obnovení byl vybrán](#select-restore-point-for-restore)a klikněte na tlačítko **obnovit konfiguraci** tooopen hello **konfigurace obnovení** okno.

    ![Průvodce obnovením konfigurace je nastavena.](./media/backup-azure-arm-restore-vms/recovery-configuration-wizard-recovery-type.png)
2. Na hello **obnovit konfiguraci** okno, máte dvě možnosti:
   * Úplná virtuální počítač obnovit
   * Obnovení zálohovaných disky

Portál nabízí možnost vytvořit pro obnovený virtuální počítač. Pokud chcete, aby konfigurace virtuálního počítače hello toocustomize nebo názvy hello prostředků, které jsou vytvořené jako součást vytvořit novou volbu virtuálních počítačů, pomocí prostředí PowerShell nebo portálu toorestore zálohovat disky a použití tooattach příkazy prostředí PowerShell je toochoice konfigurací nebo s využitím šablony virtuálního počítače, se dodává s obnovením obnovit hello toocustomize disky virtuálních počítačů. V tématu [obnovení virtuálního počítače s konfigurací speciální síťových](#restoring-vms-with-special-network-configurations) podrobnosti o tom toorestore virtuálních počítačů, která má několik síťových adaptérů nebo v části nástroj pro vyrovnávání zatížení. Pokud vaše virtuální počítač Windows používá [ROZBOČOVAČE licencování](../virtual-machines/windows/hybrid-use-benefit-licensing.md), třeba toorestore disky a pomocí prostředí PowerShell nebo šablony uvedeného níže toocreate hello virtuální počítač a ujistěte se, že zadáváte LicenseType jako "Windows_Server" při vytváření virtuálních počítačů tooavail ROZBOČOVAČE výhody obnoveným virtuálním Počítačem. 
 
## <a name="create-a-new-vm-from-restore-point"></a>Vytvoření nového virtuálního počítače z bodu obnovení
Pokud si nejste již existuje, [vyberte bod obnovení](#restoring-vms-with-special-network-configurations) před pokračováním toocreating nový virtuální počítač z obnovení bodu. Jakmile je vybrán bod obnovení, na hello **obnovit konfiguraci** okno, zadejte nebo vyberte hodnoty pro každý hello následující pole:

* **Obnovení typ** – vytvoření virtuálního počítače.
* **Název virtuálního počítače** – zadejte název pro hello virtuálních počítačů. Hello název musí mít skupiny prostředků jedinečný toohello (u virtuálních počítačů nasazených Resource Managerem) nebo cloudové služby (pro klasické virtuální počítač). Hello virtuálního počítače nelze nahradit, pokud již existuje v rámci předplatného hello.
* **Skupina prostředků** – použít existující skupinu prostředků nebo vytvořte novou. Pokud obnovujete klasické virtuální počítač, použijte tento název pole toospecify hello novou cloudovou službu. Pokud vytváříte novou skupinu nebo cloudovou službu prostředku, musí být globálně jedinečný název hello. Obvykle je název cloudové služby hello související s veřejnou adresou URL, -, například: [cloudservice]. cloudapp.net. Když zkusíte toouse název hello cloudu prostředků skupiny nebo Cloudová služba, která je již používán, Azure přiřadí hello prostředků skupiny nebo Cloudová služba hello stejný název jako hello virtuálních počítačů. Azure zobrazí prostředků skupiny nebo cloudové služby a virtuální počítače nejsou přiřazeny žádné skupiny vztahů. Další informace najdete v tématu [jak toomigrate ze skupiny vztahů tooa regionální virtuální síť (VNet)](../virtual-network/virtual-networks-migrate-to-regional-vnet.md).
* **Virtuální síť** – vyberte hello hello virtuální síť (VNET) při vytváření virtuálního počítače. pole Hello poskytuje všechny virtuální sítě přidružené k předplatnému hello. Skupiny prostředků hello virtuálního počítače se zobrazí v závorkách.
* **Podsíť** – Pokud hello virtuální sítě má podsítě, ve výchozím nastavení je vybrán první podsíť hello. Pokud existují další podsítě, vyberte podsíť hello potřeby.
* **Účet úložiště** -této nabídky jsou uvedeny hello účty úložiště ve hello stejné umístění jako hello trezoru služeb zotavení. Účty úložiště, které jsou Zónově redundantní nejsou podporovány. Pokud neexistují žádné účty úložiště s hello stejné umístění jako hello trezor služeb zotavení, musíte vytvořit před zahájením operace obnovení hello. Typ účtu úložiště Hello replikace je uveden v závorkách.

![Průvodce konfigurací obnovení je nastaven.](./media/backup-azure-arm-restore-vms/recovery-configuration-wizard.png)

> [!NOTE]
> 1. Pokud obnovujete virtuálních počítačů nasazených Resource Managerem, musíte určit virtuální síť (VNET). Virtuální síť (VNET) je pro klasické virtuální počítač volitelné.
> 2. Pokud obnovujete virtuální počítače s spravované disky, ujistěte se, že vybraný účet úložiště není povolen pro Encryption(SSE) služby úložiště v celé jeho životnosti.
> 3. Na základě hello úložiště typu účtu úložiště vybrané (standard nebo premium), všechny disky, obnovení bude premium nebo standardní disky. Aktuálně nepodporujeme smíšený režim disků při obnovení.  
>
>

Na hello **obnovit konfiguraci** okně klikněte na tlačítko **OK** toofinalize hello obnovit konfiguraci. Na hello **obnovení** okně klikněte na tlačítko **obnovení** operace obnovení tootrigger hello.

## <a name="restore-backed-up-disks"></a>Obnovení zálohovaných disky
Pokud chcete toocustomize hello virtuálního počítače chcete toocreate z zálohovat discích, než co se nachází v okně Konfigurace obnovení, vyberte **obnovit disky** jako hodnota **obnovit typu**. Tato volba požádá o účet úložiště, které jsou zkopírovány disky ze zálohy. Při výběru účtu úložiště, vyberte účet, že sdílené složky hello stejné umístění jako trezor služeb zotavení hello. Účty úložiště, které jsou Zónově redundantní nejsou podporovány. Pokud neexistují žádné účty úložiště s hello stejné umístění jako hello trezor služeb zotavení, musíte vytvořit před zahájením operace obnovení hello. Typ účtu úložiště Hello replikace je uveden v závorkách.

Po dokončení operace obnovení můžete provést tyto akce:
* [Použití šablony toocustomize hello obnovit virtuální počítač](#use-templates-to-customize-restore-vm)
* [Použití hello obnovit disky tooattach tooan existující virtuální počítač](../virtual-machines/windows/attach-managed-disk-portal.md)
* [Vytvoření nového virtuálního počítače z obnovené disky pomocí prostředí PowerShell.](./backup-azure-vms-automation.md#restore-an-azure-vm)

Na hello **obnovit konfiguraci** okně klikněte na tlačítko **OK** toofinalize hello obnovit konfiguraci. Na hello **obnovení** okně klikněte na tlačítko **obnovení** operace obnovení tootrigger hello.

![Konfigurace obnovení byla dokončena](./media/backup-azure-arm-restore-vms/trigger-restore-operation.png)

## <a name="track-hello-restore-operation"></a>Sledovat operace obnovení hello
Jakmile spustíte operaci obnovení hello, hello služby Backup vytvoří úlohu pro sledování hello operace obnovení. Hello služby Backup také vytvoří a dočasně zobrazí hello oznámení v oblasti oznámení portálu. Pokud nevidíte hello oznámení, můžete vždy kliknutím tooview ikonu oznámení hello oznámení.

![Spustí obnovení](./media/backup-azure-arm-restore-vms/restore-notification.png)

tooview hello operace při zpracování nebo tooview po jeho dokončení, otevřete seznam úloh zálohování hello.

1. V nabídce Azure hello, klikněte na tlačítko **Procházet** a v seznamu hello služeb, zadejte **služeb zotavení**. Hello seznam služeb upraví toowhat, které zadáte. Až se zobrazí **trezory služeb zotavení**, vyberte ho.

    ![Otevřený trezor služeb zotavení](./media/backup-azure-arm-restore-vms/open-recovery-services-vault.png)

    Zobrazí se Hello seznamu trezorů v předplatném hello.

    ![Trezory seznam služeb zotavení](./media/backup-azure-arm-restore-vms/list-of-rs-vaults.png)
2. Hello seznamu vyberte hello trezoru přidružené hello virtuálního počítače můžete obnovit. Po kliknutí na tlačítko hello trezoru, otevře se jeho řídicí panel.
3. Na řídicím panelu trezoru hello na hello **úlohy zálohování** dlaždici, klikněte na tlačítko **virtuální počítače Azure** toodisplay hello úlohy přidružený k trezoru hello.

    ![panelu trezoru](./media/backup-azure-arm-restore-vms/vault-dashboard-jobs.png)

    Hello **úlohy zálohování** okno otevře a zobrazí hello seznam úloh.

    ![Seznam virtuálních počítačů v trezoru](./media/backup-azure-arm-restore-vms/restore-job-in-progress.png)
    
## <a name="use-templates-toocustomize-restore-vm"></a>Pomocí šablony toocustomize obnovení virtuálního počítače
Jednou [dokončení operace obnovení disky](#Track-the-restore-operation), hello šablonu, aby se vygenerovala můžete použít jako součást obnovení operace toocreate nový virtuální počítač s konfigurací liší od zálohování konfigurace nebo toocustomize názvy prostředků vytvořit, protože vytvoření nového virtuálního počítače z bodu obnovení. 

> [!NOTE]
> Šablony budou přidáni jako součást obnovení disky pro body obnovení po 1. března 2017 provedeny. Jsou použitelné pro disk bez šifrování a nespravované virtuální počítače. Podpora pro šifrované virtuální počítače a spravovat virtuální počítače disku se připravuje a v budoucích verzích. 
>
>

Šablona hello tooget generované v rámci možnost obnovení disky,

1. Přejděte podrobnosti úlohy toorestore odpovídající toohello úlohy. 
2. Na úvodní obrazovka podrobnosti úlohy obnovení, klikněte na *nasazení šablony* tlačítko tooinitiate šablonu nasazení. 

     ![obnovit úlohu procházení](./media/backup-azure-arm-restore-vms/restore-job-drill-down.png)
   
V okně Šablony hello nasazení pro vlastní nasazení, použijte šablonu nasazení příliš[upravit a nasadit šablonu hello](../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template) nebo připojit další přizpůsobení podle [šablonu](../azure-resource-manager/resource-group-authoring-templates.md) před nasazením. 

   ![načítání nasazení šablony](./media/backup-azure-arm-restore-vms/loading-template.png)
   
Po zadání hello požadované hodnoty, přijměte hello *podmínky a ujednání* a klikněte na **nákupu**.

   ![odesílá se nasazení šablony](./media/backup-azure-arm-restore-vms/submitting-template.png)

## <a name="post-restore-steps"></a>Po obnovení kroky
* Pokud použijete distribuci systému Linux na základě inicializací cloudu, jako například Ubuntu, z bezpečnostních důvodů, hesla zablokuje post obnovení. Prosím používá rozšíření VMAccess na hello obnovit virtuální počítač příliš[resetovat heslo hello](../virtual-machines/linux/classic/reset-access.md). Doporučujeme používat klíče SSH na tyto distribuce tooavoid resetování hesla post obnovení.
* Při zálohování konfigurace hello rozšíření bude nainstalována, ale nebude povolen. Pokud zjistíte, jakýkoli problém, přeinstalujte prosím rozšíření. 
* Pokud zálohovaná hello virtuální počítač má statickou IP adresu, obnovení post, obnovený virtuální počítač bude mít dynamické tooavoid konflikt IP při vytváření obnovit virtuální počítač. Další informace o tom, jak můžete [přidat statickou IP toorestored virtuálních počítačů](../virtual-network/virtual-networks-reserved-private-ip.md#how-to-add-a-static-internal-ip-to-an-existing-vm)
* Nastavená hodnota dostupnosti nebude mít obnovený virtuální počítač. Doporučujeme použít možnost obnovení disků a [přidání sady dostupnosti](../virtual-machines/windows/tutorial-availability-sets.md) při vytvoření virtuálního počítače z prostředí PowerShell nebo pomocí šablony obnovit disky. 


## <a name="backup-for-restored-vms"></a>Zálohování pro obnovený virtuální počítače
Pokud jste obnovili virtuálních počítačů toosame skupinu prostředků s hello stejný název jako původně zálohovat virtuální počítač, zálohování bude pokračovat na hello post obnovení virtuálního počítače. Pokud máte obnovit virtuální počítač tooa jiné skupině prostředků, nebo zadat jiný název pro obnovený virtuální počítač, je zpracovaná jako nový virtuální počítač a potřebujete toosetup zálohování pro virtuální počítač obnovený.

## <a name="restoring-a-vm-during-azure-datacenter-disaster"></a>Obnovení virtuálního počítače během datové centrum Azure po havárii
Zálohování Azure umožňuje obnovení zálohovaných virtuální počítače toohello spárované datového centra v případě, že hello primární datového centra, kde běží virtuálních počítačů po havárii prostředí a jste nakonfigurovali toobe trezoru Backup geograficky redundantní. Během takových scénářů budete potřebovat tooselect účet úložiště, který je přítomen v spárované datového centra a zbytek procesu obnovení hello zůstává stejné. Azure Backup používá výpočetní služba z spárované geograficky toocreate hello obnovení virtuálního počítače. Další informace o [Azure Data center odolnost proti chybám](../resiliency/resiliency-technical-guidance-recovery-loss-azure-region.md)

## <a name="restoring-domain-controller-vms"></a>Obnovení virtuálních počítačů řadiče domény
Zálohování virtuálních počítačů řadiči domény (DC) je podporované scénáře s Azure Backup. Však musí dát pozor během procesu obnovení hello. proces obnovení správný Hello závisí na hello struktura domény hello. V nejjednodušším případě hello máte u jednoho řadiče domény v jedné doméně. Běžně pro produkční zatížení, budete mít na jednu doménu s více řadičů domény, případně s řadiče domény se místně. Nakonec můžete mít doménová struktura s více domén. 

Ze Active Directory perspektivy hello virtuální počítač Azure je jako další virtuální počítač na moderní podporovaném hypervisoru. Hello hlavní rozdíl s místními hypervisory je, že je v Azure k dispozici žádné konzoly virtuálního počítače. Konzola je nutný v některých případech, například obnovení pomocí typ zálohy úplného obnovení (BMR). Obnovení virtuálního počítače z úložiště záloh hello je však úplné nahrazení pro úplné obnovení systému. Active Directory obnovení režimu (DSRM) je také k dispozici, proto všechny scénáře obnovení služby Active Directory je přijatelná. Podrobnější informace, zkontrolujte [zálohování a obnovení aspekty virtualizovaných řadičů domény](https://technet.microsoft.com/en-us/library/virtual_active_directory_domain_controller_virtualization_hyperv(v=ws.10).aspx#backup_and_restore_considerations_for_virtualized_domain_controllers) a [plánování obnovení doménové struktury služby Active Directory](https://technet.microsoft.com/en-us/library/planning-active-directory-forest-recovery(v=ws.10).aspx).

### <a name="single-dc-in-a-single-domain"></a>Jednoho řadiče domény v jedné doméně
Hello virtuálních počítačů lze obnovit (např. žádné jiné virtuální počítače) z hello Azure portálu nebo pomocí prostředí PowerShell.

### <a name="multiple-dcs-in-a-single-domain"></a>Víc řadičů domény v jedné doméně
Pokud jiné řadiče domény ze stejné domény dostupný přes hello hello sítě, můžete je hello řadič domény obnovit jako žádné virtuální počítače. Pokud je poslední zbývající řadič domény v doméně hello hello nebo se provádí obnovení v izolované síti, musí být sledována procedury obnovení doménové struktury.

### <a name="multiple-domains-in-one-forest"></a>V jedné doménové struktuře víc domén
Pokud jiné řadiče domény ze stejné domény dostupný přes hello hello sítě, můžete je hello řadič domény obnovit jako žádné virtuální počítače. Však ve všech ostatních případech se doporučuje obnovení doménové struktury.

## <a name="restoring-vms-with-special-network-configurations"></a>Obnovení virtuálních počítačů s zvláštní síťové konfigurace
Je možné tooback zálohu a obnovení virtuálních počítačů s hello následující zvláštní síťové konfigurace. Tyto konfigurace však vyžadují některé zvláštní pozornost při procházení hello proces obnovení.

* Virtuální počítače v části Vyrovnávání zatížení (interních a externích)
* Virtuální počítače s víc vyhrazených IP adres
* Virtuální počítače s více síťovými kartami

> [!IMPORTANT]
> Při vytváření konfigurace hello speciální sítě pro virtuální počítače, je nutné použít PowerShell toocreate virtuální počítače z disků hello obnovit.
>
>

toofully vytvořte ho znovu hello virtuální počítače po obnovení toodisk, postupujte takto:

1. Obnovení z trezoru služeb zotavení pomocí disků hello [prostředí PowerShell](backup-azure-vms-automation.md#restore-an-azure-vm)
2. Vytvoření konfigurace virtuálního počítače hello požadované pro nástroj pro vyrovnávání zatížení nebo více Síťových nebo více vyhrazené IP pomocí hello rutiny prostředí PowerShell a použít ho toocreate hello virtuální počítač požadované konfigurace.

   * Vytvoření virtuálního počítače v rámci cloudové služby s [interní zátěže.](https://azure.microsoft.com/documentation/articles/load-balancer-internal-getstarted/)
   * Vytvoření virtuálního počítače tooconnect příliš[internetové nástroj pro vyrovnávání zatížení](https://azure.microsoft.com/en-us/documentation/articles/load-balancer-internet-getstarted/)
   * Vytvoření virtuálního počítače s [několik síťových adaptérů](https://azure.microsoft.com/documentation/articles/virtual-networks-multiple-nics/)
   * Vytvoření virtuálního počítače s [více vyhrazené IP adresy](https://azure.microsoft.com/documentation/articles/virtual-networks-reserved-public-ip/)

## <a name="next-steps"></a>Další kroky
Teď, když obnovíte virtuální počítače, najdete v části řešení potíží najdete v článku informace o běžných chyb s virtuálními počítači hello. Také podívejte se na článek hello na správu úloh pomocí vašich virtuálních počítačů.

* [Řešení potíží s chybami](backup-azure-vms-troubleshoot.md#restore)
* [Správa virtuálních počítačů](backup-azure-manage-vms.md)
