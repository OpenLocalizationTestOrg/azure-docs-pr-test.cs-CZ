---
title: "Identita spravované služby ve službě App Service a Azure Functions | Microsoft Docs"
description: "Koncepční odkaz a Instalační příručka pro podporu identita spravované služby ve službě Azure App Service a Azure Functions"
services: app-service
author: mattchenderson
manager: cfowler
editor: 
ms.service: app-service
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 09/13/2017
ms.author: mahender
ms.openlocfilehash: 6b2dcaa4b0e0f59bf8a632b48813ba6a24202ec5
ms.sourcegitcommit: 7f1ce8be5367d492f4c8bb889ad50a99d85d9a89
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/06/2017
---
# <a name="how-to-use-azure-managed-service-identity-public-preview-in-app-service-and-azure-functions"></a>Jak používat Azure spravované služby Identity (verze public preview) ve službě App Service a Azure Functions

> [!NOTE] 
> Identita spravované služby pro služby App Service a Azure Functions je aktuálně ve verzi preview.

Toto téma ukazuje, jak vytvořit identitu spravované aplikace pro aplikace služby App Service a Azure Functions a způsobu jeho použití přistupovat k dalším prostředkům. Identita spravované služby ze služby Azure Active Directory umožňuje aplikaci snadno přistupovat k dalším prostředkům chráněné AAD, například Azure Key Vault. Identita spravuje platformy Azure a zřizovat nebo otočit všech tajných klíčů nevyžaduje. Další informace o identitě spravované služby najdete v tématu [identita spravované služby přehled](../active-directory/msi-overview.md).

## <a name="creating-an-app-with-an-identity"></a>Vytvoření aplikace pomocí identity

Vytvoření aplikace pomocí identity vyžaduje další vlastnost, kterou chcete nastavit na aplikaci.

> [!NOTE] 
> Pouze primární slotu pro webový server obdrží identitu. Spravovat identity služby pro nasazení, které se ještě nepodporují sloty.


### <a name="using-the-azure-portal"></a>Použití webu Azure Portal

Nastavit identita spravované služby v portálu, se nejprve vytvořit aplikace jako normální a pak povolte funkci.

1. Vytvoření aplikace v portálu běžným způsobem. Přejděte na ni na portálu.

2. Pokud pomocí funkce aplikace, přejděte do **funkce**. Pro ostatní typy aplikace přejděte dolů k položce **nastavení** skupiny v levém navigačním panelu.

3. Vyberte **identita spravované služby**.

4. Přepínač **registrují s Azure Active Directory** k **na**. Klikněte na **Uložit**.

![Identita spravované služby ve službě App Service](media/app-service-managed-service-identity/msi-blade.png)

### <a name="using-the-azure-cli"></a>Použití Azure CLI

Pokud chcete nastavit identitu spravované služby pomocí rozhraní příkazového řádku Azure, budete muset použít `az webapp assign-identity` příkaz s existující aplikaci. Máte tři možnosti pro spuštění v příkladech v této části:

- Použití [prostředí cloudu Azure](../cloud-shell/overview.md) z portálu Azure.
- Používání embedded prostředí cloudu Azure prostřednictvím "Zkuste ho" tlačítko, umístěné v pravém horním rohu každé blok kódu níže.
- [Nainstalujte nejnovější verzi 2.0 rozhraní příkazového řádku](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.21 nebo novější) Pokud byste radši chtěli použít místní konzoly rozhraní příkazového řádku. 

Následující postup vás provede vytvořením webové aplikace a jeho přiřazení identity pomocí rozhraní příkazového řádku:

