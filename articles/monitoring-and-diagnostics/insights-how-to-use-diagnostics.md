---
title: "Povolit monitorování a Diagnostika v Microsoft Azure | Microsoft Docs"
description: "Naučte se nastavení diagnostiky pro vaše prostředky v Azure."
author: rboucher
manager: carolz
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: af1947a9-c211-4aa1-8924-880a86240be4
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/06/2017
ms.author: robb
ms.openlocfilehash: b82bb1ab419831e803689edb2a2a7fe256dde5a2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="enable-monitoring-and-diagnostics"></a>Povolení monitorování a diagnostiky
V [portálu Azure](https://portal.azure.com), můžete konfigurovat bohaté, časté, monitorování a Diagnostika data o vašich prostředků. Můžete také [REST API](https://msdn.microsoft.com/library/azure/dn931932.aspx) nebo [.NET SDK](http://www.nuget.org/packages/Microsoft.Azure.Management.Monitor) ke konfiguraci diagnostiky prostřednictvím kódu programu.

Data diagnostiky, monitorování a metriky v Azure je uložen do účtu úložiště podle vašeho výběru. To umožňuje použít libovolnou nástrojů, které chcete číst data, z Průzkumníka úložiště, do Power BI a jiných nástrojů.

## <a name="when-you-create-a-resource"></a>Při vytváření prostředku
Většina služeb umožňují zapněte diagnostiku, při prvním vytváření je do [portálu Azure](https://portal.azure.com).

1. Přejděte na **nový** a vyberte prostředek, které vás zajímají.
2. Vyberte **volitelné konfiguraci**.
    ![Okno diagnostiky](./media/insights-how-to-use-diagnostics/Insights_CreateTime.png)
3. Vyberte **diagnostiky**a klikněte na tlačítko **na**. Musíte zvolit účet úložiště, které chcete diagnostiky ukládání. Budete budou účtovat normální datové sazby za úložiště a transakce při odeslání diagnostiky do účtu úložiště.
4. Klikněte na tlačítko **OK** a vytvořit prostředek.

## <a name="change-settings-for-an-existing-resource"></a>Změna nastavení pro existující prostředek
Pokud jste již vytvořili prostředku a chcete změnit nastavení diagnostiky (Chcete-li například změnit úroveň shromažďování dat), můžete udělat toto právo na portálu Azure.

1. Přejděte na prostředek a kliknutím **nastavení** příkaz.
2. Vyberte **diagnostiky**.
3. **Diagnostiky** okno obsahuje všechny možné diagnostiky a monitorování kolekce dat pro tento prostředek. Pro některé prostředky, můžete také **uchování** zásady pro data, vyčistěte z vašeho účtu úložiště.
    ![Úložiště diagnostiky](./media/insights-how-to-use-diagnostics/Insights_StorageDiagnostics.png)
4. Jakmile jste vybrali nastavení, klikněte na možnost **Uložit** příkaz. Může trvat o něco při pro monitorování dat. objeví, pokud povolíte ji poprvé.

### <a name="categories-of-data-collection-for-virtual-machines"></a>Kategorie shromažďování dat pro virtuální počítače
Pro virtuální počítače všechny metriky a protokoly budou popsané v intervalech jedné minuty, abyste měli vždy nejnovější informace o vašem počítači.

* **Základní metriky** : stavu metriky o vašem virtuálním počítači, jako je například procesoru a paměti
* **Síťové a webové metriky** : metrik týkajících se připojení k síti a webové služby
* **Metriky rozhraní .NET** : metriky o aplikace .NET a ASP.NET, které jsou spuštěny na virtuálním počítači
* **Metriky SQL** : Pokud je spuštěná služba Microsoft SQL, jeho metrik výkonu.
* **Protokoly událostí aplikace systému Windows** : události systému Windows, které se odesílají do aplikace kanál
* **Protokoly událostí systému Windows** : události systému Windows, které se odesílají do systému kanál. To také zahrnuje všechny události z [Antimalware od Microsoftu](http://go.microsoft.com/fwlink/?LinkID=404171&clcid=0x409).
* **Protokoly událostí zabezpečení systému Windows** : události systému Windows, které se odesílají do zabezpečený kanál
* **Diagnostické protokoly infrastruktury** : protokolování diagnostiky infrastruktury pro kolekce
* **Protokoly služby IIS** : protokoly o serveru služby IIS

Všimněte si, že v tuto chvíli nepodporuje některých distribucích systému Linux, a, Agent hosta musí být nainstalována na virtuálním počítači.

## <a name="next-steps"></a>Další kroky
* [Přijímejte oznámení o výstrahách](insights-receive-alert-notifications.md) vždy, když nastanou provozní události nebo když metriky překročí prahovou hodnotu.
* [Monitorování služby metriky](insights-how-to-customize-monitoring.md) zkontrolovat služby je k dispozici a dobře reagovaly.
* [Automatické škálování počtu instancí](insights-how-to-scale.md) a ujistěte se, vaše škálování služby na základě poptávky.
* [Monitorování výkonu aplikací](../application-insights/app-insights-azure-web-apps.md) Pokud chcete zjistit, přesně jak kód provádí v cloudu.
* [Zobrazení událostí a protokolu aktivity](insights-debugging-with-events.md) další vše, co se stalo ve službě.
* [Sledování stavu služby](insights-service-health.md) Chcete-li zjistit, kdy Azure došlo výkonu snížení nebo služba přerušení.

