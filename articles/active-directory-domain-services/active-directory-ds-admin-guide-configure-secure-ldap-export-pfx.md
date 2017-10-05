---
title: "Konfigurace zabezpečeného LDAP (LDAPS) ve službě Azure AD Domain Services | Microsoft Docs"
description: "Konfigurace zabezpečení protokolu LDAP (LDAPS) pro spravované doméně služby Azure AD Domain Services"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: c6da94b6-4328-4230-801a-4b646055d4d7
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: maheshu
ms.openlocfilehash: 5d46f376d46b8bbf3f93de57a7d4e31abdbcdb2f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configure-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a><span data-ttu-id="6b2ba-103">Konfigurace zabezpečeného LDAP (LDAPS) pro spravované doméně služby Azure AD Domain Services</span><span class="sxs-lookup"><span data-stu-id="6b2ba-103">Configure secure LDAP (LDAPS) for an Azure AD Domain Services managed domain</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="6b2ba-104">Než začnete</span><span class="sxs-lookup"><span data-stu-id="6b2ba-104">Before you begin</span></span>
<span data-ttu-id="6b2ba-105">Ujistěte se, když jste dokončili [úloha 1 – získání certifikátu pro zabezpečený LDAP](active-directory-ds-admin-guide-configure-secure-ldap.md).</span><span class="sxs-lookup"><span data-stu-id="6b2ba-105">Ensure you've completed [Task 1 - obtain a certificate for secure LDAP](active-directory-ds-admin-guide-configure-secure-ldap.md).</span></span>


## <a name="task-2---export-the-secure-ldap-certificate-to-a-pfx-file"></a><span data-ttu-id="6b2ba-106">Úloha 2 – zabezpečený LDAP certifikát, který chcete exportovat. Soubor PFX</span><span class="sxs-lookup"><span data-stu-id="6b2ba-106">Task 2 - export the secure LDAP certificate to a .PFX file</span></span>
<span data-ttu-id="6b2ba-107">Než začnete tuto úlohu, ujistěte se, získání certifikátu zabezpečeného LDAP od veřejné certifikační autority nebo jste vytvořili certifikát podepsaný svým držitelem.</span><span class="sxs-lookup"><span data-stu-id="6b2ba-107">Before you start this task, ensure that you have obtained the secure LDAP certificate from a public certification authority or have created a self-signed certificate.</span></span>

<span data-ttu-id="6b2ba-108">Proveďte následující kroky, LDAPS certifikát, který chcete exportovat. Soubor PFX.</span><span class="sxs-lookup"><span data-stu-id="6b2ba-108">Perform the following steps, to export the LDAPS certificate to a .PFX file.</span></span>

1. <span data-ttu-id="6b2ba-109">Stiskněte **spustit** tlačítko a typ **R**. V **spustit** dialogové okno, typ **konzoly mmc** a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="6b2ba-109">Press the **Start** button and type **R**. In the **Run** dialog, type **mmc** and click **OK**.</span></span>

    ![Spustit konzolu MMC](./media/active-directory-domain-services-admin-guide/secure-ldap-start-run.png)
2. <span data-ttu-id="6b2ba-111">Na **řízení uživatelských účtů** výzva, klikněte na tlačítko **Ano** ke spuštění konzoly MMC (Microsoft Management Console) jako správce.</span><span class="sxs-lookup"><span data-stu-id="6b2ba-111">On the **User Account Control** prompt, click **YES** to launch MMC (Microsoft Management Console) as administrator.</span></span>
3. <span data-ttu-id="6b2ba-112">Z **soubor** nabídky, klikněte na tlačítko **přidat nebo odebrat modul Snap-in...** .</span><span class="sxs-lookup"><span data-stu-id="6b2ba-112">From the **File** menu, click **Add/Remove Snap-in...**.</span></span>

    ![Přidání modulu snap-in konzoly MMC](./media/active-directory-domain-services-admin-guide/secure-ldap-add-snapin.png)
4. <span data-ttu-id="6b2ba-114">V **přidat nebo odebrat moduly Snap in** dialogovém okně, vyberte **certifikáty** modul snap-in a klikněte na tlačítko **Přidat >** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6b2ba-114">In the **Add or Remove Snap-ins** dialog, select the **Certificates** snap-in, and click the **Add >** button.</span></span>

    ![Přidání modulu snap-in Certifikáty konzoly MMC](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-snapin.png)
5. <span data-ttu-id="6b2ba-116">V **modul snap-in Certifikáty** průvodci vyberte **účet počítače** a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="6b2ba-116">In the **Certificates snap-in** wizard, select **Computer account** and click **Next**.</span></span>

    ![Přidat modul snap-in Certifikáty pro účet počítače](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-computer-account.png)
