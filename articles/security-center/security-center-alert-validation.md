---
title: "aaaAlerts ověřování v Azure Security Center | Microsoft Docs"
description: "Tento dokument vám pomůže toovalidate hello výstrah zabezpečení v Azure Security Center."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: f8f17a55-e672-4d86-8ba9-6c3ce2e71a57
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/11/2017
ms.author: yurid
ms.openlocfilehash: 030e9e74303758192eedaf517f1cb0d2e4a7852e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="alerts-validation-in-azure-security-center"></a>Ověřování výstrah ve službě Azure Security Center
Tento dokument vám pomůže se naučit jak tooverify Pokud váš systém byl správně nakonfigurován pro Azure Security Center výstrahy.

## <a name="what-are-security-alerts"></a>Co jsou výstrahy zabezpečení?
Security Center automaticky shromažďuje, analyzuje a integruje data protokolu z vaše prostředky Azure, sítě hello a připojených partnerských řešení, jako jsou brány firewall a endpoint protection řešení, toodetect a výstrah toothreats. Čtení [správy a zda odpovídá toosecurity výstrahy v Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts) Další informace o výstrahách zabezpečení a čtení [pochopení výstrah zabezpečení v Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type) toolearn další o hello různé typy výstrah.

## <a name="alert-validation"></a>Ověřování výstrah
Po instalaci agenta Security Center ve vašem počítači, postupujte podle hello kroků z počítače hello místo prostředků hello napadení toobe hello výstrahy:

1. Zkopírujte plochu spustitelný soubor (pro příklad calc.exe) toohello počítače nebo jiného adresáře usnadnění vaší práce.
2. Přejmenujte tento soubor příliš**ASC_AlertTest_662jfi039N.exe**.
3. Otevřete příkazový řádek se hello a spusťte tento soubor s parametrem (pouze název falešných argument), jako například: *ASC_AlertTest_662jfi039N.exe - foo*
4. Počkejte 5 minut too10 a otevřete výstrahy Security Center. Existuje byste měli najít výstrahy podobné toofollowing, jeden:

    ![Ověřování výstrah](./media/security-center-alert-validation/security-center-alert-validation-fig1.png)

Při revizi tuto výstrahu, zkontrolujte, zda pole hello povoleno auditování argumenty, zobrazí se jako true. Pokud se zobrazí hodnotu false, je třeba argumenty příkazového řádku tooenable auditování. Můžete povolit tuto možnost, pomocí hello následující příkaz:

*reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\policies\system\Audit" /f /v "ProcessCreationIncludeCmdLine_Enabled"*


## <a name="see-also"></a>Viz také
Tento článek se zavedl toohello výstrahy ověření procesu. Teď, když jste obeznámeni s toto ověření, zkuste hello následující články:

* [Správa a zda odpovídá toosecurity výstrahy v Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts). Zjistěte, jak toomanage výstrah a incidentů toosecurity reakce ve službě Security Center.
* [Monitorování stavu zabezpečení ve službě Azure Security Center](security-center-monitoring.md). Zjistěte, jak toomonitor hello stav svých prostředků Azure.
* [Principy výstrah zabezpečení ve službě Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type). Další informace o různých typech hello výstrahy zabezpečení.
* [Průvodce odstraňováním potíží pro službu Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide). Zjistěte, jak tootroubleshoot běžné problémy ve službě Security Center. 
* [Azure Security Center – nejčastější dotazy](security-center-faq.md). Nejčastější dotazy o použití hello služby najít.
* [Blog o zabezpečení Azure](http://blogs.msdn.com/b/azuresecurity/). Přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů.

