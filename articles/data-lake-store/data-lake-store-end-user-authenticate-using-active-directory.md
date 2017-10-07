---
title: "Ověřování koncového uživatele: Data Lake Store s Azure Active Directory | Microsoft Docs"
description: "Zjistěte, jak tooachieve ověřování koncového uživatele s Data Lake Store pomocí Azure Active Directory"
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
ms.openlocfilehash: fd58f4f2d8fc915b8bc51d9e5b040d2cee34047e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="end-user-authentication-with-data-lake-store-using-azure-active-directory"></a>Ověření koncových uživatelů s Data Lake Store pomocí Azure Active Directory
> [!div class="op_single_selector"]
> * [Ověřování služba-služba](data-lake-store-authenticate-using-active-directory.md)
> * [Ověřování koncových uživatelů](data-lake-store-end-user-authenticate-using-active-directory.md)
> 
> 

Azure Data Lake Store využívá Azure Active Directory pro ověřování. Před vytváření aplikace, která funguje s Azure Data Lake Store nebo Azure Data Lake Analytics, musíte nejprve rozhodnout, jak chcete tooauthenticate vaší aplikace pomocí Azure Active Directory (Azure AD). Hello dvě hlavní možnosti k dispozici jsou:

* Ověření koncových uživatelů (v tomto článku)
* Ověřování služba-služba

Obě tyto možnosti má za následek aplikace se součástí tokenu OAuth 2.0, který získá připojené tooeach požadavek tooAzure Data Lake Store nebo Azure Data Lake Analytics.

Tento článek rozhovory o vytvoření **nativní aplikaci Azure AD pro ověřování koncového uživatele**. Pokyny v konfiguraci aplikace Azure AD pro ověřování service-to-service najdete v tématu [Service-to-service ověřování s Data Lake Store pomocí Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

## <a name="prerequisites"></a>Požadavky
* Předplatné Azure. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).

* ID vašeho předplatného. Můžete ji načíst z hello portálu Azure. Například je k dispozici v okně účtu Data Lake Store hello.
  
    ![Získání ID předplatného](./media/data-lake-store-end-user-authenticate-using-active-directory/get-subscription-id.png)

* Název domény vaší služby Azure AD. Můžete jej načíst po výběru ukázáním hello myši v pravém horním rohu hello hello portálu Azure. Z hello snímku obrazovky je název domény hello **contoso.onmicrosoft.com**, a hello identifikátor GUID v závorkách je ID hello klienta. 
  
    ![Získat doménu AAD](./media/data-lake-store-end-user-authenticate-using-active-directory/get-aad-domain.png)

## <a name="end-user-authentication"></a>Ověřování koncových uživatelů
Toto je hello doporučenému přístupu, pokud chcete toolog koncového uživatele v aplikaci tooyour přes Azure AD. Vaše aplikace bude mít tooaccess prostředků Azure s hello stejnou úroveň přístupu jako hello koncového uživatele, který je přihlášen. Váš koncový uživatel potřebovat tooprovide přihlašovacích pravidelně v pořadí pro přístup k vaší aplikaci toomaintain.

Hello výsledek toho, že koncový uživatel hello přihlášení je, že aplikace je daná přístupový token a obnovovací token. Získá přístupový token Hello připojené tooeach požadavek tooData Lake Store nebo Data Lake Analytics a je platná pro jednu hodinu, ve výchozím nastavení. token obnovení Hello lze použít tooobtain nový přístupový token ale platí pro až tootwo týdny ve výchozím nastavení, pokud se používá pravidelně. Můžete vytvořit dva různé přístupy pro koncového uživatele přihlásit.

### <a name="using-hello-oauth-20-pop-up"></a>Pomocí automaticky otevírané okno hello OAuth 2.0
Aplikaci můžete spustit OAuth 2.0 autorizace automaticky otevírané okno, ve které hello koncového uživatele můžete zadat své přihlašovací údaje. Toto automaticky otevírané okno funguje taky s procesem hello Azure AD dvoufaktorové ověřování (2FA), v případě potřeby. 

> [!NOTE]
> Tato metoda není dosud podporována v hello Azure AD Authentication Library (ADAL) pro Python nebo Java.
> 
> 

### <a name="directly-passing-in-user-credentials"></a>Předávání přímo v uživatelských přihlašovacích údajů
Aplikace můžete přímo zadat tooAzure přihlašovací údaje uživatele AD. Tato metoda funguje jenom s účty organizace ID uživatele; není kompatibilní s osobní / "live ID" uživatelské účty, včetně těch, které končí na @outlook.com nebo @live.com. Kromě toho tato metoda není kompatibilní s uživatelskými účty, které vyžadují Azure AD dvoufaktorové ověřování (2FA).

