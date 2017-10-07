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
# <a name="configure-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a>Konfigurace zabezpečeného LDAP (LDAPS) pro spravované doméně služby Azure AD Domain Services

## <a name="before-you-begin"></a>Než začnete
Ujistěte se, když jste dokončili [úloha 1 – získání certifikátu pro zabezpečený LDAP](active-directory-ds-admin-guide-configure-secure-ldap.md).


## <a name="task-2---export-hello-secure-ldap-certificate-tooa-pfx-file"></a>Úloha 2 – export hello tooa certifikát zabezpečený LDAP. Soubor PFX
Než začnete tuto úlohu, ujistěte se, získání certifikátu hello zabezpečený LDAP od veřejné certifikační autority nebo jste vytvořili certifikát podepsaný svým držitelem.

Proveďte následující kroky hello, tooexport hello LDAPS certifikátu tooa. Soubor PFX.

1. Stiskněte klávesu hello **spustit** tlačítko a typ **R**. V hello **spustit** dialogové okno, typ **konzoly mmc** a klikněte na tlačítko **OK**.

    ![Spusťte konzolu MMC hello](./media/active-directory-domain-services-admin-guide/secure-ldap-start-run.png)
2. Na hello **řízení uživatelských účtů** výzva, klikněte na tlačítko **Ano** toolaunch konzoly MMC (Microsoft Management Console) jako správce.
3. Z hello **soubor** nabídky, klikněte na tlačítko **přidat nebo odebrat modul Snap-in...** .

    ![Přidat modul snap-in tooMMC konzoly](./media/active-directory-domain-services-admin-guide/secure-ldap-add-snapin.png)
4. V hello **přidat nebo odebrat moduly Snap in** dialogové okno, vyberte hello **certifikáty** modul snap-in a klikněte na tlačítko hello **Přidat >** tlačítko.

    ![Přidat konzole tooMMC modul snap-in Certifikáty](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-snapin.png)
5. V hello **modul snap-in Certifikáty** průvodci vyberte **účet počítače** a klikněte na tlačítko **Další**.

    ![Přidat modul snap-in Certifikáty pro účet počítače](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-computer-account.png)
6. Na hello **vybrat počítač** vyberte **místní počítač: (hello počítači je spuštěna tato konzola)** a klikněte na tlačítko **Dokončit**.

    ![Přidat modul snap-in Certifikáty - vyberte počítače](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-local-computer.png)
7. V hello **přidat nebo odebrat moduly Snap in** dialogové okno, klikněte na tlačítko **OK** tooadd hello certifikátů modulu snap-in tooMMC.

    ![Přidejte certifikáty modul snap-in tooMMC - v](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-snapin-done.png)
8. V okně konzoly MMC hello klikněte tooexpand **kořenový adresář konzoly**. Měli byste vidět načíst hello modul snap-in Certifikáty. Klikněte na tlačítko **certifikáty (místní počítač)** tooexpand. Klikněte na tlačítko tooexpand hello **osobní** uzlu, za nímž následuje hello **certifikáty** uzlu.

    ![Úložiště otevřete osobních certifikátů](./media/active-directory-domain-services-admin-guide/secure-ldap-open-personal-store.png)
9. Měli byste vidět hello certifikát podepsaný svým držitelem, které jsme vytvořili. Můžete zkontrolovat vlastnosti hello hello certifikát tooensure hello kryptografický otisk odpovídajících položek, které hlášené v systému windows hello prostředí PowerShell, když jste vytvořili certifikát hello.
10. Vyberte hello certifikát podepsaný svým držitelem a **klikněte pravým tlačítkem na**. Z nabídky hello klikněte pravým tlačítkem, vyberte **všechny úlohy** a vyberte **exportovat...** .

    ![Export certifikátu](./media/active-directory-domain-services-admin-guide/secure-ldap-export-cert.png)
11. V hello **Průvodce exportem certifikátu**, klikněte na tlačítko **Další**.

    ![Průvodce exportem certifikátu](./media/active-directory-domain-services-admin-guide/secure-ldap-export-cert-wizard.png)
12. Na hello **exportovat soukromý klíč** vyberte **Ano, exportovat soukromý klíč hello**a klikněte na tlačítko **Další**.

    ![Exportovat privátní klíč certifikátu](./media/active-directory-domain-services-admin-guide/secure-ldap-export-private-key.png)

    > [!WARNING]
    > Je nutné exportovat privátní klíč hello společně s hello certifikátu. Pokud zadáte soubor PFX, který neobsahuje privátní klíč certifikátu hello hello, povolení zabezpečeného LDAP vaší spravované domény nezdaří.
    >
    >
13. Na hello **formát souboru pro Export** vyberte **Personal Information Exchange – PKCS #12 (. PFX)** hello formát souboru pro hello exportovat certifikát.

    ![Formát souboru pro export certifikátu](./media/active-directory-domain-services-admin-guide/secure-ldap-export-to-pfx.png)

    > [!NOTE]
    > Pouze hello. Formát souboru PFX je podporován. Neexportovat toohello certifikát hello. Formát souboru CER.
    >
    >
14. Na hello **zabezpečení** stránky, vyberte hello **heslo** možnost a zadejte hello tooprotect a heslo. Soubor PFX. Heslo si zapamatujte, protože bude potřeba v dalším úkolem hello. Klikněte na tlačítko **Další** tooproceed.

    ![Heslo pro export certifikátu ](./media/active-directory-domain-services-admin-guide/secure-ldap-export-select-password.png)

    > [!NOTE]
    > Toto heslo si poznamenejte. Je třeba při povolování zabezpečený LDAP pro tuto doménu spravovaných v [úloha 3 – povolení zabezpečeného LDAP pro spravované doméně hello](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)
    >
    >
15. Na hello **soubor tooExport** stránky, zadejte název souboru hello a umístění, kam chcete tooexport hello certifikátu.

    ![Cesta pro export certifikátu](./media/active-directory-domain-services-admin-guide/secure-ldap-export-select-path.png)
16. Na hello klikněte na následující stránce **Dokončit** souboru tooexport hello certifikátu tooa PFX. Dialogové okno potvrzení byste měli vidět, pokud byla exportována hello certifikátu.

    ![Export certifikátu provést](./media/active-directory-domain-services-admin-guide/secure-ldap-exported-as-pfx.png)


## <a name="next-step"></a>Další krok
[Úloha 3 – povolení zabezpečeného LDAP pro spravované doméně hello](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)
