---
title: "aaaGet začít s Azure Active Directory Identity Protection a Microsoft Graph | Microsoft Docs"
description: "Poskytuje úvod tooquery Microsoft Graph seznam rizikových událostech a přidružené informace ze služby Azure Active Directory."
services: active-directory
keywords: "ochrany identit Azure active directory, riziko událostí, ohrožení zabezpečení, zásady zabezpečení, Microsoft Graph"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: fa109ba7-a914-437b-821d-2bd98e681386
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 75b8b7629a0120d8101f6fde0d9163122503d276
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-active-directory-identity-protection-and-microsoft-graph"></a>Začínáme s Azure Active Directory Identity Protection a Microsoft Graph
Microsoft Graph je hello Microsoft unified koncový bod rozhraní API a hello domácí z [Azure Active Directory Identity Protection](active-directory-identityprotection.md) rozhraní API. Hello prvního rozhraní API, **identityRiskEvents**, vám umožní tooquery Microsoft Graph seznam z [rizik události](active-directory-identityprotection-risk-events-types.md) a související informace. Tento článek vám pomůže začít dotazování toto rozhraní API. Podrobný úvod, úplnou dokumentaci a přístup toohello grafu Explorer najdete v tématu hello [Microsoft Graph lokality](https://graph.microsoft.io/).

> [!IMPORTANT]
> Společnost Microsoft doporučuje, která můžete spravovat Azure AD pomocí hello [centra pro správu Azure AD](https://aad.portal.azure.com) v hello hello portál Azure místo použití portálu Azure classic, kterou se odkazuje v tomto článku.

Existují tři kroky tooaccessing Identity Protection data prostřednictvím Microsoft Graph:

1. Přidáte aplikaci s tajný klíč klienta. 
2. Použijte tento tajný klíč a několik dalších informace tooauthenticate tooMicrosoft grafu, kterou budete dostávat ověřovací token. 
3. Použijte tento koncový bod tokenu toomake požadavky toohello rozhraní API a vrátit data pro ochranu Identity.

Než začnete, budete potřebovat:

* Správce oprávnění toocreate hello aplikace ve službě Azure AD
* Hello název domény vašeho klienta (například contoso.onmicrosoft.com)

## <a name="add-an-application-with-a-client-secret"></a>Přidat aplikaci s tajný klíč klienta
1. [Přihlaste se](https://manage.windowsazure.com) tooyour portál Azure classic jako správce. 
2. V klikněte v levém navigačním podokně hello na **služby Active Directory**. 
   
    ![Vytváření aplikací](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_01.png)
3. Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.
4. V nabídce hello hello nahoře, klikněte na tlačítko **aplikace**.
   
    ![Vytváření aplikací](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_02.png)
5. Klikněte na tlačítko **přidat** na hello dolní části stránky hello.
   
    ![Vytváření aplikací](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_03.png)
6. Na hello **co chcete toodo** dialogové okno, klikněte na tlačítko **přidat aplikaci, kterou vyvíjí Moje organizace**.
   
    ![Vytváření aplikací](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_04.png)
7. Na hello **Řekněte nám o své aplikaci** dialogové okno, proveďte následující kroky hello:
   
    ![Vytváření aplikací](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_05.png)
   
    a. V hello **název** textovému poli, zadejte název vaší aplikace (např: aplikace AADIP riziko událostí rozhraní API).
   
    b. Jako **typ**, vyberte **webové aplikace nebo webové rozhraní API**.
   
    c. Klikněte na **Další**.
8. Na hello **vlastností aplikace** dialogové okno, proveďte následující kroky hello:
   
    ![Vytváření aplikací](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_06.png)
   
    a. V hello **přihlašovací adresa URL** textovému poli, typ `http://localhost`.
   
    b. V hello **identifikátor ID URI aplikace** textovému poli, typ `http://localhost`.
   
    c. Klikněte na **Dokončit**.

Můžete teď konfigurovat vaší aplikace.

![Vytváření aplikací](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_07.png)

## <a name="grant-your-application-permission-toouse-hello-api"></a>Udělení oprávnění vaší aplikace toouse hello rozhraní API
1. Na stránce vaší aplikace, v nabídce hello hello nahoře, klikněte na **konfigurace**. 
   
    ![Vytváření aplikací](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_08.png)
2. V hello **oprávnění tooother aplikace** klikněte na tlačítko **přidat aplikaci**.
   
    ![Vytváření aplikací](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_09.png)
3. Na hello **oprávnění tooother aplikace** dialogové okno, proveďte následující kroky hello:
   
    ![Vytváření aplikací](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_10.png)
   
    a. Vyberte **Microsoft Graph**.
   
    b. Klikněte na **Dokončit**.
4. Klikněte na tlačítko **oprávnění aplikací: 0**a potom vyberte **čtení informací o události riziko všechny identity**.
   
    ![Vytváření aplikací](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_11.png)
5. Klikněte na tlačítko **Uložit** v hello dolní části stránky hello.
   
    ![Vytváření aplikací](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_12.png)

## <a name="get-an-access-key"></a>Získání přístupového klíče
1. Na stránce vaší aplikace v hello **klíče** vyberte 1 rok jako doba trvání.
   
    ![Vytváření aplikací](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_13.png)
2. Klikněte na tlačítko **Uložit** v hello dolní části stránky hello.
   
    ![Vytváření aplikací](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_12.png)
3. v části klíče hello hello hodnotu nově vytvořený klíč zkopírujte a vložte jej do bezpečného umístění.
   
    ![Vytváření aplikací](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_14.png)
   
   > [!NOTE]
   > Pokud tento klíč ztratíte, bude mít tooreturn toothis části a vytvořte nový klíč. Zachovat tento klíč tajný klíč: každý, kdo je přístup ke svým datům.
   > 
   > 
4. V hello **vlastnosti** část, kopie hello **ID klienta**a pak ji vložit do bezpečného umístění. 

## <a name="authenticate-toomicrosoft-graph-and-query-hello-identity-risk-events-api"></a>Ověření tooMicrosoft grafu a dotaz hello Identity riziko události rozhraní API
V tomto okamžiku byste měli mít:

* ID klienta Hello, které jste zkopírovali výše
* Hello klíče, který jste zkopírovali výše
* Hello název domény vašeho klienta

tooauthenticate, odeslání a post požadavku příliš`https://login.microsoft.com` s hello následující parametry v textu hello:

* grant_type: "**client_credentials**"
* prostředek: "**https://graph.microsoft.com**"
* client_id:<your client ID>
* tajný klíč client_secret:<your key>

> [!NOTE]
> Potřebujete tooprovide hodnoty pro hello **client_id** a hello **tajný klíč client_secret** parametr.
> 
> 

V případě úspěchu vrátí ověřovací token.  
hello toocall rozhraní API, vytvoří hlavičku s hello následující parametr:

    `Authorization`=”<token_type> <access_token>"


Při ověřování, můžete najít typ tokenu hello a přístupový token v hello vrátil tokenu.

Odesílat hlavičku jako požadavek toohello, následující adresy URL rozhraní API:`https://graph.microsoft.com/beta/identityRiskEvents`

odpověď Hello, pokud bylo úspěšné, je kolekce identity rizikových událostech a související data ve formátu OData JSON, který lze analyzovat a zpracovávají podle svých potřeb hello.

Tady je ukázkový kód pro ověřování a volání rozhraní API hello pomocí prostředí Powershell.  
Stačí přidat vaše ID klienta, klíče a klienta domény.

    $ClientID       = "<your client ID here>"        # Should be a ~36 hex character string; insert your info here
    $ClientSecret   = "<your client secret here>"    # Should be a ~44 character string; insert your info here
    $tenantdomain   = "<your tenant domain here>"    # For example, contoso.onmicrosoft.com

    $loginURL       = "https://login.microsoft.com"
    $resource       = "https://graph.microsoft.com"

    $body       = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}
    $oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body

    Write-Output $oauth

    if ($oauth.access_token -ne $null) {
        $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}

        $url = "https://graph.microsoft.com/beta/identityRiskEvents"
        Write-Output $url

        $myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)

        foreach ($event in ($myReport.Content | ConvertFrom-Json).value) {
            Write-Output $event
        }

    } else {
        Write-Host "ERROR: No Access Token"
    } 