### <a name="what-do-i-need-toouse-this-approach"></a>Co dělat potřebuji toouse tento přístup?
* Název domény Azure AD. To je již uveden v požadavku hello tohoto článku.
* Azure AD **nativní aplikace**
* ID aplikací pro nativní aplikace hello Azure AD
* Identifikátor URI přesměrování pro hello nativní aplikaci Azure AD
* Nastavte delegovaná oprávnění.


## <a name="step-1-create-an-active-directory-native-application"></a>Krok 1: Vytvoření nativní aplikaci služby Active Directory

Vytvořit a nakonfigurovat nativní aplikaci Azure AD pro ověřování koncového uživatele s Azure Data Lake Store pomocí Azure Active Directory. Pokyny najdete v tématu [vytvořit aplikaci Azure AD](../azure-resource-manager/resource-group-create-service-principal-portal.md).

Při podle pokynů hello hello výše odkaz, zkontrolujte, zda jste vybrali **nativní** pro typ aplikace, jak ukazuje následující snímek obrazovky hello.

![Vytvořit webovou aplikaci](./media/data-lake-store-end-user-authenticate-using-active-directory/azure-active-directory-create-native-app.png "vytvořit nativní aplikace")

## <a name="step-2-get-application-id-and-redirect-uri"></a>Krok 2: Získání id aplikace a identifikátor URI pro přesměrování

V tématu [získání ID aplikace hello](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key) tooretrieve hello id aplikace (také nazývané hello ID klienta v hello portál Azure classic) nativní aplikace hello Azure AD.

tooretrieve hello identifikátor URI pro přesměrování, postupujte podle následujících kroků hello.

1. Hello portálu Azure, vyberte **Azure Active Directory**, klikněte na tlačítko **registrace aplikace**a najít a klikněte na tlačítko nativní aplikace hello Azure AD, kterou jste právě vytvořili.

2. Z hello **nastavení** hello aplikaci, klikněte na **identifikátory URI přesměrování**.

    ![Identifikátor URI pro přesměrování GET](./media/data-lake-store-end-user-authenticate-using-active-directory/azure-active-directory-redirect-uri.png)

3. Zkopírujte zobrazené hodnoty hello.


## <a name="step-3-set-permissions"></a>Krok 3: Nastavte oprávnění.

1. Hello portálu Azure, vyberte **Azure Active Directory**, klikněte na tlačítko **registrace aplikace**a najít a klikněte na tlačítko nativní aplikace hello Azure AD, kterou jste právě vytvořili.

2. Z hello **nastavení** hello aplikaci, klikněte na **požadovaná oprávnění**a potom klikněte na **přidat**.

    ![id klienta](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-set-permission-1.png)

3. V hello **přidat přístup pomocí rozhraní API** okně klikněte na tlačítko **vybrat rozhraní API**, klikněte na tlačítko **Azure Data Lake**a potom klikněte na **vyberte**.

    ![id klienta](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-set-permission-2.png)
 
4.  V hello **přidat přístup pomocí rozhraní API** okně klikněte na tlačítko **vyberte oprávnění**, vyberte hello políčko toogive **plný přístup k úložišti Lake tooData**a potom klikněte na **vyberte** .

    ![id klienta](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-set-permission-3.png)

    Klikněte na **Done** (Hotovo).

5. Hello zopakujte poslední dva kroky toogrant oprávnění pro **Windows Azure Service Management API** také.
   
## <a name="next-steps"></a>Další kroky
V tomto článku jste vytvořili nativní aplikaci Azure AD a shromáždit hello informace, které potřebujete ve vaší klientské aplikace vytvářet pomocí .NET SDK, sady Java SDK, REST API atd. Nyní můžete přejít toohello následující články, které mluvit o tom, jak ověřit toouse hello Azure AD webové aplikace toofirst s Data Lake Store a poté proveďte jiné operace v úložišti hello.

* [Začínáme s Azure Data Lake Storem pomocí sady .NET SDK](data-lake-store-get-started-net-sdk.md)
* [Začínáme s Azure Data Lake Store pomocí sady Java SDK](data-lake-store-get-started-java-sdk.md)
* [Začínáme s Azure Data Lake Store pomocí rozhraní REST API](data-lake-store-get-started-rest-api.md)

