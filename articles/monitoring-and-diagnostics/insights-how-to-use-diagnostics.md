---
title: "aaaEnable monitorování a Diagnostika v Microsoft Azure | Microsoft Docs"
description: "Zjistěte, jak tooset až diagnostiky pro vaše prostředky v Azure."
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
ms.openlocfilehash: 865d010c5846fff6d871e20eca2bc4ac35028354
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-monitoring-and-diagnostics"></a>Povolení monitorování a diagnostiky
V hello [portálu Azure](https://portal.azure.com), můžete konfigurovat bohaté, časté, monitorování a Diagnostika data o vašich prostředků. Můžete taky hello [REST API](https://msdn.microsoft.com/library/azure/dn931932.aspx) nebo [.NET SDK](http://www.nuget.org/packages/Microsoft.Azure.Management.Monitor) tooconfigure diagnostiky prostřednictvím kódu programu.

Data diagnostiky, monitorování a metriky v Azure je uložen do účtu úložiště podle vašeho výběru. To vám umožní toouse ať nástrojů má tooread hello dat, z Průzkumníka úložiště, tooPower BI výrobců toothird nástrojů.

## <a name="when-you-create-a-resource"></a>Při vytváření prostředku
Většina služeb umožňují diagnostiku tooenable při jejich prvním vytvoření v hello [portálu Azure](https://portal.azure.com).

1. Přejděte příliš**nový** a zvolte hello prostředků, které vás zajímají.
2. Vyberte **volitelné konfiguraci**.
    ![Okno diagnostiky](./media/insights-how-to-use-diagnostics/Insights_CreateTime.png)
3. Vyberte **diagnostiky**a klikněte na tlačítko **na**. Budete potřebovat účet úložiště hello toochoose, které chcete toobe diagnostiky, které jsou uloženy do. Budete mít účtovat normální datové sazby za úložiště a transakce při odesílání účet úložiště pro diagnostiku tooa.
4. Klikněte na tlačítko **OK** a vytvořte prostředek hello.

## <a name="change-settings-for-an-existing-resource"></a>Změna nastavení pro existující prostředek
Pokud jste již vytvořili prostředku a chcete, aby nastavení diagnostiky hello toochange (toochange hello úroveň shromažďování dat, např.), můžete provést tohoto práva v hello portálu Azure.

1. Přejděte toohello prostředků a klikněte na tlačítko hello **nastavení** příkaz.
2. Vyberte **diagnostiky**.
3. Hello **diagnostiky** okno obsahuje všechny možné diagnostiky hello a shromažďování dat pro tento prostředek monitorování. Pro některé prostředky, můžete také **uchování** zásady pro hello data, tooclean ho nahoru z vašeho účtu úložiště.
    ![Úložiště diagnostiky](./media/insights-how-to-use-diagnostics/Insights_StorageDiagnostics.png)
4. Jakmile jste vybrali nastavení, klikněte na možnost hello **Uložit** příkaz. Může trvat o něco při pro monitorování dat tooshow nahoru Chcete-li povolit ho pro hello poprvé.

### <a name="categories-of-data-collection-for-virtual-machines"></a>Kategorie shromažďování dat pro virtuální počítače
Pro virtuální počítače všechny metriky a protokoly se zaznamená v intervalech jedné minuty, takže můžete vždy hello většina aktuální informace o vašem počítači.

* **Základní metriky** : stavu metriky o vašem virtuálním počítači, jako je například procesoru a paměti
* **Síťové a webové metriky** : metrik týkajících se připojení k síti a webové služby
* **Metriky rozhraní .NET** : metriky o aplikace .NET a ASP.NET hello spuštěny na virtuálním počítači
* **Metriky SQL** : Pokud je spuštěná služba Microsoft SQL, jeho metrik výkonu.
* **Protokoly událostí aplikace systému Windows** : události systému Windows, odesílaných toohello aplikace kanálu
* **Protokoly událostí systému Windows** : události systému Windows, odesílaných toohello systému kanálu. To také zahrnuje všechny události z [Antimalware od Microsoftu](http://go.microsoft.com/fwlink/?LinkID=404171&clcid=0x409).
* **Protokoly událostí zabezpečení systému Windows** : události systému Windows, které se odesílají toohello zabezpečený kanál
* **Diagnostické protokoly infrastruktury** : protokolování hello diagnostiky kolekce infrastruktury
* **Protokoly služby IIS** : protokoly o serveru služby IIS

Upozorňujeme, že v tuto chvíli nepodporuje některých distribucích systému Linux a, hello Agent hosta musí být nainstalován na hello virtuálního počítače.

## <a name="next-steps"></a>Další kroky
* [Přijímejte oznámení o výstrahách](insights-receive-alert-notifications.md) vždy, když nastanou provozní události nebo když metriky překročí prahovou hodnotu.
* [Monitorování služby metriky](insights-how-to-customize-monitoring.md) toomake, že je služba dostupná a dobře reagovaly.
* [Automatické škálování počtu instancí](insights-how-to-scale.md) toomake, že vaše škálování služby na základě poptávky.
* [Monitorování výkonu aplikací](../application-insights/app-insights-azure-web-apps.md) Pokud chcete toounderstand přesně jak kód provádí v cloudu hello.
* [Zobrazení událostí a protokolu aktivity](insights-debugging-with-events.md) toolearn vše, co se stalo ve službě.
* [Sledování stavu služby](insights-service-health.md) toofind se při Azure došlo výkonu snížení nebo služba přerušení.

