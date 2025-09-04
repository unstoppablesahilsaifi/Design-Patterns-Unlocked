
# 🎯 **Singleton Pattern**

---

## 📌 **Definition**

👉 Singleton ek **Creational Design Pattern** hai jo ensure karta hai ki **ek class ka sirf ek hi object** bane aur us object ko globally access kiya ja sake.

---

## 🌍 **Real-Life Analogy**

* 👤 **Government PM/President** → Sirf ek hota hai poore desh ka. Har jagah se wahi instance access hota hai.  
* 🖨️ **Printer Spooler** → Agar multiple objects ban gaye toh sab apna-apna print queue maintain karenge → mess ho jayega. Isliye ek hi instance chahiye jo sab handle kare.
* **Database Connection** → Ek hi shared connection pool maintain karna efficient hai.  
---

## ⚠️ **Normal (Without Singleton) Problem**

```java
class DatabaseConnection {
    public DatabaseConnection() {
        System.out.println("Database Connection Created...");
    }
}

public class Main {
    public static void main(String[] args) {
        DatabaseConnection db1 = new DatabaseConnection();
        DatabaseConnection db2 = new DatabaseConnection();
        
        System.out.println(db1);
        System.out.println(db2);
    }
}
````

### ❌ Issue:

* Har `new DatabaseConnection()` call ek **naya object** banata hai.
* Database connections heavy hote hain → multiple connections banenge toh memory waste hoga.

---

## ✅ **Singleton Solution**

```java
class DatabaseConnection {
    // Step 1: Single instance ko hold karne ke liye static variable
    private static DatabaseConnection instance;
    
    // Step 2: Constructor private (taaki koi new na kar sake)
    private DatabaseConnection() {
        System.out.println("Database Connection Created...");
    }
    
    // Step 3: Public static method jo ek hi object return karega
    public static DatabaseConnection getInstance() {
        if (instance == null) {
            instance = new DatabaseConnection();
        }
        return instance;
    }
}

public class Main {
    public static void main(String[] args) {
        DatabaseConnection db1 = DatabaseConnection.getInstance();
        DatabaseConnection db2 = DatabaseConnection.getInstance();
        
        System.out.println(db1);
        System.out.println(db2);
    }
}
```

### 🎯 Output:

```
Database Connection Created...
DatabaseConnection@6d06d69c
DatabaseConnection@6d06d69c
```

👉 Sirf **ek hi instance** bana aur dono reference usi ko point kar rahe hain.

---

## 💡 **Where Singleton is Used in Real Life**

* 🗄️ Database connections (JDBC)
* 📝 Logging frameworks (Log4j, SLF4J)
* ⚡ Cache management
* ⚙️ Configuration managers
* 🌱 Spring Beans (by default singleton scope)

---

# ❓ **Doubt Clarifications**

---

## 🔑 **Normal Object Creation**

Jab hum aise likhte hain:

```java
DatabaseConnection db1 = new DatabaseConnection();
DatabaseConnection db2 = new DatabaseConnection();
```

👉 Har `new` call **naya object** banata hai.
Isliye `db1` aur `db2` alag-alag addresses pe honge.

---

## 🔑 **Singleton ka Twist**

Singleton me hum **direct `new` nahi karne dete** (constructor ko `private` bana dete hain).

* Matlab koi bhi `DatabaseConnection db = new DatabaseConnection();` likhega → ❌ compile error.
* Sirf class khud hi apne andar object create kar sakti hai.

---

## 🔑 **Kaise hota hai?**

```java
private static DatabaseConnection instance; // initially null
```

* Ye ek static variable hai jo **poore program me ek hi jagah memory me hota hai**.

```java
public static DatabaseConnection getInstance() {
    if (instance == null) {   // pehli baar null hoga
        instance = new DatabaseConnection();  // tabhi ek object banega
    }
    return instance;          // baaki time wahi return hoga
}
```

---

## 🔁 **Execution Flow**

```java
DatabaseConnection db1 = DatabaseConnection.getInstance();
```

👉 Pehli baar call hua:

* `instance == null` tha → naya object ban gaya → `db1` usko point karega.

```java
DatabaseConnection db2 = DatabaseConnection.getInstance();
```

👉 Doosri baar call hua:

* Ab `instance != null` hai → naya object nahi banega.
* Wahi **pehle se existing object** return hoga → `db2` bhi usi ko point karega.

---

## ✅ **Proof**

```java
System.out.println(db1);
System.out.println(db2);
```

🖥️ Output:

```
DatabaseConnection@6d06d69c
DatabaseConnection@6d06d69c
```

👉 Dono ka **address same** hai → matlab ek hi object.

---

## ⚡️ **Short Summary**

* Singleton ka main funda: **ek hi object banega, jitni baar chaho access karo**.
* Hum `new` directly nahi karte, kyunki `private constructor` hai.
* `getInstance()` ek **gatekeeper** hai jo ensure karega ki sirf ek hi object bane.


```
