**Event bubbling** and **event capturing** are two phases of **event propagation** in the DOM, describing how an event travels through the DOM hierarchy.

---

### **Event Propagation Basics**
When an event (e.g., `click`, `keydown`) occurs on a target element, it doesn't just stay confined to that element. Instead, it travels through the DOM in **phases**:

1. **Capturing Phase** (Event Capturing)
2. **Target Phase**
3. **Bubbling Phase** (Event Bubbling)

---

### **1. Event Capturing**
- **What is it?**  
  In the **capturing phase**, the event starts at the **root** of the DOM tree (`document`) and travels **down** the DOM hierarchy toward the target element.

- **How does it work?**
  - The event listener on parent elements can catch the event **before** it reaches the target element.
  - Capturing is **not enabled by default** but can be enabled by setting the third parameter of `addEventListener()` to `true`.

- **Example:**
  ```html
  <body>
    <div id="parent">
      <button id="child">Click me</button>
    </div>
  </body>
  ```

  ```javascript
  const parent = document.getElementById("parent");
  const child = document.getElementById("child");

  parent.addEventListener(
    "click",
    () => console.log("Parent captured"),
    true // Enable capturing phase
  );

  child.addEventListener("click", () => console.log("Child clicked"));
  ```

  **Output when clicking the button (`#child`):**
  ```
  Parent captured
  Child clicked
  ```

---

### **2. Target Phase**
- **What is it?**  
  This phase occurs **at the target element** itself, where the event's default behavior is executed.

- **Example:**  
  Continuing from the above example, the `child.addEventListener()` will handle the event during the **target phase**.

---

### **3. Event Bubbling**
- **What is it?**  
  In the **bubbling phase**, the event starts at the target element and travels **up** the DOM hierarchy to the root (`document`).

- **How does it work?**
  - This is the **default behavior** in most cases.
  - Event listeners on parent elements can catch events that were triggered on their child elements as they "bubble up."

- **Example:**
  ```javascript
  parent.addEventListener("click", () => console.log("Parent bubbled"));
  ```

  **Output when clicking the button (`#child`):**
  ```
  Child clicked
  Parent bubbled
  ```

---

### **Key Differences Between Bubbling and Capturing**
| **Aspect**           | **Event Capturing**                          | **Event Bubbling**                     |
|-----------------------|----------------------------------------------|----------------------------------------|
| **Direction**         | Top-down (from `document` to target element) | Bottom-up (from target element to `document`) |
| **Default Behavior**  | Not enabled by default                      | Enabled by default                     |
| **Use Case**          | Handle events before they reach the target  | Handle events after the target processes them |

---

### **Practical Applications**
#### **a. Event Delegation**
- Use **bubbling** to handle events for multiple child elements by attaching a single listener to a parent element.

  ```javascript
  const parent = document.getElementById("parent");
  parent.addEventListener("click", (e) => {
    if (e.target.tagName === "BUTTON") {
      console.log(`Button ${e.target.id} clicked`);
    }
  });
  ```

#### **b. Prevent Unintended Parent Handling**
- Use `stopPropagation()` to prevent events from bubbling or capturing beyond a certain element.

  ```javascript
  child.addEventListener("click", (e) => {
    e.stopPropagation();
    console.log("Child clicked only");
  });
  ```

#### **c. Capturing for Global Behavior**
- Use **capturing** to intercept events before they reach a specific target, such as for analytics or logging purposes.

---

### **When to Use Bubbling or Capturing?**
- **Bubbling** is more common since most interactions are handled at or after the target level.
- **Capturing** is useful when you need to intercept events before they reach their target.

---

### **Summary**
- **Event Capturing**: Top-down phase of event propagation.
- **Event Bubbling**: Bottom-up phase of event propagation.
- Together, they give developers flexibility in how they manage events across the DOM hierarchy. Understanding their behavior helps prevent unexpected interactions and bugs!
