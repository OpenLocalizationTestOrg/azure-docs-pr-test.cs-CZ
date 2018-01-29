---
title: "Správa agenta Azure Log Analytics | Microsoft Docs"
description: "Tento článek popisuje různé správu úlohy, které obvykle provedete během životního cyklu z monitorování agenta MMA (Microsoft) nasazený na počítači."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2018
ms.author: magoedte
ms.openlocfilehash: a17418142fb5f52a93d7a56cb2e6e6e97a250002
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/23/2018
---
# <a name="managing-and-maintaining-the-log-analytics-agent-for-windows-and-linux"></a>Správu a údržbu agenta analýzy protokolů pro systém Windows a Linux

Po počátečním nasazení agenta systému Windows nebo Linux pro analýzy protokolů můžete změnit konfiguraci agenta v závislosti na situaci, nebo ji odeberte z počítače, pokud bylo dosaženo do vyřazení fáze životního cyklu.  Tyto úlohy běžné údržby můžete snadno spravovat ručně nebo pomocí automatizace, což snižuje provozní chyba i výdaje.

## <a name="adding-or-removing-a-workspace"></a>Přidání nebo odebrání pracovního prostoru 

### <a name="windows-agent"></a>Agent služby Windows

#### <a name="update-settings-from-control-panel"></a>Aktualizovat nastavení v Ovládacích panelech

1. Přihlásit se k počítači pomocí účtu, který má práva správce.
2. Otevřete **Ovládací panely**.
3. Vyberte **agenta Microsoft Monitoring Agent** a klikněte **Azure Log Analytics (OMS)** kartě.
4. Pokud odebrání pracovního prostoru, vyberte ho a pak klikněte na tlačítko **odebrat**. Tento krok opakujte pro všechny ostatní prostoru má agent přestane být.
5. Pokud přidáváte pracovního prostoru, klikněte na tlačítko **přidat** a na **přidat pracovní prostor Log Analytics** dialogové okno, vložte ID pracovního prostoru a klíč pracovního prostoru (primární klíč). Pokud počítač by měl nahlásit pracovní prostor analýzy protokolů v cloudu Azure Government, vyberte z rozevíracího seznamu Azure Cloud Azure US Government. 
6. Klikněte na tlačítko **OK** a uložte změny.

#### <a name="remove-a-workspace-using-powershell"></a>Odebrání pracovního prostoru pomocí prostředí PowerShell 

```PowerShell
$workspaceId = "<Your workspace Id>"
$mma = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'
$mma.RemoveCloudWorkspace($workspaceId)
$mma.ReloadConfiguration()
```

#### <a name="add-a-workspace-in-azure-commercial-using-powershell"></a>Přidat pracovní prostor v Azure komerční pomocí prostředí PowerShell 

```PowerShell
$workspaceId = "<Your workspace Id>"
$workspaceKey = "<Your workspace Key>”
$mma = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'
$mma.AddCloudWorkspace($workspaceId, $workspaceKey)
$mma.ReloadConfiguration()
```

#### <a name="add-a-workspace-in-azure-for-us-government-using-powershell"></a>Přidat pracovní prostor v Azure pro US Government pomocí prostředí PowerShell 

```PowerShell
$workspaceId = "<Your workspace Id>"
$workspaceKey = "<Your workspace Key>”
$mma = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'
$mma.AddCloudWorkspace($workspaceId, $workspaceKey, 1)
$mma.ReloadConfiguration()
```

>[!NOTE]
>Pokud jste použili příkazového řádku nebo skriptu dříve pro instalaci nebo konfiguraci agenta, `EnableAzureOperationalInsights` byl nahrazen `AddCloudWorkspace` a `RemoveCloudWorkspace`.
>

## <a name="update-proxy-settings"></a>Aktualizovat nastavení proxy serveru 
Konfigurace agenta pro komunikaci se service pomocí proxy serveru nebo [OMS brány](log-analytics-oms-gateway.md) po nasazení, použijte jednu z následujících metod pro dokončení této úlohy.

