
<br>

> [!NOTE]
> Pokud správce vám byl přiřazen uživatelské jméno a heslo, je velmi pravděpodobné, že už máte svůj pracovní nebo školní ID (někdy také říká *ID organizace*). Pokud ano, můžete okamžitě začít toouse tooaccess prostředky Azure, které vyžadují jednu váš účet Azure. Pokud zjistíte, že tyto prostředky nelze použít, musíte tooreturn toothis článek o pomoc. Další informace najdete v tématu [účty, které můžete použít pro přihlášení](https://msdn.microsoft.com/library/azure/dn629581.aspx#BKMK_SignInAccounts) a [jak Azure předplatného je související tooAzure AD](https://msdn.microsoft.com/library/azure/dn629581.aspx#BKMK_SubRelationToDir).
> 
> 

Hello kroky jsou jednoduché. Je třeba toolocate vaše podepsaný na identitu ve hello portál Azure classic, zjišťovat vaše výchozí doménu služby Azure Active Directory a přidat nové uživatele tooit jako Azure společné správce.

## <a name="locate-your-default-directory-in-hello-azure-classic-portal"></a>Najít výchozí adresář v hello portál Azure classic
Začněte tím, že přihlášení toohello [portál Azure classic](https://manage.windowsazure.com) s vaši osobní identitu účtu Microsoft. Po přihlášení se posuňte se dolů hello blue panely hello levé straně a klikněte na tlačítko **služby ACTIVE DIRECTORY**.

![Azure Active Directory](./media/virtual-machines-common-create-aad-work-id/azureactivedirectorywidget.png)

Začněme tím hledání některé informace o vaší identity v Azure. Měli byste vidět něco podobného jako hello následující v hlavním podokně hello, zobrazující, že máte jeden výchozí adresář.

![](./media/virtual-machines-common-create-aad-work-id/defaultaadlisting.png)

Umožňuje zjistit některé další informace o něm. Klikněte na tlačítko hello výchozí adresář řádek, což vám přináší do hello výchozí adresář vlastnosti.  

![](./media/virtual-machines-common-create-aad-work-id/defaultdirectorypage.png)

tooview výchozí název domény hello, klikněte na tlačítko **domény**.

![](./media/virtual-machines-common-create-aad-work-id/domainclicktoseeyourdefaultdomain.png)

V tomto poli musí být schopný toosee při vytvoření hello účet Azure, Azure Active Directory vytvořit osobní výchozí doméně, která je hodnota hash (číslo vygenerovat z textového řetězce) pro své osobní ID použít jako subdoména onmicrosoft.com. To je toowhich domény hello nyní přidáte nového uživatele.

## <a name="creating-a-new-user-in-hello-default-domain"></a>Vytvoření nového uživatele v doméně výchozí hello
Klikněte na tlačítko **uživatelé** a vyhledejte vaší jeden osobní účet. Měli byste vidět v hello **ZDROJEM** sloupec, který je **účtu Microsoft**. Chceme toocreate uživatele ve výchozím. onmicrosoft.com domény Azure Active Directory.

![](./media/virtual-machines-common-create-aad-work-id/defaultdirectoryuserslisting.png)

Vytvoříme toofollow [tyto pokyny](https://technet.microsoft.com/library/hh967632.aspx#BKMK_1) v hello několika dalších krocích, ale použít konkrétní příklad.

V dolní části hello hello stránky, klikněte na tlačítko **+ přidat uživatele**. Hello stránce, který se zobrazí, zadejte hello nové uživatelské jméno a ujistěte se, hello **typ uživatele** **nového uživatele ve vaší organizaci**. V tomto příkladu hello nové uživatelské jméno je `ahmet`. Vyberte hello výchozí doménu, který je zjištěn dříve jako hello doménu pro ahmet na e-mailovou adresu. Klikněte na šipku další hello po dokončení.

![](./media/virtual-machines-common-create-aad-work-id/addingauserwithdirectorydropdown.png)

Přidat další podrobnosti pro Ahmet, ale ujistěte se, zda text hello tooselect odpovídající **ROLE** hodnotu. Je snadno toouse **globálního správce** pracují toomake zda věcí, ale pokud používáte roli nižší úrovně, které je vhodné. Tento příklad používá hello **uživatele** role. (Další informace v [oprávnění správce role](https://msdn.microsoft.com/library/azure/dn468213.aspx#BKMK_1).) Nepovolujte službu Multi-Factor authentication, pokud chcete, aby toouse vícefaktorového ověřování pro každý protokol v operaci. Až budete hotovi, klikněte na šipku další hello.

![](./media/virtual-machines-common-create-aad-work-id/userprofileuseradmin.png)

Klikněte na tlačítko hello **vytvořit** tlačítko toogenerate a zobrazení Ahmet dočasné heslo.

![](./media/virtual-machines-common-create-aad-work-id/gettemporarypasswordforuser.png)

Zkopírujte hello uživatelské jméno e-mailovou adresu, nebo použijte **odeslání HESLA v e-MAILU**. Hello informace toolog na budete potřebovat za chvíli.

![](./media/virtual-machines-common-create-aad-work-id/receivedtemporarypassworddialog.png)

Teď byste měli vidět hello nového uživatele, **Ahmet hello vývojáře**, z domácích zdrojů z Azure Active Directory. Hello nový pracovní nebo školní identity jste vytvořili v Azure Active Directory. Ale tuto identitu ještě nemá oprávnění toouse Azure prostředky.

![](./media/virtual-machines-common-create-aad-work-id/defaultdirectoryusersaftercreate.png)

Pokud používáte **odeslání HESLA v e-MAILU**, je odeslána hello následující druh e-mailu.

![](./media/virtual-machines-common-create-aad-work-id/emailreceivedfromnewusercreation.png)

## <a name="adding-azure-co-administrator-rights-for-subscriptions"></a>Přidání práv Azure spolusprávce pro předplatné
Teď musíte tooadd hello nového uživatele jako spolusprávce předplatného, hello nového uživatele můžete přihlásit toohello portálu pro správu. toodo, hello levém panelu klikněte na tlačítko **nastavení**.

![](./media/virtual-machines-common-create-aad-work-id/thesettingswidget.png)

V oblasti hello hlavní nastavení, klikněte na tlačítko **správci** v hello top a jste měli vidět jenom vaše osobní Microsoft identitu účtu. V dolní části hello hello stránky, klikněte na tlačítko **+ přidat** toospecify společné správce. Zde zadejte e-mailovou adresu hello hello nového uživatele, které jste vytvořili, včetně výchozí doménu. Jak ukazuje následující snímek obrazovky hello, se zobrazí zelená značka zaškrtnutí další uživatele toohello hello výchozí adresář. Mějte na paměti, tooselect všechny hello odběry, které chcete mít tooadminister tento uživatel toobe.

![](./media/virtual-machines-common-create-aad-work-id/addingnewuserascoadmin.png)

Až skončíte, teď byste měli vidět dvě uživatelů, včetně nové identity společné správce. Odhlaste se z portálu hello.

![](./media/virtual-machines-common-create-aad-work-id/newuseraddedascoadministrator.png)

## <a name="logging-in-and-changing-hello-new-users-password"></a>Protokolování a změna hello nové heslo uživatele
Přihlaste se jako hello nového uživatele, kterou jste vytvořili.

![](./media/virtual-machines-common-create-aad-work-id/signinginwithnewuser.png)

Budete mít okamžitě výzvami toocreate nové heslo.

![](./media/virtual-machines-common-create-aad-work-id/mustupdateyourpassword.png)

Jste měli údaje úspěch, který může vypadat následující hello.

![](./media/virtual-machines-common-create-aad-work-id/successtourdialog.png)

## <a name="next-steps"></a>Další kroky
Teď můžete použít vaše nové identity toouse Azure Active Directory [šablony skupiny prostředků Azure](../articles/xplat-cli-azure-resource-manager.md).

    azure login
    info:    Executing command login
    warn:    Please note that currently you can login only via Microsoft organizational account or service principal. For instructions on how tooset them up, please read http://aka.ms/Dhf67j.
    Username: ahmet@aztrainpassxxxxxoutlook.onmicrosoft.com
    Password: *********
    /info:    Added subscription Azure Pass
    info:    Setting subscription Azure Pass as default
    +
    info:    login command OK
    ralph@local:~$ azure config mode arm
    info:    New mode is arm
    ralph@local:~$ azure group list
    info:    Executing command group list
    + Listing resource groups
    info:    No matched resource groups were found
    info:    group list command OK
    ralph@local:~$ azure group create newgroup westus
    info:    Executing command group create
    + Getting resource group newgroup
    + Creating resource group newgroup
    info:    Created resource group newgroup
    data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/newgroup
    data:    Name:                newgroup
    data:    Location:            westus
    data:    Provisioning State:  Succeeded
    data:    Tags:
    data:
    info:    group create command OK
