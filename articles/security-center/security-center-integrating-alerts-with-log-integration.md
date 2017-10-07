---
title: "Integrace protokolu aaaIntegrating Azure Security Center výstrahy s Azure | Microsoft Docs"
description: "Tento článek vám pomůže začít pracovat s integrací výstrahy Security Center s integrací protokolů Azure."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: d2d088d3-d38d-47ff-a062-c78e0fd59226
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/23/2017
ms.author: terrylan
ms.openlocfilehash: 2649036ee990bf0f48fa0cb35c7495ac932c29ed
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="integrating-azure-security-center-alerts-with-azure-log-integration"></a>Integrace Azure Security Center výstrahy s integrací protokolů Azure
Mnoho operací zabezpečení a reakce na incidenty týmy závisí na informace o zabezpečení a událostí správy (SIEM) řešení jako výchozí bod pro triaging a prošetřování výstrah zabezpečení hello. Díky integraci protokolu Azure můžete integrovat Azure Security Center výstrahy s vaším řešením SIEM.

Integrace protokolů Azure aktuálně podporuje HP ArcSight, Splunk a IBM QRadar.

## <a name="what-logs-can-i-integrate"></a>Jaké protokoly můžete integrovat?
Azure vytvoří rozsáhlé protokolování pro každou službu. Tyto protokoly jsou klasifikovány jako:

* **Řízení nebo protokoly** , poskytují přehled o hello operace vytvoření správce prostředků Azure, aktualizace a odstranění. Tyto události rovině řízení jsou prezentované v hello aktivity protokolů Azure.
* **Datové roviny protokoly** , poskytují přehled o události hello vyvolá, když pomocí prostředek služby Azure. Příkladem je protokolu událostí systému Windows hello, kde může získat informace o události zabezpečení z prohlížeče událostí hello zabezpečený kanál. Azure diagnostické protokoly jsou prezentované dat roviny událostí (které jsou generované virtuálního počítače nebo služby Azure).

Integrace protokolů Azure aktuálně podporuje integraci hello:

* Azure protokoly virtuálních počítačů
* Protokoly auditu Azure
* Výstrahy Azure Security Center

## <a name="install-azure-log-integration"></a>Nainstalujte integrační protokolů Azure
Stáhněte si [integrace Azure protokolu](https://www.microsoft.com/download/details.aspx?id=53324).

Hello protokolů Azure integrační služba shromažďuje telemetrická data z hello počítače, na kterém je nainstalovaný.  Mezi shromažďovaná telemetrická data je:

* Výjimky, ke kterým došlo během provádění integrace protokolů Azure
* Metriky o počtu hello dotazy a zpracování událostí
* Statistiky, o které Azlog.exe jsou používány možnosti příkazového řádku

> [!NOTE]
> Zrušením zaškrtnutí políčka tuto možnost můžete vypnout kolekce telemetrická data.
>
>

## <a name="integrate-azure-audit-logs-and-security-center-alerts"></a>Integrovat výstrahy a protokolování auditu Azure Security Center
1. Hello otevřete příkazový řádek a **cd** do **c:\Program Files\Microsoft Azure protokolu integrace**.
2. Spustit hello **azlog createazureid** toocreate příkaz [Azure Active Directory Service Principal](../active-directory/active-directory-application-objects.md) v Azure Active Directory (AD) hello klientům, které jsou hostiteli hello předplatných Azure.

    Budete vyzváni k přihlášení Azure.

   > [!NOTE]
   > Musí být předplatné hello vlastníka správce nebo spolusprávce předplatného hello.
   >
   >

    TooAzure ověřování se provádí prostřednictvím služby Azure AD.  Vytvoření objektu služby pro integraci protokolů Azure vytvoří hello Azure AD identity, který je přiřazen přístup tooread z předplatných Azure.
3. Spustit hello **azlog Autorizovat <SubscriptionID> ** příkaz tooassign čtečky přístup na hello předplatné toohello instanční objekt vytvořili v kroku 2. Pokud nezadáte **SubscriptionID**, pak hello instanční objekt je přiřazené hello čtečky role tooall odběry toowhich máte přístup.

   > [!NOTE]
   > Mohou se zobrazit upozornění, pokud spustíte hello **Autorizovat** příkaz ihned po hello **createazureid** příkaz. Některé latence mezi při vytvoření účtu hello Azure AD a pokud není k dispozici pro použití účtu hello neexistuje. Pokud Počkejte asi 10 sekund, po spuštění hello **createazureid** příkaz toorun hello **Autorizovat** příkaz, pak byste neměli vidět tato upozornění.
   >
   >
4. Zkontrolujte, že existují následující tooconfirm složky, která hello soubory JSON protokolu auditu hello:

   * **c:\Users\azlog\AzureResourceManagerJson**
   * **c:\Users\azlog\AzureResourceManagerJsonLD**
5. Zkontrolujte následující tooconfirm složek, které výstrahy Security Center existovat v nich hello:

   * **c:\Users\azlog\ AzureSecurityCenterJson**
   * **c:\Users\azlog\AzureSecurityCenterJsonLD**
6. Nakonfigurujte hello SIEM soubor předávání konektor toohello příslušnou složku. Postup Hello se budou lišit v závislosti na hello SIEM, kterou používáte.

## <a name="next-steps"></a>Další kroky
toolearn informace o Azure protokoly aktivity a definice vlastností v tématu:

* [Audit operací pomocí Resource Manageru](../azure-resource-manager/resource-group-audit.md)

toolearn Další informace o Security Center, najdete v části hello následující:

* [Správa a zda odpovídá toosecurity výstrahy v Azure Security Center](security-center-managing-and-responding-alerts.md) – Další informace jak toomanage a reakce toosecurity výstrahy.
* [Nejčastější dotazy k Azure Security Center](security-center-faq.md) – přečtěte si nejčastější dotazy o použití služby hello.
* [Blog o bezpečnosti Azure](http://blogs.msdn.com/b/azuresecurity/) – získejte nejnovější informace zabezpečení Azure hello a informace.
