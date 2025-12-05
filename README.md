# Ouman EH-800 Home Assistant templates

Tästä löydät valmiit template sensorit Home Assistanttiin Ouman EH-800 lämmönsäätimelle.
Anturit ja säätimet on jaettu kolmeen erilliseen tiedostoon.
  - `ouman.yaml`: sisältää shell komennot ja anturit joilla tiedot haetaan säätimeltä sekä L1 -piirin ja ulkolämpötilan anturit/säätimet.
  - `ouman_l2.yaml`: sisältää L2 -piirin anturit/säätimet, oletuksena kaikki kommentoitu pois käytöstä
  - `ouman_relay.yaml`: sisältää releen ohjaukseen liittyät anturit, oletuksena kaikki kommentoitu pois käytöstä
  - `ouman_measurements.yaml`: sisältää kaikki lisämittauksiin liittyvät anturit/säätimet (pois lukien huonelämpö), oletuksena kaikki kommentoitu pois käytöstä

Tiedot haetaan säätimeltä oletuksena minuutin välein, poislukien hälytysanturi joka haetaan kahden minuutin välein.  
Osa säätimimistä on tehty niin että ovat `unavailable` kunnes sitä oikeasti pystyy säätämään, esim. venttiilin käsisäädön -säädin tulee käytettäväksi vasta kun ohjaustapa on muutettu käsisäädölle.


## Käyttöönotto
1. Kopioi `packages` hakemisto Home Assistantin `config` kansioon eli samaan kansioon jossa on `configuration.yaml`.
   Mikäli käytössäsi on vain L1 piiri tarvitset vain `ouman.yaml` tiedoston.
2. Muokkaa `ouman.yaml` tiedoston ensimmäisiä rivejä:
   - Oumanin käyttäjätunnus ja salasana niiden paikoille
   - Oumanin IP-osoite kaikkeen neljään kohtaan
3. Lisää Home Assistantin `configuration.yaml` tiedostoon `homeassistant:` -kohdan alle kahdella välilyönnillä sisennettynä seuraava rivi: `packages: !include_dir_named packages`  
   Lopputuloksen pitäisi näyttää tältä:
   ```yaml
   homeassistant:
     packages: !include_dir_named packages
   ```

