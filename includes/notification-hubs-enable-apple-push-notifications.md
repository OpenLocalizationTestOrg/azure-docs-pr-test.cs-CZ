

## <a name="generate-hello-certificate-signing-request-file"></a>Generování souboru hello žádost o podepsání certifikátu
Hello služby nabízených oznámení Apple (APNS) používá certifikáty tooauthenticate nabízených oznámení. Postupujte podle těchto pokynů toocreate hello nabízený certifikát toosend a přijímat oznámení. Další informace o těchto pojmech najdete v oficiální hello [Apple Push Notification Service](http://go.microsoft.com/fwlink/p/?LinkId=272584) dokumentaci.

Generování souboru hello žádost (Podepsání certifikátu), který je používán Apple toogenerate podepsaného nabízeného certifikátu.

1. Na počítači Mac spusťte nástroj Keychain Access hello. Můžete ho otevřít z hello **nástroje** složku nebo hello **jiných** složky na launchpadu hello.
2. Klikněte na **Keychain Access**, rozbalte **Průvodce certifikací**, klikněte na **Vyžádat certifikát od certifikační autority...**.
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-request-cert-from-ca.png)
3. Vyberte vaše **e-mailovou adresu uživatele** a **běžný název** , ujistěte se, že **uložit toodisk** je vybrána a potom klikněte na **pokračovat**. Nechte hello **certifikační Autority e-mailovou adresu** pole prázdné, není nutné.
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-csr-info.png)
4. Zadejte název souboru certifikátu žádosti o Podepsání hello v **uložit jako**, vyberte umístění hello v **kde**, pak klikněte na tlačítko **Uložit**.
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-save-csr.png)
   
      To umožňuje ušetřit soubor CSR hello v umístění hello vybrané; Hello výchozím umístěním je plocha hello. Mějte na paměti, hello umístění tohoto souboru.

Bude v dalším kroku svou aplikaci zaregistrovat u Applu, povolte nabízená oznámení a nahrajte Tento exportovaný CSR toocreate nabízeného certifikátu.

## <a name="register-your-app-for-push-notifications"></a>Registrace aplikace pro nabízená oznámení
toobe možné toosend nabízená oznámení tooan aplikace pro iOS, je nutné zaregistrovat aplikaci s Apple a také zaregistrovat pro nabízená oznámení.  

1. Pokud jste ještě nezaregistrovali vaší aplikace, přejděte toohello <a href="http://go.microsoft.com/fwlink/p/?LinkId=272456" target="_blank">iOS Provisioning Portal</a> hello Apple Developer Center, přihlaste se pomocí Apple ID, klikněte na **identifikátory**, pak klikněte na tlačítko **ID aplikace** a nakonec klikněte na hello  **+**  přihlásit tooregister novou aplikaci.
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-ios-appids.png)
      
2. Aktualizovat hello následující tři pole týkající se nové aplikace a pak klikněte na **pokračovat**:
   
   * **Název**: Zadejte popisný název pro vaši aplikaci v hello **název** pole hello **popis ID aplikace** části.
   * **Identifikátor sady**: v části hello **explicitní ID aplikace** zadejte **identifikátor svazku** v podobě hello `<Organization Identifier>.<Product Name>` jak je uvedeno v hello [distribuce aplikací Průvodce](https://developer.apple.com/library/mac/documentation/IDEs/Conceptual/AppDistributionGuide/ConfiguringYourApp/ConfiguringYourApp.html#//apple_ref/doc/uid/TP40012582-CH28-SW8). Hello *identifikátor organizace* a *název produktu* musí odpovídat identifikátoru organizace hello a název produktu, které budete používat při vytváření projektu prostředí XCode. V níže uvedeném snímku hello *NotificationHubs* se používá jako identifikátor organizace a *GetStarted* slouží jako název produktu hello. Ujistěte se, že to odpovídá hello hodnoty, které budete používat ve vašem projektu XCode vám umožní vám toouse hello správného profilu publikování s XCode. 
   * **Nabízená oznámení**: Kontrola hello **nabízená oznámení** možnost v hello **App Services** části.
     
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-new-appid-info.png)
     
      Tím vygenerujete ID aplikace a budete tooconfirm hello informace. Klikněte na tlačítko **zaregistrovat** tooconfirm hello nové ID aplikace.
     
      Po kliknutí na tlačítko **zaregistrovat**, zobrazí se hello **dokončení registrace** obrazovky, jak je uvedeno níže. Klikněte na **Done** (Hotovo).
      
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-registration-complete.png)


