# Greenfield 🌳 🌳 🌳

Guidelines for new projects at Ordermentum

- [Git](#git)
- [Documentation](#documentation)
- [Environments](#environments)
- [Dependencies](#dependencies)
- [Testing](#testing)
- [Structure and Naming](#structure-and-naming)
- [Code style](#code-style)
- [Logging](#logging)
- [API Design](#api-design)
- [Licensing](#licensing)

----
## 1. Git <a name="git"></a>

### 1.1 Git Workflow
We use [Feature-branch-workflow](https://www.atlassian.com/git/tutorials/comparing-workflows#feature-branch-workflow) with [Rebasing](https://www.atlassian.com/git/tutorials/merging-vs-rebasing#the-golden-rule-of-rebasing) and some elements of [Gitflow](https://www.atlassian.com/git/tutorials/comparing-workflows#gitflow-workflow) (naming and having a develop branch). The main steps are as follow:

* Checkout a new feature/bug-fix branch
    ```sh
    git checkout -b <branchname>
    ```
* Make Changes
    ```sh
    git add
    git commit -m "<description of changes>"
    ```
* Sync with remote to get changes you’ve missed
    ```sh
    git fetch
    ```
* Update your feature branch with latest changes from develop by rebasing ([More info](https://www.atlassian.com/git/tutorials/merging-vs-rebasing#the-golden-rule-of-rebasing))
    ```sh
    git rebase origin/develop
    ```
* If you don’t meet conflicts skip this step. If you have conflicts, resolve them either using your code editor, or the [CLI](https://help.github.com/articles/resolving-a-merge-conflict-using-the-command-line/)  , and then continue rebasing
    ```sh
    # resolve conflicts
    git add <file1> <file2> ...
    # NB. do NOT `git commmit`
    git rebase --continue
    ```
* Push your branch. Rebase will change history, so you'll have to use `-f` to force changes into the remote branch. If someone else is working on your branch, use the less destructive `--force-with-lease` ([Here is why](https://developer.atlassian.com/blog/2015/04/force-with-lease/)).
    ```sh
    git push -f <branchname>
    ```
* Make a Pull Request.
* Pull request will be accepted, merged, and closed by reviewer.
* Remove your local feature branch if you're done.



### 1.2 Some Git Rules
There are a set of rules to keep in mind:
* Perform work in a feature branch.
* Make pull requests to `develop`
* Never push into `develop` or `master` branch.
* Update your `develop` and do a  rebase before pushing your feature and making a Pull Request
* Resolve potential conflicts while rebasing and before making a Pull Request
* Delete local and remote feature branches after merging.
* Before making a Pull Request, make sure your feature branch builds successfully and passes all tests (including code style checks).
* Use [this .gitignore file](./.gitignore).
* Protect your `develop` and `master` branch (How to in [Github](https://help.github.com/articles/about-protected-branches/) and [Bitbucket](https://confluence.atlassian.com/bitbucketserver/using-branch-permissions-776639807.html)).

### 1.3 Writing good commit messages

Having a good guideline for creating commits and sticking to it makes working with Git and collaborating with others a lot easier. Here are some rules of thumb ([source](https://chris.beams.io/posts/git-commit/#seven-rules)):

 * Separate the subject from the body with a blank line
 * Limit the subject line to 50 characters
 * Capitalize the subject line
 * Do not end the subject line with a period
 * Use [imperative mood](https://en.wikipedia.org/wiki/Imperative_mood) in the subject line
 * Wrap the body at 72 characters
 * Use the body to explain **what** and **why** as opposed to **how**

----

## 2. Documentation <a name="documentation"></a>
* Use this [template](./README.sample.md) for `README.md`, Feel free to add uncovered sections.
* For projects with more than one repository, provide links to them in their respective `README.md` files.
* Keep `README.md` updated as project evolves.
* Comment your code. Try to make it as clear as possible what you are intending with each major section.
* If there is an open discussion on github or stackoverflow about the code or approach you're using, include the link in your comment,
* Don't use commenting as an excuse for a bad code. Keep your code clean.
* Don't use clean code as an excuse to not comment at all.
* Comment even small sections of code if you think it's not self explanatory.
* Keep comments relevant as your code evolves.

----

## 3. Environments <a name="environments"></a>
* We always define separate `development`, `test` and `production` environments.
* Load your deployment specific configurations from environment variables and never add them to the codebase as constants.
* Your config should be correctly separated from the app internals as if the codebase could be made public at any moment.
* Always validate environment variables before your app starts.
* For an example of how we handle environment variables, look here [https://github.com/ordermentum/microservice-boilerplate/config/index.js](https://github.com/ordermentum/microservice-boilerplate/config/index.js)

### 3.1 Consistent dev environments:
* Use Docker images and docker-compose files to define your development environment. This way everyone on the project is consistent.

----

## 4. Dependencies <a name="dependencies"></a>
Before using a package, check its npm page and GitHub page. Look for the number of open issues, daily downloads and number of contributors as well as the date the package was last updated.

* Check to see if the dependency has a good, mature version release frequency with a large number of maintainers.
* If less known dependency is needed, discuss it with the team before using it.
* Pay attention to Greenkeeper to see if any of your deps have been updated, have security issues, etc.

### 4.1 Consistent dependencies:
* Use yarn.

----

## 5. Testing <a name="testing"></a>
* Put your test files in a `test` folder with a structure that mirrors the `src` folder eg: [https://github.com/ordermentum/microservice-boilerplate/test](https://github.com/ordermentum/microservice-boilerplate/test)
* Write testable code, avoid side effects, extract side effects, write pure functions
* Run tests locally before making any pull requests to `develop`.
* Document your tests, with instructions.

----

## 6. Structure and Naming <a name="structure-and-naming"></a>
* Organize your files around product features / pages / components, not roles

**Bad**

```
    .
    ├── controllers
    |   ├── product.js
    |   └── user.js
    ├── models
    |   ├── product.js
    |   └── user.js
```

**Good**

```
    .
    ├── product
    |   ├── index.js
    |   ├── product.js
    |   └── product.test.js
    ├── user
    |   ├── index.js
    |   ├── user.js
    |   └── user.test.js
```
* Place your test files next to their implementation.
* Put your additional test files to a separate test folder to avoid confusion.
* Use a `./config` folder. Values to be used in config files are provided by environment variables.
* Put your scripts in a `./scripts` folder. This includes `bash` and `node` scripts for database synchronisation, build and bundling and so on.
* Place your build output in a `./build` folder. Add `build/` to `.gitignore`.
* Use `PascalCase' 'camelCase` for filenames and directory names. Use  `PascalCase`  only for Components.
* `CheckBox/index.js` should have the `CheckBox` component, as could `CheckBox.js`, but **not** `CheckBox/CheckBox.js` or `checkbox/CheckBox.js` which are redundant.
* Ideally the directory name should match the name of the default export of `index.js`.

----

## 7. Code style <a name="code-style"></a>
* Use stage-1 and higher JavaScript (modern) syntax for new projects. For old project stay consistent with existing syntax unless you intend to modernise the project.
* Include code style check before build process.
* Use [ESLint - Pluggable JavaScript linter](http://eslint.org/) to enforce code style.
* We use [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript) for JavaScript, [Read more](https://www.gitbook.com/book/duk/airbnb-javascript-guidelines/details). Use the javascript style guide required by the project or your team.
* We use [Flow type style check rules for ESLint.](https://github.com/gajus/eslint-plugin-flowtype) when using [FlowType](https://flow.org/).
* Use `.eslintignore` to exclude file or folders from code style check.
* Remove any of your `eslint` disable comments before making a Pull Request.
* Always use  `//TODO:`  comments to remind yourself and others about an unfinished job.
* Always comment and keep them relevant as code changes.
* Remove commented block of code when possible.
* Avoid js alerts in production.
* Avoid irrelevant or funny comments, logs or naming (source code may get handed over to another company/client and they may not share the same banter).
* Write testable code, avoid side effect, extract side effects, write pure functions.
* Make your names search-able with meaningful distinctions avoid shortened names. For functions Use long, descriptive names. A function name should be a verb or a verb phrase, and it needs to communicate its intention.
* Organize your functions in a file according to the step-down rule. Higher level functions should be on top and lower levels below. It makes it more natural to read the source code.

----

## 8. Logging <a name="logging"></a>
* Avoid client-side console logs in production
* Produce readable production logging. Ideally use logging libraries to be used in production mode (such as [winston](https://github.com/winstonjs/winston) or
[node-bunyan](https://github.com/trentm/node-bunyan)).

----

## 9 API design <a name="api-design"></a>
Follow resource-oriented design. This has three main factors: resources, collection, and URLs.
* A resource has data, relationships to other resources, and methods that operate against it
* A group of resources is called a collection.
* URL identifies the online location of a resource.


### 9.1 API Naming

#### 9.1.1 Naming URLs
* `/users` a collection of users (plural nouns).
* `/users/id` a resource with information about a specific user.
* A resource always should be plural in the URL. Keep verbs out of your resource URLs.
* Use verbs for non-resources. In this case, your API doesn't return any resources. Instead, you execute an operation and return the result to the client. Hence, you should use verbs instead of nouns in your URL to distinguish clearly the non-resource responses from the resource-related responses.

GET 	`/translate?text=Hallo`

#### 9.1.2 Naming fields
* The request body or response type is JSON then please follow `camelCase` to maintain the consistency.
* Expose Resources, not your database schema details. You don't have to use your `table_name` for a resource name as well. Same with resource properties, they shouldn't be the same as your column names.
* Only use nouns in your URL naming and don’t try to explain their functionality and only explain the resources (elegantly).


### 9.2 Operating on resources

#### 9.2.1 Use HTTP methods
Only use nouns in your resource URLs, avoid endpoints like `/addNewUser` or `/updateUser` .  Also avoid sending resource operations as a parameter. Instead explain the functionalities using HTTP methods:

* **GET**		Used to retrieve a representation of a resource.
* **POST**	Used to create new resources and sub-resources
* **PUT**		Used to update existing resources
* **PATCH**	Used to update existing resources.  PATCH only updates the fields that were supplied, leaving the others alone
* **DELETE**	Used to delete existing resources

### 9.3 Use sub-resources
Sub resources are used to link one resource with another, so use sub resources to represent the relation.
An API is supposed to be an interface for developers and this is a natural way to make resources explorable.
If there is a relation between resources like  employee to a company, use `id` in the URL:

* **GET**		`/schools/2/students	`	Should get the list of all students from school 2
* **GET**		`/schools/2/students/31`	Should get the details of student 31, which belongs to school 2
* **DELETE**	`/schools/2/students/31`	Should delete student 31, which belongs to school 2
* **PUT**		`/schools/2/students/31`	Should update info of student 31, Use PUT on resource-URL only, not collection
* **POST**	`/schools `					Should create a new school and return the details of the new school created. Use POST on collection-URLs

### 9.4 API Versioning
When your APIs are public for other third parties, upgrading the APIs with some breaking change would also lead to breaking the existing products or services using your APIs. Using versions in your URL can prevent that from happening:
`http://api.domain.com/v1/schools/3/students	`

### 9.5 Send feedbacks
#### 9.5.1 Errors
Response messages must be self descriptive. A good error message response might look something like this:
```json
{
"code": 1234,
"message" : "Something bad happened",
"description" : "More details"
}
```
or for validation errors:

```json
{
  "code" : 2314,
  "message" : "Validation Failed",
  "errors" : [
    {
      "code" : 1233,
      "field" : "email",
      "message" : "Invalid email"
    },
    {
       "code" : 1234,
       "field" : "password",
       "message" : "No password provided"
    }
  ]
}
```
Note: Keep security exception messages as generic as possible. For instance, Instead of saying ‘incorrect password’, you can reply back saying ‘invalid username or password’ so that we don’t unknowingly inform user that username was indeed correct and only password was incorrect.

#### 9.5.2 Align your feedback with HTTP codes.
##### The client and API worked (success – 2xx response code)
* `200 OK` This HTTP response represents success for `GET`, `PUT` or `POST` requests.
* `201 Created` This status code should be returned whenever a new instance is created. E.g on creating a new instance, using `POST` method, should always return `201` status code.
* `204 No Content` represents the request was successfully processed, but has not returned any content. `DELETE` can be a good example of this. If there is any error, then the response code would be not be of 2xx Success Category but around 4xx Client Error category.

##### The client application behaved incorrectly (client error – 4xx response code)
* `400 Bad Request` indicates that the request by the client was not processed, as the server could not understand what the client is asking for.
* `401 Unauthorized` indicates that the request lacks valid credentials needed to access the needed resources, and the client should re-request with the required credentials.
* `403 Forbidden` indicates that the request is valid and the client is authenticated, but the client is not allowed access the page or resource for any reason.
* `404 Not Found` indicates that the requested resource was not found.
* `406 Not Acceptable` A response matching the list of acceptable values defined in Accept-Charset and Accept-Language headers cannot be served.
* `410 Gone` indicates that the requested resource is no longer available and has been intentionally and permanently moved.

##### The API behaved incorrectly (server error – 5xx response code)
* `500 Internal Server Error` indicates that the request is valid, but the server could not fulfill it due to some unexpected condition.
* `503 Service Unavailable` indicates that the server is down or unavailable to receive and process the request. Mostly if the server is undergoing maintenance or facing a temporary overload.


### 9.6 Resource parameters and metadata
* Provide total numbers of resources in your response
* The amount of data the resource exposes should also be taken into account. The API consumer doesn't always need the full representation of a resource.Use a fields query parameter that takes a comma separated list of fields to include:
    ```
    GET /student?fields=id,name,age,class
    ```
* Pagination, filtering and sorting don’t need to be supported by default for all resources. Document those resources that offer filtering and sorting.


### 9.7 API security
#### 9.7.1 TLS
To secure your web API authentication, all authentications should use SSL. OAuth2 requires the authorization server and access token credentials to use TLS.
Switching between HTTP and HTTPS introduces security weaknesses and best practice is to use TLS by default for all communication. Throw an error for non-secure access to API URLs.

#### 9.7.2 Rate limiting
If your API is public or have high number of users, any client may be able to call your API thousands of times per hour. You should consider implementing rate limit early on.

#### 9.7.3 Input Validation
It's difficult to perform most attacks if the allowed values are limited.
* Validate required fields, field types (e.g. string, integer, boolean, etc), and format requirements. Return 400 Bad Request with details about any errors from bad or missing data.

* Escape parameters that will become part of the SQL statement to protect from SQL injection attacks

* As also mentioned before, don't expose your database scheme when naming your resources and defining your responses

#### 9.7.4 URL validations
Attackers can tamper with any part of an HTTP request, including the URL, query string,

#### 9.7.5 Validate incoming content-types.
The server should never assume the Content-Type. A lack of Content-Type header or an unexpected Content-Type header should result in the server rejecting the content with a `406` Not Acceptable response.

#### 9.7.6 JSON encoding
A key concern with JSON encoders is preventing arbitrary JavaScript remote code execution within the browser or node.js, on the server. Use a JSON serialiser to entered data to prevent the execution of user input on the browser/server.


### 9.8 API documentation
* Fill the `API Reference` section in [README.md template](./README.sample.md) for API.
* Describe API authentication methods with a code sample
* Explaining The URL Structure (path only, no root URL) including The request type (Method)

For each endpoint explain:
* URL Params If URL Params exist, specify them in accordance with name mentioned in URL section
```
Required: id=[integer]
Optional: photo_id=[alphanumeric]
```
* If the request type is POST, provide a working examples. URL Params rules apply here too. Separate the section into Optional and Required.
* Success Response, What should be the status code and is there any return data? This is useful when people need to know what their callbacks should expect!
    ```
    Code: 200
    Content: { id : 12 }
    ```
* Error Response, Most endpoints have many ways to fail. From unauthorised access, to wrongful parameters etc. All of those should be listed here. It might seem repetitive, but it helps prevent assumptions from being made. For example
    ```json
    {
        "code": 403,
        "message" : "Authentication failed",
        "description" : "Invalid username or password"
    }
    ```


#### 9.8.1 API design tools
There are lots of open source tools for good documentation such as [API Blueprint](https://apiblueprint.org/) and [Swagger](https://swagger.io/).

----

## 10. Licensing <a name="licensing"></a>
Make sure you use resources that you have the rights to use. If you use libraries, remember to look for MIT, Apache or BSD but if you modify them, then take a look into licence details. Copyrighted images and videos may cause legal problems.


----

Sources:
[Hive](https://github.com/wearehive/project-guidelines),
[RisingStack Engineering](https://blog.risingstack.com/),
[Mozilla Developer Network](https://developer.mozilla.org/),
[Heroku Dev Center](https://devcenter.heroku.com),
[Airbnb/javascript](https://github.com/airbnb/javascript),
[Atlassian Git tutorials](https://www.atlassian.com/git/tutorials)
