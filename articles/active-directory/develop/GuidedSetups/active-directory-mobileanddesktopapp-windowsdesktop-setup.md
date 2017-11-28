---
title: "Azure AD v2 Windows Desktop získávání spuštěno – nastavení | Microsoft Docs"
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
ms.openlocfilehash: 4065727aef04d7969d438c6ef79127bb44568be1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
## <a name="set-up-your-project"></a><span data-ttu-id="34aa4-103">Nastavení projektu</span><span class="sxs-lookup"><span data-stu-id="34aa4-103">Set up your project</span></span>

<span data-ttu-id="34aa4-104">Tato část obsahuje podrobné pokyny pro vytvoření nového projektu do ukazují, jak integrovat aplikace Windows Desktop .NET (XAML) s *přihlášení se společností Microsoft* , se můžete dotazovat webové rozhraní API, která vyžaduje token.</span><span class="sxs-lookup"><span data-stu-id="34aa4-104">This section provides step-by-step instructions for how to create a new project to demonstrate how to integrate a Windows Desktop .NET application (XAML) with *Sign-In with Microsoft* so it can query Web APIs that requires a token.</span></span>

<span data-ttu-id="34aa4-105">Aplikace vytvořené v této příručce zpřístupní tlačítko graf a zobrazit výsledky na obrazovce a odhlášení tlačítko.</span><span class="sxs-lookup"><span data-stu-id="34aa4-105">The application created by this guide exposes a button to graph and show results on screen and a sign-out button.</span></span>

> <span data-ttu-id="34aa4-106">Místo toho stáhněte projekt Visual Studio Tato ukázka dávají přednost?</span><span class="sxs-lookup"><span data-stu-id="34aa4-106">Prefer to download this sample's Visual Studio project instead?</span></span> <span data-ttu-id="34aa4-107">[Stažení projektu](https://github.com/Azure-Samples/active-directory-dotnet-desktop-msgraph-v2/archive/master.zip) a pokračujte [krok konfigurace](#create-an-application-express) před provedením konfigurace ukázka kódu.</span><span class="sxs-lookup"><span data-stu-id="34aa4-107">[Download a project](https://github.com/Azure-Samples/active-directory-dotnet-desktop-msgraph-v2/archive/master.zip) and skip to the [Configuration step](#create-an-application-express) to configure the code sample before executing.</span></span>


### <a name="create-your-application"></a><span data-ttu-id="34aa4-108">Vytvoření aplikace</span><span class="sxs-lookup"><span data-stu-id="34aa4-108">Create your application</span></span>
1. <span data-ttu-id="34aa4-109">V sadě Visual Studio:`File` > `New` > `Project`</span><span class="sxs-lookup"><span data-stu-id="34aa4-109">In Visual Studio: `File` > `New` > `Project`</span></span><br/>
2. <span data-ttu-id="34aa4-110">V části *šablony*, vyberte možnost`Visual C#`</span><span class="sxs-lookup"><span data-stu-id="34aa4-110">Under *Templates*, select `Visual C#`</span></span>
3. <span data-ttu-id="34aa4-111">Vyberte `WPF App` (nebo *aplikaci WPF* v závislosti na verzi sady Visual Studio)</span><span class="sxs-lookup"><span data-stu-id="34aa4-111">Select `WPF App` (or *WPF Application* depending on the version of your Visual Studio)</span></span>

## <a name="add-the-microsoft-authentication-library-msal-to-your-project"></a><span data-ttu-id="34aa4-112">Do projektu přidejte knihovny ověřování společnosti Microsoft (MSAL)</span><span class="sxs-lookup"><span data-stu-id="34aa4-112">Add the Microsoft Authentication Library (MSAL) to your project</span></span>
1. <span data-ttu-id="34aa4-113">V sadě Visual Studio:`Tools` > `Nuget Package Manager` > `Package Manager Console`</span><span class="sxs-lookup"><span data-stu-id="34aa4-113">In Visual Studio: `Tools` > `Nuget Package Manager` > `Package Manager Console`</span></span>
2. <span data-ttu-id="34aa4-114">Zkopírujte a vložte následující v okně konzoly Správce balíčků:</span><span class="sxs-lookup"><span data-stu-id="34aa4-114">Copy/paste the following in the Package Manager Console window:</span></span>

