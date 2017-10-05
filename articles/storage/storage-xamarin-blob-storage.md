---
title: "Používání úložiště Blob z Xamarin | Microsoft Docs"
description: "Klientská knihovna pro úložiště Azure pro Xamarin umožňuje vývojářům vytvářet iOS, Android a aplikací pro Windows Store s jejich nativní uživatelská rozhraní. Tento kurz ukazuje, jak vytvořit aplikaci, která používá Azure Blob storage pomocí Xamarin."
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
ms.openlocfilehash: 0952c0e7189dd360098744a7e19b04cd330cb617
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-use-blob-storage-from-xamarin"></a><span data-ttu-id="9abf9-104">Používání úložiště Blob z Xamarin</span><span class="sxs-lookup"><span data-stu-id="9abf9-104">How to use Blob Storage from Xamarin</span></span>
[!INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

## <a name="overview"></a><span data-ttu-id="9abf9-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="9abf9-105">Overview</span></span>
<span data-ttu-id="9abf9-106">Codebase – Xamarin umožňuje vývojářům používat sdílené C# k vytvoření iOS, Android a aplikací pro Windows Store s jejich nativní uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="9abf9-106">Xamarin enables developers to use a shared C# codebase to create iOS, Android, and Windows Store apps with their native user interfaces.</span></span> <span data-ttu-id="9abf9-107">V tomto kurzu se dozvíte, jak používat úložiště objektů Blob Azure pomocí aplikace Xamarin.</span><span class="sxs-lookup"><span data-stu-id="9abf9-107">This tutorial shows you how to use Azure Blob storage with a Xamarin application.</span></span> <span data-ttu-id="9abf9-108">Pokud vás zajímají další informace o službě Azure Storage, než začnete kód, najdete [Úvod do Microsoft Azure Storage](storage-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="9abf9-108">If you'd like to learn more about Azure Storage, before diving into the code, see [Introduction to Microsoft Azure Storage](storage-introduction.md).</span></span>

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[!INCLUDE [storage-mobile-authentication-guidance](../../includes/storage-mobile-authentication-guidance.md)]

## <a name="create-a-new-xamarin-application"></a><span data-ttu-id="9abf9-109">Vytvoření nové aplikace Xamarin</span><span class="sxs-lookup"><span data-stu-id="9abf9-109">Create a new Xamarin Application</span></span>
<span data-ttu-id="9abf9-110">V tomto kurzu jsme budete vytvářet aplikace, která je cílena, Android, iOS a Windows.</span><span class="sxs-lookup"><span data-stu-id="9abf9-110">For this tutorial, we'll be creating an app that targets Android, iOS, and Windows.</span></span> <span data-ttu-id="9abf9-111">Tato aplikace bude jednoduše vytvořit kontejner a nahrát objekt blob do tohoto kontejneru.</span><span class="sxs-lookup"><span data-stu-id="9abf9-111">This app will simply create a container and upload a blob into this container.</span></span> <span data-ttu-id="9abf9-112">Budeme používat Visual Studio v systému Windows, ale stejné learnings lze použít při vytváření aplikace pomocí Xamarin Studio v systému macOS.</span><span class="sxs-lookup"><span data-stu-id="9abf9-112">We'll be using Visual Studio on Windows, but the same learnings can be applied when creating an app using Xamarin Studio on macOS.</span></span>

<span data-ttu-id="9abf9-113">Postupujte podle těchto kroků můžete vytvořit aplikaci:</span><span class="sxs-lookup"><span data-stu-id="9abf9-113">Follow these steps to create your application:</span></span>

1. <span data-ttu-id="9abf9-114">Pokud jste tak ještě neučinili, stáhněte a nainstalujte [Xamarin pro Visual Studio](https://www.xamarin.com/download).</span><span class="sxs-lookup"><span data-stu-id="9abf9-114">If you haven't already, download and install [Xamarin for Visual Studio](https://www.xamarin.com/download).</span></span>
2. <span data-ttu-id="9abf9-115">Otevřete Visual Studio a vytvořit prázdnou aplikaci (nativní přenosné): **soubor > Nový > Projekt > napříč platformami > prázdné App(Native Portable)**.</span><span class="sxs-lookup"><span data-stu-id="9abf9-115">Open Visual Studio, and create a Blank App (Native Portable): **File > New > Project > Cross-Platform > Blank App(Native Portable)**.</span></span>
3. <span data-ttu-id="9abf9-116">Klikněte pravým tlačítkem na řešení v podokně Průzkumník řešení a vyberte **spravovat balíčky NuGet pro řešení**.</span><span class="sxs-lookup"><span data-stu-id="9abf9-116">Right-click your solution in the Solution Explorer pane and select **Manage NuGet Packages for Solution**.</span></span> <span data-ttu-id="9abf9-117">Vyhledejte **WindowsAzure.Storage** a nainstalujte nejnovější stabilní verze na všechny projekty v řešení.</span><span class="sxs-lookup"><span data-stu-id="9abf9-117">Search for **WindowsAzure.Storage** and install the latest stable version to all projects in your solution.</span></span>
4. <span data-ttu-id="9abf9-118">Sestavte a spusťte projekt.</span><span class="sxs-lookup"><span data-stu-id="9abf9-118">Build and run your project.</span></span>

<span data-ttu-id="9abf9-119">Teď byste měli mít aplikaci, která umožňuje klikněte na tlačítko, které zvýší čítače.</span><span class="sxs-lookup"><span data-stu-id="9abf9-119">You should now have an application that allows you to click a button which increments a counter.</span></span>

## <a name="create-container-and-upload-blob"></a><span data-ttu-id="9abf9-120">Vytvoření kontejneru a nahrát objekt blob</span><span class="sxs-lookup"><span data-stu-id="9abf9-120">Create container and upload blob</span></span>
<span data-ttu-id="9abf9-121">Části vašeho `(Portable)` projektu přidáte kód, který `MyClass.cs`.</span><span class="sxs-lookup"><span data-stu-id="9abf9-121">Next, under your `(Portable)` project, you'll add some code to `MyClass.cs`.</span></span> <span data-ttu-id="9abf9-122">Tento kód vytvoří kontejner a odešle objekt blob do tohoto kontejneru.</span><span class="sxs-lookup"><span data-stu-id="9abf9-122">This code creates a container and uploads a blob into this container.</span></span> <span data-ttu-id="9abf9-123">`MyClass.cs`by měl vypadat asi takto:</span><span class="sxs-lookup"><span data-stu-id="9abf9-123">`MyClass.cs` should look like the following:</span></span>

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

            // Create the blob client.
            CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

            // Retrieve reference to a previously created container.
            CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

            // Create the container if it doesn't already exist.
            await container.CreateIfNotExistsAsync();

            // Retrieve reference to a blob named "myblob".
            CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

            // Create the "myblob" blob with the text "Hello, world!"
            await blockBlob.UploadTextAsync("Hello, world!");
        }
    }
}
```

<span data-ttu-id="9abf9-124">Ujistěte se, že "your_account_name_here" a "your_account_key_here" nahraďte skutečný název svého účtu a klíč účtu.</span><span class="sxs-lookup"><span data-stu-id="9abf9-124">Make sure to replace "your_account_name_here" and "your_account_key_here" with your actual account name and account key.</span></span> 

<span data-ttu-id="9abf9-125">Systémem iOS, Android a Windows Phone, které mají projekty všechny odkazy na přenosné projektu – což znamená, že všechny sdílené kódu můžete napsat v jednom umístění a použít napříč všemi svými projekty.</span><span class="sxs-lookup"><span data-stu-id="9abf9-125">Your iOS, Android, and Windows Phone projects all have references to your Portable project - meaning you can write all of your shared code in one place and use it across all of your projects.</span></span> <span data-ttu-id="9abf9-126">Pro každý projekt, chcete-li začít využívat můžete nyní přidejte následující řádek kódu:`MyClass.performBlobOperation()`</span><span class="sxs-lookup"><span data-stu-id="9abf9-126">You can now add the following line of code to each project to start taking advantage: `MyClass.performBlobOperation()`</span></span>

### <a name="xamarinappdroid--mainactivitycs"></a><span data-ttu-id="9abf9-127">XamarinApp.Droid > MainActivity.cs</span><span class="sxs-lookup"><span data-stu-id="9abf9-127">XamarinApp.Droid > MainActivity.cs</span></span>

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

            // Set our view from the "main" layout resource
            SetContentView (Resource.Layout.Main);

            // Get our button from the layout resource,
            // and attach an event to it
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

### <a name="xamarinappios--viewcontrollercs"></a><span data-ttu-id="9abf9-128">XamarinApp.iOS > ViewController.cs</span><span class="sxs-lookup"><span data-stu-id="9abf9-128">XamarinApp.iOS > ViewController.cs</span></span>

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
                // Perform any additional setup after loading the view, typically from a nib.
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

### <a name="xamarinappwinphone--mainpagexaml--mainpagexamlcs"></a><span data-ttu-id="9abf9-129">XamarinApp.WinPhone > MainPage.xaml > MainPage.xaml.cs</span><span class="sxs-lookup"><span data-stu-id="9abf9-129">XamarinApp.WinPhone > MainPage.xaml > MainPage.xaml.cs</span></span>

```csharp
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Navigation;

