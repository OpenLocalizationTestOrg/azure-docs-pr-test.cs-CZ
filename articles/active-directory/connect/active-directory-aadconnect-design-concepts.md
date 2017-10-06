---
title: "Azure AD Connect: Koncepty návrhu | Microsoft Docs"
description: "Toto téma podrobnosti určité oblasti návrhu implementace"
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 4114a6c0-f96a-493c-be74-1153666ce6c9
ms.service: active-directory
ms.custom: azure-ad-connect
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Identity
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 1e5d5c6a716ca653fb14fc059e8155124b433732
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-design-concepts"></a>Azure AD Connect: Koncepty návrhu
účelem Hello tohoto tématu je toodescribe oblasti, které musí považovat při návrhu implementace hello služby Azure AD Connect. Toto téma se podrobné informace o určité oblasti a tyto koncepty stručně jsou popsány v i další témata.

## <a name="sourceanchor"></a>sourceAnchor
atribut sourceAnchor Hello je definován jako *atribut neměnné během hello životnost objektu*. Jednoznačně identifikuje objektů jako hello stejný objekt místně a v Azure AD. Hello atribut taky nazývá **immutableId** a hello dva názvy jsou použity zaměnitelné.

neměnné, Hello slovo, které je "nelze změnit", je důležité toothis tématu. Vzhledem k tomu, že tento atribut hodnotu nelze změnit, jakmile byla nastavena, je důležité toopick návrh, který podporuje váš scénář.

Hello atribut se používá pro hello následující scénáře:

* Pokud nový synchronizační server modul je vytvořen, nebo znovu sestavit po na scénář zotavení po havárii, tento atribut propojí existující objekty ve službě Azure AD s objekty na místě.
* Pokud přesunete z modelu synchronizované identity tooa identity jenom pro cloud a potom tento atribut umožňuje objekty příliš "pevné shodu" existující objekty ve službě Azure AD s místními objekty.
* Pokud používáte federace a potom tento atribut společně s hello **userPrincipalName** se používá v hello deklarace identity toouniquely identifikace uživatele.

Toto téma sourceAnchor pouze zmíněn, protože se týká toousers. Hello stejná pravidla použít tooall typy objektů, ale je určena pouze pro uživatele, že tento problém obvykle není vážný.

### <a name="selecting-a-good-sourceanchor-attribute"></a>Výběr atribut sourceAnchor funkční
Hodnota atributu Hello postupujte hello následující pravidla:

* Být menší než 60 znaků.
  * Kódování a počítají jako znaky 3 znaky nebude a – z, A-Z nebo 0-9
* Neobsahuje zvláštní znak: &#92;! # $ % & * + / = ? ^ &#96; { } | ~ < > ( ) ' ; : , [ ] " @ _
* Musí být globálně jedinečný
* Musí být řetězec, celé číslo nebo binárního souboru
* By neměl být založené na jméno uživatele, tyto změny
* By neměl být malá a velká písmena a vyhnout se hodnoty, které se mohou lišit podle případu
* By měla být přiřazená, když je vytvořen objekt hello

Pokud hello vybrali sourceAnchor není typu String, a zobrazí Azure AD Connect Base64Encode hello atribut hodnota tooensure žádné speciální znaky. Pokud používáte jiné federační server než služba AD FS, ujistěte se, se váš server můžete také Base64Encode hello atribut.

atribut sourceAnchor Hello je malá a velká písmena. Hodnota "JohnDoe" není hello stejné jako "johndoe". Ale by neměl mít dva různé objekty s pouze rozdíly v případě.

Pokud máte jednu doménovou strukturu je na místě, pak hello atribut byste měli používat **objectGUID**. Toto je také atribut hello použitý při použít Expresní nastavení ve službě Azure AD Connect a také hello atribut používaný DirSync.

Pokud máte více doménových struktur a nepřesouvejte uživatelů mezi doménové struktury a domény, pak **objectGUID** je dobrý atribut toouse i v tomto případě.

