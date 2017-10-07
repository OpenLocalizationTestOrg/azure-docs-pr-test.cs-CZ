---
title: "generování sestav rozhraní API aaaPrerequisites tooaccess hello Azure AD | Microsoft Docs"
description: "Další informace o vytváření sestav API hello požadavky tooaccess hello Azure AD"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: ada19f69-665c-452a-8452-701029bf4252
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/15/2017
ms.author: dhanyahk;markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: ec28a7530f341dda31268a978754b615c727d66f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="prerequisites-tooaccess-hello-azure-ad-reporting-api"></a>Generování sestav rozhraní API požadavky tooaccess hello Azure AD

Hello [rozhraní API pro generování sestav Azure AD](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview) poskytují programový přístup k datům toohello pomocí sady založené na REST API. Tato rozhraní API můžete volat z nejrůznějších programovacích jazyků a nástrojů.

Hello reporting používá rozhraní API [OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx) tooauthorize přístup toohello webových rozhraní API. 

tooget přístup prostřednictvím rozhraní API hello toohello data pro generování sestav, budete potřebovat toohave hello následující role přiřazené:

- Čtečka zabezpečení
- Správce zabezpečení
- Globální správce.


tooprepare váš přístup toohello reporting rozhraní API, musíte:

1. Registrace aplikace 
2. Udělení oprávnění 
3. Shromážděte nastavení konfigurace 

Pro dotazy, problémy nebo připomínky, prosím [souboru lístek podpory](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-troubleshooting-support-howto).

## <a name="register-an-azure-active-directory-application"></a>Zaregistrovat aplikaci Azure Active Directory

Je nutné tooregister aplikace i v případě, že se připojujete hello reporting rozhraní API pomocí skriptu. To vám dává **ID aplikace**, což je vyžadováno pro volání autorizace a umožňuje, aby váš kód tooreceive tokeny.

tooconfigure directory tooaccess hello Azure AD reporting rozhraní API, musíte se přihlásit toohello portálu Azure pomocí účtu správce služby Azure, který je taky členem skupiny hello **globálního správce** role adresáře v klientovi služby Azure AD .

> [!IMPORTANT]
> Aplikace běžící v části přihlašovací údaje s oprávněním "admin", jako to může být velmi výkonné, takže je potřeba se tookeep hello ID nebo tajný klíč přihlašovací údaje pro aplikaci zabezpečené.
> 


**tooregister aplikaci Azure Active Directory:**

1. V hello [portál Azure](https://portal.azure.com), na levém navigačním podokně text hello, klikněte na **služby Active Directory**.
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/01.png) 

2. Na hello **Azure Active Directory** okně klikněte na tlačítko **registrace aplikace**.

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/02.png) 

3. Na hello **registrace aplikace** klikněte na okno na hello nástrojů v horní části hello **nové registrace aplikace**.

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/03.png)

4. Na hello **vytvořit** okně provést hello následující kroky:

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/04.png)

    a. V hello **název** textovému poli, typ `Reporting API application`.

    b. Jako **typ aplikace**, vyberte **webovou aplikaci nebo API**.

    c. V hello **přihlašovací adresa URL** textovému poli, typ `https://localhost`.

    d. Klikněte na možnost **Vytvořit**. 


## <a name="grant-permissions"></a>Udělení oprávnění 

cíl Hello tohoto kroku je toogrant aplikace **čtení dat adresáře** oprávnění toohello **Windows Azure Active Directory** rozhraní API.

![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/16.png)
 

**toogrant hello toouse oprávnění vaší aplikace API:**

1. Na hello **registrace aplikace** klikněte na okno, v seznamu aplikací hello **aplikace Reporting rozhraní API**.

2. Na hello **aplikace Reporting rozhraní API** klikněte na okno na hello nástrojů v horní části hello **nastavení**. 

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/05.png)

3. Na hello **nastavení** okně klikněte na tlačítko **požadovaná oprávnění**. 

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/06.png)

4. Na hello **požadovaná oprávnění** okno, v hello **rozhraní API** seznamu, klikněte na tlačítko **Windows Azure Active Directory**. 

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/07.png)

5. Na hello **povolit přístup** vyberte **čtení dat adresáře**. 

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/08.png)

6. V panelu nástrojů hello hello nahoře, klikněte na **Uložit**.

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/15.png)

## <a name="gather-configuration-settings"></a>Shromážděte nastavení konfigurace 
V této části se dozvíte, jak tooget hello následujících nastavení z adresáře:

* Název domény
* ID klienta
* Tajný klíč klienta

Je nutné tyto hodnoty při konfiguraci volání toohello reporting API. 

### <a name="get-your-domain-name"></a>Získat název domény

**tooget název domény:**

1. V hello [portál Azure](https://portal.azure.com), na levém navigačním podokně text hello, klikněte na **služby Active Directory**.
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/01.png) 

2. Na hello **Azure Active Directory** okně klikněte na tlačítko **názvy domén**.

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/09.png) 

3. Zkopírujte název vaší domény z hello seznam domén.


### <a name="get-your-applications-client-id"></a>Získejte ID klienta aplikace

**tooget ID klienta aplikace:**

1. V hello [portál Azure](https://portal.azure.com), na levém navigačním podokně text hello, klikněte na **služby Active Directory**.
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/01.png) 

2. Na hello **registrace aplikace** klikněte na okno, v seznamu aplikací hello **aplikace Reporting rozhraní API**.

3. Na hello **aplikace Reporting rozhraní API** okno, v hello **ID aplikace**, klikněte na tlačítko **klikněte na tlačítko toocopy**.

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/11.png) 



### <a name="get-your-applications-client-secret"></a>Získat sdílený tajný klíč klienta vaší aplikace
tooget vaší aplikace klienta tajný, potřebujete toocreate nový klíč a uložte jeho hodnota při ukládání hello nový klíč, protože není možné tooretrieve tato hodnota už později.

**tooget tajný klíč klienta aplikace:**

1. V hello [portál Azure](https://portal.azure.com), na levém navigačním podokně text hello, klikněte na **služby Active Directory**.
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/01.png) 

2. Na hello **registrace aplikace** klikněte na okno, v seznamu aplikací hello **aplikace Reporting rozhraní API**.


3. Na hello **aplikace Reporting rozhraní API** klikněte na okno na hello nástrojů v horní části hello **nastavení**. 

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/05.png)

4. Na hello **nastavení** okno, v hello **APIR přístup** klikněte na tlačítko **klíče**. 

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/12.png)


5. Na hello **klíče** okně provést hello následující kroky:

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/14.png)

    a. V hello **popis** textovému poli, typ `Reporting API`.

    b. Jako **Expires**, vyberte **ve 2 roky**.

    c. Klikněte na **Uložit**.

    d. Zkopírujte hodnotu klíče hello.


## <a name="next-steps"></a>Další kroky
* Programová způsobem můžete jako tooaccess hello data z hello Azure AD by reporting rozhraní API? Podívejte se na [Začínáme s Azure Active Directory Reporting API hello](active-directory-reporting-api-getting-started.md).
* Pokud chcete toofind Další informace o vytváření sestav Azure Active Directory, přečtěte si téma hello [Azure Active Directory průvodce vytvářením sestav](active-directory-reporting-guide.md).  

