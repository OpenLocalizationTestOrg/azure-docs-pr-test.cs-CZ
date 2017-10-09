### <a name="grant-mobile-engagement-access-tooyour-gcm-api-key"></a>Mobile Engagement udělení přístupu tooyour klíč rozhraní API GCM
tooallow Mobile Engagement toosend nabízená oznámení vaším jménem, je nutné toogrant, že přístup tooyour klíč rozhraní API. K tomu je potřeba nakonfigurujete a zadáte svůj klíč do hello portál Mobile Engagement.

1. Z klasického portálu Azure, zkontrolujte, jestli pracujete v aplikaci hello používáte pro tento projekt a pak klikněte na tlačítko hello **Engage** tlačítko dole v hello:
   
    ![](./media/mobile-engagement-android-send-push/engage-button.png)
2. Pak klikněte na tlačítko hello **nastavení** -> **nativní oznámení** části tooenter klíč GCM:
   
    ![](./media/mobile-engagement-android-send-push/engagement-portal.png)
3. Klikněte na tlačítko hello **upravit** ikonu **klíč rozhraní API** v hello **nastavení GCM** části, jak je uvedeno níže:
   
    ![](./media/mobile-engagement-android-send-push/native-push-settings.png)
4. V místní nabídce hello vložte klíč serveru GCM jste předtím získali hello a pak klikněte na **Ok**.
   
    ![](./media/mobile-engagement-android-send-push/api-key.png)

## <a id="send"></a>Poslat tooyour aplikaci oznámení
Nyní vytvoříme kampaně jednoduché nabízených oznámení, který odesílá nabízená oznámení tooour aplikace pro práci s.

1. Přejděte toohello **dosáhnout** na portálu Mobile Engagement.
2. Klikněte na tlačítko **nové oznámení** toocreate kampaň nabízených oznámení.
   
    ![](./media/mobile-engagement-android-send-push/new-announcement.png)
3. Nastavte první pole kampaně pomocí následujících kroků hello hello:
   
    ![](./media/mobile-engagement-android-send-push/campaign-first-params.png)
   
    a. Zadejte název kampaně.
   
    b. Vyberte hello **typ doručení** jako *systémové oznámení -> jednoduché*: Toto je typ hello jednoduché Android nabízeného oznámení, který obsahuje název a řádek menšího textu.
   
    c. Vyberte **čas doručení** jako *kdykoli* tooallow hello aplikace tooreceive oznámení, zda je spuštěná aplikace hello, nebo ne.
   
    d. V hello oznámení text typ hello **název** který bude v tučné v nabízené hello.
   
    e. Do pole **Zpráva** zadejte zprávu.
4. Přejděte dolů a v hello **obsahu** vyberte **pouze oznámení**.
   
    ![](./media/mobile-engagement-android-send-push/campaign-content.png)
5. Dokončení nastavení hello nejzákladnější kampaně možné. Nyní znovu přejděte dolů a klikněte na tlačítko hello **vytvořit** tlačítko toosave kampaně.
6. Poslední krok: Kliknutím na **aktivovat** tooactivate toosend kampaň nabízených oznámení.
   
    ![](./media/mobile-engagement-android-send-push/campaign-activate.png)

