---
title: "Ověřování koncového uživatele: Data Lake Store s Azure Active Directory | Microsoft Docs"
description: "Zjistěte, jak zajistit ověření koncového uživatele s Data Lake Store pomocí Azure Active Directory"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: ec586ecd-1b42-459e-b600-fadbb7b80a9b
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/21/2017
ms.author: nitinme
ms.openlocfilehash: c20f5c39b00992d801909c8e5de292f3c2f12673
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="end-user-authentication-with-data-lake-store-using-azure-active-directory"></a>Ověření koncových uživatelů s Data Lake Store pomocí Azure Active Directory
> [!div class="op_single_selector"]
> * [Ověřování služba-služba](data-lake-store-authenticate-using-active-directory.md)
> * [Ověřování koncových uživatelů](data-lake-store-end-user-authenticate-using-active-directory.md)
> 
> 

Azure Data Lake Store využívá Azure Active Directory pro ověřování. Před vytváření aplikace, která funguje s Azure Data Lake Store nebo Azure Data Lake Analytics, je třeba nejprve rozhodnout, jak chcete ověřovat vaší aplikace pomocí Azure Active Directory (Azure AD). Jsou dvě hlavní možnosti k dispozici:

* Ověření koncových uživatelů (v tomto článku)
* Ověřování služba-služba

Obě tyto možnosti má za následek aplikace se součástí tokenu OAuth 2.0, který získá připojen k každou žádost odeslanou Azure Data Lake Store nebo Azure Data Lake Analytics.

Tento článek rozhovory o vytvoření **nativní aplikaci Azure AD pro ověřování koncového uživatele**. Pokyny v konfiguraci aplikace Azure AD pro ověřování service-to-service najdete v tématu [Service-to-service ověřování s Data Lake Store pomocí Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

## <a name="prerequisites"></a>Požadavky
* Předplatné Azure. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).

* ID vašeho předplatného. Můžete ji načíst z portálu Azure. Například je k dispozici v okně účtu Data Lake Store.
  
    ![Získání ID předplatného](./media/data-lake-store-end-user-authenticate-using-active-directory/get-subscription-id.png)

* Název domény vaší služby Azure AD. Můžete ji načíst podržením ukazatele myši v pravém horním rohu na portálu Azure. Z na následující snímek obrazovky, je název domény **contoso.onmicrosoft.com**, a identifikátor GUID v závorkách je ID klienta. 
  
    ![Získat doménu AAD](./media/data-lake-store-end-user-authenticate-using-active-directory/get-aad-domain.png)

## <a name="end-user-authentication"></a>Ověřování koncových uživatelů
Toto je doporučený postup, pokud chcete, aby koncovým uživatelem pro přihlášení do aplikace pomocí Azure AD. Vaše aplikace bude mít přístup k prostředkům Azure se stejnou úrovní přístupu jako koncový uživatel, přihlášení. Mohl koncový uživatel muset zadat přihlašovací údaje pravidelně v pořadí pro vaši aplikaci k získání přístupu.

Výsledek toho, že koncový uživatel přihlásit je, že aplikace je daná přístupový token a obnovovací token. Získá přístupový token připojena k každý požadavek na Data Lake Store nebo Data Lake Analytics, a je platná pro jednu hodinu, ve výchozím nastavení. Token obnovení lze použít k získání nového tokenu přístupu, a je platná dobu až dvou týdnů ve výchozím nastavení, pokud se používá pravidelně. Můžete vytvořit dva různé přístupy pro koncového uživatele přihlásit.

### <a name="using-the-oauth-20-pop-up"></a>Pomocí automaticky otevírané okno OAuth 2.0
Aplikaci můžete spustit OAuth 2.0 autorizace automaticky otevírané okno, ve kterém může uživatel zadat své přihlašovací údaje. Toto automaticky otevírané okno taky spolupracuje se službou Azure AD dvoufaktorové ověřování (2FA) proces, podle potřeby. 

> [!NOTE]
> Tato metoda není dosud podporována v Azure AD Authentication Library (ADAL) pro Python nebo Java.
> 
> 

### <a name="directly-passing-in-user-credentials"></a>Předávání přímo v uživatelských přihlašovacích údajů
Aplikace můžete přímo zadat přihlašovací údaje uživatele do služby Azure AD. Tato metoda funguje jenom s účty organizace ID uživatele; není kompatibilní s osobní / "live ID" uživatelské účty, včetně těch, které končí na @outlook.com nebo @live.com. Kromě toho tato metoda není kompatibilní s uživatelskými účty, které vyžadují Azure AD dvoufaktorové ověřování (2FA).

