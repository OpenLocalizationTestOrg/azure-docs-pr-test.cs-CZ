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
# <a name="azure-log-integration-faq"></a><span data-ttu-id="0f719-103">Integrace Azure protokolu – nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="0f719-103">Azure Log Integration FAQ</span></span>
<span data-ttu-id="0f719-104">Tento článek obsahuje odpovědi na nejčastější dotazy (FAQ) informace o integraci Azure protokolu.</span><span class="sxs-lookup"><span data-stu-id="0f719-104">This article answers frequently asked questions (FAQ) about Azure Log Integration.</span></span> 

<span data-ttu-id="0f719-105">Integrace se službou Azure protokolu je služba Windows operačního systému, které do vaší místní zabezpečení informací a událostí (SIEM) systémy správy můžete toointegrate nezpracovaná protokoly z vašich prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="0f719-105">Azure Log Integration is a Windows operating system service that you can use toointegrate raw logs from your Azure resources into your on-premises security information and event management (SIEM) systems.</span></span> <span data-ttu-id="0f719-106">Tato integrace poskytuje jednotný řídicí panel pro všechny vaše prostředky, místně nebo v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="0f719-106">This integration provides a unified dashboard for all your assets, on-premises or in hello cloud.</span></span> <span data-ttu-id="0f719-107">Můžete pak agregace, korelovat, analyzovat a výstrahy zabezpečení události související s vašimi aplikacemi.</span><span class="sxs-lookup"><span data-stu-id="0f719-107">You can then aggregate, correlate, analyze, and alert for security events associated with your applications.</span></span>

## <a name="is-hello-azure-log-integration-software-free"></a><span data-ttu-id="0f719-108">Je software, integrace se službou Azure protokolu hello volné?</span><span class="sxs-lookup"><span data-stu-id="0f719-108">Is hello Azure Log Integration software free?</span></span>
<span data-ttu-id="0f719-109">Ano.</span><span class="sxs-lookup"><span data-stu-id="0f719-109">Yes.</span></span> <span data-ttu-id="0f719-110">Není nijak zpoplatněn hello softwaru protokolu integrace se službou Azure.</span><span class="sxs-lookup"><span data-stu-id="0f719-110">There is no charge for hello Azure Log Integration software.</span></span>

## <a name="where-is-azure-log-integration-available"></a><span data-ttu-id="0f719-111">Kde je k dispozici protokolu integrace se službou Azure?</span><span class="sxs-lookup"><span data-stu-id="0f719-111">Where is Azure Log Integration available?</span></span>

<span data-ttu-id="0f719-112">Je aktuálně k dispozici v komerčních Azure a Azure Government a není k dispozici v Číně nebo v Německu.</span><span class="sxs-lookup"><span data-stu-id="0f719-112">It is currently available in Azure Commercial and Azure Government and is not available in China or Germany.</span></span>

## <a name="how-can-i-see-hello-storage-accounts-from-which-azure-log-integration-is-pulling-azure-vm-logs"></a><span data-ttu-id="0f719-113">Jak lze zobrazit účty hello úložiště, ze kterých je integrace se službou Azure protokolu stahování protokolů virtuálního počítače Azure?</span><span class="sxs-lookup"><span data-stu-id="0f719-113">How can I see hello storage accounts from which Azure Log Integration is pulling Azure VM logs?</span></span>
<span data-ttu-id="0f719-114">Spusťte příkaz hello **azlog zdrojového seznamu**.</span><span class="sxs-lookup"><span data-stu-id="0f719-114">Run hello command **azlog source list**.</span></span>

## <a name="how-can-i-tell-which-subscription-hello-azure-log-integration-logs-are-from"></a><span data-ttu-id="0f719-115">Jak poznám, které předplatné hello protokolu integrace se službou Azure protokoly jsou z?</span><span class="sxs-lookup"><span data-stu-id="0f719-115">How can I tell which subscription hello Azure Log Integration logs are from?</span></span>

