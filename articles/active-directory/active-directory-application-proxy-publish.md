---
title: "aaaPublish aplikací pomocí proxy aplikace služby Azure AD | Microsoft Docs"
description: "Na portálu classic hello publikujte místní aplikace toohello cloudu s Azure AD Application Proxy."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: d94ac3f4-cd33-4c51-9d19-544a528637d4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/14/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro; oldportal
ms.openlocfilehash: 7926998314c65521ae48aebcceb33cb0c67e0b87
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="publish-applications-using-azure-ad-application-proxy"></a>Publikování aplikací pomocí proxy aplikace služby Azure AD

> [!div class="op_single_selector"]
> * [Azure Portal](application-proxy-publish-azure-portal.md)
> * [Portál Azure Classic](active-directory-application-proxy-publish.md)

Proxy aplikace služby Azure AD umožňuje podporu zaměstnanci na vzdálených pracovištích tím, že publikujete místní aplikace toobe přístupné přes hello Internetu. Pomocí tohoto bodu, byste již měli mít [povolit Proxy aplikace v portálu Azure classic hello](active-directory-application-proxy-enable.md). Tento článek vás provede hello kroky toopublish aplikace, které jsou spuštěny v místní síti a poskytnout zabezpečený vzdálený přístup z mimo vaši síť. Po dokončení tohoto článku, budete aplikace hello připravené tooconfigure s přizpůsobené informace nebo požadavky na zabezpečení.

> [!NOTE]
> Proxy aplikace je funkce, která je k dispozici pouze v případě, že jste upgradovali toohello Premium nebo Basic edice služby Azure Active Directory. Další informace najdete v článku [Edice služby Azure Active Directory](active-directory-editions.md). Pokud chcete, aby toouse Proxy aplikace, můžete [publikování aplikací v hello portál Azure](application-proxy-publish-azure-portal.md).

