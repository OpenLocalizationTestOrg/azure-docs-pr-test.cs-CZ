---
title: "aaaRestore a virtuální počítače ze zálohy | Microsoft Docs"
description: "Zjistěte, jak toorestore virtuální počítač Azure z obnovení bodu"
services: backup
documentationcenter: 
author: trinadhk
manager: shreeshd
editor: 
keywords: "obnovení zálohy; jak toorestore; bod obnovení;"
ms.assetid: fed877b3-b496-49fb-99e4-653be7c23e3a
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: trinadhk; jimpark;
ms.openlocfilehash: 44c25f3248784accd1c2beaabb2c9a4dca3232d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="restore-virtual-machines-in-azure"></a>Obnovení virtuálních počítačů v Azure
> [!div class="op_single_selector"]
> * [Obnovení virtuálních počítačů na portálu Azure](backup-azure-arm-restore-vms.md)
> * [Obnovení virtuálních počítačů v klasickém portálu](backup-azure-restore-vms.md)
>
>

Obnovte tooa virtuálního počítače, které nový virtuální počítač z hello záloh uložených v trezoru zálohování Azure s hello následující kroky.

> [!IMPORTANT]
> Teď můžete upgradovat vaše trezory služeb tooRecovery trezory Backup. Podrobnosti najdete v tématu hello článku [upgradu tooa trezoru zálohování trezor služeb zotavení](backup-azure-upgrade-backup-to-recovery-services.md). Společnost Microsoft doporučuje tooupgrade zálohování trezory tooRecovery trezory služeb.<br/> **15. října 2017**, už nebude trezory Backup toocreate možné toouse prostředí PowerShell. <br/> **Od 1. listopadu 2017**:
>- Všechny zbývající trezory Backup bude automaticky upgradovaný tooRecovery trezory služeb.
>- Můžete nebudou moct tooaccess zálohovaných dat na portálu classic hello. Místo toho použijte hello Azure portálu tooaccess zálohovaných dat v trezory služeb zotavení.
>

## <a name="restore-workflow"></a>Obnovení pracovního postupu
### <a name="step-1-choose-an-item-toorestore"></a>Krok 1: Zvolte toorestore položky
1. Přejděte toohello **chráněné položky** kartě a vyberte hello virtuálního počítače, které chcete toorestore tooa nového virtuálního počítače.

    ![Chráněné položky](./media/backup-azure-restore-vms/protected-items.png)

    Hello **bod obnovení** sloupec v hello **chráněné položky** stránky se tak dozví hello počet bodů obnovení pro virtuální počítač. Hello **nejnovější bod obnovení** sloupec informuje hello čas poslední zálohy hello, ze kterého lze obnovit virtuální počítač.
2. Klikněte na tlačítko **obnovení** tooopen hello **obnovit položky** průvodce.

    ![Obnovit položky](./media/backup-azure-restore-vms/restore-item.png)

### <a name="step-2-pick-a-recovery-point"></a>Krok 2: Vyberte bod obnovení
1. V hello **vyberte bod obnovení** obrazovky, můžete obnovit z hello nejnovější bod obnovení, nebo z předchozího bodu v čase. Hello výchozí možnost vybrána, když se otevře se Průvodce je *nejnovější bod obnovení*.

    ![Vyberte bod obnovení](./media/backup-azure-restore-vms/select-recovery-point.png)
