---
title: "Zálohování Azure: Obnovení virtuálních počítačů pomocí portálu Azure | Microsoft Docs"
description: "Obnovení virtuálního počítače Azure z bodu obnovení pomocí portálu Azure"
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: "obnovení zálohy; Postup obnovení; bod obnovení;"
ms.assetid: 372b87c6-3544-4dc5-bbc9-c742ca502159
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/15/2017
ms.author: markgal;trinadhk;
ms.openlocfilehash: e1fe2b94d462a30f09cb23ab905542aa121ba46b
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="use-azure-portal-to-restore-virtual-machines"></a>Použití webu Azure Portal k obnovení virtuálních počítačů
> [!div class="op_single_selector"]
> * [Obnovení virtuálních počítačů v klasickém portálu](backup-azure-restore-vms.md)
> * [Obnovení virtuálních počítačů na portálu Azure](backup-azure-arm-restore-vms.md)
>
>

Ochrana dat pomocí snímky dat na definovaných intervalech. Tyto snímky jsou známé jako body obnovení a jsou uložené v trezory služeb zotavení. Pokud nebo je-li nezbytné opravit nebo znovu vytvořit virtuální počítač, můžete obnovit virtuální počítač z jakéhokoli z bodů obnovení uložené. Při obnovování bod obnovení, můžete vytvořit nový virtuální počítač, který je reprezentace v okamžiku zálohy virtuálního počítače, nebo obnovení disků a použít šablonu, která se dodává spolu s jeho přizpůsobení obnovený virtuální počítač nebo provést obnovení jednotlivých souborů. Tento článek vysvětluje, jak obnovit virtuální počítač na nový virtuální počítač nebo obnovit všechny disky zálohy. Obnovení jednotlivých souborů, najdete v části [obnovit soubory ze zálohy virtuálního počítače Azure](backup-azure-restore-files-from-vm.md)

![3-Ways-Restore-from-VM-Backup](./media/backup-azure-arm-restore-vms/azure-vm-backup-restore.png)

> [!NOTE]
> Azure má dva modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../azure-resource-manager/resource-manager-deployment-model.md). Tento článek obsahuje informace a postupy pro obnovení virtuální počítače nasazené pomocí modelu Resource Manager.
>
>

Obnovení virtuálního počítače nebo všechny disky z virtuálního počítače zálohování zahrnuje dva kroky:

1. Vyberte bod obnovení pro obnovení
2. Výběr obnovení zadejte – vytvoření nového virtuálního počítače nebo obnovení disků a zadejte požadované parametry. 

