#### <a name="configure-hello-ios-project-in-xamarin-studio"></a>Konfigurace projektu iOS hello v Xamarin studiu
1. V Xamarin.Studio, otevřete **Info.plist**a aktualizace hello **identifikátor svazku** s hello sady ID, které jste dříve vytvořili pomocí ID aplikace.

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-21.png)
2. Posuňte se dolů příliš**režimy pozadí**. Vyberte hello **povolit režimy pozadí** pole a hello **vzdáleného oznámení** pole.

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-22.png)
3. Dvakrát klikněte na projekt v hello panelu řešení tooopen **možnosti projektu**.
4. V části **sestavení**, zvolte **iOS podepisování sady**a vyberte hello odpovídající identity a profil pro zřizování právě nastavíte pro tento projekt.

   ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-20.png)

   Tím se zajistí, že takový projekt hello používá hello nový profil pro podepisování kódu. Hello oficiální Xamarin zařízení zřizování dokumentaci v tématu [zřizování zařízení Xamarin].

#### <a name="configure-hello-ios-project-in-visual-studio"></a>Konfigurace projektu iOS hello v sadě Visual Studio
1. V sadě Visual Studio, klikněte pravým tlačítkem na projekt hello a pak klikněte na tlačítko **vlastnosti**.
2. Na stránkách vlastností hello, klikněte na tlačítko hello **iOS aplikace** kartě a aktualizace hello **identifikátor** s hello ID, který jste vytvořili dříve.

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-23.png)
3. V hello **iOS podepisování sady** kartě vyberte hello odpovídající identity a zřizování profilu jste právě nastavili nahoru pro tento projekt.

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-24.png)

    Tím se zajistí, že takový projekt hello používá hello nový profil pro podepisování kódu. Hello oficiální Xamarin zařízení zřizování dokumentaci v tématu [zřizování zařízení Xamarin].
4. Klikněte dvakrát na Info.plist tooopen a potom povolit **RemoteNotifications** pod **režimy pozadí**.

[zřizování zařízení Xamarin]: http://developer.xamarin.com/guides/ios/getting_started/installation/device_provisioning/
