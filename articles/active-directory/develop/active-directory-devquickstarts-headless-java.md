---
title: "aaaAzure AD Java příkazového řádku Začínáme | Microsoft Docs"
description: "Jak toobuild Java příkazového řádku aplikace, která podepisuje uživatele v tooaccess rozhraní API."
services: active-directory
documentationcenter: java
author: navyasric
manager: mbaldwin
editor: 
ms.assetid: 51e1a8f9-6ff0-4643-a350-0ba794e26fd1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 01/23/2017
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 9ba1d1e794928a39ca1f091bd0e6eba57ce3d6aa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-java-command-line-app-tooaccess-an-api-with-azure-ad"></a>Aplikace příkazového řádku v jazyce Java tooAccess k rozhraní API pomocí Azure AD
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Díky Azure AD je jednoduchá a přímočará toooutsource Správa identit vaší webové aplikace, poskytuje jednotné přihlašování a odhlašování jenom pár řádků kódu.  V jazyce Java webové aplikace můžete to provést pomocí implementace hello komunitou vytvářený ADAL4J společnosti Microsoft.

  Zde použijeme ADAL4J na:

* Uživatel hello přihlášení do aplikace hello pomocí služby Azure AD jako zprostředkovatele identity hello.
* Zobrazí některé informace o uživateli hello.
* Přihlašovací hello uživatele mimo aplikaci hello.

V pořadí toodo to, budete muset:

1. Zaregistrovat aplikaci s Azure AD
2. Nastavte své aplikace toouse hello ADAL4J knihovny.
3. Použijte hello ADAL4J knihovny tooissue přihlášení a odhlášení požadavky tooAzure AD.
4. Data o uživateli hello vytiskněte.

spuštění, tooget [stáhnout kostru aplikace hello](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/skeleton.zip) nebo [stažení ukázky hello Dokončit](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect\\/archive/complete.zip).  Musíte také klienta služby Azure AD, ve které tooregister vaší aplikace.  Pokud již, nemáte [zjistěte, jak tooget jeden](active-directory-howto-tenant.md).

## <a name="1--register-an-application-with-azure-ad"></a>1.  Zaregistrovat aplikaci s Azure AD
tooenable tooauthenticate uživatelům aplikace, je nutné nejdříve tooregister novou aplikaci v klientovi.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Na horním panelu hello, klikněte na tlačítko na vašem účtu a v části hello **Directory** vyberte klienta hello služby Active Directory, kde chcete tooregister vaší aplikace.
3. Klikněte na **více služeb** v hello navigaci vlevo a zvolte **Azure Active Directory**.
4. Klikněte na **registrace aplikace** a zvolte **přidat**.
5. Postupujte podle pokynů hello a vytvořte novou **webové aplikace nebo WebAPI**.
  * Hello **název** z hello aplikace popíše aplikaci tooend uživatelé
  * Hello **přihlašovací adresa URL** je hello základní adresu URL aplikace.  kostru Hello výchozí hodnota je `http://localhost:8080/adal4jsample/`.
6. Po dokončení registrace AAD přiřadí vaší aplikace, jedinečné ID aplikace  Tuto hodnotu budete potřebovat v dalších částech hello, takže zkopírujte jej z karty aplikace hello.
7. Z hello **nastavení** -> **vlastnosti** stránky pro aplikace, aktualizujte hello identifikátor ID URI aplikace. Hello **identifikátor ID URI aplikace** je jedinečný identifikátor pro vaši aplikaci.  konvence Hello je toouse `https://<tenant-domain>/<app-name>`, například `http://localhost:8080/adal4jsample/`.

Po vytvoření hello portálu pro vaši aplikaci **klíč** z hello **nastavení** stránky pro aplikace a zkopírujte ho dolů.  Budete ho potřebovat za chvíli.

## <a name="2-set-up-your-app-toouse-adal4j-library-and-prerequisites-using-maven"></a>2. Nastavení aplikace toouse ADAL4J knihovny a požadavky pomocí nástroje Maven
Zde nakonfigurujeme ADAL4J toouse hello ověřovacího protokolu OpenID Connect.  ADAL4J bude použité tooissue požadavků na přihlášení a odhlášení, spravovat hello uživatelské relace a získat informace o uživateli hello, mimo jiné.

* V kořenovém adresáři hello projektu, otevírání nebo vytváření `pom.xml` a vyhledejte hello `// TODO: provide dependencies for Maven` a nahraďte hello následující:

