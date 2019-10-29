<h1>WebSocket</h1>
<p>
    Demonstrační aplikace klient / server s použitím technologie WebSocket v prostředí PHP.
</p>

<h2>Hlavní součásti</h2>
<dl>
    <dt>server.bat</dt>
    <dd>- Windows script pro rychlé spuštění serveru na lokálním počítači s nasloucháním na daném portu.</dd>
    <dt>server.php</dt>
    <dd>- Soubor s hlavním serverem, který je spouštěn v režimu příkazového řádku.</dd>
    <dt>client.php</dt>
    <dd>- Soubor s jednoduchou klientskou částí pro zasílání zpráv na server.</dd>
</dl>

<h2>Server</h2>
<h3>Namapování PHP</h3>
<div>
    <p>
        Hlavní server je spouštěn přes příkazový řádek pomocí příkazu "<strong>php -f {cesta_k_souboru}/server.php</strong>" viz. "<strong>server.bat</strong>".<br>
        V běžném případe napíše CMD při použití příkazu "<strong>php</strong>" chybu, že příkaz neexistuje.<br>
        Tento problém je potřeba vyřešit namapováním nové proměnné:
    </p>
    <ol>
        <li>Je potřeba mít na počítači nainstalované prostředí <strong>PHP</strong></li>
        <li>Nainstalujeme užitečný balíček komponent <strong>XAMPP</strong> (<a href="https://apachefriends.org" target="_blank"><strong>Zde ke stažení</strong></a>)</li>
        <li>Klíčové soubory PHP se nachází v domovském adresáři XAMPP ve složce "<strong>php</strong>"</li>
        <li>Provedeme namapování v systému Windows:
            <ul>
                <li>Tento počítač -> Vlastnosti</li>
                <li>Upřesnit nastavení systému -> Proměnné prostředí</li>
                <li>Systémové proměnné -> Path -> Nový</li>
                <li>Hodnota proměnné: "<strong>{cesta_k_adresáři_php}</strong>"</li>
            </ul>
        </li>
        <li>Provedeme kontrolu namapování v CMD pomocí příkazu: "<strong>php -v</strong>"</li>
        <li>Pokusíme se opět spustit server</li>
    </ol>
</div>

<h3>XAMPP a nastavení PHP</h3>
<div>
    <p>
        Při opětovném pokusu o spuštění serveru se pravděpodobně objeví chyba, že Socket nebylo možné vytvořit.<br>
        Tento problém způsobuje fakt, že je ve výchozím PHP nastavení WebSocket bráno jako rozšíření, které je potřeba povolit:    
    </p>
    <ol>
        <li>Přejdeme do domovského adresáře PHP</li>
        <li>Otevřeme soubor "<strong>php.ini</strong>"</li>
        <li>Najdeme řádek: "<strong>;extension=sockets</strong>"</li>
        <li>Odkomentujeme řádek smazáním "<strong>;</strong>"</li>
        <li>Restartujeme Apache v ovládacím panelu XAMPP</li>
        <li>Pokusíme se opět spustit server</li>
    </ol>
    <p>
        Vytvoření koncového bodu by mělo proběhnout úspěšné a server by měl naslouchat novým připojením na portu <strong>20205</strong>.<br>
        Pokud by se objeví další problém, že je daný port již využíván, je potřeba jej změnit v souboru "<strong>server.php</strong>".
    </p>
</div>

<h2>Klient</h2>
<h3>Spuštění klienta</h3>
<p>
    Klietským modulem je soubor: "<strong>client.php</strong>".
    Pro jeho spuštění využijeme balíček XAMPP, který má ve svém domovském adresáři složku "<strong>htdocs</strong>", do které stačí soubor "<strong>client.php</strong>" přesunout.
    Do běžného webového prohlížeče poté stačí zadat adresu: <a href="http://localhost/client.php" target="_blank"><strong>http://localhost/client.php</strong></a> a komunikace mezi klientem a serverem může začít!
</p>

<h2>Princip komunikace</h2>
<p>
    Aktivní server vždy jako první čeká na nové připojení klienta.
    Samovolně v základu nikomu nic nerozesílá ani neprovádí žádné další aktivity na pozadí.
    Klient navazuje spojení se serverem v okamžiku, kdy spustíme / navštívíme adresu <a href="http://localhost/client.php" target="_blank"><strong>http://localhost/client.php</strong></a>.
    Klient začíná komunikaci vždy jako první. Odešle zprávu na server a poté čeká na jeho odpověď.
    V případě, že je k serveru připojeno více klientů a všichni čekají na odpověď, obsluhuje jejich požadavky server postupně podle vlastní fronty čekatelů.
</p>
