---
title: "aaaHow toouse úložiště Blob z Xamarin | Microsoft Docs"
description: "Hello Klientská knihovna pro úložiště Azure pro Xamarin umožňuje vývojářům toocreate iOS, Android a aplikací pro Windows Store s jejich nativní uživatelská rozhraní. Tento kurz ukazuje, jak toouse Xamarin toocreate aplikaci, která používá úložiště objektů Blob v Azure."
services: storage
documentationcenter: xamarin
author: michaelhauss
manager: vamshik
editor: tysonn
ms.assetid: 44cb845d-cf78-4942-95b8-952da4f9a2c2
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/11/2017
ms.author: michaelhauss
ms.openlocfilehash: 751f66d1d2392c8bcf6e5f8d1b185af73582fab3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-blob-storage-from-xamarin"></a>Jak toouse úložiště Blob z Xamarin
[!INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

## <a name="overview"></a>Přehled
Xamarin umožňuje vývojářům toouse sdílené C# codebase toocreate iOS, Android a aplikací pro Windows Store s jejich nativní uživatelská rozhraní. Tento kurz ukazuje, jak toouse úložiště objektů Blob v Azure pomocí aplikace Xamarin. Pokud chcete toolearn Další informace o Azure Storage, než začnete hello kód, najdete v části [tooMicrosoft Úvod Azure Storage](storage-introduction.md).

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[!INCLUDE [storage-mobile-authentication-guidance](../../includes/storage-mobile-authentication-guidance.md)]

## <a name="create-a-new-xamarin-application"></a>Vytvoření nové aplikace Xamarin
V tomto kurzu jsme budete vytvářet aplikace, která je cílena, Android, iOS a Windows. Tato aplikace bude jednoduše vytvořit kontejner a nahrát objekt blob do tohoto kontejneru. Budeme v systému Windows, ale hello stejné learnings lze použít při vytváření aplikace pomocí Xamarin Studio v systému macOS používat Visual Studio.

Postupujte podle těchto kroků toocreate aplikace:

1. Pokud jste tak ještě neučinili, stáhněte a nainstalujte [Xamarin pro Visual Studio](https://www.xamarin.com/download).
2. Otevřete Visual Studio a vytvořit prázdnou aplikaci (nativní přenosné): **soubor > Nový > Projekt > napříč platformami > prázdné App(Native Portable)**.
3. Klikněte pravým tlačítkem na řešení v podokně Průzkumník řešení hello a vyberte **spravovat balíčky NuGet pro řešení**. Vyhledejte **WindowsAzure.Storage** a nainstalujte hello nejnovější stabilní verze tooall projekty v řešení.
4. Sestavte a spusťte projekt.

Teď byste měli mít aplikaci, která vám umožní tooclick tlačítko, které zvýší čítače.

## <a name="create-container-and-upload-blob"></a>Vytvoření kontejneru a nahrát objekt blob
Části vašeho `(Portable)` projektu přidáte nějaký kód příliš`MyClass.cs`. Tento kód vytvoří kontejner a odešle objekt blob do tohoto kontejneru. `MyClass.cs`by mělo vypadat jako následující hello:

```csharp
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Blob;
using System.Threading.Tasks;

namespace XamarinApp
{
    public class MyClass
    {
        public MyClass ()
        {
        }

        public static async Task performBlobOperation()
        {
            // Retrieve storage account from connection string.
            CloudStorageAccount storageAccount = CloudStorageAccount.Parse("DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here");

            // Create hello blob client.
            CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

            // Retrieve reference tooa previously created container.
            CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

            // Create hello container if it doesn't already exist.
            await container.CreateIfNotExistsAsync();

            // Retrieve reference tooa blob named "myblob".
            CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

            // Create hello "myblob" blob with hello text "Hello, world!"
            await blockBlob.UploadTextAsync("Hello, world!");
        }
    }
}
```

Ujistěte se, že tooreplace "your_account_name_here" a "your_account_key_here" s název skutečné účtu a klíč účtu. 

Vaše iOS, Android a Windows Phone projekty všechny přenosné projekt tooyour odkazy – což znamená, že všechny sdílené kódu můžete napsat v jednom umístění a použít napříč všemi svými projekty. Nyní můžete přidat následující řádek kódu tooeach projektu toostart díky hello:`MyClass.performBlobOperation()`

### <a name="xamarinappdroid--mainactivitycs"></a>XamarinApp.Droid > MainActivity.cs

```csharp
using Android.App;
using Android.Widget;
using Android.OS;

namespace XamarinApp.Droid
{
    [Activity (Label = "XamarinApp.Droid", MainLauncher = true, Icon = "@drawable/icon")]
    public class MainActivity : Activity
    {
        int count = 1;

        protected override async void OnCreate (Bundle bundle)
        {
            base.OnCreate (bundle);

            // Set our view from hello "main" layout resource
            SetContentView (Resource.Layout.Main);

            // Get our button from hello layout resource,
            // and attach an event tooit
            Button button = FindViewById<Button> (Resource.Id.myButton);

            button.Click += delegate {
                button.Text = string.Format ("{0} clicks!", count++);
            };

            await MyClass.performBlobOperation();
            }
        }
    }
}
```

### <a name="xamarinappios--viewcontrollercs"></a>XamarinApp.iOS > ViewController.cs

```csharp
using System;
using UIKit;

namespace XamarinApp.iOS
{
    public partial class ViewController : UIViewController
    {
        int count = 1;

        public ViewController (IntPtr handle) : base (handle)
        {
        }

        public override async void ViewDidLoad ()
        {
            int count = 1;

            public ViewController (IntPtr handle) : base (handle)
            {
            }

            public override async void ViewDidLoad ()
            {
                base.ViewDidLoad ();
                // Perform any additional setup after loading hello view, typically from a nib.
                Button.AccessibilityIdentifier = "myButton";
                Button.TouchUpInside += delegate {
                    var title = string.Format ("{0} clicks!", count++);
                    Button.SetTitle (title, UIControlState.Normal);
                };

                await MyClass.performBlobOperation();
            }

            public override void DidReceiveMemoryWarning ()
            {
                base.DidReceiveMemoryWarning ();
                // Release any cached data, images, etc that aren't in use.
            }
        }
    }
}
```

### <a name="xamarinappwinphone--mainpagexaml--mainpagexamlcs"></a>XamarinApp.WinPhone > MainPage.xaml > MainPage.xaml.cs

```csharp
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Navigation;

// hello Blank Page item template is documented at http://go.microsoft.com/fwlink/?LinkId=391641

namespace XamarinApp.WinPhone
{
    /// <summary>
    /// An empty page that can be used on its own or navigated toowithin a Frame.
    /// </summary>
    public sealed partial class MainPage : Page
    {
        int count = 1;

        public MainPage()
        {
            this.InitializeComponent();

            this.NavigationCacheMode = NavigationCacheMode.Required;
        }

        /// <summary>
        /// Invoked when this page is about toobe displayed in a Frame.
        /// </summary>
        /// <param name="e">Event data that describes how this page was reached.
        /// This parameter is typically used tooconfigure hello page.</param>
        protected override async void OnNavigatedTo(NavigationEventArgs e)
        {
            int count = 1;

            public MainPage()
            {
                this.InitializeComponent();

                this.NavigationCacheMode = NavigationCacheMode.Required;
            }

            /// <summary>
            /// Invoked when this page is about toobe displayed in a Frame.
            /// </summary>
            /// <param name="e">Event data that describes how this page was reached.
            /// This parameter is typically used tooconfigure hello page.</param>
            protected override async void OnNavigatedTo(NavigationEventArgs e)
            {
                // TODO: Prepare page for display here.

                // TODO: If your application contains multiple pages, ensure that you are
                // handling hello hardware Back button by registering for the
                // Windows.Phone.UI.Input.HardwareButtons.BackPressed event.
                // If you are using hello NavigationHelper provided by some templates,
                // this event is handled for you.
                Button.Click += delegate {
                    var title = string.Format("{0} clicks!", count++);
                    Button.Content = title;
                };

                await MyClass.performBlobOperation();
            }
        }
    }
}
```

## <a name="run-hello-application"></a>Spuštění aplikace hello
Teď můžete spustit tuto aplikaci v emulátoru Android nebo Windows Phone. Můžete taky spustit tuto aplikaci v emulátoru iOS, ale to bude vyžadovat macu. Pro konkrétní pokyny, jak toodo, přečtěte si prosím dokumentaci hello [připojení Visual Studio tooa Mac](https://developer.xamarin.com/guides/ios/getting_started/installation/windows/connecting-to-mac/)

Po spuštění aplikace, vytvoří kontejner hello `mycontainer` ve vašem účtu úložiště. Měl by obsahovat hello blob `myblob`, který má hello text, `Hello, world!`. Můžete to ověřit pomocí hello [Microsoft Azure Storage Explorer](http://storageexplorer.com/).

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste se dozvěděli, jak toocreate aplikace různé platformy v Xamarinu používající Azure Storage, konkrétně zaměřené na jeden scénář v úložišti objektů Blob. Však můžete provést mnohem víc s ne jenom úložiště objektů Blob, ale také pomocí tabulky, souboru a Queue Storage. Podrobnosti naleznete v následujících článků toolearn další hello:

* [Začínáme s úložištěm Azure Blob pomocí rozhraní .NET](storage-dotnet-how-to-use-blobs.md)
* [Začínáme s úložištěm Azure Table pomocí rozhraní .NET](storage-dotnet-how-to-use-tables.md)
* [Začínáme s úložištěm Azure Queue pomocí rozhraní .NET](storage-dotnet-how-to-use-queues.md)
* [Začínáme s úložištěm Azure File ve Windows](storage-dotnet-how-to-use-files.md)

[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

