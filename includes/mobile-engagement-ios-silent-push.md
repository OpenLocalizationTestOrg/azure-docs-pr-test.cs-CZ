> [!IMPORTANT]
> <span data-ttu-id="1aa15-101">tooreceive nabízená oznámení z Mobile Engagement, je nutné tooenable `Silent Remote Notifications` ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="1aa15-101">tooreceive Push Notifications from Mobile Engagement, you need tooenable `Silent Remote Notifications` in your application.</span></span> <span data-ttu-id="1aa15-102">Je nutné tooadd hello hodnotu vzdáleného oznámení toohello UIBackgroundModes pole v souboru Info.plist.</span><span class="sxs-lookup"><span data-stu-id="1aa15-102">You need tooadd hello remote-notification value toohello UIBackgroundModes array in your Info.plist file.</span></span>
> 
> 

1. <span data-ttu-id="1aa15-103">Otevřete `info.plist` soubor v projektu hello</span><span class="sxs-lookup"><span data-stu-id="1aa15-103">Open `info.plist` file in hello project</span></span>
2. <span data-ttu-id="1aa15-104">Klikněte pravým tlačítkem na hello horní položku v seznamu hello (`Information Property List`) a přidejte nový řádek</span><span class="sxs-lookup"><span data-stu-id="1aa15-104">Right click on hello top item in hello list (`Information Property List`) and add a new row</span></span>
   
    ![](./media/mobile-engagement-ios-silent-push/xcode-plist-add-silent-push1.png)
3. <span data-ttu-id="1aa15-105">Hello zadejte nový řádek`Required background modes`</span><span class="sxs-lookup"><span data-stu-id="1aa15-105">In hello new row enter `Required background modes`</span></span>
   
    ![](./media/mobile-engagement-ios-silent-push/xcode-plist-add-silent-push2.png)
4. <span data-ttu-id="1aa15-106">Klikněte na řádek hello tooexpand šipku vlevo hello</span><span class="sxs-lookup"><span data-stu-id="1aa15-106">Click on hello left arrow tooexpand hello row</span></span>
5. <span data-ttu-id="1aa15-107">Přidejte následující položky toohello hodnotu 0 hello`App downloads content in response toopush notifications`</span><span class="sxs-lookup"><span data-stu-id="1aa15-107">Add hello following value toohello item 0 `App downloads content in response toopush notifications`</span></span>
   
    ![](./media/mobile-engagement-ios-silent-push/xcode-plist-add-silent-push3.png)
6. <span data-ttu-id="1aa15-108">Po provedení hello změnit hello info.plist XML by měl obsahovat hello následující klíč a hodnotu:</span><span class="sxs-lookup"><span data-stu-id="1aa15-108">Once you make hello change, hello info.plist XML should contain hello following key and value:</span></span>
   
        <key>UIBackgroundModes</key>
        <array>
        <string>remote-notification</string>
        </array>
7. <span data-ttu-id="1aa15-109">Pokud používáte **Xcode 7+** a **iOS 9+**:</span><span class="sxs-lookup"><span data-stu-id="1aa15-109">If you are using **Xcode 7+** and **iOS 9+**:</span></span>
   
   * <span data-ttu-id="1aa15-110">Povolte možnost **Push Notifications** (Nabízená oznámení) v části Targets (Cíle) > Your Target Name (Název vašeho cíle) > Capabilities (Schopnosti).</span><span class="sxs-lookup"><span data-stu-id="1aa15-110">Enable **Push Notifications** in Targets > Your Target Name > Capabilities.</span></span>
