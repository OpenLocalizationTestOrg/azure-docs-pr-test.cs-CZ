---
title: "Protokol integrace se službou Azure s protokoly auditu Azure Active Directory | Microsoft Docs"
description: "Zjistěte, jak nainstalovat službu protokolu integrace se službou Azure a integrovat protokoly z protokolů auditu Azure"
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
ms.openlocfilehash: 8a1295cc86057ed72940e774d0bd423d61142e31
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="integrate-azure-active-directory-audit-logs"></a>Integrovat protokolů auditu Azure Active Directory

Události auditování Azure Active Directory (Azure AD) pomáhá identifikovat privilegovaných akcí, které došlo k chybě v Azure Active Directory. Zobrazí typy událostí, které můžete sledovat kontrolou [události sestavy auditování Azure Active Directory](/active-directory/active-directory-reporting-audit-events#list-of-audit-report-events.md).

> [!NOTE]
> Před provedením kroků v tomto článku, je nutné si [Začínáme](security-azure-log-integration-get-started.md) článek a dokončete existuje.

## <a name="steps-to-integrate-azure-active-directory-audit-logs"></a>Protokoly auditu kroky pro integraci služby Azure Active directory

1. Otevřete příkazový řádek a spusťte tento příkaz:

   ``cd c:\Program Files\Microsoft Azure Log Integration``

2. Spusťte tento příkaz: 
 
   ``azlog createazureid``

   Tento příkaz zobrazí výzvu k přihlášení Azure. Příkaz vytvoří Azure Active Directory instančního objektu v klienty Azure AD, které jsou hostiteli předplatná Azure, ve kterých přihlášený uživatel je správce, spolusprávce nebo vlastníka. Příkaz se nezdaří, pokud je uživatel přihlášený pouze uživatel guest v klientovi Azure AD. Ověřování do Azure se provádí prostřednictvím služby Azure AD. Vytvoření objektu služby pro integraci protokolu Azure vytvoří Azure AD identity, který je přiřazen přístup ke čtení z předplatných Azure.

3. Spusťte následující příkaz k poskytování vaše ID klienta. Musíte být členem role Správce klientů ke spuštění příkazu.

   ``Azlog.exe authorizedirectoryreader tenantId``

   Příklad:

   ``AZLOG.exe authorizedirectoryreader ba2c0000-d24b-4f4e-92b1-48c4469999``

4. Zkontrolujte následující složky pro potvrzení, že jsou v nich vytvořeny soubory JSON protokolů auditu Azure Active Directory:

   * **C:\Users\azlog\AzureActiveDirectoryJson**
   * **C:\Users\azlog\AzureActiveDirectoryJsonLD**

Následující video ukazuje kroky popsané v tomto článku:

> [!VIDEO https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Log-Integration-Videos-Azure-AD-Integration/player]


> [!NOTE]
> Konkrétní pokyny k uvedení informací v souborech JSON do své informace o zabezpečení a událostí systému pro správu (SIEM) obraťte se na dodavatele systému SIEM.

Komunita pomoc je k dispozici prostřednictvím [fórum MSDN integrace protokolu Azure](https://social.msdn.microsoft.com/Forums/office/home?forum=AzureLogIntegration). Toto fórum umožňuje členové komunity protokolu integrace se službou Azure pro podporu navzájem otázky, odpovědi, tipy a triky. Kromě toho týmem protokolu integrace se službou Azure sleduje toto fórum a pomáhá vždy, když je to možné.

Můžete také otevřít [žádost o podporu](../azure-supportability/how-to-create-azure-support-request.md). Vyberte **integrace protokolu** jako službu, pro kterou jsou žádosti o podporu.

## <a name="next-steps"></a>Další kroky
Další informace o integraci Azure protokolu najdete v tématu:

* [Microsoft Azure protokolu integrace pro Azure protokoly](https://www.microsoft.com/download/details.aspx?id=53324): stránka tento stažení softwaru poskytuje podrobnosti, požadavky na systém a pokyny k integraci Azure protokolu.
* [Úvod do integrace se službou Azure protokolu](security-azure-log-integration-overview.md): Tento článek vás seznámí s protokolu integrace se službou Azure, jejích klíčových funkcích a jak to funguje.
* [Partner kroky konfigurace](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/): Tento příspěvek blogu ukazuje, jak nakonfigurovat integraci Azure protokolu pro práci s partnerských řešení Splunk, HP ArcSight a IBM QRadar.
* [Nejčastější dotazy k integraci Azure protokolu](security-azure-log-integration-faq.md): Tento článek obsahuje odpovědi na otázky týkající se integrace se službou Azure protokolu.
* [Výstrahy Security Center integrování integrace se službou Azure protokolu](../security-center/security-center-integrating-alerts-with-log-integration.md): v tomto článku se dozvíte, jak synchronizovat výstrahy Security Center, společně s zabezpečení virtuálního počítače shromažďovány události pomocí diagnostiky Azure a Azure auditu protokoly s vaší analýzy protokolů nebo Řešení SIEM.
* [Protokoly auditu nové funkce pro diagnostiky Azure a Azure](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/): Tento příspěvek blogu vás seznámí s protokoly auditu Azure a další funkce, které vám pomůžou získat přehled o činnosti vašich prostředků Azure.
