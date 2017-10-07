---
title: "aaaBuild a nasazení aplikace Java API v Azure App Service"
description: "Zjistěte, jak toocreate aplikace Java API balíček a jeho nasazení tooAzure služby App Service."
services: app-service\api
documentationcenter: java
author: rmcmurray
manager: erikre
editor: tdykstra
ms.assetid: 8d21ba5f-fc57-4269-bc8f-2fcab936ec22
ms.service: app-service-api
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: get-started-article
ms.date: 04/25/2017
ms.author: rachelap;robmcm
ms.openlocfilehash: a4056fec870b1c4bed8ee14bb0e748b3ee89b9e1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="build-and-deploy-a-java-api-app-in-azure-app-service"></a>Sestavení a nasazení aplikace API Java v Azure App Service
[!INCLUDE [app-service-api-get-started-selector](../../includes/app-service-api-get-started-selector.md)]

Tento kurz ukazuje, jak toocreate aplikaci Java a nasaďte ho tooAzure App Service API Apps pomocí [Git]. Hello pokyny v tomto kurzu platí pro všechny operační systémy, které podporují Javu. Hello kód v tomto kurzu je sestaven pomocí [Maven]. [Jax-RS] je použité toocreate hello služba RESTful a vygeneruje se podle hello [Swagger] metadat specifikace pomocí hello [editoru Swagger].

## <a name="prerequisites"></a>Požadavky
1. [Sada pro vývojáře Java 8]\(nebo novější)
2. Nástroj [Maven] nainstalovaný na počítači pro vývoj
3. Nástroj [Git] nainstalovaný na počítači pro vývoj
4. Placená nebo [bezplatnou zkušební verzi] předplatné příliš[Microsoft Azure]
5. Testovací aplikace HTTP, například [Postman]

## <a name="scaffold-hello-api-using-swaggerio"></a>Rozhraní API hello vygenerované uživatelské rozhraní pomocí editoru Swagger.IO
Pomocí online editoru swagger.io hello, můžete zadat kód YAML nebo JSON pro Swagger představující strukturu hello rozhraní API. Jakmile máte hello API útoku na určený, můžete exportovat kódu pro různé platformy a rozhraní. V další části hello hello vygeneroval kód bude upravené tooinclude imitované funkce. 

Tato ukázka začíná s textu JSON pro Swagger, které vložíte do editoru swagger.io hello, který bude potom být použité toogenerate kód využitím JAX-RS tooaccess koncový bod REST API. Potom budete upravovat hello vygeneroval kód tooreturn imitovaná data, simulaci REST API vytvořené na mechanismu trvalosti.  

1. Zkopírujte následující schránky tooyour kód JSON pro Swagger hello:
   
        {
            "swagger": "2.0",
            "info": {
                "version": "v1",
                "title": "Contact List",
                "description": "A Contact list API based on Swagger and built using Java"
            },
            "host": "localhost",
            "schemes": [
                "http",
                "https"
            ],
            "basePath": "/api",
            "paths": {
                "/contacts": {
                    "get": {
                        "tags": [
                            "Contact"
                        ],
                        "operationId": "contacts_get",
                        "consumes": [],
                        "produces": [
                            "application/json",
                            "text/json"
                        ],
                        "responses": {
                            "200": {
                                "description": "OK",
                                "schema": {
                                    "type": "array",
                                    "items": {
                                        "$ref": "#/definitions/Contact"
                                    }
                                }
                            }
                        },
                        "deprecated": false
                    }
                },
                "/contacts/{id}": {
                    "get": {
                        "tags": [
                            "Contact"
                        ],
                        "operationId": "contacts_getById",
                        "consumes": [],
                        "produces": [
                            "application/json",
                            "text/json"
                        ],
                        "parameters": [
                            {
                                "name": "id",
                                "in": "path",
                                "required": true,
                                "type": "integer",
                                "format": "int32"
                            }
                        ],
                        "responses": {
                            "200": {
                                "description": "OK",
                                "schema": {
                                    "type": "array",
                                    "items": {
                                        "$ref": "#/definitions/Contact"
                                    }
                                }
                            }
                        },
                        "deprecated": false
                    }
                }
            },
            "definitions": {
                "Contact": {
                    "type": "object",
                    "properties": {
                        "Id": {
                            "format": "int32",
                            "type": "integer"
                        },
                        "Name": {
                            "type": "string"
                        },
                        "EmailAddress": {
                            "type": "string"
                        }
                    }
                }
            }
        }
