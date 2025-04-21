# Request Body key methods

### requestBody["content"].s(); => String
### requestBody["content"].b(); => Bool
### requestBody["content"].d(); => Double

### <span style="color:rgb(107, 255, 174)">requestBody["content"].has => check if the key exists??</span> 

### requestBody["content"].t(); => Type of the value

### requestBody["content"].i(); => Int64
### requestBody["content"].u(); => UnsignedInt64


## Design

![[Pasted image 20250324172658.png]]

### https://survey123.arcgis.com/share/f0e86221987d4a7aa265be4fdc2f5f72

```cpp
crow::json::wvalue response({  
    {"first", "Hello world!"}, /* stores a char const* hence a json::type::String */  
    {"second", std::string("How are you today?")}, /* stores a std::string hence a json::type::String. */  
    {"third", 54}, /* stores an int (as 54 is an int literal) hence a std::int64_t. */  
    {"fourth", (int64_t) 54l}, /* stores a long (as 54l is a long literal) hence a std::int64_t. */  
    {"fifth", 54u}, /* stores an unsigned int (as 54u is a unsigned int literal) hence a std::uint64_t. */  
    {"sixth", (uint64_t) 54ul},  
    /* stores an unsigned long (as 54ul is an unsigned long literal) hence a std::uint64_t. */  
    {"seventh", 2.f}, /* stores a float (as 2.f is a float literal) hence a double. */  
    {"eighth", 2.}, /* stores a double (as 2. is a double literal) hence a double. */  
    {"ninth", nullptr}, /* stores a std::nullptr hence json::type::Null . */  
    {"tenth", true} /* stores a bool hence json::type::True . */  
});  
return r;
```


```cpp
CROW_ROUTE(app, "/")  
        .name("hello")([] {  
            return "Hello World!";  
        });  
  
CROW_ROUTE(app, "/about")  
([]() {  
    return "About Crow example.";  
});


CROW_ROUTE(app, "/json")  
([] {  
    crow::json::wvalue x({{"message", "Hello, World!"}});  
    x["message2"] = "Hello, World.. Again!";  
    return x;  
});

// json list response  
CROW_ROUTE(app, "/json_list")  
([] {  
    crow::json::wvalue x(crow::json::wvalue::list({1, 2, 3}));  
    return x;  
});

// example which uses only response as a parameter without request being a parameter.  
CROW_ROUTE(app, "/add/<int>/<int>")  
([](crow::response &res, int a, int b) {  
    std::ostringstream os;  
    os << a + b;  
    res.write(os.str());  
    res.end();  
});  
  
// Compile error with message "Handler type is mismatched with URL paramters"  
//CROW_ROUTE(app,"/another/<int>")  
//([](int a, int b){  
//return crow::response(500);  
//});


CROW_ROUTE(app, "/add_json")  
        .methods("POST"_method)([](const crow::request &req) {  
            auto x = crow::json::load(req.body);  
            if (!x)  
                return crow::response(400);  
            int sum = x["a"].i() + x["b"].i();  
            std::ostringstream os;  
            os << sum;  
            return crow::response{os.str()};  
        });


CROW_ROUTE(app, "/params")  
([](const crow::request &req) {  
    std::ostringstream os;  
    os << "Params: " << req.url_params << "\n\n";  
    os << "The key 'foo' was " << (req.url_params.get("foo") == nullptr ? "not " : "") << "found.\n";  
    if (req.url_params.get("pew") != nullptr) {  
        double countD = crow::utility::lexical_cast<double>(req.url_params.get("pew"));  
        os << "The value of 'pew' is " << countD << '\n';  
    }    auto count = req.url_params.get_list("count");  
    os << "The key 'count' contains " << count.size() << " value(s).\n";  
    for (const auto &countVal: count) {  
        os << " - " << countVal << '\n';  
    }    return crow::response{os.str()};  
});
```

## Escape , to avoid problems


# Db Structure

```cpp
// ___________________________________  
// | user_id| questionId | answer    |  
// |_________________________________|  
// | 1      | 1          | my answer |  
// | 1      | 1          | my answer |  
// | 1      | 1          | my answer |  
// | 1      | 1          | my answer |  
// | 1      | 1          | my answer |  
// |_________________________________|  
  
// _________________  
// | id| username  |  
// |_______________|  
// | 1 | PowerCell |  
// | 1 | PowerCell |  
// | 1 | PowerCell |  
// | 1 | PowerCell |  
// |_______________|
```