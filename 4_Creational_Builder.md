
# 🚀 **Builder Design Pattern**

---

## 🎯 **What**

* Builder Pattern ek **creational design pattern** hai jo complex objects ko **step-by-step banata hai**.
* Jab ek class mein **bahut saare parameters (mandatory + optional)** hote hain to constructor overload ho jaata hai aur code messy lagta hai.
* Builder ise solve karta hai by:

  * **Readable object creation**
  * **Optional parameters handle easily**
  * **Immutability** support

---

## 🛠️ **Where Used**

* Jab ek object ke **bahut saare attributes ho** (aur har attribute optional/mandatory mix ho).
* Jab constructor overload kaafi confusing ho jaye (a.k.a **Telescoping Constructor Problem**).
* Real-world examples:

  * `StringBuilder`
  * `Stream.builder()`
  * Lombok `@Builder`
  * *Effective Java* → “use Builder when class has >4 params”

---

## ❌ **Bad Example (Without Builder – Telescoping Constructor)**

```java
class User {
    private String name;
    private int age;
    private String email;
    private String phone;

    public User(String name, int age, String email, String phone) {
        this.name = name;
        this.age = age;
        this.email = email;
        this.phone = phone;
    }

    @Override
    public String toString() {
        return name + " (" + age + ") " + email + " " + phone;
    }
}

public class Main {
    public static void main(String[] args) {
        // Must pass everything, even if not needed
        User user = new User("Sahil", 25, "sahil@gmail.com", null);
        System.out.println(user);
    }
}
```

**Output:**

```
Sahil (25) sahil@gmail.com null
```

⚠️ **Problem**:

* Agar 10 fields hoti to 10 constructor overloads banane padte.
* Readability → gone ❌

---

## ✅ **Good Example (With Builder)**

```java
class User {
    private final String name;
    private final int age;
    private final String email;
    private final String phone;

    private User(Builder builder) {
        this.name = builder.name;
        this.age = builder.age;
        this.email = builder.email;
        this.phone = builder.phone;
    }

    public static class Builder {
        private String name;  
        private int age;      
        private String email; 
        private String phone; 

        public Builder(String name) { // mandatory
            this.name = name;
        }

        public Builder age(int age) {
            this.age = age;
            return this;
        }

        public Builder email(String email) {
            this.email = email;
            return this;
        }

        public Builder phone(String phone) {
            this.phone = phone;
            return this;
        }

        public User build() {
            return new User(this);
        }
    }

    @Override
    public String toString() {
        return name + " (" + age + ") " + email + " " + phone;
    }
}

public class Main {
    public static void main(String[] args) {
        User user1 = new User.Builder("Sahil")
                            .age(25)
                            .email("sahil@gmail.com")
                            .phone("9999999999")
                            .build();

        User user2 = new User.Builder("Amit")
                            .email("amit@xyz.com")
                            .build();

        System.out.println(user1);
        System.out.println(user2);
    }
}
```

**Output:**

```
Sahil (25) sahil@gmail.com 9999999999
Amit (0) amit@xyz.com null
```

---

## 📝 **Key Points**

* `Builder` = step-by-step object creation.
* `.build()` = final step where object gets created.
* No need to pass unwanted params.
* Object is **immutable** since fields are `final` and no setters.

---

## 🔥 **Interview Follow-ups**

* Difference between **Factory vs Builder**

  * Factory → creates object in **one step**
  * Builder → creates object in **multiple steps (step-by-step)**
* How Builder solves **Telescoping Constructor Problem**
* Real-life analogy → **Pizza Order** 🍕: base mandatory, toppings optional, `.build()` = final pizza
* Example from JDK → `StringBuilder`, `Stream.builder()`
* When to use Builder vs Constructor vs Factory

---

## 📌 Compare – Normal vs Builder (For Doubt)

### ✅ Normal Constructor (short & simple, but rigid)

```java
User u1 = new User("Sahil", 25, "sahil@gmail.com", "9999999999");
```

* Short hai, lekin:

  * Har parameter order maintain karna padega
  * Agar optional chhodna ho to `null` dena padega
  * Readability kharab → samajhna mushkil ki 25 kya hai, phone ya age?

---

### ✅ Builder (longer to write, but flexible + readable)

```java
User u1 = new User.Builder("Sahil")
                  .age(25)
                  .email("sahil@gmail.com")
                  .phone("9999999999")
                  .build();
```

* Code bada hai, par:

  * Readable → clear dikhta hai `age(25)`, `email(...)`
  * Order important nahi hai (pehle phone then email bhi chalega)
  * Optional fields skip kar sakte ho easily
  * Object immutable banta hai

---

## 📌 Simple Rule

* **Few fields (2-3)** → Constructor is better 👍
* **Many fields (5+) + optional fields** → Builder pattern is better 👍

---

## 📌 Ekdum Short Example (Minimal Builder)

```java
class User {
    private String name;
    private int age;

    private User(Builder b) {
        this.name = b.name;
        this.age = b.age;
    }

    public static class Builder {
        private String name;
        private int age;

        public Builder name(String n) { this.name = n; return this; }
        public Builder age(int a) { this.age = a; return this; }

        public User build() { return new User(this); }
    }

    public String toString() { return name + " (" + age + ")"; }
}

public class Main {
    public static void main(String[] args) {
        User u = new User.Builder().name("Sahil").age(25).build();
        System.out.println(u);
    }
}
```

**Output:**

```
Sahil (25)
```

---

## 🔥 Conclusion

* **Chhoti class** → normal constructor
* **Badi/complex class (lots of params)** → Builder

---

