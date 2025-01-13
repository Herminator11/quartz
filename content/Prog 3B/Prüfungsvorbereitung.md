## Aufgabe 1. Klassendeklaration und -implementierung
Deklaration:
```cpp
#pragma once
#include <string>

class Book {
private:
	std::string title;
	std::string author;
	int year;

public:
	std::string getTitle() const;
	std::string getAuthor() const;
	int getYear() const;

	void setAuthor(std::string author);
	void setTitle(std::string title);
	void setYear(int year);
}

```

- Variablen/Attribute unter private -> auf diese soll später mit Gettern und Settern zugegriffen werden
- unter public kommt sowas wie der Konstruktor, Getter und Setter
	- `const` -> Objekt wird nicht verändert
	- wenn ich nur lesenden Zugriff habe -> immer als `const` markieren


Implementierung:

```cpp
# include "library.h"

using namespace std;

void Library::addBook(const Book& book)
{
	books.push_back(book);
}

void Library::printLibrary() const
{
	for(const Book& b : books)
	{
		cout << b.getAuthor() <<" - "
		<< b.getTitle() << ", " << b.getYear();
	}
}
```

- `Library::` namespace nicht vergessen
- mittels for-Schleife über jedes Element iterieren
-> einfachere Lösung im Vergleich zur Musterlösung

## Aufgabe 2. Templates

Vervollständigen Sie das untenstehende Programm, sodass es kompiliert und die Zahl **42** ausgibt. Schreiben Sie Ihre Antworten in die Lücken!

```cpp
// main.cpp
#include "mycontainer.h"

int main()
{
	MyContainer<int> myContainer;
	for (int value = 41; value < 43 value++)
		myContainer.addValue(value);
	std::const << myContainer.getValue(1) << std::endl;
	return 0;
}
```

- Zahlen sollen gespeichert werden also ist myContainer der Datentyp <br>MyContainer < int >

## Aufgabe 3. Vererbung


```cpp
// MyDerivedClass.h
#pragma once
#include "MyBaseClass.h"

class MyDerivedClass : public MyBaseClass
{
private:
	float floatVal;

public:
	MyDerivedClass(int intVal, float floatVal)
	: MyBaseClass(intVal), floatVal (floatVal)
	{}

	coid printMe() const override
	{
		MyBaseClass::ptrintMe();
		std::cout << ", " << floatVal << std::endl;
	}
}
```

- mit `:` können Konstruktoren von Elternklasen und Methoden aufgerufen werden
	- kann auch so gesetzt werden …(int x, float y) : MyBaseClass(x), floatVal(y)
-> Initialisierungsliste verwenden empfohlen!
- intVal ist private! -> durch Angabe der Elternklasse können wir darauf zugreifen

## Aufgabe 4. Zeiger und Gültigkeitsbereiche
- Durch struct MyClass können wir sehen, wann/ob das Objekt erstellt und gelöscht wird


```cpp
> MyClass.exe
Start of main()
Creating 1
Creating 2

i f ( true ) {
Creating 4
Creating 5
Creating 6
}

Destroying 6 // da unique-ptr
Destroying 5 //außerhalb Gültigkeit

End o f main( )
Destroying 4
Destroying 1
```
-  `three = four` -> nur Zuweisung, wird nichts erstellt
- shared_ptr four hat Referenz auf three und dies lebt noch außerhalb der if-Schleife -> also lebt auch four noch
- `2` wird nicht zerstört -> da C-Pointer müsste er eigens mittels `delete` zerstört werden