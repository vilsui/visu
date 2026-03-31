Palvelintenhallinta, ph13 - Palvelinten hallinta ICI001AS3A-3013
h1 / Ville Suikki

Alussa oli todella kovia haasteita saada oikeaa virtuaaliympäristöä pystyyn kun Macbookki on aivan pakasta vedetty 2026v malli, mutta jäätävän työstämisen, hiusten kiskomisen ja tekoälyn kanssakin sparraamisen jälkeen, löysin vasta sellaiset ohjeet että sain ympäristön pystyyn! 

Karvinen 2026: SSH public key - Login without password pääpointit:
  - SSH Avainpari muodostuu julkisesta sekä yksityisen avainparin yhdistelmästä
  - Julkinen avain kopioidaan koneelle jonka myötä salasanaa ei vtarvitse manuaalisti syöttää uudestaan, joka tehdyssä tehtävässä vaadittiin syöttämällä komento sudo systemctl status ssh - Edellytys sujuvammalle automaatiolle kuten tehtävässä käytetty Ansible.
  - Omana huomiona tuota omaa yksityistä lukkoa luodessa tuntui hieman hauskalta että "tietoturvani" parani aivan järkysttävästi kun huomasi kuinka monimutkainen oma yksityuinen avain oli verrattuna esim. omaan salasanaan.


Karvinen 2026: Hello Ansible pääpointit:
  - Ansible toimii IaC-työkaluna. Voi tallentaa versuonhallintaan kuten Git jos sattuisi palvelimien kanssa tulemaan ongelmia. Dokumentaatiota joka samalla suorittavaa työtöä. Ei agentteja jolloin resurssit ja ylläpitokustannukset säästyy ja yksi tietoturvariski vähemmän. 
  - Käyttää YAML-pohjaisia "playbookkeja" tehtävien määritykseen.
  - Omana huomiona termi Idempotenssi; Joo- hieno termi, mutta tarkoittaa että voi ajaa saman kodin monta kertaa ja tekee muutoksia  ain silloin, jos muutoksia tarvitaan.



A) SSH-demonin asennus / testaus
Asensin VirtualBoxDebianin jonka maaliin saamisesssa oli oma hommansa, mutta toivottavasti jatkossa jokainen tulevan tunnin pääsee olemaan paikanpäällä HH'ssa, niin pääsee kysymään heti paikanpäällä. Koska olin ottanut ihan puhtaakn paketin ilman olemassa olevia SSH ja muita konffauksia niin lähdin ajamaan tehtävää lävitse että ensimmäisenä taistelin itselleni SUDO oikeudet jotka sain lisättyä roottikäyttäjän omistusmuutosten jälkeen. Asensin SSH sudo apt install openssh-server, sudo systemctl start ssh, sudo systemctl enable ssh ja viimeisenä ssh localhost jonka jälkeen kirjauduin ulos exit-komennolla. 

B) Julkinen avain / Public Key

Takaisin koneellke kirjauduttuani lähdinn työstämään julkisen avaimen luontia syöttämällä komennon ssh-keygen jonka jälkeen tuli lista erinäisiä turvakysymyksiä ja sain luodun pubkeyn jonka sain kopioitua ssh-copy-id visu@localhost nimisellä komennolla. Tämän jälkeen testaus että homma toimii ja oleellinen asia oli että syötin salasanan vain kerran- jonka jälkeen jos kirjautuminen uudestaan komennolla ssh localhost onnistuu heti- niin silloin tehtävä onnistunut varmasti. Testasin varmuuden vuoksi monena otteena kirjautumalla ulos kokoonaan ja sulkemalla koneen koska sudo käyttäjäasetusten kanssa oli haasteita. 

C) Ansible

Ansible- eli Automaatiotyökalu. Sen avulla voidaan hallita useita palvelimia kerralla SSSH'n kautta ilman että tarvitsee kirjautua jokaiselle erikseen mikäli useampi kone olisi. Kun tehtävää aikaisemman kohdan olin saanut maaliin lähdin vihdoin asentamaan aniblea koneelle syötteellä sudo apt install ansible. Loin ansiblelle oman kansion jotta kaikki omat osat pysyvät omissa kansiossa ja sen jälkeen luomaan inventory tiedostoa ansiblelle samaan kansioon Nanolla. Inventory kertoo ansiblelle mitkä koneet kuuluvat mihinkäkin hallittaviin palvelimiin. Tänne tallensin Ryhmän nimen; local, hallittavan koneen nimen ja ansible_connection=ssh joka kertoi että yhteys otetaan suoraan SSH'lla. 


Ansiblen konffattuani loin playbook-tiedoston mutta ensin loin nanolla hello.yml sisällön, jolla ansiblelle välittyy tieto siitä mitä tehtäviä ajetaan ja milläkin koneilla. Tiedostoon määrittelin nimen jossa määritellään tehtävän kuvaus, hosts, tasks, debug ja msg kohdat jotka määrittivät tulostetettavan viestin. Tämän kun sain tallennettua ajoin komennon ansible-playbook -i hosts hello.yml ja sain onnistuneesti tulosteen "Hei maailma!" 


