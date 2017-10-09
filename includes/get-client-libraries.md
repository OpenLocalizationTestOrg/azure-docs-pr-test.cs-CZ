### <a name="install-via-composer"></a>Nainstalovat prostřednictvím autora
1. [Nainstalovat Git][install-git]. Všimněte si, že v systému Windows, musíte taky přidat proměnné prostředí PATH spustitelné tooyour Git hello. 
2. Vytvořte soubor s názvem **composer.json** v kořenu projektu hello a přidejte následující kód tooit hello:
   
    ```json
    {
      "require": {
        "microsoft/windowsazure": "^0.4"
      }
    }
    ```
3. Stáhněte si  **[composer.phar] [ composer-phar]**  v kořenového adresáře projektu.
4. Otevřete příkazový řádek a spusťte následující příkaz do kořenového adresáře projektu hello
   
    ```
    php composer.phar install
    ```

[php-sdk-github]: http://go.microsoft.com/fwlink/?LinkId=252719
[install-git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[download-SDK-PHP]: ../articles/php-download-sdk.md
[composer-phar]: http://getcomposer.org/composer.phar
