---
title: "kurz vývoje aplikace aaaJava pomocí Azure Cosmos DB | Microsoft Docs"
description: "Tento kurz webové aplikace Java ukazuje, jak toouse hello Azure Cosmos DB a hello toostore DocumentDB rozhraní API a přístup k datům z aplikace Java hostované na webech Azure."
keywords: "Vývoj aplikací, databázový kurz, aplikace v jazyce java, kurz vývoje webových aplikací v jazyce java, documentdb, azure, Microsoft azure"
services: cosmos-db
documentationcenter: java
author: dennyglee
manager: jhubbard
editor: mimig
ms.assetid: 0867a4a2-4bf5-4898-a1f4-44e3868f8725
ms.service: cosmos-db
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 08/22/2017
ms.author: denlee
ms.openlocfilehash: e073de23beb0037ee1e37b48a69e8fe7cdc3fc1d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="build-a-java-web-application-using-azure-cosmos-db-and-hello-documentdb-api"></a>Vytvoření webové aplikace Java pomocí Azure Cosmos DB a hello DocumentDB rozhraní API
> [!div class="op_single_selector"]
> * [.NET](documentdb-dotnet-application.md)
> * [Node.js](documentdb-nodejs-application.md)
> * [Java](documentdb-java-application.md)
> * [Python](documentdb-python-application.md)
> 
> 

Tento kurz webové aplikace Java ukazuje, jak toouse hello [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) služby toostore a přístupová data z aplikace Java hostované v Azure App Service Web Apps. V tomto tématu se naučíte:

