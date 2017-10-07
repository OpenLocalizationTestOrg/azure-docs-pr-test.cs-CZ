---
title: "aaaBack až servery VMware s serveru Azure Backup | Microsoft Docs"
description: "Pomocí serveru Azure Backup tooback VMware vCenter/ESXi servery tooAzure nebo disku. Tento článek obsahuje krok za krokem instrukce pro zálohování (nebo ochrany) = úlohy VMware."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
ms.assetid: 6b131caf-de85-4eba-b8e6-d8a04545cd9d
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/24/2017
ms.author: markgal;
ms.openlocfilehash: 3edb6880a526ed0b18605fee0fac27196a608e7a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-a-vmware-server-tooazure"></a>Zálohování server tooAzure VMware

Tento článek vysvětluje, jak tooconfigure serveru Azure Backup toohelp chránit úlohy serveru VMware. Tento článek předpokládá, že je již nainstalován Server Azure Backup. Pokud nemáte nainstalované serveru Azure Backup, přečtěte si téma [Příprava tooback až úlohy pomocí serveru Azure Backup](backup-azure-microsoft-azure-backup.md).

Azure Backup Server můžete zálohovat nebo pomoc při ochraně VMware vCenter Server verze 6.5, 6.0 a 5.5.


## <a name="create-a-secure-connection-toohello-vcenter-server"></a>Vytvoření zabezpečeného spojení toohello vCenter Server

