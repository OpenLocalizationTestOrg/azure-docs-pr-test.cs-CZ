---
title: "aaaPublish aplikací pomocí proxy aplikace služby Azure AD | Microsoft Docs"
description: "Publikujte místní aplikace toohello cloudu s Azure AD Application Proxy v hello portálu Azure."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: d94ac3f4-cd33-4c51-9d19-544a528637d4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: ed5458467fb7d4376f65a222f1ba5f23cfdfc57c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="publish-applications-using-azure-ad-application-proxy"></a>Publikování aplikací pomocí proxy aplikace služby Azure AD

> [!div class="op_single_selector"]
> * [Azure Portal](application-proxy-publish-azure-portal.md)
> * [Portál Azure Classic](active-directory-application-proxy-publish.md)

Proxy aplikace služby Azure Active Directory (AD) umožňuje podporu zaměstnanci na vzdálených pracovištích tím, že publikujete místní aplikace toobe přístupné přes hello Internetu. Můžete publikovat tyto aplikace prostřednictvím hello Azure portálu tooprovide zabezpečený vzdálený přístup mimo vaši síť.

Tento článek vás provede kroky toopublish hello místní aplikace pomocí Proxy aplikací. Po dokončení tohoto článku, uživatelé budou mít možnost tooaccess aplikace vzdáleně. A budete mít připravené tooconfigure další funkce pro aplikaci hello jako jednotné přihlašování, přizpůsobené informace a požadavky na zabezpečení.

Pokud jste nový tooApplication Proxy, další informace o této funkci s hello článku [jak tooprovide zabezpečený vzdálený přístup tooon místní aplikace](active-directory-application-proxy-get-started.md).


## <a name="publish-an-on-premises-app-for-remote-access"></a>Publikování aplikace místně pro vzdálený přístup

Postupujte podle těchto kroků toopublish aplikací pomocí Proxy aplikace. Pokud už nejsou stáhnout a nakonfigurovat konektor pro vaši organizaci, přejděte příliš[začít pracovat s Proxy aplikace a nainstalujte konektor hello](active-directory-application-proxy-enable.md) první a pak publikování aplikace.

> [!TIP]
> Pokud testujete aplikace Proxy pro hello poprvé, vyberte aplikaci, která je nastavena pro ověřování pomocí hesla. Proxy aplikací podporuje další typy ověřování, ale jsou založené na heslech aplikace hello nejjednodušší tooget si rychle a spustit. 

