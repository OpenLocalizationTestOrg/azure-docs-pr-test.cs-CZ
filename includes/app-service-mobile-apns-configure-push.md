

1. Na počítači Mac spusťte **přístup do řetězce klíčů**. Na hello levé navigační panel v části **kategorie**, otevřete **Moje certifikáty**. Najděte certifikát SSL hello, které jste si stáhli v předchozí části hello a zveřejnit, její obsah. Vyberte pouze hello certifikátu (nevybírejte hello privátní klíč), a [ho exportovat](https://support.apple.com/kb/PH20122?locale=en_US).
2. V hello [portál Azure](https://portal.azure.com/), klikněte na tlačítko **Procházet vše** > **App Services**a klikněte na vaše mobilní aplikace back-end. V části **nastavení**, klikněte na tlačítko **aplikace služby Push**a potom klikněte na název vašeho centra oznámení. Přejděte příliš**službu nabízených oznámení Apple** > **nahrát certifikát**. Nahrát soubor .p12 hello, výběr správné hello **režimu** (v závislosti na tom, jestli vašeho klienta SSL certifikát z dříve je produkční nebo izolovaného prostoru). Uložte změny.

Služby je teď nakonfigurovaná toowork s nabízenými oznámeními v systému iOS.

[1]: ./media/app-service-mobile-apns-configure-push/mobile-push-notification-hub.png
