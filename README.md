# randpass2

Create random passwords from cubic or spherical bases

A simple, yet flexible, command-line random password generator.

  - Passwords are of an arbitrarily specified length (default=64)
  - All output is to STDERR except the generated password

Requirements: PHP >5.4

_built and tested with php 8.2_

This is an evolution of [randpass](https://github.com/stroggprog/randpass). Where randpass created a grid of `n*n` characters, where n is the length of the origin string, this creates a cube by creating n number of grids (`n*n*n`).

The passwords are then generated by creating coordinates in triplets (x,y,z) to act as coordinates into the cube.

As a secondary option, the coordinates can be generated as vector coordinates within a sphere, with the sphere having the diameter of the side of the cube, and coordinates based on the centre of the sphere, e.g. <0,0,0> is the middle. In spherical mode, coordinates can be both positive and negative.

A tertiary option allows both cubic and spherical modes, with each character of the password generated by alternating between the two modes. When alternating, both the cubic and spherical modes use the same grid. The difference is in the way the coordinates are derived and used, and that in spherical mode some characters from the cubic grid are unavailable (they are within the cube, but outside the sphere - the corners)

## How it works
The passwords are generated from a string of UTF-8 characters, known as the origin string. The string is used to generate a grid of randomly positioned characters. Each character appears exactly n<sup>n</sup> times, where `n` is the number of characters in the origin string.

Further, each character only appears once per line of the grid. The number of times it may appear in each column is somewhere in the range 0-n.

The grid is known as a card. The process is repeated to generate as many cards as there are characters in the origin string. This creates the cube. Since the cards are generated independently of any other factor, the number of times a character may appear in any column in the z-coordinate is somewhere in the range 0-n.

## Options
From the help (`randpass2 -h`):

```
RandPass v1.0.1
Creates password using cubic and/or spherical algorithms (default=cubic)
Options:
    <n> = number of characters in password returned
    -c  = cubic algorithm (default)
    -e  = echo password to STDERR (still also goes to STDOUT)
    -g  = print randomly generated grid result is generated from
    -h  = print this help and exit
    -l  = limit output (almost quiet, just version and password)
    -m  = mixed spherical and cubic algorithms
    -p  = print origin and exit
    -q  = quiet mode (only print password)
    -s  = spherical algorithm

Options can be strung together, e.g. -gq
    -h overrides everything
    -m overrides -c
    -p overrides everything except -h
    -q overrides -l but not -e
    -s overrides -m and -c
Note that only the password is output to STDOUT, everything else goes to STDERR,
so redirection to a file is safe as only the password is redirected.
```

If '?' character-on-a-background starts appearing in your passwords, then you need to remove any characters from the origin string that were created using the `AltGr` key, as these are almost certainly multi-byte characters, and this generator only works with single-byte UTF-8 characters. By default, there are no multi-byte characters in the origin.

You can include non-keyboard characters, such as `╚`, `╩` & `╝` etc., but these are best avoided for the obvious reason that they are hard to remember how to type since you need to know their ASCII codes. It can also be confusing too, as `}` (AltGr+0) and `}` (Shift+]) may look the same, but since they were created by different key combinations there is no guarantee they have the same UTF-8 code.
