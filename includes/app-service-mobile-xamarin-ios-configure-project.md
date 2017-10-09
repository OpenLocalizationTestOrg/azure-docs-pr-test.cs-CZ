#### <a name="configure-hello-ios-project-in-xamarin-studio"></a><span data-ttu-id="0a9db-101">Konfigurace projektu iOS hello v Xamarin studiu</span><span class="sxs-lookup"><span data-stu-id="0a9db-101">Configure hello iOS project in Xamarin Studio</span></span>
1. <span data-ttu-id="0a9db-102">V Xamarin.Studio, otevřete **Info.plist**a aktualizace hello **identifikátor svazku** s hello sady ID, které jste dříve vytvořili pomocí ID aplikace.</span><span class="sxs-lookup"><span data-stu-id="0a9db-102">In Xamarin.Studio, open **Info.plist**, and update hello **Bundle Identifier** with hello bundle ID that you created earlier with your new app ID.</span></span>

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-21.png)
2. <span data-ttu-id="0a9db-103">Posuňte se dolů příliš**režimy pozadí**.</span><span class="sxs-lookup"><span data-stu-id="0a9db-103">Scroll down too**Background Modes**.</span></span> <span data-ttu-id="0a9db-104">Vyberte hello **povolit režimy pozadí** pole a hello **vzdáleného oznámení** pole.</span><span class="sxs-lookup"><span data-stu-id="0a9db-104">Select hello **Enable Background Modes** box and hello **Remote notifications** box.</span></span>

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-22.png)
3. <span data-ttu-id="0a9db-105">Dvakrát klikněte na projekt v hello panelu řešení tooopen **možnosti projektu**.</span><span class="sxs-lookup"><span data-stu-id="0a9db-105">Double-click your project in hello Solution Panel tooopen **Project Options**.</span></span>
4. <span data-ttu-id="0a9db-106">V části **sestavení**, zvolte **iOS podepisování sady**a vyberte hello odpovídající identity a profil pro zřizování právě nastavíte pro tento projekt.</span><span class="sxs-lookup"><span data-stu-id="0a9db-106">Under **Build**, choose **iOS Bundle Signing**, and select hello corresponding identity and provisioning profile you just set up for this project.</span></span>

   ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-20.png)

   <span data-ttu-id="0a9db-107">Tím se zajistí, že takový projekt hello používá hello nový profil pro podepisování kódu.</span><span class="sxs-lookup"><span data-stu-id="0a9db-107">This ensures that hello project uses hello new profile for code signing.</span></span> <span data-ttu-id="0a9db-108">Hello oficiální Xamarin zařízení zřizování dokumentaci v tématu [zřizování zařízení Xamarin].</span><span class="sxs-lookup"><span data-stu-id="0a9db-108">For hello official Xamarin device provisioning documentation, see [Xamarin Device Provisioning].</span></span>

#### <a name="configure-hello-ios-project-in-visual-studio"></a><span data-ttu-id="0a9db-109">Konfigurace projektu iOS hello v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0a9db-109">Configure hello iOS project in Visual Studio</span></span>
1. <span data-ttu-id="0a9db-110">V sadě Visual Studio, klikněte pravým tlačítkem na projekt hello a pak klikněte na tlačítko **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="0a9db-110">In Visual Studio, right-click hello project, and then click **Properties**.</span></span>
2. <span data-ttu-id="0a9db-111">Na stránkách vlastností hello, klikněte na tlačítko hello **iOS aplikace** kartě a aktualizace hello **identifikátor** s hello ID, který jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="0a9db-111">In hello properties pages, click hello **iOS Application** tab, and update hello **Identifier** with hello ID that you created earlier.</span></span>

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-23.png)
3. <span data-ttu-id="0a9db-112">V hello **iOS podepisování sady** kartě vyberte hello odpovídající identity a zřizování profilu jste právě nastavili nahoru pro tento projekt.</span><span class="sxs-lookup"><span data-stu-id="0a9db-112">In hello **iOS Bundle Signing** tab, select hello corresponding identity and provisioning profile you just set up for this project.</span></span>

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-24.png)

    <span data-ttu-id="0a9db-113">Tím se zajistí, že takový projekt hello používá hello nový profil pro podepisování kódu.</span><span class="sxs-lookup"><span data-stu-id="0a9db-113">This ensures that hello project uses hello new profile for code signing.</span></span> <span data-ttu-id="0a9db-114">Hello oficiální Xamarin zařízení zřizování dokumentaci v tématu [zřizování zařízení Xamarin].</span><span class="sxs-lookup"><span data-stu-id="0a9db-114">For hello official Xamarin device provisioning documentation, see [Xamarin Device Provisioning].</span></span>
4. <span data-ttu-id="0a9db-115">Klikněte dvakrát na Info.plist tooopen a potom povolit **RemoteNotifications** pod **režimy pozadí**.</span><span class="sxs-lookup"><span data-stu-id="0a9db-115">Double-click Info.plist tooopen it, and then enable **RemoteNotifications** under **Background Modes**.</span></span>

[zřizování zařízení Xamarin]: http://developer.xamarin.com/guides/ios/getting_started/installation/device_provisioning/
