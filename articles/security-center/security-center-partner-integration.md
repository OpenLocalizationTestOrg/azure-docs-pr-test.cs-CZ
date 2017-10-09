---
title: integrace aaaPartner v Azure Security Center | Microsoft Docs
description: "Další informace o tom, jak Azure Security Center integruje s partnery tooenhance celkové zabezpečení vašich prostředků Azure."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 6af354da-f27a-467a-8b7e-6cbcf70fdbcb
ms.service: security-center
ms.topic: hero-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2017
ms.author: yurid
ms.openlocfilehash: 3621335730a076721cb3c23788a47be50aa8fc73
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="partner-integration-in-azure-security-center"></a>Integrace partnerských řešení ve službě Azure Security Center

V tomto článku jsme popisují, jak Azure Security Center umožňuje integraci s partnery toohelp zvýšení celkové zabezpečení. Security Center nabízí integrované možnosti v Azure a využívá hello Azure Marketplace pro partnera certifikační a fakturace.

> [!NOTE] 
> Od června 2017 Security Center používá hello agenta Microsoft Monitoring Agent toocollect a uložit data. Další informace najdete v tématu [Migrace platformy pro Azure Security Center](security-center-platform-migration.md). Hello informace v tomto článku představuje funkce Security Center po přechodu toohello agenta Microsoft Monitoring Agent.
>

## <a name="why-deploy-partner-solutions-from-security-center"></a>Proč nasazovat partnerská řešení ze služby Security Center

Čtyři hlavní příčiny tooleverage partnera integrace v Centru zabezpečení jsou:

- **Snadné nasazení**. Nasazení řešení partnera s následující hello doporučení Security Center je mnohem jednodušší. proces nasazení Hello je možné plně automatizovat pomocí výchozí nastavení a síťové topologie. Alternativně můžou zákazníci zvolit možnost poloautomatického nasazení s větší flexibilitou a možnostmi přizpůsobení.
- **Integrované detekce**. Události zabezpečení z partnerských řešení se automaticky shromažďují, agregují a zobrazují v rámci výstrah a incidentů služby Security Center. Tyto události jsou také začleněny s detekcí z jiných zdrojů tooprovide rozšířené možnosti detekce hrozeb.
- **Jednotné monitorování stavu a správa**. Zákazníci mohou používat integrované stavu události toomonitor všech partnerských řešení na první pohled. Základní správu je k dispozici, instalační program tooadvanced snadný přístup pomocí hello partnerských řešení.
- **Export tooSIEM**. Zákazníci můžete exportovat všechny Security Center a partnera výstrahy společné systémy informace o zabezpečení a událostí správy (SIEM) tooon místní formát událostí (CEF) pomocí integrace protokolů Azure (preview).


## <a name="partners-that-integrate-with-security-center"></a>Partnerská řešení s možností integrace se službou Security Center

Security Center se aktuálně integruje s těmito řešeními:

- Ochrana koncových bodů ([Trend Micro](https://help.deepsecurity.trendmicro.com/azure-marketplace-getting-started-with-deep-security.html), Symantec a [Microsoft Antimalware pro Azure Cloud Services a Virtual Machines](https://docs.microsoft.com/azure/security/azure-security-antimalware)) 
- Firewall webových aplikací ([Barracuda](https://www.barracuda.com/products/webapplicationfirewall), [F5](https://support.f5.com/kb/en-us/products/big-ip_asm/manuals/product/bigip-ve-web-application-firewall-microsoft-azure-12-0-0.html), [Imperva](https://www.imperva.com/Products/WebApplicationFirewall-WAF), [Fortinet](https://www.fortinet.com/resources.html?limit=10&search=&document-type=data-sheets) a [Azure Application Gateway](https://azure.microsoft.com/blog/azure-web-application-firewall-waf-generally-available/)) 
- Firewall nové generace ([Check Point](https://www.checkpoint.com/products/vsec-microsoft-azure/), [Barracuda](https://campus.barracuda.com/product/nextgenfirewallf/article/NGF/AzureDeployment/), [Fortinet](http://docs.fortinet.com/d/fortigate-fortios-handbook-the-complete-guide-to-fortios-5.2) a [Cisco](http://www.cisco.com/c/en/us/td/docs/security/firepower/quick_start/azure/ftdv-azure-qsg.html)) 
- Posouzení ohrožení zabezpečení ([Qualys](https://www.qualys.com/public-clouds/microsoft-azure/))  

V čase bude Security Center rozbalte hello počet partnery v rámci těchto kategorií a přidat nové kategorie. 

## <a name="deploy-a-partner-solution"></a>Nasazení partnerského řešení

Na základě nastavení hello prostředí Azure a hello zásady zabezpečení, které jste definovali Security Center může doporučujeme nasadit jedno partnerské řešení. Hello Security Center doporučení vás provede hello proces výběru a instalaci jedno partnerské řešení. Hello celkový dojem nasazení se může lišit v závislosti na typu hello řešení a partnery, které používáte. Další informace najdete v tématu hello následující články:

- [Instalace Endpoint Protection](security-center-install-endpoint-protection.md)
- [Přidání brány firewall webových aplikací](security-center-add-web-application-firewall.md)
- [Přidání brány firewall nové generace](security-center-add-next-generation-firewall.md)
- [Není nainstalováno posouzení ohrožení zabezpečení](security-center-vulnerability-assessment-recommendations.md)

## <a name="manage-partner-solutions"></a>Správa partnerských řešení

Po nasazení tooview informace o stavu řešení hello hello a provádět úlohy správy základní, na hello **Security Center** okně, vyberte hello **Partner solutions** možnost. Další informace o správě partnerských řešení v Security Center najdete v článku [Monitorování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md).

![Integrace partnerských řešení](./media/security-center-partner-integration/security-center-partner-integration-fig1-new2.png)

> [!NOTE]
> Symantec endpoint protection podpora je omezena toodiscovery. Nejsou k dispozici žádné výstrahy týkající se stavu.
>

## <a name="see-also"></a>Viz také

V tomto článku jste se dozvěděli, jak toointegrate partnerských řešení v Azure Security Center. toolearn Další informace o Security Center, najdete v části hello následující články:

* [Průvodce plánováním a provozem služby Security Center](security-center-planning-and-operations-guide.md)
* [Spravovat a reagovat toosecurity výstrah v Security Center](security-center-managing-and-responding-alerts.md)
* [Výstrahy zabezpečení podle typu ve službě Security Center](security-center-alerts-type.md)
* [Monitorování stavu zabezpečení ve službě Security Center](security-center-monitoring.md). Zjistěte, jak toomonitor hello stav svých prostředků Azure.
* [Monitorování partnerských řešení pomocí služby Security Center](security-center-partner-solutions.md). Zjistěte, jak toomonitor hello stav vašich partnerských řešení.
* [Azure Security Center – nejčastější dotazy](security-center-faq.md). Získejte odpovědi toofrequently kladené dotazy týkající se používání služby hello.
* [Blog o zabezpečení Azure](http://blogs.msdn.com/b/azuresecurity/). Přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů.
