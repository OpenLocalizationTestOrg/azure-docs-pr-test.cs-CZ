Vytvořit přihlašovací údaje nasazení s hello [nastavený uživatel nasazení webapp az](/cli/azure/webapp/deployment/user#set) příkaz.

Uživatel nasazení je požadovaná pro FTP a místní Git nasazení tooa webové aplikace. Hello uživatelské jméno a heslo jsou na úrovni účtu. _To znamená, že se liší od přihlašovacích údajů předplatného Azure._

Následující příkaz a nahraďte v hello  *\<uživatelské jméno >* a  *\<heslo >* s nové uživatelské jméno a heslo. Hello uživatelské jméno musí být jedinečný. Hello heslo musí obsahovat alespoň osm znaků, se dvěma hello následující tři prvky: písmena, symboly a čísla. 

```azurecli-interactive
az webapp deployment user set --user-name <username> --password <password>
```

Pokud dojde ` 'Conflict'. Details: 409` chyby, uživatelské jméno změnit hello. Pokud se zobrazí chyba ` 'Bad Request'. Details: 400`, použijte silnější heslo.

Tohoto uživatele nasazení vytvoříte jenom jednou a potom ho můžete použít pro všechna nasazení v Azure.

> [!NOTE]
> Záznam hello uživatelské jméno a heslo. Budete je potřebovat toodeploy hello webovou aplikaci později.
>
>