Pokud přesunete uživatele mezi doménové struktury a domény, pak musíte vyhledat atribut, který se nemění, nebo lze přesunout s uživateli hello během přesunu hello. Doporučený postup je toointroduce syntetické atribut. Atribut, který může obsahovat něco který vypadá jako identifikátor GUID bude vhodné. Při vytváření objektu nový identifikátor GUID je vytvořen a razítkem hello uživatele. Vlastní synchronizační pravidlo lze vytvořit v hello synchronizační modul serveru toocreate tuto hodnotu podle hello **objectGUID** a aktualizace hello vybraný atribut v přidá. Při přesunutí objektu hello, ujistěte se, že tooalso kopie hello obsah této hodnoty.

Jiné řešení je toopick existující atribut, které znáte se nemění. Běžně používané atributy patří **employeeID**. Pokud uvažujete o atribut, který obsahuje písmena, ujistěte se, že žádné prvního hello případ (velká a malá písmena) můžete změnit pro hodnotu atributu hello se. Chybný atributy, které by se neměla používat zahrnují tyto atributy s názvem hello hello uživatele. V manželství nebo rozvodu je název hello očekávané toochange, což není povoleno pro tento atribut. Toto je také jeden důvod proč atributy, jako **userPrincipalName**, **e-mailu**, a **targetAddress** nejsou tooselect dokonce v instalaci hello Azure AD Connect Průvodce. Tyto atributy také obsahovat hello "@" znak, který není povolen v hello sourceAnchor.

### <a name="changing-hello-sourceanchor-attribute"></a>Změna atribut sourceAnchor hello
Hodnota atributu sourceAnchor Hello nelze změnit po vytvoření objektu hello ve službě Azure AD a hello identita je synchronizována.

Z tohoto důvodu platí následující omezení hello tooAzure AD Connect:

* atribut sourceAnchor Hello lze nastavit pouze během počáteční instalace. Pokud Průvodce instalací hello, tato možnost je jen pro čtení. Pokud potřebujete toochange toto nastavení, je nutné odinstalovat a znovu nainstalovat.
* Pokud instalujete další server Azure AD Connect a pak musí vyberte hello stejný atribut sourceAnchor jako použil. Pokud jste dříve pomocí nástroje DirSync a přesunout tooAzure AD připojit, pak musíte použít **objectGUID** vzhledem k tomu, který je atribut hello používá DirSync.
* Pokud dojde ke změně hello hodnotu pro sourceAnchor po exportovaný tooAzure AD, pak Azure AD Connect sync vyvolá chybu a nepovoluje žádné další změny na tento objekt před hello problém byl opraven a zpět v hello se mění hello sourceAnchor hello objektu Zdrojový adresář.

