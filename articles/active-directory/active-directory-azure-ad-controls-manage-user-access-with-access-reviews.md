---
title: "Správa přístupu uživatelů pomocí kontrol přístupu Azure AD | Dokumentace Microsoftu"
description: "Zjistěte, jak spravovat přístup uživatelů jako členství ve skupině nebo přiřazení k aplikaci s využitím kontrol přístupu Azure Active Directory."
services: active-directory
documentationcenter: 
author: markwahl-msft
manager: mtillman
editor: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/19/2017
ms.author: billmath
ms.openlocfilehash: 6a4d25b2eb228cafab48419a0d0eda92bba9f1ec
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="manage-user-access-with-azure-ad-access-reviews"></a>Správa přístupu uživatelů pomocí kontrol přístupu Azure AD

S Azure Active Directory (Azure AD) můžete snadno zajistit, že uživatelé mají odpovídající přístup. Požádejte přímo uživatele nebo pracovníka s rozhodovací pravomocí o účast při kontrole přístupu a znovu certifikujte (nebo potvrďte) přístup uživatelů. Revidující se mohou vyjádřit k nutnosti dalšího přístupu pro jednotlivé uživatele na základě návrhů z Azure AD. Po dokončení kontroly přístupu můžete provést změny a ukončit přístup uživatelů, kteří ho už nepotřebují.

> [!NOTE]
> Pokud chcete kontrolovat přístup jenom u uživatelů typu host a nekontrolovat všechny typy uživatelského přístupu, přečtěte si téma věnované [správě přístupu uživatelů typu host s využitím kontroly přístupu](active-directory-azure-ad-controls-manage-guest-access-with-access-reviews.md). Pokud chcete kontrolovat členství uživatelů v rolích pro správu, jako je například globální správce, přečtěte si téma věnované [spuštění kontroly přístupu v Azure AD Privileged Identity Management](active-directory-privileged-identity-management-how-to-start-security-review.md). 
>
>

## <a name="prerequisites"></a>Požadavky 

Kontroly přístupu jsou k dispozici v edici Premium P2 služby Azure AD, která je součástí řešení Microsoft Enterprise Mobility + Security, E5. Další informace najdete v článku [Edice služby Azure Active Directory](active-directory-editions.md). Každý uživatel, který komunikuje s touto funkcí při vytváření kontroly, přístupu ke kontrole nebo použití kontroly, musí mít licenci.


## <a name="create-and-perform-an-access-review"></a>Vytvoření a provedení kontroly přístupu

V rámci kontroly přístupu může revidujícími být mít jednoho nebo několik uživatelů jako revidující.  

1. Vyberte skupinu ve službě Azure AD, která má několik členů. Případně vyberte aplikaci připojenou ke službě Azure AD, který má přiřazených několik uživatelů. 

2. Rozhodněte, jestli má každý uživatel kontrolovat svůj vlastní přístup, nebo jestli má jeden nebo několik uživatelů kontrolovat přístup všech.

3. Povolte zobrazení kontrol přístupu na přístupových panelech revidujících. Jako globální správce přejděte na [stránku kontrol přístupu](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/).

4. Začněte kontrolu přístupu. Další informace najdete v popisu [vytvoření kontroly přístupu](active-directory-azure-ad-controls-create-access-review.md).

5. Požádejte revidující o informace. Ve výchozím nastavení každý z nich obdrží e-mailu od Azure AD s odkazem na panel přístupu, kde bude [provádět kontrolu přístupu](active-directory-azure-ad-controls-perform-access-review.md).

6. Pokud revidující neposkytli informace, můžete požádat službu Azure AD, aby jim odeslala připomenutí. Ve výchozím nastavení Azure AD automaticky pošle připomenutí revidujícím, kteří ještě neodpověděli, po uplynutí poloviny času.

7. Jakmile revidující poskytnou potřebné informace, zastavte kontrolu přístupu a proveďte příslušné změny. Další informace najdete v popisu [dokončení kontroly přístupu](active-directory-azure-ad-controls-complete-access-review.md).


## <a name="next-steps"></a>Další kroky

[Vytváření kontroly přístupu pro členy skupiny nebo přístupu k aplikaci](active-directory-azure-ad-controls-create-access-review.md)




