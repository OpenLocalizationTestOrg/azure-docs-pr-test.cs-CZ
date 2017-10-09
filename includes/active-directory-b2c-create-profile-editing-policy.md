profil tooenable úpravy ve vaší aplikaci, budete potřebovat toocreate profil úpravy zásad. Tato zásada popisuje hello prostředí, které budou uživatelé procházejí během profil úpravy a hello obsah tokeny, které aplikace hello obdrží při úspěšném dokončení.

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]

V části zásady hello nastavení, vyberte **zásady pro úpravy profilu** a klikněte na tlačítko **+ přidat**.

![Vyberte zásady pro úpravy profilu a klikněte na tlačítko Přidat hello](media/active-directory-b2c-create-profile-editing-policy/add-b2c-editing-policy.png)

Zadejte zásadu **název** pro tooreference vaší aplikace. Zadejte například `SiPe`.

Vyberte **Zprostředkovatelé identity** a zaškrtněte políčko **Přihlášení místním účtem**. Volitelně můžete vybrat také zprostředkovatele sociální identity, pokud už jsou nakonfigurováni. Klikněte na **OK**.

![Vyberte místní přihlášení účtu jako zprostředkovatel identity a klikněte na tlačítko OK hello](media/active-directory-b2c-create-profile-editing-policy/add-b2c-editing-identity-providers.png)

Vyberte **Atributy profilu**. Vyberte atributy hello příjemce můžete zobrazit a upravit v svůj profil. Zaškrtněte například políčka **Země/oblast**, **Zobrazované jméno** a **PSČ**. Klikněte na **OK**.

![Vyberte některých atributů a klikněte na tlačítko OK hello](media/active-directory-b2c-create-profile-editing-policy/add-b2c-editing-attributes.png)

Vyberte **Deklarace identit aplikace**. Vyberte, že deklarace identity, které má vrátit v tokenech autorizace hello odeslána zpět tooyour aplikace po úspěšné profil prostředí pro úpravy. Vyberte například **Zobrazované jméno** a **PSČ**.

![Výběr deklarací identit aplikace a kliknutí na tlačítko OK](media/active-directory-b2c-create-profile-editing-policy/add-b2c-editing-application-claims.png)

Klikněte na tlačítko **vytvořit** tooadd hello zásad. zásady Hello je uveden jako **B2C_1_SiPe**. Hello **B2C_1_** předpona je název připojení toohello.

Spustit nástroj Zásady hello výběrem **B2C_1_SiPe**. Ověřte nastavení hello zadané v tabulce hello pak klikněte na tlačítko **spustit nyní**.

![Výběr a spuštění zásady](media/active-directory-b2c-create-profile-editing-policy/run-b2c-editing-policy.png)

| Nastavení      | Hodnota  |
| ------------ | ------ |
| **Aplikace** | Aplikace Contoso B2C |
| **Výběr adresy URL odpovědi** | `https://localhost:44316/` |

Otevře novou kartu prohlížeče a můžete ověřit podle konfigurace pro úpravy prostředí pro uživatele hello profilu.

> [!NOTE]
> Zabíral tooa minut pro vytvoření zásad a aktualizuje tootake vliv.
>