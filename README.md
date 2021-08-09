
## About

`dcm_qa_ts` is a simple DICOM to NIfTI validator script and dataset to test conversion of DICOM images saved using different [transfer syntaxes](https://www.dicomlibrary.com/dicom/transfer-syntax/). DICOM can store images as either raw or compressed data. The standard allows several compression formats (each known as a Transfer Syntax). Some of these compression methods [are rarely seen outside of DICOM](https://crnl.readthedocs.io/jpeg_formats/index.html).

Be aware that the DICOM standard only requires compliant tools to handle the raw (uncompressed) transfer syntaxes. Therefore, many tools will not support these compressed transfer syntaxes. As discussed [elsewhere](https://crnl.readthedocs.io/jpeg_formats/index.html), these formats are typically inefficient in terms of compression ratio or speed. In general, it is probably a good idea to not use these transfer syntaxes. If compression is required, one should consider a modern full-file compression method such as [zstd](https://github.com/facebook/zstd). This repository validates compatibility, and does not demonstrate best practices.

This repository does not include [all possible](https://www.dicomlibrary.com/dicom/transfer-syntax/) DICOM transfer syntaxes. Rather, it is restricted to the lossless image formats supported by popular tools.

As noted below, dcm2niix must be compiled with external libraries to support JPEG2000 and JPEG-LS. Therefore, minimal installations of dcm2niix will not handle those formats.

## DataSets

All data was acquired Siemens Trio with VB17. Images are listed by series number. Image data is identical for all images, they were all created from the same image, but each given a separate series number and MediaStorageSOPInstanceUID (e.g.`dcmodify  -gin -m "(0020,0011)=9" 9a.dcm`).

* `1.dcm`
  * Transfer Syntax: 1.2.840.10008.1.2.1
  * Compression: **None**, Explicit VR Little Endian
  * Compressed using: None, raw transmission from scanner to [dcmtk 3.54 PACS](https://dicom.offis.de/dcmtk.php.en).

* `2.dcm`
  * Transfer Syntax: 1.2.840.10008.1.2
  * Compression: **None**, Implicit VR Endian: Default Transfer Syntax for DICOM
  * Compressed using: [gdcmconv](http://gdcm.sourceforge.net/html/gdcmconv.html)
  * Compression command: `gdcmconv -w -M 2a.dcm 2.dcm`

* `3.dcm`
  * Transfer Syntax: 1.2.840.10008.1.2.2
  * Compression: **None**, Explicit VR Big Endian
  * Compressed using: [dcmconv](https://support.dcmtk.org/docs/dcmconv.html)
  * Compression command: `dcmconv +tb 3a.dcm 3.dcm`

* `4.dcm`
  * Transfer Syntax: 1.2.840.10008.1.2.5
  * Compression: Run-Length-Encoding (RLE) Lossless
  * Compressed using: [gdcmconv](http://gdcm.sourceforge.net/html/gdcmconv.html)
  * Compression command: `gdcmconv --rle 4a.dcm 4.dcm`
  * Notes: dcm2niix will decompress this using [in-built function](https://github.com/rordenlab/dcm2niix/blob/2bf2e482aec8e9959c6bd8e833cdccba3607c617/console/nii_dicom.cpp#L3470)

* `5.dcm`
  * Transfer Syntax: 1.2.840.10008.1.2.4.70
  * Compression: JPEG Lossless, Nonhierarchical, First- Order Prediction
  * Compressed using: [dcmcjpeg](https://support.dcmtk.org/docs/dcmcjpeg.html)
  * Compression command: `dcmcjpeg +e1 5a.dcm 5.dcm`
  * Notes: dcm2niix will decompress this using [in-built function](https://github.com/rordenlab/dcm2niix/blob/2bf2e482aec8e9959c6bd8e833cdccba3607c617/console/jpg_0XC3.cpp#L105)

* `6.dcm`
  * Transfer Syntax: 1.2.840.10008.1.2.4.80
  * Compression: JPEG-LS Lossless Image Compression
  * Compressed using: [gdcmconv](http://gdcm.sourceforge.net/html/gdcmconv.html)
  * Compression command: `gdcmconv --jpeg 6a.dcm 6.dcm`
  * Notes: dcm2niix will decompress this using [in-built function](https://github.com/rordenlab/dcm2niix/blob/2bf2e482aec8e9959c6bd8e833cdccba3607c617/console/jpg_0XC3.cpp#L105)

* `7.dcm`
  * Transfer Syntax: 1.2.840.10008.1.2.4.80
  * Compression: JPEG-LS Lossless Image Compression
  * Compressed using: [gdcmconv](http://gdcm.sourceforge.net/html/gdcmconv.html)
  * Compression command: `gdcmconv --jpegls 7a.dcm 7.dcm`
  * Notes: dcm2niix will decompress this using [CharLS](https://github.com/team-charls/charls)
  
* `8.dcm`
  * Transfer Syntax: 1.2.840.10008.1.2.4.80
  * Compression: JPEG-LS Lossless Image Compression
  * Compressed using: [dcmcjpls](https://support.dcmtk.org/docs/dcmcjpls.html)
  * Compression command: `dcmcjpls 8a.dcm 8.dcm`
  * Notes: dcm2niix will decompress this using [CharLS](https://github.com/team-charls/charls)

* `9.dcm`
  * Transfer Syntax: 1.2.840.10008.1.2.4.90
  * Compression: JPEG 2000 Image Compression (Lossless Only)
  * Compressed using: [gdcmconv](http://gdcm.sourceforge.net/html/gdcmconv.html)
  * Compression command: `gdcmconv --j2k 9a.dcm 9.dcm`
  * Notes: dcm2niix will decompress this using [OpenJPEG](https://www.openjpeg.org)

## Links

 * These images are identical to those at [dcm_qa](https://github.com/neurolabusc/dcm_qa) with the exception of the conversion to various transfer syntaxes.
 * [Notes on DICOM compression](https://crnl.readthedocs.io/jpeg_formats/index.html).
 * The DICOM standard includes [verbose notes on compression](http://dicom.nema.org/medical/dicom/current/output/chtml/part05/sect_8.2.html).
 * David Clunie provides concise notes on [DICOM compression](https://www.dclunie.com/medical-image-faq/html/part6.html).
 