<span data-ttu-id="0f719-116">V případě hello protokolů auditu, které jsou umístěny v hello **AzureResourcemanagerJson** adresáře, ID předplatného hello je v názvu souboru protokolu hello.</span><span class="sxs-lookup"><span data-stu-id="0f719-116">In hello case of audit logs that are placed in hello **AzureResourcemanagerJson** directories, hello subscription ID is in hello log file name.</span></span> <span data-ttu-id="0f719-117">To platí také pro protokoly v hello **AzureSecurityCenterJson** složky.</span><span class="sxs-lookup"><span data-stu-id="0f719-117">This is also true for logs in hello **AzureSecurityCenterJson** folder.</span></span> <span data-ttu-id="0f719-118">Například:</span><span class="sxs-lookup"><span data-stu-id="0f719-118">For example:</span></span>

<span data-ttu-id="0f719-119">20170407T070805_2768037.0000000023. **1111e5ee-1111-111b-a11e-1e111e1111dc**.json</span><span class="sxs-lookup"><span data-stu-id="0f719-119">20170407T070805_2768037.0000000023.**1111e5ee-1111-111b-a11e-1e111e1111dc**.json</span></span>

<span data-ttu-id="0f719-120">Protokoly auditu Azure Active Directory zahrnují ID klienta hello hello názvu.</span><span class="sxs-lookup"><span data-stu-id="0f719-120">Azure Active Directory audit logs include hello tenant ID as part of hello name.</span></span>

<span data-ttu-id="0f719-121">Diagnostické protokoly, které se načítají z centra událostí nebudou obsahovat ID předplatného hello hello názvu.</span><span class="sxs-lookup"><span data-stu-id="0f719-121">Diagnostic logs that are read from an event hub do not include hello subscription ID as part of hello name.</span></span> <span data-ttu-id="0f719-122">Místo toho obsahují hello popisný název zadaný jako součást hello vytvoření hello event hub zdroje.</span><span class="sxs-lookup"><span data-stu-id="0f719-122">Instead, they include hello friendly name specified as part of hello creation of hello event hub source.</span></span> 

## <a name="how-can-i-update-hello-proxy-configuration"></a><span data-ttu-id="0f719-123">Jak můžete aktualizovat konfiguraci proxy serveru hello?</span><span class="sxs-lookup"><span data-stu-id="0f719-123">How can I update hello proxy configuration?</span></span>
<span data-ttu-id="0f719-124">Pokud vaše nastavení proxy serveru přímo neumožňuje přístup k úložišti Azure, otevřete hello **AZLOG. SOUBOR EXE. KONFIGURACE** souboru v **c:\Program Files\Microsoft Azure protokolu integrace**.</span><span class="sxs-lookup"><span data-stu-id="0f719-124">If your proxy setting does not allow Azure storage access directly, open hello **AZLOG.EXE.CONFIG** file in **c:\Program Files\Microsoft Azure Log Integration**.</span></span> <span data-ttu-id="0f719-125">Aktualizace hello souboru tooinclude hello **defaultProxy –** oddíl s adresou proxy serveru hello vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="0f719-125">Update hello file tooinclude hello **defaultProxy** section with hello proxy address of your organization.</span></span> <span data-ttu-id="0f719-126">Po dokončení aktualizace hello zastavení a spuštění služby hello pomocí příkazů hello **net stop azlog** a **net start azlog**.</span><span class="sxs-lookup"><span data-stu-id="0f719-126">After hello update is done, stop and start hello service by using hello commands **net stop azlog** and **net start azlog**.</span></span>

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

## <a name="how-can-i-see-hello-subscription-information-in-windows-events"></a><span data-ttu-id="0f719-127">Jak lze zobrazit informace o předplatném hello v události systému Windows?</span><span class="sxs-lookup"><span data-stu-id="0f719-127">How can I see hello subscription information in Windows events?</span></span>
<span data-ttu-id="0f719-128">Hello předplatné ID toohello popisný název připojení při přidávání hello zdroje:</span><span class="sxs-lookup"><span data-stu-id="0f719-128">Append hello subscription ID toohello friendly name while adding hello source:</span></span>

    Azlog source add <sourcefriendlyname>.<subscription id> <StorageName> <StorageKey>  
<span data-ttu-id="0f719-129">událost Hello XML má hello následující metadata, včetně hello ID předplatného:</span><span class="sxs-lookup"><span data-stu-id="0f719-129">hello event XML has hello following metadata, including hello subscription ID:</span></span>

![Událost XML][1]

