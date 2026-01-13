### Please create a Python program to detect code smells.
Your goal is to create a python function that will detect any code smells present. The program will reccieve a python string containing java code. The function could expect strings that contain an entire class or just a standalone method and must be robust enough to handle detection in either case. This program should follow the same logic you would follow if given a similar task. Any methods or classed made must end with a suffix of your name at the end.


### **Code Smells**
It may contain one, more than one, or none of the following code smells as defined: 

---

### **1. Complex Method - Implementation Smell**  
This smell occurs when a method has high cyclomatic complexity.

---

### **Long Parameter List - Implementation Smell**  
This smell occurs when a method accepts a long list of parameters

---

### **Multifaceted Abstraction - Design Smell**  
This smell arises when an abstraction has more than one responsibility assigned to it.

---

### **Instructions:**  
2. Mark a file as smelly if any methods or classes within are smelly , **one entry per file**
3. When evaluating classes, if any method is smelly, the entire file is deemed smelly. A single file can contain more than one unique smell. Add all unique smells to the category field 
4. **Do NOT include line numbers** in the output.  
5. The output should be added to a **pandas dataframe** with the following columns: 
   - **Category** (None, Complex Method, Long Parameter List, Multifaceted Abstraction; if a file has more than one, add all in that field) 
   - **Project** (Name of the project)
   - **Package** (Name of the package folder(s))
   - **Type** (If the file is a class or a standalone method) 
   - **Name**  (Name of the file)
6. One you finish analyzing a project, export the dataframe to a csv
---

Examples are not literal and just a representation of table structure
### **Example Pandas Table Output:**  
```markdown
| Category                 | Project   | Package                                | Type   | Name          |
| Multifaceted Abstraction | Example 1 | com.example1.example                   | Method | example1.Java |
| Long Parameter List      | Example 2 | com.example2.example                   | Class  | example2.Java |
| Complex Method           | Example 3 | com.example3.example/SomeClassExample  | Class  | example2.Java |
| None                     | Example 4 | com.example4.example/SomeHelpfulMethod | Method | example2.Java |