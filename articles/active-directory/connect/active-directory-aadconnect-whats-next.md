---
title: "Azure AD Connect: Další kroky a jak toomanage Azure AD Connect | Microsoft Docs"
description: "Zjistěte, jak tooextend hello výchozí konfigurace a provozní úlohy pro Azure AD Connect."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: curtand
ms.assetid: c18bee36-aebf-4281-b8fc-3fe14116f1a5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 4404aaff24d54d76b83baca3b331a6a250ba4c03
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="next-steps-and-how-toomanage-azure-ad-connect"></a>Další kroky a jak toomanage Azure AD Connect
Použijte hello provozní postupy v této článku toocustomize Azure Active Directory (Azure AD) připojit toomeet potřebám a požadavkům vaší organizace.  

## <a name="add-additional-sync-admins"></a>Přidat další synchronizaci admins
Ve výchozím nastavení jsou pouze hello uživatele, který hello instalace a místní správce může toomanage hello nainstalovat synchronizační modul. Pro možnost tooaccess toobe dalším osobám a spravovat hello synchronizační modul, vyhledat hello skupinu s názvem ADSyncAdmins na místním serveru hello a přidat je toothis skupiny.

## <a name="assign-licenses-tooazure-ad-premium-and-enterprise-mobility-suite-users"></a>Přiřazení tooAzure licencí AD Premium a Enterprise Mobility Suite uživatelů
Teď, když uživatelé byly synchronizovány toohello cloudu, je nutné tooassign je licenci, můžete začít s cloudové aplikace, jako je například Office 365.

### <a name="tooassign-an-azure-ad-premium-or-enterprise-mobility-suite-license"></a>tooassign Azure AD Premium nebo Enterprise Mobility Suite licencí

1. Přihlaste se toohello portálu Azure jako správce.
2. Na levé straně hello vyberte **služby Active Directory**.
3. Na hello **služby Active Directory** klikněte dvakrát hello adresář, který má hello uživatele můžete určit tooset nahoru.
4. Hello horní části stránky adresáře hello, vyberte **licence**.
5. Na hello **licence** vyberte **Active Directory Premium** nebo **Enterprise Mobility Suite**a potom klikněte na **přiřadit**.
6. V dialogovém okně hello vyberte možnost uživatelé hello má tooassign licence a potom klikněte na hello zaškrtnutí ikonu toosave hello změny.

## <a name="verify-hello-scheduled-synchronization-task"></a>Ověření úlohy plánované synchronizace hello
Použijte hello Azure portálu toocheck hello stav synchronizace.

### <a name="tooverify-hello-scheduled-synchronization-task"></a>Úloha tooverify hello naplánované synchronizace
1. Přihlaste se toohello portálu Azure jako správce.
2. Na levé straně hello vyberte **služby Active Directory**.
3. Na hello **služby Active Directory** klikněte dvakrát hello adresář, který má hello uživatele můžete určit tooset nahoru.
4. Hello horní části stránky adresáře hello, vyberte **integrace adresáře**.
5. V části **integraci s místní službou active directory**, Poznámka hello čas poslední synchronizace.

<center>![Čas synchronizace adresáře](./media/active-directory-aadconnect-whats-next/verify.png)</center>

## <a name="start-a-scheduled-synchronization-task"></a>Spustit úlohu plánované synchronizace
Pokud potřebujete toorun úloha synchronizace, musíte znovu spuštěním prostřednictvím Průvodce Azure AD Connect hello.  Je nutné tooprovide přihlašovací údaje Azure AD.  V Průvodci hello vyberte hello **přizpůsobit možnosti synchronizace** úloh a klikněte na tlačítko **Další** toomove prostřednictvím Průvodce hello. Na konci hello, ujistěte se, že hello **zahájit proces synchronizace hello ihned po dokončení počáteční konfigurace hello** políčka.

<center>![Spustit synchronizaci.](./media/active-directory-aadconnect-whats-next/startsynch.png)</center>

Další informace o hello Azure AD Connect sync Scheduler najdete v tématu [Azure AD Connect Scheduler](active-directory-aadconnectsync-feature-scheduler.md).

## <a name="additional-tasks-available-in-azure-ad-connect"></a>Další úlohy, které jsou k dispozici ve službě Azure AD Connect
Po počáteční instalace systému služby Azure AD Connect můžete vždy spustit Průvodce hello znovu z zástupce hello Azure AD Connect úvodní stránky nebo plochy.  Si všimnete, že znovu projít průvodce hello poskytuje některé nové možnosti ve formě hello další úkoly.  

Hello následující tabulka obsahuje souhrn tyto úlohy a stručný popis jednotlivých úloh.

![Seznam dalších úloh](./media/active-directory-aadconnect-whats-next/addtasks.png)

| Další úlohy | Popis |
| --- | --- |
| **Zobrazení hello si vybrali tento scénář** |Zobrazte vaše aktuální řešení Azure AD Connect.  To zahrnuje obecná nastavení, synchronizovat adresáře a nastavení synchronizace. |
| **Přizpůsobit možnosti synchronizace** |Změnit hello aktuální konfiguraci jako přidává další konfigurace toohello doménových struktur služby Active Directory nebo povolení možnosti synchronizace, jako je například uživatele, skupiny, zařízení nebo zpětný zápis hesla. |
| **Zapnout pracovní režim** |Fáze informace, které není synchronizován okamžitě a není exportovat tooAzure AD nebo místní služby Active Directory.  Pomocí této funkce můžete zobrazit náhled hello synchronizace předtím, než k nim dojde. |

## <a name="next-steps"></a>Další kroky
Další informace o [integrace místních identit s Azure Active Directory](active-directory-aadconnect.md).