1. Pokud používáte Azure CLI v místní konzole, nejdřív přihlásit k Azure pomocí [az přihlášení](/cli/azure/#login). Používáte účet, který je přidružen k předplatnému Azure, pod kterou chcete nasadit aplikaci:

    ```azurecli-interactive
    az login
    ```
2. Vytvoření webové aplikace pomocí rozhraní příkazového řádku. Další příklady použití rozhraní příkazového řádku službou App Service naleznete v tématu [ukázky rozhraní příkazového řádku služby aplikace](../app-service/app-service-cli-samples.md):

    ```azurecli-interactive
    az group create --name myResourceGroup --location westus
    az appservice plan create --name myplan --resource-group myResourceGroup --sku S1
    az webapp create --name myapp --resource-group myResourceGroup --plan myplan
    ```

3. Spustit `assign-identity` příkaz pro vytvoření identity pro tuto aplikaci:

    ```azurecli-interactive
    az webapp assign-identity --name myApp --resource-group myResourceGroup
    ```

### <a name="using-an-azure-resource-manager-template"></a>Pomocí šablony Azure Resource Manager

Šablonu Azure Resource Manager můžete použít k automatizaci nasazení vašich prostředků Azure. Další informace o nasazení do služby App Service a funkce, najdete v části [automatizaci nasazení prostředků ve službě App Service](../app-service/app-service-deploy-complex-application-predictably.md) a [automatizaci nasazení prostředků v Azure Functions](../azure-functions/functions-infrastructure-as-code.md).

Všechny prostředek typu `Microsoft.Web/sites` lze vytvořit pomocí identity zahrnutím následující vlastnost v definici prostředků:
```json
"identity": {
    "type": "SystemAssigned"
}    
```

Tato hodnota informuje Azure k vytvoření a Správa identit pro vaši aplikaci.

Webové aplikace může například vypadat jako následující:
```json
{
    "apiVersion": "2016-08-01",
    "type": "Microsoft.Web/sites",
    "name": "[variables('appName')]",
    "location": "[resourceGroup().location]",
    "identity": {
        "type": "SystemAssigned"
    },
    "properties": {
        "name": "[variables('appName')]",
        "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]",
        "hostingEnvironment": "",
        "clientAffinityEnabled": false,
        "alwaysOn": true
    },
    "dependsOn": [
        "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]"
    ]
}
```

Při vytvoření webu má následující další vlastnosti:
```json
"identity": {
    "tenantId": "<TENANTID>",
    "principalId": "<PRINCIPALID>"
}
```

Kde `<TENANTID>` a `<PRINCIPALID>` jsou nahrazeny identifikátory GUID. Vlastnost tenantId identifikuje jaké klienta AAD aplikace patří. PrincipalId je jedinečný identifikátor pro novou identitu aplikace. V rámci AAD aplikace má stejný název, který jste zadali pro vaše instance služby App Service nebo Azure Functions.

## <a name="obtaining-tokens-for-azure-resources"></a>Získat tokeny pro prostředky Azure

Aplikace můžete použít svou identitu získat tokeny k jiným prostředkům chráněn AAD, například Azure Key Vault. Tyto tokeny představují aplikace přístup k prostředku a ne všechny konkrétního uživatele aplikace. 

> [!IMPORTANT]
> Musíte nakonfigurovat cílový prostředek pro povolení přístupu z vaší aplikace. Například pokud požádáte o token pro Key Vault, budete muset Ujistěte se, že jste přidali zásadu přístupu, která zahrnuje identitu vaší aplikace. Jinak vaše volání Key Vault budou odmítnuty, i v případě, že obsahují token. Další více o prostředky, ke kterým podporují tokeny identita spravované služby najdete v tématu [služeb Azure, podpora Azure AD ověření](../active-directory/msi-overview.md#which-azure-services-support-managed-service-identity).

Není protokolu REST pro získání tokenu v App Service a Azure Functions. Pro aplikace .NET knihovně Microsoft.Azure.Services.AppAuthentication poskytuje abstrakci přes tento protokol a podporuje místní vývojové prostředí.

### <a name="asal"></a>Pomocí Microsoft.Azure.Services.AppAuthentication knihovna pro .NET

Pro aplikace .NET a funkcí je nejjednodušší způsob, jak pracovat s identitou spravované služby prostřednictvím Microsoft.Azure.Services.AppAuthentication balíčku. Tato knihovna vám také umožní Otestujte svůj kód místně na vývojovém počítači, pomocí účtu uživatele ze sady Visual Studio [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/overview?view=azure-cli-latest), nebo integrované ověřování Active Directory. Další informace o možnostech lokální vývoj s této knihovny najdete v tématu [Microsoft.Azure.Services.AppAuthentication odkaz]. V této části se dozvíte, jak začít pracovat s knihovnou ve vašem kódu.

1. Přidejte odkazy na [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) a [Microsoft.Azure.KeyVault](https://www.nuget.org/packages/Microsoft.Azure.KeyVault) balíčků NuGet do vaší aplikace.

2.  Přidejte následující kód do vaší aplikace:

```csharp
using Microsoft.Azure.Services.AppAuthentication;
using Microsoft.Azure.KeyVault;
// ...
var azureServiceTokenProvider = new AzureServiceTokenProvider();
string accessToken = await azureServiceTokenProvider.GetAccessTokenAsync("https://management.azure.com/");
// OR
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(azureServiceTokenProvider.KeyVaultTokenCallback));
```

Další informace o Microsoft.Azure.Services.AppAuthentication a operacích zpřístupňuje najdete v tématu [Microsoft.Azure.Services.AppAuthentication odkaz] a [služby App Service a KeyVault s MSI .NET Ukázka](https://github.com/Azure-Samples/app-service-msi-keyvault-dotnet).

### <a name="using-the-rest-protocol"></a>Pomocí protokolu REST

Aplikace s identitou, spravované služby má dvě proměnné definované:
- MSI_ENDPOINT
- MSI_SECRET

**MSI_ENDPOINT** je místní adresa URL, ze kterého aplikace žádosti o tokeny. K získání tokenu pro prostředek, ujistěte se, požadavek HTTP GET na tento koncový bod, včetně následujících parametrů:

> [!div class="mx-tdBreakAll"]
> |Název parametru|V|Popis|
> |-----|-----|-----|
> |Prostředek|Dotaz|AAD identifikátor URI prostředku, pro který by měl získat token.|
> |verze rozhraní API.|Dotaz|Verze rozhraní API tokenů, který se má použít. "2017-09-01" je aktuálně podporovány pouze verze.|
> |Tajný kód|Záhlaví|Hodnota proměnné prostředí MSI_SECRET.|


Úspěšná odpověď 200 OK zahrnuje text JSON s následujícími vlastnostmi:

> [!div class="mx-tdBreakAll"]
> |Název vlastnosti|Popis|
> |-------------|----------|
> |access_token|Požadovaný přístupový token. Volání webové služby můžete použít tento token k ověření přijímající webové služby.|
> |expires_on|Čas, kdy vyprší platnost přístupového tokenu. Datum je reprezentován jako počet sekund od pod hodnotou 1970-01-01T0:0:0Z UTC až do okamžiku vypršení platnosti. Tato hodnota se používá k určení doby platnosti tokenů v mezipaměti.|
> |Prostředek|Identifikátor ID URI aplikace přijímající webové služby.|
> |token_type|Určuje hodnotu typ tokenu. Pouze typ, který podporuje Azure AD je nosiče. Další informace o nosné tokeny najdete v tématu [rámci autorizace OAuth 2.0: použití tokenů nosiče (RFC 6750)](http://www.rfc-editor.org/rfc/rfc6750.txt).|


Tato odpověď je stejný jako [odpovědi pro požadavek tokenu přístupu do služby AAD](../active-directory/develop/active-directory-protocols-oauth-service-to-service.md#service-to-service-access-token-response).

> [!NOTE] 
> Proměnné prostředí se nastaví při prvním spuštění procesu, takže po povolení identita spravované služby pro vaši aplikaci budete muset restartovat aplikaci, nebo znovu nasadit před jeho kód `MSI_ENDPOINT` a `MSI_SECRET` jsou k dispozici pro váš kód.

### <a name="rest-protocol-examples"></a>Příklady protokol REST
Žádost o příklad může vypadat následovně:
```
GET /MSI/token?resource=https://vault.azure.net&api-version=2017-09-01 HTTP/1.1
Host: localhost:4141
Secret: 853b9a84-5bfa-4b22-a3f3-0b9a43d9ad8a
```
A ukázková odpověď může vypadat následovně:
```
HTTP/1.1 200 OK
Content-Type: application/json
 
{
    "access_token": "eyJ0eXAi…",
    "expires_on": "09/14/2017 00:00:00 PM +00:00",
    "resource": "https://vault.azure.net",
    "token_type": "Bearer"
}
```

### <a name="code-examples"></a>Příklady kódu
Pro vytvoření této žádosti v jazyce C#:
```csharp
public static async Task<HttpResponseMessage> GetToken(string resource, string apiversion)  {
    HttpClient client = new HttpClient();
    client.DefaultRequestHeaders.Add("Secret", Environment.GetEnvironmentVariable("MSI_SECRET"));
    return await client.GetAsync(String.Format("{0}/?resource={1}&api-version={2}", Environment.GetEnvironmentVariable("MSI_ENDPOINT"), resource, apiversion));
}
```
> [!TIP]
> Pro jazyky rozhraní .NET, můžete také použít [Microsoft.Azure.Services.AppAuthentication](#asal) místo věnujte to vyžadovat sami.

V Node.JS:
```javascript
const rp = require('request-promise');
const getToken = function(resource, apiver, cb) {
    var options = {
        uri: `${process.env["MSI_ENDPOINT"]}/?resource=${resource}&api-version=${apiver}`,
        headers: {
            'Secret': process.env["MSI_SECRET"]
        }
    };
    rp(options)
        .then(cb);
}
```

V prostředí PowerShell:
```powershell
$apiVersion = "2017-09-01"
$resourceURI = "https://<AAD-resource-URI-for-resource-to-obtain-token>"
$tokenAuthURI = $env:MSI_ENDPOINT + "?resource=$resourceURI&api-version=$apiVersion"
$tokenResponse = Invoke-RestMethod -Method Get -Headers @{"Secret"="$env:MSI_SECRET"} -Uri $tokenAuthURI
$accessToken = $tokenResponse.access_token
```


[Microsoft.Azure.Services.AppAuthentication odkaz]: https://go.microsoft.com/fwlink/p/?linkid=862452
