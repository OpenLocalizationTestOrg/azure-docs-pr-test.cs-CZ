---
title: "aaaAzure protokolu integrace – nejčastější dotazy | Microsoft Docs"
description: "Tento článek obsahuje odpovědi na otázky týkající se integrace se službou Azure protokolu."
services: security
documentationcenter: na
author: TomShinder
manager: MBaldwin
editor: TerryLanfear
ms.assetid: d06d1ac5-5c3b-49de-800e-4d54b3064c64
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload8: na
ms.date: 08/07/2017
ms.author: TomSh
ms.custom: azlog
ms.openlocfilehash: e886035c9a180d0cd5fcbe9cc02483782df6dbe4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-log-integration-faq"></a>Integrace Azure protokolu – nejčastější dotazy
Tento článek obsahuje odpovědi na nejčastější dotazy (FAQ) informace o integraci Azure protokolu. 

Integrace se službou Azure protokolu je služba Windows operačního systému, které do vaší místní zabezpečení informací a událostí (SIEM) systémy správy můžete toointegrate nezpracovaná protokoly z vašich prostředků Azure. Tato integrace poskytuje jednotný řídicí panel pro všechny vaše prostředky, místně nebo v cloudu hello. Můžete pak agregace, korelovat, analyzovat a výstrahy zabezpečení události související s vašimi aplikacemi.

## <a name="is-hello-azure-log-integration-software-free"></a>Je software, integrace se službou Azure protokolu hello volné?
Ano. Není nijak zpoplatněn hello softwaru protokolu integrace se službou Azure.

## <a name="where-is-azure-log-integration-available"></a>Kde je k dispozici protokolu integrace se službou Azure?

Je aktuálně k dispozici v komerčních Azure a Azure Government a není k dispozici v Číně nebo v Německu.

## <a name="how-can-i-see-hello-storage-accounts-from-which-azure-log-integration-is-pulling-azure-vm-logs"></a>Jak lze zobrazit účty hello úložiště, ze kterých je integrace se službou Azure protokolu stahování protokolů virtuálního počítače Azure?
Spusťte příkaz hello **azlog zdrojového seznamu**.

## <a name="how-can-i-tell-which-subscription-hello-azure-log-integration-logs-are-from"></a>Jak poznám, které předplatné hello protokolu integrace se službou Azure protokoly jsou z?

V případě hello protokolů auditu, které jsou umístěny v hello **AzureResourcemanagerJson** adresáře, ID předplatného hello je v názvu souboru protokolu hello. To platí také pro protokoly v hello **AzureSecurityCenterJson** složky. Například:

20170407T070805_2768037.0000000023. **1111e5ee-1111-111b-a11e-1e111e1111dc**.json

Protokoly auditu Azure Active Directory zahrnují ID klienta hello hello názvu.

Diagnostické protokoly, které se načítají z centra událostí nebudou obsahovat ID předplatného hello hello názvu. Místo toho obsahují hello popisný název zadaný jako součást hello vytvoření hello event hub zdroje. 

## <a name="how-can-i-update-hello-proxy-configuration"></a>Jak můžete aktualizovat konfiguraci proxy serveru hello?
Pokud vaše nastavení proxy serveru přímo neumožňuje přístup k úložišti Azure, otevřete hello **AZLOG. SOUBOR EXE. KONFIGURACE** souboru v **c:\Program Files\Microsoft Azure protokolu integrace**. Aktualizace hello souboru tooinclude hello **defaultProxy –** oddíl s adresou proxy serveru hello vaší organizace. Po dokončení aktualizace hello zastavení a spuštění služby hello pomocí příkazů hello **net stop azlog** a **net start azlog**.

    <?xml version="1.0" encoding="utf-8"?>
    <configuration>
      <system.net>
        <connectionManagement>
          <add address="*" maxconnection="400" />
        </connectionManagement>
        <defaultProxy>
          <proxy usesystemdefault="true"
          proxyaddress=http://127.0.0.1:8888
          bypassonlocal="true" />
        </defaultProxy>
      </system.net>
      <system.diagnostics>
        <performanceCounters filemappingsize="20971520" />
      </system.diagnostics>   

## <a name="how-can-i-see-hello-subscription-information-in-windows-events"></a>Jak lze zobrazit informace o předplatném hello v události systému Windows?
Hello předplatné ID toohello popisný název připojení při přidávání hello zdroje:

    Azlog source add <sourcefriendlyname>.<subscription id> <StorageName> <StorageKey>  
událost Hello XML má hello následující metadata, včetně hello ID předplatného:

![Událost XML][1]

## <a name="error-messages"></a>Chybové zprávy
### <a name="when-i-run-hello-command-azlog-createazureid-why-do-i-get-hello-following-error"></a>Při spuštění příkazu hello **azlog createazureid**, proč se zobrazí následující chyba hello?
Chyba:

  *Se nezdařilo toocreate aplikace AAD - klienta 72f988bf-86f1-41af-91ab-2d7cd011db37 - důvod = "Zakázáno" - zpráva = "nedostatečná oprávnění toocomplete hello operaci."*

