1. Otevřete hello Android SDK Manager kliknutím na ikonu hello na panelu nástrojů hello Android studiu nebo kliknutím **nástroje** -> **Android** -> **SDK Manager**v nabídce hello. Vyhledejte cílovou verzi hello hello Android SDK, který se používá ve vašem projektu, otevřete ho kliknutím na **zobrazit podrobnosti balíčku**a zvolte **rozhraní Google API**, pokud již není nainstalována.
2. Klikněte na tlačítko hello **SDK Tools** kartě. Pokud jste ještě nenainstalovali služby Google Play, klikněte na položku **Google Play Services**, jak vidíte níže. Pak klikněte na tlačítko **použít** tooinstall. 
   
    Všimněte si hello SDK cestu, pro použití v pozdější fázi. 
   
    ![](./media/notification-hubs-android-studio-add-google-play-services/notification-hubs-android-studio-sdk-manager.png)
3. Otevřete hello **build.gradle** soubor v adresáři aplikace hello.
   
    ![](./media/notification-hubs-android-studio-add-google-play-services/notification-hubs-android-studio-add-google-play-dependency.png)
4. Do části *dependencies* přidejte tento řádek: 
   
           compile 'com.google.android.gms:play-services-gcm:9.2.0'
5. Klikněte na tlačítko hello **synchronizovat projekt se soubory Gradle** ikonu na panelu nástrojů hello.
6. Otevřete **AndroidManifest.xml** a přidejte tuto značku toohello *aplikace* značky.
   
        <meta-data android:name="com.google.android.gms.version"
            android:value="@integer/google_play_services_version" />

