# Component-GeoNetwork
Information about the data catalogue of the project. This is GeoNetwork

* https://catalogue.grumets.cat/geonetwork/ad4gd

We decided to use the ISO 19115:2003. It is by far the most used version. While 19115-1 has been arroud for years, there aren't much implementations outthere.

## How to get the metadata records using CSW

* As ISO19115:2003: https://catalogue.grumets.cat/geonetwork/ad4gd/cat/csw?REQUEST=GetRecords&SERVICE=CSW&version=2.0.2&resultType=results&typeNames=gmd:MD_Metadata&namespace=xmlns(gmd=http://www.isotc211.org/2005/gmd)&outputSchema=http://www.isotc211.org/2005/gmd&maxRecords=100&elementSetName=full
* As DCAT: https://catalogue.grumets.cat/geonetwork/ad4gd/cat/csw?REQUEST=GetRecords&SERVICE=CSW&version=2.0.2&resultType=results&typeNames=dcat&namespace=xmlns(gmd=http://www.isotc211.org/2005/gmd)&outputSchema=http://www.w3.org/ns/dcat%23&maxRecords=100&elementSetName=full

## Linked data trick. How to add URL/URI to some places that are not prepared for that.
The ISO19139 XML schema implementation add the possibility to transform any string into a link. This is done by replacing `<gco:CharacterString>` by a `<gmx:anchor>`. This is an example:

```
   <gmd:individualName>
      <gco:CharacterString>Thomas Hodson</gco:CharacterString>
   </gmd:individualName>
```
Can be replaced by
```
   <gmd:individualName>
      <gmx:Anchor xlink:href="https://thomashodson.com/">Thomas Hodson</gmx:Anchor>
   </gmd:individualName>
```
We use this trick in several places:

### Limitation in CI_Citation
In ISO19115:2003 CI_Citation there is a linkage mechanism. In our implementation, if there is need to a URL to be associated with a cintation, we associate it to the title.

Example:

```
   <gmd:CI_Citation>
      <gmd:title>
         <gmx:Anchor xlink:href="https://ad4gd.eu/pilots/" xlink:type="simple">AD4GD project category</gmx:Anchor>
      </gmd:title>
      <gmd:date>
         <gmd:CI_Date/>
      </gmd:date>
   </gmd:CI_Citation>
```

### How to encode link data concepts to ISO 191115 keyworks
In ISO 19115:2003 there is no easy way to associate a URI to a term. It is possible to associate a URI to a Citation of a thesuarus as a "collection of concepts" but not to an individual keyword. Actualy the since a Citation has no URL, thesaurus cannot be expressed as a URL easily. Here is the solution that combines the 2 previous tricks

```
         <gmd:descriptiveKeywords>
            <gmd:MD_Keywords>
               <gmd:keyword>
                  <gmx:Anchor xlink:href="https://dd.eionet.europa.eu/vocabularyconcept/aq/pollutant/6002"
                              xlink:type="simple">PM1</gmx:Anchor>
               </gmd:keyword>
               <gmd:keyword>
                  <gmx:Anchor xlink:href="http://vocab.nerc.ac.uk/collection/P07/current/YUDG9DXK/"
                              xlink:type="simple">PM2.5</gmx:Anchor>
               </gmd:keyword>
               <gmd:thesaurusName>
                  <gmd:CI_Citation>
                     <gmd:title>
                        <gmx:Anchor xlink:href="http://vocab.nerc.ac.uk/collection/P07/current/"
                                    xlink:type="simple">Climate and Forecast Standard Names</gmx:Anchor>
                     </gmd:title>
                     <gmd:date>
                        <gmd:CI_Date>
                           <gmd:date>
                              <gco:Date>2024-09-04</gco:Date>
                           </gmd:date>
                           <gmd:dateType>
                              <gmd:CI_DateTypeCode codeList="http://standards.iso.org/iso/19139/resources/gmxCodelists.xml#CI_DateTypeCode"
                                                   codeListValue="creation"/>
                           </gmd:dateType>
                        </gmd:CI_Date>
                     </gmd:date>
                  </gmd:CI_Citation>
               </gmd:thesaurusName>
            </gmd:MD_Keywords>
         </gmd:descriptiveKeywords>
```

## Issues
* The JSON as base64: https://github.com/geonetwork/core-geonetwork/issues/8343#issuecomment-2328020366


