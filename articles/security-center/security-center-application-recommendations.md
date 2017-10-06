---
title: aaaProtecting aplikace v Azure Security Center | Microsoft Docs
description: "Tento dokument adresy doporučení v Azure Security Center, které vám pomohou ochránit vaše aplikace Azure a zůstat souladu se zásadami zabezpečení."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: b5fc7a9e-24b1-415f-b3b5-62a53f5dd424
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/01/2016
ms.author: terrylan
ms.openlocfilehash: da5e02cc2bad55c64e4da14e4e10efd6ddeab39e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="protecting-your-applications-in-azure-security-center"></a>Ochrana aplikací v Azure Security Center
Azure Security Center analyzuje stav zabezpečení hello vašich prostředků Azure. Když Security Center identifikuje potenciální ohrožení zabezpečení, vytvoří se doporučení, která vás provede procesem hello konfigurace hello potřebné ovládací prvky.  Doporučení se týkají tooAzure typy prostředků: virtuální počítače (VM), sítě, SQL a aplikace.

Tento článek se zaměřuje na doporučení, které se vztahují tooapplications.  Aplikace center doporučení ohledně nasazení brány firewall webových aplikací.  Použití hello tabulce jako referenční toohelp porozumíte doporučení k dispozici aplikace hello a co každé z nich dělá, pokud ji použijete.

## <a name="available-application-recommendations"></a>Doporučení k dispozici aplikace
| Doporučení | Popis |
| --- | --- |
| [Přidání brány firewall webových aplikací](security-center-add-web-application-firewall.md) |Doporučuje, která můžete nasadit brány firewall webových aplikací (firewall webových aplikací) pro koncových bodů webové. Doporučení firewall webových aplikací je zobrazený pro všechny veřejné přístupných IP adresy (IP úrovni Instance nebo IP skupinu s vyrovnáváním zatížení), skupinu zabezpečení sítě spojenou s otevřete příchozí webovými porty (80,443).</br></br>Security Center doporučí, že zřídit toohelp firewall webových aplikací bránit proti útokům na cílení na vaše webové aplikace na virtuální počítače a služby App Service Environment. Je aplikaci služby prostředí (App Service Environment) [Premium](https://azure.microsoft.com/pricing/details/app-service/) služby možnost plánu služby Azure App Service, která poskytuje plně izolovaném a vyhrazeném prostředí pro zabezpečené spouštění aplikací Azure App Service. toolearn Další informace o App Service Environment, najdete v části hello [dokumentace k aplikaci služby prostředí](../app-service/app-service-app-service-environments-readme.md).</br></br>Několika webových aplikací ve službě Security Center můžete chránit tak, že přidáte tyto aplikace tooyour existující firewall webových aplikací nasazení. |
| [Finalizace ochrany aplikací](security-center-add-web-application-firewall.md#finalize-application-protection) |Konfigurace hello toocomplete firewall webových aplikací, provoz musí být zařízení přesměrovanou toohello firewall webových aplikací. Následující toto doporučení dokončení změny potřebné instalační hello. |

## <a name="see-also"></a>Viz také
toolearn Další informace o doporučení, které se vztahují tooother typy prostředků Azure, najdete v části hello následující:

* [Ochrana virtuálních počítačů v Azure Security Center](security-center-virtual-machine-recommendations.md)
* [Ochrana sítě v Azure Security Center](security-center-network-recommendations.md)
* [Ochrana služby Azure SQL v Azure Security Center](security-center-sql-service-recommendations.md)

toolearn Další informace o Security Center, najdete v části hello následující:

* [Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – zjistěte, jak tooconfigure zásady zabezpečení pro skupiny prostředků a předplatná Azure.
* [Správa a zda odpovídá toosecurity výstrahy v Azure Security Center](security-center-managing-and-responding-alerts.md) – zjistěte, jak toomanage a reakce toosecurity výstrahy.
* [Nejčastější dotazy k Azure Security Center](security-center-faq.md) – přečtěte si nejčastější dotazy o použití služby hello.
