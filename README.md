## curently WIP
perl is a trash language but it's fun to golf in, because you can shoot yourself in the foot super easily or use the terrible features to your advantage, and nobody can read it, well, except for you after this hopefully<br>
this is intended mostly for beginners cuz if you are pro perl golfer then will probably know all of these<br>
you must practice or else you will probably forget everything cause there is way too many things in perl<br>
also plz tell me more tricks so i can add them here

preface
```pl
# include this at the top of the file if you are a beginner until you have completed your initial solution
# this will force you to write "good perl" and so you won't foot shoot yourself too badly
use Moose;

# if your platform doesn't support Moose try
use warnings;
use diagnostics;
use strict;

# can also include this so you can use "say" it's just print but with newline
# easier to debug stuff since you dont need to reassign $\ or put $/ at the end of each print
use 5.01;

say "ebic";
say "ebic";

# alternatively use Dumper which will probably be even better for debugging
use Dumper;

my %h = qw(1 2 3 4);
print Dumper %h
```

variables
```pl
$scalar = 3;
@arr = (63, 520);
%hash = ("bruh" => 3, "nice" => 2);
$scalar_arr = [63, 524]; # not really useful at the moment but it will get its time to shine
${-34 - 35} = 3; # can also use evaluated strings as variables

print $scalar, "\n";
print join(", ", @arr), "\n";
print "$_ => $hash{$_}\n" for keys %hash; # use $ to get scalar from hash not % (same w/ array)
print join(", ", @$scalar_arr), "\n";
print $$_ for -100 .. 0; # "3" this uses double variables to get var w/ string value
```

strings
```pl
$_ = 3;
print 'bruh $_'; # "bruh $_"
print "bruh $_"; # "bruh 3"

@A = split / /, "i like           chicken";
@A = qq(@A);
print "@A"; # "i like           chicken"

@A = <@A>; # glob
print "@A"; # "i like chicken"

# using literal newline

print "$a -- $e$/";
print "$a -- $e\n";
print "$a -- $e
"; # shorter
```

type casting
```pl
print "3" + 3; # 6
print "n" == "x"; # 1 (true since int("n") == 0)
print "n" eq "x"; # "" (false since casting correctly)
print 1 eq "1"; # 1

$a = 0;
$a += "3 4"; # a => 3
$a += "3a
4"; # a => 6
$a += "3
4"; # a => 9

$a = 0;
$a += 032 # a => 26

$a = 0;
$a += "032" # a => 32 doesn't cast octal string

$a = 0;
@A = "1 2 3";
$a += "@A" # a => 1 funny way to get 1st item in numeric context
```