### <a name="windows-agent"></a>Agent služby Windows

#### <a name="update-settings-using-control-panel"></a>Aktualizovat nastavení ovládacího panelu

1. Přihlásit se k počítači pomocí účtu, který má práva správce.
2. Otevřete **Ovládací panely**.
3. Vyberte **agenta Microsoft Monitoring Agent** a klikněte **nastavení proxy serveru** kartě.
4. Klikněte na tlačítko **použít proxy server** a zadejte adresu URL a port počet proxy server nebo brána. Pokud proxy server nebo brány OMS vyžaduje ověřování, zadejte uživatelské jméno a heslo k ověření a pak klikněte na tlačítko **OK**. 

#### <a name="update-settings-using-powershell"></a>Aktualizovat nastavení pomocí prostředí PowerShell 

Zkopírujte následující vzorový kód prostředí PowerShell, aktualizovat informace specifické pro vaše prostředí a uložit s příponou PS1.  Spusťte skript na každém počítači, který se připojuje přímo ke službě Analýza protokolů.

```PowerShell
param($ProxyDomainName="https://proxy.contoso.com:30443", $cred=(Get-Credential))

# First we get the Health Service configuration object.  We need to determine if we
#have the right update rollup with the API we need.  If not, no need to run the rest of the script.
$healthServiceSettings = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'

$proxyMethod = $healthServiceSettings | Get-Member -Name 'SetProxyInfo'

if (!$proxyMethod)
{
     Write-Output 'Health Service proxy API not present, will not update settings.'
     return
}

Write-Output "Clearing proxy settings."
$healthServiceSettings.SetProxyInfo('', '', '')

$ProxyUserName = $cred.username

Write-Output "Setting proxy to $ProxyDomainName with proxy username $ProxyUserName."
$healthServiceSettings.SetProxyInfo($ProxyDomainName, $ProxyUserName, $cred.GetNetworkCredential().password)
```  

### <a name="linux-agent"></a>Agenta systému Linux
Pokud vaše počítače Linux je nutné komunikují prostřednictvím proxy serveru nebo brány OMS k analýze protokolů, proveďte následující kroky.  Hodnota konfigurace proxy má následující syntaxi: `[protocol://][user:password@]proxyhost[:port]`.  *Proxyhost* vlastnost přijímá parametry s plně kvalifikovaný název domény nebo IP adresu proxy serveru.

1. Spuštěním následujících příkazů upravte soubor `/etc/opt/microsoft/omsagent/proxy.conf` a změňte hodnoty na vaše konkrétní nastavení.

    ```
    proxyconf="https://proxyuser:proxypassword@proxyserver01:30443"
    sudo echo $proxyconf >>/etc/opt/microsoft/omsagent/proxy.conf
    sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/proxy.conf 
    ```

2. Restartujte agenta spuštěním následujícího příkazu: 

    ```
    sudo /opt/microsoft/omsagent/bin/service_control restart [<workspace id>]
    ``` 

## <a name="uninstall-agent"></a>Odinstalace agenta
Použijte jeden z následujících postupů odinstalujte agenta nástroje systému Windows nebo Linux pomocí Průvodce příkazového řádku nebo instalační program.

### <a name="windows-agent"></a>Agent služby Windows

#### <a name="uninstall-from-control-panel"></a>Odinstalovat z ovládacích panelů
1. Přihlásit se k počítači pomocí účtu, který má práva správce.  
2. V **ovládací panely**, klikněte na tlačítko **programy a funkce**.
3. V **programy a funkce**, klikněte na tlačítko **agenta Microsoft Monitoring Agent**, klikněte na tlačítko **odinstalace**a potom klikněte na **Ano**.

>[!NOTE]
>Průvodce instalací agenta lze spustit také dvojitým kliknutím na soubor **MMASetup -<platform>.exe**, která je k dispozici ke stažení z pracovního prostoru v portálu Azure.

