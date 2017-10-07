---
title: "aaaPython Flask webové aplikace kurz pro Azure Cosmos DB | Microsoft Docs"
description: "Projděte si databázový kurz na používání Azure Cosmos DB toostore a přístup dat z webové aplikace Python Flask hostované v Azure. Naleznete zde řešení pro vývoj aplikací."
keywords: "Vývoj aplikací, python flask, webová aplikace python, vývoj pro web python"
services: cosmos-db
documentationcenter: python
author: mimig1
manager: jhubbard
editor: cgronlun
ms.assetid: 20ebec18-67c2-4988-a760-be7c30cfb745
ms.service: cosmos-db
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 08/09/2017
ms.author: mimig
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 87b73c656ed96a7efbd162843a1529d435f027f0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="build-a-python-flask-web-application-using-azure-cosmos-db"></a>Sestavení webové aplikace Python Flask využívající službu Azure Cosmos DB
> [!div class="op_single_selector"]
> * [.NET](documentdb-dotnet-application.md)
> * [Node.js](documentdb-nodejs-application.md)
> * [Java](documentdb-java-application.md)
> * [Python](documentdb-python-application.md)
> 
> 

Tento kurz ukazuje, jak data toostore a přístup k databázi Azure Cosmos toouse z Python webové aplikace hostované v Azure a předpokládá, že máte zkušenosti s používáním Pythonu a webů Azure.

Tento databázový kurz se zabývá:

1. Vytváření a zřizování účtu Cosmos DB.
2. Vytvoření aplikace Python Flask.
3. Připojení tooand pomocí Cosmos DB z webové aplikace.
4. Nasazení hello webové aplikace tooAzure.

Podle tohoto kurzu sestavíte jednoduchou hlasovací aplikaci, která vám umožní toovote v anketě.

![Snímek obrazovky hlasovací aplikace hello vytvořené v tomto kurzu databáze](./media/documentdb-python-application/cosmos-db-pythonr-run-application.png)

## <a name="database-tutorial-prerequisites"></a>Předpoklady pro databázový kurz
Před následující hello pokyny v tomto článku, se ujistěte, že máte nainstalované tyto položky hello:

* Aktivní účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).
 
    NEBO 

    Místní instalace hello [emulátoru DB Cosmos Azure](local-emulator.md).
