přihlášení tooenable na vaší aplikace, budete potřebovat toocreate sign-in zásady. Tato zásada popisuje hello prostředí, které budou spotřebitelé procházejí během přihlášení a hello obsah tokeny, které hello aplikace se zobrazí na úspěšné přihlášení.

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]Klikněte na **Zásady přihlašování**.

Klikněte na tlačítko **+ přidat** hello horní části okna hello.

Hello **název** Určuje název zásady přihlášení hello používá vaše aplikace. Zadejte například **SiIn**.

Klikněte na **Zprostředkovatelé identity** a vyberte **Přihlášení místním účtem**. Volitelně můžete vybrat také zprostředkovatele sociální identity, pokud už jsou nakonfigurováni. Klikněte na **OK**.

Klikněte na **Deklarace identit aplikace**. Zde se rozhodnout, že deklarace identity, které mají být vráceny v tokenech hello odeslána zpět tooyour aplikace po úspěšné možností přihlašování. Vyberte například **Zobrazované jméno**, **Zprostředkovatel identity**, **PSČ** a **ID objektu uživatele**. Klikněte na **OK**.

Klikněte na možnost **Vytvořit**. Všimněte si, že zásady hello právě vytvořili, zobrazuje jako **B2C_1_SiIn** (hello **B2C\_1\_**  se automaticky přidá fragment) v hello **zásady přihlášení**okno.

Kliknutím otevřete hello zásad **B2C_1_SiIn**.

Vyberte **aplikace Contoso B2C** v hello **aplikace** rozevíracího seznamu a `https://localhost:44321/` v hello **adresa URL odpovědi nebo identifikátor URI pro přesměrování** rozevíracího seznamu.

Klikněte na **Spustit**. Otevře novou kartu prohlížeče a mohou být spuštěny prostřednictvím prostředí příjemce hello podepisování do vaší aplikace.

> [!NOTE]
> Zabíral tooa minut pro vytvoření zásad a aktualizuje tootake vliv.
>