---
title: "aaaAzure Key Vault řešení v Log Analytics | Microsoft Docs"
description: "Řešení Azure Key Vault hello můžete použít v tooreview analýzy protokolů, protokoly Azure Key Vault."
services: log-analytics
documentationcenter: 
author: richrundmsft
manager: jochan
editor: 
ms.assetid: 5e25e6d6-dd20-4528-9820-6e2958a40dae
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/09/2017
ms.author: richrund
ms.openlocfilehash: 1c6eae26ded7ad55b0159a3be09cdc9901596298
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-key-vault-analytics-solution-in-log-analytics"></a>Azure Key Vault Analytics řešení v analýzy protokolů

![Symbol Key Vault](./media/log-analytics-azure-keyvault/key-vault-analytics-symbol.png)

V tooreview analýzy protokolů, Azure Key Vault AuditEvent protokolů můžete použít hello Azure Key Vault řešení.

toouse hello řešení, musíte tooenable protokolování Azure Key Vault diagnostiky a přímé hello pracovní prostor analýzy protokolů tooa diagnostiky. Není nutné toowrite hello protokoly tooAzure Blob storage.

> [!NOTE]
> V ledna 2017 podporované hello způsob odesílání protokolů z tooLog Key Vault, analýzy se změnila. Pokud používáte hello Key Vault řešení zobrazuje *(nepoužívané)* v hello název označují příliš[migrace z řešení Key Vault staré hello](#migrating-from-the-old-key-vault-solution) kroky musíte toofollow.
>
>

## <a name="install-and-configure-hello-solution"></a>Instalace a konfigurace řešení hello
Použijte následující pokyny tooinstall hello a konfigurovat řešení Azure Key Vault hello:

1. Povolit hello řešení Azure Key Vault z [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.KeyVaultAnalyticsOMS?tab=Overview) nebo pomocí hello procesu popsaného v tématu [řešení přidat analýzy protokolů z hello řešení Galerie](log-analytics-add-solutions.md).
2. Povolte diagnostiku protokolování pro toomonitor prostředky hello Key Vault, pomocí buď hello [portál](#enable-key-vault-diagnostics-in-the-portal) nebo [prostředí PowerShell](#enable-key-vault-diagnostics-using-powershell)

### <a name="enable-key-vault-diagnostics-in-hello-portal"></a>Povolte diagnostiku Key Vault hello portálu

1. V hello portálu Azure přejděte toomonitor prostředků toohello Key Vault
2. Vyberte *protokolů diagnostiky* tooopen hello následující stránky

   ![Obrázek dlaždice Azure Key Vault](./media/log-analytics-azure-keyvault/log-analytics-keyvault-enable-diagnostics01.png)
3. Klikněte na tlačítko *zapněte diagnostiku* tooopen hello následující stránky

   ![Obrázek dlaždice Azure Key Vault](./media/log-analytics-azure-keyvault/log-analytics-keyvault-enable-diagnostics02.png)
4. Klikněte na tlačítko tooturn na Diagnostika, *na* pod *stav*
5. Klikněte na políčko hello *odeslat tooLog Analytics*
6. Vyberte existující pracovní prostor analýzy protokolů, nebo vytvořit pracovní prostor
7. tooenable *AuditEvent* protokoly, klikněte na zaškrtávací políčko hello v protokolu
8. Klikněte na tlačítko *Uložit* tooenable hello protokolování diagnostiky tooLog Analytics

### <a name="enable-key-vault-diagnostics-using-powershell"></a>Povolit Key Vault diagnostiku pomocí prostředí PowerShell
Hello následující skript prostředí PowerShell představuje příklad, jak toouse `Set-AzureRmDiagnosticSetting` tooenable diagnostické protokolování pro Key Vault:
```
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$kv = Get-AzureRmKeyVault -VaultName 'ContosoKeyVault'

Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId  -WorkspaceId $workspaceId -Enabled $true
```



## <a name="review-azure-key-vault-data-collection-details"></a>Zkontrolujte podrobnosti kolekce dat Azure Key Vault
Řešení Azure Key Vault shromažďuje diagnostické protokoly přímo ze hello Key Vault.
Není nutné toowrite hello protokoly tooAzure úložiště objektů Blob a žádný agent je vyžadována pro shromažďování dat.

Hello následující tabulka uvádí metody shromažďování dat a další podrobnosti o tom, jak se údaje pro Azure Key Vault.

| Platforma | Přímé agenta | Agent systémy Center Operations Manager | Azure | Nástroj Operations Manager vyžaduje? | Dat agenta nástroje Operations Manager odeslána prostřednictvím skupiny pro správu | Četnost shromažďování dat |
| --- | --- | --- | --- | --- | --- | --- |
| Azure |  |  |&#8226; |  |  | v případě přijetí |

## <a name="use-azure-key-vault"></a>Použití Azure Key Vault
Po jste [instalaci hello řešení](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.KeyVaultAnalyticsOMS?tab=Overview), zobrazení dat Key Vault hello kliknutím hello **Azure Key Vault** dlaždice z hello **přehled** stránky analýzy protokolů.

![Obrázek dlaždice Azure Key Vault](./media/log-analytics-azure-keyvault/log-analytics-keyvault-tile.png)

Po kliknutí na tlačítko hello **přehled** dlaždice, můžete zobrazit souhrny protokolů a pak zobrazit v toodetails pro hello následujících kategorií:

* Objem všechny operace trezoru klíčů v čase
* Se nezdařila operace svazky v čase
* Průměrná latence provozní operací
* Kvality služeb pro operace s hello počet operací, které trvat víc než 1000 ms a seznam operací, které trvat víc než 1000 ms

![Obrázek řídicí panel Azure Key Vault](./media/log-analytics-azure-keyvault/log-analytics-keyvault01.png)

![Obrázek řídicí panel Azure Key Vault](./media/log-analytics-azure-keyvault/log-analytics-keyvault02.png)

### <a name="tooview-details-for-any-operation"></a>Podrobnosti o tooview pro všechny operace
1. Na hello **přehled** klikněte na tlačítko hello **Azure Key Vault** dlaždici.
2. Na hello **Azure Key Vault** řídicí panel, zkontrolujte souhrnné informace hello v jednom z okna hello a pak klikněte na jednu tooview podrobné informace o něm hello protokolu vyhledávání stránce.

    Na žádném z hello protokolu hledání stránky můžete zobrazit výsledky čas, podrobné výsledky a historii hledání protokolu. Můžete také filtrovat podle výsledků hello toonarrow omezující vlastnosti.

## <a name="log-analytics-records"></a>Záznamy služby Log Analytics
Hello Azure Key Vault řešení analyzuje záznamy, které mají typ **KeyVaults** shromážděná z [AuditEvent protokoly](../key-vault/key-vault-logging.md) v Azure Diagnostics.  Vlastnosti pro tyto záznamy jsou v následující tabulce hello:  

| Vlastnost | Popis |
|:--- |:--- |
| Typ |*AzureDiagnostics* |
| SourceSystem |*Azure* |
| CallerIpAddress |IP adresa klienta hello, který vytvořil požadavek hello |
| Kategorie | *AuditEvent* |
| CorrelationId |Volitelný GUID, který hello klienta můžete předat toocorrelate protokoly na straně klienta s protokoly na straně služby (Key Vault). |
| DurationMs |Čas, kdy trvalo požadavku REST API hello tooservice, v milisekundách. Tentokrát nezahrnuje latenci sítě, takže hello čas, který budete vyhodnocovat na straně klienta hello se může lišit. |
| httpStatusCode_d |Vrácený požadavek hello stavový kód HTTP (například *200*) |
| id_s |Jedinečné ID požadavku hello |
| identity_claim_appid_g | Identifikátor GUID pro id aplikace hello |
| OperationName |Název hello operace, jak je uvedeno v [protokolování Azure Key Vault](../key-vault/key-vault-logging.md) |
| OperationVersion |Verze rozhraní REST API požadovaná klientem hello (například *2015-06-01*) |
| requestUri_s |Identifikátor URI požadavku hello |
| Prostředek |Název trezoru klíčů hello |
| ResourceGroup |Skupiny prostředků hello trezoru klíčů |
| ID prostředku |ID prostředku Azure Resource Manageru Pro protokoly Key Vault to je ID prostředku Key Vault hello |
| ResourceProvider |*SPOLEČNOSTI MICROSOFT. KEYVAULT* |
| ResourceType | *TREZORY* |
| ResultSignature |Stav protokolu HTTP (například *OK*) |
| ResultType |Výsledek požadavku REST API (například *úspěch*) |
| SubscriptionId |ID předplatného Azure hello předplatného obsahující hello Key Vault |

## <a name="migrating-from-hello-old-key-vault-solution"></a>Migrace z řešení Key Vault staré hello
V ledna 2017 podporované hello způsob odesílání protokolů z tooLog Key Vault, analýzy se změnila. Tyto změny poskytují hello následující výhody:
+ Protokoly zapisují přímo tooLog Analytics bez hello potřebovat toouse účet úložiště
+ Menší latenci hello čase, kdy protokoly generované toothem, který je k dispozici v analýzy protokolů
+ Méně kroků konfigurace
+ Běžný formát pro všechny typy Azure diagnostics

toouse hello aktualizovat řešení:

1. [Konfigurace diagnostiky toobe odeslaný přímo tooLog Analytics Key Vault](#enable-key-vault-diagnostics-in-the-portal)  
2. Povolení hello Azure Key Vault řešení pomocí hello procesu popsaného v tématu [řešení přidat analýzy protokolů z hello Galerie řešení](log-analytics-add-solutions.md)
3. Aktualizovat žádné uložené dotazy, řídicí panely nebo výstrahy toouse hello nový datový typ.
  + Typ je změna z: KeyVaults tooAzureDiagnostics. Můžete použít hello ResourceType toofilter tooKey protokoly trezoru.
  - Místo: `Type=KeyVaults`, použijte`Type=AzureDiagnostics ResourceType=VAULTS`
  + Pole: (názvy polí jsou malá a velká písmena)
  - Pro každé pole, které má příponu \_s, \_d, nebo \_g v hello názvu, změna hello první znak toolower případu
  - Pro každé pole, které má příponu \_o název, hello dat je rozdělená do jednotlivých polí podle hello vnořené názvy polí. Například hello UPN volající hello je uložený v poli`identity_claim_http_schemas_xmlsoap_org_ws_2005_05_identity_claims_upn_s`
   - Pole CallerIpAddress změnit tooCallerIPAddress
   - Pole RemoteIPCountry je již k dispozici
4. Odebrat hello *klíč trezoru Analytics (nepoužívané)* řešení. Pokud používáte prostředí PowerShell, použijte`Set-AzureOperationalInsightsIntelligencePack -ResourceGroupName <resource group that hello workspace is in> -WorkspaceName <name of hello log analytics workspace> -IntelligencePackName "KeyVault" -Enabled $false`

Data jsou shromažďována před hello změn není zobrazená v nové řešení hello. Můžete dál tooquery pro tato data pomocí hello starého typu a názvy polí.

## <a name="troubleshooting"></a>Řešení potíží
[!INCLUDE [log-analytics-troubleshoot-azure-diagnostics](../../includes/log-analytics-troubleshoot-azure-diagnostics.md)]

## <a name="next-steps"></a>Další kroky
* Použití [přihlásit analýzy protokolů hledání](log-analytics-log-searches.md) tooview podrobná data Azure Key Vault.
