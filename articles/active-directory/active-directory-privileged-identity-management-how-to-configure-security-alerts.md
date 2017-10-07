---
title: "výstrahy zabezpečení tooconfigure aaaHow | Microsoft Docs"
description: "Zjistěte, jak výstrahy zabezpečení tooconfigure pro rozšíření Azure Privileged Identity Management."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 4e0c911a-36c6-42a0-8f79-a01c03d2d04f
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/06/2017
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 1b3c4a7d36fa3f81bb3fe2574d675fdf0ab34909
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-security-alerts-in-azure-ad-privileged-identity-management"></a>Jak výstrahy tooconfigure zabezpečení v Azure AD Privileged Identity Management
## <a name="security-alerts"></a>Výstrahy zabezpečení
Azure Správa privilegovaných identit (PIM) generuje výstrahy, když je aktivita podezřelého nebo unsafe ve vašem prostředí. Výstrahy, zobrazí na řídicím panelu PIM hello. Vyberte výstrahy toosee hello sestavy, zda text hello seznamy uživatelů nebo rolí, které spustí výstrahu hello.

![Výstrahy zabezpečení PIM řídicí panel – snímek obrazovky][1]

| Výstrahy | Aktivační události | Doporučení |
| --- | --- | --- |
| **Role se přiřazují mimo PIM** |Správce byl trvale přiřadit role tooa mimo PIM rozhraní hello. |Zkontrolujte hello nové přiřazení role. Vzhledem k tomu, že jiné služby lze přiřadit pouze trvalých správců, oprávněné přiřazení tooan podle potřeby změňte. |
| **Role jsou příliš často aktivován** |Nebyly k dispozici příliš mnoho opětovné aktivace Dobrý den, stejný atribut role v čase hello povolen v nastavení hello. |Obraťte se na hello uživatele toosee, proč se aktivaci hello role mnoho časy. Možná hello času je příliš krátká pro ně omezení toocomplete úkoly, nebo možná používáte skriptů tooautomatically aktivovat roli. |
| **Role nevyžadují službu Multi-Factor authentication pro aktivaci** |Existuje rolí bez MFA povolený v nastaveních hello. |Jsme vícefaktorové ověřování vyžadovat pro hello nejvíce vysoce privilegované role, ale důrazně vyzývá povolit MFA pro aktivaci všech rolí. |
| **Nepoužívají správci svoje privilegované role** |Existují oprávněné správce, kteří nedávno neaktivovali jejich rolí. |Spusťte uživatelé přístup zkontrolujte toodetermine hello, které už nepotřebují přístup. |
| **Existuje příliš mnoho globální správci** |Existuje více globálních správců, než se nedoporučuje. |Pokud máte velký počet globálních správců, je pravděpodobné, že uživatelé jsou stále větší oprávnění než potřebují. Přesunutí uživatelé tooless privilegované role, nebo se přesvědčte některá z nich vhodné pro roli hello místo trvale přiřazená. |

## <a name="configure-security-alert-settings"></a>Konfigurace nastavení výstrah zabezpečení
Můžete přizpůsobit některé z hello výstrah zabezpečení v PIM toowork s prostředím a cíle zabezpečení. Postupujte podle těchto kroků tooreach hello nastavení okno:

1. Přihlaste se toohello [portál Azure](https://portal.azure.com/) a vyberte hello **Azure AD Privileged Identity Management** dlaždice z řídicího panelu hello.
2. Vyberte **spravovat privilegované role** > **nastavení** > **výstrahy nastavení**.
   
    ![Přejděte toosecurity nastavení výstrah][2]

### <a name="roles-are-being-activated-too-frequently-alert"></a>Výstraha "Role jsou aktivované příliš často"
Tato výstraha, že aktivačních událostí, pokud uživatel aktivuje hello stejné privilegované role několikrát v zadaném období. Můžete nakonfigurovat i hello časové období a hello počet aktivací.

* **Obnovení aktivace časový rámec**: Zadejte počet dnů, hodin, minut a podruhé hello období chcete toouse tootrack podezřelé obnovení.
* **Počet obnovení aktivace**: Zadejte hello počet aktivací, z 2 too100, které považujete za worthy výstrahy v časovém rámci hello jste zvolili. Můžete změnit tato nastavení podle přesunutí posuvníku hello nebo hello textového pole zadejte číslo.

### <a name="there-are-too-many-global-administrators-alert"></a>Výstraha "Existuje příliš mnoho globální správci"
PIM aktivuje tuto výstrahu, pokud jsou splněny dva různé kritéria a nakonfigurujete oba dva. Nejprve je třeba tooreach určité hranice globální správci. Procento vaše přiřazení role celkový druhý, musí být globální správce. Pokud splňujete pouze jeden z těchto měření, hello výstraha se nezobrazí.  

* **Minimální počet globálních správců**: Zadejte číslo hello globálních správců z 2 too100, vezměte v úvahu dobu unsafe.
* **Procento globálních správců**: Zadejte procento hello správců, kteří jsou globální správci z 0 % too100 %, která nebezpečné ve vašem prostředí.

### <a name="administrators-arent-using-their-privileged-roles-alert"></a>Výstraha "Správci nejsou pomocí jejich privilegované role"
Tato výstraha aktivuje, pokud uživatel přejde určitou dobu bez aktivace role.

* **Počet dnů**: Zadejte hello počet dní od 0 too100, který uživatel může přejít bez aktivace role.

## <a name="next-steps"></a>Další kroky
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-configure-security-alerts/PIM_security_dash.png
[2]: ./media/active-directory-privileged-identity-management-how-to-configure-security-alerts/PIM_security_settings.png
