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
# <a name="back-up-a-vmware-server-tooazure"></a><span data-ttu-id="990e5-104">Zálohování server tooAzure VMware</span><span class="sxs-lookup"><span data-stu-id="990e5-104">Back up a VMware server tooAzure</span></span>

<span data-ttu-id="990e5-105">Tento článek vysvětluje, jak tooconfigure serveru Azure Backup toohelp chránit úlohy serveru VMware.</span><span class="sxs-lookup"><span data-stu-id="990e5-105">This article explains how tooconfigure Azure Backup Server toohelp protect VMware server workloads.</span></span> <span data-ttu-id="990e5-106">Tento článek předpokládá, že je již nainstalován Server Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="990e5-106">This article assumes you already have Azure Backup Server installed.</span></span> <span data-ttu-id="990e5-107">Pokud nemáte nainstalované serveru Azure Backup, přečtěte si téma [Příprava tooback až úlohy pomocí serveru Azure Backup](backup-azure-microsoft-azure-backup.md).</span><span class="sxs-lookup"><span data-stu-id="990e5-107">If you don't have Azure Backup Server installed, see [Prepare tooback up workloads using Azure Backup Server](backup-azure-microsoft-azure-backup.md).</span></span>

<span data-ttu-id="990e5-108">Azure Backup Server můžete zálohovat nebo pomoc při ochraně VMware vCenter Server verze 6.5, 6.0 a 5.5.</span><span class="sxs-lookup"><span data-stu-id="990e5-108">Azure Backup Server can back up, or help protect, VMware vCenter Server version 6.5, 6.0 and 5.5.</span></span>


## <a name="create-a-secure-connection-toohello-vcenter-server"></a><span data-ttu-id="990e5-109">Vytvoření zabezpečeného spojení toohello vCenter Server</span><span class="sxs-lookup"><span data-stu-id="990e5-109">Create a secure connection toohello vCenter Server</span></span>

