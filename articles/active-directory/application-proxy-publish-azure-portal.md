---
title: "Publikování aplikací pomocí proxy aplikace služby Azure AD | Dokumentace Microsoftu"
description: "Publikujte místní aplikace do cloudu s Azure AD Application Proxy na portálu Azure."
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
ms.openlocfilehash: e00a939f2b20ab8e0a2ddf0ff91e59db440213ac
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="publish-applications-using-azure-ad-application-proxy"></a>Publikování aplikací pomocí proxy aplikace služby Azure AD

> [!div class="op_single_selector"]
> * [Azure Portal](application-proxy-publish-azure-portal.md)
> * [Portál Azure Classic](active-directory-application-proxy-publish.md)

Proxy aplikace služby Azure Active Directory (AD) umožňuje podporují zaměstnanci na vzdálených pracovištích a publikování aplikací místní přístup přes internet. Můžete publikovat tyto aplikace prostřednictvím portálu Azure poskytnout zabezpečený vzdálený přístup z mimo vaši síť.

Tento článek vás provede kroky k publikování aplikace místně pomocí Proxy aplikace. Po dokončení tohoto článku, bude mít přístup k aplikaci vzdáleně vaši uživatelé. A budete moci konfigurovat další funkce pro aplikaci jako jednotné přihlašování, přizpůsobené informace a požadavky na zabezpečení.

Pokud jste ještě Proxy aplikace, další informace o této funkci s článkem [jak poskytnout zabezpečený vzdálený přístup k místním aplikacím](active-directory-application-proxy-get-started.md).


## <a name="publish-an-on-premises-app-for-remote-access"></a>Publikování aplikace místně pro vzdálený přístup

Postupujte podle těchto kroků k publikování aplikací pomocí Proxy aplikace. Pokud už nejsou stáhnout a nakonfigurovat konektor pro vaši organizaci, přejděte na [začít pracovat s Proxy aplikace a nainstalujte konektor](active-directory-application-proxy-enable.md) první a pak publikování aplikace.

> [!TIP]
> Pokud testujete Proxy aplikace poprvé, vyberte aplikaci, která je nastavena pro ověřování pomocí hesla. Proxy aplikací podporuje další typy ověřování, ale jsou založené na heslech aplikace nejjednodušší ke zprovoznění rychle a spustit. 

