---
title: "aaaAdd brány firewall webových aplikací v Azure Security Center | Microsoft Docs"
description: "Tento dokument ukazuje, jak tooimplement hello doporučení služby Azure Security Center ** přidat webové aplikace brány firewall ** a ** dokončení ochrany aplikace **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 8f56139a-4466-48ac-90fb-86d002cf8242
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/09/2017
ms.author: terrylan
ms.openlocfilehash: bff0aa2a5c6e0dde23396f93de52abe295053581
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-a-web-application-firewall-in-azure-security-center"></a>Přidání brány firewall webových aplikací v Azure Security Center
Azure Security Center může doporučujeme přidat brány firewall webových aplikací (firewall webových aplikací) z toosecure partnera Microsoft webových aplikací. Tento dokument vás příklad provede tooapply tohoto doporučení.

Doporučení firewall webových aplikací je zobrazený pro všechny veřejné přístupných IP adresy (IP úrovni Instance nebo IP skupinu s vyrovnáváním zatížení), skupinu zabezpečení sítě spojenou s otevřete příchozí webovými porty (80,443).

Security Center doporučí, že zřídit toohelp firewall webových aplikací bránit proti útokům na cílení na vaše webové aplikace na virtuální počítače a služby App Service Environment. Je aplikaci služby prostředí (App Service Environment) [Premium](https://azure.microsoft.com/pricing/details/app-service/) služby možnost plánu služby Azure App Service, která poskytuje plně izolovaném a vyhrazeném prostředí pro zabezpečené spouštění aplikací Azure App Service. toolearn Další informace o App Service Environment, najdete v části hello [dokumentace k aplikaci služby prostředí](../app-service/app-service-app-service-environments-readme.md).

> [!NOTE]
> Toto téma představuje hello služby pomocí příklad nasazení.  Tento dokument není to podrobný průvodce.
>
>

## <a name="implement-hello-recommendation"></a>Implementace doporučení hello
1. V hello **doporučení** vyberte **zabezpečení webové aplikace pomocí brány firewall webových aplikací**.
   ![Zabezpečení webové aplikace][1]
2. V hello **zabezpečení webových aplikací pomocí brány firewall webových aplikací** okně vyberte webovou aplikaci. Hello **přidání brány Firewall webových aplikací** otevře se okno.
   ![Přidání brány firewall webových aplikací][2]
3. Pokud můžete vybrat toouse existující brány firewall webových aplikací k dispozici nebo můžete vytvořit nový. V tomto příkladu nejsou žádné existující WAFs k dispozici, vytvoříme firewall webových aplikací.
4. toocreate firewall webových aplikací, vyberte řešení z hello seznam integrovaných partnerů. V tomto příkladu jsme vyberte **brány Firewall webových aplikací Barracuda**.
5. Hello **brány Firewall webových aplikací Barracuda** otevře se okno poskytování informací o hello partnerských řešení. Vyberte **vytvořit** v okně informace hello.

   ![Okno informace o brány firewall][3]

6. Hello **nové brány Firewall webových aplikací** otevře se okno, kde můžete provádět **konfigurace virtuálního počítače** kroky a zadejte **firewall webových aplikací informace**. Vyberte **konfigurace virtuálního počítače**.
7. V hello **konfigurace virtuálního počítače** okno, zadejte informace požadované toospin hello virtuálního počítače, který spouští hello firewall webových aplikací.
   ![Konfigurace virtuálního počítače][4]
8. Vrátí toohello **nové brány Firewall webových aplikací** a vyberte **firewall webových aplikací informace**. V hello **firewall webových aplikací informace** okně nakonfigurujete hello firewall webových aplikací, sám sebe. Krok 7 vám umožní tooconfigure hello virtuálního počítače, na které hello firewall webových aplikací spustí a krok 8 vám umožní tooprovision hello firewall webových aplikací, sám sebe.

## <a name="finalize-application-protection"></a>Dokončit ochranu aplikace
1. Vrátí toohello **doporučení** okno. Nový záznam vygenerovalo po vytvoření hello firewall webových aplikací, nazývá **dokončit ochranu aplikace**. Tato položka způsobem zjistíte, je nutné, aby toocomplete hello proces ve skutečnosti připojení hello firewall webových aplikací v rámci hello virtuální sítě Azure tak, aby ho může chránit aplikace hello.

   ![Dokončit ochranu aplikace][5]

2. Vyberte **dokončit ochranu aplikace**. Otevře se nové okno. Uvidíte, že se webové aplikace, které potřebuje toohave jeho provoz přesměruje.
3. Vyberte hello webové aplikace. Otevře se okno umožňující kroky pro dokončení instalace brány firewall webových aplikací hello. Hello kroky a pak vyberte **omezení přenosu**. Security Center pak hello kabeláž up za vás.

   ![Omezení přenosu][6]

> [!NOTE]
> Několika webových aplikací ve službě Security Center můžete chránit tak, že přidáte tyto aplikace tooyour existující firewall webových aplikací nasazení.
>
>

Nyní jsou plně integrované Hello protokoly z této firewall webových aplikací. Security Center můžete spustit automaticky shromažďování a analyzování protokolů hello tak, aby ho můžete surface tooyou výstrahy důležité zabezpečení.

## <a name="next-steps"></a>Další kroky
Tento dokument ukázal, jak tooimplement hello Security Center doporučení "Přidat webovou aplikaci." toolearn Další informace o konfiguraci brány firewall webových aplikací najdete v části hello následující:

* [Konfigurace brány Firewall webových aplikací (firewall webových aplikací) pro služby App Service Environment](../app-service-web/app-service-app-service-environment-web-application-firewall.md)

toolearn Další informace o Security Center, najdete v části hello následující:

* [Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – zjistěte, jak tooconfigure zásady zabezpečení pro skupiny prostředků a předplatná Azure.
* [Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – zjistěte, jak toomonitor hello stav svých prostředků Azure.
* [Správa a zda odpovídá toosecurity výstrahy v Azure Security Center](security-center-managing-and-responding-alerts.md) – zjistěte, jak toomanage a reakce toosecurity výstrahy.
* [Správa doporučení zabezpečení v Azure Security Center](security-center-recommendations.md) – zjistěte, jak vám doporučení pomáhají chránit prostředky v Azure.
* [Nejčastější dotazy k Azure Security Center](security-center-faq.md) – přečtěte si nejčastější dotazy o použití služby hello.
* [Blog o bezpečnosti Azure](http://blogs.msdn.com/b/azuresecurity/) – přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů.

<!--Image references-->
[1]: ./media/security-center-add-web-application-firewall/secure-web-application.png
[2]:./media/security-center-add-web-application-firewall/add-a-waf.png
[3]: ./media/security-center-add-web-application-firewall/info-blade.png
[4]: ./media/security-center-add-web-application-firewall/select-vm-config.png
[5]: ./media/security-center-add-web-application-firewall/finalize-waf.png
[6]: ./media/security-center-add-web-application-firewall/restrict-traffic.png
