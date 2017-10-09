
1. V Průzkumníku řešení Visual Studio, klikněte pravým tlačítkem na projekt aplikace Windows Store hello a klikněte na tlačítko **úložiště** > **propojit aplikaci se hello Store**.

    ![Přidružit aplikace Windows Store](./media/app-service-mobile-register-wns/notification-hub-associate-win8-app.png)
2. V Průvodci hello, klikněte na tlačítko **Další**a přihlaste se pomocí účtu Microsoft. Zadejte název aplikace v rámci **rezervovat nový název aplikace**a potom klikněte na **rezervy**.
3. Po registraci aplikace hello je úspěšně vytvořil, vyberte hello nový název aplikace, klikněte na tlačítko **Další**a potom klikněte na **přidružit**. Tento postup přidá požadované hello Windows Store registrační informace toohello manifest aplikace.
4. Opakujte kroky 1 a 3 pro projekt aplikace Windows Phone Store hello pomocí hello stejné registrace, který jste dříve vytvořili pro aplikace pro Windows Store hello.  
5. Procházet toohello [Centrum vývojářů pro Windows](https://dev.windows.com/en-us/overview)a přihlaste se pomocí účtu Microsoft. Klikněte na nové registrace aplikace hello v **Moje aplikace**a potom rozbalte **služby** > **nabízená oznámení**.
6. Na hello **nabízená oznámení** klikněte na tlačítko **Web služeb Live Services** pod **nabízených oznámení Windows služby (WNS) a Microsoft Azure Mobile Apps**. Poznamenejte si hodnoty hello hello **SID balíčku** a hello *aktuální* hodnotu **tajný klíč aplikace**. 

    ![Nastavení aplikace ve středisku pro vývojáře hello](./media/app-service-mobile-register-wns/mobile-services-win8-app-push-auth.png)

   > [!IMPORTANT]
   > tajný klíč aplikace Hello a SID balíčku jsou důležitá pověření zabezpečení. Tyto hodnoty s nikým nesdílejte ani je nedistribuujte s vaší aplikací.
   >
   >