2. Přejděte toohello [Online Swagger Editor]. Potom klikněte hello **souboru -> Paste JSON** položku nabídky.
   
    ![Položka nabídky Paste JSON (Vložit JSON)][paste-json]
3. Vložte hello kontakty seznamu rozhraní API JSON pro Swagger jste zkopírovali dříve. 
   
    ![Vložení kódu JSON do Swaggeru][pasted-swagger]
4. Zobrazit hello stránky dokumentace a souhrn rozhraní API vykreslený v editoru hello. 
   
    ![Zobrazení dokumentů generovaných ve Swaggeru][view-swagger-generated-docs]
5. Vyberte hello **generovat Server -> JAX-RS** nabídky možnost tooscaffold hello serverový kód budete upravovat novější tooadd imitované implementace. 
   
    ![Generování položky nabídky kódu][generate-code-menu-item]
   
    Po vygenerování kódu hello budete je třeba zadat toodownload souboru ZIP. Tento soubor obsahuje hello kód automaticky vytvořený generátorem kódu Swagger hello a všechny přidružené skripty sestavení. Rozbalte hello celou knihovnu tooa adresáře na pracovní stanici. 

## <a name="edit-hello-code-tooadd-api-implementation"></a>Upravit kód tooadd hello implementace rozhraní API
V této části nahradíte hello generované Swagger kódu na straně serveru implementace vlastním kódem. Hello nový kód vrátí toohello entity kontaktů ArrayList pro volajícího klienta. 

1. Otevřete hello *Contact.java* souboru modelu, který se nachází v hello *src/gen/java/vstupně-výstupní operace/swagger/model* složky, pomocí [Visual Studio Code] nebo svém oblíbeném textovém editoru. 
   
    ![Otevření souboru modelu kontaktů][open-contact-model-file]
2. Přidejte následující konstruktor v rámci hello hello **kontaktujte** třídy. 
   
        public Contact(Integer id, String name, String email) 
        {
            this.id = id;
            this.name = name;
            this.emailAddress = email;
        }
3. Otevřete hello *ContactsApiServiceImpl.java* soubor implementace služby, který je umístěný ve hello *src/main/java/vstupně-výstupní operace/swagger/rozhraní api/impl* složky, pomocí [Visual Studio Code]nebo svém oblíbeném textovém editoru.
   
    ![Otevření souboru kódu služby kontaktů][open-contact-service-code-file]
4. Přepište hello kód v souboru hello tento nový kód tooadd kód služby toohello imitované implementace. 
   
        package io.swagger.api.impl;
   
        import io.swagger.api.*;
        
        import io.swagger.model.Contact;
        import java.util.*;
        import io.swagger.api.NotFoundException;
               
        import javax.ws.rs.core.Response;
        import javax.ws.rs.core.SecurityContext;
   
        @javax.annotation.Generated(value = "class io.swagger.codegen.languages.JaxRSServerCodegen", date = "2015-11-24T21:54:11.648Z")
        public class ContactsApiServiceImpl extends ContactsApiService {
   
            private ArrayList<Contact> loadContacts()
            {
                ArrayList<Contact> list = new ArrayList<Contact>();
                list.add(new Contact(1, "Barney Poland", "barney@contoso.com"));
                list.add(new Contact(2, "Lacy Barrera", "lacy@contoso.com"));
                list.add(new Contact(3, "Lora Riggs", "lora@contoso.com"));
                return list;
            }
   
            @Override
            public Response contactsGet(SecurityContext securityContext)
            throws NotFoundException {
                ArrayList<Contact> list = loadContacts();
                return Response.ok().entity(list).build();
                }
   
            @Override
            public Response contactsGetById(Integer id, SecurityContext securityContext)
            throws NotFoundException {
                ArrayList<Contact> list = loadContacts();
                Contact ret = null;
   
                for(int i=0; i<list.size(); i++)
                {
                    if(list.get(i).getId() == id)
                        {
                            ret = list.get(i);
                        }
                }
                return Response.ok().entity(ret).build();
            }
        }
