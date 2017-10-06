---
title: "aaaCustomize webové aplikace pravidla brány firewall v Azure Application Gateway - portálu Azure | Microsoft Docs"
description: "Tento článek obsahuje informace o tom, jak pravidla brány firewall webových aplikací toocustomize v aplikační brány s hello portálu Azure."
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 1159500b-17ba-41e7-88d6-b96986795084
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: 
ms.workload: infrastructure-services
ms.date: 03/28/2017
ms.author: gwallace
ms.openlocfilehash: 36a999279e0370b9f803e12257856a56753b23a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="customize-web-application-firewall-rules-through-hello-azure-portal"></a>Přizpůsobení pravidel brány firewall webových aplikací prostřednictvím hello portálu Azure

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-customize-waf-rules-portal.md)
> * [PowerShell](application-gateway-customize-waf-rules-powershell.md)
> * [Azure CLI 2.0](application-gateway-customize-waf-rules-cli.md)

brány firewall webových aplikací Hello Azure Application Gateway (firewall webových aplikací) poskytuje ochranu pro webové aplikace. Tyto ochrany jsou poskytovány hello aplikace otevřete webový projekt zabezpečení (OWASP) nastavit pravidlo jádra (řádku). Některá pravidla můžete způsobit falešně pozitivních zjištění a blokování skutečné provozu. Z tohoto důvodu Application Gateway poskytuje hello schopností toocustomize pravidlo skupiny a pravidel. Další informace o hello konkrétní pravidlo skupiny a pravidel najdete v tématu [seznam webových aplikací brány firewall řádku pravidlo skupiny a pravidel](application-gateway-crs-rulegroups-rules.md).

>[!NOTE]
> Pokud svoji službu application gateway nepoužívá hello firewall webových aplikací vrstvy, hello možnost tooupgrade hello aplikace brány toohello firewall webových aplikací úroveň se zobrazí v pravém podokně hello. 

![Povolit firewall webových aplikací][fig1]

## <a name="view-rule-groups-and-rules"></a>Zobrazit skupiny pravidla a pravidla

**skupiny tooview pravidla a pravidla**
   1. Procházet toohello aplikační bránu a potom vyberte **brány firewall webových aplikací**.  
   2. Vyberte **rozšířeného pravidla konfigurace**.  
   Toto zobrazení uvádí tabulku na stránku hello všech skupin pravidlo hello součástí hello zvolené sady pravidel. Jsou vybrány všechny hello pravidlo zaškrtávacích políček.

![Konfigurace zakázaná pravidla][1]

## <a name="search-for-rules-toodisable"></a>Vyhledejte toodisable pravidla

Hello **webové aplikace, nastavení brány firewall** okno poskytuje možnost hello toofilter hello pravidla prostřednictvím fulltextové vyhledávání. výsledek Hello zobrazí pouze skupiny hello pravidla a pravidla, které obsahují text hello, kterého jste hledali.

![Vyhledejte pravidla][2]

## <a name="disable-rule-groups-and-rules"></a>Zakázat pravidlo skupiny a pravidel

Pokud vaše jste zakázání pravidla, můžete zakázat skupinu celé pravidlo nebo konkrétní pravidla v rámci jedné nebo více skupin pravidlo. 

**toodisable pravidlo skupiny nebo konkrétní pravidla**

   1. Hledat hello pravidla nebo skupiny pravidel, které chcete toodisable.
   2. Zrušte zaškrtnutí políček hello hello pravidla, které chcete toodisable. 
   2. Vyberte **Uložit**. 

![Uložit změny][3]

## <a name="next-steps"></a>Další kroky

Po dokončení konfigurace zakázaná pravidla, dozvíte, jak tooview protokolů firewall webových aplikací. Další informace najdete v tématu [diagnostics Application Gateway](application-gateway-diagnostics.md#diagnostic-logging).

[fig1]: ./media/application-gateway-customize-waf-rules-portal/1.png
[1]: ./media/application-gateway-customize-waf-rules-portal/figure1.png
[2]: ./media/application-gateway-customize-waf-rules-portal/figure2.png
[3]: ./media/application-gateway-customize-waf-rules-portal/figure3.png
