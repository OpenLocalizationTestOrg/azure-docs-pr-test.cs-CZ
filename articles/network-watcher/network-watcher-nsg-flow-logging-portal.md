---
title: "aaaManage toku protokolů skupiny zabezpečení sítě s sledovací proces sítě Azure | Microsoft Docs"
description: "Tato stránka vysvětluje, jak toomanage skupinu zabezpečení sítě toku přihlásí sledovací proces sítě Azure"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 01606cbf-d70b-40ad-bc1d-f03bb642e0af
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: fb250337ab9d1a0c0d0d3569c00bc221dd102a3f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-network-security-group-flow-logs-in-hello-azure-portal"></a>Správa sítě zabezpečení skupiny toku protokolů hello portálu Azure

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-nsg-flow-logging-portal.md)
> - [PowerShell](network-watcher-nsg-flow-logging-powershell.md)
> - [CLI 1.0](network-watcher-nsg-flow-logging-cli-nodejs.md)
> - [CLI 2.0](network-watcher-nsg-flow-logging-cli.md)
> - [REST API](network-watcher-nsg-flow-logging-rest.md)

Protokoly toku skupiny zabezpečení sítě jsou funkce sledovací proces sítě, která vám umožní tooview informace o příchozí a odchozí provoz IP prostřednictvím skupinu zabezpečení sítě. Tyto protokoly toku jsou zapsané ve formátu JSON a obsahují důležité informace, včetně: 

- Odchozí a příchozí tok na základě pravidla.
- Hello síťovou kartu, která hello postup se týká.
- 5 řazené kolekce členů informace o toku hello (zdrojové nebo cílové adresy IP, zdrojový nebo cílový port, protokol).
- Informace o tom, jestli je povolené nebo zakázané přenosy.

## <a name="before-you-begin"></a>Než začnete

Tento scénář předpokládá, že jste již provedli kroky hello v [vytvořit instanci sledovací proces sítě](network-watcher-create.md). scénář Hello také předpokládá, že máte skupinu prostředků s platným virtuálním počítačem.

## <a name="register-insights-provider"></a>Registrace zprostředkovatele statistiky

Pro tok protokolování toowork úspěšně, hello **Microsoft.Insights** zprostředkovatele musí být zaregistrován. tooregister hello poskytovatele, hello proveďte následující kroky: 

1. Přejděte příliš**odběry**a potom vyberte hello předplatné, pro které chcete tooenable toku protokoly. 
2. Na hello **předplatné** vyberte **zprostředkovatelé prostředků**. 
3. Podívejte se na hello seznamu zprostředkovatelů a ověřte, že hello **microsoft.insights** zprostředkovatel registroval. Pokud ne, pak vyberte **zaregistrovat**.

![Zprostředkovatelé zobrazení][providers]

## <a name="enable-flow-logs"></a>Povolení toku protokolů

Tyto kroky provést uživatele hello proces povolení toku protokoly na skupinu zabezpečení sítě.

### <a name="step-1"></a>Krok 1

Přejděte tooa sledovací proces sítě instance a potom vyberte **NSG toku protokoly**.

![Přehled protokoly toku][1]

### <a name="step-2"></a>Krok 2

Hello seznamu vyberte skupinu zabezpečení sítě.

![Přehled protokoly toku][2]

### <a name="step-3"></a>Krok 3 

Na hello **tok se nastavení protokolů** okně nastavte stav hello příliš**na**a pak nakonfigurujte účet úložiště.  Až budete hotoví, vyberte **OK**. Potom vyberte **Uložit**.

![Přehled protokoly toku][3]

## <a name="download-flow-logs"></a>Stažení protokolů o toku

Tok protokoly se ukládají do účtu úložiště. Stáhnout vaše protokoly tooview tok je.

### <a name="step-1"></a>Krok 1

Vyberte protokoly toodownload toku, **toku protokoly si můžete stáhnout z účtů úložiště**. Tento krok přejdete zobrazení účtu úložiště tooa kde si můžete vybrat, které toodownload protokoly.

![Nastavení protokolů toku][4]

### <a name="step-2"></a>Krok 2

Přejděte účet toohello správné úložiště. Potom vyberte **kontejnery** > **insights-log-networksecuritygroupflowevent**.

![Nastavení protokolů toku][5]

### <a name="step-3"></a>Krok 3

Přejděte toohello umístění protokolu hello toku, vyberte ho a potom vyberte **Stáhnout**.

![Nastavení protokolů toku][6]

Informace o struktuře hello hello protokolu najdete v článku [přehled protokolu toku skupiny zabezpečení sítě](network-watcher-nsg-flow-logging-overview.md).

## <a name="next-steps"></a>Další kroky

Zjistěte, jak příliš[vizualizovat protokolů NSG toku pomocí PowerBI](network-watcher-visualize-nsg-flow-logs-power-bi.md).

<!-- Image references -->
[1]: ./media/network-watcher-nsg-flow-logging-portal/figure1.png
[2]: ./media/network-watcher-nsg-flow-logging-portal/figure2.png
[3]: ./media/network-watcher-nsg-flow-logging-portal/figure3.png
[4]: ./media/network-watcher-nsg-flow-logging-portal/figure4.png
[5]: ./media/network-watcher-nsg-flow-logging-portal/figure5.png
[6]: ./media/network-watcher-nsg-flow-logging-portal/figure6.png
[providers]: ./media/network-watcher-nsg-flow-logging-portal/providers.png
