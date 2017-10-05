---
title: "Zálohujte servery VMware s serveru Azure Backup | Microsoft Docs"
description: "Používejte Azure Backup Server pro zálohování servery VMware vCenter/ESXi do Azure nebo disk. Tento článek obsahuje krok za krokem instrukce pro zálohování (nebo ochrany) = úlohy VMware."
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
ms.openlocfilehash: ad331dffb7c31d12290f4223967c568e4535fe3c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="back-up-a-vmware-server-to-azure"></a><span data-ttu-id="a8da9-104">Zálohovat VMware server do Azure</span><span class="sxs-lookup"><span data-stu-id="a8da9-104">Back up a VMware server to Azure</span></span>

<span data-ttu-id="a8da9-105">Tento článek vysvětluje postup konfigurace serveru Azure Backup k ochraně úlohy serveru VMware.</span><span class="sxs-lookup"><span data-stu-id="a8da9-105">This article explains how to configure Azure Backup Server to help protect VMware server workloads.</span></span> <span data-ttu-id="a8da9-106">Tento článek předpokládá, že je již nainstalován Server Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="a8da9-106">This article assumes you already have Azure Backup Server installed.</span></span> <span data-ttu-id="a8da9-107">Pokud nemáte nainstalované serveru Azure Backup, přečtěte si téma [Příprava zálohování úloh pomocí serveru Azure Backup](backup-azure-microsoft-azure-backup.md).</span><span class="sxs-lookup"><span data-stu-id="a8da9-107">If you don't have Azure Backup Server installed, see [Prepare to back up workloads using Azure Backup Server](backup-azure-microsoft-azure-backup.md).</span></span>

<span data-ttu-id="a8da9-108">Azure Backup Server můžete zálohovat nebo pomoc při ochraně VMware vCenter Server verze 6.5, 6.0 a 5.5.</span><span class="sxs-lookup"><span data-stu-id="a8da9-108">Azure Backup Server can back up, or help protect, VMware vCenter Server version 6.5, 6.0 and 5.5.</span></span>


## <a name="create-a-secure-connection-to-the-vcenter-server"></a><span data-ttu-id="a8da9-109">Vytvoření bezpečného připojení k systému vCenter Server</span><span class="sxs-lookup"><span data-stu-id="a8da9-109">Create a secure connection to the vCenter Server</span></span>

<span data-ttu-id="a8da9-110">Ve výchozím serveru Azure Backup komunikuje se každý Server vCenter přes kanál protokolu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a8da9-110">By default, Azure Backup Server communicates with each vCenter Server via an HTTPS channel.</span></span> <span data-ttu-id="a8da9-111">Chcete-li zapnout zabezpečenou komunikaci, doporučujeme nainstalovat certifikát VMware certifikační autoritou (CA) na serveru Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="a8da9-111">To turn on the secure communication, we recommend that you install the VMware Certificate Authority (CA) certificate on Azure Backup Server.</span></span> <span data-ttu-id="a8da9-112">Pokud není zapotřebí zabezpečenou komunikaci a přejete zakažte požadavek na protokol HTTPS, najdete v části [zakázat zabezpečený komunikační protokol](backup-azure-backup-server-vmware.md#disable-secure-communication-protocol).</span><span class="sxs-lookup"><span data-stu-id="a8da9-112">If you don't require secure communication, and would prefer to disable the HTTPS requirement, see [Disable secure communication protocol](backup-azure-backup-server-vmware.md#disable-secure-communication-protocol).</span></span> <span data-ttu-id="a8da9-113">K vytvoření bezpečného připojení mezi serverem pro zálohování Azure a vCenter Server, importujte důvěryhodný certifikát na serveru Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="a8da9-113">To create a secure connection between Azure Backup Server and the vCenter Server, import the trusted certificate on Azure Backup Server.</span></span>

<span data-ttu-id="a8da9-114">Obvykle použijete v prohlížeči na počítači serveru Azure Backup pro připojení k systému vCenter Server prostřednictvím vSphere webového klienta.</span><span class="sxs-lookup"><span data-stu-id="a8da9-114">Typically, you use a browser on the Azure Backup Server machine to connect to the vCenter Server via the vSphere Web Client.</span></span> <span data-ttu-id="a8da9-115">Při prvním pomocí serveru Azure Backup prohlížeče pro připojení k systému vCenter Server, připojení není zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="a8da9-115">The first time you use the Azure Backup Server browser to connect to the vCenter Server, the connection isn't secure.</span></span> <span data-ttu-id="a8da9-116">Následující obrázek znázorňuje nezabezpečené připojení.</span><span class="sxs-lookup"><span data-stu-id="a8da9-116">The following image shows the unsecured connection.</span></span>

![Příklad nezabezpečené připojení k serveru VMware](./media/backup-azure-backup-server-vmware/unsecure-url.png)

<span data-ttu-id="a8da9-118">Pokud chcete tento problém vyřešit a vytvoří zabezpečené připojení, stáhněte certifikáty důvěryhodné kořenové certifikační Autority.</span><span class="sxs-lookup"><span data-stu-id="a8da9-118">To fix this issue, and create a secure connection, download the trusted root CA certificates.</span></span>

1. <span data-ttu-id="a8da9-119">V prohlížeči na serveru Azure Backup zadejte adresu URL vSphere webového klienta.</span><span class="sxs-lookup"><span data-stu-id="a8da9-119">In the browser on Azure Backup Server, enter the URL to the vSphere Web Client.</span></span> <span data-ttu-id="a8da9-120">Zobrazí se stránka vSphere webovému klientovi přihlášení.</span><span class="sxs-lookup"><span data-stu-id="a8da9-120">The vSphere Web Client login page appears.</span></span>

    ![vSphere webového klienta](./media/backup-azure-backup-server-vmware/vsphere-web-client.png)

    <span data-ttu-id="a8da9-122">V dolní části informace o Správci a vývojáři, vyhledejte **stažení důvěryhodné kořenové Certifikační autority** odkaz.</span><span class="sxs-lookup"><span data-stu-id="a8da9-122">At the bottom of the information for administrators and developers, locate the **Download trusted root CA certificates** link.</span></span>

    ![Odkaz ke stažení certifikáty důvěryhodné kořenové certifikační Autority](./media/backup-azure-backup-server-vmware/vmware-download-ca-cert-prompt.png)

  <span data-ttu-id="a8da9-124">Pokud nevidíte přihlašovací stránky vSphere webovému klientovi, zkontrolujte nastavení proxy serveru v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="a8da9-124">If you don't see the vSphere Web Client login page, check your browser's proxy settings.</span></span>

2. <span data-ttu-id="a8da9-125">Klikněte na tlačítko **stažení důvěryhodné kořenové Certifikační autority**.</span><span class="sxs-lookup"><span data-stu-id="a8da9-125">Click **Download trusted root CA certificates**.</span></span>

    <span data-ttu-id="a8da9-126">VCenter Server stáhne soubor do místního počítače.</span><span class="sxs-lookup"><span data-stu-id="a8da9-126">The vCenter Server downloads a file to your local computer.</span></span> <span data-ttu-id="a8da9-127">Název souboru je s názvem **Stáhnout**.</span><span class="sxs-lookup"><span data-stu-id="a8da9-127">The file's name is named **download**.</span></span> <span data-ttu-id="a8da9-128">V závislosti na prohlížeči obdržíte zprávu s dotazem, zda chcete otevřít nebo uložit soubor.</span><span class="sxs-lookup"><span data-stu-id="a8da9-128">Depending on your browser, you receive a message that asks whether to open or save the file.</span></span>

    ![stáhnout zprávy, když se stáhnou certifikáty](./media/backup-azure-backup-server-vmware/download-certs.png)

3. <span data-ttu-id="a8da9-130">Uložte soubor do umístění na serveru Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="a8da9-130">Save the file to a location on Azure Backup Server.</span></span> <span data-ttu-id="a8da9-131">Při ukládání souboru přidáte příponu názvu souboru .zip.</span><span class="sxs-lookup"><span data-stu-id="a8da9-131">When you save the file, add the .zip file name extension.</span></span>

    <span data-ttu-id="a8da9-132">Soubor je soubor ZIP, který obsahuje informace o certifikátech.</span><span class="sxs-lookup"><span data-stu-id="a8da9-132">The file is a .zip file that contains the information about the certificates.</span></span> <span data-ttu-id="a8da9-133">S příponou .zip můžete použít nástroje extrakce.</span><span class="sxs-lookup"><span data-stu-id="a8da9-133">With the .zip extension, you can use the extraction tools.</span></span>