1. Přihlaste se jako správce v [portál Azure](https://portal.azure.com/).
2. Vyberte **Azure Active Directory** > **podnikové aplikace, které** > **novou aplikaci**.

  ![Přidat podniková aplikace](./media/application-proxy-publish-azure-portal/add-app.png)

3. Vyberte **všechny**, pak vyberte **místní aplikace**.  

  ![Přidat vlastní aplikaci](./media/application-proxy-publish-azure-portal/add-your-own.png)

4. Zadejte následující informace o vaší aplikaci:

   - **Název**: název aplikace, která se zobrazí na panelu přístup a na portálu Azure. 

   - **Interní adresa URL**: adresa URL, který používáte pro přístup k aplikaci uvnitř vaší privátní sítě. Můžete zadat konkrétní cestu na beck-endovém serveru, kterou chcete publikovat, zatímco zbytek serveru publikovaný nebude. Tímto způsobem můžete publikovat různé stránky na stejném serveru jako různé aplikace a dejte každé z nich vlastní název a pravidla přístupu.

     > [!TIP]
     > Pokud publikujete cestu, ujistěte se, že zahrnuje všechny nezbytné obrázky, skripty a šablony stylů pro vaši aplikaci. Pokud například vaše aplikace je v cestě https://yourapp/app a používá obrázky nacházející se v cestě https://yourapp/media, pak byste měli publikujete https://yourapp/ jako cestu. Tato interní adresa URL nemusí být cílová stránka, která se uživatelům zobrazí. Další informace najdete v tématu [nastavit vlastní domovskou stránku pro publikované aplikace](application-proxy-office365-app-launcher.md).

   - **Externí adresu URL**: adresu vaši uživatelé budou moct pro přístup k aplikaci mimo vaši síť. Pokud nechcete použít výchozí doménu Proxy aplikací, přečtěte si informace o [vlastních domén v Azure AD Application Proxy](active-directory-application-proxy-custom-domains.md).
   - **Předběžné ověřování**: jak Proxy aplikace ověřuje uživatele, než jim poskytne přístup k aplikaci. 

     - Azure Active Directory: Proxy aplikace přesměruje uživatele na stránku pro přihlášení ke službě Azure AD, která ověří jejich oprávnění k adresáři a aplikaci. Doporučujeme zachovat tuto možnost jako výchozí, tak, aby můžete využít výhod funkcí zabezpečení Azure AD jako podmíněného přístupu a vícefaktorového ověřování.
     - Průchod: Uživatelé nemají k ověřování na základě Azure Active Directory pro přístup k aplikaci. Přesto můžete nastavit požadavky na ověřování na back-end.
   - **Konektor skupiny**: konektory zpracovat vzdálený přístup k vaší aplikaci a konektor skupiny k uspořádání konektory a aplikace podle oblasti, sítě nebo účel. Pokud nemáte žádné skupiny konektor dosud vytvořena, vaše aplikace je přiřazen k **výchozí**.

   ![Konfigurace aplikace](./media/application-proxy-publish-azure-portal/configure-app.png)
5. V případě potřeby nakonfigurujte další nastavení. Pro většinu aplikací byste měli mít tato nastavení na jejich výchozí stavy. 
   - **Časový limit pro aplikace k back-end**:, nastavte hodnotu **dlouho** pouze v případě, že aplikace je pomalá k ověření a připojení. 
   - **Překlad adresy URL v záhlaví**: ponechte tuto hodnotu jako **Ano** Pokud aplikace nevyžaduje původní hlavičku hostitele v žádosti o ověření.
   - **Překlad adres URL do těla aplikace**: ponechte tuto hodnotu jako **ne** Pokud máte pevně zakódované HTML odkazy na další místní aplikace a nepoužívejte vlastní domény. Další informace najdete v tématu [odkaz překlad pomocí Proxy aplikace](application-proxy-link-translation.md).
   
   ![Konfigurace aplikace](./media/application-proxy-publish-azure-portal/additional-settings.png)

6. Vyberte **Přidat**.


## <a name="add-a-test-user"></a>Přidání testovacího uživatele 

Pokud chcete otestovat, že aplikace byla publikovaný správně, přidejte testovací uživatelský účet. Ověřte, že tento účet již má oprávnění pro přístup k aplikaci uvnitř podnikové sítě.

1. Zpět v okně rychlý start vyberte **přiřadit uživatele pro testování**.

  ![Přiřazení uživatele pro testování](./media/application-proxy-publish-azure-portal/assign-user.png)

2. Na uživatele a skupiny okně, vyberte **přidat**.

  ![Přidat uživatele nebo skupiny](./media/application-proxy-publish-azure-portal/add-user.png)

3. V okně Přidat přiřazení vyberte **uživatelů a skupin** potom vyberte účet, který chcete přidat. 
4. Vyberte **přiřadit**.

## <a name="test-your-published-app"></a>Testování publikované aplikace

V prohlížeči přejděte na externí adresu URL, kterou jste nakonfigurovali během kroku publikovat. By měl vidět na úvodní obrazovce a mít možnost se přihlásit pomocí účtu test, který nastavíte.

![Testování publikované aplikace](./media/application-proxy-publish-azure-portal/test-app.png)


## <a name="next-steps"></a>Další kroky
- [Stáhnout konektory](active-directory-application-proxy-enable.md) a [vytvořit konektor skupiny](active-directory-application-proxy-connectors-azure-portal.md) k publikování aplikací na samostatných sítí a umístění.

- [Nastavení jednotného přihlašování](application-proxy-sso-azure-portal.md) nově publikované aplikace.
