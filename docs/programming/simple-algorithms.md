# Simple but helpfull algorithms

## Split number into digits
Example: `int num = 123; -> 1, 2, 3`

``` cpp linenums="1"
std::vector<int> digits;

int num = 123;
while(num > 0)
{
    // Or use them directly
    digits.push_back(num % 10);
    num /= 10;
}

// Example to print (reverse iterate vector)
for(size_t i = digits.size(); i > 0; i--)
    std::cout << std::to_string(digits.at(i));
```

**Thanks to https://stackoverflow.com/a/9302745**