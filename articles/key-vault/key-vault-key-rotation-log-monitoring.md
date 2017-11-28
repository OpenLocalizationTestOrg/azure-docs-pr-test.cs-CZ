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
# <a name="set-up-azure-key-vault-with-end-to-end-key-rotation-and-auditing"></a><span data-ttu-id="73dfd-103">Nastavení kompletní obměny klíčů a auditování ve službě Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="73dfd-103">Set up Azure Key Vault with end-to-end key rotation and auditing</span></span>
## <a name="introduction"></a><span data-ttu-id="73dfd-104">Úvod</span><span class="sxs-lookup"><span data-stu-id="73dfd-104">Introduction</span></span>
<span data-ttu-id="73dfd-105">Po vytvoření trezoru klíčů, bude možné toostart pomocí tohoto toostore trezoru klíčů a tajných klíčů.</span><span class="sxs-lookup"><span data-stu-id="73dfd-105">After creating your key vault, you will be able toostart using that vault toostore your keys and secrets.</span></span> <span data-ttu-id="73dfd-106">Aplikace už nepotřebujete toopersist klíčů nebo tajných klíčů, ale spíše bude o ně požádat z trezoru klíčů hello podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="73dfd-106">Your applications no longer need toopersist your keys or secrets, but rather will request them from hello key vault as needed.</span></span> <span data-ttu-id="73dfd-107">To vám umožní tooupdate klíče a tajné klíče bez ovlivnění hello chování vaší aplikace, což otevře a široké možnosti kolem vašeho klíče a tajné správy.</span><span class="sxs-lookup"><span data-stu-id="73dfd-107">This allows you tooupdate keys and secrets without affecting hello behavior of your application, which opens up a breadth of possibilities around your key and secret management.</span></span>

<span data-ttu-id="73dfd-108">Tento článek vás provede příklad použití Azure Key Vault toostore tajný klíč, v tomto případě klíčem účtu úložiště Azure, který přistupuje aplikaci.</span><span class="sxs-lookup"><span data-stu-id="73dfd-108">This article walks through an example of using Azure Key Vault toostore a secret, in this case an Azure Storage Account key that is accessed by an application.</span></span> <span data-ttu-id="73dfd-109">Také ukazuje implementaci naplánované oběh tento klíč účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="73dfd-109">It also demonstrates implementation of a scheduled rotation of that storage account key.</span></span> <span data-ttu-id="73dfd-110">Nakonec provede procesem ukázka jak toomonitor hello protokoly auditu trezoru klíčů a vyvolat výstrahy, když jsou vytvářeny neočekávané požadavky.</span><span class="sxs-lookup"><span data-stu-id="73dfd-110">Finally, it walks through a demonstration of how toomonitor hello key vault audit logs and raise alerts when unexpected requests are made.</span></span>

