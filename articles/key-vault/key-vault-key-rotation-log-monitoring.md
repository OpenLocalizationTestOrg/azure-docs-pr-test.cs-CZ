---
title: "aaaSet si Azure Key Vault s začátku do konce střídání klíče a auditování | Microsoft Docs"
description: "Pomocí této jak tootoohelp, které získat vytvořili s střídání klíče a monitorování protokoly trezoru klíčů."
services: key-vault
documentationcenter: 
author: swgriffith
manager: mbaldwin
tags: 
ms.assetid: 9cd7e15e-23b8-41c0-a10a-06e6207ed157
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: jodehavi;stgriffi
ms.openlocfilehash: e0c393873077e3b91adc9fa7f39128bc1b6abe26
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-azure-key-vault-with-end-to-end-key-rotation-and-auditing"></a>Nastavení kompletní obměny klíčů a auditování ve službě Azure Key Vault
## <a name="introduction"></a>Úvod
Po vytvoření trezoru klíčů, bude možné toostart pomocí tohoto toostore trezoru klíčů a tajných klíčů. Aplikace už nepotřebujete toopersist klíčů nebo tajných klíčů, ale spíše bude o ně požádat z trezoru klíčů hello podle potřeby. To vám umožní tooupdate klíče a tajné klíče bez ovlivnění hello chování vaší aplikace, což otevře a široké možnosti kolem vašeho klíče a tajné správy.

Tento článek vás provede příklad použití Azure Key Vault toostore tajný klíč, v tomto případě klíčem účtu úložiště Azure, který přistupuje aplikaci. Také ukazuje implementaci naplánované oběh tento klíč účtu úložiště. Nakonec provede procesem ukázka jak toomonitor hello protokoly auditu trezoru klíčů a vyvolat výstrahy, když jsou vytvářeny neočekávané požadavky.

> [!NOTE]
> V tomto kurzu není určený tooexplain v podrobností hello počátečním nastavení trezoru klíčů. Další informace naleznete v tématu [Začínáme s Azure Key Vault](key-vault-get-started.md) Pokyny Multiplatformního rozhraní příkazového řádku najdete v tématu [Key Vault spravovat pomocí rozhraní příkazového řádku](key-vault-manage-with-cli2.md).
>
>

## <a name="set-up-key-vault"></a>Nastavení služby Key Vault
tooenable aplikaci tooretrieve tajného klíče z Key Vault, musíte nejprve vytvořit hello tajný klíč a nahrajte ho tooyour trezoru. Můžete to provést spusťte relaci prostředí Azure PowerShell a přihlášení tooyour účet Azure s hello následující příkaz:

```powershell
Login-AzureRmAccount
```

V okně hello automaticky otevírané okno prohlížeče zadejte účet Azure uživatelské jméno a heslo. PowerShell získá všechna předplatná hello, které jsou spojeny s tímto účtem. Používá prostředí PowerShell text hello první z nich ve výchozím nastavení.

Pokud máte více předplatných, můžete mít toospecify hello ten, který byl použité toocreate trezoru klíčů. Zadejte hello následující toosee hello předplatných pro váš účet:

```powershell
Get-AzureRmSubscription
```

toospecify hello odběr, který je spojen s hello trezoru klíčů, který budete protokolovat, zadejte:

```powershell
Set-AzureRmContext -SubscriptionId <subscriptionID>
```

Vzhledem k tomu, že tento článek ukazuje, ukládání klíč účtu úložiště jako tajný klíč, musíte si tento klíč účtu úložiště.

```powershell
Get-AzureRmStorageAccountKey -ResourceGroupName <resourceGroupName> -Name <storageAccountName>
```

Po načtení váš tajný klíč (v tomto případě klíč účtu úložiště), musíte převést tuto tooa zabezpečený řetězec a pak vytvoření tajného klíče s hodnotou, kterou v trezoru klíčů.

