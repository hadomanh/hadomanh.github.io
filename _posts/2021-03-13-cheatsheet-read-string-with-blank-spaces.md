---
layout: post
title: Read string with blank spaces
subtitle: String cheatsheet
cover-img: /assets/img/path.jpg
thumbnail-img: /assets/img/thumbnail/1.jpg
share-img: /assets/img/path.jpg
gh-repo: https://github.com/hadomanh/
gh-badge: [star, fork, follow]
tags: [C, C++, Java, cheatsheet]
---
## C:
```c
    scanf("%d%*c", &number);
    scanf("%[^\n]%*c", string);
```

## C++:
```cpp
    cin << number;
    cin.ignore();
    cin.getline(cin, string);
```

## Java:
```java
    Scanner scanner = new Scanner(System.in).useDelimiter("\\n");
    string = scanner.next();
    scanner.close();
```