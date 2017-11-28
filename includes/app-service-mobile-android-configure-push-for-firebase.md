
1. <span data-ttu-id="8344d-101">V [portál Azure](https://portal.azure.com/), klikněte na tlačítko **Procházet vše** > **App Services**a potom klikněte na vaše mobilní aplikace back-end.</span><span class="sxs-lookup"><span data-stu-id="8344d-101">In the [Azure portal](https://portal.azure.com/), click **Browse All** > **App Services**, and then click your Mobile Apps back end.</span></span> <span data-ttu-id="8344d-102">V části **nastavení**, klikněte na tlačítko **aplikace služby Push**a potom klikněte na název vašeho centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="8344d-102">Under **Settings**, click **App Service Push**, and then click your notification hub name.</span></span>
2. <span data-ttu-id="8344d-103">Přejděte na **Google (GCM)**, zadejte **klíč serveru** hodnotu, která jste získali z Firebase v předchozím postupu a potom klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="8344d-103">Go to **Google (GCM)**, enter the **Server Key** value that you obtained from Firebase in the previous procedure, and then click **Save**.</span></span>

    ![Nastavit klíč rozhraní GCM API na portálu](./media/app-service-mobile-android-configure-push/mobile-push-api-key.png)

<span data-ttu-id="8344d-105">Back-end mobilní aplikace je nyní nakonfigurován pro použití Firebase Cloud Messaging.</span><span class="sxs-lookup"><span data-stu-id="8344d-105">The Mobile Apps back end is now configured to use Firebase Cloud Messaging.</span></span> <span data-ttu-id="8344d-106">To umožňuje odesílat nabízená oznámení do vaší aplikaci spuštěnou na zařízení s Androidem pomocí centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="8344d-106">This enables you to send push notifications to your app running on an Android device, by using the notification hub.</span></span>

<!-- URLs. -->


<!-- images -->
