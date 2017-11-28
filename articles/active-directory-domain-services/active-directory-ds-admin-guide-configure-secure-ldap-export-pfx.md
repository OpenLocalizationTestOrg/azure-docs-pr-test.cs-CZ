---
title: "aaaConfigure zabezpečeného LDAP (LDAPS) v Azure AD Domain Services | Microsoft Docs"
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
ms.openlocfilehash: 356b28f8392b0e203df9c81177ec842d52866c4f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a><span data-ttu-id="b3f48-103">Konfigurace zabezpečeného LDAP (LDAPS) pro spravované doméně služby Azure AD Domain Services</span><span class="sxs-lookup"><span data-stu-id="b3f48-103">Configure secure LDAP (LDAPS) for an Azure AD Domain Services managed domain</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="b3f48-104">Než začnete</span><span class="sxs-lookup"><span data-stu-id="b3f48-104">Before you begin</span></span>
<span data-ttu-id="b3f48-105">Ujistěte se, když jste dokončili [úloha 1 – získání certifikátu pro zabezpečený LDAP](active-directory-ds-admin-guide-configure-secure-ldap.md).</span><span class="sxs-lookup"><span data-stu-id="b3f48-105">Ensure you've completed [Task 1 - obtain a certificate for secure LDAP](active-directory-ds-admin-guide-configure-secure-ldap.md).</span></span>


## <a name="task-2---export-hello-secure-ldap-certificate-tooa-pfx-file"></a><span data-ttu-id="b3f48-106">Úloha 2 – export hello tooa certifikát zabezpečený LDAP. Soubor PFX</span><span class="sxs-lookup"><span data-stu-id="b3f48-106">Task 2 - export hello secure LDAP certificate tooa .PFX file</span></span>
<span data-ttu-id="b3f48-107">Než začnete tuto úlohu, ujistěte se, získání certifikátu hello zabezpečený LDAP od veřejné certifikační autority nebo jste vytvořili certifikát podepsaný svým držitelem.</span><span class="sxs-lookup"><span data-stu-id="b3f48-107">Before you start this task, ensure that you have obtained hello secure LDAP certificate from a public certification authority or have created a self-signed certificate.</span></span>

<span data-ttu-id="b3f48-108">Proveďte následující kroky hello, tooexport hello LDAPS certifikátu tooa. Soubor PFX.</span><span class="sxs-lookup"><span data-stu-id="b3f48-108">Perform hello following steps, tooexport hello LDAPS certificate tooa .PFX file.</span></span>

1. <span data-ttu-id="b3f48-109">Stiskněte klávesu hello **spustit** tlačítko a typ **R**. V hello **spustit** dialogové okno, typ **konzoly mmc** a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="b3f48-109">Press hello **Start** button and type **R**. In hello **Run** dialog, type **mmc** and click **OK**.</span></span>

    ![Spusťte konzolu MMC hello](./media/active-directory-domain-services-admin-guide/secure-ldap-start-run.png)
2. <span data-ttu-id="b3f48-111">Na hello **řízení uživatelských účtů** výzva, klikněte na tlačítko **Ano** toolaunch konzoly MMC (Microsoft Management Console) jako správce.</span><span class="sxs-lookup"><span data-stu-id="b3f48-111">On hello **User Account Control** prompt, click **YES** toolaunch MMC (Microsoft Management Console) as administrator.</span></span>
3. <span data-ttu-id="b3f48-112">Z hello **soubor** nabídky, klikněte na tlačítko **přidat nebo odebrat modul Snap-in...** .</span><span class="sxs-lookup"><span data-stu-id="b3f48-112">From hello **File** menu, click **Add/Remove Snap-in...**.</span></span>

    ![Přidat modul snap-in tooMMC konzoly](./media/active-directory-domain-services-admin-guide/secure-ldap-add-snapin.png)
4. <span data-ttu-id="b3f48-114">V hello **přidat nebo odebrat moduly Snap in** dialogové okno, vyberte hello **certifikáty** modul snap-in a klikněte na tlačítko hello **Přidat >** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b3f48-114">In hello **Add or Remove Snap-ins** dialog, select hello **Certificates** snap-in, and click hello **Add >** button.</span></span>

    ![Přidat konzole tooMMC modul snap-in Certifikáty](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-snapin.png)
5. <span data-ttu-id="b3f48-116">V hello **modul snap-in Certifikáty** průvodci vyberte **účet počítače** a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="b3f48-116">In hello **Certificates snap-in** wizard, select **Computer account** and click **Next**.</span></span>

    ![Přidat modul snap-in Certifikáty pro účet počítače](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-computer-account.png)
