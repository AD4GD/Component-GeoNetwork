# Component-GeoNetwork
Information about the data catalogue of the project. This is GeoNetwork

* https://catalogue.grumets.cat/geonetwork/ad4gd

## How to get the metadata records using CSW

* As ISO19115:2003: https://catalogue.grumets.cat/geonetwork/ad4gd/cat/csw?REQUEST=GetRecords&SERVICE=CSW&version=2.0.2&resultType=results&typeNames=gmd:MD_Metadata&namespace=xmlns(gmd=http://www.isotc211.org/2005/gmd)&outputSchema=http://www.isotc211.org/2005/gmd&maxRecords=100&elementSetName=full
* As DCAT: https://catalogue.grumets.cat/geonetwork/ad4gd/cat/csw?REQUEST=GetRecords&SERVICE=CSW&version=2.0.2&resultType=results&typeNames=dcat&namespace=xmlns(gmd=http://www.isotc211.org/2005/gmd)&outputSchema=http://www.w3.org/ns/dcat%23&maxRecords=100&elementSetName=full

## Issues
* The JSON as base64: https://github.com/geonetwork/core-geonetwork/issues/8343#issuecomment-2328020366


