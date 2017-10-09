> [!IMPORTANT]
> tooreceive nabízená oznámení z Mobile Engagement, je nutné tooenable `Silent Remote Notifications` ve vaší aplikaci. Je nutné tooadd hello hodnotu vzdáleného oznámení toohello UIBackgroundModes pole v souboru Info.plist.
> 
> 

1. Otevřete `info.plist` soubor v projektu hello
2. Klikněte pravým tlačítkem na hello horní položku v seznamu hello (`Information Property List`) a přidejte nový řádek
   
    ![](./media/mobile-engagement-ios-silent-push/xcode-plist-add-silent-push1.png)
3. Hello zadejte nový řádek`Required background modes`
   
    ![](./media/mobile-engagement-ios-silent-push/xcode-plist-add-silent-push2.png)
4. Klikněte na řádek hello tooexpand šipku vlevo hello
5. Přidejte následující položky toohello hodnotu 0 hello`App downloads content in response toopush notifications`
   
    ![](./media/mobile-engagement-ios-silent-push/xcode-plist-add-silent-push3.png)
6. Po provedení hello změnit hello info.plist XML by měl obsahovat hello následující klíč a hodnotu:
   
        <key>UIBackgroundModes</key>
        <array>
        <string>remote-notification</string>
        </array>
7. Pokud používáte **Xcode 7+** a **iOS 9+**:
   
   * Povolte možnost **Push Notifications** (Nabízená oznámení) v části Targets (Cíle) > Your Target Name (Název vašeho cíle) > Capabilities (Schopnosti).

