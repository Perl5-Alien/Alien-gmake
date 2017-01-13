# Alien::gmake [![Build Status](https://secure.travis-ci.org/Perl5-Alien/Alien-gmake.png)](http://travis-ci.org/Perl5-Alien/Alien-gmake)

Find or build GNU Make

# SYNOPSIS

    use Alien::gmake;
    # GNU Make should now be in your PATH if it wasn't already
    
    my $gmake = Alien::gmake->exe;
    system $gmake, 'all';
    system $gmake, 'install';

# DESCRIPTION

Some packages insist on using GNU Make.  Some platforms refuse to come with GNU Make.
Sometimes you just want to be able to build packages that require GNU Make without
having to check the version of Make each time.  This module is for that.  It uses the
system provided GNU Make if it can be found.  Otherwise it will download and install
it into a directory not normally in your path so that it can be used when you 
`use Alien::gmake`.  This way you can use it when you need it, but not muck up your
environment when you don't.

If possible, it is better to fix the package so that it doesn't require GNU make
extensions, making it more portable.  Unfortunately, sometimes this isn't an option.

This class is a subclass of [Alien::Base](https://metacpan.org/pod/Alien::Base), so all of the methods documented there
should work with this class.

# METHODS

## exe

    my $gmake = Alien::gmake->exe;

Return the "name" of GNU make.  Normally this is either `make` or `gmake`.  On
Windows (and possibly other platforms), it _may_ be the full path to the GNU make
executable.

To be usable on all platforms you will have to first add directories returned
from `bin_dir` to your `PATH`, for example:

    use Alien::gmake;
    use Env qw( @PATH );
    
    unshift @PATH, Alien::gmake->bin_dir;
    system "@{[ Alien::gmake->exe ]}";
    system "@{[ Alien::gmake->exe ]} install";

## bin\_dir

    my @dir = Alien::gmake->bin_dir;

Returns the list of directories that should be added to `PATH` in order for the
shell to find GNU make.  If GNU make is already in the `PATH`, this will return
the empty list.  For example:

    use Alien::gmake;
    use Env qw( @PATH );
    
    unshift @PATH, Alien::gmake->bin_dir;

# HELPERS

## gmake

    %{gmake}

Returns either make or gmake depending on how GNU make is called on your 
system.

# AUTHOR

Graham Ollis <plicease@cpan.org>

# COPYRIGHT AND LICENSE

This software is copyright (c) 2014 by Graham Ollis.

This is free software; you can redistribute it and/or modify it under
the same terms as the Perl 5 programming language system itself.