regex (there are definitely lots more tricks but i don't know them yet)<br>
this is a huge foot shooter so be careful<br>
check out the variable section after for some more regex tricks<br>
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

# foot shooting example with BOOBA
$_ = <>;
/(.)(.)/;
print $1 =~ /e/ ? $1 : $2 # will print "" since $1 & $2 are reassigned on the match (same with s///) use eq or assign match

($a, $b) = /(.)(.)/;
print $a =~ /e/ ? $a : $b # work

# get count of match
$_ = <>;
$c = /\d/g; # bad
($c) = /\d/g; # bad (will get first item that matched)
$c = () = /\d/g; # need to cast to array first and turn entire array into scalar
print $a =~ /e/ ? $a : $b # good

# cool sliding window using match eval
"123456" =~ /...(?{print "$&
"})^/ # "123\n234\n345\n456\n"
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

# /l modifies \w to match unicode but who cares about that when we got EGIRL HAHAHAHA
$_ = "nicen";
print s/n/"g"/egirl # "giceg"

# return how many matches were substituted while also substituting
$_ = "nnicenn";
print s/nn/g/g, $/, $_ # "2\ngniceg"

# you can eval without modifying the varaible
$_ = "sheesh";
s/./$e .= $& . e/ger;
print; # "sheesh"
```

translate (tr is the same as y)
you can negate this one with !~ as well but i never used it
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
```

for loops (never use foreach cuz it sucks)
another foot shooter
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

# if the first input is 0 in a <> <> range you may run into some trouble because it will try to do character range with "0\n"
# idk why this doesn't do the same with other numbers

# stdin "0\n5"
print$_, $/ for<>..<> # ""
print$_, $/ for<>|0..<> # "0\n1\n2\n3\n4\n5\n"

# stdin "1\n5"
print$_, $/ for<>..<> # "1\n2\n3\n4\n5"
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
# can read n chars from stdin

print$_,$/ for "wow", "chicken"; # "wow\nchicken"

# stdin: "weird,people,are,inside"
$/ = ",";
print "$_
" for <>; # "weird,\npeople,\nare,\ninside\n"

print$_,$/ for "wow", "chicken"; # "wow\nchicken"

# stdin: "rust"
$/=\1;
print $_, $/ for<> # "r\nu\ns\nt\n"

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
# can maybe replace |0 (int) but i can't think of something rn 

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
# why did perl do this? i have no idea (foot shooter with `my` declaration)

print sort {$a-$b} "32", "chicken" # chicken32

# only really happens in for / map / subroutine
my $a = 3;
print sort {$a-$b} 1..5; # "21354"

# --------------------------------------------

%!
# just found this one today lol while looking through golf shinh sols
# hashmap with a lot of items

$\+=$_ for<>;print
$\+=<>for%!;print # shorter

# --------------------------------------------
@- @+

# regex start / end match positions of $& $1 $2 $3 ...

$_ = "wow";
/w/;
print "@-" # 0

# global doesn't do anything unless you use sub or something
/w/g;
print "@-" # "0"

/w(.)/;
print "@-" # "0 1"


/w(.+)/;
print "@-" # "0 1"
print "@+" # "3 3" # since $& and $1 both end at 3

# --------------------------------------------

$=

# holds value of 60
# useful for time problems (60 seconds, 60 minutes)
# same as $- except can also hold negative numbers
# replacement for int function

# stdin: "3\n23\n2\n22"
<>;
print 60*(32+$_)for<>;
print$=*(32+$_)for<>; # shorter way

print int-5.3;
print$-=-5.3; # shorter
print-5.3|0; # doesn't work cuz of twos complement or something

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

built-in functions<br>
there are more but these are the useful ones i've found<br>
read more here [https://perldoc.perl.org/functions](https://perldoc.perl.org/functions)
```pl
glob

# splits a string into an array
# doesn't really work if you have {} or * in the string in it since it's the same as bash glob

@A = split /\s*/, <>
@A = glob <>; # shorter

@A = glob `dd`;

# --------------------------------------------

reverse

# reverses scalar and array
# kindy finnicky with scalar might need to inchworm it

print reverse "bruh"; # "bruh"
print ~~reverse "bruh"; # "hurb"

print reverse "bruh", "yeet"; # "yeetbruh"

# --------------------------------------------

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

# --------------------------------------------

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

# you can replace the {} with ,
# ONLY IF YOU DONT HAVE ANY OTHER EXPRESSIONS INSIDE AND NO BRACKETS EITHER RIGHT NEXT TO THE MAP
map $_+3, 1,2,3
map $a=3,$_+$a, 1,2,3 # bad
map ($a+=3)+$_, 1..3 # bad
map $_+($a+=3), 1..3 # good

# can also use the // trick but not as common as `for` as the outer block as that is usually shorter
$_ = <>;
@A = map{$- += $_ for $'..99} //..3

# --------------------------------------------

chomp
# kills a single trailing newline
# basically just $var =~ s/\n$//
# useful because perl input includes the $/ value if there is one
# returns the amount of lines chomped

# stdin: "bruh\nchicken\n"
chomp, $\ .= $_ for<>; print # "bruhchicken"

# stdin: "bruh\negg"
e=<>;
print $e # "bruh\n"

chomp($e=<>);
print $e # "bruh"

# also useful for lists
# <> can be scalar / array depending on context
@A=<>;
print "@A" # "bruh\n egg"

chomp(@A=<>);
print "@A" # "bruh egg"


# chomp returns the amount of lines chomped
# maybe useful if you need amount of lines or something while also assigning chomped values
# stdin: "bruh\negg\nneat"
print chomp(($n,@A)=<>); # 2

# --------------------------------------------

chop
# similar to chomp
# $var =~ s/.$//s
# not really that useful unless you want to delete the last char whilst returning it
# can also use on lists

# --------------------------------------------

do

# usually use this in ternaries when im doing testing and need multiple statements

/n/ ? do {$- += 1; $f += $-} : 0 for<>;

# --------------------------------------------

print
# print stuff
# if you have brackets right next to print you need to use + to lower print precedence
$a = 3;
print ($a += 3), "nice" # "6"
print(($a += 3),"nice") # lonq version
print+($a += 3),"nice" # "6nice"

# --------------------------------------------

printf / sprintf

# like printf / sprintf in c

$_ = sprintf "%b", <>;
printf "%b", <>;

# --------------------------------------------

eval
# interprets a string as if it was perl code injected in the current spot
# useful if you want to use $_ & regex without for loop reassigning
# also useful if you want to inject variables as operators, literals, etc

eval '$- += 30' x 5;
print $-; # "150"

# variable inject example
$_ = "aab"
/(.)(?!\1)(.)/;
eval'y/$1$2/$2$1/'; # won't work will interpret as literal $1$2
eval"y/$1$2/$2$1/";
print # "baa"

# --------------------------------------------

hex / oct
# convert from hex, oct, binary to dec

print hex "3a"; "58"
print oct "x" . "3a"; "58"
print oct b . "3a"; "58"
print oct 0 . "3a"; "58"

$_ = "032";
print oct; "26"

# --------------------------------------------

vec

# string indexing thing using bits
# bit num have to be power of 2
# i dont use it too much cuz im too noob
# usually list are shorter 

$a = "chicken";
vec($a, 0, 8) = ord("f"); # 8 bits index 0 from right to left
print $a # "fhicken"


$a = "chicken";
print vec($a, 0, 8) # 99 (ord of "c")
print vec($a, 0, 16) # 25448 (oct b . sprintf "%08b%08b", ord("c"), ord("h"))

# --------------------------------------------

pack

# --------------------------------------------

unpack

# --------------------------------------------

lc / uc / ucfirst / lcfirst

# string func
# lower upper upper-first-char lower-first-char

print lcfirst "BRUH" # "bRUH"

# --------------------------------------------

pop / shift / unshift / push
# add any amount of items to front / back or remove first / last

@A = (1, 2, 3);
print shift @A # "1"
unshift @A, "wow" # @A => ("wow", 2, 3)
print pop @A # "3"
push @A, "shirt" # @A => ("wow", 2, "shift")

# --------------------------------------------

keys
# keys of a hashmap as a list

# --------------------------------------------

sqrt
# lower precedence **.5

$_ = 32;
print sqrt$_*=3;
print+($_*=3)**.5;

# --------------------------------------------

pos
# position of regex match for global match
# kinda like @+ for each match
# kinda like @- for substitution eval
# idk dont really use this often

$a = "wow";
while (/w/g) {
	print pos $a # "1\n3"
}

$a =~ s/w/print pos $a/ge; # "02"

# --------------------------------------------

chr / ord
# character representation of number
# numeric representation of character

# --------------------------------------------

sin / cos / log
# some math functions

sub tan {
	my $n = pop;
	sin ($n) / cos ($n)
}

sub log10 {
	log(pop) / log(10)
}
```

command line / shebang flags<br>
must be on the first line or wont work
you can mix and match these but the order does matter with -e or -E<br>
read more here [https://metacpan.org/pod/perlrun](https://metacpan.org/pod/perlrun)
```pl
-p
# while(<>){eval '$code\n;'; print}

# use the semicolon trailing semicolon to your advantage
#!perl -p
$_ .= "nice"; s/bruh//
$_ .= "nice"; s;bruh; # shorter but be careful cuz it will actually replace it with \n

# --------------------------------------------

-n
# while(<>){eval 'code\n;';}
# also be careful of s;;

# stdin: "e\nf"
#!perl -n
$a .= $_;
print "[$_] [$a]"; # "[e\n] [e\n][f\n] [f\nf\n]"
$a =~ s;e;f

# --------------------------------------------

-M
# only useful in bash

#!perl -p
use List::Util qw(uniq);
$_=uniq/./g 

# shorter
perl -MList::Util=uniq -pe '$_=uniq/./g'

# --------------------------------------------

-0

# sets $/ to chr (oct (n))
# sets to \0 if nothing provided

#!perl -0
print ord($/) # "0"

#!perl -053
print ord($/) # "43" cuz oct(53) == 43

# --------------------------------------------

-l
# set $\ to $/ unless you provide an octal number after
# while(<>){chomp; eval '$code\n;';}

#!perl -053l

print "neat";
print "neat"; # neat+neat+

# "fix" for previous -n flag code using tr lol

# stdin: "e\nf"
#!perl -ln
$a.=$_;
print "[$_] [$a]"; # "[e] [e] [f] [ff]"
$a =~ y;e;f

#!perl -l153 -053
print $/ # "+k"

# --------------------------------------------

-a
# while(<>){@F = split /\s+/, $_; eval '$code\n;';}
# s;; trick should work i think

# stdin: "egg     wow"

#!perl -a
print "@F" # "egg wow"

# --------------------------------------------

-F
# same as -a but you can specify the regex
# defaults to //
# can omit the // if there's no literal space

# stdin: "seg\nwow"
#!perl -F
print "@F" # "s e g\nw o w"

# stdin: "s+e+g\nw+o+w"
#!perl -F/\+/
print "@F" # "s e g\nw o w"
```

misc
```pl
}{

# eskimo greeting operator
# useful when you have perl command line flags -n or -p
# no eskimo -n while(<>) {eval 'code1\n;'}
# eskimo -n while(<>) {code1 }{ eval '$code2\n;'}
# with -p while(<>) {code1 }{ eval 'code2\n;'; print}

# sum of inputs
#!perl -p
$\+=$_ }{

# --------------------------------------------

~~
# inchworm operator
# force scalar context

# stdin "1\nbruh\n3\na\nb\nc"
@A = map <>, 1..<>;
print scalar @A # length 5? what?!??!?!

# stdin "1\nbruh\n3\na\nb\nc"
@A = map ~~<>, 1..<>;
print ~~@A # length 1? noice

print reverse "bruh" # "bruh" no work cuz reverse default takes list
print ~~reverse "bruh" # "hurb"


# --------------------------------------------
\
# thelens device

# max of list
@A = (1, 2, 59);
\@B[@A]; # causes index 0-59 to be set to undef

print $#B # 59

# uniq values in list
use List::Util qw(uniq); # bad

\%H{@A};
print keys%H # good although it's random order
print ~~%H # count of unique

# can also use with input as it will cast to array
\@B[<>];

print $#B
# --------------------------------------------

lvalue

# another foot shooter
# ternary binds $a and $b as lvalue so it will actually look like print + (/n/ ? $a : $b) *= 3;
# you only need to worry about this if you are doing *= or += x= that kind of thing
# can usually solve with brackets
$a += 3, print /n/ ? $a : $b *= 3 for <>; # bad
$a += 3, print /n/ ? $a : ($b *= 3) for <>; # good

$a += 3, $b += 2, print /n/ ? $a : $b * 3 for <>; # works fine

# this can also be used to your advantage
${--$| ? a : b} += <> for %!; print "$a $b"
--$| ? $a : $b += <> for %!; print "$a $b" # shorter

# --------------------------------------------

my 
# local varaible declaration

# variable reset
(@A=()), push(@A, 1..$_), $A[0] = 3, print "@A$/" for <>;
my@A, push(@A, 1..$_), $A[0] = 3, print "@A$/" for <>;

# recursion
sub fib {
  $n = pop;
  $n < 2 ? $n : fib ($n - 1) + fib ($n - 2)
}
print fib 3 # "-3" # nani?!?!??!?!

sub fib {
  my $n = pop;
  $n < 2 ? $n : fib ($n - 1) + fib ($n - 2)
}
print fib 3 # "2" # will treat $n as a global variable unless you explicitly say it's not


# --------------------------------------------

redo
# ah shit, here we go again
# can be used for recursion as it saves $_ value on redo

($_ += $_ / 3) < 30 ? redo : print $_, $/ for <>

# block form
do{$_.="bruh"}until/.{10}/;print
{$_.="bruh";/.{10}/||redo}print # shorter

# with command line flags
#!perl -pl
($_ += $_ / 3) < 30 && redo
```


tips to reading perl
you will be able to do these steps in your head as you get better
```pl
# example code
$_=<>;eval's/(.)\1+//g;'x999;print

# separate it out by semicolon / }

$_=<>;
eval's/(.)\1+//g;'x999;
print

# add whitespace / brackets back
$_ = <>;
eval ('s/(.)\1+//g;' x 999);
print

# wow now you know that the code will read input into $_
# delete consecutive chars but leave singleton chars
# do that 999 times
# then implicitly print the $_ variable
# you are now perl pro

# --------------------------------------------

# example code 2

<>;s/(.)(.)/$-+=$1*$2+$_/ge,print$-,$/for<>


# expand for loops into their block form
<>;
for (<>) {
  s/(.)(?=.)/$- += $1 * $2 + $_/ge;
  print $-, $/
}

# wow now you know it will throw away first input
# sub used for looping this time since we dont need $_ later
# loop over $_ implicitly with adjacent digits globally (g flag)
# add to $- variable with some math involving match groups (e flag evals the replacement)
# print the intermediate sums (previous sums + current sum) plus the newline
```