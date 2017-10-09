---
title: "aaaAzure AD v2 Windows Desktop Začínáme - instalace | Microsoft Docs"
description: "Jak aplikace Windows Desktop .NET (XAML) můžete volat rozhraní API, které vyžadují přístupové tokeny bodem v2 Azure Active Directory"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: 097ea99bef01e15edaa5ff914ff4e18392b77c5a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
## <a name="set-up-your-project"></a><span data-ttu-id="dcdd0-103">Nastavení projektu</span><span class="sxs-lookup"><span data-stu-id="dcdd0-103">Set up your project</span></span>

<span data-ttu-id="dcdd0-104">Tato část obsahuje podrobné pokyny, jak toocreate nový projekt toodemonstrate jak toointegrate Windows Desktop .NET aplikace (XAML) s *přihlášení se společností Microsoft* , se můžete dotazovat webové rozhraní API, která vyžaduje token.</span><span class="sxs-lookup"><span data-stu-id="dcdd0-104">This section provides step-by-step instructions for how toocreate a new project toodemonstrate how toointegrate a Windows Desktop .NET application (XAML) with *Sign-In with Microsoft* so it can query Web APIs that requires a token.</span></span>

<span data-ttu-id="dcdd0-105">Hello aplikace vytvořené v této příručce zpřístupní tlačítko toograph a zobrazit výsledky na obrazovce a odhlášení tlačítko.</span><span class="sxs-lookup"><span data-stu-id="dcdd0-105">hello application created by this guide exposes a button toograph and show results on screen and a sign-out button.</span></span>

> <span data-ttu-id="dcdd0-106">Dáváte přednost toodownload projektu sady Visual Studio Tato ukázka místo?</span><span class="sxs-lookup"><span data-stu-id="dcdd0-106">Prefer toodownload this sample's Visual Studio project instead?</span></span> <span data-ttu-id="dcdd0-107">[Stažení projektu](https://github.com/Azure-Samples/active-directory-dotnet-desktop-msgraph-v2/archive/master.zip) a přeskočit toohello [krok konfigurace](#create-an-application-express) ukázka kódu hello tooconfigure před provedením.</span><span class="sxs-lookup"><span data-stu-id="dcdd0-107">[Download a project](https://github.com/Azure-Samples/active-directory-dotnet-desktop-msgraph-v2/archive/master.zip) and skip toohello [Configuration step](#create-an-application-express) tooconfigure hello code sample before executing.</span></span>


### <a name="create-your-application"></a><span data-ttu-id="dcdd0-108">Vytvoření aplikace</span><span class="sxs-lookup"><span data-stu-id="dcdd0-108">Create your application</span></span>
1. <span data-ttu-id="dcdd0-109">V sadě Visual Studio:`File` > `New` > `Project`</span><span class="sxs-lookup"><span data-stu-id="dcdd0-109">In Visual Studio: `File` > `New` > `Project`</span></span><br/>
2. <span data-ttu-id="dcdd0-110">V části *šablony*, vyberte možnost`Visual C#`</span><span class="sxs-lookup"><span data-stu-id="dcdd0-110">Under *Templates*, select `Visual C#`</span></span>
3. <span data-ttu-id="dcdd0-111">Vyberte `WPF App` (nebo *aplikaci WPF* v závislosti na verzi hello sady Visual Studio)</span><span class="sxs-lookup"><span data-stu-id="dcdd0-111">Select `WPF App` (or *WPF Application* depending on hello version of your Visual Studio)</span></span>

## <a name="add-hello-microsoft-authentication-library-msal-tooyour-project"></a><span data-ttu-id="dcdd0-112">Přidání projektu tooyour hello Microsoft ověřování knihovny (MSAL)</span><span class="sxs-lookup"><span data-stu-id="dcdd0-112">Add hello Microsoft Authentication Library (MSAL) tooyour project</span></span>
1. <span data-ttu-id="dcdd0-113">V sadě Visual Studio:`Tools` > `Nuget Package Manager` > `Package Manager Console`</span><span class="sxs-lookup"><span data-stu-id="dcdd0-113">In Visual Studio: `Tools` > `Nuget Package Manager` > `Package Manager Console`</span></span>
2. <span data-ttu-id="dcdd0-114">Zkopírujte a vložte následující hello v hello okna konzoly Správce balíčků:</span><span class="sxs-lookup"><span data-stu-id="dcdd0-114">Copy/paste hello following in hello Package Manager Console window:</span></span>

```powershell
Install-Package Microsoft.Identity.Client -Pre
```

