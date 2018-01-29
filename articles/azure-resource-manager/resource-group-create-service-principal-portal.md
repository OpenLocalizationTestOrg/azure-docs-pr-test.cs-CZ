---
title: "Vytvoření identity pro aplikaci Azure portálu | Microsoft Docs"
description: "Popisuje, jak vytvořit novou aplikaci Azure Active Directory a objektu služby, které je možné pomocí řízení přístupu na základě rolí ve službě Správce prostředků Azure, které pokud chcete spravovat přístup k prostředkům."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/16/2018
ms.author: tomfitz
ms.openlocfilehash: 89f2f834c84cd5ae50b2d5b9d46e792065080d8a
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/17/2018
---
# <a name="use-portal-to-create-an-azure-active-directory-application-and-service-principal-that-can-access-resources"></a>Vytvoření aplikace Azure Active Directory a objektu služby, které mají přístup k prostředkům pomocí portálu

Když máte aplikaci, která potřebuje přístup k nebo úpravám prostředků, musíte nastavit aplikaci Azure Active Directory (AD) a přiřadit požadovaná oprávnění. Tento postup je vhodnější spuštění aplikace vlastní oprávnění, protože:

* Můžete přiřadit oprávnění k identitě aplikace, která se liší od vlastní oprávnění. Tato oprávnění jsou obvykle omezené na přesně co aplikaci je třeba provést.
* Nemáte ke změně pověření aplikace, pokud vaše odpovědnosti změnit. 
* Certifikát můžete použít k automatizaci ověřování při provádění bezobslužného skriptu.

Tento článek ukazuje, jak provádět tyto kroky prostřednictvím portálu. Zaměřuje se na jednoho klienta aplikace, kde je záměrem spustit v rámci organizace jenom jedna aplikace. Obvykle používají aplikace jednoho klienta pro-obchodní aplikace, které běží v rámci vaší organizace.

## <a name="required-permissions"></a>Požadovaná oprávnění

K dokončení tohoto článku, musíte mít dostatečná oprávnění k registraci aplikace ve službě klientovi Azure AD a přiřazení aplikace k roli ve vašem předplatném Azure. Ujistíme se, že máte správná oprávnění k provedení těchto kroků.

### <a name="check-azure-active-directory-permissions"></a>Zkontrolujte oprávnění služby Azure Active Directory

