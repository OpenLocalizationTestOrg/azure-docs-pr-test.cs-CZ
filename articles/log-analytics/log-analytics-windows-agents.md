---
title: "aaaConnect Windows počítače tooAzure Log Analytics | Microsoft Docs"
description: "Tento článek ukazuje počítače se systémem Windows hello hello kroky tooconnect ve vaší místní infrastruktury toohello analýzy protokolů služby pomocí přizpůsobená verze hello Microsoft Monitoring Agent (MMA)."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 932f7b8c-485c-40c1-98e3-7d4c560876d2
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/03/2017
ms.author: magoedte
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7e15f9eeb0440bd2f6557d7215df701526e4f9aa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-windows-computers-toohello-log-analytics-service-in-azure"></a>Připojení Windows počítače toohello analýzy protokolů služby v Azure

Tento článek popisuje kroky hello počítače se systémem Windows tooconnect pracovních prostorů tooOMS místní infrastruktury, pomocí přizpůsobená verze hello Microsoft Monitoring Agent (MMA). Potřebujete tooinstall a agentů pro všechny počítače hello má tooonboard, aby služba toosend data toohello analýzy protokolů a tooview a postupovat podle dat připojit. Každý agent může hlásit toomultiple pracovních prostorů.

Můžete nainstalovat agenty pomocí instalačního programu, příkazového řádku nebo pomocí požadovaného stavu konfigurace (DSC) ve službě Azure Automation.  

>[!NOTE]
Pro virtuální počítače běžící v Azure, můžete zjednodušit instalace pomocí hello [rozšíření virtuálního počítače](log-analytics-azure-vm-extension.md).

Na počítačích s připojením k Internetu hello agent používá hello připojení toohello Internet toosend data tooOMS. Pro počítače, které nemají připojení k Internetu, můžete použít server proxy nebo hello [OMS brány](log-analytics-oms-gateway.md).

Připojení počítače tooOMS vašeho systému Windows je jednoduchý pomocí tří jednoduché kroky:

1. Stáhněte instalační soubor agenta hello z portálu OMS hello
2. Instalace agenta hello pomocí zvolené metodě hello
3. Konfigurace agenta hello nebo přidejte další pracovní prostory, v případě potřeby

Hello následující diagram znázorňuje hello vztah mezi počítače se systémem Windows a OMS po nainstalovat a nakonfigurovat agenty.

![OMS-direct agenta – diagram](./media/log-analytics-windows-agents/oms-direct-agent-diagram.png)

Pokud vaše zásady zabezpečení IT neumožňují počítače ve vaší síti tooconnect toohello Internetu, můžete nakonfigurovat vaše počítače tooconnect toohello OMS brány. Další informace a na tom, jak tooconfigure toocommunicate vaše servery prostřednictvím brány OMS toohello OMS služby, najdete v části kroky [připojit počítače tooOMS pomocí hello OMS brány](log-analytics-oms-gateway.md).

## <a name="system-requirements-and-required-configuration"></a>Požadavky na systém a požadované konfiguraci
Před instalací nebo nasadit agenty, zkontrolujte následující podrobnosti tooensure hello požadavky splňujete hello.

- Hello OMS MMA můžete nainstalovat pouze na počítačích se systémem Windows Server 2008 s aktualizací SP 1 nebo novější nebo Windows 7 SP1 nebo novější.
- Budete potřebovat předplatné Azure.  Další informace najdete v tématu [začít pracovat s analýzy protokolů](log-analytics-get-started.md).
- Každý počítač se systémem Windows musí být schopný tooconnect toohello Internetu pomocí protokolu HTTPS nebo toohello OMS brány. Toto připojení může být direct, prostřednictvím proxy serveru, nebo prostřednictvím hello OMS brány.
- Hello OMS MMA můžete nainstalovat na samostatných počítačů, serverů a virtuálních počítačů. Pokud chcete tooOMS tooconnect virtuálních počítačů hostovaných v Azure, najdete v části [tooLog virtuální počítače Azure připojit Analytics](log-analytics-azure-vm-extension.md).
- Hello agent potřebuje toouse port 443 protokolu TCP pro různé prostředky.

