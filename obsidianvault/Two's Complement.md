Classic unsigned numbers are converted from base 2 to base 10 by doing the following:
$$
\displaystyle\sum_{i=31}^0 x_i \cdot 2^i
$$
where $x$ is the value of the $i$'th bit, either 0 or 1.
For the case of 0101, that would be converted to base 10 like so:p
$$
(0 \cdot 2^3) + (1 \cdot 2^2) + (0 \cdot 2^1) + (1 \cdot 2^0) = 5.
$$
More generally, we say that the value of the $i$'th digit $d$ is
$$
d \cdot Base^i
$$
where $i$ increases from right to left.

A number in two's complement can be converted to base 10 by negating the first term in the sequence above. That becomes
$$
x_{31} \cdot (-2^{31}) \displaystyle\sum_{i=30}^0 x_i \cdot 2^i
$$
