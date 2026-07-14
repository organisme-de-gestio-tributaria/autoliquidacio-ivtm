# Webservice d'autoliquidacions IVTM (Impost sobre Vehicles de Tracció Mecànica) per ajuntaments

## Procediment d’adhesió
Contacteu amb el departament d'Administració Digital de l’ORGT ( orgt.administraciodigital@diba.cat )

## Informació tècnica
Cal que accediu al webservice mitjançat operacions SOAP. L’especificació la podeu obtenir del [fitxer WSDL disponible en aquest repositori](https://github.com/organisme-de-gestio-tributaria/autoliquidacio-ivtm/blob/main/AutoliquidacioIVTM_wsdl.xml).

El servei es troba a les següents URL's:
* Producció: https://wsreal.orgt.diba.cat/AutoliquidacioIVTM/AutoliquidacioIVTMService.svc
* Pre-produció (proves): https://wsproves.orgt.diba.cat/AutoliquidacioIVTM/AutoliquidacioIVTMService.svc

Respecte a la seguretat, cal que tingueu en compte:
1. L’accés al webservice serà via https i amb certificat d'òrgan. 
2. El client del webservice ha de suportar el protocol TLS 1.2.
3. **Previ a les proves cal que comuniqueu el certificat utilitzat a l’ORGT ja que és necessari instal·lar la clau pública als servidors de la ORGT.** Contacteu amb el departament d'Administració Digital de l’ORGT ( orgt.administraciodigital@diba.cat )

Les operacions disponibles són:

1. **ComunicacioMatricula**. Comunica una nova matrícula.

   *Paràmetres petició*: número d'autoliquidació, número exprés gestor, matrícula, número de bastidor

   *Paràmetres resposta*: 

2. **BeneficisFiscals**. Consulta els beneficis fiscals d'un municipi en l'exercici demanat.
   
   *Paràmetres petició*: codi de municipi, exercici

   *Paràmetres resposta*: una llista de les Exempcions (codi, descripcio) i una llista amb les Bonificacions (codi, descripcio, percentatge de bonificació)

3. **Calcul**. Realitza el càlcul d'una autoliquidació d'IVTM.

   *Paràmetres petició*: NIF gestoria, número exprés gestor, tipus d'operacio [Matriculacio o Rehabilitacio], matrícula rehabilitada, codi de municipi, data de matriculació, conjunt de dades del Vehicle, conjunt de dades del Titular, codi de Benefici Fiscal

   *Paràmetres resposta*: número d'autoliquidació, conjunt de dades calculades (import anual, import periode, import a pagar, codi de tarifa, percentatge de Benefici Fiscal)
   
   *Paràmetres resposta opcionals*: documentació en PDF si l'import a pagar és 0€

4. **Presentar**. Presenta una o vàries autoliquidacions i retorna les dades de pagament.

   *Paràmetres petició*: llista de números d'autoliquidació (si hi ha més d'un número d'autoliquidació, genera un abonaré conjunt)

   *Paràmetres resposta*: conjunt de dades de l'abonaré (referencia, identificadora, data d'emissió, data de caducitat, emissora), codi HTML per poder redireccionar el pagament on-line per TPV

5. **Pagament**. Pagamament d'una autoliquidació d'IVTM.

   *Paràmetres petició*: emissor, número d'autoliquidació, conjunt de dades del pagament (import, IBAN, NIF titular, nom titular)

   *Paràmetres resposta*: document de pagament en PDF, conjunt de dades calculades (import anual, import periode, import a pagar, codi de tarifa, % de Benefici Fiscal), número d'autoliquidació, conjunt de dades de l'abonaré (referencia, identificadora, data d'emissió, data de caducitat, emissora), NRC, IBAN

6. **AcreditarPagament**. Recupera el document de pagamament d'una autoliquidació d'IVTM.

   *Paràmetres petició*: número d'autoliquidació
  
   *Paràmetres petició opcionals*: número justificant de pagament (NRC), data de pagament, entitat de pagament

   *Paràmetres resposta*: codi del model del document, document de pagament en PDF


#### Notes i més informació:
* [Comentaris del WSDL](https://wsproves.orgt.diba.cat/AutoliquidacioIVTM/AutoliquidacioIVTMService.svc/mex?singleWsdl). Podeu cancel·lar la sol·licitud de certificat en l'accedir al WSDL.
* Cal notar que totes les dades de tipus text han d'estar en majúscules.
* Els imports es representen mitjançant nombres sense decimals en cèntims d'euro. Així, per exemple, l'import 101,23€ es representa amb 10123. D'altra banda, l'import de 102€ es representa com 10200.
