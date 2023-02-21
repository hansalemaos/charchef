### 3 functions to normalize strings, repair bad encoding, replace non-printable characters 


#### The function use numba under the hood - that means the first run is very slow, (compile time), but then the speed-up is tremendous.


##### pip install charchef

```python
from charchef import aa_convert_utf8_to_ascii_,aa_repair_bad_conversion_to_utf8,aa_replace_non_printable_chars
text = r"""ąćęłńóśźż ĄĆĘŁŃÓŚŹ\x00Ż Junto à Estação de Carcavelos; Bragança Situado 
en el núcleo de Es Caló de Sant Agustí frente al Hostal Rafalet. Cartão MOBI.E R. 
Conselheiro Emídio Navarro (frente ao ISEL) R. Conselheiro Emidio Navarro (frente ao ISEL)
 àáâãäåa èéêëe ìíîïi òóôõöo ùúûüu ýÿy Suzy &amp; John &quot; &pound;682m 
 \u00FF\u00FF\u00F0\u00f0\x95\xFF SmÃ¶rgÃ¥s Non ti suscita niente la parola pietÃ\xa0 RosÅ½ RUF MICH ZURÃœCK.
 aqu\195\173 09. BÃ¡t NhÃ£ TÃ¢m Kinh crianÃ§a KoÃ§ University Technische UniversitÃ¤t Dresden UniversitÃ¤t 
 fÃ¼r Musik und darstellende Kunst Wien Technische UniversitÃ¤t Wien Ã\x89cole Nationale SupÃ©rieure 
 des Beaux-Arts Paris Universidad SimÃ³n BolÃ\xadvar (USB) 240         Åland Islands   2014.0    
 MARIEHAMN   11437.0 1 240         Åland Islands   2010.0      MARIEHAMN   5829.5  1 240        
 Albania         2011.0      Durrës      113249.0 240         Albania         2011.0      TIRANA 
 418495.0 240         Albania         2011.0      Durrës      56511.0 "Tutu Au Mic' – dumbéa"
    """.splitlines()
bigc1 = aa_convert_utf8_to_ascii_(
        str_=text,
        preprocessing_functions=(
            "8x_3_lower_case_escaped",
            "8x_3_upper_case_escaped",
            "8u_4_upper_case_escaped",
            "8u_4_lower_case_escaped",
            "8x_69_upper_case_escaped",
            "8x_69_lower_case_escaped",
            "8n_escaped",
            "8wrong_chars",
            "8zerox_unescaped_lower",
            "8zerox_unescaped_upper",
            "8html_entity",
        ),
        preprocessing_function_non_printable=(
            "substitute_allcontrols_s",
            "substitute_allcontrols",
            "substitute_allcontrols2",
            "substitute_allcontrols2_s",
            "substitute_allcontrols3",
            "substitute_allcontrols3_s",
        ),
        respect_german_letters=True,
    )

bigc2 = aa_repair_bad_conversion_to_utf8(
        str_=text,
        functions=(
            "8x_3_lower_case_escaped",
            "8x_3_upper_case_escaped",
            "8u_4_upper_case_escaped",
            "8u_4_lower_case_escaped",
            "8x_69_upper_case_escaped",
            "8x_69_lower_case_escaped",
            "8n_escaped",
            "8wrong_chars",
            "8zerox_unescaped_lower",
            "8zerox_unescaped_upper",
            "8html_entity",
        ),
    )

bigc3 = aa_replace_non_printable_chars(
        str_="\x00rsi\\x00d\x00ad \x0aSimÃ³n BolÃ\xadvar",
        functions=(
            "substitute_allcontrols_s",
            "substitute_allcontrols",
            "substitute_allcontrols2",
            "substitute_allcontrols2_s",
        ),
        removex0a=False,
    )
	
	
bigc1 # replaces all accents, special characters ... 
Out[3]: 
['acelnoszz ACELNOSZZ Junto a Estacao de Carcavelos; Braganca Situado ',
 'en el nucleo de Es Calo de Sant Agusti frente al Hostal Rafalet. Cartao MOBI.E R. ',
 'Conselheiro Emidio Navarro (frente ao ISEL) R. Conselheiro Emidio Navarro (frente ao ISEL)',
 ' aaaaaeaa eeeee iiiii oooooeo uuuueu yyy Suzy & John " PS682m ',
 ' yydd*y Smorgas Non ti suscita niente la parola pieti RosZ RUF MICH ZURUCK.',
 ' aqui 09. Bat Nha Tam Kinh crianca Koc University Technische Universitat Dresden Universitat ',
 ' fur Musik und darstellende Kunst Wien Technische Universitat Wien Ecole Nationale Superieure ',
 ' des Beaux-Arts Paris Universidad Simon Bolivar (USB) 240         Sland Islands   2014.0    ',
 ' MARIEHAMN   11437.0 1 240         Sland Islands   2010.0      MARIEHAMN   5829.5  1 240        ',
 ' Albania         2011.0      Durres      113249.0 240         Albania         2011.0      TIRANA ',
 ' 418495.0 240         Albania         2011.0      Durres      56511.0 "Tutu Au Mic\' - dumbea"',
 '    ']
 
 
bigc2 # Repairs messed up Unicode
Out[4]: 
['ąćęłńóśźż ĄĆĘŁŃÓŚŹŻ Junto à Estação de Carcavelos; Bragança Situado ',
 'en el núcleo de Es Caló de Sant Agustí frente al Hostal Rafalet. Cartão MOBI.E R. ',
 'Conselheiro Emídio Navarro (frente ao ISEL) R. Conselheiro Emidio Navarro (frente ao ISEL)',
 ' àáâãäåa èéêëe ìíîïi òóôõöo ùúûüu ýÿy Suzy & John " £682m ',
 ' ÿÿðð•ÿ Smörgås Non ti suscita niente la parola pietí RosŽ RUF MICH ZURÜCK.',
 ' aquí 09. Bát Nhã Tâm Kinh criança Koç University Technische Universität Dresden Universität ',
 ' für Musik und darstellende Kunst Wien Technische Universität Wien École Nationale Supérieure ',
 ' des Beaux-Arts Paris Universidad Simón Bolívar (USB) 240         Šland Islands   2014.0    ',
 ' MARIEHAMN   11437.0 1 240         Šland Islands   2010.0      MARIEHAMN   5829.5  1 240        ',
 ' Albania         2011.0      Durrës      113249.0 240         Albania         2011.0      TIRANA ',
 ' 418495.0 240         Albania         2011.0      Durrës      56511.0 "Tutu Au Mic\' – dumbéa"',
 '    ']
 
 
bigc3 # Removes non-printable characters
Out[5]: ['rsidad SimÃ³n BolÃ\xadvar']
```