// The Blank Page item template is documented at http://go.microsoft.com/fwlink/?LinkId=391641

namespace XamarinApp.WinPhone
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
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
        /// Invoked when this page is about to be displayed in a Frame.
        /// </summary>
        /// <param name="e">Event data that describes how this page was reached.
        /// This parameter is typically used to configure the page.</param>
        protected override async void OnNavigatedTo(NavigationEventArgs e)
        {
            int count = 1;

            public MainPage()
            {
                this.InitializeComponent();

                this.NavigationCacheMode = NavigationCacheMode.Required;
            }

            /// <summary>
            /// Invoked when this page is about to be displayed in a Frame.
            /// </summary>
            /// <param name="e">Event data that describes how this page was reached.
            /// This parameter is typically used to configure the page.</param>
            protected override async void OnNavigatedTo(NavigationEventArgs e)
            {
                // TODO: Prepare page for display here.

                // TODO: If your application contains multiple pages, ensure that you are
                // handling the hardware Back button by registering for the
                // Windows.Phone.UI.Input.HardwareButtons.BackPressed event.
                // If you are using the NavigationHelper provided by some templates,
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

## <a name="run-the-application"></a><span data-ttu-id="9abf9-130">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="9abf9-130">Run the application</span></span>
<span data-ttu-id="9abf9-131">Teď můžete spustit tuto aplikaci v emulátoru Android nebo Windows Phone.</span><span class="sxs-lookup"><span data-stu-id="9abf9-131">You can now run this application in an Android or Windows Phone emulator.</span></span> <span data-ttu-id="9abf9-132">Můžete taky spustit tuto aplikaci v emulátoru iOS, ale to bude vyžadovat macu.</span><span class="sxs-lookup"><span data-stu-id="9abf9-132">You can also run this application in an iOS emulator, but this will require a Mac.</span></span> <span data-ttu-id="9abf9-133">Pro konkrétní pokyny o tom, jak to provést, přečtěte si dokumentaci pro [připojení Visual Studio pro Mac](https://developer.xamarin.com/guides/ios/getting_started/installation/windows/connecting-to-mac/)</span><span class="sxs-lookup"><span data-stu-id="9abf9-133">For specific instructions on how to do this, please read the documentation for [connecting Visual Studio to a Mac](https://developer.xamarin.com/guides/ios/getting_started/installation/windows/connecting-to-mac/)</span></span>

<span data-ttu-id="9abf9-134">Po spuštění aplikace, vytvoří kontejner `mycontainer` ve vašem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="9abf9-134">Once you run your app, it will create the container `mycontainer` in your Storage account.</span></span> <span data-ttu-id="9abf9-135">Objekt blob, měl by obsahovat `myblob`, text, na kterém je `Hello, world!`.</span><span class="sxs-lookup"><span data-stu-id="9abf9-135">It should contain the blob, `myblob`, which has the text, `Hello, world!`.</span></span> <span data-ttu-id="9abf9-136">Můžete to ověřit pomocí [Microsoft Azure Storage Explorer](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="9abf9-136">You can verify this by using the [Microsoft Azure Storage Explorer](http://storageexplorer.com/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="9abf9-137">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9abf9-137">Next steps</span></span>
<span data-ttu-id="9abf9-138">V tomto kurzu jste zjistili, jak vytvořit aplikaci různé platformy v Xamarinu, která používá Azure Storage, konkrétně zaměřené na jeden scénář v úložišti objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="9abf9-138">In this tutorial, you learned how to create a cross-platform application in Xamarin that uses Azure Storage, specifically focusing on one scenario in Blob Storage.</span></span> <span data-ttu-id="9abf9-139">Však můžete provést mnohem víc s ne jenom úložiště objektů Blob, ale také pomocí tabulky, souboru a Queue Storage.</span><span class="sxs-lookup"><span data-stu-id="9abf9-139">However, you can do a lot more with not only Blob Storage, but also with Table, File, and Queue Storage.</span></span> <span data-ttu-id="9abf9-140">Podrobnosti naleznete další informace v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="9abf9-140">Please check out the following articles to learn more:</span></span>

* [<span data-ttu-id="9abf9-141">Začínáme s úložištěm Azure Blob pomocí rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="9abf9-141">Get started with Azure Blob storage using .NET</span></span>](storage-dotnet-how-to-use-blobs.md)
* [<span data-ttu-id="9abf9-142">Začínáme s úložištěm Azure Table pomocí rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="9abf9-142">Get started with Azure Table storage using .NET</span></span>](storage-dotnet-how-to-use-tables.md)
* [<span data-ttu-id="9abf9-143">Začínáme s úložištěm Azure Queue pomocí rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="9abf9-143">Get started with Azure Queue storage using .NET</span></span>](storage-dotnet-how-to-use-queues.md)
* [<span data-ttu-id="9abf9-144">Začínáme s úložištěm Azure File ve Windows</span><span class="sxs-lookup"><span data-stu-id="9abf9-144">Get started with Azure File storage on Windows</span></span>](storage-dotnet-how-to-use-files.md)

[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