Ve výchozím serveru Azure Backup komunikuje se každý Server vCenter přes kanál protokolu HTTPS. tooturn na hello zabezpečenou komunikaci, doporučujeme nainstalovat certifikát hello VMware certifikační autoritou (CA) na serveru Azure Backup. Pokud není zapotřebí zabezpečenou komunikaci a přejete požadavek HTTPS hello toodisable, najdete v části [zakázat zabezpečený komunikační protokol](backup-azure-backup-server-vmware.md#disable-secure-communication-protocol). toocreate zabezpečené připojení mezi serverem pro zálohování Azure a hello systému vCenter Server, importujte certifikát důvěryhodné hello na serveru Azure Backup.

V prohlížeči se obvykle používá na hello serveru Azure Backup počítač tooconnect toohello vCenter Server prostřednictvím hello vSphere webového klienta. Hello prvním použití hello serveru Azure Backup prohlížeče tooconnect toohello vCenter Server, hello připojení není zabezpečení. Hello následující obrázek znázorňuje hello nezabezpečené připojení.

![Příklad serveru tooVMware nezabezpečené připojení](./media/backup-azure-backup-server-vmware/unsecure-url.png)

toofix-li tento problém a vytvoří zabezpečené připojení, stáhněte si hello důvěryhodné kořenové Certifikační autority.

1. V prohlížeči hello na serveru Azure Backup zadejte hello URL toohello vSphere webového klienta. Zobrazí se Hello vSphere webovému klientovi přihlašovací stránku.

    ![vSphere webového klienta](./media/backup-azure-backup-server-vmware/vsphere-web-client.png)

    V dolní části hello hello informací pro správce a vývojáře, vyhledejte hello **stažení důvěryhodné kořenové Certifikační autority** odkaz.

    ![Odkaz toodownload hello důvěryhodné kořenové Certifikační autority](./media/backup-azure-backup-server-vmware/vmware-download-ca-cert-prompt.png)

  Pokud nevidíte hello vSphere webovému klientovi přihlašovací stránky, zkontrolujte nastavení proxy serveru v prohlížeči.

2. Klikněte na tlačítko **stažení důvěryhodné kořenové Certifikační autority**.

    Hello systému vCenter Server stáhne soubor tooyour místní počítač. Hello název souboru je s názvem **Stáhnout**. V závislosti na prohlížeči, obdržíte zprávu s dotazem, zda tooopen nebo uložení souboru hello.

    ![stáhnout zprávy, když se stáhnou certifikáty](./media/backup-azure-backup-server-vmware/download-certs.png)

3. Hello souboru tooa umístění pro uložení na Azure Backup Server. Při ukládání hello souboru přidáte příponu názvu souboru .zip hello.

    Hello soubor je soubor .zip, který obsahuje hello informace o certifikátech hello. S příponou .zip hello můžete použít nástroje pro extrakci hello.

4. Klikněte pravým tlačítkem na **download.zip**a potom vyberte **Extrahovat vše** tooextract hello obsah.

    soubor ZIP Hello extrahuje jeho obsah tooa složku s názvem **certifikátů**. Dva typy souborů se zobrazí ve složce hello certifikátů. soubor kořenového certifikátu Hello má příponu, která začíná číslem pořadí jako.0 a.1.
    
    soubor seznamu CRL Hello má příponu, která začíná pořadí jako .r0 nebo .r1. soubor seznamu CRL Hello je přidružen k certifikátu.

    ![Stáhněte si soubor extrahovat místně ](./media/backup-azure-backup-server-vmware/extracted-files-in-certs-folder.png)

5. V hello **certifikátů** složku, klikněte pravým tlačítkem na soubor hello kořenového certifikátu a pak klikněte na **přejmenovat**.

    ![Přejmenujte kořenový certifikát ](./media/backup-azure-backup-server-vmware/rename-cert.png)

    Změňte too.crt rozšíření hello kořenový certifikát. Po zobrazení dotazu, pokud si chcete toochange hello rozšíření, klikněte na tlačítko **Ano** nebo **OK**. Jinak můžete změnit hello soubor určený funkce. Ikona Hello hello souboru změny tooan ikonu, která představuje kořenový certifikát.

6. Klikněte pravým tlačítkem na hello kořenový certifikát a hello rozbalovací nabídce vyberte **nainstalovat certifikát**.

    Hello **Průvodce importem certifikátu** zobrazí se dialogové okno.

7. V hello **Průvodce importem certifikátu** dialogové okno, vyberte **místního počítače** jako cíl hello hello certifikátu, a pak klikněte na tlačítko **Další** toocontinue.

    ![Možnosti cílového úložiště certifikátů ](./media/backup-azure-backup-server-vmware/certificate-import-wizard1.png)

    Zobrazení dotazu, pokud chcete, aby počítač toohello tooallow změny, klikněte na tlačítko **Ano** nebo **OK**, tooall hello změny.

8. Na hello **úložiště certifikátů** vyberte **všechny certifikáty umístit v následujícím úložiště hello**a potom klikněte na **Procházet** úložiště certifikátů toochoose hello.

    ![Certifikáty umístit v pozici konkrétní úložiště](./media/backup-azure-backup-server-vmware/cert-import-wizard-local-store.png)

    Hello **vybrat úložiště certifikátů** zobrazí se dialogové okno.

    ![Hierarchie složky úložiště certifikátů](./media/backup-azure-backup-server-vmware/cert-store.png)

9. Vyberte **důvěryhodné kořenové certifikační autority** jako cílovou složku hello hello certifikáty a pak klikněte na tlačítko **OK**.

    ![Certifikát cílovou složku](./media/backup-azure-backup-server-vmware/certificate-store-selected.png)

    Hello **důvěryhodné kořenové certifikační autority** složky je potvrzeno, že úložiště certifikátů hello. Klikněte na **Další**.

    ![Složka úložiště certifikátů](./media/backup-azure-backup-server-vmware/certificate-import-wizard2.png)

10. Na hello **hello dokončení Průvodce importem certifikátu** zkontrolujte hello certifikátu je do požadované složky hello a pak klikněte na tlačítko **Dokončit**.

    ![Ověřte, zda je certifikát ve správné složce hello](./media/backup-azure-backup-server-vmware/cert-wizard-final-screen.png)

    Zobrazí se dialogové okno, import úspěšný certifikát hello je potvrzen.

11. Přihlaste se toohello vCenter Server tooconfirm, že připojení je bezpečné.

  Pokud importem certifikátu hello neproběhne úspěšně, a nemůže navázat zabezpečené připojení, dokumentaci hello VMware vSphere na [získání certifikátů serveru](http://pubs.vmware.com/vsphere-60/index.jsp#com.vmware.wssdk.dsg.doc/sdk_sg_server_certificate_Appendixes.6.4.html).

  Pokud máte zabezpečené hranice v rámci vaší organizace a nechcete, aby tooturn na hello protokol HTTPS, použijte následující postup toodisable hello zabezpečený komunikační hello.

### <a name="disable-secure-communication-protocol"></a>Zakažte zabezpečené komunikační protokol

Pokud vaše organizace nevyžaduje hello protokol HTTPS, použijte následující kroky toodisable HTTPS hello. toodisable hello výchozí chování, vytvořte klíč registru, který ignoruje hello výchozí chování.

1. Zkopírujte a vložte následující text do souboru TXT hello.

  ```
  Windows Registry Editor Version 5.00
  [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft Data Protection Manager\VMWare]
  "IgnoreCertificateValidation"=dword:00000001
  ```

2. Uložte hello souboru tooyour serveru Azure Backup počítače. Název souboru hello použijte DisableSecureAuthentication.reg.

3. Dvakrát klikněte na položku registru hello souboru tooactivate hello.


## <a name="create-a-role-and-user-account-on-hello-vcenter-server"></a>Vytvořte účet role a uživatele v systému vCenter Server hello

Role na hello vCenter Server, je předdefinovanou sadu oprávnění. Správce serveru vCenter vytvoří hello role. tooassign oprávnění správce hello páry uživatelské účty s rolí. tooestablish hello potřebné uživatelské přihlašovací údaje tooback hello vCenter Server počítače, vytvořit roli s konkrétní oprávnění a pak přidružit hello uživatelský účet s rolí hello.

Azure Backup Server používá tooauthenticate uživatelské jméno a heslo s hello systému vCenter Server. Azure Backup Server používá tyto přihlašovací údaje jako ověřování pro všechny operace zálohování.

tooadd vCenter role serveru a jeho oprávnění pro správce a zálohování:

1. Přihlášení v toohello systému vCenter Server a pak v systému vCenter Server hello **Navigátor** panelu, klikněte na tlačítko **správy**.

    ![Možnost správy panelu Navigátor serveru vCenter](./media/backup-azure-backup-server-vmware/vmware-navigator-panel.png)

2. V **správy** vyberte **role**a potom v hello **role** klikněte na panel hello přidat ikona role (hello + symbol).

    ![Přidání role](./media/backup-azure-backup-server-vmware/vmware-define-new-role.png)

    Hello **vytvořit roli** zobrazí se dialogové okno.

    ![Vytvoření role](./media/backup-azure-backup-server-vmware/vmware-define-new-role-priv.png)

3. V hello **vytvořit roli** dialogové okno, v hello **název Role** zadejte *BackupAdminRole*. Název role Hello může být vám líbí, ale měla by být rozpoznatelném za účelem hello role.

4. Vyberte hello oprávnění pro příslušnou verzi vCenter hello a pak klikněte na tlačítko **OK**. Následující tabulka Hello identifikuje hello vyžaduje oprávnění pro vCenter 6.0 a vCenter 5.5.

  Když vyberete hello oprávnění, klikněte na tlačítko hello ikonu další toohello nadřazený štítek tooexpand hello nadřazených a zobrazení hello podřízené oprávnění. tooselect hello VirtualMachine oprávnění, je nutné toogo několik úrovní do hello nadřazených podřízenou hierarchii. Nepotřebujete tooselect všechny podřízené oprávnění v rámci nadřazené oprávnění.

  ![Oprávnění hierarchie nadřazený-podřízený](./media/backup-azure-backup-server-vmware/cert-add-privilege-expand.png)

  Po kliknutí na tlačítko **OK**, hello nové role se zobrazí v seznamu hello na panelu role hello.

|Oprávnění pro vCenter 6.0| Oprávnění pro vCenter 5,5|
|--------------------------|---------------------------|
|Datastore.AllocateSpace   | Datastore.AllocateSpace|
|Global.ManageCustomFields | Global.ManageCustomerFields|
|Global.SetCustomFields    |   |
|Host.Local.CreateVM       | Network.Assign |
|Network.Assign            |  |
|Resource.AssignVMToPool   |  |
|VirtualMachine.Config.AddNewDisk  | VirtualMachine.Config.AddNewDisk   |
|VirtualMachine.Config.AdvanceConfig| VirtualMachine.Config.AdvancedConfig|
|VirtualMachine.Config.ChangeTracking| VirtualMachine.Config.ChangeTracking |
|VirtualMachine.Config.HostUSBDevice||
|VirtualMachine.Config.QueryUnownedFiles|    |
|VirtualMachine.Config.SwapPlacement| VirtualMachine.Config.SwapPlacement |
|VirtualMachine.Interact.PowerOff| VirtualMachine.Interact.PowerOff |
|VirtualMachine.Inventory.Create| VirtualMachine.Inventory.Create |
|VirtualMachine.Provisioning.DiskRandomAccess| |
|VirtualMachine.Provisioning.DiskRandomRead|VirtualMachine.Provisioning.DiskRandomRead |
|VirtualMachine.State.CreateSnapshot| VirtualMachine.State.CreateSnapshot|
|VirtualMachine.State.RemoveSnapshot|VirtualMachine.State.RemoveSnapshot |
</br>



## <a name="create-a-vcenter-server-user-account-and-permissions"></a>Vytvořte uživatelský účet systému vCenter Server a oprávnění

Po hello je nastavit role s oprávněními, vytvořte účet uživatele. Hello uživatelský účet má jméno a heslo, které poskytuje hello přihlašovací údaje, které se používají pro ověřování.

1. toocreate účet uživatele v systému vCenter Server hello **Navigátor** panelu, klikněte na tlačítko **uživatelů a skupin**.

    ![Možnost uživatelé a skupiny](./media/backup-azure-backup-server-vmware/vmware-userandgroup-panel.png)

    Hello **vCenter uživatelů a skupin** se zobrazí panel.

    ![panel vCenter uživatelů a skupin](./media/backup-azure-backup-server-vmware/usersandgroups.png)

2. V hello **vCenter uživatelů a skupin** panel, vyberte hello **uživatelé** a pak klikněte hello přidat ikona uživatelů (hello + symbol).

    Hello **nového uživatele** zobrazí se dialogové okno.

3. V hello **nového uživatele** dialogové okno pole, přidejte informace o uživateli hello a pak klikněte na tlačítko **OK**. V tomto postupu se uživatelské jméno hello BackupAdmin.

    ![Dialogové okno Nový uživatel](./media/backup-azure-backup-server-vmware/vmware-new-user-account.png)

    Hello nový uživatelský účet se zobrazí v seznamu hello.

4. tooassociate hello uživatelský účet s rolí hello v hello **Navigátor** panelu, klikněte na tlačítko **globální oprávnění**. V hello **globální oprávnění** panel, vyberte hello **spravovat** a pak klikněte hello přidat ikona (hello + symbol).

    ![Globální oprávnění panely](./media/backup-azure-backup-server-vmware/vmware-add-new-perms.png)

    Hello **globální oprávnění Root - přidat oprávnění** zobrazí se dialogové okno.

5. V hello **globální oprávnění Root - přidat oprávnění** dialogové okno, klikněte na tlačítko **přidat** toochoose hello uživatele nebo skupinu.

    ![Vyberte uživatele nebo skupinu](./media/backup-azure-backup-server-vmware/vmware-add-new-global-perm.png)

    Hello **vyberte uživatele nebo skupiny** zobrazí se dialogové okno.

6. V hello **vyberte uživatele nebo skupiny** dialogovém okně vyberte **BackupAdmin** a pak klikněte na **přidat**.

    V **uživatelé**, hello *doména\uživatelské_jméno* formát se používá pro hello uživatelský účet. Pokud chcete toouse jinou doménu, vyberte ji ze hello **domény** seznamu.

    ![Přidat uživatele BackupAdmin](./media/backup-azure-backup-server-vmware/vmware-assign-account-to-role.png)

    Klikněte na tlačítko **OK** tooadd hello vybrané uživatele toohello **přidat oprávnění** dialogové okno.

7. Teď, když jste zformulovali hello uživatele, přiřaďte role toohello hello uživatele. V **přiřadit Role**, hello rozevíracím seznamu vyberte **BackupAdminRole**a potom klikněte na **OK**.

    ![Přiřadit uživatele toorole](./media/backup-azure-backup-server-vmware/vmware-choose-role.png)

  Na hello **spravovat** ve hello **globální oprávnění** panelů, hello nový uživatelský účet a hello přidružené role jsou uvedeny v seznamu hello.


## <a name="establish-vcenter-server-credentials-on-azure-backup-server"></a>Vytvořit přihlašovací údaje serveru vCenter na serveru Azure Backup

Před přidáním hello VMware server tooAzure zálohovat Server nainstalovat [aktualizace 1 pro Azure Backup Server](https://support.microsoft.com/help/3175529/update-1-for-microsoft-azure-backup-server).

1. tooopen serveru Azure Backup, dvakrát klikněte na ikonu hello na ploše hello serveru Azure Backup.

    ![Zálohování serveru Azure – ikona](./media/backup-azure-backup-server-vmware/mabs-icon.png)

    Pokud nemůžete najít hello ikony na ploše hello, otevřete Azure Backup Server z hello seznam nainstalovaných aplikací. název aplikace Azure Backup Server Hello se nazývá Microsoft Azure Backup.

2. V konzole serveru Azure Backup hello klikněte **správy**, klikněte na tlačítko **produkční servery**a pak na pásu karet nástroje hello, klikněte na **spravovat VMware**.

    ![Azure Backup Server konzoly](./media/backup-azure-backup-server-vmware/add-vmware-credentials.png)

    Hello **spravovat pověření** zobrazí se dialogové okno.

    ![Dialogové okno Azure Backup Server spravovat pověření](./media/backup-azure-backup-server-vmware/mabs-manage-credentials-dialog.png)

3. V hello **spravovat pověření** dialogové okno, klikněte na tlačítko **přidat** tooopen hello **přidat přihlašovací údaje** dialogové okno.

4. V hello **přidat přihlašovací údaje** dialogovém okně zadejte název a popis pro hello nových přihlašovacích údajů. Pak zadejte hello uživatelské jméno a heslo. Název Hello *přihlašovacích údajů Contoso Vcenter* se používá v dalším postupu hello tooidentify hello přihlašovací údaje. Použití hello stejné uživatelské jméno a heslo, které se používá pro hello systému vCenter Server. Pokud nejsou v hello systému vCenter Server a Azure Backup Server hello ve stejné doméně, **uživatelské jméno**, zadejte doménu hello.

    ![Dialogové okno Azure Backup Server přidat přihlašovací údaje](./media/backup-azure-backup-server-vmware/mabs-add-credential-dialog2.png)

    Klikněte na tlačítko **přidat** tooadd hello nové přihlašovací údaje tooAzure zálohování serveru. Hello nových přihlašovacích údajů se zobrazí v seznamu hello v hello **spravovat pověření** dialogové okno.
    
    ![Dialogové okno Azure Backup Server spravovat pověření](./media/backup-azure-backup-server-vmware/new-list-of-mabs-creds.png)

5. tooclose hello **spravovat pověření** dialogovém okně klikněte na hello **X** v pravém horním rohu hello.


## <a name="add-hello-vcenter-server-tooazure-backup-server"></a>Přidat hello vCenter Server tooAzure zálohování serveru

Průvodce přidání provozního serveru je použité tooadd hello vCenter Server tooAzure zálohování serveru.

tooopen Průvodce přidání produkčním serveru, dokončení hello následující postup:

1. V konzole serveru Azure Backup hello klikněte **správy**, klikněte na tlačítko **produkční servery**a potom klikněte na **přidat**.

    ![Průvodce přidání serveru otevřete výroby](./media/backup-azure-backup-server-vmware/add-vcenter-to-mabs.png)

    Hello **Průvodce přidání serveru produkční** zobrazí se dialogové okno.

    ![Průvodce přidání provozním serveru](./media/backup-azure-backup-server-vmware/production-server-add-wizard.png)

2. Na hello **provozním serveru vyberte typ** vyberte **servery VMware**a potom klikněte na **Další**.

3. V **název nebo IP adresa serveru**, zadejte hello plně kvalifikovaný název domény (FQDN) nebo IP adresa serveru VMware hello. Pokud všechny servery ESXi hello spravuje hello stejný vCenter, můžete použít název vCenter hello.

    ![Zadejte plně kvalifikovaný název domény nebo IP adresa serveru VMware](./media/backup-azure-backup-server-vmware/add-vmware-server-provide-server-name.png)

4. V **SSL Port**, zadejte hello port, který je použité toocommunicate pomocí serveru VMware hello. Pomocí portu 443, což je výchozí port hello, pokud si nejste jisti, že jiný port je povinný.

5. V **zadejte přihlašovací údaje**, vyberte hello přihlašovacích údajů, kterou jste vytvořili dříve.

    ![Zadejte přihlašovací údaje](./media/backup-azure-backup-server-vmware/identify-creds.png)

6. Klikněte na tlačítko **přidat** tooadd hello VMware server toohello seznam **přidat servery VMware**a potom klikněte na **Další** toomove toohello další stránku průvodce hello.

    ![Přidání serveru VMWare a přihlašovacích údajů](./media/backup-azure-backup-server-vmware/add-vmware-server-credentials.png)

7. V hello **Souhrn** klikněte na tlačítko **přidat** tooadd hello zadaný VMware server tooAzure zálohování serveru.

    ![Přidat VMware server tooAzure zálohování serveru](./media/backup-azure-backup-server-vmware/tasks-screen.png)

  Zálohování serveru VMware Hello je bez agenta zálohování a okamžitě přidá nový server hello. Hello **Dokončit** stránka zobrazuje hello výsledky.

  ![Stránku](./media/backup-azure-backup-server-vmware/summary-screen.png)

  tooadd více instancí systému vCenter Server tooAzure zálohování serveru, opakování hello předchozí kroky v této části.

Po přidání hello vCenter Server tooAzure zálohovat Server hello dalším krokem je toocreate skupiny ochrany. Skupina ochrany Hello určuje hello různé podrobnosti krátký nebo dlouhodobé uchovávání a je kde definovat a aplikovat zásady zálohování hello. zásady zálohování Hello je hello plán pro při zálohování, a co se zálohuje.


## <a name="configure-a-protection-group"></a>Konfiguraci skupiny ochrany

Pokud nepoužijete System Center Data Protection Manager nebo serveru Azure Backup před, přečtěte si téma [plánování zálohování na disk](https://technet.microsoft.com/library/hh758026.aspx) tooprepare hardwarového prostředí. Po můžete zkontrolovat, zda máte správné úložiště, použijte hello vytvořením nové skupiny ochrany Průvodce tooadd VMware virtuální počítače.

1. V konzole serveru Azure Backup hello klikněte **ochrany**a v hello pásu karet nástroje klikněte na **nový** Průvodce vytvořením nové skupiny ochrany tooopen hello.

    ![Průvodce vytvořením nové skupiny ochrany otevřete hello](./media/backup-azure-backup-server-vmware/open-protection-wizard.png)

    Hello **vytvořením nové skupiny ochrany** se zobrazí dialogové okno průvodce.

    ![Dialogové okno Průvodce vytvořením nové skupiny ochrany](./media/backup-azure-backup-server-vmware/protection-wizard.png)

    Klikněte na tlačítko **Další** tooadvance toohello **vybrat typ skupiny ochrany** stránky.

2. Na hello **typ skupiny ochrany vyberte** vyberte **servery** a pak klikněte na **Další**. Hello **vybrat členy skupiny** se zobrazí stránka.

3. Na hello **vybrat členy skupiny** se zobrazí stránka, hello Dostupní členové a hello vybrané členy. Vyberte členy hello má tooprotect a pak klikněte na tlačítko **Další**.

    ![Vybrat členy skupiny](./media/backup-azure-backup-server-vmware/server-add-selected-members.png)

    Když vyberete člena, pokud vyberete složku obsahující jiné složky nebo virtuálních počítačů, budou vybrány také těchto složek a virtuálních počítačů. zahrnutí Hello hello složek a virtuální počítače v nadřazené složky hello se nazývá ochrany na úrovni složek. tooremove složku nebo virtuální počítač, hello zrušte zaškrtnutí políčka.

    Pokud virtuální počítač nebo složku obsahující virtuální počítač, již chráněné tooAzure, nelze vybrat tohoto virtuálního počítače znovu. To znamená Jakmile je virtuální počítač chráněný tooAzure, nemůže být chráněn znovu, která zabraňuje pro jeden virtuální počítač se vytváří body obnovení duplicitní. Pokud chcete, aby toosee kterou instanci serveru Azure Backup již chrání člen, bod toohello člen toosee hello název hello chrání server.

4. Na hello **vyberte způsob ochrany dat** stránky, zadejte název pro skupinu ochrany hello. Nebyly vybrány krátkodobou ochranu (toodisk) a online ochranu. Pokud chcete toouse online ochrany (tooAzure), je nutné použít toodisk krátkodobé ochrany. Klikněte na tlačítko **Další** tooproceed toohello krátkodobé ochrany rozsahu.

    ![Vyberte způsob ochrany dat](./media/backup-azure-backup-server-vmware/name-protection-group.png)

5. Na hello **zadat krátkodobé cíle** stránky, pro **rozsah uchování**, zadat hello počet dní, které chcete body obnovení tooretain *uložené toodisk*. Pokud chcete toochange hello čas a dny, kdy jsou pořizovány body obnovení, klikněte na tlačítko **upravit**. Hello krátkodobé body obnovení jsou úplné zálohy. Nejsou přírůstkové zálohy. Jakmile budete spokojeni s hello krátkodobé cíle, klikněte na tlačítko **Další**.

    ![Určení krátkodobých cílů](./media/backup-azure-backup-server-vmware/short-term-goals.png)

6. Na hello **zkontrolovat přidělení disku** zkontrolujte a v případě potřeby upravit hello místa na disku pro hello virtuálních počítačů. Hello doporučená přidělení disku jsou založené na rozsahu uchování hello, který je uveden v hello **zadat krátkodobé cíle** stránky, hello typu úlohy a hello velikost hello chráněných dat (identifikovaného v kroku 3).  

  - **Velikost dat:** velikost hello dat ve skupině ochrany hello.
  - **Místo na disku:** hello doporučené množství místa na disku pro skupinu ochrany hello. Pokud chcete toto nastavení toomodify, měli byste přidělit celkové místo, které je o něco větší než hello dobu, kdy očekáváte, že každý zdroj dat roste.
  - **Společné umístění dat:** při povolení společné umístění, více zdrojů dat v hello ochrany můžete namapovat tooa jednu repliku a svazek bodu obnovení. Společné umístění se nepodporuje pro všechny úlohy.
  - **Automaticky rozšiřovat:** Pokud povolíte toto nastavení, pokud data ve skupině hello chráněné přesáhnou hello počáteční přidělení, System Center Data Protection Manager se pokusí o 25 procent velikost disku tooincrease hello.
  - **Podrobnosti o fondu úložiště:** zobrazuje stav hello hello fondu úložiště, včetně celkové a zbývající velikosti disku.

    ![Zkontrolovat přidělení disku](./media/backup-azure-backup-server-vmware/review-disk-allocation.png)

    Jakmile budete spokojeni s hello přidělení místa, klikněte na tlačítko **Další**.

7. Na hello **vyberte způsob vytvoření repliky** určete, jak chcete počáteční kopii toogenerate hello nebo repliky hello chráněných dat na serveru Azure Backup.

    Výchozí hodnota Hello je **automaticky přes síť hello** a **nyní**. Pokud chcete použít výchozí hello, doporučujeme vám, že zadáte dobu mimo špičku. Zvolte **později** a zadejte den a čas.

    Pro velké objemy dat nebo méně než optimální síťové podmínky zvažte replikaci hello dat offline pomocí vyměnitelného média.

    Po provedení volby, klikněte na tlačítko **Další**.

    ![Vyberte způsob vytvoření repliky](./media/backup-azure-backup-server-vmware/replica-creation.png)

8. Na hello **možnosti kontroly konzistence** vyberte, jak a kdy kontrol konzistence tooautomate hello. Kontroly konzistence můžete spustit, když se stane nekonzistentní data repliky, nebo podle nastaveného plánu.

    Pokud nechcete, aby tooconfigure automatické kontroly konzistence, můžete spustit ruční kontrolu. V oblasti ochrany hello hello serveru Azure Backup konzoly, klikněte pravým tlačítkem na skupinu ochrany hello a pak vyberte **provést kontrolu konzistence**.

    Klikněte na tlačítko **Další** toomove toohello další stránku.

9. Na hello **zadat Data Online ochrany** vyberte jeden nebo více zdrojů dat, které chcete tooprotect. Můžete vybrat členy hello jednotlivě, nebo klikněte na tlačítko **Vybrat vše** toochoose všechny členy. Jakmile vyberete hello členů, klikněte na tlačítko **Další**.

    ![Zadejte data pro online ochranu](./media/backup-azure-backup-server-vmware/select-data-to-protect.png)

10. Na hello **zadejte plán Online zálohování** zadejte hello plán toogenerate bodů obnovení ze zálohy disku hello. Po vygenerování hello bod obnovení je trezor služeb zotavení přenášená toohello v Azure. Jakmile budete spokojeni s hello plán online zálohování, klikněte na tlačítko **Další**.

    ![Zadejte plán online zálohování](./media/backup-azure-backup-server-vmware/online-backup-schedule.png)

11. Na hello **zadejte zásady uchovávání Online** stránky, znamená to, jak dlouho tooretain hello zálohování dat v Azure. Po definování zásad hello klikněte na tlačítko **Další**.

    ![Zadejte zásady online uchovávání dat.](./media/backup-azure-backup-server-vmware/retention-policy.png)

    Neexistuje žádný časový limit pro jak dlouho ponechat data v Azure. Pokud ukládáte data bodu obnovení v Azure, hello jenom limit je, že nemůže mít víc než 9999 bodů obnovení za chráněné instance. V tomto příkladu je chráněný instance hello hello VMware server.

12. Na hello **Souhrn** zkontrolujte hello podrobnosti pro členy skupiny ochrany a nastavení a pak klikněte na tlačítko **vytvořit skupinu**.

    ![Souhrn nastavení a člena skupiny ochrany](./media/backup-azure-backup-server-vmware/protection-group-summary.png)

## <a name="next-steps"></a>Další kroky
Pokud chcete použít Azure Backup Server tooprotect VMware úlohy, může zajímat pomocí serveru Azure Backup toohelp chránit [Microsoft Exchange server](./backup-azure-exchange-mabs.md), [Microsoft SharePoint farmy](./backup-azure-backup-sharepoint-mabs.md), nebo [Databáze systému SQL Server](./backup-azure-sql-mabs.md).

Informace o problémech s registrace agenta hello konfigurace skupiny ochrany hello nebo zálohování úloh, najdete v [řešení potíží s Azure Backup Server](./backup-azure-mabs-troubleshoot.md).
