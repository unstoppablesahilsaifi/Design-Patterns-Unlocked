
# 🎭 Abstract Factory Pattern

---

## 📌 Quick Intro

Factory Pattern = ek hi factory, ek hi family ke objects banati hai.  

Abstract Factory = factory ki factory → matlab multiple related objects banane ka framework.  

Samajh gaya bhai 👌 tu chaahta hai ki Abstract Factory Pattern ko bhi waise hi samjhaun jaise Factory ko samjhaya tha → **pehle without pattern (problem), phir with pattern (solution), simple code + output**.  
Chalo karte hain 👇  

---

## 📌 Problem (Without Abstract Factory)

Maan le tum ek **UI Application** bana rahe ho jo **Windows aur MacOS** dono pe chalni chahiye.  

Har OS ke liye tumhe **Button** aur **Checkbox** banane hain.  

---

### ❌ Without Abstract Factory (Direct `new` use karke)

```java
interface Button { void paint(); }
class WindowsButton implements Button {
    public void paint() { System.out.println("Windows Button"); }
}
class MacButton implements Button {
    public void paint() { System.out.println("Mac Button"); }
}

interface Checkbox { void paint(); }
class WindowsCheckbox implements Checkbox {
    public void paint() { System.out.println("Windows Checkbox"); }
}
class MacCheckbox implements Checkbox {
    public void paint() { System.out.println("Mac Checkbox"); }
}

public class Main {
    public static void main(String[] args) {
        String os = "MAC";  // ya to WINDOWS ya MAC

        // 👇 Client khud object decide kar raha hai
        if (os.equals("WINDOWS")) {
            Button b = new WindowsButton();
            Checkbox c = new WindowsCheckbox();
            b.paint();
            c.paint();
        } else {
            Button b = new MacButton();
            Checkbox c = new MacCheckbox();
            b.paint();
            c.paint();
        }
    }
}
````

---

### ✅ Output (if `os = "MAC"`)

```
Mac Button
Mac Checkbox
```

---

### ❌ Issues

* Client ke andar **if-else bada hota jaayega** agar aur products aaye (Slider, Textbox, Menu, etc).
* Har jagah client ko class names pata hone chahiye (`WindowsButton`, `MacButton`).
* **Tightly coupled** ho gaya.

---

## 📌 Solution (With Abstract Factory)

👉 Abstract Factory ka matlab:
Ek **factory of factories** jo related objects (Button + Checkbox) ek family ke hisaab se return kare.

---

### ✅ With Abstract Factory

```java
// Step 1: Products
interface Button { void paint(); }
interface Checkbox { void paint(); }

// Concrete Products - Windows
class WindowsButton implements Button {
    public void paint() { System.out.println("Windows Button"); }
}
class WindowsCheckbox implements Checkbox {
    public void paint() { System.out.println("Windows Checkbox"); }
}

// Concrete Products - Mac
class MacButton implements Button {
    public void paint() { System.out.println("Mac Button"); }
}
class MacCheckbox implements Checkbox {
    public void paint() { System.out.println("Mac Checkbox"); }
}

// Step 2: Abstract Factory
interface GUIFactory {
    Button createButton();
    Checkbox createCheckbox();
}

// Step 3: Concrete Factories
class WindowsFactory implements GUIFactory {
    public Button createButton() { return new WindowsButton(); }
    public Checkbox createCheckbox() { return new WindowsCheckbox(); }
}

class MacFactory implements GUIFactory {
    public Button createButton() { return new MacButton(); }
    public Checkbox createCheckbox() { return new MacCheckbox(); }
}

// Step 4: Client
public class Main {
    public static void main(String[] args) {
        String os = "MAC";  // suppose user Mac par hai

        GUIFactory factory = null;
        if (os.equals("WINDOWS")) {
            factory = new WindowsFactory();
        } else {
            factory = new MacFactory();
        }

        Button b = factory.createButton();
        Checkbox c = factory.createCheckbox();

        b.paint();
        c.paint();
    }
}
```

---

### ✅ Output (if `os = "MAC"`)

```
Mac Button
Mac Checkbox
```

---

## 📌 Difference samajh le

* **Without Factory** → Client har product ke liye `new` kar raha hai (WindowsButton, MacButton, WindowsCheckbox…).
* **With Abstract Factory** → Client sirf factory choose karta hai (WindowsFactory ya MacFactory), aur **us factory se saare related objects mil jaate hain**.

👉 Matlab, ek baar `MacFactory` select ki → saare Mac UI components mile.

---

💡 Simple line me:
**Factory Pattern** ek object banata hai.
**Abstract Factory Pattern** ek **family of related objects** banata hai.

---




# 🔥 Factory vs Abstract Factory

---

## 1. Factory Pattern (simple)

* Ek **single object** banata hai.  
* Client ke paas abhi bhi `if-else` rahta hai agar multiple product families hain.  

👉 Example:

```java
Shape s = ShapeFactory.getShape("CIRCLE");
````

Sirf **ek hi object** bana: Circle ya Square.

---

## 2. Abstract Factory Pattern

* Ek **family of related objects** banata hai.
* Client bas ek baar factory choose karta hai (Windows/Mac), phir us factory se multiple products le sakta hai **without aur if-else**.

👉 Example:

```java
GUIFactory factory = new MacFactory();  // only once decision

Button b = factory.createButton();
Checkbox c = factory.createCheckbox();
```

---

## 📍 Tera doubt:

"Client code me if-else abhi bhi hai, kya fayda hua?"

### ✅ Answer:

* Agar sirf ek jagah (startup point) pe if-else hai → that’s okay.
* Lekin **baaki client code clean ho gaya**.
* Agar 10 jagah Button aur Checkbox bana rahe ho → bina Abstract Factory ke 10 jagah if-else likhna padta.
* Abstract Factory me ek hi jagah OS decide kar lo, phir poore codebase me factory se object aata rahega.

---

## 📌 Real World Example

Soch lo tum ek app bana rahe ho jo Windows aur Mac dono pe chale:

### ❌ Without Abstract Factory

Har jagah check karna padega:

```java
if (os.equals("WINDOWS")) 
    new WindowsButton();
else 
    new MacButton();

if (os.equals("WINDOWS")) 
    new WindowsCheckbox();
else 
    new MacCheckbox();
```

👉 Agar app ke 50 screens hain → 100 jagah if-else likhna padega.

---

### ✅ With Abstract Factory

Sirf ek jagah decide:

```java
GUIFactory factory = new MacFactory(); // app start ke time
```

Phir poore app me use karo:

```java
Button b = factory.createButton();
Checkbox c = factory.createCheckbox();
```

👉 Ab **0 if-else duplication**.

---

## 🎯 Summary

* **Factory**: ek object banane ke liye helpful.
* **Abstract Factory**: ek **family of related objects** banane ke liye helpful, aur client ke codebase ko clean rakhta hai.

---

💡 Simple line me:
**Factory Pattern = ek waiter se ek dish** 🍽️
**Abstract Factory Pattern = ek restaurant choose karo, us restaurant ka pura menu milta hai** 🍴🥗🍕