## <a name="publish-an-app-using-hello-wizard"></a>Publikování aplikace pomocí Průvodce hello
1. Přihlaste se jako správce v hello [portál Azure classic](https://manage.windowsazure.com/).
2. Přejděte tooActive adresář a vyberte hello adresář, kde jste povolili Proxy aplikace.
   
    ![Active Directory – ikona](./media/active-directory-application-proxy-publish/ad_icon.png)
3. Klikněte na tlačítko hello **aplikace** a pak klikněte hello **přidat** tlačítko dole hello úvodní obrazovka
   
    ![Přidání aplikace](./media/active-directory-application-proxy-publish/aad_appproxy_selectdirectory.png)
4. Vyberte **Publikování aplikace, která bude přístupná mimo vaši síť**.
   
    ![Publikování aplikace, která bude přístupná mimo vaši síť](./media/active-directory-application-proxy-publish/aad_appproxy_addapp.png)
5. Zadejte následující informace o vaší aplikaci hello:
   
   * **Název**: hello uživatelsky přívětivý název vaší aplikace. Musí být v rámci adresáře jedinečný.
   * **Interní adresa URL**: hello adresu, která hello konektor Proxy aplikace používá tooaccess aplikaci hello uvnitř vaší privátní sítě. Při publikování hello zbytek hello serveru, můžete zadat konkrétní cestu na hello back-end serveru toopublish. Tímto způsobem můžete publikovat různé stránky na stejný server hello a poskytněte každé z nich vlastní název a pravidla přístupu.
     
     > [!TIP]
     > Pokud publikujete cestu, ujistěte se, že obsahuje všechny potřebné obrázků hello, skriptů a stylů pro vaši aplikaci. Například pokud vaše aplikace je v https://yourapp/app a používá nacházející se v https://yourapp/media bitové kopie, pak byste měli publikovat https://yourapp/ jako cestu hello.
     > 
     > 
   * **Metoda předběžného ověření**: jak Proxy aplikace ověřuje uživatele před poskytnete jim přístup tooyour aplikace. Vyberte jednu z možností hello z rozevírací nabídky hello.
     
     * Azure Active Directory: Proxy aplikace přesměruje uživatele toosign pomocí Azure AD, která ověří jejich oprávnění k hello adresáři a aplikaci.
     * Průchod: Uživatelé nemají tooauthenticate tooaccess hello aplikace.
     
     ![Vlastnosti aplikace](./media/active-directory-application-proxy-publish/aad_appproxy_appproperties.png)  
6. toofinish hello průvodce, klikněte na tlačítko zaškrtnutí hello v hello dolní části obrazovky hello. Hello aplikace je nyní definována ve službě Azure AD.

## <a name="assign-users-and-groups-toohello-application"></a>Přiřazení uživatelů a skupin toohello aplikace
V pořadí pro vaši uživatelé tooaccess k publikované aplikaci, musíte tooassign je samostatně nebo ve skupinách. (Nezapomeňte tooassign sami příliš přístup.) Každý uživatel, kterého přiřadíte, musí mít licenci Azure úrovně Basic nebo vyšší. Můžete přiřadit licence jednotlivě nebo toogroups. Další informace najdete v tématu [přiřazování uživatelů aplikace tooan](active-directory-applications-guiding-developers-assigning-users.md). 

Pro aplikace, které vyžadují předběžné ověření přiřazení uživatele uděluje oprávnění toouse hello aplikace. Pro aplikace, které nevyžadují předběžné ověření, přiřazení uživatele znamená, že tento uživatel hello hello k aplikaci přístup prostřednictvím hello přístupového panelu.

1. Po dokončení Průvodce přidáním aplikace hello najdete v části hello úvodní stránky pro vaši aplikaci. Vyberte toomanage, kdo má přístup k aplikaci toohello, **uživatelů a skupin**.
   
    ![Přiřazování uživatelů ze stránky „Rychlý start“ proxy aplikace – snímek obrazovky](./media/active-directory-application-proxy-publish/aad_appproxy_usersgroups.png)
2. Vyhledejte konkrétní skupinu v adresáři nebo si zobrazte všechny uživatele. výsledky hledání hello toodisplay, klikněte na tlačítko zaškrtnutí hello.
   
      ![Hledání skupin nebo uživatelů – snímek obrazovky](./media/active-directory-application-proxy-publish/aad_appproxy_search.png)
3. Vyberte jednotlivé uživatele nebo skupinu tooassign toothis aplikace a klikněte na tlačítko **přiřadit**. Jste vyzváni tooconfirm tuto akci.

> [!NOTE]
> K aplikacím, které využívají integrované ověřování systému Windows, můžete přiřadit pouze uživatele a skupiny synchronizované s místní službou Active Directory. Hosty a uživatele, kteří se přihlašují pomocí účtu Microsoft, nelze k aplikacím publikovaným pomocí proxy aplikace služby Azure Active Directory přiřadit. Ujistěte se, vaši uživatelé přihlašovali pomocí přihlašovacích údajů, které jsou součástí hello stejné doméně jako publikovaná aplikace hello.
> 
> 

## <a name="test-your-published-application"></a>Test publikované aplikace
Po publikování aplikace můžete otestovat ho tak, že přejdete toohello adresu URL, kterou jste publikovali. Ujistěte se, že k ní máte přístup, že se správně vykresluje a že všechno funguje podle očekávání. Pokud máte potíže nebo se zobrazí chybové hlášení, zkuste hello [Průvodce odstraňováním potíží s](active-directory-application-proxy-troubleshoot.md).

## <a name="configure-your-application"></a>Konfigurace aplikace
Můžete publikované aplikace upravovat nebo nastavovat pokročilé možnosti na stránce konfigurace hello. Na této stránce můžete svou aplikaci přizpůsobit změnou hello názvu nebo nahráním loga. Můžete také spravovat pravidla přístupu k jako hello metodu předběžného ověření nebo vícefaktorové ověřování.

![Pokročilá konfigurace](./media/active-directory-application-proxy-publish/aad_appproxy_configure.png)

Po publikování aplikací pomocí Proxy Azure Active Directory aplikace, se objeví v seznamu aplikace hello ve službě Azure AD a je můžete spravovat.

Pokud zakážete služby Proxy aplikace po publikování aplikace, aplikace hello již nejsou dostupné z oblasti mimo vaší privátní sítě. Uživatelé můžou stále přístup hello aplikace místní jako obvykle.

tooview aplikaci a zkontrolujte jestli to přístupný, dvakrát klikněte na název hello aplikace hello. Pokud je zakázána hello Proxy aplikace služby a aplikace hello není k dispozici, zobrazí se zpráva upozornění hello horní části úvodní obrazovka.

Vyberte aplikaci v seznamu hello toodelete aplikaci a potom klikněte na **odstranit**.

## <a name="next-steps"></a>Další kroky
* [Publikování aplikací s použitím vlastního názvu domény](active-directory-application-proxy-custom-domains.md)
* [Povolení jednoduchého přihlášení](active-directory-application-proxy-sso-using-kcd.md)
* [Povolení podmíněného přístupu](active-directory-application-proxy-conditional-access.md)
* [Práce s aplikacemi využívajícími deklarace](active-directory-application-proxy-claims-aware-apps.md)

Hello nejnovější novinky a aktualizace, najdete na naší hello [blogu Proxy aplikace](http://blogs.technet.com/b/applicationproxyblog/)

