---
title: "Azure Active Directory Domain Services: Vytvoření skupiny administrators řadič domény hello Azure AD | Microsoft Docs"
description: "Povolit Azure Active Directory Domain Services pomocí hello portál Azure classic"
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
ms.date: 07/13/2017
ms.author: maheshu
ms.openlocfilehash: d629ab9632ef6a45b549630999ff9a122d60c040
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-azure-active-directory-domain-services-using-hello-azure-classic-portal"></a>Povolit Azure Active Directory Domain Services pomocí hello portál Azure classic
Tento článek popisuje a provede hello úlohy konfigurace, které jsou požadovány pro vás tooenable Azure Active Directory Domain Services (Azure AD DS) pro klienta služby Azure Active Directory (Azure AD).

> [!NOTE]
> [**Místo toho zkuste hello nové (preview) Azure portálu prostředí**](active-directory-ds-getting-started.md). 
>

## <a name="task-1-create-hello-azure-ad-dc-administrators-group"></a>Úloha 1: vytvoření skupiny administrators řadič domény hello Azure AD
první úlohou Hello je toocreate skupiny pro správu v klientovi služby Azure AD. Tuto speciální skupinu pro správu se nazývá *AAD řadič domény správci*. Členové této skupiny mají oprávnění správce na počítačích, které jsou připojené k doméně toohello Azure Active Directory Domain Services spravované domény. Na počítačích připojených k doméně je této skupiny přidat toohello skupiny administrators. Členové této skupiny můžou navíc použít počítače vzdáleně připojené k toodomain tooconnect vzdálené plochy.  

> [!NOTE]
> Nemáte oprávnění správce domény nebo správce podnikové sítě na hello spravované domény, který jste vytvořili pomocí Azure Active Directory Domain Services. Na spravovaných domén tato oprávnění jsou vyhrazené hello služby a nejsou k dispozici toousers v rámci klienta hello provedeny. Můžete však použít hello speciální skupinu pro správu vytvořit v této konfigurace úloh tooperform některé privilegované operace. Tyto operace zahrnují připojení počítače toohello doméně, patřící toohello skupiny správy na počítačích připojených k doméně a konfigurace zásad skupiny.
>

V této úloze konfigurace vytvořte skupiny pro správu hello a přidejte jeden nebo více uživatelů ve vaší skupině toohello directory. toocreate hello skupiny pro správu pro Azure Active Directory Domain Services, hello následující:

1. Přejděte toohello [portál Azure classic](https://manage.windowsazure.com).
2. V levém podokně hello vyberte hello **služby Active Directory** tlačítko.
3. Vyberte, pro které chcete tooenable Azure Active Directory Domain Services klienta hello Azure AD (adresář). Můžete vytvořit pouze jednu doménu pro každý adresář Azure AD.

    ![Vyberte adresář Azure AD](./media/active-directory-domain-services-getting-started/select-aad-directory.png)
4. Na hello **preview directory** klikněte na tlačítko hello **skupiny** kartě.
5. Klikněte na klienta skupiny tooyour Azure AD, v podokně úloh hello v hello dolní části okna hello tooadd **přidat skupinu**.

    ![tlačítko Přidat skupinu Hello](./media/active-directory-domain-services-getting-started/add-group-button.png)
6. V hello **přidat skupinu** dialogové okno pole, vytvořte skupinu s názvem **AAD řadič domény správci**a poté nastavte **typ skupiny** příliš**zabezpečení**.

   > [!WARNING]
   > tooenable přístupu ve vaší doméně, spravovat Azure Active Directory Domain Services, vytvořte skupinu s tímto názvem přesný.
   >
   >

    ![Dialogové okno Přidat skupinu Hello](./media/active-directory-domain-services-getting-started/create-admin-group.png)
7. V hello **popis** zadejte popis, který umožňuje ostatním toounderstand že této skupině uděluje oprávnění pro správu v rámci Azure Active Directory Domain Services.
8. Po vytvoření skupiny hello, klikněte na tlačítko tooview název skupiny hello jeho vlastnosti.
9. tooadd uživatele jako členy skupiny hello v hello dolní části okna hello, klikněte na tlačítko hello **přidat členy** tlačítko.

    ![Přidání tlačítka členy skupiny](./media/active-directory-domain-services-getting-started/add-group-members-button.png)
10. V hello **přidat členy** dialogové okno, vyberte hello uživatelů, kteří by měl být členy této skupiny a potom klikněte na ikonu zaškrtnutí hello v hello dolní pravá.

    ![Přidat skupinu uživatelů tooadministrators](./media/active-directory-domain-services-getting-started/add-group-members.png)


## <a name="next-step"></a>Další krok
[Úloha 2: vytvoření nebo výběr virtuální sítě Azure](active-directory-ds-getting-started-vnet.md)