### <a name="network"></a>Síť

Pro Windows agenty tooconnect tooand registrace službou hello OMS musí mít přístup k prostředkům toonetwork, včetně hello čísla portů a adres URL domény.

- Pro proxy servery budete potřebovat tooensure, který hello odpovídající proxy serveru, které prostředky jsou nakonfigurované v nastavení agenta.
- Pro brány firewall, které omezují přístup toohello Internet, vám nebo vaší sítě technici potřebovat tooconfigure tooOMS přístup toopermit vaší brány firewall. V nastavení agenta nemusíte nic konfigurovat.

Hello následující tabulka ukazuje prostředky potřebné pro komunikaci.

>[!NOTE]
>Některé z následujících prostředků hello zmínili Operational Insights, která byla předchozí název pro analýzu protokolu.

| Prostředek agenta | Porty | Obejít kontrolu protokolu HTTPS |
|---|---|---|
| *.ods.opinsights.azure.com | 443 | Ano |
| *.oms.opinsights.azure.com | 443 | Ano |
| *.blob.core.windows.net | 443 | Ano |
| *.azure-automation.net | 443 | Ano |



## <a name="download-hello-agent-setup-file-from-oms"></a>Stáhněte instalační soubor agenta hello od OMS
1. Na portálu OMS hello, na hello **přehled** klikněte na tlačítko hello **nastavení** dlaždici.  Klikněte na tlačítko hello **připojené zdroje** karty v horní části hello.  
    ![Karta připojené zdroje](./media/log-analytics-windows-agents/oms-direct-agent-connected-sources.png)
2. Klikněte na tlačítko **servery Windows** a pak klikněte na **stáhnout agenta Windows** použít tooyour počítač procesor typ toodownload hello instalační soubor.
3. Na hello napravo od **ID pracovního prostoru**, klikněte na ikonu hello kopírování a vložení hello ID do poznámkového bloku.
4. Na hello napravo od **primární klíč**, klikněte na ikonu hello kopírování a vložení hello klíč do poznámkového bloku.     

## <a name="install-hello-agent-using-setup"></a>Nainstalujte agenta hello pomocí instalačního programu
1. Spusťte instalační program tooinstall hello agenta na počítači, které chcete toomanage.
2. Na úvodní stránku hello, klikněte na tlačítko **Další**.
3. Na stránce hello licenční podmínky přečíst hello licencí a pak klikněte na **souhlasím**.
4. Na stránce hello cílovou složku, změnit nebo zachovat hello výchozí instalační složku a pak klikněte na tlačítko **Další**.
5. Na stránce Možnosti instalace agenta hello můžete zvolit tooconnect hello agenta tooAzure Log Analytics (OMS), Operations Manager, nebo můžete nechat hello volby prázdné budete chtít tooconfigure hello později. Klikněte na **Další**.   
    - Pokud jste zvolili tooconnect tooAzure analýzy protokolů (OMS), vložte hello **ID pracovního prostoru** a **klíč pracovního prostoru (primární klíč)** zkopírovali v předchozím postupu hello do poznámkového bloku a pak klikněte na  **Další**.  
        ![Vložte ID pracovního prostoru a primární klíč](./media/log-analytics-windows-agents/connect-workspace.png)
    - Pokud jste zvolili tooconnect tooOperations správce, zadejte hello **název skupiny pro správu**, **serveru pro správu** název, a **Port serveru pro správu**a potom klikněte na **Další**. Na stránce hello účet akce agenta, zvolte hello místní systémový účet nebo účet místní domény a pak klikněte na tlačítko **Další**.  
        ![Konfigurace skupiny pro správu](./media/log-analytics-windows-agents/oms-mma-om-setup01.png)![účet akce agenta](./media/log-analytics-windows-agents/oms-mma-om-setup02.png)

