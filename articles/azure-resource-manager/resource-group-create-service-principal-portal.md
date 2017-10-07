---
title: "aaaCreate identitu pro aplikace v portálu Azure | Microsoft Docs"
description: "Popisuje, jak toocreate novou aplikaci Azure Active Directory a objektu, který lze použít s hello řízení přístupu na základě rolí v Azure Resource Manager toomanage služby přístup tooresources."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 7068617b-ac5e-47b3-a1de-a18c918297b6
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/15/2017
ms.author: tomfitz
ms.openlocfilehash: 9624715ac612f42df6f9e9e67b8233bd4b914174
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-portal-toocreate-an-azure-active-directory-application-and-service-principal-that-can-access-resources"></a>Použijte portál toocreate aplikaci Azure Active Directory a objektu služby, které mají přístup k prostředkům

Když máte aplikaci, která potřebuje tooaccess nebo úpravám prostředků, musíte nastavit aplikaci Azure Active Directory (AD) a přiřadit tooit hello požadované oprávnění. Tento postup je vhodnější toorunning aplikace hello vlastní oprávnění, protože:

* Můžete přiřadit oprávnění toohello identita aplikace, která se liší od vlastní oprávnění. Obvykle se tato oprávnění se s omezeným přístupem tooexactly jaké aplikace hello musí toodo.
* Pokud vaše odpovědnosti změnit nemají aplikace hello toochange pověření. 
* Při provádění bezobslužného skriptu, můžete použít ověřování pomocí certifikátu tooautomate.

Toto téma ukazuje, jak tooperform ty kroky prostřednictvím portálu hello. Zaměřuje se na jednoho klienta aplikace, kde aplikace hello je určený toorun v rámci organizace jenom jeden. Obvykle používají aplikace jednoho klienta pro-obchodní aplikace, které běží v rámci vaší organizace.
 
## <a name="required-permissions"></a>Požadovaná oprávnění
toocomplete tohoto tématu, musíte mít dostatečná oprávnění tooregister aplikace s klientovi Azure AD a přiřazení role tooa aplikace hello ve vašem předplatném Azure. Ujistíme se, že máte správná oprávnění tooperform hello těchto kroků.

