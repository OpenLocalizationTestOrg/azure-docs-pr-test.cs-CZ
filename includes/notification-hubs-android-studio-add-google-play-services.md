1. <span data-ttu-id="d6e23-101">Otevřete hello Android SDK Manager kliknutím na ikonu hello na panelu nástrojů hello Android studiu nebo kliknutím **nástroje** -> **Android** -> **SDK Manager**v nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="d6e23-101">Open hello Android SDK Manager by clicking hello icon on hello toolbar of Android Studio or by clicking **Tools** -> **Android** -> **SDK Manager** on hello menu.</span></span> <span data-ttu-id="d6e23-102">Vyhledejte cílovou verzi hello hello Android SDK, který se používá ve vašem projektu, otevřete ho kliknutím na **zobrazit podrobnosti balíčku**a zvolte **rozhraní Google API**, pokud již není nainstalována.</span><span class="sxs-lookup"><span data-stu-id="d6e23-102">Locate hello target version of hello Android SDK that is used in your project , open it by clicking **Show Package Details**, and choose **Google APIs**, if it is not already installed.</span></span>
2. <span data-ttu-id="d6e23-103">Klikněte na tlačítko hello **SDK Tools** kartě. Pokud jste ještě nenainstalovali služby Google Play, klikněte na položku **Google Play Services**, jak vidíte níže.</span><span class="sxs-lookup"><span data-stu-id="d6e23-103">Click hello **SDK Tools** tab. If you haven't already installed Google Play Service, click **Google Play Services** as shown below.</span></span> <span data-ttu-id="d6e23-104">Pak klikněte na tlačítko **použít** tooinstall.</span><span class="sxs-lookup"><span data-stu-id="d6e23-104">Then click **Apply** tooinstall.</span></span> 
   
    <span data-ttu-id="d6e23-105">Všimněte si hello SDK cestu, pro použití v pozdější fázi.</span><span class="sxs-lookup"><span data-stu-id="d6e23-105">Note hello SDK path, for use in a later step.</span></span> 
   
    ![](./media/notification-hubs-android-studio-add-google-play-services/notification-hubs-android-studio-sdk-manager.png)
3. <span data-ttu-id="d6e23-106">Otevřete hello **build.gradle** soubor v adresáři aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="d6e23-106">Open hello **build.gradle** file in hello app directory.</span></span>
   
    ![](./media/notification-hubs-android-studio-add-google-play-services/notification-hubs-android-studio-add-google-play-dependency.png)
4. <span data-ttu-id="d6e23-107">Do části *dependencies* přidejte tento řádek:</span><span class="sxs-lookup"><span data-stu-id="d6e23-107">Add this line under *dependencies*:</span></span> 
   
           compile 'com.google.android.gms:play-services-gcm:9.2.0'
5. <span data-ttu-id="d6e23-108">Klikněte na tlačítko hello **synchronizovat projekt se soubory Gradle** ikonu na panelu nástrojů hello.</span><span class="sxs-lookup"><span data-stu-id="d6e23-108">Click hello **Sync Project with Gradle Files** icon in hello tool bar.</span></span>
6. <span data-ttu-id="d6e23-109">Otevřete **AndroidManifest.xml** a přidejte tuto značku toohello *aplikace* značky.</span><span class="sxs-lookup"><span data-stu-id="d6e23-109">Open **AndroidManifest.xml** and add this tag toohello *application* tag.</span></span>
   
        <meta-data android:name="com.google.android.gms.version"
            android:value="@integer/google_play_services_version" />

