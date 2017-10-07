---
title: "konektory portálu Proxy aplikace Azure AD aaaClassic | Microsoft Docs"
description: "Zahrnuje jak toocreate a spravovat skupiny konektorů v Azure AD Application Proxy."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: b283796a-9679-4c79-b703-802bb850f65d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro; oldportal
ms.openlocfilehash: 43559b0f4ffc3c7dbbf00901e89ac276d01796e2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="publish-applications-on-separate-networks-and-locations-using-connector-groups"></a>Publikování aplikací na samostatných sítí a umístění pomocí konektoru skupin
> [!div class="op_single_selector"]
> * [Azure Portal](active-directory-application-proxy-connectors-azure-portal.md)
> * [Portál Azure Classic](active-directory-application-proxy-connectors.md)
>
>

Konektor skupiny jsou užitečné pro různé scénáře, včetně:

* Weby s několik datových center vzájemně propojena. V takovém případě chcete tookeep tolik provoz v rámci datového centra hello nejdříve protože datacenter křížové odkazy jsou nákladné a pomalé. Můžete nasadit konektorů v každé datacenter tooserve pouze hello aplikace, které se nacházejí v rámci datového centra hello. Tento postup minimalizuje datacenter křížové odkazy a poskytuje zcela transparentní tooyour uživatele.
* Správa aplikace nainstalované v izolovaných sítích, které nejsou součástí hello hlavní podnikové síti. V izolovaných sítích tooalso izolovat aplikace toohello síti můžete použít konektory tooinstall vyhrazené skupiny konektor.
* Pro aplikace nainstalované v IaaS pro přístup do cloudu konektor skupiny poskytují běžné služby toosecure hello přístup tooall hello aplikace. Konektor skupiny nemáte vytvoření další závislosti na vaší podnikové síti nebo fragmentu hello aplikační prostředí. Konektory lze nainstalovat na každé cloudové datacentrum a sloužit pouze aplikace, které se nacházejí v této síti. Můžete nainstalovat několik konektorů tooachieve vysokou dostupnost.
* Podpora pro prostředí s více doménovými strukturami ve kterých můžete nasadit pro každou doménovou strukturu konkrétní konektory a nastavit tooserve konkrétní aplikace.
* Konektor skupiny lze použít v lokalitách zotavení po havárii tooeither zjistit převzetí služeb při selhání nebo zálohu pro hello hlavní lokalitu.
* Konektor skupiny můžete také být použité tooserve více společností z jednoho klienta.

## <a name="prerequisite-create-your-connectors"></a>Předpoklad: Vytvoření vaší konektory
toogroup konektory, [nainstalovat více konektorů](active-directory-application-proxy-enable.md), potom zadejte název a seskupovat je. Nakonec máte tooassign je toospecific aplikace.

## <a name="step-1-create-connector-groups"></a>Krok 1: Vytvoření skupiny pro konektor
Můžete vytvořit libovolný počet skupin konektor. Vytvoření konektoru skupiny se provádí v hello portál Azure classic.

1. Vyberte adresář a klikněte na tlačítko **konfigurace**.  
    ![Proxy aplikace nakonfigurovat snímek – kliknutím na Spravovat skupiny konektoru](./media/active-directory-application-proxy-connectors/app_proxy_connectors_creategroup.png)
2. V části Proxy aplikace, klikněte na **spravovat skupiny konektor** a vytvořte skupinu konektor tím, že název skupiny hello.  
    ![Snímek obrazovky skupin konektoru proxy aplikace – název nové skupiny](./media/active-directory-application-proxy-connectors/app_proxy_connectors_namegroup.png)

## <a name="step-2-assign-connectors-tooyour-groups"></a>Krok 2: Přiřazení konektory tooyour skupiny
Po vytvoření skupiny pro konektor hello přesunete hello konektory toohello příslušné skupiny.

1. V části **Proxy aplikace**, klikněte na tlačítko **spravovat konektory**.
2. V části **skupiny**vyberte hello skupiny, které chcete použít pro každý konektor. Může trvat hello konektory až too10 minut toobecome active hello nové skupiny.  
    ![Snímek obrazovky konektory proxy aplikace – vyberte skupinu z rozevírací nabídky](./media/active-directory-application-proxy-connectors/app_proxy_connectors_connectorlist.png)

## <a name="step-3-assign-applications-tooyour-connector-groups"></a>Krok 3: Přiřaďte aplikace tooyour konektor skupiny
poslední krok Hello je tooset každou skupinu konektor toohello aplikací, který ji obsluhuje.

1. V hello portál Azure classic, v adresáři, vyberte hello aplikace tooassign toohello skupiny a klikněte na **konfigurace**.
2. V části **konektor skupiny**, vyberte skupinu hello chcete hello toouse aplikace. Tato změna se použije okamžitě.  
    ![Snímek obrazovky skupiny konektoru proxy aplikace – vyberte skupinu z rozevírací nabídky](./media/active-directory-application-proxy-connectors/app_proxy_connectors_newgroup.png)

## <a name="see-also"></a>Viz také
* [Povolení Proxy aplikace](active-directory-application-proxy-enable.md)
* [Povolení jednoduchého přihlášení](active-directory-application-proxy-sso-using-kcd.md)
* [Povolení podmíněného přístupu](active-directory-application-proxy-conditional-access.md)
* [Řešení problémů, které máte s pomocí Proxy aplikace](active-directory-application-proxy-troubleshoot.md)

Hello nejnovější novinky a aktualizace, najdete na naší hello [blogu Proxy aplikace](http://blogs.technet.com/b/applicationproxyblog/)
