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
## <a name="set-up-your-project"></a>Nastavení projektu

Tato část obsahuje podrobné pokyny, jak toocreate nový projekt toodemonstrate jak toointegrate Windows Desktop .NET aplikace (XAML) s *přihlášení se společností Microsoft* , se můžete dotazovat webové rozhraní API, která vyžaduje token.

Hello aplikace vytvořené v této příručce zpřístupní tlačítko toograph a zobrazit výsledky na obrazovce a odhlášení tlačítko.

> Dáváte přednost toodownload projektu sady Visual Studio Tato ukázka místo? [Stažení projektu](https://github.com/Azure-Samples/active-directory-dotnet-desktop-msgraph-v2/archive/master.zip) a přeskočit toohello [krok konfigurace](#create-an-application-express) ukázka kódu hello tooconfigure před provedením.


### <a name="create-your-application"></a>Vytvoření aplikace
1. V sadě Visual Studio:`File` > `New` > `Project`<br/>
2. V části *šablony*, vyberte možnost`Visual C#`
3. Vyberte `WPF App` (nebo *aplikaci WPF* v závislosti na verzi hello sady Visual Studio)

## <a name="add-hello-microsoft-authentication-library-msal-tooyour-project"></a>Přidání projektu tooyour hello Microsoft ověřování knihovny (MSAL)
1. V sadě Visual Studio:`Tools` > `Nuget Package Manager` > `Package Manager Console`
2. Zkopírujte a vložte následující hello v hello okna konzoly Správce balíčků:

```powershell
Install-Package Microsoft.Identity.Client -Pre
```

> balíček Hello výše nainstaluje hello Microsoft ověřování knihovny (MSAL). MSAL zpracovává získávání, ukládání do mezipaměti a aktualizaci tooaccess toskens použít uživatelské rozhraní API, které jsou chráněné službou Azure Active Directory v2.

## <a name="add-hello-code-tooinitialize-msal"></a>Přidat hello kódu tooinitialize MSAL
Tento krok vám pomůže vytvořit třída toohandle interakci s MSAL knihovnu, jako je například zpracování tokenů.

1. Otevřete hello `App.xaml.cs` souboru a přidat hello odkaz pro třídu toohello MSAL knihovny:

```csharp
using Microsoft.Identity.Client;
```
<!-- Workaround for Docs conversion bug -->
<ol start="2">
<li>
Aktualizujte následující toohello – třída aplikace hello:
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

## <a name="create-your-applications-ui"></a>Vytvoření uživatelského rozhraní aplikace
Hello části níže ukazuje, jak aplikaci můžete dotazovat chráněných back-end serveru, jako je Microsoft Graph. Soubor MainWindow.xaml by měl automaticky vytvoří jako součást vaše šablona projektu. Tento soubor otevřít tento soubor a pak postupujte podle pokynů hello níže:

Nahraďte aplikace `<Grid>` být hello následující:

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