4. <span data-ttu-id="a8da9-134">Klikněte pravým tlačítkem na **download.zip**a potom vyberte **Extrahovat vše** extrahujte obsah.</span><span class="sxs-lookup"><span data-stu-id="a8da9-134">Right-click **download.zip**, and then select **Extract All** to extract the contents.</span></span>

    <span data-ttu-id="a8da9-135">Soubor .zip extrahuje její obsah do složky s názvem **certifikátů**.</span><span class="sxs-lookup"><span data-stu-id="a8da9-135">The .zip file extracts its contents to a folder named **certs**.</span></span> <span data-ttu-id="a8da9-136">Dva typy souborů se zobrazí ve složce pomocí certifikátů.</span><span class="sxs-lookup"><span data-stu-id="a8da9-136">Two types of files appear in the certs folder.</span></span> <span data-ttu-id="a8da9-137">Soubor kořenového certifikátu má příponu, která začíná číslem pořadí jako.0 a.1.</span><span class="sxs-lookup"><span data-stu-id="a8da9-137">The root certificate file has an extension that begins with a numbered sequence like .0 and .1.</span></span>
    
    <span data-ttu-id="a8da9-138">Soubor seznamu CRL má příponu, která začíná pořadí jako .r0 nebo .r1.</span><span class="sxs-lookup"><span data-stu-id="a8da9-138">The CRL file has an extension that begins with a sequence like .r0 or .r1.</span></span> <span data-ttu-id="a8da9-139">Soubor seznamu CRL je přidružen k certifikátu.</span><span class="sxs-lookup"><span data-stu-id="a8da9-139">The CRL file is associated with a certificate.</span></span>

    ![<span data-ttu-id="a8da9-140">Stáhněte si soubor extrahovat místně</span><span class="sxs-lookup"><span data-stu-id="a8da9-140">Download file extracted locally</span></span> ](./media/backup-azure-backup-server-vmware/extracted-files-in-certs-folder.png)

5. <span data-ttu-id="a8da9-141">V **certifikátů** složku, klikněte pravým tlačítkem na soubor kořenového certifikátu a pak klikněte na **přejmenovat**.</span><span class="sxs-lookup"><span data-stu-id="a8da9-141">In the **certs** folder, right-click the root certificate file, and then click **Rename**.</span></span>

    ![<span data-ttu-id="a8da9-142">Přejmenujte kořenový certifikát</span><span class="sxs-lookup"><span data-stu-id="a8da9-142">Rename root certificate</span></span> ](./media/backup-azure-backup-server-vmware/rename-cert.png)

    <span data-ttu-id="a8da9-143">Změňte rozšíření kořenový certifikát na .crt.</span><span class="sxs-lookup"><span data-stu-id="a8da9-143">Change the root certificate's extension to .crt.</span></span> <span data-ttu-id="a8da9-144">Po zobrazení dotazu, pokud jste si jisti, kterou chcete změnit rozšíření, klikněte na tlačítko **Ano** nebo **OK**.</span><span class="sxs-lookup"><span data-stu-id="a8da9-144">When you're asked if you're sure you want to change the extension, click **Yes** or **OK**.</span></span> <span data-ttu-id="a8da9-145">Jinak můžete změnit funkce určený v souboru.</span><span class="sxs-lookup"><span data-stu-id="a8da9-145">Otherwise, you change the file's intended function.</span></span> <span data-ttu-id="a8da9-146">Ikona souboru se změní na ikonu, která reprezentuje kořenový certifikát.</span><span class="sxs-lookup"><span data-stu-id="a8da9-146">The icon for the file changes to an icon that represents a root certificate.</span></span>

6. <span data-ttu-id="a8da9-147">Klikněte pravým tlačítkem na kořenový certifikát a v místní nabídce vyberte **nainstalovat certifikát**.</span><span class="sxs-lookup"><span data-stu-id="a8da9-147">Right-click the root certificate and from the pop-up menu, select **Install Certificate**.</span></span>

    <span data-ttu-id="a8da9-148">**Průvodce importem certifikátu** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a8da9-148">The **Certificate Import Wizard** dialog box appears.</span></span>

7. <span data-ttu-id="a8da9-149">V **Průvodce importem certifikátu** dialogové okno, vyberte **místního počítače** jako cíl pro certifikát a pak klikněte na tlačítko **Další** pokračujte.</span><span class="sxs-lookup"><span data-stu-id="a8da9-149">In the **Certificate Import Wizard** dialog box, select **Local Machine** as the destination for the certificate, and then click **Next** to continue.</span></span>

    ![<span data-ttu-id="a8da9-150">Možnosti cílového úložiště certifikátů</span><span class="sxs-lookup"><span data-stu-id="a8da9-150">Certificate storage destination options</span></span> ](./media/backup-azure-backup-server-vmware/certificate-import-wizard1.png)

    <span data-ttu-id="a8da9-151">Zobrazení dotazu, pokud chcete povolit změny v počítači, klikněte na tlačítko **Ano** nebo **OK**, všechny změny.</span><span class="sxs-lookup"><span data-stu-id="a8da9-151">If you're asked if you want to allow changes to the computer, click **Yes** or **OK**, to all the changes.</span></span>

8. <span data-ttu-id="a8da9-152">Na **úložiště certifikátů** vyberte **všechny certifikáty umístit v následujícím úložišti**a potom klikněte na **Procházet** vybrat úložiště certifikátů.</span><span class="sxs-lookup"><span data-stu-id="a8da9-152">On the **Certificate Store** page, select **Place all certificates in the following store**, and then click **Browse** to choose the certificate store.</span></span>

    ![Certifikáty umístit v pozici konkrétní úložiště](./media/backup-azure-backup-server-vmware/cert-import-wizard-local-store.png)

    <span data-ttu-id="a8da9-154">**Vybrat úložiště certifikátů** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a8da9-154">The **Select Certificate Store** dialog box appears.</span></span>

    ![Hierarchie složky úložiště certifikátů](./media/backup-azure-backup-server-vmware/cert-store.png)

9. <span data-ttu-id="a8da9-156">Vyberte **důvěryhodné kořenové certifikační autority** jako cílová složka pro certifikáty a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="a8da9-156">Select **Trusted Root Certification Authorities** as the destination folder for the certificates, and then click **OK**.</span></span>

    ![Certifikát cílovou složku](./media/backup-azure-backup-server-vmware/certificate-store-selected.png)

    <span data-ttu-id="a8da9-158">**Důvěryhodné kořenové certifikační autority** složky je potvrzeno, že úložiště certifikátů.</span><span class="sxs-lookup"><span data-stu-id="a8da9-158">The **Trusted Root Certification Authorities** folder is confirmed as the certificate store.</span></span> <span data-ttu-id="a8da9-159">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="a8da9-159">Click **Next**.</span></span>

    ![Složka úložiště certifikátů](./media/backup-azure-backup-server-vmware/certificate-import-wizard2.png)

10. <span data-ttu-id="a8da9-161">Na **dokončení Průvodce importem certifikátu** stránka, zkontrolujte, zda je certifikát na požadovanou složku a pak klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="a8da9-161">On the **Completing the Certificate Import Wizard** page, verify that the certificate is in the desired folder, and then click **Finish**.</span></span>

    ![Ověřte, zda je certifikát ve správné složce](./media/backup-azure-backup-server-vmware/cert-wizard-final-screen.png)

    <span data-ttu-id="a8da9-163">Zobrazí se dialogové okno, import úspěšný certifikátu je potvrzen.</span><span class="sxs-lookup"><span data-stu-id="a8da9-163">A dialog box appears, the successful certificate import is confirmed.</span></span>

