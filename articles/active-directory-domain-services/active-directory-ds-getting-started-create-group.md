---
title: "Azure Active Directory Domain Services: Vytvořit skupinu Správci řadič domény Azure AD | Microsoft Docs"
description: "Povolení služby Azure Active Directory Domain Services pomocí portálu Azure Classic"
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
ms.openlocfilehash: 5ed0125e05928cf0f6d9941e099b433ecb46e6e2
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="enable-azure-active-directory-domain-services-using-the-azure-classic-portal"></a>Povolení služby Azure Active Directory Domain Services pomocí portálu Azure Classic
Tento článek popisuje a provede úlohy konfigurace, které jsou požadovány pro povolit Azure Active Directory Domain Services (Azure AD DS) pro klienta služby Azure Active Directory (Azure AD).

> [!NOTE]
> [**Místo toho zkuste nové rozhraní portálu (preview) Azure**](active-directory-ds-getting-started.md). 
>

## <a name="task-1-create-the-azure-ad-dc-administrators-group"></a>Úloha 1: vytvoření skupiny administrators řadič domény Azure AD
První úlohou je vytvoření skupiny pro správu v klientovi služby Azure AD. Tuto speciální skupinu pro správu se nazývá *AAD řadič domény správci*. Členové této skupiny mají oprávnění správce na počítačích, které jsou připojené k doméně do Azure Active Directory Domain Services spravované domény. Na počítačích připojených k doméně je této skupiny přidat do skupiny administrators. Členové této skupiny navíc můžete použít vzdálené plochy se vzdáleně připojit k doméně počítače.  

> [!NOTE]
> Nemáte oprávnění správce domény nebo správce podnikové sítě na spravované domény, který jste vytvořili pomocí Azure Active Directory Domain Services. Na spravovaných domén tato oprávnění jsou vyhrazené pomocí služby a nejsou k dispozici uživatelům v rámci klienta. Ale můžete vytvořit v této úloze konfigurace speciální skupinu pro správu provádět některé privilegované operace. Tyto operace zahrnují připojení počítače k doméně, které patří do skupiny správy na počítačích připojených k doméně a konfigurace zásad skupiny.
>

V této úloze konfigurace vytvořte skupiny pro správu a přidejte jeden nebo více uživatelů ve vašem adresáři do skupiny. Pokud chcete vytvořit skupinu pro správu pro Azure Active Directory Domain Services, postupujte takto:

1. Přejděte na [portál Azure Classic](https://manage.windowsazure.com).
2. V levém podokně vyberte tlačítko **Active Directory**.
3. Vyberte klienta Azure AD (adresář), pro který chcete povolit Azure Active Directory Domain Services. Můžete vytvořit pouze jednu doménu pro každý adresář Azure AD.

    ![Vyberte adresář Azure AD](./media/active-directory-domain-services-getting-started/select-aad-directory.png)
4. Na **preview directory** klikněte na tlačítko **skupiny** kartě.
5. Chcete-li přidat skupinu ke klientovi Azure AD, v podokně úloh v dolní části okna klikněte na tlačítko **přidat skupinu**.

    ![Tlačítko Přidat skupinu](./media/active-directory-domain-services-getting-started/add-group-button.png)
6. V **přidat skupinu** dialogové okno pole, vytvořte skupinu s názvem **AAD řadič domény správci**a poté nastavte **typ skupiny** k **zabezpečení**.

   > [!WARNING]
   > Pokud chcete povolit přístup v rámci Azure Active Directory Domain Services spravované domény, vytvořte skupinu s tímto názvem přesný.
   >
   >

    ![Dialogové okno Přidat skupinu](./media/active-directory-domain-services-getting-started/create-admin-group.png)
7. V **popis** zadejte popis, který umožňuje ostatním uživatelům pochopit, že této skupině uděluje oprávnění pro správu v rámci Azure Active Directory Domain Services.
8. Po vytvoření skupiny, klikněte na název skupiny a zobrazte její vlastnosti.
9. Chcete-li přidat uživatele jako členy skupiny v dolní části okna klikněte na tlačítko **přidat členy** tlačítko.

    ![Přidání tlačítka členy skupiny](./media/active-directory-domain-services-getting-started/add-group-members-button.png)
10. V **přidat členy** dialogovém okně vyberte uživatele, kteří by měl být členy této skupiny a pak klikněte na ikonu zaškrtnutí vpravo dole.

    ![Přidání uživatelů do skupiny administrators](./media/active-directory-domain-services-getting-started/add-group-members.png)


## <a name="next-step"></a>Další krok
[Úloha 2: vytvoření nebo výběr virtuální sítě Azure](active-directory-ds-getting-started-vnet.md)
