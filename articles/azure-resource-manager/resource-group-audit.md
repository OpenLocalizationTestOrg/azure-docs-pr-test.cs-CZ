---
title: "Zobrazit protokoly aktivita Azure k monitorování zdrojů | Microsoft Docs"
description: "Použijte protokoly aktivity zkontrolujte akcemi uživatelů a chyby. Zobrazuje portálu prostředí Azure PowerShell, rozhraní příkazového řádku Azure a REST."
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
ms.openlocfilehash: 9f90bc80c146c6c2da04aacbc110f7d389c0baa2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="view-activity-logs-to-audit-actions-on-resources"></a>Zobrazit protokoly aktivity akce u prostředků
Prostřednictvím protokolů činnosti můžete určit:

* jaké operace provedené na prostředky v rámci vašeho předplatného
* Kdo inicioval operaci (i když operations iniciovaná back-end službu nevrátí uživatele jako volající)
* Při operaci došlo k chybě
* Stav operace
* Hodnoty další vlastnosti, které vám můžou pomoct zkoumání operaci

[!INCLUDE [resource-manager-audit-limitations](../../includes/resource-manager-audit-limitations.md)]

Můžete načíst informace z protokolů aktivity prostřednictvím portálu, prostředí PowerShell, rozhraní příkazového řádku Azure, rozhraní API pro přehledy REST, nebo [knihovny .NET Insights](https://www.nuget.org/packages/Microsoft.Azure.Insights/).

## <a name="portal"></a>Portál
1. Chcete-li zobrazit protokoly aktivity přes portál, vyberte **monitorování**.
   
    ![Vyberte protokoly aktivity](./media/resource-group-audit/select-monitor.png)

   Nebo pokud chcete automaticky filtrovat protokol aktivit pro konkrétní prostředek nebo skupina prostředků, vyberte **protokol aktivit** z tohoto okna prostředků. Všimněte si, že je protokol aktivit automaticky filtrovaná podle vybraného prostředku.
   
    ![Filtrovat podle prostředků](./media/resource-group-audit/filtered-by-resource.png)
2. V **protokol aktivit** okně se zobrazí souhrn posledních operací.
   
    ![Zobrazit akce](./media/resource-group-audit/audit-summary.png)
3. Pokud chcete omezit počet operací zobrazí, vyberte jiné podmínky. Například na následujícím obrázku **časový interval** a **událostí iniciovaná** pole změnit tak, aby zobrazení akce prováděné konkrétního uživatele nebo aplikace pro poslední měsíc. Vyberte **použít** pro zobrazení výsledků dotazu.
   
    ![Nastavení možností filtru](./media/resource-group-audit/set-filter.png)

4. Pokud potřebujete spusťte dotaz znovu později, vyberte **Uložit** a zadejte název dotazu.
   
    ![uložení dotazu](./media/resource-group-audit/save-query.png)
5. Chcete-li rychle spuštění dotazu, můžete vybrat jeden z předdefinovaných dotazů, například selhání nasazení.

    ![Vybrat dotaz](./media/resource-group-audit/select-quick-query.png)

   Vybraný dotaz automaticky nastaví hodnoty požadované filtru.

    ![Zobrazit chyby nasazení](./media/resource-group-audit/view-failed-deployment.png)   

6. Vyberte jednu z operace se zobrazí souhrn události.

    ![Operace zobrazení](./media/resource-group-audit/view-operation.png)  

## <a name="powershell"></a>PowerShell
1. Chcete-li načíst položky protokolu, spusťte **Get-AzureRmLog** příkaz. Můžete zadat další parametry pro filtrování seznamu položek. Pokud nezadáte počáteční a koncový čas, vrátí se položky za poslední hodinu. Chcete-li například získat operace pro skupinu prostředků během poslední hodiny spustit:

  ```powershell
  Get-AzureRmLog -ResourceGroup ExampleGroup
  ```
   
    Následující příklad ukazuje, jak používat protokol aktivit k operacím research prováděné během zadané doby. Počáteční a koncové datum nejsou zadány ve formátu data.

  ```powershell
  Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime 2015-08-28T06:00 -EndTime 2015-09-10T06:00
  ```

    Nebo můžete použít funkce datum zadat rozsah dat, jako je například posledních 14 dní.
   
  ```powershell 
  Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-14)
  ```

2. V závislosti na čas spuštění, který zadáte může vrátit předchozí příkazy dlouhý seznam operací pro skupinu prostředků. Můžete filtrovat výsledky pro co hledáte tím, že poskytuje kritéria vyhledávání. Například pokud chcete prozkoumat, jak byla zastavena webovou aplikaci, můžete spustit následující příkaz:

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

3. Můžete vyhledat akce prováděné určitého uživatele, i pro skupinu prostředků, která již existuje.

  ```powershell 
  Get-AzureRmLog -ResourceGroup deletedgroup -StartTime (Get-Date).AddDays(-14) -Caller someone@contoso.com
  ```

4. Můžete filtrovat pro operace se nezdařila.

  ```powershell
  Get-AzureRmLog -ResourceGroup ExampleGroup -Status Failed
  ```

5. Můžete se zaměřit na jednu chybu pohledem na stavové zprávy pro tuto položku.
   
        ((Get-AzureRmLog -Status Failed -ResourceGroup ExampleGroup -DetailedOutput).Properties[1].Content["statusMessage"] | ConvertFrom-Json).error
   
    Která vrací:
   
        code           message                                                                        
        ----           -------                                                                        
        DnsRecordInUse DNS record dns.westus.cloudapp.azure.com is already used by another public IP. 


## <a name="azure-cli"></a>Azure CLI
* Chcete-li načíst položky protokolu, spusťte **zobrazit skupiny azure protokolu** příkaz.

  ```azurecli
  azure group log show ExampleGroup --json
  ```


## <a name="rest-api"></a>REST API
Operace REST pro práci s protokolu aktivit jsou součástí [rozhraní REST API pro přehledy](https://msdn.microsoft.com/library/azure/dn931943.aspx). Načtení aktivity protokolu události, najdete v části [seznam událostí správy v předplatném](https://msdn.microsoft.com/library/azure/dn931934.aspx).

## <a name="next-steps"></a>Další kroky
* Azure protokoly aktivity s Power BI můžete použít k získání lepší přehled o akcích v rámci vašeho předplatného. V tématu [zobrazení a analýza protokolů Azure aktivity v Power BI a další](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/).
* Další informace o nastavení zásad zabezpečení najdete v tématu [řízení přístupu na základě Role v Azure](../active-directory/role-based-access-control-configure.md).
* Další informace o příkazy pro zobrazení operace nasazení najdete v tématu [zobrazit operace nasazení](resource-manager-deployment-operations.md).
* Informace o tom, aby se zabránilo odstranění prostředku pro všechny uživatele, najdete v části [zamknutí prostředků pomocí Azure Resource Manageru](resource-group-lock-resources.md).

