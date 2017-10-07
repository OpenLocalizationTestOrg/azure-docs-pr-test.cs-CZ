---
title: "aaaConfigure ověřování a autorizace pro vlastní aplikaci, která volá rozhraní API pro Azure časové řady přehledy hello | Microsoft Docs"
description: "Tento kurz popisuje, jak tooconfigure ověřování a autorizace pro vlastní aplikaci, která volá rozhraní API pro Azure časové řady přehledy hello"
keywords: 
services: time-series-insights
documentationcenter: 
author: dmdenmsft
manager: almineev
editor: cgronlun
ms.assetid: 
ms.service: time-series-insights
ms.devlang: na
ms.topic: how-to-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/24/2017
ms.author: dmden
ms.openlocfilehash: 5043468bfc2af3c0d27e8602508d92ba2848409e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="authentication-and-authorization-for-azure-time-series-insights-api"></a>Ověřování a autorizace pro rozhraní API pro Azure časové řady přehledy

Tento článek vysvětluje, jak hello tooconfigure vlastní aplikaci, která volá rozhraní API pro Azure časové řady přehledy.

## <a name="service-principal"></a>Instanční objekt

Tato část vysvětluje, jak tooconfigure tooaccess aplikace hello rozhraní API pro přehledy časové řady jménem aplikace hello. aplikace Hello následně zadávat dotazy na data nebo publikování referenční data v prostředí časové řady Statistika hello se přihlašovací údaje aplikací a hello pověření uživatele.

Když máte aplikaci, která potřebuje tooaccess časové řady statistiky, musíte nastavit aplikaci Azure Active Directory a přiřadit hello zásady přístupu k datům v prostředí časové řady Statistika hello. Tento postup je vhodnější toorunning aplikace hello vlastní oprávnění, protože:

* Můžete přiřadit oprávnění toohello identita aplikace, která se liší od vlastní oprávnění. Obvykle se tato oprávnění se s omezeným přístupem tooexactly jaké aplikace hello musí toodo. Například můžete povolit, že tooonly aplikace hello číst data v prostředí s konkrétní Statistika časové řady.
* Pokud vaše odpovědnosti změnit nemáte aplikace hello toochange přihlašovací údaje.
* Pokud používáte bezobslužného skriptu, můžete použít certifikát nebo klíče tooautomate ověřování aplikace.

Tento článek ukazuje, jak hello tooperform ty kroky prostřednictvím portálu Azure. Zaměřuje se na jednoho klienta aplikace, kde aplikace hello je určený toorun v pouze jedné organizaci. Jednoho klienta aplikace se obvykle používají pro-obchodní aplikace, které běží ve vaší organizaci.

tok Hello instalační program se skládá z tři hlavní kroky:

1. Vytvoření aplikace v Azure Active Directory.
2. Autorizovat tuto aplikaci tooaccess hello časové řady Statistika prostředí.
3. Pomocí ID aplikace hello a klíče tooacquire tokenu toohello `"https://api.timeseries.azure.com/"` cílové skupiny nebo prostředku. Hello token pak lze použít toocall hello rozhraní API pro přehledy časové řady.

Tady jsou hello podrobné kroky:

1. V hello portálu Azure, vyberte **Azure Active Directory** > **registrace aplikace** > **nové registrace aplikace**.

   ![Nové registrace aplikace v Azure Active Directory](media/authentication-and-authorization/active-directory-new-application-registration.png)  

2. Poskytují aplikace hello toobe typu názvem, vyberte hello **webovou aplikaci nebo rozhraní API**, vyberte libovolný platný identifikátor URI pro **přihlašovací adresa URL**a klikněte na tlačítko **vytvořit**.

   ![Vytvoření aplikace hello v Azure Active Directory](media/authentication-and-authorization/active-directory-create-web-api-application.png)

3. Vyberte nově vytvořené aplikace a zkopírujte jeho aplikace ID tooyour oblíbeném textovém editoru.

   ![Zkopírujte ID aplikace hello](media/authentication-and-authorization/active-directory-copy-application-id.png)

4. Vyberte **klíče**, zadejte název klíče hello, vyberte hello vypršení platnosti a klikněte na **Uložit**.

   ![Výběr aplikace klíče](media/authentication-and-authorization/active-directory-application-keys.png)

   ![Zadejte název klíče hello a vypršení platnosti a klikněte na tlačítko Uložit](media/authentication-and-authorization/active-directory-application-keys-save.png)

5. Zkopírujte hello klíče tooyour oblíbeném textovém editoru.

   ![Zkopírovat klíč aplikace hello](media/authentication-and-authorization/active-directory-copy-application-key.png)

6. Hello časové řady Statistika prostředí, vyberte **zásady přístupu k datům** a klikněte na tlačítko **přidat**.

   ![Přidat nové data přístup zásad toohello časové řady Přehled prostředí](media/authentication-and-authorization/time-series-insights-data-access-policies-add.png)

7. V hello **vyberte uživatele** dialogové okno, vložte název aplikace hello (z kroku 2) nebo ID aplikace (z kroku 3).

   ![Vyhledání aplikace v dialogovém okně vyberte uživatele hello](media/authentication-and-authorization/time-series-insights-data-access-policies-select-user.png)

8. Vyberte hello role (**čtečky** pro dotazování na data, **Přispěvatel** pro dotazování dat a změna referenční data) a klikněte na tlačítko **Ok**.

   ![Vyberte v dialogovém okně vybrat roli hello čtečky nebo přispěvatele](media/authentication-and-authorization/time-series-insights-data-access-policies-select-role.png)

9. Uložit zásadu hello kliknutím **Ok**.

10. Pomocí ID aplikace hello (z kroku 3) a token hello klíče (z kroku 5) tooacquire aplikace jménem aplikace hello. Hello token je pak možné předat v hello `Authorization` záhlaví při volání aplikace hello hello rozhraní API pro přehledy časové řady.

    Pokud používáte C#, můžete použít následující kód tooacquire hello token jménem aplikace hello hello. Kompletní příklad, najdete v části [dotaz na data pomocí jazyka C#](time-series-insights-query-data-csharp.md).

    ```csharp
    var authenticationContext = new AuthenticationContext(
        "https://login.microsoftonline.com/common",
        TokenCache.DefaultShared);

    AuthenticationResult token = await authenticationContext.AcquireTokenAsync(
        // Set hello resource URI toohello Azure Time Series Insights API
        resource: "https://api.timeseries.azure.com/", 
        clientCredential: new ClientCredential(
            // Application ID of application registered in Azure Active Directory
            clientId: "1bc3af48-7e2f-4845-880a-c7649a6470b8", 
            // Application key of hello application that's registered in Azure Active Directory
            clientSecret: "aBcdEffs4XYxoAXzLB1n3R2meNCYdGpIGBc2YC5D6L2="));

    string accessToken = token.AccessToken;
    ```

## <a name="next-steps"></a>Další kroky

Pomocí ID aplikace hello a klíč ve vaší aplikaci. Ukázkový kód, který volá hello rozhraní API pro přehledy časové řady, najdete v části [dotaz na data pomocí jazyka C#](time-series-insights-query-data-csharp.md).

## <a name="see-also"></a>Viz také

* [Dotaz na rozhraní API](/rest/api/time-series-insights/time-series-insights-reference-queryapi) pro hello úplné dotazu API – referenční informace
* [Vytvoření služby hlavní v hello portálu Azure](../azure-resource-manager/resource-group-create-service-principal-portal.md)