```powershell
$secretvalue = ConvertTo-SecureString <storageAccountKey> -AsPlainText -Force

Set-AzureKeyVaultSecret -VaultName <vaultName> -Name <secretName> -SecretValue $secretvalue
```
V dalším kroku získáte hello identifikátor URI pro hello tajný klíč, který jste vytvořili. Používá se v pozdější fázi při volání tooretrieve trezoru klíčů hello váš tajný klíč. Spusťte následující příkaz prostředí PowerShell hello a poznamenejte si hodnotu ID hello, což je tajný klíč hello URI:

```powershell
Get-AzureKeyVaultSecret –VaultName <vaultName>
```

## <a name="set-up-hello-application"></a>Nastavení aplikace hello
Teď, když máte uložené tajný klíč, můžete použít kód tooretrieve a používat ho. Existuje několik tooachieve požadované kroky to. Hello první a nejdůležitější krok je registrace vaší aplikace v Azure Active Directory a potom že Key Vault informace o vaší aplikaci tak, aby ji můžete povolit požadavky od vaší aplikace.

> [!NOTE]
> Aplikace musí být vytvořeny na hello stejné klienta Azure Active Directory jako váš trezor klíčů.
>
>

Otevřete kartu aplikace hello služby Azure Active Directory.

![Otevřete aplikace v Azure Active Directory](./media/keyvault-keyrotation/AzureAD_Header.png)

Zvolte **přidat** tooadd tooyour aplikaci Azure Active Directory.

![Zvolte možnost Přidat](./media/keyvault-keyrotation/Azure_AD_AddApp.png)

Ponechat typ aplikace hello jako **webové aplikace nebo webové rozhraní API** a zadejte název vaší aplikace.

![Název hello aplikace](./media/keyvault-keyrotation/AzureAD_NewApp1.png)

Poskytují aplikace **adresa URL přihlašování** a **identifikátor ID URI aplikace**. Mohou to být nic, co chcete použít pro tuto ukázku, a může změnit později v případě potřeby.

![Zadejte požadované identifikátory URI](./media/keyvault-keyrotation/AzureAD_NewApp2.png)

Po přidání tooAzure služby Active Directory aplikace hello přesměrován zpět na stránku aplikace hello. Klikněte na tlačítko hello **konfigurace** kartě a vyhledejte a zkopírujte hello **ID klienta** hodnotu. Poznamenejte si ID klienta hello na pozdější kroky.

V dalším kroku vygenerujte klíč pro vaši aplikaci, můžete spolupracovat s vaší služby Azure Active Directory. To můžete vytvořit pod hello **klíče** část v hello **konfigurace** kartě. Poznamenejte si klíč hello nově vygenerované z vaší aplikace Azure Active Directory pro použití v pozdější fázi.

![Klíče aplikace služby Azure Active Directory](./media/keyvault-keyrotation/Azure_AD_AppKeys.png)

Před vytvořením žádné volání z aplikace do trezoru klíčů hello, se musí zjistit trezoru klíčů hello o aplikace a její oprávnění. Hello následující příkaz má název trezoru hello a hello ID klienta z aplikací Azure Active Directory a uděluje **získat** trezoru klíčů tooyour přístupu pro aplikace hello.

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName <vaultName> -ServicePrincipalName <clientIDfromAzureAD> -PermissionsToSecrets Get
```

V tomto okamžiku jsou připravené toostart vytváření voláními aplikace. V aplikaci je nutné nainstalovat hello NuGet balíčky požadované toointeract s Azure Key Vault a Azure Active Directory. Z konzoly Správce balíčků Visual Studio hello zadejte následující příkazy hello. Na zápis hello tohoto článku, hello aktuální verze balíčku hello Azure Active Directory je 3.10.305231913, tak může chcete tooconfirm hello nejnovější verzi a aktualizují odpovídajícím způsobem.

```powershell
Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 3.10.305231913