## <a name="select-restore-point-for-restore"></a>Vyberte bod obnovení pro obnovení
1. Přihlaste se k [portálu Azure](http://portal.azure.com/)
2. V nabídce Azure, klikněte na tlačítko **Procházet** a v seznamu služeb, zadejte **služeb zotavení**. Seznam služeb přizpůsobí co zadáte. Až se zobrazí **trezory služeb zotavení**, vyberte ho.

    ![Otevřený trezor služeb zotavení](./media/backup-azure-arm-restore-vms/open-recovery-services-vault.png)

    Zobrazí se seznam trezorů v rámci předplatného.

    ![Trezory seznam služeb zotavení](./media/backup-azure-arm-restore-vms/list-of-rs-vaults.png)
3. V seznamu vyberte trezor přidružený virtuálních počítačů, které chcete obnovit. Když kliknete na trezor, otevře se jeho řídicí panel.

    ![Trezory seznam služeb zotavení](./media/backup-azure-arm-restore-vms/select-vault-open-vault-blade.png)
4. Teď, když jste v řídícím panelu trezoru. Na **zálohování položek** dlaždici, klikněte na tlačítko **virtuální počítače Azure** zobrazíte virtuální počítače přidružené k trezoru.

    ![panelu trezoru](./media/backup-azure-arm-restore-vms/vault-dashboard.png)

    **Zálohování položek** okno otevře a zobrazí seznam virtuálních počítačů Azure.

    ![Seznam virtuálních počítačů v trezoru](./media/backup-azure-arm-restore-vms/list-of-vms-in-vault.png)
5. V seznamu vyberte virtuální počítač otevřete řídicí panel. Otevře se řídicí panel virtuálních počítačů do oblasti monitorování, který obsahuje dlaždici body obnovení.

    ![Seznam virtuálních počítačů v trezoru](./media/backup-azure-arm-restore-vms/vm-blade.png)
6. V nabídce řídícího panelu virtuálních počítačů, klikněte na tlačítko **obnovení**

    ![Seznam virtuálních počítačů v trezoru](./media/backup-azure-arm-restore-vms/vm-blade-menu-restore.png)

    Otevře se okno obnovení.

    ![okno obnovení](./media/backup-azure-arm-restore-vms/restore-blade.png)
7. Na **obnovení** okně klikněte na tlačítko **bod obnovení** otevřete **obnovení vyberte bod** okno.

    ![okno obnovení](./media/backup-azure-arm-restore-vms/recovery-point-selector.png)

    Ve výchozím nastavení dialogové okno zobrazí všechny body obnovení z posledních 30 dnů. Použití **filtru** změnit časový rozsah bodů obnovení zobrazí. Ve výchozím nastavení zobrazí se body obnovení všech konzistence. Upravit **obnovit všechny body** filtru vybrat konkrétní konzistence bodů obnovení. Další informace o jednotlivých typech bodu obnovení najdete v tématu Vysvětlení [konzistenci dat](backup-azure-vms-introduction.md#data-consistency).  

   * **Obnovení bodu konzistence** z tohoto seznamu vyberte:
     * Body obnovení konzistentní havárií
     * Body obnovení konzistentní aplikací,
     * Body obnovení konzistentní se systémem souborů
     * Všechny body obnovení.  
8. Zvolte bod obnovení a klikněte na tlačítko **OK**.

    ![Vyberte bod obnovení](./media/backup-azure-arm-restore-vms/select-recovery-point.png)

    **Obnovení** se zobrazí okno bodu obnovení je nastaven.

    ![bod obnovení je nastaven.](./media/backup-azure-arm-restore-vms/recovery-point-set.png)
9. Na **obnovení** okně **obnovit konfiguraci** automaticky otevře po nastavení bodu obnovení.

## <a name="choosing-a-vm-restore-configuration"></a>Výběr obnovení konfigurace virtuálního počítače
Teď, když jste vybrali bodu obnovení, vyberte takovou konfiguraci pro obnovení virtuálního počítače. Vaše možnosti pro konfiguraci obnovený virtuální počítač se mají použít: portál Azure nebo PowerShell.

1. Pokud nejsou již existuje, přejdete do **obnovení** okno. Zkontrolujte [bod obnovení byl vybrán](#select-restore-point-for-restore)a klikněte na tlačítko **obnovit konfiguraci** otevřete **konfigurace obnovení** okno.

    ![Průvodce obnovením konfigurace je nastavena.](./media/backup-azure-arm-restore-vms/recovery-configuration-wizard-recovery-type.png)
2. Na **obnovit konfiguraci** okno, máte dvě možnosti:
   * Úplná virtuální počítač obnovit
   * Obnovení zálohovaných disky

Portál nabízí možnost vytvořit pro obnovený virtuální počítač. Pokud chcete přizpůsobit konfigurace virtuálního počítače nebo názvy prostředků, které jsou vytvořené jako součást vytvořit novou volbu virtuálních počítačů, použijte prostředí PowerShell nebo portálu k obnovení zálohovaných disky a připojte je k výběru konfigurace virtuálního počítače nebo použít šablonu, která se dodává s disky obnovení k přizpůsobení obnovený virtuální počítač použít příkazy prostředí PowerShell. V tématu [obnovení virtuálního počítače s konfigurací speciální síťových](#restoring-vms-with-special-network-configurations) podrobnosti o tom, jak obnovit virtuální počítač, který má několik síťových adaptérů nebo v části Vyrovnávání zatížení. Pokud vaše virtuální počítač Windows používá [ROZBOČOVAČE licencování](../virtual-machines/windows/hybrid-use-benefit-licensing.md), budete muset obnovit disků a použít PowerShell nebo uvedeného níže uvedená šablona k vytvoření virtuálního počítače a ujistěte se, že zadáváte LicenseType jako "Windows_Server" při vytváření virtuálního počítače využívat výhody centra obnoveným virtuálním Počítačem. 
 
## <a name="create-a-new-vm-from-restore-point"></a>Vytvoření nového virtuálního počítače z bodu obnovení
Pokud si nejste již existuje, [vyberte bod obnovení](#restoring-vms-with-special-network-configurations) než budete pokračovat ve vytváření nového virtuálního počítače z bodu obnovení. Když je vybraný bod obnovení, na **obnovit konfiguraci** okno, zadejte nebo vyberte hodnoty pro každý z těchto polí:

* **Obnovení typ** – vytvoření virtuálního počítače.
* **Název virtuálního počítače** – zadejte název pro virtuální počítač. Název musí být jedinečný do skupiny prostředků (pro virtuálních počítačů nasazených Resource Managerem) nebo cloudové služby (pro klasické virtuální počítač). Virtuální počítač nelze nahradit, pokud již existuje v rámci předplatného.
* **Skupina prostředků** – použít existující skupinu prostředků nebo vytvořte novou. Pokud obnovujete klasické virtuální počítač, použijte toto pole k zadání názvu novou cloudovou službu. Pokud vytváříte novou skupinu nebo cloudovou službu prostředku, musí být globálně jedinečný název. Obvykle je název cloudové služby související s veřejnou adresou URL, -, například: [cloudservice]. cloudapp.net. Pokud se pokusíte použít název skupiny nebo cloudové služby prostředků cloudu, která již byl použit, Azure přiřadí prostředků skupiny nebo cloudové služby má stejný název jako virtuální počítač. Azure zobrazí prostředků skupiny nebo cloudové služby a virtuální počítače nejsou přiřazeny žádné skupiny vztahů. Další informace najdete v tématu [jak migrovat z skupiny vztahů regionální virtuální síť (VNet)](../virtual-network/virtual-networks-migrate-to-regional-vnet.md).
* **Virtuální síť** -vyberte virtuální síť (VNET), při vytváření virtuálního počítače. Pole poskytuje všechny virtuální sítě přidružený k odběru. Skupina prostředků virtuálního počítače se zobrazí v závorkách.
* **Podsíť** – Pokud virtuální sítě má podsítě, ve výchozím nastavení je vybrán první podsíť. Pokud existují další podsítě, vyberte požadované podsítě.
* **Účet úložiště** -této nabídky jsou uvedené účty úložiště ve stejném umístění jako trezor služeb zotavení. Účty úložiště, které jsou Zónově redundantní nejsou podporovány. Pokud nejsou žádné účty úložiště s stejné umístění jako trezor služeb zotavení, je nutné ho vytvořit před zahájením operace obnovení. Typ replikace účtu úložiště je uveden v závorkách.

![Průvodce konfigurací obnovení je nastaven.](./media/backup-azure-arm-restore-vms/recovery-configuration-wizard.png)

> [!NOTE]
> 1. Pokud obnovujete virtuálních počítačů nasazených Resource Managerem, musíte určit virtuální síť (VNET). Virtuální síť (VNET) je pro klasické virtuální počítač volitelné.
> 2. Pokud obnovujete virtuální počítače s spravované disky, ujistěte se, že vybraný účet úložiště není povolen pro Encryption(SSE) služby úložiště v celé jeho životnosti.
> 3. Na základě úložiště typu účtu úložiště vybrané (standard nebo premium), všechny disky, obnovení bude premium nebo standardní disky. Aktuálně nepodporujeme smíšený režim disků při obnovení.  
>
>

Na **obnovit konfiguraci** okně klikněte na tlačítko **OK** pro dokončení konfigurace obnovení. Na **obnovení** okně klikněte na tlačítko **obnovení** pro spuštění operace obnovení.

## <a name="restore-backed-up-disks"></a>Obnovení zálohovaných disky
Pokud chcete přizpůsobit virtuálního počítače chcete vytvořit z zálohovat discích, než co se nachází v okně Konfigurace obnovení, vyberte **obnovit disky** jako hodnota **obnovit typu**. Tato volba požádá o účet úložiště, které jsou zkopírovány disky ze zálohy. Při výběru účtu úložiště, vyberte účet, který sdílí stejné umístění jako trezor služeb zotavení. Účty úložiště, které jsou Zónově redundantní nejsou podporovány. Pokud nejsou žádné účty úložiště s stejné umístění jako trezor služeb zotavení, je nutné ho vytvořit před zahájením operace obnovení. Typ replikace účtu úložiště je uveden v závorkách.

Po dokončení operace obnovení můžete provést tyto akce:
* [Pomocí šablony můžete přizpůsobit obnoveným virtuálním Počítačem.](#use-templates-to-customize-restore-vm)
* [Použijte pro připojení k existující virtuální počítač obnovený disky](../virtual-machines/windows/attach-managed-disk-portal.md)
* [Vytvoření nového virtuálního počítače z obnovené disky pomocí prostředí PowerShell.](./backup-azure-vms-automation.md#restore-an-azure-vm)

Na **obnovit konfiguraci** okně klikněte na tlačítko **OK** pro dokončení konfigurace obnovení. Na **obnovení** okně klikněte na tlačítko **obnovení** pro spuštění operace obnovení.

![Konfigurace obnovení byla dokončena](./media/backup-azure-arm-restore-vms/trigger-restore-operation.png)

## <a name="track-the-restore-operation"></a>Sledování operací obnovení
Jakmile spustíte operaci obnovení, služba Backup vytvoří úlohu pro sledování operaci obnovení. Služba zálohování vytvoří také a dočasně zobrazí oznámení v oblasti oznámení portálu. Pokud se nezobrazí oznámení, vždy kliknutím na ikonu oznámení a zobrazit oznámení.

![Spustí obnovení](./media/backup-azure-arm-restore-vms/restore-notification.png)

Chcete-li zobrazit operace bude během zpracování nebo pokud chcete zjistit, kdy byla dokončena, otevřete seznam úloh zálohování.

1. V nabídce Azure, klikněte na tlačítko **Procházet** a v seznamu služeb, zadejte **služeb zotavení**. Seznam služeb přizpůsobí co zadáte. Až se zobrazí **trezory služeb zotavení**, vyberte ho.

    ![Otevřený trezor služeb zotavení](./media/backup-azure-arm-restore-vms/open-recovery-services-vault.png)

    Zobrazí se seznam trezorů v rámci předplatného.

    ![Trezory seznam služeb zotavení](./media/backup-azure-arm-restore-vms/list-of-rs-vaults.png)
2. V seznamu vyberte trezor spojené s virtuálním Počítačem můžete obnovit. Když kliknete na trezor, otevře se jeho řídicí panel.
3. V řídícím panelu trezoru na **úlohy zálohování** dlaždici, klikněte na tlačítko **virtuální počítače Azure** zobrazí úlohy přidruženého k úložišti.

    ![panelu trezoru](./media/backup-azure-arm-restore-vms/vault-dashboard-jobs.png)

    **Úlohy zálohování** okno otevře a zobrazí se seznam úloh.

    ![Seznam virtuálních počítačů v trezoru](./media/backup-azure-arm-restore-vms/restore-job-in-progress.png)
    
## <a name="use-templates-to-customize-restore-vm"></a>Pomocí šablony lze přizpůsobit obnovení virtuálního počítače
Jednou [dokončení operace obnovení disky](#Track-the-restore-operation), můžete použít šablonu, která je generována jako součást operace obnovení k vytvoření nového virtuálního počítače s konfigurací liší od konfigurace zálohy nebo přizpůsobit názvy prostředků, vytvořit, protože vytvoření nového virtuálního počítače z bodu obnovení. 

> [!NOTE]
> Šablony budou přidáni jako součást obnovení disky pro body obnovení po 1. března 2017 provedeny. Jsou použitelné pro disk bez šifrování a nespravované virtuální počítače. Podpora pro šifrované virtuální počítače a spravovat virtuální počítače disku se připravuje a v budoucích verzích. 
>
>

Chcete-li získat generované v rámci možnost obnovení disky, šablony

1. Přejděte k obnovení úlohy podrobnosti odpovídající úlohy. 
2. Na obrazovce podrobnosti úlohy obnovení, klikněte na *nasazení šablony* tlačítko zahájíte nasazení šablony. 

     ![obnovit úlohu procházení](./media/backup-azure-arm-restore-vms/restore-job-drill-down.png)
   
V okně Šablona nasazení pro vlastní nasazení pomocí nasazení šablony do [upravit a nasadit šablonu](../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template) nebo připojit další přizpůsobení podle [šablonu](../azure-resource-manager/resource-group-authoring-templates.md) před nasazením. 

   ![načítání nasazení šablony](./media/backup-azure-arm-restore-vms/loading-template.png)
   
Po zadání požadované hodnoty, přijměte *podmínky a ujednání* a klikněte na **nákupu**.

   ![odesílá se nasazení šablony](./media/backup-azure-arm-restore-vms/submitting-template.png)

## <a name="post-restore-steps"></a>Po obnovení kroky
* Pokud použijete distribuci systému Linux na základě inicializací cloudu, jako například Ubuntu, z bezpečnostních důvodů, hesla zablokuje post obnovení. Na obnovený virtuální počítač použijte rozšíření VMAccess [resetování hesla](../virtual-machines/linux/classic/reset-access.md). Doporučujeme, abyste pomocí klíče SSH na tyto distribuce, aby se zabránilo resetování hesla post obnovení.
* Při zálohování konfigurace rozšíření bude nainstalována, ale nebude povolen. Pokud zjistíte, jakýkoli problém, přeinstalujte prosím rozšíření. 
* Pokud zálohované virtuální počítač má statickou IP adresu, obnovení post obnovený virtuální počítač bude mít dynamických IP, abyste se vyhnuli konflikt při vytváření obnovit virtuální počítač. Další informace o tom, jak můžete [přidat statickou IP adresu do obnoveným virtuálním Počítačem.](../virtual-network/virtual-networks-reserved-private-ip.md#how-to-add-a-static-internal-ip-to-an-existing-vm)
* Nastavená hodnota dostupnosti nebude mít obnovený virtuální počítač. Doporučujeme použít možnost obnovení disků a [přidání sady dostupnosti](../virtual-machines/windows/tutorial-availability-sets.md) při vytvoření virtuálního počítače z prostředí PowerShell nebo pomocí šablony obnovit disky. 


## <a name="backup-for-restored-vms"></a>Zálohování pro obnovený virtuální počítače
Pokud jste obnovili virtuálních počítačů do stejné skupiny prostředků se stejným názvem jako původně zálohovat virtuální počítač, zálohování bude pokračovat na post obnovení virtuálního počítače. Pokud máte virtuální počítač obnovit do jiné skupiny prostředků, nebo zadat jiný název pro obnovený virtuální počítač, je zpracovaná jako nový virtuální počítač a potřebujete instalační program Zálohování pro virtuální počítač obnovený.

## <a name="restoring-a-vm-during-azure-datacenter-disaster"></a>Obnovení virtuálního počítače během datové centrum Azure po havárii
Zálohování Azure umožňuje obnovení zálohovat virtuální počítače do spárované datového centra v případě, že primární datového centra, kde běží virtuálních počítačů po havárii prostředí a jste nakonfigurovali úložiště záloh být geograficky redundantní. Během takových scénářů je nutné vybrat účet úložiště, který je přítomen v spárované datového centra a zbytek procesu obnovení zůstává stejné. Azure Backup používá výpočetní služby z spárované geograficky vytvořit obnovený virtuální počítač. Další informace o [Azure Data center odolnost proti chybám](../resiliency/resiliency-technical-guidance-recovery-loss-azure-region.md)

## <a name="restoring-domain-controller-vms"></a>Obnovení virtuálních počítačů řadiče domény
Zálohování virtuálních počítačů řadiči domény (DC) je podporované scénáře s Azure Backup. Však musí dát pozor během procesu obnovení. Proces obnovení správný závisí na strukturu domény. V nejjednodušším případě máte u jednoho řadiče domény v jedné doméně. Běžně pro produkční zatížení, budete mít na jednu doménu s více řadičů domény, případně s řadiče domény se místně. Nakonec můžete mít doménová struktura s více domén. 

Z hlediska Active Directory je virtuální počítač Azure jako další virtuální počítač na moderní podporovaném hypervisoru. Hlavní rozdíl s místními hypervisory je, že je v Azure k dispozici žádné konzoly virtuálního počítače. Konzola je nutný v některých případech, například obnovení pomocí typ zálohy úplného obnovení (BMR). Obnovení virtuálního počítače z trezoru záloh je však úplné nahrazení pro úplné obnovení systému. Active Directory obnovení režimu (DSRM) je také k dispozici, proto všechny scénáře obnovení služby Active Directory je přijatelná. Podrobnější informace, zkontrolujte [zálohování a obnovení aspekty virtualizovaných řadičů domény](https://technet.microsoft.com/en-us/library/virtual_active_directory_domain_controller_virtualization_hyperv(v=ws.10).aspx#backup_and_restore_considerations_for_virtualized_domain_controllers) a [plánování obnovení doménové struktury služby Active Directory](https://technet.microsoft.com/en-us/library/planning-active-directory-forest-recovery(v=ws.10).aspx).

### <a name="single-dc-in-a-single-domain"></a>Jednoho řadiče domény v jedné doméně
Virtuální počítač může být obnovena (např. žádné jiné virtuální počítače) z Azure portálu nebo pomocí prostředí PowerShell.

### <a name="multiple-dcs-in-a-single-domain"></a>Víc řadičů domény v jedné doméně
Pokud jiné řadiče domény do stejné domény dostupný přes síť, můžete je obnovit řadič domény jako žádné virtuální počítače. Pokud je poslední zbývající řadič domény v doméně, nebo se provádí obnovení v izolované síti, musí být sledována procedury obnovení doménové struktury.

### <a name="multiple-domains-in-one-forest"></a>V jedné doménové struktuře víc domén
Pokud jiné řadiče domény do stejné domény dostupný přes síť, můžete je obnovit řadič domény jako žádné virtuální počítače. Však ve všech ostatních případech se doporučuje obnovení doménové struktury.

## <a name="restoring-vms-with-special-network-configurations"></a>Obnovení virtuálních počítačů s zvláštní síťové konfigurace
Je možné k zálohování a obnovení virtuálních počítačů s následující zvláštní síťové konfigurace. Tyto konfigurace však vyžadují některé zvláštní pozornost při procházení proces obnovení.

* Virtuální počítače v části Vyrovnávání zatížení (interních a externích)
* Virtuální počítače s víc vyhrazených IP adres
* Virtuální počítače s více síťovými kartami

> [!IMPORTANT]
> Při vytváření konfigurace speciální sítě pro virtuální počítače, musíte použít PowerShell k vytvoření virtuálních počítačů z disků obnovit.
>
>

Chcete-li plně znovu virtuální počítače po obnovení na disk, postupujte takto:

1. Obnovení z trezoru služeb zotavení pomocí disky [prostředí PowerShell](backup-azure-vms-automation.md#restore-an-azure-vm)
2. Vytvoření požadované konfigurace pro nástroj pro vyrovnávání zatížení virtuálního počítače nebo více Síťových nebo více vyhrazenou IP adresu pomocí rutiny prostředí PowerShell a použít je pro vytvoření virtuálního počítače z žádoucí konfigurace.

   * Vytvoření virtuálního počítače v rámci cloudové služby s [interní zátěže.](https://azure.microsoft.com/documentation/articles/load-balancer-internal-getstarted/)
   * Vytvoření virtuálního počítače pro připojení k [internetové nástroj pro vyrovnávání zatížení](https://azure.microsoft.com/en-us/documentation/articles/load-balancer-internet-getstarted/)
   * Vytvoření virtuálního počítače s [několik síťových adaptérů](https://azure.microsoft.com/documentation/articles/virtual-networks-multiple-nics/)
   * Vytvoření virtuálního počítače s [více vyhrazené IP adresy](https://azure.microsoft.com/documentation/articles/virtual-networks-reserved-public-ip/)

## <a name="next-steps"></a>Další kroky
Teď, když obnovíte virtuální počítače, najdete v článku Poradce při potížích informace na běžné chyby s virtuálními počítači. Navíc najdete v článku na správu úloh pomocí vašich virtuálních počítačů.

* [Řešení potíží s chybami](backup-azure-vms-troubleshoot.md#restore)
* [Správa virtuálních počítačů](backup-azure-manage-vms.md)
