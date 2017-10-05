---
title: Funkce Smooth Streaming kurzu aplikace Windows Store | Microsoft Docs
description: "Naučte se používat Azure Media Services k vytvoření aplikace Windows Store jazyka C# s ovládacím prvkem XML MediaElement, který datový proud Smooth přehrávání obsahu."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 0fa5d8c5-3d5f-4886-ae55-fb6de4f5256d
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/11/2017
ms.author: juliako
ms.openlocfilehash: c9bb3b1915543fea3561cb309f55c4e8a74ded6d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-build-a-smooth-streaming-windows-store-application"></a><span data-ttu-id="ba056-103">Jak vytvořit funkce Smooth Streaming aplikaci pro Windows Store</span><span class="sxs-lookup"><span data-stu-id="ba056-103">How to Build a Smooth Streaming Windows Store Application</span></span>

<span data-ttu-id="ba056-104">Technologie Smooth Streaming klienta SDK pro Windows 8 umožňuje vývojářům vytvářet aplikace pro Windows Store, které můžete přehrát na vyžádání i živé vysílání funkce Smooth Streaming obsah.</span><span class="sxs-lookup"><span data-stu-id="ba056-104">The Smooth Streaming Client SDK for Windows 8 enables developers to build Windows Store applications that can play on-demand and live Smooth Streaming content.</span></span> <span data-ttu-id="ba056-105">Kromě základních přehrávání technologie Smooth Streaming obsahu, že sada SDK poskytuje také bohaté funkce, jako jsou Microsoft PlayReady ochrany, kvality úrovně omezení, Live formátu DVR, zvuk stream přepínání, naslouchá stav aktualizace (jako jsou například změny úrovně kvality) a chybové události a tak dále.</span><span class="sxs-lookup"><span data-stu-id="ba056-105">In addition to the basic playback of Smooth Streaming content, the SDK also provides rich features like Microsoft PlayReady protection, quality level restriction, Live DVR, audio stream switching, listening for status updates (such as quality level changes) and error events, and so on.</span></span> <span data-ttu-id="ba056-106">Další informace o podporovaných funkcích najdete v tématu [poznámky k verzi](http://www.iis.net/learn/media/smooth-streaming/smooth-streaming-client-sdk-for-windows-8-release-notes).</span><span class="sxs-lookup"><span data-stu-id="ba056-106">For more information of the supported features, see the [release notes](http://www.iis.net/learn/media/smooth-streaming/smooth-streaming-client-sdk-for-windows-8-release-notes).</span></span> <span data-ttu-id="ba056-107">Další informace najdete v tématu [Player Framework pro Windows 8](http://playerframework.codeplex.com/).</span><span class="sxs-lookup"><span data-stu-id="ba056-107">For more information, see [Player Framework for Windows 8](http://playerframework.codeplex.com/).</span></span> 

<span data-ttu-id="ba056-108">Tento kurz obsahuje čtyři lekce:</span><span class="sxs-lookup"><span data-stu-id="ba056-108">This tutorial contains four lessons:</span></span>

1. <span data-ttu-id="ba056-109">Vytvořte základní technologie Smooth Streaming aplikace úložiště</span><span class="sxs-lookup"><span data-stu-id="ba056-109">Create a Basic Smooth Streaming Store Application</span></span>
2. <span data-ttu-id="ba056-110">Přidat posuvníku k řízení průběh média</span><span class="sxs-lookup"><span data-stu-id="ba056-110">Add a Slider Bar to Control the Media Progress</span></span>
3. <span data-ttu-id="ba056-111">Vyberte vysílání funkce Smooth Streaming datové proudy</span><span class="sxs-lookup"><span data-stu-id="ba056-111">Select Smooth Streaming Streams</span></span>
4. <span data-ttu-id="ba056-112">Vyberte vysílání funkce Smooth Streaming sleduje</span><span class="sxs-lookup"><span data-stu-id="ba056-112">Select Smooth Streaming Tracks</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ba056-113">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ba056-113">Prerequisites</span></span>
* <span data-ttu-id="ba056-114">Windows 8 32bitové nebo 64bitové verze.</span><span class="sxs-lookup"><span data-stu-id="ba056-114">Windows 8 32-bit or 64-bit.</span></span> <span data-ttu-id="ba056-115">Můžete získat [zkušební verze Windows 8 Enterprise](http://msdn.microsoft.com/evalcenter/jj554510.aspx) z webu MSDN.</span><span class="sxs-lookup"><span data-stu-id="ba056-115">You can get [Windows 8 Enterprise Evaluation](http://msdn.microsoft.com/evalcenter/jj554510.aspx) from MSDN.</span></span>
* <span data-ttu-id="ba056-116">Visual Studio 2012 nebo Visual Studio Express 2012 (nebo novější).</span><span class="sxs-lookup"><span data-stu-id="ba056-116">Visual Studio 2012 or Visual Studio Express 2012 (or a later version).</span></span> <span data-ttu-id="ba056-117">Můžete získat zkušební verzi z [zde](http://www.microsoft.com/visualstudio/11/downloads).</span><span class="sxs-lookup"><span data-stu-id="ba056-117">You can get the trial version from [here](http://www.microsoft.com/visualstudio/11/downloads).</span></span>
* <span data-ttu-id="ba056-118">[Microsoft klienta funkce Smooth Streaming SDK pro systém Windows 8](http://visualstudiogallery.msdn.microsoft.com/04423d13-3b3e-4741-a01c-1ae29e84fea6?SRC=Homehttp://visualstudiogallery.msdn.microsoft.com/04423d13-3b3e-4741-a01c-1ae29e84fea6?SRC=Home).</span><span class="sxs-lookup"><span data-stu-id="ba056-118">[Microsoft Smooth Streaming Client SDK for Windows 8](http://visualstudiogallery.msdn.microsoft.com/04423d13-3b3e-4741-a01c-1ae29e84fea6?SRC=Homehttp://visualstudiogallery.msdn.microsoft.com/04423d13-3b3e-4741-a01c-1ae29e84fea6?SRC=Home).</span></span>

<span data-ttu-id="ba056-119">Dokončené řešení pro každé lekce si můžete stáhnout z ukázky kódu vývojáře MSDN (Galerie kódů):</span><span class="sxs-lookup"><span data-stu-id="ba056-119">The completed solution for each lesson can be downloaded from MSDN Developer Code Samples (Code Gallery):</span></span> 

* <span data-ttu-id="ba056-120">[Lekce 1](http://code.msdn.microsoft.com/Smooth-Streaming-Client-0bb1471f) – jednoduchý systém Windows 8 funkce Smooth Streaming Media Player</span><span class="sxs-lookup"><span data-stu-id="ba056-120">[Lesson 1](http://code.msdn.microsoft.com/Smooth-Streaming-Client-0bb1471f) - A Simple Windows 8 Smooth Streaming Media Player,</span></span> 
* <span data-ttu-id="ba056-121">[Lekce 2](http://code.msdn.microsoft.com/A-simple-Windows-8-Smooth-ee98f63a) – jednoduchý systém Windows 8 funkce Smooth Streaming Media Player pomocí jezdce lze ovládací prvek,</span><span class="sxs-lookup"><span data-stu-id="ba056-121">[Lesson 2](http://code.msdn.microsoft.com/A-simple-Windows-8-Smooth-ee98f63a) - A Simple Windows 8 Smooth Streaming Media Player with a Slider Bar Control,</span></span> 
* <span data-ttu-id="ba056-122">[Lekce 3](http://code.msdn.microsoft.com/A-Windows-8-Smooth-883c3b44) – Windows 8 funkce Smooth Streaming Media Player s výběrem datového proudu</span><span class="sxs-lookup"><span data-stu-id="ba056-122">[Lesson 3](http://code.msdn.microsoft.com/A-Windows-8-Smooth-883c3b44) - A Windows 8 Smooth Streaming Media Player with Stream Selection,</span></span>  
* <span data-ttu-id="ba056-123">[Lekce 4](http://code.msdn.microsoft.com/A-Windows-8-Smooth-aa9e4907) – Windows 8 funkce Smooth Streaming Media Player s výběrem sledovat.</span><span class="sxs-lookup"><span data-stu-id="ba056-123">[Lesson 4](http://code.msdn.microsoft.com/A-Windows-8-Smooth-aa9e4907)  - A Windows 8 Smooth Streaming Media Player with Track Selection.</span></span>

## <a name="lesson-1-create-a-basic-smooth-streaming-store-application"></a><span data-ttu-id="ba056-124">Lekce 1: Vytvoření základní technologie Smooth Streaming aplikace úložiště</span><span class="sxs-lookup"><span data-stu-id="ba056-124">Lesson 1: Create a Basic Smooth Streaming Store Application</span></span>

<span data-ttu-id="ba056-125">V této lekci vytvoříte aplikaci pro Windows Store s ovládacím prvkem MediaElement datový proud Smooth přehrávání obsahu.</span><span class="sxs-lookup"><span data-stu-id="ba056-125">In this lesson, you will create a Windows Store application with a MediaElement control to play Smooth Stream content.</span></span>  <span data-ttu-id="ba056-126">Běžící aplikaci vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="ba056-126">The running application looks like:</span></span>

![Příklad aplikace Smooth Streaming Windows Store][PlayerApplication]

<span data-ttu-id="ba056-128">Další informace o vývoji aplikací Windows Store najdete v tématu [vývoji kvalitních aplikací pro Windows 8](http://msdn.microsoft.com/windows/apps/br229512.aspx).</span><span class="sxs-lookup"><span data-stu-id="ba056-128">For more information on developing Windows Store application, see [Develop Great Apps for Windows 8](http://msdn.microsoft.com/windows/apps/br229512.aspx).</span></span> <span data-ttu-id="ba056-129">V této lekci obsahuje následující postupy:</span><span class="sxs-lookup"><span data-stu-id="ba056-129">This lesson contains the following procedures:</span></span>

1. <span data-ttu-id="ba056-130">Vytvoření projektu Windows Store</span><span class="sxs-lookup"><span data-stu-id="ba056-130">Create a Windows Store project</span></span>
2. <span data-ttu-id="ba056-131">Návrh uživatelského rozhraní (XAML)</span><span class="sxs-lookup"><span data-stu-id="ba056-131">Design the user interface (XAML)</span></span>
3. <span data-ttu-id="ba056-132">Úprava souboru kódu</span><span class="sxs-lookup"><span data-stu-id="ba056-132">Modify the code behind file</span></span>
4. <span data-ttu-id="ba056-133">Kompilace a testování aplikace</span><span class="sxs-lookup"><span data-stu-id="ba056-133">Compile and test the application</span></span>

<span data-ttu-id="ba056-134">**Chcete-li vytvořit projekt Windows Store**</span><span class="sxs-lookup"><span data-stu-id="ba056-134">**To create a Windows Store project**</span></span>

1. <span data-ttu-id="ba056-135">Spusťte Visual Studio 2012 nebo novějším.</span><span class="sxs-lookup"><span data-stu-id="ba056-135">Run Visual Studio 2012 or later.</span></span>
2. <span data-ttu-id="ba056-136">V nabídce **Soubor** klikněte na tlačítko **Nový** a pak klikněte na tlačítko **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="ba056-136">From the **FILE** menu, click **New**, and then click **Project**.</span></span>
3. <span data-ttu-id="ba056-137">Z tohoto dialogového okna Nový projekt zadejte nebo vyberte tyto hodnoty:</span><span class="sxs-lookup"><span data-stu-id="ba056-137">From the New Project dialog, type or select  the following values:</span></span>

| <span data-ttu-id="ba056-138">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="ba056-138">Name</span></span> | <span data-ttu-id="ba056-139">Hodnota</span><span class="sxs-lookup"><span data-stu-id="ba056-139">Value</span></span> |
| --- | --- |
| <span data-ttu-id="ba056-140">Skupina šablon</span><span class="sxs-lookup"><span data-stu-id="ba056-140">Template group</span></span> |<span data-ttu-id="ba056-141">Nainstalovaná, šablony/Visual C# / Windows Store</span><span class="sxs-lookup"><span data-stu-id="ba056-141">Installed/Templates/Visual C#/Windows Store</span></span> |
| <span data-ttu-id="ba056-142">Šablona</span><span class="sxs-lookup"><span data-stu-id="ba056-142">Template</span></span> |<span data-ttu-id="ba056-143">Prázdná aplikace (XAML)</span><span class="sxs-lookup"><span data-stu-id="ba056-143">Blank App (XAML)</span></span> |
| <span data-ttu-id="ba056-144">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="ba056-144">Name</span></span> |<span data-ttu-id="ba056-145">SSPlayer</span><span class="sxs-lookup"><span data-stu-id="ba056-145">SSPlayer</span></span> |
| <span data-ttu-id="ba056-146">Umístění</span><span class="sxs-lookup"><span data-stu-id="ba056-146">Location</span></span> |<span data-ttu-id="ba056-147">C:\SSTutorials</span><span class="sxs-lookup"><span data-stu-id="ba056-147">C:\SSTutorials</span></span> |
| <span data-ttu-id="ba056-148">Název řešení</span><span class="sxs-lookup"><span data-stu-id="ba056-148">Solution Name</span></span> |<span data-ttu-id="ba056-149">SSPlayer</span><span class="sxs-lookup"><span data-stu-id="ba056-149">SSPlayer</span></span> |
| <span data-ttu-id="ba056-150">Vytvořte adresář pro řešení</span><span class="sxs-lookup"><span data-stu-id="ba056-150">Create directory for solution</span></span> |<span data-ttu-id="ba056-151">(zaškrtnuto)</span><span class="sxs-lookup"><span data-stu-id="ba056-151">(selected)</span></span> |

1. <span data-ttu-id="ba056-152">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="ba056-152">Click **OK**.</span></span>

<span data-ttu-id="ba056-153">**Chcete-li přidat odkaz na technologie Smooth Streaming Client SDK**</span><span class="sxs-lookup"><span data-stu-id="ba056-153">**To add a reference to the Smooth Streaming Client SDK**</span></span>

1. <span data-ttu-id="ba056-154">V Průzkumníku řešení klikněte pravým tlačítkem **SSPlayer**a potom klikněte na **přidat odkaz na**.</span><span class="sxs-lookup"><span data-stu-id="ba056-154">From Solution Explorer, right-click **SSPlayer**, and then click **Add Reference**.</span></span>
2. <span data-ttu-id="ba056-155">Zadejte nebo vyberte tyto hodnoty:</span><span class="sxs-lookup"><span data-stu-id="ba056-155">Type or select the following values:</span></span>

| <span data-ttu-id="ba056-156">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="ba056-156">Name</span></span> | <span data-ttu-id="ba056-157">Hodnota</span><span class="sxs-lookup"><span data-stu-id="ba056-157">Value</span></span> |
| --- | --- |
| <span data-ttu-id="ba056-158">Odkaz na skupinu</span><span class="sxs-lookup"><span data-stu-id="ba056-158">Reference group</span></span> |<span data-ttu-id="ba056-159">Rozšíření/Windows</span><span class="sxs-lookup"><span data-stu-id="ba056-159">Windows/Extensions</span></span> |
| <span data-ttu-id="ba056-160">Referenční informace</span><span class="sxs-lookup"><span data-stu-id="ba056-160">Reference</span></span> |<span data-ttu-id="ba056-161">Vyberte Microsoft klienta funkce Smooth Streaming SDK pro systém Windows 8 a balíček Microsoft Visual C++ Runtime</span><span class="sxs-lookup"><span data-stu-id="ba056-161">Select Microsoft Smooth Streaming Client SDK for Windows 8 and Microsoft Visual C++ Runtime Package</span></span> |

1. <span data-ttu-id="ba056-162">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="ba056-162">Click **OK**.</span></span> 

<span data-ttu-id="ba056-163">Po přidání odkazů, je nutné vybrat cílovou platformu (x64 nebo x86), přidávání odkazů nebude fungovat pro jakýkoli procesor platformy konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="ba056-163">After adding the references, you must select the targeted platform (x64 or x86), adding references will not work for Any CPU platform configuration.</span></span>  <span data-ttu-id="ba056-164">V Průzkumníku řešení zobrazí se žlutý označit upozornění pro tyto přidat odkazy.</span><span class="sxs-lookup"><span data-stu-id="ba056-164">In solution explorer, you will see yellow warning mark for these added references.</span></span>

<span data-ttu-id="ba056-165">**Při návrhu player uživatelského rozhraní**</span><span class="sxs-lookup"><span data-stu-id="ba056-165">**To design the player user interface**</span></span>

1. <span data-ttu-id="ba056-166">V Průzkumníku řešení poklikejte na **MainPage.xaml** a otevře se v zobrazení návrhu.</span><span class="sxs-lookup"><span data-stu-id="ba056-166">From Solution Explorer, double click **MainPage.xaml** to open it in the design view.</span></span>
2. <span data-ttu-id="ba056-167">Vyhledejte  **&lt;mřížky&gt;**  a  **&lt;/Grid&gt;**  značky souboru XAML a vložte následující kód mezi dvě značky:</span><span class="sxs-lookup"><span data-stu-id="ba056-167">Locate the **&lt;Grid&gt;** and **&lt;/Grid&gt;**  tags the XAML file, and paste the following code between the two tags:</span></span>

         <Grid.RowDefinitions>

            <RowDefinition Height="20"/>    <!-- spacer -->
            <RowDefinition Height="50"/>    <!-- media controls -->
            <RowDefinition Height="100*"/>  <!-- media element -->
            <RowDefinition Height="80*"/>   <!-- media stream and track selection -->
            <RowDefinition Height="50"/>    <!-- status bar -->
         </Grid.RowDefinitions>

         <StackPanel Name="spMediaControl" Grid.Row="1" Orientation="Horizontal">
            <TextBlock x:Name="tbSource" Text="Source :  " FontSize="16" FontWeight="Bold" VerticalAlignment="Center" />
            <TextBox x:Name="txtMediaSource" Text="http://ecn.channel9.msdn.com/o9/content/smf/smoothcontent/elephantsdream/Elephants_Dream_1024-h264-st-aac.ism/manifest" FontSize="10" Width="700" Margin="0,4,0,10" />
            <Button x:Name="btnSetSource" Content="Set Source" Width="111" Height="43" Click="btnSetSource_Click"/>
            <Button x:Name="btnPlay" Content="Play" Width="111" Height="43" Click="btnPlay_Click"/>
            <Button x:Name="btnPause" Content="Pause"  Width="111" Height="43" Click="btnPause_Click"/>
            <Button x:Name="btnStop" Content="Stop"  Width="111" Height="43" Click="btnStop_Click"/>
            <CheckBox x:Name="chkAutoPlay" Content="Auto Play" Height="55" Width="Auto" IsChecked="{Binding AutoPlay, ElementName=mediaElement, Mode=TwoWay}"/>
            <CheckBox x:Name="chkMute" Content="Mute" Height="55" Width="67" IsChecked="{Binding IsMuted, ElementName=mediaElement, Mode=TwoWay}"/>
         </StackPanel>

         <StackPanel Name="spMediaElement" Grid.Row="2" Height="435" Width="1072"
                    HorizontalAlignment="Center" VerticalAlignment="Center">
            <MediaElement x:Name="mediaElement" Height="356" Width="924" MinHeight="225"
                          HorizontalAlignment="Center" VerticalAlignment="Center" 
                          AudioCategory="BackgroundCapableMedia" />
            <StackPanel Orientation="Horizontal">
                <Slider x:Name="sliderProgress" Width="924" Height="44"
                        HorizontalAlignment="Center" VerticalAlignment="Center"
                        PointerPressed="sliderProgress_PointerPressed"/>
                <Slider x:Name="sliderVolume" 
                        HorizontalAlignment="Right" VerticalAlignment="Center" Orientation="Vertical" 
                        Height="79" Width="148" Minimum="0" Maximum="1" StepFrequency="0.1" 
                        Value="{Binding Volume, ElementName=mediaElement, Mode=TwoWay}" 
                        ToolTipService.ToolTip="{Binding Value, RelativeSource={RelativeSource Mode=Self}}"/>
            </StackPanel>
         </StackPanel>

         <StackPanel Name="spStatus" Grid.Row="4" Orientation="Horizontal">
            <TextBlock x:Name="tbStatus" Text="Status :  " 
               FontSize="16" FontWeight="Bold" VerticalAlignment="Center" HorizontalAlignment="Center" />
            <TextBox x:Name="txtStatus" FontSize="10" Width="700" VerticalAlignment="Center"/>
         </StackPanel>
   
   <span data-ttu-id="ba056-168">Prvek MediaElement se používá k přehrávání média.</span><span class="sxs-lookup"><span data-stu-id="ba056-168">The MediaElement control is used to playback media.</span></span> <span data-ttu-id="ba056-169">Posuvník s názvem sliderProgress se použije v další lekce k řízení průběh média.</span><span class="sxs-lookup"><span data-stu-id="ba056-169">The slider control named sliderProgress will be used in the next lesson to control the media progress.</span></span>
3. <span data-ttu-id="ba056-170">Stiskněte klávesu **CTRL + S** k uložení souboru.</span><span class="sxs-lookup"><span data-stu-id="ba056-170">Press **CTRL+S** to save the file.</span></span>

<span data-ttu-id="ba056-171">Ovládací prvek MediaElement nepodporuje vysílání funkce Smooth Streaming obsahu out-of-box.</span><span class="sxs-lookup"><span data-stu-id="ba056-171">The MediaElement control does not support Smooth Streaming content out-of-box.</span></span> <span data-ttu-id="ba056-172">Pokud chcete povolit podporu technologie Smooth Streaming, je nutné zaregistrovat obslužné rutiny byte – datový proud Smooth Streaming příponu názvu souboru a typ MIME.</span><span class="sxs-lookup"><span data-stu-id="ba056-172">To enable the Smooth Streaming support, you must register the Smooth Streaming byte-stream handler by file name extension and MIME type.</span></span>  <span data-ttu-id="ba056-173">Pokud chcete zaregistrovat, můžete použít metodu MediaExtensionManager.RegisterByteStremHandler Windows.Media oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="ba056-173">To register, you use the MediaExtensionManager.RegisterByteStremHandler method of the Windows.Media namespace.</span></span>

<span data-ttu-id="ba056-174">V tomto souboru XAML některé obslužné rutiny událostí jsou přidruženy k ovládacích prvků.</span><span class="sxs-lookup"><span data-stu-id="ba056-174">In this XAML file, some event handlers are associated with the controls.</span></span>  <span data-ttu-id="ba056-175">Je nutné zadat tyto obslužné rutiny událostí.</span><span class="sxs-lookup"><span data-stu-id="ba056-175">You must define those event handlers.</span></span>

<span data-ttu-id="ba056-176">**K úpravě souboru kódu**</span><span class="sxs-lookup"><span data-stu-id="ba056-176">**To modify the code behind file**</span></span>

1. <span data-ttu-id="ba056-177">V Průzkumníku řešení klikněte pravým tlačítkem **MainPage.xaml**a potom klikněte na **kód zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="ba056-177">From Solution Explorer, right-click **MainPage.xaml**, and then click **View Code**.</span></span>
2. <span data-ttu-id="ba056-178">Na začátek souboru přidejte následující příkaz using:</span><span class="sxs-lookup"><span data-stu-id="ba056-178">At the top of the file, add the following using statement:</span></span>
   
        using Windows.Media;
3. <span data-ttu-id="ba056-179">Na začátku **MainPage** třídy, přidejte následující datový člen:</span><span class="sxs-lookup"><span data-stu-id="ba056-179">At the beginning of the **MainPage** class, add the following data member:</span></span>
   
         private MediaExtensionManager extensions = new MediaExtensionManager();
4. <span data-ttu-id="ba056-180">Na konci **MainPage** konstruktoru, přidejte následující dva řádky:</span><span class="sxs-lookup"><span data-stu-id="ba056-180">At the end of the **MainPage** constructor, add the following two lines:</span></span>
   
        extensions.RegisterByteStreamHandler("Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", ".ism", "text/xml");
        extensions.RegisterByteStreamHandler("Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", ".ism", "application/vnd.ms-sstr+xml");
5. <span data-ttu-id="ba056-181">Na konci **MainPage** třídy, vložte následující kód:</span><span class="sxs-lookup"><span data-stu-id="ba056-181">At the end of the **MainPage** class, paste the following code:</span></span>
   
         # region UI Button Click Events
         private void btnPlay_Click(object sender, RoutedEventArgs e)
         {

         mediaElement.Play();
         txtStatus.Text = "MediaElement is playing ...";
         }
         private void btnPause_Click(object sender, RoutedEventArgs e)
         {

         mediaElement.Pause();
         txtStatus.Text = "MediaElement is paused";
         }
         private void btnSetSource_Click(object sender, RoutedEventArgs e)
         {

         sliderProgress.Value = 0;
         mediaElement.Source = new Uri(txtMediaSource.Text);

         if (chkAutoPlay.IsChecked == true)
         {
             txtStatus.Text = "MediaElement is playing ...";
         }
         else
         {
             txtStatus.Text = "Click the Play button to play the media source.";
         }
         }
         private void btnStop_Click(object sender, RoutedEventArgs e)
         {

         mediaElement.Stop();
         txtStatus.Text = "MediaElement is stopped";
         }
         private void sliderProgress_PointerPressed(object sender, PointerRoutedEventArgs e)
         {

         txtStatus.Text = "Seek to position " + sliderProgress.Value;
         mediaElement.Position = new TimeSpan(0, 0, (int)(sliderProgress.Value));
         }
         # endregion

<span data-ttu-id="ba056-182">Obslužné rutiny události sliderProgress_PointerPressed je definována v tomto poli.</span><span class="sxs-lookup"><span data-stu-id="ba056-182">The sliderProgress_PointerPressed event handler is defined here.</span></span>  <span data-ttu-id="ba056-183">Existují další funguje udělat, aby ho pracuje, získat, který se bude vztahovat v další lekci tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="ba056-183">There are more works to do to get it working, which will be covered in the next lesson of this tutorial.</span></span>
6. <span data-ttu-id="ba056-184">Stiskněte klávesu **CTRL + S** k uložení souboru.</span><span class="sxs-lookup"><span data-stu-id="ba056-184">Press **CTRL+S** to save the file.</span></span>

<span data-ttu-id="ba056-185">Dokončení kódu na pozadí se vypadat například takto:</span><span class="sxs-lookup"><span data-stu-id="ba056-185">The finished the code behind file shall look like this:</span></span>

![Codeview v aplikaci Visual Studio o technologie Smooth Streaming Windows Store][CodeViewPic]

<span data-ttu-id="ba056-187">**Pro zkompilování a testování aplikace**</span><span class="sxs-lookup"><span data-stu-id="ba056-187">**To compile and test the application**</span></span>

1. <span data-ttu-id="ba056-188">Z **sestavení** nabídky, klikněte na tlačítko **nástroje Configuration Manager**.</span><span class="sxs-lookup"><span data-stu-id="ba056-188">From the **BUILD** menu, click **Configuration Manager**.</span></span>
2. <span data-ttu-id="ba056-189">Změna **platforma Active řešení** tak, aby odpovídaly vývojové platformy.</span><span class="sxs-lookup"><span data-stu-id="ba056-189">Change **Active solution platform** to match your development platform.</span></span>
3. <span data-ttu-id="ba056-190">Stiskněte klávesu **F6** kompilace projektu.</span><span class="sxs-lookup"><span data-stu-id="ba056-190">Press **F6** to compile the project.</span></span> 
4. <span data-ttu-id="ba056-191">Stisknutím klávesy **F5** spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ba056-191">Press **F5** to run the application.</span></span>
5. <span data-ttu-id="ba056-192">V horní části aplikace můžete buď použít výchozí nastavení adresy URL technologie Smooth Streaming nebo zadejte jiný.</span><span class="sxs-lookup"><span data-stu-id="ba056-192">At the top of the application, you can either use the default Smooth Streaming URL or enter a different one.</span></span> 
6. <span data-ttu-id="ba056-193">Klikněte na tlačítko **nastavit zdroj**.</span><span class="sxs-lookup"><span data-stu-id="ba056-193">Click **Set Source**.</span></span> <span data-ttu-id="ba056-194">Protože **automatické přehrávání** je povoleno ve výchozím nastavení, se automaticky přehrání média.</span><span class="sxs-lookup"><span data-stu-id="ba056-194">Because **Auto Play** is enabled by default, the media shall play automatically.</span></span>  <span data-ttu-id="ba056-195">Můžete řídit média pomocí **přehrání**, **pozastavení** a **Zastavit** tlačítka.</span><span class="sxs-lookup"><span data-stu-id="ba056-195">You can control the media using the **Play**, **Pause** and **Stop** buttons.</span></span>  <span data-ttu-id="ba056-196">Můžete ovládat svazku média pomocí svislé posuvníku.</span><span class="sxs-lookup"><span data-stu-id="ba056-196">You can control the media volume using the vertical slider.</span></span>  <span data-ttu-id="ba056-197">Ale vodorovné posuvník pro řízení průběh média není plně dosud implementována.</span><span class="sxs-lookup"><span data-stu-id="ba056-197">However the horizontal slider for controlling the media progress is not fully implemented yet.</span></span> 

<span data-ttu-id="ba056-198">Dokončili jste lesson1.</span><span class="sxs-lookup"><span data-stu-id="ba056-198">You have completed lesson1.</span></span>  <span data-ttu-id="ba056-199">V této lekci použijte ovládací prvek MediaElement k přehrávání obsahu technologie Smooth Streaming.</span><span class="sxs-lookup"><span data-stu-id="ba056-199">In this lesson, you use a MediaElement control to playback Smooth Streaming content.</span></span>  <span data-ttu-id="ba056-200">V další lekci přidáte jezdce k řízení průběh obsah Smooth Streaming.</span><span class="sxs-lookup"><span data-stu-id="ba056-200">In the next lesson, you will add a slider to control the progress of the Smooth Streaming content.</span></span>

## <a name="lesson-2-add-a-slider-bar-to-control-the-media-progress"></a><span data-ttu-id="ba056-201">Lekce 2: Přidání posuvníku k řízení průběh média</span><span class="sxs-lookup"><span data-stu-id="ba056-201">Lesson 2: Add a Slider Bar to Control the Media Progress</span></span>

<span data-ttu-id="ba056-202">V Lekce 1 jste vytvořili aplikaci pro Windows Store s ovládacím prvkem MediaElement XAML k přehrávání technologie Smooth Streaming mediální obsah.</span><span class="sxs-lookup"><span data-stu-id="ba056-202">In lesson 1, you created a Windows Store application with a MediaElement XAML control to playback Smooth Streaming media content.</span></span>  <span data-ttu-id="ba056-203">Některé funkce základní média jako spuštění, zastavení nebo pozastavení pochází.</span><span class="sxs-lookup"><span data-stu-id="ba056-203">It comes some basic media functions like start, stop and pause.</span></span>  <span data-ttu-id="ba056-204">V této lekci přidáte ovládací prvek typu jezdec panelu do aplikace.</span><span class="sxs-lookup"><span data-stu-id="ba056-204">In this lesson, you will add a slider bar control to the application.</span></span>

<span data-ttu-id="ba056-205">V tomto kurzu budeme používat časovač aktualizovat pozice posuvníku založené na aktuální pozici MediaElement ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="ba056-205">In this tutorial, we will use a timer to update the slider position based on the current position of the MediaElement control.</span></span>  <span data-ttu-id="ba056-206">Posuvník počáteční a koncová čas také je potřeba aktualizovat v případě živý obsah.</span><span class="sxs-lookup"><span data-stu-id="ba056-206">The slider start and end time also need to be updated in case of live content.</span></span>  <span data-ttu-id="ba056-207">To může lépe ošetřit v události adaptivní zdroj aktualizace.</span><span class="sxs-lookup"><span data-stu-id="ba056-207">This can be better handled in the adaptive source update event.</span></span>

<span data-ttu-id="ba056-208">Média zdroje jsou objekty, které generují data média.</span><span class="sxs-lookup"><span data-stu-id="ba056-208">Media sources are objects that generate media data.</span></span>  <span data-ttu-id="ba056-209">Překladač zdroj přebírá adresu URL nebo bajtů datového proudu a vytvoří odpovídající médium zdroj pro tento obsah.</span><span class="sxs-lookup"><span data-stu-id="ba056-209">The source resolver takes a URL or byte stream and creates the appropriate media source for that content.</span></span>  <span data-ttu-id="ba056-210">Překladač zdroj je standardní způsob pro aplikace, které chcete vytvořit médium zdroje.</span><span class="sxs-lookup"><span data-stu-id="ba056-210">The source resolver is the standard way for the applications to create media sources.</span></span> 

<span data-ttu-id="ba056-211">V této lekci obsahuje následující postupy:</span><span class="sxs-lookup"><span data-stu-id="ba056-211">This lesson contains the following procedures:</span></span>

1. <span data-ttu-id="ba056-212">Zaregistrovat obslužná rutina technologie Smooth Streaming</span><span class="sxs-lookup"><span data-stu-id="ba056-212">Register the Smooth Streaming handler</span></span> 
2. <span data-ttu-id="ba056-213">Přidání obslužných rutin událostí na úrovni správce adaptivní zdroje</span><span class="sxs-lookup"><span data-stu-id="ba056-213">Add the adaptive source manager level event handlers</span></span>
3. <span data-ttu-id="ba056-214">Přidání obslužných rutin událostí na úrovni adaptivní zdroje</span><span class="sxs-lookup"><span data-stu-id="ba056-214">Add the adaptive source level event handlers</span></span>
4. <span data-ttu-id="ba056-215">Přidání obslužných rutin událostí MediaElement</span><span class="sxs-lookup"><span data-stu-id="ba056-215">Add MediaElement event handlers</span></span>
5. <span data-ttu-id="ba056-216">Přidat posuvníku panelu související kódu</span><span class="sxs-lookup"><span data-stu-id="ba056-216">Add slider bar related code</span></span>
6. <span data-ttu-id="ba056-217">Kompilace a testování aplikace</span><span class="sxs-lookup"><span data-stu-id="ba056-217">Compile and test the application</span></span>

<span data-ttu-id="ba056-218">**K registraci obslužné rutiny byte – datový proud Smooth Streaming a předat propertyset**</span><span class="sxs-lookup"><span data-stu-id="ba056-218">**To register the Smooth Streaming byte-stream handler and pass the propertyset**</span></span>

1. <span data-ttu-id="ba056-219">V Průzkumníku řešení klikněte pravým tlačítkem na **MainPage.xaml**a potom klikněte na **kód zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="ba056-219">From Solution Explorer, right click **MainPage.xaml**, and then click **View Code**.</span></span>
2. <span data-ttu-id="ba056-220">Na začátek souboru přidejte následující příkaz using:</span><span class="sxs-lookup"><span data-stu-id="ba056-220">At the beginning of the file, add the following using statement:</span></span>

        using Microsoft.Media.AdaptiveStreaming;
3. <span data-ttu-id="ba056-221">Na začátku MainPage třídy, přidejte následující členy dat:</span><span class="sxs-lookup"><span data-stu-id="ba056-221">At the beginning of the MainPage class, add the following data members:</span></span>

         private Windows.Foundation.Collections.PropertySet propertySet = new Windows.Foundation.Collections.PropertySet();             
         private IAdaptiveSourceManager adaptiveSourceManager;
4. <span data-ttu-id="ba056-222">Uvnitř **MainPage** konstruktoru, přidejte následující kód po **to. Inicializace Components();**  řádku a registrace kód napsaný v předchozí lekci řádky:</span><span class="sxs-lookup"><span data-stu-id="ba056-222">Inside the **MainPage** constructor, add the following code after the **this.Initialize Components();** line and the registration code lines written in the previous lesson:</span></span>

        // Gets the default instance of AdaptiveSourceManager which manages Smooth 
        //Streaming media sources.
        adaptiveSourceManager = AdaptiveSourceManager.GetDefault();
        // Sets property key value to AdaptiveSourceManager default instance.
        // {A5CE1DE8-1D00-427B-ACEF-FB9A3C93DE2D}" must be hardcoded.
        propertySet["{A5CE1DE8-1D00-427B-ACEF-FB9A3C93DE2D}"] = adaptiveSourceManager;
5. <span data-ttu-id="ba056-223">Uvnitř **MainPage** konstruktoru, upravte tyto dvě metody RegisterByteStreamHandler přidat stanovilo parametry:</span><span class="sxs-lookup"><span data-stu-id="ba056-223">Inside the **MainPage** constructor, modify the two RegisterByteStreamHandler methods to add the forth parameters:</span></span>

         // Registers Smooth Streaming byte-stream handler for ".ism" extension and, 
         // "text/xml" and "application/vnd.ms-ss" mime-types and pass the propertyset. 
         // http://*.ism/manifest URI resources will be resolved by Byte-stream handler.
         extensions.RegisterByteStreamHandler(

            "Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", 
            ".ism", 
            "text/xml", 
            propertySet );
         extensions.RegisterByteStreamHandler(

            "Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", 
            ".ism", 
            "application/vnd.ms-sstr+xml", 
         propertySet);
6. <span data-ttu-id="ba056-224">Stiskněte klávesu **CTRL + S** k uložení souboru.</span><span class="sxs-lookup"><span data-stu-id="ba056-224">Press **CTRL+S** to save the file.</span></span>

<span data-ttu-id="ba056-225">**Přidání obslužné rutiny událostí na úrovni správce adaptivní zdroje**</span><span class="sxs-lookup"><span data-stu-id="ba056-225">**To add the adaptive source manager level event handler**</span></span>

1. <span data-ttu-id="ba056-226">V Průzkumníku řešení klikněte pravým tlačítkem na **MainPage.xaml**a potom klikněte na **kód zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="ba056-226">From Solution Explorer, right click **MainPage.xaml**, and then click **View Code**.</span></span>
2. <span data-ttu-id="ba056-227">Uvnitř **MainPage** třídy, přidejte následující datový člen:</span><span class="sxs-lookup"><span data-stu-id="ba056-227">Inside the **MainPage** class, add the following data member:</span></span>
   
     <span data-ttu-id="ba056-228">privátní adaptiveSource AdaptiveSource = null;</span><span class="sxs-lookup"><span data-stu-id="ba056-228">private AdaptiveSource adaptiveSource = null;</span></span>
3. <span data-ttu-id="ba056-229">Na konci **MainPage** třídy, přidejte následující obslužnou rutinu události:</span><span class="sxs-lookup"><span data-stu-id="ba056-229">At the end of the **MainPage** class, add the following event handler:</span></span>
   
         # region Adaptive Source Manager Level Events
         private void mediaElement_AdaptiveSourceOpened(AdaptiveSource sender, AdaptiveSourceOpenedEventArgs args)
         {

            adaptiveSource = args.AdaptiveSource;
         }

         # endregion Adaptive Source Manager Level Events
4. <span data-ttu-id="ba056-230">Na konci **MainPage** konstruktoru, přidejte následující řádek k odběru události open adaptivní zdroje:</span><span class="sxs-lookup"><span data-stu-id="ba056-230">At the end of the **MainPage** constructor, add the following line to subscribe to the adaptive source open event:</span></span>
   
         adaptiveSourceManager.AdaptiveSourceOpenedEvent += 
           new AdaptiveSourceOpenedEventHandler(mediaElement_AdaptiveSourceOpened);
5. <span data-ttu-id="ba056-231">Stiskněte klávesu **CTRL + S** k uložení souboru.</span><span class="sxs-lookup"><span data-stu-id="ba056-231">Press **CTRL+S** to save the file.</span></span>

<span data-ttu-id="ba056-232">**Přidání obslužných rutin událostí na úrovni adaptivní zdroje**</span><span class="sxs-lookup"><span data-stu-id="ba056-232">**To add adaptive source level event handlers**</span></span>

1. <span data-ttu-id="ba056-233">V Průzkumníku řešení klikněte pravým tlačítkem na **MainPage.xaml**a potom klikněte na **kód zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="ba056-233">From Solution Explorer, right click **MainPage.xaml**, and then click **View Code**.</span></span>
2. <span data-ttu-id="ba056-234">Uvnitř **MainPage** třídy, přidejte následující datový člen:</span><span class="sxs-lookup"><span data-stu-id="ba056-234">Inside the **MainPage** class, add the following data member:</span></span>
   
     <span data-ttu-id="ba056-235">privátní AdaptiveSourceStatusUpdatedEventArgs adaptiveSourceStatusUpdate;   privátní manifestu manifestObject;</span><span class="sxs-lookup"><span data-stu-id="ba056-235">private AdaptiveSourceStatusUpdatedEventArgs adaptiveSourceStatusUpdate;   private Manifest manifestObject;</span></span>
3. <span data-ttu-id="ba056-236">Na konci **MainPage** třídy, přidejte následující obslužné rutiny událostí:</span><span class="sxs-lookup"><span data-stu-id="ba056-236">At the end of the **MainPage** class, add the following event handlers:</span></span>

         # region Adaptive Source Level Events
         private void mediaElement_ManifestReady(AdaptiveSource sender, ManifestReadyEventArgs args)
         {

            adaptiveSource = args.AdaptiveSource;
            manifestObject = args.AdaptiveSource.Manifest;
         }

         private void mediaElement_AdaptiveSourceStatusUpdated(AdaptiveSource sender, AdaptiveSourceStatusUpdatedEventArgs args)
         {

            adaptiveSourceStatusUpdate = args;
         }

         private void mediaElement_AdaptiveSourceFailed(AdaptiveSource sender, AdaptiveSourceFailedEventArgs args)
         {

            txtStatus.Text = "Error: " + args.HttpResponse;
         }

         # endregion Adaptive Source Level Events
4. <span data-ttu-id="ba056-237">Na konci **mediaElement AdaptiveSourceOpened** metoda, přidejte následující kód k odběru události:</span><span class="sxs-lookup"><span data-stu-id="ba056-237">At the end of the **mediaElement AdaptiveSourceOpened** method, add the following code to subscribe to the events:</span></span>
   
         adaptiveSource.ManifestReadyEvent +=

                    mediaElement_ManifestReady;
         adaptiveSource.AdaptiveSourceStatusUpdatedEvent += 

            mediaElement_AdaptiveSourceStatusUpdated;
         adaptiveSource.AdaptiveSourceFailedEvent += 

            mediaElement_AdaptiveSourceFailed;
5. <span data-ttu-id="ba056-238">Stiskněte klávesu **CTRL + S** k uložení souboru.</span><span class="sxs-lookup"><span data-stu-id="ba056-238">Press **CTRL+S** to save the file.</span></span>

<span data-ttu-id="ba056-239">Stejné události jsou k dispozici na adaptivní zdroj Manager úrovni také, které lze použít pro zpracování funkce, které jsou společné pro všechny elementy média v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ba056-239">The same events are available on Adaptive Source manger level as well, which can be used for handling functionality common to all media elements in the app.</span></span> <span data-ttu-id="ba056-240">Každý AdaptiveSource zahrnuje vlastní události a všechny události AdaptiveSource předána pod AdaptiveSourceManager.</span><span class="sxs-lookup"><span data-stu-id="ba056-240">Each AdaptiveSource includes its own events and all AdaptiveSource events will be cascaded under AdaptiveSourceManager.</span></span>

<span data-ttu-id="ba056-241">**Chcete-li přidat Element média obslužné rutiny událostí**</span><span class="sxs-lookup"><span data-stu-id="ba056-241">**To add Media Element event handlers**</span></span>

1. <span data-ttu-id="ba056-242">V Průzkumníku řešení klikněte pravým tlačítkem na **MainPage.xaml**a potom klikněte na **kód zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="ba056-242">From Solution Explorer, right click **MainPage.xaml**, and then click **View Code**.</span></span>
2. <span data-ttu-id="ba056-243">Na konci **MainPage** třídy, přidejte následující obslužné rutiny událostí:</span><span class="sxs-lookup"><span data-stu-id="ba056-243">At the end of the **MainPage** class, add the following event handlers:</span></span>

         # region Media Element Event Handlers
         private void MediaOpened(object sender, RoutedEventArgs e)
         {

            txtStatus.Text = "MediaElement opened";
         }

         private void MediaFailed(object sender, ExceptionRoutedEventArgs e)
         {

            txtStatus.Text= "MediaElement failed: " + e.ErrorMessage;
         }

         private void MediaEnded(object sender, RoutedEventArgs e)
         {

            txtStatus.Text ="MediaElement ended.";
         }

         # endregion Media Element Event Handlers
3. <span data-ttu-id="ba056-244">Na konci **MainPage** konstruktoru, přidejte následující kód do dolní index k událostem:</span><span class="sxs-lookup"><span data-stu-id="ba056-244">At the end of the **MainPage** constructor, add the following code to subscript to the events:</span></span>

         mediaElement.MediaOpened += MediaOpened;
         mediaElement.MediaEnded += MediaEnded;
         mediaElement.MediaFailed += MediaFailed;
4. <span data-ttu-id="ba056-245">Stiskněte klávesu **CTRL + S** k uložení souboru.</span><span class="sxs-lookup"><span data-stu-id="ba056-245">Press **CTRL+S** to save the file.</span></span>

<span data-ttu-id="ba056-246">**Chcete-li přidat posuvníku související kódu**</span><span class="sxs-lookup"><span data-stu-id="ba056-246">**To add slider bar related code**</span></span>

1. <span data-ttu-id="ba056-247">V Průzkumníku řešení klikněte pravým tlačítkem na **MainPage.xaml**a potom klikněte na **kód zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="ba056-247">From Solution Explorer, right click **MainPage.xaml**, and then click **View Code**.</span></span>
2. <span data-ttu-id="ba056-248">Na začátek souboru přidejte následující příkaz using:</span><span class="sxs-lookup"><span data-stu-id="ba056-248">At the beginning of the file, add the following using statement:</span></span>
      
        using Windows.UI.Core;
3. <span data-ttu-id="ba056-249">Uvnitř **MainPage** třídy, přidejte následující členy dat:</span><span class="sxs-lookup"><span data-stu-id="ba056-249">Inside the **MainPage** class, add the following data members:</span></span>
   
         public static CoreDispatcher _dispatcher;
         private DispatcherTimer sliderPositionUpdateDispatcher;
4. <span data-ttu-id="ba056-250">Na konci **MainPage** konstruktoru, přidejte následující kód:</span><span class="sxs-lookup"><span data-stu-id="ba056-250">At the end of the **MainPage** constructor, add the following code:</span></span>
   
         _dispatcher = Window.Current.Dispatcher;
         PointerEventHandler pointerpressedhandler = new PointerEventHandler(sliderProgress_PointerPressed);
         sliderProgress.AddHandler(Control.PointerPressedEvent, pointerpressedhandler, true);    
5. <span data-ttu-id="ba056-251">Na konci **MainPage** třídy, přidejte následující kód:</span><span class="sxs-lookup"><span data-stu-id="ba056-251">At the end of the **MainPage** class, add the following code:</span></span>

         # region sliderMediaPlayer
         private double SliderFrequency(TimeSpan timevalue)
         {

            long absvalue = 0;
            double stepfrequency = -1;

            if (manifestObject != null)
            {
                absvalue = manifestObject.Duration - (long)manifestObject.StartTime;
            }
            else
            {
                absvalue = mediaElement.NaturalDuration.TimeSpan.Ticks;
            }

            TimeSpan totalDVRDuration = new TimeSpan(absvalue);

            if (totalDVRDuration.TotalMinutes >= 10 && totalDVRDuration.TotalMinutes < 30)
            {
               stepfrequency = 10;
            }
            else if (totalDVRDuration.TotalMinutes >= 30 
                     && totalDVRDuration.TotalMinutes < 60)
            {
                stepfrequency = 30;
            }
            else if (totalDVRDuration.TotalHours >= 1)
            {
                stepfrequency = 60;
            }

            return stepfrequency;
         }

         void updateSliderPositionoNTicks(object sender, object e)
         {

            sliderProgress.Value = mediaElement.Position.TotalSeconds;
         }

         public void setupTimer()
         {

            sliderPositionUpdateDispatcher = new DispatcherTimer();
            sliderPositionUpdateDispatcher.Interval = new TimeSpan(0, 0, 0, 0, 300);
            startTimer();
         }

         public void startTimer()
         {

            sliderPositionUpdateDispatcher.Tick += updateSliderPositionoNTicks;
            sliderPositionUpdateDispatcher.Start();
         }

         // Slider start and end time must be updated in case of live content
         public async void setSliderStartTime(long startTime)
         {

            await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
                TimeSpan timespan = new TimeSpan(adaptiveSourceStatusUpdate.StartTime);
                double absvalue = (int)Math.Round(timespan.TotalSeconds, MidpointRounding.AwayFromZero);
                sliderProgress.Minimum = absvalue;
            });
         }

         // Slider start and end time must be updated in case of live content
         public async void setSliderEndTime(long startTime)
         {

            await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
                TimeSpan timespan = new TimeSpan(adaptiveSourceStatusUpdate.EndTime);
                double absvalue = (int)Math.Round(timespan.TotalSeconds, MidpointRounding.AwayFromZero);
                sliderProgress.Maximum = absvalue;
            });
         }

         # endregion sliderMediaPlayer
      
>[!NOTE]
><span data-ttu-id="ba056-252">CoreDispatcher slouží ke změnám vlákna uživatelského rozhraní od jiných vlákna uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="ba056-252">CoreDispatcher is used to make changes to the UI thread from non UI Thread.</span></span> <span data-ttu-id="ba056-253">V případě úzkým místem na vlákno dispečera můžete vybrat vývojáře použít dispečera poskytované si chtít aktualizovat prvek uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="ba056-253">In case of bottleneck on dispatcher thread, developer can choose to use dispatcher provided by UI-element he/she intends to update.</span></span>  <span data-ttu-id="ba056-254">Například:</span><span class="sxs-lookup"><span data-stu-id="ba056-254">For example:</span></span>
   
         await sliderProgress.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () => { TimeSpan 

         timespan = new TimeSpan(adaptiveSourceStatusUpdate.EndTime); 
         double absvalue  = (int)Math.Round(timespan.TotalSeconds, MidpointRounding.AwayFromZero); 

         sliderProgress.Maximum = absvalue; }); 
6. <span data-ttu-id="ba056-255">Na konci **mediaElement_AdaptiveSourceStatusUpdated** metoda, přidejte následující kód:</span><span class="sxs-lookup"><span data-stu-id="ba056-255">At the end of the **mediaElement_AdaptiveSourceStatusUpdated** method, add the following code:</span></span>

         setSliderStartTime(args.StartTime);
         setSliderEndTime(args.EndTime);
7. <span data-ttu-id="ba056-256">Na konci **MediaOpened** metoda, přidejte následující kód:</span><span class="sxs-lookup"><span data-stu-id="ba056-256">At the end of the **MediaOpened** method, add the following code:</span></span>

         sliderProgress.StepFrequency = SliderFrequency(mediaElement.NaturalDuration.TimeSpan);
         sliderProgress.Width = mediaElement.Width;
         setupTimer();
8. <span data-ttu-id="ba056-257">Stiskněte klávesu **CTRL + S** k uložení souboru.</span><span class="sxs-lookup"><span data-stu-id="ba056-257">Press **CTRL+S** to save the file.</span></span>

<span data-ttu-id="ba056-258">**Pro zkompilování a testování aplikace**</span><span class="sxs-lookup"><span data-stu-id="ba056-258">**To compile and test the application**</span></span>

1. <span data-ttu-id="ba056-259">Stiskněte klávesu **F6** kompilace projektu.</span><span class="sxs-lookup"><span data-stu-id="ba056-259">Press **F6** to compile the project.</span></span> 
2. <span data-ttu-id="ba056-260">Stisknutím klávesy **F5** spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ba056-260">Press **F5** to run the application.</span></span>
3. <span data-ttu-id="ba056-261">V horní části aplikace můžete buď použít výchozí nastavení adresy URL technologie Smooth Streaming nebo zadejte jiný.</span><span class="sxs-lookup"><span data-stu-id="ba056-261">At the top of the application, you can either use the default Smooth Streaming URL or enter a different one.</span></span> 
4. <span data-ttu-id="ba056-262">Klikněte na tlačítko **nastavit zdroj**.</span><span class="sxs-lookup"><span data-stu-id="ba056-262">Click **Set Source**.</span></span> 
5. <span data-ttu-id="ba056-263">Otestujte posuvníku.</span><span class="sxs-lookup"><span data-stu-id="ba056-263">Test the slider bar.</span></span>

<span data-ttu-id="ba056-264">Když jste dokončili Lekce 2.</span><span class="sxs-lookup"><span data-stu-id="ba056-264">You have completed lesson 2.</span></span>  <span data-ttu-id="ba056-265">V této lekci přidané jezdce do aplikace.</span><span class="sxs-lookup"><span data-stu-id="ba056-265">In this lesson you added a slider to application.</span></span> 

## <a name="lesson-3-select-smooth-streaming-streams"></a><span data-ttu-id="ba056-266">Lekce 3: Vyberte vysílání funkce Smooth Streaming datové proudy</span><span class="sxs-lookup"><span data-stu-id="ba056-266">Lesson 3: Select Smooth Streaming Streams</span></span>
<span data-ttu-id="ba056-267">Technologie Smooth Streaming je umožňující obsah datového proudu s více zvukových stop jazyk, které jsou lze vybrat pomocí prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="ba056-267">Smooth Streaming is capable to stream content with multiple language audio tracks that are selectable by the viewers.</span></span>  <span data-ttu-id="ba056-268">V této lekci povolíte prohlížečům vyberte datových proudů.</span><span class="sxs-lookup"><span data-stu-id="ba056-268">In this lesson, you will enable viewers to select streams.</span></span> <span data-ttu-id="ba056-269">V této lekci obsahuje následující postupy:</span><span class="sxs-lookup"><span data-stu-id="ba056-269">This lesson contains the following procedures:</span></span>

1. <span data-ttu-id="ba056-270">Upravte soubor XAML</span><span class="sxs-lookup"><span data-stu-id="ba056-270">Modify the XAML file</span></span>
2. <span data-ttu-id="ba056-271">Upravte soubor behand kódu</span><span class="sxs-lookup"><span data-stu-id="ba056-271">Modify the code behand file</span></span>
3. <span data-ttu-id="ba056-272">Kompilace a testování aplikace</span><span class="sxs-lookup"><span data-stu-id="ba056-272">Compile and test the application</span></span>

<span data-ttu-id="ba056-273">**K úpravě souboru XAML**</span><span class="sxs-lookup"><span data-stu-id="ba056-273">**To modify the XAML file**</span></span>

1. <span data-ttu-id="ba056-274">V Průzkumníku řešení klikněte pravým tlačítkem **MainPage.xaml**a potom klikněte na **Návrhář zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="ba056-274">From Solution Explorer, right-click **MainPage.xaml**, and then click **View Designer**.</span></span>
2. <span data-ttu-id="ba056-275">Vyhledejte &lt;Grid.RowDefinitions&gt;a upravte RowDefinitions tak bude vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="ba056-275">Locate &lt;Grid.RowDefinitions&gt;, and modify the RowDefinitions so they looks like:</span></span>
   
         <Grid.RowDefinitions>            
            <RowDefinition Height="20"/>
            <RowDefinition Height="50"/>
            <RowDefinition Height="100"/>
            <RowDefinition Height="80"/>
            <RowDefinition Height="50"/>
         </Grid.RowDefinitions>
3. <span data-ttu-id="ba056-276">Uvnitř &lt;mřížky&gt;&lt;/Grid&gt; značky, přidejte následující kód k definování ListBox – ovládací prvek, aby uživatelé mohli zobrazit seznam dostupných datových proudů a vybrat datové proudy:</span><span class="sxs-lookup"><span data-stu-id="ba056-276">Inside the &lt;Grid&gt;&lt;/Grid&gt; tags, add the following code to define a listbox control, so users can see the list of available streams, and select streams:</span></span>

         <Grid Name="gridStreamAndBitrateSelection" Grid.Row="3">
            <Grid.RowDefinitions>
                <RowDefinition Height="300"/>
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="250*"/>
                <ColumnDefinition Width="250*"/>
            </Grid.ColumnDefinitions>
            <StackPanel Name="spStreamSelection" Grid.Row="1" Grid.Column="0">
                <StackPanel Orientation="Horizontal">
                    <TextBlock Name="tbAvailableStreams" Text="Available Streams:" FontSize="16" VerticalAlignment="Center"></TextBlock>
                    <Button Name="btnChangeStreams" Content="Submit" Click="btnChangeStream_Click"/>
                </StackPanel>
                <ListBox x:Name="lbAvailableStreams" Height="200" Width="200" Background="Transparent" HorizontalAlignment="Left" 
                    ScrollViewer.VerticalScrollMode="Enabled" ScrollViewer.VerticalScrollBarVisibility="Visible">
                    <ListBox.ItemTemplate>
                        <DataTemplate>
                            <CheckBox Content="{Binding Name}" IsChecked="{Binding isChecked, Mode=TwoWay}" />
                        </DataTemplate>
                    </ListBox.ItemTemplate>
                </ListBox>
            </StackPanel>
         </Grid>
4. <span data-ttu-id="ba056-277">Stiskněte klávesu **CTRL + S** a uložte změny.</span><span class="sxs-lookup"><span data-stu-id="ba056-277">Press **CTRL+S** to save the changes.</span></span>

<span data-ttu-id="ba056-278">**K úpravě souboru kódu**</span><span class="sxs-lookup"><span data-stu-id="ba056-278">**To modify the code behind file**</span></span>

1. <span data-ttu-id="ba056-279">V Průzkumníku řešení klikněte pravým tlačítkem **MainPage.xaml**a potom klikněte na **kód zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="ba056-279">From Solution Explorer, right-click **MainPage.xaml**, and then click **View Code**.</span></span>
2. <span data-ttu-id="ba056-280">Uvnitř SSPlayer oboru názvů přidejte novou třídu:</span><span class="sxs-lookup"><span data-stu-id="ba056-280">Inside the SSPlayer namespace, add a new class:</span></span>
   
        #region class Stream
   
        public class Stream
        {
            private IManifestStream stream;
            public bool isCheckedValue;
            public string name;
   
            public string Name
            {
                get { return name; }
                set { name = value; }
            }
   
            public IManifestStream ManifestStream
            {
                get { return stream; }
                set { stream = value; }
            }
   
            public bool isChecked
            {
                get { return isCheckedValue; }
                set
                {
                    // mMke the video stream always checked.
                    if (stream.Type == MediaStreamType.Video)
                    {
                        isCheckedValue = true;
                    }
                    else
                    {
                        isCheckedValue = value;
                    }
                }
            }
   
            public Stream(IManifestStream streamIn)
            {
                stream = streamIn;
                name = stream.Name;
            }
        }
        #endregion class Stream
3. <span data-ttu-id="ba056-281">Na začátku MainPage třídy, přidejte následující definice proměnné:</span><span class="sxs-lookup"><span data-stu-id="ba056-281">At the beginning of the MainPage class, add the following variable definitions:</span></span>
   
         private List<Stream> availableStreams;
         private List<Stream> availableAudioStreams;
         private List<Stream> availableTextStreams;
         private List<Stream> availableVideoStreams;
4. <span data-ttu-id="ba056-282">Uvnitř MainPage třídy přidejte následující oblasti:</span><span class="sxs-lookup"><span data-stu-id="ba056-282">Inside the MainPage class, add the following region:</span></span>
   
        #region stream selection
        ///<summary>
        ///Functionality to select streams from IManifestStream available streams
        /// </summary>
   
        // This function is called from the mediaElement_ManifestReady event handler 
        // to retrieve the streams and populate them to the local data members.
        public void getStreams(Manifest manifestObject)
        {
            availableStreams = new List<Stream>();
            availableVideoStreams = new List<Stream>();
            availableAudioStreams = new List<Stream>();
            availableTextStreams = new List<Stream>();
   
            try
            {
                for (int i = 0; i<manifestObject.AvailableStreams.Count; i++)
                {
                    Stream newStream = new Stream(manifestObject.AvailableStreams[i]);
                    newStream.isChecked = false;
   
                    //populate the stream lists based on the types
                    availableStreams.Add(newStream);
   
                    switch (newStream.ManifestStream.Type)
                    {
                        case MediaStreamType.Video:
                            availableVideoStreams.Add(newStream);
                            break;
                        case MediaStreamType.Audio:
                            availableAudioStreams.Add(newStream);
                            break;
                        case MediaStreamType.Text:
                            availableTextStreams.Add(newStream);
                            break;
                    }
   
                    // Select the default selected streams from the manifest.
                    for (int j = 0; j<manifestObject.SelectedStreams.Count; j++)
                    {
                        string selectedStreamName = manifestObject.SelectedStreams[j].Name;
                        if (selectedStreamName.Equals(newStream.Name))
                        {
                            newStream.isChecked = true;
                            break;
                        }
                    }
                }
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }
        }
   
        // This function set the list box ItemSource
        private async void refreshAvailableStreamsListBoxItemSource()
        {
            try
            {
                //update the stream check box list on the UI
                await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, ()
                    => { lbAvailableStreams.ItemsSource = availableStreams; });
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }
        }
   
        // This function creates a selected streams list
        private void createSelectedStreamsList(List<IManifestStream> selectedStreams)
        {
            bool isOneVideoSelected = false;
            bool isOneAudioSelected = false;
   
            // Only one video stream can be selected
            for (int j = 0; j<availableVideoStreams.Count; j++)
            {
                if (availableVideoStreams[j].isChecked && (!isOneVideoSelected))
                {
                    selectedStreams.Add(availableVideoStreams[j].ManifestStream);
                    isOneVideoSelected = true;
                }
            }
   
            // Select the frist video stream from the list if no video stream is selected
            if (!isOneVideoSelected)
            {
                availableVideoStreams[0].isChecked = true;
                selectedStreams.Add(availableVideoStreams[0].ManifestStream);
            }
   
            // Only one audio stream can be selected
            for (int j = 0; j<availableAudioStreams.Count; j++)
            {
                if (availableAudioStreams[j].isChecked && (!isOneAudioSelected))
                {
                    selectedStreams.Add(availableAudioStreams[j].ManifestStream);
                    isOneAudioSelected = true;
                    txtStatus.Text = "The audio stream is changed to " + availableAudioStreams[j].ManifestStream.Name;
                }
            }
   
            // Select the frist audio stream from the list if no audio steam is selected.
            if (!isOneAudioSelected)
            {
                availableAudioStreams[0].isChecked = true;
                selectedStreams.Add(availableAudioStreams[0].ManifestStream);
            }
   
            // Multiple text streams are supported.
            for (int j = 0; j < availableTextStreams.Count; j++)
            {
                if (availableTextStreams[j].isChecked)
                {
                    selectedStreams.Add(availableTextStreams[j].ManifestStream);
                }
            }
        }
   
        // Change streams on a smooth streaming presentation with multiple video streams.
        private async void changeStreams(List<IManifestStream> selectStreams)
        {
            try
            {
                IReadOnlyList<IStreamChangedResult> returnArgs =
                    await manifestObject.SelectStreamsAsync(selectStreams);
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }
        }
        #endregion stream selection
5. <span data-ttu-id="ba056-283">Metoda mediaElement_ManifestReady najít, doplňovací následující kód na konci funkce:</span><span class="sxs-lookup"><span data-stu-id="ba056-283">Locate the mediaElement_ManifestReady method, append the following code at the end of the function:</span></span>
   
        getStreams(manifestObject);
        refreshAvailableStreamsListBoxItemSource();
   
    <span data-ttu-id="ba056-284">Proto když MediaElement manifest je připraven, kód získá seznam dostupných datových proudů a naplní pole se seznamem uživatelského rozhraní se seznamem.</span><span class="sxs-lookup"><span data-stu-id="ba056-284">So when MediaElement manifest is ready, the code gets a list of the available streams, and populates the UI list box with the list.</span></span>
6. <span data-ttu-id="ba056-285">Uvnitř MainPage třídy, vyhledejte rozhraní tlačítka klikněte na události oblast a potom přidejte následující definice funkce:</span><span class="sxs-lookup"><span data-stu-id="ba056-285">Inside the MainPage class, locate the UI buttons click events region, and then add the following function definition:</span></span>
   
        private void btnChangeStream_Click(object sender, RoutedEventArgs e)
        {
            List<IManifestStream> selectedStreams = new List<IManifestStream>();
   
            // Create a list of the selected streams
            createSelectedStreamsList(selectedStreams);
   
            // Change streams on the presentation
            changeStreams(selectedStreams);
        }

<span data-ttu-id="ba056-286">**Pro zkompilování a testování aplikace**</span><span class="sxs-lookup"><span data-stu-id="ba056-286">**To compile and test the application**</span></span>

1. <span data-ttu-id="ba056-287">Stiskněte klávesu **F6** kompilace projektu.</span><span class="sxs-lookup"><span data-stu-id="ba056-287">Press **F6** to compile the project.</span></span> 
2. <span data-ttu-id="ba056-288">Stisknutím klávesy **F5** spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ba056-288">Press **F5** to run the application.</span></span>
3. <span data-ttu-id="ba056-289">V horní části aplikace můžete buď použít výchozí nastavení adresy URL technologie Smooth Streaming nebo zadejte jiný.</span><span class="sxs-lookup"><span data-stu-id="ba056-289">At the top of the application, you can either use the default Smooth Streaming URL or enter a different one.</span></span> 
4. <span data-ttu-id="ba056-290">Klikněte na tlačítko **nastavit zdroj**.</span><span class="sxs-lookup"><span data-stu-id="ba056-290">Click **Set Source**.</span></span> 
5. <span data-ttu-id="ba056-291">Výchozí jazyk je audio_eng.</span><span class="sxs-lookup"><span data-stu-id="ba056-291">The default language is audio_eng.</span></span> <span data-ttu-id="ba056-292">Zkuste přepínat mezi audio_eng a audio_es.</span><span class="sxs-lookup"><span data-stu-id="ba056-292">Try to switch between audio_eng and audio_es.</span></span> <span data-ttu-id="ba056-293">Při, vyberte nového datového proudu, musíte kliknout na tlačítko pro odeslání.</span><span class="sxs-lookup"><span data-stu-id="ba056-293">Everytime, you select a new stream, you must click the Submit button.</span></span>

<span data-ttu-id="ba056-294">Když jste dokončili Lekce 3.</span><span class="sxs-lookup"><span data-stu-id="ba056-294">You have completed lesson 3.</span></span>  <span data-ttu-id="ba056-295">V této lekci přidáte funkci zvolit datové proudy.</span><span class="sxs-lookup"><span data-stu-id="ba056-295">In this lesson, you add the functionality to choose streams.</span></span>

## <a name="lesson-4-select-smooth-streaming-tracks"></a><span data-ttu-id="ba056-296">Lekce 4: Vyberte vysílání funkce Smooth Streaming sleduje</span><span class="sxs-lookup"><span data-stu-id="ba056-296">Lesson 4: Select Smooth Streaming Tracks</span></span>
<span data-ttu-id="ba056-297">Prezentace technologie Smooth Streaming může obsahovat více videosouborů zakódovaných pomocí různé úrovně kvality (přenosové rychlosti) a jejich řešení.</span><span class="sxs-lookup"><span data-stu-id="ba056-297">A Smooth Streaming presentation can contain multiple video files encoded with different quality levels (bit rates) and resolutions.</span></span> <span data-ttu-id="ba056-298">V této lekci povolíte uživatelům vybrat sleduje.</span><span class="sxs-lookup"><span data-stu-id="ba056-298">In this lesson, you will enable users to select tracks.</span></span> <span data-ttu-id="ba056-299">V této lekci obsahuje následující postupy:</span><span class="sxs-lookup"><span data-stu-id="ba056-299">This lesson contains the following procedures:</span></span>

1. <span data-ttu-id="ba056-300">Upravte soubor XAML</span><span class="sxs-lookup"><span data-stu-id="ba056-300">Modify the XAML file</span></span>
2. <span data-ttu-id="ba056-301">Upravte soubor behand kódu</span><span class="sxs-lookup"><span data-stu-id="ba056-301">Modify the code behand file</span></span>
3. <span data-ttu-id="ba056-302">Kompilace a testování aplikace</span><span class="sxs-lookup"><span data-stu-id="ba056-302">Compile and test the application</span></span>

<span data-ttu-id="ba056-303">**K úpravě souboru XAML**</span><span class="sxs-lookup"><span data-stu-id="ba056-303">**To modify the XAML file**</span></span>

1. <span data-ttu-id="ba056-304">V Průzkumníku řešení klikněte pravým tlačítkem **MainPage.xaml**a potom klikněte na **Návrhář zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="ba056-304">From Solution Explorer, right-click **MainPage.xaml**, and then click **View Designer**.</span></span>
2. <span data-ttu-id="ba056-305">Vyhledejte &lt;mřížky&gt; značky s názvem **gridStreamAndBitrateSelection**, přidejte následující text na konci značky:</span><span class="sxs-lookup"><span data-stu-id="ba056-305">Locate the &lt;Grid&gt; tag with the name **gridStreamAndBitrateSelection**, append the following code at the end of the tag:</span></span>
   
         <StackPanel Name="spBitRateSelection" Grid.Row="1" Grid.Column="1">
         <StackPanel Orientation="Horizontal">
             <TextBlock Name="tbBitRate" Text="Available Bitrates:" FontSize="16" VerticalAlignment="Center"/>
             <Button Name="btnChangeTracks" Content="Submit" Click="btnChangeTrack_Click" />
         </StackPanel>
         <ListBox x:Name="lbAvailableVideoTracks" Height="200" Width="200" Background="Transparent" HorizontalAlignment="Left" 
                  ScrollViewer.VerticalScrollMode="Enabled" ScrollViewer.VerticalScrollBarVisibility="Visible">
             <ListBox.ItemTemplate>
                 <DataTemplate>
                     <CheckBox Content="{Binding Bitrate}" IsChecked="{Binding isChecked, Mode=TwoWay}"/>
                 </DataTemplate>
             </ListBox.ItemTemplate>
         </ListBox>
         </StackPanel>
3. <span data-ttu-id="ba056-306">Stiskněte klávesu **CTRL + S** se uložit změny he</span><span class="sxs-lookup"><span data-stu-id="ba056-306">Press **CTRL+S** to save he changes</span></span>

<span data-ttu-id="ba056-307">**K úpravě souboru kódu**</span><span class="sxs-lookup"><span data-stu-id="ba056-307">**To modify the code behind file**</span></span>

1. <span data-ttu-id="ba056-308">V Průzkumníku řešení klikněte pravým tlačítkem **MainPage.xaml**a potom klikněte na **kód zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="ba056-308">From Solution Explorer, right-click **MainPage.xaml**, and then click **View Code**.</span></span>
2. <span data-ttu-id="ba056-309">Uvnitř SSPlayer oboru názvů přidejte novou třídu:</span><span class="sxs-lookup"><span data-stu-id="ba056-309">Inside the SSPlayer namespace, add a new class:</span></span>
   
        #region class Track
        public class Track
        {
            private IManifestTrack trackInfo;
            public string _bitrate;
            public bool isCheckedValue;
   
            public IManifestTrack TrackInfo
            {
                get { return trackInfo; }
                set { trackInfo = value; }
            }
   
            public string Bitrate
            {
                get { return _bitrate; }
                set { _bitrate = value; }
            }
   
            public bool isChecked
            {
                get { return isCheckedValue; }
                set
                {
                    isCheckedValue = value;
                }
            }
   
            public Track(IManifestTrack trackInfoIn)
            {
                trackInfo = trackInfoIn;
                _bitrate = trackInfoIn.Bitrate.ToString();
            }
            //public Track() { }
        }
        #endregion class Track
3. <span data-ttu-id="ba056-310">Na začátku MainPage třídy, přidejte následující definice proměnné:</span><span class="sxs-lookup"><span data-stu-id="ba056-310">At the beginning of the MainPage class, add the following variable definitions:</span></span>
   
        private List<Track> availableTracks;
4. <span data-ttu-id="ba056-311">Uvnitř MainPage třídy přidejte následující oblasti:</span><span class="sxs-lookup"><span data-stu-id="ba056-311">Inside the MainPage class, add the following region:</span></span>
   
        #region track selection
        /// <summary>
        /// Functionality to select video streams
        /// </summary>
   
        /// This Function gets the tracks for the selected video stream
        public void getTracks(Manifest manifestObject)
        {
            availableTracks = new List<Track>();
   
            IManifestStream videoStream = getVideoStream();
            IReadOnlyList<IManifestTrack> availableTracksLocal = videoStream.AvailableTracks;
            IReadOnlyList<IManifestTrack> selectedTracksLocal = videoStream.SelectedTracks;
   
            try
            {
                for (int i = 0; i < availableTracksLocal.Count; i++)
                {
                    Track thisTrack = new Track(availableTracksLocal[i]);
                    thisTrack.isChecked = true;
   
                    for (int j = 0; j < selectedTracksLocal.Count; j++)
                    {
                        string selectedTrackName = selectedTracksLocal[j].Bitrate.ToString();
                        if (selectedTrackName.Equals(thisTrack.Bitrate))
                        {
                            thisTrack.isChecked = true;
                            break;
                        }
                    }
                    availableTracks.Add(thisTrack);
                }
            }
            catch (Exception e)
            {
                txtStatus.Text = e.Message;
            }
        }
   
        // This function gets the video stream that is playing
        private IManifestStream getVideoStream()
        {
            IManifestStream videoStream = null;
            for (int i = 0; i < manifestObject.SelectedStreams.Count; i++)
            {
                if (manifestObject.SelectedStreams[i].Type == MediaStreamType.Video)
                {
                    videoStream = manifestObject.SelectedStreams[i];
                    break;
                }
            }
            return videoStream;
        }
   
        // This function set the UI list box control ItemSource
        private async void refreshAvailableTracksListBoxItemSource()
        {
            try
            {
                // Update the track check box list on the UI 
                await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, ()
                    => { lbAvailableVideoTracks.ItemsSource = availableTracks; });
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }        
        }
   
        // This function creates a list of the selected tracks.
        private void createSelectedTracksList(List<IManifestTrack> selectedTracks)
        {
            // Create a list of selected tracks
            for (int j = 0; j < availableTracks.Count; j++)
            {
                if (availableTracks[j].isCheckedValue == true)
                {
                    selectedTracks.Add(availableTracks[j].TrackInfo);
                }
            }
        }
   
        // This function selects the tracks based on user selection 
        private void changeTracks(List<IManifestTrack> selectedTracks)
        {
            IManifestStream videoStream = getVideoStream();
            try
            {
                videoStream.SelectTracks(selectedTracks);
            }
            catch (Exception ex)
            {
                txtStatus.Text = ex.Message;
            }
        }
        #endregion track selection
5. <span data-ttu-id="ba056-312">Metoda mediaElement_ManifestReady najít, doplňovací následující kód na konci funkce:</span><span class="sxs-lookup"><span data-stu-id="ba056-312">Locate the mediaElement_ManifestReady method, append the following code at the end of the function:</span></span>
   
         getTracks(manifestObject);
         refreshAvailableTracksListBoxItemSource();
6. <span data-ttu-id="ba056-313">Uvnitř MainPage třídy, vyhledejte rozhraní tlačítka klikněte na události oblast a potom přidejte následující definice funkce:</span><span class="sxs-lookup"><span data-stu-id="ba056-313">Inside the MainPage class, locate the UI buttons click events region, and then add the following function definition:</span></span>
   
         private void btnChangeStream_Click(object sender, RoutedEventArgs e)
         {
            List<IManifestStream> selectedStreams = new List<IManifestStream>();

            // Create a list of the selected streams
            createSelectedStreamsList(selectedStreams);

            // Change streams on the presentation
            changeStreams(selectedStreams);
         }

<span data-ttu-id="ba056-314">**Pro zkompilování a testování aplikace**</span><span class="sxs-lookup"><span data-stu-id="ba056-314">**To compile and test the application**</span></span>

1. <span data-ttu-id="ba056-315">Stiskněte klávesu **F6** kompilace projektu.</span><span class="sxs-lookup"><span data-stu-id="ba056-315">Press **F6** to compile the project.</span></span> 
2. <span data-ttu-id="ba056-316">Stisknutím klávesy **F5** spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ba056-316">Press **F5** to run the application.</span></span>
3. <span data-ttu-id="ba056-317">V horní části aplikace můžete buď použít výchozí nastavení adresy URL technologie Smooth Streaming nebo zadejte jiný.</span><span class="sxs-lookup"><span data-stu-id="ba056-317">At the top of the application, you can either use the default Smooth Streaming URL or enter a different one.</span></span> 
4. <span data-ttu-id="ba056-318">Klikněte na tlačítko **nastavit zdroj**.</span><span class="sxs-lookup"><span data-stu-id="ba056-318">Click **Set Source**.</span></span> 
5. <span data-ttu-id="ba056-319">Ve výchozím nastavení jsou vybrány všechny sleduje video datového proudu.</span><span class="sxs-lookup"><span data-stu-id="ba056-319">By default, all of the tracks of the video stream are selected.</span></span> <span data-ttu-id="ba056-320">Experimentovat míry změny bit, můžete vybrat nejnižší přenosová rychlost k dispozici a pak vyberte nejvyšší přenosovou rychlost, k dispozici.</span><span class="sxs-lookup"><span data-stu-id="ba056-320">To experiment the bit rate changes, you can select the lowest bit rate available, and then select the highest bit rate available.</span></span> <span data-ttu-id="ba056-321">Musíte kliknout na odeslání po každé změně.</span><span class="sxs-lookup"><span data-stu-id="ba056-321">You must click Submit after each change.</span></span>  <span data-ttu-id="ba056-322">Zobrazí se změny kvalitu videa.</span><span class="sxs-lookup"><span data-stu-id="ba056-322">You can see the video quality changes.</span></span>

<span data-ttu-id="ba056-323">Když jste dokončili Lekce 4.</span><span class="sxs-lookup"><span data-stu-id="ba056-323">You have completed lesson 4.</span></span>  <span data-ttu-id="ba056-324">V této lekci přidáte funkci vybrat sleduje.</span><span class="sxs-lookup"><span data-stu-id="ba056-324">In this lesson, you add the functionality to choose tracks.</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="ba056-325">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="ba056-325">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="ba056-326">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="ba056-326">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="other-resources"></a><span data-ttu-id="ba056-327">Další prostředky:</span><span class="sxs-lookup"><span data-stu-id="ba056-327">Other Resources:</span></span>
* [<span data-ttu-id="ba056-328">Jak sestavit aplikaci technologie Smooth Streaming JavaScript Windows 8 s pokročilé funkce</span><span class="sxs-lookup"><span data-stu-id="ba056-328">How to build a Smooth Streaming Windows 8 JavaScript application with advanced features</span></span>](http://blogs.iis.net/cenkd/archive/2012/08/10/how-to-build-a-smooth-streaming-windows-8-javascript-application-with-advanced-features.aspx)
* [<span data-ttu-id="ba056-329">Technologie Smooth Streaming technický přehled</span><span class="sxs-lookup"><span data-stu-id="ba056-329">Smooth Streaming Technical Overview</span></span>](http://www.iis.net/learn/media/on-demand-smooth-streaming/smooth-streaming-technical-overview)

[PlayerApplication]: ./media/media-services-build-smooth-streaming-apps/SSClientWin8-1.png
[CodeViewPic]: ./media/media-services-build-smooth-streaming-apps/SSClientWin8-2.png

