
### Introduction: Using JSP Built-In Server Objects
In this video, I'll show you how to make use of JSP built-in server objects. These objects are provided by the JSP environment, meaning you don't need to create them—they're available for you to use directly in your JSP pages. We'll explore some of the most commonly used server objects and demonstrate how to access and utilize them in your applications.

### What Are JSP Built-In Server Objects?
**Definition**: JSP built-in server objects are pre-defined objects that are automatically available in your JSP pages. These objects allow you to interact with the server, access client information, and manage user sessions without the need to manually create these objects.

**Commonly Used JSP Server Objects**:
1. **`request`**: Contains information about the HTTP request, including headers and form data. You'll often use this object to read data sent by the client, such as form inputs.
2. **`response`**: Used to send HTTP-specific information back to the client. This includes setting headers, managing cookies, and controlling the content sent to the client.
3. **`out`**: A `JspWriter` object used to send output to the client. You've already used this object in previous videos for methods like `out.println()`.
4. **`session`**: Represents a unique session for each user. It's commonly used to store user-specific data, similar to a shopping cart in an online store.
5. **`application`**: Shared across all users of a web application. It's used to store global data that can be accessed by all users.

**Note**: This is not an exhaustive list. There are other server objects available, but these are the most commonly used in JSP applications.

### How JSP Built-In Server Objects Work
**Request/Response Cycle**:
- **Request Object**: When a client (browser) makes an HTTP request to a JSP page, a `request` object is automatically created. This object contains information about the request, such as headers and body data.
- **Response Object**: After processing the request, the JSP page sends back a response using the `response` object. This response includes the content to be displayed in the client's browser.

### Example: Using the Request Object in JSP
**Scenario**: Let's create a simple JSP page that uses the `request` object to determine the type of browser and the language preference of the user.

**Steps**:
1. **Create a New JSP File**:
   - In Eclipse, open your existing project (`jspdemo`).
   - Navigate to the `WebContent` folder, right-click, and select `New > File`.
   - Name the file `builtin-test.jsp` and click `Finish`.

2. **Write the HTML and JSP Code**:
   - Set up the basic HTML structure and use the `request` object to access the client's browser and language information.

**Example Code**:
```html
<body>
    <h3>Browser and Language Information</h3>
    <p>Request User-Agent: <%= request.getHeader("User-Agent") %></p>
    <p>Request Language: <%= request.getLocale() %></p>
</body>
```

**Explanation**:
- **`request.getHeader("User-Agent")`**: Retrieves the `User-Agent` header from the request, which contains information about the client's browser and operating system.
- **`request.getLocale()`**: Retrieves the locale setting from the request, indicating the language preference set in the client's browser.

### Running the JSP File
**Steps**:
- Right-click on `builtin-test.jsp`, select `Run As > Run on Server`.
- The application will display the client's browser and operating system information, as well as the language preference.

**Output**:
- The output will show details about the browser (e.g., Chrome, Firefox) and operating system (e.g., macOS, Windows) the client is using. It will also display the preferred language (e.g., English, Spanish).

**Testing in Different Browsers**:
- You can copy the URL of the JSP page and paste it into different browsers to see how the `User-Agent` and language information changes based on the browser and system you're using.

### Conclusion: Using JSP Built-In Server Objects
In this video, we explored how to use JSP built-in server objects, specifically the `request` object, to gather information about the client's browser and language preference. These objects are essential tools for developing dynamic web applications. In the next videos, we'll dive deeper into other server objects and demonstrate their use in real-world scenarios.