### <a name="what-do-i-need-to-use-this-approach"></a>Co je potřeba tuto metodu použijte?
* Název domény Azure AD. To je již uveden v požadovaných součástí tohoto článku.
* Azure AD **nativní aplikace**
* ID aplikací pro nativní aplikaci Azure AD
* Identifikátor URI přesměrování pro nativní aplikaci Azure AD
* Nastavte delegovaná oprávnění.


## <a name="step-1-create-an-active-directory-native-application"></a>Krok 1: Vytvoření nativní aplikaci služby Active Directory

Vytvořit a nakonfigurovat nativní aplikaci Azure AD pro ověřování koncového uživatele s Azure Data Lake Store pomocí Azure Active Directory. Pokyny najdete v tématu [vytvořit aplikaci Azure AD](../azure-resource-manager/resource-group-create-service-principal-portal.md).

Při těchto pokynů na výše uvedený odkaz, zkontrolujte, zda jste vybrali **nativní** pro typ aplikace, jak je vidět na tomto snímku obrazovky.

![Vytvořit webovou aplikaci](./media/data-lake-store-end-user-authenticate-using-active-directory/azure-active-directory-create-native-app.png "vytvořit nativní aplikace")

## <a name="step-2-get-application-id-and-redirect-uri"></a>Krok 2: Získání id aplikace a identifikátor URI pro přesměrování

V tématu [získání ID aplikace](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key) načíst id aplikace nativní aplikace Azure AD (také nazývané ID klienta na portálu Azure classic).

Chcete-li získat identifikátor URI přesměrování, postupujte podle následujících kroků.

1. Z portálu Azure vyberte **Azure Active Directory**, klikněte na tlačítko **registrace aplikace**a najít a klikněte na nativní aplikaci Azure AD, kterou jste právě vytvořili.

2. Z **nastavení** pro aplikaci, klikněte na **identifikátory URI přesměrování**.

    ![Identifikátor URI pro přesměrování GET](./media/data-lake-store-end-user-authenticate-using-active-directory/azure-active-directory-redirect-uri.png)

3. Zkopírujte zobrazené hodnoty.


## <a name="step-3-set-permissions"></a>Krok 3: Nastavte oprávnění.

1. Z portálu Azure vyberte **Azure Active Directory**, klikněte na tlačítko **registrace aplikace**a najít a klikněte na nativní aplikaci Azure AD, kterou jste právě vytvořili.

2. Z **nastavení** pro aplikaci, klikněte na **požadovaná oprávnění**a potom klikněte na **přidat**.

    ![id klienta](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-set-permission-1.png)

3. V **přidat přístup pomocí rozhraní API** okně klikněte na tlačítko **vybrat rozhraní API**, klikněte na tlačítko **Azure Data Lake**a potom klikněte na **vyberte**.

    ![id klienta](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-set-permission-2.png)
 
4.  V **přidat přístup pomocí rozhraní API** okně klikněte na tlačítko **vyberte oprávnění**, zaškrtněte políčko umožnit **úplný přístup do Data Lake Store**a potom klikněte na **vyberte**.

    ![id klienta](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-set-permission-3.png)

    Klikněte na **Done** (Hotovo).

5. Zopakujte poslední dva kroky k udělení oprávnění pro **Windows Azure Service Management API** také.
   
## <a name="next-steps"></a>Další kroky
V tomto článku jste vytvořili nativní aplikaci Azure AD a shromáždit informace, které budete potřebovat v klientských aplikacích vytvářet pomocí .NET SDK, sady Java SDK, REST API atd. Nyní můžete přejít na následující články, které komunikovat o tom, jak používat webové aplikace Azure AD nejprve ověřit pomocí Data Lake Store a poté provést jiné operace v úložišti.

* [Začínáme s Azure Data Lake Storem pomocí sady .NET SDK](data-lake-store-get-started-net-sdk.md)
* [Začínáme s Azure Data Lake Store pomocí sady Java SDK](data-lake-store-get-started-java-sdk.md)
* [Začínáme s Azure Data Lake Store pomocí rozhraní REST API](data-lake-store-get-started-rest-api.md)