Install-Package Microsoft.Azure.KeyVault
```

V kódu aplikace vytvořte třídu toohold hello metodu pro ověřování Azure Active Directory. V tomto příkladu se nazývá třídy **Utils**. Přidat hello následující příkaz using:

```csharp
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

Dál přidejte hello následujících token JWT hello tooretrieve metoda z Azure Active Directory. Pro udržovatelnosti může být vhodné toomove hello pevně řetězcové hodnoty do konfiguraci webu nebo aplikace.

```csharp
public async static Task<string> GetToken(string authority, string resource, string scope)
{
    var authContext = new AuthenticationContext(authority);

    ClientCredential clientCred = new ClientCredential("<AzureADApplicationClientID>","<AzureADApplicationClientKey>");

    AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

    if (result == null)

    throw new InvalidOperationException("Failed tooobtain hello JWT token");

    return result.AccessToken;
}
```

Přidání nezbytného kódu toocall hello Key Vault a načtení tajná hodnota. Nejprve je nutné přidat hello následující příkaz using:

```csharp
using Microsoft.Azure.KeyVault;
```

Přidání hello metoda volání tooinvoke Key Vault a načtení váš tajný klíč. Tato metoda zadejte tajný klíč hello identifikátor URI, který jste uložili v předchozím kroku. Všimněte si použití hello hello **gettoken –** metoda z hello **Utils** třídy, které jste vytvořili dříve.

```csharp
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetToken));

var sec = kv.GetSecretAsync(<SecretID>).Result.Value;
```

Při spuštění aplikace můžete by měl nyní být ověřování tooAzure služby Active Directory a potom načítání tajná hodnota z Azure Key Vault.

## <a name="key-rotation-using-azure-automation"></a>Střídání klíče pomocí Azure Automation.
Existují různé možnosti pro implementaci strategie otočení pro hodnoty, které ukládáte jako Azure Key Vault tajných klíčů. Jako součást ruční proces lze otáčet tajné klíče, se může prostřednictvím kódu programu otáčet pomocí volání rozhraní API nebo může být otáčet prostřednictvím skriptu pro automatizaci. Pro účely hello tohoto článku, budete pomocí prostředí Azure PowerShell v kombinaci s Azure Automation toochange přístupový klíč účtu úložiště Azure. Pomocí tohoto nového klíče potom aktualizujte tajný klíč trezoru klíčů.

tooallow Azure Automation tooset tajný hodnoty v trezoru klíčů, musíte získat ID klienta hello hello připojení s názvem AzureRunAsConnection, která byla vytvořena, když jste vytvořili vaší instanci Azure Automation. Toto ID můžete najít tak, že zvolíte **prostředky** z vaší instanci Azure Automation. Z tohoto umístění, zvolte **připojení** a pak vyberte hello **AzureRunAsConnection** služby zásadu. Poznamenejte si hello **ID aplikace**.

![ID klienta Azure Automation.](./media/keyvault-keyrotation/Azure_Automation_ClientID.png)

V **prostředky**, zvolte **moduly**. Z **moduly**, vyberte **Galerie**a poté vyhledejte a **Import** aktualizované verze jednotlivých hello následující moduly:

    Azure
    Azure.Storage
    AzureRM.Profile
    AzureRM.KeyVault
    AzureRM.Automation
    AzureRM.Storage


> [!NOTE]
> V hello zápis tohoto článku, hello dříve uvedené moduly potřeby toobe aktualizuje jenom pro následující skript hello. Pokud zjistíte, že je možné úlohu automatizace, potvrďte, že jste importovali všechny potřebné moduly a jejich závislosti.
>
>

