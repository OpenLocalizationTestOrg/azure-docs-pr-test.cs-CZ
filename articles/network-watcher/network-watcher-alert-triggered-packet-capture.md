---
title: "monitorování pomocí výstrah a Azure Functions sítě aaaUse paketu zachycení toodo proaktivní | Microsoft Docs"
description: "Tento článek popisuje, jak toocreate výstraha aktivuje zachytáváním paketů s sledovací proces sítě Azure"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 75e6e7c4-b3ba-4173-8815-b00d7d824e11
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 4722a831f3a9d5537c0e6f53daba4dfc35d0cf24
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-packet-capture-for-proactive-network-monitoring-with-alerts-and-azure-functions"></a>Použít zachytáváním paketů pro monitorování proaktivní sítě pomocí výstrah a Azure Functions

Zachycení dat ze sítě sledovacích procesů paketu vytvoří zaznamenání relace tootrack provozu do a z virtuálních počítačů. Hello zachycení soubor může mít filtr, který je definován tootrack pouze hello přenosy, které chcete toomonitor. Tato data jsou pak uloženy v storage blob nebo místně na počítači hosta hello.

Tato funkce můžete spustit vzdáleně z jiných automatizace scénáře, jako je Azure Functions. Poskytuje zachytávání paketů hello schopností toorun proaktivní zachycení na základě definované sítě anomálií. Mezi další použití patří shromažďování statistiku sítě, získání informací o síti vniknutí, ladění komunikaci klienta se serverem a další.

Prostředky, které jsou nasazené v Azure spustit 24 hodin denně 7. Vás a vašich zaměstnanců nelze monitorovat aktivně hello stavu všech prostředků, 24 nebo 7. Co se například stane, pokud se vyskytne problém na 2: 00?

Pomocí sledovací proces sítě, výstrahy a funkce z v rámci hello Azure ekosystém můžete proaktivně odpoví hello dat a nástroje toosolve problémy ve vaší síti.

![Scénář][scenario]

## <a name="prerequisites"></a>Požadavky

* nejnovější verze Hello [prostředí Azure PowerShell](/powershell/azure/install-azurerm-ps).
* Existující instanci sledovací proces sítě. Pokud jste ještě nemáte, [vytvořit instanci sledovací proces sítě](network-watcher-create.md).
* Existující virtuální počítač v hello stejné oblasti jako sledovací proces sítě s hello [rozšíření Windows](../virtual-machines/windows/extensions-nwa.md) nebo [rozšíření virtuálního počítače Linux](../virtual-machines/linux/extensions-nwa.md).

## <a name="scenario"></a>Scénář

V tomto příkladu váš virtuální počítač odesílá víc segmentů TCP než obvykle a chcete toobe výstrahy. Segmentů TCP slouží jako příklad zde, ale žádné výstrahy podmínku můžete použít.

Když se zobrazí výstraha, budete chtít toounderstand data na úrovni paketů tooreceive proč komunikace zvýšilo. Potom můžete provést kroky tooreturn hello virtuálního počítače tooregular komunikace.

Tento scénář předpokládá, že máte existující instanci sledovací proces sítě a skupinu prostředků s platným virtuálním počítačem.

Hello následujícím seznamu je přehled hello pracovního postupu, který probíhá:

1. Výstrahy na vašem virtuálním počítači.
1. Výstraha Hello volání funkce Azure prostřednictvím webhook, jehož.
1. Funkce Azure zpracovává hello výstrahy a spustí relaci zachytávání paketů sledovací proces sítě.
1. zachytáváním paketů Hello běží na hello virtuálních počítačů a shromažďuje provoz.
1. Hello paketu zachycení soubor odešle tooa účtu úložiště ke kontrole a diagnostiku.

tooautomate tento proces jsme vytvořte a připojte se výstrahu na naše tootrigger virtuálních počítačů v případě incidentu hello. Také jsme do sledovací proces sítě vytvořit toocall funkce.