> [!NOTE]
> <span data-ttu-id="73dfd-111">V tomto kurzu není určený tooexplain v podrobností hello počátečním nastavení trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="73dfd-111">This tutorial is not intended tooexplain in detail hello initial setup of your key vault.</span></span> <span data-ttu-id="73dfd-112">Další informace naleznete v tématu [Začínáme s Azure Key Vault](key-vault-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="73dfd-112">For this information, see [Get started with Azure Key Vault](key-vault-get-started.md).</span></span> <span data-ttu-id="73dfd-113">Pokyny Multiplatformního rozhraní příkazového řádku najdete v tématu [Key Vault spravovat pomocí rozhraní příkazového řádku](key-vault-manage-with-cli2.md).</span><span class="sxs-lookup"><span data-stu-id="73dfd-113">For Cross-Platform Command-Line Interface instructions, see [Manage Key Vault using CLI](key-vault-manage-with-cli2.md).</span></span>
>
>

## <a name="set-up-key-vault"></a><span data-ttu-id="73dfd-114">Nastavení služby Key Vault</span><span class="sxs-lookup"><span data-stu-id="73dfd-114">Set up Key Vault</span></span>
<span data-ttu-id="73dfd-115">tooenable aplikaci tooretrieve tajného klíče z Key Vault, musíte nejprve vytvořit hello tajný klíč a nahrajte ho tooyour trezoru.</span><span class="sxs-lookup"><span data-stu-id="73dfd-115">tooenable an application tooretrieve a secret from Key Vault, you must first create hello secret and upload it tooyour vault.</span></span> <span data-ttu-id="73dfd-116">Můžete to provést spusťte relaci prostředí Azure PowerShell a přihlášení tooyour účet Azure s hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="73dfd-116">This can be accomplished by starting an Azure PowerShell session and signing in tooyour Azure account with hello following command:</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="73dfd-117">V okně hello automaticky otevírané okno prohlížeče zadejte účet Azure uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="73dfd-117">In hello pop-up browser window, enter your Azure account user name and password.</span></span> <span data-ttu-id="73dfd-118">PowerShell získá všechna předplatná hello, které jsou spojeny s tímto účtem.</span><span class="sxs-lookup"><span data-stu-id="73dfd-118">PowerShell will get all hello subscriptions that are associated with this account.</span></span> <span data-ttu-id="73dfd-119">Používá prostředí PowerShell text hello první z nich ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="73dfd-119">PowerShell uses hello first one by default.</span></span>

<span data-ttu-id="73dfd-120">Pokud máte více předplatných, můžete mít toospecify hello ten, který byl použité toocreate trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="73dfd-120">If you have multiple subscriptions, you might have toospecify hello one that was used toocreate your key vault.</span></span> <span data-ttu-id="73dfd-121">Zadejte hello následující toosee hello předplatných pro váš účet:</span><span class="sxs-lookup"><span data-stu-id="73dfd-121">Enter hello following toosee hello subscriptions for your account:</span></span>

```powershell
Get-AzureRmSubscription
```

<span data-ttu-id="73dfd-122">toospecify hello odběr, který je spojen s hello trezoru klíčů, který budete protokolovat, zadejte:</span><span class="sxs-lookup"><span data-stu-id="73dfd-122">toospecify hello subscription that's associated with hello key vault you will be logging, enter:</span></span>

```powershell
Set-AzureRmContext -SubscriptionId <subscriptionID>
```

<span data-ttu-id="73dfd-123">Vzhledem k tomu, že tento článek ukazuje, ukládání klíč účtu úložiště jako tajný klíč, musíte si tento klíč účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="73dfd-123">Because this article demonstrates storing a storage account key as a secret, you must get that storage account key.</span></span>

```powershell
Get-AzureRmStorageAccountKey -ResourceGroupName <resourceGroupName> -Name <storageAccountName>
```

<span data-ttu-id="73dfd-124">Po načtení váš tajný klíč (v tomto případě klíč účtu úložiště), musíte převést tuto tooa zabezpečený řetězec a pak vytvoření tajného klíče s hodnotou, kterou v trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="73dfd-124">After retrieving your secret (in this case, your storage account key), you must convert that tooa secure string and then create a secret with that value in your key vault.</span></span>

```powershell
$secretvalue = ConvertTo-SecureString <storageAccountKey> -AsPlainText -Force

Set-AzureKeyVaultSecret -VaultName <vaultName> -Name <secretName> -SecretValue $secretvalue
```
<span data-ttu-id="73dfd-125">V dalším kroku získáte hello identifikátor URI pro hello tajný klíč, který jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="73dfd-125">Next, get hello URI for hello secret you created.</span></span> <span data-ttu-id="73dfd-126">Používá se v pozdější fázi při volání tooretrieve trezoru klíčů hello váš tajný klíč.</span><span class="sxs-lookup"><span data-stu-id="73dfd-126">This is used in a later step when you call hello key vault tooretrieve your secret.</span></span> <span data-ttu-id="73dfd-127">Spusťte následující příkaz prostředí PowerShell hello a poznamenejte si hodnotu ID hello, což je tajný klíč hello URI:</span><span class="sxs-lookup"><span data-stu-id="73dfd-127">Run hello following PowerShell command and make note of hello ID value, which is hello secret URI:</span></span>

```powershell
Get-AzureKeyVaultSecret –VaultName <vaultName>
```

## <a name="set-up-hello-application"></a><span data-ttu-id="73dfd-128">Nastavení aplikace hello</span><span class="sxs-lookup"><span data-stu-id="73dfd-128">Set up hello application</span></span>
<span data-ttu-id="73dfd-129">Teď, když máte uložené tajný klíč, můžete použít kód tooretrieve a používat ho.</span><span class="sxs-lookup"><span data-stu-id="73dfd-129">Now that you have a secret stored, you can use code tooretrieve and use it.</span></span> <span data-ttu-id="73dfd-130">Existuje několik tooachieve požadované kroky to.</span><span class="sxs-lookup"><span data-stu-id="73dfd-130">There are a few steps required tooachieve this.</span></span> <span data-ttu-id="73dfd-131">Hello první a nejdůležitější krok je registrace vaší aplikace v Azure Active Directory a potom že Key Vault informace o vaší aplikaci tak, aby ji můžete povolit požadavky od vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="73dfd-131">hello first and most important step is registering your application with Azure Active Directory and then telling Key Vault your application information so that it can allow requests from your application.</span></span>

> [!NOTE]
> <span data-ttu-id="73dfd-132">Aplikace musí být vytvořeny na hello stejné klienta Azure Active Directory jako váš trezor klíčů.</span><span class="sxs-lookup"><span data-stu-id="73dfd-132">Your application must be created on hello same Azure Active Directory tenant as your key vault.</span></span>
>
>

<span data-ttu-id="73dfd-133">Otevřete kartu aplikace hello služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="73dfd-133">Open hello applications tab of Azure Active Directory.</span></span>

![Otevřete aplikace v Azure Active Directory](./media/keyvault-keyrotation/AzureAD_Header.png)

<span data-ttu-id="73dfd-135">Zvolte **přidat** tooadd tooyour aplikaci Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="73dfd-135">Choose **ADD** tooadd an application tooyour Azure Active Directory.</span></span>

![Zvolte možnost Přidat](./media/keyvault-keyrotation/Azure_AD_AddApp.png)

<span data-ttu-id="73dfd-137">Ponechat typ aplikace hello jako **webové aplikace nebo webové rozhraní API** a zadejte název vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="73dfd-137">Leave hello application type as **WEB APPLICATION AND/OR WEB API** and give your application a name.</span></span>

![Název hello aplikace](./media/keyvault-keyrotation/AzureAD_NewApp1.png)

<span data-ttu-id="73dfd-139">Poskytují aplikace **adresa URL přihlašování** a **identifikátor ID URI aplikace**.</span><span class="sxs-lookup"><span data-stu-id="73dfd-139">Give your application a **SIGN-ON URL** and an **APP ID URI**.</span></span> <span data-ttu-id="73dfd-140">Mohou to být nic, co chcete použít pro tuto ukázku, a může změnit později v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="73dfd-140">These can be anything you want for this demo, and they can be changed later if needed.</span></span>

![Zadejte požadované identifikátory URI](./media/keyvault-keyrotation/AzureAD_NewApp2.png)

<span data-ttu-id="73dfd-142">Po přidání tooAzure služby Active Directory aplikace hello přesměrován zpět na stránku aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="73dfd-142">After hello application is added tooAzure Active Directory, you will be brought into hello application page.</span></span> <span data-ttu-id="73dfd-143">Klikněte na tlačítko hello **konfigurace** kartě a vyhledejte a zkopírujte hello **ID klienta** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="73dfd-143">Click hello **Configure** tab and then find and copy hello **Client ID** value.</span></span> <span data-ttu-id="73dfd-144">Poznamenejte si ID klienta hello na pozdější kroky.</span><span class="sxs-lookup"><span data-stu-id="73dfd-144">Make note of hello client ID for later steps.</span></span>

<span data-ttu-id="73dfd-145">V dalším kroku vygenerujte klíč pro vaši aplikaci, můžete spolupracovat s vaší služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="73dfd-145">Next, generate a key for your application so it can interact with your Azure Active Directory.</span></span> <span data-ttu-id="73dfd-146">To můžete vytvořit pod hello **klíče** část v hello **konfigurace** kartě. Poznamenejte si klíč hello nově vygenerované z vaší aplikace Azure Active Directory pro použití v pozdější fázi.</span><span class="sxs-lookup"><span data-stu-id="73dfd-146">You can create this under hello **Keys** section in hello **Configuration** tab. Make note of hello newly generated key from your Azure Active Directory application for use in a later step.</span></span>

![Klíče aplikace služby Azure Active Directory](./media/keyvault-keyrotation/Azure_AD_AppKeys.png)

<span data-ttu-id="73dfd-148">Před vytvořením žádné volání z aplikace do trezoru klíčů hello, se musí zjistit trezoru klíčů hello o aplikace a její oprávnění.</span><span class="sxs-lookup"><span data-stu-id="73dfd-148">Before establishing any calls from your application into hello key vault, you must tell hello key vault about your application and its permissions.</span></span> <span data-ttu-id="73dfd-149">Hello následující příkaz má název trezoru hello a hello ID klienta z aplikací Azure Active Directory a uděluje **získat** trezoru klíčů tooyour přístupu pro aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="73dfd-149">hello following command takes hello vault name and hello client ID from your Azure Active Directory app and grants **Get** access tooyour key vault for hello application.</span></span>

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName <vaultName> -ServicePrincipalName <clientIDfromAzureAD> -PermissionsToSecrets Get
```

<span data-ttu-id="73dfd-150">V tomto okamžiku jsou připravené toostart vytváření voláními aplikace.</span><span class="sxs-lookup"><span data-stu-id="73dfd-150">At this point, you are ready toostart building your application calls.</span></span> <span data-ttu-id="73dfd-151">V aplikaci je nutné nainstalovat hello NuGet balíčky požadované toointeract s Azure Key Vault a Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="73dfd-151">In your application, you must install hello NuGet packages required toointeract with Azure Key Vault and Azure Active Directory.</span></span> <span data-ttu-id="73dfd-152">Z konzoly Správce balíčků Visual Studio hello zadejte následující příkazy hello.</span><span class="sxs-lookup"><span data-stu-id="73dfd-152">From hello Visual Studio Package Manager console, enter hello following commands.</span></span> <span data-ttu-id="73dfd-153">Na zápis hello tohoto článku, hello aktuální verze balíčku hello Azure Active Directory je 3.10.305231913, tak může chcete tooconfirm hello nejnovější verzi a aktualizují odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="73dfd-153">At hello writing of this article, hello current version of hello Azure Active Directory package is 3.10.305231913, so you might want tooconfirm hello latest version and update accordingly.</span></span>

```powershell
Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 3.10.305231913

Install-Package Microsoft.Azure.KeyVault
```

<span data-ttu-id="73dfd-154">V kódu aplikace vytvořte třídu toohold hello metodu pro ověřování Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="73dfd-154">In your application code, create a class toohold hello method for your Azure Active Directory authentication.</span></span> <span data-ttu-id="73dfd-155">V tomto příkladu se nazývá třídy **Utils**.</span><span class="sxs-lookup"><span data-stu-id="73dfd-155">In this example, that class is called **Utils**.</span></span> <span data-ttu-id="73dfd-156">Přidat hello následující příkaz using:</span><span class="sxs-lookup"><span data-stu-id="73dfd-156">Add hello following using statement:</span></span>

```csharp
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

<span data-ttu-id="73dfd-157">Dál přidejte hello následujících token JWT hello tooretrieve metoda z Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="73dfd-157">Next, add hello following method tooretrieve hello JWT token from Azure Active Directory.</span></span> <span data-ttu-id="73dfd-158">Pro udržovatelnosti může být vhodné toomove hello pevně řetězcové hodnoty do konfiguraci webu nebo aplikace.</span><span class="sxs-lookup"><span data-stu-id="73dfd-158">For maintainability, you may want toomove hello hard-coded string values into your web or application configuration.</span></span>

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

<span data-ttu-id="73dfd-159">Přidání nezbytného kódu toocall hello Key Vault a načtení tajná hodnota.</span><span class="sxs-lookup"><span data-stu-id="73dfd-159">Add hello necessary code toocall Key Vault and retrieve your secret value.</span></span> <span data-ttu-id="73dfd-160">Nejprve je nutné přidat hello následující příkaz using:</span><span class="sxs-lookup"><span data-stu-id="73dfd-160">First you must add hello following using statement:</span></span>

```csharp
using Microsoft.Azure.KeyVault;
```

<span data-ttu-id="73dfd-161">Přidání hello metoda volání tooinvoke Key Vault a načtení váš tajný klíč.</span><span class="sxs-lookup"><span data-stu-id="73dfd-161">Add hello method calls tooinvoke Key Vault and retrieve your secret.</span></span> <span data-ttu-id="73dfd-162">Tato metoda zadejte tajný klíč hello identifikátor URI, který jste uložili v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="73dfd-162">In this method, you provide hello secret URI that you saved in a previous step.</span></span> <span data-ttu-id="73dfd-163">Všimněte si použití hello hello **gettoken –** metoda z hello **Utils** třídy, které jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="73dfd-163">Note hello use of hello **GetToken** method from hello **Utils** class created previously.</span></span>

```csharp
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetToken));

var sec = kv.GetSecretAsync(<SecretID>).Result.Value;
```

<span data-ttu-id="73dfd-164">Při spuštění aplikace můžete by měl nyní být ověřování tooAzure služby Active Directory a potom načítání tajná hodnota z Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="73dfd-164">When you run your application, you should now be authenticating tooAzure Active Directory and then retrieving your secret value from Azure Key Vault.</span></span>

## <a name="key-rotation-using-azure-automation"></a><span data-ttu-id="73dfd-165">Střídání klíče pomocí Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="73dfd-165">Key rotation using Azure Automation</span></span>
<span data-ttu-id="73dfd-166">Existují různé možnosti pro implementaci strategie otočení pro hodnoty, které ukládáte jako Azure Key Vault tajných klíčů.</span><span class="sxs-lookup"><span data-stu-id="73dfd-166">There are various options for implementing a rotation strategy for values you store as Azure Key Vault secrets.</span></span> <span data-ttu-id="73dfd-167">Jako součást ruční proces lze otáčet tajné klíče, se může prostřednictvím kódu programu otáčet pomocí volání rozhraní API nebo může být otáčet prostřednictvím skriptu pro automatizaci.</span><span class="sxs-lookup"><span data-stu-id="73dfd-167">Secrets can be rotated as part of a manual process, they may be rotated programmatically by using API calls, or they may be rotated by way of an Automation script.</span></span> <span data-ttu-id="73dfd-168">Pro účely hello tohoto článku, budete pomocí prostředí Azure PowerShell v kombinaci s Azure Automation toochange přístupový klíč účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="73dfd-168">For hello purposes of this article, you will be using Azure PowerShell combined with Azure Automation toochange an Azure Storage Account access key.</span></span> <span data-ttu-id="73dfd-169">Pomocí tohoto nového klíče potom aktualizujte tajný klíč trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="73dfd-169">You will then update a key vault secret with that new key.</span></span>

<span data-ttu-id="73dfd-170">tooallow Azure Automation tooset tajný hodnoty v trezoru klíčů, musíte získat ID klienta hello hello připojení s názvem AzureRunAsConnection, která byla vytvořena, když jste vytvořili vaší instanci Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="73dfd-170">tooallow Azure Automation tooset secret values in your key vault, you must get hello client ID for hello connection named AzureRunAsConnection, which was created when you established your Azure Automation instance.</span></span> <span data-ttu-id="73dfd-171">Toto ID můžete najít tak, že zvolíte **prostředky** z vaší instanci Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="73dfd-171">You can find this ID by choosing **Assets** from your Azure Automation instance.</span></span> <span data-ttu-id="73dfd-172">Z tohoto umístění, zvolte **připojení** a pak vyberte hello **AzureRunAsConnection** služby zásadu.</span><span class="sxs-lookup"><span data-stu-id="73dfd-172">From there, you choose **Connections** and then select hello **AzureRunAsConnection** service principle.</span></span> <span data-ttu-id="73dfd-173">Poznamenejte si hello **ID aplikace**.</span><span class="sxs-lookup"><span data-stu-id="73dfd-173">Take note of hello **Application ID**.</span></span>

![ID klienta Azure Automation.](./media/keyvault-keyrotation/Azure_Automation_ClientID.png)

<span data-ttu-id="73dfd-175">V **prostředky**, zvolte **moduly**.</span><span class="sxs-lookup"><span data-stu-id="73dfd-175">In **Assets**, choose **Modules**.</span></span> <span data-ttu-id="73dfd-176">Z **moduly**, vyberte **Galerie**a poté vyhledejte a **Import** aktualizované verze jednotlivých hello následující moduly:</span><span class="sxs-lookup"><span data-stu-id="73dfd-176">From **Modules**, select **Gallery**, and then search for and **Import** updated versions of each of hello following modules:</span></span>

    Azure
    Azure.Storage
    AzureRM.Profile
    AzureRM.KeyVault
    AzureRM.Automation
    AzureRM.Storage


> [!NOTE]
> <span data-ttu-id="73dfd-177">V hello zápis tohoto článku, hello dříve uvedené moduly potřeby toobe aktualizuje jenom pro následující skript hello.</span><span class="sxs-lookup"><span data-stu-id="73dfd-177">At hello writing of this article, only hello previously noted modules needed toobe updated for hello following script.</span></span> <span data-ttu-id="73dfd-178">Pokud zjistíte, že je možné úlohu automatizace, potvrďte, že jste importovali všechny potřebné moduly a jejich závislosti.</span><span class="sxs-lookup"><span data-stu-id="73dfd-178">If you find that your automation job is failing, confirm that you have imported all necessary modules and their dependencies.</span></span>
>
>

<span data-ttu-id="73dfd-179">Po načtení ID aplikace hello pro připojení k Azure Automation, se musí zjistit trezoru klíčů, tato aplikace má přístup tooupdate tajných klíčů v trezoru.</span><span class="sxs-lookup"><span data-stu-id="73dfd-179">After you have retrieved hello application ID for your Azure Automation connection, you must tell your key vault that this application has access tooupdate secrets in your vault.</span></span> <span data-ttu-id="73dfd-180">Můžete to provést pomocí hello následující příkaz prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="73dfd-180">This can be accomplished with hello following PowerShell command:</span></span>

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName <vaultName> -ServicePrincipalName <applicationIDfromAzureAutomation> -PermissionsToSecrets Set
```

<span data-ttu-id="73dfd-181">Potom vyberte **Runbooky** v rámci vaší instanci Azure Automation a potom vyberte **přidat Runbook**.</span><span class="sxs-lookup"><span data-stu-id="73dfd-181">Next, select **Runbooks** under your Azure Automation instance, and then select **Add a Runbook**.</span></span> <span data-ttu-id="73dfd-182">Vyberte možnost **Rychle vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="73dfd-182">Select **Quick Create**.</span></span> <span data-ttu-id="73dfd-183">Název vaší sady runbook a vyberte **prostředí PowerShell** jako typ runbooku hello.</span><span class="sxs-lookup"><span data-stu-id="73dfd-183">Name your runbook and select **PowerShell** as hello runbook type.</span></span> <span data-ttu-id="73dfd-184">Máte možnost tooadd hello popis.</span><span class="sxs-lookup"><span data-stu-id="73dfd-184">You have hello option tooadd a description.</span></span> <span data-ttu-id="73dfd-185">Nakonec klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="73dfd-185">Finally, click **Create**.</span></span>

![Vytvoření sady runbook](./media/keyvault-keyrotation/Create_Runbook.png)

<span data-ttu-id="73dfd-187">Vložte následující skript prostředí PowerShell v podokně editor hello pro nový runbook hello:</span><span class="sxs-lookup"><span data-stu-id="73dfd-187">Paste hello following PowerShell script in hello editor pane for your new runbook:</span></span>

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

<span data-ttu-id="73dfd-188">V podokně editor hello, zvolte **testovací podokno** tootest vašeho skriptu.</span><span class="sxs-lookup"><span data-stu-id="73dfd-188">From hello editor pane, choose **Test pane** tootest your script.</span></span> <span data-ttu-id="73dfd-189">Po hello skriptu běží bez chyb, můžete vybrat **publikovat**, a pak můžete použít plán pro runbook hello zpět v podokně Konfigurace runbook hello.</span><span class="sxs-lookup"><span data-stu-id="73dfd-189">Once hello script is running without error, you can select **Publish**, and then you can apply a schedule for hello runbook back in hello runbook configuration pane.</span></span>

## <a name="key-vault-auditing-pipeline"></a><span data-ttu-id="73dfd-190">Auditování kanálu Key Vault</span><span class="sxs-lookup"><span data-stu-id="73dfd-190">Key Vault auditing pipeline</span></span>
<span data-ttu-id="73dfd-191">Při nastavování trezoru klíčů, můžete zapnout auditování toocollect protokoly na žádosti o přístup provedených toohello trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="73dfd-191">When you set up a key vault, you can turn on auditing toocollect logs on access requests made toohello key vault.</span></span> <span data-ttu-id="73dfd-192">Tyto protokoly se ukládají v určené účtu Azure Storage a mohou být vyžádány limitu, monitorovat a analyzovat.</span><span class="sxs-lookup"><span data-stu-id="73dfd-192">These logs are stored in a designated Azure Storage account and can be pulled out, monitored, and analyzed.</span></span> <span data-ttu-id="73dfd-193">Hello následující scénář používá funkce Azure, Azure logic apps a toocreate protokoly auditu trezoru klíčů toosend kanál e-mailu při aplikaci, která odpovídají ID aplikace hello hello webové aplikace načte tajné klíče z trezoru hello.</span><span class="sxs-lookup"><span data-stu-id="73dfd-193">hello following scenario uses Azure functions, Azure logic apps, and key vault audit logs toocreate a pipeline toosend an email when an app that does match hello app ID of hello web app retrieves secrets from hello vault.</span></span>

<span data-ttu-id="73dfd-194">Nejprve musíte povolit protokolování na váš trezor klíčů.</span><span class="sxs-lookup"><span data-stu-id="73dfd-194">First, you must enable logging on your key vault.</span></span> <span data-ttu-id="73dfd-195">To lze provést pomocí následujících příkazů prostředí PowerShell hello (úplné podrobnosti si můžete prohlédnout v [protokolování key vault](key-vault-logging.md)):</span><span class="sxs-lookup"><span data-stu-id="73dfd-195">This can be done via hello following PowerShell commands (full details can be seen at [key-vault-logging](key-vault-logging.md)):</span></span>

```powershell
$sa = New-AzureRmStorageAccount -ResourceGroupName <resourceGroupName> -Name <storageAccountName> -Type Standard\_LRS -Location 'East US'
$kv = Get-AzureRmKeyVault -VaultName '<vaultName>'
Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent
```

<span data-ttu-id="73dfd-196">Jakmile je tato možnost povolena, protokoly auditu počáteční shromažďování do hello určený účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="73dfd-196">After this is enabled, audit logs start collecting into hello designated storage account.</span></span> <span data-ttu-id="73dfd-197">Tyto protokoly obsahovat události o tom, jak a kdy vašim trezorům klíčů přistupuje a kým.</span><span class="sxs-lookup"><span data-stu-id="73dfd-197">These logs contain events about how and when your key vaults are accessed, and by whom.</span></span>

> [!NOTE]
> <span data-ttu-id="73dfd-198">Informace o protokolování můžete přistupovat 10 minut po operaci hello trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="73dfd-198">You can access your logging information 10 minutes after hello key vault operation.</span></span> <span data-ttu-id="73dfd-199">Obvykle bude rychlejší.</span><span class="sxs-lookup"><span data-stu-id="73dfd-199">It will usually be quicker than this.</span></span>
>
>

<span data-ttu-id="73dfd-200">dalším krokem Hello je příliš[vytvořit frontu Azure Service Bus](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md).</span><span class="sxs-lookup"><span data-stu-id="73dfd-200">hello next step is too[create an Azure Service Bus queue](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md).</span></span> <span data-ttu-id="73dfd-201">Toto je, kde jsou poslat protokoly auditu trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="73dfd-201">This is where key vault audit logs are pushed.</span></span> <span data-ttu-id="73dfd-202">Po hello zpráv protokolu auditování ve frontě hello se aplikace logiky hello je převezme a funguje na ně.</span><span class="sxs-lookup"><span data-stu-id="73dfd-202">When hello audit log messages are in hello queue, hello logic app picks them up and acts on them.</span></span> <span data-ttu-id="73dfd-203">Vytvoření služby service bus s hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="73dfd-203">Create a service bus with hello following steps:</span></span>

1. <span data-ttu-id="73dfd-204">Vytvoření oboru názvů Service Bus (Pokud již máte, který chcete toouse v takovém případě přeskočit tooStep 2).</span><span class="sxs-lookup"><span data-stu-id="73dfd-204">Create a Service Bus namespace (if you already have one that you want toouse for this, skip tooStep 2).</span></span>
2. <span data-ttu-id="73dfd-205">Procházejte toohello service bus v hello portál Azure a vyberte hello obor názvů, které chcete v toocreate hello fronty.</span><span class="sxs-lookup"><span data-stu-id="73dfd-205">Browse toohello service bus in hello Azure portal and select hello namespace you want toocreate hello queue in.</span></span>
3. <span data-ttu-id="73dfd-206">Vyberte **nový** a zvolte **Service Bus > fronty** a zadejte hello požadované podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="73dfd-206">Select **New** and choose **Service Bus > Queue** and enter hello required details.</span></span>
4. <span data-ttu-id="73dfd-207">Zvolit informace o připojení k Service Bus hello výběr hello obor názvů a **informace o připojení**.</span><span class="sxs-lookup"><span data-stu-id="73dfd-207">Select hello Service Bus connection information by choosing hello namespace and clicking **Connection Information**.</span></span> <span data-ttu-id="73dfd-208">Tyto informace budete potřebovat pro další části hello.</span><span class="sxs-lookup"><span data-stu-id="73dfd-208">You will need this information for hello next section.</span></span>

<span data-ttu-id="73dfd-209">Dále [vytvoření Azure funkce](../azure-functions/functions-create-first-azure-function.md) protokoly trezoru klíčů toopoll uvnitř hello účtu úložiště a vyzvednutí nové události.</span><span class="sxs-lookup"><span data-stu-id="73dfd-209">Next, [create an Azure function](../azure-functions/functions-create-first-azure-function.md) toopoll key vault logs within hello storage account and pick up new events.</span></span> <span data-ttu-id="73dfd-210">Bude jím funkci, která se spustí podle plánu.</span><span class="sxs-lookup"><span data-stu-id="73dfd-210">This will be a function that is triggered on a schedule.</span></span>

<span data-ttu-id="73dfd-211">toocreate Azure funkce, vyberte **nový > funkce aplikace** v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="73dfd-211">toocreate an Azure function, choose **New > Function App** in hello Azure portal.</span></span> <span data-ttu-id="73dfd-212">Během vytváření můžete použít existující hostingový plán nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="73dfd-212">During creation, you can use an existing hosting plan or create a new one.</span></span> <span data-ttu-id="73dfd-213">Může se rovněž rozhodnout pro dynamické hostování.</span><span class="sxs-lookup"><span data-stu-id="73dfd-213">You could also opt for dynamic hosting.</span></span> <span data-ttu-id="73dfd-214">Další informace o funkce hostování možnosti naleznete na [jak tooscale Azure Functions](../azure-functions/functions-scale.md).</span><span class="sxs-lookup"><span data-stu-id="73dfd-214">More details on Function hosting options can be found at [How tooscale Azure Functions](../azure-functions/functions-scale.md).</span></span>

<span data-ttu-id="73dfd-215">Když je vytvořen hello funkce Azure, přejděte tooit a zvolte časovač funkce a C\#.</span><span class="sxs-lookup"><span data-stu-id="73dfd-215">When hello Azure function is created, navigate tooit and choose a timer function and C\#.</span></span> <span data-ttu-id="73dfd-216">Pak klikněte na tlačítko **vytvořit tuto funkci**.</span><span class="sxs-lookup"><span data-stu-id="73dfd-216">Then click **Create this function**.</span></span>

![Okno Azure spustit funkce](./media/keyvault-keyrotation/Azure_Functions_Start.png)

<span data-ttu-id="73dfd-218">Na hello **vývoj** kartě, nahraďte kód run.csx hello hello následující:</span><span class="sxs-lookup"><span data-stu-id="73dfd-218">On hello **Develop** tab, replace hello run.csx code with hello following:</span></span>

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
> <span data-ttu-id="73dfd-219">Ujistěte se, že tooreplace hello proměnné v předcházející účet úložiště tooyour toopoint kód zápis hello protokoly trezoru klíčů, hello hello služby service bus, které jste vytvořili dříve, a hello protokoly úložiště trezoru klíčů toohello konkrétní cesty.</span><span class="sxs-lookup"><span data-stu-id="73dfd-219">Make sure tooreplace hello variables in hello preceding code toopoint tooyour storage account where hello key vault logs are written, hello service bus you created earlier, and hello specific path toohello key vault storage logs.</span></span>
>
>

<span data-ttu-id="73dfd-220">Funkce Hello převezme hello nejnovější soubor protokolu z účtu úložiště hello kde zapisují protokoly hello trezoru klíčů, získá hello nejnovější události z tohoto souboru a nabízených oznámení, je tooa fronty Service Bus.</span><span class="sxs-lookup"><span data-stu-id="73dfd-220">hello function picks up hello latest log file from hello storage account where hello key vault logs are written, grabs hello latest events from that file, and pushes them tooa Service Bus queue.</span></span> <span data-ttu-id="73dfd-221">Vzhledem k tomu, že jeden soubor může mít více událostí, měli vytvořit sync.txt soubor, který funkce hello také zjistí toodetermine hello časové razítko hello poslední událost, která byla zachyceny.</span><span class="sxs-lookup"><span data-stu-id="73dfd-221">Since a single file could have multiple events, you should create a sync.txt file that hello function also looks at toodetermine hello time stamp of hello last event that was picked up.</span></span> <span data-ttu-id="73dfd-222">To zajistí, že nemáte push hello stejnou událost vícekrát.</span><span class="sxs-lookup"><span data-stu-id="73dfd-222">This ensures that you don't push hello same event multiple times.</span></span> <span data-ttu-id="73dfd-223">Tento soubor sync.txt obsahuje časové razítko poslední události došlo k hello.</span><span class="sxs-lookup"><span data-stu-id="73dfd-223">This sync.txt file contains a timestamp for hello last encountered event.</span></span> <span data-ttu-id="73dfd-224">Hello protokoly, při načítání, mají toobe seřazené podle tooensure hello časového razítka jsou seřazené správně.</span><span class="sxs-lookup"><span data-stu-id="73dfd-224">hello logs, when loaded, have toobe sorted based on hello timestamp tooensure they are ordered correctly.</span></span>

<span data-ttu-id="73dfd-225">Pro tuto funkci jsme odkazovat na několik dalších knihovny, které nejsou k dispozici předinstalované hello v Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="73dfd-225">For this function, we reference a couple of additional libraries that are not available out of hello box in Azure Functions.</span></span> <span data-ttu-id="73dfd-226">tooinclude tyto, potřebujeme toopull Azure Functions je pomocí nástroje NuGet.</span><span class="sxs-lookup"><span data-stu-id="73dfd-226">tooinclude these, we need Azure Functions toopull them using NuGet.</span></span> <span data-ttu-id="73dfd-227">Zvolte hello **zobrazit soubory** možnost.</span><span class="sxs-lookup"><span data-stu-id="73dfd-227">Choose hello **View Files** option.</span></span>

![Zobrazení souborů](./media/keyvault-keyrotation/Azure_Functions_ViewFiles.png)

<span data-ttu-id="73dfd-229">A přidejte do souboru s názvem souboru project.json s následujícím obsahem:</span><span class="sxs-lookup"><span data-stu-id="73dfd-229">And add a file called project.json with following content:</span></span>

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
<span data-ttu-id="73dfd-230">Při **Uložit**, Azure Functions stáhne binární hello požadované soubory.</span><span class="sxs-lookup"><span data-stu-id="73dfd-230">Upon **Save**, Azure Functions will download hello required binaries.</span></span>

<span data-ttu-id="73dfd-231">Přepínač toohello **integrací** kartě a poskytnout parametr časovače hello toouse smysluplný název v rámci funkce hello.</span><span class="sxs-lookup"><span data-stu-id="73dfd-231">Switch toohello **Integrate** tab and give hello timer parameter a meaningful name toouse within hello function.</span></span> <span data-ttu-id="73dfd-232">V hello předcházející kódu, se očekává, že toobe časovače hello názvem *myTimer*.</span><span class="sxs-lookup"><span data-stu-id="73dfd-232">In hello preceding code, it expects hello timer toobe called *myTimer*.</span></span> <span data-ttu-id="73dfd-233">Zadejte [výraz CRON](../app-service-web/web-sites-create-web-jobs.md#CreateScheduledCRON) následujícím způsobem: 0 \* \* \* \* \* pro hello časovače, které způsobí hello funkce toorun jednou za minutu.</span><span class="sxs-lookup"><span data-stu-id="73dfd-233">Specify a [CRON expression](../app-service-web/web-sites-create-web-jobs.md#CreateScheduledCRON) as follows: 0 \* \* \* \* \* for hello timer that will cause hello function toorun once a minute.</span></span>

<span data-ttu-id="73dfd-234">Na stejné hello **integrací** přidejte vstup typu hello **Azure Blob Storage**.</span><span class="sxs-lookup"><span data-stu-id="73dfd-234">On hello same **Integrate** tab, add an input of hello type **Azure Blob Storage**.</span></span> <span data-ttu-id="73dfd-235">To bude odkazovat toohello sync.txt soubor, který obsahuje časové razítko poslední událost hello zvážení funkcí hello hello.</span><span class="sxs-lookup"><span data-stu-id="73dfd-235">This will point toohello sync.txt file that contains hello timestamp of hello last event looked at by hello function.</span></span> <span data-ttu-id="73dfd-236">To bude možné v rámci funkce hello podle názvu parametru hello.</span><span class="sxs-lookup"><span data-stu-id="73dfd-236">This will be available within hello function by hello parameter name.</span></span> <span data-ttu-id="73dfd-237">V předchozích kód hello, hello Azure Blob Storage vstup očekává hello parametr název toobe *inputBlob*.</span><span class="sxs-lookup"><span data-stu-id="73dfd-237">In hello preceding code, hello Azure Blob Storage input expects hello parameter name toobe *inputBlob*.</span></span> <span data-ttu-id="73dfd-238">Zvolte účet úložiště hello, kde bude uložený soubor sync.txt hello (může být hello stejný nebo jiný účet úložiště).</span><span class="sxs-lookup"><span data-stu-id="73dfd-238">Choose hello storage account where hello sync.txt file will reside (it could be hello same or a different storage account).</span></span> <span data-ttu-id="73dfd-239">V poli cesta hello zadejte cestu hello tam, kde platný hello soubor ve formátu hello {container-name}/path/to/sync.txt.</span><span class="sxs-lookup"><span data-stu-id="73dfd-239">In hello path field, provide hello path where hello file lives in hello format {container-name}/path/to/sync.txt.</span></span>

<span data-ttu-id="73dfd-240">Přidat výstup hello typu *Azure Blob Storage* výstup.</span><span class="sxs-lookup"><span data-stu-id="73dfd-240">Add an output of hello type *Azure Blob Storage* output.</span></span> <span data-ttu-id="73dfd-241">To bude odkazovat toohello sync.txt soubor, který jste definovali ve vstupu hello.</span><span class="sxs-lookup"><span data-stu-id="73dfd-241">This will point toohello sync.txt file you defined in hello input.</span></span> <span data-ttu-id="73dfd-242">Toto je používáno hello funkce toowrite hello časové razítko poslední událost hello prohlédli.</span><span class="sxs-lookup"><span data-stu-id="73dfd-242">This is used by hello function toowrite hello timestamp of hello last event looked at.</span></span> <span data-ttu-id="73dfd-243">Hello předchozí kód předpokládá, že tento parametr toobe názvem *outputBlob*.</span><span class="sxs-lookup"><span data-stu-id="73dfd-243">hello preceding code expects this parameter toobe called *outputBlob*.</span></span>

<span data-ttu-id="73dfd-244">Funkce hello je nyní připraven.</span><span class="sxs-lookup"><span data-stu-id="73dfd-244">At this point, hello function is ready.</span></span> <span data-ttu-id="73dfd-245">Ujistěte se, že tooswitch back toohello **vývoj** kartě a uložte hello kódu.</span><span class="sxs-lookup"><span data-stu-id="73dfd-245">Make sure tooswitch back toohello **Develop** tab and save hello code.</span></span> <span data-ttu-id="73dfd-246">Okno výstup hello případné chyby kompilace zkontrolujte a opravte je odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="73dfd-246">Check hello output window for any compilation errors and correct them accordingly.</span></span> <span data-ttu-id="73dfd-247">Pokud hello kód zkompiloval, hello kód by měl nyní kontrolovat protokoly trezoru klíčů hello každou minutu a vkládání všechny nové události do hello definované fronty Service Bus.</span><span class="sxs-lookup"><span data-stu-id="73dfd-247">If hello code compiles, then hello code should now be checking hello key vault logs every minute and pushing any new events onto hello defined Service Bus queue.</span></span> <span data-ttu-id="73dfd-248">Měli byste vidět informace o protokolování zapsat okno Protokol toohello pokaždé, když je spuštěna funkce hello.</span><span class="sxs-lookup"><span data-stu-id="73dfd-248">You should see logging information write out toohello log window every time hello function is triggered.</span></span>

### <a name="azure-logic-app"></a><span data-ttu-id="73dfd-249">Azure logic Apps</span><span class="sxs-lookup"><span data-stu-id="73dfd-249">Azure logic app</span></span>
<span data-ttu-id="73dfd-250">Dále musíte vytvořit aplikaci Azure logiku, která převezme hello události funkce hello je vkládání toohello fronty Service Bus, analyzuje obsah hello a odešle e-mailu na základě podmínky se shodoval.</span><span class="sxs-lookup"><span data-stu-id="73dfd-250">Next you must create an Azure logic app that picks up hello events that hello function is pushing toohello Service Bus queue, parses hello content, and sends an email based on a condition being matched.</span></span>

<span data-ttu-id="73dfd-251">[Vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md) přechodem příliš**nový > aplikace logiky**.</span><span class="sxs-lookup"><span data-stu-id="73dfd-251">[Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md) by going too**New > Logic App**.</span></span>

<span data-ttu-id="73dfd-252">Po vytvoření aplikace logiky hello přejděte tooit a zvolte **upravit**.</span><span class="sxs-lookup"><span data-stu-id="73dfd-252">Once hello logic app is created, navigate tooit and choose **edit**.</span></span> <span data-ttu-id="73dfd-253">V modulu snap-in editor aplikace logiky hello zvolte **frontou Service Bus** a zadejte vaše přihlašovací údaje tooconnect Service Bus je toohello fronty.</span><span class="sxs-lookup"><span data-stu-id="73dfd-253">Within hello logic app editor, choose **Service Bus Queue** and enter your Service Bus credentials tooconnect it toohello queue.</span></span>

![Azure Logic App Service Bus](./media/keyvault-keyrotation/Azure_LogicApp_ServiceBus.png)

<span data-ttu-id="73dfd-255">Dále vyberte **přidat podmínku**.</span><span class="sxs-lookup"><span data-stu-id="73dfd-255">Next choose **Add a condition**.</span></span> <span data-ttu-id="73dfd-256">V podmínce hello přepínač toohello advanced editoru a zadejte hello následující kód, nahraďte APP_ID hello skutečné APP_ID vaší webové aplikace:</span><span class="sxs-lookup"><span data-stu-id="73dfd-256">In hello condition, switch toohello advanced editor and enter hello following code, replacing APP_ID with hello actual APP_ID of your web app:</span></span>

```
@equals('<APP_ID>', json(decodeBase64(triggerBody()['ContentData']))['identity']['claim']['appid'])
```

<span data-ttu-id="73dfd-257">Tento výraz v podstatě vrátí **false** Pokud hello *appid* z hello není hello příchozí události (což je hello těla zprávy služby Service Bus hello) *appid* z hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="73dfd-257">This expression essentially returns **false** if hello *appid* from hello incoming event (which is hello body of hello Service Bus message) is not hello *appid* of hello app.</span></span>

<span data-ttu-id="73dfd-258">Teď vytvořte akce v rámci **Pokud ne, nedělat nic**.</span><span class="sxs-lookup"><span data-stu-id="73dfd-258">Now, create an action under **If no, do nothing**.</span></span>

![Aplikace logiky Azure vybrat akci](./media/keyvault-keyrotation/Azure_LogicApp_Condition.png)

<span data-ttu-id="73dfd-260">Pro akci hello zvolte **Office 365 - odesílání e-mailu**.</span><span class="sxs-lookup"><span data-stu-id="73dfd-260">For hello action, choose **Office 365 - send email**.</span></span> <span data-ttu-id="73dfd-261">Vyplňte pole toocreate hello toosend e-mailu při hello definované podmínky vrátí **false**.</span><span class="sxs-lookup"><span data-stu-id="73dfd-261">Fill out hello fields toocreate an email toosend when hello defined condition returns **false**.</span></span> <span data-ttu-id="73dfd-262">Pokud nemáte Office 365, může si prohlédnete alternativy tooachieve hello stejné výsledky.</span><span class="sxs-lookup"><span data-stu-id="73dfd-262">If you do not have Office 365, you could look at alternatives tooachieve hello same results.</span></span>

<span data-ttu-id="73dfd-263">V tuto chvíli máte kanál tooend end, které hledá nové protokoly auditu trezoru klíčů jednou za minutu.</span><span class="sxs-lookup"><span data-stu-id="73dfd-263">At this point, you have an end tooend pipeline that looks for new key vault audit logs once a minute.</span></span> <span data-ttu-id="73dfd-264">Vynutí nové protokoly najde tooa frontou service bus.</span><span class="sxs-lookup"><span data-stu-id="73dfd-264">It pushes new logs it finds tooa service bus queue.</span></span> <span data-ttu-id="73dfd-265">aplikace logiky Hello se aktivuje, když pojmenováváme novou zprávu ve frontě hello.</span><span class="sxs-lookup"><span data-stu-id="73dfd-265">hello logic app is triggered when a new message lands in hello queue.</span></span> <span data-ttu-id="73dfd-266">Pokud hello *appid* v rámci hello událostí neodpovídá ID aplikace hello hello volání aplikace, odešle e-mailu.</span><span class="sxs-lookup"><span data-stu-id="73dfd-266">If hello *appid* within hello event does not match hello app ID of hello calling application, it sends an email.</span></span>
