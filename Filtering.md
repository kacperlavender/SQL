Sometimes you want to work with every row in a table, such as:
- Purging all data from a table used to stage new data warehouse feeds 
- Modifying all rows in a table after a new column has been added
- Retrieving all rows from a message queue table

In cases like these, your SQL statements won’t need to have a where clause, since you don’t need to exclude any rows from consideration. Most of the time, however, you will want to narrow your focus to a subset of a table’s rows. Therefore, all the SQL data statements (except the insert statement) include an optional where clause con‐ taining one or more filter conditions used to restrict the number of rows acted on by the SQL statement. Additionally, the select statement includes a having clause in which filter conditions pertaining to grouped data may be included.


### (In)Equality conditions
```sql
mysql> select c.email
    -> from customer c
    -> inner join rental r
    -> on c.customer_id = r.customer_id
    -> where date(r.rental_date) = '2005-06-14'; #or <>  
+---------------------------------------+
| email                                 |
+---------------------------------------+
| CATHERINE.CAMPBELL@sakilacustomer.org |
| JOYCE.EDWARDS@sakilacustomer.org      |
...
| TERRANCE.ROUSH@sakilacustomer.org     |
| TERRENCE.GUNDERSON@sakilacustomer.org |
+---------------------------------------+
16 rows in set (0.02 sec)
```


### Range Conditions
```sql
mysql> select customer_id, rental_date
    -> from rental
    -> where rental_date < '2005-05-25';
+-------------+---------------------+
| customer_id | rental_date         |
+-------------+---------------------+
|         130 | 2005-05-24 22:53:30 |
|         459 | 2005-05-24 22:54:33 |
|         408 | 2005-05-24 23:03:39 |
|         333 | 2005-05-24 23:04:41 |
|         222 | 2005-05-24 23:05:21 |
|         549 | 2005-05-24 23:08:07 |
|         269 | 2005-05-24 23:11:53 |
|         239 | 2005-05-24 23:31:46 |
+-------------+---------------------+
8 rows in set (0.00 sec)
```


### The between operator
```sql
mysql> select customer_id, payment_date, amount
    -> from payment
    -> where amount between 10.0 and 11.99;
+-------------+---------------------+--------+
| customer_id | payment_date        | amount |
+-------------+---------------------+--------+
|           2 | 2005-07-30 13:47:43 |  10.99 |
|           3 | 2005-07-27 20:23:12 |  10.99 |
|          12 | 2005-08-01 06:50:26 |  10.99 |
...
|         591 | 2005-07-07 20:45:51 |  11.99 |
|         592 | 2005-07-06 22:58:31 |  11.99 |
|         595 | 2005-07-31 11:51:46 |  10.99 |
+-------------+---------------------+--------+
114 rows in set (0.01 sec)
```
_Make sure that you specify the lower amount first._

### String ranges
While ranges of dates and numbers are easy to understand, you can also build condi‐ tions that search for ranges of strings, which are a bit harder to visualize. Say, for example, you are searching for customers whose last name falls within a range.
```sql
mysql> select last_name, first_name
    -> from customer
    -> where last_name between 'FA' and 'FR';
+------------+------------+
| last_name  | first_name |
+------------+------------+
| FARNSWORTH | JOHN       |
| FENNELL    | ALEXANDER  |
| FERGUSON   | BERTHA     |
| FERNANDEZ  | MELINDA    |
...
| FOWLER     | JO         |
| FOX        | HOLLY      |
+------------+------------+
18 rows in set (0.00 sec)
```