2. toopick dřívějšího bodu v čase, zvolte hello **vyberte datum** možnost hello rozevírací seznam a vyberte datum v kalendáři prvku hello kliknutím na hello **ikonu kalendáře**. V ovládacím prvku hello všechna data, které mají body obnovení jsou vyplněny světla šedé stín a jsou volitelný uživatelem hello.

    ![Vyberte datum](./media/backup-azure-restore-vms/select-date.png)

    Když kliknete na datum v kalendáři prvku hello, body obnovení hello k dispozici na že data budou zobrazena v následující tabulce body obnovení. Hello **čas** sloupec označuje hello dobu, na které hello pořízení snímku. Hello **typ** sloupec zobrazuje hello [konzistence](https://azure.microsoft.com/documentation/articles/backup-azure-vms/#consistency-of-recovery-points) hello bodu obnovení. záhlaví tabulky Hello znázorňuje hello počet bodů obnovení, které jsou k dispozici v daný den v závorkách.

    ![Body obnovení](./media/backup-azure-restore-vms/recovery-points.png)
3. Vyberte bod obnovení hello z hello **body obnovení** tabulky a klikněte na tlačítko hello další šipku toogo toohello další obrazovce.

### <a name="step-3-specify-a-destination-location"></a>Krok 3: Zadejte cílové umístění
1. V hello **výběr obnovení instance** obrazovky zadat podrobnosti o kde toorestore hello virtuálního počítače.

   * Zadejte název virtuálního počítače hello: V dané cloudové službě, název virtuálního počítače hello by měl být jedinečný. Nepodporujeme přepsání zápis existující virtuální počítač.
   * Vyberte cloudovou službu pro hello virtuálních počítačů: Toto je povinné pro vytvoření virtuálního počítače. Můžete vybrat tooeither použít stávající cloudovou službu nebo vytvořit novou cloudovou službu.

        Ať název cloudové služby je zachyceny by měly být globálně jedinečný. Obvykle získá související s veřejnou adresou URL hello tvar [cloudservice] hello název cloudové služby. cloudapp.net. Azure nedovolí toocreate novou cloudovou službu Pokud hello název se již používá. Pokud si zvolíte toocreate novou cloudovou službu, bude zadaný stejný název jako hello virtuální počítač – v které případu hello virtuálních počítačů hello název zachyceny by měl být dostatečně jedinečné toobe použít toohello související cloudové služby.

        Zobrazuje se pouze cloudové služby a virtuální sítě, které nejsou přiřazeny žádné skupiny vztahů v hello obnovení Podrobnosti instance. [Další informace](../virtual-network/virtual-networks-migrate-to-regional-vnet.md).
2. Vyberte účet úložiště pro virtuální počítač hello: Toto je povinné pro vytvoření hello virtuálních počítačů. Můžete vybrat z existující účty úložiště ve hello stejné oblasti jako trezor zálohování Azure hello. Účty úložiště, které jsou zóny redundantní nebo typ úložiště Premium se nepodporuje.

    Pokud nejsou žádné účty úložiště s podporovanou konfiguraci, vytvořte účet úložiště operace obnovení předchozí toostarting podporovanou konfiguraci.

    ![Vyberte virtuální síť.](./media/backup-azure-restore-vms/restore-sa.png)
3. Vyberte virtuální síť: hello virtuální síť (VNET) pro hello virtuální počítač, měl by být vybrán v době vytváření hello hello virtuálních počítačů. Hello obnovit uživatelské rozhraní zobrazí všechny hello virtuální sítě v rámci tohoto předplatného, který lze použít. Není to povinné tooselect virtuální sítě pro virtuální počítač obnovit hello – bude možné tooconnect toohello obnovit virtuální počítač přes hello internet, i když hello virtuální sítě není použita.

    Pokud hello cloudové služby vybrali je přidružený k virtuální síti, pak virtuální sítě hello nelze změnit.

    ![Vyberte virtuální síť.](./media/backup-azure-restore-vms/restore-cs-vnet.png)
4. Vyberte podsíť: V případě, že hello virtuální sítě má podsítě, ve výchozím nastavení bude vybrána první podsíť hello. Zvolte podsíť hello podle svého výběru z rozevíracího seznamu možností hello. Podrobnosti podsíť najdete tooNetworks rozšíření v hello [domovské stránky portálu](https://manage.windowsazure.com/), přejděte příliš**virtuální sítě** a vyberte hello virtuální sítě a rozbalení do podrobnosti o podsíti toosee konfigurace.

    ![Vyberte podsíť.](./media/backup-azure-restore-vms/select-subnet.png)
5. Klikněte na tlačítko hello **odeslání** ikonu v hello Průvodce toosubmit hello podrobnosti a vytvořit úlohu obnovení.

## <a name="track-hello-restore-operation"></a>Sledovat operace obnovení hello
Po vstupu všechny informace o hello do Průvodce obnovením hello a odeslat ho Azure Backup se pokusí toocreate operace obnovení úlohy tootrack hello.

![Vytvoření úlohy obnovení](./media/backup-azure-restore-vms/create-restore-job.png)

Pokud hello úlohy vytvoření úspěšné, zobrazí se informační zprávy oznámení s informací, že se vytvoří tuto úlohu hello. Další informace získáte kliknutím na hello **zobrazit úlohy** tlačítko, které vás zavedou příliš**úlohy** kartě.

![Úloha obnovení byla vytvořena](./media/backup-azure-restore-vms/restore-job-created.png)

Po dokončení operace obnovení hello bude označena jako dokončená v **úlohy** kartě.

![Dokončení úlohy obnovení](./media/backup-azure-restore-vms/restore-job-complete.png)

Po obnovení virtuálního počítače může být nutné toore instalace hello rozšíření stávajících hello hello původní virtuální počítač a [upravit koncové body hello](../virtual-machines/windows/classic/setup-endpoints.md) pro virtuální počítač hello v hello portálu Azure.

## <a name="post-restore-steps"></a>Po obnovení kroky
Pokud použijete distribuci systému Linux na základě inicializací cloudu, jako například Ubuntu, z bezpečnostních důvodů, bude blokován heslo post obnovení. Prosím používá rozšíření VMAccess na hello obnovit virtuální počítač příliš[resetovat heslo hello](../virtual-machines/linux/classic/reset-access.md). Doporučujeme používat klíče SSH na tyto distribuce tooavoid resetování hesla post obnovení.

## <a name="backup-for-restored-vms"></a>Zálohování pro obnovený virtuální počítače
Pokud jste obnovili virtuálních počítačů toosame Cloudová služba se hello stejný název jako původně zálohovat virtuální počítač, zálohování bude pokračovat na hello post obnovení virtuálního počítače. Pokud jste se obnovit virtuální počítač tooa jinou cloudovou službu nebo zadat jiný název pro virtuální počítač obnovený, to bude považována za nového virtuálního počítače a je třeba toosetup zálohování pro virtuální počítač obnovený.

## <a name="restoring-a-vm-during-azure-datacenter-disaster"></a>Obnovení virtuálního počítače během Azure DataCenter po havárii
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

<!--- WK: this following original supportability statement is incorrect, taking it out.
hello challenge arises because DSRM mode is not present in Azure. So toorestore such a VM, you cannot use hello Azure portal. hello only supported restore mechanism is disk-based restore using PowerShell.

> [!WARNING]
> For Domain Controller VMs in a multi-DC environment, do not use hello Azure portal for restore! Only PowerShell based restore is supported
>
>

Read more about hello [USN rollback problem](https://technet.microsoft.com/library/dd363553) and hello strategies suggested toofix it.
--->

## <a name="restoring-vms-with-special-network-configurations"></a>Obnovení virtuálních počítačů s zvláštní síťové konfigurace
Azure Backup podporuje zálohování pro následující konfigurace speciální sítě virtuálních počítačů.

* Virtuální počítače v části Vyrovnávání zatížení (interních a externích)
* Virtuální počítače s víc vyhrazených IP adres
* Virtuální počítače s více síťovými kartami

Tyto konfigurace vyžádá následující aspekty při jejich obnovení.

> [!TIP]
> Použijte prosím PowerShell na základě obnovení toku toorecreate hello speciální konfigurace sítě virtuálních počítačů post obnovení.
>
>

### <a name="restoring-from-hello-ui"></a>Obnovení ze hello uživatelského rozhraní:
Při obnovení z uživatelského rozhraní, **vždy vyberte novou cloudovou službu**. Upozorňujeme, že vzhledem k tomu, že portál trvá pouze povinné parametry během obnovení tok, virtuální počítače obnovit pomocí uživatelského rozhraní ztratí hello zvláštní síťové konfigurace, které mohou mít. Jinými slovy, obnovení virtuálních počítačů bude normální virtuálních počítačů bez konfigurace pro vyrovnávání zatížení nebo více síťového adaptéru nebo více vyhrazenou IP adresu.

### <a name="restoring-from-powershell"></a>Obnovení z prostředí PowerShell:
Prostředí PowerShell má hello možnost toojust obnovit ze zálohy hello disky virtuálních počítačů a vytvořit hello virtuálního počítače. To je užitečné, když obnovení virtuálních počítačů, které vyžadují zvláštní síťové konfigurace uvedených výše.

V pořadí toofully znovu post virtuálního počítače hello obnovování disků, postupujte takto:

1. Obnovení hello disky pomocí úložiště záloh [zálohování Azure PowerShell](backup-azure-vms-classic-automation.md#restore-an-azure-vm)
2. Vytvoření hello konfigurace požadované pro nástroj pro vyrovnávání zatížení nebo více Síťových nebo více vyhrazené IP pomocí hello rutiny prostředí PowerShell a použít ho toocreate hello virtuální počítač požadované konfigurace.

   * Vytvoření virtuálního počítače v rámci cloudové služby s [interní zátěže.](https://azure.microsoft.com/documentation/articles/load-balancer-internal-getstarted/)
   * Vytvoření virtuálního počítače tooconnect příliš[internetové nástroj pro vyrovnávání zatížení](https://azure.microsoft.com/en-us/documentation/articles/load-balancer-internet-getstarted/)
   * Vytvoření virtuálního počítače s [několik síťových adaptérů](https://azure.microsoft.com/documentation/articles/virtual-networks-multiple-nics/)
   * Vytvoření virtuálního počítače s [více vyhrazené IP adresy](https://azure.microsoft.com/documentation/articles/virtual-networks-reserved-public-ip/)

## <a name="next-steps"></a>Další kroky
* [Řešení potíží s chybami](backup-azure-vms-troubleshoot.md#restore)
* [Správa virtuálních počítačů](backup-azure-manage-vms.md)
