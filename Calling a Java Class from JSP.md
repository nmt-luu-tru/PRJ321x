
### Introduction: Calling a Java Class from JSP
In this video, we'll explore how to call a Java class from a JSP page. Previously, we discussed minimizing the use of scriptlets and declarations within JSP pages to keep your code clean and maintainable. One effective way to do this is by refactoring your business logic into separate Java classes. This video will guide you through the process of creating a Java class and calling it from a JSP file.

### Why Refactor Java Code into Separate Classes?
**Best Practices**: Including large amounts of Java code directly in JSP files can lead to poor design and maintainability issues. Instead, you should:
- **Minimize Scriptlets and Declarations**: Use them sparingly for small tasks, but avoid filling your JSP pages with complex logic.
- **Refactor to Java Classes**: Move your business logic to separate Java classes. This allows you to reuse code and keep your JSP pages clean.
- **Utilize MVC Architecture**: For more complex applications, consider using the Model-View-Controller (MVC) pattern to separate business logic from the presentation layer.

### Step 1: Creating the Java Class
**To-Do List**:
1. **Create the Java Class**: We'll start by creating a new Java class that contains our business logic.
2. **Call the Java Class from the JSP Page**: Next, we'll make a call to this class from our JSP page.

**Implementation**:
1. **Creating a Package**:
   - Open your existing `jspdemo` project in Eclipse.
   - Navigate to `Java Resources > src`.
   - Right-click on `src` and select `New > Package`.
   - Name the package `com.luv2code.jsp` and click `Finish`.

2. **Creating the Java Class**:
   - Right-click on the newly created package `com.luv2code.jsp`.
   - Select `New > Class`.
   - Name the class `FunUtils` and click `Finish`.

**Example Code for FunUtils**:
```java
package com.luv2code.jsp;

public class FunUtils {
    public static String makeItLower(String data) {
        return data.toLowerCase();
    }
}
```

**Explanation**:
- **Class Creation**: The `FunUtils` class contains a static method `makeItLower`, which takes a string input and returns it in lowercase.
- **Refactoring**: This method was previously written directly in a JSP declaration. By moving it to a separate class, we make our code more modular and easier to maintain.

### Step 2: Calling the Java Class from JSP
**Implementation**:
1. **Create a New JSP File**:
   - Navigate to the `WebContent` directory in your project.
   - Right-click on `WebContent` and select `New > File`.
   - Name the file `fun-test.jsp` and click `Finish`.

2. **Write the HTML and JSP Code**:
   - Start by setting up the basic HTML structure.
   - Use a JSP expression to call the `makeItLower` method from the `FunUtils` class.

**Example Code for fun-test.jsp**:
```html
<%@ page import="com.luv2code.jsp.FunUtils" %>
<body>
    <h3>Let's have some fun:</h3>
    <%= FunUtils.makeItLower("FUN FUN FUN") %>
</body>
```

**Explanation**:
- **Import Statement**: The import statement `<%@ page import="com.luv2code.jsp.FunUtils" %>` allows us to use the `FunUtils` class in our JSP without needing to reference the full package name each time.
- **JSP Expression**: We use `<%= FunUtils.makeItLower("FUN FUN FUN") %>` to call the `makeItLower` method. The string "FUN FUN FUN" is passed to the method, and the lowercase result is included in the HTML output.

### Running the JSP File
**Steps**:
- Right-click on `fun-test.jsp` and select `Run As > Run on Server`.
- If prompted to restart the server (due to the new class addition), click `OK`.
- The output will display in your browser, showing the text "fun fun fun" in lowercase.

### Conclusion: Calling Java Classes from JSP
In this video, we successfully refactored Java code into a separate class and called it from a JSP page. This approach helps keep your JSP files clean and maintainable while allowing you to reuse code across multiple pages. In future videos, we'll explore more advanced techniques, so stay tuned!