* Jak toobuild základní aplikaci JavaServer stránky (JSP) v prostředí Eclipse.
* Jak toowork s hello Azure Cosmos DB služby pomocí hello [Azure Cosmos DB Java SDK](https://github.com/Azure/azure-documentdb-java).

Tento kurz aplikace Java ukazuje, jak úloh toocreate webovou aplikaci pro správu úkolů umožňující toocreate můžete načíst a označit jako dokončené, jak ukazuje následující obrázek hello. Jednotlivé úlohy hello v seznamu úkolů hello jsou ukládány jako dokumenty JSON v Azure Cosmos DB.

![Aplikace pro seznam úkolů v jazyce Java](./media/documentdb-java-application/image1.png)

> [!TIP]
> V tomto kurzu vývoje aplikace se předpokládá, že již máte zkušenosti s jazykem Java. Pokud jste nový tooJava nebo hello [požadovaných nástrojů](#Prerequisites), doporučujeme stáhnout úplný hello [todo](https://github.com/Azure-Samples/documentdb-java-todo-app) projektu z Githubu a postupovat podle [hello pokyny na konci hello článek](#GetProject). Po jejím vytvořené, můžete zkontrolovat hello článku toogain porozuměli hello kódu v kontextu hello hello projektu.  
> 
> 

## <a id="Prerequisites"></a>Předpoklady pro tento kurz webové aplikace Java
Než začnete tento kurz vývoje aplikace, musíte mít následující hello:

* Aktivní účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).

    NEBO

    Místní instalace hello [emulátoru DB Cosmos Azure](local-emulator.md).
* [Java Development Kit (JDK) 7+](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
* [Integrované vývojové prostředí Eclipse pro vývojáře v jazyce Java EE](http://www.eclipse.org/downloads/packages/eclipse-ide-java-ee-developers/lunasr1)
* [Web s Azure s Java runtime environment (např. Tomcat nebo Jetty) povolen.](../app-service-web/web-sites-java-get-started.md)

Pokud tyto nástroje pro hello instalujete poprvé, coreservlets.com poskytuje návod hello procesu instalace v části rychlý Start hello z jejich [kurz: instalace TomCat7 a jeho použití s Eclipse](http://www.coreservlets.com/Apache-Tomcat-Tutorial/tomcat-7-with-eclipse.html) článku.

## <a id="CreateDB"></a>Krok 1: Vytvoření účtu Azure Cosmos DB
Začněme vytvořením účtu služby Azure Cosmos DB. Pokud již máte účet, nebo pokud používáte hello emulátoru DB Cosmos Azure pro účely tohoto kurzu, můžete přeskočit příliš[krok 2: vytvoření aplikace Java JSP hello](#CreateJSP).

[!INCLUDE [create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

[!INCLUDE [keys](../../includes/cosmos-db-keys.md)]

## <a id="CreateJSP"></a>Krok 2: Vytvoření aplikace Java JSP hello
toocreate hello aplikace JSP:

1. Nejdříve začneme vytvořením projektu Java. Spusťte Eclipse, klikněte na **File** (Soubor), pak na **New** (Nový) a nakonec na **Dynamic Web Project** (Dynamický webový projekt). Pokud nevidíte **Dynamic Web Project** uvedené jako dostupné projekt, hello následující: klikněte na tlačítko **soubor**, klikněte na tlačítko **nový**, klikněte na tlačítko **projektu**..., rozbalte položku **webové**, klikněte na tlačítko **Dynamic Web Project**a klikněte na tlačítko **Další**.
   
    ![Vývoj aplikace Java JSP](./media/documentdb-java-application/image10.png)
2. Zadejte název projektu v hello **název projektu** pole a v hello **cílový modul Runtime** rozevírací nabídky, volitelně vyberte hodnotu (např. Apache Tomcat v7.0) a pak klikněte na tlačítko **Dokončit**. Výběr cílový modul runtime umožňuje vám toorun projektu místně prostřednictvím Eclipse.
3. V prostředí Eclipse v zobrazení Project Exploreru hello rozbalte projekt. Klikněte pravým tlačítkem na **WebContent**, pak na **New** (Nový) a nakonec na **JSP File** (Soubor JSP).
4. V hello **nový soubor JSP** dialogové okno, název souboru hello **index.jsp**. Zachovat hello nadřazené složky jako **WebContent**, jak ukazuje následující obrázek hello a pak klikněte na **Další**.
   
    ![Vytvoření nového souboru JSP – kurz vývoje aplikace Java](./media/documentdb-java-application/image11.png)
5. V hello **vybrat šablonu JSP** dialogové okno, hello pro účely tohoto kurzu možnost **nový soubor JSP (html)**a potom klikněte na **Dokončit**.
6. Když se soubor index.jsp hello se otevře v prostředí Eclipse, přidejte text toodisplay **Hello, World!** v rámci existující hello <body> elementu. Aktualizovaný <body> obsah by měl vypadat jako hello následující kód:
   
        <body>
            <% out.println("Hello World!"); %>
        </body>
7. Uložte soubor index.jsp hello.
8. Pokud v kroku 2 nastavíte cílový modul runtime, můžete kliknout na **projektu** a potom **spustit** toorun aplikaci JSP místně:
   
    ![Hello World – kurz aplikace Java](./media/documentdb-java-application/image12.png)

## <a id="InstallSDK"></a>Krok 3: Instalace hello DocumentDB Java SDK
Hello nejjednodušší způsob, jak toopull v hello DocumentDB Java SDK a jeho závislé součásti je prostřednictvím [Apache Maven](http://maven.apache.org/).

toodo, budete potřebovat tooconvert projekt maven tooa projektu provedením hello následující kroky:

1. Klikněte pravým tlačítkem na projekt v Project Exploreru hello, klikněte na tlačítko **konfigurace**, klikněte na tlačítko **převést tooMaven projektu**.
2. V hello **vytvořit nový POM** okno, přijměte výchozí hodnoty hello a klikněte na tlačítko **Dokončit**.
3. V **Project Exploreru**, otevřete soubor pom.xml hello.
4. Na hello **závislosti** na kartě hello **závislosti** podokně klikněte na tlačítko **přidat**.
5. V hello **vyberte závislost** okně hello následující:
   
   * V hello **Id skupiny** zadejte com.microsoft.azure.
   * V hello **Id artefaktů** zadejte azure-documentdb.
   * V hello **verze** zadejte 1.5.1.
     
   ![Instalace DocumentDB Java Application SDK](./media/documentdb-java-application/image13.png)
     
   * Nebo přidejte XML závislosti hello Id skupiny a Id artefaktů přímo toohello pom.xml pomocí textového editoru:
     
        <dependency><groupId>com.microsoft.azure</groupId> <artifactId>azure-documentdb</artifactId> <version>1.9.1</version></dependency>
6. Klikněte na tlačítko **OK** a Maven nainstaluje DocumentDB Java SDK hello.
7. Uložte soubor pom.xml hello.

## <a id="UseService"></a>Krok 4: Využití služby Azure Cosmos DB hello v aplikaci Java
1. Nejdříve definujme objekt TodoItem hello v TodoItem.java:
   
        @Data
        @Builder
        public class TodoItem {
            private String category;
            private boolean complete;
            private String id;
            private String name;
        }
   
    V tomto projektu používáme [Project Lombok](http://projectlombok.org/) toogenerate hello konstruktor, metody getter, setter a tvůrce. Alternativně můžete tento kód napsat ručně nebo jej vygenerovat hello IDE.
2. Služba Azure Cosmos DB hello tooinvoke, musíte vytvořit instanci novou **DocumentClient**. Obecně platí, je nejlepší hello tooreuse **DocumentClient** – spíš než vytvořit nového klienta pro každý další požadavek. Hello klienta můžeme opakovaně nástrojem pro zabalení hello klienta v **DocumentClientFactory**. DocumentClientFactory.java, je nutné URI a PRIMARY KEY hodnotu serveru toopaste hello jste uložili tooyour schránky v [krok 1](#CreateDB). Nahraďte [YOUR\_ENDPOINT\_HERE] hodnotou URI a [YOUR\_KEY\_HERE] hodnotou PRIMARY KEY.
   
        private static final String HOST = "[YOUR_ENDPOINT_HERE]";
        private static final String MASTER_KEY = "[YOUR_KEY_HERE]";
   
        private static DocumentClient documentClient = new DocumentClient(HOST, MASTER_KEY,
                        ConnectionPolicy.GetDefault(), ConsistencyLevel.Session);
   
        public static DocumentClient getDocumentClient() {
            return documentClient;
        }
3. Nyní Vytvořme objekt DAO (Data Access) tooabstract, uložením naše tooAzure položky ToDo Cosmos DB.
   
    Pořadí položek ToDo toosave tooa kolekce, hello klient potřebuje tooknow které databáze a kolekce toopersist příliš (jako odkazovaná odkazů na sebe sama). Obecně platí, je nejlepší toocache hello databázi a kolekci, pokud je to možné tooavoid nadbytečným přístupům toohello databáze.
   
    Hello následující kódu ukazuje, jak tooretrieve databázi a kolekci, pokud existuje, nebo vytvořte novou, pokud neexistuje:
   
        public class DocDbDao implements TodoDao {
            // hello name of our database.
            private static final String DATABASE_ID = "TodoDB";
   
            // hello name of our collection.
            private static final String COLLECTION_ID = "TodoCollection";
   
            // hello Azure Cosmos DB Client
            private static DocumentClient documentClient = DocumentClientFactory
                    .getDocumentClient();
   
            // Cache for hello database object, so we don't have tooquery for it to
            // retrieve self links.
            private static Database databaseCache;
   
            // Cache for hello collection object, so we don't have tooquery for it to
            // retrieve self links.
            private static DocumentCollection collectionCache;
   
            private Database getTodoDatabase() {
                if (databaseCache == null) {
                    // Get hello database if it exists
                    List<Database> databaseList = documentClient
                            .queryDatabases(
                                    "SELECT * FROM root r WHERE r.id='" + DATABASE_ID
                                            + "'", null).getQueryIterable().toList();
   
                    if (databaseList.size() > 0) {
                        // Cache hello database object so we won't have tooquery for it
                        // later tooretrieve hello selfLink.
                        databaseCache = databaseList.get(0);
                    } else {
                        // Create hello database if it doesn't exist.
                        try {
                            Database databaseDefinition = new Database();
                            databaseDefinition.setId(DATABASE_ID);
   
                            databaseCache = documentClient.createDatabase(
                                    databaseDefinition, null).getResource();
                        } catch (DocumentClientException e) {
                            // TODO: Something has gone terribly wrong - hello app wasn't
                            // able tooquery or create hello collection.
                            // Verify your connection, endpoint, and key.
                            e.printStackTrace();
                        }
                    }
                }
   
                return databaseCache;
            }
   
            private DocumentCollection getTodoCollection() {
                if (collectionCache == null) {
                    // Get hello collection if it exists.
                    List<DocumentCollection> collectionList = documentClient
                            .queryCollections(
                                    getTodoDatabase().getSelfLink(),
                                    "SELECT * FROM root r WHERE r.id='" + COLLECTION_ID
                                            + "'", null).getQueryIterable().toList();
   
                    if (collectionList.size() > 0) {
                        // Cache hello collection object so we won't have tooquery for it
                        // later tooretrieve hello selfLink.
                        collectionCache = collectionList.get(0);
                    } else {
                        // Create hello collection if it doesn't exist.
                        try {
                            DocumentCollection collectionDefinition = new DocumentCollection();
                            collectionDefinition.setId(COLLECTION_ID);
   
                            collectionCache = documentClient.createCollection(
                                    getTodoDatabase().getSelfLink(),
                                    collectionDefinition, null).getResource();
                        } catch (DocumentClientException e) {
                            // TODO: Something has gone terribly wrong - hello app wasn't
                            // able tooquery or create hello collection.
                            // Verify your connection, endpoint, and key.
                            e.printStackTrace();
                        }
                    }
                }
   
                return collectionCache;
            }
        }
4. dalším krokem Hello je toowrite některé hello toopersist kód TodoItems v kolekci toohello. V tomto příkladu použijeme [Gson](https://code.google.com/p/google-gson/) tooserialize a deserializovat dokumenty tooJSON TodoItem Plain staré objekty Java (Pojo).
   
        // We'll use Gson for POJO <=> JSON serialization for this example.
        private static Gson gson = new Gson();
   
        @Override
        public TodoItem createTodoItem(TodoItem todoItem) {
            // Serialize hello TodoItem as a JSON Document.
            Document todoItemDocument = new Document(gson.toJson(todoItem));
   
            // Annotate hello document as a TodoItem for retrieval (so that we can
            // store multiple entity types in hello collection).
            todoItemDocument.set("entityType", "todoItem");
   
            try {
                // Persist hello document using hello DocumentClient.
                todoItemDocument = documentClient.createDocument(
                        getTodoCollection().getSelfLink(), todoItemDocument, null,
                        false).getResource();
            } catch (DocumentClientException e) {
                e.printStackTrace();
                return null;
            }
   
            return gson.fromJson(todoItemDocument.toString(), TodoItem.class);
        }
5. Podobně jako databáze a kolekce Azure Cosmos DB se i dokumenty odkazují pomocí odkazů na sebe sama. Hello následující pomocné funkce umožňuje nám načíst dokumenty jiný atribut (například "id"), spíš než vlastního odkazu:
   
        private Document getDocumentById(String id) {
            // Retrieve hello document using hello DocumentClient.
            List<Document> documentList = documentClient
                    .queryDocuments(getTodoCollection().getSelfLink(),
                            "SELECT * FROM root r WHERE r.id='" + id + "'", null)
                    .getQueryIterable().toList();
   
            if (documentList.size() > 0) {
                return documentList.get(0);
            } else {
                return null;
            }
        }
6. Jsme můžete použít metodu helper hello v kroku 5 tooretrieve dokumentu JSON objektu TodoItem podle id a jeho následné deserializaci tooa POJO:
   
        @Override
        public TodoItem readTodoItem(String id) {
            // Retrieve hello document by id using our helper method.
            Document todoItemDocument = getDocumentById(id);
   
            if (todoItemDocument != null) {
                // De-serialize hello document in tooa TodoItem.
                return gson.fromJson(todoItemDocument.toString(), TodoItem.class);
            } else {
                return null;
            }
        }
7. Také můžeme použít hello DocumentClient tooget kolekce nebo seznamu objektů Todoitem pomocí DocumentDB SQL:
   
        @Override
        public List<TodoItem> readTodoItems() {
            List<TodoItem> todoItems = new ArrayList<TodoItem>();
   
            // Retrieve hello TodoItem documents
            List<Document> documentList = documentClient
                    .queryDocuments(getTodoCollection().getSelfLink(),
                            "SELECT * FROM root r WHERE r.entityType = 'todoItem'",
                            null).getQueryIterable().toList();
   
            // De-serialize hello documents in tooTodoItems.
            for (Document todoItemDocument : documentList) {
                todoItems.add(gson.fromJson(todoItemDocument.toString(),
                        TodoItem.class));
            }
   
            return todoItems;
        }
8. Existuje mnoho způsobů tooupdate dokument s hello DocumentClient. V naší aplikaci seznamu úkolů Chceme mít tootoggle toobe informaci, jestli dokončení úkolu. Toho lze dosáhnout aktualizací atributu "úplná" hello v rámci hello dokumentu:
   
        @Override
        public TodoItem updateTodoItem(String id, boolean isComplete) {
            // Retrieve hello document from hello database
            Document todoItemDocument = getDocumentById(id);
   
            // You can update hello document as a JSON document directly.
            // For more complex operations - you could de-serialize hello document in
            // tooa POJO, update hello POJO, and then re-serialize hello POJO back in to
            // a document.
            todoItemDocument.set("complete", isComplete);
   
            try {
                // Persist/replace hello updated document.
                todoItemDocument = documentClient.replaceDocument(todoItemDocument,
                        null).getResource();
            } catch (DocumentClientException e) {
                e.printStackTrace();
                return null;
            }
   
            return gson.fromJson(todoItemDocument.toString(), TodoItem.class);
        }
9. Nakonec chcete mít možnost toodelete hello úkolu ze seznamu. toodo, můžeme použít metodu helper hello jsme napsali dříve tooretrieve hello vlastního odkazu a potom sdělte hello klienta toodelete ho:
   
        @Override
        public boolean deleteTodoItem(String id) {
            // Azure Cosmos DB refers toodocuments by self link rather than id.
   
            // Query for hello document tooretrieve hello self link.
            Document todoItemDocument = getDocumentById(id);
   
            try {
                // Delete hello document by self link.
                documentClient.deleteDocument(todoItemDocument.getSelfLink(), null);
            } catch (DocumentClientException e) {
                e.printStackTrace();
                return false;
            }
   
            return true;
        }

## <a id="Wire"></a>Krok 5: Vzájemné propojení zbytku hello hello projektu vývoje aplikace Java společně
Teď, když jsme dokončili hello fun bits - již zbývá je toobuild rychlé uživatelské rozhraní a propojit je s až tooour DAO.

1. Nejdříve začneme vytvořením kontroleru toocall náš objekt DAO:
   
        public class TodoItemController {
            public static TodoItemController getInstance() {
                if (todoItemController == null) {
                    todoItemController = new TodoItemController(TodoDaoFactory.getDao());
                }
                return todoItemController;
            }
   
            private static TodoItemController todoItemController;
   
            private final TodoDao todoDao;
   
            TodoItemController(TodoDao todoDao) {
                this.todoDao = todoDao;
            }
   
            public TodoItem createTodoItem(@NonNull String name,
                    @NonNull String category, boolean isComplete) {
                TodoItem todoItem = TodoItem.builder().name(name).category(category)
                        .complete(isComplete).build();
                return todoDao.createTodoItem(todoItem);
            }
   
            public boolean deleteTodoItem(@NonNull String id) {
                return todoDao.deleteTodoItem(id);
            }
   
            public TodoItem getTodoItemById(@NonNull String id) {
                return todoDao.readTodoItem(id);
            }
   
            public List<TodoItem> getTodoItems() {
                return todoDao.readTodoItems();
            }
   
            public TodoItem updateTodoItem(@NonNull String id, boolean isComplete) {
                return todoDao.updateTodoItem(id, isComplete);
            }
        }
   
    Ve složitější aplikaci může hello kontroler obsahovat komplikovanou obchodní logiku nad hello DAO.
2. V dalším kroku vytvoříme řadič servlet požadavky HTTP tooroute toohello:
   
        public class TodoServlet extends HttpServlet {
            // API Keys
            public static final String API_METHOD = "method";
   
            // API Methods
            public static final String CREATE_TODO_ITEM = "createTodoItem";
            public static final String GET_TODO_ITEMS = "getTodoItems";
            public static final String UPDATE_TODO_ITEM = "updateTodoItem";
   
            // API Parameters
            public static final String TODO_ITEM_ID = "todoItemId";
            public static final String TODO_ITEM_NAME = "todoItemName";
            public static final String TODO_ITEM_CATEGORY = "todoItemCategory";
            public static final String TODO_ITEM_COMPLETE = "todoItemComplete";
   
            public static final String MESSAGE_ERROR_INVALID_METHOD = "{'error': 'Invalid method'}";
   
            private static final long serialVersionUID = 1L;
            private static final Gson gson = new Gson();
   
            @Override
            protected void doGet(HttpServletRequest request,
                    HttpServletResponse response) throws ServletException, IOException {
   
                String apiResponse = MESSAGE_ERROR_INVALID_METHOD;
   
                TodoItemController todoItemController = TodoItemController
                        .getInstance();
   
                String id = request.getParameter(TODO_ITEM_ID);
                String name = request.getParameter(TODO_ITEM_NAME);
                String category = request.getParameter(TODO_ITEM_CATEGORY);
                boolean isComplete = StringUtils.equalsIgnoreCase("true",
                        request.getParameter(TODO_ITEM_COMPLETE)) ? true : false;
   
                switch (request.getParameter(API_METHOD)) {
                case CREATE_TODO_ITEM:
                    apiResponse = gson.toJson(todoItemController.createTodoItem(name,
                            category, isComplete));
                    break;
                case GET_TODO_ITEMS:
                    apiResponse = gson.toJson(todoItemController.getTodoItems());
                    break;
                case UPDATE_TODO_ITEM:
                    apiResponse = gson.toJson(todoItemController.updateTodoItem(id,
                            isComplete));
                    break;
                default:
                    break;
                }
   
                response.getWriter().println(apiResponse);
            }
   
            @Override
            protected void doPost(HttpServletRequest request,
                    HttpServletResponse response) throws ServletException, IOException {
                doGet(request, response);
            }
        }
3. Budeme potřebovat webové uživatelské rozhraní toodisplay toohello uživatele. Znovu napište hello index.jsp, že jsme vytvořili výše:
    ```html
        <html>
        <head>
          <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
          <meta http-equiv="X-UA-Compatible" content="IE=edge;" />
          <title>Azure Cosmos DB Java Sample</title>
   
          <!-- Bootstrap -->
          <link href="//ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.min.css" rel="stylesheet">
   
          <style>
            /* Add padding toobody for fixed nav bar */
            body {
              padding-top: 50px;
            }
          </style>
        </head>
        <body>
          <!-- Nav Bar -->
          <div class="navbar navbar-inverse navbar-fixed-top" role="navigation">
            <div class="container">
              <div class="navbar-header">
                <a class="navbar-brand" href="#">My Tasks</a>
              </div>
            </div>
          </div>
   
          <!-- Body -->
          <div class="container">
            <h1>My ToDo List</h1>
   
            <hr/>
   
            <!-- hello ToDo List -->
            <div class = "todoList">
              <table class="table table-bordered table-striped" id="todoItems">
                <thead>
                  <tr>
                    <th>Name</th>
                    <th>Category</th>
                    <th>Complete</th>
                  </tr>
                </thead>
                <tbody>
                </tbody>
              </table>
   
              <!-- Update Button -->
              <div class="todoUpdatePanel">
                <form class="form-horizontal" role="form">
                  <button type="button" class="btn btn-primary">Update Tasks</button>
                </form>
              </div>
   
            </div>
   
            <hr/>
   
            <!-- Item Input Form -->
            <div class="todoForm">
              <form class="form-horizontal" role="form">
                <div class="form-group">
                  <label for="inputItemName" class="col-sm-2">Task Name</label>
                  <div class="col-sm-10">
                    <input type="text" class="form-control" id="inputItemName" placeholder="Enter name">
                  </div>
                </div>
   
                <div class="form-group">
                  <label for="inputItemCategory" class="col-sm-2">Task Category</label>
                  <div class="col-sm-10">
                    <input type="text" class="form-control" id="inputItemCategory" placeholder="Enter category">
                  </div>
                </div>
   
                <button type="button" class="btn btn-primary">Add Task</button>
              </form>
            </div>
   
          </div>
   
          <!-- Placed at hello end of hello document so hello pages load faster -->
          <script src="//ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.js"></script>
          <script src="//ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.min.js"></script>
          <script src="assets/todo.js"></script>
        </body>
        </html>
    ```
4. A nakonec zápis některých klienta JavaScript tootie hello webové uživatelské rozhraní a společně hello servletem:
   
        var todoApp = {
          /*
           * API methods toocall Java backend.
           */
          apiEndpoint: "api",
   
          createTodoItem: function(name, category, isComplete) {
            $.post(todoApp.apiEndpoint, {
                "method": "createTodoItem",
                "todoItemName": name,
                "todoItemCategory": category,
                "todoItemComplete": isComplete
              },
              function(data) {
                var todoItem = data;
                todoApp.addTodoItemToTable(todoItem.id, todoItem.name, todoItem.category, todoItem.complete);
              },
              "json");
          },
   
          getTodoItems: function() {
            $.post(todoApp.apiEndpoint, {
                "method": "getTodoItems"
              },
              function(data) {
                var todoItemArr = data;
                $.each(todoItemArr, function(index, value) {
                  todoApp.addTodoItemToTable(value.id, value.name, value.category, value.complete);
                });
              },
              "json");
          },
   
          updateTodoItem: function(id, isComplete) {
            $.post(todoApp.apiEndpoint, {
                "method": "updateTodoItem",
                "todoItemId": id,
                "todoItemComplete": isComplete
              },
              function(data) {},
              "json");
          },
   
          /*
           * UI Methods
           */
          addTodoItemToTable: function(id, name, category, isComplete) {
            var rowColor = isComplete ? "active" : "warning";
   
            todoApp.ui_table().append($("<tr>")
              .append($("<td>").text(name))
              .append($("<td>").text(category))
              .append($("<td>")
                .append($("<input>")
                  .attr("type", "checkbox")
                  .attr("id", id)
                  .attr("checked", isComplete)
                  .attr("class", "isComplete")
                ))
              .addClass(rowColor)
            );
          },
   
          /*
           * UI Bindings
           */
          bindCreateButton: function() {
            todoApp.ui_createButton().click(function() {
              todoApp.createTodoItem(todoApp.ui_createNameInput().val(), todoApp.ui_createCategoryInput().val(), false);
              todoApp.ui_createNameInput().val("");
              todoApp.ui_createCategoryInput().val("");
            });
          },
   
          bindUpdateButton: function() {
            todoApp.ui_updateButton().click(function() {
              // Disable button temporarily.
              var myButton = $(this);
              var originalText = myButton.text();
              $(this).text("Updating...");
              $(this).prop("disabled", true);
   
              // Call api tooupdate todo items.
              $.each(todoApp.ui_updateId(), function(index, value) {
                todoApp.updateTodoItem(value.name, value.value);
                $(value).remove();
              });
   
              // Re-enable button.
              setTimeout(function() {
                myButton.prop("disabled", false);
                myButton.text(originalText);
              }, 500);
            });
          },
   
          bindUpdateCheckboxes: function() {
            todoApp.ui_table().on("click", ".isComplete", function(event) {
              var checkboxElement = $(event.currentTarget);
              var rowElement = $(event.currentTarget).parents('tr');
              var id = checkboxElement.attr('id');
              var isComplete = checkboxElement.is(':checked');
   
              // Toggle table row color
              if (isComplete) {
                rowElement.addClass("active");
                rowElement.removeClass("warning");
              } else {
                rowElement.removeClass("active");
                rowElement.addClass("warning");
              }
   
              // Update hidden inputs for update panel.
              todoApp.ui_updateForm().children("input[name='" + id + "']").remove();
   
              todoApp.ui_updateForm().append($("<input>")
                .attr("type", "hidden")
                .attr("class", "updateComplete")
                .attr("name", id)
                .attr("value", isComplete));
   
            });
          },
   
          /*
           * UI Elements
           */
          ui_createNameInput: function() {
            return $(".todoForm #inputItemName");
          },
   
          ui_createCategoryInput: function() {
            return $(".todoForm #inputItemCategory");
          },
   
          ui_createButton: function() {
            return $(".todoForm button");
          },
   
          ui_table: function() {
            return $(".todoList table tbody");
          },
   
          ui_updateButton: function() {
            return $(".todoUpdatePanel button");
          },
   
          ui_updateForm: function() {
            return $(".todoUpdatePanel form");
          },
   
          ui_updateId: function() {
            return $(".todoUpdatePanel .updateComplete");
          },
   
          /*
           * Install hello TodoApp
           */
          install: function() {
            todoApp.bindCreateButton();
            todoApp.bindUpdateButton();
            todoApp.bindUpdateCheckboxes();
   
            todoApp.getTodoItems();
          }
        };
   
        $(document).ready(function() {
          todoApp.install();
        });
5. Skvělé! Nyní již zbývá je tootest hello aplikace. Místní spuštění aplikace hello a přidejte několik položek Todo vyplněním hello názvů a kategorie položek a kliknutím na tlačítko **přidat úloha**.
6. Jakmile se zobrazí položka hello, můžete aktualizovat, zda je dokončená, přepínáním přepnutím hello políčka a kliknutím na **aktualizace úlohy**.

## <a id="Deploy"></a>Krok 6: Nasazení vaší tooAzure aplikace Java webů
Díky weby Azure je nasazování aplikací Java stejně snadné jako Export aplikace jako souboru WAR a jeho nahrání buď přes řízení zdrojů (např. Git) nebo FTP.

1. tooexport aplikace jako souboru WAR, klikněte pravým tlačítkem na projekt v **Project Exploreru**, klikněte na tlačítko **exportovat**a potom klikněte na **soubor WAR**.
2. V hello **WAR Export** okně hello následující:
   
   * Hello webového projektu pole zadejte azure-documentdb-java-sample.
   * Hello cílového pole zvolte cílový soubor WAR hello toosave.
   * Klikněte na **Dokončit**.
3. Teď, když máte soubor WAR v ručně, můžete tento soubor jednoduše nahrát tooyour webu Azure na **webapps** adresáře. Pokyny pro nahrání souboru hello, v tématu [přidat tooAzure aplikace Java App Service Web Apps](../app-service-web/web-sites-java-add-app.md).
   
    Jakmile soubor WAR hello nahrané toohello adresáře webapps, hello běhové prostředí zjistí, že jste jej přidali a automaticky ho načte.
4. tooview hotový produkt, přejděte toohttp://YOUR\_lokality\_NAME.azurewebsites.net/azure-java-sample/ a začněte přidávat úkoly!

## <a id="GetProject"></a>Získat hello projektu z Githubu
Všechny ukázky hello v tomto kurzu jsou součástí hello [todo](https://github.com/Azure-Samples/documentdb-java-todo-app) projektu na Githubu. tooimport hello projekt todo do prostředí Eclipse, ujistěte se, máte hello software a prostředky uvedené v hello [požadavky](#Prerequisites) části a pak hello následující:

1. Nainstalujte [Project Lombok](http://projectlombok.org/). Lombok je použité toogenerate konstruktory, metod getter a setter v projektu hello. Jakmile budete mít stažen soubor lombok.jar hello, klikněte dvakrát na jeho tooinstall ji nebo ji nainstalovat z příkazového řádku hello.
2. Pokud je prostředí Eclipse otevřené, zavřete ji a restartujte ji tooload Lombok.
3. V prostředí Eclipse v hello **soubor** nabídky, klikněte na tlačítko **Import**.
4. V hello **Import** okně klikněte na tlačítko **Git**, klikněte na tlačítko **projekty z Gitu**a potom klikněte na **Další**.
5. Na hello **vybrat zdroj úložiště** obrazovky, klikněte na tlačítko **klon URI**.
6. Na hello **zdrojové úložiště Git** obrazovky v hello **URI** zadejte https://github.com/Azure-Samples/java-todo-app.git a pak klikněte na tlačítko **Další**.
7. Na hello **výběr větve** obrazovky, ujistěte se, že **hlavní** je vybrána a potom klikněte na **Další**.
8. Na hello **místní cílovou** obrazovky, klikněte na tlačítko **Procházet** tooselect do složky, kam lze zkopírovat hello úložiště a potom klikněte na **Další**.
9. Na hello **vyberte toouse Průvodce pro import projektů** obrazovky, ujistěte se, že **importovat existující projekty** je vybrána a potom klikněte na **Další**.
10. Na hello **Import projektů** obrazovky, zrušit výběr hello **DocumentDB** projektu a pak klikněte na **Dokončit**. Projekt DocumentDB Hello obsahuje hello Azure Cosmos DB Java SDK, kterou přidáme jako závislost místo.
11. V **Project Exploreru**, přejděte tooazure-documentdb-java-sample\src\com.microsoft.azure.documentdb.sample.dao\DocumentClientFactory.java a nahraďte hodnoty HOST a MASTER_KEY hello hello URI a primární klíč pro vaše Azure Cosmos DB účet a potom soubor uložit hello. Další informace najdete v části [Krok 1. Vytvoření účtu Azure Cosmos DB databáze](#CreateDB).
12. V **Project Exploreru**, klikněte pravým tlačítkem na hello **azure-documentdb-java-sample**, klikněte na tlačítko **cesta sestavení**a pak klikněte na tlačítko **konfigurace cesta sestavení**.
13. Na hello **cesta sestavení Java** obrazovky v pravém podokně hello vyberte hello **knihovny** a pak klikněte **přidat externí JARs**. Přejděte toohello umístění souboru lombok.jar hello a klikněte na tlačítko **otevřete**a potom klikněte na **OK**.
14. Pomocí kroku 12 tooopen hello **vlastnosti** znovu okno a v levém podokně hello klikněte na **Targeted Runtimes**.
15. Na hello **Targeted Runtimes** obrazovky, klikněte na tlačítko **nový**, vyberte **Apache Tomcat v7.0**a potom klikněte na **OK**.
16. Pomocí kroku 12 tooopen hello **vlastnosti** znovu okno a v levém podokně hello klikněte na **omezující vlastnosti projektu**.
17. Na hello **omezující vlastnosti projektu** obrazovku, vyberte **dynamický webový modul** a **Java**a potom klikněte na **OK**.
18. Na hello **servery** kartě dole hello úvodní obrazovka, klikněte pravým tlačítkem na **Tomcat v7.0 Server at localhost** a pak klikněte na **přidávat a odebírat**.
19. Na hello **přidávat a odebírat** okně přesunout **azure-documentdb-java-sample** toohello **konfigurovaná** pole a pak klikněte na **Dokončit**.
20. V hello **servery** kartě, klikněte pravým tlačítkem na **Tomcat v7.0 Server at localhost**a potom klikněte na **restartujte**.
21. V prohlížeči přejděte toohttp://localhost:8080 / azure-documentdb-java-sample / a začněte přidávat položky seznamu úkolů tooyour. Všimněte si, že pokud jste změnili výchozí port hodnoty, změňte hodnotu toohello 8080, který jste vybrali.
22. toodeploy tooan vašeho projektu webu Azure, najdete v části [kroku 6. Nasazení vaší aplikace tooAzure weby](#Deploy).

[1]: media/documentdb-java-application/keys.png
