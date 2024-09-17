# Component-GeoNetwork
Information about the data catalogue of the project. This is GeoNetwork

* https://catalogue.grumets.cat/geonetwork/ad4gd

We decided to use the ISO 19115:2003. It is by far the most used version. While 19115-1 has been arroud for years, there aren't much implementations outthere.

**Table of content**
1. [How to get the metadata records using CSW](#how-to-get-the-metadata-records-using-csw)
2. [Linked data trick. How to add URL/URI to some places that are not prepared for that](#linked-data-trick-how-to-add-urluri-to-some-places-that-are-not-prepared-for-that)
3. [Linking to documents stored in the Geonetwork as attachments](#linking-to-documents-stored-in-the-geonetwork-as-attachments)
4. [Adding the schema of the data](#adding-the-schema-of-the-data)
5. [Defining a datacube](#defining-a-datacube)
6. [Common issues](#common-issues)
7. [Known issues](#known-issues)

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

This practice may seem very peculiar, but in fact it is also used by the EEA catalogue as it can be seen here:

https://sdi.eea.europa.eu/catalogue/srv/cat/csw?REQUEST=GetRecordById&SERVICE=CSW&version=2.0.2&typeNames=gmd:MD_Metadata&namespace=xmlns(gmd=http://www.isotc211.org/2005/gmd)&outputSchema=http://www.isotc211.org/2005/gmd&maxRecords=100&elementSetName=full&id=e4fcc5da-e51f-4b66-a9b0-78da0cfd9a15

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

## Adding the schema of the data
Adding the application schema language for the data can also be done by linking the citation title with an anchor. Unfortunately the title is not shown in the Geonetwork visualization interface for some unknown reason, so we had to add an alternative title with the same information. The purpose of the schema information is to validate the distribution info. In this case, we use use one of the attached files as a URL instead of an external resource. Note that you can add attached resources but do not have them as associated resources to avoid having them visible in the distribution info.

```
<gmd:applicationSchemaInfo>
      <gmd:MD_ApplicationSchemaInformation>
         <gmd:name>
            <gmd:CI_Citation>
               <gmd:title>
                  <gmx:Anchor xlink:href="https://catalogue.grumets.cat/geonetwork/ad4gd/api/records/63a296c5-f132-4174-bac5-babe5ad94f2a/attachments/GBIF_Mammals_ANSI_csvw.txt">Mammals occurrences GBIF test schema</gmx:Anchor>
               </gmd:title>
               <gmd:alternateTitle>
                  <gmx:Anchor xlink:href="https://catalogue.grumets.cat/geonetwork/ad4gd/api/records/63a296c5-f132-4174-bac5-babe5ad94f2a/attachments/GBIF_Mammals_ANSI_csvw.txt">Mammals occurrences GBIF test schema</gmx:Anchor>
               </gmd:alternateTitle>
               <gmd:date>
                  <gmd:CI_Date/>
            </gmd:CI_Citation>
         </gmd:name>
         <gmd:schemaLanguage>
            <gco:CharacterString>JSON</gco:CharacterString>
         </gmd:schemaLanguage>
         <gmd:constraintLanguage>
            <gco:CharacterString>CSVW</gco:CharacterString>
         </gmd:constraintLanguage>
      </gmd:MD_ApplicationSchemaInformation>
  </gmd:applicationSchemaInfo>
```
This is how it is presented in the user interface.
![image](https://github.com/user-attachments/assets/664c8c28-5950-42d9-82e0-919f8ff11cc4)

## Defining a datacube
The definition of a datacube requires at least 3 things (this respects the xarray and the CIS model)
* Dimensions (called coordinates in xarray and domainSet in CIS)
* Bands (called data variables in xarray and rangeType in CIS)
* Other metadata (called attributes in xarray and metadata in CIS)

Lets imagine that we what to describe this datacube using ISO 19115 (here we see it expressed as an python xarray in ODC)

```
<xarray.Dataset>
Dimensions:          (time: 7, y: 668, x: 688)
Coordinates:
  * time             (time) datetime64[ns] 1987-01-01 1992-01-01 ... 2017-01-01
  * y                (y) float64 4.488e+06 4.488e+06 ... 4.748e+06 4.748e+06
  * x                (x) float64 2.599e+05 2.603e+05 ... 5.275e+05 5.279e+05
    spatial_ref      int32 32631
Data variables:
    Aquatic          (time, y, x) float32 -9.999e+03 -9.999e+03 ... -9.999e+03
    Forest           (time, y, x) float32 -9.999e+03 -9.999e+03 ... -9.999e+03
    HerbaceousCrops  (time, y, x) float32 -9.999e+03 -9.999e+03 ... -9.999e+03
    WoodyCrops       (time, y, x) float32 -9.999e+03 -9.999e+03 ... -9.999e+03
    Shrublands       (time, y, x) float32 -9.999e+03 -9.999e+03 ... -9.999e+03
Attributes:
    crs:           EPSG:32631
    grid_mapping:  spatial_ref
```

### Declaring a datacube
This the way we declare that the data is a "datacube". We beleive that "datacube" is an aggregation of "layers" and "files" so in some sense is a upper hierachycal level.
```
   <gmd:hierarchyLevelName>
      <gco:CharacterString>Datacube</gco:CharacterString>
   </gmd:hierarchyLevelName>
```

### Dimensions
The dimensions are described in the statial representation. 

![image](https://github.com/user-attachments/assets/354ca856-74e1-4f5a-a979-c44dd7e4e6a2)

In our case, this is a georectified grid with 3 dimensions: x, y and time. The names of the dimensions are codified in ISO as collumn, row and time respectively. To fully define the grid, we need to split the defintion in two parts. In the spatial representation we can define the periodicity of the grid and in the extent we can define the actual positions.

```
    <gmd:spatialRepresentationInfo>
      <gmd:MD_Georectified>
         <gmd:numberOfDimensions>
            <gco:Integer>3</gco:Integer>
         </gmd:numberOfDimensions>
         <gmd:axisDimensionProperties>
            <gmd:MD_Dimension>
               <gmd:dimensionName>
                  <gmd:MD_DimensionNameTypeCode codeList="http://standards.iso.org/iso/19139/resources/gmxCodelists.xml#MD_DimensionNameTypeCode"
                                                codeListValue="row"/>
               </gmd:dimensionName>
               <gmd:dimensionSize>
                  <gco:Integer>668</gco:Integer>
               </gmd:dimensionSize>
               <gmd:resolution>
                  <gco:Length uom="m">390</gco:Length>
               </gmd:resolution>
            </gmd:MD_Dimension>
         </gmd:axisDimensionProperties>
         <gmd:axisDimensionProperties>
            <gmd:MD_Dimension>
               <gmd:dimensionName>
                  <gmd:MD_DimensionNameTypeCode codeList="http://standards.iso.org/iso/19139/resources/gmxCodelists.xml#MD_DimensionNameTypeCode"
                                                codeListValue="column"/>
               </gmd:dimensionName>
               <gmd:dimensionSize>
                  <gco:Integer>688</gco:Integer>
               </gmd:dimensionSize>
               <gmd:resolution>
                  <gco:Length uom="m">390</gco:Length>
               </gmd:resolution>
            </gmd:MD_Dimension>
         </gmd:axisDimensionProperties>
         <gmd:axisDimensionProperties>
            <gmd:MD_Dimension>
               <gmd:dimensionName>
                  <gmd:MD_DimensionNameTypeCode codeList="http://standards.iso.org/iso/19139/resources/gmxCodelists.xml#MD_DimensionNameTypeCode"
                                                codeListValue="time"/>
               </gmd:dimensionName>
               <gmd:dimensionSize>
                  <gco:Integer>7</gco:Integer>
               </gmd:dimensionSize>
               <gmd:resolution>
                  <gco:Distance uom="years">5</gco:Distance>
               </gmd:resolution>
            </gmd:MD_Dimension>
         </gmd:axisDimensionProperties>
         <gmd:cellGeometry>
            <gmd:MD_CellGeometryCode codeList="http://standards.iso.org/iso/19139/resources/gmxCodelists.xml#MD_CellGeometryCode"
                                     codeListValue="area"/>
         </gmd:cellGeometry>
         <gmd:cornerPoints>
            <gml:Point gml:id="NW_corner">
               <gml:pos>260280 4747860</gml:pos>
            </gml:Point>
         </gmd:cornerPoints>
         <gmd:cornerPoints>
            <gml:Point gml:id="SE_corner">
               <gml:pos>527430 4488900</gml:pos>
            </gml:Point>
         </gmd:cornerPoints>
         <gmd:pointInPixel>
            <gmd:MD_PixelOrientationCode>center</gmd:MD_PixelOrientationCode>
         </gmd:pointInPixel>
      </gmd:MD_Georectified>
   </gmd:spatialRepresentationInfo>
```
The actual ranges of values of the dimensions are present in the extent. The temporal extent can be defined as a list of time instances or as a time interval. The Geonetwork view does not present the time instants in the view mode so we go for the interval. We are also forced to duplicate the bbox in WGS84 lat/long and in the crs of the dataset.

![image](https://github.com/user-attachments/assets/f62bdfe6-9d3e-4a84-b062-3e1f26a44a0b)

```
          <gmd:extent>
            <gmd:EX_Extent>
               <gmd:geographicElement>
                  <gmd:EX_GeographicBoundingBox>
                     <gmd:extentTypeCode>
                        <gco:Boolean>true</gco:Boolean>
                     </gmd:extentTypeCode>
                     <gmd:westBoundLongitude>
                        <gco:Decimal>0.0639119375078333</gco:Decimal>
                     </gmd:westBoundLongitude>
                     <gmd:eastBoundLongitude>
                        <gco:Decimal>3.33828975521333</gco:Decimal>
                     </gmd:eastBoundLongitude>
                     <gmd:southBoundLatitude>
                        <gco:Decimal>40.5143870238267</gco:Decimal>
                     </gmd:southBoundLatitude>
                     <gmd:northBoundLatitude>
                        <gco:Decimal>42.8845959400033</gco:Decimal>
                     </gmd:northBoundLatitude>
                  </gmd:EX_GeographicBoundingBox>
               </gmd:geographicElement>
               <gmd:geographicElement>
                  <gmd:EX_BoundingPolygon>
                     <gmd:polygon>
                        <gml:MultiSurface gml:id="d1017385e573" srsName="EPSG:25831">
                           <gml:surfaceMember>
                              <gml:Polygon gml:id="d1017385e575" srsName="EPSG:25831">
                                 <gml:exterior>
                                    <gml:LinearRing>
                                       <gml:posList srsDimension="2">260085 4488705 527625 4488705 527625 4748055 260085 4748055 260085 4488705</gml:posList>
                                    </gml:LinearRing>
                                 </gml:exterior>
                              </gml:Polygon>
                           </gml:surfaceMember>
                        </gml:MultiSurface>
                     </gmd:polygon>
                  </gmd:EX_BoundingPolygon>
               </gmd:geographicElement>
               <gmd:temporalElement>
                  <gmd:EX_TemporalExtent>
                     <gmd:extent>
                        <gml:TimePeriod gml:id="d993013e629">
                           <gml:beginPosition>1987-01-01</gml:beginPosition>
                           <gml:endPosition>2017-01-01</gml:endPosition>
                        </gml:TimePeriod>
                     </gmd:extent>
                  </gmd:EX_TemporalExtent>
               </gmd:temporalElement>
            </gmd:EX_Extent>
         </gmd:extent>
```

### Bands
The bands are described in the content information. For each band we can describe the band name, the data type and the range of values.

![image](https://github.com/user-attachments/assets/4212a9b8-d6b9-4575-b35d-dc72f05178fd)

```
      <gmd:contentInfo>
      <gmd:MD_CoverageDescription>
         <gmd:attributeDescription>
            <gco:RecordType>Terrestrial Connectivity Index</gco:RecordType>
         </gmd:attributeDescription>
         <gmd:contentType>
            <gmd:MD_CoverageContentTypeCode codeList="http://standards.iso.org/iso/19139/resources/gmxCodelists.xml#MD_CoverageContentTypeCode"
                                            codeListValue="image"/>
         </gmd:contentType>
         <gmd:dimension/>
         <gmd:dimension>
            <gmd:MD_Band>
               <gmd:sequenceIdentifier>
                  <gco:MemberName>
                     <gco:aName>
                        <gco:CharacterString>Aquatic</gco:CharacterString>
                     </gco:aName>
                     <gco:attributeType>
                        <gco:TypeName>
                           <gco:aName>
                              <gco:CharacterString>FLOAT</gco:CharacterString>
                           </gco:aName>
                        </gco:TypeName>
                     </gco:attributeType>
                  </gco:MemberName>
               </gmd:sequenceIdentifier>
               <gmd:descriptor>
                  <gco:CharacterString>Aquatic habitat</gco:CharacterString>
               </gmd:descriptor>
               <gmd:maxValue>
                  <gco:Real>3</gco:Real>
               </gmd:maxValue>
               <gmd:minValue>
                  <gco:Real>0</gco:Real>
               </gmd:minValue>
            </gmd:MD_Band>
         </gmd:dimension>
         <gmd:dimension>
            <gmd:MD_Band>
               <gmd:sequenceIdentifier>
                  <gco:MemberName>
                     <gco:aName>
                        <gco:CharacterString>Forest</gco:CharacterString>
                     </gco:aName>
                     <gco:attributeType>
                        <gco:TypeName>
                           <gco:aName>
                              <gco:CharacterString>FLOAT</gco:CharacterString>
                           </gco:aName>
                        </gco:TypeName>
                     </gco:attributeType>
                  </gco:MemberName>
               </gmd:sequenceIdentifier>
               <gmd:descriptor>
                  <gco:CharacterString>Forest habitat</gco:CharacterString>
               </gmd:descriptor>
...
   </gmd:contentInfo>
```


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
* Title of the citations is not shown in the full view mode: https://github.com/geonetwork/core-geonetwork/issues/8370
