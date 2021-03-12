---
layout: post
title: CSV file management using C++
subtitle: Discussing about CRUD operations in a CSV file
cover-img: /assets/img/path.jpg
thumbnail-img: /assets/img/thumbnail/1.jpg
share-img: /assets/img/path.jpg
gh-repo: https://github.com/hadomanh/
gh-badge: [star, fork, follow]
tags: [C++, CSV, cheatsheet]
---

**CSV** is a simple file format used to store tabular data such as a spreadsheet or a database. **CSV** stands for **Comma Separated Values**. The data fields in a CSV file are separated/delimited by a comma (', ') and the individual rows are separated by a newline ('\n'). 

**CSV** file management in C++ is similar to text-type file management, except for a few modifications.

## Read
```cpp
    fstream fin(filename);

	if (fin.fail()) {
		cout << "File not found: " << filename << endl;
		fin.close();
		exit(0);
	}

	// Helper vars
	string line, col;

	// Read the column names
	// Extract the first line in the file
	getline(fin, line);

	// Create a stringstream from line
	stringstream ss(line);

	// Extract each column name
	while (getline(ss, col, ','));

	// Read data, line by line
	while (getline(fin, line)) {
		// Create a stringstream of the current line
		stringstream ss(line);

        // Extract each column data
		while (getline(ss, col, ',')) {
            cout << col << ' ';
        }
        cout << endl;
	}

	// Close file
	fin.close();

```