6. Na stránce Připraveno tooInstall hello zkontrolujte vybrané možnosti a pak klikněte na tlačítko **nainstalovat**.
7. Na hello konfigurace byla úspěšně dokončena stránky, klikněte na tlačítko **Dokončit**.
8. Po dokončení hello **agenta Microsoft Monitoring Agent** se zobrazí v **ovládací panely**. Můžete zkontrolovat konfiguraci existuje a ujistěte se, že tento agent hello připojené tooOperational přehledy (OMS). Když připojené tooOMS hello agenta zobrazí zpráva s oznámením: **hello agenta Microsoft Monitoring Agent byl úspěšně připojen toohello služby Microsoft Operations Management Suite.**

## <a name="configure-proxy-settings"></a>Konfigurace nastavení proxy serveru

Můžete použít následující postup tooconfigure nastavení proxy serveru pro hello Microsoft Monitoring Agent pomocí ovládacích panelů hello. Toouse musíte pro každý server, tento postup. Pokud máte mnoho serverů, je nutné, aby tooconfigure, může pro vás snadnější toouse skriptu tooautomate tohoto procesu. Pokud ano, najdete v dalším postupu hello [tooconfigure nastavení proxy serveru pro hello Microsoft Monitoring Agent pomocí skriptu](#to-configure-proxy-settings-for-the-microsoft-monitoring-agent-using-a-script).

### <a name="tooconfigure-proxy-settings-for-hello-microsoft-monitoring-agent-using-control-panel"></a>nastavení proxy serveru tooconfigure hello Microsoft Monitoring Agent pomocí ovládacích panelů
1. Otevřete **Ovládací panely**.
2. Otevřete **Microsoft Monitoring Agent**.
3. Klikněte na tlačítko hello **nastavení proxy serveru** kartě.  
    ![karta nastavení proxy serveru](./media/log-analytics-windows-agents/proxy-direct-agent-proxy.png)
4. Vyberte **použít proxy server** a zadejte adresu URL hello a čísla portu, pokud je potřebné, podobně jako toohello příkladu. Pokud proxy server vyžaduje ověřování, zadejte hello uživatelské jméno a heslo tooaccess hello proxy serveru.


### <a name="verify-agent-connectivity-toooms"></a>Ověřte připojení tooOMS agenta

Snadno můžete ověřit, zda jsou agenty komunikaci s OMS pomocí hello následující postup:

1.  Otevřete ovládací panely v počítači hello s agentem Windows hello.
2.  Otevřete agenta Microsoft Monitoring Agent.
3.  Klikněte na kartu hello Azure Log Analytics (OMS).
4.  Ve sloupci Stav hello měli byste vidět, že tento agent hello připojení úspěšně toohello služby Operations Management Suite.

![připojení agenta](./media/log-analytics-windows-agents/mma-connected.png)


### <a name="tooconfigure-proxy-settings-for-hello-microsoft-monitoring-agent-using-a-script"></a>nastavení proxy serveru tooconfigure hello Microsoft Monitoring Agent pomocí skriptu
Zkopírujte hello následující ukázkové, jej aktualizovat s informace o konkrétní tooyour prostředí, uložit s příponou PS1 a spusťte skript hello na každém počítači, který se připojuje přímo toohello OMS služby.

    param($ProxyDomainName="http://proxy.contoso.com:80", $cred=(Get-Credential))

    # First we get hello Health Service configuration object.  We need toodetermine if we
    #have hello right update rollup with hello API we need.  If not, no need toorun hello rest of hello script.
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

    Write-Output "Setting proxy too$ProxyDomainName with proxy username $ProxyUserName."
    $healthServiceSettings.SetProxyInfo($ProxyDomainName, $ProxyUserName, $cred.GetNetworkCredential().password)



## <a name="install-hello-agent-using-hello-command-line"></a>Nainstalujte agenta hello hello příkazového řádku
- Upravit a pak použijte následující příklad tooinstall hello agenta pomocí příkazového řádku hello hello. Příklad Hello provede plně bezobslužnou instalaci.

    >[!NOTE]
    Pokud chcete, aby tooupgrade agenta, je třeba toouse hello analýzy protokolů skriptování rozhraní API. Najdete v další části tooupgrade hello agenta.

    ```dos
    MMASetup-AMD64.exe /Q:A /R:N /C:"setup.exe /qn ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_ID=<your workspace id> OPINSIGHTS_WORKSPACE_KEY=<your workspace key> AcceptEndUserLicenseAgreement=1"
    ```

Hello agent používá IExpress jako jeho Self-Extractor pomocí hello `/c` příkaz. Můžete zobrazit hello přepínače příkazového řádku na [přepínače příkazového řádku pro IExpress](https://support.microsoft.com/help/197147/command-line-switches-for-iexpress-software-update-packages) a pak aktualizaci hello příklad toosuit vašim potřebám.

|MMA specifické možnosti                   |Poznámky         |
|---------------------------------------|--------------|
|ADD_OPINSIGHTS_WORKSPACE               | 1 = konfigurovat hello agenta tooreport tooa prostoru                |
|OPINSIGHTS_WORKSPACE_ID                | Id pracovního prostoru (guid) pro tooadd prostoru hello                    |
|OPINSIGHTS_WORKSPACE_KEY               | Klíče používané tooinitially prostoru ověřování pomocí pracovního prostoru hello |
|OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE  | Zadejte hello cloudového prostředí, kde se nachází hello prostoru <br> 0 = komerční cloudu azure (výchozí) <br> 1 = azure Government |
|OPINSIGHTS_PROXY_URL               | Identifikátor URI pro hello proxy toouse |
|OPINSIGHTS_PROXY_USERNAME               | Uživatelské jméno tooaccess ověřený server proxy |
|OPINSIGHTS_PROXY_PASSWORD               | Heslo tooaccess ověřený server proxy |

>[!NOTE]
tooavoid stiskne hello příkazového řádku maximální délku IExpress, nainstalujte agenta hello se žádný pracovní prostor nakonfigurované a pak používat konfigurace tooset skript pro hello prostoru.

>[!NOTE]
Pokud dojde `Command line option syntax error.` při použití hello `OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE` parametr, můžete použít hello následující alternativní řešení:
```dos
MMASetup-AMD64.exe /C /T:.\MMAExtract
cd .\MMAExtract
setup.exe /qn ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_AZURE_CLOUD_TYPE=1 OPINSIGHTS_WORKSPACE_ID=<your workspace id> OPINSIGHTS_WORKSPACE_KEY=<your workspace key> AcceptEndUserLicenseAgreement=1
```

## <a name="add-a-workspace-using-a-script"></a>Přidat pracovní prostor pomocí skriptu
Přidáte pracovní prostor pomocí skriptování API agenta hello analýzy protokolů hello následující ukázka:

```PowerShell
$mma = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'
$mma.AddCloudWorkspace($workspaceId, $workspaceKey)
$mma.ReloadConfiguration()
```

tooadd pracovního prostoru v Azure US Government, použijte hello následující ukázka skriptu:
```PowerShell
$mma = New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg'
$mma.AddCloudWorkspace($workspaceId, $workspaceKey, 1)
$mma.ReloadConfiguration()
```

>[!NOTE]
Pokud jste použili hello příkazového řádku nebo skriptu dříve tooinstall nebo konfiguraci agenta hello `EnableAzureOperationalInsights` byl nahrazen `AddCloudWorkspace`.

## <a name="install-hello-agent-using-dsc-in-azure-automation"></a>Instalace agenta hello pomocí DSC v Azure Automation.

Můžete použít následující skript příklad tooinstall hello agenta pomocí ve službě Azure Automation DSC hello. Příklad Hello nainstaluje hello 64bitový agent, identifikovaný hello `URI` hodnotu. Můžete taky hello 32bitová verze nahrazením hello hodnota identifikátoru URI. Hello identifikátory URI pro obě verze jsou:

- 64bitový agent Windows - https://go.microsoft.com/fwlink/?LinkId=828603
- 32bitový agent Windows - https://go.microsoft.com/fwlink/?LinkId=828604


>[!NOTE]
Tento postup a skriptu příklad neprovede upgrade existujícího agenta.

1. Import hello xPSDesiredStateConfiguration DSC modul z [http://www.powershellgallery.com/packages/xPSDesiredStateConfiguration](http://www.powershellgallery.com/packages/xPSDesiredStateConfiguration) do Azure Automation.  
2.  Vytvoření proměnných assetů Azure Automation pro *OPSINSIGHTS_WS_ID* a *OPSINSIGHTS_WS_KEY*. Nastavit *OPSINSIGHTS_WS_ID* ID pracovního prostoru analýzy protokolů OMS tooyour a nastavte *OPSINSIGHTS_WS_KEY* toohello primární klíč pracovního prostoru.
3.  Použít následující skript a uložte ho jako MMAgent.ps1 hello
4.  Upravit a pak pomocí hello následující příklad tooinstall hello agenta pomocí DSC ve službě Azure Automation. Importujte MMAgent.ps1 do Azure Automation pomocí rozhraní Azure Automation hello nebo rutinu.
5.  Přiřaďte toohello konfigurace uzlu. Během 15 minut uzel hello zkontroluje konfiguraci a hello MMA se posune toohello uzlu.

```PowerShell
Configuration MMAgent
{
    $OIPackageLocalPath = "C:\MMASetup-AMD64.exe"
    $OPSINSIGHTS_WS_ID = Get-AutomationVariable -Name "OPSINSIGHTS_WS_ID"
    $OPSINSIGHTS_WS_KEY = Get-AutomationVariable -Name "OPSINSIGHTS_WS_KEY"


    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    Node OMSnode {
        Service OIService
        {
            Name = "HealthService"
            State = "Running"
            DependsOn = "[Package]OI"
        }

        xRemoteFile OIPackage {
            Uri = "https://go.microsoft.com/fwlink/?LinkId=828603"
            DestinationPath = $OIPackageLocalPath
        }

        Package OI {
            Ensure = "Present"
            Path  = $OIPackageLocalPath
            Name = "Microsoft Monitoring Agent"
            ProductId = "8A7F2C51-4C7D-4BFD-9014-91D11F24AAE2"
            Arguments = '/C:"setup.exe /qn ADD_OPINSIGHTS_WORKSPACE=1 OPINSIGHTS_WORKSPACE_ID=' + $OPSINSIGHTS_WS_ID + ' OPINSIGHTS_WORKSPACE_KEY=' + $OPSINSIGHTS_WS_KEY + ' AcceptEndUserLicenseAgreement=1"'
            DependsOn = "[xRemoteFile]OIPackage"
        }
    }
}


```

### <a name="get-hello-latest-productid-value"></a>Získat nejnovější hodnotu ProductId hello

Hello `ProductId value` v hello MMAgent.ps1 skript je jedinečný tooeach verze agenta. Při publikování aktualizovaná verze všech agentů, změní se hello ProductId hodnotu. Ano při hello ProductId změn v hello budoucí, můžete nalézt hello verze agenta pomocí jednoduchého skriptu. Až budete mít nejnovější verze agenta hello nainstalovaná na testovací server, můžete použít následující skript tooget hello nainstalován ProductId hodnota hello. Pomocí hodnoty nejnovější ProductId hello, můžete aktualizovat hodnotu hello v hello MMAgent.ps1 skriptu.

```PowerShell
$InstalledApplications  = Get-ChildItem hklm:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall


foreach ($Application in $InstalledApplications)

{

     $Key = Get-ItemProperty $Application.PSPath

     if ($Key.DisplayName -eq "Microsoft Monitoring Agent")

     {

        $Key.DisplayName

        Write-Output ("Product ID is: " + $Key.PSChildName.Substring(1,$Key.PSChildName.Length -2))

     }

}  
```

## <a name="configure-an-agent-manually-or-add-additional-workspaces"></a>Nakonfigurujte agenta ručně, nebo přidejte další pracovní prostory
Pokud jste nainstalovali agenty, ale nenakonfigurovali nebo pokud chcete hello agenta tooreport toomultiple pracovní prostory, můžete použít následující informace tooenable agenta hello nebo překonfigurování. Po dokončení konfigurace agenta hello, bude zaregistrujte službou hello agenta a získají potřebné informace o konfiguraci a sady management Pack, které obsahují informace o řešení.

1. Po instalaci agenta Microsoft Monitoring Agent hello, otevřete **ovládací panely**.
2. Otevřete **agenta Microsoft Monitoring Agent** a pak klikněte na tlačítko hello **Azure Log Analytics (OMS)** kartě.   
3. Klikněte na tlačítko **přidat** tooopen hello **přidat pracovní prostor Log Analytics** pole.
4. Vložení hello **ID pracovního prostoru** a **klíč pracovního prostoru (primární klíč)** zkopírovali do poznámkového bloku v předchozím postupu pro hello prostoru má tooadd a pak klikněte na tlačítko **OK**.  
    ![Konfigurace služby Operational Insights](./media/log-analytics-windows-agents/add-workspace.png)

Po shromáždění dat z počítačů monitorovaných agentem hello hello počet počítačů monitorovaných pomocí OMS objeví na portálu OMS hello na hello **připojené zdroje** kartě v **nastavení** jako  **Připojené servery**.


## <a name="toodisable-an-agent"></a>toodisable agenta
1. Po instalaci agenta hello, otevřete **ovládací panely**.
2. Otevřete Microsoft Monitoring Agent a pak klikněte na tlačítko hello **Azure Log Analytics (OMS)** kartě.
3. Vyberte pracovní prostor a pak klikněte na tlačítko **odebrat**. Tento krok opakujte pro všechny jiné pracovní prostory.


## <a name="optionally-configure-agents-tooreport-tooan-operations-manager-management-group"></a>Volitelně můžete nakonfigurujte skupinu správy agentů tooreport tooan nástroje Operations Manager

Pokud používáte nástroje Operations Manager v infrastruktuře IT, můžete také použít agenta MMA hello jako agenta nástroje Operations Manager.

### <a name="tooconfigure-mma-agents-tooreport-tooan-operations-manager-management-group"></a>skupinu správy nástroje Operations Manager tooreport tooconfigure MMA agenty tooan
1.  Otevřete v počítači hello kde je nainstalován hello agent **ovládací panely**.  
2.  Otevřete **agenta Microsoft Monitoring Agent** a pak klikněte na tlačítko hello **nástroje Operations Manager** kartě.  
    ![Karta Microsoft Monitoring Agent Operations Manager](./media/log-analytics-windows-agents/om-mg01.png)
3.  Pokud vaše servery nástroje Operations Manager integrace se službou Active Directory, klikněte na tlačítko **automaticky aktualizovat přiřazení skupin pro správu ze služby AD DS**.
4.  Klikněte na tlačítko **přidat** tooopen hello **přidat skupinu pro správu** dialogové okno.  
    ![Přidat skupinu pro správu agenta Microsoft Monitoring Agent](./media/log-analytics-windows-agents/oms-mma-om02.png)
5.  V **název skupiny pro správu** pole, hello název typu skupiny pro správu.
6.  V hello **primární server pro správu** pole, zadejte název počítače hello hello primární server pro správu.
7.  V hello **port serveru pro správu** pole, číslo portu TCP hello typu.
8.  V části **účet akce agenta**, zvolte hello místní systémový účet nebo účet místní domény.
9.  Klikněte na tlačítko **OK** tooclose hello **přidat skupinu pro správu** dialogové okno a pak klikněte na tlačítko **OK** tooclose hello **Microsoft Monitoring Agent vlastnosti**dialogové okno.


## <a name="next-steps"></a>Další kroky

- [Přidat řešení pro analýzu protokolu z hello řešení Galerie](log-analytics-add-solutions.md) tooadd funkce a shromáždění dat.