> <span data-ttu-id="dcdd0-115">balíček Hello výše nainstaluje hello Microsoft ověřování knihovny (MSAL).</span><span class="sxs-lookup"><span data-stu-id="dcdd0-115">hello package above installs hello Microsoft Authentication Library (MSAL).</span></span> <span data-ttu-id="dcdd0-116">MSAL zpracovává získávání, ukládání do mezipaměti a aktualizaci tooaccess toskens použít uživatelské rozhraní API, které jsou chráněné službou Azure Active Directory v2.</span><span class="sxs-lookup"><span data-stu-id="dcdd0-116">MSAL handles acquiring, caching and refreshing user toskens used tooaccess APIs protected by Azure Active Directory v2.</span></span>

## <a name="add-hello-code-tooinitialize-msal"></a><span data-ttu-id="dcdd0-117">Přidat hello kódu tooinitialize MSAL</span><span class="sxs-lookup"><span data-stu-id="dcdd0-117">Add hello code tooinitialize MSAL</span></span>
<span data-ttu-id="dcdd0-118">Tento krok vám pomůže vytvořit třída toohandle interakci s MSAL knihovnu, jako je například zpracování tokenů.</span><span class="sxs-lookup"><span data-stu-id="dcdd0-118">This step will help you create a class toohandle interaction with MSAL Library, such as handling of tokens.</span></span>

1. <span data-ttu-id="dcdd0-119">Otevřete hello `App.xaml.cs` souboru a přidat hello odkaz pro třídu toohello MSAL knihovny:</span><span class="sxs-lookup"><span data-stu-id="dcdd0-119">Open hello `App.xaml.cs` file and add hello reference for MSAL library toohello class:</span></span>

```csharp
using Microsoft.Identity.Client;
```
<!-- Workaround for Docs conversion bug -->
<ol start="2">
<li>
<span data-ttu-id="dcdd0-120">Aktualizujte následující toohello – třída aplikace hello:</span><span class="sxs-lookup"><span data-stu-id="dcdd0-120">Update hello App class toohello following:</span></span>
</li>
</ol>

```csharp
public partial class App : Application
{
    //Below is hello clientId of your app registration. 
    //You have tooreplace hello below with hello Application Id for your app registration
    private static string ClientId = "your_client_id_here";

    public static PublicClientApplication PublicClientApp = new PublicClientApplication(ClientId);

}
```

## <a name="create-your-applications-ui"></a><span data-ttu-id="dcdd0-121">Vytvoření uživatelského rozhraní aplikace</span><span class="sxs-lookup"><span data-stu-id="dcdd0-121">Create your application’s UI</span></span>
<span data-ttu-id="dcdd0-122">Hello části níže ukazuje, jak aplikaci můžete dotazovat chráněných back-end serveru, jako je Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="dcdd0-122">hello section below shows how an application can query a protected backend server like Microsoft Graph.</span></span> <span data-ttu-id="dcdd0-123">Soubor MainWindow.xaml by měl automaticky vytvoří jako součást vaše šablona projektu.</span><span class="sxs-lookup"><span data-stu-id="dcdd0-123">A MainWindow.xaml file should automatically be created as a part of your project template.</span></span> <span data-ttu-id="dcdd0-124">Tento soubor otevřít tento soubor a pak postupujte podle pokynů hello níže:</span><span class="sxs-lookup"><span data-stu-id="dcdd0-124">Open this file this file and then follow hello instructions below:</span></span>

<span data-ttu-id="dcdd0-125">Nahraďte aplikace `<Grid>` být hello následující:</span><span class="sxs-lookup"><span data-stu-id="dcdd0-125">Replace your application’s `<Grid>` with be hello following:</span></span>

```xml
<Grid>
    <StackPanel Background="Azure">
        <StackPanel Orientation="Horizontal" HorizontalAlignment="Right">
            <Button x:Name="CallGraphButton" Content="Call Microsoft Graph API" HorizontalAlignment="Right" Padding="5" Click="CallGraphButton_Click" Margin="5" FontFamily="Segoe Ui"/>
            <Button x:Name="SignOutButton" Content="Sign-Out" HorizontalAlignment="Right" Padding="5" Click="SignOutButton_Click" Margin="5" Visibility="Collapsed" FontFamily="Segoe Ui"/>
        </StackPanel>
        <Label Content="API Call Results" Margin="0,0,0,-5" FontFamily="Segoe Ui" />
        <TextBox x:Name="ResultText" TextWrapping="Wrap" MinHeight="120" Margin="5" FontFamily="Segoe Ui"/>
        <Label Content="Token Info" Margin="0,0,0,-5" FontFamily="Segoe Ui" />
        <TextBox x:Name="TokenInfoText" TextWrapping="Wrap" MinHeight="70" Margin="5" FontFamily="Segoe Ui"/>
    </StackPanel>
</Grid>
```
