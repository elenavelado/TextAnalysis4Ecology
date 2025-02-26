# Análisis de textos con R: Ejemplos útiles para la ecología
<img src="images/ecoinf_10.jpg" />

Elena Velado-Alonso
26/02/2025

- [<span class="toc-section-number">1</span> Análisis de textos con R:
  ejemplos útiles para la
  ecología.](#análisis-de-textos-con-r-ejemplos-útiles-para-la-ecología)
  - [<span class="toc-section-number">1.1</span> Caracteres en R
    base](#caracteres-en-r-base)
  - [<span class="toc-section-number">1.2</span> Paquete `stringr`:
    caracteres con filosofía
    tidy](#paquete-stringr-caracteres-con-filosofía-tidy)
  - [<span class="toc-section-number">1.3</span> Limpieza automática de
    nombres taxonómicos con
    gbif](#limpieza-automática-de-nombres-taxonómicos-con-gbif)
  - [<span class="toc-section-number">1.4</span> Leer pdf en R y limpiar
    textos](#leer-pdf-en-r-y-limpiar-textos)
  - [<span class="toc-section-number">1.5</span> Limpieza de
    textos](#limpieza-de-textos)
  - [<span class="toc-section-number">1.6</span> Análisis de textos:
    Frecuencia y nubes de
    palabras](#análisis-de-textos-frecuencia-y-nubes-de-palabras)
  - [<span class="toc-section-number">1.7</span> Análisis de textos:
    Relación entre
    palabras](#análisis-de-textos-relación-entre-palabras)
  - [<span class="toc-section-number">1.8</span> Recursos y
    agradecimientos](#recursos-y-agradecimientos)

# Análisis de textos con R: ejemplos útiles para la ecología.

Trabajar con caracteres (string data) en R suele ser percibido como una
tarea ardua por muchos usuarios. Normalmente, la formación en
programación y análisis de datos se centra en cuestiones numéricas y
analíticas, y rara vez se trata en profundidad el trabajo con
caracteres. Sin embargo, el trabajo en ecología requiere trabajar con
caracteres frecuentemente. Además, el creciente uso de bases de datos y
los principios FAIR <https://www.go-fair.org/fair-principles/> está
aumentando la información presentada en caracteres y la necesidad de
saber trabajar con ellos en los entornos de programación. Por tanto, no
saber trabajar con caracteres limita la reproducibilidad de los flujos
de trabajo en ecología.

<img src="images/meme1.png" data-fig-align="left" />

En este seminario:

- repasaremos conceptos básicos del trabajo con caracteres en R

- veremos ejemplos prácticos para la limpieza automática de nombres
  taxonómicos

- aprenderemos a leer archivos pdf

- limpiar textos para su posterior análisis

- haremos gráficos de nubes de palabras

- cálculos simples de las frecuencias de palabras en textos y
  correlación entre palabras para el análisis del contenido.

## Caracteres en R base

En R, los textos se representan como series de caracteres (character
strings). Los caracteres se definen con comillas simples o dobles y
pueden ser guardados en objetos de clase “character”. Por ejemplo:

``` r
"esto es un texto"
```

    [1] "esto es un texto"

``` r
a <- "esto es otro texto"
a
```

    [1] "esto es otro texto"

``` r
class(a)
```

    [1] "character"

Dentro de los character strings se pueden usar comillar. Si se usa
comillas simples, se pueden usar comillas dobles dentro del texto y
viceversa. Por ejemplo:

``` r
x <- "El grupo de trabajo de Ecoinformática es el más 'cool' de la AEET"
x
```

    [1] "El grupo de trabajo de Ecoinformática es el más 'cool' de la AEET"

Se pueden concatenar caracteres con la función `paste()`, que además
permite definir cómo se realiza la concatenación. También la función
`rep()`nos ayuda a repetir elementos de un vector. Por ejemplo:

``` r
name <- "Mochi"

surname <- "Condatos"

paste(name, surname)
```

    [1] "Mochi Condatos"

``` r
paste(name, surname, sep = "_")
```

    [1] "Mochi_Condatos"

``` r
paste0(name, surname)
```

    [1] "MochiCondatos"

``` r
paste(rep(name, 4), collapse = '')
```

    [1] "MochiMochiMochiMochi"

Distintas funciones nos pueden ayudar a visibililzar los caracteres:

- `print()` nos permite imprimir el contenido de un objeto.

- `noquote()` nos permite imprimir el contenido de un objeto sin
  comillas.

- `cat()` concatena e imprime y también nos permite definir cómo se
  imprime el contenido de un objeto.

``` r
name <- "Mochi"

surname <- "Condatos"

scientist <- paste(name, surname)

print(scientist)
```

    [1] "Mochi Condatos"

``` r
noquote(scientist)
```

    [1] Mochi Condatos

``` r
cat(letters, sep = "-") 
```

    a-b-c-d-e-f-g-h-i-j-k-l-m-n-o-p-q-r-s-t-u-v-w-x-y-z

Se puede contar el número de elementos que tiene un objeto string con la
función `length()`.

La función `nchar()` nos da el número de caracteres de un string.

``` r
length(scientist)
```

    [1] 1

``` r
nchar(scientist)
```

    [1] 14

``` r
length(letters)
```

    [1] 26

Los caracteres se pueden convertir en mayúsculas o minúsculas con las
funciones `toupper()` y `tolower()`:

``` r
toupper(scientist)
```

    [1] "MOCHI CONDATOS"

``` r
tolower(scientist)
```

    [1] "mochi condatos"

Para reemplazar alguno de los caracteres de un string, se puede usar la
función `chartr()`. Por ejemplo, para reemplazar las vocales con tilde
por vocales sin tilde:

``` r
x <- "Mochi es el mejor pERRo para aprender a programar."
chartr(old = "ERR", new = "err", x)
```

    [1] "Mochi es el mejor perro para aprender a programar."

Para extraer partes de un string, se puede usar la función `substr()`.
Por ejemplo, para extraer los primeros 5 caracteres de un string:

``` r
substr(x, start = 1, stop = 5)
```

    [1] "Mochi"

``` r
substr(x, start = 18, stop = 23)
```

    [1] " pERRo"

También se puede separar un string en partes más pequeñas con la función
`strsplit()`. Por ejemplo, para separar las palabras de una frase:

``` r
x <- "Mochi es el mejor perro para aprender a programar."
y <- strsplit(x, split = " ")
y
```

    [[1]]
    [1] "Mochi"      "es"         "el"         "mejor"      "perro"     
    [6] "para"       "aprender"   "a"          "programar."

También podemos identificar patrones de caracteres con la función
`grep()`. Por ejemplo, para identificar las palabras que contienen la
letra “o”, las que empiezan por “M” o las que terminan en “r”:

``` r
z <- c("Mochi", "es", "el", "mejor", "perro", "para", "aprender", "a", "programar")
  
grep("o", z, value=TRUE)
```

    [1] "Mochi"     "mejor"     "perro"     "programar"

``` r
grepl("o", z)
```

    [1]  TRUE FALSE FALSE  TRUE  TRUE FALSE FALSE FALSE  TRUE

``` r
grep("^M", z, value=TRUE)
```

    [1] "Mochi"

``` r
grep("r$", z, value=TRUE)
```

    [1] "mejor"     "aprender"  "programar"

Para especificar la posicion en un string hay que usar expresiones
regulares, por ejemplo “^” para caracteres al principio y “\$” para
caracteres al final. Por tanto, algunos caracteres son usados como
metacaracteres con un significado especial en las expresiones regulares.
El punto (.), el asterisco (\*), el signo de interrogación (?), el signo
más (+), el signo de intercalación (^), el signo del dólar (\$), los
corchetes (\[, \]), las llaves ({, }), el guión (-), entre otros, tienen
un significado no literal. Puedes consultar más información por ejemplo
aquí:

``` r
?regex
```

o aqui <https://uc-r.github.io/regex#regex_functions_base>.

En mi opinión, las expresiones regulares y los metacaracteres son una de
las dificultades que nos encontramos a la hora de trabajar con
caracteres en R base. Algunas funcionalizades de los paquetes “tidy”
simplifican el trabajo con expresiones regulares, como veremos más
adelante.

Para modificar el contenido de un string, se pueden usar las funciones
`sub()` y `gsub()`. La función `sub()` reemplaza la primera ocurrencia
de un patrón en un string, mientras que `gsub()` reemplaza todas las
ocurrencias de un patrón en un string. Por ejemplo:

``` r
texto <- "El grupo de trabajo de Ecoinformática de la Asociación Española de Ecología Terrestre pretende fomentar el intercambio de experiencias y conocimientos sobre cualquier aspecto relacionado con la ecoinformática, además de contribuir a la formación de los nuevos ecólogos en buenas prácticas y técnicas de computación que les permitan desarrollar al máximo sus proyectos de investigación."
texto
```

    [1] "El grupo de trabajo de Ecoinformática de la Asociación Española de Ecología Terrestre pretende fomentar el intercambio de experiencias y conocimientos sobre cualquier aspecto relacionado con la ecoinformática, además de contribuir a la formación de los nuevos ecólogos en buenas prácticas y técnicas de computación que les permitan desarrollar al máximo sus proyectos de investigación."

``` r
nuevo_texto <- gsub("Asociación Española de Ecología Terrestre", "AEET", texto)
nuevo_texto
```

    [1] "El grupo de trabajo de Ecoinformática de la AEET pretende fomentar el intercambio de experiencias y conocimientos sobre cualquier aspecto relacionado con la ecoinformática, además de contribuir a la formación de los nuevos ecólogos en buenas prácticas y técnicas de computación que les permitan desarrollar al máximo sus proyectos de investigación."

R base tiene una serie de funciones muy útiles para comparar dos
vectores de caracteres.

| Set Operations in R |                |
|---------------------|----------------|
| Function            | Description    |
| union()             | set union      |
| intersect()         | intersection   |
| setdiff()           | set difference |
| setequal()          | equal sets     |
| identical()         | exact equality |
| is.element()        | is element     |
| sort()              | sorting        |

Yo uso frecuentemente estas funciones para trabajar con especies y
nombres taxonómicos. Por ejemplo:

``` r
species1 <- c("Protaetia morio", "Agrypnus murinus", "Stenobothrus lineatus", "Philoscia muscorum")

species2 <- c("Protaetia morio", "Tettigonia cantans", "Stenobothrus lineatus", "Mangora acalypha")

union(species1, species2) #todas las especies únicas
```

    [1] "Protaetia morio"       "Agrypnus murinus"      "Stenobothrus lineatus"
    [4] "Philoscia muscorum"    "Tettigonia cantans"    "Mangora acalypha"     

``` r
intersect(species1, species2) #especies comunes
```

    [1] "Protaetia morio"       "Stenobothrus lineatus"

``` r
setdiff(species1, species2) #especies en species1 pero no en species2
```

    [1] "Agrypnus murinus"   "Philoscia muscorum"

``` r
setdiff(species2, species1) #especies en species2 pero no en species1
```

    [1] "Tettigonia cantans" "Mangora acalypha"  

``` r
setequal(species1, species2) #son iguales?
```

    [1] FALSE

``` r
species3 <- c("Philoscia muscorum", "Stenobothrus lineatus", "Agrypnus murinus", "Protaetia morio")

setequal(species1, species3)
```

    [1] TRUE

``` r
identical(species1, species3)
```

    [1] FALSE

``` r
is.element("Protaetia morio", species1)
```

    [1] TRUE

``` r
"Protaetia morio" %in% species1
```

    [1] TRUE

``` r
sort(species1)
```

    [1] "Agrypnus murinus"      "Philoscia muscorum"    "Protaetia morio"      
    [4] "Stenobothrus lineatus"

Una de mis funciones favoritas para encontrar errores al trabajar con
nombres de especies es la función `agrep` que permite buscar patrones
aproximados en un vector de caracteres.

``` r
datos_abejas <- data.frame(
  Especie = c("Andrena_chrysosceles", "Lasioglossum_villosulum", "Bombus_silvarum",
              "Halictus_tumulorum", "Apis_mellifera"),
  Abundancia = c(20, 15, 8, 12, 5)
)

"Bombus_sylvarum" %in% datos_abejas
```

    [1] FALSE

``` r
agrep("Bombus_sylvarum", datos_abejas$Especie, value = TRUE)
```

    [1] "Bombus_silvarum"

## Paquete `stringr`: caracteres con filosofía tidy

`stringr` es un paquete desarrollado para manejar caracteres bajo la
filosofía tidy, que mejora algunas aplicabilidades como trabajar con NA.

Existen cuatro familias principales de funciones en stringr:

- Manipulación de caracteres: estas funciones permiten manipular
  caracteres individuales dentro de vectores.

- Herramientas de espacios en blanco para añadir, eliminar y manipular
  espacios en blanco.

- Operaciones sensibles a la localización.

- Funciones de concordancia de patrones, incluyendo las expresiones
  regulares.

``` r
library(stringr)
scientist
```

    [1] "Mochi Condatos"

``` r
str_length(scientist) #equivalente a nchart()
```

    [1] 14

``` r
str_sub(scientist, start = 2, end = -2) #equivalente a substr()
```

    [1] "ochi Condato"

``` r
str_to_upper(scientist) #equivalente a toupper()
```

    [1] "MOCHI CONDATOS"

``` r
str_to_lower(scientist) #equivalente a tolower()
```

    [1] "mochi condatos"

``` r
texto <- "Hola AEET"
texto
```

    [1] "Hola AEET"

``` r
nuevo_texto <- str_replace(texto, "AEET", "EcoInformatica ")
nuevo_texto
```

    [1] "Hola EcoInformatica "

``` r
str_trim(nuevo_texto) #elimina espacios en blanco al principio y al final
```

    [1] "Hola EcoInformatica"

``` r
str_split(nuevo_texto, boundary("word")) #separa palabras
```

    [[1]]
    [1] "Hola"           "EcoInformatica"

``` r
str_split(nuevo_texto, "") #separa caracteres
```

    [[1]]
     [1] "H" "o" "l" "a" " " "E" "c" "o" "I" "n" "f" "o" "r" "m" "a" "t" "i" "c" "a"
    [20] " "

``` r
print(datos_abejas$Especie)
```

    [1] "Andrena_chrysosceles"    "Lasioglossum_villosulum"
    [3] "Bombus_silvarum"         "Halictus_tumulorum"     
    [5] "Apis_mellifera"         

``` r
str_detect("Apis_mellifera", datos_abejas$Especie) #equivalente a grepl()
```

    [1] FALSE FALSE FALSE FALSE  TRUE

``` r
str_replace("Bombus_mellifera", "Bombus", "Apis") #equivalente a gsub()
```

    [1] "Apis_mellifera"

``` r
abejorros <- c("Bombus_silvarum", "Bombus_terrestris", "Bombus_pascuorum", "Bombus_pratorum", "Bombus_humilis")

str_count(abejorros, "Bombus") #cuenta el número de veces que aparece un patrón
```

    [1] 1 1 1 1 1

``` r
str_locate(abejorros, "Bombus") #localiza la posición de un patrón
```

         start end
    [1,]     1   6
    [2,]     1   6
    [3,]     1   6
    [4,]     1   6
    [5,]     1   6

``` r
str_match(abejorros, "Bombus") #devuelve el patrón encontrado
```

         [,1]    
    [1,] "Bombus"
    [2,] "Bombus"
    [3,] "Bombus"
    [4,] "Bombus"
    [5,] "Bombus"

``` r
str_split(abejorros, "_") #equivalente a strsplit()
```

    [[1]]
    [1] "Bombus"   "silvarum"

    [[2]]
    [1] "Bombus"     "terrestris"

    [[3]]
    [1] "Bombus"    "pascuorum"

    [[4]]
    [1] "Bombus"   "pratorum"

    [[5]]
    [1] "Bombus"  "humilis"

``` r
poema_espronceda <- str_c("Con diez cañones por banda, viento ",
                          "en popa a toda vela, no corta el mar, ",
                          "sino vuela un velero bergantín: bajel ",
                          "pirata que llaman, por su bravura, ",
                          "el Temido, en todo mar conocido ",
                          "del uno al otro confín.")

poema_espronceda
```

    [1] "Con diez cañones por banda, viento en popa a toda vela, no corta el mar, sino vuela un velero bergantín: bajel pirata que llaman, por su bravura, el Temido, en todo mar conocido del uno al otro confín."

``` r
str_flatten(poema_espronceda) #equivalente a paste()
```

    [1] "Con diez cañones por banda, viento en popa a toda vela, no corta el mar, sino vuela un velero bergantín: bajel pirata que llaman, por su bravura, el Temido, en todo mar conocido del uno al otro confín."

``` r
cat(str_wrap(poema_espronceda, width = 27)) #adapta el texto a un tamaño determinado
```

    Con diez cañones por banda,
    viento en popa a toda vela,
    no corta el mar, sino vuela
    un velero bergantín: bajel
    pirata que llaman, por su
    bravura, el Temido, en todo
    mar conocido del uno al
    otro confín.

Puedes encontrar más información sobre el paquete `stringr` en la
documentación oficial <https://stringr.tidyverse.org/> y la hoja resumen
<https://github.com/rstudio/cheatsheets/blob/main/strings.pdf>.

## Limpieza automática de nombres taxonómicos con gbif

En ecología, es frecuente trabajar con nombres taxonómicos de especies.
Estos nombres pueden contener errores tipográficos, abreviaturas,
sinónimos, etc. que dificultan su análisis. Aunque es recomendable hacer
una limpieza detallada y manual, se puede hacer una aproximación de
limpieza automática de nombres taxonómicos con la Taxonomía Base de GBIF
<https://www.gbif.org/dataset/d7dddbf4-2cf0-4f39-9b2a-bb099caae36c> y el
paquete `rgbif`.

``` r
library(tidyverse)
```

    ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ✔ dplyr     1.1.4     ✔ purrr     1.0.2
    ✔ forcats   1.0.0     ✔ readr     2.1.5
    ✔ ggplot2   3.5.1     ✔ tibble    3.2.1
    ✔ lubridate 1.9.3     ✔ tidyr     1.3.1
    ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ✖ dplyr::filter() masks stats::filter()
    ✖ dplyr::lag()    masks stats::lag()
    ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
library(rgbif)
```

    Warning: package 'rgbif' was built under R version 4.4.2

``` r
library(here)
```

    Warning: package 'here' was built under R version 4.4.2

    here() starts at C:/Users/velad002/OneDrive/InterRest/TextMining_Seminar/TextAnalysis4Ecology

``` r
#For Bees
#1) Load bee spp names
df.e <- read_delim(here("data/df.e.csv"), delim = ";")
```

    Rows: 1052 Columns: 3
    ── Column specification ────────────────────────────────────────────────────────
    Delimiter: ";"
    chr (3): Country, Date, Bee_name

    ℹ Use `spec()` to retrieve the full column specification for this data.
    ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
#Create poll spp name dataset
Poll_spp <- df.e |> select(Bee_name) |>  
  distinct() |>
  arrange(Bee_name) |> pull()
Poll_spp
```

      [1] "Andrena alfkenella"         "Andrena bicolor"           
      [3] "Andrena chrysosceles"       "Andrena cineraria"         
      [5] "Andrena dorsata"            "Andrena flavipes"          
      [7] "Andrena hattorfiana"        "Andrena labialis"          
      [9] "Andrena labiata"            "Andrena minutula"          
     [11] "Andrena mynutula"           "Andrena nitida"            
     [13] "Andrena proxima"            "Anthidium byssinum"        
     [15] "Anthidium manicatum"        "Anthidium nanum"           
     [17] "Anthidium punctatum"        "Apis melifera"             
     [19] "Apis mellifera"             "Apis melllifera"           
     [21] "Bombus bohemicus"           "Bombus campestris"         
     [23] "Bombus hortorum"            "Bombus hypnorum"           
     [25] "Bombus lapidarius"          "Bombus lapydarius"         
     [27] "Bombus lucorum"             "Bombus pascuorum"          
     [29] "Bombus pratorum"            "Bombus ruderarius"         
     [31] "Bombus rupestris"           "Bombus rupestriss"         
     [33] "Bombus spec"                "Bombus sylvarum"           
     [35] "Bombus sylvestris"          "Bombus terrestris"         
     [37] "Bombus vestalis"            "Ceratina cyanea"           
     [39] "Cerceris quinquefasciata"   "Chelostoma florisomne"     
     [41] "Coelioxys alata"            "Colletes daviesanus"       
     [43] "Epeolus variegatus"         "Halictus quadricinctus"    
     [45] "Halictus rubicundus"        "Halictus scabiosae"        
     [47] "Halictus simple"            "Halictus simplex"          
     [49] "Halictus spec"              "Halictus subauratus"       
     [51] "Halictus tumulorum"         "Hallictus subauratus"      
     [53] "Hylaeus angustatus"         "Hylaeus brevicornis"       
     [55] "Hylaeus communis"           "Hylaeus confusus"          
     [57] "Hylaeus dilatatus"          "Hylaeus nigritus"          
     [59] "Hylaeus styriacus"          "Lasioglossum aeratum"      
     [61] "Lasioglossum albipes"       "Lasioglossum calceatum"    
     [63] "Lasioglossum costulatum"    "Lasioglossum interruptum"  
     [65] "Lasioglossum laevigatum"    "Lasioglossum laticeps"     
     [67] "Lasioglossum lativentre"    "Lasioglossum leucopus"     
     [69] "Lasioglossum leucozonium"   "Lasioglossum lineare"      
     [71] "Lasioglossum lucidulum"     "Lasioglossum malachurum"   
     [73] "Lasioglossum morio"         "Lasioglossum nitidiusculum"
     [75] "Lasioglossum pauxillum"     "Lasioglossum pygmaeum"     
     [77] "Lasioglossum villosulum"    "Lasioglossum vilosulum"    
     [79] "Lindenius albilabris"       "Megachile centuncularis"   
     [81] "Megachile lapponica"        "Megachile maritima"        
     [83] "Megachile pilidens"         "Megachile versicolor"      
     [85] "Megachile willughbiella"    "Melitta haemorrhoidalis"   
     [87] "Nomada emarginata"          "Nomada flavoguttata"       
     [89] "Osma aurulenta"             "Osmia adunca"              
     [91] "Osmia aurulenta"            "Osmia campanularum"        
     [93] "Osmia cantabrica"           "Osmia leucomelana"         
     [95] "Osmia rapunculi"            "Osmia spinulosa"           
     [97] "Osmia truncorum"            "Sphecodes crassus"         
     [99] "Sphecodes gibbus"           "Stelis odontopyga"         
    [101] "Stelis signata"             "Xylocopa violacea"         

``` r
#2) Retrieve GBIF taxonomy
#name_backbone_checklist()
#Lookup names in the GBIF backbone taxonomy in a checklist.

gbif_names <- name_backbone_checklist(name_data = Poll_spp, order = "   Hymenoptera")
gbif_names
```

    # A tibble: 102 × 26
       usageKey scientificName       canonicalName rank  status confidence matchType
          <int> <chr>                <chr>         <chr> <chr>       <int> <chr>    
     1  1357170 Andrena alfkenella … Andrena alfk… SPEC… ACCEP…        100 EXACT    
     2  1356887 Andrena bicolor Fab… Andrena bico… SPEC… ACCEP…        100 EXACT    
     3  1358259 Andrena chrysoscele… Andrena chry… SPEC… ACCEP…        100 EXACT    
     4  1358088 Andrena cineraria (… Andrena cine… SPEC… ACCEP…        100 EXACT    
     5  1357872 Andrena dorsata (Ki… Andrena dors… SPEC… ACCEP…        100 EXACT    
     6  1357156 Andrena flavipes Pa… Andrena flav… SPEC… ACCEP…        100 EXACT    
     7  1357364 Andrena hattorfiana… Andrena hatt… SPEC… ACCEP…        100 EXACT    
     8  1357222 Andrena labialis (K… Andrena labi… SPEC… ACCEP…        100 EXACT    
     9  1357207 Andrena labiata Fab… Andrena labi… SPEC… ACCEP…        100 EXACT    
    10  1357013 Andrena minutula (K… Andrena minu… SPEC… ACCEP…        100 EXACT    
    # ℹ 92 more rows
    # ℹ 19 more variables: kingdom <chr>, phylum <chr>, order <chr>, family <chr>,
    #   genus <chr>, species <chr>, kingdomKey <int>, phylumKey <int>,
    #   classKey <int>, orderKey <int>, familyKey <int>, genusKey <int>,
    #   speciesKey <int>, synonym <lgl>, class <chr>, acceptedUsageKey <int>,
    #   verbatim_name <chr>, verbatim_index <dbl>, verbatim_order <chr>

``` r
#Match and unmatched records
matched <-  gbif_names |>  filter(matchType == "EXACT") #92 matched
matched
```

    # A tibble: 92 × 26
       usageKey scientificName       canonicalName rank  status confidence matchType
          <int> <chr>                <chr>         <chr> <chr>       <int> <chr>    
     1  1357170 Andrena alfkenella … Andrena alfk… SPEC… ACCEP…        100 EXACT    
     2  1356887 Andrena bicolor Fab… Andrena bico… SPEC… ACCEP…        100 EXACT    
     3  1358259 Andrena chrysoscele… Andrena chry… SPEC… ACCEP…        100 EXACT    
     4  1358088 Andrena cineraria (… Andrena cine… SPEC… ACCEP…        100 EXACT    
     5  1357872 Andrena dorsata (Ki… Andrena dors… SPEC… ACCEP…        100 EXACT    
     6  1357156 Andrena flavipes Pa… Andrena flav… SPEC… ACCEP…        100 EXACT    
     7  1357364 Andrena hattorfiana… Andrena hatt… SPEC… ACCEP…        100 EXACT    
     8  1357222 Andrena labialis (K… Andrena labi… SPEC… ACCEP…        100 EXACT    
     9  1357207 Andrena labiata Fab… Andrena labi… SPEC… ACCEP…        100 EXACT    
    10  1357013 Andrena minutula (K… Andrena minu… SPEC… ACCEP…        100 EXACT    
    # ℹ 82 more rows
    # ℹ 19 more variables: kingdom <chr>, phylum <chr>, order <chr>, family <chr>,
    #   genus <chr>, species <chr>, kingdomKey <int>, phylumKey <int>,
    #   classKey <int>, orderKey <int>, familyKey <int>, genusKey <int>,
    #   speciesKey <int>, synonym <lgl>, class <chr>, acceptedUsageKey <int>,
    #   verbatim_name <chr>, verbatim_index <dbl>, verbatim_order <chr>

``` r
unmatched <-  gbif_names |> filter(matchType != "EXACT") #10 unmached
unmatched
```

    # A tibble: 10 × 26
       usageKey scientificName       canonicalName rank  status confidence matchType
          <int> <chr>                <chr>         <chr> <chr>       <int> <chr>    
     1  1357013 Andrena minutula (K… Andrena minu… SPEC… ACCEP…        100 FUZZY    
     2  1341976 Apis mellifera Linn… Apis mellife… SPEC… ACCEP…        100 FUZZY    
     3  1341976 Apis mellifera Linn… Apis mellife… SPEC… ACCEP…        100 FUZZY    
     4  1340301 Bombus lapidarius (… Bombus lapid… SPEC… ACCEP…        100 FUZZY    
     5  1340344 Bombus rupestris (F… Bombus rupes… SPEC… ACCEP…        100 FUZZY    
     6  1338466 Coelioxys alatus Fö… Coelioxys al… SPEC… ACCEP…        100 FUZZY    
     7  1353310 Halictus simplex Bl… Halictus sim… SPEC… ACCEP…         99 FUZZY    
     8  1353234 Halictus subauratus… Halictus sub… SPEC… ACCEP…         93 FUZZY    
     9  1354041 Lasioglossum villos… Lasioglossum… SPEC… ACCEP…        100 FUZZY    
    10  5039340 Osmia aurulenta (Pa… Osmia aurule… SPEC… ACCEP…         93 FUZZY    
    # ℹ 19 more variables: kingdom <chr>, phylum <chr>, order <chr>, family <chr>,
    #   genus <chr>, species <chr>, kingdomKey <int>, phylumKey <int>,
    #   classKey <int>, orderKey <int>, familyKey <int>, genusKey <int>,
    #   speciesKey <int>, synonym <lgl>, class <chr>, acceptedUsageKey <int>,
    #   verbatim_name <chr>, verbatim_index <dbl>, verbatim_order <chr>

``` r
unique(unmatched$matchType)
```

    [1] "FUZZY"

``` r
#"FUZZY" because of misspelling   

#Checking unmatched names
fixing <- unmatched |>  select(scientificName, matchType, verbatim_name)
#When it is FUZZY it seems to find the correct name easily
#Be careful other situations can also happen

#Creating a data.frame that contents old name with typo and new names with corrected scientific name

fixing <- fixing |>
  rename(Bee_name = verbatim_name) |>
  mutate(sci_name =  word(scientificName, 1,2)) |>
  select(-c(scientificName, matchType))
fixing
```

    # A tibble: 10 × 2
       Bee_name               sci_name               
       <chr>                  <chr>                  
     1 Andrena mynutula       Andrena minutula       
     2 Apis melifera          Apis mellifera         
     3 Apis melllifera        Apis mellifera         
     4 Bombus lapydarius      Bombus lapidarius      
     5 Bombus rupestriss      Bombus rupestris       
     6 Coelioxys alata        Coelioxys alatus       
     7 Halictus simple        Halictus simplex       
     8 Hallictus subauratus   Halictus subauratus    
     9 Lasioglossum vilosulum Lasioglossum villosulum
    10 Osma aurulenta         Osmia aurulenta        

``` r
#3) Creating a dataframe with a new column with the Fixed names: Fixed_bee_name

df.e <- df.e |>
  left_join(fixing) |>
  mutate(Fixed_bee_name = ifelse(is.na(sci_name), Bee_name, sci_name)) |>
  select(- sci_name)
```

    Joining with `by = join_by(Bee_name)`

``` r
df.e
```

    # A tibble: 1,052 × 4
       Country Date     Bee_name               Fixed_bee_name         
       <chr>   <chr>    <chr>                  <chr>                  
     1 Germany 7-6-2022 Coelioxys alata        Coelioxys alatus       
     2 Germany 7-6-2022 Andrena chrysosceles   Andrena chrysosceles   
     3 Germany 7-6-2022 Bombus lapydarius      Bombus lapidarius      
     4 Germany 7-6-2022 Lasioglossum vilosulum Lasioglossum villosulum
     5 Germany 7-6-2022 Anthidium byssinum     Anthidium byssinum     
     6 Germany 7-6-2022 Bombus lapidarius      Bombus lapidarius      
     7 Germany 7-6-2022 Lasioglossum pauxillum Lasioglossum pauxillum 
     8 Germany 7-6-2022 Andrena alfkenella     Andrena alfkenella     
     9 Germany 7-6-2022 Osma aurulenta         Osmia aurulenta        
    10 Germany 4-8-2022 Bombus lapidarius      Bombus lapidarius      
    # ℹ 1,042 more rows

## Leer pdf en R y limpiar textos

La función `pdf_data` del paquete `pdftools` permite extraer el texto de
un archivo pdf. Sin embargo, si existen fotografías o figuras no siempre
es posible extraer el texto de estos o puede haber errores al leer el
archivo. En estos casos se puede usar la función `pdf_ocr_text` para
extraer el texto mediante reconocimiento óptico (OCR).

``` r
library(pdftools)
```

    Warning: package 'pdftools' was built under R version 4.4.2

    Using poppler version 23.08.0

``` r
library(purrr)

#folder path
folder_path <- here("texts/")

#PDF files: Notas Ecoinformáticas + description of Ecoinformatica group
pdf_files <- list.files(folder_path, pattern = "\\.pdf$", full.names = TRUE)

#Function to extract text from PDFs already in tidyformat 
read_pdf <- function(doc) {
  t <- pdf_data(doc) 
  pt <- map_chr(t, ~ .x |>  select(text) |>  unlist() |>  paste0(collapse = " "))
  textocompleto <- paste0(pt, collapse = " ")
  
  # If the extracted text is too short, use OCR
  if (nchar(textocompleto) < 15) {
    textocompleto <- paste0(pdf_ocr_text(doc), collapse = " ")
  }
  
  # Return a tibble with the document name and extracted text
  tibble(document = doc, text = textocompleto)
}

textos <- map(pdf_files, read_pdf)

textos_df <- bind_rows(textos) |> 
  mutate(document = str_replace(document, pattern = paste0(folder_path, "/"), replacement = ""))

str(textos_df)
```

    tibble [18 × 2] (S3: tbl_df/tbl/data.frame)
     $ document: chr [1:18] "1416-Texto del artículo-5034-1-10-20170426.pdf" "1451-Texto del artículo-5298-1-10-20170801.pdf" "1524-Texto del artículo-5730-3-10-20171212.pdf" "1570-Texto del artículo-5970-1-10-20180421.pdf" ...
     $ text    : chr [1:18] "AEET ASOCIACIÓN ESPAÑOLA DE ECOLOGÍA TERRESTRE Ecosistemas 26(1): 126-127 [Enero-abril 2017] Doi.: 10.7818/ECOS"| __truncated__ "AEET ASOCIACIÓN ESPAÑOLA DE ECOLOGÍA TERRESTRE ecosistemas Ecosistemas 26(2): 64-66 [Mayo-Agosto 2017] Doi.: 10"| __truncated__ "AEET ASOCIACIÓN ESPAÑOLA DE ECOLOGÍA TERRESTRE Ecosistemas 26(3): 110-111 [Septiembre-Diciembre 2017] Doi.: 10."| __truncated__ "AEET ASOCIACIÓN ESPAÑOLA DE ECOLOGÍA TERRESTRE Ecosistemas 27(1): 128-129 [Enero-Abril 2018] Doi.: 10.7818/ECOS"| __truncated__ ...

``` r
unique(textos_df$document)
```

     [1] "1416-Texto del artículo-5034-1-10-20170426.pdf" 
     [2] "1451-Texto del artículo-5298-1-10-20170801.pdf" 
     [3] "1524-Texto del artículo-5730-3-10-20171212.pdf" 
     [4] "1570-Texto del artículo-5970-1-10-20180421.pdf" 
     [5] "1591-Texto del artículo-6177-1-10-20180729.pdf" 
     [6] "1604-Texto del artículo-6174-1-10-20180729.pdf" 
     [7] "1699-Texto del artículo-6714-1-10-20190508.pdf" 
     [8] "1838-Texto del artículo-7395-1-10-20191228.pdf" 
     [9] "1880-Texto del artículo-7465-1-10-20200114.pdf" 
    [10] "1948-Texto del artículo-7770-1-10-20200424.pdf" 
    [11] "1995-Texto del artículo-8172-1-10-20200720.pdf" 
    [12] "2129-Texto del artículo-8660-1-10-20201228.pdf" 
    [13] "2256-Texto del artículo-10468-1-10-20210709.pdf"
    [14] "2332-Texto del artículo-11367-1-10-20220420.pdf"
    [15] "2527-Texto del artículo-12698-1-10-20230411.pdf"
    [16] "2745-Texto del artículo-14476-9-10-20241126.pdf"
    [17] "2797-Texto del artículo-14383-3-10-20240709.pdf"
    [18] "Ecoinformática-AEET.pdf"                        

## Limpieza de textos

El análisis de textos suele requerir un trabajo previo de limpieza de
caracteres y palabras. Cuestiones simples como el uso de mayúsculas y
minúsculas, fechas, horas y otros números, signos de puntuación, etc.
pueden limitar la capacidad del procesamiento de datos por parte de los
ordenadores y sesgar el análisis de textos. Por ello, los textos suelen
requerir un proceso de normalización antes de ser procesados.

Dos puntos claves a tener en cuenta a la hora de analizar textos en la
limpieza de textos son la eliminación de palabras vacías (stop words) y
la normalización de palabras.

En los lenguajes naturales hay una serie de palabras que no aportan
significado al texto, como preposiciones, conjunciones, artículos, etc,
pero que suelen aparecer muy frecuentemente. Estas palabras se conocen
como palabras vacías (stop words). No hay una lista definitiva de
palabras vacías, pero distintos paquetes ofrecen propuestas de palabras
vacías para hacer filtrado de las mismas. En R, el paquete `tidytext`
tiene una lista de palabras vacías en inglés que se pueden usar para
eliminarlas de los textos. En español, el paquete `tm` tiene una lista
de palabras vacías en castellano que se pueden usar para eliminarlas de
los textos.

Además, en el lenguaje muy frecuentemente utilizamos conjuntos de
palabras, combinaciones léxicas, que suelen ir juntas en el lenguaje
natural y tienen un significado conjunto, como por ejemplo “análisis
estadístico” y que su análisis debería hacerse preferencialmente de
manera conjunta.

Existen otros aspectos de este tipo a tener en cuenta antes de realizar
análisis de texto, pero que yo no voy a ahondar en ellos en este
seminario. Un ejemplo es la lematización (*stemming*) que consiste en
reducir las palabras a su raíz o lema, de manera que se evita la
duplicación devido a plurales, la conjugación de verbos, palabras
derivadas, etc..

``` r
library(textclean)
```

    Warning: package 'textclean' was built under R version 4.4.2

``` r
library(tidytext)
```

    Warning: package 'tidytext' was built under R version 4.4.2

``` r
library(tm)
```

    Warning: package 'tm' was built under R version 4.4.2

    Loading required package: NLP

    Warning: package 'NLP' was built under R version 4.4.2


    Attaching package: 'NLP'

    The following object is masked from 'package:ggplot2':

        annotate

``` r
stop_words_es <- bind_rows(stop_words,
                               tibble(word = tm::stopwords("spanish"),
                                          lexicon = "custom")) |> 
  filter(lexicon == "custom")

word_c <- textos_df |> 
  select(text) |> 
  slice(18) |>
  str_to_lower() |> 
  replace_date(replacement = "") |> # remove dates
  replace_time(replacement = "") |> # remove time
  str_remove_all(pattern = "[[:punct:]]") |> # remove punctuation
  str_remove_all(pattern = "[^\\s]*[0-9][^\\s]*") |>  # remove mixed string n number
  #replace_contraction() |>  # replace contraction to their multi-word forms
  #replace_word_elongation() |>  # replace informal writing with known semantic replacements
  str_squish() |>  # reduces repeated whitespace inside a string
  str_trim() |>  # removes whitespace from start and end of string
  str_replace_all("análisis estadístico", "análisis_estadístico") |> #avoiding collocations
  str_replace_all("proyectos de investigación", "proyectos") |> #avoiding collocations
  str_replace_all("bases de datos", "bases_datos") |> 
  str_replace_all("grupo de trabajo", "grupo_trabajo") |> #avoiding collocations
  str_replace_all("asociación española de ecología terrestre", "AEET") |>  #avoiding collocations
  str_split(boundary("word")) |> 
  unlist() |>
  as_tibble(value = .) |>
  rename(word = value) |>
  anti_join(stop_words_es) |>  # remove stop words
  filter(!str_detect(word, "^http"))  |>  #removes webpages
  filter(!str_detect(word, "^www"))  |>  #removes webpages
  filter(!str_detect(word, "^doi")) |> #removes bibliographic repetitions
  filter(!str_detect(word, "^pp")) |>  #removes bibliographic repetitions
  filter(!str_detect(word, "^al")) |>  #removes bibliographic repetitions
  drop_na() |> 
  group_by(word) |> 
  mutate(countWords = n()) |> 
  distinct() |> arrange(desc(countWords))
```

    Joining with `by = join_by(word)`

## Análisis de textos: Frecuencia y nubes de palabras

Una cuestión central en el análisis de textos es cómo cuantificar de qué
trata un documento. Una de las formas más directas y simples es contar
las palabras que aparecen en el texto. Las nubes de palabras sirven para
visualizar las palabras más frecuentes en un texto de manera sencilla.
En R, el paquete `ggwordcloud` permite hacer nubes de palabras de forma
sencilla. Existen otros paquetes que permiten hacer nubes de palabras,
como `wordcloud2`. Puedes consultar estos recursos para más información:
<https://r-graph-gallery.com/wordcloud.html>

``` r
library(ggwordcloud)
```

    Warning: package 'ggwordcloud' was built under R version 4.4.2

``` r
word_c |>
  ggplot( aes(label = word, size = countWords)) +
  geom_text_wordcloud() +
  theme_minimal()
```

![](Seminario_AnalisisTextosEcologia_files/figure-commonmark/nubes%20palabras-1.png)

En este tutorial explican como mejorar la estética y composición de las
nubes de palabras:
<https://cran.r-project.org/web/packages/ggwordcloud/vignettes/ggwordcloud.html>.

Otros análisis basados en la frecuencia de palabras son usados para
derivar la importancia de las palabras en un conjunto de textos. Puedes
echar un vistazo a este tutorial <https://www.tidytextmining.com/tfidf>

## Análisis de textos: Relación entre palabras

En el análisis de textos es común que queramos analizar una colección de
textos relacionados entre sí (por ejemplo todos los textos de un mismo
autor, o todos los artículos resultantes de una misma búsqueda de
palabras clave). El conjunto de textos conforman el *corpus*. Los
textos, a su vez, pueden dividirse en unidades más pequeñas de interés
para el análisis, como por ejemplo párrafos, oraciones o palabras. Estas
unidades se conocen como *tokens*.

El paquete `tidytext` permite facilita la *tokenización* del corpus.

Es común que para el análisis de textos se construyan matrices de
términos-documentos, donde las filas representan los términos y las
columnas los documentos, parecidas a las matrices de presencias y
ausencias que utilizamos en ecología. Sin embargo, en la filosofía tidy
se mantendría la estructura de un *toke* por fila.

Existen diversas metodologías y marcos teóricos a la hora de hacer
análisis de textos. Entre ellas, el estudio de la relación entre
palabras es una de las más comunes. En esta aproximación se examina qué
palabras tienden a seguir a otras inmediatamente, o qué palabras tienden
a co-ocurrir dentro de los mismos documentos. En este segundo análisis
me voy a centrar en este seminario.

Existen otras muchas formas de analizar textos como por ejemplo a través
del análisis de sentimientos, la clasificación de textos, modelización
de temas, etc. Aquí podéis encontrar más información sobre el análisis
de textos en R: <https://guides.library.upenn.edu/penntdm/r>.

``` r
library(tidytext) #text miny under tidy philosophy
library(widyr) #widen, process & re-tidy data (calculates paiwise correlations and distance)
```

    Warning: package 'widyr' was built under R version 4.4.2

``` r
clean_text_R_corr_20 <- function(text) {
  corpus <- text |>  
  str_to_lower() |> 
  replace_date(replacement = "") |> # remove dates
  replace_time(replacement = "") |> # remove time
  str_remove_all(pattern = "[[:punct:]]") |> # remove punctuation
  str_remove_all(pattern = "[^\\s]*[0-9][^\\s]*") |>  # remove mixed string n number
  replace_contraction() |>  # replace contraction to their multi-word forms
  replace_word_elongation() |>  # replace informal writing with known semantic replacements
  str_squish() |>  # reduces repeated whitespace inside a string
  str_trim() |>  # removes whitespace from start and end of string
  str_replace_all("análisis estadístico", "análisis_estadístico") |> #avoiding collocations
  str_replace_all("proyectos de investigación", "proyectos") |> #avoiding collocations
  str_replace_all("bases de datos", "bases_datos") |> 
  str_replace_all("buenas prácticas", "buenas_prácticas") |> 
  str_replace_all("grupo de trabajo", "grupo_trabajo") |> #avoiding collocations
  str_replace_all("asociación española de ecología terrestre", "AEET") |>  #avoiding collocations
  str_remove_all(pattern = "^http*") |>  #not working ¿?
  str_remove_all(pattern = "^www*") |>  # remove webpages
  as_tibble(value = .) |>
  rename(text = value) 
  
unigram_probs <- corpus |> 
  unnest_tokens(word, text, strip_numeric = TRUE) |>  
  count(word, sort = TRUE) |> 
  mutate(p = n / sum(n))

tidy_skipgrams <- corpus |> 
  unnest_tokens(ngram, text, token = "ngrams", n = 20) |> 
  mutate(ngramID = row_number()) |>  
  unnest_tokens(word, ngram)


skipgram_probs <- tidy_skipgrams |> 
  pairwise_count(word, ngramID, diag = TRUE, sort = TRUE) |>  #Count the number of times each pair of items appear together within a group defined by "feature."
  mutate(p = n / sum(n))


normalized_prob <- skipgram_probs |> 
  filter(n > 20) |> 
  rename(word1 = item1, word2 = item2) |> 
  left_join(unigram_probs |> 
              select(word1 = word, p1 = p),
            by = "word1") |> 
  left_join(unigram_probs |> 
              select(word2 = word, p2 = p),
            by = "word2") |> 
  mutate(p_together = p / p1 / p2)

output <- normalized_prob |> 
  filter(word1 == "ecoinformática") |> 
  filter(!word1 %in% stop_words_es$word) |> 
  filter(!word2 %in% stop_words_es$word) |> 
  arrange(-p_together)

return(output)
  
}


textos_df_clean <- textos_df |>
  mutate(r_corr_gram = map(text, clean_text_R_corr_20))


textos_df_clean |>
  unnest(r_corr_gram) |>
  select(document, word2, p_together)
```

    # A tibble: 22 × 3
       document                                        word2          p_together
       <chr>                                           <chr>               <dbl>
     1 1570-Texto del artículo-5970-1-10-20180421.pdf  ecoinformática      25.7 
     2 1570-Texto del artículo-5970-1-10-20180421.pdf  grupo_trabajo       23.1 
     3 1570-Texto del artículo-5970-1-10-20180421.pdf  iavs                18.0 
     4 2256-Texto del artículo-10468-1-10-20210709.pdf ecoinformática      67.9 
     5 2256-Texto del artículo-10468-1-10-20210709.pdf nota                21.5 
     6 2256-Texto del artículo-10468-1-10-20210709.pdf cienciometría        8.48
     7 2332-Texto del artículo-11367-1-10-20220420.pdf ecoinformática      63.0 
     8 2527-Texto del artículo-12698-1-10-20230411.pdf ecoinformática     201.  
     9 2745-Texto del artículo-14476-9-10-20241126.pdf ecoinformática      60.1 
    10 2745-Texto del artículo-14476-9-10-20241126.pdf verónica            41.1 
    # ℹ 12 more rows

``` r
textos_df_clean |>
  unnest(r_corr_gram) |>
  select(document, word2, p_together) |> 
  filter(!word2 %in% c("ecoinformática", "verónica", "cruzalonso", "cualquier"))|>
  distinct( word2, p_together) |> 
  ggplot( aes(label = word2, size = p_together*10)) +
  geom_text_wordcloud() +
  theme_minimal()
```

![](Seminario_AnalisisTextosEcologia_files/figure-commonmark/relacion%20palabras-1.png)

## Recursos y agradecimientos

El contenido de este seminario se basa en el código desarrollado para la
publicación *Reassessing science communication for effective farmland
biodiversity conservation*
<https://www.sciencedirect.com/science/article/pii/S0169534724000326>.

Las distintas secciones se han desarrollado utilizando la información de
distintos tutoriales, libros y páginas web. La gran mayoría citadas a lo
largo del seminario. Otras fuentes de interés para el manejo de
caracteres y el análisis de textos en R son:
<https://link.springer.com/book/10.1007/978-3-319-03164-4>
<http://jvera.rbind.io/post/2017/10/16/spanish-stopwords-for-tidytext-package/>
<https://bookdown.org/dereksonderegger/444/string-manipulation.html>
<https://github.com/gagolews/stringi?tab=readme-ov-file>
<https://www.tidytextmining.com/>
<https://users.ssc.wisc.edu/~hemken/Rworkshops/dwr/character.html>

La limpieza de datos utilizando gbif es una sugerencia de Jose Lanuza:
<https://scholar.google.com/citations?user=F6C5gFcAAAAJ&hl=en> y el
código está inspirado en el código desarrollado para la base de datos de
abejas de la Península Ibérica
<https://github.com/ibartomeus/IberianBees>.

Ira Hannappel <https://www.uni-goettingen.de/en/659155.html> me ha
cedido los datos de abejas utilizados para la limpieza de nombres
taxonómicos.

La plantilla de quarto ha sido copiada del repositorio
<https://github.com/DatSciR/ciencia_datos/tree/main>

En la creación de estos materiales se ha utilizado R copilot.

Agradecer a la Asociación Española de Ecología Terrestre y al Grupo de
Ecoinformática la oportunidad de impartir este seminario. Especialmente
a Verónica Cruz Alonso, Julen Astigarraga y Elena Quintero por su papel
de organizadores.
