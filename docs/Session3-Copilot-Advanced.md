# Session 3: Copilot Advanced

## Overview

In the previous sections you discovered how to use all out-of-the-box features from GitHub Copilot. In this section you will learn techniques to get more accurate results, providing Copilot what he doesn't know: your best practices, naming conventions, data model, internal APIs, etc...

We will use advanced reusable prompts and customization capabilities to make Copilot act like a real co-worker and not just the Copilot everyone is using.

## Prompt Engineering Techniques

### Provide Examples: One-shot and Few-shots Programming

Talking about prompt engineering, you can also use the chat to provide examples to Copilot. It's a good way to help Copilot understand what you want to do and generate better code. You can provide examples in the chat by typing with the `validator.ts` file open:

#### One-shot Programming

```
Write me unit tests for phone number validators methods using mocha and chai in the current file.
Use the following examples for positive test (test that should return true): 
it('should return true if the phone number is a valid international number', () => { expect(validatePhoneNumber('+33606060606')).to.be.true; });
Organize test in logic suites and generate at least 4 positives tests and 2 negatives tests for each method.
```

#### Few-shot Programming

```
Write me unit tests for all validators methods using mocha and chai in the current file.
Use the following examples for positive test (test that should return true): 
it('should return true if the phone number is a valid international number', () => { expect(validatePhoneNumber('+33606060606')).to.be.true; });
it('should return true if the phone number is a valid local american number', () => { expect(validatePhoneNumber('202-939-9889')).to.be.true; });
it('should throw an error if the given phone number is empty', () => { expect(validatePhoneNumber('')).to.throw(); });
Organize test in logic suites and generate at least 4 positives tests and 2 negatives tests for each method.
```

You can use this technique to generate code that keeps the styling code from another file. For example if you want to create sample records for music style like the Albums in `albums-api > Models > Album.cs` file, open it and type:

```
Write a MusicStyle record that contains a List<MusicStyle> with 6 sample values like in the Album.cs file.
```

## Role Prompting

Also called foundational prompt, it's a general prompt you're giving to Copilot Chat to personnalize his behavior and setup your flavour of Copilot.

This is probably the first thing to do when you start a new task with Copilot Chat: provide a clear description of what you want to build and how do you want Copilot to help you.

This is very powerfull when handled properly so be sure to start every coding sessions with a role prompt and save your best prompt for future use.

### Structure of a Role Prompt

What can you include in a role prompt:

- Provide solid context and background information on what you want to build.
- Define GitHub Copilot's role and setting expectations about what feedback we are looking for.
- Be specific in the quality of answers and ask for reference and additional resources to learn more and ensure the answers you receive are correct
- Resume the task and ask if the instructions are clear

### Example of a Role Prompt

Start a new conversation and type the following prompt:

```
I'm working on a new mobile application that is built on React Native. 
I need to build a new feature that will allow the user to upload a picture of a dog and get the breed of the dog. 
I will need to use the following set of APIs to work on the breeds: https://dog.ceo/api/breeds. I need to be sure that my code is secured against at least the OWASP Top 10 treats (https://owasp.org/Top10/). 
I need to have unit tests for the code using Jest framework.
I need you to act as my own code coach to ensure that my code fits all these requirements. 
When possible, please provide links and references for additional learning. 
Do you understand these instructions?
```

From there you can start asking questions and from time to time, ensure Copilot still follows the instructions by asking:

```
Are you still using the instructions I provided?
```

### Test Your Role Prompt

You can test your role prompt by asking questions about best practices for accessibility on React Native Apps and OWASP Top 10 treats. You can also ask to generate code for the upload feature and check if the generated code is secured and accessible.

Try these questions for example:

```
How can I make my app accessible with react native?

What is the most secure way to upload a photo from my app?
```

## Custom Instructions

*This feature is available only on VS Code, Visual Studio and the GitHub Website for the moment*

This feature is easing the customization of Copilot by providing an instruction file that will be:

- used as meta instructions for all you chat/edit requests
- stored in the repo as code which means it will be automatically shared among team members

It very powerful to add context for Copilot specifically dedicated for the current codebase.

### The copilot-instructions.md File

Start using it by simply creating a `.github/copilot-instructions.md`. Start simple by adding these simple instructions and make a few requests to Copilot chat to see the impact:

```markdown
Please answer in french but provide code in English.
We code in TypeScript and use Jest for testing our code.
When possible, please provide links and references for additional learning.
```

This is a very basic example. By the way you can provide more advanced information on your project to improve responses of Copilot. Here is a few examples:

**Example 1:**

```markdown
The backend code is using NestJS in TypeScript, Prisma as our ORM, and PostgreSQL as our database.
The frontend code is using VueJS in TypeScript with Vue Router and Vuex for state management.
We use Docker for containerization and deploy on Azure.
We use GitHub Actions for CI/CD.
```

