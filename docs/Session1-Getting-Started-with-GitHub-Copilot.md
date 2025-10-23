# Session 1: Code Completion with GitHub Copilot

## Overview

This section will guide you through the first steps with GitHub Copilot, starting with code completion. You will learn what you can do and how to use it at its full potential.

## Let's Start with the Basics

### Write Code

**What is a prompt?** In the context of Copilot, a prompt is a piece of natural language description that is used to generate code suggestions. It can be a single line or multiple lines description.

#### Generate Code from Prompt

Create a new `album-viewer/src/utils/validators.ts` file and try the different following prompts to see the different suggestions:

```typescript
// validate date from text input in french format and convert it to a date object
```

Copilot can help you also to write RegExp patterns. Try these:

```typescript
// function that validates the format of a GUID string

// function that validates the format of a IPV6 address string
```

#### More Complex Code Generation

In the `albums-api/Controllers/AlbumController.cs` file try to complete the `Get(int id)` method by replacing the current return:

```csharp
// GET api/<AlbumController>/5
[HttpGet("{id}")]
public IActionResult Get(int id)
{
    // here
}
```

In the same file you can show other prompts but use other copilot calling methods like inline. From the the editor, press `ALT+/` to open inline chat:

```csharp
// function that sort albums by name, artist or genre
```

### Next Edit Suggestion

Next edit suggestion is an evolution of the standard completion in GitHub Copilot. When you are modifying code and accepting a code suggestion, it can have an impact on other parts of your code, and it will automatically suggest the next change in your code, not only directly where your cursor is but where your natural next action will probably be.

**Setup:**
1. Select the **Preferences: Open Tools->Options->Github->Copilot 
2. Search for the **Next Edit Suggestions** settings and enable it

**Exercise:**
Open the `albums-api/Models/Album.cs` file and, on the Album constructor, add a new input parameter `Year` of type `int` and see the Next Edit Suggestion propose to change the body of the method accordingly.

## Everyday Developer's Tasks

### Write Tests

Copilot can also help you generate tests for your code. It can generate unit tests, integration tests, end to end tests, and load testing tests with JMeter scripts for example.

Open the `album-api/Controllers/UnsecuredController.cs` file and type questions like these to the chat. Why not trying inline with `Ctrl + i` first and again with the chat view after to see the difference?

> Generate a unit tests class for this code

You can also use Copilot to help you generate Stubs and Mocks for your tests.

> Generate a mock for FileStream class
> Use that mock in the unit tests

Remember that Copilot chat is keeping track of the previous Q & A in the conversation, that's why you can reference the previously generated mock and test easily.

### Write CI Workflows

Copilot will help you in writing your pipeline definition files to generate the code for the different steps and tasks. Here are some examples of what it can do:

- Generate a pipeline definition file from scratch
- Accelerate the writing of a pipeline definition file by generating the code for the different steps, tasks and pieces of script
- Help discover marketplace tasks and extensions that match your need

#### Step 1: Generate from Scratch

Create a new file `workflow.yml` in the `.github/workflows` folder of the project and start typing the following prompt:

```yaml
# GitHub Action workflow that runs on push to main branch
# Docker build and push the album-api image to ACR
```

Copilot will generate the pipeline block by block. When generating pipeline YAML, you may need to press Enter to move to a new line to prompt the generation of the next block and Tab to validate it.

> **Note:** It will often generate a task with a few errors coming from bad indentation or missing quotes around a task name. You can easily fix these with your IDE and your developer skills :)

#### Step 2: Add Tasks from Prompts

You probably have a GitHub Action workflow with at least a "login" task to your container registry and a "docker build and deploy" task. Add a new comment after those tasks to tag the docker image with the GitHub run id and push it to the registry:

```yaml
# tag the image with the GitHub run id and push to docker hub
```

You can play with other prompts like:

```yaml
# run tests on the album-api image

# deploy the album-api image to the dev AKS cluster
```

#### Step 3: Add Scripts from Prompts

Copilot is also very useful when you need to write custom scripts like the following example:

```yaml
# find and replace the %%VERSION%% by the GitHub action run id in every appmanifest.yml file
```

### Infrastructure As Code

Copilot can also help you write Infrastructure as Code. It can generate code for Terraform, ARM, Bicep, Pulumi, etc... and also Kubernetes manifest files.

#### Bicep

Open the `main.bicep` file in `iac/bicep` folder and start typing prompts at the end of the file to add new resources:

```bicep
// Container Registry

// Azure Open AI resource
```

#### Terraform

Open the `app.tf` file in `iac/terraform` folder and start typing prompts at the end of the file to add new resources:

```terraform
# Container Registry

# Azure Open AI resource
```

## Big Tasks vs Small Tasks

Copilot will probably always be more effective with prompts to generate small but precisely described pieces of code rather than a whole class with a unique multiple lines prompt.

Copilot completion is even more effective when you use it to generate small pieces of code step by step. For more complex tasks, we will see in the next section that Copilot Chat is more powerful.

This is because completion must be almost "instant" to be natural to use where a chat request can take a few seconds to be processed without being annoying.

**The best strategy** to generate big pieces of code is always starting by the basic structure of your code with a simple prompt and then add small pieces one by one.

### Big Prompts That Could Work

Back in the `album-viewer/src/utils` add a new file `viz.ts` to create a function that generates a graph. Here is a sample prompt to do that:

```typescript
// generate a plot with D3.js of the selling price of the album by year
// x-axis are the month series and y-axis show the numbers of albums sold
// data from the sales of album are loaded from an external source and are in json format
```

Copilot will probably try to complete the prompt by adding more details. You can try to add more details yourself or follow Copilot's suggestions. When you want it to stop and start generating the code just jump to another line and let Copilot do its work.

Once you achieve generating the code for the chart you probably see that your IDE warns you about the d3 object that is unknown. For that also Copilot helps. Return to the top of the file and start typing `import d3` to let Copilot autocomplete:

```typescript
import d3 from "d3";
```

### Try Again by Building It Step by Step

Try to generate the code for the plot by cutting it into small pieces following the steps below:

```typescript
import * as d3 from 'd3';

// load the data from a json file and create the d3 svg in the then function
```

Inside the then function, start by setting up the basics of the plot:

```typescript
// create the svg

// create the scales for the x and y axis
// x-axis are the month series and y-axis show the numbers of albums sold

// create axes for the x and y axis
```

From there you can just ask Copilot to complete the chart:

```typescript
// generate a line chart based on the albums sales data
```

You will always get better results by cutting big tasks into small chunks with Copilot autocompletion.

## Side Quest #1: Generate Git Commit Comment

Yes, writing a commit message should be mandatory and developers tend to be lazy. GitHub Copilot can help with that.

1. Just edit any file by adding some relevant content into it
2. On the Git commit panel, click the small magical button on the right
3. Look at the git commit message Copilot has generated for you

## Side Quest #2: Writing Documentation

Copilot can understand natural language prompts and generate code, and because it's just language to it, it can also understand code and explain it in natural language to help you document your code.

### Simple Documentation Comment

To see that, just put your cursor on top of a Class, a method or any line of code and start typing the comment handler for the selected language to trigger Copilot. In languages like Java, C# or TS for example, just type `//` and let the magic happen.

Here is an example in the `album-viewer/routes/index.js` file. Insert a line and start typing on line 13 inside the try block:

```javascript
router.get("/", async function (req, res, next) {
  try {
    // Invoke the album-api via Dapr
    const url = `http://127.0.0.1:${DaprHttpPort}/v1.0/invoke/${AlbumService}/method/albums`;
```

### Standardized Documentation Comment (JavaDoc, JsDoc, etc...)

For this one, to trigger the documentation comment generation, you need to respect the specific comment format:

- `/**` (for JS/TS) in the index.js file for example
- `///` for C# in the AlbumController.cs of the albums-api file for example

```csharp
/// <summary>
/// AlbumController is responsible for handling HTTP requests related to albums.
/// It provides endpoints to retrieve all albums, a specific album by ID, and sort albums by name, artist, or genre.
/// </summary>
public class AlbumController : ControllerBase
```

### Writing Markdown and HTML Documentation

Copilot is also very powerful to help you write documentation. It can generate markdown and HTML code and accelerate the writing of your README.md files for example.

**Note:** Completion is deactivated by default on markdown and text files. You can activate it by clicking on the Copilot icon in the bottom right corner of your IDE and select **Enable for Markdown** or **Enable for Plain Text**.

You can show that by asking it to create a new file `DOCS.md` in the root of the project and ask it to include readme information (tip sometimes using the @workspace initiator produces better results)



**Remember**: Copilot is most effective when you provide clear context and break down complex tasks into smaller, manageable pieces. Always review and test the generated code!