11. <span data-ttu-id="a8da9-164">Přihlásit se k serveru vCenter a potvrďte, že je zabezpečené připojení.</span><span class="sxs-lookup"><span data-stu-id="a8da9-164">Sign in to the vCenter Server to confirm that your connection is secure.</span></span>

  <span data-ttu-id="a8da9-165">Pokud neproběhne import certifikátu úspěšně, a nemůže navázat zabezpečené připojení, v dokumentaci k VMware vSphere na [získání certifikátů serveru](http://pubs.vmware.com/vsphere-60/index.jsp#com.vmware.wssdk.dsg.doc/sdk_sg_server_certificate_Appendixes.6.4.html).</span><span class="sxs-lookup"><span data-stu-id="a8da9-165">If the certificate import is not successful, and you cannot establish a secure connection, consult the VMware vSphere documentation on [obtaining server certificates](http://pubs.vmware.com/vsphere-60/index.jsp#com.vmware.wssdk.dsg.doc/sdk_sg_server_certificate_Appendixes.6.4.html).</span></span>

  <span data-ttu-id="a8da9-166">Pokud máte zabezpečené hranice v rámci vaší organizace a nechcete zapnout protokol HTTPS, použijte následující postup zakázání zabezpečené komunikace.</span><span class="sxs-lookup"><span data-stu-id="a8da9-166">If you have secure boundaries within your organization, and don't want to turn on the HTTPS protocol, use the following procedure to disable the secure communications.</span></span>

### <a name="disable-secure-communication-protocol"></a><span data-ttu-id="a8da9-167">Zakažte zabezpečené komunikační protokol</span><span class="sxs-lookup"><span data-stu-id="a8da9-167">Disable secure communication protocol</span></span>

<span data-ttu-id="a8da9-168">Pokud vaše organizace nevyžaduje protokol HTTPS, použijte následující postup zakázání protokolu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="a8da9-168">If your organization doesn't require the HTTPS protocol, use the following steps to disable HTTPS.</span></span> <span data-ttu-id="a8da9-169">Výchozí chování zakázat, vytvořte klíč registru, který ignoruje výchozí chování.</span><span class="sxs-lookup"><span data-stu-id="a8da9-169">To disable the default behavior, create a registry key that ignores the default behavior.</span></span>

1. <span data-ttu-id="a8da9-170">Zkopírujte a vložte následující text do souboru TXT.</span><span class="sxs-lookup"><span data-stu-id="a8da9-170">Copy and paste the following text into a .txt file.</span></span>

  ```
  Windows Registry Editor Version 5.00
  [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft Data Protection Manager\VMWare]
  "IgnoreCertificateValidation"=dword:00000001
  ```

2. <span data-ttu-id="a8da9-171">Uložte soubor do počítače serveru Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="a8da9-171">Save the file to your Azure Backup Server computer.</span></span> <span data-ttu-id="a8da9-172">Název souboru použijte DisableSecureAuthentication.reg.</span><span class="sxs-lookup"><span data-stu-id="a8da9-172">For the file name, use DisableSecureAuthentication.reg.</span></span>

3. <span data-ttu-id="a8da9-173">Poklikejte na soubor aktivovat položku registru.</span><span class="sxs-lookup"><span data-stu-id="a8da9-173">Double-click the file to activate the registry entry.</span></span>


## <a name="create-a-role-and-user-account-on-the-vcenter-server"></a><span data-ttu-id="a8da9-174">Vytvoření účtu role a uživatele v systému vCenter Server</span><span class="sxs-lookup"><span data-stu-id="a8da9-174">Create a role and user account on the vCenter Server</span></span>

<span data-ttu-id="a8da9-175">Role v systému vCenter Server, je předdefinovanou sadu oprávnění.</span><span class="sxs-lookup"><span data-stu-id="a8da9-175">On the vCenter Server, a role is a predefined set of privileges.</span></span> <span data-ttu-id="a8da9-176">Správce serveru vCenter vytvoří role.</span><span class="sxs-lookup"><span data-stu-id="a8da9-176">A vCenter Server administrator creates the roles.</span></span> <span data-ttu-id="a8da9-177">Přiřazení oprávnění správce páry uživatelské účty s rolí.</span><span class="sxs-lookup"><span data-stu-id="a8da9-177">To assign permissions, the administrator pairs user accounts with a role.</span></span> <span data-ttu-id="a8da9-178">Zavést potřebné uživatelské přihlašovací údaje k zálohování do počítače serveru vCenter, vytvořit roli s konkrétní oprávnění a pak přidružit uživatelský účet k roli.</span><span class="sxs-lookup"><span data-stu-id="a8da9-178">To establish the necessary user credentials to back up the vCenter Server computer, create a role with specific privileges, and then associate the user account with the role.</span></span>

<span data-ttu-id="a8da9-179">Azure Backup Server používá uživatelské jméno a heslo k ověření pomocí systému vCenter Server.</span><span class="sxs-lookup"><span data-stu-id="a8da9-179">Azure Backup Server uses a username and password to authenticate with the vCenter Server.</span></span> <span data-ttu-id="a8da9-180">Azure Backup Server používá tyto přihlašovací údaje jako ověřování pro všechny operace zálohování.</span><span class="sxs-lookup"><span data-stu-id="a8da9-180">Azure Backup Server uses these credentials as authentication for all backup operations.</span></span>

<span data-ttu-id="a8da9-181">Přidání role serveru vCenter a jeho oprávnění pro správce a zálohování:</span><span class="sxs-lookup"><span data-stu-id="a8da9-181">To add a vCenter Server role and its privileges for a backup administrator:</span></span>

1. <span data-ttu-id="a8da9-182">Přihlaste se k systému vCenter Server a pak v systému vCenter Server **Navigátor** panelu, klikněte na tlačítko **správy**.</span><span class="sxs-lookup"><span data-stu-id="a8da9-182">Sign in to the vCenter Server, and then in the vCenter Server **Navigator** panel, click **Administration**.</span></span>

    ![Možnost správy panelu Navigátor serveru vCenter](./media/backup-azure-backup-server-vmware/vmware-navigator-panel.png)

2. <span data-ttu-id="a8da9-184">V **správy** vyberte **role**a potom v **role** panelu klikněte na ikonu Přidat role (+ symbol).</span><span class="sxs-lookup"><span data-stu-id="a8da9-184">In **Administration** select **Roles**, and then in the **Roles** panel click the add role icon (the + symbol).</span></span>

    ![Přidání role](./media/backup-azure-backup-server-vmware/vmware-define-new-role.png)

    <span data-ttu-id="a8da9-186">**Vytvořit roli** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a8da9-186">The **Create Role** dialog box appears.</span></span>

    ![Vytvoření role](./media/backup-azure-backup-server-vmware/vmware-define-new-role-priv.png)

3. <span data-ttu-id="a8da9-188">V **vytvořit roli** dialogu **název Role** zadejte *BackupAdminRole*.</span><span class="sxs-lookup"><span data-stu-id="a8da9-188">In the **Create Role** dialog box, in the **Role name** box, enter *BackupAdminRole*.</span></span> <span data-ttu-id="a8da9-189">Název role může být vám líbí, ale měla by být rozpoznatelném za účelem role.</span><span class="sxs-lookup"><span data-stu-id="a8da9-189">The role name can be whatever you like, but it should be recognizable for the role's purpose.</span></span>

4. <span data-ttu-id="a8da9-190">Vyberte oprávnění pro příslušnou verzi vCenter a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="a8da9-190">Select the privileges for the appropriate version of vCenter, and then click **OK**.</span></span> <span data-ttu-id="a8da9-191">V následující tabulce jsou uvedeny požadovaná oprávnění pro vCenter 6.0 a vCenter 5.5.</span><span class="sxs-lookup"><span data-stu-id="a8da9-191">The following table identifies the required privileges for vCenter 6.0 and vCenter 5.5.</span></span>

  <span data-ttu-id="a8da9-192">Když vyberete oprávnění, klikněte na ikonu vedle nadřazený štítek rozbalit nadřazené a podřízené oprávnění zobrazit.</span><span class="sxs-lookup"><span data-stu-id="a8da9-192">When you select the privileges, click the icon next to the parent label to expand the parent and view the child privileges.</span></span> <span data-ttu-id="a8da9-193">Pokud chcete vybrat oprávnění virtuální počítač, budete muset přejít několik úrovní do hierarchie nadřazený-podřízený.</span><span class="sxs-lookup"><span data-stu-id="a8da9-193">To select the VirtualMachine privileges, you need to go several levels into the parent child hierarchy.</span></span> <span data-ttu-id="a8da9-194">Nemusíte vyberte všechny podřízené oprávnění v rámci nadřazené oprávnění.</span><span class="sxs-lookup"><span data-stu-id="a8da9-194">You don't need to select all child privileges within a parent privilege.</span></span>

  ![Oprávnění hierarchie nadřazený-podřízený](./media/backup-azure-backup-server-vmware/cert-add-privilege-expand.png)

  <span data-ttu-id="a8da9-196">Po kliknutí na tlačítko **OK**, nová role se zobrazí v seznamu na panelu role.</span><span class="sxs-lookup"><span data-stu-id="a8da9-196">After you click **OK**, the new role appears in the list on the Roles panel.</span></span>

|<span data-ttu-id="a8da9-197">Oprávnění pro vCenter 6.0</span><span class="sxs-lookup"><span data-stu-id="a8da9-197">Privileges for vCenter 6.0</span></span>| <span data-ttu-id="a8da9-198">Oprávnění pro vCenter 5,5</span><span class="sxs-lookup"><span data-stu-id="a8da9-198">Privileges for vCenter 5.5</span></span>|
|--------------------------|---------------------------|
|<span data-ttu-id="a8da9-199">Datastore.AllocateSpace</span><span class="sxs-lookup"><span data-stu-id="a8da9-199">Datastore.AllocateSpace</span></span>   | <span data-ttu-id="a8da9-200">Datastore.AllocateSpace</span><span class="sxs-lookup"><span data-stu-id="a8da9-200">Datastore.AllocateSpace</span></span>|
|<span data-ttu-id="a8da9-201">Global.ManageCustomFields</span><span class="sxs-lookup"><span data-stu-id="a8da9-201">Global.ManageCustomFields</span></span> | <span data-ttu-id="a8da9-202">Global.ManageCustomerFields</span><span class="sxs-lookup"><span data-stu-id="a8da9-202">Global.ManageCustomerFields</span></span>|
|<span data-ttu-id="a8da9-203">Global.SetCustomFields</span><span class="sxs-lookup"><span data-stu-id="a8da9-203">Global.SetCustomFields</span></span>    |   |
|<span data-ttu-id="a8da9-204">Host.Local.CreateVM</span><span class="sxs-lookup"><span data-stu-id="a8da9-204">Host.Local.CreateVM</span></span>       | <span data-ttu-id="a8da9-205">Network.Assign</span><span class="sxs-lookup"><span data-stu-id="a8da9-205">Network.Assign</span></span> |
|<span data-ttu-id="a8da9-206">Network.Assign</span><span class="sxs-lookup"><span data-stu-id="a8da9-206">Network.Assign</span></span>            |  |
|<span data-ttu-id="a8da9-207">Resource.AssignVMToPool</span><span class="sxs-lookup"><span data-stu-id="a8da9-207">Resource.AssignVMToPool</span></span>   |  |
|<span data-ttu-id="a8da9-208">VirtualMachine.Config.AddNewDisk</span><span class="sxs-lookup"><span data-stu-id="a8da9-208">VirtualMachine.Config.AddNewDisk</span></span>  | <span data-ttu-id="a8da9-209">VirtualMachine.Config.AddNewDisk</span><span class="sxs-lookup"><span data-stu-id="a8da9-209">VirtualMachine.Config.AddNewDisk</span></span>   |
|<span data-ttu-id="a8da9-210">VirtualMachine.Config.AdvanceConfig</span><span class="sxs-lookup"><span data-stu-id="a8da9-210">VirtualMachine.Config.AdvanceConfig</span></span>| <span data-ttu-id="a8da9-211">VirtualMachine.Config.AdvancedConfig</span><span class="sxs-lookup"><span data-stu-id="a8da9-211">VirtualMachine.Config.AdvancedConfig</span></span>|
|<span data-ttu-id="a8da9-212">VirtualMachine.Config.ChangeTracking</span><span class="sxs-lookup"><span data-stu-id="a8da9-212">VirtualMachine.Config.ChangeTracking</span></span>| <span data-ttu-id="a8da9-213">VirtualMachine.Config.ChangeTracking</span><span class="sxs-lookup"><span data-stu-id="a8da9-213">VirtualMachine.Config.ChangeTracking</span></span> |
|<span data-ttu-id="a8da9-214">VirtualMachine.Config.HostUSBDevice</span><span class="sxs-lookup"><span data-stu-id="a8da9-214">VirtualMachine.Config.HostUSBDevice</span></span>||
|<span data-ttu-id="a8da9-215">VirtualMachine.Config.QueryUnownedFiles</span><span class="sxs-lookup"><span data-stu-id="a8da9-215">VirtualMachine.Config.QueryUnownedFiles</span></span>|    |
|<span data-ttu-id="a8da9-216">VirtualMachine.Config.SwapPlacement</span><span class="sxs-lookup"><span data-stu-id="a8da9-216">VirtualMachine.Config.SwapPlacement</span></span>| <span data-ttu-id="a8da9-217">VirtualMachine.Config.SwapPlacement</span><span class="sxs-lookup"><span data-stu-id="a8da9-217">VirtualMachine.Config.SwapPlacement</span></span> |
|<span data-ttu-id="a8da9-218">VirtualMachine.Interact.PowerOff</span><span class="sxs-lookup"><span data-stu-id="a8da9-218">VirtualMachine.Interact.PowerOff</span></span>| <span data-ttu-id="a8da9-219">VirtualMachine.Interact.PowerOff</span><span class="sxs-lookup"><span data-stu-id="a8da9-219">VirtualMachine.Interact.PowerOff</span></span> |
|<span data-ttu-id="a8da9-220">VirtualMachine.Inventory.Create</span><span class="sxs-lookup"><span data-stu-id="a8da9-220">VirtualMachine.Inventory.Create</span></span>| <span data-ttu-id="a8da9-221">VirtualMachine.Inventory.Create</span><span class="sxs-lookup"><span data-stu-id="a8da9-221">VirtualMachine.Inventory.Create</span></span> |
|<span data-ttu-id="a8da9-222">VirtualMachine.Provisioning.DiskRandomAccess</span><span class="sxs-lookup"><span data-stu-id="a8da9-222">VirtualMachine.Provisioning.DiskRandomAccess</span></span>| |
|<span data-ttu-id="a8da9-223">VirtualMachine.Provisioning.DiskRandomRead</span><span class="sxs-lookup"><span data-stu-id="a8da9-223">VirtualMachine.Provisioning.DiskRandomRead</span></span>|<span data-ttu-id="a8da9-224">VirtualMachine.Provisioning.DiskRandomRead</span><span class="sxs-lookup"><span data-stu-id="a8da9-224">VirtualMachine.Provisioning.DiskRandomRead</span></span> |
|<span data-ttu-id="a8da9-225">VirtualMachine.State.CreateSnapshot</span><span class="sxs-lookup"><span data-stu-id="a8da9-225">VirtualMachine.State.CreateSnapshot</span></span>| <span data-ttu-id="a8da9-226">VirtualMachine.State.CreateSnapshot</span><span class="sxs-lookup"><span data-stu-id="a8da9-226">VirtualMachine.State.CreateSnapshot</span></span>|
|<span data-ttu-id="a8da9-227">VirtualMachine.State.RemoveSnapshot</span><span class="sxs-lookup"><span data-stu-id="a8da9-227">VirtualMachine.State.RemoveSnapshot</span></span>|<span data-ttu-id="a8da9-228">VirtualMachine.State.RemoveSnapshot</span><span class="sxs-lookup"><span data-stu-id="a8da9-228">VirtualMachine.State.RemoveSnapshot</span></span> |
</br>



## <a name="create-a-vcenter-server-user-account-and-permissions"></a><span data-ttu-id="a8da9-229">Vytvořte uživatelský účet systému vCenter Server a oprávnění</span><span class="sxs-lookup"><span data-stu-id="a8da9-229">Create a vCenter Server user account and permissions</span></span>

<span data-ttu-id="a8da9-230">Po vytvoření role s oprávněními, vytvořte účet uživatele.</span><span class="sxs-lookup"><span data-stu-id="a8da9-230">After the role with privileges is set up, create a user account.</span></span> <span data-ttu-id="a8da9-231">Uživatelský účet má jméno a heslo, které poskytuje pověření, které se používají pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="a8da9-231">The user account has a name and password, which provides the credentials that are used for authentication.</span></span>

1. <span data-ttu-id="a8da9-232">Chcete-li vytvořit účet uživatele v systému vCenter Server **Navigátor** panelu, klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="a8da9-232">To create a user account, in the vCenter Server **Navigator** panel, click **Users and Groups**.</span></span>

    ![Možnost uživatelé a skupiny](./media/backup-azure-backup-server-vmware/vmware-userandgroup-panel.png)

    <span data-ttu-id="a8da9-234">**VCenter uživatelů a skupin** se zobrazí panel.</span><span class="sxs-lookup"><span data-stu-id="a8da9-234">The **vCenter Users and Groups** panel appears.</span></span>

    ![panel vCenter uživatelů a skupin](./media/backup-azure-backup-server-vmware/usersandgroups.png)

2. <span data-ttu-id="a8da9-236">V **vCenter uživatelů a skupin** panel, vyberte **uživatelé** a pak klikněte na ikonu Přidat uživatele (+ symbol).</span><span class="sxs-lookup"><span data-stu-id="a8da9-236">In the **vCenter Users and Groups** panel, select the **Users** tab, and then click the add users icon (the + symbol).</span></span>

    <span data-ttu-id="a8da9-237">**Nového uživatele** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a8da9-237">The **New User** dialog box appears.</span></span>

3. <span data-ttu-id="a8da9-238">V **nového uživatele** dialogové okno pole, přidejte informace o uživateli a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="a8da9-238">In the **New User** dialog box, add the user's information and then click **OK**.</span></span> <span data-ttu-id="a8da9-239">V tomto postupu je uživatelské jméno BackupAdmin.</span><span class="sxs-lookup"><span data-stu-id="a8da9-239">In this procedure, the username is BackupAdmin.</span></span>

    ![Dialogové okno Nový uživatel](./media/backup-azure-backup-server-vmware/vmware-new-user-account.png)

    <span data-ttu-id="a8da9-241">V seznamu se zobrazí nový uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="a8da9-241">The new user account appears in the list.</span></span>

4. <span data-ttu-id="a8da9-242">Chcete-li přidružit uživatelský účet s rolí, v **Navigátor** panelu, klikněte na tlačítko **globální oprávnění**.</span><span class="sxs-lookup"><span data-stu-id="a8da9-242">To associate the user account with the role, in the **Navigator** panel, click **Global Permissions**.</span></span> <span data-ttu-id="a8da9-243">V **globální oprávnění** panel, vyberte **spravovat** a pak klikněte na ikonu Přidat (+ symbol).</span><span class="sxs-lookup"><span data-stu-id="a8da9-243">In the **Global Permissions** panel, select the **Manage** tab, and then click the add icon (the + symbol).</span></span>

    ![Globální oprávnění panely](./media/backup-azure-backup-server-vmware/vmware-add-new-perms.png)

    <span data-ttu-id="a8da9-245">**Globální oprávnění Root - přidat oprávnění** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a8da9-245">The **Global Permissions Root - Add Permission** dialog box appears.</span></span>

5. <span data-ttu-id="a8da9-246">V **globální oprávnění Root - přidat oprávnění** dialogové okno, klikněte na tlačítko **přidat** vybrat uživatele nebo skupinu.</span><span class="sxs-lookup"><span data-stu-id="a8da9-246">In the **Global Permission Root - Add Permission** dialog box, click **Add** to choose the user or group.</span></span>

    ![Vyberte uživatele nebo skupinu](./media/backup-azure-backup-server-vmware/vmware-add-new-global-perm.png)

    <span data-ttu-id="a8da9-248">**Vyberte uživatele nebo skupiny** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a8da9-248">The **Select Users/Groups** dialog box appears.</span></span>

6. <span data-ttu-id="a8da9-249">V **vyberte uživatele nebo skupiny** dialogovém okně vyberte **BackupAdmin** a pak klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="a8da9-249">In the **Select Users/Groups** dialog box, choose **BackupAdmin** and then click **Add**.</span></span>

    <span data-ttu-id="a8da9-250">V **uživatelé**, *doména\uživatelské_jméno* formát se používá pro uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="a8da9-250">In **Users**, the *domain\username* format is used for the user account.</span></span> <span data-ttu-id="a8da9-251">Pokud chcete použít jinou doménu, vyberte ho **domény** seznamu.</span><span class="sxs-lookup"><span data-stu-id="a8da9-251">If you want to use a different domain, choose it from the **Domain** list.</span></span>

    ![Přidat uživatele BackupAdmin](./media/backup-azure-backup-server-vmware/vmware-assign-account-to-role.png)

    <span data-ttu-id="a8da9-253">Klikněte na tlačítko **OK** přidání vybraných uživatelů k **přidat oprávnění** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a8da9-253">Click **OK** to add the selected users to the **Add Permission** dialog box.</span></span>

7. <span data-ttu-id="a8da9-254">Teď, když jste zformulovali uživatele, přiřadíte uživatele k roli.</span><span class="sxs-lookup"><span data-stu-id="a8da9-254">Now that you've identified the user, assign the user to the role.</span></span> <span data-ttu-id="a8da9-255">V **přiřadit Role**, v rozevíracím seznamu vyberte **BackupAdminRole**a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="a8da9-255">In **Assigned Role**, from the drop-down list, select **BackupAdminRole**, and then click **OK**.</span></span>

    ![Přiřadit uživatele k roli](./media/backup-azure-backup-server-vmware/vmware-choose-role.png)

  <span data-ttu-id="a8da9-257">Na **spravovat** ve **globální oprávnění** panelů, nový uživatelský účet a přidružené role, které jsou uvedeny v tomto seznamu.</span><span class="sxs-lookup"><span data-stu-id="a8da9-257">On the **Manage** tab in the **Global Permissions** panel, the new user account and the associated role appear in the list.</span></span>


## <a name="establish-vcenter-server-credentials-on-azure-backup-server"></a><span data-ttu-id="a8da9-258">Vytvořit přihlašovací údaje serveru vCenter na serveru Azure Backup</span><span class="sxs-lookup"><span data-stu-id="a8da9-258">Establish vCenter Server credentials on Azure Backup Server</span></span>

<span data-ttu-id="a8da9-259">Než přidáte VMware server na serveru Azure Backup, nainstalovat [aktualizace 1 pro Azure Backup Server](https://support.microsoft.com/help/3175529/update-1-for-microsoft-azure-backup-server).</span><span class="sxs-lookup"><span data-stu-id="a8da9-259">Before you add the VMware server to Azure Backup Server, install [Update 1 for Azure Backup Server](https://support.microsoft.com/help/3175529/update-1-for-microsoft-azure-backup-server).</span></span>

1. <span data-ttu-id="a8da9-260">Otevřete Azure Backup Server, dvakrát klikněte na ikonu na ploše serveru Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="a8da9-260">To open Azure Backup Server, double-click the icon on the Azure Backup Server desktop.</span></span>

    ![Zálohování serveru Azure – ikona](./media/backup-azure-backup-server-vmware/mabs-icon.png)

    <span data-ttu-id="a8da9-262">Pokud nemůžete najít ikony na ploše, otevřete Azure Backup Server ze seznamu nainstalovaných aplikací.</span><span class="sxs-lookup"><span data-stu-id="a8da9-262">If you can't find the icon on the desktop, open Azure Backup Server from the list of installed apps.</span></span> <span data-ttu-id="a8da9-263">Název aplikace Azure Backup Server se nazývá Microsoft Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="a8da9-263">The Azure Backup Server app name is called Microsoft Azure Backup.</span></span>

2. <span data-ttu-id="a8da9-264">V konzole serveru Azure Backup, klikněte na **správy**, klikněte na tlačítko **produkční servery**a pak na pásu karet nástroje klikněte na **spravovat VMware**.</span><span class="sxs-lookup"><span data-stu-id="a8da9-264">In the Azure Backup Server console, click **Management**, click **Production Servers**, and then on the tool ribbon, click **Manage VMware**.</span></span>

    ![Azure Backup Server konzoly](./media/backup-azure-backup-server-vmware/add-vmware-credentials.png)

    <span data-ttu-id="a8da9-266">**Spravovat pověření** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a8da9-266">The **Manage Credentials** dialog box appears.</span></span>

    ![Dialogové okno Azure Backup Server spravovat pověření](./media/backup-azure-backup-server-vmware/mabs-manage-credentials-dialog.png)

3. <span data-ttu-id="a8da9-268">V **spravovat pověření** dialogové okno, klikněte na tlačítko **přidat** otevřete **přidat přihlašovací údaje** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a8da9-268">In the **Manage Credentials** dialog box, click **Add** to open the **Add Credential** dialog box.</span></span>

4. <span data-ttu-id="a8da9-269">V **přidat přihlašovací údaje** dialogovém okně zadejte název a popis nové přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="a8da9-269">In the **Add Credential** dialog box, enter a name and a description for the new credential.</span></span> <span data-ttu-id="a8da9-270">Zadejte uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="a8da9-270">Then specify the username and password.</span></span> <span data-ttu-id="a8da9-271">Název, *přihlašovacích údajů Contoso Vcenter* slouží k identifikaci přihlašovací údaje v dalším postupu.</span><span class="sxs-lookup"><span data-stu-id="a8da9-271">The name, *Contoso Vcenter credential* is used to identify the credential in the next procedure.</span></span> <span data-ttu-id="a8da9-272">Použijte stejné uživatelské jméno a heslo, které se používá pro vCenter Server.</span><span class="sxs-lookup"><span data-stu-id="a8da9-272">Use the same username and password that is used for the vCenter Server.</span></span> <span data-ttu-id="a8da9-273">Pokud vCenter Server a Azure Backup Server nejsou ve stejné doméně, v **uživatelské jméno**, zadejte doménu.</span><span class="sxs-lookup"><span data-stu-id="a8da9-273">If the vCenter Server and Azure Backup Server are not in the same domain, in **User name**, specify the domain.</span></span>

    ![Dialogové okno Azure Backup Server přidat přihlašovací údaje](./media/backup-azure-backup-server-vmware/mabs-add-credential-dialog2.png)

    <span data-ttu-id="a8da9-275">Klikněte na tlačítko **přidat** pro přidání nových přihlašovacích údajů do serveru Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="a8da9-275">Click **Add** to add the new credential to Azure Backup Server.</span></span> <span data-ttu-id="a8da9-276">Nových přihlašovacích údajů se zobrazí v seznamu **spravovat pověření** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a8da9-276">The new credential appears in the list in the **Manage Credentials** dialog box.</span></span>
    
    ![Dialogové okno Azure Backup Server spravovat pověření](./media/backup-azure-backup-server-vmware/new-list-of-mabs-creds.png)

5. <span data-ttu-id="a8da9-278">Zavřete **spravovat pověření** dialogové okno, klikněte **X** v pravém horním rohu.</span><span class="sxs-lookup"><span data-stu-id="a8da9-278">To close the **Manage Credentials** dialog box, click the **X** in the upper-right corner.</span></span>


## <a name="add-the-vcenter-server-to-azure-backup-server"></a><span data-ttu-id="a8da9-279">Přidání systému vCenter Server do serveru Azure Backup</span><span class="sxs-lookup"><span data-stu-id="a8da9-279">Add the vCenter Server to Azure Backup Server</span></span>

<span data-ttu-id="a8da9-280">Průvodce přidání provozního serveru se používá k přidání systému vCenter Server na serveru Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="a8da9-280">Production Server Addition Wizard is used to add the vCenter Server to Azure Backup Server.</span></span>

<span data-ttu-id="a8da9-281">Chcete-li spustit Průvodce přidání provozním serveru, proveďte následující postup:</span><span class="sxs-lookup"><span data-stu-id="a8da9-281">To open Production Server Addition Wizard, complete the following procedure:</span></span>

1. <span data-ttu-id="a8da9-282">V konzole serveru Azure Backup, klikněte na **správy**, klikněte na tlačítko **produkční servery**a potom klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="a8da9-282">In the Azure Backup Server console, click **Management**, click **Production Servers**, and then click **Add**.</span></span>

    ![Průvodce přidání serveru otevřete výroby](./media/backup-azure-backup-server-vmware/add-vcenter-to-mabs.png)

    <span data-ttu-id="a8da9-284">**Průvodce přidání serveru produkční** zobrazí se dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a8da9-284">The **Production Server Addition Wizard** dialog box appears.</span></span>

    ![Průvodce přidání provozním serveru](./media/backup-azure-backup-server-vmware/production-server-add-wizard.png)

2. <span data-ttu-id="a8da9-286">Na **provozním serveru vyberte typ** vyberte **servery VMware**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="a8da9-286">On the **Select Production Server type** page, select **VMware Servers**, and then click **Next**.</span></span>

3. <span data-ttu-id="a8da9-287">V **název nebo IP adresa serveru**, zadejte plně kvalifikovaný název domény (FQDN) nebo IP adresa serveru VMware.</span><span class="sxs-lookup"><span data-stu-id="a8da9-287">In **Server Name/IP Address**, specify the fully qualified domain name (FQDN) or IP address of the VMware server.</span></span> <span data-ttu-id="a8da9-288">Pokud stejný vCenter spravuje všechny servery ESXi, můžete název vCenter.</span><span class="sxs-lookup"><span data-stu-id="a8da9-288">If all the ESXi servers are managed by the same vCenter, you can use the vCenter name.</span></span>

    ![Zadejte plně kvalifikovaný název domény nebo IP adresa serveru VMware](./media/backup-azure-backup-server-vmware/add-vmware-server-provide-server-name.png)

4. <span data-ttu-id="a8da9-290">V **SSL Port**, zadejte port, který se používá ke komunikaci se serverem VMware.</span><span class="sxs-lookup"><span data-stu-id="a8da9-290">In **SSL Port**, enter the port that is used to communicate with the VMware server.</span></span> <span data-ttu-id="a8da9-291">Použijte port 443, který je výchozím portem, pokud si nejste jisti, že jiný port je povinný.</span><span class="sxs-lookup"><span data-stu-id="a8da9-291">Use port 443, which is the default port, unless you know that a different port is required.</span></span>

5. <span data-ttu-id="a8da9-292">V **zadejte přihlašovací údaje**, vyberte pověření, které jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="a8da9-292">In **Specify Credential**, select the credential that you created earlier.</span></span>

    ![Zadejte přihlašovací údaje](./media/backup-azure-backup-server-vmware/identify-creds.png)

6. <span data-ttu-id="a8da9-294">Klikněte na tlačítko **přidat** přidat VMware server do seznamu **přidat servery VMware**a potom klikněte na **Další** pro přesun na další stránku průvodce.</span><span class="sxs-lookup"><span data-stu-id="a8da9-294">Click **Add** to add the VMware server to the list of **Added VMware Servers**, and then click **Next** to move to the next page in the wizard.</span></span>

    ![Přidání serveru VMWare a přihlašovacích údajů](./media/backup-azure-backup-server-vmware/add-vmware-server-credentials.png)

7. <span data-ttu-id="a8da9-296">V **Souhrn** klikněte na tlačítko **přidat** přidat zadaný server VMware do serveru Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="a8da9-296">In the **Summary** page, click **Add** to add the specified VMware server to Azure Backup Server.</span></span>

    ![Přidat VMware server na serveru Azure Backup](./media/backup-azure-backup-server-vmware/tasks-screen.png)

  <span data-ttu-id="a8da9-298">Zálohování serveru VMware je bez agenta zálohování a okamžitě přidá nový server.</span><span class="sxs-lookup"><span data-stu-id="a8da9-298">The VMware server backup is an agentless backup, and the new server is added immediately.</span></span> <span data-ttu-id="a8da9-299">**Dokončit** stránce se zobrazují výsledky.</span><span class="sxs-lookup"><span data-stu-id="a8da9-299">The **Finish** page shows you the results.</span></span>

  ![Stránku](./media/backup-azure-backup-server-vmware/summary-screen.png)

  <span data-ttu-id="a8da9-301">Chcete-li více instancí systému vCenter Server přidat do serveru Azure Backup, opakujte předchozí kroky v této části.</span><span class="sxs-lookup"><span data-stu-id="a8da9-301">To add multiple instances of vCenter Server to Azure Backup Server, repeat the previous steps in this section.</span></span>

<span data-ttu-id="a8da9-302">Po přidání systému vCenter Server do serveru Azure Backup, dalším krokem je vytvoření skupiny ochrany.</span><span class="sxs-lookup"><span data-stu-id="a8da9-302">After you add the vCenter Server to Azure Backup Server, the next step is to create a protection group.</span></span> <span data-ttu-id="a8da9-303">Skupiny ochrany určuje různé podrobnosti krátký nebo dlouhodobé uchovávání a je kde definovat a aplikovat zásady zálohování.</span><span class="sxs-lookup"><span data-stu-id="a8da9-303">The protection group specifies the various details for short or long-term retention, and it is where you define and apply the backup policy.</span></span> <span data-ttu-id="a8da9-304">Zásady zálohování je plán, když dojde k zálohování, a co se zálohuje.</span><span class="sxs-lookup"><span data-stu-id="a8da9-304">The backup policy is the schedule for when backups occur, and what is backed up.</span></span>


## <a name="configure-a-protection-group"></a><span data-ttu-id="a8da9-305">Konfiguraci skupiny ochrany</span><span class="sxs-lookup"><span data-stu-id="a8da9-305">Configure a protection group</span></span>

<span data-ttu-id="a8da9-306">Pokud nepoužijete System Center Data Protection Manager nebo serveru Azure Backup před, přečtěte si téma [plánování zálohování na disk](https://technet.microsoft.com/library/hh758026.aspx) při přípravě svého prostředí hardwaru.</span><span class="sxs-lookup"><span data-stu-id="a8da9-306">If you have not used System Center Data Protection Manager or Azure Backup Server before, see [Plan for disk backups](https://technet.microsoft.com/library/hh758026.aspx) to prepare your hardware environment.</span></span> <span data-ttu-id="a8da9-307">Po můžete zkontrolovat, zda máte správné úložiště, použijte Průvodce vytvořením nové skupiny ochrany přidat virtuální počítače VMware.</span><span class="sxs-lookup"><span data-stu-id="a8da9-307">After you check that you have proper storage, use the Create New Protection Group wizard to add VMware virtual machines.</span></span>

1. <span data-ttu-id="a8da9-308">V konzole serveru Azure Backup, klikněte na **ochrany**a na pásu karet nástroje klikněte na tlačítko **nový** otevřete Průvodce vytvořením nové skupiny ochrany.</span><span class="sxs-lookup"><span data-stu-id="a8da9-308">In the Azure Backup Server console, click **Protection**, and in the tool ribbon, click **New** to open the Create New Protection Group wizard.</span></span>

    ![Otevřete Průvodce vytvořením nové skupiny ochrany](./media/backup-azure-backup-server-vmware/open-protection-wizard.png)

    <span data-ttu-id="a8da9-310">**Vytvořením nové skupiny ochrany** se zobrazí dialogové okno průvodce.</span><span class="sxs-lookup"><span data-stu-id="a8da9-310">The **Create New Protection Group** wizard dialog box appears.</span></span>

    ![Dialogové okno Průvodce vytvořením nové skupiny ochrany](./media/backup-azure-backup-server-vmware/protection-wizard.png)

    <span data-ttu-id="a8da9-312">Klikněte na tlačítko **Další** pro přechod **vybrat typ skupiny ochrany** stránky.</span><span class="sxs-lookup"><span data-stu-id="a8da9-312">Click **Next** to advance to the **Select protection group type** page.</span></span>

2. <span data-ttu-id="a8da9-313">Na **typ skupiny ochrany vyberte** vyberte **servery** a pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="a8da9-313">On the **Select Protection group type** page, select **Servers** and then click **Next**.</span></span> <span data-ttu-id="a8da9-314">**Vybrat členy skupiny** se zobrazí stránka.</span><span class="sxs-lookup"><span data-stu-id="a8da9-314">The **Select group members** page appears.</span></span>

3. <span data-ttu-id="a8da9-315">Na **vybrat členy skupiny** se zobrazí stránka, Dostupní členové a vybrané členy.</span><span class="sxs-lookup"><span data-stu-id="a8da9-315">On the **Select group members** page, the available members and the selected members appear.</span></span> <span data-ttu-id="a8da9-316">Vyberte členy, které chcete chránit a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="a8da9-316">Select the members that you want to protect, and then click **Next**.</span></span>

    ![Vybrat členy skupiny](./media/backup-azure-backup-server-vmware/server-add-selected-members.png)

    <span data-ttu-id="a8da9-318">Když vyberete člena, pokud vyberete složku obsahující jiné složky nebo virtuálních počítačů, budou vybrány také těchto složek a virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="a8da9-318">When you select a member, if you select a folder that contains other folders or VMs, those folders and VMs are also selected.</span></span> <span data-ttu-id="a8da9-319">Zahrnutí složky a virtuální počítače v nadřazené složky se nazývá ochrany na úrovni složek.</span><span class="sxs-lookup"><span data-stu-id="a8da9-319">The inclusion of the folders and VMs in the parent folder is called folder-level protection.</span></span> <span data-ttu-id="a8da9-320">Chcete-li odebrat složku nebo virtuálního počítače, zrušte zaškrtnutí políčka.</span><span class="sxs-lookup"><span data-stu-id="a8da9-320">To remove a folder or VM, clear the check box.</span></span>

    <span data-ttu-id="a8da9-321">Pokud virtuální počítač nebo složku obsahující virtuální počítač, je již chráněné na Azure, nemůžete vybrat tohoto virtuálního počítače znovu.</span><span class="sxs-lookup"><span data-stu-id="a8da9-321">If a VM, or a folder containing a VM, is already protected to Azure, you cannot select that VM again.</span></span> <span data-ttu-id="a8da9-322">To znamená Jakmile je virtuální počítač chráněný do Azure, nemůže být chráněn akci, která zabraňuje pro jeden virtuální počítač se vytváří body obnovení duplicitní.</span><span class="sxs-lookup"><span data-stu-id="a8da9-322">That is, after a VM is protected to Azure, it cannot be protected again, which prevents duplicate recovery points from being created for one VM.</span></span> <span data-ttu-id="a8da9-323">Pokud chcete zobrazit, kterou instanci serveru Azure Backup již chrání členem, přejděte na člen zobrazíte název ochranu serveru.</span><span class="sxs-lookup"><span data-stu-id="a8da9-323">If you want to see which Azure Backup Server instance already protects a member, point to the member to see the name of the protecting server.</span></span>

4. <span data-ttu-id="a8da9-324">Na **vyberte způsob ochrany dat** stránky, zadejte název pro skupinu ochrany.</span><span class="sxs-lookup"><span data-stu-id="a8da9-324">On the **Select Data Protection Method** page, enter a name for the protection group.</span></span> <span data-ttu-id="a8da9-325">Nebyly vybrány krátkodobou ochranu (na disk) a online ochranu.</span><span class="sxs-lookup"><span data-stu-id="a8da9-325">Short-term protection (to disk) and online protection are selected.</span></span> <span data-ttu-id="a8da9-326">Pokud chcete použít online ochrany (do Azure), musíte použít krátkodobou ochranu na disk.</span><span class="sxs-lookup"><span data-stu-id="a8da9-326">If you want to use online protection (to Azure), you must use short-term protection to disk.</span></span> <span data-ttu-id="a8da9-327">Klikněte na tlačítko **Další** pokračovat do rozsahu krátkodobé ochrany.</span><span class="sxs-lookup"><span data-stu-id="a8da9-327">Click **Next** to proceed to the short-term protection range.</span></span>

    ![Vyberte způsob ochrany dat](./media/backup-azure-backup-server-vmware/name-protection-group.png)

5. <span data-ttu-id="a8da9-329">Na **zadat krátkodobé cíle** stránky, pro **rozsah uchování**, zadejte počet dní, které chcete zachovat body obnovení, které jsou *uložené na disk*.</span><span class="sxs-lookup"><span data-stu-id="a8da9-329">On the **Specify Short-Term Goals** page, for **Retention Range**, specify the number of days that you want to retain recovery points that are *stored to disk*.</span></span> <span data-ttu-id="a8da9-330">Pokud chcete změnit čas a dny v případě, že jsou pořizovány body obnovení, klikněte na tlačítko **upravit**.</span><span class="sxs-lookup"><span data-stu-id="a8da9-330">If you want to change the time and days when recovery points are taken, click **Modify**.</span></span> <span data-ttu-id="a8da9-331">Krátkodobé body obnovení jsou úplné zálohy.</span><span class="sxs-lookup"><span data-stu-id="a8da9-331">The short-term recovery points are full backups.</span></span> <span data-ttu-id="a8da9-332">Nejsou přírůstkové zálohy.</span><span class="sxs-lookup"><span data-stu-id="a8da9-332">They are not incremental backups.</span></span> <span data-ttu-id="a8da9-333">Jakmile budete spokojeni s krátkodobé cíle, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="a8da9-333">When you are satisfied with the short-term goals, click **Next**.</span></span>

    ![Určení krátkodobých cílů](./media/backup-azure-backup-server-vmware/short-term-goals.png)

6. <span data-ttu-id="a8da9-335">Na **zkontrolovat přidělení disku** zkontrolujte a v případě potřeby upravte místo na disku pro virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="a8da9-335">On the **Review Disk Allocation** page, review and if necessary, modify the disk space for the VMs.</span></span> <span data-ttu-id="a8da9-336">Doporučená přidělení disku jsou založené na rozsahu uchování, který je uveden v **zadat krátkodobé cíle** vyberte typ úlohy a velikosti chráněných dat (identifikovaného v kroku 3).</span><span class="sxs-lookup"><span data-stu-id="a8da9-336">The recommended disk allocations are based on the retention range that is specified in the **Specify Short-Term Goals** page, the type of workload, and the size of the protected data (identified in step 3).</span></span>  

  - <span data-ttu-id="a8da9-337">**Velikost dat:** velikost dat ve skupině ochrany.</span><span class="sxs-lookup"><span data-stu-id="a8da9-337">**Data size:** Size of the data in the protection group.</span></span>
  - <span data-ttu-id="a8da9-338">**Místo na disku:** doporučená velikost místa na disku pro skupinu ochrany.</span><span class="sxs-lookup"><span data-stu-id="a8da9-338">**Disk space:** The recommended amount of disk space for the protection group.</span></span> <span data-ttu-id="a8da9-339">Pokud chcete toto nastavení změnit, měli byste přidělit celkové místo, které je o něco větší než velikost, kdy očekáváte, že každý zdroj dat roste.</span><span class="sxs-lookup"><span data-stu-id="a8da9-339">If you want to modify this setting, you should allocate total space that is slightly larger than the amount that you estimate each data source grows.</span></span>
  - <span data-ttu-id="a8da9-340">**Společné umístění dat:** při povolení společné umístění, více datového zdroje v oblasti ochrany můžete namapovat na jednu repliku a obnovení bodu svazku.</span><span class="sxs-lookup"><span data-stu-id="a8da9-340">**Colocate data:** If you turn on colocation, multiple data sources in the protection can map to a single replica and recovery point volume.</span></span> <span data-ttu-id="a8da9-341">Společné umístění se nepodporuje pro všechny úlohy.</span><span class="sxs-lookup"><span data-stu-id="a8da9-341">Colocation isn't supported for all workloads.</span></span>
  - <span data-ttu-id="a8da9-342">**Automaticky rozšiřovat:** Pokud povolíte toto nastavení, pokud data ve skupině ochrany přesáhnou počáteční přidělení, System Center Data Protection Manager pokusí a zvyšte velikost disku o 25 procent.</span><span class="sxs-lookup"><span data-stu-id="a8da9-342">**Automatically grow:** If you turn on this setting, if data in the protected group outgrows the initial allocation, System Center Data Protection Manager tries to increase the disk size by 25 percent.</span></span>
  - <span data-ttu-id="a8da9-343">**Podrobnosti o fondu úložiště:** zobrazuje stav fondu úložiště včetně celkové a zbývající velikosti disku.</span><span class="sxs-lookup"><span data-stu-id="a8da9-343">**Storage pool details:** Shows the status of the storage pool, including total and remaining disk size.</span></span>

    ![Zkontrolovat přidělení disku](./media/backup-azure-backup-server-vmware/review-disk-allocation.png)

    <span data-ttu-id="a8da9-345">Jakmile budete spokojeni s přidělení místa, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="a8da9-345">When you are satisfied with the space allocation, click **Next**.</span></span>

7. <span data-ttu-id="a8da9-346">Na **vyberte způsob vytvoření repliky** určete, jak chcete vygenerovat počáteční kopii nebo repliky chráněných dat na serveru Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="a8da9-346">On the **Choose Replica Creation Method** page, specify how you want to generate the initial copy, or replica, of the protected data on Azure Backup Server.</span></span>

    <span data-ttu-id="a8da9-347">Výchozí hodnota je **automaticky přes síť** a **nyní**.</span><span class="sxs-lookup"><span data-stu-id="a8da9-347">The default is **Automatically over the network** and **Now**.</span></span> <span data-ttu-id="a8da9-348">Pokud chcete použít výchozí nastavení, doporučujeme vám, že zadáte dobu mimo špičku.</span><span class="sxs-lookup"><span data-stu-id="a8da9-348">If you use the default, we recommend that you specify an off-peak time.</span></span> <span data-ttu-id="a8da9-349">Zvolte **později** a zadejte den a čas.</span><span class="sxs-lookup"><span data-stu-id="a8da9-349">Choose **Later** and specify a day and time.</span></span>

    <span data-ttu-id="a8da9-350">Pro velké objemy dat nebo méně než optimální síťové podmínky zvažte replikaci dat offline pomocí vyměnitelného média.</span><span class="sxs-lookup"><span data-stu-id="a8da9-350">For large amounts of data or less-than-optimal network conditions, consider replicating the data offline by using removable media.</span></span>

    <span data-ttu-id="a8da9-351">Po provedení volby, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="a8da9-351">After you have made your choices, click **Next**.</span></span>

    ![Vyberte způsob vytvoření repliky](./media/backup-azure-backup-server-vmware/replica-creation.png)

8. <span data-ttu-id="a8da9-353">Na **možnosti kontroly konzistence** stránky, vyberte, jak a kdy automatizovat kontroly konzistence.</span><span class="sxs-lookup"><span data-stu-id="a8da9-353">On the **Consistency Check Options** page, select how and when to automate the consistency checks.</span></span> <span data-ttu-id="a8da9-354">Kontroly konzistence můžete spustit, když se stane nekonzistentní data repliky, nebo podle nastaveného plánu.</span><span class="sxs-lookup"><span data-stu-id="a8da9-354">You can run consistency checks when replica data becomes inconsistent, or on a set schedule.</span></span>

    <span data-ttu-id="a8da9-355">Pokud nechcete konfigurovat automatické kontroly konzistence, můžete spustit ruční kontrolu.</span><span class="sxs-lookup"><span data-stu-id="a8da9-355">If you don't want to configure automatic consistency checks, you can run a manual check.</span></span> <span data-ttu-id="a8da9-356">V oblasti Ochrana serveru Azure Backup konzoly klikněte pravým tlačítkem na skupinu ochrany a pak vyberte **provést kontrolu konzistence**.</span><span class="sxs-lookup"><span data-stu-id="a8da9-356">In the protection area of the Azure Backup Server console, right-click the protection group and then select **Perform Consistency Check**.</span></span>

    <span data-ttu-id="a8da9-357">Klikněte na tlačítko **Další** pro přesun na další stránku.</span><span class="sxs-lookup"><span data-stu-id="a8da9-357">Click **Next** to move to the next page.</span></span>

9. <span data-ttu-id="a8da9-358">Na **zadat Data Online ochrany** vyberte jeden nebo více zdrojů dat, které chcete chránit.</span><span class="sxs-lookup"><span data-stu-id="a8da9-358">On the **Specify Online Protection Data** page, select one or more data sources that you want to protect.</span></span> <span data-ttu-id="a8da9-359">Můžete vybrat členy jednotlivě, nebo klikněte na tlačítko **Vybrat vše** vybrat všechny členy.</span><span class="sxs-lookup"><span data-stu-id="a8da9-359">You can select the members individually, or click **Select All** to choose all members.</span></span> <span data-ttu-id="a8da9-360">Po výběru členů, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="a8da9-360">After you choose the members, click **Next**.</span></span>

    ![Zadejte data pro online ochranu](./media/backup-azure-backup-server-vmware/select-data-to-protect.png)

10. <span data-ttu-id="a8da9-362">Na **zadejte plán Online zálohování** stránky, zadejte plán pro generování bodů obnovení ze zálohy disku.</span><span class="sxs-lookup"><span data-stu-id="a8da9-362">On the **Specify Online Backup Schedule** page, specify the schedule to generate recovery points from the disk backup.</span></span> <span data-ttu-id="a8da9-363">Po vygenerování bod obnovení, bude převedena do trezoru služeb zotavení v Azure.</span><span class="sxs-lookup"><span data-stu-id="a8da9-363">After the recovery point is generated, it is transferred to the Recovery Services vault in Azure.</span></span> <span data-ttu-id="a8da9-364">Jakmile budete spokojeni s plán online zálohování, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="a8da9-364">When you are satisfied with the online backup schedule, click **Next**.</span></span>

    ![Zadejte plán online zálohování](./media/backup-azure-backup-server-vmware/online-backup-schedule.png)

11. <span data-ttu-id="a8da9-366">Na **zadejte zásady uchovávání Online** vyberte, jak dlouho chcete zachovat zálohovaná data v Azure.</span><span class="sxs-lookup"><span data-stu-id="a8da9-366">On the **Specify Online Retention Policy** page, indicate how long you want to retain the backup data in Azure.</span></span> <span data-ttu-id="a8da9-367">Po definování zásad klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="a8da9-367">After the policy is defined, click **Next**.</span></span>

    ![Zadejte zásady online uchovávání dat.](./media/backup-azure-backup-server-vmware/retention-policy.png)

    <span data-ttu-id="a8da9-369">Neexistuje žádný časový limit pro jak dlouho ponechat data v Azure.</span><span class="sxs-lookup"><span data-stu-id="a8da9-369">There is no time limit for how long you can keep data in Azure.</span></span> <span data-ttu-id="a8da9-370">Pokud ukládáte data bodu obnovení v Azure, jenom limit je, že nemůžete mít více než 9999 bodů obnovení za chráněné instance.</span><span class="sxs-lookup"><span data-stu-id="a8da9-370">When you store recovery point data in Azure, the only limit is that you cannot have more than 9999 recovery points per protected instance.</span></span> <span data-ttu-id="a8da9-371">V tomto příkladu je chráněný instance serveru VMware.</span><span class="sxs-lookup"><span data-stu-id="a8da9-371">In this example, the protected instance is the VMware server.</span></span>

12. <span data-ttu-id="a8da9-372">Na **Souhrn** stránka, zkontrolujte podrobnosti pro členy skupiny ochrany a nastavení a pak klikněte na tlačítko **vytvořit skupinu**.</span><span class="sxs-lookup"><span data-stu-id="a8da9-372">On the **Summary** page, review the details for your protection group members and settings, and then click **Create Group**.</span></span>

    ![Souhrn nastavení a člena skupiny ochrany](./media/backup-azure-backup-server-vmware/protection-group-summary.png)

## <a name="next-steps"></a><span data-ttu-id="a8da9-374">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a8da9-374">Next steps</span></span>
<span data-ttu-id="a8da9-375">Pokud používáte Azure Backup Server k ochraně úloh VMware, může zajímat pomocí serveru Azure Backup k ochraně [Microsoft Exchange server](./backup-azure-exchange-mabs.md), [Microsoft SharePoint farmy](./backup-azure-backup-sharepoint-mabs.md), nebo [databáze systému SQL Server](./backup-azure-sql-mabs.md).</span><span class="sxs-lookup"><span data-stu-id="a8da9-375">If you use Azure Backup Server to protect VMware workloads, you may be interested in using Azure Backup Server to help protect a [Microsoft Exchange server](./backup-azure-exchange-mabs.md), a [Microsoft SharePoint farm](./backup-azure-backup-sharepoint-mabs.md), or a [SQL Server database](./backup-azure-sql-mabs.md).</span></span>

<span data-ttu-id="a8da9-376">Podrobnosti o problémech s zaregistrováním agenta naleznete v tématu Konfigurace skupiny ochrany nebo zálohování úloh, [řešení potíží s Azure Backup Server](./backup-azure-mabs-troubleshoot.md).</span><span class="sxs-lookup"><span data-stu-id="a8da9-376">For information on problems with registering the agent, configuring the protection group, or backing up jobs, see [Troubleshoot Azure Backup Server](./backup-azure-mabs-troubleshoot.md).</span></span>