6. <span data-ttu-id="6b2ba-118">Na **vybrat počítač** vyberte **místní počítač: (počítač je spuštěna tato konzola)** a klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="6b2ba-118">On the **Select Computer** page, select **Local computer: (the computer this console is running on)** and click **Finish**.</span></span>

    ![Přidat modul snap-in Certifikáty - vyberte počítače](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-local-computer.png)
7. <span data-ttu-id="6b2ba-120">V **přidat nebo odebrat moduly Snap in** dialogové okno, klikněte na tlačítko **OK** přidat modul snap-in Certifikáty konzoly MMC.</span><span class="sxs-lookup"><span data-stu-id="6b2ba-120">In the **Add or Remove Snap-ins** dialog, click **OK** to add the certificates snap-in to MMC.</span></span>

    ![Přidat modul snap-in Certifikáty konzoly MMC - provést](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-snapin-done.png)
8. <span data-ttu-id="6b2ba-122">V okně konzoly MMC kliknutím rozbalte **kořenový adresář konzoly**.</span><span class="sxs-lookup"><span data-stu-id="6b2ba-122">In the MMC window, click to expand **Console Root**.</span></span> <span data-ttu-id="6b2ba-123">Měli byste vidět modulu snap-in Certifikáty načíst.</span><span class="sxs-lookup"><span data-stu-id="6b2ba-123">You should see the Certificates snap-in loaded.</span></span> <span data-ttu-id="6b2ba-124">Klikněte na tlačítko **certifikáty (místní počítač)** rozbalte.</span><span class="sxs-lookup"><span data-stu-id="6b2ba-124">Click **Certificates (Local Computer)** to expand.</span></span> <span data-ttu-id="6b2ba-125">Kliknutím rozbalte položku **osobní** uzlu, za nímž následuje **certifikáty** uzlu.</span><span class="sxs-lookup"><span data-stu-id="6b2ba-125">Click to expand the **Personal** node, followed by the **Certificates** node.</span></span>

    ![Úložiště otevřete osobních certifikátů](./media/active-directory-domain-services-admin-guide/secure-ldap-open-personal-store.png)
9. <span data-ttu-id="6b2ba-127">Měli byste vidět certifikát podepsaný svým držitelem, které jsme vytvořili.</span><span class="sxs-lookup"><span data-stu-id="6b2ba-127">You should see the self-signed certificate we created.</span></span> <span data-ttu-id="6b2ba-128">Můžete prozkoumat vlastnosti certifikátu, ujistěte se, že kryptografický otisk se shoduje s hlášené v prostředí PowerShell systému windows, když jste vytvořili certifikát.</span><span class="sxs-lookup"><span data-stu-id="6b2ba-128">You can examine the properties of the certificate to ensure the thumbprint matches that reported on the PowerShell windows when you created the certificate.</span></span>
10. <span data-ttu-id="6b2ba-129">Vyberte certifikát podepsaný svým držitelem a **klikněte pravým tlačítkem na**.</span><span class="sxs-lookup"><span data-stu-id="6b2ba-129">Select the self-signed certificate and **right click**.</span></span> <span data-ttu-id="6b2ba-130">V místní nabídce vyberte **všechny úlohy** a vyberte **exportovat...** .</span><span class="sxs-lookup"><span data-stu-id="6b2ba-130">From the right-click menu, select **All Tasks** and select **Export...**.</span></span>

    ![Export certifikátu](./media/active-directory-domain-services-admin-guide/secure-ldap-export-cert.png)
11. <span data-ttu-id="6b2ba-132">V **Průvodce exportem certifikátu**, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="6b2ba-132">In the **Certificate Export Wizard**, click **Next**.</span></span>

    ![Průvodce exportem certifikátu](./media/active-directory-domain-services-admin-guide/secure-ldap-export-cert-wizard.png)
12. <span data-ttu-id="6b2ba-134">Na **exportovat soukromý klíč** vyberte **Ano, exportovat soukromý klíč**a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="6b2ba-134">On the **Export Private Key** page, select **Yes, export the private key**, and click **Next**.</span></span>

    ![Exportovat privátní klíč certifikátu](./media/active-directory-domain-services-admin-guide/secure-ldap-export-private-key.png)

    > [!WARNING]
    > <span data-ttu-id="6b2ba-136">Je nutné exportovat privátní klíč společně se certifikát.</span><span class="sxs-lookup"><span data-stu-id="6b2ba-136">You MUST export the private key along with the certificate.</span></span> <span data-ttu-id="6b2ba-137">Pokud zadáte soubor PFX, který neobsahuje privátní klíč pro certifikát, povolení zabezpečeného LDAP vaší spravované domény nezdaří.</span><span class="sxs-lookup"><span data-stu-id="6b2ba-137">If you provide a PFX that does not contain the private key for the certificate, enabling secure LDAP for your managed domain fails.</span></span>
    >
    >
