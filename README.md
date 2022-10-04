variables
```pl
$scalar = 3;
@arr = (63, 520);
%hash = ("bruh" => 3, "nice" => 2);

print $scalar, "\n";
print join(", ", @arr), "\n";
print "$_ => $hash{$_}\n" for keys %hash;
```