1. Přihlaste se jako správce v hello [portál Azure](https://portal.azure.com/).
2. Vyberte **Azure Active Directory** > **podnikové aplikace, které** > **novou aplikaci**.

  ![Přidat podniková aplikace](./media/application-proxy-publish-azure-portal/add-app.png)

3. Vyberte **všechny**, pak vyberte **místní aplikace**.  

  ![Přidat vlastní aplikaci](./media/application-proxy-publish-azure-portal/add-your-own.png)

4. Zadejte následující informace o vaší aplikaci hello:

   - **Název**: název hello hello aplikace, která se zobrazí na panelu hello přístupu a v hello portálu Azure. 

   - **Interní adresa URL**: hello URL, kterou používají aplikace hello tooaccess uvnitř vaší privátní sítě. Při publikování hello zbytek hello serveru, můžete zadat konkrétní cestu na hello back-end serveru toopublish. Tímto způsobem můžete publikovat různé stránky na stejném serveru jako různé aplikace hello a poskytněte každé z nich vlastní název a pravidla přístupu.

     > [!TIP]
     > Pokud publikujete cestu, ujistěte se, že obsahuje všechny potřebné obrázků hello, skriptů a stylů pro vaši aplikaci. Například pokud vaše aplikace je v https://yourapp/app a používá nacházející se v https://yourapp/media bitové kopie, pak byste měli publikovat https://yourapp/ jako cestu hello. Tato interní adresa URL nemá toobe hello cílová stránka, která se uživatelům zobrazí. Další informace najdete v tématu [nastavit vlastní domovskou stránku pro publikované aplikace](application-proxy-office365-app-launcher.md).

   - **Externí adresu URL**: hello adresu vaši uživatelé pak budou muset tooin pořadí tooaccess hello aplikace z mimo vaši síť. Pokud nechcete, aby toouse hello výchozí Proxy aplikace doménu, přečtěte si informace o [vlastních domén v Azure AD Application Proxy](active-directory-application-proxy-custom-domains.md).
   - **Předběžné ověřování**: jak Proxy aplikace ověřuje uživatele před poskytnete jim přístup tooyour aplikace. 

     - Azure Active Directory: Proxy aplikace přesměruje uživatele toosign pomocí Azure AD, která ověří jejich oprávnění k hello adresáři a aplikaci. Doporučujeme zachovat tuto možnost jako výchozí hello tak, aby můžete využít výhod funkcí zabezpečení Azure AD jako podmíněného přístupu a vícefaktorového ověřování.
     - Průchod: Uživatelé nemají tooauthenticate proti aplikace hello tooaccess Azure Active Directory. Přesto můžete nastavit požadavky na ověřování na back-end hello.
   - **Konektor skupiny**: konektory proces hello vzdáleného přístupu tooyour aplikace a konektor skupiny vám pomáhají uspořádat konektory a aplikace podle oblasti, sítě nebo účel. Pokud nemáte žádné skupiny konektor dosud vytvořena, vaše aplikace je příliš přiřazen**výchozí**.

   ![Konfigurace aplikace](./media/application-proxy-publish-azure-portal/configure-app.png)
5. V případě potřeby nakonfigurujte další nastavení. Pro většinu aplikací byste měli mít tato nastavení na jejich výchozí stavy. 
   - **Časový limit pro aplikace k back-end**: nastavení této hodnoty příliš**dlouho** pouze v případě, že aplikace je pomalá tooauthenticate a připojení. 
   - **Překlad adresy URL v záhlaví**: ponechte tuto hodnotu jako **Ano** Pokud aplikace nevyžaduje hello původní hlavičku hostitele v žádosti o ověření hello.
   - **Překlad adres URL do těla aplikace**: ponechte tuto hodnotu jako **ne** Pokud máte pevně zakódované HTML odkazy tooother místní aplikace a nepoužívejte vlastní domény. Další informace najdete v tématu [odkaz překlad pomocí Proxy aplikace](application-proxy-link-translation.md).
   
   ![Konfigurace aplikace](./media/application-proxy-publish-azure-portal/additional-settings.png)

6. Vyberte **Přidat**.


## <a name="add-a-test-user"></a>Přidání testovacího uživatele 

tootest, že aplikace byla publikovaný správně, přidejte testovací uživatelský účet. Ověřte, že tento účet již má oprávnění tooaccess hello aplikace z uvnitř podnikové sítě hello.

1. Zpět v okně rychlý start hello, vyberte **přiřadit uživatele pro testování**.

  ![Přiřazení uživatele pro testování](./media/application-proxy-publish-azure-portal/assign-user.png)

2. Hello uživatelů a skupin okně, vyberte **přidat**.

  ![Přidat uživatele nebo skupiny](./media/application-proxy-publish-azure-portal/add-user.png)

3. V okně přiřazení hello přidat, vyberte **uživatelů a skupin** zvolte účet hello chcete tooadd. 
4. Vyberte **přiřadit**.

## <a name="test-your-published-app"></a>Testování publikované aplikace

V prohlížeči přejděte externí adresu URL toohello, který jste nakonfigurovali během hello publikování krok. Byste měli vidět hello úvodní obrazovce a být schopný toosign se pomocí účtu testovací hello nastavíte.

![Testování publikované aplikace](./media/application-proxy-publish-azure-portal/test-app.png)


## <a name="next-steps"></a>Další kroky
- [Stáhnout konektory](active-directory-application-proxy-enable.md) a [vytvořit konektor skupiny](active-directory-application-proxy-connectors-azure-portal.md) toopublish aplikací na samostatných sítí a umístění.

- [Nastavení jednotného přihlašování](application-proxy-sso-azure-portal.md) nově publikované aplikace.
