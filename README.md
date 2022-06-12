NBD Ćwiczenia 3 – Neo4J
Ćwiczenie należy oddać w formie pliku tekstowego zawierającego ponumerowane zapytania (zgodnie z numeracją zapytań w tekście ćwiczenia) oraz zestawu plików z wynikami w postaci graficznej i tekstowej (np. SVG i JSON – niestety dostępne opcje eksportu regularnie ulegają zmianie) o nazwach wynikX.svg i wynikX.json, gdzie X to numer zapytania (pliki graficzny i tekstowy należy wyeksportować z pomocą webowego interfejsu bazy, rozszerzenia zależą od wybranych formatów) 
W wypadku wykonywania zadań na szkolnych maszynach wirtualnych pobierz nową wersję bazy Neo4j Community Edition spod adresu http://neo4j.com/download/ i zastąp nią obecną wersję zainstalowaną na wirtualce (trzeba usunąć folder neo4j z folderu domowego użytkownika labnbd i na to miejsce rozpakować pobrane archiwum). Serwer uruchamiamy przez /neo4j/bin/neo4j start, interfejs webowy dostępny jest pod adresem http://localhost:7474 
Zaimportuj dane uruchamiając zapytania zgodnie z instrukcjami wyświetlanymi po wpisaniu polecenia :play movie-graph. Przeanalizuj i uruchom przykładowe zapytania.
Zbuduj następujące zapytania
 
1.	Wszystkie filmy
MATCH (m:Movie) RETURN m

2.	Wszystkie filmy, w których grał Hugo Weaving 
MATCH (hugo:Person {name: "Hugo Weaving"})-[:ACTED_IN]->(hugoWeavingMovies) RETURN hugo,hugoWeavingMovies

3.	Reżyserzy filmów, w których grał Hugo Weaving 
MATCH (:Person{name: "Hugo Weaving"}) - [:ACTED_IN] -> (m: Movie) <-[:DIRECTED]- (director: Person) RETURN director

4.	Wszystkie osoby, z którymi Hugo Weaving grał w tych samych filmach 
MATCH (:Person {name:"Hugo Weaving"})-[:ACTED_IN]->(m:Movie)<-[:ACTED_IN]-(coActors: Person) RETURN coActors

5.	Wszystkie filmy osób, które grały w Matrix
MATCH (:Person)-[:ACTED_IN]->(m:Movie {title: "The Matrix"})<-[:ACTED_IN]-(coActors: Person) MATCH (coActors: Person) -[:ACTED_IN]-> (mov: Movie) RETURN mov

6.	Listę aktorów (aktor = osoba, która grała przynajmniej w jednym filmie) wraz z ilością filmów, w których grali
MATCH (actor:Person) -[:ACTED_IN]-> (m: Movie) RETURN actor, COUNT(*)

7.	Listę osób, które napisały scenariusz filmu, które wyreżyserowały wraz z tytułami takich filmów (koniunkcja – ten sam autor scenariusza i reżyser) 
MATCH (mov:Movie)<-[:DIRECTED]-(directors: Person) MATCH (mov:Movie)<-[:WROTE]-(writers: Person) WHERE directors = writers RETURN directors, writers, mov

8.	Listę filmów, w których grał zarówno Hugo Weaving jak i Keanu Reeves
MATCH (:Person{name: "Hugo Weaving"}) -[:ACTED_IN]-> (mov: Movie) MATCH (:Person{name: "Keanu Reeves"}) -[:ACTED_IN]-> (mov: Movie) RETURN mov

9.	(za 0.2pkt) Zestaw zapytań powodujących uzupełnienie bazy danych o film Captain America: The First Avenger wraz z uzupełnieniem informacji o reżyserze, scenarzystach i odtwórcach głównych ról (w oparciu o skrócone informacje z IMDB - http://www.imdb.com/title/tt0458339/) + zapytanie pokazujące dodany do bazy film wraz odtwórcami głównych ról, scenarzystą i reżyserem. Plik SVG ma pokazywać wynik ostatniego zapytania.  

CREATE (captainAmerica :Movie {title:'Captain America: The First Avenge', released:2011, tagline:'When patriots become heroes'}) MERGE (Chris: Person{name: "Chris Evans", born: 1981}) CREATE (Chris) -[:ACTED_IN]-> (captainAmerica) MERGE (Hugo: Person{name: "Hugo Weaving", born: 1960}) CREATE (Hugo) -[:ACTED_IN]-> (captainAmerica) MERGE (Joe: Person{name: "Joe Johnston", born: 1950}) CREATE (Joe) -[:DIRECTED]-> (captainAmerica) MERGE (Christopher: Person{name: "Christopher Markus", born: 1970}) CREATE (Christopher) -[:WROTE]-> (captainAmerica) MERGE (Stephen: Person{name: "Stephen McFeely", born: 1970}) CREATE (Stephen) -[:WROTE]-> (captainAmerica) MERGE (JoeS: Person{name: "Joe Simon", born: 1913}) CREATE (JoeS) -[:WROTE]-> (captainAmerica) RETURN captainAmerica, Chris, Hugo, Joe, Christopher, Stephen, JoeS
Uwaga 1: W wypadku zadania 9 dopuszczalne jest wykorzystanie większej niż 1 ilości zapytań

