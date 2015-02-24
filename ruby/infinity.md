# Infinity in ruby (useful for lazy numerators, range, ...

```ruby
inf = 1/0.0
r = -inf..inf # => -Infinity..Infinity
r.include?(0) # => true
r.include?(-1000) # => true
r.include?(10000) # => true
```
