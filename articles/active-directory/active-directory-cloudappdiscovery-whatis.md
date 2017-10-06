---
title: "aaaFinding nespravované cloudových aplikací s Cloud App Discovery. | Microsoft Docs"
description: "Poskytuje informace o hledání a Správa aplikací pomocí Cloud App Discovery, jaké jsou výhody hello a jak to funguje."
services: active-directory
keywords: "cloud app discovery,. Správa aplikací"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: db968bf5-22ae-489f-9c3e-14df6e1fef0a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 50c24af9bb400e4be11f4ad2d1de13d26f5467bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="finding-unmanaged-cloud-applications-with-cloud-app-discovery"></a>Hledání nespravované cloudových aplikací s Cloud App Discovery
## <a name="overview"></a>Přehled
V rámci moderní firmy IT oddělení nemají často informace o všech cloudových aplikací hello používat členové jejich organizace toodo práci. Je snadno toosee, proč by správci mají obavy o neoprávněný přístup k datům toocorporate, možné úniku a další bezpečnostní rizika. Tento nedostatek povědomí o můžete nastavit, vytváření plánu pro práci s těchto rizik zabezpečení pravděpodobně složitý.

Cloud App Discovery je funkce Premium Azure Active Directory (AD), jehož pomocí můžete toodiscover cloudových aplikací používá hello lidé ve vaší organizaci.

**Cloud App Discovery můžete:**

* Najít cloudové aplikace hello používá a měřit toto využití počet uživatelů, objemu přenosů nebo počet webové požadavky toohello aplikace.
* Identifikujte hello uživatele, kteří používají aplikaci.
* Exportujte data pro offline analýzu.
* Přepněte tyto aplikace pod kontrolou IT a povolení jednotného přihlašování na pro správu uživatelů.

## <a name="how-it-works"></a>Jak to funguje
1. Agenti využití aplikace jsou nainstalovány na počítače uživatelů.
2. aplikace informace o využití Hello zachycenou hello agentů je odesílán prostřednictvím zabezpečeného, šifrovaný kanál toohello cloudové aplikace zjišťování služby.
3. Hello služby Cloud App Discovery vyhodnotí hello dat a generuje sestavy.

![Diagram cloud App Discovery](./media/active-directory-cloudappdiscovery/cad01.png)

tooget pracovat s Cloud App Discovery, najdete v části [získávání spuštěna s Cloud App Discovery](http://social.technet.microsoft.com/wiki/contents/articles/30962.getting-started-with-cloud-app-discovery.aspx)

## <a name="related-articles"></a>Související články
* [Cloud App Discovery zabezpečení a důležité informace o ochraně osobních údajů](active-directory-cloudappdiscovery-security-and-privacy-considerations.md)  
* [Příručka pro nasazení zásad skupiny zjišťování cloudové aplikace](http://social.technet.microsoft.com/wiki/contents/articles/30965.cloud-app-discovery-group-policy-deployment-guide.aspx)
* [Průvodce nasazením cloud App Discovery System Center](http://social.technet.microsoft.com/wiki/contents/articles/30968.cloud-app-discovery-system-center-deployment-guide.aspx)
* [Nastavení registru Cloud App Discovery na Proxy servery s vlastní porty](active-directory-cloudappdiscovery-registry-settings-for-proxy-services.md)
* [Protokol změn agenta cloud App Discovery.](http://social.technet.microsoft.com/wiki/contents/articles/24616.cloud-app-discovery-agent-changelog.aspx)
* [Nejčastější dotazy k cloud App Discovery.](http://social.technet.microsoft.com/wiki/contents/articles/24037.cloud-app-discovery-frequently-asked-questions.aspx)
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md)

