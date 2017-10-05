---
title: "Požadavky pro přístup k Azure AD reporting rozhraní API. | Dokumentace Microsoftu"
description: "Další informace o požadavcích pro přístup k Azure AD reporting rozhraní API"
services: active-directory
documentationcenter: 
author: dhanyahk
manager: femila
editor: 
ms.assetid: ada19f69-665c-452a-8452-701029bf4252
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/16/2017
ms.author: dhanyahk;markvi
ms.openlocfilehash: 6e409fc56b77f37dac7f37382e664c577666ad4d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="prerequisites-to-access-the-azure-ad-reporting-api"></a>Požadavky na přístup k Azure AD reporting rozhraní API
[Rozhraní API pro generování sestav Azure AD](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview) poskytují programový přístup k datům prostřednictvím sady založené na REST API. Tato rozhraní API můžete volat z nejrůznějších programovacích jazyků a nástrojů.

Generování sestav používá rozhraní API [OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx) autorizovat přístup k webovému rozhraní API. 

Chcete-li připravit váš přístup k rozhraní API pro vytváření sestav, postupujte takto:

1. Vytvoření aplikace v klientovi služby Azure AD 
2. Udělení příslušných oprávnění aplikací pro přístup k datům Azure AD
3. Shromážděte nastavení konfigurace z adresáře

Pro dotazy, problémy nebo připomínky, obraťte se na [AAD Reporting pomoci](mailto:aadreportinghelp@microsoft.com).

## <a name="create-an-azure-ad-application"></a>Vytvořit aplikaci Azure AD
Chcete-li nastavení konfigurace adresáře pro přístup k Azure AD reporting rozhraní API, musíte se přihlásit k portálu Azure classic pomocí účtu správce předplatného Azure, který je taky členem skupiny roli globálního správce adresáře v klientovi služby Azure AD.

> [!IMPORTANT]
> Aplikace běžící v části přihlašovací údaje s oprávněním "admin", jako to může být velmi výkonné, takže Přesvědčte se, k zabezpečení přihlašovacích údajů ID nebo tajný klíč aplikace.
> 
> 

1. V [portál Azure classic](https://manage.windowsazure.com), v levém navigačním podokně klikněte na tlačítko **služby Active Directory**.
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/01.png) 
2. Z **služby active directory** seznamu, vyberte adresář.
3. V nabídce v horní části, klikněte na tlačítko **aplikace**.
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/02.png) 
4. Na dolním panelu klikněte na tlačítko **přidat**.
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/03.png) 
5. Na **co chcete udělat?** dialogové okno, klikněte na tlačítko **přidat aplikaci, kterou vyvíjí Moje organizace**. 
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/04.png) 
6. Na **Řekněte nám o své aplikaci** dialogové okno, proveďte následující kroky: 
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/05.png) 
   
    a. V **název** textovému poli, zadejte název (např: vytváření sestav aplikace rozhraní API).
   
    b. Vyberte **webovou aplikaci nebo webové rozhraní API**.
   
    c. Klikněte na **Další**.
7. Na **vlastností aplikace** dialogové okno, proveďte následující kroky: 
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/06.png) 
   
    a. V **přihlašovací adresa URL** textovému poli, typ `https://localhost`.
   
    b. V **identifikátor ID URI aplikace** textovému poli, typ ```https://localhost/ReportingApiApp```.
   
    c. Klikněte na **Dokončit**.

## <a name="grant-your-application-permission-to-use-the-api"></a>Udělit oprávnění vaše aplikace k používání rozhraní API
1. V [portál Azure classic](https://manage.windowsazure.com/), v levém navigačním podokně klikněte na tlačítko **služby Active Directory**.
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/01.png) 
2. Z **služby active directory** seznamu, vyberte adresář.
3. V nabídce v horní části, klikněte na tlačítko **aplikace**.
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/02.png)
4. V seznamu aplikací vyberte nově vytvořené aplikace.
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/07.png)
5. V nabídce v horní části, klikněte na tlačítko **konfigurace**.
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/08.png)
6. V **oprávnění k ostatním aplikacím** části pro **Azure Active Directory** prostředků, klikněte na tlačítko **oprávnění aplikací** rozevíracího seznamu a potom vyberte **Čtení dat adresáře**.
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/09.png)
7. Na dolním panelu klikněte na tlačítko **Uložit**.
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/10.png)

## <a name="gather-configuration-settings-from-your-directory"></a>Shromážděte nastavení konfigurace z adresáře
V této části se dozvíte, jak získat z adresáře následující nastavení:

* Název domény
* ID klienta
* Tajný klíč klienta

Je nutné tyto hodnoty při konfiguraci volání do rozhraní API pro generování sestav. 

### <a name="get-your-domain-name"></a>Získat název domény
1. V [portál Azure classic](https://manage.windowsazure.com), v levém navigačním podokně klikněte na tlačítko **služby Active Directory**.
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/01.png) 
2. Z **služby active directory** seznamu, vyberte adresář.
3. V nabídce v horní části, klikněte na tlačítko **domény**.
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/11.png) 
4. V **název domény** sloupce, zkopírujte název vaší domény.
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/12.png) 

### <a name="get-the-applications-client-id"></a>Získejte ID klienta aplikace.
1. V [portál Azure classic](https://manage.windowsazure.com), v levém navigačním podokně klikněte na tlačítko **služby Active Directory**.
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/01.png) 
2. Z **služby active directory** seznamu, vyberte adresář.
3. V nabídce v horní části, klikněte na tlačítko **aplikace**.
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/02.png) 
4. V seznamu aplikací vyberte nově vytvořené aplikace.
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/07.png)
5. V nabídce v horní části, klikněte na tlačítko **konfigurace**.
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/08.png)
6. Kopie vašeho **ID klienta**.
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/13.png)

### <a name="get-the-applications-client-secret"></a>Získat sdílený tajný klíč klienta aplikace.
Získat sdílený tajný klíč klienta aplikace, musíte vytvořit nový klíč a uložit jeho hodnota při ukládání nového klíče, protože není možné později už načíst tuto hodnotu.

1. V [portál Azure classic](https://manage.windowsazure.com), v levém navigačním podokně klikněte na tlačítko **služby Active Directory**.
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/01.png) 
2. Z **služby active directory** seznamu, vyberte adresář.
3. V nabídce v horní části, klikněte na tlačítko **aplikace**.
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/02.png) 
4. V seznamu aplikací vyberte nově vytvořené aplikace.
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/07.png)
5. V nabídce v horní části, klikněte na tlačítko **konfigurace**.
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/08.png)
6. V **klíče** část, proveďte následující kroky: 
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/14.png)
   
    a. Ze seznamu dobu trvání vyberte dobu trvání.
   
    b. Na dolním panelu klikněte na tlačítko **Uložit**.
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/10.png)
   
    c. Zkopírujte hodnotu klíče.

## <a name="next-steps"></a>Další kroky
* Chcete pro přístup k datům z Azure AD reporting rozhraní API programové způsobem? Podívejte se na [Začínáme s Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md).
* Pokud chcete získat další informace o vytváření sestav Azure Active Directory, přečtěte si téma [Azure Active Directory průvodce vytvářením sestav](active-directory-reporting-guide.md).  