#### <a name="uninstall-from-the-command-line"></a>Odinstalovat z příkazového řádku
Stažený soubor pro agenta je samostatný instalační balíček vytvořen s IExpress.  Instalační program pro agenta a podpůrné soubory jsou obsažené v balíčku a potřeba extrahovat aby bylo možné správně, odinstalujte ji pomocí příkazového řádku vidět v následujícím příkladu. 

1. Přihlásit se k počítači pomocí účtu, který má práva správce.  
2. K extrakci instalačních souborů agenta z příkazového řádku se zvýšenými spustit `extract MMASetup-<platform>.exe` a zobrazí výzvu pro cestu k extrakci souborů.  Alternativně můžete zadat cestu předání argumentů `extract MMASetup-<platform>.exe /c:<Path> /t:<Path>`.  Další informace o příkazového řádku swtiches nepodporuje IExpress najdete v tématu [přepínače příkazového řádku pro IExpress](https://support.microsoft.com/help/197147/command-line-switches-for-iexpress-software-update-packages) a aktualizujte příklad podle svých potřeb.
3. Na příkazovém řádku zadejte `%WinDir%\System32\msiexec.exe /x <Path>:\MOMAgent.msi /qb`.  

### <a name="linux-agent"></a>Agenta systému Linux
Pokud chcete odebrat agenta, proveďte následující kroky.

1. Stáhněte na počítač [univerzální skript](https://github.com/Microsoft/OMS-Agent-for-Linux/releases) agenta pro Linux.
2. Spusťte na počítači soubor .sh sady s argumentem *--purge*. Tím se kompletně odebere agent a jeho konfigurace.

    `sudo sh ./omsagent-<version>.universal.x64.sh --purge`

## <a name="configure-agent-to-report-to-an-operations-manager-management-group"></a>Konfiguraci agenta tak, aby odesílaly do skupiny správy nástroje Operations Manager

### <a name="windows-agent"></a>Agent služby Windows
Proveďte následující postup pro konfiguraci OMS agenta pro Windows tak, aby odesílaly skupiny pro správu System Center Operations Manager. 

1. Přihlásit se k počítači pomocí účtu, který má práva správce.
2. Otevřete **Ovládací panely**. 
3. Klikněte na tlačítko **agenta Microsoft Monitoring Agent** a pak klikněte na tlačítko **nástroje Operations Manager** kartě.
4. Pokud vaše servery nástroje Operations Manager integrace se službou Active Directory, klikněte na tlačítko **automaticky aktualizovat přiřazení skupin pro správu ze služby AD DS**.
5. Klikněte na tlačítko **přidat** otevřete **přidat skupinu pro správu** dialogové okno.
6. V **název skupiny pro správu** pole, zadejte název skupiny pro správu.
7. V **primární server pro správu** pole, zadejte název počítače primární server pro správu.
8. V **port serveru pro správu** pole, zadejte číslo portu TCP.
9. V části **účet akce agenta**, zvolte účet místní systém nebo účet místní domény.
10. Klikněte na tlačítko **OK** zavřete **přidat skupinu pro správu** dialogové okno a pak klikněte na tlačítko **OK** zavřete **vlastnosti agenta monitorování Microsoft** Dialogové okno.

### <a name="linux-agent"></a>Agenta systému Linux
Proveďte následující kroky konfigurace agenta OMS pro Linux informuje o skupině pro správu System Center Operations Manager. 

1. Upravte soubor`/etc/opt/omi/conf/omiserver.conf`
2. Ujistěte se, že na začátek řádku s `httpsport=` definuje port 1270. Například:`httpsport=1270`
3. Restartujte OMI server:`sudo /opt/omi/bin/service_control restart`

## <a name="next-steps"></a>Další postup

Zkontrolujte [řešení potíží s agenta systému Linux](log-analytics-agent-linux-support.md) když dojde k potížím při instalaci nebo správě agenta.  