### <a name="check-azure-active-directory-permissions"></a>Zkontrolujte oprávnění služby Azure Active Directory
1. Přihlaste se tooyour účet Azure prostřednictvím hello [portál Azure](https://portal.azure.com).
2. Vyberte **Azure Active Directory**.

     ![Vyberte služby azure active directory](./media/resource-group-create-service-principal-portal/select-active-directory.png)
3. V Azure Active Directory, vyberte **uživatelská nastavení**.

     ![Vyberte nastavení uživatele](./media/resource-group-create-service-principal-portal/select-user-settings.png)
4. Zkontrolujte hello **registrace aplikace** nastavení. Pokud nastavení příliš**Ano**, uživatelé bez oprávnění správce můžete zaregistrovat aplikace AD. Toto nastavení znamená, že každý uživatel v klientovi Azure AD hello můžete zaregistrovat aplikaci. Abyste mohli pokračovat příliš[oprávnění předplatné Azure zkontrolujte](#check-azure-subscription-permissions).

     ![Zobrazit registrace aplikace](./media/resource-group-create-service-principal-portal/view-app-registrations.png)
5. Pokud registrace aplikace hello nastavení je nastaven příliš**ne**jen správce uživatelé mohou registrovat aplikace. Je nutné toocheck, zda je váš účet správce pro tenanta Azure AD hello. Vyberte **přehled** a **najít uživatele** z rychlé úlohy.

     ![Najít uživatele](./media/resource-group-create-service-principal-portal/find-user.png)
6. Vyhledávání pro váš účet a vyberte ho, když ji najít.

     ![Hledat uživatele](./media/resource-group-create-service-principal-portal/show-user.png)
7. Pro váš účet, vyberte **Directory role**. 

     ![role adresáře](./media/resource-group-create-service-principal-portal/select-directory-role.png)
8. Zobrazte vaše role přiřazené adresář ve službě Azure AD. Pokud je váš účet přiřazenou roli uživatele toohello, ale hello registrace nastavením aplikace (z předchozích kroků hello), je omezený tooadmin uživatelé, požádejte vašeho správce tooeither přiřadit můžete tooan roli správce, nebo tooenable uživatelé tooregister aplikace.

     ![role zobrazení](./media/resource-group-create-service-principal-portal/view-role.png)

### <a name="check-azure-subscription-permissions"></a>Zkontrolujte oprávnění předplatného Azure
Ve vašem předplatném Azure, musí mít váš účet `Microsoft.Authorization/*/Write` přístup tooassign role tooa aplikace AD. Tato akce je poskytována prostřednictvím hello [vlastníka](../active-directory/role-based-access-built-in-roles.md#owner) role nebo [správce přístupu uživatelů](../active-directory/role-based-access-built-in-roles.md#user-access-administrator) role. Pokud je váš účet přiřazenou toohello **Přispěvatel** role, nemáte dostatečná oprávnění. Obdržíte chybu při pokusu o role hlavního tooa služby tooassign hello. 

toocheck oprávnění svého předplatného:

1. Nejsou-li již zobrazena v účtu Azure AD z předchozích kroků hello, vyberte **Azure Active Directory** v levém podokně hello.

2. Najít váš účet Azure AD. Vyberte **přehled** a **najít uživatele** z rychlé úlohy.

     ![Najít uživatele](./media/resource-group-create-service-principal-portal/find-user.png)
2. Vyhledávání pro váš účet a vyberte ho, když ji najít.

     ![Hledat uživatele](./media/resource-group-create-service-principal-portal/show-user.png) 
     
3. Vyberte **prostředky Azure**.

     ![Vyberte prostředky](./media/resource-group-create-service-principal-portal/select-azure-resources.png) 
3. Zobrazit přiřazené role a zjistit, jestli máte tooassign odpovídající oprávnění AD aplikace tooa role. Pokud ne, požádejte správce tooadd vaše předplatné můžete tooUser přístupu k roli správce. V hello následující bitové kopie je uživatel hello přiřazené toohello roli vlastníka pro obě předplatná, což znamená, že tento uživatel má odpovídající oprávnění. 

     ![Zobrazit oprávnění](./media/resource-group-create-service-principal-portal/view-assigned-roles.png)

## <a name="create-an-azure-active-directory-application"></a>Vytvoření aplikace Azure Active Directory
1. Přihlaste se tooyour účet Azure prostřednictvím hello [portál Azure](https://portal.azure.com).
2. Vyberte **Azure Active Directory**.

     ![Vyberte služby azure active directory](./media/resource-group-create-service-principal-portal/select-active-directory.png)

4. Vyberte **registrace aplikace**.   

     ![Vyberte aplikaci registrace](./media/resource-group-create-service-principal-portal/select-app-registrations.png)
5. Vyberte **Přidat**.

     ![Přidat aplikaci](./media/resource-group-create-service-principal-portal/select-add-app.png)

6. Zadejte název a adresu URL pro aplikaci hello. Vyberte buď **webovou aplikaci nebo API** nebo **nativní** hello typ aplikace se má toocreate. Po nastavení hodnoty hello, vyberte **vytvořit**.

     ![název aplikace](./media/resource-group-create-service-principal-portal/create-app.png)

Vytvoření vaší aplikace.

## <a name="get-application-id-and-authentication-key"></a>Získat klíč ID a ověřování aplikace
Pokud protokolování prostřednictvím kódu programu, je potřeba hello ID pro vaše aplikace a ověřovací klíč. tooget tyto hodnoty, hello použijte následující kroky:

1. Z **registrace aplikace** v Azure Active Directory, vyberte svou aplikaci.

     ![Vyberte aplikaci](./media/resource-group-create-service-principal-portal/select-app.png)
2. Kopírování hello **ID aplikace** a ukládá je v kódu aplikace. Hello aplikace v hello [ukázkové aplikace](#sample-applications) naleznete v části toothis hodnotu jako id klienta hello.

     ![id klienta](./media/resource-group-create-service-principal-portal/copy-app-id.png)
3. Vyberte toogenerate ověřovací klíč, **klíče**.

     ![Vyberte klíče](./media/resource-group-create-service-principal-portal/select-keys.png)
4. Zadejte popis hello klíče a dobu trvání pro klíč hello. Až budete hotoví, vyberte **Uložit**.

     ![klíč uložit](./media/resource-group-create-service-principal-portal/save-key.png)

     Po uložení hello klíč, je zobrazená hodnota hello hello klíče. Tato hodnota zkopírujte, protože nejste klíč možné tooretrieve hello později. Hodnota klíče hello poskytnete s toolog ID aplikace hello v jako aplikace hello. Uložení klíče hodnoty hello, kde vaše aplikace, můžete jej načíst.

     ![Uložit klíč](./media/resource-group-create-service-principal-portal/copy-key.png)

## <a name="get-tenant-id"></a>Získání ID klienta
Při protokolování prostřednictvím kódu programu, je nutné ID klienta hello toopass s žádostí o ověření. 

1. ID klienta tooget hello, vyberte **vlastnosti** pro vašeho tenanta Azure AD. 

     ![Vyberte vlastnosti, které Azure AD](./media/resource-group-create-service-principal-portal/select-ad-properties.png)

2. Kopírování hello **ID adresáře**. Tato hodnota je vaše ID klienta.

     ![id klienta](./media/resource-group-create-service-principal-portal/copy-directory-id.png)

## <a name="assign-application-toorole"></a>Přiřadit toorole aplikace
tooaccess prostředky v rámci vašeho předplatného, je nutné přiřadit role tooa aplikace hello. Rozhodněte, jakou roli představuje hello správná oprávnění pro aplikace hello. toolearn o hello dostupných rolí, najdete v části [RBAC: integrovaným rolím](../active-directory/role-based-access-built-in-roles.md).

Můžete nastavit obor hello na úrovni hello hello předplatné, skupinu prostředků nebo prostředek. Oprávnění jsou zděděné toolower úrovně rozsahu. Například přidání že aplikační role čtečky toohello pro skupinu prostředků znamená, že můžete číst hello skupinu prostředků a všechny prostředky, které obsahuje.

1. Přejděte toohello úroveň oboru, které chcete aplikaci tooassign hello. Například tooassign role v hello obor předplatného, vyberte **odběry**. Místo toho můžete třeba vybrat skupinu prostředků nebo prostředek.

     ![Vyberte předplatné](./media/resource-group-create-service-principal-portal/select-subscription.png)

2. Vyberte hello určitý odběr (skupinu prostředků nebo prostředek) tooassign hello aplikace.

     ![Vyberte předplatné pro přiřazení](./media/resource-group-create-service-principal-portal/select-one-subscription.png)

3. Vyberte **přístup k ovládacímu prvku (IAM)**.

     ![Vybrat přístupu](./media/resource-group-create-service-principal-portal/select-access-control.png)

4. Vyberte **Přidat**.

     ![Vyberte Přidat](./media/resource-group-create-service-principal-portal/select-add.png)
6. Vyberte roli hello chcete tooassign toohello aplikace. Hello následující obrázek ukazuje hello **čtečky** role.

     ![Vyberte roli](./media/resource-group-create-service-principal-portal/select-role.png)

8. Vyhledávání pro vaši aplikaci a vyberte jej.

     ![Hledat aplikace](./media/resource-group-create-service-principal-portal/search-app.png)
9. Vyberte **OK** toofinish přiřazení hello role. Zobrazí aplikace v seznamu hello uživatelů přiřazenou roli tooa pro tento obor.

## <a name="log-in-as-hello-application"></a>Přihlaste se jako aplikace hello

Aplikace je teď nastavený v Azure Active Directory. Máte ID a klíč toouse pro přihlášení jako aplikace hello. aplikace Hello je přiřazen tooa role, která poskytuje ho určité akce, které můžete provádět. Informace o protokolování jako aplikace hello prostřednictvím různých platforem najdete v tématu:

* [PowerShell](resource-group-authenticate-service-principal.md#provide-credentials-through-powershell)
* [Azure CLI](resource-group-authenticate-service-principal-cli.md#provide-credentials-through-azure-cli)
* [REST](/rest/api/#create-the-request)
* [.NET](/dotnet/azure/dotnet-sdk-azure-authenticate?view=azure-dotnet)
* [Java](/java/azure/java-sdk-azure-authenticate)
* [Node.js](/nodejs/azure/node-sdk-azure-get-started?view=azure-node-2.0.0)
* [Python](/python/azure/python-sdk-azure-authenticate?view=azure-python)
* [Ruby](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)


## <a name="next-steps"></a>Další kroky
* tooset až víceklientské aplikace, najdete v části [tooauthorization Příručka pro vývojáře s hello rozhraní API služby Azure Resource Manager](resource-manager-api-authentication.md).
* toolearn o zadání zásady zabezpečení, najdete v části [řízení přístupu na základě Role v Azure](../active-directory/role-based-access-control-configure.md).  
* Seznam dostupných akcí, které může být povolen nebo odepřen toousers najdete v tématu [poskytovatel prostředků Azure Resource Manager operations](../active-directory/role-based-access-control-resource-provider-operations.md).
