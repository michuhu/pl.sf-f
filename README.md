# Polish science-fiction and fantasy usenet group archive
(ENG) The archive of polish science-fiction and fantasy usenet groups

Archiwum dwóch głównych polskich grup usenet o tematyce science-fiction & fantasy:
* pl.listserv.sf-f (~86 tys. wiadomości)
* pl.rec.fantastyka.sf-f (~663. tys wiadomości)

## TL;DR
Linki do gotowych plików mbox znajdziecie poniżej:

[https://archive.org/details/pl.sf-f](https://archive.org/details/pl.sf-f)


**MIRROR**:
* UMAPA
  * [listserv-sf-f.zip](https://www.umapa.pl/usenet/listserv-sf-f.zip)
  * [sf-f.zip](https://www.umapa.pl/usenet/sf-f.zip)

*Przydałby się dodatkowy mirror tych danych. Gdybyś miał(a) miejsce u siebie, by przechować kopię tych mboxów (spakowane ~600 MB), daj mi proszę znać.* 

## WPROWADZENIE:

> Jak sie macie---fani SF&F???
> 
> Jerz.
  
Tak brzmiała pierwsza wiadomość opublikowana na polskiej grupiej fanów SF-F - **pl.listserv.sf-f** z **10 października 1994 roku**. Wysłana z serwerów **Centralnego Urzędu Planowania (cup.gov.pl)**. Tyle historii w jednej wiadomości. Piękne, co? Cztery lata później grupa listserv.sf-f została zamknięta i przekształcona w (legendarną;)) **pl.rec.fantastyka.sf-f**.

Powodem zabawy w przywrócenie do życia tych zakurzonych danych, były wspominki na **pl.rec.fantastyka** oraz sentyment do *retro internetu*. Na dane źródłowe trafiłem tutaj: <https://usenet.nereid.pl>. Wielki szacunek dla kolegi [@wolfpld](https://github.com/wolfpld) za utrzymanie tak zacnego archiwum, nie tylko SF-F, ale **CAŁEGO (!)** polskiego usenetu.
Podziękowania wędrują również do Szymona Sokoła, który dostarczył kilkaset pierwszych, brakujących postów.

Niestety format tych danych i ich "spakowanie" okazały się nieco przykre dla użytkowników nietechincznych, na co zwracało uwagę wiele osób na grupach usenet. Wziąłem się tedy za robotę, porozozmawiałem z Bartoszem ([@wolfpld](https://github.com/wolfpld) jak wypakować dane (dzięki za pomoc i współpracę).
Problemem okazały się formaty dat - szalone lata '90, braki standaryzacji, np. różne zapisy dat, błędne zapisy dat, nieistniejące już nazwy stref czasowych etc. Napisałem bardzo prosty skrypt w python, który ujednolica format daty, tak, aby wiadomości mogły być ustawione chronologicznie w wątkach. 

## TECHNIKALIA:

Projekt [@wolfpld](https://github.com/wolfpld) znajduje się tutaj: [Usenet Archive Toolkit ](https://github.com/wolfpld/usenetarchive/)

Tak naprawdę potrzebujesz tylko skompilować:
- package
- repack-lz4
- export-messages

#### Ściągamy potrzebne archiwum usenet stąd: <https://usenet.nereid.pl>

**Wykonujemy po kolei komendy**

#### Rozpakowanie formatu .usenet
`./package -x plik.usenet folder-unpack`

#### Przepakowanie w format lz4
`./repack-lz4 folder-unpack`

#### Wyciągnięcie pojedynczych emaili
`./export-messages folder-unpack folder-out`

W tej chwili w `folder-out` mamy już pojedyncze wiadomości email, ale wszystkie będą miały jako nazwę ID wiadomości, czyli np. `61c02d3d$0$548$65785112@news.neostrada.pl`. Większość systemów będzie go chciała odczytać jako skrypt `PERL` (końcówka .pl), więc wystarczy, że dodamy rozszerzenie `.eml` i po sprawie.

#### skrypt do dodawania rozszerzenia plików w folderze
`for i in *; do mv -- „$i" "$i.eml"; done`

*-- (double dash) jest potrzebny, ponieważ niektóre pliki wiadomości zaczynają się od "-" (dash), co powoduje problem przy rozpakowywaniu

Ok, mamy już pojedyncze maile. Niestety, formaty dat są bardzo różne co sprawia, że wątki są niemiłosiernie poszatkowane i wiadomości ciężko czytać w sensownej kolejności. Uruchamiamy mój skrypt, który znajduje się w repo [eml-date-fix](https://github.com/michuhu/eml-date-fix): 

`python eml-date-fix.py eml_folder`

Jako wynik dostaniesz wewnątrz folderu `eml_folder`, subfolder `/fixed`, z przekonwertowanymi plikami.
Ewentualne błędy będą zapisane w pliku `log.txt`. Jeśli go nie będzie, wszystko przebiegło bezproblemowo.

### Jak przekonwertować pojedyncze maile do jednego mboxa?
Posłużymy się gotowym skryptem eml2mbox jakich pełno, np. stąd: [emlToMbox](https://github.com/Jachimo/emlToMbox)

`$ ./emlToMbox.py inputdir/ output.mbox`

Teraz możesz dodać pliki mbox do ulubionego programu pocztowego i radować się piękną kartą historii polskiego sf-f :)

## Dodatkowa notka:
**Mozilla Thunderbird**
* Po pierwsze, sprawdź ustawienia lokalizacji w swoim Thunderbird, bo to wpływa na format odczytywania daty, tj. np. polski `dd/mm/YYYY`, a amerykański `mm/dd/yyyy` (wciąż nie potrafię zrozumieć czemu tak utrudniają sobie życie?!)
* Jeśli importujesz pliki mbox do programu Mozilla Thunderbird używając import-export-tool, uważaj z jakim formatem daty je importujesz. 

## TO DO:
* dodać mirrory mboxów
* dodać mirrory polskiego usenet
* własny skrypt eml2mbox (?)
