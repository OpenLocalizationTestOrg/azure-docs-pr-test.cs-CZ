
1. <span data-ttu-id="3fcb1-101">V Průzkumníku řešení Visual Studio, klikněte pravým tlačítkem na projekt aplikace Windows Store hello a klikněte na tlačítko **úložiště** > **propojit aplikaci se hello Store**.</span><span class="sxs-lookup"><span data-stu-id="3fcb1-101">In Visual Studio Solution Explorer, right-click hello Windows Store app project, and click **Store** > **Associate App with hello Store**.</span></span>

    ![Přidružit aplikace Windows Store](./media/app-service-mobile-register-wns/notification-hub-associate-win8-app.png)
2. <span data-ttu-id="3fcb1-103">V Průvodci hello, klikněte na tlačítko **Další**a přihlaste se pomocí účtu Microsoft.</span><span class="sxs-lookup"><span data-stu-id="3fcb1-103">In hello wizard, click **Next**, and sign in with your Microsoft account.</span></span> <span data-ttu-id="3fcb1-104">Zadejte název aplikace v rámci **rezervovat nový název aplikace**a potom klikněte na **rezervy**.</span><span class="sxs-lookup"><span data-stu-id="3fcb1-104">Type a name for your app in **Reserve a new app name**, and then click **Reserve**.</span></span>
3. <span data-ttu-id="3fcb1-105">Po registraci aplikace hello je úspěšně vytvořil, vyberte hello nový název aplikace, klikněte na tlačítko **Další**a potom klikněte na **přidružit**.</span><span class="sxs-lookup"><span data-stu-id="3fcb1-105">After hello app registration is successfully created, select hello new app name, click **Next**, and then click **Associate**.</span></span> <span data-ttu-id="3fcb1-106">Tento postup přidá požadované hello Windows Store registrační informace toohello manifest aplikace.</span><span class="sxs-lookup"><span data-stu-id="3fcb1-106">This adds hello required Windows Store registration information toohello application manifest.</span></span>
4. <span data-ttu-id="3fcb1-107">Opakujte kroky 1 a 3 pro projekt aplikace Windows Phone Store hello pomocí hello stejné registrace, který jste dříve vytvořili pro aplikace pro Windows Store hello.</span><span class="sxs-lookup"><span data-stu-id="3fcb1-107">Repeat steps 1 and 3 for hello Windows Phone Store app project by using hello same registration you previously created for hello Windows Store app.</span></span>  
5. <span data-ttu-id="3fcb1-108">Procházet toohello [Centrum vývojářů pro Windows](https://dev.windows.com/en-us/overview)a přihlaste se pomocí účtu Microsoft.</span><span class="sxs-lookup"><span data-stu-id="3fcb1-108">Browse toohello [Windows Dev Center](https://dev.windows.com/en-us/overview), and sign in with your Microsoft account.</span></span> <span data-ttu-id="3fcb1-109">Klikněte na nové registrace aplikace hello v **Moje aplikace**a potom rozbalte **služby** > **nabízená oznámení**.</span><span class="sxs-lookup"><span data-stu-id="3fcb1-109">Click hello new app registration in **My apps**, and then expand **Services** > **Push notifications**.</span></span>
6. <span data-ttu-id="3fcb1-110">Na hello **nabízená oznámení** klikněte na tlačítko **Web služeb Live Services** pod **nabízených oznámení Windows služby (WNS) a Microsoft Azure Mobile Apps**.</span><span class="sxs-lookup"><span data-stu-id="3fcb1-110">On hello **Push notifications** page, click **Live Services site** under **Windows Push Notification Services (WNS) and Microsoft Azure Mobile Apps**.</span></span> <span data-ttu-id="3fcb1-111">Poznamenejte si hodnoty hello hello **SID balíčku** a hello *aktuální* hodnotu **tajný klíč aplikace**.</span><span class="sxs-lookup"><span data-stu-id="3fcb1-111">Make a note of hello values of hello **Package SID** and hello *current*  value in **Application Secret**.</span></span> 

    ![Nastavení aplikace ve středisku pro vývojáře hello](./media/app-service-mobile-register-wns/mobile-services-win8-app-push-auth.png)

   > [!IMPORTANT]
   > <span data-ttu-id="3fcb1-113">tajný klíč aplikace Hello a SID balíčku jsou důležitá pověření zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="3fcb1-113">hello application secret and package SID are important security credentials.</span></span> <span data-ttu-id="3fcb1-114">Tyto hodnoty s nikým nesdílejte ani je nedistribuujte s vaší aplikací.</span><span class="sxs-lookup"><span data-stu-id="3fcb1-114">Do not share these values with anyone or distribute them with your app.</span></span>
   >
   >