## <a name="error-messages"></a><span data-ttu-id="0f719-131">Chybové zprávy</span><span class="sxs-lookup"><span data-stu-id="0f719-131">Error messages</span></span>
### <a name="when-i-run-hello-command-azlog-createazureid-why-do-i-get-hello-following-error"></a><span data-ttu-id="0f719-132">Při spuštění příkazu hello **azlog createazureid**, proč se zobrazí následující chyba hello?</span><span class="sxs-lookup"><span data-stu-id="0f719-132">When I run hello command **azlog createazureid**, why do I get hello following error?</span></span>
<span data-ttu-id="0f719-133">Chyba:</span><span class="sxs-lookup"><span data-stu-id="0f719-133">Error:</span></span>

  <span data-ttu-id="0f719-134">*Se nezdařilo toocreate aplikace AAD - klienta 72f988bf-86f1-41af-91ab-2d7cd011db37 - důvod = "Zakázáno" - zpráva = "nedostatečná oprávnění toocomplete hello operaci."*</span><span class="sxs-lookup"><span data-stu-id="0f719-134">*Failed toocreate AAD Application - Tenant 72f988bf-86f1-41af-91ab-2d7cd011db37 - Reason = 'Forbidden' - Message = 'Insufficient privileges toocomplete hello operation.'*</span></span>

<span data-ttu-id="0f719-135">Hello **azlog createazureid** příkaz pokusí toocreate má přístup k objektu služby ve všech klientů hello Azure AD pro hello předplatné, které hello přihlášení k Azure.</span><span class="sxs-lookup"><span data-stu-id="0f719-135">hello **azlog createazureid** command attempts toocreate a service principal in all hello Azure AD tenants for hello subscriptions that hello Azure login has access to.</span></span> <span data-ttu-id="0f719-136">Pokud vaše Azure přihlášení je pouze uživatel guest v tomto klientovi Azure AD, hello příkazu se nezdaří s "nedostatečná oprávnění toocomplete hello operace."</span><span class="sxs-lookup"><span data-stu-id="0f719-136">If your Azure login is only a guest user in that Azure AD tenant, hello command fails with "Insufficient privileges toocomplete hello operation."</span></span> <span data-ttu-id="0f719-137">Požádejte správce tooadd hello klienta účtu jako uživatel v klientovi hello.</span><span class="sxs-lookup"><span data-stu-id="0f719-137">Ask hello tenant admin tooadd your account as a user in hello tenant.</span></span>

### <a name="when-i-run-hello-command-azlog-authorize-why-do-i-get-hello-following-error"></a><span data-ttu-id="0f719-138">Při spuštění příkazu hello **azlog Autorizovat**, proč se zobrazí následující chyba hello?</span><span class="sxs-lookup"><span data-stu-id="0f719-138">When I run hello command **azlog authorize**, why do I get hello following error?</span></span>
<span data-ttu-id="0f719-139">Chyba:</span><span class="sxs-lookup"><span data-stu-id="0f719-139">Error:</span></span>

  <span data-ttu-id="0f719-140">*Upozornění vytvoření přiřazení Role - AuthorizationFailed: hello klienta janedo@microsoft.com' s objektem id 'fe9e03e4-4dad-4328-910f-fd24a9660bd2, nemá autorizaci tooperform akci 'Microsoft.Authorization/roleAssignments/write' v oboru, nebo odběry / 70d 95299-d689-4c 97-b971-0d8ff0000000'.*</span><span class="sxs-lookup"><span data-stu-id="0f719-140">*Warning creating Role Assignment - AuthorizationFailed: hello client janedo@microsoft.com' with object id 'fe9e03e4-4dad-4328-910f-fd24a9660bd2' does not have authorization tooperform action 'Microsoft.Authorization/roleAssignments/write' over scope '/subscriptions/70d95299-d689-4c97-b971-0d8ff0000000'.*</span></span>

