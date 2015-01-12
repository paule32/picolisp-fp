
Floating Point Math Library for (64 bit) PicoLisp
=================================================

PicoLisp has gotten along quite well for its entire history without floating point number support. Perhaps this floating point library is a solution looking for a problem, but it seems to me that floating point support might occasionally be useful - if only to shut up PicoLisp newcomers who demand floating point support!
 
PicoLisp does not have a floating point number type, only an infinite-precision integer type that can also be used as a fixed-point numeric type. However, it is possible to write a set of functions that treat PicoLisp integers as floating point numbers and perform floating point arithmetic.

The IEEE-754 standard specifies the bit encoding for single and double precision floating point numbers. Single precision numbers are encoded into 32 bit signed inteers, and double precision numbers are encoded into 64 bit signed integers. The fp.l library uses native functions written in C in the fp.c file to interpret PicoLisp integers as floating point numbers to perform floating point arithmetic and to call the functions in the C header file <math.h>. These functions include trigonometric and logarithmic/exponential operations.

The library fp.l provides several conversion functions to get to and from floating point numbers in PicoLisp. The function names in fp.l use a code to indicate which type of number is expected as an input or produced as an output. 32 bit single precision floating numbers are designated by the letter "r" (for real - the letter "f" that might be used for "float" is already used in the PicoLisp frac.l library to designate fractions or rational numbers). 64 bit double precision floating point numbers are designated by the letter "d", PicoLisp fixed point numbers are designated with "fix", integers are designated with "l" (long), and strings are designated with "s". 

The string to number conversion functions are

s2r - converts a string representation of a number to a single precision float. 

(s2r "1.0" ) gives 1065353216 - just a jumble of decimal digits, but

(hex 1065353216) gives 3F800000 

This shows the IEEE 754 format in hexadecimal. The most significant bit of the 32 bit word is the sign bit - zero in this case. The next eight bits (the 3F8 part) forms the exponent, and the 24 bit mantissa is represented by an implicit leading one and 23 bits of zeros. 

(hex (s2r "2.0") ) gives 4000000

In this case the sign bit is zero, the exponent is 400, and the mantissa is again an implicit leading one bit followed by 23 zero bits.

Going the other way,

(r2s 1065353216) gives "1.000000"

The default string format has six digits after the decimal point. An optional extra parameter can be supplied to format the string result using the standard C language printf formatting fields.

(r2s 1065353216 "%5.2f") gives "1.00"
(r2s 1065353216 "%5.2e") gives "1.00e+00"

Double precision works the same way:

(s2d "1.0") gives 4607182418800017408, and

(hex 4607182418800017408) gives 3FF0000000000000

Here, the most signifcant bit is the sign bit (zero), the 3FF0 is the exponent, and the mantissa is an implicit leading one followed by 52 zero bits.

(s2d "2.0") gives 4611686018427387904, and
(hex (s2d "2.0") ) gives 4000000000000000

(d2s 4607182418800017408) gives "1.000000", and
(d2s 4607182418800017408 "%5.2f") gives "1.00", and
(d2s 4607182418800017408 "%5.2e") gives "1.00e+00"

Using the string conversion functions are the easiest way to specify and view floating point numbers:

(setq A (s2d "1.0") )
(d2s A)

The following functions convert between the various numeric types: r2d, d2r, l2d, d2l, l2r, r2l, fix2d, d2fix, fix2r, r2fix.

Single precision floating point arithmetic is supported for addition, subtraction (including negation), multiplication, and division:

(r2s (r+ (s2r "1.0") (s2r "2.0") ) ) gives "3.000000"
(r2s (r- (s2r "1.0") (s2r "2.0") ) ) gives "-1.000000"
(r2s (r* (s2r "1.0") (s2r "2.0") ) ) gives "2.000000"
(r2s (r/ (s2r "1.0") (s2r "2.0") ) ) gives "0.500000"

(r2s (r- (s2r "1.0") ) ) gives "-1.000000"
(r2s (r+ (s2r "1.0") (s2r "2.0") (s2r "3.0") ) ) gives "6.000000"

Double precision floating point arithmetic works the same way:

(d2s (d+ (s2d "1.0") (s2d "2.0") ) ) gives "3.000000"
(d2s (d- (s2d "1.0") (s2d "2.0") ) ) gives "-1.000000"
(d2s (d* (s2d "1.0") (s2d "2.0") ) ) gives "2.000000"
(d2s (d/ (s2d "1.0") (s2d "2.0") ) ) gives "0.500000"

(d2s (d- (s2d "1.0") ) ) gives "-1.000000"
(d2s (d+ (s2d "1.0") (s2d "2.0") (s2d "3.0") ) ) gives "6.000000"

The existing integer comparison functions (<, <=, >, >=) can be used directly on floating point numbers (of the same type, single or double precision).

(> (s2d "1.0") (s2d "2.0") ) gives NIL
(< (s2d "1.0") (s2d "2.0") ) gives T

The rest of fp.l implements the functions provided in <math.h> for trigonometric functions, hyperbolic functions, square root, logarithm functions, and exponential functions. Floating point constants are also provided from <math.h> for Pi and several common roots and logarithmic values.

(d2s (dasin (s2d "1.0") ) ) gives "1.570796", or Pi/2 = 90 degrees

(d2s (dexp (s2d "1.0") ) ) gives "2.718282", or the constant "e"


Installation
============

To install:

1) Copy the fp.l file to the PicoLisp installation directory in the lib subdirecory.

2) Compile the fp.c file and create the shared libary libfp.so. Use the supplied build script for Linux or its equivalent for your operating system. 

3) Copy the libfp.so file (or its equivalent) to the lib subdirectory of the PicoLisp installation directory.

4) Use fp.l as any other PicoLisp library.


Files
=====

fp.l       - the PicoLisp source file
fp.c       - the C source file
fptest.l   - a set of tests to verify the functions in fp.l
build      - a simple build script for Linux
readme.txt - this file

--------------------------------------------------------------------------------

Greg Lee

25 Nov 2014

