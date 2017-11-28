#### <a name="configure-the-ios-project-in-xamarin-studio"></a><span data-ttu-id="add98-101">Konfigurace projektu pro iOS v Xamarin studiu</span><span class="sxs-lookup"><span data-stu-id="add98-101">Configure the iOS project in Xamarin Studio</span></span>
1. <span data-ttu-id="add98-102">V Xamarin.Studio, otevřete **Info.plist**a aktualizovat **identifikátor svazku** s ID sady, který jste dříve vytvořili pomocí ID aplikace.</span><span class="sxs-lookup"><span data-stu-id="add98-102">In Xamarin.Studio, open **Info.plist**, and update the **Bundle Identifier** with the bundle ID that you created earlier with your new app ID.</span></span>

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-21.png)
2. <span data-ttu-id="add98-103">Přejděte dolů k položce **režimy pozadí**.</span><span class="sxs-lookup"><span data-stu-id="add98-103">Scroll down to **Background Modes**.</span></span> <span data-ttu-id="add98-104">Vyberte **povolit režimy pozadí** pole a **vzdáleného oznámení** pole.</span><span class="sxs-lookup"><span data-stu-id="add98-104">Select the **Enable Background Modes** box and the **Remote notifications** box.</span></span>

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-22.png)
3. <span data-ttu-id="add98-105">Dvakrát klikněte na projekt v panelu řešení otevřete **možnosti projektu**.</span><span class="sxs-lookup"><span data-stu-id="add98-105">Double-click your project in the Solution Panel to open **Project Options**.</span></span>
4. <span data-ttu-id="add98-106">V části **sestavení**, zvolte **iOS podepisování sady**a vyberte odpovídající identitě a zřizování profilu jste právě nastavili nahoru pro tento projekt.</span><span class="sxs-lookup"><span data-stu-id="add98-106">Under **Build**, choose **iOS Bundle Signing**, and select the corresponding identity and provisioning profile you just set up for this project.</span></span>

   ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-20.png)

   <span data-ttu-id="add98-107">Tím se zajistí, že projektu používá nový profil pro podepisování kódu.</span><span class="sxs-lookup"><span data-stu-id="add98-107">This ensures that the project uses the new profile for code signing.</span></span> <span data-ttu-id="add98-108">Oficiální Xamarin zařízení zřizování dokumentaci, najdete v části [zřizování zařízení Xamarin].</span><span class="sxs-lookup"><span data-stu-id="add98-108">For the official Xamarin device provisioning documentation, see [Xamarin Device Provisioning].</span></span>

#### <a name="configure-the-ios-project-in-visual-studio"></a><span data-ttu-id="add98-109">Konfigurace projektu pro iOS v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="add98-109">Configure the iOS project in Visual Studio</span></span>
1. <span data-ttu-id="add98-110">V sadě Visual Studio, klikněte pravým tlačítkem na projekt a pak klikněte na tlačítko **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="add98-110">In Visual Studio, right-click the project, and then click **Properties**.</span></span>
2. <span data-ttu-id="add98-111">Na stránkách vlastností klikněte na tlačítko **iOS aplikace** kartě a aktualizovat **identifikátor** s ID, který jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="add98-111">In the properties pages, click the **iOS Application** tab, and update the **Identifier** with the ID that you created earlier.</span></span>

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-23.png)
3. <span data-ttu-id="add98-112">V **iOS podepisování sady** , vyberte odpovídající identitě a zřizování profilu stačí nastavit pro tento projekt.</span><span class="sxs-lookup"><span data-stu-id="add98-112">In the **iOS Bundle Signing** tab, select the corresponding identity and provisioning profile you just set up for this project.</span></span>

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-24.png)

    <span data-ttu-id="add98-113">Tím se zajistí, že projektu používá nový profil pro podepisování kódu.</span><span class="sxs-lookup"><span data-stu-id="add98-113">This ensures that the project uses the new profile for code signing.</span></span> <span data-ttu-id="add98-114">Oficiální Xamarin zařízení zřizování dokumentaci, najdete v části [zřizování zařízení Xamarin].</span><span class="sxs-lookup"><span data-stu-id="add98-114">For the official Xamarin device provisioning documentation, see [Xamarin Device Provisioning].</span></span>
4. <span data-ttu-id="add98-115">Klikněte dvakrát na Info.plist otevřete ho a pak povolte **RemoteNotifications** pod **režimy pozadí**.</span><span class="sxs-lookup"><span data-stu-id="add98-115">Double-click Info.plist to open it, and then enable **RemoteNotifications** under **Background Modes**.</span></span>

<span data-ttu-id="add98-116">[zřizování zařízení Xamarin]: http://developer.xamarin.com/guides/ios/getting_started/installation/device_provisioning/</span><span class="sxs-lookup"><span data-stu-id="add98-116">[Xamarin Device Provisioning]: http://developer.xamarin.com/guides/ios/getting_started/installation/device_provisioning/</span></span>