<span data-ttu-id="0f719-141">Hello **azlog Autorizovat** příkaz přiřadí hello role služby objektu čtečky toohello Azure AD (vytvořené pomocí **azlog createazureid**) toohello poskytuje odběry.</span><span class="sxs-lookup"><span data-stu-id="0f719-141">hello **azlog authorize** command assigns hello role of reader toohello Azure AD service principal (created with **azlog createazureid**) toohello provided subscriptions.</span></span> <span data-ttu-id="0f719-142">Pokud není hello přihlášení k Azure společné správce nebo vlastníka předplatného hello, se nezdaří s chybovou zprávou "Autorizace se nezdařilo".</span><span class="sxs-lookup"><span data-stu-id="0f719-142">If hello Azure login is not a co-administrator or an owner of hello subscription, it fails with an "Authorization Failed" error message.</span></span> <span data-ttu-id="0f719-143">Azure na základě rolí řízení přístupu (RBAC) společné správce nebo vlastníka je potřebné toocomplete tuto akci.</span><span class="sxs-lookup"><span data-stu-id="0f719-143">Azure Role-Based Access Control (RBAC) of co-administrator or owner is needed toocomplete this action.</span></span>

## <a name="where-can-i-find-hello-definition-of-hello-properties-in-hello-audit-log"></a><span data-ttu-id="0f719-144">Kde najdu hello Definice hello vlastností v protokolu auditu hello?</span><span class="sxs-lookup"><span data-stu-id="0f719-144">Where can I find hello definition of hello properties in hello audit log?</span></span>
<span data-ttu-id="0f719-145">Přejděte na téma:</span><span class="sxs-lookup"><span data-stu-id="0f719-145">See:</span></span>

