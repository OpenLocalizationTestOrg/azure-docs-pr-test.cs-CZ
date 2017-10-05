---
title: "Nastavit Azure Key Vault se klíče otočení začátku do konce a auditování | Microsoft Docs"
description: "Použijte tento postup můžete získat nastavit střídání klíče a monitorování protokoly trezoru klíčů."
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
ms.openlocfilehash: 38c342802ed687985ac6f84f5a590a1a0dcc6c6a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="set-up-azure-key-vault-with-end-to-end-key-rotation-and-auditing"></a><span data-ttu-id="c26df-103">Nastavení kompletní obměny klíčů a auditování ve službě Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="c26df-103">Set up Azure Key Vault with end-to-end key rotation and auditing</span></span>
## <a name="introduction"></a><span data-ttu-id="c26df-104">Úvod</span><span class="sxs-lookup"><span data-stu-id="c26df-104">Introduction</span></span>
<span data-ttu-id="c26df-105">Po vytvoření trezoru klíčů, budete moct začít používat tento trezor k ukládání klíčů a tajných klíčů.</span><span class="sxs-lookup"><span data-stu-id="c26df-105">After creating your key vault, you will be able to start using that vault to store your keys and secrets.</span></span> <span data-ttu-id="c26df-106">Aplikace už nutné k uchování klíčů nebo tajných klíčů, ale spíše bude o ně požádat z trezoru klíčů podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="c26df-106">Your applications no longer need to persist your keys or secrets, but rather will request them from the key vault as needed.</span></span> <span data-ttu-id="c26df-107">To umožňuje aktualizovat klíče a tajné klíče, aniž by to ovlivnilo chování vaší aplikace, což otevře a široké možnosti kolem vašeho klíče a tajné správy.</span><span class="sxs-lookup"><span data-stu-id="c26df-107">This allows you to update keys and secrets without affecting the behavior of your application, which opens up a breadth of possibilities around your key and secret management.</span></span>

<span data-ttu-id="c26df-108">Tento článek vás provede příklad použití Azure Key Vault pro uložení tajný klíč, v tomto případě klíčem účtu úložiště Azure, který přistupuje aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c26df-108">This article walks through an example of using Azure Key Vault to store a secret, in this case an Azure Storage Account key that is accessed by an application.</span></span> <span data-ttu-id="c26df-109">Také ukazuje implementaci naplánované oběh tento klíč účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="c26df-109">It also demonstrates implementation of a scheduled rotation of that storage account key.</span></span> <span data-ttu-id="c26df-110">Nakonec provede procesem ukázka sledování protokolů auditu trezoru klíčů a vyvolat výstrahy, když jsou vytvářeny neočekávané požadavky.</span><span class="sxs-lookup"><span data-stu-id="c26df-110">Finally, it walks through a demonstration of how to monitor the key vault audit logs and raise alerts when unexpected requests are made.</span></span>

