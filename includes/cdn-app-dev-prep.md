## <a name="prerequisites"></a>Požadavky
Před jsme můžete napsat kód správu CDN, potřebujeme toodo některé přípravy tooenable naše toointeract kód s hello Azure Resource Manager.  toodo, budete muset:

* Vytvoření prostředku skupiny toocontain hello profil CDN, které jsme v tomto kurzu vytvoříte
* Konfigurace ověřování tooprovide Azure Active Directory pro naši aplikaci
* Použití oprávnění, která toohello skupinu prostředků, aby jen na autorizované uživatele z našich klienta Azure AD mohou komunikovat s naše profil CDN

### <a name="creating-hello-resource-group"></a>Vytvoření skupiny prostředků hello
1. Přihlaste se k hello [portálu Azure](https://portal.azure.com).
2. Klikněte na tlačítko hello **nový** tlačítko v levé horní části hello a potom **správy**, a **skupiny prostředků**.

    ![Vytvoření nové skupiny prostředků](./media/cdn-app-dev-prep/cdn-new-rg-1-include.png)
3. Volání vaší skupiny prostředků *CdnConsoleTutorial*.  Vyberte předplatné a zvolte umístění okolo vás.  Pokud chcete, můžete kliknutím na hello **Pin toodashboard** políčko toopin hello prostředků skupiny toohello řídicí panel portálu hello.  To znamená, že jednodušší toofind později.  Po provedení váš výběr, klikněte na tlačítko **vytvořit**.

    ![Názvy skupiny prostředků hello](./media/cdn-app-dev-prep/cdn-new-rg-2-include.png)
4. Po vytvoření skupiny prostředků hello, pokud nebyla připnete ji tooyour řídicí panel, najdete ho kliknutím na **Procházet**, pak **skupiny prostředků**.  Klikněte na tlačítko tooopen skupiny prostředků hello ho.  Poznamenejte si vaše **ID předplatného**.  Budeme ho později potřebovat.

    ![Názvy skupiny prostředků hello](./media/cdn-app-dev-prep/cdn-subscription-id-include.png)

### <a name="creating-hello-azure-ad-application-and-applying-permissions"></a>Vytvoření aplikace hello Azure AD a použití oprávnění
Existují dva přístupy tooapp ověřování s Azure Active Directory: jednotlivé uživatele nebo hlavní název služby. Hlavní název služby je podobné tooa účtu služby v systému Windows.  Místo udělení toointeract oprávnění určitého uživatele pomocí profilů CDN hello, jsme místo udělení oprávnění hello toohello instanční objekt.  Objekty služby se obecně používají pro neinteraktivní, automatizované procesy.  I když v tomto kurzu je zápis interaktivní konzoly aplikace, budete jsme se zaměřit na hello služby hlavní přístup.

Vytvoření objektu služby se skládá z několika kroků, včetně vytváření aplikací Azure Active Directory.  toodo toho vytvoříme příliš[v tomto kurzu](../articles/resource-group-create-service-principal-portal.md).

> [!IMPORTANT]
> Být jisti toofollow všechny kroky hello v hello [propojené kurzu](../articles/resource-group-create-service-principal-portal.md).  Je *velmi důležité* dokončit ho přesně tak, jak je popsáno.  Ujistěte se, že toonote vaše **ID klienta**, **název domény klienta** (běžně *. onmicrosoft.com* domény Pokud jste zadali vlastní domény), **ID klienta** , a **ověřovací klíč klienta**, jak jsme budete potřebovat později.  Se velmi pečlivě tooguard vaše **ID klienta** a **ověřovací klíč klienta**, jak tyto přihlašovací údaje mohou být použity kýmkoli tooexecute operace jako hello instanční objekt.
>
> Když získáte toohello krok s názvem konfigurace víceklientské aplikace, vyberte **ne**.
>
> Když získáte toohello krok [přiřadit toorole aplikace](../articles/azure-resource-manager/resource-group-create-service-principal-portal.md#assign-application-to-role), skupinu prostředků použijte hello jsme vytvořili předtím, *CdnConsoleTutorial*, ale místo hello **čtečky** role, přiřadit Hello **Přispěvatel profil CDN** role.  Po přiřazení hello aplikace hello **Přispěvatel profil CDN** role ve vaší skupině prostředků, návratový toothis kurzu. 
>
>

Po vytvoření hello hlavní a přiřazené vaší služby **Přispěvatel profil CDN** role, hello **uživatelé** okno skupiny prostředků by měl vypadat podobně jako toothis.

![Okno Uživatelé](./media/cdn-app-dev-prep/cdn-service-principal-include.png)

### <a name="interactive-user-authentication"></a>Interaktivním ověřování uživatelů
Pokud místo hlavní název služby, byste měli místo ověřování interaktivní jednotlivých uživatelů, proces hello je velmi podobné toothat pro hlavní název služby.  Ve skutečnosti, budete potřebovat toofollow hello stejným způsobem, ale provést několik menších změn.

> [!IMPORTANT]
> Následujícím postupem další, pouze pokud zvolíte ověřování jednotlivých uživatelů toouse místo instanční objekt.
>
>

1. Při vytváření aplikace, místo **webové aplikace**, zvolte **nativní aplikace**.

    ![Nativní aplikace](./media/cdn-app-dev-prep/cdn-native-application-include.png)
2. Na další stránku hello, zobrazí se výzva pro **identifikátor URI pro přesměrování**.  Hello URI nebude ověřena, ale mějte na paměti, jste zadali.  Budete ho potřebovat později.
3. Neexistuje žádné nutné toocreate **ověřovací klíč klienta**.
4. Místo přiřazení hlavní toohello služby **Přispěvatel profil CDN** role, vytvoříme tooassign jednotlivé uživatele nebo skupiny.  V tomto příkladu uvidíte, že jste přidělený *uživatel Demo CDN* toohello **Přispěvatel profil CDN** role.  

    ![Přístup jednotlivých uživatelů](./media/cdn-app-dev-prep/cdn-aad-user-include.png)