5. Otevřete příkazový řádek a změňte adresář toohello kořenové složky vaší aplikace.
6. Spusťte následující kód hello toobuild příkazu Maven hello a spustit místně pomocí aplikačního serveru Jetty hello. 
   
        mvn package jetty:run
7. Měli byste vidět hello příkazové okno, že Jetty tento kód spustil na portu 8080. 
   
    ![Otevření souboru kódu služby kontaktů][run-jetty-war]
8. Použití [Postman] metoda toomake žádost toohello "get all contacts" API na adrese http://localhost: 8080/api/contacts.
   
    ![Hello volání rozhraní API kontaktů][calling-contacts-api]
9. Použití [Postman] toomake žádost toohello "get specific contact" API metoda umístěné na adrese http://localhost: 8080/api/contacts/2.
   
    ![Hello volání rozhraní API kontaktů][calling-specific-contact-api]
10. Nakonec sestavte soubor Java WAR (webový archiv) hello tak, že spustíte následující příkaz Mavenu v konzole hello. 
    
         mvn package war:war
11. Jakmile soubor WAR hello se umístí do hello **cíl** složky. Přejděte do hello **cíl** složku a přejmenujte hello soubor WAR příliš**ROOT.war**. (Ujistěte se, že hello použití velkých písmen odpovídá tomuto formátu).
    
          rename swagger-jaxrs-server-1.0.0.war ROOT.war
12. Nakonec spuštěním následujících příkazů z kořenové složky vaší aplikace toocreate hello hello **nasazení** složky toouse toodeploy hello WAR souboru tooAzure. 
    
          mkdir deploy
          mkdir deploy\webapps
          copy target\ROOT.war deploy\webapps
          cd deploy

## <a name="publish-hello-output-tooazure-app-service"></a>Publikování tooAzure výstup hello služby App Service
V této části se dozvíte, jak toocreate nové aplikace API hello pomocí portálu Azure, tuto aplikaci API připravit na hostování aplikací Java a nasazení hello nově vytvořené WAR souboru tooAzure služby App Service toorun novou aplikaci API. 

1. Vytvoření nové aplikace API v hello [portál Azure], kliknutím hello **nový -> Web + mobilní zařízení -> aplikace API** položky nabídky, zadáním podrobností o aplikaci a potom kliknutím na **vytvořit**.
   
    ![Vytvoření nové aplikace API][create-api-app]
2. Po vytvoření aplikace API otevřete v této aplikaci **nastavení** okno a potom klikněte na hello **nastavení aplikace** položku nabídky. Vyberte hello nejnovější verzi Javy z dostupných možností hello, pak vyberte hello nejnovější verzi serveru Tomcat hello **webový kontejner** nabídce a pak klikněte na tlačítko **Uložit**.
   
    ![Nastavení Javy v okně aplikace API hello][set-up-java]
3. Klikněte na tlačítko hello **přihlašovací údaje nasazení** nabídky nastavení položky a zadejte uživatelské jméno a heslo, které chcete toouse pro publikování tooyour soubory aplikace API. 
   
    ![Nastavení přihlašovacích údajů nasazení][deployment-credentials]
4. Klikněte na tlačítko hello **zdroj nasazení** položku nabídky nastavení. Potom klikněte hello **vybrat zdroj** tlačítko, vyberte hello **místní úložiště Git** a potom klikněte na **OK**. Tím se vytvoří úložiště Git spuštěné v Azure, které bude přidružené k vaší aplikaci API. Pokaždé, když potvrdíte nějaký kód toohello *hlavní* větve úložiště Git, váš kód publikuje v živé, běžící instanci aplikace API. 
   
    ![Nastavení nového místního úložiště Git][select-git-repo]
5. Zkopírujte schránky tooyour URL hello nového úložiště Git. Uložte ji, protože ji budete zanedlouho potřebovat. 
   
    ![Nastavení nového úložiště Git pro vaši aplikaci][copy-git-repo-url]
6. Git push hello WAR toohello online úložiště souborů. toodo to, že přejdete do hello **nasazení** dříve vytvořená složka tak, aby mohli snadno Potvrdit kód hello až toohello úložiště spuštěného v App Service. Jednou jste v okně konzoly hello a do hello složku, kde je umístěna, složka webapps hello vydávat hello postupu hello toolaunch příkazy Git a spusťte nasazení provedením. 
   
         git init
         git add .
         git commit -m "initial commit"
         git remote add azure [YOUR GIT URL]
         git push azure master
   
    Jakmile odešlete hello **nabízené** požadavek, zobrazí se výzva k hello hesla, které jste předtím vytvořili pro přihlašovací údaje nasazení hello. Po zadání přihlašovacích údajů, měli byste vidět, že vaše portálu zobrazení, které hello aktualizace byl nasazen.
