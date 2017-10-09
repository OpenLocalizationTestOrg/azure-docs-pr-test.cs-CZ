[!INCLUDE [active-directory-b2c-portal-add-application](active-directory-b2c-portal-add-application.md)]

tooregister mobilní nebo nativní aplikaci použít hello nastavení zadané v tabulce hello.

![Příklad nastavení registrace nové mobilní nebo nativní aplikace](./media/active-directory-b2c-register-mobile-native-app/b2c-new-mobile-native-app-settings.png)

| Nastavení      | Ukázková hodnota  | Popis                                        |
| ------------ | ------- | -------------------------------------------------- |
| **Název** | Aplikace Contoso B2C | Zadejte **název** hello aplikaci, která popisuje tooconsumers vaší aplikace. |
| **Nativní klient** | Ano | Pro mobilní nebo nativní aplikaci vyberte **Ano**. |
| **Vlastní identifikátor URI přesměrování** | `com.onmicrosoft.contoso.appname://redirect/path` | Zadejte identifikátor URI přesměrování s vlastním schématem. Nezapomeňte zvolit [dobrý identifikátor URI přesměrování](../articles/active-directory-b2c/active-directory-b2c-app-registration.md#choosing-a-native-application-redirect-uri) a nepoužívat speciální znaky jako podtržítka. |

Klikněte na tlačítko **vytvořit** tooregister vaší aplikace.

Nově zaregistrovaný aplikace se zobrazí v seznamu aplikace hello pro klienta B2C hello. Vyberte svou mobilní nebo nativní aplikaci ze seznamu hello. Zobrazí se podokno vlastností aplikace Hello.

![Vlastnosti aplikace](./media/active-directory-b2c-register-mobile-native-app/b2c-mobile-native-app-properties.png)

Poznamenejte si hello globálně jedinečný **ID klienta aplikace**. Můžete použít hello ID v kódu aplikace.

Pokud vaše nativní aplikace volá webové rozhraní API zabezpečené pomocí Azure AD B2C, proveďte tyto kroky:
   1. Vytvořte tajný klíč aplikace tak, že budete toohello **klíče** okno a kliknutím na hello **vygenerovat klíč** tlačítko. Poznamenejte si hello **klíč aplikace** hodnotu. Použijte hodnotu hello jako tajný klíč aplikace hello v kódu aplikace.
   2. Klikněte na **Přístup přes rozhraní API**, pak na **Přidat** a vyberte vaše webové rozhraní API a obory (oprávnění).

> [!NOTE]
> **Tajný klíč aplikace** je důležitý údaj zabezpečení a musí být řádně zabezpečen.
> 