## <a name="next-steps"></a>Další kroky
Blahopřejeme, jste právě provedli první volání tooMicrosoft grafu.  
Nyní můžete dotazovat identity rizikových událostech a používat hello data, ale svých potřeb.

toolearn Další informace o Microsoft Graph a jak hello toobuild aplikací pomocí rozhraní Graph API, projděte si hello [dokumentace](https://graph.microsoft.io/docs) zdaleka na hello [Microsoft Graph lokality](https://graph.microsoft.io/). Zajistěte také, zda text hello toobookmark [Azure AD Identity Protection API](https://graph.microsoft.io/docs/api-reference/beta/resources/identityprotection_root) stránky, která obsahuje seznam všech hello API ochrany Identity k dispozici v grafu. Jak jsme přidat nové způsoby toowork s ochranou Identity prostřednictvím rozhraní API, uvidíte je na této stránce.

## <a name="additional-resources"></a>Další zdroje
* [Ochrany identit Azure Active Directory](active-directory-identityprotection.md)
* [Typy rizikových událostí detekovaných službou Azure Active Directory Identity Protection](active-directory-identityprotection-risk-events-types.md)
* [Microsoft Graph](https://graph.microsoft.io/)
* [Přehled Microsoft Graph](https://graph.microsoft.io/docs)
* [Kořenovém adresáři služby Azure AD Identity Protection](https://graph.microsoft.io/docs/api-reference/beta/resources/identityprotection_root)

