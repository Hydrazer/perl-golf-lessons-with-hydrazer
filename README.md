variables
```pl
$scalar = 3;
@arr = (63, 520);
%hash = ("bruh" => 3, "nice" => 2);
$scalar_arr = [63, 524]; # not really useful at the moment but it will get its time to shine

print $scalar, "\n";
print join(", ", @arr), "\n";
print "$_ => $hash{$_}\n" for keys %hash;
print join(", ", @$scalar_arr), "\n";
```

special variables from most to least useful (how common it can be used to reduce code)<br>
read more here [https://perldoc.perl.org/perlvar](https://perldoc.perl.org/perlvar)
```pl
$_

# default topic variable for loop
# can regex match without using =~
# can use functions without specifying variable

print"$_\n" for ("bruh", "nice", "chicken");# "bruh\nnice\nchicken\n"
print for ("bruh", "nice", "chicken"); # "bruhnicechicken"

$_ = "bruh";
print length;# 4

$_ = "bruh moment";
s/bruh/nice/g;
print; # "nice moment"

# --------------------------------------------

$/

# input record separator
# defaults to newline "\n"

print$_,$/ for "wow", "chicken"; # "wow\nchicken"

$/ = ",";

# stdin: "weird,people,are,inside"
print "$_
" for <>; # "weird,\npeople,\nare,\ninside\n"


print$_,$/ for "wow", "chicken"; # "wow\nchicken"

# --------------------------------------------

$"

# list separator
# defaults to space

$a = 3;
print $a += 3, $", $a *= 5; # "6 30"

@a = ("ppap", "chicken");
print "@a" # "ppap chicken"

print "@{[sort map {$_ . 42} 'ppap', 'chicken']}" # "chicken42 ppap42"

$" = " => ";
print "@{[sort map {$_ . 42} 'ppap', 'chicken']}" # "chicken42 => ppap42"

# --------------------------------------------

$\

# output record separator
# defaults to ""

$_ = "wow";
$\ = "chicken";
print # "wowchicken"

# stdin: "nice\nsheesh\nmyguy"
chomp, $\ .= "3$_" for <>;
print "bruh" # bruh3nice3sheesh3myguy

# stdin: "3\n1\n2\n3"
<>;
$\ += $_ for <>;
print "bruh" # "bruh6"

# --------------------------------------------

$,

# output field separator
# defaults to ""
# more niche than the next few ones but i wanted to keep the separators together lol

print "cool", "chicken"; # "coolchicken"

$, = "nice";

print "cool", "chicken"; # "coolnicechicken"


# --------------------------------------------

$` $& $'

# regex match before
# regex match
# regex match after
# all IMMUTABLE so dont try to modify them

$_ = "bruh";
/r/;
print "$`, $&, $'"; # "b, r, uh"

# stdin: "32\nsheesh"
`dd` =~ $/;
print $` + 63, "$' my guy" # "95sheesh my guy"

# --------------------------------------------

$1 $2 $3 ...

# regex capture groups
# all IMMUTABLE so dont try to modify them

# stdin: "123123213123213"
<> =~ s/(.)(.)/$- += $1 + $2/ger; # haha booba
print $- # sum of sum of every 2 digits 

# stdin: "123123213123213"
<> =~ s/(.)(?=(.))/$- += $1 + $2/ger; # weird booba
print $- # sum of every digit plus the one next to it

# stdin: "32 59\n32 1"
`dd` =~ / .+
(\d+)/;

print $` * $& + $1 / $' # some funky math with 4 different numbers

# --------------------------------------------

$-

# always integer
# holds value of 0 default
# will never go below 0

$- -= 1; # 0
$- += 3 / 4; # 0

$- = 10;
$- /= 4; # 2

$- = 6;
print $- - 3.5; # 2.5

$- = 6;
print $- -= 3.5; # 2

# --------------------------------------------

@_
# subroutine argument list
# pop / shift will implicitly call this var

sub bruh {
  ($wow, $v) = @_;
  $wow + $v
}

print bruh 3, 5 # 8

sub bruh {
  shift / pop
}

print bruh 100, 5 # 20

# --------------------------------------------

$=

# holds value of 60
# useful for time problems (60 seconds, 60 minutes)
# same as $- except can also hold negative numbers

# stdin: "3\n23\n2\n22"
<>;
print 60*(32+$_)for<>;
print$=*(32+$_)for<>; # shorter way

# --------------------------------------------

$.

# amount of inputs read
# defaults to undef
# will often be 1 since the first input is usually not useful (amount of lines)
# iffy with `for<>` loops as it will assign $. to the amount of inputs (reads all input before the for<>)

# factorial
$n = 1;
$n *= $_ for 1..<>;
print $n

$. *= $_ for 1..<>;
print $. # shorter way


# multiply each input by the amount of inputs + 1
# stdin: "3\n1\n2\n3"
$n = <> + 1;
print$_*$n,$/for<>;

<>;
print$.*$_,$/for<>; # shorter way

# --------------------------------------------

$< $>

# some big numbers
# not really mutable (well idk how it works lol)

$b = 1;
@A = map {($a, $b) = ($b, $a + $b); $a} 1..$<;
print "@A" # a lot of fib numbers

# --------------------------------------------

$;

# something with hashes idk
# defaults to chr(28) or "\034"
# usually just use a variable next to a keyword to save whitespace

$; = <>;
$\+=3*$;for<>;
print

# --------------------------------------------

$%

# something about output page number idk
# defaults to 0
# usually same usecase as $;

# --------------------------------------------

$|

# holds the value of 0
# idk how it fully works but it's a cool boolean flipping thing

print $|; # 0
print ++$|; # 1
print ++$|; # 1

$| = 0;
print --$|; # 1
print --$|; # 0
print --$|; # 1


$| = 2;
print $|; # 1

$| = 0.99;
print $|; # 0

$| = -0.99;
print $|; # 0

# --------------------------------------------

$( $)

# 2 big numbers in a string idk maybe you will find a use for this
# can kinda use like $< $>
```
