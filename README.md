nnzoom: identify how to re-compress compressed files
====================================================

In an effort to store things uncompressed (to make best use of deduplication),
nnzoom tries to find out how to get the same binary stream.

Sometimes it is trivial - just the right header options need to be re-created,
timestamp and/or filename, though the stored filename may be somethign you
can't re-create easily.

It's often tedious to identify the compression level if not the best/fast
compression method was used, as only these are saved in the headers.

Occasionally it's hard, as some program versions and options are using
different bitstreams, and not everytime the official GNU sources.

        * gzip-1.2.4a.tar.gz {"gzip": ["-9", 35615, 8, 0, 917999688, 2, 3, null, null, null, null]}
        * gzip-1.2.4.tar.gz {"gzip": ["-9", 35615, 8, 0, 745769892, 2, 3, null, null, null, null]}
        * gzip-1.3.12.tar.gz {"gzip": ["-9", 35615, 8, 0, 1176517776, 2, 3, null, null, null, null]}
        * gzip-1.3.13.tar.gz {"gzip-1.3.12-ubuntu --rsyncable": ["-9", 35615, 8, 0, 0, 2, 3, null, null, null, null]}
        * gzip-1.3.9.tar.gz {"gzip": ["-9", 35615, 8, 0, 1166171396, 2, 3, null, null, null, null]}
        * gzip-1.4.tar.gz {"gzip": ["-9", 35615, 8, 0, 0, 2, 3, null, null, null, null]}
        * gzip-1.5.tar.gz {"gzip-1.3.12-ubuntu --rsyncable": ["-9", 35615, 8, 0, 0, 2, 3, null, null, null, null]}
        * gzip-1.6.tar.gz {"gzip": ["-9", 35615, 8, 0, 0, 2, 3, null, null, null, null]}

Currently supported
-------------------

- Deflate with "gzip" with any versions.  Many of the "--rsyncable" patches
  provide slightly different bitstreams, so ideally, these need to be kept as a
  separate executable.  Also gzip 1.2.4 sometimes compressed the final bytes
  differently.
- Deflate with "minigzip" and/or a zlib based tool.
- Deflate with "pigz", which has rsyncable as an option by default.
- As a test, "zopfli", and "7z -tgzip"

TODO
====

- Other compression tools (bzip2, xz)
- older "libatomic_ops" and "gc" tarballs use a too good compression which is not pigz/zopfli/7z
- Older "libz" likewise (wonder why?)
- Check if BSD is really using zlib which I called minigzip