* [Microsoft Visual Studio Community 2017](http://www.visualstudio.com/).  
* [Python Tools pro Visual Studio](https://github.com/Microsoft/PTVS/).  
* [Microsoft Azure SDK pro Python 2.7](https://azure.microsoft.com/downloads/). 
* [Python 2.7.13](https://www.python.org/downloads/windows/). 

> [!IMPORTANT]
> Pokud instalujete Python 2.7 pro hello poprvé, ujistěte se, že v úvodní obrazovka přizpůsobit Python 2.7.13, vyberete **přidat python.exe tooPath**.
> 
> ![Snímek obrazovky úvodní obrazovka přizpůsobit Python 2.7.11, kde je nutné tooselect přidat python.exe tooPath](./media/documentdb-python-application/cosmos-db-python-install.png)
> 
> 

* [Microsoft Visual C++ Compiler for Python 2.7](https://www.microsoft.com/en-us/download/details.aspx?id=44266).

## <a name="step-1-create-an-azure-cosmos-db-database-account"></a>Krok 1: Vytvoření účtu databáze Azure Cosmos DB
Začněme vytvořením účtu služby Cosmos DB. Pokud již máte účet, nebo pokud používáte hello emulátoru DB Cosmos Azure pro účely tohoto kurzu, můžete přeskočit příliš[krok 2: vytvoření nové webové aplikace Python Flask](#step-2-create-a-new-python-flask-web-application).

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

<br/>
Nyní vám ukážeme jak toocreate nové webové aplikace Python Flask z hello pozadí.

## <a name="step-2-create-a-new-python-flask-web-application"></a>Krok 2: Vytvoření nové webové aplikace Python Flask
1. V sadě Visual Studio na hello **soubor** nabídce bodu příliš**nový**a potom klikněte na **projektu**.
   
    Hello **nový projekt** zobrazí se dialogové okno.
2. V levém podokně hello rozbalte **šablony** a potom **Python**a potom klikněte na **webové**. 
3. Vyberte **webový projekt Flask** v prostředním podokně hello a potom v hello **název** zadejte **kurzu**a potom klikněte na **OK**. Mějte na paměti, že názvy balíčků Python by měl být celé malými písmeny, jak je popsáno v hello [průvodci správným stylem pro kód Python](https://www.python.org/dev/peps/pep-0008/#package-and-module-names).
   
    Tyto nové tooPython Flask je architektura vývoj webových aplikací, které urychluje tvorbu webových aplikací v Pythonu rychlejší.
   
    ![Snímek obrazovky znázorňující hello okno Nový projekt v sadě Visual Studio se zvýrazněným hello vlevo, vybraným hello uprostřed a názvem tutorial hello v poli Název hello Python Flask webovým projektem Python](./media/documentdb-python-application/image9.png)
4. V hello **Python Tools pro Visual Studio** okně klikněte na tlačítko **nainstalovat do virtuálního prostředí**. 
   
    ![Snímek obrazovky databázového kurzu hello – nástroje Python Tools pro Visual Studio – okno](./media/documentdb-python-application/python-install-virtual-environment.png)
5. V hello **Přidání virtuálního prostředí** okně můžete přijmout výchozí hodnoty hello a použít Python 2.7 jako základní prostředí hello, protože pydocumentdb v tuto aktuálně nepodporuje Python 3.x a pak klikněte na tlačítko **vytvořit**. Toto nastaví hello požadované virtuální prostředí Python pro váš projekt.
   
    ![Snímek obrazovky databázového kurzu hello – nástroje Python Tools pro Visual Studio – okno](./media/documentdb-python-application/image10_A.png)
   
    Zobrazí okno výstup Hello `Successfully installed Flask-0.10.1 Jinja2-2.8 MarkupSafe-0.23 Werkzeug-0.11.5 itsdangerous-0.24 'requirements.txt' was installed successfully.` při hello prostředí úspěšně nainstalováno.

## <a name="step-3-modify-hello-python-flask-web-application"></a>Krok 3: Úprava webové aplikace Python Flask hello
### <a name="add-hello-python-flask-packages-tooyour-project"></a>Přidání projektu tooyour balíčků Python Flask hello
Po nastavení projektu, budete potřebovat tooadd hello požadované balíčky tooyour projekt Flask, včetně pydocumentdb, tedy hello balíčku Python pro DocumentDB.

1. V Průzkumníku řešení otevřete soubor hello s názvem **requirements.txt** a nahraďte obsah hello hello následující:
   
        flask==0.9
        flask-mail==0.7.6
        sqlalchemy==0.7.9
        flask-sqlalchemy==0.16
        sqlalchemy-migrate==0.7.2
        flask-whooshalchemy==0.55a
        flask-wtf==0.8.4
        pytz==2013b
        flask-babel==0.8
        flup
        pydocumentdb>=1.0.0
2. Uložit hello **requirements.txt** souboru. 
3. V Průzkumníkovi řešení klikněte pravým tlačítkem na **env** a pak levým na **Nainstalovat z requirements.txt**.
   
    ![Snímek obrazovky znázorňující env (Python 2.7) vybraná při instalaci z requirements.txt zvýrazněných v seznamu hello](./media/documentdb-python-application/cosmos-db-python-install-from-requirements.png)
   
    Po úspěšné instalaci se zobrazí okno výstup hello hello následující:
   
        Successfully installed Babel-2.3.2 Tempita-0.5.2 WTForms-2.1 Whoosh-2.7.4 blinker-1.4 decorator-4.0.9 flask-0.9 flask-babel-0.8 flask-mail-0.7.6 flask-sqlalchemy-0.16 flask-whooshalchemy-0.55a0 flask-wtf-0.8.4 flup-1.0.2 pydocumentdb-1.6.1 pytz-2013b0 speaklater-1.3 sqlalchemy-0.7.9 sqlalchemy-migrate-0.7.2
   
   > [!NOTE]
   > Ve výjimečných případech může se zobrazit chyba v okně výstupu hello. Pokud k tomu dojde, zkontrolujte, jestli chyba hello je související toocleanup. Někdy hello čištění nezdaří, ale bude stále k úspěšné instalace hello (Posunout nahoru v tooverify okno výstup hello to). Instalaci můžete zkontrolovat [ověření hello virtuální prostředí](#verify-the-virtual-environment). Pokud hello instalace se nezdařila, ale je hello ověření úspěšné, je OK toocontinue.
   > 
   > 

### <a name="verify-hello-virtual-environment"></a>Ověřte hello virtuálního prostředí
Ujistěme se, že se vše správně nainstalovalo.

1. Vytvoření řešení hello stisknutím **Ctrl**+**Shift**+**B**.
2. Po úspěšném sestavení hello spustit hello webu stisknutím **F5**. Tím se spustí vývojový server Flask hello a spustí webový prohlížeč. Měli byste vidět následující stránku hello.
   
    ![Hello prázdný Python Flask webový vývojový projekt zobrazí v prohlížeči](./media/documentdb-python-application/image12.png)
3. Ukončete ladění webu hello stisknutím **Shift**+**F5** v sadě Visual Studio.

### <a name="create-database-collection-and-document-definitions"></a>Vytvoření definic databáze, kolekcí a dokumentů
Nyní vytvořte hlasovací aplikaci tak, že přidáme nové soubory a jiné aktualizujeme.

1. V Průzkumníku řešení klikněte pravým tlačítkem na hello **kurzu** projektu, klikněte na tlačítko **přidat**a potom klikněte na **novou položku**. Vyberte **prázdný soubor Python** a název souboru hello **forms.py**.  
2. Přidejte následující kód toohello forms.py soubor hello a pak hello soubor uložte.

```python
from flask.ext.wtf import Form
from wtforms import RadioField

class VoteForm(Form):
    deploy_preference  = RadioField('Deployment Preference', choices=[
        ('Web Site', 'Web Site'),
        ('Cloud Service', 'Cloud Service'),
        ('Virtual Machine', 'Virtual Machine')], default='Web Site')
```


### <a name="add-hello-required-imports-tooviewspy"></a>Přidat tooviews.py hello požadované importy
1. V Průzkumníku řešení rozbalte hello **kurzu** složku a otevřete hello **views.py** souboru. 
2. Přidejte následující import příkazy toohello horní části hello hello **views.py** uložte soubor hello. Tyto importovat Cosmos DB PythonSDK a hello balíčky Flask.
   
    ```python
    from forms import VoteForm
    import config
    import pydocumentdb.document_client as document_client
    ```

### <a name="create-database-collection-and-document"></a>Vytvoření databáze, kolekce a dokumentu
* Pořád ještě v **views.py**, přidejte následující kód toohello konec souboru hello hello. To má na starosti vytvoření databáze hello používá hello formuláře. Neodstraňujte žádný existující kód hello v **views.py**. Tomuto elementu end toohello jednoduše připojte.

```python
@app.route('/create')
def create():
    """Renders hello contact page."""
    client = document_client.DocumentClient(config.DOCUMENTDB_HOST, {'masterKey': config.DOCUMENTDB_KEY})

    # Attempt toodelete hello database.  This allows this toobe used toorecreate as well as create
    try:
        db = next((data for data in client.ReadDatabases() if data['id'] == config.DOCUMENTDB_DATABASE))
        client.DeleteDatabase(db['_self'])
    except:
        pass

    # Create database
    db = client.CreateDatabase({ 'id': config.DOCUMENTDB_DATABASE })

    # Create collection
    collection = client.CreateCollection(db['_self'],{ 'id': config.DOCUMENTDB_COLLECTION })

    # Create document
    document = client.CreateDocument(collection['_self'],
        { 'id': config.DOCUMENTDB_DOCUMENT,
          'Web Site': 0,
          'Cloud Service': 0,
          'Virtual Machine': 0,
          'name': config.DOCUMENTDB_DOCUMENT 
        })

    return render_template(
       'create.html',
        title='Create Page',
        year=datetime.now().year,
        message='You just created a new database, collection, and document.  Your old votes have been deleted')
```


### <a name="read-database-collection-document-and-submit-form"></a>Čtení databáze, kolekce a dokumentu a odeslání formuláře
* Pořád ještě v **views.py**, přidejte následující kód toohello konec souboru hello hello. To má na starosti nastavení formuláře hello a čtení hello databáze, kolekce a dokumentu. Neodstraňujte žádný existující kód hello v **views.py**. Tomuto elementu end toohello jednoduše připojte.

```python
@app.route('/vote', methods=['GET', 'POST'])
def vote(): 
    form = VoteForm()
    replaced_document ={}
    if form.validate_on_submit(): # is user submitted vote  
        client = document_client.DocumentClient(config.DOCUMENTDB_HOST, {'masterKey': config.DOCUMENTDB_KEY})

        # Read databases and take first since id should not be duplicated.
        db = next((data for data in client.ReadDatabases() if data['id'] == config.DOCUMENTDB_DATABASE))

        # Read collections and take first since id should not be duplicated.
        coll = next((coll for coll in client.ReadCollections(db['_self']) if coll['id'] == config.DOCUMENTDB_COLLECTION))

        # Read documents and take first since id should not be duplicated.
        doc = next((doc for doc in client.ReadDocuments(coll['_self']) if doc['id'] == config.DOCUMENTDB_DOCUMENT))

        # Take hello data from hello deploy_preference and increment our database
        doc[form.deploy_preference.data] = doc[form.deploy_preference.data] + 1
        replaced_document = client.ReplaceDocument(doc['_self'], doc)

        # Create a model toopass tooresults.html
        class VoteObject:
            choices = dict()
            total_votes = 0

        vote_object = VoteObject()
        vote_object.choices = {
            "Web Site" : doc['Web Site'],
            "Cloud Service" : doc['Cloud Service'],
            "Virtual Machine" : doc['Virtual Machine']
        }
        vote_object.total_votes = sum(vote_object.choices.values())

        return render_template(
            'results.html', 
            year=datetime.now().year, 
            vote_object = vote_object)

    else :
        return render_template(
            'vote.html', 
            title = 'Vote',
            year=datetime.now().year,
            form = form)
```


### <a name="create-hello-html-files"></a>Vytvoření souborů HTML hello
1. V Průzkumníku řešení v hello **kurzu** složky správné, klikněte na tlačítko hello **šablony** složku, klikněte na tlačítko **přidat**a potom klikněte na **novou položku**. 
2. Vyberte **stránku HTML**a pak do pole zadejte název hello **create.html**. 
3. Opakujte kroky 1 a 2 toocreate dva další soubory HTML: results.html a vote.html.
4. Přidejte následující kód příliš hello**create.html** v hello `<body>` elementu. Tento kód zobrazí zprávu, že jsme vytvořili novou databázi, kolekci a dokument.
   
    ```html
    {% extends "layout.html" %}
    {% block content %}
    <h2>{{ title }}.</h2>
    <h3>{{ message }}</h3>
    <p><a href="{{ url_for('vote') }}" class="btn btn-primary btn-large">Vote &raquo;</a></p>
    {% endblock %}
    ```
5. Přidejte následující kód příliš hello**results.html** v hello `<body`> elementu. Zobrazuje výsledky hello hello dotazování.
   
    ```html
    {% extends "layout.html" %}
    {% block content %}
    <h2>Results of hello vote</h2>
        <br />
   
    {% for choice in vote_object.choices %}
    <div class="row">
        <div class="col-sm-5">{{choice}}</div>
            <div class="col-sm-5">
                <div class="progress">
                    <div class="progress-bar" role="progressbar" aria-valuenow="{{vote_object.choices[choice]}}" aria-valuemin="0" aria-valuemax="{{vote_object.total_votes}}" style="width: {{(vote_object.choices[choice]/vote_object.total_votes)*100}}%;">
                                {{vote_object.choices[choice]}}
                </div>
            </div>
            </div>
    </div>
    {% endfor %}
   
    <br />
    <a class="btn btn-primary" href="{{ url_for('vote') }}">Vote again?</a>
    {% endblock %}
    ```
6. Přidejte následující kód příliš hello**vote.html** v hello `<body`> elementu. Zobrazí hello dotazování a přijímá hlasy hello. Při registraci hlasů hello, hello ovládací prvek předává přes tooviews.py kde jsme rozpozná hello udělené hlasy a připojí dokument hello odpovídajícím způsobem.
   
    ```html
    {% extends "layout.html" %}
    {% block content %}
    <h2>What is your favorite way toohost an application on Azure?</h2>
    <form action="" method="post" name="vote">
        {{form.hidden_tag()}}
            {{form.deploy_preference}}
            <button class="btn btn-primary" type="submit">Vote</button>
    </form>
    {% endblock %}
    ```
7. V hello **šablony** složky, nahraďte obsah hello **index.html** s následující hello. Tato stránka slouží jako hello úvodní stránka pro vaši aplikaci.
   
    ```html
    {% extends "layout.html" %}
    {% block content %}
    <h2>Python + Azure Cosmos DB Voting Application.</h2>
    <h3>This is a sample Cosmos DB voting application using PyDocumentDB</h3>
    <p><a href="{{ url_for('create') }}" class="btn btn-primary btn-large">Create/Clear hello Voting Database &raquo;</a></p>
    <p><a href="{{ url_for('vote') }}" class="btn btn-primary btn-large">Vote &raquo;</a></p>
    {% endblock %}
    ```

### <a name="add-a-configuration-file-and-change-hello-initpy"></a>Přidejte konfigurační soubor a změňte hello \_ \_init\_\_.py
1. V Průzkumníku řešení klikněte pravým tlačítkem na hello **kurzu** projektu, klikněte na tlačítko **přidat**, klikněte na tlačítko **nová položka**, vyberte **prázdný soubor Python**a potom Název souboru hello **config.py**. Formuláře ve Flasku vyžadují tento konfigurační soubor. Můžete ho tooprovide tajný klíč. Ten ale pro tento kurz není nezbytný.
2. Přidejte následující hello tooconfig.py kódu, budete potřebovat hodnoty hello tooalter **DOCUMENTDB\_hostitele** a **DOCUMENTDB\_klíč** v dalším kroku hello.
   
    ```python
    CSRF_ENABLED = True
    SECRET_KEY = 'you-will-never-guess'
   
    DOCUMENTDB_HOST = 'https://YOUR_DOCUMENTDB_NAME.documents.azure.com:443/'
    DOCUMENTDB_KEY = 'YOUR_SECRET_KEY_ENDING_IN_=='
   
    DOCUMENTDB_DATABASE = 'voting database'
    DOCUMENTDB_COLLECTION = 'voting collection'
    DOCUMENTDB_DOCUMENT = 'voting document'
    ```
3. V hello [portál Azure](https://portal.azure.com/), přejděte toohello **klíče** okno kliknutím **Procházet**, **Azure Cosmos DB účty**, klikněte dvakrát na název hello Dobrý den účtu toouse a pak klikněte na tlačítko hello **klíče** tlačítka na hello **Essentials** oblasti. V hello **klíče** okno, kopie hello **URI** a vložte ji do hello **config.py** souboru jako hodnota hello hello **DOCUMENTDB\_hostitele**  vlastnost. 
4. Zpět v hello portál Azure, v hello **klíče** okně kopie hello hodnotu hello **primární klíč** nebo hello **sekundární klíč**a vložte jej do hello **config.py**  souboru jako hodnota hello hello **DOCUMENTDB\_klíč** vlastnost.
5. V hello  **\_ \_init\_\_.py** soubor, přidejte následující řádek hello. 
   
        app.config.from_object('config')
   
    Tak, aby obsah hello hello souboru je:
   
    ```python
    from flask import Flask
    app = Flask(__name__)
    app.config.from_object('config')
    import tutorial.views
    ```
6. Po přidání všech souborů hello, Průzkumník řešení by měl vypadat takto:
   
    ![Snímek obrazovky okna Průzkumníka řešení Visual Studio hello](./media/documentdb-python-application/cosmos-db-python-solution-explorer.png)

## <a name="step-4-run-your-web-application-locally"></a>Krok 4: Místní spuštění webové aplikace
1. Vytvoření řešení hello stisknutím **Ctrl**+**Shift**+**B**.
2. Po úspěšném sestavení hello spustit hello webu stisknutím **F5**. Měli byste vidět následující hello na obrazovce.
   
    ![Snímek obrazovky znázorňující hello Python + Azure Cosmos DB hlasovací aplikace zobrazí ve webovém prohlížeči](./media/documentdb-python-application/cosmos-db-pythonr-run-application.png)
3. Klikněte na tlačítko **Create/Clear hello hlasování databáze** toogenerate hello databáze.
   
    ![Snímek obrazovky hello vytvořit stránku hello webové aplikace – podrobnosti o vývoji](./media/documentdb-python-application/cosmos-db-python-run-create-page.png)
4. Pak vyberte svou možnost kliknutím na **Vote** (Hlasovat).
   
    ![Snímek obrazovky aplikace hello webové aplikace se zobrazenou otázkou k hlasování](./media/documentdb-python-application/cosmos-db-vote.png)
5. Pro každý hlas, který udělíte navýší příslušný čítač hello.
   
    ![Snímek obrazovky hello výsledky zobrazené stránce hlasování hello](./media/documentdb-python-application/cosmos-db-voting-results.png)
6. Zastavte ladění projektu hello stisknutím kláves Shift + F5.

## <a name="step-5-deploy-hello-web-application-tooazure"></a>Krok 5: Nasazení hello webové aplikace tooAzure
Teď, když máte hello hotové aplikace správně pracovat se Cosmos DB, vytvoříme toodeploy tento tooAzure.

1. Klikněte pravým tlačítkem na projekt hello v Průzkumníkovi řešení (ujistěte se, že nejste spuštěná místně) a vyberte **publikovat**.  
   
     ![Snímek obrazovky hello tutorial vybraného v Průzkumníkovi řešení se zvýrazněnou možností publikovat hello](./media/documentdb-python-application/image20.png)
2. V hello **publikovat** dialogové okno, vyberte **Microsoft Azure App Service**, vyberte **vytvořit nový**a potom klikněte na **publikovat**.
   
    ![Snímek obrazovky okna publikování webu hello se zvýrazněnou službou Microsoft Azure App Service](./media/documentdb-python-application/cosmos-db-python-publish.png)
3. V hello **vytvořit službu App Service** dialogovém okně zadejte název hello pro vaši webovou aplikaci spolu s vaší **předplatné**, **skupiny prostředků**, a **plán služby App Service** , pak klikněte na tlačítko **vytvořit**.
   
    ![Snímek obrazovky okna okno Microsoft Azure webové aplikace hello](./media/documentdb-python-application/cosmos-db-python-create-app-service.png)
4. Během pár sekund bude Visual Studio dokončí publikování app service a spustí prohlížeč, kde uvidíte vaše handiwork běžící v Azure!

    ![Snímek obrazovky okna okno Microsoft Azure webové aplikace hello](./media/documentdb-python-application/cosmos-db-python-appservice-created.png)

## <a name="troubleshooting"></a>Řešení potíží
Pokud je to první aplikace Python hello jste spustili na počítači, ujistěte se, že hello následující složky (nebo hello ekvivalentní umístění instalací) jsou součástí vaše proměnná PATH:

    C:\Python27\site-packages;C:\Python27\;C:\Python27\Scripts;

Pokud narazíte na chyby na stránce hlasování a jste projekt pojmenovali jinak než **kurzu**, ujistěte se, že  **\_ \_init\_\_.py** odkazy na hello název správný projekt v řádku hello: `import tutorial.view`.

## <a name="next-steps"></a>Další kroky
Blahopřejeme! Právě dokončení svou první webovou aplikaci Python, pomocí Cosmos DB a publikovali jste ji tooAzure.

Toto téma často aktualizujeme a vylepšujeme podle vaší zpětné vazby.  Jednou jste dokončili kurz hello, použijte prosím hlasovací tlačítka v hello horní a dolní části této stránky hello a být jisti tooinclude svůj názor, jaká vylepšení chcete toosee provedeny. Pokud byste nám chtěli toocontact přímo, cítíte volné tooinclude e-mailovou adresou v komentářích.

tooadd další funkce tooyour webovou aplikaci, zkontrolujte hello rozhraní API dostupná v hello [Azure Cosmos DB Python SDK](documentdb-sdk-python.md).

Další informace o Azure, Visual Studio a Pythonu najdete v tématu hello [středisku pro vývojáře Python](https://azure.microsoft.com/develop/python/). 

Další kurzy Pythonu Flask najdete v části [hello Flask velký kurz, část I: Hello, World!](http://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-i-hello-world). 

[Visual Studio Express]: http://www.visualstudio.com/products/visual-studio-express-vs.aspx
[2]: https://www.python.org/downloads/windows/
[3]: https://www.microsoft.com/download/details.aspx?id=44266
[Microsoft Web Platform Installer]: http://www.microsoft.com/web/downloads/platform.aspx
[Azure portal]: http://portal.azure.com
