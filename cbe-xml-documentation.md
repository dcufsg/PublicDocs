# The Data Structure of the National Folklore Collection *Main Manuscript Collection*

The *Main Manuscript Collection* is one of five major collections of folklore held by [Cnuasach Bhéaloideas Éireann/the National Folklore Collection](https://www.ucd.ie/irishfolklore/en/) (NFC), University College Dublin (UCD), and comprises 2,400 bound volumes of material collected since 1932. Full- and part-time collectors compiled the majority of these under the auspices of the Irish Folklore Commission (1935–1971). Approximately two thirds of the material is in Irish and it includes every aspect of the Irish oral tradition.

This collection is currently being digitsed as part of the Dúchas project by [Fiontar & Scoil na Gaeilge](http://www.dcu.ie/fiontar_scoilnagaeilge/) (FSG), Dublin City University, in conjunction with the NFC. The digitisation will produce (1) a collection of scanned images and (2) an XML data set that indexes and annotates the images. This document describes the data structure used in the XML data set.

**Note:** This documentation is a work in progress and should be treated as such.

## Table of Contents  

1. [Introduction](#introduction)  
2. [Volumes](#volumes)  
3. [Pages](#pages)  
4. [Parts](#parts)  
5. [Items](#items)  
6. [Topics](#topics)  
7. [Note types](#notetypes)  

## Introduction 

A similar process to digitise the NFC *Schools' Collection* (hereafter referred to as CBÉS) and *Photographic Collection* (hereafter referred to as CBÉG) was previously undertaken during the period 2013–2017, also under the auspices of the Dúchas project. The results of this work have been made publicly available at [www.duchas.ie](https://www.duchas.ie) and are in the process of being archived for preservation by the [UCD Digital Library](https://digital.ucd.ie/).

This present document is heavily indebted to a previous specification of the CBÉS data structure, *The Data Structure of Bailiúchán na Scol*, authored by [Michal Boleslav Měchura](http://www.lexiconista.com) in 2013. There is good reason for this: it is proposed that the data structure of the *Main Manuscript Collection* (hereafter referred to as CBÉ) should not diverge signifc

It should be noted that the data structure outlined below represents the internal data structure for the CBÉ database and not an authoritative schema for encoding folkloristic data. Not all elements of the CBÉ data structure will be made publicly available and the data may be variously restructured or transformed for presentation on the dúchas.ie website or for dissemination via the Dúchas application programming interface (API). It should also be possible to reconstruct the data according to other specifications such as those provided by the Text Encoding Initiative (TEI) if required.  

### Changes since

#### Developments in relation to the storage, retrieval and dissemination of image media
The CBÉS data structure features 
string replacement operations

### Editorial tooling
Ardán/LXLN

#### Additive metadata model
either/or vs additive tagging (lang) (internal ed practices)

#### Additional metadata
CBÉG Geonames etc.

### Overview of the CBÉ data structure

The CBÉ collection consists of stories, accounts, diaries and other materials which have been physically bound into volumes. 

Each volume can be subdivided into several chapters where each chapter pertains to a particular school, class and teacher. Chapters can further be subdivided into stories where each story is a self- contained unit of text written by a particular pupil (or, exceptionally, by the teacher himself or herself).

Each volume is represented in the data set by a `pVolume` (GA: *imleabhar*) record, and each page by a `pPage` (GA: *leathanach*) record. Each `pPage` record contains a reference to a `pVolume` record to tell you which volume the page is in. Each `pPage` record also contains a reference that identifies scanned images of the page.

Together, `pVolume` and `pPage` records represent the collection’s **physical structure** (hence the prefix `p`). In parallel to the physical structure, a **logical structure** exists which annotates and indexes the physical structure. It consists of `lPart` records and `lItem` records (notice the prefix `l`).

An `lPart` (GA: *cuid*) record represents a chapter. It is consonant with the [msPart](
http://www.tei-c.org/release/doc/tei-p5-doc/en/html/MS.html#mspt) element as specified in the TEI P5 Guidelines. It contains a reference to a pVolume record to tell you which volume the chapter is in, and a reference to a `pPage` record within that volume that represents the chapter’s title page. An `lPart` record also contains data about the school from which originates, its geographical location, the name of the teacher who compiled the chapter, and so on.

An `lItem` (GA: *mír*) record represents a story. It is consonant with the TEI [msItem](
http://www.tei-c.org/release/doc/tei-p5-doc/en/html/MS.html#mscoit) element. It contains a reference to its parent `lPart` record and to one or more `pPage` records. Notice that mapping between stories and pages is many-to-many: a story can span over several pages and a page can accommodate several stories. An `lItem` record also contains detailed data about the people who were involved in its writing, the subject it pertains to and other data.

In addition to the physical and logical structure, several auxiliary record types exist in the data set which serve as look-up lists. These include topic records and noteType records. Their names do not have a prefix.

Each entry in the data set consists of an ID number and an XML document of one of types mentioned above (`pVolume`, `lPart` etc). An element within an XML document may refer to another XML document with its ID number.

The rest of this document describes each record type in detail.


```xml
<pVolume>
  <volumeNumber/>
</pVolume>
```

## Volumes



## Pages



## Parts


http://www.tei-c.org/release/doc/tei-p5-doc/en/html/MS.html#mspt parts

## Items

http://www.tei-c.org/release/doc/tei-p5-doc/en/html/MS.html#mscoit
