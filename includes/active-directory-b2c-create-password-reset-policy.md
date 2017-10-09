tooenable podrobných heslo resetovat na aplikace, budete potřebovat toocreate zásady resetování hesel. Poznámka: Toto heslo klienta celou hello resetovat zadána možnost [zde](../articles/active-directory-b2c/active-directory-b2c-reference-sspr.md). Tato zásada popisuje hello prostředí, které budou spotřebitelé hello procházejí během resetování hesla a hello obsah tokeny, které hello aplikace se zobrazí při úspěšném dokončení.

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]

V části zásady hello nastavení, vyberte **heslo resetovat zásady** a klikněte na tlačítko **+ přidat**.

![Vyberte zásady registrace nebo přihlášení a klikněte na tlačítko Přidat hello](media/active-directory-b2c-create-password-reset-policy/add-b2c-password-reset-policy.png)

Zadejte zásadu **název** pro tooreference vaší aplikace. Zadejte například `SSPR`.

Vyberte **Zprostředkovatelé identity** a zaškrtněte políčko **Resetovat heslo pomocí e-mailové adresy**. Klikněte na **OK**.

![Vyberte resetovat heslo pomocí e-mailovou adresu jako zprostředkovatel identity a klikněte na tlačítko OK hello](media/active-directory-b2c-create-password-reset-policy/add-b2c-password-reset-identity-providers.png)

Vyberte **Deklarace identit aplikace**. Vyberte, že deklarace identity, které má vrátit v tokenech autorizace hello odeslána zpět tooyour aplikace po úspěšné heslo resetovat prostředí. Vyberte například **ID objektu uživatele**.

![Výběr deklarací identit aplikace a kliknutí na tlačítko OK](media/active-directory-b2c-create-password-reset-policy/add-b2c-password-reset-application-claims.png)

Klikněte na tlačítko **vytvořit** tooadd hello zásad. zásady Hello je uveden jako **B2C_1_SSPR**. Hello **B2C_1_** předpona je název připojení toohello.

Spustit nástroj Zásady hello výběrem **B2C_1_SSPR**. Ověřte nastavení hello zadané v tabulce hello pak klikněte na tlačítko **spustit nyní**.

![Výběr a spuštění zásady](media/active-directory-b2c-create-password-reset-policy/run-b2c-password-reset-policy.png)

| Nastavení      | Hodnota  |
| ------------ | ------ |
| **Aplikace** | Aplikace Contoso B2C |
| **Výběr adresy URL odpovědi** | `https://localhost:44316/` |

Otevře novou kartu prohlížeče a můžete ověřit, že resetování hesla hello prostředí pro uživatele ve vaší aplikaci.

> [!NOTE]
> Zabíral tooa minut pro vytvoření zásad a aktualizuje tootake vliv.
>