<span data-ttu-id="990e5-110">Ve výchozím serveru Azure Backup komunikuje se každý Server vCenter přes kanál protokolu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="990e5-110">By default, Azure Backup Server communicates with each vCenter Server via an HTTPS channel.</span></span> <span data-ttu-id="990e5-111">tooturn na hello zabezpečenou komunikaci, doporučujeme nainstalovat certifikát hello VMware certifikační autoritou (CA) na serveru Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="990e5-111">tooturn on hello secure communication, we recommend that you install hello VMware Certificate Authority (CA) certificate on Azure Backup Server.</span></span> <span data-ttu-id="990e5-112">Pokud není zapotřebí zabezpečenou komunikaci a přejete požadavek HTTPS hello toodisable, najdete v části [zakázat zabezpečený komunikační protokol](backup-azure-backup-server-vmware.md#disable-secure-communication-protocol).</span><span class="sxs-lookup"><span data-stu-id="990e5-112">If you don't require secure communication, and would prefer toodisable hello HTTPS requirement, see [Disable secure communication protocol](backup-azure-backup-server-vmware.md#disable-secure-communication-protocol).</span></span> <span data-ttu-id="990e5-113">toocreate zabezpečené připojení mezi serverem pro zálohování Azure a hello systému vCenter Server, importujte certifikát důvěryhodné hello na serveru Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="990e5-113">toocreate a secure connection between Azure Backup Server and hello vCenter Server, import hello trusted certificate on Azure Backup Server.</span></span>

<span data-ttu-id="990e5-114">V prohlížeči se obvykle používá na hello serveru Azure Backup počítač tooconnect toohello vCenter Server prostřednictvím hello vSphere webového klienta.</span><span class="sxs-lookup"><span data-stu-id="990e5-114">Typically, you use a browser on hello Azure Backup Server machine tooconnect toohello vCenter Server via hello vSphere Web Client.</span></span> <span data-ttu-id="990e5-115">Hello prvním použití hello serveru Azure Backup prohlížeče tooconnect toohello vCenter Server, hello připojení není zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="990e5-115">hello first time you use hello Azure Backup Server browser tooconnect toohello vCenter Server, hello connection isn't secure.</span></span> <span data-ttu-id="990e5-116">Hello následující obrázek znázorňuje hello nezabezpečené připojení.</span><span class="sxs-lookup"><span data-stu-id="990e5-116">hello following image shows hello unsecured connection.</span></span>

![Příklad serveru tooVMware nezabezpečené připojení](./media/backup-azure-backup-server-vmware/unsecure-url.png)

<span data-ttu-id="990e5-118">toofix-li tento problém a vytvoří zabezpečené připojení, stáhněte si hello důvěryhodné kořenové Certifikační autority.</span><span class="sxs-lookup"><span data-stu-id="990e5-118">toofix this issue, and create a secure connection, download hello trusted root CA certificates.</span></span>

1. <span data-ttu-id="990e5-119">V prohlížeči hello na serveru Azure Backup zadejte hello URL toohello vSphere webového klienta.</span><span class="sxs-lookup"><span data-stu-id="990e5-119">In hello browser on Azure Backup Server, enter hello URL toohello vSphere Web Client.</span></span> <span data-ttu-id="990e5-120">Zobrazí se Hello vSphere webovému klientovi přihlašovací stránku.</span><span class="sxs-lookup"><span data-stu-id="990e5-120">hello vSphere Web Client login page appears.</span></span>

    ![vSphere webového klienta](./media/backup-azure-backup-server-vmware/vsphere-web-client.png)

    <span data-ttu-id="990e5-122">V dolní části hello hello informací pro správce a vývojáře, vyhledejte hello **stažení důvěryhodné kořenové Certifikační autority** odkaz.</span><span class="sxs-lookup"><span data-stu-id="990e5-122">At hello bottom of hello information for administrators and developers, locate hello **Download trusted root CA certificates** link.</span></span>

    ![Odkaz toodownload hello důvěryhodné kořenové Certifikační autority](./media/backup-azure-backup-server-vmware/vmware-download-ca-cert-prompt.png)

  <span data-ttu-id="990e5-124">Pokud nevidíte hello vSphere webovému klientovi přihlašovací stránky, zkontrolujte nastavení proxy serveru v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="990e5-124">If you don't see hello vSphere Web Client login page, check your browser's proxy settings.</span></span>

2. <span data-ttu-id="990e5-125">Klikněte na tlačítko **stažení důvěryhodné kořenové Certifikační autority**.</span><span class="sxs-lookup"><span data-stu-id="990e5-125">Click **Download trusted root CA certificates**.</span></span>

    <span data-ttu-id="990e5-126">Hello systému vCenter Server stáhne soubor tooyour místní počítač.</span><span class="sxs-lookup"><span data-stu-id="990e5-126">hello vCenter Server downloads a file tooyour local computer.</span></span> <span data-ttu-id="990e5-127">Hello název souboru je s názvem **Stáhnout**.</span><span class="sxs-lookup"><span data-stu-id="990e5-127">hello file's name is named **download**.</span></span> <span data-ttu-id="990e5-128">V závislosti na prohlížeči, obdržíte zprávu s dotazem, zda tooopen nebo uložení souboru hello.</span><span class="sxs-lookup"><span data-stu-id="990e5-128">Depending on your browser, you receive a message that asks whether tooopen or save hello file.</span></span>

    ![stáhnout zprávy, když se stáhnou certifikáty](./media/backup-azure-backup-server-vmware/download-certs.png)

3. <span data-ttu-id="990e5-130">Hello souboru tooa umístění pro uložení na Azure Backup Server.</span><span class="sxs-lookup"><span data-stu-id="990e5-130">Save hello file tooa location on Azure Backup Server.</span></span> <span data-ttu-id="990e5-131">Při ukládání hello souboru přidáte příponu názvu souboru .zip hello.</span><span class="sxs-lookup"><span data-stu-id="990e5-131">When you save hello file, add hello .zip file name extension.</span></span>

    <span data-ttu-id="990e5-132">Hello soubor je soubor .zip, který obsahuje hello informace o certifikátech hello.</span><span class="sxs-lookup"><span data-stu-id="990e5-132">hello file is a .zip file that contains hello information about hello certificates.</span></span> <span data-ttu-id="990e5-133">S příponou .zip hello můžete použít nástroje pro extrakci hello.</span><span class="sxs-lookup"><span data-stu-id="990e5-133">With hello .zip extension, you can use hello extraction tools.</span></span>

4. <span data-ttu-id="990e5-134">Klikněte pravým tlačítkem na **download.zip**a potom vyberte **Extrahovat vše** tooextract hello obsah.</span><span class="sxs-lookup"><span data-stu-id="990e5-134">Right-click **download.zip**, and then select **Extract All** tooextract hello contents.</span></span>

    <span data-ttu-id="990e5-135">soubor ZIP Hello extrahuje jeho obsah tooa složku s názvem **certifikátů**.</span><span class="sxs-lookup"><span data-stu-id="990e5-135">hello .zip file extracts its contents tooa folder named **certs**.</span></span> <span data-ttu-id="990e5-136">Dva typy souborů se zobrazí ve složce hello certifikátů.</span><span class="sxs-lookup"><span data-stu-id="990e5-136">Two types of files appear in hello certs folder.</span></span> <span data-ttu-id="990e5-137">soubor kořenového certifikátu Hello má příponu, která začíná číslem pořadí jako.0 a.1.</span><span class="sxs-lookup"><span data-stu-id="990e5-137">hello root certificate file has an extension that begins with a numbered sequence like .0 and .1.</span></span>
    
    <span data-ttu-id="990e5-138">soubor seznamu CRL Hello má příponu, která začíná pořadí jako .r0 nebo .r1.</span><span class="sxs-lookup"><span data-stu-id="990e5-138">hello CRL file has an extension that begins with a sequence like .r0 or .r1.</span></span> <span data-ttu-id="990e5-139">soubor seznamu CRL Hello je přidružen k certifikátu.</span><span class="sxs-lookup"><span data-stu-id="990e5-139">hello CRL file is associated with a certificate.</span></span>

    ![<span data-ttu-id="990e5-140">Stáhněte si soubor extrahovat místně</span><span class="sxs-lookup"><span data-stu-id="990e5-140">Download file extracted locally</span></span> ](./media/backup-azure-backup-server-vmware/extracted-files-in-certs-folder.png)

5. <span data-ttu-id="990e5-141">V hello **certifikátů** složku, klikněte pravým tlačítkem na soubor hello kořenového certifikátu a pak klikněte na **přejmenovat**.</span><span class="sxs-lookup"><span data-stu-id="990e5-141">In hello **certs** folder, right-click hello root certificate file, and then click **Rename**.</span></span>

    ![<span data-ttu-id="990e5-142">Přejmenujte kořenový certifikát</span><span class="sxs-lookup"><span data-stu-id="990e5-142">Rename root certificate</span></span> ](./media/backup-azure-backup-server-vmware/rename-cert.png)

    <span data-ttu-id="990e5-143">Změňte too.crt rozšíření hello kořenový certifikát.</span><span class="sxs-lookup"><span data-stu-id="990e5-143">Change hello root certificate's extension too.crt.</span></span> <span data-ttu-id="990e5-144">Po zobrazení dotazu, pokud si chcete toochange hello rozšíření, klikněte na tlačítko **Ano** nebo **OK**.</span><span class="sxs-lookup"><span data-stu-id="990e5-144">When you're asked if you're sure you want toochange hello extension, click **Yes** or **OK**.</span></span> <span data-ttu-id="990e5-145">Jinak můžete změnit hello soubor určený funkce.</span><span class="sxs-lookup"><span data-stu-id="990e5-145">Otherwise, you change hello file's intended function.</span></span> <span data-ttu-id="990e5-146">Ikona Hello hello souboru změny tooan ikonu, která představuje kořenový certifikát.</span><span class="sxs-lookup"><span data-stu-id="990e5-146">hello icon for hello file changes tooan icon that represents a root certificate.</span></span>

6. <span data-ttu-id="990e5-147">Klikněte pravým tlačítkem na hello kořenový certifikát a hello rozbalovací nabídce vyberte **nainstalovat certifikát**.</span><span class="sxs-lookup"><span data-stu-id="990e5-147">Right-click hello root certificate and from hello pop-up menu, select **Install Certificate**.</span></span>

    <span data-ttu-id="990e5-148">Hello **Průvodce importem certifikátu** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="990e5-148">hello **Certificate Import Wizard** dialog box appears.</span></span>

7. <span data-ttu-id="990e5-149">V hello **Průvodce importem certifikátu** dialogové okno, vyberte **místního počítače** jako cíl hello hello certifikátu, a pak klikněte na tlačítko **Další** toocontinue.</span><span class="sxs-lookup"><span data-stu-id="990e5-149">In hello **Certificate Import Wizard** dialog box, select **Local Machine** as hello destination for hello certificate, and then click **Next** toocontinue.</span></span>

    ![<span data-ttu-id="990e5-150">Možnosti cílového úložiště certifikátů</span><span class="sxs-lookup"><span data-stu-id="990e5-150">Certificate storage destination options</span></span> ](./media/backup-azure-backup-server-vmware/certificate-import-wizard1.png)

    <span data-ttu-id="990e5-151">Zobrazení dotazu, pokud chcete, aby počítač toohello tooallow změny, klikněte na tlačítko **Ano** nebo **OK**, tooall hello změny.</span><span class="sxs-lookup"><span data-stu-id="990e5-151">If you're asked if you want tooallow changes toohello computer, click **Yes** or **OK**, tooall hello changes.</span></span>

8. <span data-ttu-id="990e5-152">Na hello **úložiště certifikátů** vyberte **všechny certifikáty umístit v následujícím úložiště hello**a potom klikněte na **Procházet** úložiště certifikátů toochoose hello.</span><span class="sxs-lookup"><span data-stu-id="990e5-152">On hello **Certificate Store** page, select **Place all certificates in hello following store**, and then click **Browse** toochoose hello certificate store.</span></span>

    ![Certifikáty umístit v pozici konkrétní úložiště](./media/backup-azure-backup-server-vmware/cert-import-wizard-local-store.png)

    <span data-ttu-id="990e5-154">Hello **vybrat úložiště certifikátů** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="990e5-154">hello **Select Certificate Store** dialog box appears.</span></span>

    ![Hierarchie složky úložiště certifikátů](./media/backup-azure-backup-server-vmware/cert-store.png)

9. <span data-ttu-id="990e5-156">Vyberte **důvěryhodné kořenové certifikační autority** jako cílovou složku hello hello certifikáty a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="990e5-156">Select **Trusted Root Certification Authorities** as hello destination folder for hello certificates, and then click **OK**.</span></span>

    ![Certifikát cílovou složku](./media/backup-azure-backup-server-vmware/certificate-store-selected.png)

    <span data-ttu-id="990e5-158">Hello **důvěryhodné kořenové certifikační autority** složky je potvrzeno, že úložiště certifikátů hello.</span><span class="sxs-lookup"><span data-stu-id="990e5-158">hello **Trusted Root Certification Authorities** folder is confirmed as hello certificate store.</span></span> <span data-ttu-id="990e5-159">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="990e5-159">Click **Next**.</span></span>

    ![Složka úložiště certifikátů](./media/backup-azure-backup-server-vmware/certificate-import-wizard2.png)

10. <span data-ttu-id="990e5-161">Na hello **hello dokončení Průvodce importem certifikátu** zkontrolujte hello certifikátu je do požadované složky hello a pak klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="990e5-161">On hello **Completing hello Certificate Import Wizard** page, verify that hello certificate is in hello desired folder, and then click **Finish**.</span></span>

    ![Ověřte, zda je certifikát ve správné složce hello](./media/backup-azure-backup-server-vmware/cert-wizard-final-screen.png)

    <span data-ttu-id="990e5-163">Zobrazí se dialogové okno, import úspěšný certifikát hello je potvrzen.</span><span class="sxs-lookup"><span data-stu-id="990e5-163">A dialog box appears, hello successful certificate import is confirmed.</span></span>

11. <span data-ttu-id="990e5-164">Přihlaste se toohello vCenter Server tooconfirm, že připojení je bezpečné.</span><span class="sxs-lookup"><span data-stu-id="990e5-164">Sign in toohello vCenter Server tooconfirm that your connection is secure.</span></span>

  <span data-ttu-id="990e5-165">Pokud importem certifikátu hello neproběhne úspěšně, a nemůže navázat zabezpečené připojení, dokumentaci hello VMware vSphere na [získání certifikátů serveru](http://pubs.vmware.com/vsphere-60/index.jsp#com.vmware.wssdk.dsg.doc/sdk_sg_server_certificate_Appendixes.6.4.html).</span><span class="sxs-lookup"><span data-stu-id="990e5-165">If hello certificate import is not successful, and you cannot establish a secure connection, consult hello VMware vSphere documentation on [obtaining server certificates](http://pubs.vmware.com/vsphere-60/index.jsp#com.vmware.wssdk.dsg.doc/sdk_sg_server_certificate_Appendixes.6.4.html).</span></span>

  <span data-ttu-id="990e5-166">Pokud máte zabezpečené hranice v rámci vaší organizace a nechcete, aby tooturn na hello protokol HTTPS, použijte následující postup toodisable hello zabezpečený komunikační hello.</span><span class="sxs-lookup"><span data-stu-id="990e5-166">If you have secure boundaries within your organization, and don't want tooturn on hello HTTPS protocol, use hello following procedure toodisable hello secure communications.</span></span>

### <a name="disable-secure-communication-protocol"></a><span data-ttu-id="990e5-167">Zakažte zabezpečené komunikační protokol</span><span class="sxs-lookup"><span data-stu-id="990e5-167">Disable secure communication protocol</span></span>

<span data-ttu-id="990e5-168">Pokud vaše organizace nevyžaduje hello protokol HTTPS, použijte následující kroky toodisable HTTPS hello.</span><span class="sxs-lookup"><span data-stu-id="990e5-168">If your organization doesn't require hello HTTPS protocol, use hello following steps toodisable HTTPS.</span></span> <span data-ttu-id="990e5-169">toodisable hello výchozí chování, vytvořte klíč registru, který ignoruje hello výchozí chování.</span><span class="sxs-lookup"><span data-stu-id="990e5-169">toodisable hello default behavior, create a registry key that ignores hello default behavior.</span></span>

1. <span data-ttu-id="990e5-170">Zkopírujte a vložte následující text do souboru TXT hello.</span><span class="sxs-lookup"><span data-stu-id="990e5-170">Copy and paste hello following text into a .txt file.</span></span>

  ```
  Windows Registry Editor Version 5.00
  [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft Data Protection Manager\VMWare]
  "IgnoreCertificateValidation"=dword:00000001
  ```

2. <span data-ttu-id="990e5-171">Uložte hello souboru tooyour serveru Azure Backup počítače.</span><span class="sxs-lookup"><span data-stu-id="990e5-171">Save hello file tooyour Azure Backup Server computer.</span></span> <span data-ttu-id="990e5-172">Název souboru hello použijte DisableSecureAuthentication.reg.</span><span class="sxs-lookup"><span data-stu-id="990e5-172">For hello file name, use DisableSecureAuthentication.reg.</span></span>

3. <span data-ttu-id="990e5-173">Dvakrát klikněte na položku registru hello souboru tooactivate hello.</span><span class="sxs-lookup"><span data-stu-id="990e5-173">Double-click hello file tooactivate hello registry entry.</span></span>


## <a name="create-a-role-and-user-account-on-hello-vcenter-server"></a><span data-ttu-id="990e5-174">Vytvořte účet role a uživatele v systému vCenter Server hello</span><span class="sxs-lookup"><span data-stu-id="990e5-174">Create a role and user account on hello vCenter Server</span></span>

<span data-ttu-id="990e5-175">Role na hello vCenter Server, je předdefinovanou sadu oprávnění.</span><span class="sxs-lookup"><span data-stu-id="990e5-175">On hello vCenter Server, a role is a predefined set of privileges.</span></span> <span data-ttu-id="990e5-176">Správce serveru vCenter vytvoří hello role.</span><span class="sxs-lookup"><span data-stu-id="990e5-176">A vCenter Server administrator creates hello roles.</span></span> <span data-ttu-id="990e5-177">tooassign oprávnění správce hello páry uživatelské účty s rolí.</span><span class="sxs-lookup"><span data-stu-id="990e5-177">tooassign permissions, hello administrator pairs user accounts with a role.</span></span> <span data-ttu-id="990e5-178">tooestablish hello potřebné uživatelské přihlašovací údaje tooback hello vCenter Server počítače, vytvořit roli s konkrétní oprávnění a pak přidružit hello uživatelský účet s rolí hello.</span><span class="sxs-lookup"><span data-stu-id="990e5-178">tooestablish hello necessary user credentials tooback up hello vCenter Server computer, create a role with specific privileges, and then associate hello user account with hello role.</span></span>

<span data-ttu-id="990e5-179">Azure Backup Server používá tooauthenticate uživatelské jméno a heslo s hello systému vCenter Server.</span><span class="sxs-lookup"><span data-stu-id="990e5-179">Azure Backup Server uses a username and password tooauthenticate with hello vCenter Server.</span></span> <span data-ttu-id="990e5-180">Azure Backup Server používá tyto přihlašovací údaje jako ověřování pro všechny operace zálohování.</span><span class="sxs-lookup"><span data-stu-id="990e5-180">Azure Backup Server uses these credentials as authentication for all backup operations.</span></span>

<span data-ttu-id="990e5-181">tooadd vCenter role serveru a jeho oprávnění pro správce a zálohování:</span><span class="sxs-lookup"><span data-stu-id="990e5-181">tooadd a vCenter Server role and its privileges for a backup administrator:</span></span>

1. <span data-ttu-id="990e5-182">Přihlášení v toohello systému vCenter Server a pak v systému vCenter Server hello **Navigátor** panelu, klikněte na tlačítko **správy**.</span><span class="sxs-lookup"><span data-stu-id="990e5-182">Sign in toohello vCenter Server, and then in hello vCenter Server **Navigator** panel, click **Administration**.</span></span>

    ![Možnost správy panelu Navigátor serveru vCenter](./media/backup-azure-backup-server-vmware/vmware-navigator-panel.png)

2. <span data-ttu-id="990e5-184">V **správy** vyberte **role**a potom v hello **role** klikněte na panel hello přidat ikona role (hello + symbol).</span><span class="sxs-lookup"><span data-stu-id="990e5-184">In **Administration** select **Roles**, and then in hello **Roles** panel click hello add role icon (hello + symbol).</span></span>

    ![Přidání role](./media/backup-azure-backup-server-vmware/vmware-define-new-role.png)

    <span data-ttu-id="990e5-186">Hello **vytvořit roli** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="990e5-186">hello **Create Role** dialog box appears.</span></span>

    ![Vytvoření role](./media/backup-azure-backup-server-vmware/vmware-define-new-role-priv.png)

3. <span data-ttu-id="990e5-188">V hello **vytvořit roli** dialogové okno, v hello **název Role** zadejte *BackupAdminRole*.</span><span class="sxs-lookup"><span data-stu-id="990e5-188">In hello **Create Role** dialog box, in hello **Role name** box, enter *BackupAdminRole*.</span></span> <span data-ttu-id="990e5-189">Název role Hello může být vám líbí, ale měla by být rozpoznatelném za účelem hello role.</span><span class="sxs-lookup"><span data-stu-id="990e5-189">hello role name can be whatever you like, but it should be recognizable for hello role's purpose.</span></span>

4. <span data-ttu-id="990e5-190">Vyberte hello oprávnění pro příslušnou verzi vCenter hello a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="990e5-190">Select hello privileges for hello appropriate version of vCenter, and then click **OK**.</span></span> <span data-ttu-id="990e5-191">Následující tabulka Hello identifikuje hello vyžaduje oprávnění pro vCenter 6.0 a vCenter 5.5.</span><span class="sxs-lookup"><span data-stu-id="990e5-191">hello following table identifies hello required privileges for vCenter 6.0 and vCenter 5.5.</span></span>

  <span data-ttu-id="990e5-192">Když vyberete hello oprávnění, klikněte na tlačítko hello ikonu další toohello nadřazený štítek tooexpand hello nadřazených a zobrazení hello podřízené oprávnění.</span><span class="sxs-lookup"><span data-stu-id="990e5-192">When you select hello privileges, click hello icon next toohello parent label tooexpand hello parent and view hello child privileges.</span></span> <span data-ttu-id="990e5-193">tooselect hello VirtualMachine oprávnění, je nutné toogo několik úrovní do hello nadřazených podřízenou hierarchii.</span><span class="sxs-lookup"><span data-stu-id="990e5-193">tooselect hello VirtualMachine privileges, you need toogo several levels into hello parent child hierarchy.</span></span> <span data-ttu-id="990e5-194">Nepotřebujete tooselect všechny podřízené oprávnění v rámci nadřazené oprávnění.</span><span class="sxs-lookup"><span data-stu-id="990e5-194">You don't need tooselect all child privileges within a parent privilege.</span></span>

  ![Oprávnění hierarchie nadřazený-podřízený](./media/backup-azure-backup-server-vmware/cert-add-privilege-expand.png)

  <span data-ttu-id="990e5-196">Po kliknutí na tlačítko **OK**, hello nové role se zobrazí v seznamu hello na panelu role hello.</span><span class="sxs-lookup"><span data-stu-id="990e5-196">After you click **OK**, hello new role appears in hello list on hello Roles panel.</span></span>

|<span data-ttu-id="990e5-197">Oprávnění pro vCenter 6.0</span><span class="sxs-lookup"><span data-stu-id="990e5-197">Privileges for vCenter 6.0</span></span>| <span data-ttu-id="990e5-198">Oprávnění pro vCenter 5,5</span><span class="sxs-lookup"><span data-stu-id="990e5-198">Privileges for vCenter 5.5</span></span>|
|--------------------------|---------------------------|
|<span data-ttu-id="990e5-199">Datastore.AllocateSpace</span><span class="sxs-lookup"><span data-stu-id="990e5-199">Datastore.AllocateSpace</span></span>   | <span data-ttu-id="990e5-200">Datastore.AllocateSpace</span><span class="sxs-lookup"><span data-stu-id="990e5-200">Datastore.AllocateSpace</span></span>|
|<span data-ttu-id="990e5-201">Global.ManageCustomFields</span><span class="sxs-lookup"><span data-stu-id="990e5-201">Global.ManageCustomFields</span></span> | <span data-ttu-id="990e5-202">Global.ManageCustomerFields</span><span class="sxs-lookup"><span data-stu-id="990e5-202">Global.ManageCustomerFields</span></span>|
|<span data-ttu-id="990e5-203">Global.SetCustomFields</span><span class="sxs-lookup"><span data-stu-id="990e5-203">Global.SetCustomFields</span></span>    |   |
|<span data-ttu-id="990e5-204">Host.Local.CreateVM</span><span class="sxs-lookup"><span data-stu-id="990e5-204">Host.Local.CreateVM</span></span>       | <span data-ttu-id="990e5-205">Network.Assign</span><span class="sxs-lookup"><span data-stu-id="990e5-205">Network.Assign</span></span> |
|<span data-ttu-id="990e5-206">Network.Assign</span><span class="sxs-lookup"><span data-stu-id="990e5-206">Network.Assign</span></span>            |  |
|<span data-ttu-id="990e5-207">Resource.AssignVMToPool</span><span class="sxs-lookup"><span data-stu-id="990e5-207">Resource.AssignVMToPool</span></span>   |  |
|<span data-ttu-id="990e5-208">VirtualMachine.Config.AddNewDisk</span><span class="sxs-lookup"><span data-stu-id="990e5-208">VirtualMachine.Config.AddNewDisk</span></span>  | <span data-ttu-id="990e5-209">VirtualMachine.Config.AddNewDisk</span><span class="sxs-lookup"><span data-stu-id="990e5-209">VirtualMachine.Config.AddNewDisk</span></span>   |
|<span data-ttu-id="990e5-210">VirtualMachine.Config.AdvanceConfig</span><span class="sxs-lookup"><span data-stu-id="990e5-210">VirtualMachine.Config.AdvanceConfig</span></span>| <span data-ttu-id="990e5-211">VirtualMachine.Config.AdvancedConfig</span><span class="sxs-lookup"><span data-stu-id="990e5-211">VirtualMachine.Config.AdvancedConfig</span></span>|
|<span data-ttu-id="990e5-212">VirtualMachine.Config.ChangeTracking</span><span class="sxs-lookup"><span data-stu-id="990e5-212">VirtualMachine.Config.ChangeTracking</span></span>| <span data-ttu-id="990e5-213">VirtualMachine.Config.ChangeTracking</span><span class="sxs-lookup"><span data-stu-id="990e5-213">VirtualMachine.Config.ChangeTracking</span></span> |
|<span data-ttu-id="990e5-214">VirtualMachine.Config.HostUSBDevice</span><span class="sxs-lookup"><span data-stu-id="990e5-214">VirtualMachine.Config.HostUSBDevice</span></span>||
|<span data-ttu-id="990e5-215">VirtualMachine.Config.QueryUnownedFiles</span><span class="sxs-lookup"><span data-stu-id="990e5-215">VirtualMachine.Config.QueryUnownedFiles</span></span>|    |
|<span data-ttu-id="990e5-216">VirtualMachine.Config.SwapPlacement</span><span class="sxs-lookup"><span data-stu-id="990e5-216">VirtualMachine.Config.SwapPlacement</span></span>| <span data-ttu-id="990e5-217">VirtualMachine.Config.SwapPlacement</span><span class="sxs-lookup"><span data-stu-id="990e5-217">VirtualMachine.Config.SwapPlacement</span></span> |
|<span data-ttu-id="990e5-218">VirtualMachine.Interact.PowerOff</span><span class="sxs-lookup"><span data-stu-id="990e5-218">VirtualMachine.Interact.PowerOff</span></span>| <span data-ttu-id="990e5-219">VirtualMachine.Interact.PowerOff</span><span class="sxs-lookup"><span data-stu-id="990e5-219">VirtualMachine.Interact.PowerOff</span></span> |
|<span data-ttu-id="990e5-220">VirtualMachine.Inventory.Create</span><span class="sxs-lookup"><span data-stu-id="990e5-220">VirtualMachine.Inventory.Create</span></span>| <span data-ttu-id="990e5-221">VirtualMachine.Inventory.Create</span><span class="sxs-lookup"><span data-stu-id="990e5-221">VirtualMachine.Inventory.Create</span></span> |
|<span data-ttu-id="990e5-222">VirtualMachine.Provisioning.DiskRandomAccess</span><span class="sxs-lookup"><span data-stu-id="990e5-222">VirtualMachine.Provisioning.DiskRandomAccess</span></span>| |
|<span data-ttu-id="990e5-223">VirtualMachine.Provisioning.DiskRandomRead</span><span class="sxs-lookup"><span data-stu-id="990e5-223">VirtualMachine.Provisioning.DiskRandomRead</span></span>|<span data-ttu-id="990e5-224">VirtualMachine.Provisioning.DiskRandomRead</span><span class="sxs-lookup"><span data-stu-id="990e5-224">VirtualMachine.Provisioning.DiskRandomRead</span></span> |
|<span data-ttu-id="990e5-225">VirtualMachine.State.CreateSnapshot</span><span class="sxs-lookup"><span data-stu-id="990e5-225">VirtualMachine.State.CreateSnapshot</span></span>| <span data-ttu-id="990e5-226">VirtualMachine.State.CreateSnapshot</span><span class="sxs-lookup"><span data-stu-id="990e5-226">VirtualMachine.State.CreateSnapshot</span></span>|
|<span data-ttu-id="990e5-227">VirtualMachine.State.RemoveSnapshot</span><span class="sxs-lookup"><span data-stu-id="990e5-227">VirtualMachine.State.RemoveSnapshot</span></span>|<span data-ttu-id="990e5-228">VirtualMachine.State.RemoveSnapshot</span><span class="sxs-lookup"><span data-stu-id="990e5-228">VirtualMachine.State.RemoveSnapshot</span></span> |
</br>



## <a name="create-a-vcenter-server-user-account-and-permissions"></a><span data-ttu-id="990e5-229">Vytvořte uživatelský účet systému vCenter Server a oprávnění</span><span class="sxs-lookup"><span data-stu-id="990e5-229">Create a vCenter Server user account and permissions</span></span>

<span data-ttu-id="990e5-230">Po hello je nastavit role s oprávněními, vytvořte účet uživatele.</span><span class="sxs-lookup"><span data-stu-id="990e5-230">After hello role with privileges is set up, create a user account.</span></span> <span data-ttu-id="990e5-231">Hello uživatelský účet má jméno a heslo, které poskytuje hello přihlašovací údaje, které se používají pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="990e5-231">hello user account has a name and password, which provides hello credentials that are used for authentication.</span></span>

1. <span data-ttu-id="990e5-232">toocreate účet uživatele v systému vCenter Server hello **Navigátor** panelu, klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="990e5-232">toocreate a user account, in hello vCenter Server **Navigator** panel, click **Users and Groups**.</span></span>

    ![Možnost uživatelé a skupiny](./media/backup-azure-backup-server-vmware/vmware-userandgroup-panel.png)

    <span data-ttu-id="990e5-234">Hello **vCenter uživatelů a skupin** se zobrazí panel.</span><span class="sxs-lookup"><span data-stu-id="990e5-234">hello **vCenter Users and Groups** panel appears.</span></span>

    ![panel vCenter uživatelů a skupin](./media/backup-azure-backup-server-vmware/usersandgroups.png)

2. <span data-ttu-id="990e5-236">V hello **vCenter uživatelů a skupin** panel, vyberte hello **uživatelé** a pak klikněte hello přidat ikona uživatelů (hello + symbol).</span><span class="sxs-lookup"><span data-stu-id="990e5-236">In hello **vCenter Users and Groups** panel, select hello **Users** tab, and then click hello add users icon (hello + symbol).</span></span>

    <span data-ttu-id="990e5-237">Hello **nového uživatele** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="990e5-237">hello **New User** dialog box appears.</span></span>

3. <span data-ttu-id="990e5-238">V hello **nového uživatele** dialogové okno pole, přidejte informace o uživateli hello a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="990e5-238">In hello **New User** dialog box, add hello user's information and then click **OK**.</span></span> <span data-ttu-id="990e5-239">V tomto postupu se uživatelské jméno hello BackupAdmin.</span><span class="sxs-lookup"><span data-stu-id="990e5-239">In this procedure, hello username is BackupAdmin.</span></span>

    ![Dialogové okno Nový uživatel](./media/backup-azure-backup-server-vmware/vmware-new-user-account.png)

    <span data-ttu-id="990e5-241">Hello nový uživatelský účet se zobrazí v seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="990e5-241">hello new user account appears in hello list.</span></span>

4. <span data-ttu-id="990e5-242">tooassociate hello uživatelský účet s rolí hello v hello **Navigátor** panelu, klikněte na tlačítko **globální oprávnění**.</span><span class="sxs-lookup"><span data-stu-id="990e5-242">tooassociate hello user account with hello role, in hello **Navigator** panel, click **Global Permissions**.</span></span> <span data-ttu-id="990e5-243">V hello **globální oprávnění** panel, vyberte hello **spravovat** a pak klikněte hello přidat ikona (hello + symbol).</span><span class="sxs-lookup"><span data-stu-id="990e5-243">In hello **Global Permissions** panel, select hello **Manage** tab, and then click hello add icon (hello + symbol).</span></span>

    ![Globální oprávnění panely](./media/backup-azure-backup-server-vmware/vmware-add-new-perms.png)

    <span data-ttu-id="990e5-245">Hello **globální oprávnění Root - přidat oprávnění** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="990e5-245">hello **Global Permissions Root - Add Permission** dialog box appears.</span></span>

5. <span data-ttu-id="990e5-246">V hello **globální oprávnění Root - přidat oprávnění** dialogové okno, klikněte na tlačítko **přidat** toochoose hello uživatele nebo skupinu.</span><span class="sxs-lookup"><span data-stu-id="990e5-246">In hello **Global Permission Root - Add Permission** dialog box, click **Add** toochoose hello user or group.</span></span>

    ![Vyberte uživatele nebo skupinu](./media/backup-azure-backup-server-vmware/vmware-add-new-global-perm.png)

    <span data-ttu-id="990e5-248">Hello **vyberte uživatele nebo skupiny** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="990e5-248">hello **Select Users/Groups** dialog box appears.</span></span>

6. <span data-ttu-id="990e5-249">V hello **vyberte uživatele nebo skupiny** dialogovém okně vyberte **BackupAdmin** a pak klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="990e5-249">In hello **Select Users/Groups** dialog box, choose **BackupAdmin** and then click **Add**.</span></span>

    <span data-ttu-id="990e5-250">V **uživatelé**, hello *doména\uživatelské_jméno* formát se používá pro hello uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="990e5-250">In **Users**, hello *domain\username* format is used for hello user account.</span></span> <span data-ttu-id="990e5-251">Pokud chcete toouse jinou doménu, vyberte ji ze hello **domény** seznamu.</span><span class="sxs-lookup"><span data-stu-id="990e5-251">If you want toouse a different domain, choose it from hello **Domain** list.</span></span>

    ![Přidat uživatele BackupAdmin](./media/backup-azure-backup-server-vmware/vmware-assign-account-to-role.png)

    <span data-ttu-id="990e5-253">Klikněte na tlačítko **OK** tooadd hello vybrané uživatele toohello **přidat oprávnění** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="990e5-253">Click **OK** tooadd hello selected users toohello **Add Permission** dialog box.</span></span>

7. <span data-ttu-id="990e5-254">Teď, když jste zformulovali hello uživatele, přiřaďte role toohello hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="990e5-254">Now that you've identified hello user, assign hello user toohello role.</span></span> <span data-ttu-id="990e5-255">V **přiřadit Role**, hello rozevíracím seznamu vyberte **BackupAdminRole**a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="990e5-255">In **Assigned Role**, from hello drop-down list, select **BackupAdminRole**, and then click **OK**.</span></span>

    ![Přiřadit uživatele toorole](./media/backup-azure-backup-server-vmware/vmware-choose-role.png)

  <span data-ttu-id="990e5-257">Na hello **spravovat** ve hello **globální oprávnění** panelů, hello nový uživatelský účet a hello přidružené role jsou uvedeny v seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="990e5-257">On hello **Manage** tab in hello **Global Permissions** panel, hello new user account and hello associated role appear in hello list.</span></span>


## <a name="establish-vcenter-server-credentials-on-azure-backup-server"></a><span data-ttu-id="990e5-258">Vytvořit přihlašovací údaje serveru vCenter na serveru Azure Backup</span><span class="sxs-lookup"><span data-stu-id="990e5-258">Establish vCenter Server credentials on Azure Backup Server</span></span>

<span data-ttu-id="990e5-259">Před přidáním hello VMware server tooAzure zálohovat Server nainstalovat [aktualizace 1 pro Azure Backup Server](https://support.microsoft.com/help/3175529/update-1-for-microsoft-azure-backup-server).</span><span class="sxs-lookup"><span data-stu-id="990e5-259">Before you add hello VMware server tooAzure Backup Server, install [Update 1 for Azure Backup Server](https://support.microsoft.com/help/3175529/update-1-for-microsoft-azure-backup-server).</span></span>

1. <span data-ttu-id="990e5-260">tooopen serveru Azure Backup, dvakrát klikněte na ikonu hello na ploše hello serveru Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="990e5-260">tooopen Azure Backup Server, double-click hello icon on hello Azure Backup Server desktop.</span></span>

    ![Zálohování serveru Azure – ikona](./media/backup-azure-backup-server-vmware/mabs-icon.png)

    <span data-ttu-id="990e5-262">Pokud nemůžete najít hello ikony na ploše hello, otevřete Azure Backup Server z hello seznam nainstalovaných aplikací.</span><span class="sxs-lookup"><span data-stu-id="990e5-262">If you can't find hello icon on hello desktop, open Azure Backup Server from hello list of installed apps.</span></span> <span data-ttu-id="990e5-263">název aplikace Azure Backup Server Hello se nazývá Microsoft Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="990e5-263">hello Azure Backup Server app name is called Microsoft Azure Backup.</span></span>

2. <span data-ttu-id="990e5-264">V konzole serveru Azure Backup hello klikněte **správy**, klikněte na tlačítko **produkční servery**a pak na pásu karet nástroje hello, klikněte na **spravovat VMware**.</span><span class="sxs-lookup"><span data-stu-id="990e5-264">In hello Azure Backup Server console, click **Management**, click **Production Servers**, and then on hello tool ribbon, click **Manage VMware**.</span></span>

    ![Azure Backup Server konzoly](./media/backup-azure-backup-server-vmware/add-vmware-credentials.png)

    <span data-ttu-id="990e5-266">Hello **spravovat pověření** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="990e5-266">hello **Manage Credentials** dialog box appears.</span></span>

    ![Dialogové okno Azure Backup Server spravovat pověření](./media/backup-azure-backup-server-vmware/mabs-manage-credentials-dialog.png)

3. <span data-ttu-id="990e5-268">V hello **spravovat pověření** dialogové okno, klikněte na tlačítko **přidat** tooopen hello **přidat přihlašovací údaje** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="990e5-268">In hello **Manage Credentials** dialog box, click **Add** tooopen hello **Add Credential** dialog box.</span></span>

4. <span data-ttu-id="990e5-269">V hello **přidat přihlašovací údaje** dialogovém okně zadejte název a popis pro hello nových přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="990e5-269">In hello **Add Credential** dialog box, enter a name and a description for hello new credential.</span></span> <span data-ttu-id="990e5-270">Pak zadejte hello uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="990e5-270">Then specify hello username and password.</span></span> <span data-ttu-id="990e5-271">Název Hello *přihlašovacích údajů Contoso Vcenter* se používá v dalším postupu hello tooidentify hello přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="990e5-271">hello name, *Contoso Vcenter credential* is used tooidentify hello credential in hello next procedure.</span></span> <span data-ttu-id="990e5-272">Použití hello stejné uživatelské jméno a heslo, které se používá pro hello systému vCenter Server.</span><span class="sxs-lookup"><span data-stu-id="990e5-272">Use hello same username and password that is used for hello vCenter Server.</span></span> <span data-ttu-id="990e5-273">Pokud nejsou v hello systému vCenter Server a Azure Backup Server hello ve stejné doméně, **uživatelské jméno**, zadejte doménu hello.</span><span class="sxs-lookup"><span data-stu-id="990e5-273">If hello vCenter Server and Azure Backup Server are not in hello same domain, in **User name**, specify hello domain.</span></span>

    ![Dialogové okno Azure Backup Server přidat přihlašovací údaje](./media/backup-azure-backup-server-vmware/mabs-add-credential-dialog2.png)

    <span data-ttu-id="990e5-275">Klikněte na tlačítko **přidat** tooadd hello nové přihlašovací údaje tooAzure zálohování serveru.</span><span class="sxs-lookup"><span data-stu-id="990e5-275">Click **Add** tooadd hello new credential tooAzure Backup Server.</span></span> <span data-ttu-id="990e5-276">Hello nových přihlašovacích údajů se zobrazí v seznamu hello v hello **spravovat pověření** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="990e5-276">hello new credential appears in hello list in hello **Manage Credentials** dialog box.</span></span>
    
    ![Dialogové okno Azure Backup Server spravovat pověření](./media/backup-azure-backup-server-vmware/new-list-of-mabs-creds.png)

5. <span data-ttu-id="990e5-278">tooclose hello **spravovat pověření** dialogovém okně klikněte na hello **X** v pravém horním rohu hello.</span><span class="sxs-lookup"><span data-stu-id="990e5-278">tooclose hello **Manage Credentials** dialog box, click hello **X** in hello upper-right corner.</span></span>


## <a name="add-hello-vcenter-server-tooazure-backup-server"></a><span data-ttu-id="990e5-279">Přidat hello vCenter Server tooAzure zálohování serveru</span><span class="sxs-lookup"><span data-stu-id="990e5-279">Add hello vCenter Server tooAzure Backup Server</span></span>

<span data-ttu-id="990e5-280">Průvodce přidání provozního serveru je použité tooadd hello vCenter Server tooAzure zálohování serveru.</span><span class="sxs-lookup"><span data-stu-id="990e5-280">Production Server Addition Wizard is used tooadd hello vCenter Server tooAzure Backup Server.</span></span>

<span data-ttu-id="990e5-281">tooopen Průvodce přidání produkčním serveru, dokončení hello následující postup:</span><span class="sxs-lookup"><span data-stu-id="990e5-281">tooopen Production Server Addition Wizard, complete hello following procedure:</span></span>

1. <span data-ttu-id="990e5-282">V konzole serveru Azure Backup hello klikněte **správy**, klikněte na tlačítko **produkční servery**a potom klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="990e5-282">In hello Azure Backup Server console, click **Management**, click **Production Servers**, and then click **Add**.</span></span>

    ![Průvodce přidání serveru otevřete výroby](./media/backup-azure-backup-server-vmware/add-vcenter-to-mabs.png)

    <span data-ttu-id="990e5-284">Hello **Průvodce přidání serveru produkční** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="990e5-284">hello **Production Server Addition Wizard** dialog box appears.</span></span>

    ![Průvodce přidání provozním serveru](./media/backup-azure-backup-server-vmware/production-server-add-wizard.png)

2. <span data-ttu-id="990e5-286">Na hello **provozním serveru vyberte typ** vyberte **servery VMware**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="990e5-286">On hello **Select Production Server type** page, select **VMware Servers**, and then click **Next**.</span></span>

3. <span data-ttu-id="990e5-287">V **název nebo IP adresa serveru**, zadejte hello plně kvalifikovaný název domény (FQDN) nebo IP adresa serveru VMware hello.</span><span class="sxs-lookup"><span data-stu-id="990e5-287">In **Server Name/IP Address**, specify hello fully qualified domain name (FQDN) or IP address of hello VMware server.</span></span> <span data-ttu-id="990e5-288">Pokud všechny servery ESXi hello spravuje hello stejný vCenter, můžete použít název vCenter hello.</span><span class="sxs-lookup"><span data-stu-id="990e5-288">If all hello ESXi servers are managed by hello same vCenter, you can use hello vCenter name.</span></span>

    ![Zadejte plně kvalifikovaný název domény nebo IP adresa serveru VMware](./media/backup-azure-backup-server-vmware/add-vmware-server-provide-server-name.png)

4. <span data-ttu-id="990e5-290">V **SSL Port**, zadejte hello port, který je použité toocommunicate pomocí serveru VMware hello.</span><span class="sxs-lookup"><span data-stu-id="990e5-290">In **SSL Port**, enter hello port that is used toocommunicate with hello VMware server.</span></span> <span data-ttu-id="990e5-291">Pomocí portu 443, což je výchozí port hello, pokud si nejste jisti, že jiný port je povinný.</span><span class="sxs-lookup"><span data-stu-id="990e5-291">Use port 443, which is hello default port, unless you know that a different port is required.</span></span>

5. <span data-ttu-id="990e5-292">V **zadejte přihlašovací údaje**, vyberte hello přihlašovacích údajů, kterou jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="990e5-292">In **Specify Credential**, select hello credential that you created earlier.</span></span>

    ![Zadejte přihlašovací údaje](./media/backup-azure-backup-server-vmware/identify-creds.png)

6. <span data-ttu-id="990e5-294">Klikněte na tlačítko **přidat** tooadd hello VMware server toohello seznam **přidat servery VMware**a potom klikněte na **Další** toomove toohello další stránku průvodce hello.</span><span class="sxs-lookup"><span data-stu-id="990e5-294">Click **Add** tooadd hello VMware server toohello list of **Added VMware Servers**, and then click **Next** toomove toohello next page in hello wizard.</span></span>

    ![Přidání serveru VMWare a přihlašovacích údajů](./media/backup-azure-backup-server-vmware/add-vmware-server-credentials.png)

7. <span data-ttu-id="990e5-296">V hello **Souhrn** klikněte na tlačítko **přidat** tooadd hello zadaný VMware server tooAzure zálohování serveru.</span><span class="sxs-lookup"><span data-stu-id="990e5-296">In hello **Summary** page, click **Add** tooadd hello specified VMware server tooAzure Backup Server.</span></span>

    ![Přidat VMware server tooAzure zálohování serveru](./media/backup-azure-backup-server-vmware/tasks-screen.png)

  <span data-ttu-id="990e5-298">Zálohování serveru VMware Hello je bez agenta zálohování a okamžitě přidá nový server hello.</span><span class="sxs-lookup"><span data-stu-id="990e5-298">hello VMware server backup is an agentless backup, and hello new server is added immediately.</span></span> <span data-ttu-id="990e5-299">Hello **Dokončit** stránka zobrazuje hello výsledky.</span><span class="sxs-lookup"><span data-stu-id="990e5-299">hello **Finish** page shows you hello results.</span></span>

  ![Stránku](./media/backup-azure-backup-server-vmware/summary-screen.png)

  <span data-ttu-id="990e5-301">tooadd více instancí systému vCenter Server tooAzure zálohování serveru, opakování hello předchozí kroky v této části.</span><span class="sxs-lookup"><span data-stu-id="990e5-301">tooadd multiple instances of vCenter Server tooAzure Backup Server, repeat hello previous steps in this section.</span></span>

<span data-ttu-id="990e5-302">Po přidání hello vCenter Server tooAzure zálohovat Server hello dalším krokem je toocreate skupiny ochrany.</span><span class="sxs-lookup"><span data-stu-id="990e5-302">After you add hello vCenter Server tooAzure Backup Server, hello next step is toocreate a protection group.</span></span> <span data-ttu-id="990e5-303">Skupina ochrany Hello určuje hello různé podrobnosti krátký nebo dlouhodobé uchovávání a je kde definovat a aplikovat zásady zálohování hello.</span><span class="sxs-lookup"><span data-stu-id="990e5-303">hello protection group specifies hello various details for short or long-term retention, and it is where you define and apply hello backup policy.</span></span> <span data-ttu-id="990e5-304">zásady zálohování Hello je hello plán pro při zálohování, a co se zálohuje.</span><span class="sxs-lookup"><span data-stu-id="990e5-304">hello backup policy is hello schedule for when backups occur, and what is backed up.</span></span>


## <a name="configure-a-protection-group"></a><span data-ttu-id="990e5-305">Konfiguraci skupiny ochrany</span><span class="sxs-lookup"><span data-stu-id="990e5-305">Configure a protection group</span></span>

<span data-ttu-id="990e5-306">Pokud nepoužijete System Center Data Protection Manager nebo serveru Azure Backup před, přečtěte si téma [plánování zálohování na disk](https://technet.microsoft.com/library/hh758026.aspx) tooprepare hardwarového prostředí.</span><span class="sxs-lookup"><span data-stu-id="990e5-306">If you have not used System Center Data Protection Manager or Azure Backup Server before, see [Plan for disk backups](https://technet.microsoft.com/library/hh758026.aspx) tooprepare your hardware environment.</span></span> <span data-ttu-id="990e5-307">Po můžete zkontrolovat, zda máte správné úložiště, použijte hello vytvořením nové skupiny ochrany Průvodce tooadd VMware virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="990e5-307">After you check that you have proper storage, use hello Create New Protection Group wizard tooadd VMware virtual machines.</span></span>

1. <span data-ttu-id="990e5-308">V konzole serveru Azure Backup hello klikněte **ochrany**a v hello pásu karet nástroje klikněte na **nový** Průvodce vytvořením nové skupiny ochrany tooopen hello.</span><span class="sxs-lookup"><span data-stu-id="990e5-308">In hello Azure Backup Server console, click **Protection**, and in hello tool ribbon, click **New** tooopen hello Create New Protection Group wizard.</span></span>

    ![Průvodce vytvořením nové skupiny ochrany otevřete hello](./media/backup-azure-backup-server-vmware/open-protection-wizard.png)

    <span data-ttu-id="990e5-310">Hello **vytvořením nové skupiny ochrany** se zobrazí dialogové okno průvodce.</span><span class="sxs-lookup"><span data-stu-id="990e5-310">hello **Create New Protection Group** wizard dialog box appears.</span></span>

    ![Dialogové okno Průvodce vytvořením nové skupiny ochrany](./media/backup-azure-backup-server-vmware/protection-wizard.png)

    <span data-ttu-id="990e5-312">Klikněte na tlačítko **Další** tooadvance toohello **vybrat typ skupiny ochrany** stránky.</span><span class="sxs-lookup"><span data-stu-id="990e5-312">Click **Next** tooadvance toohello **Select protection group type** page.</span></span>

2. <span data-ttu-id="990e5-313">Na hello **typ skupiny ochrany vyberte** vyberte **servery** a pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="990e5-313">On hello **Select Protection group type** page, select **Servers** and then click **Next**.</span></span> <span data-ttu-id="990e5-314">Hello **vybrat členy skupiny** se zobrazí stránka.</span><span class="sxs-lookup"><span data-stu-id="990e5-314">hello **Select group members** page appears.</span></span>

3. <span data-ttu-id="990e5-315">Na hello **vybrat členy skupiny** se zobrazí stránka, hello Dostupní členové a hello vybrané členy.</span><span class="sxs-lookup"><span data-stu-id="990e5-315">On hello **Select group members** page, hello available members and hello selected members appear.</span></span> <span data-ttu-id="990e5-316">Vyberte členy hello má tooprotect a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="990e5-316">Select hello members that you want tooprotect, and then click **Next**.</span></span>

    ![Vybrat členy skupiny](./media/backup-azure-backup-server-vmware/server-add-selected-members.png)

    <span data-ttu-id="990e5-318">Když vyberete člena, pokud vyberete složku obsahující jiné složky nebo virtuálních počítačů, budou vybrány také těchto složek a virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="990e5-318">When you select a member, if you select a folder that contains other folders or VMs, those folders and VMs are also selected.</span></span> <span data-ttu-id="990e5-319">zahrnutí Hello hello složek a virtuální počítače v nadřazené složky hello se nazývá ochrany na úrovni složek.</span><span class="sxs-lookup"><span data-stu-id="990e5-319">hello inclusion of hello folders and VMs in hello parent folder is called folder-level protection.</span></span> <span data-ttu-id="990e5-320">tooremove složku nebo virtuální počítač, hello zrušte zaškrtnutí políčka.</span><span class="sxs-lookup"><span data-stu-id="990e5-320">tooremove a folder or VM, clear hello check box.</span></span>

    <span data-ttu-id="990e5-321">Pokud virtuální počítač nebo složku obsahující virtuální počítač, již chráněné tooAzure, nelze vybrat tohoto virtuálního počítače znovu.</span><span class="sxs-lookup"><span data-stu-id="990e5-321">If a VM, or a folder containing a VM, is already protected tooAzure, you cannot select that VM again.</span></span> <span data-ttu-id="990e5-322">To znamená Jakmile je virtuální počítač chráněný tooAzure, nemůže být chráněn znovu, která zabraňuje pro jeden virtuální počítač se vytváří body obnovení duplicitní.</span><span class="sxs-lookup"><span data-stu-id="990e5-322">That is, after a VM is protected tooAzure, it cannot be protected again, which prevents duplicate recovery points from being created for one VM.</span></span> <span data-ttu-id="990e5-323">Pokud chcete, aby toosee kterou instanci serveru Azure Backup již chrání člen, bod toohello člen toosee hello název hello chrání server.</span><span class="sxs-lookup"><span data-stu-id="990e5-323">If you want toosee which Azure Backup Server instance already protects a member, point toohello member toosee hello name of hello protecting server.</span></span>

4. <span data-ttu-id="990e5-324">Na hello **vyberte způsob ochrany dat** stránky, zadejte název pro skupinu ochrany hello.</span><span class="sxs-lookup"><span data-stu-id="990e5-324">On hello **Select Data Protection Method** page, enter a name for hello protection group.</span></span> <span data-ttu-id="990e5-325">Nebyly vybrány krátkodobou ochranu (toodisk) a online ochranu.</span><span class="sxs-lookup"><span data-stu-id="990e5-325">Short-term protection (toodisk) and online protection are selected.</span></span> <span data-ttu-id="990e5-326">Pokud chcete toouse online ochrany (tooAzure), je nutné použít toodisk krátkodobé ochrany.</span><span class="sxs-lookup"><span data-stu-id="990e5-326">If you want toouse online protection (tooAzure), you must use short-term protection toodisk.</span></span> <span data-ttu-id="990e5-327">Klikněte na tlačítko **Další** tooproceed toohello krátkodobé ochrany rozsahu.</span><span class="sxs-lookup"><span data-stu-id="990e5-327">Click **Next** tooproceed toohello short-term protection range.</span></span>

    ![Vyberte způsob ochrany dat](./media/backup-azure-backup-server-vmware/name-protection-group.png)

5. <span data-ttu-id="990e5-329">Na hello **zadat krátkodobé cíle** stránky, pro **rozsah uchování**, zadat hello počet dní, které chcete body obnovení tooretain *uložené toodisk*.</span><span class="sxs-lookup"><span data-stu-id="990e5-329">On hello **Specify Short-Term Goals** page, for **Retention Range**, specify hello number of days that you want tooretain recovery points that are *stored toodisk*.</span></span> <span data-ttu-id="990e5-330">Pokud chcete toochange hello čas a dny, kdy jsou pořizovány body obnovení, klikněte na tlačítko **upravit**.</span><span class="sxs-lookup"><span data-stu-id="990e5-330">If you want toochange hello time and days when recovery points are taken, click **Modify**.</span></span> <span data-ttu-id="990e5-331">Hello krátkodobé body obnovení jsou úplné zálohy.</span><span class="sxs-lookup"><span data-stu-id="990e5-331">hello short-term recovery points are full backups.</span></span> <span data-ttu-id="990e5-332">Nejsou přírůstkové zálohy.</span><span class="sxs-lookup"><span data-stu-id="990e5-332">They are not incremental backups.</span></span> <span data-ttu-id="990e5-333">Jakmile budete spokojeni s hello krátkodobé cíle, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="990e5-333">When you are satisfied with hello short-term goals, click **Next**.</span></span>

    ![Určení krátkodobých cílů](./media/backup-azure-backup-server-vmware/short-term-goals.png)

6. <span data-ttu-id="990e5-335">Na hello **zkontrolovat přidělení disku** zkontrolujte a v případě potřeby upravit hello místa na disku pro hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="990e5-335">On hello **Review Disk Allocation** page, review and if necessary, modify hello disk space for hello VMs.</span></span> <span data-ttu-id="990e5-336">Hello doporučená přidělení disku jsou založené na rozsahu uchování hello, který je uveden v hello **zadat krátkodobé cíle** stránky, hello typu úlohy a hello velikost hello chráněných dat (identifikovaného v kroku 3).</span><span class="sxs-lookup"><span data-stu-id="990e5-336">hello recommended disk allocations are based on hello retention range that is specified in hello **Specify Short-Term Goals** page, hello type of workload, and hello size of hello protected data (identified in step 3).</span></span>  

  - <span data-ttu-id="990e5-337">**Velikost dat:** velikost hello dat ve skupině ochrany hello.</span><span class="sxs-lookup"><span data-stu-id="990e5-337">**Data size:** Size of hello data in hello protection group.</span></span>
  - <span data-ttu-id="990e5-338">**Místo na disku:** hello doporučené množství místa na disku pro skupinu ochrany hello.</span><span class="sxs-lookup"><span data-stu-id="990e5-338">**Disk space:** hello recommended amount of disk space for hello protection group.</span></span> <span data-ttu-id="990e5-339">Pokud chcete toto nastavení toomodify, měli byste přidělit celkové místo, které je o něco větší než hello dobu, kdy očekáváte, že každý zdroj dat roste.</span><span class="sxs-lookup"><span data-stu-id="990e5-339">If you want toomodify this setting, you should allocate total space that is slightly larger than hello amount that you estimate each data source grows.</span></span>
  - <span data-ttu-id="990e5-340">**Společné umístění dat:** při povolení společné umístění, více zdrojů dat v hello ochrany můžete namapovat tooa jednu repliku a svazek bodu obnovení.</span><span class="sxs-lookup"><span data-stu-id="990e5-340">**Colocate data:** If you turn on colocation, multiple data sources in hello protection can map tooa single replica and recovery point volume.</span></span> <span data-ttu-id="990e5-341">Společné umístění se nepodporuje pro všechny úlohy.</span><span class="sxs-lookup"><span data-stu-id="990e5-341">Colocation isn't supported for all workloads.</span></span>
  - <span data-ttu-id="990e5-342">**Automaticky rozšiřovat:** Pokud povolíte toto nastavení, pokud data ve skupině hello chráněné přesáhnou hello počáteční přidělení, System Center Data Protection Manager se pokusí o 25 procent velikost disku tooincrease hello.</span><span class="sxs-lookup"><span data-stu-id="990e5-342">**Automatically grow:** If you turn on this setting, if data in hello protected group outgrows hello initial allocation, System Center Data Protection Manager tries tooincrease hello disk size by 25 percent.</span></span>
  - <span data-ttu-id="990e5-343">**Podrobnosti o fondu úložiště:** zobrazuje stav hello hello fondu úložiště, včetně celkové a zbývající velikosti disku.</span><span class="sxs-lookup"><span data-stu-id="990e5-343">**Storage pool details:** Shows hello status of hello storage pool, including total and remaining disk size.</span></span>

    ![Zkontrolovat přidělení disku](./media/backup-azure-backup-server-vmware/review-disk-allocation.png)

    <span data-ttu-id="990e5-345">Jakmile budete spokojeni s hello přidělení místa, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="990e5-345">When you are satisfied with hello space allocation, click **Next**.</span></span>

7. <span data-ttu-id="990e5-346">Na hello **vyberte způsob vytvoření repliky** určete, jak chcete počáteční kopii toogenerate hello nebo repliky hello chráněných dat na serveru Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="990e5-346">On hello **Choose Replica Creation Method** page, specify how you want toogenerate hello initial copy, or replica, of hello protected data on Azure Backup Server.</span></span>

    <span data-ttu-id="990e5-347">Výchozí hodnota Hello je **automaticky přes síť hello** a **nyní**.</span><span class="sxs-lookup"><span data-stu-id="990e5-347">hello default is **Automatically over hello network** and **Now**.</span></span> <span data-ttu-id="990e5-348">Pokud chcete použít výchozí hello, doporučujeme vám, že zadáte dobu mimo špičku.</span><span class="sxs-lookup"><span data-stu-id="990e5-348">If you use hello default, we recommend that you specify an off-peak time.</span></span> <span data-ttu-id="990e5-349">Zvolte **později** a zadejte den a čas.</span><span class="sxs-lookup"><span data-stu-id="990e5-349">Choose **Later** and specify a day and time.</span></span>

    <span data-ttu-id="990e5-350">Pro velké objemy dat nebo méně než optimální síťové podmínky zvažte replikaci hello dat offline pomocí vyměnitelného média.</span><span class="sxs-lookup"><span data-stu-id="990e5-350">For large amounts of data or less-than-optimal network conditions, consider replicating hello data offline by using removable media.</span></span>

    <span data-ttu-id="990e5-351">Po provedení volby, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="990e5-351">After you have made your choices, click **Next**.</span></span>

    ![Vyberte způsob vytvoření repliky](./media/backup-azure-backup-server-vmware/replica-creation.png)

8. <span data-ttu-id="990e5-353">Na hello **možnosti kontroly konzistence** vyberte, jak a kdy kontrol konzistence tooautomate hello.</span><span class="sxs-lookup"><span data-stu-id="990e5-353">On hello **Consistency Check Options** page, select how and when tooautomate hello consistency checks.</span></span> <span data-ttu-id="990e5-354">Kontroly konzistence můžete spustit, když se stane nekonzistentní data repliky, nebo podle nastaveného plánu.</span><span class="sxs-lookup"><span data-stu-id="990e5-354">You can run consistency checks when replica data becomes inconsistent, or on a set schedule.</span></span>

    <span data-ttu-id="990e5-355">Pokud nechcete, aby tooconfigure automatické kontroly konzistence, můžete spustit ruční kontrolu.</span><span class="sxs-lookup"><span data-stu-id="990e5-355">If you don't want tooconfigure automatic consistency checks, you can run a manual check.</span></span> <span data-ttu-id="990e5-356">V oblasti ochrany hello hello serveru Azure Backup konzoly, klikněte pravým tlačítkem na skupinu ochrany hello a pak vyberte **provést kontrolu konzistence**.</span><span class="sxs-lookup"><span data-stu-id="990e5-356">In hello protection area of hello Azure Backup Server console, right-click hello protection group and then select **Perform Consistency Check**.</span></span>

    <span data-ttu-id="990e5-357">Klikněte na tlačítko **Další** toomove toohello další stránku.</span><span class="sxs-lookup"><span data-stu-id="990e5-357">Click **Next** toomove toohello next page.</span></span>

9. <span data-ttu-id="990e5-358">Na hello **zadat Data Online ochrany** vyberte jeden nebo více zdrojů dat, které chcete tooprotect.</span><span class="sxs-lookup"><span data-stu-id="990e5-358">On hello **Specify Online Protection Data** page, select one or more data sources that you want tooprotect.</span></span> <span data-ttu-id="990e5-359">Můžete vybrat členy hello jednotlivě, nebo klikněte na tlačítko **Vybrat vše** toochoose všechny členy.</span><span class="sxs-lookup"><span data-stu-id="990e5-359">You can select hello members individually, or click **Select All** toochoose all members.</span></span> <span data-ttu-id="990e5-360">Jakmile vyberete hello členů, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="990e5-360">After you choose hello members, click **Next**.</span></span>

    ![Zadejte data pro online ochranu](./media/backup-azure-backup-server-vmware/select-data-to-protect.png)

10. <span data-ttu-id="990e5-362">Na hello **zadejte plán Online zálohování** zadejte hello plán toogenerate bodů obnovení ze zálohy disku hello.</span><span class="sxs-lookup"><span data-stu-id="990e5-362">On hello **Specify Online Backup Schedule** page, specify hello schedule toogenerate recovery points from hello disk backup.</span></span> <span data-ttu-id="990e5-363">Po vygenerování hello bod obnovení je trezor služeb zotavení přenášená toohello v Azure.</span><span class="sxs-lookup"><span data-stu-id="990e5-363">After hello recovery point is generated, it is transferred toohello Recovery Services vault in Azure.</span></span> <span data-ttu-id="990e5-364">Jakmile budete spokojeni s hello plán online zálohování, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="990e5-364">When you are satisfied with hello online backup schedule, click **Next**.</span></span>

    ![Zadejte plán online zálohování](./media/backup-azure-backup-server-vmware/online-backup-schedule.png)

11. <span data-ttu-id="990e5-366">Na hello **zadejte zásady uchovávání Online** stránky, znamená to, jak dlouho tooretain hello zálohování dat v Azure.</span><span class="sxs-lookup"><span data-stu-id="990e5-366">On hello **Specify Online Retention Policy** page, indicate how long you want tooretain hello backup data in Azure.</span></span> <span data-ttu-id="990e5-367">Po definování zásad hello klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="990e5-367">After hello policy is defined, click **Next**.</span></span>

    ![Zadejte zásady online uchovávání dat.](./media/backup-azure-backup-server-vmware/retention-policy.png)

    <span data-ttu-id="990e5-369">Neexistuje žádný časový limit pro jak dlouho ponechat data v Azure.</span><span class="sxs-lookup"><span data-stu-id="990e5-369">There is no time limit for how long you can keep data in Azure.</span></span> <span data-ttu-id="990e5-370">Pokud ukládáte data bodu obnovení v Azure, hello jenom limit je, že nemůže mít víc než 9999 bodů obnovení za chráněné instance.</span><span class="sxs-lookup"><span data-stu-id="990e5-370">When you store recovery point data in Azure, hello only limit is that you cannot have more than 9999 recovery points per protected instance.</span></span> <span data-ttu-id="990e5-371">V tomto příkladu je chráněný instance hello hello VMware server.</span><span class="sxs-lookup"><span data-stu-id="990e5-371">In this example, hello protected instance is hello VMware server.</span></span>

12. <span data-ttu-id="990e5-372">Na hello **Souhrn** zkontrolujte hello podrobnosti pro členy skupiny ochrany a nastavení a pak klikněte na tlačítko **vytvořit skupinu**.</span><span class="sxs-lookup"><span data-stu-id="990e5-372">On hello **Summary** page, review hello details for your protection group members and settings, and then click **Create Group**.</span></span>

    ![Souhrn nastavení a člena skupiny ochrany](./media/backup-azure-backup-server-vmware/protection-group-summary.png)

## <a name="next-steps"></a><span data-ttu-id="990e5-374">Další kroky</span><span class="sxs-lookup"><span data-stu-id="990e5-374">Next steps</span></span>
<span data-ttu-id="990e5-375">Pokud chcete použít Azure Backup Server tooprotect VMware úlohy, může zajímat pomocí serveru Azure Backup toohelp chránit [Microsoft Exchange server](./backup-azure-exchange-mabs.md), [Microsoft SharePoint farmy](./backup-azure-backup-sharepoint-mabs.md), nebo [Databáze systému SQL Server](./backup-azure-sql-mabs.md).</span><span class="sxs-lookup"><span data-stu-id="990e5-375">If you use Azure Backup Server tooprotect VMware workloads, you may be interested in using Azure Backup Server toohelp protect a [Microsoft Exchange server](./backup-azure-exchange-mabs.md), a [Microsoft SharePoint farm](./backup-azure-backup-sharepoint-mabs.md), or a [SQL Server database](./backup-azure-sql-mabs.md).</span></span>

<span data-ttu-id="990e5-376">Informace o problémech s registrace agenta hello konfigurace skupiny ochrany hello nebo zálohování úloh, najdete v [řešení potíží s Azure Backup Server](./backup-azure-mabs-troubleshoot.md).</span><span class="sxs-lookup"><span data-stu-id="990e5-376">For information on problems with registering hello agent, configuring hello protection group, or backing up jobs, see [Troubleshoot Azure Backup Server](./backup-azure-mabs-troubleshoot.md).</span></span>