Po načtení ID aplikace hello pro připojení k Azure Automation, se musí zjistit trezoru klíčů, tato aplikace má přístup tooupdate tajných klíčů v trezoru. Můžete to provést pomocí hello následující příkaz prostředí PowerShell:

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName <vaultName> -ServicePrincipalName <applicationIDfromAzureAutomation> -PermissionsToSecrets Set
```

Potom vyberte **Runbooky** v rámci vaší instanci Azure Automation a potom vyberte **přidat Runbook**. Vyberte možnost **Rychle vytvořit**. Název vaší sady runbook a vyberte **prostředí PowerShell** jako typ runbooku hello. Máte možnost tooadd hello popis. Nakonec klikněte na **vytvořit**.

![Vytvoření sady runbook](./media/keyvault-keyrotation/Create_Runbook.png)

Vložte následující skript prostředí PowerShell v podokně editor hello pro nový runbook hello:

```powershell
$connectionName = "AzureRunAsConnection"
try
{
    # Get hello connection "AzureRunAsConnection "
    $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         

    "Logging in tooAzure..."
    Add-AzureRmAccount `
        -ServicePrincipal `
        -TenantId $servicePrincipalConnection.TenantId `
        -ApplicationId $servicePrincipalConnection.ApplicationId `
        -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint
    "Login complete."
}
catch {
    if (!$servicePrincipalConnection)
    {
        $ErrorMessage = "Connection $connectionName not found."
        throw $ErrorMessage
    } else{
        Write-Error -Message $_.Exception
        throw $_.Exception
    }
}

#Optionally you may set hello following as parameters
$StorageAccountName = <storageAccountName>
$RGName = <storageAccountResourceGroupName>
$VaultName = <keyVaultName>
$SecretName = <keyVaultSecretName>

#Key name. For example key1 or key2 for hello storage account
New-AzureRmStorageAccountKey -ResourceGroupName $RGName -Name $StorageAccountName -KeyName "key2" -Verbose
$SAKeys = Get-AzureRmStorageAccountKey -ResourceGroupName $RGName -Name $StorageAccountName

$secretvalue = ConvertTo-SecureString $SAKeys[1].Value -AsPlainText -Force

$secret = Set-AzureKeyVaultSecret -VaultName $VaultName -Name $SecretName -SecretValue $secretvalue
```

V podokně editor hello, zvolte **testovací podokno** tootest vašeho skriptu. Po hello skriptu běží bez chyb, můžete vybrat **publikovat**, a pak můžete použít plán pro runbook hello zpět v podokně Konfigurace runbook hello.

## <a name="key-vault-auditing-pipeline"></a>Auditování kanálu Key Vault
Při nastavování trezoru klíčů, můžete zapnout auditování toocollect protokoly na žádosti o přístup provedených toohello trezoru klíčů. Tyto protokoly se ukládají v určené účtu Azure Storage a mohou být vyžádány limitu, monitorovat a analyzovat. Hello následující scénář používá funkce Azure, Azure logic apps a toocreate protokoly auditu trezoru klíčů toosend kanál e-mailu při aplikaci, která odpovídají ID aplikace hello hello webové aplikace načte tajné klíče z trezoru hello.

Nejprve musíte povolit protokolování na váš trezor klíčů. To lze provést pomocí následujících příkazů prostředí PowerShell hello (úplné podrobnosti si můžete prohlédnout v [protokolování key vault](key-vault-logging.md)):

```powershell
$sa = New-AzureRmStorageAccount -ResourceGroupName <resourceGroupName> -Name <storageAccountName> -Type Standard\_LRS -Location 'East US'
$kv = Get-AzureRmKeyVault -VaultName '<vaultName>'
Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent
```

Jakmile je tato možnost povolena, protokoly auditu počáteční shromažďování do hello určený účet úložiště. Tyto protokoly obsahovat události o tom, jak a kdy vašim trezorům klíčů přistupuje a kým.

> [!NOTE]
> Informace o protokolování můžete přistupovat 10 minut po operaci hello trezoru klíčů. Obvykle bude rychlejší.
>
>

dalším krokem Hello je příliš[vytvořit frontu Azure Service Bus](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md). Toto je, kde jsou poslat protokoly auditu trezoru klíčů. Po hello zpráv protokolu auditování ve frontě hello se aplikace logiky hello je převezme a funguje na ně. Vytvoření služby service bus s hello následující kroky:

1. Vytvoření oboru názvů Service Bus (Pokud již máte, který chcete toouse v takovém případě přeskočit tooStep 2).
2. Procházejte toohello service bus v hello portál Azure a vyberte hello obor názvů, které chcete v toocreate hello fronty.
3. Vyberte **nový** a zvolte **Service Bus > fronty** a zadejte hello požadované podrobnosti.
4. Zvolit informace o připojení k Service Bus hello výběr hello obor názvů a **informace o připojení**. Tyto informace budete potřebovat pro další části hello.

Dále [vytvoření Azure funkce](../azure-functions/functions-create-first-azure-function.md) protokoly trezoru klíčů toopoll uvnitř hello účtu úložiště a vyzvednutí nové události. Bude jím funkci, která se spustí podle plánu.

toocreate Azure funkce, vyberte **nový > funkce aplikace** v hello portálu Azure. Během vytváření můžete použít existující hostingový plán nebo vytvořte novou. Může se rovněž rozhodnout pro dynamické hostování. Další informace o funkce hostování možnosti naleznete na [jak tooscale Azure Functions](../azure-functions/functions-scale.md).

Když je vytvořen hello funkce Azure, přejděte tooit a zvolte časovač funkce a C\#. Pak klikněte na tlačítko **vytvořit tuto funkci**.

![Okno Azure spustit funkce](./media/keyvault-keyrotation/Azure_Functions_Start.png)

Na hello **vývoj** kartě, nahraďte kód run.csx hello hello následující:

```csharp
#r "Newtonsoft.Json"

