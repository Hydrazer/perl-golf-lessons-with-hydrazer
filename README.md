perl is a trash language but it's fun to golf in, because you can shoot yourself in the foot super easily and nobody can read it, well, except for you after this hopefully<br>
this is intended mostly for beginners cuz if you are pro perl golfer then will probably know all of these<br>
you must practice or else you will probably forget everything cause there is way too many things in perl<br>
also plz tell me more tricks so i can add them here

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

regex (there are definitely lots more tricks but i don't know them yet)<br>
check out the variable section after for some more regex tricks<br>
this is mostly recap and important stuff for golf<br>
this probably wont' teach you anything if you don't already know regex so maybe try [https://perldoc.perl.org/perlretut](https://perldoc.perl.org/perlretut) then practice on [https://regexcrossword.com/](https://regexcrossword.com/) and [https://regexr.com/](https://regexr.com/)<br>
try to find your own tricks then tell me them so i can add them here [https://perldoc.perl.org/perlre](https://perldoc.perl.org/perlre)

match
```pl
# match variable on literal string
$bruh = "chicken";
$bruh =~ m/chi/;

print $&;

# can have any delimiter
$bruh =~ m!chi!;

print $&;

# usually dont need m unless you are nesting matches (only with substitution / match eval thing)
$bruh =~ /chi/;

print $&;

# match a regex variable
# topic value $_ will implicity have run `$_ =~` on every bare regex
$bruh = "c|5";
$_ = "5";
/$bruh/;
print $& # "5"

# match a literal string (no variable interpolation)
$bruh = "c|5";
$_ = '$bruh';
m'$bruh';
print $& # "$bruh"

# sometimes you need to use \Q and \E if you have special characters in your string
# and want to match literal `*+[({` etc
$bruh = "***";
$_ = "*** bruh ***";
/$bruh/; # error
/\Q$bruh/; # good
/\Q$bruh\E bruh/; # good

# match returns truthy / falsy value on success / failure
$_ = "chicken";
print /n/ ? "there is n" : "no n";

# negate match (truthy when no match)
$_ = "chicken";
print $_ !~ /n/ ? "no n" : "there is no n";
```

substitution
```pl
$_ = "112233";
s/[1-2]/bruh/g; # $_ => "bruhbruhbruhbruh33"

# negate substitution
# returns 1 if didn't match
# returns "" if it did match
$_ = "112233";
$_ !~ s/[1-2]/bruh/g; # $_ => "bruhbruhbruhbruh33"

$_ = "112233";
print + ($_ !~ s/[1-2]/bruh/g); # $_ => "bruhbruhbruhbruh33", returns ""

# can also use any delimeter
$nice = "112233";
$nice =~ s{[1-2]}{bruh}g; # this is a funny one
$nice =~ s![1-2]!bruh!g;

# literal string again
$nice =~ s'[1-2]'bruh'g;

# eval the substition
$nice = "111222333";
$nice =~ s/1|2/$a = 3; $& + $a/ge; # $nice => "444666333"

# match any case
$nice = "aaAAcc";
$nice =~ s/a/$&c/gi; # $nice => "acacAcAccc"

# return the substituted string
$a = "nicen";
print $a =~ s/n/g/gr # "giceg"

# idk what /l does but EGIRL HAHAHAHA
$_ = "nicen";
print s/n/"g"/egirl # "giceg"

# return how many matches were substituted while also substituting
$_ = "nnicenn";
print s/nn/g/g, $/, $_ # "2\ngniceg"

```

translate (tr is the same as y)
you can negate this one too but i never used it
```pl
$_ = "nice";
y/n/c/;

print; # "cice"

# return translated
$_ = "nice";
print y/n/c/r; # "cice"

# negate match and return
$_ = "nice";
print y/n/c/rc; # "nccc"

# count of chars (negate nothing == everthing)
print y///c; # 4

# delete some stuff and return the amount of deletions
$_ = "nice";
print $_ =~ y/n//d; # 4

# squash your stuff (not needed too commonly)
$_ = "nniiiiiiceeeee";
print $_ =~ y/n//crs; # nnice

# --------------------------------------------

# cool sliding window using match eval
"123456" =~ /...(?{print "$&
"})^/ # "123\n234\n345\n456\n"
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

# stdin: "weird,people,are,inside"
$/ = ",";
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

# using array and other args
$, = ",";
@A = qw(neat fish and chips);
print @A, "what" # "neat,fish,and,chips,what"

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

$a $b
# variables used in sorting
# why did perl do this? i have no idea (foot shooter)

print sort {$a-$b} "32", "chicken" # chicken32

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
# can also use for string interpolation if you have \w after

$; = <>;
$\+=3*$;for<>;
print;

$; = $a = "chicken";
print "${a}wow"; # chickenwow
print "$;wow"; # chickenwow

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

for loops (never use foreach cuz it sucks)
```pl
# get all input and do math on them, newline separated
foreach $a (<>){print $a + 3, "\n"}
for $a (<>){print $a + 3, "\n"}
print $_ + 3, "\n" for<>;
print $_ + 3, $/ for<>;

# using $' to hold nested loop variables (MAKE SURE YOU DONT USE ANY MORE REGEX OR IT WILL REASSIGN. try to use `eq ne cmp` that kind of thing)
$_ = <>;

map{$n=$_;print $n + $_,$/}$_..99; # wont even work cuz it will reassign $_ the moment the map loop starts
for$n($_..99){print $n + $_,$/}
print $_ + $', $/for//..99;

# another example of using it inside the for loop w/ map
$, = $/;
//, print map $_ + $', 1..$' for 1..99;
```

built-in functions
```pl
sort

# defaults to string sort like js

$, = ",";
@A = ("chicken", "finger", "123");
print sort @A # "123,chicken,finger"

$, = ",";
@A = (63, 1, 2, 3);
@A = sort{$b <=> $a}@A # @A => (63, 3, 2, 1)

# can usually get away with - but i think there was one time it didn't work idk
@A = sort{$b-$a}@A # @A => (63, 3, 2, 1)

map

# applies some function to a list
# uses $_

@A = (3, 2, 3);
@A = map {$_ + 6} @A # @A => (9, 7, 9)

# when used with `if` or `&&` or `and` it will return "" when falsy
@A = (323, 3, 432, 5);
@A = map {$_ > 5 && $_ + 6} @A # @A => (329, "", 438, "");

# when used with `unless` or `||` or `or` it will return 1 when falsy
@A = (323, 3, 432, 5);
@A = map {$_ + 6 unless $_ <= 5} @A # @A => (329, 1, 438, 1);

# you can replace the {} with , IF YOU DONT HAVE ANY OTHER EXPRESSIONS INSIDE AND NO BRACKETS EITHER RIGHT NEXT TO THE MAP
map $_+3, 1,2,3
map $a=3,$_+$a, 1,2,3 # bad
map ($a+=3)+$_, 1..3 # bad
map $_+($a+=3), 1..3 # good
```