13. <span data-ttu-id="6b2ba-138">Na **formát souboru pro Export** vyberte **Personal Information Exchange – PKCS #12 (. PFX)** jako formát souboru pro exportovaný certifikát.</span><span class="sxs-lookup"><span data-stu-id="6b2ba-138">On the **Export File Format** page, select **Personal Information Exchange - PKCS #12 (.PFX)** as the file format for the exported certificate.</span></span>

    ![Formát souboru pro export certifikátu](./media/active-directory-domain-services-admin-guide/secure-ldap-export-to-pfx.png)

    > [!NOTE]
    > <span data-ttu-id="6b2ba-140">Pouze. Formát souboru PFX je podporován.</span><span class="sxs-lookup"><span data-stu-id="6b2ba-140">Only the .PFX file format is supported.</span></span> <span data-ttu-id="6b2ba-141">Neexportovat certifikát, který chcete. Formát souboru CER.</span><span class="sxs-lookup"><span data-stu-id="6b2ba-141">Do not export the certificate to the .CER file format.</span></span>
    >
    >
14. <span data-ttu-id="6b2ba-142">Na **zabezpečení** vyberte **heslo** možnost a zadejte heslo k ochraně. Soubor PFX.</span><span class="sxs-lookup"><span data-stu-id="6b2ba-142">On the **Security** page, select the **Password** option and type in a password to protect the .PFX file.</span></span> <span data-ttu-id="6b2ba-143">Heslo si zapamatujte, protože bude potřeba v dalším úkolem.</span><span class="sxs-lookup"><span data-stu-id="6b2ba-143">Remember this password since it will be needed in the next task.</span></span> <span data-ttu-id="6b2ba-144">Klikněte na tlačítko **Další** pokračovat.</span><span class="sxs-lookup"><span data-stu-id="6b2ba-144">Click **Next** to proceed.</span></span>

    ![<span data-ttu-id="6b2ba-145">Heslo pro export certifikátu</span><span class="sxs-lookup"><span data-stu-id="6b2ba-145">Password for certificate export</span></span> ](./media/active-directory-domain-services-admin-guide/secure-ldap-export-select-password.png)

    > [!NOTE]
    > <span data-ttu-id="6b2ba-146">Toto heslo si poznamenejte.</span><span class="sxs-lookup"><span data-stu-id="6b2ba-146">Make a note of this password.</span></span> <span data-ttu-id="6b2ba-147">Je třeba při povolování zabezpečený LDAP pro tuto doménu spravovaných v [úloha 3 – povolení zabezpečeného LDAP pro spravované doméně](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)</span><span class="sxs-lookup"><span data-stu-id="6b2ba-147">You need it while enabling secure LDAP for this managed domain in [Task 3 - enable secure LDAP for the managed domain](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)</span></span>
    >
    >
15. <span data-ttu-id="6b2ba-148">Na **soubor pro Export** stránky, zadejte název souboru a umístění, kde chcete exportovat certifikát.</span><span class="sxs-lookup"><span data-stu-id="6b2ba-148">On the **File to Export** page, specify the file name and location where you'd like to export the certificate.</span></span>

    ![Cesta pro export certifikátu](./media/active-directory-domain-services-admin-guide/secure-ldap-export-select-path.png)
16. <span data-ttu-id="6b2ba-150">Na následující stránce, klikněte na tlačítko **Dokončit** a vyexportovat si certifikát do souboru PFX.</span><span class="sxs-lookup"><span data-stu-id="6b2ba-150">On the following page, click **Finish** to export the certificate to a PFX file.</span></span> <span data-ttu-id="6b2ba-151">Pokud byla exportována certifikát, měli byste vidět dialogové okno potvrzení.</span><span class="sxs-lookup"><span data-stu-id="6b2ba-151">You should see confirmation dialog when the certificate has been exported.</span></span>

    ![Export certifikátu provést](./media/active-directory-domain-services-admin-guide/secure-ldap-exported-as-pfx.png)


## <a name="next-step"></a><span data-ttu-id="6b2ba-153">Další krok</span><span class="sxs-lookup"><span data-stu-id="6b2ba-153">Next step</span></span>
[<span data-ttu-id="6b2ba-154">Úloha 3 – povolení zabezpečeného LDAP pro spravované doméně</span><span class="sxs-lookup"><span data-stu-id="6b2ba-154">Task 3 - enable secure LDAP for the managed domain</span></span>](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)
