---
title: "Registrace smlouvy Azure Enterprise ve službě Azure Cost Management | Dokumentace Microsoftu"
description: "Použijte svoji smlouvu Enterprise k registraci ve službě Azure Cost Management by Cloudyn."
services: cost-management
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 10/11/2017
ms.topic: quickstart
ms.custom: mvc
ms.service: cost-management
manager: carmonm
ms.openlocfilehash: 41a9df712b07253d9f5f9db8542fb9917592320f
ms.sourcegitcommit: d03907a25fb7f22bec6a33c9c91b877897e96197
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/12/2017
---
# <a name="register-an-azure-enterprise-agreement-and-view-cost-data"></a>Registrace smlouvy Azure Enterprise a zobrazení informací o nákladech

Pomocí své smlouvy Azure Enterprise se zaregistrujete ve službě Azure Cost Management by Cloudyn. Registrací získáte přístup k portálu Cloudyn. Tento rychlý start podrobně popisuje proces registrace nezbytný k vytvoření zkušebního předplatného a přihlášení k portálu Cloudyn. Ukazuje také, jak rovnou začít zobrazovat informace o nákladech.

## <a name="log-in-to-azure"></a>Přihlaste se k Azure.

- Přihlaste se k webu Azure Portal na adrese http://portal.azure.com.

## <a name="create-a-trial-registration"></a>Vytvoření registrace zkušební verze

1. Na webu Azure Portal klikněte v seznamu služeb na **Cost Management a fakturace**.
2. V části **Přehled** klikněte na **Cost Management**.  
    ![Stránka služby Cost Management](./media/quick-register-ea/cost-mgt-billing-service.png)
3. Na stránce **Cost Management** klikněte na **Přejít do služby Cost Management** a v novém okně se otevře stránka pro registraci do Cloudyn.
4. Na stránce pro registraci zkušební verze portálu Cloudyn zadejte název vaší společnosti a vyberte **Správce prováděcí smlouvy Azure Enterprise**.  
    ![registrace zkušební verze](./media/quick-register-ea/trial-reg.png)
5. Zadejte klíč rozhraní API vaší prováděcí smlouvy Enterprise Portal. Pokud svůj klíč nemáte po ruce, klikněte na odkaz na web [Enterprise Portal](https://ea.azure.com) a proveďte následující kroky:
  1. Přihlaste se k webu Azure Enterprise, klikněte na **Sestavy**, pak na **Přístupový klíč rozhraní API** a zkopírujte váš primární klíč.  
    ![Klíč rozhraní API EA](./media/quick-register-ea/ea-key.png)
  3. Vraťte se na registrační stránku a vložte váš klíč rozhraní API.
6. Vyjádřete souhlas s podmínkami použití a ověřte váš klíč. Kliknutím na **Další** udělte Cloudyn oprávnění ke shromažďování dat o prostředcích Azure. Mezi shromažďovaná data patří informace o využití, výkonu, fakturaci a značkách z vašich předplatných.  
    ![ověření klíče](./media/quick-register-ea/ea-key-validated.png)
7. V části **Pozvat další účastníky** můžete zadáním příslušných e-mailových adres přidat další uživatele. Jakmile budete hotovi, klikněte na **Další**. Přidání všech vašich fakturačních dat do Cloudyn trvá přibližně dvě hodiny.
8. Kliknutím na **Přejít do Cloudyn** otevřete portál Cloudyn a na stránce **Správa cloudových účtů** by se měly zobrazit informace o vašem zaregistrovaném účtu EA.

Pokud chcete zhlédnout videokurz k registraci smlouvy Enterprise, podívejte se na video [How to Find Your EA Enrollment ID and API Key for use in Azure Cost Management by Cloudyn](https://youtu.be/u_phLs_udig) (Vyhledání ID prováděcí smlouvy EA a klíče rozhraní API pro použití ve službě Azure Cost Management by Cloudyn).

[!INCLUDE [cost-management-create-account-view-data](../../includes/cost-management-create-account-view-data.md)]

## <a name="next-steps"></a>Další kroky

V tomto rychlém startu jste použili informace o své smlouvě Azure Enterprise k registraci ve službě Cost Management. Také jste se přihlásili k portálu Cloudyn a začali jste zobrazovat informace o nákladech. Další informace o službě Azure Cost Management by Cloudyn najdete v kurzu pro službu Cost Management.

> [!div class="nextstepaction"]
> [Kontrola využití a nákladů](./tutorial-review-usage.md)