```powershell
Install-Package Microsoft.Identity.Client -Pre
```

> <span data-ttu-id="34aa4-115">Výše uvedené balíček nainstaluje Microsoft ověřování knihovny (MSAL).</span><span class="sxs-lookup"><span data-stu-id="34aa4-115">The package above installs the Microsoft Authentication Library (MSAL).</span></span> <span data-ttu-id="34aa4-116">MSAL zpracovává získávání, ukládání do mezipaměti a aktualizaci toskens uživatel používá pro přístup k rozhraní API, které jsou chráněné službou Azure Active Directory v2.</span><span class="sxs-lookup"><span data-stu-id="34aa4-116">MSAL handles acquiring, caching and refreshing user toskens used to access APIs protected by Azure Active Directory v2.</span></span>

## <a name="add-the-code-to-initialize-msal"></a><span data-ttu-id="34aa4-117">Přidat kód pro inicializaci MSAL</span><span class="sxs-lookup"><span data-stu-id="34aa4-117">Add the code to initialize MSAL</span></span>
<span data-ttu-id="34aa4-118">Tento krok vám pomůže vytvořit třídu pro zpracování interakci s MSAL knihovnu, jako je například zpracování tokenů.</span><span class="sxs-lookup"><span data-stu-id="34aa4-118">This step will help you create a class to handle interaction with MSAL Library, such as handling of tokens.</span></span>

1. <span data-ttu-id="34aa4-119">Otevřete `App.xaml.cs` souboru a odkaz na MSAL knihovny přidejte do třídy:</span><span class="sxs-lookup"><span data-stu-id="34aa4-119">Open the `App.xaml.cs` file and add the reference for MSAL library to the class:</span></span>

```csharp
using Microsoft.Identity.Client;
```
<!-- Workaround for Docs conversion bug -->
<ol start="2">
<li>
<span data-ttu-id="34aa4-120">Aktualizace třídy aplikace takto:</span><span class="sxs-lookup"><span data-stu-id="34aa4-120">Update the App class to the following:</span></span>
</li>
</ol>

```csharp
public partial class App : Application
{
    //Below is the clientId of your app registration. 
    //You have to replace the below with the Application Id for your app registration
    private static string ClientId = "your_client_id_here";

    public static PublicClientApplication PublicClientApp = new PublicClientApplication(ClientId);

}
```

## <a name="create-your-applications-ui"></a><span data-ttu-id="34aa4-121">Vytvoření uživatelského rozhraní aplikace</span><span class="sxs-lookup"><span data-stu-id="34aa4-121">Create your application’s UI</span></span>
<span data-ttu-id="34aa4-122">Následující části ukazuje, jak aplikaci můžete dotazovat chráněných back-end serveru, jako je Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="34aa4-122">The section below shows how an application can query a protected backend server like Microsoft Graph.</span></span> <span data-ttu-id="34aa4-123">Soubor MainWindow.xaml by měl automaticky vytvoří jako součást vaše šablona projektu.</span><span class="sxs-lookup"><span data-stu-id="34aa4-123">A MainWindow.xaml file should automatically be created as a part of your project template.</span></span> <span data-ttu-id="34aa4-124">Tento soubor otevřít tento soubor a pak postupujte podle pokynů níže:</span><span class="sxs-lookup"><span data-stu-id="34aa4-124">Open this file this file and then follow the instructions below:</span></span>

<span data-ttu-id="34aa4-125">Nahraďte aplikace `<Grid>` být následující:</span><span class="sxs-lookup"><span data-stu-id="34aa4-125">Replace your application’s `<Grid>` with be the following:</span></span>

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
