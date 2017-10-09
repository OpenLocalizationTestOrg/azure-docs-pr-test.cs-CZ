Azure určí, že vaše aplikace používá Python, **pokud jsou splněné obě tyto podmínky**:

* soubor Requirements.txt v kořenové složce hello
* jakýkoli soubor .py v kořenové složce hello nebo soubor runtime.txt, který určuje jazyk python

Když, je případ hello, použije skript konkrétní nasazení, Python, který provede standardní synchronizaci hello soubory, jakož i další operace jazyka Python, jako:

* Automatická správa virtuálního prostředí
* Instalace balíčků uvedených v souboru requirements.txt pomocí nástroje pip
* Vytvoření hello odpovídající souboru web.config podle hello vybraná verze Python.
* Shromáždění statických souborů pro aplikace Django

Můžete řídit některé aspekty kroky nasazení výchozí hello bez nutnosti toocustomize hello skriptu.

Pokud chcete tooskip všechny kroky nasazení specifické pro Python, můžete vytvořit tento prázdný soubor:

    \.skipPythonDeployment

Pokud chcete u aplikace rozhraní Django tooskip shromažďování statických souborů:

    \.skipDjango 

Pro větší kontrolu nad nasazením můžete přepsat výchozí skript nasazení hello vytvořením hello následující soubory:

    \.deployment
    \deploy.cmd

Můžete použít hello [rozhraní příkazového řádku Azure] [ Azure command-line interface] toocreate hello soubory.  Použijte tento příkaz ze složky projektu:

    azure site deploymentscript --python

Pokud tyto soubory neexistují, Azure vytvoří dočasný skript nasazení a spustí ho.  Je identické toohello jeden vytvoříte příkazem hello výše.

[Azure command-line interface]: http://azure.microsoft.com/downloads/
