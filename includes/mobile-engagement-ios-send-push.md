### <a name="grant-access-tooyour-push-certificate-toomobile-engagement"></a>Udělení přístupu tooyour certifikátu Push tooMobile zapojení
tooallow Mobile Engagement toosend nabízená oznámení vaším jménem, je nutné toogrant, že přístup tooyour certifikátu. K tomu je potřeba nakonfigurujete a zadáte vašeho certifikátu na portál Mobile Engagement hello. Musíte nejdřív získat certifikát .p12 podle pokynů v [dokumentaci od společnosti Apple](https://developer.apple.com/library/prerelease/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html#//apple_ref/doc/uid/TP40012582-CH26-SW6).

1. Přejděte tooyour portál Mobile Engagement. Zkontrolujte, jestli v hello správné a potom klikněte na hello **Engage** tlačítko dole v hello:
   
    ![](./media/mobile-engagement-ios-send-push/engage-button.png)
2. Klikněte na hello **nastavení** stránky na portálu Engagement. Zde klikněte na hello **nativní oznámení** části tooupload svůj certifikát .p12:
   
    ![](./media/mobile-engagement-ios-send-push/engagement-portal.png)
3. Vyberte svůj soubor .p12, nahrajte ho a zadejte heslo:
   
    ![](./media/mobile-engagement-ios-send-push/native-push-settings.png)

## <a id="send"></a>Poslat tooyour aplikaci oznámení
Teď vytvoříme jednoduchou kampaň nabízených oznámení, který bude odesílat aplikace tooour na nabízené:

1. Přejděte toohello **dosáhnout** na portálu Mobile Engagement.
2. Klikněte na tlačítko **nové oznámení** toocreate kampaň nabízených
   
    ![](./media/mobile-engagement-ios-send-push/new-announcement.png)
3. Instalační program hello první pole své kampaně:
   
    ![](./media/mobile-engagement-ios-send-push/campaign-first-params.png)
   
   * Zadejte **Název** kampaně. 
   * Vyberte hello **čas doručení** jako **pouze mimo aplikaci**: Toto je hello jednoduchý typ nabízeného oznámení Apple, která obsahuje text.
   * V textu hello oznámení, zadejte první hello **název** která bude hello první řádek v nabízené hello.
   * Zadejte vaše **zpráva** která bude druhý řádek hello
4. Posuňte se dolů a v hello části obsahu vyberte **pouze oznámení**
   
    ![](./media/mobile-engagement-ios-send-push/campaign-content.png)
5. Dokončení nastavení hello nejzákladnější kampaně. Nyní přejděte dolů a klikněte na **vytvořit** tlačítko toosave kampaň nabízených oznámení. 
6. Nakonec kliknutím na **aktivovat** toosend nabízených oznámení. 
   
    ![](./media/mobile-engagement-ios-send-push/campaign-activate.png)
7. Bude možné přijímat oznámení hello na zařízení s iOS v centru oznámení hello jako hello následující:
   
    ![](./media/mobile-engagement-ios-send-push/iphone-notification.png)
8. Pokud máte Apple Watch spárovat s tímto zařízením iOS se zobrazí hello oznámení na vaše Apple Watch:
   
    ![](./media/mobile-engagement-ios-send-push/apple-watch.png)