using System;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Auth;
using Microsoft.WindowsAzure.Storage.Blob;
using Microsoft.ServiceBus.Messaging;
using System.Text;

public static void Run(TimerInfo myTimer, TextReader inputBlob, TextWriter outputBlob, TraceWriter log)
{
    log.Info("Starting");

    CloudStorageAccount sourceStorageAccount = new CloudStorageAccount(new StorageCredentials("<STORAGE_ACCOUNT_NAME>", "<STORAGE_ACCOUNT_KEY>"), true);

    CloudBlobClient sourceCloudBlobClient = sourceStorageAccount.CreateCloudBlobClient();

    var connectionString = "<SERVICE_BUS_CONNECTION_STRING>";
    var queueName = "<SERVICE_BUS_QUEUE_NAME>";

    var sbClient = QueueClient.CreateFromConnectionString(connectionString, queueName);

    DateTime dtPrev = DateTime.UtcNow;
    if(inputBlob != null)
    {
        var txt = inputBlob.ReadToEnd();

        if(!string.IsNullOrEmpty(txt))
        {
            dtPrev = DateTime.Parse(txt);
            log.Verbose($"SyncPoint: {dtPrev.ToString("O")}");
        }
        else
        {
            dtPrev = DateTime.UtcNow;
            log.Verbose($"Sync point file didnt have a date. Setting toonow.");
        }
    }

    var now = DateTime.UtcNow;

    string blobPrefix = "insights-logs-auditevent/resourceId=/SUBSCRIPTIONS/<SUBSCRIPTION_ID>/RESOURCEGROUPS/<RESOURCE_GROUP_NAME>/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/<KEY_VAULT_NAME>/y=" + now.Year +"/m="+now.Month.ToString("D2")+"/d="+ (now.Day).ToString("D2")+"/h="+(now.Hour).ToString("D2")+"/m=00/";

    log.Info($"Scanning:  {blobPrefix}");

    IEnumerable<IListBlobItem> blobs = sourceCloudBlobClient.ListBlobs(blobPrefix, true);

    log.Info($"found {blobs.Count()} blobs");

    foreach(var item in blobs)
    {
        if (item is CloudBlockBlob)
        {
            CloudBlockBlob blockBlob = (CloudBlockBlob)item;

            log.Info($"Syncing: {item.Uri}");

            string sharedAccessUri = GetContainerSasUri(blockBlob);

            CloudBlockBlob sourceBlob = new CloudBlockBlob(new Uri(sharedAccessUri));

            string text;
            using (var memoryStream = new MemoryStream())
            {
                sourceBlob.DownloadToStream(memoryStream);
                text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
            }

            dynamic dynJson = JsonConvert.DeserializeObject(text);

            //required tooorder by time as they may not be in hello file
            var results = ((IEnumerable<dynamic>) dynJson.records).OrderBy(p => p.time);

            foreach (var jsonItem in results)
            {
                DateTime dt = Convert.ToDateTime(jsonItem.time);

                if(dt>dtPrev){
                    log.Info($"{jsonItem.ToString()}");

                    var payloadStream = new MemoryStream(Encoding.UTF8.GetBytes(jsonItem.ToString()));
                    //When sending tooServiceBus, use hello payloadStream and set keeporiginal tootrue
                    var message = new BrokeredMessage(payloadStream, true);
                    sbClient.Send(message);
                    dtPrev = dt;
                }
            }
        }
    }
    outputBlob.Write(dtPrev.ToString("o"));
}

