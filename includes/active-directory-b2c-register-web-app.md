[!INCLUDE [active-directory-b2c-portal-add-application](active-directory-b2c-portal-add-application.md)]

tooregister webové aplikace, použijte hello nastavení zadané v tabulce hello.

![Příklad nastavení registrace nové webové aplikace](./media/active-directory-b2c-register-web-app/b2c-new-app-settings.png)

| Nastavení      | Ukázková hodnota  | Popis                                        |
| ------------ | ------- | -------------------------------------------------- |
| **Název** | Aplikace Contoso B2C | Zadejte **název** hello aplikaci, která popisuje tooconsumers vaší aplikace. | 
| **Zahrnout webovou aplikaci nebo webové rozhraní API** | Ano | Pro webovou aplikaci vyberte **Ano**. |
| **Povolit implicitní tok** | Ano | Vyberte **Ano**, pokud vaše aplikace používá [přihlášení OpenID Connect](../articles/active-directory-b2c/active-directory-b2c-reference-oidc.md). |
| **Adresa URL odpovědi** | `https://localhost:44316` | Adresy URL odpovědí jsou koncové body, kam Azure AD B2C vrací všechny tokeny, které vaše aplikace požaduje. Zadejte [vhodnou](../articles/active-directory-b2c/active-directory-b2c-app-registration.md#choosing-a-web-app-or-api-reply-url) **Adresu URL odpovědi**. V tomto příkladu je aplikace místní a naslouchá na portu 44316. |

Klikněte na tlačítko **vytvořit** tooregister vaší aplikace.

Nově zaregistrovaný aplikace se zobrazí v seznamu aplikace hello pro klienta B2C hello. Vyberte ze seznamu hello vaší webové aplikace. Zobrazí se podokno vlastnost Hello webové aplikace.

![Vlastnosti webové aplikace](./media/active-directory-b2c-register-web-app/b2c-web-app-properties.png)

Poznamenejte si hello globálně jedinečný **ID klienta aplikace**. Můžete použít hello ID v kódu aplikace.
