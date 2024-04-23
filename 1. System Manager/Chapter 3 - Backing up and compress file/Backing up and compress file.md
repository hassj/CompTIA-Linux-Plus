# CHAPTER 3: BACKING UP AND COMPRESS FILE

## Tar

Tar command used to create file archive. it commonly used in standard file system. By default tar is not compress, but it make the content size small than original. Due to the more efficient use of
blocks in file system and not compression. By default block size is 4K

`tar -cf file.tar file1 file2`
` tar -xf file.tar dest_location1`

various compression utilities supported on linux

- gzip / gunzip
- bzip2 / bunzip2
- xz -z / xz -d

> These can be used indepently or with tar command.

Creating a zipped archived

>tar -czf (gzip)
>
>tar -cjf (bzip2)
>
> tar -cJf (xz)

## Using less command to page through the arvchive

`less file.tar`

## Using cpio (copy io) as an archiving tool

`find ./ -name "*.html" | cpio -o /tmp/doc.cpio`
`cpio -id  < /doc.cpio `




