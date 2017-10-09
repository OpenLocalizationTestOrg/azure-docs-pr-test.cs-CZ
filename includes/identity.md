Správa identity je stejně důležité ve veřejném cloudu hello jako na místní. toohelp s tímto Azure podporuje několik různých cloudových technologií identity. Tato nastavení zahrnují tyto:

* Windows Server Active Directory (označovaného jako právě AD) můžete spustit v cloudu hello pomocí virtuální počítače vytvořené pomocí Azure virtuálních počítačů. Tento přístup má smysl při použití Azure tooextend překážek místní datacentra do cloudu hello.
* Můžete použít Azure Active Directory toogive jeden uživatelům přihlášení příliš[Software jako služba (SaaS)](https://azure.microsoft.com/overview/what-is-saas/) aplikace. Společnosti Microsoft Office 365 používá tuto technologii, například a aplikace běžící v Azure nebo jiných platforem cloudu jej také použít.
* Aplikace běžící v hello cloudu nebo místně pomocí Azure Active Directory řízení přístupu uživatelů toolet Přihlaste se pomocí identit ze sítě Facebook, Google, Microsoft a jiných poskytovatelů identit.

Tento článek popisuje všechny tři z těchto možností.

## <a name="table-of-contents"></a>Obsah
* [Systémem Windows Server Active Directory ve virtuálních počítačích](#adinvm)
* [Pomocí služby Azure Active Directory](#ad)
* [Pomocí řízení přístupu Azure Active Directory](#ac)

## <a name="adinvm"></a>Systémem Windows Server Active Directory ve virtuálních počítačích
Systém Windows Server AD ve virtuálních počítačích Azure je podobné jako u spuštěním místně. [Obrázek 1](#fig1) ukazuje, jak to vypadá typický příklad.

![Azure Active Directory ve virtuálním počítači](./media/identity/identity_01_ADinVM.png)

<a name="Fig1"></a>Obrázek 1: Windows Server Active Directory můžete spustit v Azure virtuální počítače připojené tooan organizace místního datového centra pomocí Azure Virtual Network.

V příkladu hello tady uvedené běží Windows Server AD ve virtuálních počítačích, které jsou vytvořené pomocí virtuálních počítačů Azure IaaS technologie hello platformy. Tyto virtuální počítače a několik dalších se seskupují do překážek místní datacentra tooan připojené virtuální sítě pomocí virtuální sítě Azure. virtuální síť Hello odřízne skupiny, do cloudu virtuálních počítačů, které pracují se hello do místní sítě prostřednictvím připojení virtuální privátní sítě (VPN). Díky této lze tyto virtuální počítače Azure vypadat jako jakékoli jiné podsítě toohello místního datového centra. Jak ukazuje obrázek hello, dva z těchto virtuálních počítačů se systémem řadiče domény systému Windows Server AD. Hello jiné virtuální počítače ve virtuální síti hello může používat aplikace, například SharePoint, nebo se používá jiným způsobem, například pro vývoj a testování. Hello místního datového centra je spuštěna také dva řadiče domény systému Windows Server AD.

Existuje několik možností pro připojování hello řadiče domény v cloudu hello s spuštěné místně:

* Ujistěte se, všech těchto součástí domény služby Active Directory.
* Vytvoření samostatné domény místního prostředí AD a v cloudu hello, které jsou součástí hello stejné doménové struktuře.
* Vytvořit samostatný doménových struktur služby AD v cloudu hello a místní a pak připojit hello doménovými strukturami pomocí vztahy důvěryhodnosti mezi doménovými strukturami nebo Windows Server Active Directory Federation Services (AD FS), který můžete taky spustit ve virtuálních počítačích na platformě Azure.

Ať volba se provádí, správce měli ujistit, že žádosti o ověření z místních uživatelů přejděte řadiče domény toocloud pouze v případě potřeby, protože hello odkaz toohello cloudu je pravděpodobně toobe nižší než místní sítě. Další faktor tooconsider v připojování cloudové a místní řadiče domény je hello přenosy dat vytvářené pomocí replikace. Řadiče domény v cloudu hello jsou obvykle ve své vlastní lokalitě Active Directory, která umožňuje Správce tooschedule jak často se provádí replikace. Azure poplatky pro přenosy mimo datového centra Azure, i když není pro bajtů odeslaných v, což může mít vliv volby hello Správce replikace. Je také vhodné odkazující, že zatímco Azure poskytují vlastní podporu systému DNS (Domain Name), tato služba chybí funkce vyžadované službou Active Directory (jako je podpora pro záznamy DNS dynamické a SRV). Z tohoto důvodu se systémem Windows Server AD ve službě Azure vyžaduje nastavení vlastní servery DNS v cloudu hello.

Systém Windows Server AD ve virtuálních počítačích Azure můžete mít smysl v několika různých situacích. Zde je několik příkladů:

* Pokud používáte Azure Virtual Machines jako rozšíření vlastního datového centra, může spouštět aplikace hello cloudu, který potřebují akcí toohandle řadiče místní domény, například požadavky integrované ověřování systému Windows nebo dotazů protokolu LDAP. SharePoint, například často komunikuje se službou Active Directory, a proto i když je možné toorun farmy služby SharePoint v Azure pomocí místního adresáře, nastavení řadiče domény v cloudu hello se výrazně zlepšit výkon. (Je důležité toorealize, že to není nezbytně nutné, ale; hodně aplikací lze úspěšně spustit v cloudu hello použití pouze místní řadičů domény.)
* Předpokládejme, že vzdálených pobočce chybí hello prostředky toorun vlastní řadiče domény. V současné době jeho uživatelé musí ověřit toodomain řadiče na hello druhé straně hello world – přihlášení jsou pomalé. Spuštění služby Active Directory v Azure v datacentru společnosti Microsoft blíže urychlit to bez nutnosti další servery v pobočce hello.
* Organizace, která používá Azure pro zotavení po havárii mohou udržovat malého active virtuálních počítačů v cloudu hello, včetně řadič domény. Potom může být tooexpand tato lokalita připravená jako potřebné tootake přes selhání jinde.

Existují také další možnosti. Například nejste požadované tooconnect systému Windows Server AD v cloudu tooan hello místního datového centra. Pokud byste chtěli toorun farmu služby SharePoint, která obsluhuje konkrétní sadu uživatelů, například všechny kterého by přihlásit pouze s identitami, založené na cloudu, můžete v Azure vytvořit samostatné doménové struktuře. Jak použít tuto technologii, závisí na jaké jsou vaše cíle. (Podrobné pokyny k systému Windows Server AD pomocí Azure, [zde](http://msdn.microsoft.com/library/windowsazure/jj156090.aspx).)

## <a name="ad"></a>Pomocí služby Azure Active Directory
Protože aplikace SaaS stále častěji, vyvolají napadne Otázka: jaký druh adresářová služba by měla tyto cloudové aplikace používají? Otázka toothat odpovědí společnosti Microsoft je Azure Active Directory.

Existují dvě hlavní možnosti pro použití v cloudu hello tato adresářová služba:

* Jednotlivce a organizace, které používají jenom SaaS aplikace můžete závisí na Azure Active Directory jako jejich jedinou adresářové služby.
* Organizace, se systémem Windows Server Active Directory můžete připojit tooAzure jejich místní adresář služby Active Directory a pak ho použít toogive uživatelé jednotné přihlašování tooSaaS aplikace.

[Obrázek 2](#fig2) znázorňuje hello první z těchto dvou možností, kde všechny, které je nutné je Azure Active Directory.

![Azure Active Directory ve virtuálním počítači](./media/identity/identity_02_AD.png)

<a name="fig2"></a>Obrázek 2: Azure Active Directory poskytuje uživatelům v organizaci tooSaaS přihlášení aplikace, včetně služeb Office 365.

Jak ukazuje obrázek hello, Azure AD je víceklientské služby. To znamená, že současně podporuje mnoho různých organizací ukládání directory informace o uživatelích v každé z nich. V tomto příkladu je uživatel v organizaci A probíhá tooaccess aplikaci SaaS. Tato aplikace může být součástí Office 365, jako je například služby SharePoint Online, nebo může být něco jiného - tuto technologii můžete také použít aplikací od jiných výrobců. Protože Azure AD podporuje protokol hello SAML 2.0, všechny možnosti, které vyžaduje z aplikace hello možnost toointeract používá tento oborový standard. (Ve skutečnosti aplikace, které používají Azure AD můžete spustit v jakékoli datacenter, ne jenom datového centra Azure.)

proces Hello začíná, když hello uživatel získá přístup k aplikaci SaaS (krok 1). toouse této aplikace hello uživatel musí poskytnout tokenem vydaným službou Azure AD.

Tento token obsahuje informace, které identifikují hello uživatele a je digitálně podepsané službou Azure AD. tooget hello token, hello uživatel se ověřuje sám tooAzure AD zadáním uživatelského jména a hesla (krok 2). Azure AD poté vrátí hello token potřebné informace (krok 3).

Tento token se pak odešlou toohello SaaS aplikace (krok 4), který ověří podpis hello token a používá jeho obsahem (krok 5). Obvykle hello aplikace bude používat informace o identitě hello hello token obsahuje toodecide, jaké informace hello uživatel tooaccess a případně jinými způsoby.

Pokud aplikace hello potřebuje další informace o uživateli hello než co je obsažených v tokenu hello, je tato žádost přímo z Azure AD pomocí Azure AD Graph API hello (krok 6). V hello počáteční verze služby Azure AD, je poměrně jednoduché schématu adresáře hello: obsahuje pouze uživatelé a skupiny a vztahů mezi nimi. Aplikace můžete použít tento toolearn informace o připojeních mezi uživateli. Předpokládejme například, že potřebuje tooknow, který správce tento uživatel je toodecide, zda je povolen přístup toosome bloku dat aplikace. Můžete to informace pomocí dotazu na Azure AD prostřednictvím hello rozhraní Graph API.

Hello rozhraní Graph API používá obyčejnou RESTful protokol, který umožňuje přehledné toouse od většiny klientů, včetně mobilních zařízení. Hello rozhraní API podporuje také rozšíření hello definované OData, přidání akcí, například dotaz jazyka toolet klientům přístup k datům užitečnější způsoby. (Další informace o protokolu OData, najdete v části [představení OData](http://download.microsoft.com/download/E/5/A/E5A59052-EE48-4D64-897B-5F7C608165B8/IntroducingOData.pdf).) Vzhledem k hello rozhraní Graph API můžou být použité toolearn o vztahy mezi uživateli, umožňuje aplikacím pochopit hello sociálních graf, který je součástí schéma hello Azure AD pro určitou organizací (která je důvod, proč se označuje jako hello rozhraní Graph API). A tooauthenticate samotné požádá o tooAzure AD pro rozhraní Graph API, aplikace používá OAuth 2.0.

Pokud organizace nepoužívá Windows Server Active Directory – nemá žádné místní servery nebo doménách - a spoléhá výhradně na cloudové aplikace, které používají Azure AD, pomocí právě tento adresář cloudu by přidělte hello firma uživatelé jednoho přihlášení tooall z nich. Ještě při tomto scénáři získá častější každý den, většina organizací stále použít místní domény vytvořené službou Windows Server Active Directory. Azure AD má užitečné role tooplay sem taky jako [obrázek 3](#fig3) zobrazuje.

![Azure Active Directory ve virtuálním počítači](./media/identity/identity_03_AD.png)
<a id="fig3"></a>obrázek 3: organizace může provést federaci Windows Server Active Directory s Azure Active Directory toogive svým aplikacím tooSaaS přihlašování uživatelů.

V tomto scénáři, které uživatel v organizaci B tooaccess aplikaci SaaS. Než to udělá, správci directory organizace hello potřeba vytvořit vztah federace s Azure AD pomocí služby AD FS, jak ukazuje obrázek hello. Tyto správce musíte také nakonfigurovat synchronizaci dat mezi organizace hello místní Windows Server AD a Azure AD. Zkopíruje automaticky uživatele a informace skupiny z hello místní adresář tooAzure AD. Všimněte si, co to umožňují: V důsledku toho je hello organizace rozšíření jeho místního adresáře do cloudu hello. Kombinování systému Windows Server AD a Azure AD tímto způsobem organizace hello adresářová služba, která můžete spravovat jako jednu entitu, přitom stále má nároky na pracovišti a v cloudu hello.

toouse Azure AD, hello první přihlášení uživatele domény služby Active Directory v místě tooher obvyklým (krok 1). Když se uživatel pokusí tooaccess hello SaaS aplikace (krok 2), výsledkem procesu federace hello Azure AD vydání jí token pro tuto aplikaci (krok 3). (Další informace o tom, jak funguje federační najdete v tématu [založené na deklaracích Identity pro Windows: technologie a scénáře](http://www.davidchappell.com/writing/white_papers/Claims-Based_Identity_for_Windows_v3.0--Chappell.docx).) Jako dříve, tento token obsahuje informace, které identifikují hello uživatele a je digitálně podepsané službou Azure AD. Tento token se pak odešlou toohello SaaS aplikace (krok 4), který ověří podpis hello token a používá jeho obsahem (krok 5). A je v předchozím scénáři hello hello SaaS aplikace můžete použít hello rozhraní Graph API toolearn více o uživateli, pokud nezbytné (krok 6).

Azure AD není v současné době o úplné nahrazení pro místní systém Windows Server AD. Jak již bylo uvedeno hello cloudového adresáře má mnohem jednodušší schéma a také chybí akcí, například zásady skupiny, hello možnost toostore informace o počítačích a podpora pro LDAP. (Ve skutečnosti počítače s Windows nesmí být nakonfigurované toolet, které uživatelé přihlašovat tooit nic ale Azure AD pomocí – to není podporováno.) Místo toho hello počátečních cílů Azure AD zahrnout, když necháte enterprise uživatelům přístup k aplikacím v cloudu hello bez zachování samostatné přihlášení a uvolnění místní správci directory ruční synchronizaci svého místního adresáře s Každá aplikace SaaS jejich organizace používá. V čase ale očekávejte této cloudové adresáře služby tooaddress širší rozsah scénáře.

## <a name="ac"></a>Pomocí řízení přístupu Azure Active Directory
Cloudové identity technologie lze použít toosolve různé problémy. Azure Active Directory můžete uživatelům v organizaci aplikace SaaS toomultiple přihlašování, třeba. Ale technologie identity v cloudu hello lze také jinými způsoby.

Předpokládejme například, že aplikace, které toolet svým uživatelům se přihlásit pomocí tokeny vydané několik *zprostředkovatelů identity (IdPs)*. Velké množství poskytovatelů identit různých existují ještě dnes, včetně Facebook, Google, Microsoft a ostatní uživatelé a aplikace často umožňují uživatelům přihlásit pomocí některého z těchto identit. Proč by měla aplikace nepokoušejte toomaintain vlastní seznam uživatelů a hesla při můžete místo toho spoléhají na identity, které již existují? Přijetí existující identity usnadňuje životnosti i pro uživatele, kteří mají jednu méně tooremember uživatelské jméno a heslo a hello lidé, kteří vytvářejí aplikace hello, který už nepotřebujete toomaintain vlastní seznam uživatelských jmen a hesel.

Ale při každé zprostředkovatele identity vydá nějaký druh tokenu, nejsou tyto tokeny standardní – každý IdP má svou vlastní formát. Kromě toho hello informace v těchto tokenů také není standardní. Aplikace, které si přeje tooaccept tokeny vystavený, Řekněme, Facebook, Google a Microsoft se potýkají s hello výzvy zápisu toohandle jedinečný kód, každý z těchto různých formátech.

Ale Proč to udělat? Proč není místo toho vytvořte prostředník, který může vytvořit jeden formát tokenu běžné reprezentací informací o identitě? Tento postup by aby jednodušší hello vývojářům vytvářet aplikace, protože nyní potřebují toohandle pouze jeden typ tokenu. Azure Active Directory, řízení přístupu k tomu přesně, poskytuje prostředník v cloudu hello pro práci s různými tokeny. [Obrázek 4](#fig4) ukazuje, jak to funguje

![Azure Active Directory ve virtuálním počítači](./media/identity/identity_04_IdentityProviders.png)
<a id="fig4"></a>obrázek 4: Azure Active Directory řízení přístupu usnadňuje aplikace tooaccept identity tokeny vydané zprostředkovatelé jinou identitu.

proces Hello začíná, když se uživatel pokusí aplikace hello tooaccess z prohlížeče. aplikace Hello přesměruje jeho tooan IdP podle svého výběru (a také důvěřuje aplikace hello). Jana ověřuje samu sebe toothis IdP, například zadáním uživatelského jména a hesla (krok 1), a vrátí hello IdP token, který obsahuje informace o jeho (krok 2).

Hello obrázku, řízení přístupu podporuje celou řadu různých IdPs založené na cloudu, včetně účtů vytvořených Google, Yahoo, Facebook, Microsoft (dřív se mu říkalo Windows Live ID) nebo kteréhokoli poskytovatele služeb, OpenID. Podporuje také identit, které jsou vytvořené pomocí Azure Active Directory a prostřednictvím federace se službou AD FS, Windows Server Active Directory. cílem Hello je toocover hello nejčastěji používaná identity dnes, zda jste vydaným touto IdPs v hello cloudu nebo místně.

Jakmile prohlížeče hello uživatele tokenu deklarací identity z jeho zvolený IdP, odešle tento token tooAccess ovládací prvek (krok 3). Řízení přístupu ověří hello token, a ujistěte se, že ho opravdu vydala tento IdP a potom vytvoří nový token podle toohello pravidla, která byla definována pro tuto aplikaci. Podobně jako Azure Active Directory řízení přístupu je víceklientské služby, ale jsou hello klientům aplikace místo organizace zákazníka. Každá aplikace můžete získat vlastní obor názvů, jak ukazuje obrázek hello a můžete definovat různá pravidla o autorizaci a další.

Tato pravidla umožňují definovat, jak by měla být tokeny od různých IdPs převede na token řízení přístupu správce každou aplikaci. Například pokud jiný IdPs používat různé typy pro zastoupení uživatelských jmen, řízení přístupu pravidla můžete převést všechny tyto do stejného typu uživatelské jméno. Řízení přístupu pak odešle tento nový token back toohello prohlížeč (krok 4), která ji odešle toohello aplikace (krok 5). Jakmile má token hello řízení přístupu, aplikace hello ověřuje, že tento token skutečně vydala řízení přístupu, a potom používá hello informace, které obsahuje (krok 6).

Během tohoto procesu může vypadat poněkud složité, ve skutečnosti umožňuje životnosti výrazně jednodušší pro hello creator aplikace hello. Místo zpracování různých tokeny obsahující různé informace, může aplikace hello akceptovat identity pocházející vystavený více poskytovatelů identity při přesto obdržíte jenom jeden token se seznámit informace. Navíc místo vyžadovat, že každý toobe aplikace nakonfigurovaná tootrust různé IdPs, tyto vztahy důvěryhodnosti se místo toho spravuje pomocí řízení přístupu – ho potřebovat důvěřovat pouze aplikace.

Je vhodné odkazující, že žádná data o řízení přístupu je vázané tooWindows – ji může použít stejně dobře aplikace Linux, který přijat pouze identity Google a sítě Facebook. A přestože řízení přístupu je součástí hello rodiny Azure Active Directory, můžete si ho představit jako zcela odlišným služba z co je popsáno v předchozí části hello. Při obou technologií pracovat s identitou, jejich adres výrazně lišit problémy (i když Microsoft má uvedli, že očekává toointegrate hello dva v určitém okamžiku).

Práce s identitou je důležité téměř každou aplikaci. cílem Hello řízení přístupu je toomake ho usnadňuje vývojářům toocreate aplikace, které přijímají identity od zprostředkovatelů různých identity. Vložením tato služba v cloudu hello Microsoft udělal, je k dispozici tooany aplikace běžící na jakékoli platformě.

## <a name="about-hello-author"></a>O hello Autor
David Chappell je hlavní Chappell z & partnerů [www.davidchappell.com](http://www.davidchappell.com) v San Franciscu, kalifornské.

