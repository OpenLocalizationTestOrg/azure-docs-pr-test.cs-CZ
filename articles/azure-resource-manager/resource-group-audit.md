---
title: "aaaView aktivita Azure protokoly toomonitor prostředky | Microsoft Docs"
description: "Použití hello aktivity protokoly tooreview uživatele akcí a chyby. Zobrazuje portálu prostředí Azure PowerShell, rozhraní příkazového řádku Azure a REST."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: fcdb3125-13ce-4c3b-9087-f514c5e41e73
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: tomfitz
ms.openlocfilehash: 8430ed2a9c1dfe5f13423a55d358e590b0facb22
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="view-activity-logs-tooaudit-actions-on-resources"></a>Zobrazit aktivitu protokoluje akce tooaudit na prostředky
Prostřednictvím protokolů činnosti můžete určit:

* jaké operace provedené na hello prostředky ve vašem předplatném
* Kdo inicioval hello operace (i když operations iniciovaná back-end službu nevrátí uživatele jako volající hello)
* Pokud došlo k operaci hello
* Hello stav operace hello
* Hello hodnotách jiných vlastností, které vám můžou pomoct zkoumání operaci hello

[!INCLUDE [resource-manager-audit-limitations](../../includes/resource-manager-audit-limitations.md)]

Mohou načítat informace z protokolů činnosti hello prostřednictvím portálu hello prostředí PowerShell, rozhraní příkazového řádku Azure, rozhraní API pro přehledy REST, nebo [knihovny .NET Insights](https://www.nuget.org/packages/Microsoft.Azure.Insights/).

## <a name="portal"></a>Portál
1. Vyberte protokoly aktivity hello tooview prostřednictvím portálu hello **monitorování**.
   
    ![Vyberte protokoly aktivity](./media/resource-group-audit/select-monitor.png)

   Nebo tooautomatically filtru hello protokol aktivit pro konkrétní prostředek nebo skupina prostředků, vyberte **protokol aktivit** z tohoto okna prostředků. Všimněte si, že protokol aktivit hello se automaticky filtruje hello vybraný prostředek.
   
    ![Filtrovat podle prostředků](./media/resource-group-audit/filtered-by-resource.png)
2. V hello **protokol aktivit** okně se zobrazí souhrn posledních operací.
   
    ![Zobrazit akce](./media/resource-group-audit/audit-summary.png)
3. toorestrict hello počet operací se zobrazí, vyberte jiné podmínky. Například hello následující obrázek ukazuje hello **časový interval** a **událostí iniciovaná** pole změněna tooview hello akce provedené konkrétní uživatele nebo aplikace pro hello poslední měsíc. Vyberte **použít** tooview hello výsledky dotazu.
   
    ![Nastavení možností filtru](./media/resource-group-audit/set-filter.png)

4. Pokud později potřebujete toorun hello dotazu, vyberte **Uložit** a pojmenujte hello dotazu.
   
    ![uložení dotazu](./media/resource-group-audit/save-query.png)
5. tooquickly spuštění dotazu, můžete vybrat jeden z předdefinovaných hello dotazů, například selhání nasazení.

    ![Vybrat dotaz](./media/resource-group-audit/select-quick-query.png)

   Vybraný dotaz Hello automaticky nastaví hello požadované hodnoty filtru.

    ![Zobrazit chyby nasazení](./media/resource-group-audit/view-failed-deployment.png)   

6. Vyberte jednu z hello operations toosee souhrn události hello.

    ![Operace zobrazení](./media/resource-group-audit/view-operation.png)  

## <a name="powershell"></a>PowerShell
1. položky protokolu tooretrieve, spusťte hello **Get-AzureRmLog** příkaz. Je zadat další parametry toofilter hello seznamu položek. Pokud nezadáte počáteční a koncový čas, vrátí se záznamy pro hello poslední hodinu. Například spusťte tooretrieve hello operace pro skupinu prostředků během poslední hodiny hello:

  ```powershell
  Get-AzureRmLog -ResourceGroup ExampleGroup
  ```
   
    Hello následující příklad ukazuje, jak toouse hello aktivity protokolu tooresearch operace prováděné během zadané doby. Hello počátečním a koncovým datem nejsou zadány ve formátu data.

  ```powershell
  Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime 2015-08-28T06:00 -EndTime 2015-09-10T06:00
  ```

    Nebo můžete použít funkce toospecify hello datum rozsah dat, jako je například hello posledních 14 dní.
   
  ```powershell 
  Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-14)
  ```

2. V závislosti na hello čas zahájení, který zadáte může vrátit předchozí příkazy hello dlouhý seznam operací pro skupinu prostředků hello. Můžete filtrovat výsledky hello co hledáte tím, že poskytuje kritéria vyhledávání. Například pokud se pokoušíte tooresearch jak webové aplikace byla zastavena, může spustit hello následující příkaz:

  ```powershell
  Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-14) | Where-Object OperationName -eq Microsoft.Web/sites/stop/action
  ```

    V tomto příkladu zobrazující se provádí akce zastavení someone@contoso.com. 

  ```powershell 
  Authorization     :
  Scope     : /subscriptions/xxxxx/resourcegroups/ExampleGroup/providers/Microsoft.Web/sites/ExampleSite
  Action    : Microsoft.Web/sites/stop/action
  Role      : Subscription Admin
  Condition :
  Caller            : someone@contoso.com
  CorrelationId     : 84beae59-92aa-4662-a6fc-b6fecc0ff8da
  EventSource       : Administrative
  EventTimestamp    : 8/28/2015 4:08:18 PM
  OperationName     : Microsoft.Web/sites/stop/action
  ResourceGroupName : ExampleGroup
  ResourceId        : /subscriptions/xxxxx/resourcegroups/ExampleGroup/providers/Microsoft.Web/sites/ExampleSite
  Status            : Succeeded
  SubscriptionId    : xxxxx
  SubStatus         : OK
  ```

3. Můžete vyhledat hello akce prováděné určitého uživatele, i pro skupinu prostředků, která již existuje.

  ```powershell 
  Get-AzureRmLog -ResourceGroup deletedgroup -StartTime (Get-Date).AddDays(-14) -Caller someone@contoso.com
  ```

4. Můžete filtrovat pro operace se nezdařila.

  ```powershell
  Get-AzureRmLog -ResourceGroup ExampleGroup -Status Failed
  ```

5. Můžete se zaměřit na jednu chybu prohlížením hello stavovou zprávu pro tuto položku.
   
        ((Get-AzureRmLog -Status Failed -ResourceGroup ExampleGroup -DetailedOutput).Properties[1].Content["statusMessage"] | ConvertFrom-Json).error
   
    Která vrací:
   
        code           message                                                                        
        ----           -------                                                                        
        DnsRecordInUse DNS record dns.westus.cloudapp.azure.com is already used by another public IP. 


## <a name="azure-cli"></a>Azure CLI
* tooretrieve položky protokolu, spusťte hello **zobrazit skupiny azure protokolu** příkaz.

  ```azurecli
  azure group log show ExampleGroup --json
  ```


## <a name="rest-api"></a>REST API
Hello operace REST pro práci s hello protokolu aktivit jsou součástí hello [rozhraní REST API pro přehledy](https://msdn.microsoft.com/library/azure/dn931943.aspx). tooretrieve aktivity protokolu události, viz [seznam událostí správy hello v předplatném](https://msdn.microsoft.com/library/azure/dn931934.aspx).

## <a name="next-steps"></a>Další kroky
* Azure protokoly aktivity lze použít s Power BI toogain lepší přehled o hello akce v rámci vašeho předplatného. V tématu [zobrazení a analýza protokolů Azure aktivity v Power BI a další](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/).
* toolearn o nastavení zásad zabezpečení, najdete v části [řízení přístupu na základě Role v Azure](../active-directory/role-based-access-control-configure.md).
* toolearn o hello příkazy pro zobrazení operace nasazení, najdete v části [zobrazit operace nasazení](resource-manager-deployment-operations.md).
* jak zjistit, tooprevent odstranění prostředku pro všechny uživatele, toolearn [zamknutí prostředků pomocí Azure Resource Manageru](resource-group-lock-resources.md).

