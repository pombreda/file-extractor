File\_Extractor is a modular archive scanning and extraction library that supports several popular compressed file formats. It gives a common interface to the supported formats, allowing one version of user code. It has a simple C interface, and is written in lightweight C++.

## Features ##

  * Supports ZIP, GZIP, 7-Zip (7Z), and RAR`[1]` archive formats.
  * Non-archive files act like archive of that one file, simplifying code.
  * Modular design allows removal of support for unneeded archive formats.
  * Optionally supports wide-character paths on Windows.
  * Archive file type identification can be customized

`[1]` RAR support is optional; enabling it changes the library's license.

## Code example ##

This minimal code example scans "demo.zip" for any .txt files and prints their contents to stdout. If it encounters an error, it prints it to stderr and exits program.

```
void error( fex_err_t );

void print_text_files()
{
    fex_t* fex;
    error( fex_open( &fex, "demo.zip" ) );
    while ( !fex_done( fex ) )
    {
        if ( fex_has_extension( fex_name( fex ), ".txt" ) )
        {
            const void* p;
            error( fex_data( fex, &p ) );
            fwrite( p, fex_size( fex ), 1, stdout );
        }
        error( fex_next( fex ) );
    }
    fex_close( fex );
}

void error( fex_err_t err )
{
    if ( err != NULL )
    {
        const char* str = fex_err_str( err );
        fprintf( stderr, "Error: %s\n", str );
        exit( EXIT_FAILURE );
    }
}
```