```sql
mysql> select title, rating
    -> from film
    -> where rating = "G" or rating = "PG";
+---------------------------+--------+
| title                     | rating |
+---------------------------+--------+
| ACADEMY DINOSAUR          | PG     |
| ACE GOLDFINGER            | G      |
| AFFAIR PREJUDICE          | G      |
| AFRICAN EGG               | G      |
| AGENT TRUMAN              | PG     |
| ALAMO VIDEOTAPE           | G      |
| ALASKA PHANTOM            | PG     |
| ALI FOREVER               | PG     |
| AMADEUS HOLY              | PG     |
| AMISTAD MIDSUMMER         | G      |
| ANGELS LIFE               | G      |
| ANNIE IDENTITY            | G      |
| ARIZONA BANG              | PG     |
| ARMAGEDDON LOST           | G      |
| ARSENIC INDEPENDENCE      | PG     |
| ATLANTIS CAUSE            | G      |
| AUTUMN CROW               | G      |
| BAKED CLEOPATRA           | G      |
| BALLROOM MOCKINGBIRD      | G      |
| BARBARELLA STREETCAR      | G      |
| BAREFOOT MANCHURIAN       | G      |
| BEACH HEARTBREAKERS       | G      |
| BEAUTY GREASE             | G      |
| BEDAZZLED MARRIED         | PG     |
| BEHAVIOR RUNAWAY          | PG     |
| BILL OTHERS               | PG     |
| BIRCH ANTITRUST           | PG     |
| BIRD INDEPENDENCE         | G      |
| BIRDS PERDITION           | G      |
| BLACKOUT PRIVATE          | PG     |
| BLANKET BEVERLY           | G      |
| BLOOD ARGONAUTS           | G      |
| BLUES INSTINCT            | G      |
| BOILED DARES              | PG     |
| BONNIE HOLOCAUST          | G      |
| BORN SPINAL               | PG     |
| BORROWERS BEDAZZLED       | G      |
| BOUND CHEAPER             | PG     |
| BRANNIGAN SUNRISE         | PG     |
| BREAKFAST GOLDFINGER      | G      |
| BRIDE INTRIGUE            | G      |
| BRINGING HYSTERICAL       | PG     |
| BUCKET BROTHERHOOD        | PG     |
| BUGSY SONG                | G      |
| BULWORTH COMMANDMENTS     | G      |
| BUNCH MINDS               | G      |
| BUTTERFLY CHOCOLAT        | G      |
| CAPER MOTIONS             | G      |
| CAROL TEXAS               | PG     |
| CARRIE BUNCH              | PG     |
| CASABLANCA SUPER          | G      |
| CASUALTIES ENCINO         | G      |
| CAT CONEHEADS             | G      |
| CATCH AMISTAD             | G      |
| CENTER DINOSAUR           | PG     |
| CHAINSAW UPTOWN           | PG     |
| CHAMPION FLATLINERS       | PG     |
| CHARADE DUFFEL            | PG     |
| CHASING FIGHT             | PG     |
| CHEAPER CLYDE             | G      |
| CHICKEN HELLFIGHTERS      | PG     |
| CHILL LUCK                | PG     |
| CHINATOWN GLADIATOR       | PG     |
| CHISUM BEHAVIOR           | G      |
| CHITTY LOCK               | G      |
| CIDER DESIRE              | PG     |
| CITIZEN SHREK             | G      |
| CLASH FREDDY              | G      |
| CLERKS ANGELS             | G      |
| COAST RAINBOW             | PG     |
| COLDBLOODED DARLING       | G      |
| COLOR PHILADELPHIA        | G      |
| CONNECTION MICROCOSMOS    | G      |
| CONQUERER NUTS            | G      |
| CONTROL ANTHEM            | G      |
| COWBOY DOOM               | PG     |
| CRAZY HOME                | PG     |
| CROSSROADS CASUALTIES     | G      |
| CROW GREASE               | PG     |
| CRUELTY UNFORGIVEN        | G      |
| CYCLONE FAMILY            | PG     |
| DADDY PITTSBURGH          | G      |
| DAISY MENAGERIE           | G      |
| DALMATIONS SWEDEN         | PG     |
| DANCING FEVER             | G      |
| DANGEROUS UPTOWN          | PG     |
| DARN FORRESTER            | G      |
| DAWN POND                 | PG     |
| DAY UNFAITHFUL            | G      |
| DAZED PUNK                | G      |
| DESPERATE TRAINSPOTTING   | G      |
| DESTINY SATURDAY          | G      |
| DIARY PANIC               | G      |
| DISCIPLE MOTHER           | PG     |
| DIVORCE SHINING           | G      |
| DOCTOR GRAIL              | G      |
| DOGMA FAMILY              | G      |
| DOWNHILL ENOUGH           | G      |
| DRACULA CRYSTAL           | G      |
| DREAM PICKUP              | PG     |
| DRUMLINE CYCLONE          | G      |
| DRUMS DYNAMITE            | PG     |
| DUDE BLINDNESS            | G      |
| DUFFEL APOCALYPSE         | G      |
| DWARFS ALTER              | G      |
| DYING MAKER               | PG     |
| EASY GLADIATOR            | G      |
| EFFECT GLADIATOR          | PG     |
| EGG IGBY                  | PG     |
| EGYPT TENENBAUMS          | PG     |
| EMPIRE MALKOVICH          | G      |
| ENCINO ELF                | G      |
| EVE RESURRECTION          | G      |
| EVERYONE CRAFT            | PG     |
| EXCITEMENT EVE            | G      |
| EXORCIST STING            | G      |
| EXPENDABLE STALLION       | PG     |
| EXTRAORDINARY CONQUERER   | G      |
| FANTASIA PARK             | G      |
| FARGO GANDHI              | G      |
| FATAL HAUNTED             | PG     |
| FERRIS MOTHER             | PG     |
| FICTION CHRISTMAS         | PG     |
| FIDELITY DEVIL            | G      |
| FIREBALL PHILADELPHIA     | PG     |
| FIREHOUSE VIETNAM         | G      |
| FLATLINERS KILLER         | G      |
| FOOL MOCKINGBIRD          | PG     |
| FRENCH HOLIDAY            | PG     |
| FRISCO FORREST            | PG     |
| FROST HEAD                | PG     |
| FULL FLATLINERS           | PG     |
| GABLES METROPOLIS         | PG     |
| GARDEN ISLAND             | G      |
| GASLIGHT CRUSADE          | PG     |
| GHOST GROUNDHOG           | G      |
| GILBERT PELICAN           | G      |
| GLADIATOR WESTWARD        | PG     |
| GLASS DYING               | G      |
| GOLDFINGER SENSIBILITY    | G      |
| GOODFELLAS SALUTE         | PG     |
| GOSFORD DONNIE            | G      |
| GRADUATE LORD             | G      |
| GRAFFITI LOVE             | PG     |
| GRAPES FURY               | G      |
| GREASE YOUTH              | G      |
| GREEK EVERYONE            | PG     |
| GRIT CLOCKWORK            | PG     |
| GUN BONNIE                | G      |
| HANGING DEEP              | G      |
| HAPPINESS UNITED          | G      |
| HARPER DYING              | G      |
| HATE HANDICAP             | PG     |
| HEARTBREAKERS BRIGHT      | G      |
| HEAVEN FREEDOM            | PG     |
| HEAVYWEIGHTS BEAST        | G      |
| HELLFIGHTERS SIERRA       | PG     |
| HILLS NEIGHBORS           | G      |
| HOCUS FRIDA               | G      |
| HOLES BRANNIGAN           | PG     |
| HOLLYWOOD ANONYMOUS       | PG     |
| HOOK CHARIOTS             | G      |
| HOOSIERS BIRDCAGE         | G      |
| HORN WORKING              | PG     |
| HUNGER ROOF               | G      |
| HURRICANE AFFAIR          | PG     |
| HYDE DOCTOR               | G      |
| HYSTERICAL GRAIL          | PG     |
| INSTINCT AIRPORT          | PG     |
| INTRIGUE WORST            | G      |
| INVASION CYCLONE          | PG     |
| IRON MOON                 | PG     |
| ITALIAN AFRICAN           | G      |
| JAPANESE RUN              | G      |
| JAWBREAKER BROOKLYN       | PG     |
| JAWS HARRY                | G      |
| JEDI BENEATH              | PG     |
| JEKYLL FROGMEN            | PG     |
| JERSEY SASSY              | PG     |
| JUMANJI BLADE             | G      |
| KENTUCKIAN GIANT          | PG     |
| KILL BROTHERHOOD          | G      |
| LADY STAGE                | PG     |
| LAWLESS VISION            | G      |
| LEGALLY SECRETARY         | PG     |
| LEGEND JEDI               | PG     |
| LIAISONS SWEET            | PG     |
| LIBERTY MAGNIFICENT       | G      |
| LION UNCUT                | PG     |
| LOLA AGENT                | PG     |
| LONELY ELEPHANT           | G      |
| LOSER HUSTLER             | PG     |
| LOVELY JINGLE             | PG     |
| LOVER TRUMAN              | G      |
| LUST LOCK                 | G      |
| MAGIC MALLRATS            | PG     |
| MAIDEN HOME               | PG     |
| MAJESTIC FLOATS           | PG     |
| MALKOVICH PET             | G      |
| MALLRATS UNITED           | PG     |
| MANCHURIAN CURTAIN        | PG     |
| MARRIED GO                | G      |
| MASSAGE IMAGE             | PG     |
| MEET CHOCOLATE            | G      |
| MENAGERIE RUSHMORE        | G      |
| MIDNIGHT WESTWARD         | G      |
| MIDSUMMER GROUNDHOG       | G      |
| MIGHTY LUCK               | PG     |
| MILE MULAN                | PG     |
| MINORITY KISS             | G      |
| MOB DUFFEL                | G      |
| MOCKINGBIRD HOLLYWOOD     | PG     |
| MODERN DORADO             | PG     |
| MONEY HAROLD              | PG     |
| MONSOON CAUSE             | PG     |
| MONSTER SPARTACUS         | PG     |
| MONTEREY LABYRINTH        | G      |
| MOON BUNCH                | PG     |
| MOONWALKER FOOL           | G      |
| MOSQUITO ARMAGEDDON       | G      |
| MOTIONS DETAILS           | PG     |
| MOURNING PURPLE           | PG     |
| MOVIE SHAKESPEARE         | PG     |
| MULAN MOON                | G      |
| MULHOLLAND BEAST          | PG     |
| MUPPET MILE               | PG     |
| MURDER ANTITRUST          | PG     |
| MUSCLE BRIGHT             | G      |
| MUSKETEERS WAIT           | PG     |
| MUSSOLINI SPOILERS        | G      |
| NECKLACE OUTBREAK         | PG     |
| NEWSIES STORY             | G      |
| NEWTON LABYRINTH          | PG     |
| NIGHTMARE CHILL           | PG     |
| NOON PAPI                 | G      |
| NORTHWEST POLISH          | PG     |
| NOVOCAINE FLIGHT          | G      |
| OKLAHOMA JUMANJI          | PG     |
| OLEANDER CLUE             | PG     |
| OPEN AFRICAN              | PG     |
| OPERATION OPERATION       | G      |
| OPPOSITE NECKLACE         | PG     |
| OSCAR GOLD                | PG     |
| OTHERS SOUP               | PG     |
| PACIFIC AMISTAD           | G      |
| PANIC CLUB                | G      |
| PANKY SUBMARINE           | G      |
| PAPI NECKLACE             | PG     |
| PARTY KNOCK               | PG     |
| PATHS CONTROL             | PG     |
| PATRIOT ROMAN             | PG     |
| PATTON INTERVIEW          | PG     |
| PEAK FOREVER              | PG     |
| PELICAN COMFORTS          | PG     |
| PET HAUNTING              | PG     |
| PICKUP DRIVING            | G      |
| PILOT HOOSIERS            | PG     |
| PINOCCHIO SIMON           | PG     |
| PIRATES ROXANNE           | PG     |
| POLISH BROOKLYN           | PG     |
| POLLOCK DELIVERANCE       | PG     |
| POTLUCK MIXED             | G      |
| POTTER CONNECTICUT        | PG     |
| PRESIDENT BANG            | PG     |
| PRIMARY GLASS             | G      |
| PRIVATE DROP              | PG     |
| PULP BEVERLY              | G      |
| PUNK DIVORCE              | PG     |
| QUEEN LUKE                | PG     |
| RAINBOW SHOCK             | PG     |
| RANGE MOONWALKER          | PG     |
| REBEL AIRPORT             | G      |
| RECORDS ZORRO             | PG     |
| RESURRECTION SILVERADO    | PG     |
| RIDER CADDYSHACK          | PG     |
| RINGS HEARTBREAKERS       | G      |
| ROCK INSTINCT             | G      |
| ROOM ROMAN                | PG     |
| RUSH GOODFELLAS           | PG     |
| SABRINA MIDNIGHT          | PG     |
| SAGEBRUSH CLUELESS        | G      |
| SAINTS BRIDE              | G      |
| SAMURAI LION              | G      |
| SANTA PARIS               | PG     |
| SASSY PACKER              | G      |
| SATISFACTION CONFIDENTIAL | G      |
| SATURDAY LAMBS            | G      |
| SCISSORHANDS SLUMS        | G      |
| SEA VIRGIN                | PG     |
| SECRET GROUNDHOG          | PG     |
| SECRETARY ROUGE           | PG     |
| SECRETS PARADISE          | G      |
| SENSIBILITY REAR          | PG     |
| SHANE DARKNESS            | PG     |
| SHANGHAI TYCOON           | PG     |
| SHAWSHANK BUBBLE          | PG     |
| SHINING ROSES             | G      |
| SIDE ARK                  | G      |
| SILVERADO GOLDFINGER      | PG     |
| SKY MIRACLE               | PG     |
| SLEEPLESS MONSOON         | G      |
| SLEEPY JAPANESE           | PG     |
| SLUMS DUCK                | PG     |
| SMILE EARRING             | G      |
| SNATCH SLIPPER            | PG     |
| SNOWMAN ROLLERCOASTER     | G      |
| SPIKING ELEMENT           | G      |
| SPLASH GUMP               | PG     |
| SPOILERS HELLFIGHTERS     | G      |
| SQUAD FISH                | PG     |
| STAGE WORLD               | PG     |
| STAR OPERATION            | PG     |
| STEERS ARMAGEDDON         | PG     |
| STOCK GLASS               | PG     |
| STONE FIRE                | G      |
| STRANGER STRANGERS        | G      |
| SUGAR WONKA               | PG     |
| SUICIDES SILENCE          | G      |
| SUMMER SCARFACE           | G      |
| SUPER WYOMING             | PG     |
| SUPERFLY TRIP             | PG     |
| SUSPECTS QUILLS           | PG     |
| SWEDEN SHINING            | PG     |
| SWEETHEARTS SUSPECTS      | G      |
| TADPOLE PARK              | PG     |
| TALENTED HOMICIDE         | PG     |
| TEEN APOLLO               | G      |
| TELEGRAPH VOYAGE          | PG     |
| TEMPLE ATTRACTION         | PG     |
| TEQUILA PAST              | PG     |
| TIMBERLAND SKY            | G      |
| TITANS JERK               | PG     |
| TOMATOES HELLFIGHTERS     | PG     |
| TOOTSIE PILOT             | PG     |
| TORQUE BOUND              | G      |
| TRACY CIDER               | G      |
| TRADING PINOCCHIO         | PG     |
| TRAFFIC HOBBIT            | G      |
| TRAMP OTHERS              | PG     |
| TRAP GUYS                 | G      |
| TREATMENT JEKYLL          | PG     |
| TROJAN TOMORROW           | PG     |
| TROUBLE DATE              | PG     |
| TRUMAN CRAZY              | G      |
| TURN STAR                 | G      |
| TWISTED PIRATES           | PG     |
| TYCOON GATHERING          | G      |
| UNBREAKABLE KARATE        | G      |
| UNFORGIVEN ZOOLANDER      | PG     |
| UPTOWN YOUNG              | PG     |
| VALLEY PACKER             | G      |
| VOLUME HOUSE              | PG     |
| WAGON JAWS                | PG     |
| WAKE JAWS                 | G      |
| WALLS ARTIST              | PG     |
| WAR NOTTING               | G      |
| WARDROBE PHANTOM          | G      |
| WARLOCK WEREWOLF          | G      |
| WARS PLUTO                | G      |
| WASTELAND DIVINE          | PG     |
| WATCH TRACY               | PG     |
| WATERFRONT DELIVERANCE    | G      |
| WATERSHIP FRONTIER        | G      |
| WEDDING APOLLO            | PG     |
| WEREWOLF LOLA             | G      |
| WEST LION                 | G      |
| WIZARD COLDBLOODED        | PG     |
| WON DARES                 | PG     |
| WONDERLAND CHRISTMAS      | PG     |
| WORDS HUNTER              | PG     |
| WORST BANGER              | PG     |
| YOUNG LANGUAGE            | G      |
+---------------------------+--------+
372 rows in set (0.00 sec)

#OR 

mysql> select title, rating
    -> from film
    -> where rating in ('G', 'PG');
+---------------------------+--------+
| title                     | rating |
+---------------------------+--------+
| ACADEMY DINOSAUR          | PG     |
| ACE GOLDFINGER            | G      |
| AFFAIR PREJUDICE          | G      |
...
| WORDS HUNTER              | PG     |
| WORST BANGER              | PG     |
| YOUNG LANGUAGE            | G      |
+---------------------------+--------+
372 rows in set (0.00 sec)
```


### Using subqueries