7. Pokud chcete znovu použít Postman toohit hello nově nasazené aplikace API spuštěné v Azure App Service, se zobrazí, zda text hello chování je konzistentní a že vrací kontaktní údaje podle očekávání, a pomocí jednoduchého kódu změny toohello Swagger.io automaticky generovaný kód Java. 
   
    ![Použití živého REST API kontaktů Java v Azure][postman-calling-azure-contacts]

## <a name="next-steps"></a>Další kroky
V tomto článku jste možnost toostart s soubor JSON pro Swagger a některé automaticky generovaný kód Java získaný v editoru Swagger.io hello. Jednoduchými změnami kódu a procesem nasazení v Gitu jste získali funkční aplikaci API napsanou v jazyce Java. Hello dalším kurzu se dozvíte, jak příliš[využívat aplikace API z klientů JavaScript pomocí CORS][App Service API CORS]. Další kurzy v hello řady zobrazit jak tooimplement ověřování a autorizace.

toobuild s tímto příkladem, další informace o hello [sada SDK úložiště pro jazyk Java] toopersist hello JSON BLOB. Nebo můžete použít hello [sada SDK Document DB Java] toosave vaše obraťte se na data tooAzure DB dokumentu. 

<a name="see-also"></a>

## <a name="see-also"></a>Viz také
Další informace o používání Javy v Azure najdete na webu [Azure pro vývojáře v Javě](/java/azure).

<!-- URL List -->

[App Service API CORS]: app-service-api-cors-consume-javascript.md
[portál Azure]: https://portal.azure.com/
[sada SDK Document DB Java]: ../documentdb/documentdb-java-application.md
[bezplatnou zkušební verzi]: https://azure.microsoft.com/pricing/free-trial/
[Git]: http://www.git-scm.com/
[Azure Java Developer Center]: /develop/java/
[Sada pro vývojáře Java 8]: http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
[Jax-RS]: https://jax-rs-spec.java.net/
[Maven]: https://maven.apache.org/
[Microsoft Azure]: https://azure.microsoft.com/
[Online Swagger Editor]: http://editor2.swagger.io/
[Postman]: https://www.getpostman.com/
[sada SDK úložiště pro jazyk Java]:../storage/blobs/storage-java-how-to-use-blob-storage.md
[Swagger]: http://swagger.io/
[editoru Swagger]: http://editor.swagger.io/
[Visual Studio Code]: https://code.visualstudio.com

<!-- IMG List -->

[paste-json]: ./media/app-service-api-java-api-app/paste-json.png
[pasted-swagger]: ./media/app-service-api-java-api-app/pasted-swagger.png
[view-swagger-generated-docs]: ./media/app-service-api-java-api-app/view-swagger-generated-docs.png
[generate-code-menu-item]: ./media/app-service-api-java-api-app/generate-code-menu-item.png
[open-contact-model-file]: ./media/app-service-api-java-api-app/open-contact-model-file.png
[open-contact-service-code-file]: ./media/app-service-api-java-api-app/open-contact-service-code-file.png
[run-jetty-war]: ./media/app-service-api-java-api-app/run-jetty-war.png
[calling-contacts-api]: ./media/app-service-api-java-api-app/calling-contacts-api.png
[calling-specific-contact-api]: ./media/app-service-api-java-api-app/calling-specific-contact-api.png
[create-api-app]: ./media/app-service-api-java-api-app/create-api-app.png
[set-up-java]: ./media/app-service-api-java-api-app/set-up-java.png
[deployment-credentials]: ./media/app-service-api-java-api-app/deployment-credentials.png
[select-git-repo]: ./media/app-service-api-java-api-app/select-git-repo.png
[copy-git-repo-url]: ./media/app-service-api-java-api-app/copy-git-repo-url.png
[postman-calling-azure-contacts]: ./media/app-service-api-java-api-app/postman-calling-azure-contacts.png