6. <span data-ttu-id="b3f48-118">Na hello **vybrat počítač** vyberte **místní počítač: (hello počítači je spuštěna tato konzola)** a klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="b3f48-118">On hello **Select Computer** page, select **Local computer: (hello computer this console is running on)** and click **Finish**.</span></span>

    ![Přidat modul snap-in Certifikáty - vyberte počítače](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-local-computer.png)
7. <span data-ttu-id="b3f48-120">V hello **přidat nebo odebrat moduly Snap in** dialogové okno, klikněte na tlačítko **OK** tooadd hello certifikátů modulu snap-in tooMMC.</span><span class="sxs-lookup"><span data-stu-id="b3f48-120">In hello **Add or Remove Snap-ins** dialog, click **OK** tooadd hello certificates snap-in tooMMC.</span></span>

    ![Přidejte certifikáty modul snap-in tooMMC - v](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-snapin-done.png)
8. <span data-ttu-id="b3f48-122">V okně konzoly MMC hello klikněte tooexpand **kořenový adresář konzoly**.</span><span class="sxs-lookup"><span data-stu-id="b3f48-122">In hello MMC window, click tooexpand **Console Root**.</span></span> <span data-ttu-id="b3f48-123">Měli byste vidět načíst hello modul snap-in Certifikáty.</span><span class="sxs-lookup"><span data-stu-id="b3f48-123">You should see hello Certificates snap-in loaded.</span></span> <span data-ttu-id="b3f48-124">Klikněte na tlačítko **certifikáty (místní počítač)** tooexpand.</span><span class="sxs-lookup"><span data-stu-id="b3f48-124">Click **Certificates (Local Computer)** tooexpand.</span></span> <span data-ttu-id="b3f48-125">Klikněte na tlačítko tooexpand hello **osobní** uzlu, za nímž následuje hello **certifikáty** uzlu.</span><span class="sxs-lookup"><span data-stu-id="b3f48-125">Click tooexpand hello **Personal** node, followed by hello **Certificates** node.</span></span>

    ![Úložiště otevřete osobních certifikátů](./media/active-directory-domain-services-admin-guide/secure-ldap-open-personal-store.png)
9. <span data-ttu-id="b3f48-127">Měli byste vidět hello certifikát podepsaný svým držitelem, které jsme vytvořili.</span><span class="sxs-lookup"><span data-stu-id="b3f48-127">You should see hello self-signed certificate we created.</span></span> <span data-ttu-id="b3f48-128">Můžete zkontrolovat vlastnosti hello hello certifikát tooensure hello kryptografický otisk odpovídajících položek, které hlášené v systému windows hello prostředí PowerShell, když jste vytvořili certifikát hello.</span><span class="sxs-lookup"><span data-stu-id="b3f48-128">You can examine hello properties of hello certificate tooensure hello thumbprint matches that reported on hello PowerShell windows when you created hello certificate.</span></span>
10. <span data-ttu-id="b3f48-129">Vyberte hello certifikát podepsaný svým držitelem a **klikněte pravým tlačítkem na**.</span><span class="sxs-lookup"><span data-stu-id="b3f48-129">Select hello self-signed certificate and **right click**.</span></span> <span data-ttu-id="b3f48-130">Z nabídky hello klikněte pravým tlačítkem, vyberte **všechny úlohy** a vyberte **exportovat...** .</span><span class="sxs-lookup"><span data-stu-id="b3f48-130">From hello right-click menu, select **All Tasks** and select **Export...**.</span></span>

    ![Export certifikátu](./media/active-directory-domain-services-admin-guide/secure-ldap-export-cert.png)
11. <span data-ttu-id="b3f48-132">V hello **Průvodce exportem certifikátu**, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="b3f48-132">In hello **Certificate Export Wizard**, click **Next**.</span></span>

    ![Průvodce exportem certifikátu](./media/active-directory-domain-services-admin-guide/secure-ldap-export-cert-wizard.png)
12. <span data-ttu-id="b3f48-134">Na hello **exportovat soukromý klíč** vyberte **Ano, exportovat soukromý klíč hello**a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="b3f48-134">On hello **Export Private Key** page, select **Yes, export hello private key**, and click **Next**.</span></span>

    ![Exportovat privátní klíč certifikátu](./media/active-directory-domain-services-admin-guide/secure-ldap-export-private-key.png)

    > [!WARNING]
    > <span data-ttu-id="b3f48-136">Je nutné exportovat privátní klíč hello společně s hello certifikátu.</span><span class="sxs-lookup"><span data-stu-id="b3f48-136">You MUST export hello private key along with hello certificate.</span></span> <span data-ttu-id="b3f48-137">Pokud zadáte soubor PFX, který neobsahuje privátní klíč certifikátu hello hello, povolení zabezpečeného LDAP vaší spravované domény nezdaří.</span><span class="sxs-lookup"><span data-stu-id="b3f48-137">If you provide a PFX that does not contain hello private key for hello certificate, enabling secure LDAP for your managed domain fails.</span></span>
    >
    >
