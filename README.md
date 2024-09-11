# Component-GeoNetwork
Information about the data catalogue of the project. This is GeoNetwork

* https://catalogue.grumets.cat/geonetwork/ad4gd

We decided to use the ISO 19115:2003. It is by far the most used version. While 19115-1 has been arroud for years, there aren't much implementations outthere.

## How to get the metadata records using CSW

* As ISO19115:2003: https://catalogue.grumets.cat/geonetwork/ad4gd/cat/csw?REQUEST=GetRecords&SERVICE=CSW&version=2.0.2&resultType=results&typeNames=gmd:MD_Metadata&namespace=xmlns(gmd=http://www.isotc211.org/2005/gmd)&outputSchema=http://www.isotc211.org/2005/gmd&maxRecords=100&elementSetName=full
* As DCAT: https://catalogue.grumets.cat/geonetwork/ad4gd/cat/csw?REQUEST=GetRecords&SERVICE=CSW&version=2.0.2&resultType=results&typeNames=dcat&namespace=xmlns(gmd=http://www.isotc211.org/2005/gmd)&outputSchema=http://www.w3.org/ns/dcat%23&maxRecords=100&elementSetName=full

## Limitation in CI_Citation
In ISO19115:2003 CI_Citation there is linkage mechanim. I our implementation, if there is need to a URL to be procided, we associate it to the title of the citation as an Anchor

## how to encode Essential Variables as keyworks
In ISO 19115:2003 there is no easy way to associate a URI to a term. It is possible to associate a URI to a Citation of a thesuarus but not to an keyword individual keyword.

## Issues
* The JSON as base64: https://github.com/geonetwork/core-geonetwork/issues/8343#issuecomment-2328020366


