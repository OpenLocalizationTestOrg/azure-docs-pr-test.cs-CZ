tooenable registrace v aplikaci, musíte toocreate registrační zásadě. Tato zásada popisuje hello prostředí, které uživatelé procházejí během registrace a přijme obsah hello tokeny, které hello aplikace na úspěšné zápisy.

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]

Klikněte na **Zásady registrace**.

Klikněte na tlačítko **+ přidat** hello horní části okna hello.

Hello **název** Určuje název registrační zásadě hello používá vaše aplikace. Zadejte například **SiUp**.

Klikněte na **Zprostředkovatelé identity** a vyberte **Registrace pomocí e-mailu**. Volitelně můžete vybrat také zprostředkovatele sociální identity, pokud už jsou nakonfigurováni. Klikněte na **OK**.

Klikněte na **Atributy registrace**. Zde vyberte atributy, že chcete toocollect z hello příjemce během registrace. Vyberte například **Země/oblast**, **Zobrazované jméno** a **PSČ**. Klikněte na **OK**.

Klikněte na **Deklarace identit aplikace**. Zde se rozhodnout, že deklarace identity, které mají být vráceny v tokenech hello odeslána zpět tooyour aplikace po úspěšné registraci praxe. Vyberte například **Zobrazované jméno**, **Zprostředkovatel identity**, **PSČ**, **Uživatel je nový** a **ID objektu uživatele**.

Klikněte na možnost **Vytvořit**. Hello vytvořena zásada se zobrazí jako **B2C_1_SiUp** (hello **B2C\_1\_**  se automaticky přidá fragment) v hello **registrace zásady** okno.

Kliknutím otevřete hello zásad **B2C_1_SiUp**.

Vyberte **aplikace Contoso B2C** v hello **aplikace** rozevíracího seznamu a `https://localhost:44321/` v hello **adresa URL odpovědi nebo identifikátor URI pro přesměrování** rozevíracího seznamu.

Klikněte na **Spustit**. Otevře novou kartu prohlížeče a mohou být spuštěny prostřednictvím prostředí příjemce hello registrace do vaší aplikace.

> [!NOTE]
> Zabíral tooa minut pro vytvoření zásad a aktualizuje tootake vliv.
>