1. V hello Developer Center, v části ID aplikace vyhledejte ID aplikace hello, který jste právě vytvořili a klikněte na jeho řádek.
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-ios-appids2.png)
   
      Kliknutím na ID aplikace hello se zobrazí podrobnosti o aplikaci hello. Klikněte na tlačítko hello **upravit** tlačítko dole v hello.
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-edit-appid.png)
      
2. Posuňte se toohello dolní části úvodní obrazovka a klikněte na tlačítko hello **vytvořit certifikát...**  tlačítko části hello **Development Push certifikát SSL**.
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-create-cert.png)
   
      Zobrazí Pomocníka "Přidat iOS certifikátu" hello.
   
   > [!NOTE]
   > Tento kurz používá vývojový certifikát. Dobrý den, stejný postup se používá při registraci produkčního certifikátu. Ujistěte se, že používáte hello stejný typ certifikátu při odesílání oznámení.
   > 
   > 
3. Klikněte na tlačítko **zvolit soubor**, procházet toohello umístění, kam jste uložili soubor CSR hello, který jste vytvořili v první úloze hello a pak klikněte na tlačítko **generování**.
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-cert-choose-csr.png)
4. Po vytvoření certifikátu hello hello portálu klikněte na tlačítko hello **Stáhnout** tlačítko a klikněte na tlačítko **provádí**.
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-download-cert.png)
   
      To hello certifikátu a jeho uložení tooyour počítače v složky se staženými soubory.
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-cert-downloaded.png)
   
   > [!NOTE]
   > Ve výchozím nastavení, hello stažený soubor vývojářského certifikátu název **aps_development.cer**.
   > 
   > 
5. Poklikejte na stažený nabízený certifikát hello **aps_development.cer**.
   
      Tím se nainstaluje nový certifikát hello v hello klíčenky, jak je uvedeno níže:
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-cert-in-keychain.png)
   
   > [!NOTE]
   > Název Hello ve vašem certifikátu se může lišit, ale bude předponu **Apple Development iOS Push Services:**.
   > 
   > 
6. V nástroji Keychain Access, klikněte pravým tlačítkem na hello nový nabízený certifikát, který jste vytvořili v hello **certifikáty** kategorie. Klikněte na tlačítko **exportovat**, název souboru hello, vyberte hello **.p12** formátu a potom klikněte na **Uložit**.
   
    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-export-cert-p12.png)
   
    Ujistěte se, poznamenejte si název souboru hello a umístění hello exportovaného certifikátu .p12. Bude tooenable použít ověřování pomocí služby APNS.
   
   > [!NOTE]
   > V tomto kurzu vytvoříme soubor QuickStart.p12. Váš název souboru a umístění se můžou lišit.
   > 
   > 

## <a name="create-a-provisioning-profile-for-hello-app"></a>Vytvořit profil pro zřizování pro aplikace hello
1. Zpět v hello <a href="http://go.microsoft.com/fwlink/p/?LinkId=272456" target="_blank">iOS Provisioning Portal</a>, vyberte **profily zřizování**, vyberte **všechny**a potom klikněte na hello  **+**  tlačítko toocreate nový profil. Spustí se hello **přidání Zřizovacího profilu iOS** Průvodce
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-new-provisioning-profile.png)
2. Vyberte **vývoj aplikací pro iOS** pod **vývoj** jako typ zřizovacího profilu hello a klikněte na **pokračovat**. 
3. Potom vyberte právě vytvořené hello ID aplikace hello **ID aplikace** rozevíracího seznamu a klikněte na **pokračovat**
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-select-appid-for-provisioning.png)
4. V hello **vyberte certifikáty** obrazovky, vyberte svůj obvyklý vývojářský certifikát použít pro podepisování kódu a klikněte na **pokračovat**. Toto není hello nabízený certifikát, který jste právě vytvořili.
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-select-cert.png)
5. Potom vyberte hello **zařízení** toouse pro účely testování a klikněte na tlačítko **pokračovat**
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-select-devices.png)
6. Nakonec název profilu hello v **název profilu**, klikněte na tlačítko **generování**.
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-name-profile.png)
7. Když je vytvořen nový profil pro zřizování hello klikněte na toodownload ho a nainstalujte ji na vývojovém počítači Xcode. Potom klikněte na **Done** (Hotovo).
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-profile-ready.png)
