---
title: "aaaView vrátil SAML hello služby Řízení přístupu (Java)"
description: "Zjistěte, jak tooview SAML vrácený hello služby Řízení přístupu v aplikacích Java hostované v Azure."
services: active-directory
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 6cd216f9-eb43-46b4-b30d-f194d0ae2d48
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.custom: aaddev
ms.openlocfilehash: b6733bc98b505cfa89a4ce456f368ee15da11427
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooview-saml-returned-by-hello-azure-access-control-service"></a>Jak tooview SAML vrácený hello služby Řízení přístupu Azure
Tento průvodce vám ukáže, jak tooview hello základní Security (Assertion Markup Language SAML) vrátila tooyour aplikace hello služby Řízení přístupu Azure (ACS). Průvodce Hello vychází hello [jak tooAuthenticate webovým uživatelům s Azure přístup k řízení služby pomocí Eclipse](active-directory-java-authenticate-users-access-control-eclipse.md) tématu, tím, že poskytuje kód, který zobrazí informace SAML hello. aplikace Hello Dokončit bude vypadat podobně jako následující toohello.

![Příklad výstupu SAML][saml_output]

Další informace o ACS najdete v tématu hello [další kroky](#next_steps) části.

> [!NOTE]
> Hello filtru řízení služeb Azure přístup je náhled technologie komunity. Jako předběžné verze softwaru není oficiálně společností Microsoft.
> 
> 

## <a name="prerequisites"></a>Požadavky
toocomplete hello úlohy v této příručce, dokončení hello ukázku najdete na adrese [jak tooAuthenticate webovým uživatelům s Azure přístup k řízení služby pomocí Eclipse](active-directory-java-authenticate-users-access-control-eclipse.md) a používejte ho jako výchozí bod pro účely tohoto kurzu hello.

## <a name="add-hello-jspwriter-library-tooyour-build-path-and-deployment-assembly"></a>Přidat hello JspWriter tooyour sestavení a nasazení cesta sestavení knihovny
Přidání hello knihovny, který obsahuje hello **javax.servlet.jsp.JspWriter** tooyour třída sestavení a nasazení cesta sestavení. Pokud používáte Tomcat, je hello knihovně **jsp api.jar**, který se nachází v hello Apache **lib** složky.

1. V prostředí Eclipse v prohlížeči projektu klikněte pravým tlačítkem na **MyACSHelloWorld**, klikněte na tlačítko **cesta sestavení**, klikněte na tlačítko **konfigurovat cestu sestavení**, klikněte na tlačítko hello **knihovny** a pak klikněte **přidat externí JARs**.
2. V hello **JAR výběr** dialogové okno, přejděte toohello nezbytné JAR, vyberte ho a pak klikněte na tlačítko **otevřete**.
3. S hello **vlastnosti pro MyACSHelloWorld** stále otevřená, klikněte na dialogové okno **nasazení sestavení**.
4. V hello **webové nasazení sestavení** dialogové okno, klikněte na tlačítko **přidat**.
5. V hello **nové – Direktiva Assembly** dialogové okno, klikněte na tlačítko **položky cesta sestavení Java** a pak klikněte na **Další**.
6. Vyberte hello příslušnou knihovnu a klikněte na **Dokončit**.
7. Klikněte na tlačítko **OK** tooclose hello **vlastnosti pro MyACSHelloWorld** dialogové okno.

## <a name="modify-hello-jsp-file-toodisplay-saml"></a>Upravit toodisplay soubor JSP hello SAML
Upravit **index.jsp** toouse hello následující kód.

    <%@ page language="java" contentType="text/html; charset=UTF-8"
        pageEncoding="UTF-8"%>
        <%@ page import="javax.xml.parsers.*"
                 import="javax.xml.transform.*"
                 import="org.w3c.dom.*"
                 import="java.io.*"
                 import="javax.xml.transform.stream.*"
                 import="javax.xml.transform.dom.*"
                 import="javax.xml.xpath.*"
                 import="javax.servlet.jsp.JspWriter" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Sample ACS Filter</title>
    </head>
    <body>
        <h3>SAML information from sample ACS program</h3>
        <%!
        void displaySAMLInfo(Node node, String parent, JspWriter out)
        {

            try
            {
                String nodeName;
                int nChild, i;

                nodeName = node.getNodeName();
                out.println("<br>");
                out.println("<u>Examining <b>" + parent + nodeName + "</b></u><br>");

                   // Attributes.
                   NamedNodeMap attribsMap = node.getAttributes();
                   if (null != attribsMap)
                   {
                         for (i=0; i < attribsMap.getLength(); i++)
                         {
                                Node attrib = attribsMap.item(i);
                                out.println("Attribute: <b>" + attrib.getNodeName() + "</b>: " + attrib.getNodeValue()  + "<br>");
                         }
                   }

                   // Child nodes.
                   NodeList list = node.getChildNodes();
                   if (null != list)
                    {
                          nChild = list.getLength();
                          if (nChild > 0)
                          {                    

                                 // If it is a text node, just print hello text.
                                 if (list.item(0).getNodeName() == "#text")
                                 {
                                     out.println("Text value: <b>" + list.item(0).getTextContent() + "</b><br>");
                                 }
                                 else
                                 {
                                     // Print out hello child node names.
                                     out.print("Contains " + nChild + " child node(s): ");   
                                        for (i=0; i < nChild; i++)
                                     {
                                        Node temp = list.item(i);

                                        out.print("<b>" + temp.getNodeName() + "</b>");
                                        if (i < nChild - 1)
                                        {
                                            // Separate hello names.
                                            out.print(", ");
                                        }
                                        else
                                        {
                                            // Finish hello sentence.
                                            out.print(".");
                                        }

                                     }
                                     out.println("<br>");

                                     // Process hello child nodes.
                                     for (i=0; i < nChild; i++)
                                     {
                                        Node temp = list.item(i);
                                        displaySAMLInfo(temp, parent + nodeName + "\\", out);
                                     }
                               }
                          }
                      }
                  }
                catch (Exception e)
                {
                    System.out.println("Exception encountered.");
                    e.printStackTrace();            
                }
            }
        %>

        <%
        try
        {
            String data  = (String) request.getAttribute("ACSSAML");

            DocumentBuilder docBuilder;
            Document doc = null;
            DocumentBuilderFactory docBuilderFactory = DocumentBuilderFactory.newInstance();
            docBuilderFactory.setIgnoringElementContentWhitespace(true);
            docBuilder = docBuilderFactory.newDocumentBuilder();
            byte[] xmlDATA = data.getBytes();

            ByteArrayInputStream in = new ByteArrayInputStream(xmlDATA);
            doc = docBuilder.parse(in);
            doc.getDocumentElement().normalize();

            // Iterate hello child nodes of hello doc.
            NodeList list = doc.getChildNodes();

            for (int i=0; i < list.getLength(); i++)
            {
                displaySAMLInfo(list.item(i), "", out);
            }
        }
        catch (Exception e)
        {
            out.println("Exception encountered.");
            e.printStackTrace();
        }

        %>
    </body>
    </html>

## <a name="run-hello-application"></a>Spuštění aplikace hello
1. Spusťte aplikaci v emulátoru hello počítače nebo nasadit tooAzure pomocí hello kroků popsaných v [jak tooAuthenticate webovým uživatelům s Azure přístup k řízení služby pomocí Eclipse](active-directory-java-authenticate-users-access-control-eclipse.md).
2. Spusťte prohlížeč a otevřete webovou aplikaci. Po přihlášení tooyour aplikace se zobrazí informace o SAML, včetně assertion hello zabezpečení poskytované poskytovatelem identity hello.

## <a name="next-steps"></a>Další kroky
toofurther prozkoumat funkce služby ACS a tooexperiment se složitější scénáři najdete v tématu [2.0 služby Řízení přístupu][Access Control Service 2.0].

[Prerequisites]: #pre
[Modify hello JSP file toodisplay SAML]: #modify_jsp
[Add hello JspWriter library tooyour build path and deployment assembly]: #add_library
[Run hello application]: #run_application
[Next steps]: #next_steps
[Access Control Service 2.0]: http://go.microsoft.com/fwlink/?LinkID=212360
[How tooAuthenticate Web Users with Azure Access Control Service Using Eclipse]: active-directory-java-authenticate-users-access-control-eclipse
[saml_output]: ./media/active-directory-java-view-saml-returned-by-access-control/SAML_Output.png
