# Zipping /Compression

* &#x20;Basic use case is compressing on the fly. Some data will be buffered by the [zipfile ](https://github.com/BuzonIO/zipfly#lib)deflater, but memory inflation is going to be very constrained. Data will be written to destination at fairly regular intervals.
*
