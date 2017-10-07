---
title: aaaAzure protokolu integrace s protokoly auditu Azure Active Directory | Microsoft Docs
description: "Zjistěte, jak tooinstall hello Azure protokolu integrační služby a integraci protokoly z protokolů auditu Azure"
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomShinder
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ums.workload: na
ms.date: 08/08/2017
ms.author: barclayn
ms.custom: azlog
ms.openlocfilehash: 3ee8fa3b8b5e9bd33202e57ed5327cd8d3127f00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-azure-active-directory-audit-logs"></a>Integrovat protokolů auditu Azure Active Directory

Události auditování Azure Active Directory (Azure AD) pomáhá identifikovat privilegovaných akcí, které došlo k chybě v Azure Active Directory. Zobrazí se hello typy událostí, které můžete sledovat kontrolou [události sestavy auditování Azure Active Directory](/active-directory/active-directory-reporting-audit-events#list-of-audit-report-events.md).

> [!NOTE]
> Před provedením hello kroky v tomto článku, je nutné si hello [Začínáme](security-azure-log-integration-get-started.md) článek a proveďte kroky hello existuje.

## <a name="steps-toointegrate-azure-active-directory-audit-logs"></a>Protokoly auditu Azure Active directory toointegrate kroky

1. Otevřete příkazový řádek se hello a spusťte tento příkaz:

   ``cd c:\Program Files\Microsoft Azure Log Integration``

2. Spusťte tento příkaz: 
 
   ``azlog createazureid``

   Tento příkaz zobrazí výzvu k přihlášení Azure. Hello příkaz potom vytvoří Azure Active Directory instančního objektu v Azure AD hello klientům hello předplatná Azure, ve které hello přihlášený uživatel je správce, spolusprávce nebo vlastníka tohoto hostitele. příkaz Hello se nezdaří, pokud je pouze uživatel guest v klientovi Azure AD hello hello přihlášeného uživatele. TooAzure ověřování se provádí prostřednictvím služby Azure AD. Vytvoření objektu služby pro integraci protokolu Azure vytvoří hello identit Azure AD, který je přiřazen přístup tooread z předplatných Azure.

3. Spusťte následující příkaz tooprovide hello vaše ID klienta. Je nutné toobe členem hello klienta správce role toorun hello příkaz.

   ``Azlog.exe authorizedirectoryreader tenantId``

   Příklad:

   ``AZLOG.exe authorizedirectoryreader ba2c0000-d24b-4f4e-92b1-48c4469999``

4. Zkontrolujte, že hello následující tooconfirm složky, která hello soubory JSON protokolu auditování Azure Active Directory jsou vytvořené v nich:

   * **C:\Users\azlog\AzureActiveDirectoryJson**
   * **C:\Users\azlog\AzureActiveDirectoryJsonLD**

Hello toto video ukazuje hello kroky popsané v tomto článku:

> [!VIDEO https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Log-Integration-Videos-Azure-AD-Integration/player]


> [!NOTE]
> Konkrétní pokyny k uvedení hello informace v souborech JSON hello do své informace o zabezpečení a událostí systému pro správu (SIEM) obraťte se na dodavatele systému SIEM.

Komunita pomoc je k dispozici prostřednictvím hello [fórum MSDN integrace protokolu Azure](https://social.msdn.microsoft.com/Forums/office/home?forum=AzureLogIntegration). Toto fórum umožňuje lidem v toosupport komunity protokolu integrace se službou Azure hello navzájem otázky, odpovědi, tipy a triky. Kromě toho týmu Integrace protokolu Azure hello monitoruje Toto fórum a pomáhá vždy, když je to možné.

Můžete také otevřít [žádost o podporu](../azure-supportability/how-to-create-azure-support-request.md). Vyberte **integrace protokolu** jako hello služby, pro kterou jsou žádosti o podporu.

## <a name="next-steps"></a>Další kroky
toolearn Další informace o integraci Azure protokolu, najdete v části:

* [Microsoft Azure protokolu integrace pro Azure protokoly](https://www.microsoft.com/download/details.aspx?id=53324): stránka tento stažení softwaru poskytuje podrobnosti, požadavky na systém a pokyny k integraci Azure protokolu.
* [Úvod tooAzure integrace protokolu](security-azure-log-integration-overview.md): Tento článek představuje tooAzure integrace protokolu, jejích klíčových funkcích a jak to funguje.
* [Partner kroky konfigurace](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/): Tento příspěvek blogu ukazuje, jak tooconfigure integrace se službou Azure protokolu toowork s partnerských řešení Splunk, HP ArcSight a IBM QRadar.
* [Nejčastější dotazy k integraci Azure protokolu](security-azure-log-integration-faq.md): Tento článek obsahuje odpovědi na otázky týkající se integrace se službou Azure protokolu.
* [Výstrahy Security Center integrování integrace se službou Azure protokolu](../security-center/security-center-integrating-alerts-with-log-integration.md): Tento článek ukazuje, jak toosync Security Center výstrahy, společně s shromážděných pomocí diagnostiky Azure a protokoly auditu Azure, s vaší analýzy protokolů událostí zabezpečení virtuálního počítače nebo Řešení SIEM.
* [Protokoly auditu nové funkce pro diagnostiky Azure a Azure](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/): Tento příspěvek blogu vás seznámí tooAzure protokoly auditu a další funkce, které vám pomohou analyzovat hello operations vašich prostředků Azure.
