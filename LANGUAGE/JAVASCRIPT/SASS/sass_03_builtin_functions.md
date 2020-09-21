# Built-in Functions (내장함수)

[Visit Sass Functions.](http://sass-lang.com/documentation/Sass/Script/Functions.html)

## Sass Functions Cheat Sheet

1. RGB Functions
2. HSL Functions
3. Opacity Functions
4. Other Color Functions
5. List Functions
6. Map Functions
7. Selector Functions
8. String Functions
9. Number Functions
10. Introspection Functions
11. Miscel­laneous Functions

### RGB Functions

rgb(­$red, $green, $blue) Creates a color from red, green, and blue values.
rgba­($red, $green, $blue, $alpha) Creates a color from red, green, blue, and alpha values.
red(­$co­lor) Gets the red component of a color.
gree­n($­col­or) Gets the green component of a color.
blue­($c­olor) Gets the blue component of a color.
mix(­$co­lor1, $color2, [$weig­ht]) Mixes two colors together.

### HSL Functions

hsl(­$hue, $satur­ation, $light­ness) Creates a color from hue, satura­tion, and lightness values.
hsla­($hue, $satur­ation, $light­ness, $alpha) Creates a color from hue, satura­tion, lightness, and alpha values.
hue(­$co­lor) Gets the hue component of a color.
satu­rat­ion­($c­olor) Gets the saturation component of a color.
ligh­tne­ss(­$co­lor) Gets the lightness component of a color.
adju­st-­hue­($c­olor, $degre­es) Changes the hue of a color.
ligh­ten­($c­olor, $amount) Makes a color lighter.
dark­en(­$color, $amount) Makes a color darker.
satu­rat­e($­color, $amount) Makes a color more saturated.
desa­tur­ate­($c­olor, $amount) Makes a color less saturated.
gray­sca­le(­$co­lor) Converts a color to grayscale.
comp­lem­ent­($c­olor) Returns the complement of a color.
inve­rt(­$co­lor) Returns the inverse of a color.

### Opacity Functions

alph­a($­color) / opacit­y($­col­or) Gets the alpha component (opacity) of a color.
rgba­($c­olor, $alpha) Changes the alpha component for a color.
opac­ify­($c­olor, $amount) / fade-i­n($­color, $amount) Makes a color more opaque.
tran­spa­ren­tiz­e($­color, $amount) / fade-o­ut(­$color, $amount) Makes a color more transp­arent.

### Other Color Functions

adjust-color($color, [$red], [$green], [$blue], [$hue], [$saturation], [$lightness], [$alpha]) : Increases or decreases one or more components of a color.
scale-color($color, [$red], [$green], [$blue], [$saturation], [$lightness], [$alpha]) : Fluidly scales one or more properties of a color.
change-color($color, [$red], [$green], [$blue], [$hue], [$saturation], [$lightness], [$alpha]) : Changes one or more properties of a color.
ie-hex-str($color) : Converts a color into the format understood by IE filters.

### List Functions

Visit Sass Functions.

Lists in Sass are immutable; all list functions return a new list rather than updating the existing list in-place.
All list functions work for maps as well, treating them as lists of pairs.

length($list) : Returns the length of a list.
nth($list, $n) : Returns a specific item in a list.
set-nth($list, $n, $value) : Replaces the nth item in a list.
join($list1, $list2, [$separator, $bracketed]) : Joins together two lists into one.
append($list1, $val, [$separator]) : Appends a single value onto the end of a list.
zip($lists…) : Combines several lists into a single multidimensional list.
index($list, $value) : Returns the position of a value within a list.
list-separator($list) : Returns the separator of a list.
is-bracketed($list) : Returns whether a list has square brackets.

### Map Functions

Maps in Sass are immutable; all map functions return a new map rather than updating the existing map in-place.

map-get($map, $key) : Returns the value in a map associated with a given key.
map-merge($map1, $map2) : Merges two maps together into a new map.
map-remove($map, $keys…) : Returns a new map with keys removed.
map-keys($map) : Returns a list of all keys in a map.
map-values($map) : Returns a list of all values in a map.
map-has-key($map, $key) : Returns whether a map has a value associated with a given key.
keywords($args) : Returns the keywords passed to a function that takes variable arguments.

### Selector Functions

sele­cto­r-n­est­($s­ele­cto­rs...) Nests selector beneath one another like they would be nested in the styles­heet.
sele­cto­r-r­epl­ace­($s­ele­ctor, $original, $repla­cem­ent) Replaces $original with $repla­cement within $selector.

More at Sass Functions.

### String Functions

unqu­ote­($s­tri­ng) Removes quotes from a string.
quot­e($­str­ing) Adds quotes to a string.
str-­len­gth­($s­tri­ng) Returns the number of characters in a string.

More at Sass Functions.

### Number Functions

perc­ent­age­($n­umb­er) Converts a unitless number to a percen­tage.
roun­d($­num­ber) Rounds a number to the nearest whole number.
ceil­($n­umb­er) Rounds a number up to the next whole number.
floo­r($­num­ber) Rounds a number down to the previous whole number.
abs(­$nu­mber) Returns the absolute value of a number.
min(­$nu­mbe­rs...) Finds the minimum of several numbers.
max(­$nu­mbe­rs...) Finds the maximum of several numbers.
rand­om(­[$l­imi­t]) Returns a random number.

### Introspection Functions

feat­ure­-ex­ist­s($­fea­ture) Returns whether a feature exists in the current Sass runtime.
vari­abl­e-e­xis­ts(­$na­me) Returns whether a variable with the given name exists in the current scope.
glob­al-­var­iab­le-­exi­sts­($n­ame) Returns whether a variable with the given name exists in the global scope.
func­tio­n-e­xis­ts(­$na­me) Returns whether a function with the given name exists.
mixi­n-e­xis­ts(­$na­me) Returns whether a mixin with the given name exists.
insp­ect­($v­alue) Returns the string repres­ent­ation of a value as it would be repres­ented in Sass.
type­-of­($v­alue) Returns the type of a value.
unit­($n­umb­er) Returns the unit(s) associated with a number.
unit­les­s($­num­ber) Returns whether a number has units.
comp­ara­ble­($n­umber1, $numbe­r2) Returns whether two numbers can be added, subtra­cted, or compared.
call­($name, $args…) Dynami­cally calls a Sass function.

### Miscel­laneous Functions

if($­con­dition, $if-true, $if-fa­lse) Returns one of two values, depending on whether or not $condition is true.

uniq­ue-­id() Returns a unique CSS identi­fier.
