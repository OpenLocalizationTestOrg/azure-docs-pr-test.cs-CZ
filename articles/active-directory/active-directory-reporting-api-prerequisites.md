---
title: "aaaPrerequisites tooaccess hello Azure AD reporting rozhraní API. | Dokumentace Microsoftu"
description: "Další informace o vytváření sestav API hello požadavky tooaccess hello Azure AD"
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
ms.openlocfilehash: e9d7ceaedb07d18fbd75b70d68b5cfbebc756c36
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="prerequisites-tooaccess-hello-azure-ad-reporting-api"></a>Generování sestav rozhraní API požadavky tooaccess hello Azure AD
Hello [rozhraní API pro generování sestav Azure AD](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview) poskytují programový přístup k datům toohello pomocí sady založené na REST API. Tato rozhraní API můžete volat z nejrůznějších programovacích jazyků a nástrojů.

Hello reporting používá rozhraní API [OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx) tooauthorize přístup toohello webových rozhraní API. 

tooprepare váš přístup toohello reporting rozhraní API, musíte:

1. Vytvoření aplikace v klientovi služby Azure AD 
2. Udělení hello aplikace příslušná oprávnění tooaccess hello data služby Azure AD
3. Shromážděte nastavení konfigurace z adresáře

Pro dotazy, problémy nebo připomínky, obraťte se na [AAD Reporting pomoci](mailto:aadreportinghelp@microsoft.com).

## <a name="create-an-azure-ad-application"></a>Vytvořit aplikaci Azure AD
tooconfigure directory tooaccess hello Azure AD reporting rozhraní API, musíte se přihlásit toohello portál Azure classic pomocí účtu správce předplatného Azure, který je také členem hello globální správce adresáře role v klientovi služby Azure AD.

> [!IMPORTANT]
> Aplikace běžící v části přihlašovací údaje s oprávněním "admin", jako to může být velmi výkonné, takže je potřeba se tookeep hello ID nebo tajný klíč přihlašovací údaje pro aplikaci zabezpečené.
> 
> 

1. V hello [portál Azure classic](https://manage.windowsazure.com), na levém navigačním podokně text hello, klikněte na **služby Active Directory**.
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/01.png) 
2. Z hello **služby active directory** seznamu, vyberte adresář.
3. V nabídce hello hello nahoře, klikněte na tlačítko **aplikace**.
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/02.png) 
4. Na dolním panelu hello, klikněte na tlačítko **přidat**.
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/03.png) 
5. Na hello **co chcete toodo?** dialogové okno, klikněte na tlačítko **přidat aplikaci, kterou vyvíjí Moje organizace**. 
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/04.png) 
6. Na hello **Řekněte nám o své aplikaci** dialogové okno, proveďte následující kroky hello: 
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/05.png) 
   
    a. V hello **název** textovému poli, zadejte název (např: vytváření sestav aplikace rozhraní API).
   
    b. Vyberte **webovou aplikaci nebo webové rozhraní API**.
   
    c. Klikněte na **Další**.
7. Na hello **vlastností aplikace** dialogové okno, proveďte následující kroky hello: 
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/06.png) 
   
    a. V hello **přihlašovací adresa URL** textovému poli, typ `https://localhost`.
   
    b. V hello **identifikátor ID URI aplikace** textovému poli, typ ```https://localhost/ReportingApiApp```.
   
    c. Klikněte na **Dokončit**.

## <a name="grant-your-application-permission-toouse-hello-api"></a>Udělení oprávnění vaší aplikace toouse hello rozhraní API
1. V hello [portál Azure classic](https://manage.windowsazure.com/), na levém navigačním podokně text hello, klikněte na **služby Active Directory**.
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/01.png) 
2. Z hello **služby active directory** seznamu, vyberte adresář.
3. V nabídce hello hello nahoře, klikněte na tlačítko **aplikace**.
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/02.png)
4. V seznamu aplikace hello vyberte nově vytvořené aplikace.
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/07.png)
5. V nabídce hello hello nahoře, klikněte na tlačítko **konfigurace**.
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/08.png)
6. V hello **oprávnění tooother aplikace** části pro hello **Azure Active Directory** prostředků, klikněte na tlačítko hello **oprávnění aplikací** rozevíracího seznamu a potom Vyberte **čtení dat adresáře**.
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/09.png)
7. Na dolním panelu hello, klikněte na tlačítko **Uložit**.
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/10.png)

## <a name="gather-configuration-settings-from-your-directory"></a>Shromážděte nastavení konfigurace z adresáře
V této části se dozvíte, jak tooget hello následujících nastavení z adresáře:

* Název domény
* ID klienta
* Tajný klíč klienta

Je nutné tyto hodnoty při konfiguraci volání toohello reporting API. 

### <a name="get-your-domain-name"></a>Získat název domény
1. V hello [portál Azure classic](https://manage.windowsazure.com), na levém navigačním podokně text hello, klikněte na **služby Active Directory**.
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/01.png) 
2. Z hello **služby active directory** seznamu, vyberte adresář.
3. V nabídce hello hello nahoře, klikněte na tlačítko **domény**.
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/11.png) 
4. V hello **název domény** sloupce, zkopírujte název vaší domény.
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/12.png) 

### <a name="get-hello-applications-client-id"></a>Získání ID klienta aplikace hello
1. V hello [portál Azure classic](https://manage.windowsazure.com), na levém navigačním podokně text hello, klikněte na **služby Active Directory**.
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/01.png) 
2. Z hello **služby active directory** seznamu, vyberte adresář.
3. V nabídce hello hello nahoře, klikněte na tlačítko **aplikace**.
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/02.png) 
4. V seznamu aplikace hello vyberte nově vytvořené aplikace.
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/07.png)
5. V nabídce hello hello nahoře, klikněte na tlačítko **konfigurace**.
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/08.png)
6. Kopie vašeho **ID klienta**.
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/13.png)

### <a name="get-hello-applications-client-secret"></a>Získat sdílený tajný klíč klienta aplikace hello
tooget vaší aplikace klienta tajný, potřebujete toocreate nový klíč a uložte jeho hodnota při ukládání hello nový klíč, protože není možné tooretrieve tato hodnota už později.

1. V hello [portál Azure classic](https://manage.windowsazure.com), na levém navigačním podokně text hello, klikněte na **služby Active Directory**.
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/01.png) 
2. Z hello **služby active directory** seznamu, vyberte adresář.
3. V nabídce hello hello nahoře, klikněte na tlačítko **aplikace**.
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/02.png) 
4. V seznamu aplikace hello vyberte nově vytvořené aplikace.
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/07.png)
5. V nabídce hello hello nahoře, klikněte na tlačítko **konfigurace**.
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/08.png)
6. V hello **klíče** část, proveďte následující kroky hello: 
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/14.png)
   
    a. Ze seznamu dobu trvání hello vyberte dobu trvání.
   
    b. Na dolním panelu hello, klikněte na tlačítko **Uložit**.
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/10.png)
   
    c. Zkopírujte hodnotu klíče hello.

## <a name="next-steps"></a>Další kroky
* Programová způsobem můžete jako tooaccess hello data z hello Azure AD by reporting rozhraní API? Podívejte se na [Začínáme s Azure Active Directory Reporting API hello](active-directory-reporting-api-getting-started.md).
* Pokud chcete toofind Další informace o vytváření sestav Azure Active Directory, přečtěte si téma hello [Azure Active Directory průvodce vytvářením sestav](active-directory-reporting-guide.md).  

