
1. Přejděte toohello [Google Cloud Console](https://console.developers.google.com/project), přihlaste se pomocí svých přihlašovacích údajů účtu Google. 
2. Klikněte na **Create Project** (Vytvořit projekt), zadejte název projektu a potom klikněte na **Create** (Vytvořit). Pokud požadovaný, provádět hello ověření SMS a klikněte na **vytvořit** znovu.
   
    ![Vytvoření nového projektu](./media/mobile-services-enable-google-cloud-messaging/mobile-services-google-new-project.png)   
   
     Zadejte novou položku **Project name** (Název projektu) a klikněte na **Create project** (Vytvořit projekt).
3. Klikněte na tlačítko hello **nástroje a další** tlačítko a pak klikněte na **informace o projektu**. Poznamenejte si hello **číslo projektu**. Budete potřebovat tooset tuto hodnotu jako hello `SenderId` proměnné v hello klientskou aplikaci.
   
    ![Nástroje a další](./media/mobile-services-enable-google-cloud-messaging/notification-hubs-utilities-and-more.png)
4. V hello projektu řídicího panelu, v části **mobilních rozhraní API**, klikněte na tlačítko **Google Cloud Messaging**, klepnutím na další stránku hello **povolit rozhraní API** a přijměte podmínky hello služby. 
   
    ![Povolení GCM](./media/mobile-services-enable-google-cloud-messaging/enable-GCM.png)
   
    ![Povolení GCM](./media/mobile-services-enable-google-cloud-messaging/enable-gcm-2.png) 
5. Na řídicím panelu projektu hello, klikněte na tlačítko **pověření** > **vytvoření pověření** > **klíč rozhraní API**. 
   
    ![](./media/mobile-services-enable-google-cloud-messaging/mobile-services-google-create-server-key.png)
6. V části **Create a new key** (Vytvořit nový klíč) klikněte na **Server key** (Klíč serveru), zadejte název klíče a potom klikněte na **Create** (Vytvořit).
7. Poznamenejte si hello **klíč rozhraní API** hodnotu.
   
    Budete používat tento tooenable hodnotu klíče rozhraní API Azure tooauthenticate s GCM a odesílání nabízených oznámení jménem vaší aplikace.