1. Přihlaste se k účtu Azure prostřednictvím [portál Azure](https://portal.azure.com).

1. Vyberte **Azure Active Directory**.

   ![Vyberte služby azure active directory](./media/resource-group-create-service-principal-portal/select-active-directory.png)

1. V Azure Active Directory, vyberte **uživatelská nastavení**.

   ![Vyberte nastavení uživatele](./media/resource-group-create-service-principal-portal/select-user-settings.png)

1. Zkontrolujte **registrace aplikace** nastavení. Pokud nastavena na **Ano**, uživatelé bez oprávnění správce můžete zaregistrovat aplikace AD. Toto nastavení znamená, že každý uživatel v klientovi Azure AD můžete zaregistrovat aplikaci. Můžete přejít k [oprávnění předplatné Azure zkontrolujte](#check-azure-subscription-permissions).

   ![Zobrazit registrace aplikace](./media/resource-group-create-service-principal-portal/view-app-registrations.png)

1. Pokud registrace aplikace, nastavení je nastavený na **ne**jen správce uživatelé mohou registrovat aplikace. Zkontrolujte, zda je váš účet správce pro tenanta Azure AD. Vyberte **přehled** a **najít uživatele** z rychlé úlohy.

   ![Najít uživatele](./media/resource-group-create-service-principal-portal/find-user.png)

1. Vyhledávání pro váš účet a vyberte ho, když ji najít.

   ![Hledat uživatele](./media/resource-group-create-service-principal-portal/show-user.png)

1. Pro váš účet, vyberte **Directory role**.

   ![role adresáře](./media/resource-group-create-service-principal-portal/select-directory-role.png)

1. Zobrazte vaše role přiřazené adresář ve službě Azure AD. Pokud je váš účet přiřazenou roli uživatele, ale nastavení registrace aplikace (z předchozích kroků) je omezený na správci, požádejte správce, aby buď přiřadit k roli správce, nebo umožňuje uživatelům registrovat aplikace.

   ![role zobrazení](./media/resource-group-create-service-principal-portal/view-role.png)

### <a name="check-azure-subscription-permissions"></a>Zkontrolujte oprávnění předplatného Azure

Ve vašem předplatném Azure, musí mít váš účet `Microsoft.Authorization/*/Write` přístup k aplikaci AD přiřadit roli. Tato akce je poskytována prostřednictvím [vlastníka](../active-directory/role-based-access-built-in-roles.md#owner) role nebo [správce přístupu uživatelů](../active-directory/role-based-access-built-in-roles.md#user-access-administrator) role. Pokud váš účet je přiřazen k **Přispěvatel** role, nemáte dostatečná oprávnění. Obdržíte chybu při pokusu o přiřazení objektu služby roli.

Zkontrolujte oprávnění svého předplatného:

1. Nejsou-li již zobrazena v účtu Azure AD z předchozích kroků, vyberte **Azure Active Directory** v levém podokně.

1. Najít váš účet Azure AD. Vyberte **přehled** a **najít uživatele** z rychlé úlohy.

   ![Najít uživatele](./media/resource-group-create-service-principal-portal/find-user.png)

1. Vyhledávání pro váš účet a vyberte ho, když ji najít.

   ![Hledat uživatele](./media/resource-group-create-service-principal-portal/show-user.png)

1. Vyberte **prostředky Azure**.

   ![Vyberte prostředky](./media/resource-group-create-service-principal-portal/select-azure-resources.png)

1. Zobrazit přiřazené role a zjistit, pokud máte odpovídající oprávnění k aplikaci AD přiřadit roli. Pokud ne, požádejte správce předplatného můžete přidat do role správce přístupu uživatelů. Na následujícím obrázku je přiřadit uživatele k roli vlastníka pro obě předplatná, což znamená, že tento uživatel má odpovídající oprávnění.

   ![Zobrazit oprávnění](./media/resource-group-create-service-principal-portal/view-assigned-roles.png)

## <a name="create-an-azure-active-directory-application"></a>Vytvoření aplikace Azure Active Directory

1. Přihlaste se k účtu Azure prostřednictvím [portál Azure](https://portal.azure.com).
1. Vyberte **Azure Active Directory**.

   ![Vyberte služby azure active directory](./media/resource-group-create-service-principal-portal/select-active-directory.png)

1. Vyberte **registrace aplikace**.

   ![Vyberte aplikaci registrace](./media/resource-group-create-service-principal-portal/select-app-registrations.png)

1. Vyberte **nové registrace aplikace**.

   ![Přidat aplikaci](./media/resource-group-create-service-principal-portal/select-add-app.png)

1. Zadejte název a adresu URL pro aplikaci. Vyberte **webovou aplikaci nebo API** pro typ aplikace, kterou chcete vytvořit. Nelze vytvořit přihlašovací údaje pro **nativní** aplikace; proto tento typ nelze použít pro automatické aplikaci. Po nastavení hodnoty, vyberte **vytvořit**.

   ![název aplikace](./media/resource-group-create-service-principal-portal/create-app.png)

Vytvoření vaší aplikace.

## <a name="get-application-id-and-authentication-key"></a>Získat klíč ID a ověřování aplikace

Pokud protokolování prostřednictvím kódu programu, je potřeba ID pro vaše aplikace a ověřovací klíč. Tyto hodnoty, použijte následující kroky:

1. Z **registrace aplikace** v Azure Active Directory, vyberte svou aplikaci.

   ![Vyberte aplikaci](./media/resource-group-create-service-principal-portal/select-app.png)

1. Kopírování **ID aplikace** a ukládá je v kódu aplikace. Některé [ukázkové aplikace](#log-in-as-the-application) odkaz na tuto hodnotu jako ID klienta.

   ![ID klienta](./media/resource-group-create-service-principal-portal/copy-app-id.png)

1. Generovat ověřovací klíč, vyberte **klíče**.

   ![Vyberte klíče](./media/resource-group-create-service-principal-portal/select-keys.png)

1. Zadejte popis klíč a dobu trvání pro klíč. Až budete hotoví, vyberte **Uložit**.

   ![klíč uložit](./media/resource-group-create-service-principal-portal/save-key.png)

   Po uložení klíče, se zobrazí hodnota klíče. Zkopírujte tuto hodnotu, protože nemůžete později načíst klíč. Hodnota klíče byl poskytnout ID aplikace pro přihlášení jako aplikace. Uložení klíče hodnoty, kde vaše aplikace, můžete jej načíst.

   ![Uložit klíč](./media/resource-group-create-service-principal-portal/copy-key.png)

## <a name="get-tenant-id"></a>Získání ID klienta

Když protokolování prostřednictvím kódu programu, potřebujete předat ID klienta s žádostí o ověření.

1. Vyberte **Azure Active Directory**.

   ![Vyberte služby azure active directory](./media/resource-group-create-service-principal-portal/select-active-directory.png)

1. Chcete-li získat ID klienta, vyberte **vlastnosti** pro vašeho tenanta Azure AD.

   ![Vyberte vlastnosti, které Azure AD](./media/resource-group-create-service-principal-portal/select-ad-properties.png)

1. Kopírování **ID adresáře**. Tato hodnota je vaše ID klienta.

   ![ID klienta](./media/resource-group-create-service-principal-portal/copy-directory-id.png)

## <a name="assign-application-to-role"></a>Přiřazení aplikace k roli

Pro přístup k prostředkům ve vašem předplatném, je nutné přiřadit aplikace k roli. Rozhodněte, jakou roli představuje správná oprávnění pro aplikaci. Další informace o dostupných rolí najdete v tématu [RBAC: integrovaným rolím](../active-directory/role-based-access-built-in-roles.md).

Rozsah můžete nastavit na úrovni předplatné, skupinu prostředků nebo prostředek. Na nižších úrovních oboru dědí oprávnění. Například přidání aplikace do role Čtenář pro skupinu prostředků znamená, že ho může číst skupině prostředků a všechny prostředky, které obsahuje.

1. Přejděte na úroveň oboru, který chcete přiřadit aplikaci. Například přiřazení role v oboru předplatné, vyberte **odběry**. Místo toho můžete třeba vybrat skupinu prostředků nebo prostředek.

   ![Vyberte předplatné](./media/resource-group-create-service-principal-portal/select-subscription.png)

1. Vyberte určitý odběr (skupinu prostředků nebo prostředek) aplikaci přiřadit.

   ![Vyberte předplatné pro přiřazení](./media/resource-group-create-service-principal-portal/select-one-subscription.png)

1. Vyberte **přístup k ovládacímu prvku (IAM)**.

   ![Vybrat přístupu](./media/resource-group-create-service-principal-portal/select-access-control.png)

1. Vyberte **Přidat**.

   ![Vyberte Přidat](./media/resource-group-create-service-principal-portal/select-add.png)

1. Vyberte roli, kterou chcete přiřadit k aplikaci. Na následujícím obrázku **čtečky** role.

   ![Vyberte roli](./media/resource-group-create-service-principal-portal/select-role.png)

1. Vyhledávání pro vaši aplikaci a vyberte jej.

   ![Hledat aplikace](./media/resource-group-create-service-principal-portal/search-app.png)

1. Vyberte **Uložit** k dokončení přiřazení role. Zobrazí aplikace v seznamu Uživatelé s přiřazenou rolí pro tento obor.

## <a name="log-in-as-the-application"></a>Přihlaste se jako aplikace

Aplikace je teď nastavený v Azure Active Directory. Máte ID a klíč, použít pro přihlášení jako aplikace. Aplikace je přiřadit roli, která poskytuje ho určité akce, které můžete provádět. Informace o protokolování jako aplikace prostřednictvím různých platforem najdete v tématu:

* [PowerShell](resource-group-authenticate-service-principal.md#provide-credentials-through-powershell)
* [Azure CLI](resource-group-authenticate-service-principal-cli.md)
* [REST](/rest/api/#create-the-request)
* [.NET](/dotnet/azure/dotnet-sdk-azure-authenticate?view=azure-dotnet)
* [Java](/java/azure/java-sdk-azure-authenticate)
* [Node.js](/nodejs/azure/node-sdk-azure-get-started?view=azure-node-2.0.0)
* [Python](/python/azure/python-sdk-azure-authenticate?view=azure-python)
* [Ruby](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)

## <a name="next-steps"></a>Další postup
* Víceklientské aplikace, naleznete v tématu [Příručka pro vývojáře k autorizaci s rozhraním API pro Azure Resource Manager](resource-manager-api-authentication.md).
* Další informace o určení zásady zabezpečení najdete v tématu [řízení přístupu na základě Role v Azure](../active-directory/role-based-access-control-configure.md).  
* Seznam dostupných akcí, které může být povolen nebo odepřen uživatelům najdete v tématu [poskytovatel prostředků Azure Resource Manager operations](../active-directory/role-based-access-control-resource-provider-operations.md).
