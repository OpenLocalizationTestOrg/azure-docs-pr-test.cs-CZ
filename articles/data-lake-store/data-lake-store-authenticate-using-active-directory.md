---
title: "Ověřování služby služby: Data Lake Store s Azure Active Directory | Microsoft Docs"
description: "Zjistěte, jak tooachieve service-to-service ověřování s Data Lake Store pomocí Azure Active Directory"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 820b7c5d-4863-4225-9bd1-df4d8f515537
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/21/2017
ms.author: nitinme
ms.openlocfilehash: 2e56237a75f020067b3248a1e1cfaf3c8df1371c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="service-to-service-authentication-with-data-lake-store-using-azure-active-directory"></a>Service-to-service ověřování s Data Lake Store pomocí Azure Active Directory
> [!div class="op_single_selector"]
> * [Ověřování služba-služba](data-lake-store-authenticate-using-active-directory.md)
> * [Ověřování koncových uživatelů](data-lake-store-end-user-authenticate-using-active-directory.md)
> 
> 

Azure Data Lake Store využívá Azure Active Directory pro ověřování. Před vytváření aplikace, která funguje s Azure Data Lake Store nebo Azure Data Lake Analytics, musíte nejprve rozhodnout, jak chcete tooauthenticate vaší aplikace pomocí Azure Active Directory (Azure AD). Hello dvě hlavní možnosti k dispozici jsou:

* Ověřování koncových uživatelů 
* Do služby ověřování (v tomto článku) 

Obě tyto možnosti má za následek aplikace se součástí tokenu OAuth 2.0, který získá připojené tooeach požadavek tooAzure Data Lake Store nebo Azure Data Lake Analytics.

Tento článek rozhovory o vytvoření **webové aplikace Azure AD pro ověřování service-to-service**. Pokyny v konfiguraci aplikace Azure AD pro ověřování koncového uživatele naleznete v části [ověřování koncového uživatele s Data Lake Store pomocí Azure Active Directory](data-lake-store-end-user-authenticate-using-active-directory.md).

## <a name="prerequisites"></a>Požadavky
* Předplatné Azure. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).

## <a name="step-1-create-an-active-directory-web-application"></a>Krok 1: Vytvoření webové aplikace služby Active Directory

Vytvořit a nakonfigurovat webovou aplikaci Azure AD pro service-to-service ověřování s Azure Data Lake Store pomocí Azure Active Directory. Pokyny najdete v tématu [vytvořit aplikaci Azure AD](../azure-resource-manager/resource-group-create-service-principal-portal.md).

Při podle pokynů hello hello výše odkaz, zkontrolujte, zda jste vybrali **webovou aplikaci nebo API** pro typ aplikace, jak ukazuje následující snímek obrazovky hello.

![Vytvořit webovou aplikaci](./media/data-lake-store-authenticate-using-active-directory/azure-active-directory-create-web-app.png "vytvořit webovou aplikaci")

## <a name="step-2-get-application-id-authentication-key-and-tenant-id"></a>Krok 2: Získání id aplikace, kód ověřování a id klienta
Pokud protokolování prostřednictvím kódu programu, je potřeba hello id pro vaši aplikaci. Pokud aplikace hello bude spuštěna pod svoje vlastní přihlašovací údaje, budete také potřebovat ověřovací klíč.

* Pokyny jak ID aplikace hello tooretrieve a ověřovací klíč (také nazývané hello tajný klíč klienta) pro aplikace, najdete v části [ID a ověřovací klíč aplikace Get](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key).

* Pokyny jak tooretrieve hello ID klienta, najdete v části [získání ID tenanta](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-tenant-id).