static string GetContainerSasUri(CloudBlockBlob blob)
{
    SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();

    sasConstraints.SharedAccessStartTime = DateTime.UtcNow.AddMinutes(-5);
    sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
    sasConstraints.Permissions = SharedAccessBlobPermissions.Read;

    //Generate hello shared access signature on hello container, setting hello constraints directly on hello signature.
    string sasBlobToken = blob.GetSharedAccessSignature(sasConstraints);

    //Return hello URI string for hello container, including hello SAS token.
    return blob.Uri + sasBlobToken;
}
```


> [!NOTE]
> Ujistěte se, že tooreplace hello proměnné v předcházející účet úložiště tooyour toopoint kód zápis hello protokoly trezoru klíčů, hello hello služby service bus, které jste vytvořili dříve, a hello protokoly úložiště trezoru klíčů toohello konkrétní cesty.
>
>

Funkce Hello převezme hello nejnovější soubor protokolu z účtu úložiště hello kde zapisují protokoly hello trezoru klíčů, získá hello nejnovější události z tohoto souboru a nabízených oznámení, je tooa fronty Service Bus. Vzhledem k tomu, že jeden soubor může mít více událostí, měli vytvořit sync.txt soubor, který funkce hello také zjistí toodetermine hello časové razítko hello poslední událost, která byla zachyceny. To zajistí, že nemáte push hello stejnou událost vícekrát. Tento soubor sync.txt obsahuje časové razítko poslední události došlo k hello. Hello protokoly, při načítání, mají toobe seřazené podle tooensure hello časového razítka jsou seřazené správně.

Pro tuto funkci jsme odkazovat na několik dalších knihovny, které nejsou k dispozici předinstalované hello v Azure Functions. tooinclude tyto, potřebujeme toopull Azure Functions je pomocí nástroje NuGet. Zvolte hello **zobrazit soubory** možnost.

![Zobrazení souborů](./media/keyvault-keyrotation/Azure_Functions_ViewFiles.png)

A přidejte do souboru s názvem souboru project.json s následujícím obsahem:

```json
    {
      "frameworks": {
        "net46":{
          "dependencies": {
                "WindowsAzure.Storage": "7.0.0",
                "WindowsAzure.ServiceBus":"3.2.2"
          }
        }
       }
    }