Tento scénář hello následující:

* Vytvoří Azure funkci, která se spouští zachytáváním paketů.
* Vytvoří pravidlo výstrahy na virtuálním počítači a nakonfiguruje hello pravidlo výstrahy toocall hello Azure funkce.

## <a name="create-an-azure-function"></a>Vytvoření Azure funkce

prvním krokem Hello je toocreate výstrahu Azure funkce tooprocess hello a vytvořte zachytáváním paketů.

1. V hello [portál Azure](https://portal.azure.com), vyberte **nový** > **výpočetní** > **aplikaci funkce**.

    ![Vytvoření aplikace – funkce][1-1]

2. Na hello **aplikaci funkce** okno, zadejte následující hodnoty hello a potom vyberte **OK** toocreate hello aplikace:

    |**Nastavení** | **Hodnota** | **Podrobnosti** |
    |---|---|---|
    |**Název aplikace**|PacketCaptureExample|Název Hello hello funkce aplikace.|
    |**Předplatné**|[Předplatné] hello předplatné, pro které toocreate hello funkce aplikace.||
    |**Skupina prostředků**|PacketCaptureRG|Hello prostředků skupiny toocontain hello funkce aplikace.|
    |**Hostování plán**|Plán spotřeba| Typ Hello naplánujte vaše aplikace používá funkce. Možnosti jsou spotřeby nebo plán služby Azure App Service. |
    |**Umístění**|Střed USA| Hello oblast, ve které toocreate hello funkce aplikace.|
    |**Účet úložiště**|{automaticky vygenerovanou}| Hello účtu úložiště, které potřebuje funkce Azure pro úložiště pro obecné účely.|

3. Na hello **PacketCaptureExample funkce aplikace** vyberte **funkce** > **vlastní funkce**  >  **+**.

4. Vyberte **HttpTrigger-Powershell**a pak zadejte zbývající informace hello. Nakonec toocreate hello funkce, vyberte **vytvořit**.

    |**Nastavení** | **Hodnota** | **Podrobnosti** |
    |---|---|---|
    |**Scénář**|experimentální|Typ scénáře|
    |**Pojmenujte svoji funkci**|AlertPacketCapturePowerShell|Název funkce hello|
    |**Úroveň oprávnění**|Funkce|Úroveň oprávnění pro funkci hello|

![Příklad funkce][functions1]

> [!NOTE]
> Šablona Hello prostředí PowerShell je experimentální a nemá plnou podporu.

Úpravy jsou požadovány pro tento příklad a jsou vysvětlené v hello následující kroky.

### <a name="add-modules"></a>Přidání modulů

toouse rutiny prostředí PowerShell sledovací proces sítě, nahrajte hello nejnovější prostředí PowerShell modulu toohello funkce aplikace.

1. Na místním počítači s hello nejnovější nainstalované moduly prostředí Azure PowerShell spusťte následující příkaz prostředí PowerShell hello:

    ```powershell
    (Get-Module AzureRM.Network).Path
    ```

    Tento příklad poskytuje hello místní cesty moduly prostředí Azure PowerShell. Tyto složky se používají v pozdější fázi. Hello moduly, které se používají v tomto scénáři jsou:

    * AzureRM.Network

    * AzureRM.Profile

    * AzureRM.Resources

    ![Složky prostředí PowerShell][functions5]

1. Vyberte **funkce nastavení aplikace** > **přejděte tooApp Editor služby**.

    ![Nastavení aplikace – funkce][functions2]

1. Klikněte pravým tlačítkem na hello **AlertPacketCapturePowershell** složku a pak vytvořte složku s názvem **azuremodules**. 

4. Vytvořte podsložku pro každý modul, který potřebujete.

    ![Složky a podsložky][functions3]

    * AzureRM.Network

    * AzureRM.Profile

    * AzureRM.Resources

1. Klikněte pravým tlačítkem na hello **AzureRM.Network** podsložku a potom vyberte **nahrání souborů**. 

6. Přejděte tooyour Azure moduly. V místní hello **AzureRM.Network** složky, vyberte všechny hello soubory ve složce hello. Potom vyberte **OK**. 

7. Opakujte tyto kroky pro **AzureRM.Profile** a **AzureRM.Resources**.

    ![Nahrání souborů][functions6]

1. Po dokončení, každé složky by měly mít soubory modulu prostředí PowerShell hello z místního počítače.

    ![Soubory prostředí PowerShell][functions7]

### <a name="authentication"></a>Authentication

rutiny prostředí PowerShell hello toouse, je třeba ověřit. Nakonfigurujte ověřování v aplikaci funkce hello. tooconfigure ověřování, je nutné nakonfigurovat proměnné prostředí a nahrát aplikaci funkce toohello šifrovaný soubor klíče.

> [!NOTE]
> Tento scénář obsahuje pouze jeden z příkladů jak tooimplement ověřování s Azure Functions. Existují jiné způsoby toodo to.

#### <a name="encrypted-credentials"></a>Zašifrované přihlašovací údaje

Hello následující skript prostředí PowerShell vytvoří soubor klíče s názvem **PassEncryptKey.key**. Nabízí taky zašifrovaná verze hello hesla, která se dodává. Toto heslo je hello stejné heslo, která je definována pro hello aplikaci Azure Active Directory, která se používá k ověřování.

```powershell
#Variables
$keypath = "C:\temp\PassEncryptKey.key"
$AESKey = New-Object Byte[] 32
$Password = "<insert a password here>"

#Keys
[Security.Cryptography.RNGCryptoServiceProvider]::Create().GetBytes($AESKey) 
Set-Content $keypath $AESKey

#Get encrypted password
$secPw = ConvertTo-SecureString -AsPlainText $Password -Force
$AESKey = Get-content $KeyPath
$Encryptedpassword = $secPw | ConvertFrom-SecureString -Key $AESKey
$Encryptedpassword
```

V hello Editor služby aplikace hello funkce aplikace, vytvořte složku s názvem **klíče** pod **AlertPacketCapturePowerShell**. Nahrajte hello **PassEncryptKey.key** soubor, který jste vytvořili v předchozí ukázce prostředí PowerShell hello.

![Funkce klíč][functions8]

### <a name="retrieve-values-for-environment-variables"></a>Načtení hodnoty pro proměnné prostředí

poslední požadavek Hello je tooset až hello proměnné prostředí, které jsou nezbytné tooaccess hello hodnoty pro ověřování. Hello následující seznam uvádí hello proměnné prostředí, které jsou vytvořené:

* AzureClientID

* AzureTenant

* AzureCredPassword


#### <a name="azureclientid"></a>AzureClientID

ID klienta Hello je hello ID aplikace aplikace v Azure Active Directory.

1. Pokud ještě nemáte toouse aplikace, spusťte následující příklad toocreate hello aplikace.

    ```powershell
    $app = New-AzureRmADApplication -DisplayName "ExampleAutomationAccount_MF" -HomePage "https://exampleapp.com" -IdentifierUris "https://exampleapp1.com/ExampleFunctionsAccount" -Password "<same password as defined earlier>"
    New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId
    Start-Sleep 15
    New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $app.ApplicationId
    ```

   > [!NOTE]
   > Hello heslo, které použijete při vytvoření aplikace hello by měla být hello stejné heslo, které jste předtím vytvořili při ukládání souboru klíče hello.

1. V hello portálu Azure, vyberte **odběry**. Vyberte toouse hello předplatného a pak vyberte **přístup k ovládacímu prvku (IAM)**.

    ![Funkce IAM][functions9]

1. Zvolte účet toouse hello a potom vyberte **vlastnosti**. Zkopírujte hello ID aplikace.

    ![ID aplikace funkce][functions10]

#### <a name="azuretenant"></a>AzureTenant

Získejte ID klienta hello spuštěním hello následující ukázka prostředí PowerShell:

```powershell
(Get-AzureRmSubscription -SubscriptionName "<subscriptionName>").TenantId
```

#### <a name="azurecredpassword"></a>AzureCredPassword

Hello hodnota proměnné prostředí AzureCredPassword hello je hello hodnotu, kterou můžete získat z systémem hello následující ukázka prostředí PowerShell. V tomto příkladu je hello stejný jako ten, které se zobrazí v předchozím hello **šifrovat přihlašovací údaje** části. Hello hodnotu, která je potřeba je hello výstup hello `$Encryptedpassword` proměnné.  Toto je služba hello hlavní se heslo, které jste zašifrovali pomocí skriptu prostředí PowerShell hello.

```powershell
#Variables
$keypath = "C:\temp\PassEncryptKey.key"
$AESKey = New-Object Byte[] 32
$Password = "<insert a password here>"

#Keys
[Security.Cryptography.RNGCryptoServiceProvider]::Create().GetBytes($AESKey) 
Set-Content $keypath $AESKey

#Get encrypted password
$secPw = ConvertTo-SecureString -AsPlainText $Password -Force
$AESKey = Get-content $KeyPath
$Encryptedpassword = $secPw | ConvertFrom-SecureString -Key $AESKey
$Encryptedpassword
```

### <a name="store-hello-environment-variables"></a>Proměnné prostředí hello úložiště

1. Přejděte toohello funkce aplikace. Potom vyberte **funkce nastavení aplikace** > **konfigurovat nastavení aplikace**.

    ![Konfigurace nastavení aplikace][functions11]

1. Přidání proměnné prostředí hello a jejich hodnoty toohello aplikace nastavení a potom vyberte **Uložit**.

    ![Nastavení aplikace][functions12]

### <a name="add-powershell-toohello-function"></a>Přidání funkce toohello prostředí PowerShell

Je teď čas toomake volání sledovací proces sítě z v rámci hello Azure funkce. V závislosti na požadavcích hello hello implementací této funkce se může lišit. Obecný tok hello hello kódu však vypadá takto:

1. Proces vstupní parametry.
2. Paket existující dotaz zaznamená tooverify omezení a vyřešit konflikty názvů.
3. Vytvořte zachytáváním paketů s příslušnými parametry.
4. Dotazování paketu zachytit pravidelně, dokud nebude dokončeno.
5. Upozorněte uživatele hello, relaci zachytávání paketů hello je dokončena.

Hello následující příklad je prostředí PowerShell kód, který můžete použít ve funkci hello. Existují hodnoty, které je třeba toobe nahrazena pro **subscriptionId**, **resourceGroupName**, a **storageAccountName**.

```powershell
            #Import Azure PowerShell modules required toomake calls tooNetwork Watcher
            Import-Module "D:\home\site\wwwroot\AlertPacketCapturePowerShell\azuremodules\AzureRM.Profile\AzureRM.Profile.psd1" -Global
            Import-Module "D:\home\site\wwwroot\AlertPacketCapturePowerShell\azuremodules\AzureRM.Network\AzureRM.Network.psd1" -Global
            Import-Module "D:\home\site\wwwroot\AlertPacketCapturePowerShell\azuremodules\AzureRM.Resources\AzureRM.Resources.psd1" -Global

            #Process alert request body
            $requestBody = Get-Content $req -Raw | ConvertFrom-Json

            #Storage account ID toosave captures in
            $storageaccountid = "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}"

            #Packet capture vars
            $packetcapturename = "PSAzureFunction"
            $packetCaptureLimit = 10
            $packetCaptureDuration = 10

            #Credentials
            $tenant = $env:AzureTenant
            $pw = $env:AzureCredPassword
            $clientid = $env:AzureClientId
            $keypath = "D:\home\site\wwwroot\AlertPacketCapturePowerShell\keys\PassEncryptKey.key"

            #Authentication
            $secpassword = $pw | ConvertTo-SecureString -Key (Get-Content $keypath)
            $credential = New-Object System.Management.Automation.PSCredential ($clientid, $secpassword)
            Add-AzureRMAccount -ServicePrincipal -Tenant $tenant -Credential $credential #-WarningAction SilentlyContinue | out-null


            #Get hello VM that fired hello alert
            if($requestBody.context.resourceType -eq "Microsoft.Compute/virtualMachines")
            {
                Write-Output ("Subscription ID: {0}" -f $requestBody.context.subscriptionId)
                Write-Output ("Resource Group:  {0}" -f $requestBody.context.resourceGroupName)
                Write-Output ("Resource Name:  {0}" -f $requestBody.context.resourceName)
                Write-Output ("Resource Type:  {0}" -f $requestBody.context.resourceType)

                #Get hello Network Watcher in hello VM's region
                $nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq $requestBody.context.resourceRegion}
                $networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName

                #Get existing packetCaptures
                $packetCaptures = Get-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher

                #Remove existing packet capture created by hello function (if it exists)
                $packetCaptures | %{if($_.Name -eq $packetCaptureName)
                { 
                    Remove-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -PacketCaptureName $packetCaptureName
                }}

                #Initiate packet capture on hello VM that fired hello alert
                if ((Get-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher).Count -lt $packetCaptureLimit){
                    echo "Initiating Packet Capture"
                    New-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -TargetVirtualMachineId $requestBody.context.resourceId -PacketCaptureName $packetCaptureName -StorageAccountId $storageaccountid -TimeLimitInSeconds $packetCaptureDuration
                    Out-File -Encoding Ascii -FilePath $res -inputObject "Packet Capture created on ${requestBody.context.resourceID}"
                }
            } 
 ``` 
#### <a name="retrieve-hello-function-url"></a>Načíst URL pro hello – funkce 
1. Po vytvoření funkce, nakonfigurujte vaše adresa URL výstrahy toocall hello, který je spojen s hello funkce. tooget tuto hodnotu, adresa URL funkce hello kopírování z aplikace funkce.

    ![Hledání URL funkce hello][functions13]

2. Zkopírujte hello funkce URL pro aplikaci funkce.

    ![Kopírování URL funkce hello][2]

Pokud budete potřebovat vlastní vlastnosti v hello datovou část požadavku POST webhooku hello, podívejte se příliš[konfigurace webhook, jehož na výstrahu Azure metriky](../monitoring-and-diagnostics/insights-webhooks-alerts.md).

## <a name="configure-an-alert-on-a-vm"></a>Konfigurace výstrahy na virtuálním počítači

Výstrahy můžou být nakonfigurované toonotify jednotlivce, když konkrétní metrika překračuje prahovou hodnotu, která je přiřazena tooit. V tomto příkladu je výstraha hello na hello segmentů TCP, které se odesílají, ale hello může být pro výstraha mnoho jiné metriky. Výstraha v tomto příkladu je nakonfigurované toocall funkci webhooku toocall hello.

### <a name="create-hello-alert-rule"></a>Vytvořit pravidlo výstrahy hello

Přejděte tooan existující virtuální počítač a poté přidejte pravidlo výstrahy. Podrobnější dokumentaci týkající se konfigurace výstrahy naleznete na [vytvoření výstrahy v Azure monitorování pro služby Azure – portál Azure](../monitoring-and-diagnostics/insights-alerts-portal.md). Zadejte následující hodnoty v hello hello **pravidlo výstrahy** a pak vyberte **OK**.

  |**Nastavení** | **Hodnota** | **Podrobnosti** |
  |---|---|---|
  |**Název**|TCP_Segments_Sent_Exceeded|Název pravidla výstrahy hello.|
  |**Popis**|Segmentů TCP odeslané překročil prahovou hodnotu|Hello popis pro pravidlo výstrahy hello.||
  |**Metrika**|Odeslané segmenty TCP| Hello metriky toouse tootrigger hello výstraha. |
  |**Podmínka**|Více než| Hello podmínku toouse při vyhodnocování metrika hello.|
  |**Prahová hodnota**|100| Hodnota Hello hello metriku, která aktivuje výstrahu hello. Tato hodnota je třeba nastavit tooa platnou hodnotu pro vaše prostředí.|
  |**Období**|Přes hello posledních pět minut| Určuje interval hello v které toolook pro hello prahovou hodnotu na metrika hello.|
  |**Webhooku**|[URL webhooku se nenačetla z funkce aplikace]| Adresa URL webhooku Hello z hello funkce aplikace, který byl vytvořen v předchozích krocích hello.|

> [!NOTE]
> Metrika segmentů TCP Hello není povoleno ve výchozím nastavení. Další informace o další metriky tooenable navštivte stránky [zapínání monitorování a Diagnostika](../monitoring-and-diagnostics/insights-how-to-use-diagnostics.md).

## <a name="review-hello-results"></a>Zkontrolujte výsledky hello

Po hello kritéria pro výstrahy aktivační události hello vytvoří se zachytáváním paketů. Přejděte tooNetwork sledovacích procesů a poté vyberte **zachytáváním paketů**. Na této stránce můžete vybrat hello paketu zachycení soubor odkaz toodownload hello zachytáváním paketů.

![Zobrazení zachytávání paketů][functions14]

Pokud soubor zachycení hello je uložený místně, můžete ji načíst přihlášením toohello virtuálního počítače.

Pokyny týkající se stahování souborů z účtů úložiště Azure najdete v tématu [Začínáme s Azure Blob storage pomocí rozhraní .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md). Jiný nástroj, můžete je [Storage Explorer](http://storageexplorer.com/).

Po stažení vaší zachycení, můžete ji zobrazit pomocí libovolného nástroje, který může číst **CAP** souboru. Tady jsou odkazy tootwo tyto nástroje:

- [Microsoft Message Analyzer](https://technet.microsoft.com/library/jj649776.aspx)
- [WireShark](https://www.wireshark.org/)

## <a name="next-steps"></a>Další kroky

Zjistěte, jak tooview vaše paketu zaznamená navštivte stránky [analýza zachytávání paketů s Wireshark](network-watcher-deep-packet-inspection.md).


[1]: ./media/network-watcher-alert-triggered-packet-capture/figure1.png
[1-1]: ./media/network-watcher-alert-triggered-packet-capture/figure1-1.png
[2]: ./media/network-watcher-alert-triggered-packet-capture/figure2.png
[3]: ./media/network-watcher-alert-triggered-packet-capture/figure3.png
[functions1]:./media/network-watcher-alert-triggered-packet-capture/functions1.png
[functions2]:./media/network-watcher-alert-triggered-packet-capture/functions2.png
[functions3]:./media/network-watcher-alert-triggered-packet-capture/functions3.png
[functions4]:./media/network-watcher-alert-triggered-packet-capture/functions4.png
[functions5]:./media/network-watcher-alert-triggered-packet-capture/functions5.png
[functions6]:./media/network-watcher-alert-triggered-packet-capture/functions6.png
[functions7]:./media/network-watcher-alert-triggered-packet-capture/functions7.png
[functions8]:./media/network-watcher-alert-triggered-packet-capture/functions8.png
[functions9]:./media/network-watcher-alert-triggered-packet-capture/functions9.png
[functions10]:./media/network-watcher-alert-triggered-packet-capture/functions10.png
[functions11]:./media/network-watcher-alert-triggered-packet-capture/functions11.png
[functions12]:./media/network-watcher-alert-triggered-packet-capture/functions12.png
[functions13]:./media/network-watcher-alert-triggered-packet-capture/functions13.png
[functions14]:./media/network-watcher-alert-triggered-packet-capture/functions14.png
[scenario]:./media/network-watcher-alert-triggered-packet-capture/scenario.png
