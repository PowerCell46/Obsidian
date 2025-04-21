
```cpp
int main() {  
    for (int i = 0; i < 10; ++i) {  
        for (int j = 0; j < 10; ++j) {  
            std::cout << i << ' ' << j << '\n';  
            if (false)  
                goto exitLoops;  
        }  
    }    exitLoops:  
        std::cout << "Exited loops.\n";  
        return 0;  
  
    return 0;  
}
```