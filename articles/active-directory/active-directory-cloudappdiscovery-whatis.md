---
title: "Hledání nespravované cloudových aplikací s Cloud App Discovery. | Microsoft Docs"
description: "Poskytuje informace o hledání a Správa aplikací pomocí Cloud App Discovery, jaké jsou výhody a jak to funguje."
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
ms.openlocfilehash: 6284ff5bac8edbc19561d0916adef153526dfbe3
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="finding-unmanaged-cloud-applications-with-cloud-app-discovery"></a>Hledání nespravované cloudových aplikací s Cloud App Discovery
## <a name="overview"></a>Přehled
V rámci moderní firmy IT oddělení nemají často informace všech cloudových aplikací, které členové své organizace používat ke své práci. Je snadno zjistit, proč by správci mají obavy o neoprávněný přístup k podnikovým datům, možné úniku a další bezpečnostní rizika. Tento nedostatek povědomí o můžete nastavit, vytváření plánu pro práci s těchto rizik zabezpečení pravděpodobně složitý.

Cloud App Discovery je funkce Premium Azure Active Directory (AD), která umožňuje zjišťování cloudových aplikací používá lidé ve vaší organizaci.

**Cloud App Discovery můžete:**

* Najít cloudové aplikace, který je používán a měřit toto využití počet uživatelů, objemu provozu nebo počet žádostí o webové aplikaci.
* Identifikujte uživatele, kteří používají aplikaci.
* Exportujte data pro offline analýzu.
* Přepněte tyto aplikace pod kontrolou IT a povolení jednotného přihlašování na pro správu uživatelů.

## <a name="how-it-works"></a>Jak to funguje
1. Agenti využití aplikace jsou nainstalovány na počítače uživatelů.
2. Informace o využití aplikace zachycenou agentů je odeslána přes zabezpečené, šifrovaný kanál ke službě cloud app discovery.
3. Služba Cloud App Discovery vyhodnotí data a generuje sestavy.

![Diagram cloud App Discovery](./media/active-directory-cloudappdiscovery/cad01.png)

Chcete-li začít používat Cloud App Discovery, přečtěte si téma [získávání spuštěna s Cloud App Discovery](http://social.technet.microsoft.com/wiki/contents/articles/30962.getting-started-with-cloud-app-discovery.aspx)

## <a name="related-articles"></a>Související články
* [Cloud App Discovery zabezpečení a důležité informace o ochraně osobních údajů](active-directory-cloudappdiscovery-security-and-privacy-considerations.md)  
* [Příručka pro nasazení zásad skupiny zjišťování cloudové aplikace](http://social.technet.microsoft.com/wiki/contents/articles/30965.cloud-app-discovery-group-policy-deployment-guide.aspx)
* [Průvodce nasazením cloud App Discovery System Center](http://social.technet.microsoft.com/wiki/contents/articles/30968.cloud-app-discovery-system-center-deployment-guide.aspx)
* [Nastavení registru Cloud App Discovery na Proxy servery s vlastní porty](active-directory-cloudappdiscovery-registry-settings-for-proxy-services.md)
* [Protokol změn agenta cloud App Discovery.](http://social.technet.microsoft.com/wiki/contents/articles/24616.cloud-app-discovery-agent-changelog.aspx)
* [Nejčastější dotazy k cloud App Discovery.](http://social.technet.microsoft.com/wiki/contents/articles/24037.cloud-app-discovery-frequently-asked-questions.aspx)
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md)