* [<span data-ttu-id="0f719-146">Operace auditu pomocí Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="0f719-146">Audit operations with Azure Resource Manager</span></span>](../azure-resource-manager/resource-group-audit.md)
* [<span data-ttu-id="0f719-147">Seznam událostí správy hello v předplatném v hello REST API služby Azure monitorování</span><span class="sxs-lookup"><span data-stu-id="0f719-147">List hello management events in a subscription in hello Azure Monitor REST API</span></span>](https://msdn.microsoft.com/library/azure/dn931934.aspx)

## <a name="where-can-i-find-details-on-azure-security-center-alerts"></a><span data-ttu-id="0f719-148">Kde můžete najít podrobnosti o ve výstrahách Azure Security Center?</span><span class="sxs-lookup"><span data-stu-id="0f719-148">Where can I find details on Azure Security Center alerts?</span></span>
<span data-ttu-id="0f719-149">V tématu [správy a zda odpovídá toosecurity výstrahy v Azure Security Center](../security-center/security-center-managing-and-responding-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="0f719-149">See [Managing and responding toosecurity alerts in Azure Security Center](../security-center/security-center-managing-and-responding-alerts.md).</span></span>

## <a name="how-can-i-modify-what-is-collected-with-vm-diagnostics"></a><span data-ttu-id="0f719-150">Jak lze upravit co je shromažďováno s diagnostikou virtuálního počítače?</span><span class="sxs-lookup"><span data-stu-id="0f719-150">How can I modify what is collected with VM diagnostics?</span></span>
<span data-ttu-id="0f719-151">Podrobnosti o tom, jak tooget, upravit a nastavení konfigurace Azure Diagnostics hello, najdete v tématu [tooenable použijte PowerShell Azure Diagnostics ve virtuálním počítači s Windows](../virtual-machines/windows/ps-extensions-diagnostics.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="0f719-151">For details on how tooget, modify, and set hello Azure Diagnostics configuration, see [Use PowerShell tooenable Azure Diagnostics in a virtual machine running Windows](../virtual-machines/windows/ps-extensions-diagnostics.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> 

<span data-ttu-id="0f719-152">Následující ukázka Hello získá hello Azure Diagnostics konfigurace:</span><span class="sxs-lookup"><span data-stu-id="0f719-152">hello following example gets hello Azure Diagnostics configuration:</span></span>

    -AzureRmVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient
    $publicsettings = (Get-AzureRmVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient).PublicSettings
    $encodedconfig = (ConvertFrom-Json -InputObject $publicsettings).xmlCfg
    $xmlconfig = [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($encodedconfig))
    Write-Host $xmlconfig

    $xmlconfig | Out-File -Encoding utf8 -FilePath "d:\WADConfig.xml"

<span data-ttu-id="0f719-153">Následující ukázka Hello upraví konfiguraci Azure Diagnostics hello.</span><span class="sxs-lookup"><span data-stu-id="0f719-153">hello following example modifies hello Azure Diagnostics configuration.</span></span> <span data-ttu-id="0f719-154">V této konfiguraci pouze událost ID 4624 a událost ID 4625 se shromažďují z protokolu událostí zabezpečení hello.</span><span class="sxs-lookup"><span data-stu-id="0f719-154">In this configuration, only event ID 4624 and event ID 4625 are collected from hello security event log.</span></span> <span data-ttu-id="0f719-155">Antimalware od Microsoftu pro Azure události se shromažďují z protokolu událostí systému hello.</span><span class="sxs-lookup"><span data-stu-id="0f719-155">Microsoft Antimalware for Azure events are collected from hello system event log.</span></span> <span data-ttu-id="0f719-156">Podrobnosti o použití hello výrazech XPath v tématu [využívání události](https://msdn.microsoft.com/library/windows/desktop/dd996910(v=vs.85)).</span><span class="sxs-lookup"><span data-stu-id="0f719-156">For details on hello use of XPath expressions, see [Consuming Events](https://msdn.microsoft.com/library/windows/desktop/dd996910(v=vs.85)).</span></span>

    <WindowsEventLog scheduledTransferPeriod="PT1M">
        <DataSource name="Security!*[System[(EventID=4624 or EventID=4625)]]" />
        <DataSource name="System!*[System[Provider[@Name='Microsoft Antimalware']]]"/>
    </WindowsEventLog>

<span data-ttu-id="0f719-157">Hello následující příklad nastaví hello Azure Diagnostics konfiguraci:</span><span class="sxs-lookup"><span data-stu-id="0f719-157">hello following example sets hello Azure Diagnostics configuration:</span></span>

    $diagnosticsconfig_path = "d:\WADConfig.xml"
    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient -DiagnosticsConfigurationPath $diagnosticsconfig_path -StorageAccountName log3121 -StorageAccountKey <storage key>

<span data-ttu-id="0f719-158">Po provedení změn, zkontrolujte že hello správný, že jsou shromažďovány události tooensure účet úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="0f719-158">After you make changes, check hello storage account tooensure that hello correct events are collected.</span></span>

<span data-ttu-id="0f719-159">Pokud máte problémy při hello instalace a konfigurace, otevřete prosím [žádost o podporu](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request).</span><span class="sxs-lookup"><span data-stu-id="0f719-159">If you have any issues during hello installation and configuration, please open a [support request](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request).</span></span> <span data-ttu-id="0f719-160">Vyberte **integrace protokolu** jako hello služby, pro kterou jsou žádosti o podporu.</span><span class="sxs-lookup"><span data-stu-id="0f719-160">Select **Log Integration** as hello service for which you are requesting support.</span></span>

## <a name="can-i-use-azure-log-integration-toointegrate-network-watcher-logs-into-my-siem"></a><span data-ttu-id="0f719-161">Můžete použít protokol integrace se službou Azure toointegrate sledovací proces sítě protokoly do mé SIEM?</span><span class="sxs-lookup"><span data-stu-id="0f719-161">Can I use Azure Log Integration toointegrate Network Watcher logs into my SIEM?</span></span>

<span data-ttu-id="0f719-162">Azure sledovací proces sítě generuje velká množství informací o protokolování.</span><span class="sxs-lookup"><span data-stu-id="0f719-162">Azure Network Watcher generates large quantities of logging information.</span></span> <span data-ttu-id="0f719-163">Tyto protokoly nejsou určeny toobe odeslané tooa SIEM.</span><span class="sxs-lookup"><span data-stu-id="0f719-163">These logs are not meant toobe sent tooa SIEM.</span></span> <span data-ttu-id="0f719-164">cíl Hello podporována pouze pro protokoly sledovací proces sítě je účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="0f719-164">hello only supported destination for Network Watcher logs is a storage account.</span></span> <span data-ttu-id="0f719-165">Integrace se službou Azure protokolu nepodporuje čtení tyto protokoly a přitom k dispozici tooa SIEM.</span><span class="sxs-lookup"><span data-stu-id="0f719-165">Azure Log Integration does not support reading these logs and making them available tooa SIEM.</span></span>

<!--Image references-->
[1]: ./media/security-azure-log-integration-faq/event-xml.png
