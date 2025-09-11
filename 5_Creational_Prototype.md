
# 🚀 Prototype Design Pattern


* Prototype ek **creational design pattern** hai.
* Isme hum **new object ko create karne ke bajaye ek existing object ka clone banaate hain**.
* Useful jab:
  * Object creation **expensive/complex** ho
  * Object ko **baar-baar same config** ke sath banana ho

👉 Ek line: **Prototype = “Copy-Paste Object”**

---

## 🛠️ Where used

* Game characters cloning (enemy objects)
* Document editors (copying shapes, formatting)
* Cache mein same config wale objects dupliate karna
* Java libraries: `Object.clone()` method

---

## ❌ Without Prototype (Problem)

```java
class Employee {
    private String name;
    private String dept;

    public Employee(String name, String dept) {
        // Imagine DB call or API call to fetch defaults (expensive)
        System.out.println("Fetching data from DB..."); 
        this.name = name;
        this.dept = dept;
    }

    @Override
    public String toString() {
        return name + " - " + dept;
    }
}

public class Main {
    public static void main(String[] args) {
        Employee e1 = new Employee("Sahil", "IT");
        Employee e2 = new Employee("Amit", "IT"); // Again fetch DB ❌

        System.out.println(e1);
        System.out.println(e2);
    }
}
````

**Output:**

```
Fetching data from DB...
Fetching data from DB...
Sahil - IT
Amit - IT
```

⚠️ Problem: Har object banate time **DB/API call repeat ho raha hai**, costly ❌

---

## ✅ With Prototype (Solution)

```java
class Employee implements Cloneable {
    private String name;
    private String dept;

    public Employee(String name, String dept) {
        System.out.println("Fetching data from DB...");
        this.name = name;
        this.dept = dept;
    }

    // Clone method (shallow copy)
    @Override
    protected Object clone() throws CloneNotSupportedException {
        return super.clone();
    }

    public void setName(String name) { this.name = name; }

    @Override
    public String toString() {
        return name + " - " + dept;
    }
}

public class Main {
    public static void main(String[] args) throws CloneNotSupportedException {
        Employee e1 = new Employee("Sahil", "IT");

        // Clone from existing object
        Employee e2 = (Employee) e1.clone();
        e2.setName("Amit");  // only change name

        System.out.println(e1);
        System.out.println(e2);
    }
}
```

**Output:**

```
Fetching data from DB...
Sahil - IT
Amit - IT
```

👉 Dekha? **DB call ek hi baar hua**, aur dusra object bas clone se ban gaya ✅

---

## 📝 Key Points

* Prototype pattern = **clone existing object instead of re-creating**.
* Java mein `Object.clone()` support karta hai.
* **Shallow Copy** vs **Deep Copy** important hai:

  * Shallow → references copy ho jaati hain
  * Deep → actual new objects ban jaate hain

---

## 🔥 Interview Follow-ups

* Difference between **Prototype vs Factory**?

  * Factory → har baar **new object banata hai**
  * Prototype → **existing object ka clone** deta hai
* How to implement **deep copy** in Java? (e.g., using serialization)
* Where Prototype is better than Builder?

  * Builder → jab config step-by-step define karni ho
  * Prototype → jab ek existing template object already hai


---

## ⚡ Actual Prototype ka fayda kaha hai?

* Har case mein **ek original (prototype)** object banana padta hai ✅
* Lekin uske baad ke **clones cheap hote hain** (costly constructor / DB call / API call repeat nahi hota)

👉 Matlab:

* **Normal way** → har baar costly constructor chalega ❌
* **Prototype way** → costly constructor ek hi baar chalega, baaki clones "saste copies" honge ✅

---

## 🔍 Example samajh:

### Without Prototype

```java
Employee e1 = new Employee("Sahil", "IT"); // DB call
Employee e2 = new Employee("Amit", "IT");  // DB call again ❌
Employee e3 = new Employee("Raj", "IT");   // DB call again ❌
```

👉 Har object par **DB/API call repeat**.

### With Prototype

```java
Employee e1 = new Employee("Sahil", "IT"); // DB call ek hi baar

Employee e2 = (Employee) e1.clone(); // no DB call
e2.setName("Amit");

Employee e3 = (Employee) e1.clone(); // no DB call
e3.setName("Raj");
```

👉 Sirf ek hi baar **DB call hua**, baaki sab **clone** hue bina extra cost ke.

---

## 📌 Simple Analogy

* **Without Prototype** → Har baar restaurant jaake khana banana
* **With Prototype** → Ek hi dish banao, fir usi ka copy-box bana lo ghar mein (clone) 🍱

---

## ✅ Correct Understanding

* Haan bhai, **1 original object banega hi** (wo prototype hai)
* Lekin uske baad jitne bhi objects chahiye, wo **cheap clone** se banenge → yehi Prototype ka asli fayda hai

---

🔥 Matlab:
Prototype = **Ek baar mehenga kaam karo, fir uska unlimited sasta copy nikaal lo**