13. <span data-ttu-id="b3f48-138">Na hello **formát souboru pro Export** vyberte **Personal Information Exchange – PKCS #12 (. PFX)** hello formát souboru pro hello exportovat certifikát.</span><span class="sxs-lookup"><span data-stu-id="b3f48-138">On hello **Export File Format** page, select **Personal Information Exchange - PKCS #12 (.PFX)** as hello file format for hello exported certificate.</span></span>

    ![Formát souboru pro export certifikátu](./media/active-directory-domain-services-admin-guide/secure-ldap-export-to-pfx.png)

    > [!NOTE]
    > <span data-ttu-id="b3f48-140">Pouze hello. Formát souboru PFX je podporován.</span><span class="sxs-lookup"><span data-stu-id="b3f48-140">Only hello .PFX file format is supported.</span></span> <span data-ttu-id="b3f48-141">Neexportovat toohello certifikát hello. Formát souboru CER.</span><span class="sxs-lookup"><span data-stu-id="b3f48-141">Do not export hello certificate toohello .CER file format.</span></span>
    >
    >
14. <span data-ttu-id="b3f48-142">Na hello **zabezpečení** stránky, vyberte hello **heslo** možnost a zadejte hello tooprotect a heslo. Soubor PFX.</span><span class="sxs-lookup"><span data-stu-id="b3f48-142">On hello **Security** page, select hello **Password** option and type in a password tooprotect hello .PFX file.</span></span> <span data-ttu-id="b3f48-143">Heslo si zapamatujte, protože bude potřeba v dalším úkolem hello.</span><span class="sxs-lookup"><span data-stu-id="b3f48-143">Remember this password since it will be needed in hello next task.</span></span> <span data-ttu-id="b3f48-144">Klikněte na tlačítko **Další** tooproceed.</span><span class="sxs-lookup"><span data-stu-id="b3f48-144">Click **Next** tooproceed.</span></span>

    ![<span data-ttu-id="b3f48-145">Heslo pro export certifikátu</span><span class="sxs-lookup"><span data-stu-id="b3f48-145">Password for certificate export</span></span> ](./media/active-directory-domain-services-admin-guide/secure-ldap-export-select-password.png)

    > [!NOTE]
    > <span data-ttu-id="b3f48-146">Toto heslo si poznamenejte.</span><span class="sxs-lookup"><span data-stu-id="b3f48-146">Make a note of this password.</span></span> <span data-ttu-id="b3f48-147">Je třeba při povolování zabezpečený LDAP pro tuto doménu spravovaných v [úloha 3 – povolení zabezpečeného LDAP pro spravované doméně hello](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)</span><span class="sxs-lookup"><span data-stu-id="b3f48-147">You need it while enabling secure LDAP for this managed domain in [Task 3 - enable secure LDAP for hello managed domain](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)</span></span>
    >
    >
15. <span data-ttu-id="b3f48-148">Na hello **soubor tooExport** stránky, zadejte název souboru hello a umístění, kam chcete tooexport hello certifikátu.</span><span class="sxs-lookup"><span data-stu-id="b3f48-148">On hello **File tooExport** page, specify hello file name and location where you'd like tooexport hello certificate.</span></span>

    ![Cesta pro export certifikátu](./media/active-directory-domain-services-admin-guide/secure-ldap-export-select-path.png)
16. <span data-ttu-id="b3f48-150">Na hello klikněte na následující stránce **Dokončit** souboru tooexport hello certifikátu tooa PFX.</span><span class="sxs-lookup"><span data-stu-id="b3f48-150">On hello following page, click **Finish** tooexport hello certificate tooa PFX file.</span></span> <span data-ttu-id="b3f48-151">Dialogové okno potvrzení byste měli vidět, pokud byla exportována hello certifikátu.</span><span class="sxs-lookup"><span data-stu-id="b3f48-151">You should see confirmation dialog when hello certificate has been exported.</span></span>

    ![Export certifikátu provést](./media/active-directory-domain-services-admin-guide/secure-ldap-exported-as-pfx.png)


## <a name="next-step"></a><span data-ttu-id="b3f48-153">Další krok</span><span class="sxs-lookup"><span data-stu-id="b3f48-153">Next step</span></span>
[<span data-ttu-id="b3f48-154">Úloha 3 – povolení zabezpečeného LDAP pro spravované doméně hello</span><span class="sxs-lookup"><span data-stu-id="b3f48-154">Task 3 - enable secure LDAP for hello managed domain</span></span>](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)
