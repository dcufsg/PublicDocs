# The Data Structure of the National Folklore Collection *Main Manuscript Collection*

The *Main Manuscript Collection* is one of five major collections of folklore held by [Cnuasach Bhéaloideas Éireann/the National Folklore Collection](https://www.ucd.ie/irishfolklore/en/) (NFC), University College Dublin (UCD), and comprises 2,400 bound volumes of material collected since 1932. Full- and part-time collectors compiled the majority of these under the auspices of the Irish Folklore Commission (1935–1971). Approximately two thirds of the material is in Irish and it includes every aspect of the Irish oral tradition.

This collection is currently being digitsed as part of the Dúchas project by [Fiontar & Scoil na Gaeilge](http://www.dcu.ie/fiontar_scoilnagaeilge/) (FSG), Dublin City University (DCU), in conjunction with the NFC. The digitisation will produce (1) a collection of scanned images and (2) an XML data set that indexes and annotates the images. This document describes the data structure used in the XML data set.

**Note:** This documentation is a work in progress and should be treated as such.

## Table of Contents  

1. [Introduction](#introduction)  
2. [Volumes](#volumes)  
3. [Pages](#pages)  
4. [Parts](#parts)  
5. [Items](#items)  
6. [Topics](#topics)  
7. [Note types](#notetypes)  
8. [References to persons](#references-to-persons)
9. [Dates](#dates)
10. [Consistency with CBÉS data structure](#consistency-with-cbÉs-data-structure)

## Introduction 

A similar process to digitise the NFC *Schools' Collection* (hereafter referred to as CBÉS) and *Photographic Collection* (hereafter referred to as CBÉG) was previously undertaken during the period 2013–2017, also under the auspices of the Dúchas project. The results of this work have been made publicly available at [www.duchas.ie](https://www.duchas.ie) and are in the process of being archived for preservation by the [UCD Digital Library](https://digital.ucd.ie/).

This present document is heavily indebted to a previous specification of the CBÉS data structure, *The Data Structure of Bailiúchán na Scol*, authored by [Michal Boleslav Měchura](http://www.lexiconista.com) in 2013. There is good reason for this: the data structure of the *Main Manuscript Collection* (hereafter referred to as CBÉ) will be, to a large extent, a more generalised adaptation of the CBÉS data structure. This will aid future developments as part of the Dúchas project (see [Consistency with CBÉS data structure](#consistency-with-cbÉs-data-structure) below).

It should be noted that the data structure outlined in this document represents the internal data structure for the CBÉ database and not an authoritative schema for encoding folkloristic or archival data. Not all elements of the CBÉ data structure will be made publicly available and the data may be variously restructured or transformed for presentation on the dúchas.ie website or for dissemination via the forthcoming Dúchas application programming interface (API). It should also be possible to reconstruct the data according to other specifications, such as the Metadata Object Description Schema (MODS) or those provided by the Text Encoding Initiative (TEI), as required.  

### Changed understandings since developing the CBÉS data structure

#### Developments in relation to the storage, retrieval and dissemination of image media
The CBÉS data structure references individual image files where each page in a volume is associated with an image:

```xml
<fileNameBig default="CBES_0085\CBES_0085_1A.jpg" />
<fileNameSmall default="CBES_0085\CBES_0085_1A_t.jpg" />
```

This convention may no longer be optimal for the following reasons:

- Images need to be served at a flexible range of sizes. Since September 2017, the dúchas.ie website features a responsive design and it is intended to add to the range of image sizes served in order to better cater for a wide range of user devices. The number and variety of images offered is likely to change over time.
- With [IIIF media standards](http://iiif.io/) and server technology coming on stream, it may soon no longer be necessary or desirable to retain multiple static copies of images in local storage.

Instead, it is proposed that the XML code for CBÉ will reference a single authoritative image file. Paths to derivate images can be constructed at the application level using string replacement operations.

#### Editorial tooling
Editorial work for CBÉS was carried out the Léacslann editorial system previously developed by FSG. As was the case for CBÉG, however, it is proposed to annotate the CBÉ volumes in a newer editorial system, Ardán. While this is an application-level concern, and is thus outside the scope of this document, there is no doubt that the possibilites afforded by Ardán's more flexible data manipulation tools have influenced the data structure design.

#### Additive metadata models
Various aspects of the CBÉS data structure preferred a boolean or either/or approach to certain metadata. For example, stories in CBÉS were marked as being either Irish-language, English-language, or mixed-language texts. In many cases such as this the CBÉ data structure will provide for an additive or tag-based approach where, for instance, one or many or no languages may be assigned to a given item in each volume. This is a function of both the wider scope of the CBÉ collection - which encompasses, to continue from the previous examples, some texts in Manx, Breton, and other languages, as well as non-linguistic materials - and evolving internal editorial practices.

#### Additional metadata
The data structure of the CBÉG collection introduced a number of new annotation options, as compared with CBÉS. These include structured references to international locations via the [Geonames](http://www.geonames.org/) API; more granular date annotations; annotations with reference to the CBÉD database (see [References to persons](#references-to-persons) below); format and other image metadata, as well as; an additional topic/subject taxonomy. It is proposed to port many of these additional metadata features to CBÉ, as well as some others that will be specific to the new collection.

### Overview of the CBÉ data structure

The CBÉ collection consists of stories, accounts, questionnaires, diaries and other materials which have been physically bound into volumes. Clearly, there is a great degree of variabilty in the composition of each volume. Some volumes contain the work of a single collector, while others feature multiple collectors and multiple informants and may be collected in various locations. 

Each volume is represented in the data set by a `pVolume` (GA: *imleabhar*) record, and each page by a `pPage` (GA: *leathanach*) record. Each `pPage` record contains a reference to a `pVolume` record to tell you which volume the page is in. Each `pPage` record also contains a reference that identifies scanned images of the page.

Together, `pVolume` and `pPage` records represent the collection’s **physical structure** (hence the prefix `p`). In parallel to the physical structure, a **logical structure** exists which annotates and indexes the physical structure. It consists of `lPart` records and `lItem` records (notice the prefix `l`).

An `lPart` record (GA: *cuid*) represents a distinct manuscript section within a volume and may have originally been physically separate from the other contents of the volume. It is consonant with the [`msPart`](http://www.tei-c.org/release/doc/tei-p5-doc/en/html/MS.html#mspt) element as specified in the TEI P5 Guidelines. It contains a reference to a `pVolume` record to tell you which volume the manuscript part is in, and a reference to a `pPage` record within that volume that represents the manuscript part’s title page. An `lPart` record may also contain data about the location where the material was collected, the collector(s) who sourced the material, and so on.

An `lItem` (GA: *mír*) record represents a discrete individual work, such as a story or diary entry . It is consonant with the TEI [`msItem`](http://www.tei-c.org/release/doc/tei-p5-doc/en/html/MS.html#mscoit) element. It contains a reference to its parent `lPart` record and to one or more `pPage` records. Notice that mapping between items and pages is many-to-many: a story can span over several pages and a page can accommodate several stories. An `lItem` record also contains detailed data about the people who were involved in its writing, the subject it pertains to and other data.

In addition to the physical and logical structure, several auxiliary record types exist in the data set which serve as look-up lists. These include `topic` records and `noteType` records. Their names do not have a prefix.

Each entry in the data set consists of an ID number and an XML document of one of types mentioned above (`pVolume`, `lPart` etc). An element within an XML document may refer to another XML document with its ID number.

Each record type is described in detail below.

### Relationships of inheritance within volumes

In many cases where, for example, all of the parts in a volume were collected in a single location or many of the items in a manuscript part were obtained by the same collector, it may be beneficial to allow `lPart` or `lItem` records to 'inherit' certain metadata from the volume or part, respectively, to which they belong. Such a facility may be an aid to the progress of the editorial annotation work. Relationships of inheritance, as such, will not be explicitly defined within the data structure. That is to say, there will not be an element that explicitly defines an 'Item X inherits collector properties from Volume Y'-type relationship. Rather, relationships of inheritance will be facilitated within the editorial tooling as an application-level concern.

## Volumes

### Example

Entry ID 231579

```xml
<pVolume>
  <status>3</status>
  <volumeNumber>0109</volumeNumber>
  <notes></notes>
</pVolume>
```

### `<pVolume>`

Represents a volume.

#### Child elements

| Name          | Cardinality   | Description  |
| ------------- |---------------| ------|
| `<status>`    | exactly one   | $1600 |
| `<volumeNumber>`     | centered      |   $12 |
| `<notes>` | are neat      |    $1 |


## Pages

### Example

Entry ID 331620

```xml
<pPage>
  <image>
    <fileName>CBES_0085\CBES_0085_209.jpg</fileName>
  </image>
  <volume id="4344035" />
  <pageNumber>209</pageNumber>
  <listingOrder>209</listingOrder>
  <condition>
    <conditionDesc>2</conditionDesc>
  </condition>
  <notes></notes>
</pPage>
```

## Parts

### Example

Entry ID 4667213

```xml
<lPart>
  <volume id="4360513">
    <listingOrder>2</listingOrder>
  </volume>
  <page id="4360522" />
  <locationIreland>
    <county>100001</county>
    <georefIreland>100001</georefIreland>
  </locationIreland>
  <locationAbroad>
    <country>BE</country>
    <georefAbroad>2784804</georefAbroad>
  </locationAbroad>
  <collectors>
    <person>80607834</person>
  </collectors>
  <relevantPublications>
    <publication doi="10.2307/20521241">
      Ó Broin, Tomás. "Scéalaí Tíre: Bailiúchán Seanchais ó Ghaillimh." <title>Béaloideas</title> 24 (1955): Ii-133.
    </publication>
  </relevantPublications>
  <notes></notes>
</lPart>
```


## Items

### Example

Entry ID 551579

```xml
<lItem>
  <title>Scéal</title>
  <status>3</status>
  <chapter id="4427865">
    <listingOrder>19</listingOrder>
  </chapter>
  <pages>
    <page id="4360522" />
    <page id="4360523" />
  </pages>
  <date>
    <year>2002</year>
    <month>01</month>
    <day>18</day>
  </date>
  <languages>
    <language>ga</language>
  </languages>
  <contentDescription type="0">
    <mode>0</mode>
    <script>0</script>
  </contentDescription>
  <subItem>
    <contentDescription type="2"></contentDescription> 
  <subItem>
  <topics></topics>
  <collectors>
    <person>80607834</person>
  </collectors>
  <informants>
    <person>80607834</person>
    <person>80607834</person>
  </informant>
  <relevantPerson>
    <person>80607834</person>
  </relevantPerson>
  <extraInfo status="EDIT">
    <text lang="ga"></text>
    <text lang="en"></text>
  </extraInfo>
  <relevantCollections>
    <collection id="cbeg"></collection>
  </relevantCollections>
  <relevantPublications>
    <publication doi="10.2307/20521241">
      Ó Broin, Tomás. "Scéalaí Tíre: Bailiúchán Seanchais ó Ghaillimh." <title>Béaloideas</title> 24 (1955): Ii-133.
    </publication>
  </relevantPublications>
  <notes></notes>
</lItem>
```

## Dates

Dates may be referenced within the CBÉ data structure for several reasons. To allow the greatest flexibility, the `<date>` element provides for a number of different scenarios.

### Known dates

```xml
<date>
  <year>2002</year>
  <month>01</month>
  <day>18</day>
</date>
```

Known dates may include as few as one element (`<year>`) and as many as three elements.

### Qualified dates

```xml
<date qualifier="INFER">
  <month>02</month>
  <day>09</day>
</date>
```

The `@qualifier` attribute will accept a range of values consistent with the [date qualifiers specified in MODS v3](https://www.loc.gov/standards/mods/v3/mods-userguide-generalapp.html#list). These are `APPROX` (a date that may not be exact), `INFER` (the date has not been transcribed directly from a resource, but is a reasonable assumption), and `QUESTION`(the date is questionable).

### Periods of time

```xml
<date>
  <startDate>
    <year>1940</year>
  </startDate>
  <endDate>
    <year>1949</year>
  </endDate>
</date>
```
This can be used to demarcate a span of time and all standard `<date>` child elements for days, months and years are permitted.

## References to persons

Persons named within the CBÉ metadata will be identified by reference to objects in the *Persons Database* (hereafter CBÉD). Given that many persons appear in several NFC collections - some photographers in CBÉG are also collectors in CBÉ, for example - objects in CBÉD serve as a single source of truth for personal metadata across the entire data set. Persons in CBÉ metadata are referenced by their CBÉD ID, stored in a `<person>` element.

The CBÉD data structure will be expanded to account for the elements described below.

### Example

```xml
<person>
  <name>Seán <surname>Ó hEochaidh</surname></name>
  <gender>m</gender>
  <birthDate>
    <date>
      <year>1913</year>
      <month>02</month>
      <day>09</day>
    </date>
  </birthDate>
  <deathDate>
    <date>
      <year>2002</year>
      <month>01</month>
      <day>18</day>
    </date>
  </deathDate>
  <occupations>
    <occupation id="22"></occupation>
  </occupations>
  <viaf>1740563</viaf>
  <bio>1714</bio>
  <notes></notes>
</person>
```

### `<person>`

Represents a person.

#### Child elements

| Name            | Cardinality   | Description  |
| ----------------|---------------|--------------|
| `<name>`        | one or more   | The person's full name. The first name in the tree will taken as the person's authoritative name |
| `<gender>`      | exactly one   | The person's gender    |
| `<birthDate>`   | exactly one   | The person's date of birth |
| `<deathDate>`   | exactly one   | The person's date of death |
| `<occupations>` | none or one or more | Any occupations associated with the person |
| `<viaf>`        | none or one   | The person's [Virtual International File Authority](http://viaf.org/) ID |
| `<bio>`         | none or one   | The person's [Ainm.ie](https://www.ainm.ie) biography ID |
| `<notes>`       | none or one or more | Any editorial notes |

## Consistency with CBÉS data structure

As stated previously, the CBÉ data structure was designed as a generalised version of the earlier CBÉS data structure. There are two primary reasons for this:

- It is proposed to move the CBÉS collection from FSG's older editorial platform, Léacslann, to our most up-to-date system, Ardán. A consistent data structure between both collections means they can share the same editorial interface.
- A consistent data structure for CBÉ and CBÉS reduces complexity and makes it easier to provide and maintain facilities such as the forthcoming Dúchas API.

It may indeed be possible to programmatically transform the CBÉS XML structure so that it is fully uniform with the generalised CBÉ structure. For example, CBÉS-specific elements such as `<teacherName>` might be mapped onto general CBÉ elements such as `<collector type="teacher">`. Thus, complexity at the application level could be greatly reduced.
