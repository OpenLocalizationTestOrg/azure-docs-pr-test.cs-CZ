[!INCLUDE [active-directory-b2c-portal-add-application](active-directory-b2c-portal-add-application.md)]

tooregister webového rozhraní API, použijte nastavení hello zadané v tabulce hello.

![Příklad nastavení registrace nového webového rozhraní API](./media/active-directory-b2c-register-web-api/b2c-new-web-api-settings.png)

| Nastavení      | Ukázková hodnota  | Popis                                        |
| ------------ | ------- | -------------------------------------------------- |
| **Název** | API Contoso B2C | Zadejte **název** hello aplikaci, která popisuje tooconsumers vaše rozhraní API. | 
| **Zahrnout webovou aplikaci nebo webové rozhraní API** | Ano | Pro webové rozhraní API vyberte **Ano**. |
| **Povolit implicitní tok** | Ano | Vyberte **Ano**, pokud vaše aplikace používá [přihlášení OpenID Connect](../articles/active-directory-b2c/active-directory-b2c-reference-oidc.md). |
| **Adresa URL odpovědi** | `https://localhost:44316/` | Adresy URL odpovědí jsou koncové body, kam Azure AD B2C vrací všechny tokeny, které vaše aplikace požaduje. Zadejte [vhodnou](../articles/active-directory-b2c/active-directory-b2c-app-registration.md#choosing-a-web-app-or-api-reply-url) **Adresu URL odpovědi**. V tomto příkladu je webového rozhraní API místní a naslouchá na portu 44316. |
| **Identifikátor URI ID aplikace** | rozhraní api | Hello identifikátor ID URI aplikace je identifikátor hello použitý pro webového rozhraní API. Hello úplný identifikátor, který je pro vás vygeneroval URI včetně hello domény. |

Klikněte na tlačítko **vytvořit** tooregister vaší aplikace.

Nově zaregistrovaný aplikace se zobrazí v seznamu aplikace hello pro klienta B2C hello. Vyberte ze seznamu hello webového rozhraní API. Zobrazí se podokno vlastnost Hello rozhraní API.

![Vlastnosti webového rozhraní API](./media/active-directory-b2c-register-web-api/b2c-web-api-properties.png)

Poznamenejte si hello globálně jedinečný **ID klienta aplikace**. Můžete použít hello ID v kódu aplikace.