## <a name="step-3-assign-hello-azure-ad-application-toohello-azure-data-lake-store-account-file-or-folder-only-for-service-to-service-authentication"></a>Krok 3: Přiřaďte hello Azure AD application toohello Azure Data Lake Store účtu souboru nebo složky (pouze pro ověřování service-to-service)
1. Přihlášení toohello nové [portál Azure](https://portal.azure.com) a otevřete hello účtu Azure Data Lake Store, které chcete tooassociate s hello aplikaci Azure Active Directory, které jste vytvořili dříve.
2. V okně účtu Data Lake Store klikněte na možnost **Průzkumník dat**.
   
    ![Vytváření adresářů v účtu Data Lake Store](./media/data-lake-store-authenticate-using-active-directory/adl.start.data.explorer.png "vytváření adresářů v účtu Data Lake")
3. V hello **Průzkumníku dat** okně klikněte na tlačítko hello souboru nebo složky, pro který má aplikace toohello Azure AD tooprovide přístup a pak klikněte na tlačítko **přístup**. soubor tooa tooconfigure přístup, musíte kliknout na **přístup** z hello **náhled souboru** okno.
   
    ![Nastavit seznamy ACL v systému souborů Data Lake](./media/data-lake-store-authenticate-using-active-directory/adl.acl.1.png "nastavit seznamy ACL v Data Lake systému souborů")
4. Hello **přístup** okno uvádí přístup hello standardní a vlastní přístup již přiřazen toohello kořenové. Klikněte na tlačítko hello **přidat** ikonu tooadd úrovni vlastní seznamy ACL.
   
    ![Standardní a vlastní přístup](./media/data-lake-store-authenticate-using-active-directory/adl.acl.2.png "standardní a vlastní přístup")
5. Klikněte na tlačítko hello **přidat** ikonu tooopen hello **přidat vlastní přístup** okno. V tomto okně klikněte na tlačítko **vybrat uživatele nebo skupinu**a potom v **vybrat uživatele nebo skupinu** okno, podívejte se na aplikaci Azure Active Directory hello jste vytvořili dříve. Pokud máte spoustu skupin toosearch z, použijte hello textového pole v horní toofilter hello na název skupiny hello. Klikněte na skupinu hello tooadd a pak klikněte na tlačítko **vyberte**.
   
    ![Přidat skupinu](./media/data-lake-store-authenticate-using-active-directory/adl.acl.3.png "přidat skupinu")
6. Klikněte na tlačítko **vyberte oprávnění**, vyberte hello oprávnění a jestli chcete tooassign hello oprávnění jako výchozí seznam ACL, získat přístup k seznamu ACL, nebo obojí. Klikněte na **OK**.
   
    ![Přiřazení oprávnění toogroup](./media/data-lake-store-authenticate-using-active-directory/adl.acl.4.png "přiřadit oprávnění toogroup")
   
    Další informace o oprávněních v Data Lake Store a seznamy ACL výchozí nebo přístupu najdete v tématu [řízení přístupu v Data Lake Store](data-lake-store-access-control.md).
7. V hello **přidat vlastní přístup** okně klikněte na tlačítko **OK**. Nově přidaná Hello skupiny s oprávněními hello spojené se teď uvedený v hello **přístup** okno.
   
    ![Přiřazení oprávnění toogroup](./media/data-lake-store-authenticate-using-active-directory/adl.acl.5.png "přiřadit oprávnění toogroup")

## <a name="step-4-get-hello-oauth-20-token-endpoint-only-for-java-based-applications"></a>Krok 4: Získání hello koncový bod tokenu OAuth 2.0 (pouze pro aplikace založené na jazyce Java)

1. Přihlášení toohello nové [portál Azure](https://portal.azure.com) a hello levém podokně klikněte na položku služby Active Directory.

2. V levém podokně hello, klikněte na **registrace aplikace**.

3. Z hello horní části okna registrace hello aplikací, klikněte na tlačítko **koncové body**.

    ![Koncový bod tokenu OAuth](./media/data-lake-store-authenticate-using-active-directory/oauth-token-endpoint.png "koncový bod tokenu OAuth")

4. Z hello seznam koncových bodů zkopírujte koncový bod tokenu OAuth 2.0 hello.

    ![Koncový bod tokenu OAuth](./media/data-lake-store-authenticate-using-active-directory/oauth-token-endpoint-1.png "koncový bod tokenu OAuth")   

## <a name="next-steps"></a>Další kroky
V tomto článku jste vytvořili webovou aplikaci Azure AD a shromáždit hello informace, které budete potřebovat v klientských aplikacích vytvářet pomocí .NET SDK, sady Java SDK atd. Nyní můžete přejít toohello následující články, které mluvit o tom, jak ověřit toouse hello Azure AD webové aplikace toofirst s Data Lake Store a poté proveďte jiné operace v úložišti hello.

* [Začínáme s Azure Data Lake Storem pomocí sady .NET SDK](data-lake-store-get-started-net-sdk.md)
* [Začínáme s Azure Data Lake Store pomocí sady Java SDK](data-lake-store-get-started-java-sdk.md)

Tento článek projít můžete hello základní kroky potřebné tooget uživatele hlavní nahoru a spuštěná, aby vaše aplikace. Můžete se podívat na hello následující články tooget Další informace:
* [Použití prostředí PowerShell toocreate instančního objektu](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-authenticate-service-principal)
* [Použití ověřování pomocí certifikátu pro objekt zabezpečení ověřování služby](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-authenticate-service-principal#create-service-principal-with-certificate)
* [Ostatní metody tooauthenticate tooAzure AD](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-authentication-scenarios)