4. Oletuksena käytössä on seuraavat anturit ja säätimet:
   - L1 toteutunut
   - L1 tavoite
   - L1 käyrä
   - L1 venttiilin asento
   - L1 hienosäädön vaikutus menovesi
   - L1 hienosäätö menovesi
   - ulkolämpötila

   Mikäli nämä riittää niin tiedostoihin ei tarvitse koskea enempää.
   
   Mikäli tarvitse muita antureita/säätimiä niin käy läpi `packages` -kansion tiedostot ja ota sieltä kommentit poistamalla käyttöön tarvitsemasi.  
   Käyttöönotto tapahtuu poistamalla aina yhden "ryppään" kaikkien rivien edestä kommentointimerkki (#) sekä yksi välilyönti ( ) eli joka rivin alusta pois `# `.  
   Yksi rypäs tarkoittaa peräkkäin olevia rivejä ilman tyhjiä rivejä.  
   Sisennykset ovat tärkeitä, niiden täytyy olla oikein että kaikki toimii.  
   `name:` kohdasta voit nimetä sensorin/säätimen haluamallasi tavalla tarvittaessa.
   
   Tässä vielä esimerkiksi yksi "rypäs" L2 tiedostosta, tässä vielä valittavana kaksi eri nimeä anturille:
   ```yaml
       ## Mikäli sinulla on lattialämmitys käytössä L2 piirissä niin tämä anturi on lattailämmityksen ennakoinnin hidastus vaikutus, ota jompikumpi nimi käyttöön
           # - name: Ouman L2 ulkolämpötilan hidastus vaikutus
           # - name: Ouman L2 lattialämmityksen ennakoinnin vaikutus
           # unique_id: S_292_85
           # device_class: temperature
           # unit_of_measurement: "°C"
           # state: "{{ state_attr('sensor.ouman_vastaus', 'data').split('292_85=')[1].split(';')[0] }}"
           # availability: "{{ state_attr('sensor.ouman_vastaus', 'error') == false }}"
   ```
   Muokataan se tähän muotoon:
   ```yaml
       ## Mikäli sinulla on lattialämmitys käytössä L2 piirissä niin tämä anturi on lattialämmityksen ennakoinnin hidastus vaikutus, ota jompikumpi nimi käyttöön
           - name: Ouman L2 ulkolämpötilan hidastus vaikutus
           # - name: Ouman L2 lattialämmityksen ennakoinnin vaikutus
           unique_id: S_292_85
           device_class: temperature
           unit_of_measurement: "°C"
           state: "{{ state_attr('sensor.ouman_vastaus', 'data').split('292_85=')[1].split(';')[0] }}"
           availability: "{{ state_attr('sensor.ouman_vastaus', 'error') == false }}"
   ```


5. Käynnistä Home Assistant uudestaan. Sen jälkeen antureiden/säätimien pitäisi tulla näkyviin.


## Lista antureista ja säätimistä
Tässä listattuna kaikki anturit ja säätimet jotka tiedostoista löytyvät.

### L1:
- anturit:
  - L1 toteutunut
  - L1 tavoite
  - L1 käyrä
  - L1 venttiilin asento
  - L1 hienosäädön vaikutus menovesi
  - L1 huonelämpötila
  - L1 huonelämpötila hidastettu
  - L1 huonekompensoinnin vaikutus
  - L1 laskennallinen huoneasetusarvo
  - L1 hienosäädön vaikutus huone
  - L1 huonekompensoinnin aikakorjaus
  - L1 ulkolämpötilan hidastus vaikutus
  - L1 lattialämmityksen ennakoinnin vaikutus
- säätimet:
  - L1 kotona/poissa kytkin
  - L1 hienosäätö menovesi
  - L1 hienosäätö huonelämpö
  - L1 lämmönpudotus menovesi
  - L1 suuri lämmönpudotus menovesi
  - L1 lämmönpudotus huonelämpö
  - L1 suuri lämmönpudotus huonelämpö
  - L1 huonelämpö asetus
  - L1 vakiolämpötilasäädin asetus
  - L1 3-piste käyrän 1/3
  - L1 3-piste käyrän 2/3
  - L1 3-piste käyrän 3/3
  - L1 5-piste käyrän 1/5
  - L1 5-piste käyrän 2/5
  - L1 5-piste käyrän 3/5
  - L1 5-piste käyrän 4/5
  - L1 5-piste käyrän 5/5
  - L1 5-piste käyrän asetuspiste 1/3
  - L1 5-piste käyrän asetuspiste 2/3
  - L1 5-piste käyrän asetuspiste 3/3
  - L1 käsiajo
  - L1 ohjaustapa

### L2:
- anturit:
  - L2 toteutunut
  - L2 tavoite
  - L2 käyrä
  - L2 venttiilin asento
  - L2 hienosäädön vaikutus menovesi
  - L2 huonelämpötila
  - L2 huonelämpötila hidastettu
  - L2 huonekompensoinnin vaikutus
  - L2 laskennallinen huoneasetusarvo
  - L2 hienosäädön vaikutus huone
  - L2 huonekompensoinnin aikakorjaus
  - L2 ulkolämpötilan hidastus vaikutus
  - L2 lattialämmityksen ennakoinnin vaikutus
- säätimet:
  - L2 kotona/poissa kytkin
  - L2 hienosäätö menovesi
  - L2 hienosäätö huonelämpö
  - L2 lämmönpudotus menovesi
  - L2 suuri lämmönpudotus menovesi
  - L2 lämmönpudotus huonelämpö
  - L2 suuri lämmönpudotus huonelämpö
  - L2 huonelämpö asetus
  - L2 vakiolämpötilasäädin asetus
  - L2 3-piste käyrän 1/3
  - L2 3-piste käyrän 2/3
  - L2 3-piste käyrän 3/3
  - L2 5-piste käyrän 1/5
  - L2 5-piste käyrän 2/5
  - L2 5-piste käyrän 3/5
  - L2 5-piste käyrän 4/5
  - L2 5-piste käyrän 5/5
  - L2 5-piste käyrän asetuspiste 1/3
  - L2 5-piste käyrän asetuspiste 2/3
  - L2 5-piste käyrän asetuspiste 3/3
  - L2 käsiajo
  - L2 ohjaustapa

### Ulkolämpötila:
- ulkolämpötila
- ulkolämpötilan keskiarvo vrk
- ulkolämpötila hidastettu

### Rele ohjaus:
- anturit:
  - releen tila
- säätimet:
  - releohjaus tila
  - kesäpysäytys
  - aikaohjelma
  - lämpötilan mukaan
  - latauspumpun ohjaus
  - venttiilin mukaan
  - rele lämpötilan mukaan, eroalue
  - lämpötilan mukaan, arvo jossa rele vetää
  - venttiilin asennon mukaan, ON-tilaan
  - venttiilin asennon mukaan, OFF-tilaan

### Mittaukset:
- potentiometri TMR/SP
- varaajan lämpötila
- kattilan lämpötila
- paluu lämpötila
- aurinkokeräin lämpötila
- mittaus 3
- mittaus 4
- mittaus 5
- painehälytys
- poltinhälytys
- pumppuhälytys
- kattilahälytys
- kipinähälytys
- hälytys 4
- hälytys 5
- hälytys 6

### Muuta:
- hälytys


## Rajoitukset
Tiedossa on että tietojenhaku lakkaa toimimasta 75-80 kpl kohdalla. Tämän ei pitäisi ihan helposti tulla vastaan,
mutta näet aktivoitujen tietueitten määrän `Ouman rekisterit` anturin numerosta.

`unique id` -arvoja ei saa muuttaa, koska niiden perusteella haetaan aktivoidut sensorit/säätimet.

Hälytysanturi hakee vain ensimmäsenä ilmaantuneen hälytyksen.

## Rekisterit
Mikäli jotain haluamaasi anturia/säätöä ei löydy valmiina anturina niin voit katsoa [registers.md](registers.md) tiedostoa jos sieltä löytyisi haluamasi rekisteri ja luoda itse uuden anturin.  