**Example 2:**

```markdown
This is our SQL database schema for Music Albums management:

```sql
CREATE TABLE artists (
    id SERIAL PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    genre VARCHAR(100),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE albums (
    id SERIAL PRIMARY KEY,
    title VARCHAR(255) NOT NULL,
    artist VARCHAR(255) NOT NULL,
    price DECIMAL(10, 2) NOT NULL,
    image_url VARCHAR(2083),
    release_date DATE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

**Example for .NET 8 Projects:**

```markdown
This is a .NET 8 albums management API using:
- ASP.NET Core with minimal APIs and Controllers
- DAPR for state management (statestore)
- Swagger/OpenAPI for documentation
- C# 12 language features

When generating code:
- Always use async/await patterns for I/O operations
- Use proper error handling with try-catch blocks
- Follow RESTful API conventions
- Include XML documentation comments for public methods
- Use FluentAssertions for unit tests with xUnit framework

For unit tests, always include:
- Arrange/Act/Assert pattern
- Test both success and error scenarios  
- Use descriptive test method names
- Mock external dependencies
```

To test if custom instructions are working, look for references to the `.github/copilot-instructions.md` file in Copilot's responses and check if the generated code follows your specified patterns.

## Build Your Prompts Library

### Reusable Prompts

We saw how to define custom-instructions that will seamlessly apply when using GitHub Copilot. In your developer life you will also have some task for which you want to be able to have an easy-to-call, very effective set of dedicated prompts.

This is where reusable prompts come into play.

You can define a set of prompts, with the `.prompt.md` suffix in the file name, save it in the `.github/prompts` folder, and you'll be able to reference them in your Copilot Chat.

### Create Your First Prompt File

Create a new file in `.github/prompts` folder with the name `plan-album-feature.prompt.md` and add the following content:

```markdown
---
mode: 'agent'
tools: ['codebase', 'fetch', 'findTestFiles', 'githubRepo', 'search', 'usages']
description: 'Generate implementation plan for .NET 8 album-api features'
---

You are a senior .NET 8 architect working on the albums-api project. Analyze the current codebase and generate a detailed implementation plan for the requested feature.

Focus on:
- .NET 8 minimal API patterns and best practices
- ASP.NET Core dependency injection and configuration
- DAPR integration patterns used in this project
- Controller-based architecture with proper separation of concerns
- Swagger/OpenAPI documentation requirements
- CORS configuration and security considerations

The plan should include:
1. **Overview**: Brief description of the feature and its purpose
2. **Architecture Impact**: How this affects existing Program.cs, controllers, and models
3. **Implementation Steps**: 
   - Required NuGet packages
   - Configuration changes in Program.cs
   - New controllers or endpoints needed
   - Model classes and DTOs
   - Service layer modifications
4. **DAPR Integration**: If applicable, how to integrate with the existing DAPR state store
5. **Testing Strategy**: Unit tests, integration tests, and API testing approach
6. **Security Considerations**: Authentication, authorization, and CORS implications

Provide concrete code examples using C# 12 features and .NET 8 APIs where appropriate.
```

### How to Use Prompt Files

The syntax for using prompt files varies depending on your IDE:

#### In Visual Studio:
Use the `#prompt:` reference syntax:
```
#prompt:plan-album-feature Add user authentication with JWT tokens to the albums-api
```

Or use the context menu:
1. Click the **? (plus) icon** in your Copilot Chat input box
2. Look for your prompt files in the context menu
3. Select the prompt file you want to use
4. Add your specific request

#### In VS Code:
Use slash commands (if supported):
```
/plan-album-feature Add user authentication with JWT tokens to the albums-api
```

### Create a Code Review Prompt

Add another file `.github/prompts/review-dotnet-code.prompt.md`:

```markdown
---
mode: 'ask'
tools: ['codebase', 'search', 'usages']
description: 'Review .NET 8 code for best practices and security'
---

You are a senior .NET 8 code reviewer. Analyze the provided code for:

**Architecture & Design:**
- Proper use of .NET 8 minimal API features
- ASP.NET Core dependency injection patterns
- SOLID principles compliance
- Clean architecture patterns

**Security:**
- CORS configuration security
- Input validation and sanitization
- Authentication/Authorization best practices
- Secure configuration management

**Performance:**
- Async/await patterns
- Memory usage optimization
- HTTP client usage patterns
- Database query efficiency

**Code Quality:**
- C# 12 language feature usage
- Nullable reference types
- Error handling patterns
- Logging and monitoring

**DAPR Integration:**
- State store usage patterns
- Service invocation best practices
- Configuration and secrets management

Provide specific recommendations with code examples showing the improved version.
```

### Testing Your Prompts

Try these commands in your Copilot Chat:

**For feature planning:**
```
#prompt:plan-album-feature Add search functionality to find albums by artist name and genre
```

**For code review:**
```
#prompt:review-dotnet-code
```
Then follow up by referencing specific code or asking it to review your current Program.cs file.

### What Makes Effective Prompt Files

- **Clear persona definition** (e.g., "senior .NET 8 architect")
- **Specific context** about your project and tech stack
- **Structured output format** with clear sections
- **Relevant tools** specified in the metadata
- **Concrete examples** of what you want to see

Checkout https://github.com/github/awesome-copilot/blob/main/prompts/ for amazing examples of reusable prompts to start with.

## When to Use Prompts vs Instructions

Understanding when to use **prompt files** versus **custom instructions** is crucial for effective Copilot customization. Each serves different purposes in your development workflow.

### Use Custom Instructions When:

**? You want automatic, persistent behavior changes**
- Setting coding standards that apply to ALL conversations
- Defining your tech stack once (e.g., ".NET 8, ASP.NET Core, DAPR")
- Establishing team conventions (naming, patterns, testing frameworks)
- Providing context about your project architecture

**Example scenarios:**
```markdown
# .github/copilot-instructions.md
We use .NET 8 with ASP.NET Core minimal APIs.
Our project uses DAPR for state management and service invocation.
Always follow async/await patterns and include proper error handling.
Use C# 12 features like primary constructors and required properties.
```

**? Instructions work behind the scenes**
- Applied automatically to every Copilot request
- No need to remember to use them
- Team members get consistent behavior automatically
- Ideal for project-wide conventions

### Use Prompt Files When:

**? You want specific, on-demand functionality**
- Creating detailed implementation plans
- Performing code reviews with specific criteria  
- Generating specific types of code (controllers, models, tests)
- Executing complex, multi-step tasks

**Example scenarios:**
```
#prompt:plan-album-feature Add user authentication
#prompt:review-dotnet-code 
#prompt:generate-controller Create a new ProductController
```

**? Prompts are task-specific tools**
- Called explicitly when needed
- Can be shared across projects
- Perfect for repetitive, structured tasks
- Allow for specialized personas and contexts

### Practical Decision Framework:

| Scenario | Use Instructions | Use Prompts |
|----------|-----------------|-------------|
| "Always use async/await" | ? Instructions | ? |
| "Generate an implementation plan" | ? | ? Prompts |
| "We use Jest for testing" | ? Instructions | ? |
| "Review this code for security" | ? | ? Prompts |
| "Our API uses Controllers pattern" | ? Instructions | ? |
| "Create unit tests with specific format" | ? | ? Prompts |

### Best Practice: Use Both Together

The most effective approach combines both:

**Instructions for your project context:**
```markdown
# .github/copilot-instructions.md
This is a .NET 8 albums management API using:
- ASP.NET Core with minimal APIs and Controllers
- DAPR for state management (statestore)
- Swagger/OpenAPI for documentation
- C# 12 language features

When generating code:
- Always use async/await patterns for I/O operations
- Use proper error handling with try-catch blocks
- Follow RESTful API conventions
- Include XML documentation comments for public methods
- Use FluentAssertions for unit tests with xUnit framework

For unit tests, always include:
- Arrange/Act/Assert pattern
- Test both success and error scenarios  
- Use descriptive test method names
- Mock external dependencies
```

**Prompts for specific tasks:**
```markdown
# .github/prompts/create-endpoint.prompt.md
Create a new API endpoint following our albums-api patterns...
```

This way, every Copilot interaction understands your project context (instructions), and you can call specialized tools (prompts) when needed for specific tasks.

### Migration Strategy:

1. **Start with Instructions** - Define your basic project context
2. **Identify repetitive tasks** - What do you ask Copilot to do repeatedly?
3. **Create Prompts** - Turn those repetitive tasks into reusable prompts
4. **Refine over time** - Adjust both based on your team's workflow

## Key Takeaways

1. **Prompt Engineering**: Use one-shot and few-shot examples to guide Copilot's output
2. **Role Prompting**: Set context and expectations at the beginning of coding sessions
3. **Custom Instructions**: Create repository-specific guidance that applies automatically
4. **Specialized Instructions**: Use file-specific instructions for different technologies
5. **Reusable Prompts**: Build a library of reference commands for common tasks

## Next Steps

- Create your first `copilot-instructions.md` file for your project
- Experiment with role prompting for different development tasks
- Build specialized instruction files for different parts of your codebase
- Develop reusable prompts for your most common development tasks
- Test prompt files with the `#prompt:` syntax in Visual Studio