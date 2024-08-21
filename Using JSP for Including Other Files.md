
### Introduction: Using JSP for Including Other Files
In this video, I'll show you how to use JSP for including other files in your web pages. This technique is particularly useful for including standard headers and footers across multiple pages on your website, ensuring consistency and reducing the need to duplicate code.

### Why Include Files in JSP?
**Common Use Case**: When building a website, you often want the same header and footer to appear on every page. Instead of copying and pasting the same code across multiple files, you can create separate files for your header and footer and include them in your JSP files. This approach simplifies maintenance and ensures consistency across your site.

**Types of Files**: You can include either HTML files or JSP files using the `jsp:include` directive. This gives you flexibility depending on whether the content you're including is static (HTML) or dynamic (JSP).

### Example: Including Header and Footer Files
**Basic Syntax**:
```jsp
<jsp:include page="my-header.html" />
<jsp:include page="my-footer.jsp" />
```

**Explanation**:
- **`<jsp:include>` Directive**: This directive is used to include the content of another file (HTML or JSP) into your JSP page. The `page` attribute specifies the file to include.

### Step 1: Creating the Header File
**Implementation**:
1. **Create a Header File**:
   - Open your existing project (`jspdemo`) in Eclipse.
   - Navigate to the `WebContent` folder, right-click, and select `New > File`.
   - Name the file `my-header.html` and click `Finish`.

2. **Write the HTML Code for the Header**:
   - Add a simple header with the title "JSP Tutorial" centered on the page.

**Example Code for my-header.html**:
```html
<h1 style="text-align: center;">JSP Tutorial</h1>
```

**Explanation**:
- **Header Content**: This header will be included at the top of each page on your website, displaying "JSP Tutorial" in a large, centered font.

### Step 2: Creating the Footer File
**Implementation**:
1. **Create a Footer File**:
   - In the `WebContent` folder, right-click and select `New > File`.
   - Name the file `my-footer.jsp` and click `Finish`.

2. **Write the JSP Code for the Footer**:
   - Add code to display the current date and time, indicating when the page was last modified.

**Example Code for my-footer.jsp**:
```jsp
<p>Last updated: <%= new java.util.Date() %></p>
```

**Explanation**:
- **Dynamic Footer Content**: This JSP code generates a timestamp showing the last time the page was accessed, making your site appear fresh and up-to-date.

### Step 3: Creating the Main Page
**Implementation**:
1. **Create the Main JSP File**:
   - In the `WebContent` folder, right-click and select `New > File`.
   - Name the file `homepage.jsp` and click `Finish`.

2. **Include the Header and Footer**:
   - Use the `jsp:include` directive to include the `my-header.html` at the top and the `my-footer.jsp` at the bottom of the page. Add some placeholder content in between.

**Example Code for homepage.jsp**:
```jsp
<body>
    <jsp:include page="my-header.html" />
    <p>Welcome to our homepage!</p>
    <p>Here is some content for the main page...</p>
    <jsp:include page="my-footer.jsp" />
</body>
```

**Explanation**:
- **Including the Header and Footer**: The `jsp:include` directive pulls in the content from `my-header.html` and `my-footer.jsp` into the `homepage.jsp` file. This ensures that the header and footer are consistent across all pages that include these files.
- **Main Content**: You can place any page-specific content between the included header and footer.

### Running the JSP File
**Steps**:
- Right-click on `homepage.jsp`, select `Run As > Run on Server`.
- The application will display the header, the main content, and the footer with the current timestamp.

**Output**:
- The header "JSP Tutorial" will appear at the top, followed by your main content, and then the footer showing the last modified date and time.

### Conclusion: Using JSP for File Inclusion
In this video, we demonstrated how to include external header and footer files in your JSP pages using the `jsp:include` directive. This method simplifies your website's structure, making it easier to maintain consistency across multiple pages. In future videos, we'll explore more advanced JSP features, so stay tuned!
