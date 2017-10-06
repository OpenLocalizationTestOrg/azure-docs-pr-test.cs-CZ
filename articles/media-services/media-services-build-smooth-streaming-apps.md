---
title: aaaSmooth Streaming Windows Store aplikace kurzu | Microsoft Docs
description: "Zjistěte, jak řídit toouse Azure Media Services toocreate aplikace Windows Store jazyka C# s XML MediaElement tooplayback datový proud Smooth obsah."
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
ms.openlocfilehash: b02aa2c7f68fe22a23ea846d72fdd23bfba2b19c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toobuild-a-smooth-streaming-windows-store-application"></a>Jak tooBuild technologie Smooth Streaming aplikaci pro Windows Store

Hello technologie Smooth Streaming klienta SDK pro Windows 8 povolí vývojáři toobuild aplikace Windows Store můžete přehrát na vyžádání i živé vysílání funkce Smooth Streaming obsah. Kromě základních přehrávání toohello funkce Smooth Streaming obsahu, hello SDK také poskytuje bohaté funkce jako je Microsoft PlayReady ochrany, omezení úrovně kvality, živé formátu DVR, zvukový datový proud přepínání, naslouchá stav aktualizací (jako jsou například změny úrovně kvality ) a chybové události a tak dále. Další informace podporované hello funkcí najdete v tématu hello [poznámky k verzi](http://www.iis.net/learn/media/smooth-streaming/smooth-streaming-client-sdk-for-windows-8-release-notes). Další informace najdete v tématu [Player Framework pro Windows 8](http://playerframework.codeplex.com/). 

Tento kurz obsahuje čtyři lekce:

1. Vytvořte základní technologie Smooth Streaming aplikace úložiště
2. Přidat posuvníku panelu tooControl hello průběh média
3. Vyberte vysílání funkce Smooth Streaming datové proudy
4. Vyberte vysílání funkce Smooth Streaming sleduje

## <a name="prerequisites"></a>Požadavky
* Windows 8 32bitové nebo 64bitové verze. Můžete získat [zkušební verze Windows 8 Enterprise](http://msdn.microsoft.com/evalcenter/jj554510.aspx) z webu MSDN.
* Visual Studio 2012 nebo Visual Studio Express 2012 (nebo novější). Můžete získat zkušební verzi hello z [zde](http://www.microsoft.com/visualstudio/11/downloads).
* [Microsoft klienta funkce Smooth Streaming SDK pro systém Windows 8](http://visualstudiogallery.msdn.microsoft.com/04423d13-3b3e-4741-a01c-1ae29e84fea6?SRC=Homehttp://visualstudiogallery.msdn.microsoft.com/04423d13-3b3e-4741-a01c-1ae29e84fea6?SRC=Home).

Hello dokončit řešení pro každé lekce si můžete stáhnout z ukázky kódu vývojáře MSDN (Galerie kódů): 

* [Lekce 1](http://code.msdn.microsoft.com/Smooth-Streaming-Client-0bb1471f) – jednoduchý systém Windows 8 funkce Smooth Streaming Media Player 
* [Lekce 2](http://code.msdn.microsoft.com/A-simple-Windows-8-Smooth-ee98f63a) – jednoduchý systém Windows 8 funkce Smooth Streaming Media Player pomocí jezdce lze ovládací prvek, 
* [Lekce 3](http://code.msdn.microsoft.com/A-Windows-8-Smooth-883c3b44) – Windows 8 funkce Smooth Streaming Media Player s výběrem datového proudu  
* [Lekce 4](http://code.msdn.microsoft.com/A-Windows-8-Smooth-aa9e4907) – Windows 8 funkce Smooth Streaming Media Player s výběrem sledovat.

## <a name="lesson-1-create-a-basic-smooth-streaming-store-application"></a>Lekce 1: Vytvoření základní technologie Smooth Streaming aplikace úložiště

V této lekci vytvoříte aplikaci pro Windows Store s MediaElement řízení tooplay Smooth datového proudu obsahu.  spuštěné aplikace Hello vypadá takto:

![Příklad aplikace Smooth Streaming Windows Store][PlayerApplication]

Další informace o vývoji aplikací Windows Store najdete v tématu [vývoji kvalitních aplikací pro Windows 8](http://msdn.microsoft.com/windows/apps/br229512.aspx). V této lekci obsahuje hello následující postupy:

1. Vytvoření projektu Windows Store
2. Návrh hello uživatelské rozhraní (XAML)
3. Úprava souboru hello kódu
4. Kompilace a testování aplikace hello

**toocreate projekt Windows Store**

1. Spusťte Visual Studio 2012 nebo novějším.
2. Z hello **soubor** nabídky, klikněte na tlačítko **nový**a potom klikněte na **projektu**.
3. Z dialogového okna Nový projekt hello typu nebo vyberte hello následující hodnoty:

| Name (Název) | Hodnota |
| --- | --- |
| Skupina šablon |Nainstalovaná, šablony/Visual C# / Windows Store |
| Šablona |Prázdná aplikace (XAML) |
| Name (Název) |SSPlayer |
| Umístění |C:\SSTutorials |
| Název řešení |SSPlayer |
| Vytvořte adresář pro řešení |(zaškrtnuto) |

1. Klikněte na **OK**.

**tooadd toohello referenční dokumentace technologie Smooth Streaming Client SDK**

1. V Průzkumníku řešení klikněte pravým tlačítkem **SSPlayer**a potom klikněte na **přidat odkaz na**.
2. Zadejte nebo vyberte hello následující hodnoty:

| Name (Název) | Hodnota |
| --- | --- |
| Odkaz na skupinu |Rozšíření/Windows |
| Referenční informace |Vyberte Microsoft klienta funkce Smooth Streaming SDK pro systém Windows 8 a balíček Microsoft Visual C++ Runtime |

1. Klikněte na **OK**. 

Po přidání hello odkazy, je nutné vybrat hello cílové platformy (x64 nebo x86), přidávání odkazů nebude fungovat pro jakýkoli procesor platformy konfiguraci.  V Průzkumníku řešení zobrazí se žlutý označit upozornění pro tyto přidat odkazy.

**toodesign hello player uživatelského rozhraní**

1. V Průzkumníku řešení poklikejte na **MainPage.xaml** tooopen v návrhu hello zobrazení.
2. Vyhledejte hello  **&lt;mřížky&gt;**  a  **&lt;/Grid&gt;**  značky hello souboru XAML a vložte následující hello kód mezi hello dvě značky:

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
   
   Hello prvek MediaElement je použité tooplayback média. Hello posuvník s názvem sliderProgress se použije probíhá hello Další lekce toocontrol hello média.
3. Stiskněte klávesu **CTRL + S** toosave hello souboru.

Hello prvek MediaElement nepodporuje vysílání funkce Smooth Streaming obsahu out-of-box. Podpora technologie Smooth Streaming tooenable hello je nutné zaregistrovat obslužná rutina hello technologie Smooth Streaming byte-stream příponu názvu souboru a typ MIME.  tooregister, použijte metodu MediaExtensionManager.RegisterByteStremHandler hello hello Windows.Media oboru názvů.

V tomto souboru XAML některé obslužné rutiny událostí jsou přidruženy k hello ovládací prvky.  Je nutné zadat tyto obslužné rutiny událostí.

**toomodify hello souboru kódu**

1. V Průzkumníku řešení klikněte pravým tlačítkem **MainPage.xaml**a potom klikněte na **kód zobrazení**.
2. Horní hello hello souboru přidejte následující hello pomocí příkazu:
   
        using Windows.Media;
3. Na začátku hello hello **MainPage** třídy, přidejte následující – datový člen hello:
   
         private MediaExtensionManager extensions = new MediaExtensionManager();
4. Na konci hello hello **MainPage** konstruktoru, přidejte následující dva řádky hello:
   
        extensions.RegisterByteStreamHandler("Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", ".ism", "text/xml");
        extensions.RegisterByteStreamHandler("Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", ".ism", "application/vnd.ms-sstr+xml");
5. Na konci hello hello **MainPage** třídy, vložte následující kód hello:
   
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
             txtStatus.Text = "Click hello Play button tooplay hello media source.";
         }
         }
         private void btnStop_Click(object sender, RoutedEventArgs e)
         {

         mediaElement.Stop();
         txtStatus.Text = "MediaElement is stopped";
         }
         private void sliderProgress_PointerPressed(object sender, PointerRoutedEventArgs e)
         {

         txtStatus.Text = "Seek tooposition " + sliderProgress.Value;
         mediaElement.Position = new TimeSpan(0, 0, (int)(sliderProgress.Value));
         }
         # endregion

obslužné rutiny události sliderProgress_PointerPressed Hello je definována v tomto poli.  Existují další tooget toodo funguje ho práce, která se bude vztahovat v další lekci hello tohoto kurzu.
6. Stiskněte klávesu **CTRL + S** toosave hello souboru.

Hello souboru kódu dokončení hello se vypadat takto:

![Codeview v aplikaci Visual Studio o technologie Smooth Streaming Windows Store][CodeViewPic]

**toocompile a testování aplikace hello**

1. Z hello **sestavení** nabídky, klikněte na tlačítko **nástroje Configuration Manager**.
2. Změna **platforma Active řešení** toomatch vývojové platformy.
3. Stiskněte klávesu **F6** toocompile hello projektu. 
4. Stiskněte klávesu **F5** toorun hello aplikace.
5. V horní části hello hello aplikace můžete použít výchozí hello adresy URL technologie Smooth Streaming nebo zadejte jiný. 
6. Klikněte na tlačítko **nastavit zdroj**. Protože **automatické přehrávání** je ve výchozím nastavení, hello média se přehrát automaticky povolené.  Můžete řídit hello média pomocí hello **přehrání**, **pozastavení** a **Zastavit** tlačítka.  Můžete ovládat svazku hello média pomocí posuvníku svislé hello.  Ale hello vodorovné posuvník pro řízení média průběh ještě není implementován plně hello. 

Dokončili jste lesson1.  V této lekci použijete tooplayback řízení MediaElement technologie Smooth Streaming obsah.  V další lekci hello přidáte posuvníku toocontrol hello průběh hello technologie Smooth Streaming obsah.

## <a name="lesson-2-add-a-slider-bar-toocontrol-hello-media-progress"></a>Lekce 2: Přidání posuvníku panelu tooControl hello průběh média

V lekci 1 jste vytvořili aplikaci pro Windows Store MediaElement XAML řízení tooplayback technologie Smooth Streaming mediální obsah.  Některé funkce základní média jako spuštění, zastavení nebo pozastavení pochází.  V této lekci přidáte aplikace toohello posuvníku panelu ovládacího prvku.

V tomto kurzu budeme používat časovač tooupdate hello posuvníku pozice založené na aktuální pozici hello prvek MediaElement hello.  posuvník Hello začínat a končit čas také toobe potřeba aktualizovat v případě živý obsah.  To může lépe ošetřit hello adaptivní zdroj aktualizace události.

Média zdroje jsou objekty, které generují data média.  překladač zdroj Hello přebírá adresu URL nebo bajtů datového proudu a vytvoří hello odpovídající médium zdroje pro obsah.  překladač zdroj Hello je standardní způsob hello zdroje média toocreate aplikace hello. 

V této lekci obsahuje hello následující postupy:

1. Registraci obslužné rutiny technologie Smooth Streaming hello 
2. Přidání obslužných rutin událostí na úrovni hello adaptivní zdroj manager
3. Přidání obslužných rutin událostí na úrovni adaptivní zdroj hello
4. Přidání obslužných rutin událostí MediaElement
5. Přidat posuvníku panelu související kódu
6. Kompilace a testování aplikace hello

**tooregister hello technologie Smooth Streaming byte-stream obslužné rutiny a předejte jí hello propertyset**

1. V Průzkumníku řešení klikněte pravým tlačítkem na **MainPage.xaml**a potom klikněte na **kód zobrazení**.
2. Od začátku hello hello souboru, přidejte hello následující příkaz using:

        using Microsoft.Media.AdaptiveStreaming;
3. Na hello začátku hello MainPage třídy, přidejte hello následující datové členy:

         private Windows.Foundation.Collections.PropertySet propertySet = new Windows.Foundation.Collections.PropertySet();             
         private IAdaptiveSourceManager adaptiveSourceManager;
4. Uvnitř hello **MainPage** konstruktoru, přidejte následující kód po hello hello **to. Inicializace Components();**  řádku a řádky kódu registrace hello napsané v předchozí lekci hello:

        // Gets hello default instance of AdaptiveSourceManager which manages Smooth 
        //Streaming media sources.
        adaptiveSourceManager = AdaptiveSourceManager.GetDefault();
        // Sets property key value tooAdaptiveSourceManager default instance.
        // {A5CE1DE8-1D00-427B-ACEF-FB9A3C93DE2D}" must be hardcoded.
        propertySet["{A5CE1DE8-1D00-427B-ACEF-FB9A3C93DE2D}"] = adaptiveSourceManager;
5. Uvnitř hello **MainPage** konstruktoru, upravte hello dva RegisterByteStreamHandler metody tooadd hello stanovilo parametry:

         // Registers Smooth Streaming byte-stream handler for ".ism" extension and, 
         // "text/xml" and "application/vnd.ms-ss" mime-types and pass hello propertyset. 
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
6. Stiskněte klávesu **CTRL + S** toosave hello souboru.

**tooadd hello adaptivní zdroj manager obslužné rutiny událostí na úrovni**

1. V Průzkumníku řešení klikněte pravým tlačítkem na **MainPage.xaml**a potom klikněte na **kód zobrazení**.
2. Uvnitř hello **MainPage** třídy, přidejte následující – datový člen hello:
   
     privátní adaptiveSource AdaptiveSource = null;
3. Na konci hello hello **MainPage** třídy, přidejte následující obslužné rutiny události hello:
   
         # region Adaptive Source Manager Level Events
         private void mediaElement_AdaptiveSourceOpened(AdaptiveSource sender, AdaptiveSourceOpenedEventArgs args)
         {

            adaptiveSource = args.AdaptiveSource;
         }

         # endregion Adaptive Source Manager Level Events
4. Na konci hello hello **MainPage** konstruktoru, přidejte následující řádek toosubscribe toohello adaptivní zdroj otevřete událostí hello:
   
         adaptiveSourceManager.AdaptiveSourceOpenedEvent += 
           new AdaptiveSourceOpenedEventHandler(mediaElement_AdaptiveSourceOpened);
5. Stiskněte klávesu **CTRL + S** toosave hello souboru.

**obslužné rutiny událostí na úrovni tooadd adaptivní zdroje**

1. V Průzkumníku řešení klikněte pravým tlačítkem na **MainPage.xaml**a potom klikněte na **kód zobrazení**.
2. Uvnitř hello **MainPage** třídy, přidejte následující – datový člen hello:
   
     privátní AdaptiveSourceStatusUpdatedEventArgs adaptiveSourceStatusUpdate;   privátní manifestu manifestObject;
3. Na konci hello hello **MainPage** třídy, přidejte následující obslužné rutiny událostí hello:

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
4. Na konci hello hello **mediaElement AdaptiveSourceOpened** metoda, přidejte následující kód toosubscribe toohello události hello:
   
         adaptiveSource.ManifestReadyEvent +=

                    mediaElement_ManifestReady;
         adaptiveSource.AdaptiveSourceStatusUpdatedEvent += 

            mediaElement_AdaptiveSourceStatusUpdated;
         adaptiveSource.AdaptiveSourceFailedEvent += 

            mediaElement_AdaptiveSourceFailed;
5. Stiskněte klávesu **CTRL + S** toosave hello souboru.

Hello stejné události jsou k dispozici na adaptivní zdroj Manager úrovni také, které lze použít pro zpracování funkce společné tooall média prvky v aplikaci hello. Každý AdaptiveSource zahrnuje vlastní události a všechny události AdaptiveSource předána pod AdaptiveSourceManager.

**obslužné rutiny událostí tooadd Element média**

1. V Průzkumníku řešení klikněte pravým tlačítkem na **MainPage.xaml**a potom klikněte na **kód zobrazení**.
2. Na konci hello hello **MainPage** třídy, přidejte následující obslužné rutiny událostí hello:

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
3. Na konci hello hello **MainPage** konstruktoru, přidejte následující kód toosubscript toohello události hello:

         mediaElement.MediaOpened += MediaOpened;
         mediaElement.MediaEnded += MediaEnded;
         mediaElement.MediaFailed += MediaFailed;
4. Stiskněte klávesu **CTRL + S** toosave hello souboru.

**tooadd posuvníku panelu související kódu**

1. V Průzkumníku řešení klikněte pravým tlačítkem na **MainPage.xaml**a potom klikněte na **kód zobrazení**.
2. Od začátku hello hello souboru, přidejte hello následující příkaz using:
      
        using Windows.UI.Core;
3. Uvnitř hello **MainPage** třídy, přidejte následující členy data hello:
   
         public static CoreDispatcher _dispatcher;
         private DispatcherTimer sliderPositionUpdateDispatcher;
4. Na konci hello hello **MainPage** konstruktoru, přidejte následující kód hello:
   
         _dispatcher = Window.Current.Dispatcher;
         PointerEventHandler pointerpressedhandler = new PointerEventHandler(sliderProgress_PointerPressed);
         sliderProgress.AddHandler(Control.PointerPressedEvent, pointerpressedhandler, true);    
5. Na konci hello hello **MainPage** třídy, přidejte následující kód hello:

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
>CoreDispatcher je použité toomake změny přístup z více vláken toohello uživatelského rozhraní od jiných vlákna uživatelského rozhraní. V případě potíže na vlákno dispečera vývojář může zvolit, že toouse dispečera poskytované elementu uživatelského rozhraní, že si má v úmyslu tooupdate.  Například:
   
         await sliderProgress.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () => { TimeSpan 

         timespan = new TimeSpan(adaptiveSourceStatusUpdate.EndTime); 
         double absvalue  = (int)Math.Round(timespan.TotalSeconds, MidpointRounding.AwayFromZero); 

         sliderProgress.Maximum = absvalue; }); 
6. Na konci hello hello **mediaElement_AdaptiveSourceStatusUpdated** metoda, přidejte následující kód hello:

         setSliderStartTime(args.StartTime);
         setSliderEndTime(args.EndTime);
7. Na konci hello hello **MediaOpened** metoda, přidejte následující kód hello:

         sliderProgress.StepFrequency = SliderFrequency(mediaElement.NaturalDuration.TimeSpan);
         sliderProgress.Width = mediaElement.Width;
         setupTimer();
8. Stiskněte klávesu **CTRL + S** toosave hello souboru.

**toocompile a testování aplikace hello**

1. Stiskněte klávesu **F6** toocompile hello projektu. 
2. Stiskněte klávesu **F5** toorun hello aplikace.
3. V horní části hello hello aplikace můžete použít výchozí hello adresy URL technologie Smooth Streaming nebo zadejte jiný. 
4. Klikněte na tlačítko **nastavit zdroj**. 
5. Test hello posuvníku.

Když jste dokončili Lekce 2.  V této lekci jste přidali tooapplication posuvníku. 

## <a name="lesson-3-select-smooth-streaming-streams"></a>Lekce 3: Vyberte vysílání funkce Smooth Streaming datové proudy
Technologie Smooth Streaming je schopný toostream obsah s více zvukových stop jazyk, které se vyvolá pomocí prohlížeče hello.  V této lekci povolíte datové proudy tooselect prohlížeče. V této lekci obsahuje hello následující postupy:

1. Upravte soubor XAML hello
2. Upravte soubor behand hello kódu
3. Kompilace a testování aplikace hello

**souboru XAML toomodify hello**

1. V Průzkumníku řešení klikněte pravým tlačítkem **MainPage.xaml**a potom klikněte na **Návrhář zobrazení**.
2. Vyhledejte &lt;Grid.RowDefinitions&gt;a upravte hello RowDefinitions tak bude vypadat takto:
   
         <Grid.RowDefinitions>            
            <RowDefinition Height="20"/>
            <RowDefinition Height="50"/>
            <RowDefinition Height="100"/>
            <RowDefinition Height="80"/>
            <RowDefinition Height="50"/>
         </Grid.RowDefinitions>
3. Uvnitř hello &lt;mřížky&gt;&lt;/Grid&gt; značky, přidejte následující hello kód toodefine ListBox – ovládací prvek, aby uživatelé mohli zobrazit hello seznam dostupných datových proudů a vybrat datové proudy:

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
4. Stiskněte klávesu **CTRL + S** toosave hello změny.

**toomodify hello souboru kódu**

1. V Průzkumníku řešení klikněte pravým tlačítkem **MainPage.xaml**a potom klikněte na **kód zobrazení**.
2. Uvnitř hello SSPlayer oboru názvů přidejte novou třídu:
   
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
                    // mMke hello video stream always checked.
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
3. Na hello začátku hello MainPage třídy, přidejte následující proměnné definice hello:
   
         private List<Stream> availableStreams;
         private List<Stream> availableAudioStreams;
         private List<Stream> availableTextStreams;
         private List<Stream> availableVideoStreams;
4. Uvnitř hello MainPage třídy přidejte hello následující oblasti:
   
        #region stream selection
        ///<summary>
        ///Functionality tooselect streams from IManifestStream available streams
        /// </summary>
   
        // This function is called from hello mediaElement_ManifestReady event handler 
        // tooretrieve hello streams and populate them toohello local data members.
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
   
                    //populate hello stream lists based on hello types
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
   
                    // Select hello default selected streams from hello manifest.
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
   
        // This function set hello list box ItemSource
        private async void refreshAvailableStreamsListBoxItemSource()
        {
            try
            {
                //update hello stream check box list on hello UI
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
   
            // Select hello frist video stream from hello list if no video stream is selected
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
                    txtStatus.Text = "hello audio stream is changed too" + availableAudioStreams[j].ManifestStream.Name;
                }
            }
   
            // Select hello frist audio stream from hello list if no audio steam is selected.
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
5. Hello mediaElement_ManifestReady metoda najít, doplňovací hello následující kód na konci hello hello funkce:
   
        getStreams(manifestObject);
        refreshAvailableStreamsListBoxItemSource();
   
    Proto když MediaElement manifest je připraven, hello kód získá seznam dostupných datových proudů hello a naplní pole se seznamem hello uživatelského rozhraní pomocí seznamu hello.
6. Uvnitř hello MainPage třídy, vyhledejte hello tlačítka uživatelského rozhraní klikněte na tlačítko oblasti události a poté přidejte následující definice funkce hello:
   
        private void btnChangeStream_Click(object sender, RoutedEventArgs e)
        {
            List<IManifestStream> selectedStreams = new List<IManifestStream>();
   
            // Create a list of hello selected streams
            createSelectedStreamsList(selectedStreams);
   
            // Change streams on hello presentation
            changeStreams(selectedStreams);
        }

**toocompile a testování aplikace hello**

1. Stiskněte klávesu **F6** toocompile hello projektu. 
2. Stiskněte klávesu **F5** toorun hello aplikace.
3. V horní části hello hello aplikace můžete použít výchozí hello adresy URL technologie Smooth Streaming nebo zadejte jiný. 
4. Klikněte na tlačítko **nastavit zdroj**. 
5. výchozí jazyk Hello je audio_eng. Zkuste tooswitch mezi audio_eng a audio_es. Při, vyberte nového datového proudu, musíte kliknout na tlačítko Odeslat hello.

Když jste dokončili Lekce 3.  V této lekci přidáte datové proudy toochoose funkce hello.

## <a name="lesson-4-select-smooth-streaming-tracks"></a>Lekce 4: Vyberte vysílání funkce Smooth Streaming sleduje
Prezentace technologie Smooth Streaming může obsahovat více videosouborů zakódovaných pomocí různé úrovně kvality (přenosové rychlosti) a jejich řešení. V této lekci povolíte uživatelům tooselect sleduje. V této lekci obsahuje hello následující postupy:

1. Upravte soubor XAML hello
2. Upravte soubor behand hello kódu
3. Kompilace a testování aplikace hello

**souboru XAML toomodify hello**

1. V Průzkumníku řešení klikněte pravým tlačítkem **MainPage.xaml**a potom klikněte na **Návrhář zobrazení**.
2. Vyhledejte hello &lt;mřížky&gt; značky s názvem hello **gridStreamAndBitrateSelection**, připojit hello následující kód na konci hello hello značky:
   
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
3. Stiskněte klávesu **CTRL + S** toosave změní

**toomodify hello souboru kódu**

1. V Průzkumníku řešení klikněte pravým tlačítkem **MainPage.xaml**a potom klikněte na **kód zobrazení**.
2. Uvnitř hello SSPlayer oboru názvů přidejte novou třídu:
   
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
3. Na hello začátku hello MainPage třídy, přidejte následující proměnné definice hello:
   
        private List<Track> availableTracks;
4. Uvnitř hello MainPage třídy přidejte hello následující oblasti:
   
        #region track selection
        /// <summary>
        /// Functionality tooselect video streams
        /// </summary>
   
        /// This Function gets hello tracks for hello selected video stream
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
   
        // This function gets hello video stream that is playing
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
   
        // This function set hello UI list box control ItemSource
        private async void refreshAvailableTracksListBoxItemSource()
        {
            try
            {
                // Update hello track check box list on hello UI 
                await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, ()
                    => { lbAvailableVideoTracks.ItemsSource = availableTracks; });
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }        
        }
   
        // This function creates a list of hello selected tracks.
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
   
        // This function selects hello tracks based on user selection 
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
5. Hello mediaElement_ManifestReady metoda najít, doplňovací hello následující kód na konci hello hello funkce:
   
         getTracks(manifestObject);
         refreshAvailableTracksListBoxItemSource();
6. Uvnitř hello MainPage třídy, vyhledejte hello tlačítka uživatelského rozhraní klikněte na tlačítko oblasti události a poté přidejte následující definice funkce hello:
   
         private void btnChangeStream_Click(object sender, RoutedEventArgs e)
         {
            List<IManifestStream> selectedStreams = new List<IManifestStream>();

            // Create a list of hello selected streams
            createSelectedStreamsList(selectedStreams);

            // Change streams on hello presentation
            changeStreams(selectedStreams);
         }

**toocompile a testování aplikace hello**

1. Stiskněte klávesu **F6** toocompile hello projektu. 
2. Stiskněte klávesu **F5** toorun hello aplikace.
3. V horní části hello hello aplikace můžete použít výchozí hello adresy URL technologie Smooth Streaming nebo zadejte jiný. 
4. Klikněte na tlačítko **nastavit zdroj**. 
5. Ve výchozím nastavení jsou vybrány všechny hello sleduje hello video datového proudu. tooexperiment hello bit míra změn, můžete vybrat hello nejnižší přenosová rychlost k dispozici a potom vyberte hello nejvyšší přenosová rychlost k dispozici. Musíte kliknout na odeslání po každé změně.  Uvidíte hello kvalitu videa změny.

Když jste dokončili Lekce 4.  V této lekci přidáte hello funkce toochoose sleduje.

## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="other-resources"></a>Další prostředky:
* [Jak toobuild aplikace technologie Smooth Streaming JavaScript Windows 8 s pokročilé funkce](http://blogs.iis.net/cenkd/archive/2012/08/10/how-to-build-a-smooth-streaming-windows-8-javascript-application-with-advanced-features.aspx)
* [Technologie Smooth Streaming technický přehled](http://www.iis.net/learn/media/on-demand-smooth-streaming/smooth-streaming-technical-overview)

[PlayerApplication]: ./media/media-services-build-smooth-streaming-apps/SSClientWin8-1.png
[CodeViewPic]: ./media/media-services-build-smooth-streaming-apps/SSClientWin8-2.png