## <a name="using-msds-consistencyguid-as-sourceanchor"></a>Použití msDS-ConsistencyGuid jako sourceAnchor
Ve výchozím nastavení, Azure AD Connect (verze 1.1.486.0 a starší) jako atribut sourceAnchor hello používá objectGUID. Identifikátoru GUID se generuje systémem. Nelze zadat jeho hodnotu, při vytváření místní objekty služby AD. Jak je popsáno v části [sourceAnchor](#sourceanchor), existují scénáře, kde je nutné toospecify hello sourceAnchor hodnotu. Pokud hello scénáře tooyou použít, musíte použít atribut konfigurovat AD (například msDS-ConsistencyGuid) jako atribut sourceAnchor hello.

Azure AD Connect (verze 1.1.524.0 a po) teď usnadňuje hello použití msDS-ConsistencyGuid jako atribut sourceAnchor. Při použití této funkce, Azure AD Connect automaticky nakonfiguruje hello synchronizační pravidla pro:

1. Použijte msDS-ConsistencyGuid jako atribut sourceAnchor hello pro uživatelské objekty. ObjectGUID se používá pro jiné typy objektů.

2. Pro všechny zadané místní uživatele AD objekt, jehož atribut msDS-ConsistencyGuid není vyplněný, Azure AD Connect zápisů jeho atribut msDS-ConsistencyGuid back toohello hodnota objectGUID v místní službě Active Directory. Po naplnění atribut msDS-ConsistencyGuid hello Azure AD Connect pak exportuje hello objekt tooAzure AD.

>[!NOTE]
> Jednou místní AD objektu je importovat do Azure AD Connect (která je, importována do hello prostoru konektoru AD a promítnout do hello Metaverse), jeho sourceAnchor hodnotu nelze změnit, již. Hodnota sourceAnchor hello toospecify danou místní AD objektu, konfigurovat jeho atribut msDS-ConsistencyGuid před jeho importu do Azure AD Connect.

### <a name="permission-required"></a>Potřebná oprávnění
Pro tuto funkci toowork musejí mít hello AD DS účet používaný toosynchronize s místní služby Active Directory oprávnění zápisu oprávnění toohello msDS-ConsistencyGuid atribut v místní službě Active Directory.

### <a name="how-tooenable-hello-consistencyguid-feature---new-installation"></a>Jak tooenable hello ConsistencyGuid funkce – nové instalace
Při nové instalaci můžete povolit použití hello ConsistencyGuid jako sourceAnchor. Tato část obsahuje Express a vlastní instalace v podrobnostech.

  >[!NOTE]
  > Pouze novější verze služby Azure AD Connect (1.1.524.0 a po) podporuje hello použití ConsistencyGuid jako sourceAnchor při nové instalaci.

### <a name="how-tooenable-hello-consistencyguid-feature"></a>Jak tooenable hello ConsistencyGuid funkce
V současné době hello funkce jde Povolit jenom během pouze nové instalace Azure AD Connect.

#### <a name="express-installation"></a>Expresní instalace
Při instalaci Azure AD Connect s režimem Express, Průvodce Azure AD Connect hello automaticky určuje atribut toouse hello nejvhodnější AD jako atribut sourceAnchor hello pomocí hello logiku následující:

* Nejprve hello Azure AD Connect Průvodce dotazy, které jako použit atribut hello AD tooretrieve klienta vaší služby Azure AD hello atribut sourceAnchor v instalaci hello předchozí Azure AD Connect (pokud existuje). Pokud tyto informace jsou k dispozici, Azure AD Connect používá atribut hello stejnou AD.

  >[!NOTE]
  > Pouze novější verze služby Azure AD Connect (1.1.524.0 a po) jsou uloženy informace o atribut sourceAnchor hello v klientovi služby Azure AD použít během instalace. Starší verze služby Azure AD Connect nepodporují.

* Pokud není k dispozici informace o atribut sourceAnchor hello používá, hello Průvodce zkontroluje stav hello atributu msDS-ConsistencyGuid hello ve vaší místní službě Active Directory. Pokud atribut hello není nakonfigurovaný na všech objektů v adresáři hello, použije průvodce hello hello msDS-ConsistencyGuid jako atribut sourceAnchor hello. Pokud atribut hello je nakonfigurovaný na jeden nebo více objektů v adresáři hello, ukončí hello průvodce atribut hello používá jiné aplikace a není vhodný jako atribut sourceAnchor...

* V takovém případě hello Průvodce spadne zpět toousing objectGUID jako atribut sourceAnchor hello.

* Jakmile hello atribut sourceAnchor je určeno, Průvodce hello ukládá informace hello v klientovi služby Azure AD. Hello informace se použijí při budoucích instalaci Azure AD Connect.

Po dokončení expresní instalace hello Průvodce vás upozorní, které atribut vydané jako atribut hello zdrojové ukotvení.

![Průvodce informuje AD atribut vydat pro sourceAnchor](./media/active-directory-aadconnect-design-concepts/consistencyGuid-01.png)

#### <a name="custom-installation"></a>Vlastní instalace
Při instalaci Azure AD Connect s režimem vlastní, Průvodce Azure AD Connect hello nabízí dvě možnosti při konfiguraci atribut sourceAnchor:

![Vlastní instalace – sourceAnchor konfigurace](./media/active-directory-aadconnect-design-concepts/consistencyGuid-02.png)

| Nastavení | Popis |
| --- | --- |
| Aby Azure spravovat hello zdrojové ukotvení | Tuto možnost vyberte, pokud chcete, aby pro vás Azure AD toopick hello atribut. Pokud vyberete tuto možnost, použije průvodce Azure AD Connect hello stejné [logiku výběr atribut sourceAnchor použít během instalace Express](#express-installation). Podobně jako tooExpress instalace hello Průvodce vás upozorní, které atribut vydané jako hello atributu zdrojové ukotvení po dokončení instalace vlastní. |
| Konkrétní atribut | Tuto možnost vyberte, pokud chcete toospecify existující atribut AD jako atribut sourceAnchor hello. |

### <a name="how-tooenable-hello-consistencyguid-feature---existing-deployment"></a>Jak tooenable hello funkce ConsistencyGuid - stávajícího nasazení
Pokud máte existující nasazení Azure AD Connect, který používá jako hello zdrojové ukotvení atribut objectGUID, můžete přepnout tak toousing ConsistencyGuid místo.

>[!NOTE]
> Pouze novější verze služby Azure AD Connect (1.1.552.0 a po) podporuje přepínání z ObjectGuid tooConsistencyGuid jako atribut hello zdrojové ukotvení.

tooswitch z tooConsistencyGuid objectGUID jako atribut zdrojové ukotvení hello:

1. Spusťte Průvodce hello Azure AD Connect a klikněte na **konfigurace** toogo toohello úlohy obrazovky.

2. Vyberte hello **konfigurace zdrojové ukotvení** úloh možnost a klikněte na tlačítko **Další**.

   ![Povolit ConsistencyGuid pro existující nasazení – krok 2](./media/active-directory-aadconnect-design-concepts/consistencyguidexistingdeployment01.png)

3. Zadejte své přihlašovací údaje správce Azure AD a klikněte na tlačítko **Další**.

4. Průvodce Azure AD Connect analyzuje stav hello atributu msDS-ConsistencyGuid hello ve vaší místní službě Active Directory. Pokud atribut hello není nakonfigurovaný na všech objektu v adresáři, Azure AD Connect k závěru, že žádná jiná aplikace právě používá hello atribut a je bezpečné toouse hello jej jako atribut zdrojové ukotvení hello. Klikněte na tlačítko **Další** toocontinue.

   ![Povolit ConsistencyGuid pro existující nasazení – krok 4](./media/active-directory-aadconnect-design-concepts/consistencyguidexistingdeployment02.png)

5. V hello **připraven tooConfigure** obrazovky, klikněte na tlačítko **konfigurace** změna konfigurace toomake hello.

   ![Povolit ConsistencyGuid pro existující nasazení – krok 5](./media/active-directory-aadconnect-design-concepts/consistencyguidexistingdeployment03.png)

6. Po dokončení konfigurace hello hello Průvodce označuje, že msDS-ConsistencyGuid je nyní používán jako atribut hello zdrojové ukotvení.

   ![Povolit ConsistencyGuid pro existující nasazení – krok 6](./media/active-directory-aadconnect-design-concepts/consistencyguidexistingdeployment04.png)

Během analýzy hello (krok 4) Pokud je na jeden nebo více objektů v adresáři hello nakonfigurovaná hello atribut hello průvodce ukončí atribut hello používá jiná aplikace a vrátí chybu, jak je znázorněno v následujícím diagramu hello. Pokud jste si jisti, že tento atribut hello nepoužívá existující aplikace, musíte toocontact podporu pro informace o tom, jak toosuppress hello chyby.

![Povolit ConsistencyGuid pro existující nasazení – chyba](./media/active-directory-aadconnect-design-concepts/consistencyguidexistingdeploymenterror.png)

### <a name="impact-on-ad-fs-or-third-party-federation-configuration"></a>Dopad na služby AD FS nebo konfigurace federování třetích stran
Pokud používáte Azure AD Connect toomanage místní nasazení služby AD FS, hello Azure AD Connect automaticky aktualizuje hello hello stejnou AD atribut toouse pravidla deklarace identity jako sourceAnchor. Tím se zajistí, že deklarace identity ImmutableID hello generované služby AD FS je v souladu s hello sourceAnchor hodnoty exportovaný tooAzure AD.

Pokud spravujete služby AD FS mimo Azure AD Connect nebo třetích stran federační servery používají pro ověřování, je nutné ručně aktualizovat hello pravidla deklarace identity pro deklarace identity toobe konzistentní s hodnotami sourceAnchor hello exportovat tooAzure AD jako ImmutableID popsané v části článku [pravidla deklarací identity upravit AD FS](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-federation-management#modclaims). Průvodce Hello vrátí hello po dokončení instalace následující upozornění:

![Konfigurace federace třetích stran](./media/active-directory-aadconnect-design-concepts/consistencyGuid-03.png)

### <a name="adding-new-directories-tooexisting-deployment"></a>Přidání nového nasazení tooexisting adresáře
Předpokládejme, že jste nasadili Azure AD Connect s povolenou funkcí ConsistencyGuid hello a teď chcete tooadd další adresáře toohello nasazení. Při pokusu o tooadd hello adresáře, Průvodce Azure AD Connect kontroluje stav hello atributu mSDS-ConsistencyGuid hello v adresáři hello. Pokud atribut hello je nakonfigurovaný na jeden nebo více objektů v adresáři hello, ukončí hello průvodce atribut hello používá jiné aplikace a vrátí chybu, jak je znázorněno v následujícím diagramu hello. Pokud jste si jisti, že tento atribut hello nepoužívá existující aplikace, musíte toocontact podporu pro informace o tom, jak toosuppress hello chyby.

![Přidání nového nasazení tooexisting adresáře](./media/active-directory-aadconnect-design-concepts/consistencyGuid-04.png)

## <a name="azure-ad-sign-in"></a>Azure AD přihlášení
Při integraci místního adresáře do Azure AD, je důležité toounderstand vliv hello způsob uživatelské nastavení synchronizace hello ověřuje. Azure AD používá uživatele hello tooauthenticate userPrincipalName (UPN). Při synchronizaci vaši uživatelé však musíte zvolit toobe atribut hello používá pro hodnotu userPrincipalName pečlivě.

### <a name="choosing-hello-attribute-for-userprincipalname"></a>Výběr hello atribut userPrincipalName
Při výběru pro poskytnutí hello hodnotu UPN toobe používá v Azure jeden atribut hello zajistil

* hodnoty atributů Hello odpovídat toohello UPN syntaxi (RFC 822), musí být ve formátu hellousername@domain
* přípona Hello v hello hodnoty odpovídá tooone Dobrý den ověření vlastní domény ve službě Azure AD

V Expresní nastavení hello předpokládá, že je volba pro hello atribut userPrincipalName. Pokud atribut userPrincipalName hello neobsahuje hodnotu hello chcete, aby vaši uživatelé toosign v tooAzure, pak je nutné vybrat **vlastní instalace**.

### <a name="custom-domain-state-and-upn"></a>Stav vlastní domény a UPN
Je důležité tooensure se ověřené domény pro příponu UPN hello.

Jan je uživatel v doméně contoso.com. Chcete, aby Jan toouse hello místní UPN john@contoso.com toosign v tooAzure po vám tak synchronizaci uživatelů tooyour Azure AD directory contoso.onmicrosoft.com. toodo, potřebujete tooadd a ověřte contoso.com jako vlastní doménu, ve službě Azure AD, před zahájením synchronizuje se uživatelé hello. Pokud přípona UPN hello John, například contoso.com, neodpovídá ověřenou doménu ve službě Azure AD, Azure AD nahrazuje příponu UPN hello s contoso.onmicrosoft.com.

### <a name="non-routable-on-premises-domains-and-upn-for-azure-ad"></a>Směrovatelný bez místní domény a UPN pro Azure AD
Některé organizace mají směrovat domény, jako je contoso.local nebo jednoduchý přípony domény jako contoso. Nejste tooverify možné směrovat domény ve službě Azure AD. Azure AD Connect pro synchronizaci tooonly ověřenou doménu ve službě Azure AD. Když vytvoříte adresář služby Azure AD, vytvoří se směrovatelné domény, který se stane výchozí doménu pro vaši službu Azure AD například contoso.onmicrosoft.com. Proto bude nutné tooverify všech ostatních směrovatelné domén v takové situaci v případě, že nechcete, aby toosync toohello výchozí domény onmicrosoft.com.

Čtení [přidat název tooAzure vaši vlastní doménu služby Active Directory](../active-directory-add-domain.md) Další informace o přidání a ověření domény.

Azure AD Connect zjistí, pokud běží v prostředí domény směrovat a bude odpovídajícím způsobem upozornit z pokračovat Expresní nastavení. Pokud pracujete v směrovat domény, pak je pravděpodobné že hello UPN uživatelů hello máte příliš směrovat přípony. Například pokud používáte v contoso.local, Azure AD Connect navrhuje jste toouse vlastní nastavení místo použitím expresního nastavení. Pomocí vlastních nastavení, budete moct toospecify hello atribut, který má být použit jako toosign UPN v tooAzure po hello uživatelé jsou synchronizovaná tooAzure AD.

## <a name="next-steps"></a>Další kroky
Přečtěte si další informace o [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md).