```
Při **Uložit**, Azure Functions stáhne binární hello požadované soubory.

Přepínač toohello **integrací** kartě a poskytnout parametr časovače hello toouse smysluplný název v rámci funkce hello. V hello předcházející kódu, se očekává, že toobe časovače hello názvem *myTimer*. Zadejte [výraz CRON](../app-service-web/web-sites-create-web-jobs.md#CreateScheduledCRON) následujícím způsobem: 0 \* \* \* \* \* pro hello časovače, které způsobí hello funkce toorun jednou za minutu.

Na stejné hello **integrací** přidejte vstup typu hello **Azure Blob Storage**. To bude odkazovat toohello sync.txt soubor, který obsahuje časové razítko poslední událost hello zvážení funkcí hello hello. To bude možné v rámci funkce hello podle názvu parametru hello. V předchozích kód hello, hello Azure Blob Storage vstup očekává hello parametr název toobe *inputBlob*. Zvolte účet úložiště hello, kde bude uložený soubor sync.txt hello (může být hello stejný nebo jiný účet úložiště). V poli cesta hello zadejte cestu hello tam, kde platný hello soubor ve formátu hello {container-name}/path/to/sync.txt.

Přidat výstup hello typu *Azure Blob Storage* výstup. To bude odkazovat toohello sync.txt soubor, který jste definovali ve vstupu hello. Toto je používáno hello funkce toowrite hello časové razítko poslední událost hello prohlédli. Hello předchozí kód předpokládá, že tento parametr toobe názvem *outputBlob*.

Funkce hello je nyní připraven. Ujistěte se, že tooswitch back toohello **vývoj** kartě a uložte hello kódu. Okno výstup hello případné chyby kompilace zkontrolujte a opravte je odpovídajícím způsobem. Pokud hello kód zkompiloval, hello kód by měl nyní kontrolovat protokoly trezoru klíčů hello každou minutu a vkládání všechny nové události do hello definované fronty Service Bus. Měli byste vidět informace o protokolování zapsat okno Protokol toohello pokaždé, když je spuštěna funkce hello.

### <a name="azure-logic-app"></a>Azure logic Apps
Dále musíte vytvořit aplikaci Azure logiku, která převezme hello události funkce hello je vkládání toohello fronty Service Bus, analyzuje obsah hello a odešle e-mailu na základě podmínky se shodoval.

[Vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md) přechodem příliš**nový > aplikace logiky**.

Po vytvoření aplikace logiky hello přejděte tooit a zvolte **upravit**. V modulu snap-in editor aplikace logiky hello zvolte **frontou Service Bus** a zadejte vaše přihlašovací údaje tooconnect Service Bus je toohello fronty.

![Azure Logic App Service Bus](./media/keyvault-keyrotation/Azure_LogicApp_ServiceBus.png)

Dále vyberte **přidat podmínku**. V podmínce hello přepínač toohello advanced editoru a zadejte hello následující kód, nahraďte APP_ID hello skutečné APP_ID vaší webové aplikace:

```
@equals('<APP_ID>', json(decodeBase64(triggerBody()['ContentData']))['identity']['claim']['appid'])
```

Tento výraz v podstatě vrátí **false** Pokud hello *appid* z hello není hello příchozí události (což je hello těla zprávy služby Service Bus hello) *appid* z hello aplikace.

Teď vytvořte akce v rámci **Pokud ne, nedělat nic**.

![Aplikace logiky Azure vybrat akci](./media/keyvault-keyrotation/Azure_LogicApp_Condition.png)

Pro akci hello zvolte **Office 365 - odesílání e-mailu**. Vyplňte pole toocreate hello toosend e-mailu při hello definované podmínky vrátí **false**. Pokud nemáte Office 365, může si prohlédnete alternativy tooachieve hello stejné výsledky.

V tuto chvíli máte kanál tooend end, které hledá nové protokoly auditu trezoru klíčů jednou za minutu. Vynutí nové protokoly najde tooa frontou service bus. aplikace logiky Hello se aktivuje, když pojmenováváme novou zprávu ve frontě hello. Pokud hello *appid* v rámci hello událostí neodpovídá ID aplikace hello hello volání aplikace, odešle e-mailu.
