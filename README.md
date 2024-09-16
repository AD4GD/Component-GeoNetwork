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
This is the visual result (as you can see text become links that can be "cliked":
![image](https://github.com/user-attachments/assets/8180f5be-0f58-493f-9954-bfd8801f692e)

## Linking to documents stored in the Geonetwork as attachments
Adding a attached file to a metadata record is easy. The process starts by associating resources:
![image](https://github.com/user-attachments/assets/3c831a1a-e1b6-4dac-8ffe-eebff7588e1b)

Then a document could be added by droping it in the area or selecting it from the disc
![image](https://github.com/user-attachments/assets/dc804804-871e-4aa4-bf34-daa4e848ab00)
By clicking in the newly formed link to the file a associated resource metadata is created:
![image](https://github.com/user-attachments/assets/dbb9137c-4a6f-4834-9b52-01a85e52d997)
The link to the associated resource metadata ins automatically populated and can be manually finalized
![image](https://github.com/user-attachments/assets/302173b7-a6cb-40a5-ae3c-f5bf226ad013)
This create a distribution info like this:
![image](https://github.com/user-attachments/assets/0a36543e-256f-466c-b9e1-31e56197e83d)
That can be seen in XML linke this:

```
<gmd:distributionInfo>
      <gmd:MD_Distribution>
         <gmd:transferOptions>
            <gmd:MD_DigitalTransferOptions>
               <gmd:onLine>
                  <gmd:CI_OnlineResource>
                     <gmd:linkage>
                        <gmd:URL>https://catalogue.grumets.cat/geonetwork/srv/api/records/63ae32b2-2f5c-418a-bd72-439d17b3131c/attachments/Hundekehlesee.csv</gmd:URL>
                     </gmd:linkage>
                     <gmd:protocol>
                        <gco:CharacterString>WWW:LINK-1.0-http--link</gco:CharacterString>
                     </gmd:protocol>
                     <gmd:name>
                        <gco:CharacterString>Hundekehlesee.csv</gco:CharacterString>
                     </gmd:name>
                  </gmd:CI_OnlineResource>
               </gmd:onLine>
            </gmd:MD_DigitalTransferOptions>
         </gmd:transferOptions>
      </gmd:MD_Distribution>
  </gmd:distributionInfo>
```
In this case the trick of the gmx:anchor is not necessary becuase there is a linkage element available.

## Common issues
One of the most common issues in Geonetwork is the leftovers due to empty elements left in the GUI

Example:
![image](https://github.com/user-attachments/assets/78b89320-11b6-4044-9676-ff9d927d5598)

This will result in an empty element marked with `gco:nilReason="missing"`.
```
      <gmd:CI_ResponsibleParty>
         <gmd:individualName>
            <gco:CharacterString>Malte Zamzow</gco:CharacterString>
         </gmd:individualName>
         <gmd:positionName gco:nilReason="missing">
            <gco:CharacterString/>
         </gmd:positionName>
```
As we can see in the image this element is optional and could be removed from the final XML document.
![image](https://github.com/user-attachments/assets/0a4180ca-be88-4ce9-ad44-e712290b0cf1)
Unfortunately, removed elements are the difficult to recover in the GUI.

## Known issues
* The JSON as base64: https://github.com/geonetwork/core-geonetwork/issues/8343#issuecomment-2328020366


