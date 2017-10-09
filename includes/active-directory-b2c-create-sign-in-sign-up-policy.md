přihlášení tooenable na vaší aplikace, budete potřebovat toocreate sign-in zásady. Tato zásada popisuje hello prostředí, které budou spotřebitelé procházejí během přihlášení a hello obsah tokeny, které hello aplikace se zobrazí na úspěšné přihlášení.

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]

V části zásady hello nastavení, vyberte **zásady registrace nebo přihlášení** a klikněte na tlačítko **+ přidat**.

![Výběr zásad registrace nebo přihlašování a kliknutí na tlačítko Přidat](media/active-directory-b2c-create-sign-in-sign-up-policy/add-b2c-signup-signin-policy.png)

Zadejte zásadu **název** pro tooreference vaší aplikace. Zadejte například `SiUpIn`.

Vyberte **Zprostředkovatelé identity** a zaškrtněte políčko **Registrace pomocí e-mailu**. Volitelně můžete vybrat také zprostředkovatele sociální identity, pokud už jsou nakonfigurováni. Klikněte na **OK**.

![Vyberte e-mailu registrace jako zprostředkovatele identity a klikněte na tlačítko OK hello](media/active-directory-b2c-create-sign-in-sign-up-policy/add-b2c-signup-signin-identity-providers.png)

Vyberte **Atributy registrace**. Vyberte atributy, chcete-li toocollect z hello příjemce během registrace. Zaškrtněte například políčka **Země/oblast**, **Zobrazované jméno** a **PSČ**. Klikněte na **OK**.

![Vyberte některých atributů a klikněte na tlačítko OK hello](media/active-directory-b2c-create-sign-in-sign-up-policy/add-b2c-signup-signin-sign-up-attributes.png)

Vyberte **Deklarace identit aplikace**. Vyberte, že deklarace identity, které má vrátit v tokenech autorizace hello odeslána zpět tooyour aplikace po úspěšné registraci nebo přihlašovací prostředí. Vyberte například **Zobrazované jméno**, **Zprostředkovatel identity**, **PSČ**, **Uživatel je nový** a **ID objektu uživatele**.

![Výběr deklarací identit aplikace a kliknutí na tlačítko OK](media/active-directory-b2c-create-sign-in-sign-up-policy/add-b2c-signup-signin-application-claims.png)

Klikněte na tlačítko **vytvořit** tooadd hello zásad. zásady Hello je uveden jako **B2C_1_SiUpIn**. Hello **B2C_1_** předpona je název připojení toohello.

Spustit nástroj Zásady hello výběrem **B2C_1_SiUpIn**. Ověřte nastavení hello zadané v tabulce hello pak klikněte na tlačítko **spustit nyní**.

![Výběr a spuštění zásady](media/active-directory-b2c-create-sign-in-sign-up-policy/run-b2c-signup-signin-policy.png)

| Nastavení      | Hodnota  |
| ------------ | ------ |
| **Aplikace** | Aplikace Contoso B2C |
| **Výběr adresy URL odpovědi** | `https://localhost:44316/` |

Otevře novou kartu prohlížeče a můžete ověřit činnost registrace nebo přihlášení příjemce hello podle konfigurace.

> [!NOTE]
> Zabíral tooa minut pro vytvoření zásad a aktualizuje tootake vliv.
>