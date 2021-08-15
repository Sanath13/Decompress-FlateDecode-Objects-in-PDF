# Decompress-FlateDecode-Objects-in-PDF

An attempt to decompress FlateDecode stream objects in PDF files using Regular Expression and Zlib Python libraries.

Portable Document Format (PDF) is a format mean to display content identically in all platforms and media. It is one of the most popular electronic document formats. It is a layout based format, it specifies the positions and fonts of the individual characters, of which the text is composed. 

The physical structure of PDF file is split into four parts such as Header, Body, Cross reference table and Trailer. Body is the main portion of a PDF file containing all types of objects that build the document. Stream object that contains instructions to render the page. 
_Most streams are compressed, the content of the stream is then binary data._

To decompress binary data between FlateDecode stream objects, we have a program "Decompress FlateDecode" that takes pdf file as an input, decompress FlateDecode stream data using zlib python library.

__**General structure:**__
The physical structure of PDF file is split into four parts such as Header, Body, Cross reference 
table and Trailer. 

1. Header includes a version number of PDF files
%PDF 1.7

2. Body is the main portion of a PDF file containing all types of objects that build the 
document.
1 0 obj
. . .
endobj
2 0 obj
. . .
endobj
. . .

3. Cross reference table containing offset of each object stored in the body part.
xref
0 6
0000000000 65535 f 
0000000010 00000 n 
0000000079 00000 n 
0000000173 00000 n 
0000000301 00000 n 
0000000380 00000 n

4. Trailer records the offset of cross reference table reference of certain special objects 
within the body of the file like reference object of the foot.
trailer
<<
 /Size 6
 /Root 1 0 R
>>
startxref
492
%%EOF

__**Filters:**__
A filter is an optional part of the specification of a stream, indicating how the data in the stream must be decoded before it is used. For example, if a stream has an ASCIIHexDecode filter, an application reading the data in that stream will transform the ASCII hexadecimal encoded data in the stream into binary data. The goal is to replace the clear-text stream from our PDF to a compressed stream. We can encode and decode stream objects using FlateDecode.

Consider the stream object in above example.
5 0 obj
<<
 /Length 15
>>
stream
BT
200 200 TD
/F1 12 Tf
(Hello Sanath, welcome to the project work!) Tj
ET
endstream
endobj

The new version of stream object when it is compressed.
5 0 obj
<<
 /Length 52
 /Filter /FlateDecode
>>
stream
xœs
á27P05P�qáÒw3T04R�IãÒðHÍÉÉ×Q(Ï/ÊIQÔT�Éâr
á�á·
Á
endstream
endobj

PDF supports a set of filters such as ASCIIHexDecode, ASCII85Decode, LZWDecode, FlateDecode, RunLengthDecode, CCITTFaxDecode, JBIG2Decode, DCTDecode, JPXDecode, Crypt.
In this repository, our focus is on FlateDecode.

Once we have decompressed the binary data in pdf files, we can use the output to compare two or more pdf files.

__**Purpose of this method:**__
Comparing two or more pdf files is essential task in many fields including enterprises. Because of the rich contents a pdf file can have, extracting contents from pdf can be a tough task. We have several methods to do that such as Image Processing, OCR, Text Mining etc. But one method has advantages over the other. One of the reasons why extracting text by decompressing FlateDecode is, it can be used to extract other information such as font size, text style, position etc which we can't do Image proceesing.
