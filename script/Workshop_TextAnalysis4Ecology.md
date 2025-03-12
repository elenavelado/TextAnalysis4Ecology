# Text analysis with R: Useful examples for ecology
<img src="images/logoAF.jpg" />

Elena Velado-Alonso
12/03/2025

- [<span class="toc-section-number">1</span> Text analysis with R:
  useful examples for
  ecology.](#text-analysis-with-r-useful-examples-for-ecology)
- [<span class="toc-section-number">2</span> Characters in R
  base](#characters-in-r-base)
- [<span class="toc-section-number">3</span> Package `stringr`:
  characters with tidy
  philosophy](#package-stringr-characters-with-tidy-philosophy)
- [<span class="toc-section-number">4</span> Automatic cleaning of
  taxonomic names with
  gbif](#automatic-cleaning-of-taxonomic-names-with-gbif)
- [<span class="toc-section-number">5</span> Read pdf in R and clean up
  text](#read-pdf-in-r-and-clean-up-text)
- [<span class="toc-section-number">6</span> Text
  cleaning](#text-cleaning)
- [<span class="toc-section-number">7</span> Text analysis: Frequency
  and word clouds](#text-analysis-frequency-and-word-clouds)
- [<span class="toc-section-number">8</span> Text analysis: Relationship
  between words](#text-analysis-relationship-between-words)
- [<span class="toc-section-number">9</span> Resources and
  acknowledgements](#resources-and-acknowledgements)

## Text analysis with R: useful examples for ecology.

Working with characters (string data) in R is often perceived as an
difficult task by many users. Typically, training in programming and
data analysis focuses on numerical and analytical issues, but working
with characters is rarely addressed in depth. However, work in ecology
requires frequent work with characters. In addition, the increasing use
of already available databases and [FAIR
principles](https://www.go-fair.org/fair-principles/) is increasing the
information presented in characters and the need to know how to work
with them in programming environments. Therefore, not knowing how to
work with characters limits the reproducibility of ecology workflows.

<img src="images/meme1.png" data-fig-align="left" />

In this workshop, we will:

- review basic concepts of working with characters in R

- look at practical examples for automatic cleanup of taxonomic names

- learn how to read pdf files in R

- clean up texts for further analysis

- make word cloud graphs

- do simple calculations of word frequencies in texts and correlation
  between words for content analysis.

## Characters in R base

In R, texts are represented as character strings. Characters are defined
with single or double quotes and can be stored in objects of class
‘character’. For example:

``` r
"this is a text"
```

    [1] "this is a text"

``` r
a <- "this is another text"
a
```

    [1] "this is another text"

``` r
class(a)
```

    [1] "character"

Inside character strings, quotation marks can be used. If single quotes
are used, double quotes can be used inside the text and vice versa. For
example:

``` r
x <- "Germany has the 'best' weather"
x
```

    [1] "Germany has the 'best' weather"

You can concatenate characters with `paste()` function, which also
allows you to define how the concatenation is performed. Also, `rep()`
function helps us to repeat elements of a vector. For example:

``` r
name <- "Erika"

surname <- "Mustermann"

paste(name, surname)
```

    [1] "Erika Mustermann"

``` r
paste(name, surname, sep = "_")
```

    [1] "Erika_Mustermann"

``` r
paste0(name, surname)
```

    [1] "ErikaMustermann"

``` r
paste(rep(name, 4), collapse = '')
```

    [1] "ErikaErikaErikaErika"

Different functions can help us to make characters visible:

- `print()` allows us to print the contents of an object.

- `noquote()` allows us to print the contents of an object without
  quotes.

- `cat()` concatenates and prints and also allows us to define how the
  contents of an object are printed.

``` r
name <- "Erika"

surname <- "Mustermann"

scientist <- paste(name, surname)

print(scientist)
```

    [1] "Erika Mustermann"

``` r
noquote(scientist)
```

    [1] Erika Mustermann

``` r
cat(letters, sep = "-") 
```

    a-b-c-d-e-f-g-h-i-j-k-l-m-n-o-p-q-r-s-t-u-v-w-x-y-z

You can count the number of elements in a string object with `length()`
function.`nchar()` function gives the number of characters in a string.

``` r
length(scientist)
```

    [1] 1

``` r
nchar(scientist)
```

    [1] 16

``` r
length(letters)
```

    [1] 26

Characters can be converted to upper or lower case with the `toupper()`
and `tolower()` functions:

``` r
toupper(scientist)
```

    [1] "ERIKA MUSTERMANN"

``` r
tolower(scientist)
```

    [1] "erika mustermann"

To replace any of the characters in a string, you can use the `chartr()`
function. For example:

``` r
x <- "Funktional AgrobiodÑversity & Agroecology is the best group"
chartr(old = "Funktional AgrobiodÑversity & Agroecology", new = "Functional Agrobiodiversity & Agroecology", x)
```

    [1] "Functional Agrobiodiversity & Agroecology is the best group"

To extract parts of a string, `substr()` function can be used.

``` r
substr(x, start = 1, stop = 10)
```

    [1] "Funktional"

``` r
substr(x, start = 18, stop = 23)
```

    [1] "odÑver"

You can also split a string into smaller parts with `strsplit()`
function. For example, to separate the words of a sentence:

``` r
x <- "Functional Agrobiodiversity & Agroecology is the best group"
y <- strsplit(x, split = " ")
y
```

    [[1]]
    [1] "Functional"       "Agrobiodiversity" "&"                "Agroecology"     
    [5] "is"               "the"              "best"             "group"           

We can also identify character patterns with `grep()` function. For
example, to identify words containing the letter ‘y’, words beginning
with ‘F’ or words ending in ‘p’:

``` r
z <- c("Functional", "Agrobiodiversity", "&", "Agroecology", "is",          "the", "best", "group" )
  
grep("y", z, value=TRUE)
```

    [1] "Agrobiodiversity" "Agroecology"     

``` r
grepl("y", z)
```

    [1] FALSE  TRUE FALSE  TRUE FALSE FALSE FALSE FALSE

``` r
grep("^F", z, value=TRUE)
```

    [1] "Functional"

``` r
grep("p$", z, value=TRUE)
```

    [1] "group"

To specify the position in a string you have to use regular expressions,
for example ‘^’ for characters at the beginning and ‘\$’ for characters
at the end. Therefore, some characters are used as metacharacters with a
special meaning in regular expressions. The full stop (.), the asterisk
(\*), the question mark (?), the plus sign (+), the caret (^), the
dollar sign (\$), the square brackets (\[, \]), the braces ({, }), the
hyphen (-), among others, have a non-literal meaning. You can read more
information for example here:

``` r
?regex
```

or here <https://uc-r.github.io/regex#regex_functions_base>.

In my opinion, regular expressions and metacharacters are one of the
difficulties we encounter when working with characters in base R. Some
features of the ‘tidy’ packages simplify working with regular
expressions, as we will see below.

To modify the content of a string, `sub()` and `gsub()` functions can be
used. The `sub()` function replaces the first occurrence of a pattern in
a string, while `gsub()` replaces all occurrences of a pattern in a
string. For example:

``` r
info <- "Functional Agrobiodiversity & Agroecology Group is guided by principles of ecological intensification to maintain or increase long-term agricultural productivity while protecting and enhancing biodiversity"
info
```

    [1] "Functional Agrobiodiversity & Agroecology Group is guided by principles of ecological intensification to maintain or increase long-term agricultural productivity while protecting and enhancing biodiversity"

``` r
new_info <- gsub("Functional Agrobiodiversity & Agroecology Group", "FA&A", info)
new_info
```

    [1] "FA&A is guided by principles of ecological intensification to maintain or increase long-term agricultural productivity while protecting and enhancing biodiversity"

Base R has a number of useful functions for comparing two character
vectors.

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

I frequently use these functions to work with species and taxonomic
names. For example:

``` r
species1 <- c("Protaetia morio", "Agrypnus murinus", "Stenobothrus lineatus", "Philoscia muscorum")

species2 <- c("Protaetia morio", "Tettigonia cantans", "Stenobothrus lineatus", "Mangora acalypha")

union(species1, species2) #all unique species
```

    [1] "Protaetia morio"       "Agrypnus murinus"      "Stenobothrus lineatus"
    [4] "Philoscia muscorum"    "Tettigonia cantans"    "Mangora acalypha"     

``` r
intersect(species1, species2) #common species
```

    [1] "Protaetia morio"       "Stenobothrus lineatus"

``` r
setdiff(species1, species2) #species in species1 but not in species2
```

    [1] "Agrypnus murinus"   "Philoscia muscorum"

``` r
setdiff(species2, species1) #species in species2 but not in species1
```

    [1] "Tettigonia cantans" "Mangora acalypha"  

``` r
setequal(species1, species2) #are they equal?
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

One of my favourite functions for finding errors when working with
species names is the `agrep` function which allows you to search for
approximate patterns in a character vector.

``` r
bee_data <- data.frame(
  Species = c("Andrena_chrysosceles", "Lasioglossum_villosulum", "Bombus_silvarum",
              "Halictus_tumulorum", "Apis_mellifera"),
  Abundance = c(20, 15, 8, 12, 5)
)

"Bombus_sylvarum" %in% bee_data
```

    [1] FALSE

``` r
agrep("Bombus_sylvarum", bee_data$Species, value = TRUE)
```

    [1] "Bombus_silvarum"

## Package `stringr`: characters with tidy philosophy

`stringr` is a package developed to handle characters under the tidy
philosophy, which enhances some applications such as working with NA.

There are four main families of functions in `stringr`:

- Character manipulation: these functions allow to manipulate individual
  characters within vectors.

- Whitespace tools for adding, removing and manipulating whitespace.

- Location-sensitive operations.

- Pattern matching functions, including regular expressions.

``` r
library(stringr)
scientist
```

    [1] "Erika Mustermann"

``` r
str_length(scientist) #== nchart()
```

    [1] 16

``` r
str_sub(scientist, start = 2, end = -2) #== substr()
```

    [1] "rika Musterman"

``` r
str_to_upper(scientist) #== toupper()
```

    [1] "ERIKA MUSTERMANN"

``` r
str_to_lower(scientist) #== tolower()
```

    [1] "erika mustermann"

``` r
hello <- "Hallo Functional Agrobiodiversity & Agroecology Group"
hello
```

    [1] "Hallo Functional Agrobiodiversity & Agroecology Group"

``` r
new_hello <- str_replace(hello, "Functional Agrobiodiversity & Agroecology Group", "FA&A ")
new_hello
```

    [1] "Hallo FA&A "

``` r
str_trim(new_hello) #removes whitespaces
```

    [1] "Hallo FA&A"

``` r
str_split(new_hello, "") #split characteres
```

    [[1]]
     [1] "H" "a" "l" "l" "o" " " "F" "A" "&" "A" " "

``` r
print(bee_data$Species)
```

    [1] "Andrena_chrysosceles"    "Lasioglossum_villosulum"
    [3] "Bombus_silvarum"         "Halictus_tumulorum"     
    [5] "Apis_mellifera"         

``` r
str_detect("Apis_mellifera", bee_data$Species) #== grepl()
```

    [1] FALSE FALSE FALSE FALSE  TRUE

``` r
str_replace("Bombus_mellifera", "Bombus", "Apis") #== gsub()
```

    [1] "Apis_mellifera"

``` r
hummeln <- c("Bombus_silvarum", "Bombus_terrestris", "Bombus_pascuorum", "Bombus_pratorum", "Bombus_humilis")

str_count(hummeln, "Bombus") #counts the number of times a pattern appears
```

    [1] 1 1 1 1 1

``` r
str_locate(hummeln, "Bombus") #locates the position of a pattern
```

         start end
    [1,]     1   6
    [2,]     1   6
    [3,]     1   6
    [4,]     1   6
    [5,]     1   6

``` r
str_match(hummeln, "Bombus") #returns the found pattern
```

         [,1]    
    [1,] "Bombus"
    [2,] "Bombus"
    [3,] "Bombus"
    [4,] "Bombus"
    [5,] "Bombus"

``` r
str_split(hummeln, "_") #== strsplit()
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
poem <- c("Freude, schöner Götterfunken, Tochter ",
          "aus Elysium, Wir betreten feuertrunken,",
          "Himmlische, dein Heiligtum. Deine Zauber",
          "binden wieder, Was die Mode streng geteilt;",
          "Alle Menschen werden Brüder, Wo dein sanfter",
          " Flügel weilt.")


poem
```

    [1] "Freude, schöner Götterfunken, Tochter "      
    [2] "aus Elysium, Wir betreten feuertrunken,"     
    [3] "Himmlische, dein Heiligtum. Deine Zauber"    
    [4] "binden wieder, Was die Mode streng geteilt;" 
    [5] "Alle Menschen werden Brüder, Wo dein sanfter"
    [6] " Flügel weilt."                              

``` r
str_flatten(poem) #== paste()
```

    [1] "Freude, schöner Götterfunken, Tochter aus Elysium, Wir betreten feuertrunken,Himmlische, dein Heiligtum. Deine Zauberbinden wieder, Was die Mode streng geteilt;Alle Menschen werden Brüder, Wo dein sanfter Flügel weilt."

``` r
cat(str_wrap(poem, width = 30)) #adapt the text to a specific width
```

    Freude, schöner Götterfunken,
    Tochter aus Elysium, Wir betreten
    feuertrunken, Himmlische, dein Heiligtum.
    Deine Zauber binden wieder, Was die Mode
    streng geteilt; Alle Menschen werden Brüder,
    Wo dein sanfter Flügel weilt.

## Automatic cleaning of taxonomic names with gbif

In ecology, it is common to work with taxonomic names of species. These
names may contain typographical errors, abbreviations, synonyms, etc.
that make their analysis difficult. Although detailed manual cleaning is
recommended, an automatic taxonomic name cleaning approach can be done
with the GBIF Base Taxonomy
<https://www.gbif.org/dataset/d7dddbf4-2cf0-4f39-9b2a-bb099caae36c> and
`rgbif` package.

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

## Read pdf in R and clean up text

`pdf_data` function from the `pdftools` package allows to extract text
from a pdf file. However, if the pdf includes pictures or figures, it is
not always possible to extract the text correctly. In these cases,
`pdf_ocr_text` function can be used to extract the text using optical
text recognition (OCR).

``` r
library(pdftools)
```

    Warning: package 'pdftools' was built under R version 4.4.2

    Using poppler version 23.08.0

``` r
library(purrr)

#folder path
folder_path <- here("researchers/")

#PDF files: Web description Team of Functional Agrobiodiversity
pdf_files <- list.files(folder_path, pattern = "\\.pdf$", full.names = TRUE)

#Function to extract text from PDFs already in tidyformat 
read_pdf <- function(doc) {
  t <- pdf_data(doc) 
  pt <- map_chr(t, ~ .x |>  select(text) |>  unlist() |>  paste0(collapse = " "))
  completetext <- paste0(pt, collapse = " ")
  
  # If the extracted text is too short, use OCR
  if (nchar(completetext) < 15) {
    tcompletetext  <- paste0(pdf_ocr_text(doc), collapse = " ")
  }
  
  # Return a tibble with the document name and extracted text
  tibble(document = doc, text = completetext )
}

web_texts <- map(pdf_files, read_pdf)

web_texts_df <- bind_rows(web_texts) |> 
  mutate(document = str_replace(document, pattern = paste0(folder_path, "/"), replacement = ""))

str(web_texts_df)
```

    tibble [17 × 2] (S3: tbl_df/tbl/data.frame)
     $ document: chr [1:17] "Alfred Kok - Georg-August-Universität Göttingen.pdf" "Dr. Annika Haß - Georg-August-Universität Göttingen.pdf" "Dr. Carolina Ocampo Ariza - Georg-August-Universität Göttingen.pdf" "Dr. Elena Velado Alonso - Georg-August-Universität Göttingen.pdf" ...
     $ text    : chr [1:17] "3/10/25, 5:30 PM Alfred Kok - Georg-August-Universität Göttingen Funktionelle Agrobiodiversität & Agrarökologie"| __truncated__ "3/10/25, 5:26 PM Dr. Annika Haß - Georg-August-Universität Göttingen Functional Agrobiodiversity & Agroecology "| __truncated__ "3/10/25, 5:27 PM Dr. Carolina Ocampo Ariza - Georg-August-Universität Göttingen Functional Agrobiodiversity & A"| __truncated__ "3/10/25, 5:28 PM Dr. Elena Velado Alonso - Georg-August-Universität Göttingen Functional Agrobiodiversity & Agr"| __truncated__ ...

``` r
unique(web_texts_df$document)
```

     [1] "Alfred Kok - Georg-August-Universität Göttingen.pdf"               
     [2] "Dr. Annika Haß - Georg-August-Universität Göttingen.pdf"           
     [3] "Dr. Carolina Ocampo Ariza - Georg-August-Universität Göttingen.pdf"
     [4] "Dr. Elena Velado Alonso - Georg-August-Universität Göttingen.pdf"  
     [5] "Dr. Marco Ferrante - Georg-August-Universität Göttingen.pdf"       
     [6] "Dr. Nicole Beyer - Georg-August-Universität Göttingen.pdf"         
     [7] "Dr. Wiebke Kämper - Georg-August-Universität Göttingen.pdf"        
     [8] "Ira Hannappel - Georg-August-Universität Göttingen.pdf"            
     [9] "Isabelle Arimond - Georg-August-Universität Göttingen.pdf"         
    [10] "Kathrin Czechofsky - Georg-August-Universität Göttingen.pdf"       
    [11] "Lena Frank - Georg-August-Universität Göttingen.pdf"               
    [12] "Menko Koch - Georg-August-Universität Göttingen.pdf"               
    [13] "Mina Anders - Georg-August-Universität Göttingen.pdf"              
    [14] "Prof. Dr. Catrin Westphal - Georg-August-Universität Göttingen.pdf"
    [15] "Prof. Dr. Teja Tscharntke - Georg-August-Universität Göttingen.pdf"
    [16] "Qian Zhang - Georg-August-Universität Göttingen.pdf"               
    [17] "Ricarda Koch - Georg-August-Universität Göttingen.pdf"             

## Text cleaning

Text analysis often requires prior work to clean up characters and
words. Simple issues such as the use of upper and lower case, dates,
times and other numbers, punctuation marks, etc. can limit the data
processing capacity of computers and bias text analysis. Therefore,
texts often require a normalisation process before they can be
processed.

Two key points to bear in mind when text cleaning are 1) the elimination
of stop words and 2) the normalisation of words.

In natural languages there are a number of words that do not contribute
meaning to the text, such as prepositions, conjunctions, articles, etc.,
but which appear very frequently. These words are known as stop words.
There is no definitive list of stop words, but different packages offer
proposals of stop words for filtering. In R, the `tidytext` package has
a list of empty words in English that can be used to remove them from
texts. For other lenguages, the `stopwords` package can be used:
<https://cran.r-project.org/web/packages/stopwords/readme/README.html>

Moreover, in natural languages we very often use collocations, sets of
words and lexical combinations, which are used together and have a joint
meaning (for example ‘statistical analysis’), and which should
preferably be analysed together.

There are other aspects of this kind to be taken into account before
carrying out text analysis, but I will not go into them in this seminar.
One example is lemmatisation (*stemming*) which consists of reducing
words to their root or lemma, so that duplication due to plurals,
conjugation of verbs, derived words, etc. is avoided.

``` r
library(textclean)
```

    Warning: package 'textclean' was built under R version 4.4.2

``` r
library(tidytext)
```

    Warning: package 'tidytext' was built under R version 4.4.2

``` r
web_texts_df |> 
  select(text) |> 
  slice(1) |>
  str_to_lower() |> 
  replace_date(replacement = "") |> # remove dates
  replace_time(replacement = "") |> # remove time
  str_remove_all(pattern = "[[:punct:]]") |> # remove punctuation
  str_remove_all(pattern = "[^\\s]*[0-9][^\\s]*") |>  # remove mixed string n number
  #replace_contraction() |>  # replace contraction to their multi-word forms
  #replace_word_elongation() |>  # replace informal writing with known semantic replacements
  str_squish() |>  # reduces repeated whitespace inside a string
  str_trim() |>  # removes whitespace from start and end of string
  str_replace_all("functional agrobiodiversity", "f_agrobiodiversity") |> #avoiding collocations
  str_replace_all("wildlife management", "widelife_mng") |> #avoiding collocations
  str_replace_all("nature conservation", "nature_cons") |> #avoiding collocations
  str_replace_all("landscape ecology", "lnds_ecology") |> 
  str_replace_all("habitat quality", "h_quality") |>  #avoiding collocations
  str_replace_all("wild bees", "wild_bees") |>  #avoiding collocations
  str_replace_all("ecosystem services", "ecoservices") |>  #avoiding collocations
  str_split(boundary("word")) |> 
  unlist() |>
  as_tibble(value = .) |>
  rename(word = value) |>
  anti_join(stop_words) |>  # remove stop words
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

    # A tibble: 115 × 2
    # Groups:   word [115]
       word                   countWords
       <chr>                       <int>
     1 göttingen                       7
     2 georgaugustuniversität          4
     3 study                           4
     4 wild                            4
     5 pollinators                     4
     6 landscape                       4
     7 kok                             3
     8 effect                          3
     9 wild_bees                       3
    10 hoverflies                      3
    # ℹ 105 more rows

``` r
clean_text <- function(text) {
  word_count <- text |> 
    str_to_lower() |> 
    str_remove_all(pattern = "[[:punct:]]") |> # remove punctuation
    replace_date(replacement = "") |> # remove dates
    replace_time(replacement = "") |> # remove time
    str_remove_all(pattern = "[^\\s]*[0-9][^\\s]*") |>  # remove mixed string n number
    #replace_contraction() |>  # replace contraction to their multi-word forms
    #replace_word_elongation() |>  # replace informal writing with known semantic replacements
    str_squish() |>  # reduces repeated whitespace inside a string
    str_trim() |>  # removes whitespace from start and end of string
    str_replace_all("functional agrobiodiversity", "f_agrobiodiversity") |> #avoiding collocations
    str_replace_all("wildlife management", "widelife_mng") |> #avoiding collocations
    str_replace_all("nature conservation", "nature_cons") |> #avoiding collocations
    str_replace_all("landscape ecology", "lnds_ecology") |> 
    str_replace_all("habitat quality", "h_quality") |>  #avoiding collocations
    str_replace_all("wild bees", "wild_bees") |>  #avoiding collocations
    str_replace_all("ecosystem services", "ecoservices") |>  #avoiding collocations
    str_split(boundary("word")) |> 
    unlist() |>
    as_tibble(value = .) |>
    rename(word = value) |>
    anti_join(stop_words) |>  # remove stop words
    filter(!str_detect(word, "^http"))  |>  #removes webpages
    filter(!str_detect(word, "^www"))  |>  #removes webpages
    filter(!str_detect(word, "^doi")) |> #removes bibliographic repetitions
    filter(!str_detect(word, "^pp")) |>  #removes bibliographic repetitions
    filter(!str_detect(word, "^al")) |>  #removes bibliographic repetitions
    drop_na() |> 
    group_by(word) |> 
    mutate(countWords = n()) |> 
    distinct() |> arrange(desc(countWords))
  
  return(word_count)
}

web_texts_df <- web_texts_df |>
  mutate(word_count = map(text, clean_text))
```

    Joining with `by = join_by(word)`
    Joining with `by = join_by(word)`
    Joining with `by = join_by(word)`
    Joining with `by = join_by(word)`
    Joining with `by = join_by(word)`
    Joining with `by = join_by(word)`
    Joining with `by = join_by(word)`
    Joining with `by = join_by(word)`
    Joining with `by = join_by(word)`
    Joining with `by = join_by(word)`
    Joining with `by = join_by(word)`
    Joining with `by = join_by(word)`
    Joining with `by = join_by(word)`
    Joining with `by = join_by(word)`
    Joining with `by = join_by(word)`
    Joining with `by = join_by(word)`
    Joining with `by = join_by(word)`

## Text analysis: Frequency and word clouds

A central question in text analysis is how to quantify what a document
is about. One of the most straightforward and simple ways is to count
the words that appear in the text. Word clouds are used to visualise the
most frequent words in a text in a simple way. In R, the `ggwordcloud`
package allows you to make word clouds in a simple way. There are other
word cloud packages, such as `wordcloud2`. You can consult these
resources for more information:
<https://r-graph-gallery.com/wordcloud.html>

``` r
library(ggwordcloud)
```

    Warning: package 'ggwordcloud' was built under R version 4.4.2

``` r
web_texts_df |> 
  unnest(word_count) |>
  group_by(word) |>
  summarise(countWords = sum(countWords)) |>
  arrange(desc(countWords)) |>
  filter(!word %in% c("göttingen", "university", "georgaugustuniversität", 
                      "ecampus", "dr", "tel", "germany", "pm", "media")) |>
  filter(countWords > 15) |>
  ggplot( aes(label = word, size = countWords*20, colour = countWords)) +
  geom_text_wordcloud() +
  scale_color_gradient(low = "blue", high = "red") +
  theme_minimal()
```

![](Workshop_TextAnalysis4Ecology_files/figure-commonmark/word%20cloud-1.png)

In this tutorial they explain how to improve the aesthetics and
composition of word clouds:
<https://cran.r-project.org/web/packages/ggwordcloud/vignettes/ggwordcloud.html>.

Other analyses based on word frequency are used to derive the importance
of words in a set of texts. You can take a look at this tutorial
<https://www.tidytextmining.com/tfidf>.

## Text analysis: Relationship between words

In text analysis, it is common to analyse a collection of related texts
(e.g. all texts from an author, or all articles resulting from the same
keyword search). The set of texts creates a *corpus*. The texts, in
turn, can be divided into smaller units of interest for the analysis,
such as paragraphs, sentences or words. These units are known as
*tokens*.

The `tidytext` package allows easy *tokenisation* of a corpus.

It is also common to construct term-document matrices for text analysis,
where the rows represent the terms and the columns the documents,
similar to the presence-absence matrices we use in ecology for species.
However, in the tidy philosophy, the structure of one *token* per row
would be maintained.

There are various methodologies and theoretical frameworks for text
analysis. Among them, the study of the relationship between words is one
of the most common. This approach examines which words tend to follow
others immediately, or which words tend to co-occur within the same
documents. In this second analysis I will focus on this seminar.

There are many other ways to analyse texts such as sentiment analysis,
text classification, topic modelling, etc. Here you can find more
information about text analysis in R:
<https://guides.library.upenn.edu/penntdm/r>.

``` r
library(tidytext) #text miny under tidy philosophy
library(widyr) #widen, process & re-tidy data (calculates paiwise correlations and distance)
```

    Warning: package 'widyr' was built under R version 4.4.2

``` r
library(tm) #stop words in spanish
```

    Warning: package 'tm' was built under R version 4.4.2

    Loading required package: NLP

    Warning: package 'NLP' was built under R version 4.4.2


    Attaching package: 'NLP'

    The following object is masked from 'package:ggplot2':

        annotate

``` r
#folder path
folder_path <- here("texts/")

#PDF files: Notas Ecoinformáticas + description of Ecoinformatica group
pdf_files <- list.files(folder_path, pattern = "\\.pdf$", full.names = TRUE)

textos <- map(pdf_files, read_pdf)

textos_df <- bind_rows(textos) |> 
  mutate(document = str_replace(document, pattern = paste0(folder_path, "/"), replacement = ""))

str(textos_df)
```

    tibble [18 × 2] (S3: tbl_df/tbl/data.frame)
     $ document: chr [1:18] "1416-Texto del artículo-5034-1-10-20170426.pdf" "1451-Texto del artículo-5298-1-10-20170801.pdf" "1524-Texto del artículo-5730-3-10-20171212.pdf" "1570-Texto del artículo-5970-1-10-20180421.pdf" ...
     $ text    : chr [1:18] "AEET ASOCIACIÓN ESPAÑOLA DE ECOLOGÍA TERRESTRE Ecosistemas 26(1): 126-127 [Enero-abril 2017] Doi.: 10.7818/ECOS"| __truncated__ "AEET ASOCIACIÓN ESPAÑOLA DE ECOLOGÍA TERRESTRE ecosistemas Ecosistemas 26(2): 64-66 [Mayo-Agosto 2017] Doi.: 10"| __truncated__ "AEET ASOCIACIÓN ESPAÑOLA DE ECOLOGÍA TERRESTRE Ecosistemas 26(3): 110-111 [Septiembre-Diciembre 2017] Doi.: 10."| __truncated__ "AEET ASOCIACIÓN ESPAÑOLA DE ECOLOGÍA TERRESTRE Ecosistemas 27(1): 128-129 [Enero-Abril 2018] Doi.: 10.7818/ECOS"| __truncated__ ...

``` r
stop_words_es <- bind_rows(stop_words,
                               tibble(word = tm::stopwords("spanish"),
                                          lexicon = "custom")) |> 
  filter(lexicon == "custom")

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

![](Workshop_TextAnalysis4Ecology_files/figure-commonmark/word%20relationship-1.png)

## Resources and acknowledgements

The content of this seminar is based on the code developed for the
publication *Reassessing science communication for effective farmland
biodiversity conservation*
<https://www.sciencedirect.com/science/article/pii/S0169534724000326>.

The different sections have been developed using information from
various tutorials, books and websites. Most of them are cited throughout
the seminar. Other sources of interest for character handling and text
analysis in R are:
<https://link.springer.com/book/10.1007/978-3-319-03164-4>
<http://jvera.rbind.io/post/2017/10/16/spanish-stopwords-for-tidytext-package/>
<https://bookdown.org/dereksonderegger/444/string-manipulation.html>
<https://github.com/gagolews/stringi?tab=readme-ov-file>
<https://www.tidytextmining.com/>
<https://users.ssc.wisc.edu/~hemken/Rworkshops/dwr/character.html>

The data cleaning using gbif is a suggestion by Jose Lanuza:
<https://scholar.google.com/citations?user=F6C5gFcAAAAJ&hl=en> and the
code is inspired by the code developed for the Iberian Bees Database
<https://github.com/ibartomeus/IberianBees>.

Ira Hannappel <https://www.uni-goettingen.de/en/659155.html> has
provided the bee data used for the taxonomic name cleaning.

The quarto template has been copied from the
<https://github.com/DatSciR/ciencia_datos/tree/main> repository
developed by Verónica Cruz Alonso and Julen Astigarraga.

For the analysis of word co-ocurrance I’ve used Ecoinformatics Notes
from the Ecosistemas journal
<https://ecoinfaeet.github.io/notas-ecoinformaticas.html>.

R copilot has been used in the creation of these materials and DeepL as
support for the translation of the text.

This seminar was prepared for the seminar series of the Ecoinformatics
Group from the Spanish Association of Terrestrial Ecology and and
further developed for the Writing Retreat of the Functional
Agrobiodiversity & Agroecology Group. Thanks for organising!
