---
title: aaaAzure protokolu integrace s security center | Microsoft Docs
description: "Zjistěte, jak tooget Azure Security center výstrahy práce integrace protokolu"
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
ms.date: 03/22/2017
ms.author: barclayn
ms.custom: azlog
ms.openlocfilehash: bcc208d071ec03738215f2aee3b71c7b10927904
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooget-your-security-center-alerts-in-azure-log-integration"></a>Jak tooget Security Center výstrahy v integraci protokolů Azure
Tento článek obsahuje hello kroky požadované tooenable hello protokolu integrace se službou Azure service toopull výstraha zabezpečení informace generované Azure Security Center. Musíte úspěšně jste dokončili postup hello v hello [Začínáme](security-azure-log-integration-get-started.md) článek před provedením hello kroky v tomto článku.

## <a name="detailed-steps"></a>Podrobné kroky
Hello následující postup vytvoří hello požadované objekt zabezpečení služby Azure Active Directory a přiřadit hello Princip služby oprávnění ke čtení toohello předplatného:
1. Otevřete hello příkazový řádek a přejděte příliš**c:\Program Files\Microsoft Azure protokolu integrace**
2. Spusťte příkaz hello``azlog createazureid``

    Tento příkaz zobrazí výzvu k přihlášení Azure. příkaz Hello potom vytvoří [Azure Active Directory Service Principal](../active-directory/develop/active-directory-application-objects.md) v hello klienty Azure AD, které jsou hostiteli hello předplatná Azure, ve které hello přihlášeného uživatele je správce, Spolusprávce nebo vlastníka. příkaz Hello se nezdaří, pokud hello přihlášeného uživatele pouze uživatel Guest v hello klienta Azure AD. TooAzure ověřování se provádí prostřednictvím Azure Active Directory (AD). Vytvoření objektu služby pro integraci Azlog vytvoří hello Azure AD identity, který bude mít přístup tooread z předplatných Azure.

2. Potom spustíte příkaz, který přiřazuje přístup čtečky na hello předplatné toohello instanční objekt vytvořili v kroku 2. Pokud nezadáte ID předplatného, se pokusí hello příkaz tooassign hello služby hlavní čtečky role tooall odběry toowhich máte přístup. </br></br>
``azlog authorize <SubscriptionID>`` </br> například </br>
``azlog authorize 0ee55555-0bc4-4a32-a4e8-c29980000000``

    >[!NOTE]
    Mohou se zobrazit upozornění, pokud spustíte hello autorizovat příkaz ihned po příkazu createazureid hello. Některé latence mezi při vytvoření účtu hello Azure AD a pokud není k dispozici pro použití účtu hello neexistuje. Pokud počkat 60 sekund po spuštění hello createazureid příkaz toorun hello autorizovat příkaz a pak byste neměli vidět tato upozornění.

4. Zkontrolujte, že existují následující tooconfirm složky, která hello soubory JSON protokolu auditu hello:
 * **c:\Users\azlog\AzureResourceManagerJson**
 * **c:\Users\azlog\AzureResourceManagerJsonLD** </br></br>
5. Zkontrolujte následující tooconfirm složek, které výstrahy Security Center existovat v nich hello:</br></br>
 * **c:\Users\azlog\AzureSecurityCenterJson**
 * **c:\Users\azlog\AzureSecurityCenterJsonLD** </br></br>

Pokud narazíte na potíže během hello instalace a konfigurace, otevřete prosím [žádost o podporu](/azure-supportability/how-to-create-azure-support-request.md), vyberte **integrace protokolu** jako hello služby, pro kterou jsou žádosti o podporu.

## <a name="next-steps"></a>Další kroky
toolearn Další informace o integraci Azure protokolu, najdete v části hello následující dokumenty:

* [Microsoft Azure protokolu integrace pro Azure protokoly](https://www.microsoft.com/download/details.aspx?id=53324) – Download Center podrobnosti, požadavky na systém a instalovat pokyny týkající se integrace protokolů Azure.
* [Integrace protokolu tooAzure ÚVOD](security-azure-log-integration-overview.md) – Toto téma představuje integrace protokolu tooAzure, jejích klíčových funkcích a jak to funguje.
* [Partner kroky konfigurace](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) – tento příspěvek blogu ukazuje, jak tooconfigure Azure protokolu toowork integrace s partnerských řešení Splunk, HP ArcSight a IBM QRadar.
* [Protokolů Azure integrace nejčastější dotazy (FAQ)](security-azure-log-integration-faq.md) – nejčastější dotazy týkající se tento odpovědi dotazy týkající se integrace protokolů Azure.
* [Integrace Security Center výstrahy s Azure protokolu integrace](../security-center/security-center-integrating-alerts-with-log-integration.md) – tento dokument ukazuje, jak toosync Security Center výstrahy, společně s shromážděných pomocí diagnostiky Azure a protokoly auditu Azure s vaší analýzy protokolů událostí zabezpečení virtuálního počítače nebo Řešení SIEM.
* [Nové funkce pro protokoly auditu Azure a Azure diagnostics](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/) – tento příspěvek blogu vás seznámí tooAzure protokoly auditu a další funkce, které vám pomůžou proniknout do operací hello vašich prostředků Azure.