> [!NOTE]
> <span data-ttu-id="c26df-111">V tomto kurzu není určeno k podrobně popisují počátečním nastavení trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="c26df-111">This tutorial is not intended to explain in detail the initial setup of your key vault.</span></span> <span data-ttu-id="c26df-112">Další informace naleznete v tématu [Začínáme s Azure Key Vault](key-vault-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="c26df-112">For this information, see [Get started with Azure Key Vault](key-vault-get-started.md).</span></span> <span data-ttu-id="c26df-113">Pokyny Multiplatformního rozhraní příkazového řádku najdete v tématu [Key Vault spravovat pomocí rozhraní příkazového řádku](key-vault-manage-with-cli2.md).</span><span class="sxs-lookup"><span data-stu-id="c26df-113">For Cross-Platform Command-Line Interface instructions, see [Manage Key Vault using CLI](key-vault-manage-with-cli2.md).</span></span>
>
>

## <a name="set-up-key-vault"></a><span data-ttu-id="c26df-114">Nastavení služby Key Vault</span><span class="sxs-lookup"><span data-stu-id="c26df-114">Set up Key Vault</span></span>
<span data-ttu-id="c26df-115">Chcete-li aplikaci načíst tajného klíče z Key Vault, musíte nejprve vytvořit tajný klíč a nahrajte ho do trezoru.</span><span class="sxs-lookup"><span data-stu-id="c26df-115">To enable an application to retrieve a secret from Key Vault, you must first create the secret and upload it to your vault.</span></span> <span data-ttu-id="c26df-116">Můžete to provést spusťte relaci prostředí Azure PowerShell a přihlášení k účtu Azure pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="c26df-116">This can be accomplished by starting an Azure PowerShell session and signing in to your Azure account with the following command:</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="c26df-117">V automaticky otevřeném okně prohlížeče zadejte svoje uživatelské jméno a heslo k účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="c26df-117">In the pop-up browser window, enter your Azure account user name and password.</span></span> <span data-ttu-id="c26df-118">PowerShell získá všechna předplatná, které jsou spojeny s tímto účtem.</span><span class="sxs-lookup"><span data-stu-id="c26df-118">PowerShell will get all the subscriptions that are associated with this account.</span></span> <span data-ttu-id="c26df-119">Prostředí PowerShell použije první, ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="c26df-119">PowerShell uses the first one by default.</span></span>

<span data-ttu-id="c26df-120">Pokud máte více předplatných, možná muset zadat ten, který byl použit k vytvoření trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="c26df-120">If you have multiple subscriptions, you might have to specify the one that was used to create your key vault.</span></span> <span data-ttu-id="c26df-121">Zadejte následující příkaz pro zobrazení předplatných pro váš účet:</span><span class="sxs-lookup"><span data-stu-id="c26df-121">Enter the following to see the subscriptions for your account:</span></span>

```powershell
Get-AzureRmSubscription
```

<span data-ttu-id="c26df-122">Chcete-li zadat odběr, který je spojen s trezoru klíčů, který budete protokolovat, zadejte:</span><span class="sxs-lookup"><span data-stu-id="c26df-122">To specify the subscription that's associated with the key vault you will be logging, enter:</span></span>

```powershell
Set-AzureRmContext -SubscriptionId <subscriptionID>
```

<span data-ttu-id="c26df-123">Vzhledem k tomu, že tento článek ukazuje, ukládání klíč účtu úložiště jako tajný klíč, musíte si tento klíč účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="c26df-123">Because this article demonstrates storing a storage account key as a secret, you must get that storage account key.</span></span>

```powershell
Get-AzureRmStorageAccountKey -ResourceGroupName <resourceGroupName> -Name <storageAccountName>
```

<span data-ttu-id="c26df-124">Po načtení váš tajný klíč (v tomto případě klíč účtu úložiště), je potřeba, převést na zabezpečený řetězec a pak vytvořit tajný klíč s touto hodnotou v trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="c26df-124">After retrieving your secret (in this case, your storage account key), you must convert that to a secure string and then create a secret with that value in your key vault.</span></span>

```powershell
$secretvalue = ConvertTo-SecureString <storageAccountKey> -AsPlainText -Force

Set-AzureKeyVaultSecret -VaultName <vaultName> -Name <secretName> -SecretValue $secretvalue
```
<span data-ttu-id="c26df-125">V dalším kroku získáte identifikátor URI pro tajný klíč, který jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="c26df-125">Next, get the URI for the secret you created.</span></span> <span data-ttu-id="c26df-126">Používá se v pozdější fázi při volání trezoru klíčů načíst váš tajný klíč.</span><span class="sxs-lookup"><span data-stu-id="c26df-126">This is used in a later step when you call the key vault to retrieve your secret.</span></span> <span data-ttu-id="c26df-127">Spusťte následující příkaz prostředí PowerShell a poznamenejte si hodnotu ID, která je tajný identifikátor URI:</span><span class="sxs-lookup"><span data-stu-id="c26df-127">Run the following PowerShell command and make note of the ID value, which is the secret URI:</span></span>

```powershell
Get-AzureKeyVaultSecret –VaultName <vaultName>
```

## <a name="set-up-the-application"></a><span data-ttu-id="c26df-128">Nastavení aplikace</span><span class="sxs-lookup"><span data-stu-id="c26df-128">Set up the application</span></span>
<span data-ttu-id="c26df-129">Teď, když máte uložené tajný klíč, můžete k načtení a jeho použití kódu.</span><span class="sxs-lookup"><span data-stu-id="c26df-129">Now that you have a secret stored, you can use code to retrieve and use it.</span></span> <span data-ttu-id="c26df-130">Existuje několik kroků potřebných k dosažení tohoto cíle.</span><span class="sxs-lookup"><span data-stu-id="c26df-130">There are a few steps required to achieve this.</span></span> <span data-ttu-id="c26df-131">První a nejdůležitější krok je registrace vaší aplikace v Azure Active Directory a potom že Key Vault informace o vaší aplikaci tak, aby ji můžete povolit požadavky od vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="c26df-131">The first and most important step is registering your application with Azure Active Directory and then telling Key Vault your application information so that it can allow requests from your application.</span></span>

> [!NOTE]
> <span data-ttu-id="c26df-132">Aplikace musí být vytvořeny na stejném tenantovi Azure Active Directory jako váš trezor klíčů.</span><span class="sxs-lookup"><span data-stu-id="c26df-132">Your application must be created on the same Azure Active Directory tenant as your key vault.</span></span>
>
>

<span data-ttu-id="c26df-133">Otevřete kartu aplikace služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="c26df-133">Open the applications tab of Azure Active Directory.</span></span>

![Otevřete aplikace v Azure Active Directory](./media/keyvault-keyrotation/AzureAD_Header.png)

<span data-ttu-id="c26df-135">Zvolte **přidat** přidání aplikace do Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="c26df-135">Choose **ADD** to add an application to your Azure Active Directory.</span></span>

![Zvolte možnost Přidat](./media/keyvault-keyrotation/Azure_AD_AddApp.png)

<span data-ttu-id="c26df-137">Ponechat typ aplikace jako **webové aplikace nebo webové rozhraní API** a zadejte název vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="c26df-137">Leave the application type as **WEB APPLICATION AND/OR WEB API** and give your application a name.</span></span>

![Název aplikace](./media/keyvault-keyrotation/AzureAD_NewApp1.png)

<span data-ttu-id="c26df-139">Poskytují aplikace **adresa URL přihlašování** a **identifikátor ID URI aplikace**.</span><span class="sxs-lookup"><span data-stu-id="c26df-139">Give your application a **SIGN-ON URL** and an **APP ID URI**.</span></span> <span data-ttu-id="c26df-140">Mohou to být nic, co chcete použít pro tuto ukázku, a může změnit později v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="c26df-140">These can be anything you want for this demo, and they can be changed later if needed.</span></span>

![Zadejte požadované identifikátory URI](./media/keyvault-keyrotation/AzureAD_NewApp2.png)

<span data-ttu-id="c26df-142">Po přidání aplikace do Azure Active Directory je přesměrován zpět na stránku aplikace.</span><span class="sxs-lookup"><span data-stu-id="c26df-142">After the application is added to Azure Active Directory, you will be brought into the application page.</span></span> <span data-ttu-id="c26df-143">Klikněte **konfigurace** kartě a vyhledejte a zkopírujte **ID klienta** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="c26df-143">Click the **Configure** tab and then find and copy the **Client ID** value.</span></span> <span data-ttu-id="c26df-144">Poznamenejte si ID klienta na pozdější kroky.</span><span class="sxs-lookup"><span data-stu-id="c26df-144">Make note of the client ID for later steps.</span></span>

<span data-ttu-id="c26df-145">V dalším kroku vygenerujte klíč pro vaši aplikaci, můžete spolupracovat s vaší služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="c26df-145">Next, generate a key for your application so it can interact with your Azure Active Directory.</span></span> <span data-ttu-id="c26df-146">Můžete vytvořit ji do **klíče** tématu **konfigurace** kartě.</span><span class="sxs-lookup"><span data-stu-id="c26df-146">You can create this under the **Keys** section in the **Configuration** tab.</span></span> <span data-ttu-id="c26df-147">Poznamenejte si nově vygenerovaný klíč z vaší aplikace Azure Active Directory pro použití v pozdější fázi.</span><span class="sxs-lookup"><span data-stu-id="c26df-147">Make note of the newly generated key from your Azure Active Directory application for use in a later step.</span></span>

![Klíče aplikace služby Azure Active Directory](./media/keyvault-keyrotation/Azure_AD_AppKeys.png)

<span data-ttu-id="c26df-149">Před vytvořením žádné volání z aplikace do trezoru klíčů, se musí zjistit trezoru klíčů o aplikace a její oprávnění.</span><span class="sxs-lookup"><span data-stu-id="c26df-149">Before establishing any calls from your application into the key vault, you must tell the key vault about your application and its permissions.</span></span> <span data-ttu-id="c26df-150">Následující příkaz přebírá název trezoru a ID klienta z aplikací Azure Active Directory a uděluje **získat** přístup k trezoru klíčů pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c26df-150">The following command takes the vault name and the client ID from your Azure Active Directory app and grants **Get** access to your key vault for the application.</span></span>

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName <vaultName> -ServicePrincipalName <clientIDfromAzureAD> -PermissionsToSecrets Get
```

<span data-ttu-id="c26df-151">V tomto okamžiku jste připravení začít vytvářet aplikace zavolat.</span><span class="sxs-lookup"><span data-stu-id="c26df-151">At this point, you are ready to start building your application calls.</span></span> <span data-ttu-id="c26df-152">V aplikaci je nutné nainstalovat balíčky NuGet, které jsou potřebné pro interakci s Azure Key Vault a Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="c26df-152">In your application, you must install the NuGet packages required to interact with Azure Key Vault and Azure Active Directory.</span></span> <span data-ttu-id="c26df-153">Z konzoly Správce balíčků Visual Studio zadejte následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="c26df-153">From the Visual Studio Package Manager console, enter the following commands.</span></span> <span data-ttu-id="c26df-154">Na zápis tohoto článku, aktuální verzi balíčku, Azure Active Directory je 3.10.305231913, takže můžete chtít potvrďte na nejnovější verzi a aktualizují odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="c26df-154">At the writing of this article, the current version of the Azure Active Directory package is 3.10.305231913, so you might want to confirm the latest version and update accordingly.</span></span>

```powershell
Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 3.10.305231913

Install-Package Microsoft.Azure.KeyVault
```

<span data-ttu-id="c26df-155">V kódu aplikace vytvořte třídu, pro uložení metodu pro ověřování Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="c26df-155">In your application code, create a class to hold the method for your Azure Active Directory authentication.</span></span> <span data-ttu-id="c26df-156">V tomto příkladu se nazývá třídy **Utils**.</span><span class="sxs-lookup"><span data-stu-id="c26df-156">In this example, that class is called **Utils**.</span></span> <span data-ttu-id="c26df-157">Přidejte následující příkaz using:</span><span class="sxs-lookup"><span data-stu-id="c26df-157">Add the following using statement:</span></span>

```csharp
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

<span data-ttu-id="c26df-158">Dál přidejte následující metodu na JWT token načítat z Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="c26df-158">Next, add the following method to retrieve the JWT token from Azure Active Directory.</span></span> <span data-ttu-id="c26df-159">Pro udržovatelnosti můžete přesunout pevně řetězcové hodnoty do konfiguraci webu nebo aplikace.</span><span class="sxs-lookup"><span data-stu-id="c26df-159">For maintainability, you may want to move the hard-coded string values into your web or application configuration.</span></span>

```csharp
public async static Task<string> GetToken(string authority, string resource, string scope)
{
    var authContext = new AuthenticationContext(authority);

    ClientCredential clientCred = new ClientCredential("<AzureADApplicationClientID>","<AzureADApplicationClientKey>");

    AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

    if (result == null)

    throw new InvalidOperationException("Failed to obtain the JWT token");

    return result.AccessToken;
}
```

<span data-ttu-id="c26df-160">Přidání nezbytného kódu volání Key Vault a načíst vaše tajná hodnota.</span><span class="sxs-lookup"><span data-stu-id="c26df-160">Add the necessary code to call Key Vault and retrieve your secret value.</span></span> <span data-ttu-id="c26df-161">Nejprve je nutné přidat následující příkaz using:</span><span class="sxs-lookup"><span data-stu-id="c26df-161">First you must add the following using statement:</span></span>

```csharp
using Microsoft.Azure.KeyVault;
```

<span data-ttu-id="c26df-162">Přidejte volání metody vyvolání Key Vault a načtení váš tajný klíč.</span><span class="sxs-lookup"><span data-stu-id="c26df-162">Add the method calls to invoke Key Vault and retrieve your secret.</span></span> <span data-ttu-id="c26df-163">Tato metoda zadejte identifikátor URI, který jste uložili v předchozím kroku tajného klíče.</span><span class="sxs-lookup"><span data-stu-id="c26df-163">In this method, you provide the secret URI that you saved in a previous step.</span></span> <span data-ttu-id="c26df-164">Všimněte si použití **gettoken –** metoda z **Utils** třídy, které jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="c26df-164">Note the use of the **GetToken** method from the **Utils** class created previously.</span></span>

```csharp
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetToken));

var sec = kv.GetSecretAsync(<SecretID>).Result.Value;
```

<span data-ttu-id="c26df-165">Při spuštění aplikace teď byste měli být ověřování pro službu Azure Active Directory a potom načítání tajná hodnota z Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="c26df-165">When you run your application, you should now be authenticating to Azure Active Directory and then retrieving your secret value from Azure Key Vault.</span></span>

## <a name="key-rotation-using-azure-automation"></a><span data-ttu-id="c26df-166">Střídání klíče pomocí Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="c26df-166">Key rotation using Azure Automation</span></span>
<span data-ttu-id="c26df-167">Existují různé možnosti pro implementaci strategie otočení pro hodnoty, které ukládáte jako Azure Key Vault tajných klíčů.</span><span class="sxs-lookup"><span data-stu-id="c26df-167">There are various options for implementing a rotation strategy for values you store as Azure Key Vault secrets.</span></span> <span data-ttu-id="c26df-168">Jako součást ruční proces lze otáčet tajné klíče, se může prostřednictvím kódu programu otáčet pomocí volání rozhraní API nebo může být otáčet prostřednictvím skriptu pro automatizaci.</span><span class="sxs-lookup"><span data-stu-id="c26df-168">Secrets can be rotated as part of a manual process, they may be rotated programmatically by using API calls, or they may be rotated by way of an Automation script.</span></span> <span data-ttu-id="c26df-169">Pro účely tohoto článku budete používat prostředí Azure PowerShell v kombinaci s Azure Automation. Chcete-li změnit přístupový klíč účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="c26df-169">For the purposes of this article, you will be using Azure PowerShell combined with Azure Automation to change an Azure Storage Account access key.</span></span> <span data-ttu-id="c26df-170">Pomocí tohoto nového klíče potom aktualizujte tajný klíč trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="c26df-170">You will then update a key vault secret with that new key.</span></span>

<span data-ttu-id="c26df-171">Povolit Azure Automation k nastavení tajný hodnot v trezoru klíčů, musíte získat ID klienta pro připojení s názvem AzureRunAsConnection, která byla vytvořena, když jste vytvořili vaší instanci Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="c26df-171">To allow Azure Automation to set secret values in your key vault, you must get the client ID for the connection named AzureRunAsConnection, which was created when you established your Azure Automation instance.</span></span> <span data-ttu-id="c26df-172">Toto ID můžete najít tak, že zvolíte **prostředky** z vaší instanci Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="c26df-172">You can find this ID by choosing **Assets** from your Azure Automation instance.</span></span> <span data-ttu-id="c26df-173">Odtud, zvolíte **připojení** a pak vyberte **AzureRunAsConnection** služby zásadu.</span><span class="sxs-lookup"><span data-stu-id="c26df-173">From there, you choose **Connections** and then select the **AzureRunAsConnection** service principle.</span></span> <span data-ttu-id="c26df-174">Poznamenejte si **ID aplikace**.</span><span class="sxs-lookup"><span data-stu-id="c26df-174">Take note of the **Application ID**.</span></span>

![ID klienta Azure Automation.](./media/keyvault-keyrotation/Azure_Automation_ClientID.png)

<span data-ttu-id="c26df-176">V **prostředky**, zvolte **moduly**.</span><span class="sxs-lookup"><span data-stu-id="c26df-176">In **Assets**, choose **Modules**.</span></span> <span data-ttu-id="c26df-177">Z **moduly**, vyberte **Galerie**a poté vyhledejte a **Import** aktualizované verze pro každý z následujících modulů:</span><span class="sxs-lookup"><span data-stu-id="c26df-177">From **Modules**, select **Gallery**, and then search for and **Import** updated versions of each of the following modules:</span></span>

    Azure
    Azure.Storage
    AzureRM.Profile
    AzureRM.KeyVault
    AzureRM.Automation
    AzureRM.Storage


> [!NOTE]
> <span data-ttu-id="c26df-178">Na zápis tohoto článku, jenom dříve uvedené moduly musela být aktualizováno pro následující skript.</span><span class="sxs-lookup"><span data-stu-id="c26df-178">At the writing of this article, only the previously noted modules needed to be updated for the following script.</span></span> <span data-ttu-id="c26df-179">Pokud zjistíte, že je možné úlohu automatizace, potvrďte, že jste importovali všechny potřebné moduly a jejich závislosti.</span><span class="sxs-lookup"><span data-stu-id="c26df-179">If you find that your automation job is failing, confirm that you have imported all necessary modules and their dependencies.</span></span>
>
>

<span data-ttu-id="c26df-180">Po načtení ID aplikace pro připojení k Azure Automation, se musí zjistit trezoru klíčů, tato aplikace má přístup k aktualizaci tajných klíčů v trezoru.</span><span class="sxs-lookup"><span data-stu-id="c26df-180">After you have retrieved the application ID for your Azure Automation connection, you must tell your key vault that this application has access to update secrets in your vault.</span></span> <span data-ttu-id="c26df-181">Můžete to provést pomocí následujícího příkazu prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="c26df-181">This can be accomplished with the following PowerShell command:</span></span>

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName <vaultName> -ServicePrincipalName <applicationIDfromAzureAutomation> -PermissionsToSecrets Set
```

<span data-ttu-id="c26df-182">Potom vyberte **Runbooky** v rámci vaší instanci Azure Automation a potom vyberte **přidat Runbook**.</span><span class="sxs-lookup"><span data-stu-id="c26df-182">Next, select **Runbooks** under your Azure Automation instance, and then select **Add a Runbook**.</span></span> <span data-ttu-id="c26df-183">Vyberte možnost **Rychle vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="c26df-183">Select **Quick Create**.</span></span> <span data-ttu-id="c26df-184">Název vaší sady runbook a vyberte **prostředí PowerShell** jako typ runbooku.</span><span class="sxs-lookup"><span data-stu-id="c26df-184">Name your runbook and select **PowerShell** as the runbook type.</span></span> <span data-ttu-id="c26df-185">Máte možnost přidat její popis.</span><span class="sxs-lookup"><span data-stu-id="c26df-185">You have the option to add a description.</span></span> <span data-ttu-id="c26df-186">Nakonec klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="c26df-186">Finally, click **Create**.</span></span>

![Vytvoření sady runbook](./media/keyvault-keyrotation/Create_Runbook.png)

<span data-ttu-id="c26df-188">V podokně editor pro nový runbook vložte následující skript prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="c26df-188">Paste the following PowerShell script in the editor pane for your new runbook:</span></span>

```powershell
$connectionName = "AzureRunAsConnection"
try
{
    # Get the connection "AzureRunAsConnection "
    $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         

    "Logging in to Azure..."
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

#Optionally you may set the following as parameters
$StorageAccountName = <storageAccountName>
$RGName = <storageAccountResourceGroupName>
$VaultName = <keyVaultName>
$SecretName = <keyVaultSecretName>

#Key name. For example key1 or key2 for the storage account
New-AzureRmStorageAccountKey -ResourceGroupName $RGName -Name $StorageAccountName -KeyName "key2" -Verbose
$SAKeys = Get-AzureRmStorageAccountKey -ResourceGroupName $RGName -Name $StorageAccountName

$secretvalue = ConvertTo-SecureString $SAKeys[1].Value -AsPlainText -Force

$secret = Set-AzureKeyVaultSecret -VaultName $VaultName -Name $SecretName -SecretValue $secretvalue
```

<span data-ttu-id="c26df-189">V podokně editor zvolte **testovací podokno** k testování vašeho skriptu.</span><span class="sxs-lookup"><span data-stu-id="c26df-189">From the editor pane, choose **Test pane** to test your script.</span></span> <span data-ttu-id="c26df-190">Jakmile se skript spouští bez chyby, můžete vybrat **publikovat**, a pak můžete použít plán pro sady runbook zpět v podokně Konfigurace sady runbook.</span><span class="sxs-lookup"><span data-stu-id="c26df-190">Once the script is running without error, you can select **Publish**, and then you can apply a schedule for the runbook back in the runbook configuration pane.</span></span>

## <a name="key-vault-auditing-pipeline"></a><span data-ttu-id="c26df-191">Auditování kanálu Key Vault</span><span class="sxs-lookup"><span data-stu-id="c26df-191">Key Vault auditing pipeline</span></span>
<span data-ttu-id="c26df-192">Při nastavování trezoru klíčů, můžete zapnout auditování shromažďovat protokoly v žádosti o přístup provedených do trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="c26df-192">When you set up a key vault, you can turn on auditing to collect logs on access requests made to the key vault.</span></span> <span data-ttu-id="c26df-193">Tyto protokoly se ukládají v určené účtu Azure Storage a mohou být vyžádány limitu, monitorovat a analyzovat.</span><span class="sxs-lookup"><span data-stu-id="c26df-193">These logs are stored in a designated Azure Storage account and can be pulled out, monitored, and analyzed.</span></span> <span data-ttu-id="c26df-194">Následující scénář používá k vytvoření kanálu pro odeslání e-mailu, když aplikaci, která odpovídají ID aplikace webové aplikace načte tajné klíče z trezoru Azure functions, Azure logic apps a protokoly auditu trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="c26df-194">The following scenario uses Azure functions, Azure logic apps, and key vault audit logs to create a pipeline to send an email when an app that does match the app ID of the web app retrieves secrets from the vault.</span></span>

<span data-ttu-id="c26df-195">Nejprve musíte povolit protokolování na váš trezor klíčů.</span><span class="sxs-lookup"><span data-stu-id="c26df-195">First, you must enable logging on your key vault.</span></span> <span data-ttu-id="c26df-196">To lze provést pomocí následujících příkazů Powershellu (úplné podrobnosti si můžete prohlédnout v [protokolování key vault](key-vault-logging.md)):</span><span class="sxs-lookup"><span data-stu-id="c26df-196">This can be done via the following PowerShell commands (full details can be seen at [key-vault-logging](key-vault-logging.md)):</span></span>

```powershell
$sa = New-AzureRmStorageAccount -ResourceGroupName <resourceGroupName> -Name <storageAccountName> -Type Standard\_LRS -Location 'East US'
$kv = Get-AzureRmKeyVault -VaultName '<vaultName>'
Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent
```

<span data-ttu-id="c26df-197">Jakmile je tato možnost povolena, protokoly auditu počáteční shromažďování do účtu úložiště určený.</span><span class="sxs-lookup"><span data-stu-id="c26df-197">After this is enabled, audit logs start collecting into the designated storage account.</span></span> <span data-ttu-id="c26df-198">Tyto protokoly obsahovat události o tom, jak a kdy vašim trezorům klíčů přistupuje a kým.</span><span class="sxs-lookup"><span data-stu-id="c26df-198">These logs contain events about how and when your key vaults are accessed, and by whom.</span></span>

> [!NOTE]
> <span data-ttu-id="c26df-199">Informace o protokolování můžete přistupovat 10 minut po operaci trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="c26df-199">You can access your logging information 10 minutes after the key vault operation.</span></span> <span data-ttu-id="c26df-200">Obvykle bude rychlejší.</span><span class="sxs-lookup"><span data-stu-id="c26df-200">It will usually be quicker than this.</span></span>
>
>

<span data-ttu-id="c26df-201">Dalším krokem je [vytvořit frontu Azure Service Bus](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md).</span><span class="sxs-lookup"><span data-stu-id="c26df-201">The next step is to [create an Azure Service Bus queue](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md).</span></span> <span data-ttu-id="c26df-202">Toto je, kde jsou poslat protokoly auditu trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="c26df-202">This is where key vault audit logs are pushed.</span></span> <span data-ttu-id="c26df-203">Když zpráv protokolu auditování ve frontě, aplikaci logiky je převezme a funguje na ně.</span><span class="sxs-lookup"><span data-stu-id="c26df-203">When the audit log messages are in the queue, the logic app picks them up and acts on them.</span></span> <span data-ttu-id="c26df-204">Vytvoření služby service bus pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="c26df-204">Create a service bus with the following steps:</span></span>

1. <span data-ttu-id="c26df-205">Vytvoření oboru názvů Service Bus (Pokud již účet máte, kterou chcete použít pro tento, přejděte ke kroku 2).</span><span class="sxs-lookup"><span data-stu-id="c26df-205">Create a Service Bus namespace (if you already have one that you want to use for this, skip to Step 2).</span></span>
2. <span data-ttu-id="c26df-206">Přejděte do služby service bus na portálu Azure a vyberte obor názvů, které chcete vytvořit ve frontě v.</span><span class="sxs-lookup"><span data-stu-id="c26df-206">Browse to the service bus in the Azure portal and select the namespace you want to create the queue in.</span></span>
3. <span data-ttu-id="c26df-207">Vyberte **nový** a zvolte **Service Bus > fronty** a zadejte požadované podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="c26df-207">Select **New** and choose **Service Bus > Queue** and enter the required details.</span></span>
4. <span data-ttu-id="c26df-208">Zvolit informace o připojení služby Service Bus výběr obor názvů a **informace o připojení**.</span><span class="sxs-lookup"><span data-stu-id="c26df-208">Select the Service Bus connection information by choosing the namespace and clicking **Connection Information**.</span></span> <span data-ttu-id="c26df-209">Tyto informace budete potřebovat pro další části.</span><span class="sxs-lookup"><span data-stu-id="c26df-209">You will need this information for the next section.</span></span>

<span data-ttu-id="c26df-210">Dále [vytvoření Azure funkce](../azure-functions/functions-create-first-azure-function.md) dotazování protokoly trezoru klíčů v rámci účtu úložiště a vyzvednutí nové události.</span><span class="sxs-lookup"><span data-stu-id="c26df-210">Next, [create an Azure function](../azure-functions/functions-create-first-azure-function.md) to poll key vault logs within the storage account and pick up new events.</span></span> <span data-ttu-id="c26df-211">Bude jím funkci, která se spustí podle plánu.</span><span class="sxs-lookup"><span data-stu-id="c26df-211">This will be a function that is triggered on a schedule.</span></span>

<span data-ttu-id="c26df-212">Pokud chcete vytvořit Azure funkce, vyberte **nový > funkce aplikace** na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c26df-212">To create an Azure function, choose **New > Function App** in the Azure portal.</span></span> <span data-ttu-id="c26df-213">Během vytváření můžete použít existující hostingový plán nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="c26df-213">During creation, you can use an existing hosting plan or create a new one.</span></span> <span data-ttu-id="c26df-214">Může se rovněž rozhodnout pro dynamické hostování.</span><span class="sxs-lookup"><span data-stu-id="c26df-214">You could also opt for dynamic hosting.</span></span> <span data-ttu-id="c26df-215">Další informace o funkce hostování možnosti naleznete na [postup škálování Azure Functions](../azure-functions/functions-scale.md).</span><span class="sxs-lookup"><span data-stu-id="c26df-215">More details on Function hosting options can be found at [How to scale Azure Functions](../azure-functions/functions-scale.md).</span></span>

<span data-ttu-id="c26df-216">Když je vytvořen funkci Azure, přejděte k němu a zvolte časovač funkce a C\#.</span><span class="sxs-lookup"><span data-stu-id="c26df-216">When the Azure function is created, navigate to it and choose a timer function and C\#.</span></span> <span data-ttu-id="c26df-217">Pak klikněte na tlačítko **vytvořit tuto funkci**.</span><span class="sxs-lookup"><span data-stu-id="c26df-217">Then click **Create this function**.</span></span>

![Okno Azure spustit funkce](./media/keyvault-keyrotation/Azure_Functions_Start.png)

<span data-ttu-id="c26df-219">Na **vývoj** kartě, run.csx kódu nahraďte následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="c26df-219">On the **Develop** tab, replace the run.csx code with the following:</span></span>

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
            log.Verbose($"Sync point file didnt have a date. Setting to now.");
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

            //required to order by time as they may not be in the file
            var results = ((IEnumerable<dynamic>) dynJson.records).OrderBy(p => p.time);

            foreach (var jsonItem in results)
            {
                DateTime dt = Convert.ToDateTime(jsonItem.time);

                if(dt>dtPrev){
                    log.Info($"{jsonItem.ToString()}");

                    var payloadStream = new MemoryStream(Encoding.UTF8.GetBytes(jsonItem.ToString()));
                    //When sending to ServiceBus, use the payloadStream and set keeporiginal to true
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

    //Generate the shared access signature on the container, setting the constraints directly on the signature.
    string sasBlobToken = blob.GetSharedAccessSignature(sasConstraints);

    //Return the URI string for the container, including the SAS token.
    return blob.Uri + sasBlobToken;
}
```


> [!NOTE]
> <span data-ttu-id="c26df-220">Nezapomeňte nahradit proměnné v předchozí kód tak, aby odkazoval na váš účet úložiště, kde se zapisují protokoly trezoru klíčů, sběrnici služby, které jste vytvořili dříve a konkrétní cestu k úložišti protokoly trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="c26df-220">Make sure to replace the variables in the preceding code to point to your storage account where the key vault logs are written, the service bus you created earlier, and the specific path to the key vault storage logs.</span></span>
>
>

<span data-ttu-id="c26df-221">Funkce převezme nejnovější soubor protokolu z účtu úložiště, kde se zapisují protokoly trezoru klíčů, získá nejnovější události z tohoto souboru a nabízených oznámení je do fronty Service Bus.</span><span class="sxs-lookup"><span data-stu-id="c26df-221">The function picks up the latest log file from the storage account where the key vault logs are written, grabs the latest events from that file, and pushes them to a Service Bus queue.</span></span> <span data-ttu-id="c26df-222">Vzhledem k tomu, že jeden soubor může mít více událostí, měli vytvořit sync.txt souboru, který funkce se také vypadá v Chcete-li zjistit časové razítko poslední události, která byla zachyceny.</span><span class="sxs-lookup"><span data-stu-id="c26df-222">Since a single file could have multiple events, you should create a sync.txt file that the function also looks at to determine the time stamp of the last event that was picked up.</span></span> <span data-ttu-id="c26df-223">Tím se zajistí, že nemáte nabízené stejnou událost vícekrát.</span><span class="sxs-lookup"><span data-stu-id="c26df-223">This ensures that you don't push the same event multiple times.</span></span> <span data-ttu-id="c26df-224">Tento soubor sync.txt obsahuje časové razítko pro poslední došlo k události.</span><span class="sxs-lookup"><span data-stu-id="c26df-224">This sync.txt file contains a timestamp for the last encountered event.</span></span> <span data-ttu-id="c26df-225">Protokoly, při načítání, musí být seřazeny podle časového razítka zajistit, že jsou řazení správně.</span><span class="sxs-lookup"><span data-stu-id="c26df-225">The logs, when loaded, have to be sorted based on the timestamp to ensure they are ordered correctly.</span></span>

<span data-ttu-id="c26df-226">Pro tuto funkci jsme odkazovat na několik dalších knihovny, které nejsou k dispozici ihned v Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="c26df-226">For this function, we reference a couple of additional libraries that are not available out of the box in Azure Functions.</span></span> <span data-ttu-id="c26df-227">Zahrnout tyto, potřebujeme Azure Functions je načítat pomocí nástroje NuGet.</span><span class="sxs-lookup"><span data-stu-id="c26df-227">To include these, we need Azure Functions to pull them using NuGet.</span></span> <span data-ttu-id="c26df-228">Vyberte **zobrazit soubory** možnost.</span><span class="sxs-lookup"><span data-stu-id="c26df-228">Choose the **View Files** option.</span></span>

![Zobrazení souborů](./media/keyvault-keyrotation/Azure_Functions_ViewFiles.png)

<span data-ttu-id="c26df-230">A přidejte do souboru s názvem souboru project.json s následujícím obsahem:</span><span class="sxs-lookup"><span data-stu-id="c26df-230">And add a file called project.json with following content:</span></span>

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
<span data-ttu-id="c26df-231">Při **Uložit**, Azure Functions stáhnou požadované binární soubory.</span><span class="sxs-lookup"><span data-stu-id="c26df-231">Upon **Save**, Azure Functions will download the required binaries.</span></span>

<span data-ttu-id="c26df-232">Přepnout **integrací** kartě a zadejte parametr časovače smysluplný název pro použití v rámci funkce.</span><span class="sxs-lookup"><span data-stu-id="c26df-232">Switch to the **Integrate** tab and give the timer parameter a meaningful name to use within the function.</span></span> <span data-ttu-id="c26df-233">V předchozím kódu, se očekává, že časovač k volání *myTimer*.</span><span class="sxs-lookup"><span data-stu-id="c26df-233">In the preceding code, it expects the timer to be called *myTimer*.</span></span> <span data-ttu-id="c26df-234">Zadejte [výraz CRON](../app-service-web/web-sites-create-web-jobs.md#CreateScheduledCRON) následujícím způsobem: 0 \* \* \* \* \* pro časovač způsobí, že funkce se má spustit jednou za minutu.</span><span class="sxs-lookup"><span data-stu-id="c26df-234">Specify a [CRON expression](../app-service-web/web-sites-create-web-jobs.md#CreateScheduledCRON) as follows: 0 \* \* \* \* \* for the timer that will cause the function to run once a minute.</span></span>

<span data-ttu-id="c26df-235">Ve stejném **integrací** přidejte vstup typu **Azure Blob Storage**.</span><span class="sxs-lookup"><span data-stu-id="c26df-235">On the same **Integrate** tab, add an input of the type **Azure Blob Storage**.</span></span> <span data-ttu-id="c26df-236">To bude odkazovat sync.txt soubor, který obsahuje časové razítko poslední události zvážení funkcí.</span><span class="sxs-lookup"><span data-stu-id="c26df-236">This will point to the sync.txt file that contains the timestamp of the last event looked at by the function.</span></span> <span data-ttu-id="c26df-237">To bude možné v rámci funkce název parametru.</span><span class="sxs-lookup"><span data-stu-id="c26df-237">This will be available within the function by the parameter name.</span></span> <span data-ttu-id="c26df-238">V předchozí kód Azure Blob Storage vstup očekává název parametru, který se má *inputBlob*.</span><span class="sxs-lookup"><span data-stu-id="c26df-238">In the preceding code, the Azure Blob Storage input expects the parameter name to be *inputBlob*.</span></span> <span data-ttu-id="c26df-239">Zvolte účet úložiště, kde bude uložený soubor sync.txt (může to být stejný nebo jiný účet úložiště).</span><span class="sxs-lookup"><span data-stu-id="c26df-239">Choose the storage account where the sync.txt file will reside (it could be the same or a different storage account).</span></span> <span data-ttu-id="c26df-240">Do pole Cesta zadejte cestu, kde je umístěn soubor ve formátu {container-name}/path/to/sync.txt.</span><span class="sxs-lookup"><span data-stu-id="c26df-240">In the path field, provide the path where the file lives in the format {container-name}/path/to/sync.txt.</span></span>

<span data-ttu-id="c26df-241">Přidat výstup typu *Azure Blob Storage* výstup.</span><span class="sxs-lookup"><span data-stu-id="c26df-241">Add an output of the type *Azure Blob Storage* output.</span></span> <span data-ttu-id="c26df-242">To bude odkazovat na sync.txt soubor, který jste definovali ve vstupu.</span><span class="sxs-lookup"><span data-stu-id="c26df-242">This will point to the sync.txt file you defined in the input.</span></span> <span data-ttu-id="c26df-243">To se používá k zápisu časové razítko poslední události po zvážení funkcí.</span><span class="sxs-lookup"><span data-stu-id="c26df-243">This is used by the function to write the timestamp of the last event looked at.</span></span> <span data-ttu-id="c26df-244">Předchozí kód očekává, že tento parametr má být volána *outputBlob*.</span><span class="sxs-lookup"><span data-stu-id="c26df-244">The preceding code expects this parameter to be called *outputBlob*.</span></span>

<span data-ttu-id="c26df-245">Funkce je nyní připraven.</span><span class="sxs-lookup"><span data-stu-id="c26df-245">At this point, the function is ready.</span></span> <span data-ttu-id="c26df-246">Nezapomeňte přepnout zpět **vývoj** kartě a uložte kód.</span><span class="sxs-lookup"><span data-stu-id="c26df-246">Make sure to switch back to the **Develop** tab and save the code.</span></span> <span data-ttu-id="c26df-247">Ve výstupním okně případné chyby kompilace zkontrolujte a opravte je odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="c26df-247">Check the output window for any compilation errors and correct them accordingly.</span></span> <span data-ttu-id="c26df-248">Pokud kompiluje se kód, pak kód by měl nyní se kontrola protokoly trezoru klíčů každou minutu a vkládání všechny nové události do definované fronty Service Bus.</span><span class="sxs-lookup"><span data-stu-id="c26df-248">If the code compiles, then the code should now be checking the key vault logs every minute and pushing any new events onto the defined Service Bus queue.</span></span> <span data-ttu-id="c26df-249">Měli byste vidět zapsat do okna protokolu pokaždé, když se aktivuje funkce informace o protokolování.</span><span class="sxs-lookup"><span data-stu-id="c26df-249">You should see logging information write out to the log window every time the function is triggered.</span></span>

### <a name="azure-logic-app"></a><span data-ttu-id="c26df-250">Azure logic Apps</span><span class="sxs-lookup"><span data-stu-id="c26df-250">Azure logic app</span></span>
<span data-ttu-id="c26df-251">Dále musíte vytvořit aplikaci Azure logiku, která převezme události funkce je vkládání do fronty Service Bus, analyzuje obsah a odešle e-mailu na základě podmínky se shodoval.</span><span class="sxs-lookup"><span data-stu-id="c26df-251">Next you must create an Azure logic app that picks up the events that the function is pushing to the Service Bus queue, parses the content, and sends an email based on a condition being matched.</span></span>

<span data-ttu-id="c26df-252">[Vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md) přechodem na **nový > aplikace logiky**.</span><span class="sxs-lookup"><span data-stu-id="c26df-252">[Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md) by going to **New > Logic App**.</span></span>

<span data-ttu-id="c26df-253">Po vytvoření aplikace logiky, přejděte k němu a zvolte **upravit**.</span><span class="sxs-lookup"><span data-stu-id="c26df-253">Once the logic app is created, navigate to it and choose **edit**.</span></span> <span data-ttu-id="c26df-254">V editoru aplikace logiky, vyberte **frontou Service Bus** a zadejte svá pověření Service Bus pro připojení k frontě.</span><span class="sxs-lookup"><span data-stu-id="c26df-254">Within the logic app editor, choose **Service Bus Queue** and enter your Service Bus credentials to connect it to the queue.</span></span>

![Azure Logic App Service Bus](./media/keyvault-keyrotation/Azure_LogicApp_ServiceBus.png)

<span data-ttu-id="c26df-256">Dále vyberte **přidat podmínku**.</span><span class="sxs-lookup"><span data-stu-id="c26df-256">Next choose **Add a condition**.</span></span> <span data-ttu-id="c26df-257">V podmínce přepněte do editoru Upřesnit a zadejte následující kód, nahraďte APP_ID skutečné APP_ID vaší webové aplikace:</span><span class="sxs-lookup"><span data-stu-id="c26df-257">In the condition, switch to the advanced editor and enter the following code, replacing APP_ID with the actual APP_ID of your web app:</span></span>

```
@equals('<APP_ID>', json(decodeBase64(triggerBody()['ContentData']))['identity']['claim']['appid'])
```

<span data-ttu-id="c26df-258">Tento výraz v podstatě vrátí **false** Pokud *appid* z příchozí události (což je do těla zprávy služby Service Bus) není *appid* aplikace.</span><span class="sxs-lookup"><span data-stu-id="c26df-258">This expression essentially returns **false** if the *appid* from the incoming event (which is the body of the Service Bus message) is not the *appid* of the app.</span></span>

<span data-ttu-id="c26df-259">Teď vytvořte akce v rámci **Pokud ne, nedělat nic**.</span><span class="sxs-lookup"><span data-stu-id="c26df-259">Now, create an action under **If no, do nothing**.</span></span>

![Aplikace logiky Azure vybrat akci](./media/keyvault-keyrotation/Azure_LogicApp_Condition.png)

<span data-ttu-id="c26df-261">Jako akci vyberte **Office 365 - odesílání e-mailu**.</span><span class="sxs-lookup"><span data-stu-id="c26df-261">For the action, choose **Office 365 - send email**.</span></span> <span data-ttu-id="c26df-262">Vyplňte pole pro vytvoření e-mailu k odeslání, když se vrátí definované podmínky **false**.</span><span class="sxs-lookup"><span data-stu-id="c26df-262">Fill out the fields to create an email to send when the defined condition returns **false**.</span></span> <span data-ttu-id="c26df-263">Pokud nemáte Office 365, může se podíváte na alternativy, abyste dosáhli stejné výsledky.</span><span class="sxs-lookup"><span data-stu-id="c26df-263">If you do not have Office 365, you could look at alternatives to achieve the same results.</span></span>

<span data-ttu-id="c26df-264">V tuto chvíli máte kanál koncová, které hledá nové protokoly auditu trezoru klíčů jednou za minutu.</span><span class="sxs-lookup"><span data-stu-id="c26df-264">At this point, you have an end to end pipeline that looks for new key vault audit logs once a minute.</span></span> <span data-ttu-id="c26df-265">Vynutí nové protokoly, které najde do fronty service bus.</span><span class="sxs-lookup"><span data-stu-id="c26df-265">It pushes new logs it finds to a service bus queue.</span></span> <span data-ttu-id="c26df-266">Aplikace logiky se aktivuje, když pojmenováváme novou zprávu ve frontě.</span><span class="sxs-lookup"><span data-stu-id="c26df-266">The logic app is triggered when a new message lands in the queue.</span></span> <span data-ttu-id="c26df-267">Pokud *appid* v rámci událost neodpovídá ID aplikace je volající aplikace, odešle e-mailu.</span><span class="sxs-lookup"><span data-stu-id="c26df-267">If the *appid* within the event does not match the app ID of the calling application, it sends an email.</span></span>
