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
# <a name="how-toouse-blob-storage-from-xamarin"></a><span data-ttu-id="874cf-104">Jak toouse úložiště Blob z Xamarin</span><span class="sxs-lookup"><span data-stu-id="874cf-104">How toouse Blob Storage from Xamarin</span></span>
[!INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

## <a name="overview"></a><span data-ttu-id="874cf-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="874cf-105">Overview</span></span>
<span data-ttu-id="874cf-106">Xamarin umožňuje vývojářům toouse sdílené C# codebase toocreate iOS, Android a aplikací pro Windows Store s jejich nativní uživatelská rozhraní.</span><span class="sxs-lookup"><span data-stu-id="874cf-106">Xamarin enables developers toouse a shared C# codebase toocreate iOS, Android, and Windows Store apps with their native user interfaces.</span></span> <span data-ttu-id="874cf-107">Tento kurz ukazuje, jak toouse úložiště objektů Blob v Azure pomocí aplikace Xamarin.</span><span class="sxs-lookup"><span data-stu-id="874cf-107">This tutorial shows you how toouse Azure Blob storage with a Xamarin application.</span></span> <span data-ttu-id="874cf-108">Pokud chcete toolearn Další informace o Azure Storage, než začnete hello kód, najdete v části [tooMicrosoft Úvod Azure Storage](storage-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="874cf-108">If you'd like toolearn more about Azure Storage, before diving into hello code, see [Introduction tooMicrosoft Azure Storage](storage-introduction.md).</span></span>

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[!INCLUDE [storage-mobile-authentication-guidance](../../includes/storage-mobile-authentication-guidance.md)]

## <a name="create-a-new-xamarin-application"></a><span data-ttu-id="874cf-109">Vytvoření nové aplikace Xamarin</span><span class="sxs-lookup"><span data-stu-id="874cf-109">Create a new Xamarin Application</span></span>
<span data-ttu-id="874cf-110">V tomto kurzu jsme budete vytvářet aplikace, která je cílena, Android, iOS a Windows.</span><span class="sxs-lookup"><span data-stu-id="874cf-110">For this tutorial, we'll be creating an app that targets Android, iOS, and Windows.</span></span> <span data-ttu-id="874cf-111">Tato aplikace bude jednoduše vytvořit kontejner a nahrát objekt blob do tohoto kontejneru.</span><span class="sxs-lookup"><span data-stu-id="874cf-111">This app will simply create a container and upload a blob into this container.</span></span> <span data-ttu-id="874cf-112">Budeme v systému Windows, ale hello stejné learnings lze použít při vytváření aplikace pomocí Xamarin Studio v systému macOS používat Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="874cf-112">We'll be using Visual Studio on Windows, but hello same learnings can be applied when creating an app using Xamarin Studio on macOS.</span></span>

<span data-ttu-id="874cf-113">Postupujte podle těchto kroků toocreate aplikace:</span><span class="sxs-lookup"><span data-stu-id="874cf-113">Follow these steps toocreate your application:</span></span>

1. <span data-ttu-id="874cf-114">Pokud jste tak ještě neučinili, stáhněte a nainstalujte [Xamarin pro Visual Studio](https://www.xamarin.com/download).</span><span class="sxs-lookup"><span data-stu-id="874cf-114">If you haven't already, download and install [Xamarin for Visual Studio](https://www.xamarin.com/download).</span></span>
2. <span data-ttu-id="874cf-115">Otevřete Visual Studio a vytvořit prázdnou aplikaci (nativní přenosné): **soubor > Nový > Projekt > napříč platformami > prázdné App(Native Portable)**.</span><span class="sxs-lookup"><span data-stu-id="874cf-115">Open Visual Studio, and create a Blank App (Native Portable): **File > New > Project > Cross-Platform > Blank App(Native Portable)**.</span></span>
3. <span data-ttu-id="874cf-116">Klikněte pravým tlačítkem na řešení v podokně Průzkumník řešení hello a vyberte **spravovat balíčky NuGet pro řešení**.</span><span class="sxs-lookup"><span data-stu-id="874cf-116">Right-click your solution in hello Solution Explorer pane and select **Manage NuGet Packages for Solution**.</span></span> <span data-ttu-id="874cf-117">Vyhledejte **WindowsAzure.Storage** a nainstalujte hello nejnovější stabilní verze tooall projekty v řešení.</span><span class="sxs-lookup"><span data-stu-id="874cf-117">Search for **WindowsAzure.Storage** and install hello latest stable version tooall projects in your solution.</span></span>
4. <span data-ttu-id="874cf-118">Sestavte a spusťte projekt.</span><span class="sxs-lookup"><span data-stu-id="874cf-118">Build and run your project.</span></span>

<span data-ttu-id="874cf-119">Teď byste měli mít aplikaci, která vám umožní tooclick tlačítko, které zvýší čítače.</span><span class="sxs-lookup"><span data-stu-id="874cf-119">You should now have an application that allows you tooclick a button which increments a counter.</span></span>

## <a name="create-container-and-upload-blob"></a><span data-ttu-id="874cf-120">Vytvoření kontejneru a nahrát objekt blob</span><span class="sxs-lookup"><span data-stu-id="874cf-120">Create container and upload blob</span></span>
<span data-ttu-id="874cf-121">Části vašeho `(Portable)` projektu přidáte nějaký kód příliš`MyClass.cs`.</span><span class="sxs-lookup"><span data-stu-id="874cf-121">Next, under your `(Portable)` project, you'll add some code too`MyClass.cs`.</span></span> <span data-ttu-id="874cf-122">Tento kód vytvoří kontejner a odešle objekt blob do tohoto kontejneru.</span><span class="sxs-lookup"><span data-stu-id="874cf-122">This code creates a container and uploads a blob into this container.</span></span> <span data-ttu-id="874cf-123">`MyClass.cs`by mělo vypadat jako následující hello:</span><span class="sxs-lookup"><span data-stu-id="874cf-123">`MyClass.cs` should look like hello following:</span></span>

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

<span data-ttu-id="874cf-124">Ujistěte se, že tooreplace "your_account_name_here" a "your_account_key_here" s název skutečné účtu a klíč účtu.</span><span class="sxs-lookup"><span data-stu-id="874cf-124">Make sure tooreplace "your_account_name_here" and "your_account_key_here" with your actual account name and account key.</span></span> 

<span data-ttu-id="874cf-125">Vaše iOS, Android a Windows Phone projekty všechny přenosné projekt tooyour odkazy – což znamená, že všechny sdílené kódu můžete napsat v jednom umístění a použít napříč všemi svými projekty.</span><span class="sxs-lookup"><span data-stu-id="874cf-125">Your iOS, Android, and Windows Phone projects all have references tooyour Portable project - meaning you can write all of your shared code in one place and use it across all of your projects.</span></span> <span data-ttu-id="874cf-126">Nyní můžete přidat následující řádek kódu tooeach projektu toostart díky hello:`MyClass.performBlobOperation()`</span><span class="sxs-lookup"><span data-stu-id="874cf-126">You can now add hello following line of code tooeach project toostart taking advantage: `MyClass.performBlobOperation()`</span></span>

### <a name="xamarinappdroid--mainactivitycs"></a><span data-ttu-id="874cf-127">XamarinApp.Droid > MainActivity.cs</span><span class="sxs-lookup"><span data-stu-id="874cf-127">XamarinApp.Droid > MainActivity.cs</span></span>

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

### <a name="xamarinappios--viewcontrollercs"></a><span data-ttu-id="874cf-128">XamarinApp.iOS > ViewController.cs</span><span class="sxs-lookup"><span data-stu-id="874cf-128">XamarinApp.iOS > ViewController.cs</span></span>

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

### <a name="xamarinappwinphone--mainpagexaml--mainpagexamlcs"></a><span data-ttu-id="874cf-129">XamarinApp.WinPhone > MainPage.xaml > MainPage.xaml.cs</span><span class="sxs-lookup"><span data-stu-id="874cf-129">XamarinApp.WinPhone > MainPage.xaml > MainPage.xaml.cs</span></span>

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

## <a name="run-hello-application"></a><span data-ttu-id="874cf-130">Spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="874cf-130">Run hello application</span></span>
<span data-ttu-id="874cf-131">Teď můžete spustit tuto aplikaci v emulátoru Android nebo Windows Phone.</span><span class="sxs-lookup"><span data-stu-id="874cf-131">You can now run this application in an Android or Windows Phone emulator.</span></span> <span data-ttu-id="874cf-132">Můžete taky spustit tuto aplikaci v emulátoru iOS, ale to bude vyžadovat macu.</span><span class="sxs-lookup"><span data-stu-id="874cf-132">You can also run this application in an iOS emulator, but this will require a Mac.</span></span> <span data-ttu-id="874cf-133">Pro konkrétní pokyny, jak toodo, přečtěte si prosím dokumentaci hello [připojení Visual Studio tooa Mac](https://developer.xamarin.com/guides/ios/getting_started/installation/windows/connecting-to-mac/)</span><span class="sxs-lookup"><span data-stu-id="874cf-133">For specific instructions on how toodo this, please read hello documentation for [connecting Visual Studio tooa Mac](https://developer.xamarin.com/guides/ios/getting_started/installation/windows/connecting-to-mac/)</span></span>

<span data-ttu-id="874cf-134">Po spuštění aplikace, vytvoří kontejner hello `mycontainer` ve vašem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="874cf-134">Once you run your app, it will create hello container `mycontainer` in your Storage account.</span></span> <span data-ttu-id="874cf-135">Měl by obsahovat hello blob `myblob`, který má hello text, `Hello, world!`.</span><span class="sxs-lookup"><span data-stu-id="874cf-135">It should contain hello blob, `myblob`, which has hello text, `Hello, world!`.</span></span> <span data-ttu-id="874cf-136">Můžete to ověřit pomocí hello [Microsoft Azure Storage Explorer](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="874cf-136">You can verify this by using hello [Microsoft Azure Storage Explorer](http://storageexplorer.com/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="874cf-137">Další kroky</span><span class="sxs-lookup"><span data-stu-id="874cf-137">Next steps</span></span>
<span data-ttu-id="874cf-138">V tomto kurzu jste se dozvěděli, jak toocreate aplikace různé platformy v Xamarinu používající Azure Storage, konkrétně zaměřené na jeden scénář v úložišti objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="874cf-138">In this tutorial, you learned how toocreate a cross-platform application in Xamarin that uses Azure Storage, specifically focusing on one scenario in Blob Storage.</span></span> <span data-ttu-id="874cf-139">Však můžete provést mnohem víc s ne jenom úložiště objektů Blob, ale také pomocí tabulky, souboru a Queue Storage.</span><span class="sxs-lookup"><span data-stu-id="874cf-139">However, you can do a lot more with not only Blob Storage, but also with Table, File, and Queue Storage.</span></span> <span data-ttu-id="874cf-140">Podrobnosti naleznete v následujících článků toolearn další hello:</span><span class="sxs-lookup"><span data-stu-id="874cf-140">Please check out hello following articles toolearn more:</span></span>

* [<span data-ttu-id="874cf-141">Začínáme s úložištěm Azure Blob pomocí rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="874cf-141">Get started with Azure Blob storage using .NET</span></span>](storage-dotnet-how-to-use-blobs.md)
* [<span data-ttu-id="874cf-142">Začínáme s úložištěm Azure Table pomocí rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="874cf-142">Get started with Azure Table storage using .NET</span></span>](storage-dotnet-how-to-use-tables.md)
* [<span data-ttu-id="874cf-143">Začínáme s úložištěm Azure Queue pomocí rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="874cf-143">Get started with Azure Queue storage using .NET</span></span>](storage-dotnet-how-to-use-queues.md)
* [<span data-ttu-id="874cf-144">Začínáme s úložištěm Azure File ve Windows</span><span class="sxs-lookup"><span data-stu-id="874cf-144">Get started with Azure File storage on Windows</span></span>](storage-dotnet-how-to-use-files.md)

[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