Hello **azlog createazureid** příkaz pokusí toocreate má přístup k objektu služby ve všech klientů hello Azure AD pro hello předplatné, které hello přihlášení k Azure. Pokud vaše Azure přihlášení je pouze uživatel guest v tomto klientovi Azure AD, hello příkazu se nezdaří s "nedostatečná oprávnění toocomplete hello operace." Požádejte správce tooadd hello klienta účtu jako uživatel v klientovi hello.

### <a name="when-i-run-hello-command-azlog-authorize-why-do-i-get-hello-following-error"></a>Při spuštění příkazu hello **azlog Autorizovat**, proč se zobrazí následující chyba hello?
Chyba:

  *Upozornění vytvoření přiřazení Role - AuthorizationFailed: hello klienta janedo@microsoft.com' s objektem id 'fe9e03e4-4dad-4328-910f-fd24a9660bd2, nemá autorizaci tooperform akci 'Microsoft.Authorization/roleAssignments/write' v oboru, nebo odběry / 70d 95299-d689-4c 97-b971-0d8ff0000000'.*

Hello **azlog Autorizovat** příkaz přiřadí hello role služby objektu čtečky toohello Azure AD (vytvořené pomocí **azlog createazureid**) toohello poskytuje odběry. Pokud není hello přihlášení k Azure společné správce nebo vlastníka předplatného hello, se nezdaří s chybovou zprávou "Autorizace se nezdařilo". Azure na základě rolí řízení přístupu (RBAC) společné správce nebo vlastníka je potřebné toocomplete tuto akci.

## <a name="where-can-i-find-hello-definition-of-hello-properties-in-hello-audit-log"></a>Kde najdu hello Definice hello vlastností v protokolu auditu hello?
Přejděte na téma:

* [Operace auditu pomocí Azure Resource Manageru](../azure-resource-manager/resource-group-audit.md)
* [Seznam událostí správy hello v předplatném v hello REST API služby Azure monitorování](https://msdn.microsoft.com/library/azure/dn931934.aspx)

## <a name="where-can-i-find-details-on-azure-security-center-alerts"></a>Kde můžete najít podrobnosti o ve výstrahách Azure Security Center?
V tématu [správy a zda odpovídá toosecurity výstrahy v Azure Security Center](../security-center/security-center-managing-and-responding-alerts.md).

## <a name="how-can-i-modify-what-is-collected-with-vm-diagnostics"></a>Jak lze upravit co je shromažďováno s diagnostikou virtuálního počítače?
Podrobnosti o tom, jak tooget, upravit a nastavení konfigurace Azure Diagnostics hello, najdete v tématu [tooenable použijte PowerShell Azure Diagnostics ve virtuálním počítači s Windows](../virtual-machines/windows/ps-extensions-diagnostics.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Následující ukázka Hello získá hello Azure Diagnostics konfigurace:

    -AzureRmVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient
    $publicsettings = (Get-AzureRmVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient).PublicSettings
    $encodedconfig = (ConvertFrom-Json -InputObject $publicsettings).xmlCfg
    $xmlconfig = [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($encodedconfig))
    Write-Host $xmlconfig

    $xmlconfig | Out-File -Encoding utf8 -FilePath "d:\WADConfig.xml"

Následující ukázka Hello upraví konfiguraci Azure Diagnostics hello. V této konfiguraci pouze událost ID 4624 a událost ID 4625 se shromažďují z protokolu událostí zabezpečení hello. Antimalware od Microsoftu pro Azure události se shromažďují z protokolu událostí systému hello. Podrobnosti o použití hello výrazech XPath v tématu [využívání události](https://msdn.microsoft.com/library/windows/desktop/dd996910(v=vs.85)).

    <WindowsEventLog scheduledTransferPeriod="PT1M">
        <DataSource name="Security!*[System[(EventID=4624 or EventID=4625)]]" />
        <DataSource name="System!*[System[Provider[@Name='Microsoft Antimalware']]]"/>
    </WindowsEventLog>

Hello následující příklad nastaví hello Azure Diagnostics konfiguraci:

    $diagnosticsconfig_path = "d:\WADConfig.xml"
    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient -DiagnosticsConfigurationPath $diagnosticsconfig_path -StorageAccountName log3121 -StorageAccountKey <storage key>

Po provedení změn, zkontrolujte že hello správný, že jsou shromažďovány události tooensure účet úložiště hello.

Pokud máte problémy při hello instalace a konfigurace, otevřete prosím [žádost o podporu](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). Vyberte **integrace protokolu** jako hello služby, pro kterou jsou žádosti o podporu.

## <a name="can-i-use-azure-log-integration-toointegrate-network-watcher-logs-into-my-siem"></a>Můžete použít protokol integrace se službou Azure toointegrate sledovací proces sítě protokoly do mé SIEM?

Azure sledovací proces sítě generuje velká množství informací o protokolování. Tyto protokoly nejsou určeny toobe odeslané tooa SIEM. cíl Hello podporována pouze pro protokoly sledovací proces sítě je účet úložiště. Integrace se službou Azure protokolu nepodporuje čtení tyto protokoly a přitom k dispozici tooa SIEM.

<!--Image references-->
[1]: ./media/security-azure-log-integration-faq/event-xml.png