```Java
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>public-client-adal4j-sample</artifactId>
    <packaging>jar</packaging>
    <version>0.0.1-SNAPSHOT</version>
    <name>public-client-adal4j-sample</name>
    <url>http://maven.apache.org</url>
    <properties>
        <spring.version>3.0.5.RELEASE</spring.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>adal4j</artifactId>
            <version>1.1.2</version>
        </dependency>
        <dependency>
            <groupId>com.nimbusds</groupId>
            <artifactId>oauth2-oidc-sdk</artifactId>
            <version>4.5</version>
        </dependency>
        <dependency>
            <groupId>org.json</groupId>
            <artifactId>json</artifactId>
            <version>20090211</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>3.0.1</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
            <version>1.7.5</version>
        </dependency>
</dependencies>
    <build>
        <finalName>public-client-adal4j-sample</finalName>
        <plugins>
                <plugin>
            <groupId>org.codehaus.mojo</groupId>
            <artifactId>exec-maven-plugin</artifactId>
            <version>1.2.1</version>
            <configuration>
                <mainClass>PublicClient</mainClass>
            </configuration>
        </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <configuration>
                    <source>1.7</source>
                    <target>1.7</target>
                    <encoding>UTF-8</encoding>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-dependency-plugin</artifactId>
                <executions>
                    <execution>
                        <id>install</id>
                        <phase>install</phase>
                        <goals>
                            <goal>sources</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-resources-plugin</artifactId>
                <version>2.5</version>
                <configuration>
                    <encoding>UTF-8</encoding>
                </configuration>
            </plugin>
            <plugin>
        <artifactId>maven-assembly-plugin</artifactId>
        <executions>
          <execution>
            <phase>package</phase>
            <goals>
              <goal>single</goal>
            </goals>
          </execution>
        </executions>
        <configuration>
          <descriptorRefs>
            <descriptorRef>jar-with-dependencies</descriptorRef>
          </descriptorRefs>
        </configuration>
      </plugin>
      <plugin>
  <groupId>org.apache.maven.plugins</groupId>
  <artifactId>maven-assembly-plugin</artifactId>
  <configuration>
    <archive>
      <manifest>
        <mainClass>PublicClient</mainClass>
      </manifest>
    </archive>
  </configuration>
</plugin>
        </plugins>
    </build>

</project>


```



## <a name="3-create-hello-java-publicclient-file"></a>3. Vytvoření souboru Java PublicClient hello
Jak je uvedeno výše, použijeme hello rozhraní Graph API tooget data o hello přihlášeného uživatele. Pro tento toobe snadno nám jsme měli vytvořit i soubor toorepresent **objektu adresáře** a jednotlivých souborů toorepresent hello **uživatele** tak, aby hello ú vzor Java lze použít.

* Vytvořte soubor s názvem `DirectoryObject.java` který použijeme toostore základní data o žádné DirectoryObject (můžete být volné toouse to později pro jiné dotazy grafu, může provádět). Vám může vyjímání a vkládání toto z následujícího seznamu:

```Java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;

import javax.naming.ServiceUnavailableException;

import com.microsoft.aad.adal4j.AuthenticationContext;
import com.microsoft.aad.adal4j.AuthenticationResult;

public class PublicClient {

    private final static String AUTHORITY = "https://login.microsoftonline.com/common/";
    private final static String CLIENT_ID = "2a4da06c-ff07-410d-af8a-542a512f5092";

    public static void main(String args[]) throws Exception {

        try (BufferedReader br = new BufferedReader(new InputStreamReader(
                System.in))) {
            System.out.print("Enter username: ");
            String username = br.readLine();
            System.out.print("Enter password: ");
            String password = br.readLine();

            AuthenticationResult result = getAccessTokenFromUserCredentials(
                    username, password);
            System.out.println("Access Token - " + result.getAccessToken());
            System.out.println("Refresh Token - " + result.getRefreshToken());
            System.out.println("ID Token - " + result.getIdToken());
        }
    }

    private static AuthenticationResult getAccessTokenFromUserCredentials(
            String username, String password) throws Exception {
        AuthenticationContext context = null;
        AuthenticationResult result = null;
        ExecutorService service = null;
        try {
            service = Executors.newFixedThreadPool(1);
            context = new AuthenticationContext(AUTHORITY, false, service);
            Future<AuthenticationResult> future = context.acquireToken(
                    "https://graph.windows.net", CLIENT_ID, username, password,
                    null);
            result = future.get();
        } finally {
            service.shutdown();
        }

        if (result == null) {
            throw new ServiceUnavailableException(
                    "authentication result was null");
        }
        return result;
    }
}


```


## <a name="compile-and-run-hello-sample"></a>Zkompilování a spuštění ukázkových hello
Změňte zpět na tooyour kořenového adresáře a spusťte následující příkaz toobuild hello ukázka můžete zadat pouze společně s použitím hello `maven`. Tímto dojde k použití hello `pom.xml` soubor jste napsali závislosti.

`$ mvn package`

Teď byste měli mít `adal4jsample.war` ve vaší `/targets` adresáře. Může nasazení, v kontejneru vaší Tomcat a navštivte adresu URL hello 

`http://localhost:8080/adal4jsample/`

> [!NOTE]
> Je velmi snadné toodeploy WAR s nejnovější serverů Tomcat hello. Jednoduše přejděte příliš`http://localhost:8080/manager/` a postupujte podle pokynů hello na odesílání vašeho '' adal4jsample.war' souboru. Zruší autodeploy pro vás správný koncový bod hello.
> 
> 

## <a name="next-steps"></a>Další kroky
Blahopřejeme! Teď máte funkční aplikaci Java, která má hello možnost tooauthenticate uživatelé, bezpečně volání webového rozhraní API pomocí OAuth 2.0 a získat základní informace o uživateli hello.  Pokud jste to ještě neudělali, nyní je čas toopopulate hello vašeho klienta s některými uživateli.

Pro referenci hello dokončit ukázka (bez vašich hodnot nastavení) [je k dispozici jako ZIP zde](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/complete.zip), nebo ji můžete klonovat z Githubu:

```git clone --branch complete https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect.git```

