---
title: "Azure Active Directory Domain Services: Začínáme | Microsoft Docs"
description: "Povolit Azure Active Directory Domain Services pomocí hello portálu Azure (Preview)"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: ace1ed4a-bf7f-43c1-a64a-6b51a2202473
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/15/2017
ms.author: maheshu
ms.openlocfilehash: 8bde872a13bc9960d1e62c74017ff78a8953a0a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-azure-active-directory-domain-services-using-hello-azure-portal-preview"></a>Povolit Azure Active Directory Domain Services pomocí hello portálu Azure (Preview)


## <a name="task-3-configure-administrative-group"></a>Úloha 3: Konfigurace skupiny pro správu
V této úloze konfigurace můžete vytvořit skupiny pro správu v adresáři služby Azure AD. Tuto speciální skupinu pro správu se nazývá *AAD řadič domény správci*. Členové této skupiny mají oprávnění správce na počítačích, které jsou připojené k doméně toohello spravované domény. Na počítačích připojených k doméně je této skupiny přidat toohello skupiny administrators. Členové této skupiny můžou navíc použít počítače vzdáleně připojené k toodomain tooconnect vzdálené plochy.

> [!NOTE]
> Nemáte oprávnění správce domény nebo správce podnikové sítě na hello spravované domény, který jste vytvořili pomocí Azure Active Directory Domain Services. Na spravovaných domén tato oprávnění jsou vyhrazené hello služby a nejsou k dispozici toousers v rámci klienta hello provedeny. Můžete však použít hello speciální skupinu pro správu vytvořit v této konfigurace úloh tooperform některé privilegované operace. Tyto operace zahrnují připojení počítače toohello doméně, patřící toohello skupiny správy na počítačích připojených k doméně a konfigurace zásad skupiny.
>

Skupina pro správu hello Hello průvodce automaticky vytvoří v adresáři služby Azure AD. Tato skupina se nazývá 'AAD řadič domény správci'. Pokud máte existující skupinu s tímto názvem v adresáři služby Azure AD, Průvodce hello vybírá této skupiny. Můžete nakonfigurovat pomocí hello členství ve skupině **skupiny správců** stránce průvodce.

1. členství ve skupině tooconfigure, klikněte na tlačítko **AAD řadič domény správci**.

    ![Nakonfigurujte členství ve skupině](./media/getting-started/domain-services-blade-admingroup.png)

2. Klikněte na tlačítko hello **přidat členy** tlačítko tooadd uživatelé ze skupiny správce toohello adresář Azure AD.

3. Až budete hotovi, klikněte na tlačítko **OK** toomove na toohello **Souhrn** hello průvodce.

4. Na hello **Souhrn** hello průvodce, zkontrolujte nastavení konfigurace hello hello spravované domény. Krok tooany hello Průvodce toomake změn, můžete přejít zpět, v případě potřeby. Až budete hotovi, klikněte na tlačítko **OK** toocreate hello nové spravované domény.

    ![Souhrn](./media/getting-started/domain-services-blade-summary.png)

5. Zobrazí oznámení, že zobrazuje průběh hello vašeho nasazení služby Azure AD Domain Services. Klikněte na tlačítko hello oznámení toosee podrobný průběh nasazení hello.

    ![Oznámení – v průběhu nasazení](./media/getting-started/domain-services-blade-deployment-in-progress.png)


## <a name="provision-your-managed-domain"></a>Zřídit vaší spravované domény
Hello proces zřizování vaší spravované domény může trvat až hodinu tooan.

1. Během nasazení, můžete vyhledat domain services v hello **vyhledávání prostředků** vyhledávacího pole. Vyberte **Azure AD Domain Services** z výsledek hledání hello. Hello **Azure AD Domain Services** okno uvádí hello spravované domény, který se připravuje.

    ![Najít spravované doméně, se zřídí](./media/getting-started/domain-services-provisioning-state-find-resource.png)

2. Další podrobnosti o hello domény klikněte na tlačítko hello název toosee hello spravované domény (například "contoso100.com").

    ![Doménových služeb – Stav zřizování](./media/getting-started/domain-services-provisioning-state.png)

3. Hello **přehled** kartě se zobrazují se právě zřizuje tuto doménu hello. Spravované domény hello nelze nakonfigurovat, dokud je plně zřízený. To může trvat až hodinu tooan vaší spravované domény toobe plně zřízený.

    ![Doménových služeb – karta Přehled během hello Stav zřizování ](./media/getting-started/domain-services-provisioning-state-details.png)

4. Pokud spravované doméně hello plně zřízený, hello **přehled** kartě se zobrazují stav domén hello jako **systémem**.

    ![Domain Services – Karta Přehled po úplném zřízení](./media/getting-started/domain-services-provisioned.png)

5. Na hello **vlastnosti** kartě uvidíte dvě IP adresy, které jsou k dispozici pro virtuální síť hello řadiče.

    ![Doménových služeb – karta Vlastnosti po plně zřízený.](./media/getting-started/domain-services-provisioned-properties.png)


## <a name="next-step"></a>Další krok
[Úloha 4: aktualizace nastavení DNS hello hello virtuální síť Azure](active-directory-ds-getting-started-dns.md)
