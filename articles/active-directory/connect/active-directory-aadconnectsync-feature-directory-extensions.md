---
title: "Synchronizace Azure AD Connect: rozšíření adresáře | Microsoft Docs"
description: "Toto téma popisuje funkce rozšíření adresáře v Azure AD Connect."
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: 995ee876-4415-4bb0-a258-cca3cbb02193
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 8ab9944437443a6d796345782f4f798aec96a0f6
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="azure-ad-connect-sync-directory-extensions"></a>Synchronizace Azure AD Connect: rozšíření adresáře
Rozšíření adresáře můžete rozšířit schéma ve službě Azure AD s vlastními atributy z místní služby Active Directory. Tato funkce umožňuje vytvářet obchodní aplikace, které využívají atributy nadále spravovat místní. Tyto atributy mohou být využívány prostřednictvím [rozšíření adresáře Azure AD Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions) nebo [Microsoft Graph](https://graph.microsoft.io/). Můžete zobrazit dostupné atributy pomocí [Azure AD Graph explorer](https://graphexplorer.cloudapp.net) a [Microsoft Graph explorer](https://graphexplorer2.azurewebsites.net/) v uvedeném pořadí.

Žádné úlohy Office 365 v současné době využívá tyto atributy.

Můžete nakonfigurovat další atributy, které chcete synchronizovat v cestě vlastní nastavení v Průvodci instalací.
![Průvodce rozšíření schématu](./media/active-directory-aadconnectsync-feature-directory-extensions/extension2.png)  
Instalace ukazuje následující atributy, které jsou kandidáty platné:

* Typy objektů uživatelů a skupin
* Jednohodnotové atributy: řetězec, logická hodnota, celé číslo, binární
* Více hodnot atributů: String, binární

V seznamu atributů je načten z mezipaměti schématu vytvořen během instalace služby Azure AD Connect. Pokud jste rozšířili schéma Active Directory s další atributy, pak se [je třeba aktualizovat schéma](active-directory-aadconnectsync-installation-wizard.md#refresh-directory-schema) před tyto nové atributy jsou viditelné.

Objekt ve službě Azure AD může mít až 100 atributů rozšíření adresáře. Maximální délka je 250 znaků. Pokud hodnota atributu je delší, zkrátí se synchronizační modul.

Během instalace služby Azure AD Connect je zaregistrován aplikace, které jsou k dispozici tyto atributy. Můžete zobrazit tuto aplikaci na portálu Azure.  
![Aplikace rozšíření schématu](./media/active-directory-aadconnectsync-feature-directory-extensions/extension3new.png)

Tyto atributy jsou nyní k dispozici prostřednictvím grafu:  
![Graph](./media/active-directory-aadconnectsync-feature-directory-extensions/extension4.png)

Atributy mají předponu rozšíření\_{AppClientId}\_. AppClientId mají stejnou hodnotu pro všechny atributy v klientovi služby Azure AD.

## <a name="next-steps"></a>Další kroky
Další informace o [synchronizace Azure AD Connect](active-directory-aadconnectsync-whatis.md) konfigurace.

Přečtěte si další informace o [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md).
