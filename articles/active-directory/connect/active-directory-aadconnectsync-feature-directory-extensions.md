---
title: "Synchronizace Azure AD Connect: rozšíření adresáře | Microsoft Docs"
description: "Toto téma popisuje funkce rozšíření hello adresáře v Azure AD Connect."
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
ms.openlocfilehash: 31525ae914aa4d9e047ea1515b460a8311d5c815
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-directory-extensions"></a>Synchronizace Azure AD Connect: rozšíření adresáře
Rozšíření adresáře můžete tooextend hello schématu ve službě Azure AD s vlastními atributy z místní služby Active Directory. Tato funkce umožňuje využívání atributy pokračovat toomanage místní toobuild obchodní aplikace. Tyto atributy mohou být využívány prostřednictvím [rozšíření adresáře Azure AD Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions) nebo [Microsoft Graph](https://graph.microsoft.io/). Můžete zobrazit hello atributů není k dispozici pomocí [explorer Azure AD Graph](https://graphexplorer.cloudapp.net) a [Microsoft Graph explorer](https://graphexplorer2.azurewebsites.net/) v uvedeném pořadí.

Žádné úlohy Office 365 v současné době využívá tyto atributy.

Můžete nakonfigurovat další atributy, které chcete toosynchronize v cestě hello vlastní nastavení v Průvodci instalací hello.
![Průvodce rozšíření schématu](./media/active-directory-aadconnectsync-feature-directory-extensions/extension2.png)  
instalace Hello ukazuje hello atributy, které jsou platné kandidáty následující:

* Typy objektů uživatelů a skupin
* Jednohodnotové atributy: řetězec, logická hodnota, celé číslo, binární
* Více hodnot atributů: String, binární

Hello seznam atributů je načten z mezipaměti schématu hello vytvořen během instalace služby Azure AD Connect. Pokud jste rozšířili schéma služby Active Directory hello s další atributy, pak hello [je třeba aktualizovat schéma](active-directory-aadconnectsync-installation-wizard.md#refresh-directory-schema) před tyto nové atributy jsou viditelné.

Objekt ve službě Azure AD může mít až too100 atributů rozšíření adresáře. Maximální délka Hello je 250 znaků. Pokud hodnota atributu je delší, zkrátí se synchronizační modul hello.

Během instalace služby Azure AD Connect je zaregistrován aplikace, které jsou k dispozici tyto atributy. Můžete zobrazit tuto aplikaci v hello portálu Azure.  
![Aplikace rozšíření schématu](./media/active-directory-aadconnectsync-feature-directory-extensions/extension3new.png)

Tyto atributy jsou nyní k dispozici prostřednictvím grafu:  
![Graph](./media/active-directory-aadconnectsync-feature-directory-extensions/extension4.png)

atributy Hello mají předponu rozšíření\_{AppClientId}\_. Hello AppClientId má hello stejnou hodnotu pro všechny atributy v klientovi služby Azure AD.

## <a name="next-steps"></a>Další kroky
Další informace o hello [synchronizace Azure AD Connect](active-directory-aadconnectsync-whatis.md) konfigurace.

Přečtěte si další informace o [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md).
