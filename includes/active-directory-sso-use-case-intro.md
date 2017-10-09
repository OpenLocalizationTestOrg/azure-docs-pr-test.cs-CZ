Organizace používá více [Software jako služba (SaaS)](https://azure.microsoft.com/overview/what-is-saas/) aplikací pro produktivitu protože cloudové technologie a nástroje jsou čím dál víc snadno dostupné. S růstem hello počet aplikací SaaS stane náročné hello správci toomanage účty a oprávnění a pro hello uživatelé tooremember jejich různá hesla. Správu těchto aplikací jednotlivě vytvoří další práci a je méně bezpečné.

* Zaměstnanci, kteří mají tookeep zaznamenávat mnoho hesla, zpravidla toouse méně bezpečné tooremember metody, které je zápis dolů hesla nebo pomocí hello stejnými hesly napříč mnoha účty.
* Když dorazí nového zaměstnance nebo jeden opustí, všechny své účty musí být jednotlivě zřízený nebo zrušte zřízený.
* Kromě toho zaměstnanci mohou začít používat aplikace SaaS pro práci bez průchodu přes IT, což znamená, že vytváříte svoje vlastní účty v systémech, které správci IT hello neschválili a nejsou monitorování.  

Jednotné přihlašování (SSO) je řešení pro všechny tyto problémy. Ho má hello nejjednodušší způsob, jak toomanage víc aplikací a poskytněte uživatelům konzistentní prostředí přihlášení. Azure Active Directory (Azure AD) poskytuje robustní řešení jednotné přihlašování a má k dispozici mnoho předem integrovaných aplikací, s kurzy pro správce tooquickly nastavit nové aplikace a spustit zřizování uživatelů.

## <a name="how-does-azure-active-directory-integrate-apps"></a>Jak Azure Active Directory integraci aplikace?
Azure AD můžete toointegrate vaše aplikace a zřízené účty. To lze provést prostřednictvím buď dva přístupy.

* Pokud aplikace hello je předem integrovaných v aplikaci hello galerie, můžete projít tento portál tooset si aplikace a nakonfigurovat tooallow hello nastavení jednotného přihlašování. Pro žádné Galerie aplikace můžete začít používat podle postupujte podle hello jednoduché podrobné pokyny uvedené v galerii aplikace hello a hello Azure portálu tooenable jednotné přihlašování.
* Pokud aplikace hello není v hello galerie, může stále nastavíte většinu aplikací ve službě Azure AD jako vlastní aplikaci. To vyžaduje trochu další technické znalosti tooconfigure. Přidáním jakékoli aplikace, který podporuje SAML 2.0 jako federované aplikace nebo jakékoli aplikace, která má HTML na přihlašovací stránku jako heslo jednotného přihlašování k aplikaci.

V hello případu, kdy uživatelé vytvořili svoje vlastní účty pro aplikace SaaS, které nejsou spravovány IT, hello [Cloud App Discovery](../articles/active-directory/active-directory-cloudappdiscovery-whatis.md) nástroj poskytuje řešení. Tento nástroj monitoruje hello webový provoz tooidentify, které aplikace jsou používány v celé organizaci hello a hello počet lidí pomocí každého z nich. IT pomocí této informace toolearn, jaké aplikace hello uživatelé přednost a rozhodnout, které toointegrate do služby Azure AD pro jednotné přihlašování.  

Při integraci aplikace do Azure AD, můžete namapovat identity tootheir příslušných Azure AD identity zavedené aplikace hello uživatelů.  

